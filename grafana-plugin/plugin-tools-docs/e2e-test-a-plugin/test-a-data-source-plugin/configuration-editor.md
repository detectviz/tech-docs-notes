---
id: configurations
title: 測試設定
description: 使用有效和無效的設定測試後端和前端資料來源的設定編輯器
keywords:
  - grafana
  - plugins
  - plugin
  - testing
  - e2e
  - data-source
  - configuration editor
  - config
sidebar_position: 10
---

大多數資料來源插件需要驗證才能與第三方服務通訊。設定連線詳細資料的適當位置是資料來源設定頁面，且其中的詳細資料必須有效，以便用於測試設定的健康狀況檢查端點能如預期般運作。

### 測試設定編輯器是否載入

以下範例是一個簡單的 smoke 測試，用於驗證資料來源設定編輯器是否能載入：

```ts title="configurationEditor.spec.ts"
import { test, expect } from '@grafana/plugin-e2e';

test('應呈現設定編輯器', async ({ createDataSourceConfigPage, readProvisionedDataSource, page }) => {
  const ds = await readProvisionedDataSource({ fileName: 'datasources.yml' });
  await createDataSourceConfigPage({ type: ds.type });
  await expect(page.getByLabel('Path')).toBeVisible();
});
```

### 在後端資料來源插件中測試設定

後端資料來源實作了一個[健康狀況檢查](../../key-concepts/backend-plugins/#health-checks)端點，用於測試設定是否有效。在以下範例中，設定編輯器表單會填入有效值，然後點擊 `Save & test` 按鈕。點擊 `Save & test` 會呼叫 Grafana 後端以儲存設定，然後將設定傳遞給插件的後端健康狀況檢查端點。只有當兩個呼叫都產生成功的狀態碼時，測試才會成功。

```ts title="configurationEditor.spec.ts"
import { test, expect } from '@grafana/plugin-e2e';
import { MyDataSourceOptions, MySecureJsonData } from './src/types';

test('當設定有效時，"Save & test" 應成功', async ({
  createDataSourceConfigPage,
  readProvisionedDataSource,
  page,
}) => {
  const ds = await readProvisionedDataSource<MyDataSourceOptions, MySecureJsonData>({ fileName: 'datasources.yml' });
  const configPage = await createDataSourceConfigPage({ type: ds.type });
  await page.getByLabel('Path').fill(ds.jsonData.path);
  await page.getByLabel('API Key').fill(ds.secureJsonData.apiKey);
  await expect(configPage.saveAndTest()).toBeOK();
});
```

#### 測試錯誤情境

在某些情況下，當提供的設定無效時，您可能會想從上游 API 捕捉錯誤，並向使用者傳回有意義的錯誤訊息。

```ts title="configurationEditor.spec.ts"
test('當設定無效時，"Save & test" 應失敗', async ({
  createDataSourceConfigPage,
  readProvisionedDataSource,
  page,
}) => {
  const ds = await readProvisionedDataSource<MyDataSourceOptions, MySecureJsonData>({ fileName: 'datasources.yml' });
  const configPage = await createDataSourceConfigPage({ type: ds.type });
  await page.getByLabel('Path').fill(ds.jsonData.path);
  await expect(configPage.saveAndTest()).not.toBeOK();
  await expect(configPage).toHaveAlert('error', { hasText: 'API key is missing' });
});
```

### 在前端資料來源插件中測試設定

與總是呼叫其自身後端以執行健康狀況檢查的後端資料來源插件不同，前端資料來源插件可能需要呼叫第三方 API 來測試提供的設定是否有效。`DataSourceConfigPage.saveAndTest` 方法可讓您為用於測試資料來源設定的端點提供自訂路徑。

```ts title="configurationEditor.spec.ts"
test('當設定有效時，"Save & test" 應成功', async ({
  createDataSourceConfigPage,
  readProvisionedDataSource,
  selectors,
}) => {
  const ds = await readProvisionedDataSource({ fileName: 'datasources.yml' });
  const configPage = await createDataSourceConfigPage({ type: ds.type });
  const healthCheckPath = `${selectors.apis.DataSource.proxy(configPage.datasource.uid)}/test`;
  await page.route(healthCheckPath, async (route) => await route.fulfill({ status: 200, body: 'OK' })
  // 使用 Grafana 資料來源代理建構自訂健康狀況檢查 URL
  const healthCheckPath = `${selectors.apis.DataSource.proxy(
    configPage.datasource.uid,
    configPage.datasource.id.toString()
  )}/third-party-service-path`;
  await expect(configPage.saveAndTest({ path: healthCheckPath })).toBeOK();
});
```

此外，您可以斷言頁面上會顯示一個成功警示框。

```ts title="configurationEditor.spec.ts"
test('當設定有效時，"Save & test" 應顯示成功警示框', async ({
  createDataSourceConfigPage,
  readProvisionedDataSource,
  page,
}) => {
  const ds = await readProvisionedDataSource({ fileName: 'datasources.yml' });
  const configPage = await createDataSourceConfigPage({ type: ds.type });
  // 使用 Grafana 資料來源代理建構自訂健康狀況檢查 URL
  const healthCheckPath = `${selectors.apis.DataSource.proxy(
    configPage.datasource.uid,
    configPage.datasource.id.toString()
  )}/third-party-service-path`;
  await page.route(healthCheckPath, async (route) => await route.fulfill({ status: 200, body: 'OK' }));
  await expect(configPage.saveAndTest({ path: healthCheckPath })).toBeOK();
  await expect(configPage).toHaveAlert('success');
});
```

### 測試已佈建的資料來源

有時您可能會想為一個已存在的資料來源執行個體開啟設定編輯器，以驗證設定是否如預期般運作。

```ts
test('具有有效憑證的已佈建資料來源應傳回 200 狀態碼', async ({
  readProvisionedDataSource,
  gotoDataSourceConfigPage,
}) => {
  const datasource = await readProvisionedDataSource({ fileName: 'datasources.yml' });
  const configPage = await gotoDataSourceConfigPage(datasource.uid);
  await expect(configPage.saveAndTest()).toBeOK();
});
```
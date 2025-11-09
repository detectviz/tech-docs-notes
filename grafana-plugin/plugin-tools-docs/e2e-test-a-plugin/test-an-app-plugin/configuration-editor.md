---
id: app-configurations
title: 測試設定
description: 測試應用程式的設定編輯器
keywords:
  - grafana
  - plugins
  - plugin
  - testing
  - e2e
  - app
  - configuration editor
  - config
sidebar_position: 20
---

# 測試應用程式的設定編輯器

如果您的應用程式需要某些設定才能運作，應用程式設定頁面會管理這些設定。本指南說明如何建立一個特定於應用程式的 fixture，以簡化編寫測試並在不同測試之間共用邏輯。

## 測試應用程式設定頁面

應用程式有一個[健康狀況檢查](../../key-concepts/backend-plugins/#health-checks)端點，用於測試設定的有效性。在以下範例中，當點擊 **Save & test** 按鈕時，設定編輯器表單會填入有效值。

點擊 **Save & test** 按鈕會呼叫 Grafana 後端以儲存設定，然後將設定傳遞給健康狀況檢查端點。只有當兩個呼叫都產生成功的狀態碼時，測試才會成功。

### 具有基本 UI 的設定頁面

透過使用傳回 `@grafana/plugin-e2e` 中定義的預設 `AppConfigPage` 的導覽函式，新增一個 `appConfigPage` 值。

例如：

```ts title="fixtures.ts"
import { AppConfigPage, test as base } from '@grafana/plugin-e2e';
import pluginJson from '../src/plugin.json';

type AppTestFixture = {
  appConfigPage: AppConfigPage;
};

export const test = base.extend<AppTestFixture>({
  appConfigPage: async ({ gotoAppConfigPage }, use) => {
    const configPage = await gotoAppConfigPage({
      pluginId: pluginJson.id,
    });
    await use(configPage);
  },
});

export { expect } from '@grafana/plugin-e2e';
```

若要使用此值，請從您的 fixture 匯入 `test` 和 `expect`，而不是從 `@grafana/plugin-e2e` 匯入。當您在測試函式中解構 `appConfigPage` 時，其餘部分會自動導覽至設定頁面。

例如：

```ts title="configurationEditor.spec.ts"
import { test, expect } from './fixtures.ts';

test('當設定有效時，"Save & test" 應成功', async ({ appConfigPage, page }) => {
  const saveButton = page.getByRole('button', { name: /Save & test/i });

  await page.getByRole('textbox', { name: 'API Key' }).fill('secret-api-key');
  await page.getByRole('textbox', { name: 'API Url' }).clear();
  await page.getByRole('textbox', { name: 'API Url' }).fill('http://www.my-awsome-grafana-app.com/api');

  const saveResponse = appConfigPage.waitForSettingsResponse();

  await saveButton.click();
  await expect(saveResponse).toBeOK();
});
```

### 具有複雜 UI 的設定頁面

透過使用傳回 `@grafana/plugin-e2e` 中定義的預設 `AppConfigPage` 的導覽函式，新增一個 `appConfigPage`。

例如：

```ts title="fixtures.ts"
import { AppConfigPage, test as base } from '@grafana/plugin-e2e';
import pluginJson from '../src/plugin.json';

class MyAppConfigPage extends AppConfigPage {
  async fillApiKey(key: string): Promise<void> {
    await page.getByRole('textbox', { name: 'API Key' }).fill(key);
  }

  async fillApiUrl(url: string): Promise<void> {
    await page.getByRole('textbox', { name: 'API Url' }).clear();
    await page.getByRole('textbox', { name: 'API Url' }).fill(url);
  }

  async save(): Promise<void> {
    await page.getByRole('button', { name: /Save & test/i }).click();
  }
}

type AppTestFixture = {
  appConfigPage: MyAppConfigPage;
};

export const test = base.extend<AppTestFixture>({
  appConfigPage: async ({ page, selectors, grafanaVersion, request }, use, testInfo) => {
    const configPage = new MyAppConfigPage(
      { page, selectors, grafanaVersion, request, testInfo },
      {
        pluginId: pluginJson.id,
      }
    );
    await configPage.goto();
    await use(configPage);
  },
});

export { expect } from '@grafana/plugin-e2e';
```

若要使用此值，請從您的 fixture 匯入 `test` 和 `expect`，而不是從 `@grafana/plugin-e2e` 匯入。當您在測試函式中解構 `appConfigPage` 時，測試會自動導覽至設定頁面。

例如：

```ts title="configurationEditor.spec.ts"
import { test, expect } from './fixtures.ts';

test('當設定有效時，"Save & test" 應成功', async ({ appConfigPage, page }) => {
  await appConfigPage.fillApiKey('secret-api-key');
  await appConfigPage.fillApiUrl('http://www.my-awsome-grafana-app.com/api');

  const saveResponse = appConfigPage.waitForSettingsResponse();

  await appConfigPage.save();
  await expect(saveResponse).toBeOK();
});
```

## 測試錯誤情境

在某些情況下，當提供的設定無效時，您可能會想從上游 API 捕捉錯誤，並向使用者傳回有意義的錯誤訊息。

例如：

```ts title="configurationEditor.spec.ts"
import { test, expect } from './fixtures.ts';

test('當設定無效時，"Save & test" 應失敗', async ({ appConfigPage, page }) => {
  const saveButton = page.getByRole('button', { name: /Save & test/i });

  await page.getByRole('textbox', { name: 'API Url' }).clear();
  await page.getByRole('textbox', { name: 'API Url' }).fill('not a url');

  const saveResponse = appConfigPage.waitForSettingsResponse();

  await saveButton.click();

  await expect(appConfigPage).toHaveAlert('error');
  await expect(saveResponse).not.toBeOK();
});
```
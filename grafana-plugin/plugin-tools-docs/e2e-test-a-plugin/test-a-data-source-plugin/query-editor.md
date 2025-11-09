---
id: data-queries
title: 測試資料查詢
description: 測試查詢編輯器和資料查詢的執行
keywords:
  - grafana
  - plugins
  - plugin
  - testing
  - e2e
  - data-source
  - query editor
sidebar_position: 20
---

## 簡介

查詢編輯器是資料來源插件的核心部分，因為這是使用者建立用於擷取資料的查詢的地方。您的資料來源插件可以提供一個豐富的查詢編輯器，允許針對不同 API 的各種查詢類型。您的查詢編輯器甚至可以具有視覺化查詢產生器、IntelliSense 和自動完成等功能。

### 測試查詢編輯器是否載入

以下範例是一個簡單的 smoke 測試，用於驗證資料來源查詢編輯器是否能載入：

```ts title="queryEditor.spec.ts"
import { test, expect } from '@grafana/plugin-e2e';

test('應呈現查詢編輯器', async ({ panelEditPage, readProvisionedDataSource }) => {
  const ds = await readProvisionedDataSource({ fileName: 'datasources.yml' });
  await panelEditPage.datasource.set(ds.name);
  await expect(panelEditPage.getQueryEditorRow('A').getByRole('textbox', { name: 'Query Text' })).toBeVisible();
});
```

### 獨立測試查詢編輯器的部分功能

在以下範例中，查詢編輯器透過對 `/regions` 的請求載入區域，並在將它們填入下拉式選單之前篩選掉包含 `gov` 的區域。

[`<page>.mockResourceResponse`](https://github.com/grafana/plugin-tools/blob/main/packages/plugin-e2e/src/models/pages/GrafanaPage.ts#L53) 方法可讓您模擬對資料來源[資源 API](../../key-concepts/backend-plugins/#resources) 的請求回應。為了測試篩選是否如預期般運作，我們使用此方法來模擬 `/regions` 回應，並斷言當點擊區域下拉式選單時，只會顯示名稱中不含 `-gov-` 的區域。

```ts title="queryEditor.spec.ts"
test('應篩選掉 govcloud 區域', async ({ panelEditPage, selectors, readProvisionedDataSource }) => {
  const regionsMock = ['us-gov-west-1', 'us-east-1', 'us-west-1', 'us-gov-east-1'];
  const expectedRegions = ['us-east-1', 'us-west-1'];
  const ds = await readProvisionedDataSource({ fileName: 'datasources.yaml' });
  await panelEditPage.datasource.set(ds.name);
  await panelEditPage.mockResourceResponse('regions', regionsMock);
  await panelEditPage.getQueryEditorRow('A').getByText('Regions').click();
  await expect(panelEditPage.getByGrafanaSelector(selectors.components.Select.option)).toHaveText(expectedRegions);
});
```

### 測試整個資料流程

以下範例顯示了一個整合測試，用於測試插件的整個查詢資料流程。

:::warning
在端對端測試中呼叫第三方 API 可能很有用，但它也可能帶來安全隱憂以及為您的 CI 管線帶來不穩定性。您應始終仔細考慮並考慮改用模擬。
:::

```ts title="queryEditor.spec.ts"
test('當查詢有效時，資料查詢應成功', async ({
  panelEditPage,
  readProvisionedDataSource,
}) => {
  const ds = await readProvisionedDataSource({ fileName: 'datasources.yaml' });
  await panelEditPage.datasource.set(ds.name);
  await panelEditPage.getQueryEditorRow('A').getByText('Query Text').fill('SELECT * FROM dataset');
  await expect(panelEditPage.refreshPanel()).toBeOK();
});
```

### 對面板資料進行斷言

在許多情況下，僅斷言資料查詢回應為 OK 並不足以說明您的資料來源插件運作正常。您還必須對面板中顯示的資料進行斷言。

Grafana 隨附一組內建面板，[Grafana 插件目錄](https://grafana.com/grafana/plugins/)中也提供了各種社群面板。每個面板都可以以不同的方式呈現資料，因此不可能提供一種在所有面板上一致斷言顯示資料的方法。因此，我們建議您為此使用 `Table` 面板。

`<page>.panel.data` 屬性會傳回一個 [Playwright 定位器](https://playwright.dev/docs/locators)，它會解析為一或多個包含目前顯示在 Table 面板中之值的元素。這意味著您可以使用任何接受定位器作為接收類型的自動重試[匹配器](https://playwright.dev/docs/test-assertions#auto-retrying-assertions)。

```ts title="queryEditor.spec.ts"
test('資料查詢應傳回值 10 和 20', async ({ panelEditPage, readProvisionedDataSource }) => {
  const ds = await readProvisionedDataSource({ fileName: 'datasources.yml' });
  await panelEditPage.datasource.set(ds.name);
  await panelEditPage.setVisualization('Table');
  await expect(panelEditPage.refreshPanel()).toBeOK();
  await expect(panelEditPage.panel.data).toContainText(['10', '20']);
});
```

如果您想斷言 `Table` 面板中顯示了哪些欄位標頭，您可以使用 `<page>.panel.fieldNames` 屬性。

```ts title="queryEditor.spec.ts"
test('資料查詢應傳回標頭和 3', async ({ panelEditPage, readProvisionedDataSource }) => {
  const ds = await readProvisionedDataSource({ fileName: 'datasources.yml' });
  await panelEditPage.datasource.set(ds.name);
  await panelEditPage.setVisualization('Table');
  await expect(panelEditPage.refreshPanel()).toBeOK();
  await expect(panelEditPage.panel.fieldNames).toHaveText(['Stockholm', 'Vienna']);
});
```

### 在已佈建的儀表板中測試查詢

有時您可能會想為一個已存在的面板開啟面板編輯頁面，並執行查詢以確保一切如預期般運作。

```ts
test('已佈建儀表板中的查詢應傳回溫度和濕度資料', async ({
  readProvisionedDashboard,
  gotoPanelEditPage,
}) => {
  const dashboard = await readProvisionedDashboard({ fileName: 'dashboard.json' });
  const panelEditPage = await gotoPanelEditPage({ dashboard, id: '3' });
  await expect(panelEditPage.refreshPanel()).toBeOK();
  await expect(panel.fieldNames).toContainText(['temperature', 'humidity']);
  await expect(panel.data).toContainText(['25', '10']);
});
```

您也可以開啟一個已存在的儀表板，並驗證表格面板是否已呈現您預期的資料。

```ts
test('依 id 取得面板', async ({ gotoDashboardPage, readProvisionedDashboard }) => {
  const dashboard = await readProvisionedDashboard({ fileName: 'dashboard.json' });
  const dashboardPage = await gotoDashboardPage(dashboard);
  const panel1 = await dashboardPage.getPanelById('3');
  await expect(panel1.data).toContainText(['25', '32', 'staging']);
  const panel2 = await dashboardPage.getPanelByTitle('Basic table example');
  await expect(dashboardPage.panel2.fieldNames).toContainText(['Tokyo', 'Berlin']);
});
```
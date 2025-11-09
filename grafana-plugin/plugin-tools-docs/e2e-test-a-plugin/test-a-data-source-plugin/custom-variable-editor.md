---
id: variable-queries
title: 測試變數查詢
description: 測試自訂變數查詢編輯器和變數查詢的執行。
keywords:
  - grafana
  - plugins
  - plugin
  - testing
  - e2e
  - data-source
  - variable editor
sidebar_position: 30
---

## 簡介

[變數查詢](https://grafana.com/docs/grafana/latest/dashboards/variables/add-template-variables/#add-a-query-variable)允許使用者查詢資料來源以載入資料列表，例如指標名稱。然後，您可以在查詢中引用變數，使您的儀表板更具互動性和動態性。如果您的資料來源插件實作了自訂變數支援 API，您可能需要使用 `variableEditPage` fixture 來測試您的插件的變數實作是否如預期般運作。

### 測試自訂變數編輯器是否載入

以下範例是一個簡單的 smoke 測試，用於驗證自訂變數編輯器是否能載入：

```ts title="customVariableEditor.spec.ts"
import { expect, test } from '@grafana/plugin-e2e';

test('應呈現變數編輯器', async ({ variableEditPage, page, readProvisionedDataSource }) => {
  const ds = await readProvisionedDataSource({ fileName: 'datasources.yml' });
  await variableEditPage.datasource.set(ds.name);
  await expect(page.getByRole('textbox', { name: 'Query Text' })).toBeVisible();
});
```

### 獨立測試自訂變數編輯器

在以下範例中，我們測試當選擇 `ListByDimensions` 查詢類型時，自訂變數編輯器是否會呈現某個欄位：

```ts title="customVariableEditor.spec.ts"
test('僅當選擇 ListByDimensions 時才應顯示 Dimensions 欄位', async ({
  variableEditPage,
  page,
  readProvisionedDataSource,
}) => {
  const ds = await readProvisionedDataSource({ fileName: 'datasources.yaml' });
  await variableEditPage.setVariableType('Query');
  await variableEditPage.datasource.set(ds.name);
  const dimensionField = variableEditPage.getByGrafanaSelector('Dimensions');
  await expect(dimensionField).not.toBeVisible();
  await variableEditPage.getByLabel('Query type').fill('ListByDimensions');
  await page.keyboard.press('Enter');
  await expect(dimensionField).toBeVisible();
});
```

### 測試變數查詢執行流程

在下一個範例中，我們執行一個整合測試，測試插件的整個變數查詢資料流程。對於成功的變數查詢，結果選項會顯示在變數編輯頁面的底部。您可以使用 `toDisplayPreviews` 匹配器來斷言顯示了預期的預覽。

![](/img/variable-preview.png)

:::warning

雖然在端對端測試中呼叫第三方 API 可能很有用，但它也可能為您的 CI 管線帶來安全隱憂和其他問題。您應始終仔細考慮，並考慮改用模擬。

:::

```ts title="customVariableEditor.spec.ts"
test('當查詢有效時，自訂變數查詢執行器應傳回資料', async ({
  variableEditPage,
  page,
  readProvisionedDataSource,
  selectors,
}) => {
  const ds = await readProvisionedDataSource({ fileName: 'datasources.yaml' });
  await variableEditPage.setVariableType('Query');
  await variableEditPage.datasource.set(ds.name);
  const codeEditorSelector = selectors.components.CodeEditor.container;
  await variableEditPage.getByGrafanaSelector(codeEditorSelector).click();
  await page.keyboard.insertText('select distinct(environment) from dataset');
  const queryDataRequest = variableEditPage.waitForQueryDataRequest();
  await variableEditPage.runQuery();
  await queryDataRequest;
  await expect(variableEditPage).toDisplayPreviews(['test', /staging-.*/]);
});
```

:::note
與 `panelEditPage.refreshPanel` 方法不同，`variableEditPage.runQuery` 方法不會傳回 [Playwright 回應](https://playwright.dev/docs/api/class-response) promise。在上面的範例中，變數查詢會通過資料查詢端點，但您也可以使用 Playwright 的 [`waitForResponse`](https://playwright.dev/docs/api/class-page#page-wait-for-response) 方法並指定任何選擇的端點。
:::

如果您只想測試變數查詢執行器而不測試自訂變數編輯器，您可以使用來自已佈建儀表板的現有變數查詢。

```ts title="customVariableEditor.spec.ts"
test('當使用來自已佈建儀表板的有效查詢時應傳回資料', async ({
  readProvisionedDashboard,
  gotoVariableEditPage,
}) => {
  const dashboard = await readProvisionedDashboard({ fileName: 'dashboard.json' });
  const variableEditPage = await gotoVariableEditPage({ dashboard, id: '2' });
  await variableEditPage.runQuery();
  await expect(variableEditPage).toDisplayPreviews(['staging', 'test']);
});
```
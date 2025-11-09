---
id: setup-resources
title: 設定必要的資源
description: 透過佈建，設定您的插件進行端對端測試所需的任何儀表板、資料來源或其他 Grafana 資源。
keywords:
  - grafana
  - plugins
  - plugin
  - testing
  - e2e
  - end-to-end
  - data-source
  - preparation
  - provisioning
sidebar_position: 60
---

## 簡介

在許多情況下，您需要在執行端對端測試之前，在 Grafana 中設定好某些資源。例如，若要測試面板插件如何顯示資料，您需要設定一個資料來源來查詢並傳回該資料。本指南涵蓋如何透過[佈建](https://grafana.com/docs/grafana/latest/administration/provisioning/)來設定這些資源。

## 測試隔離

[測試隔離](https://playwright.dev/docs/browser-contexts#what-is-test-isolation)是 Playwright 測試的核心部分。在這個主題上，我們建議獨立測試插件功能，而不是透過某些步驟對先前步驟有依賴性的進階流程來執行它們。

### 一個具體的例子

假設您想在您的資料來源插件中測試範本變數插值。為了在 `DataSource` 檔案中進行任何插值，需要定義一個範本變數。由於目標是測試變數插值，我們不想在測試程式碼中建立範本變數。相反地，我們將在我們的測試中使用一個已預先定義好範本變數的已佈建儀表板。

在以下範例中，我們導覽到一個已佈建的儀表板。該儀表板有一個多值範本變數 `env`，其值為 `test` 和 `prod`。我們新增一個新面板並設定一個參照 `env` 變數的 SQL 查詢。然後我們監視查詢資料請求，斷言它被呼叫時帶有與範本變數關聯的擴充值。

```ts
test('在呼叫後端之前應擴充多值變數', async ({
  gotoDashboardPage,
  readProvisionedDashboard,
}) => {
  const dashboard = await readProvisionedDashboard({ fileName: 'variable.json' });
  const dashboardPage = await gotoDashboardPage(dashboard);
  const panelEditPage = await dashboardPage.addPanel();
  const queryDataSpy = panelEditPage.waitForQueryDataRequest((request) =>
    (request.postData() ?? '').includes(`select * from dataset where env in ('test', 'prod')"`)
  );
  await page.getByLabel('Query').fill('select * from dataset where env in (${env:singlequote})');
  await panelEditPage.refreshPanel();
  await expect(await queryDataSpy).toBeTruthy();
});
```

## 佈建必要的資源

您可以使用[佈建](https://grafana.com/docs/grafana/latest/administration/provisioning/)來設定儀表板和資料來源等資源。

:::note

如果在 CI 中執行端對端測試需要佈建，您可能需要從插件的 `.gitignore` 檔案中移除 `provisioning` 資料夾。

:::

:::danger

請注意不要將機密提交到公開的儲存庫。對於敏感資料，請使用[環境變數插值](https://grafana.com/docs/grafana/latest/administration/provisioning/#using-environment-variables)。

:::

## 讀取已佈建的檔案

`@grafana/plugin-e2e` 工具提供了 fixtures，讓您可以讀取您放置在 `provisioning` 資料夾中的檔案。

### readProvisionedDataSource fixture

`readProvisionedDataSource` fixture 可讓您從插件的 `provisioning/datasources` 資料夾中讀取檔案。這為您提供了型別，也讓您可以將資料來源設定集中在一個地方。

```ts title="configEditor.spec.ts"
const datasource = readProvisionedDataSource<JsonData, SecureJsonData>({ fileName: 'datasources.yml' });
await page.getByLabel('API Key').fill(datasource.secureJsonData.apiKey);
```

```ts title="queryEditor.spec.ts"
const datasource = readProvisionedDataSource({ fileName: 'datasources.yml' });
await panelEditPage.datasource.set(datasource.name);
```

### readProvisionedDashboard fixture

`readProvisionedDashboard` fixture 可讓您從 `provisioning/dashboards` 資料夾中讀取儀表板 JSON 檔案的內容。當您不想硬式編碼儀表板 UID 時，在導覽到已佈建的儀表板時它會很有用。

```ts title="variableEditPage.spec.ts"
const dashboard = await readProvisionedDashboard({ fileName: 'dashboard.json' });
const variableEditPage = new VariableEditPage(
  { request, page, selectors, grafanaVersion, testInfo },
  { dashboard, id: '2' }
);
await variableEditPage.goto();
```

### readProvisionedAlertRule fixture

`readProvisionedAlertRule` fixture 可讓您從插件的 `provisioning/alerting` 資料夾中讀取檔案。

```ts title="alerting.spec.ts"
test('載入有效的已佈建查詢時應評估為 true', async ({
  gotoAlertRuleEditPage,
  readProvisionedAlertRule,
}) => {
  const alertRule = await readProvisionedAlertRule({ fileName: 'alerts.yml' });
  const alertRuleEditPage = await gotoAlertRuleEditPage(alertRule);
  await expect(alertRuleEditPage.evaluate()).toBeOK();
});
```
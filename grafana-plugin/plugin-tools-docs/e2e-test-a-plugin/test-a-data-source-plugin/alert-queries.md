---
id: alert-queries
title: 測試警報查詢
description: 測試警報查詢以確保插件與警報功能相容
keywords:
  - grafana
  - plugins
  - plugin
  - testing
  - e2e
  - data-source
  - alert queries
sidebar_position: 60
---

## 簡介

已啟用警報功能的後端資料來源插件可以根據資料來源查詢定義警報。在您儲存警報之前，警報引擎會評估警報定義的條件，以確保來自資料來源的回應格式正確。如果格式正確，那麼您可以使用 `alertRulePage` fixture 來驗證可以從資料來源傳回的查詢輸出建立警報規則。

:::info
用於端對端測試警報規則的 API 僅與 Grafana >=9.4.0 相容。
:::

### 評估新的警報規則

以下範例使用 `alertRulePage` fixture。使用此 fixture，測試會從新增警報規則的頁面開始。然後您填寫警報規則查詢並呼叫 `evaluate` 函式。`evaluate` 會點擊 `Preview` 按鈕，這會觸發對 `eval` 端點的呼叫，以評估資料來源查詢的回應是否可用於建立警報。`toBeOK` 匹配器用於驗證評估是否成功。

```ts
test('如果查詢有效，應評估為 true', async ({ page, alertRuleEditPage, selectors }) => {
  const queryA = alertRuleEditPage.getQueryRow();
  await queryA.datasource.set('gdev-prometheus');
  await queryA.locator.getByLabel('Code').click();
  await page.waitForFunction(() => window.monaco);
  await queryA.getByGrafanaSelector(selectors.components.CodeEditor.container).click();
  await page.keyboard.insertText('topk(5, max(scrape_duration_seconds) by (job))');
  await expect(alertRuleEditPage.evaluate()).toBeOK();
});
```

### 評估已佈建的警報規則

您也可以使用已佈建的警報規則來測試您的資料來源是否與警報功能相容。例如：

```ts
test('載入有效的已佈建查詢時應評估為 true', async ({
  gotoAlertRuleEditPage,
  readProvisionedAlertRule,
}) => {
  const alertRule = await readProvisionedAlertRule({ fileName: 'alerts.yml' });
  const alertRuleEditPage = await gotoAlertRuleEditPage(alertRule);
  await expect(alertRuleEditPage.evaluate()).toBeOK();
});
```
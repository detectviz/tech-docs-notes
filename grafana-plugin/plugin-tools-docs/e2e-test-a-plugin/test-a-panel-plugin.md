---
id: test-a-panel-plugin
title: 測試面板插件
description: 如何使用 @grafana/plugin-e2e 測試面板插件。
keywords:
  - grafana
  - plugins
  - plugin
  - testing
  - e2e
  - panel plugin
sidebar_position: 80
---

面板插件可讓您以不同方式將資料視覺化。本指南將說明如何使用 `@grafana/plugin-e2e` 來測試您的面板插件在多個 Grafana 版本中是否能正確呈現資料。

## 測試資料

為了能夠測試您的面板插件，您需要為其提供測試資料。Grafana 隨附 [TestData](https://grafana.com/docs/grafana/latest/datasources/testdata/) 資料來源，可用於模擬格式化為時間序列、日誌、追蹤、註釋等的[資料框架](../key-concepts/data-frames)。

## 開始之前

若要編寫類似本指南中的端對端測試，您需要使用佈建來設定 `TestData` 資料來源。如果您尚未閱讀我們關於如何[設定資源](./setup-resources.md)的指南，請先閱讀該指南。

## 測試面板選項

為了測試您面板的行為，我們建議[佈建一個儀表板](https://grafana.com/developers/plugin-tools/e2e-test-a-plugin/setup-resources)，其中包含多個展示您面板不同狀態的面板。這可確保您的面板在各種設定下都能正常運作。透過避免依賴 Grafana 面板編輯 UI，這種方法可以減少因 UI 變更而導致的測試失敗，讓您的測試更加穩定可靠。

在需要與面板編輯選項互動的情況下，我們提供了一組 API 來簡化測試的編寫。這些 API 確保您的測試在不同版本的 Grafana 中都能一致地執行，而無需進行變更。

若要與任何 Grafana 提供的選項群組互動，請使用下列任何函式：

| 函式名稱 | 傳回的選項群組 |
| --- | --- |
| `getPanelOptions()` | 面板選項 |
| `getStandardOptions()` | 標準選項 |
| `getValueMappingOptions()` | 值對應 |
| `getDataLinksOptions()` | 資料連結 |
| `getThresholdsOptions()` | 閾值 |

若要與您的面板新增的自訂選項群組互動，請使用 `getCustomOptions('選項群組名稱')` API。

呼叫任何這些 API 都會傳回一個選項群組物件，該物件提供了與該群組內選項互動的 API。

| 函式名稱 | 傳回選項類型 |
| --- | --- |
| `getRadioGroup(label)` | `RadioGroup` |
| `getSwitch(label)` | `Switch` |
| `getTextInput(label)` | `Locator` |
| `getNumberInput(label)` | `Locator` |
| `getSliderInput(label)` | `Locator` |
| `getSelect(label)` | `Select` |
| `getMultiSelect(label)` | `MultiSelect` |
| `getColorPicker(label)` | `ColorPicker` |
| `getUnitPicker(label)` | `UnitPicker` |

### 範例

此測試確保當使用者從標準選項中選取不同的單位時，顯示的單位會在 UI 中正確更新。

```ts
test('當標準選項變更時應變更單位', async ({ panelEditPage }) => {
  const standardOptions = panelEditPage.getStandardOptions();
  const unitPicker = standardOptions.getUnitPicker('Unit');
  const unit = page.getByTestId('unit-container');

  await unitPicker.selectOption('Misc > Pixels');

  await expect(unit).toContainText('px');
});
```

此測試驗證在面板設定中選取不同的時區會更新時鐘面板中顯示的時區。

```ts
test('當選取選項時應變更時區', async ({ panelEditPage, page }) => {
  const timeFormatOptions = panelEditPage.getCustomOptions('Timezone');
  const timeZoneSelect = timeFormatOptions.getSelect('Timezone');
  const timeZone = page.getByTestId('clock-panel').getByTestId('time-zone');

  await timeZoneSelect.selectOption('Europe/Stockholm');
  await expect(timeZone).toContainText('Europe/Stockholm');
});
```

此測試驗證在時鐘面板中啟用等寬字型選項會正確地將面板的字型系列更新為「monospace」。

```ts
test('啟用等寬字型時應變更字型系列', async ({ panelEditPage, page }) => {
  const clockOptions = panelEditPage.getCustomOptions('Clock');
  const monospaceFont = clockOptions.getSwitch('Font monospace');
  const panel = page.getByTestId('clock-panel');

  await monospaceFont.check();
  await expect(panel).toHaveCSS('font-family', 'monospace');
});
```

此測試確保在時鐘面板的選項中選取「倒數計時」模式時，時鐘會以倒數計時模式執行。

```ts
test('當選取選項時應倒數計時', async ({ panelEditPage, page }) => {
  const clockOptions = panelEditPage.getCustomOptions('Clock');
  const clockMode = clockOptions.getRadioGroup('Mode');
  const panel = page.getByTestId('clock-panel-countdown');

  await clockMode.check('Countdown');
  await expect(panel).toBeVisible();
});
```

此測試驗證當從面板選項中的顏色選擇器選取新顏色時，面板的背景顏色會變更。

```ts
test('應根據選取的選項更新背景顏色', async ({ panelEditPage, page }) => {
  const color = { hex: '#73bf69', rgb: 'rgb(115, 191, 105)' };
  const clockOptions = panelEditPage.getCustomOptions('Clock');
  const backgroundColor = clockOptions.getColorPicker('Background color');
  const panel = page.getByTestId('clock-panel');

  await backgroundColor.selectOption(color.hex);
  await expect(panel).toHaveCSS('background-color', color.rgb);
});
```

Table 面板預設定義了一個名為 `Show table header` 的自訂面板選項。如果停用此開關，Table 面板應從表格中移除標頭。

以下測試驗證欄位名稱（標頭）預設會顯示，並且在未選取 `Show table header` 選項時會被移除：

```ts
test('取消勾選 "Show table header" 時應隱藏標頭', async ({ panelEditPage, selectors }) => {
  await panelEditPage.datasource.set('gdev-testdata');
  await panelEditPage.setVisualization('Table');
  await expect(await panelEditPage.panel.fieldNames.count()).toBeGreaterThan(0);
  const showTableHeaderSwitch = panelEditPage
    .getByGrafanaSelector(selectors.components.PanelEditor.OptionsPane.fieldLabel('Table Show table header'))
    .getByLabel('Toggle switch');
  await panelEditPage.collapseSection('Table');
  await showTableHeaderSwitch.uncheck();
  await expect(panelEditPage.panel.fieldNames).not.toBeVisible();
});
```

## 測試面板如何處理不同的資料類型

資料框架模型的設計具有彈性。其目的是允許資料來源根據各種不同的[資料類型](https://grafana.com/developers/dataplane/#kinds-and-formats)傳回查詢回應。Grafana 框架中的資料類型定義或宣告包括種類和格式。

面板不必支援每種資料類型。但是，如果您的面板應該支援某種資料類型，我們建議您編寫端對端測試來驗證它是否如預期般運作。

### 「無資料」情境

如果資料來源傳回 [`無資料`](https://grafana.com/developers/dataplane/#no-data-and-empty)，那麼向使用者指示這一點是個好習慣。在以下程式碼片段中，我們測試 Table 面板如何處理「無資料」情境：

```ts
test('在沒有資料回應傳遞給面板的情況下應顯示 "No data"', async ({ panelEditPage, page }) => {
  await panelEditPage.datasource.set('gdev-testdata');
  await panelEditPage.setVisualization('Table');
  await page.getByLabel('Scenario').last().click();
  await page.getByText('No Data Points').click();
  await panelEditPage.refreshPanel();
  await expect(panelEditPage.panel.locator).toContainText('No data');
});
```

#### 多個框架

Table 面板一次只能顯示一個框架。如果有多個框架傳遞給面板，無論是來自同一個查詢還是來自不同的查詢，面板都只會顯示第一個框架。

此外，面板中還會有一個下拉式選單，讓使用者可以在不同的框架之間切換。此行為特定於 Table 面板。

以下程式碼片段測試在有兩個框架傳遞給面板的情況下，插件會顯示一個包含兩個值的下拉式選單：

```ts
test('當有兩個框架傳遞給面板時，應顯示包含兩個值的下拉式選單', async ({
  panelEditPage,
  page,
  selectors,
}) => {
  await panelEditPage.datasource.set('gdev-testdata');
  await panelEditPage.setVisualization('Table');
  await panelEditPage.getQueryEditorRow('A').getByLabel('Alias').fill('a');
  await page.getByText('Add query').click();
  await panelEditPage.getQueryEditorRow('B').getByLabel('Alias').fill('b');
  await panelEditPage.refreshPanel();
  await panelEditPage.panel.locator.getByRole('combobox').click();
  await expect(panelEditPage.getByTestIdOrAriaLabel(selectors.components.Select.option)).toHaveText(['a', 'b']);
});
```
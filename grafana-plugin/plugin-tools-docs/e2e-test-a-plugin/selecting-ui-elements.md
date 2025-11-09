---
id: selecting-elements
title: 選取 UI 元素
description: 如何在端對端測試中選取頁面上的 UI 元素。
keywords:
  - grafana
  - plugins
  - plugin
  - e2e
  - end-to-end
  - example test
sidebar_position: 20
---

# 選取 UI 元素

本指南說明了選擇器在 Grafana 端對端測試中的作用，以及如何使用它們在多個 Grafana 版本中安全地與 UI 元素互動。

## Grafana 端對端選擇器

自 Grafana 7.0.0 起，Grafana 中的端對端測試依賴於 [`@grafana/e2e-selectors`](https://github.com/grafana/grafana/tree/main/packages/grafana-e2e-selectors) 套件中定義的選擇器來選取 Grafana UI 中的元素。

選取 Grafana UI 元素可能具有挑戰性，因為選擇器可能定義在 `aria-label` 屬性或 `data-testid` 屬性上。一開始，選擇器使用 `aria-label` 屬性，但現在大多數選擇器已遷移為使用 `data-testid` 屬性。

## Grafana UI 元素的 Playwright 定位器

`@grafana/plugin-e2e` 定義的所有[頁面](https://github.com/grafana/plugin-tools/tree/main/packages/plugin-e2e/src/models/pages)都公開了一個 `getByGrafanaSelector` 方法。此方法會傳回一個 Playwright [定位器](https://playwright.dev/docs/locators)，它會使用元素上定義的適當 HTML 屬性解析為一或多個元素。每當您想根據 [grafana/e2e-selectors](https://github.com/grafana/grafana/tree/main/packages/grafana-e2e-selectors) 解析 Playwright 定位器時，都應始終使用此方法。

```ts
panelEditPage.getByGrafanaSelector(selectors.components.CodeEditor.container).click();
```

## `selectors` fixture

`@grafana/e2e-selectors` 套件中定義的選擇器與特定的 Grafana 版本相關聯。這意味著選擇器可能會因版本而異，這使得在編寫針對多個 Grafana 版本的測試時很難使用 `@grafana/e2e-selectors` 中定義的選擇器。

為了解決這個問題，`@grafana/plugin-e2e` 有自己的端對端選擇器副本。這些選擇器是 `@grafana/e2e-selectors` 中定義的選擇器的子集，每個選擇器值都定義了最低的 Grafana 版本。當您開始一個新的端對端測試會話時，`@grafana/plugin-e2e` 會檢查正在測試的 Grafana 版本，並解析與正在執行的版本相關聯的選擇器。這些選擇器是透過 `selectors` fixture 提供的。

```ts
import { test, expect } from '@grafana/plugin-e2e';

test('有效的註釋查詢應傳回 200 狀態碼', async ({ annotationEditPage, page, selectors }) => {
  await annotationEditPage.datasource.set('E2E Test Data Source');
  await annotationEditPage.getByGrafanaSelector(selectors.components.CodeEditor.container).fill('SELECT * FROM users');
  await expect(annotationEditPage.runQuery()).toBeOK();
});
```

## 與插件程式碼中定義的 UI 元素互動

如上所述，當您要互動的 UI 元素具有關聯的端對端選擇器時，應始終使用 `getByGrafanaSelector` 方法。但是，許多 Grafana UI 元素沒有端對端選擇器。在這種情況下，我們建議在組合 UI 和編寫測試時，遵循 Grafana 的[測試](https://github.com/grafana/grafana/blob/401265522e584e4e71a1d92d5af311564b1ec33e/contribute/style-guides/testing.md)最佳實踐以及[考量到無障礙的測試](https://github.com/grafana/grafana/blob/401265522e584e4e71a1d92d5af311564b1ec33e/contribute/style-guides/accessibility.md#writing-tests-with-accessibility-in-mind)指南。

### 範圍定位器

為了讓您的測試更穩健，將定位器限定在您的插件上下文中是個好主意。以下範例可能有效，但它很脆弱，因為如果在頁面上您插件之外的某個地方新增了另一個帶有文字 `URL` 的元素，它將不再有效。

```ts
page.getByText('URL').click();
```

有很多方法可以限定選擇器的範圍。您可以將元件包裝在一個帶有 `data-testid` 的 div 中，並在存取元素時使用該 ID。

```ts
page.getByTestId('plugin-url-wrapper').getByText('URL').click();
```

如果您正在測試資料來源查詢編輯器，您可以將定位器限定在查詢編輯器列的範圍內。

```ts
explorePage.getQueryEditorRow('A').getByLabel('Query').fill('sum(*)');
```

### 表單元素範例

以下是一些範例，示範如何與插件中常見的一些 UI 元件互動。`InlineField` 和 `Field` 元件可以互換使用。

#### 輸入欄位

您可以使用 `InlineField` 元件與 UI 互動。

```tsx title="UI 元件"
<InlineField label="Auth key">
  <Input value={value} onChange={handleOnChange} id="config-auth-key" />
</InlineField>
```

```ts title="Playwright 測試"
await page.getByRole('textbox', { name: 'Auth key' }).fill('..');
```

#### 選取欄位

與許多其他需要您傳遞 `id` prop 才能將標籤與表單元素關聯的元件不同，`select` 元件需要您傳遞 `inputId` prop。您可以在[此處](https://github.com/grafana/grafana/blob/401265522e584e4e71a1d92d5af311564b1ec33e/contribute/style-guides/testing.md#testing-select-components)找到有關測試 `select` 元件的更多資訊。

```tsx title="UI 元件"
<InlineField label="Auth type">
  <Select inputId="config-auth-type" value={value} options={options} onChange={handleOnChange} />
</InlineField>
```

```ts title="Playwright 測試"
test('測試選取元件', async ({ page, selectors }) => {
  const configPage = await createDataSourceConfigPage({ type: 'test-datasource' });
  await page.getByRole('combobox', { name: 'Auth type' }).click();
  const option = selectors.components.Select.option;
  await expect(configPage.getByGrafanaSelector(option)).toHaveText(['val1', 'val2']);
});
```

#### 核取方塊欄位

您可以使用 `Checkbox` 元件與 UI 互動。

```tsx title="UI 元件"
<InlineField label="TLS Enabled">
  <Checkbox id="config-tls-enabled" value={value} onChange={handleOnChange} />
</InlineField>
```

在 `Checkbox` 元件中，輸入元素不可點擊，因此您需要透過設定 `force: true` 來繞過 Playwright 的可操作性檢查。

```ts title="Playwright 測試"
await page.getByRole('checkbox', { name: 'TLS Enabled' }).uncheck({ force: true });
await expect(page.getByRole('checkbox', { name: 'TLS Enabled' })).not.toBeChecked();
```

#### InlineSwitch 欄位

您可以使用 `InlineSwitch` 元件與 UI 互動。

```tsx title="UI 元件"
<InlineField label="TLS Enabled">
  <InlineSwitch
    // InlineSwitch 標籤需要與 InlineField 的標籤相符
    label="TLS Enabled"
    value={value}
    onChange={handleOnChange}
  />
</InlineField>
```

與 `Checkbox` 元件一樣，您需要透過設定 `force: true` 來繞過 Playwright 的可操作性檢查。

```ts title="Playwright 測試"
let switchLocator = page.getByLabel('TLS Enabled');
await switchLocator.uncheck({ force: true });
await expect(switchLocator).not.toBeChecked();
```

:::note

在 9.3.0 之前的 Grafana 版本中，`InlineSwitch` 元件中的標籤無法與核取方塊關聯。如果您希望您的測試在 9.3.0 之前的 Grafana 版本中執行，您需要透過以下方式存取該欄位：

:::

```ts title="Playwright 測試"
const label = 'Inline field with switch';
let switchLocator = page.getByLabel(label);
if (semver.lt(grafanaVersion, '9.3.0')) {
  switchLocator = page.locator(`[label="${label}"]`).locator('../label');
}
```
---
id: api
title: 使用 API
description: 如何使用 grafana/plugin-e2e API。
keywords:
  - grafana
  - plugins
  - plugin
  - e2e
  - end-to-end
  - API
sidebar_position: 10
---

`@grafana/plugin-e2e` 套件使用[頁面物件模型](https://playwright.dev/docs/pom)模式來簡化測試的編寫並 облегчить 維護程式碼庫。在頁面物件模型中，應用程式的每個網頁都表示為一個類別檔案。

## 類別

在 `@grafana/plugin-e2e` 套件中，類別代表 Grafana 中的頁面或元件。它們的目的是將常見的 UI 操作封裝在一個地方。它們還處理不同 Grafana 版本之間的 UI 偏差。

該套件會匯出類別，但類別也透過所謂的 [fixtures](https://playwright.dev/docs/test-fixtures) 暴露給 Playwright API。

## Fixtures

`@grafana/plugin-e2e` 套件定義了一組[自訂 fixtures](https://github.com/grafana/plugin-tools/blob/main/packages/plugin-e2e/src/types.ts)，以方便對 Grafana 插件進行端對端測試。

以下部分說明了不同類型的頁面 fixtures：

### 頁面

頁面模型物件可以代表頁面的新執行個體或已存在資源的頁面。若要查看 `@grafana/plugin-e2e` 公開的完整頁面列表，請參閱 Github [儲存庫](https://github.com/grafana/plugin-tools/tree/main/packages/plugin-e2e/src/models/pages)。

#### 在測試中使用頁面類型的新空執行個體

若要在某個類型的新空頁面中開始測試，請使用頁面物件模型名稱的駝峰式表示法。

以下範例使用變數編輯頁面。使用 `variableEditPage` fixture 時，測試將從新儀表板中的空變數編輯表單開始。

```ts
test('測試變數編輯頁面', async ({ variableEditPage }) => {
  await variableEditPage.setVariableType('Query');
});
```

#### 使用現有資源

若要使用指向已存在資源的頁面物件模型開始測試，請使用任何以 `goto` 為前綴的 fixtures。

以下範例使用 `gotoAnnotationEditPage` fixture 來解析 `AnnotationEditPage` 模型。呼叫此 fixture 將導覽至現有儀表板中現有註釋的編輯表單。

```ts
test('測試註釋查詢', async ({ gotoAnnotationEditPage }) => {
  const annotationEditPage = await gotoAnnotationEditPage({ dashboard: { uid: 'trlxrdZVk' }, id: '1' });
  await expect(annotationEditPage.runQuery()).toBeOK();
});
```

若要了解如何使用您需要的資源來佈建 Grafana 執行個體，請參閱[設定資源](./setup-resources.md)指南。

## Expect 匹配器

Playwright API 允許您透過提供自訂匹配器來擴充預設斷言。`@grafana/plugin-e2e` 定義了一組自訂匹配器，可簡化某些頁面的斷言。若要查看完整的匹配器列表，請參閱 Github [儲存庫](https://github.com/grafana/plugin-tools/tree/main/packages/plugin-e2e/src/matchers)。
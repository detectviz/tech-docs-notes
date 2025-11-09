---
id: error-handling-in-app-plugins
title: 應用程式插件中的錯誤處理
description: 如何在應用程式插件中處理錯誤。
keywords:
  - grafana
  - plugins
  - plugin
  - errors
  - error handling
  - app
  - app plugins
---

# 應用程式插件中的錯誤處理

本指南說明如何在應用程式插件中處理錯誤。

## 提供可用的預設值

讓使用者可以循序漸進地學習您的插件。提供有用的預設設定，以便：

- 使用者可以立即開始使用。
- 您可以避免不必要的錯誤訊息。

例如，透過選取預期類型的第一個欄位，面板可以在沒有任何使用者設定的情況下顯示視覺化。如果使用者明確選取一個欄位，則使用該欄位。否則，預設為 `string` 類型的第一個欄位：

```ts
const numberField = frame.fields.find((field) =>
  options.numberFieldName ? field.name === options.numberFieldName : field.type === FieldType.number
);
```

## 顯示錯誤訊息

若要向使用者顯示錯誤訊息，請 `throw` 一個包含您要顯示之訊息的 `Error`：

```ts
throw new Error('發生錯誤');
```

Grafana 會在面板的左上角顯示錯誤訊息：

![面板錯誤。](/img/panel_error.png)

我們建議您避免向使用者顯示過於技術性的錯誤訊息。如果您想讓技術使用者回報錯誤，請考慮改為將其記錄到主控台。

```ts
try {
  failingFunction();
} catch (err) {
  console.error(err);
  throw new Error('發生了問題');
}
```

:::note

Grafana 會按原樣在 UI 中顯示例外訊息，因此請使用文法正確的句子。更多資訊，請參閱[文件樣式指南](https://grafana.com/docs/writers-toolkit/)。

:::

## 另請參閱

如果您的應用程式插件捆綁了其他插件類型，請參閱以下錯誤處理指南：

- [面板插件](../panel-plugins/error-handling-for-panel-plugins.md)

有關向使用者顯示警示的一般性指導，請參閱 [Saga 設計系統](https://grafana.com/developers/saga/patterns/alert/)。
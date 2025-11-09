---
id: extensions-api-deprecated
title: UI 擴充 API 指南 (已棄用元素)
description: UI 擴充參考指南 - 已棄用元素。
keywords:
  - grafana
  - plugins
  - documentation
  - plugin.json
  - API reference
  - UI extensions
sidebar_position: 50
---

:::warning
這些元素已棄用，並已從 Grafana v12 開始移除。
:::

## `getPluginExtensions`

:::warning
此函式已從 Grafana 版本 12 開始移除。請改用 [`usePluginLinks()`](./ui-extensions.md#usepluginlinks) 或 [`usePluginComponents()`](./ui-extensions.md#useplugincomponents) hooks。
:::

此函式會擷取註冊到某個擴充點的擴充（包括連結和元件）。

```typescript
import { getPluginExtensions } from '@grafana/runtime';

const { extensions } = getPluginExtensions({
  extensionPointId: 'grafana/dashboard/panel/menu/v1',
  limitPerPlugin: 2,
  context: {
    panelId: '...',
  },
});
```

### 參數 (Parameters)

`getPluginExtensions()` 函式接受一個 `options` 物件，該物件具有以下屬性：

| 屬性 (Property)        | 描述 (Description)         | 必要 (Required) |
| ---------------------- | --------------------------- | -------- |
| **`extensionPointId`** | 用於擷取連結擴充的唯一 ID。如果您正在實作新的擴充點，這就是外掛程式在註冊擴充時所參考的內容。 **外掛程式必須以此為其外掛程式 ID 的前綴，而核心 Grafana 擴充點必須使用 `"grafana/"` 前綴。** <br /> _範例: `"grafana/dashboard/panel/menu/v1"`_ | 是 (true)     |
| **`context?`**         | 您想與擴充共享的任意物件。這可用於將資料傳遞給擴充。                   | 否 (false)    |
| **`limitPerPlugin?`**  | - 每個外掛程式要傳回的擴充數量上限。預設為無限制。       | 否 (false)    |

### 傳回值 (Return value)

此 hook 會傳回下列物件：

```typescript
const {
  // 如果尚未有任何外掛程式為此擴充點註冊擴充，則為空陣列
  extensions: PluginExtension[];
} = getPluginExtensions(options);
```

如需詳細資訊，請參閱 [`PluginExtension`](https://github.com/grafana/grafana/blob/main/packages/grafana-data/src/types/pluginExtensions.ts#L40)。


## `usePluginExtensions`

:::warning
此 hook 已從 Grafana 版本 12 開始移除。請改用 [`usePluginLinks()`](./ui-extensions.md#usepluginlinks) 或 [`usePluginComponents()`](./ui-extensions.md#useplugincomponents) hooks。
:::

此 react hook 會擷取註冊到某個擴充點的擴充（包括連結和元件）。

```typescript
import { usePluginExtensions } from '@grafana/runtime';

const { extensions, isLoading } = usePluginExtensions({
  extensionPointId: 'grafana/dashboard/panel/menu/v1',
  limitPerPlugin: 2,
  context: {
    panelId: '...',
  },
});
```

### 參數 (Parameters)

`.usePluginExtensions()` 方法接受一個 `options` 物件，該物件具有以下屬性：

| 屬性 (Property)        | 描述 (Description)       | 必要 (Required) |
| ---------------------- |----------------------------------------- | -------- |
| **`extensionPointId`** | 用於擷取連結擴充的唯一 ID。如果您正在實作新的擴充點，這就是外掛程式在註冊擴充時所參考的內容。 **外掛程式必須以此為其外掛程式 ID 的前綴，而核心 Grafana 擴充點必須使用 `"grafana/"` 前綴。** <br /> _範例: `"grafana/dashboard/panel/menu/v1"`_ | 是 (true)     |
| **`context?`**         | 您想與擴充共享的任意物件。這可用於將資料傳遞給擴充。                     | 否 (false)    |
| **`limitPerPlugin?`**  | 每個外掛程式要傳回的擴充數量上限。預設為無限制。         | 否 (false)    |

### 傳回值 (Return value)

此 hook 會傳回下列物件：

```typescript
const {
  // 如果尚未有任何外掛程式為此擴充點註冊擴充，則為空陣列
  extensions: PluginExtension[];

  // 在任何擴充此擴充點的外掛程式
  // 仍在載入中時為 `true`
  isLoading: boolean;
} = usePluginExtensions(options);
```

如需詳細資訊，請參閱 [`PluginExtension`](https://github.com/grafana/grafana/blob/main/packages/grafana-data/src/types/pluginExtensions.ts#L40)。
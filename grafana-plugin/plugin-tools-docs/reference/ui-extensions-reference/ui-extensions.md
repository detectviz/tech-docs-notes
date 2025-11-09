---
id: ui-extensions
title: UI 擴充 API 指南
description: UI 擴充的參考 API 指南。
keywords:
  - grafana
  - plugins
  - documentation
  - plugin.json
  - API reference
  - UI extensions
sidebar_position: 50
---

本頁詳細說明 UI 擴充 API，包括：

- [註冊或公開內容的方法](#i-want-to-register-or-expose-content)
- [呈現內容的掛鉤](#i-want-to-use-renderable-content)

:::note
閱讀[擴充關鍵概念](../../key-concepts/ui-extensions)以取得擴充框架的總覽。
:::

## 我想註冊或公開內容

如果您是外掛程式開發人員，並且希望其他外掛程式或 Grafana Core 從您的應用程式外掛程式呈現連結或元件：

- 使用 `the add*` API 註冊內容 (連結或元件)。如需詳細資訊，請參閱[註冊擴充](../../how-to-guides/ui-extensions/register-an-extension)。
- 使用 `expose*` API 公開元件。如需詳細資訊，請參閱[公開元件](../../how-to-guides/ui-extensions/expose-a-component)。

### `addComponent`

:::info
適用於 Grafana >=v11.1.0。
:::

使用此方法在某個擴充點註冊 [React 元件](https://react.dev/learn/your-first-component)，以貢獻新的 UI 體驗。

```typescript
export const plugin = new AppPlugin<{}>().addComponent({
  targets: ['grafana/user/profile/tab/v1'],
  title: 'New user profile tab',
  description: 'A new tab that shows extended user profile information',
  component: () => {
    return <div>Hello World!</div>;
  },
});
```

#### 參數 (Parameters)

`addComponent()` 方法接受一個 `config` 物件，該物件具有以下屬性：

| 屬性 (Property)   | 描述 (Description)                                                                                                                                                                                  |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`targets`**     | 將註冊擴充的擴充點 ID 清單。 <br /> _範例: `"grafana/dashboard/panel/menu/v1"`_。 [查看 Grafana 中可用的擴充點 &rarr;](./extension-points) |
| **`title`**       | 元件的人類可讀標題。                                                                                                                                                                                |
| **`description`** | 元件的人類可讀描述。                                                                                                                                                                              |
| **`component`**   | 將由擴充點呈現的 [React 元件](https://react.dev/learn/your-first-component)。請注意，傳遞給元件的 props 由每個擴充點定義。  |

#### 傳回值 (Return value)

此方法會傳回 `AppPlugin` 執行個體以允許鏈式呼叫。

#### 範例 (Examples)

- [在元件中存取外掛程式中繼資料](../../how-to-guides/ui-extensions/register-an-extension.md#access-the-plugins-meta-in-a-component)
- [在元件內部存取您的外掛程式狀態](../../how-to-guides/ui-extensions/register-an-extension.md#access-the-plugins-state-in-a-component)
- [在特定條件下隱藏元件](../../how-to-guides/ui-extensions/register-an-extension.md#hide-a-component-in-certain-conditions)

#### 另請參閱 (See also)

- [新增元件的最佳實務](../../how-to-guides/ui-extensions/register-an-extension.md#best-practices-for-adding-components)

### `addLink`

:::info
適用於 Grafana >=v11.1.0。
:::

使用此方法在擴充點註冊連結擴充。

```typescript
export const plugin = new AppPlugin<{}>().addLink({
  targets: ['grafana/dashboard/panel/menu/v1'],
  title: 'Declare incident',
  description: 'Declare an incident and attach the panel context to it',
  path: '/a/myorg-incidents-app/incidents',
});
```

#### 參數 (Parameters)

`addLink()` 方法接受一個 `config` 物件，該物件具有以下屬性：

| 屬性 (Property)   | 描述 (Description)                                                                                                                                                                                                                 | 必要 (Required) |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| **`targets`**     | 將註冊擴充的擴充點 ID 清單。 <br /> _範例: `"grafana/dashboard/panel/menu/v1"`。 [查看 Grafana 中可用的擴充點 &rarr;](./extension-points)_                                | 是 (true)     |
| **`title`**       | 連結的人類可讀標題。                                                                                                                                                                                                               | 是 (true)     |
| **`description`** | 連結的人類可讀描述。                                                                                                                                                                                                             | 是 (true)     |
| **`path?`**       | 您希望在使用者點擊連結時將其傳送到的應用程式外掛程式中的路徑。(使用 `path` 或 `onClick`。) <br /> _範例: `"/a/myorg-incidents-app/incidents"`_                                                | 是 (true)     |
| **`onClick?`**    | 使用者點擊連結時應觸發的回呼。(使用 `path` 或 `onClick`。)                                                                                                                               | 否 (false)    |
| **`category?`**   | 應用於將您的連結與其他連結分組的類別。                                                                                                                                                                | 否 (false)    |
| **`icon?`**       | 顯示連結時應使用的圖示。 <br /> _範例: `"edit"` 或 `"bookmark"`。 [查看所有可用的圖示名稱 &rarr;](https://github.com/grafana/grafana/blob/main/packages/grafana-data/src/types/icon.ts#L1)_ | 否 (false)    |
| **`configure?`**  | 在顯示連結之前呼叫的函式，可讓您根據其 `context` 動態變更或隱藏連結。                                                                                       | 否 (false)    |

#### 傳回值 (Return value)

此方法會傳回 `AppPlugin` 執行個體以允許鏈式呼叫。

#### 範例 (Examples)

- [在特定條件下隱藏連結](../../how-to-guides/ui-extensions/register-an-extension.md#hide-a-link-in-certain-conditions)
- [根據上下文更新路徑](../../how-to-guides/ui-extensions/register-an-extension.md#update-the-path-based-on-the-context)
- [從 `onClick()` 開啟強制回應](../../how-to-guides/ui-extensions/register-an-extension.md#open-a-modal-from-the-onclick)

### `addFunction`

:::info
適用於 Grafana >=v11.6.0。
:::

使用此方法在擴充點註冊函式擴充。

```typescript
export const plugin = new AppPlugin<{}>().addFunction({
  targets: ['grafana/dashboard/dropzone/v1'],
  title: 'Drag and drop data',
  description: 'Support for content being drag and dropped on to dashboards',
  fn: async (data: File) => {
    const text = await data.text();

    return {
      title: 'Text panel',
      panel: {
        type: 'text',
        title: 'Dropped contents',
        options: {
          mode: 'markdown',
          content: text,
        },
      },
      component: PasteEditor(text),
    };
  },
});
```

#### 參數 (Parameters)

`addFunction()` 方法接受一個 `config` 物件，該物件具有以下屬性：

| 屬性 (Property)   | 描述 (Description)                                                                                                                                                                                          | 必要 (Required) |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| **`targets`**     | 將註冊擴充的擴充點 ID 清單。 <br /> _範例: `"grafana/dashboard/panel/menu/v1"`。 [查看 Grafana 中可用的擴充點 &rarr;](./extension-points)_ | 是 (true)     |
| **`title`**       | 函式的人類可讀標題。                                                                                                                                                                                            | 是 (true)     |
| **`description`** | 函式的人類可讀描述。                                                                                                                                                                                          | 是 (true)     |
| **`fn`**          | 當擴充點動作發生時應觸發的應用程式外掛程式中的函式。                                                                                                                                  | 是 (true)     |

#### 傳回值 (Return value)

此方法會傳回 `AppPlugin` 執行個體以允許鏈式呼叫。

#### 範例 (Examples)

- [為函式建立擴充點](../../how-to-guides/ui-extensions/extension-user-use-function#create-an-extension-point-for-functions)

#### 另請參閱 (See also)

- [函式擴充的最佳實務](../../how-to-guides/ui-extensions/extension-user-use-function#best-practices-for-function-extensions)

### `exposeComponent`

:::info
適用於 Grafana >=v11.1.0。
:::

使用此方法公開 React 元件，並使其可供其他外掛程式使用。其他使用者將能夠透過呼叫 `usePluginComponent()` 並參考所公開元件的 `id`，在其擴充點呈現此元件。

```typescript
export const plugin = new AppPlugin<{}>()
    .exposeComponent({
        id: "myorg-incidents-app/create-incident-form/v1",],
        title: "Create incident form",
        description: "A form to create a new incident.",
        component: () => {
            return <div>Hello World!</div>;
        },
    });
```

#### 參數 (Parameters)

`exposeComponent()` 方法接受一個 `config` 物件，該物件具有以下屬性：

| 屬性 (Property)   | 描述 (Description)                                                                                                                                                                                                                                  |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`id`**          | 您要公開的元件的唯一字串識別碼。 **它必須以您的外掛程式 ID 為前綴。** <br /> _範例: `"myorg-incidents-app/create-incident-form/v1"`._                                                               |
| **`title`**       | 元件的人類可讀標題。                                                                                                                                                                                                    |
| **`description`** | 元件的人類可讀描述。                                                                                                                                                                                              |
| **`component`**   | 您要公開的 React 元件。 <br /> _請務必使用元件所依賴的必要 React 內容提供者將其包裝起來，因為此元件不會在與您的外掛程式相同的 React 樹下呈現。_ |

#### 傳回值 (Return value)

此方法會傳回 `AppPlugin` 執行個體以允許鏈式呼叫。

#### 範例 (Examples)

- [在公開的元件中存取外掛程式中繼資訊](../../how-to-guides/ui-extensions/expose-a-component.md#access-plugin-meta-information-in-an-exposed-component)

#### 另請參閱 (See also)

- [公開元件的最佳實務](../../how-to-guides/ui-extensions/expose-a-component.md#best-practices)

## 我想使用可呈現的內容

如果您想在擴充點呈現擴充內容，請使用下列掛鉤：

### `usePluginComponent`

:::info
適用於 Grafana >=v11.1.0。
:::

使用此 React 掛鉤擷取先前由外掛程式使用 `AppPlugin.exposeComponent()` 方法**公開**的單一元件。

```typescript
import { usePluginComponent } from '@grafana/runtime';

const { component: Component, isLoading } = usePluginComponent('myorg-incidents-app/create-incident-form/v1');
```

#### 參數 (Parameters)

- **`id`** - 識別元件的唯一 ID。

#### 傳回值 (Return value)

此掛鉤會傳回下列物件：

```typescript
const {
  // 外掛程式公開的 react 元件
  // (如果沒有使用該 id 公開的元件，則為 `null`)
  component: React.ComponentType<Props> | undefined | null;

  // 在公開元件的外掛程式仍在載入時為 `true`
  isLoading: boolean;
} = usePluginComponent(id);
```

#### 範例 (Examples)

- [如何呈現由另一個外掛程式公開的元件](../../how-to-guides/ui-extensions/use-an-exposed-component.md#use-an-exposed-component)

### `usePluginComponents`

:::info
適用於 Grafana >=v11.1.0。
:::

使用此 react 掛鉤擷取先前已使用 `AppPlugin.addComponent()` 方法在擴充點**註冊**的**元件**。

```typescript
import { usePluginComponents } from '@grafana/runtime';

const { components, isLoading } = usePluginComponents({
  extensionPointId: 'grafana/user/profile/tab/v1',
  limitPerPlugin: 1,
});
```

#### 參數 (Parameters)

`.usePluginComponents()` 方法接受一個 `options` 物件，該物件具有以下屬性：

| 屬性 (Property)        | 描述 (Description)                                                                                                                                                                                                                                                                                                                             | 必要 (Required) |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| **`extensionPointId`** | 用於擷取連結擴充的唯一 ID。如果您正在實作新的擴充點，這就是外掛程式在註冊擴充時所參考的內容。 **外掛程式必須以此為其外掛程式 ID 的前綴，而核心 Grafana 擴充點必須使用 `"grafana/"` 前綴。** <br /> _範例: `"grafana/user/profile/tab/v1"`_ | 是 (true)     |
| **`limitPerPlugin?`**  | - 每個外掛程式要傳回的擴充數量上限。預設為無限制。                                                                                                                                                                                                                                                          | 否 (False)    |

#### 傳回值 (Return value)

此掛鉤會傳回下列物件：

```typescript
const {
  // 如果尚未有任何外掛程式為此擴充點註冊擴充，則為空陣列
  components: PluginExtensionComponent[];

  // 在任何擴充此擴充點的外掛程式
  // 仍在載入中時為 `true`
  isLoading: boolean;
} = usePluginComponents(options);
```

如需詳細資訊，請參閱 [`PluginExtensionComponent`](https://github.com/grafana/grafana/blob/main/packages/grafana-data/src/types/pluginExtensions.ts#L35)。

#### 範例 (Examples)

- [使用 props 將資料傳遞給元件](../../how-to-guides/ui-extensions/extension-user-render-component.md#passing-data-to-the-components)
- [限制哪些外掛程式可以在您的擴充點註冊元件](../../how-to-guides/ui-extensions/extension-user-render-component.md#limit-which-plugins-can-register-components-in-your-extension-point)

#### 另請參閱 (See also)

- [呈現由外掛程式新增的元件的最佳實務](../../how-to-guides/ui-extensions/extension-user-render-component.md#best-practices-for-rendering-components)

### `usePluginLinks`

:::info
適用於 Grafana >=v11.1.0。
:::

使用此 React 掛鉤擷取先前已使用 `AppPlugin.addLink()` 方法在擴充點**註冊**的**連結**。

```typescript
import { usePluginLinks } from '@grafana/runtime';

const { links, isLoading } = usePluginLinks({
  extensionPointId: 'grafana/dashboard/panel/menu/v1',
  limitPerPlugin: 2,
  context: {
    panelId: '...',
  },
});
```

#### 參數 (Parameters)

`.usePluginLinks()` 方法接受一個 `options` 物件，該物件具有以下屬性：

| 屬性 (Property)        | 描述 (Description)                                                                                                                                                                                                                                                                                                                                 | 必要 (Required) |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------- |
| **`extensionPointId`** | 用於擷取連結擴充的唯一 ID。如果您正在實作新的擴充點，這就是外掛程式在註冊擴充時所參考的內容。 **外掛程式必須以此為其外掛程式 ID 的前綴，而核心 Grafana 擴充點必須使用 `"grafana/"` 前綴。** <br /> _範例: `"grafana/dashboard/panel/menu/v1"`_ | 是 (true)     |
| **`context?`**         | 您想與擴充共享的任意物件。這可用於將資料傳遞給擴充。                                                                                                                                                                                                                     | 否 (false)    |
| **`limitPerPlugin?`**  | 每個外掛程式要傳回的擴充數量上限。預設為無限制。                                                                                                                                                                                                                                                                | 否 (false)    |

#### 傳回值 (Return value)

此掛鉤會傳回下列物件：

```typescript
const {
  // 如果尚未有任何外掛程式為此擴充點註冊擴充，則為空陣列
  links: PluginExtensionLink[];

  // 在任何擴充此擴充點的外掛程式
  // 仍在載入中時為 `true`
  isLoading: boolean;
} = usePluginLinks(options);
```

如需詳細資訊，請參閱 [`PluginExtensionLink`](https://github.com/grafana/grafana/blob/main/packages/grafana-data/src/types/pluginExtensions.ts#L27)。

#### 範例 (Examples)

- [將資料傳遞給連結](../../how-to-guides/ui-extensions/create-an-extension-point.md#passing-data-to-links)
- [限制擴充點中的擴充數量](../../how-to-guides/ui-extensions/create-an-extension-point.md#limit-the-number-of-extensions-in-your-extension-point)
- [限制哪些外掛程式可以在您的擴充點註冊連結](../../how-to-guides/ui-extensions/create-an-extension-point.md#limit-which-plugins-can-register-links-in-your-extension-point)

### `usePluginFunctions`

:::info
適用於 Grafana >=v11.6.0。
:::

使用此 React 掛鉤擷取先前已使用 `AppPlugin.addFunction()` 方法在擴充點**註冊**的**函式**。

```typescript
import { usePluginFunctions } from '@grafana/runtime';

const { functions, isLoading } = usePluginFunctions<(data: string) => void>({
  extensionPointId: 'grafana/dashboard/dropzone/v1',
  limitPerPlugin: 2,
});
```

#### 參數 (Parameters)

`.usePluginFunctions()` 方法接受一個 `options` 物件，該物件具有以下屬性：

| 屬性 (Property)        | 描述 (Description)                                                                                                                                                                                                                                                                                                                                 | 必要 (Required) |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------- |
| **`extensionPointId`** | 用於擷取連結擴充的唯一 ID。如果您正在實作新的擴充點，這就是外掛程式在註冊擴充時所參考的內容。 **外掛程式必須以此為其外掛程式 ID 的前綴，而核心 Grafana 擴充點必須使用 `"grafana/"` 前綴。** <br /> _範例: `"grafana/dashboard/panel/menu/v1"`_ | 是 (true)     |
| **`limitPerPlugin?`**  | 每個外掛程式要傳回的擴充數量上限。預設為無限制。                                                                                                                                                                                                                                                                | 否 (false)    |

#### 傳回值 (Return value)

此掛鉤會傳回下列物件：

```typescript
const {
  // 如果尚未有任何外掛程式為此擴充點註冊擴充，則為空陣列
  functions: PluginExtensionFunction[];

  // 在任何擴充此擴充點的外掛程式
  // 仍在載入中時為 `true`
  isLoading: boolean;
} = usePluginLinks(options);
```

如需詳細資訊，請參閱 [`PluginExtensionFunction`](https://github.com/grafana/grafana/blob/main/packages/grafana-data/src/types/pluginExtensions.ts#L46)。

#### 範例 (Examples)

- [為函式建立擴充點](../../how-to-guides/ui-extensions/extension-user-use-function#create-an-extension-point-for-functions)
- [限制哪些外掛程式可以在您的擴充點註冊函式](../../how-to-guides/ui-extensions/extension-user-use-function#limit-which-plugins-can-register-functions-in-your-extension-point)
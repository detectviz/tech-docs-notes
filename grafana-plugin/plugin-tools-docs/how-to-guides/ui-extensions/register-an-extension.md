---
id: register-an-extension
title: 在擴充點中註冊內容
sidebar_label: 在擴充點中註冊內容
description: 在 Grafana OSS 或插件的擴充點中註冊您插件的連結或元件。
keywords:
  - grafana
  - plugins
  - plugin
  - extensions
  - ui-extensions
sidebar_position: 20
---

擴充功能是應用程式插件中的連結或 React 元件。擴充功能會連結至一個擴充點，並且它們會在核心 Grafana UI 或另一個應用程式插件中呈現。它們也可以作為傳回值的函式運作。

您可以註冊或公開一個擴充功能。與[僅公開一個元件](./expose-a-component.md)相比，當您針對一個或多個擴充點 ID 註冊一個擴充功能時，您可以控制誰有權存取您的元件。當希望擴充 Grafana 的核心 UI，或當您需要更多控制權來決定允許什麼使用您插件的擴充功能時，這可能更合適。

在[關鍵概念](../../key-concepts/ui-extensions.md)下閱讀有關擴充功能的更多資訊。

:::warning

您必須[更新](#update-the-pluginjson-metadata)您的 `plugin.json` 元資料以列出任何已註冊的擴充功能。

:::

## 使用連結擴充功能

### 註冊連結擴充功能

以下範例顯示如何將連結新增至儀表板中的面板選單：

1. 在初始化您的插件時註冊連結：

```tsx title="src/module.tsx"
import { PluginExtensionPoints } from '@grafana/data';
import pluginJson from './plugin.json';

export const plugin = new AppPlugin().addLink({
  title: 'My link', // 這會顯示為連結的標籤
  description: 'My links description',
  targets: [PluginExtensionPoints.DashboardPanelMenu], // 在面板選單中顯示它
  path: `/a/${pluginJson.id}/foo`, // 路徑只能指向插件下的某個位置
});
```

2. 使用必要的元資料更新 `plugin.json`：

```json title="src/plugin.json"
{
  ...
  "extensions": {
    "addedLinks": [
      {
        "title": "My link",
        "description": "My links description",
        "targets": ["grafana/dashboard/panel/menu"],
      }
    ]
  }
}
```

### 在某些情況下隱藏連結

您可以使用 `configure()` 函式在某些情況下隱藏連結。

1. 新增一個 `configure()` 函式：

```tsx title="src/module.tsx"
import { PluginExtensionPoints } from '@grafana/data';

export const plugin = new AppPlugin().addLink({
  title: 'My link',
  description: 'My link description',
  targets: [PluginExtensionPoints.DashboardPanelMenu],
  path: `/a/${pluginJson.id}/foo`,
  // `context` 來自擴充點。
  // （傳入 `usePluginLinks({ context })` hook。）
  configure: (context) => {
    // 傳回 `undefined` 將會在擴充點隱藏連結。
    // （在此範例中，我們不會為 "timeseries" 面板顯示連結。）
    if (context?.pluginId === 'timeseries') {
      return undefined;
    }

    // 傳回一個空物件表示我們不更新連結屬性。
    return {};
  },
});
```

2. 確保您的 `plugin.json` 是最新的：
<details>
<summary>src/plugin.json</summary>

```json title="src/plugin.json"
{
  ...
  "extensions": {
    "addedLinks": [
      {
        "title": "My link",
        "description": "My links description",
        "targets": ["grafana/dashboard/panel/menu"],
      }
    ]
  }
}
```

</details>

### 根據上下文更新路徑

1. 新增一個帶有邏輯的 `configure()` 函式：

```tsx title="src/module.tsx"
import { PluginExtensionPoints } from '@grafana/data';

export const plugin = new AppPlugin().addLink({
  title: 'My link',
  description: 'My link description',
  targets: [PluginExtensionPoints.DashboardPanelMenu],
  path: `/a/${pluginJson.id}/foo`,
  configure: (context) => {
    if (context?.pluginId === 'timeseries') {
      // 我們為 "timeseries" 面板呈現一個不同的連結。
      //
      // 注意！只能從 `configure()` 函式更新以下屬性：
      // - title
      // - description
      // - path
      // - icon
      // - category
      return {
        path: `/a/${pluginJson.id}/foo/timeseries`,
      };
    }

    // 傳回一個空物件表示不更新任何屬性。
    return {};
  },
});
```

2. 確保您的 `plugin.json` 是最新的：
<details>
<summary>src/plugin.json</summary>

```json title="src/plugin.json"
{
  ...
  "extensions": {
    "addedLinks": [
      {
        "title": "My link",
        "description": "My links description",
        "targets": ["grafana/dashboard/panel/menu"],
      }
    ]
  }
}
```

</details>

### 從 `onClick()` 開啟一個強制回應視窗

1. 在您的連結設定中新增一個 `onClick()` 處理常式：

```tsx title="src/module.tsx"
import { PluginExtensionPoints } from '@grafana/data';
import { Button, Modal } from '@grafana/ui';

export const plugin = new AppPlugin().addLink({
  title: 'My link',
  description: 'My links description',
  targets: [PluginExtensionPoints.DashboardPanelMenu],
  // `event` - 來自點擊事件的 `React.MouseEvent`
  // `context` - 與擴充功能共用的 `context` 物件
  onClick: (event, { openModal, context }) =>
    openModal({
      title: 'My modal',
      width: 500, // （可選）- 強制回應視窗的寬度（以像素為單位）
      height: 500, // （可選）- 強制回應視窗的高度（以像素為單位）

      // 呼叫 `onDismiss()` 會關閉強制回應視窗
      body: ({ onDismiss }) => (
        <div>
          <div>This is our modal.</div>

          <Modal.ButtonRow>
            <Button variant="secondary" fill="outline" onClick={onDismiss}>
              Cancel
            </Button>
            <Button onClick={onDismiss}>Ok</Button>
          </Modal.ButtonRow>
        </div>
      ),
    }),
});
```

2. 確保您的 `plugin.json` 是最新的：
<details>
<summary>src/plugin.json</summary>

```json
{
  ...
  "extensions": {
    "addedLinks": [
      {
        "title": "My link",
        "description": "My links description",
        "targets": ["grafana/dashboard/panel/menu"],
      }
    ]
  }
}
```

</details>

## 使用元件擴充功能

### 新增元件的最佳實務

- **使用 props** - 檢查擴充點傳遞給元件的 props，並使用它們來實作更量身打造的體驗。
- **使用提供者包裝您的元件** - 如果您想在您的元件中存取任何特定於插件的狀態，請確保使用必要的 React 上下文提供者（例如，適用於 Redux）將其包裝起來。
- **使用 Grafana 擴充點 ID 的列舉** - 如果您要將元件註冊到其中一個可用的 Grafana 擴充點，請確保您使用 [`@grafana/data` 公開的 `PluginExtensionPoints` 列舉](https://github.com/grafana/grafana/blob/main/packages/grafana-data/src/types/pluginExtensions.ts#L121) 套件。

### 註冊元件擴充功能

在以下範例中，我們正在註冊一個簡單的元件擴充功能。

1. 在初始化您的插件時註冊元件：

```tsx title="src/module.tsx"
import { PluginExtensionPoints } from '@grafana/data';

export const plugin = new AppPlugin().addComponent({
  title: 'User profile tab',
  description: 'User profile tab description',
  targets: [PluginExtensionPoints.UserProfileTab],
  component: () => <div>This is a new tab on the user profile page.</div>,
});
```

2. 使用必要的元資料更新 `plugin.json`：

```json title="src/plugin.json"
{
  ...
  "extensions": {
    "addedComponents": [
      {
        "title": "User profile tab",
        "description": "User profile tab description",
        "targets": ["grafana/user/profile/tab"],
      }
    ]
  }
}
```

### 在元件中存取插件的元資料

您可以使用 `usePluginContext()` hook 在您的元件中存取任何特定於插件的元資訊。該 hook 會傳回一個 [`PluginMeta`](https://github.com/grafana/grafana/blob/main/packages/grafana-data/src/types/plugin.ts#L62) 物件。這可能很有用，因為您從插件註冊的元件不會在您插件的 React 樹下呈現，而是在 UI 的其他地方。

1. 在您的元件中使用 `usePluginContext()` hook：

```tsx title="src/module.tsx"
import { usePluginContext, PluginExtensionPoints } from '@grafana/data';

export const plugin = new AppPlugin().addComponent({
  title: 'User profile tab',
  description: 'User profile tab description',
  targets: [PluginExtensionPoints.UserProfileTab],
  component: () => {
    const { meta } = usePluginContext();

    // `jsonData` 屬性是您的插件可以使用
    // Grafana Rest API 管理的物件
    return <div>Plugin specific setting: {meta.jsonData.foo}</div>;
  },
});
```

2. 確保您的 `plugin.json` 是最新的
<details>
<summary>src/plugin.json</summary>

```json title="src/plugin.json"
{
  ...
  "extensions": {
    "addedComponents": [
      {
        "title": "User profile tab",
        "description": "User profile tab description",
        "targets": ["grafana/user/profile/tab"],
      }
    ]
  }
}
```

</details>

### 在元件中存取插件的狀態

1. 使用 `usePluginContext()` hook 存取內容提供者插件的元資訊

```tsx title="src/module.tsx"
import { usePluginContext, PluginExtensionPoints } from '@grafana/data';
import { MyCustomDataProvider } from './MyCustomDataProvider';

export const plugin = new AppPlugin().addComponent({
  title: 'User profile tab',
  description: 'User profile tab description',
  targets: [PluginExtensionPoints.UserProfileTab],
  component: () => {
    const { meta } = usePluginContext();

    return (
      <MyCustomDataProvider>
        <div>Plugin specific setting: {meta.jsonData.foo}</div>
      </MyCustomDataProvider>
    );
  },
});
```

2. 確保您的 `plugin.json` 是最新的：
<details>
<summary>src/plugin.json</summary>

```json title="src/plugin.json"
{
  ...
  "extensions": {
    "addedComponents": [
      {
        "title": "User profile tab",
        "description": "User profile tab description",
        "targets": ["grafana/user/profile/tab"],
      }
    ]
  }
}
```

</details>

### 在某些情況下隱藏元件

只需從您的元件傳回 `null` 即可不呈現任何內容，從而隱藏元件。

1. 更新元件以在某些情境下傳回 `null`：

```tsx title="src/module.tsx"
import { usePluginContext, PluginExtensionPoints } from '@grafana/data';

export const plugin = new AppPlugin().addComponent({
  title: 'User profile tab',
  description: '...',
  targets: [PluginExtensionPoints.UserProfileTab],
  component: () => {
    const { meta } = usePluginContext();

    // 為了範例，此條件依賴於
    // 由您的插件管理的 `jsonData` 屬性
    if (!meta.jsonData.isExtensionEnabled) {
      return null;
    }

    return <div>Plugin specific setting: {meta.jsonData.foo}</div>;
  },
});
```

2. 確保您的 `plugin.json` 是最新的：
<details>
<summary>src/plugin.json</summary>

```json title="src/plugin.json"
{
  ...
  "extensions": {
    "addedComponents": [
      {
        "title": "User profile tab",
        "description": "User profile tab description",
        "targets": ["grafana/user/profile/tab"],
      }
    ]
  }
}
```

</details>

## 更新 plugin.json 元資料

定義連結或元件擴充功能並將其註冊到擴充點後，您必須更新您的 `plugin.json` 元資料。

例如：

```json title="src/plugin.json"
"extensions": {
  "addedLinks": [
    {
      "title": "My app",
      "description": "Link to my app",
      "targets": ["grafana/dashboard/panel/menu"],
    }
  ],
  "addedComponents": [
    {
      "title": "User profile tab",
      "description": "User profile tab description",
      "targets": ["grafana/user/profile/tab"],
    }
  ]
}
```

更多資訊，請參閱 `plugin.json` [參考](../../reference/metadata.md#extensions)。

## 疑難排解

如果您看不到您的連結或元件擴充功能，請檢查以下內容：

1. **檢查主控台日誌** - 您的連結或元件可能因驗證錯誤而未出現。在您的瀏覽器主控台中尋找相關日誌。
2. **檢查 `targets`** - 確保您使用的是正確的擴充點 ID，並始終為 Grafana 擴充點使用 `PluginExtensionPoints` 列舉。
3. **檢查連結的 `configure()` 函式** - 如果您的連結有一個傳回 `undefined` 的 `configure()` 函式，則該連結將被隱藏。
4. **檢查您的元件實作** - 如果您的元件傳回 `null`，它將不會在擴充點呈現。
5. **檢查您是否註冊了太多的連結或元件** - 某些擴充點會限制每個插件允許的連結或元件數量。如果您的插件為同一個擴充點註冊了超過允許數量的連結或元件，其中一些可能會被過濾掉。
6. **檢查 Grafana 版本** - 連結和元件擴充功能僅在 Grafana **`>=10.1.0`** 版本之後才受支援。`addLink()` 和 `addComponent()` 僅在 **>=`11.1.0`** 版本中受支援。
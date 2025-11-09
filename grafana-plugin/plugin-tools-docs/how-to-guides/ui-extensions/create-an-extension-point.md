---
id: create-an-extension-point
title: 在擴充點呈現連結
sidebar_label: 使用擴充點呈現連結
sidebar_position: 10
description: 學習如何提供一個擴充點，以便其他應用程式可以貢獻其擴充功能。
keywords:
  - grafana
  - plugins
  - plugin
  - links
  - extensions
  - extension point
  - app plugins
  - apps
---

擴充點是您插件或 Grafana UI 的一部分，您可以在其中呈現來自其他插件的內容（連結、函式或 React 元件）。使用它們來根據擴充點公開的情境擴充您的使用者體驗。

:::note
在[關鍵概念](../../key-concepts/ui-extensions.md)下閱讀有關擴充功能的更多資訊。<br />
有關參考文件，包括 API，請參閱 [UI 擴充功能參考指南](../../reference/ui-extensions-reference)。
:::

## 呈現連結的最佳實務

- **確保您的 UI 能處理多個連結** <br /> 多個插件可能會將連結新增至您的擴充點。請確保您的擴充點可以處理此情況，並仍然提供良好的使用者體驗。請參閱如何[限制擴充點中的擴充功能數量](#limit-the-number-of-extensions-in-your-extension-point)。
- **分享情境資訊** <br /> 思考哪些情境資訊對其他插件可能有用，並將其新增至 `context` 物件。例如，面板選單擴充點會分享 `panelId` 和 `timeRange`。請注意，`context{}` 物件在傳遞給連結之前總是會被凍結，因此無法被變動。
- **避免不必要的重新呈現** <br />

  - **靜態情境**

    ```ts
    // 如果 `context` 物件只有靜態值，請在元件外部定義它
    const context { foo: 'bar' };

    export const InstanceToolbar = () => {
      const { links, isLoading } = usePluginLinks({ extensionPointId, context });
    ```

  - **動態情境**
    ```ts
    export const InstanceToolbar = ({ instanceId }) => {
      // 當 `context` 物件具有「動態」值時，請務必使用 `useMemo()`
      const context = useMemo(() => ({ instanceId }), [instanceId]);
      const { links, isLoading } = usePluginLinks({ extensionPointId, context });
    ```

## 建立一個擴充點以呈現連結

```tsx
import { usePluginLinks } from '@grafana/runtime';

export const InstanceToolbar = () => {
  // `extensionPointId` 必須有前綴。
  // - 核心 Grafana -> 前綴為 "grafana/"
  // - 插件       -> 前綴為 "{your-plugin-id}/"
  //
  // 這也是插件在呼叫 `addLink()` 時使用的。
  const extensionPointId = 'myorg-foo-app/toolbar/v1';
  const { links, isLoading } = usePluginLinks({ extensionPointId });

  if (isLoading) {
    return <div>載入中...</div>;
  }

  return (
    <div>
      {/* 迴圈遍歷插件新增的連結 */}
      {links.map(({ id, title, path, onClick }) => (
        <a href={path} title={title} key={id} onClick={onClick}>
          {title}
        </a>
      ))}
    </div>
  );
};
```

## 將資料傳遞給連結

```tsx
import { usePluginLinks } from '@grafana/runtime';

export const InstanceToolbar = ({ instanceId }) => {
  const extensionPointId = 'myorg-foo-app/toolbar/v1';
  // 注意！如果 `context` 物件有任何「動態」屬性，請務必使用 `useMemo()`
  // 以防止不必要的重新呈現（否則每次呈現都會建立一個新物件，這可能
  // 導致一個新的 links{} 物件，這可能會觸發新的重新呈現，依此類推。）
  const context = useMemo(() => ({ instanceId }), [instanceId]);
  const { links, isLoading } = usePluginLinks({ extensionPointId, context });

  // ...
};
```

## 限制您擴充點中的擴充功能數量

如果您的 UI 空間有限，您可以限制您擴充點中的擴充功能數量。預設情況下沒有限制。

```tsx
import { usePluginLinks } from '@grafana/runtime';

export const InstanceToolbar = () => {
  // 每個插件只允許一個連結。
  // （如果一個插件註冊了多個連結，那麼其餘的將被忽略
  // 且不會由 hook 傳回。）
  const { links, isLoading } = usePluginLinks({ extensionPointId, limitPerPlugin: 1 });

  // ...
};
```

## 限制哪些插件可以在您的擴充點中註冊連結

```tsx
import { usePluginLinks } from '@grafana/runtime';

export const InstanceToolbar = () => {
  const { links, isLoading } = usePluginLinks({ extensionPointId, limitPerPlugin: 1 });

  // 您可以依賴 `link.pluginId` prop 來根據
  // 註冊擴充功能的插件進行篩選。
  const allowedLinks = useMemo(() => {
    const allowedPluginIds = ['myorg-a-app', 'myorg-b-app'];
    return links.filter(({ pluginId }) => allowedPluginIds.includes(pluginId));
  }, [links]);

  // ...
};
```
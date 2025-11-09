---
id: extension-user-render-component
title: 在擴充點中呈現元件
sidebar_label: 使用擴充點呈現元件
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

## 呈現元件的最佳實務

- **確保您的 UI 控制行為** <br /> 元件擴充功能可以呈現不同的版面配置，並可以回應各種使用者互動。請確保您的 UI 為呈現其他插件定義的元件定義了清晰的界線。
- **分享情境資訊** <br /> 思考哪些情境資訊對其他插件可能有用，並將其作為 `props` 傳遞給元件。

## 建立一個擴充點以呈現元件

```tsx
import { usePluginComponents } from '@grafana/runtime';

export const InstanceToolbar = () => {
  // `extensionPointId` 必須有前綴。
  // - 核心 Grafana -> 前綴為 "grafana/"
  // - 插件       -> 前綴為 "{your-plugin-id}/"
  //
  // 這也是插件在呼叫 `addComponent()` 時使用的
  const extensionPointId = 'myorg-foo-app/toolbar/v1';
  const { components, isLoading } = usePluginComponents({ extensionPointId });

  if (isLoading) {
    return <div>載入中...</div>;
  }

  return (
    <div>
      {/* 迴圈遍歷插件新增的元件 */}
      {components.map(({ id, component: Component }) => (
        <Component key={id} />
      ))}
    </div>
  );
};
```

## 將資料傳遞給元件

```tsx
import { usePluginComponents } from '@grafana/runtime';

// props 的類型（在以下程式碼區塊中作為泛型傳遞給 hook）
type ComponentProps = {
  instanceId: string;
};

export const InstanceToolbar = ({ instanceId }) => {
  const extensionPointId = 'myorg-foo-app/toolbar/v1';
  const { components, isLoading } = usePluginComponents<ComponentProps>({ extensionPointId });

  if (isLoading) {
    return <div>載入中...</div>;
  }

  return (
    <div>
      {/* 使用元件 props 分享情境資訊 */}
      {components.map(({ id, component: Component }) => (
        <Component key={id} instanceId={instanceId} />
      ))}
    </div>
  );
};
```

## 限制哪些插件可以在您的擴充點中註冊元件

```tsx
import { usePluginComponents } from '@grafana/runtime';

export const InstanceToolbar = () => {
  const extensionPointId = 'myorg-foo-app/toolbar/v1';
  const { components, isLoading } = usePluginComponents<ComponentProps>({ extensionPointId });

  // 您可以依賴 `component.pluginId` prop 來根據
  // 註冊擴充功能的插件進行篩選。
  const allowedComponents = useMemo(() => {
    const allowedPluginIds = ['myorg-a-app', 'myorg-b-app'];
    return components.filter(({ pluginId }) => allowedPluginIds.includes(pluginId));
  }, [components]);

  // ...
};
```
---
id: use-an-exposed-component
title: 使用公開的元件
sidebar_label: 使用公開的元件
description: 透過使用公開的元件來重複使用其他插件的功能。
keywords:
  - grafana
  - plugins
  - plugin
  - extensions
  - ui-extensions
sidebar_position: 30
---

應用程式插件可以[透過 React 元件公開額外的功能](./expose-a-component.md)，然後您可以將其匯入您自己的插件中。使用公開的元件來為您自己的應用程式插件增加額外的功能，以擴充使用者工作流程。

## 使用公開的元件

以下範例顯示如何將另一個插件公開的元件呈現在您的擴充點中：

```tsx
import { usePluginComponent } from '@grafana/runtime';

export const MyComponent = () => {
  const { component: Component, isLoading } = usePluginComponent('myorg-basic-app/reusable-component/v1');

  return (
    <>
      <div>My component</div>
      {isLoading ? 'Loading...' : <Component name="John" />}
    </>
  );
};
```

:::tip
有關更多詳細資訊，請[查看 API 參考指南](../../reference/ui-extensions-reference/ui-extensions.md)。
:::
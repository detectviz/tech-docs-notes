---
id: expose-a-component
title: 公開元件
sidebar_label: 公開元件
description: 透過公開元件與其他插件共用功能。
keywords:
  - grafana
  - plugins
  - plugin
  - extensions
  - ui-extensions
sidebar_position: 30
---

作為內容提供者，您可以從您的應用程式插件中公開元件以輕鬆共用功能。

與[註冊擴充功能](./register-an-extension)相比，當您公開元件時，其他插件可以在任何他們想要的地方使用它，而您無需定義要將其掛接至哪個擴充點。這也意味著元件需要更通用，因為它不是針對特定的擴充點。

## 最佳實務

- **使用提供者包裝您的元件** - 如果您想在您的元件中存取任何特定於插件的狀態，請確保使用必要的 React 上下文提供者（例如，Redux 的包裝器）將其包裝起來。

## 從應用程式插件公開元件

您可以從同一個應用程式插件中公開一個或多個元件。例如：

```tsx
import pluginJson from './plugin.json';

export const plugin = new AppPlugin()
  // 您也可以從同一個應用程式插件中公開多個元件
  .exposeComponent({
    // 重要！
    // `id` 應始終以您的插件 ID 為前綴，否則它將不會被公開。
    id: `${pluginJson.id}/reusable-component/v1`,
    title: 'Reusable component',
    description: 'A component that can be reused by other app plugins.',
    component: ({ name }: { name: string }) => <div>Hello {name}!</div>,
  });
```

:::tip
有關更多詳細資訊，請[查看 API 參考指南](../../reference/ui-extensions-reference/ui-extensions.md)。
:::

## 在公開的元件中存取插件元資訊

您可以存取擴充元件的元資料。例如：

```tsx
import { usePluginContext } from "@grafana/runtime";
import pluginJson from './plugin.json';

export const plugin = new AppPlugin()
  .exposeComponent({
    id: `${pluginJson.id}/reusable-component/v1`,
    title: 'Reusable component',
    description: 'A component that can be reused by other app plugins.',
    component: ({ name }: { name: string }) => {
      // 這是公開元件的應用程式插件的元資訊
      const { meta } = usePluginContext();

      return (
        <div>Hello {name}!</div>
        <div>Version {meta.info.version}</div>
      );
    }
  })
```

:::tip
有關更多詳細資訊，請[查看 API 參考指南](../../reference/ui-extensions-reference/ui-extensions.md)。
:::
---
id: expose-a-lazy-loaded-component
title: 公開一個延遲載入的元件
sidebar_label: 公開一個延遲載入的元件
description: 公開一個延遲載入的元件以與其他插件共用功能。
keywords:
  - grafana
  - plugins
  - plugin
  - extensions
  - ui-extensions
  - lazy-loading
  - performance
sidebar_position: 31
---

您可以從您的應用程式插件中公開一個延遲載入的元件，以與其他插件共用功能，而不會影響初始載入時間。當元件很大或並非總是需要時，這非常有用。

:::note
為了讓延遲載入能有效地減少 module.js 檔案大小，請確保您的應用程式插件及其路由已經是延遲載入的。如果應用程式插件不是延遲載入的，公開的元件程式碼可能仍然會在其他地方被靜態匯入，從而限制了效能優勢。
:::

## 公開一個延遲載入的元件

若要公開一個延遲載入的元件，您可以使用 `React.lazy` 來動態匯入該元件，然後在將其傳遞給 `exposeComponent` 方法之前，將其包裝在一個 `Suspense` 元件中。

```tsx
import React, { Suspense } from 'react';
import { AppPlugin } from '@grafana/runtime';

// 延遲載入您的元件
const MyLazyComponent = React.lazy(() => import('./MyLazyComponent'));

const SuspendedComponent = () => (
  <Suspense fallback={<div>載入中...</div>}>
    <MyLazyComponent />
  </Suspense>
);

export const plugin = new AppPlugin().exposeComponent({
  id: 'my-plugin/my-lazy-component/v1',
  title: 'My Lazy Component',
  description: 'A component that is loaded on demand.',
  component: SuspendedComponent,
});
```

:::note
您應該使用相同的模式來使用 `addComponent` 方法新增元件。
:::

## 使用延遲載入的元件

從消費者的角度來看，使用延遲載入和非延遲載入的元件沒有區別。無論元件是否延遲載入，`usePluginComponent` 或 `usePluginComponents` hook 的運作方式都相同。有關使用插件元件的更多資訊，請參閱[在擴充點中呈現元件](./extension-user-render-component)文件。
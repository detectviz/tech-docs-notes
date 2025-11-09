---
id: getting-started
title: 設定 Scenes
slug: /
---

本主題說明如何安裝 Scenes 並建立您的第一個「Hello World」場景。

## 安裝

使用 [`@grafana/create-plugin`](https://github.com/grafana/plugin-tools/blob/main/packages/create-plugin/README.md) 來開始一個全新的專案。

```bash
npx @grafana/create-plugin@latest
```

或者，透過在您的專案中執行以下命令，將 @grafana/scenes 新增至您的 Grafana 應用程式外掛程式：

```bash
yarn add @grafana/scenes
```

## Hello World 場景

以下說明如何設定「Hello World」場景。

### 1. 建立場景

使用下方的程式碼片段建立您的第一個場景。以下程式碼將建立一個場景，其中包含一個位於彈性佈局中的 Grafana 文字面板：

```ts
// helloWorldScene.ts

import { EmbeddedScene, SceneFlexLayout, SceneFlexItem, VizPanel, PanelBuilders } from '@grafana/scenes';

export function getScene() {
  return new EmbeddedScene({
    body: new SceneFlexLayout({
      children: [
        new SceneFlexItem({
          width: '50%',
          height: 300,
          body: PanelBuilders.text().setTitle('Panel title').setOption('content', 'Hello world!').build(),
        }),
      ],
    }),
  });
}
```

### 2. 渲染場景

在您的 Grafana 應用程式外掛程式頁面中使用以下程式碼來渲染「Hello World」場景：

```tsx
import React from 'react';
import { getScene } from './helloWorldScene';

export const HelloWorldPluginPage = () => {
  const scene = getScene();

  return <scene.Component model={scene} />;
};
```

:::note
渲染後的場景不會在 Grafana 外掛程式頁面中呈現。要將場景與 Grafana 側邊欄、導覽和外掛程式頁面整合，請遵循 [Scenes 應用程式](./scene-app.md) 指南。
:::

## 原始碼

[檢視範例原始碼](https://github.com/grafana/scenes/tree/main/docusaurus/docs/getting-started.tsx)
---
id: getting-started
title: 入門指南
---

`@grafana/scenes-ml` 是一個獨立的 npm 套件，可讓您將機器學習驅動的功能新增至您的場景中。

本主題說明如何安裝 Scenes ML 並在 Grafana 應用程式外掛程式中使用它。

## 安裝

如果您要將 Scenes ML 新增至現有的 Scenes 應用程式外掛程式，請先執行以下命令以確保您的外掛程式設定為最新狀態：

```bash
npx @grafana/create-plugin@latest --update
```

然後透過在您的專案中執行以下命令，將 `@grafana/scenes-ml` 新增至您的外掛程式：

```bash
yarn add @grafana/scenes-ml
```

## 將 ML 功能新增至場景

### 1. 建立場景

使用以下程式碼片段建立場景。這將為場景新增一個時間序列面板，並內建控制項，可為面板中的所有序列新增趨勢、下限和上限。

```ts
// helloMLScene.ts

import {
  EmbeddedScene,
  SceneFlexLayout,
  SceneFlexItem,
  SceneQueryRunner,
  PanelBuilders,
  sceneUtils,
} from '@grafana/scenes';
import { SceneBaseliner, MLDemoDS } from '@grafana/scenes-ml';

// 註冊 `scenes-ml` 中的示範資料來源。
// 這不是正常使用所必需的，它只是為我們提供了一些合理的示範資料。
sceneUtils.registerRuntimeDataSource({ dataSource: new MLDemoDS('ml-test', 'ml-test') });

function getForecastQueryRunner() {
  return new SceneQueryRunner({
    queries: [{ refId: 'A', datasource: { uid: 'ml-test', type: 'ml-test' }, type: 'forecasts' }],
  });
}

export function getScene() {
  return new EmbeddedScene({
    body: new SceneFlexLayout({
      children: [
        new SceneFlexItem({
          width: '50%',
          height: 300,
          body: PanelBuilders.timeseries()
            .setTitle('預測示範')
            .setData(getForecastQueryRunner())
            // 將 `SceneBaseliner` 新增至面板。
            .setHeaderActions([new SceneBaseliner({ interval: 0.95 })])
            .build(),
        }),
      ],
    }),
  });
}
```

### 2. 渲染場景

在您的 Grafana 應用程式外掛程式頁面中使用以下程式碼來渲染「Hello ML」場景：

```tsx
import React from 'react';
import { getScene } from './helloMLScene';

export const HelloMLPluginPage = () => {
  const scene = getScene();

  return <scene.Component model={scene} />;
};
```

## 原始碼

[檢視範例原始碼](https://github.com/grafana/scenes/tree/main/docusaurus/docs/scenes-ml/getting-started.tsx)
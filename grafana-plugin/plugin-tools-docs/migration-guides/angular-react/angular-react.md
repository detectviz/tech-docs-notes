---
id: migrate-angularjs-to-react
title: 從 AngularJS 遷移至 React
sidebar_position: 3
description: 如何將 Grafana 插件從 AngularJS 遷移至 React。
keywords:
  - grafana
  - plugins
  - plugin
  - React
  - ReactJS
  - Angular
  - migration
---

import DocCardList from '@theme/DocCardList';

# 從 AngularJS 遷移至 React

如果您想將插件遷移至 Grafana 基於 React 的插件平台，我們建議您在新發布的主要版本下進行。

雖然沒有從 Angular 插件到新的 React 平台的標準遷移路徑，但我們發現最簡單的遷移方法之一是：

1. 建立一個名為 `migrate-to-react` 的新分支。
2. 使用 [`create-plugin`](https://www.npmjs.com/package/@grafana/create-plugin) 工具提供的其中一個範本從頭開始。
3. 將現有程式碼逐步地、一次一個元件地移至新插件中。

## 遷移面板插件

自 Grafana 7.0 起，插件會從 `module.ts` 匯出一個 `PanelPlugin`，其中 `MyPanel` 是一個包含 `PanelProps` 中 props 的 React 元件。

```ts title="src/module.ts"
import { PanelPlugin } from '@grafana/data';

export const plugin = new PanelPlugin<MyOptions>(MyPanel);
```

```ts title="src/MyPanel.tsx"
import { PanelProps } from '@grafana/data';

interface Props extends PanelProps<SimpleOptions> {}

export function MyPanel({ options, data, width, height }: Props) {
  // ...
}
```

## 遷移資料來源插件

雖然所有插件都不同，但我們想分享一個對某些使用者有效的遷移流程。

1. 定義您的設定模型和 `ConfigEditor`。對於許多插件來說，設定編輯器是最簡單的元件，因此從它開始是個不錯的選擇。
2. 在擴充 `DataSourceApi` 的類別上實作 `testDatasource()` 方法。使用您設定模型中的設定來確保您可以成功設定和存取外部 API。
3. 實作 `query()` 方法。此時，您可以硬式編碼您的查詢，因為我們尚未實作查詢編輯器。`query()` 方法支援新的資料框架回應和舊的 `TimeSeries` 回應，因此暫時不用擔心轉換為新格式。
4. 實作 `QueryEditor`。這需要多少工作量取決於您的查詢模型有多複雜。

到目前為止，您應該能夠發布您的新版本。

若要完全遷移至新的插件平台，請將時間序列回應轉換為資料框架回應。

## 指南

以下指南將幫助您執行特定的遷移動作。

<DocCardList />
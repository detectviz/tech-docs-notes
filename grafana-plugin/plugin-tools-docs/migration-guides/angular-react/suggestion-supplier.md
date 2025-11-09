---
id: add-suggestion-supplier
title: 新增建議提供者
sidebar_position: 9
description: 如何為插件新增建議提供者。
keywords:
  - grafana
  - plugins
  - plugin
  - React
  - ReactJS
  - Angular
  - migration
  - suggestion
---

# Angular 至 React：新增建議提供者

您可以新增一個建議提供者來檢查來自面板的查詢資料，並根據偵測到的資料類型建議使用該插件。本指南提供了執行此操作的說明以及相關範例的連結。

一個很好的範例是 `stat` 面板，它會檢查查詢結果，並為單一系列給予「高」評價，為多個系列（或甚至沒有）給予「低」評價。

## 新增建議提供者

以下是作為 `module.ts` 一部分的建議提供者範例：

```ts
import { MyDataSuggestionsSupplier } from './suggestions';
...

.setSuggestionsSupplier(new MyDataSuggestionsSupplier());
```

以下是源自 polystat 的建議提供者範例：

```ts
import { VisualizationSuggestionsBuilder } from '@grafana/data';
import { MyOptions } from './types';

export class MyDataSuggestionsSupplier {
  getSuggestionsForData(builder: VisualizationSuggestionsBuilder) {
    const { dataSummary: ds } = builder;

    if (!ds.hasData) {
      return;
    }
    if (!ds.hasNumberField) {
      return;
    }

    const list = builder.getListAppender<MyOptions, {}>({
      name: 'MyPanel',
      pluginId: 'myorg-description-panel',
      options: {},
    });

    list.append({
      name: 'MyPanel',
    });
  }
}
```

:::note

建立建議提供者時，請務必確定插件確實可以為所提供的資料呈現某些內容。

如果建議提供者錯誤地將插件評為高分，最終結果通常會顯示一個空白面板和/或一則錯誤訊息。

最好只向符合插件可以處理和視覺化的已知標準的查詢資料提供插件。

:::

## 其他資源

參考這些建議提供者以取得進一步自訂的想法：

- [圓餅圖面板](https://github.com/grafana/grafana/blob/main/public/app/plugins/panel/piechart/suggestions.ts#L7)

圓餅圖面板會檢查查詢是否有超過 30 列的傳回，並且不會將自己作為視覺化選項提供，即使它可以顯示資料，但幾乎無法閱讀。

- [統計面板](https://github.com/grafana/grafana/blob/main/public/app/plugins/panel/stat/suggestions.ts#L7)

與圓餅圖面板類似，此插件為少於 10 列的資料列結果提供自己。它還會根據查詢結果中欄位的類型設定預設選項。

- [熱圖面板](https://github.com/grafana/grafana/blob/main/public/app/plugins/panel/heatmap/suggestions.ts#L8)

此面板會對資料進行一些處理，如果產生任何警告，它會省略自己不被提供。
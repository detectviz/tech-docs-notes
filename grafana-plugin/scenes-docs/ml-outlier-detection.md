---
id: outlier-detection
title: 離群值偵測
---

import PinnedSvg from '/static/img/pinned.svg';

**離群值偵測**是識別群組中一個或多個序列行為與其他序列不同的問題。

`scenes-ml` 提供了一個 `SceneOutlierDetector` 元件，它將執行離群值偵測並突顯任何行為異常的序列。它還會新增一個灰色帶，表示「叢集範圍」(可視為非離群值的資料範圍)，並 (可選地) 在偵測到離群值的時間範圍內新增註釋。

![已新增離群值的面板](/img/outliers.png)

## 用法

以下程式碼範例示範如何將離群值偵測新增至時間序列面板。

```ts
import { SceneOutlierDetector } from '@grafana/scenes-ml';

// 此處顯示預設值，所有值都是可選的。
const outlierDetector = new SceneOutlierDetector({
  sensitivity: 0.5,
  addAnnotations: false,
  pinned: false,
  onOutlierDetected: (outlier: Outlier) => {},
});
const panel = PanelBuilders.timeseries().setHeaderActions([outlierDetector]).build();
```

:::note
請確定您只將離群值偵測新增至**時間序列**面板，因為它對於其他面板類型很少有意義。
:::

### 釘選結果

預設情況下，基線會在每次狀態變更時重新計算，也就是說，每當時間範圍、查詢或間隔變更時。這並不總是可取的：例如，使用者可能想要縮小並檢視未來時間範圍內的目前預測。

啟用**釘選 <PinnedSvg className="ml-icon" />** 設定將凍結目前的結果，因此在變更時間範圍或其他設定時不會重新計算它們。

## 技術細節

`scenes-ml` 目前使用 [DBSCAN][dbscan] 演算法的變體來偵測離群值。未來可能會新增其他演算法。

[dbscan]: https://en.wikipedia.org/wiki/DBSCAN
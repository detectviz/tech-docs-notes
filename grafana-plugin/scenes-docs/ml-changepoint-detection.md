---
id: changepoint-detection
title: 變更點偵測
---

import PinnedSvg from '/static/img/pinned.svg';

**變更點偵測**試圖識別時間序列行為發生變化的時間戳記。例如，它可以用於識別時間序列的**幅度**或**變異數**的突然變化。

`scenes-ml` 中的 `SceneChangepointDetector` 元件可用於將此功能新增至面板中的所有序列。此元件將在每個偵測到的變更點新增註釋。

![已新增變更點的面板](/img/changepoints.png)

:::warning
變更點偵測目前為測試版功能。對於某些面板，底層演算法的執行速度可能會很慢，因此在使用前請務必徹底測試。
:::

## 用法

以下程式碼範例示範如何將變更點偵測新增至時間序列面板。

```ts
import { SceneChangepointDetector } from '@grafana/scenes-ml';

// 此處顯示預設值，所有值都是可選的。
const changepointDetector = new SceneChangepointDetector({
  enabled: false,
  pinned: false,
  onChangepointDetected: (changepoint: Changepoint) => {},
});
const panel = PanelBuilders.timeseries().setHeaderActions([outlierDetector]).build();
```

:::note
請確定您只將變更點偵測新增至**時間序列**面板，因為它對於其他面板類型很少有意義。
:::

### 釘選結果

預設情況下，基線會在每次狀態變更時重新計算，也就是說，每當時間範圍、查詢或間隔變更時。這並不總是可取的：例如，使用者可能想要縮小並檢視未來時間範圍內的目前預測。

啟用**釘選 <PinnedSvg className="ml-icon" />** 設定將凍結目前的結果，因此在變更時間範圍或其他設定時不會重新計算它們。

## 技術細節

`scenes-ml` 目前使用[自迴歸高斯過程變更點偵測][argpcp] (ARGPCP) 演算法，在某些情況下可能會很慢。未來可能會新增替代演算法。

[argpcp]: https://redpoll.ai/blog/changepoint/#autoregressive-gaussian-process-change-point-detector-argpcp
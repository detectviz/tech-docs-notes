---
id: baselines-and-forecasts
title: 基線與預測
---

import DiscoverSeasonalitiesSvg from '/static/img/discover-seasonalities.svg';
import PinnedSvg from '/static/img/pinned.svg';

**基線**提供時間序列資料的平滑估計值，以及資料隨時間變化的下限和上限。它們也可以用作**預測**，利用歷史資料來預測未來資料的行為。

此功能也可用於**異常偵測**，方法是突顯真實值偏離預測下限和上限的時間戳記。

可以使用 `scenes-ml` 的 `SceneBaseliner` 元件將基線新增至面板，這將新增一個控制項來啟用/停用計算、調整預測區間、發現季節性並釘選結果。

![已新增基線的面板](/img/baseliner.png)

## 用法

以下程式碼範例示範如何將基線新增至時間序列面板。

```ts
import { SceneBaseliner } from '@grafana/scenes-ml';

// 此處顯示預設值，所有值都是可選的。
const baseliner = new SceneBaseliner({
  interval: 0.95,
  discoverSeasonalities: false,
  pinned: false,
});
const panel = PanelBuilders.timeseries().setHeaderActions([baseliner]).build();
```

:::note
請確定您只將基線新增至**時間序列**面板，因為它們對於其他面板類型很少有意義。
:::

### 釘選結果

預設情況下，每次狀態變更時都會重新計算基線，也就是說，每當時間範圍、查詢或間隔變更時。這並不總是可取的：例如，使用者可能想要縮小並檢視未來時間範圍內的目前預測。

啟用**釘選 <PinnedSvg className="ml-icon" />** 設定將凍結目前的結果，因此在變更時間範圍或其他設定時不會重新計算它們。

## 技術細節

`scenes-ml` 使用 [MSTL][mstl] 演算法來產生樣本內和樣本外預測。此演算法將資料分解為**趨勢**、**季節性**和**殘差**，然後使用 [ETS][ets] 演算法來建立趨勢序列的模型。

預設情況下，演算法假設**每小時**、**每天**、**每週**和**每年**的季節性 (如果資料跨越至少兩個給定的季節長度，即每小時至少兩個小時或每天至少兩天)。

如果啟用**發現季節性 <DiscoverSeasonalitiesSvg className="ml-icon"/>** 設定，基線器將首先嘗試使用[週期圖][periodogram]偵測資料中的任何非標準季節性，並在建立資料模型時考慮這些季節性。

[mstl]: https://arxiv.org/abs/2107.13462
[ets]: https://otexts.com/fpp3/ets-forecasting.html
[periodogram]: https://www.sktime.net/en/latest/api_reference/auto_generated/sktime.param_est.seasonality.SeasonalityPeriodogram.html
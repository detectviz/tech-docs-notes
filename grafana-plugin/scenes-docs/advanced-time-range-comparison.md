---
id: advanced-time-range-comparison
title: 時間範圍比較
---

Scenes 附帶 `SceneTimeRangeCompare` 物件，可讓您使用與 `SceneTimeRange` 物件提供的時間範圍不同的時間範圍來執行 `SceneQueryRunner` 查詢。使用 `SceneTimeRangeCompare` 物件，您可以設定要對查詢執行比較的比較時間視窗。這允許將來自第二個時間範圍的資料顯示在視覺化中。

## 在場景中使用 `SceneTimeRangeCompare`

1. 建立一個包含時間範圍和查詢執行器的場景

首先建立一個包含 `SceneTimeRange` 和 `SceneQueryRunner` 的場景：

```tsx
const queryRunner = new SceneQueryRunner({
  datasource: {
    type: 'prometheus',
    uid: 'gdev-prometheus',
  },
  queries: [
    {
      refId: 'A',
      expr: 'rate(prometheus_http_requests_total{}[5m])',
    },
  ],
});

const scene = new EmbeddedScene({
  $data: queryRunner,
  $timeRange: new SceneTimeRange({ from: 'now-5m', to: 'now' }),
  body: new SceneFlexLayout({
    direction: 'row',
    children: [
      new SceneFlexItem({
        width: '50%',
        height: 300,
        body: PanelBuilders.timeseries().setTitle('使用全域時間範圍的面板').build(),
      }),
    ],
  }),
});
```

2. 將時間選擇器新增至場景控制項

使用 `SceneTimePicker` 物件來顯示和控制場景的時間範圍：

```tsx
const scene = new EmbeddedScene({
  $data: queryRunner,
  $timeRange: new SceneTimeRange({ from: 'now-5m', to: 'now' }),
  controls: [new SceneTimePicker({})]
  body: new SceneFlexLayout({
    direction: 'row',
    children: [
      new SceneFlexItem({
        width: '50%',
        height: 300,
        body: PanelBuilders.timeseries().setTitle('使用全域時間範圍的面板').build(),
      }),
    ],
  }),
});
```

3. 將時間視窗比較選擇器新增至場景控制項

建立一個 `SceneTimeRangeCompare` 場景物件，並將其新增至場景控制項中 `SceneTimePicker` 的旁邊：

```tsx
const scene = new EmbeddedScene({
  $data: queryRunner,
  $timeRange: new SceneTimeRange({ from: 'now-5m', to: 'now' }),
  controls: [new SceneTimePicker({}), new SceneTimeRangeCompare({})],
  body: new SceneFlexLayout({
    direction: 'row',
    children: [
      new SceneFlexItem({
        width: '100%',
        height: '100%',
        body: PanelBuilders.timeseries().setTitle('使用全域時間範圍的面板').build(),
      }),
    ],
  }),
});
```

時間範圍比較選擇器應顯示在時間範圍選擇器旁邊。按一下 **時間範圍比較** 核取方塊以啟用時間範圍比較，然後選擇要與目前選取的時間範圍進行比較的比較時間視窗。

## 自訂比較序列的樣式

您可以透過[設定覆寫](./visualizations.md#step-7-configure-overrides)來自訂在視覺化上呈現的比較序列的樣式。使用 `matchComparisonQuery(queryRefId: string)` 匹配器來鎖定比較查詢結果：

```tsx
const queryRunner = new SceneQueryRunner({
  datasource: {
    type: 'prometheus',
    uid: 'gdev-prometheus',
  },
  queries: [
    {
      refId: 'MyQuery',
      expr: 'rate(prometheus_http_requests_total{}[5m])',
    },
  ],
});

const panelShowingComparisonSeries = PanelBuilders.timeseries()
  .setData(queryRunner)
  .setOverrides((b) =>
    b.matchComparisonQuery('MyQuery').overrideColor({
      mode: 'fixed',
      fixedColor: 'red',
    })
  );
```

## 原始碼

[檢視範例原始碼](https://github.com/grafana/scenes/tree/main/docusaurus/docs/advanced-time-range-comparison.tsx)
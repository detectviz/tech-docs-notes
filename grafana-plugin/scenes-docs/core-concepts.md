---
id: core-concepts
title: 核心概念
---

本主題說明 Scenes 的核心概念，以及如何使用它們來建立您自己的場景。

## 場景 (Scene)

一個場景是由稱為「場景物件」(_scene objects_) 的物件集合所組成。這些物件代表場景的不同方面：資料、時間範圍、變數、版面配置和視覺化。場景物件共同形成一個「物件樹」(_object tree_)：

![Scene objects tree](/img/sceneTree.png)

Scenes 可讓您將物件分組和巢狀化。像資料、時間範圍或變數等項目可以新增至樹中的任何物件，使其可用於該物件及其所有後代物件。因此，Scenes 可讓您建立具有多個時間範圍、可共用和轉換的查詢，或巢狀變數的儀表板。

@grafana/scenes 隨附多個物件 (如 `SceneQueryRunner`、`SceneFlexLayout`、`VizPanel` 等) 來解決常見問題。然而，您也可以建立自己的場景物件來擴充功能。

## 場景物件 (Scene object)

場景是由稱為場景物件 (scene objects) 的原子物件所建構。一個場景物件的定義包含：

- **狀態 (State)** - 一個擴充 `SceneObjectState` 的介面。

```tsx
import { SceneObjectState } from '@grafana/scenes';

// 1. 建立描述場景物件狀態的介面
interface CounterState extends SceneObjectState {
  count: number;
}
```

- **模型 (Model)** - 一個擴充 `SceneObjectBase` 類別的類別。模型包含場景物件的邏輯。

```tsx
import { SceneObjectBase } from '@grafana/scenes';

export class Counter extends SceneObjectBase<CounterState> {
  public static Component = CounterRenderer;

  public constructor() {
    super({
      count: 0,
    });
  }

  public onIncrement = () => {
    this.setState({
      count: this.state.count + 1,
    });
  };
}
```

- **React 元件** - 用於渲染場景物件。

```tsx
import React from 'react';
import { SceneComponentProps } from '@grafana/scenes';

function CounterRenderer({ model }: SceneComponentProps<Counter>) {
  const { count } = model.useState();

  return (
    <div>
      <div>Counter: {count}</div>
      <button onClick={model.onIncrement}>Increase</button>
    </div>
  );
}
```

## 狀態 (State)

場景物件可以有關聯的狀態。物件狀態的形狀是透過一個「必須」擴充 `SceneObjectState` 介面的介面來表示：

```tsx
interface CounterState extends SceneObjectState {
  count: number;
}
```

### 訂閱狀態變更

元件可以透過渲染時收到的 `model` 屬性來讀取場景物件的狀態。若要訂閱狀態變更，請呼叫 `model.useState` 方法：

```tsx
function CounterRenderer({ model }: SceneComponentProps<Counter>) {
  const { count } = model.useState();

  // ...
}
```

使用 `model.useState()` 訂閱物件的狀態將使元件對狀態變更產生反應。場景物件狀態的每次變更都是不可變的，並且會導致元件重新渲染。

### 修改狀態

若要變更場景物件的狀態，請使用每個場景物件都具備的 `setState` 方法。這可以直接從元件中完成：

```tsx
function CounterRenderer({ model }: SceneComponentProps<Counter>) {
  const { count } = model.useState();
  const onIncrement = () => model.setState({ count: count + 1 });

  // ...
}
```

這也可以從場景物件類別中完成：

```tsx
export class Counter extends SceneObjectBase<CounterState> {
  // ...
  public onIncrement = () => {
    this.setState({
      count: this.state.count + 1,
    });
  };
}

function CounterRenderer({ model }: SceneComponentProps<Counter>) {
  const { count } = model.useState();

  return (
    <div>
      <div>Counter: {count}</div>
      <button onClick={model.onIncrement}>Increase</button>
    </div>
  );
}
```

:::note
我們建議您在場景物件中實作狀態修改方法，而不是在元件中，以將模型的複雜性與元件分離。
:::

## 資料和時間範圍

使用 `$data` 屬性將來自 Grafana 資料來源的資料新增至場景中。查詢是使用 `SceneQueryRunner` 場景物件進行設定的：

```tsx
import { SceneQueryRunner } from '@grafana/scenes';

const queryRunner = new SceneQueryRunner({
  datasource: {
    type: 'prometheus',
    uid: '<PROVIDE_GRAFANA_DS_UID>',
  },
  queries: [
    {
      refId: 'A',
      expr: 'rate(prometheus_http_requests_total{}[5m])',
    },
  ],
});
```

:::note
您的 Grafana 執行個體必須已設定指定的資料來源。
:::

為了讓 `SceneQueryRunner` 正常運作，您必須將時間範圍新增至場景中。每個場景物件都有一個 `$timeRange` 屬性，可以將 `SceneTimeRange` 場景物件新增至該屬性。若要為上一個範例中建立的查詢執行器指定時間範圍，請在傳遞給建構函式的物件中新增 `$timeRange` 屬性：

```tsx
import { SceneQueryRunner, SceneTimeRange } from '@grafana/scenes';

const queryRunner = new SceneQueryRunner({
  datasource: {
    type: 'prometheus',
    uid: '<PROVIDE_GRAFANA_DS_UID>',
  },
  queries: [
    {
      refId: 'A',
      expr: 'rate(prometheus_http_requests_total{}[5m])',
    },
  ],
  $timeRange: new SceneTimeRange({ from: 'now-5m', to: 'now' }),
});
```

將建立的 `queryRunner` 新增至您的場景中。現在，場景中的每個物件都能夠存取所提供的資料：

```ts
const scene = new EmbeddedScene({
    $data: queryRunner,
    body: ...
})
```

每個場景物件都有可設定的 `$data` 和 `$timeRange` 屬性。因為場景是一個物件樹，所以分別透過 `SceneQueryRunner` 和 `SceneTimeRange` 設定的資料和時間範圍，對於新增它們的物件「以及」所有後代物件都是可用的。

在以下範例中，每個 `VizPanel` 使用不同的資料。「面板 A」使用在 `EmbeddedScene` 上定義的資料，而「面板 B」則有自己設定的資料和時間範圍：

```tsx
// Scene data, used by Panel A
const queryRunner1 = new SceneQueryRunner({
  datasource: {
    type: 'prometheus',
    uid: '<PROVIDE_GRAFANA_DS_UID>',
  },
  queries: [
    {
      refId: 'A',
      expr: 'rate(prometheus_http_requests_total{}[5m])',
    },
  ],
});

// Panel B data
const queryRunner2 = new SceneQueryRunner({
  datasource: {
    type: 'prometheus',
    uid: '<PROVIDE_GRAFANA_DS_UID>',
  },
  queries: [
    {
      refId: 'A',
      expr: 'avg by (job, instance, mode) (rate(node_cpu_seconds_total[5m]))',
    },
  ],
});

const scene = new EmbeddedScene({
  $data: queryRunner1,
  // Global time range. queryRunner1 will use this time range.
  $timeRange: new SceneTimeRange({ from: 'now-5m', to: 'now' }),
  body: new SceneFlexLayout({
    direction: 'row',
    children: [
      new SceneFlexItem({
        width: '50%',
        height: 300,
        body: PanelBuilders.timeseries().setTitle('使用全域時間範圍的面板').build(),
      }),
      new SceneFlexItem({
        width: '50%',
        height: 300,
        body: PanelBuilders.timeseries()
          .setTitle('使用本地時間範圍的面板')
          // Time range defined on VizPanel object. queryRunner2 will use this time range.
          .setTimeRange(new SceneTimeRange({ from: 'now-6h', to: 'now' }))
          .setData(queryRunner2)
          .build(),
      }),
    ],
  }),
});
```

## SceneObject 的參考和父參考

非常重要的一點是，不要在多個不同的場景或同一場景內的不同位置重複使用同一個場景物件的實例。當一個場景物件成為另一個場景物件狀態的一部分時，它的父物件會被自動設定。因此，如果您想在多個場景物件的狀態中使用同一個場景物件實例，您有兩種選擇。

- 複製來源場景物件。這將會建立一個與來源物件沒有任何關聯的獨立實例。
- 使用 `SceneObjectRef` 來包裝實例。這樣可以確保物件的原始父物件不會被改變，同時允許您將該實例的參考儲存在另一個場景物件的狀態中。

## 原始碼

[檢視範例原始碼](https://github.com/grafana/scenes/tree/main/docusaurus/docs/core-concepts.tsx)
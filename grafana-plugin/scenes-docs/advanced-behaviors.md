---
id: advanced-behaviors
title: 行為
---

透過行為，您可以實作自訂的場景邏輯，這些邏輯將作為副作用執行。行為對於執行副作用很有用，例如有條件地隱藏場景上的元素或在場景物件之間附加共用功能。

## 定義行為

行為可以透過兩種方式實作：

-   作為一個純函式，在其父系被啟動時呼叫。
-   作為一個場景物件，在其父系被啟動時啟動。

行為可以使用 `$behaviors` 狀態屬性附加到場景物件。例如，您可以將行為附加到 `SceneQueryRunner`：

```ts
const queryRunner = new SceneQueryRunner({
  $behaviors: [
    /* 行為清單 */
  ],

  datasource: {
    type: 'prometheus',
    uid: 'gdev-prometheus',
  },
  queries: [
    {
      refId: 'A',
      range: true,
      format: 'time_series',
      expr: 'rate(prometheus_http_requests_total[5m])',
    },
  ],
});
```

### 作為純函式的行為

行為可以實作為一個無狀態函式，在行為父系被啟動時呼叫。此函式可以傳回一個停用處理常式，以便在父系被停用時呼叫。

以下是一個簡單的無狀態行為，它會在行為的父系啟動/停用或其狀態變更時在開發人員主控台中記錄。

```ts
const StatelessLoggerBehavior = (parent: SceneObject) => {
  console.log(`${parent.state.key} 已啟動`);

  parent.subscribeToState(() => {
    console.log(`${parent.state.key} 狀態已變更`);
  });

  return () => {
    console.log(`${parent.state.key} 已停用`);
  };
};
```

### 作為場景物件的行為

將行為實作為場景物件與實作自訂場景物件完全相同。以下範例說明了前一個範例的擴充記錄器行為，它會在場景物件啟動/停用時在開發人員主控台中記錄，並根據提供的設定批次處理父系狀態更新記錄：

```tsx
interface StatefulLoggerBehaviorState extends SceneObjectState {
  // 狀態更新的批次大小
  batchStateUpdates: number;
}

class StatefulLoggerBehavior extends SceneObjectBase<StatefulLoggerBehaviorState> {
  private _batchedStateUpdates: Array<SceneObjectState> = [];

  constructor(state: Partial<StatefulLoggerBehaviorState>) {
    super({
      batchStateUpdates: 5,
      ...state,
    });
    this.addActivationHandler(this._onActivate);
  }

  private _onActivate = () => {
    const parent = this.parent;

    if (!parent) {
      throw new Error('LoggerBehavior 必須附加到父物件');
    }

    console.log(`StatefulLoggerBehavior: ${parent.state.key} 已啟動`);

    parent.subscribeToState(() => {
      this._batchedStateUpdates.push(parent.state);

      if (this._batchedStateUpdates.length === this.state.batchStateUpdates) {
        console.log(`StatefulLoggerBehavior: ${parent.state.key} 狀態變更批次`, this._batchedStateUpdates);
        this._batchedStateUpdates = [];
      }
    });

    return () => {
      console.log(`StatefulLoggerBehavior: ${parent.state.key} 已停用`);
    };
  };
}
```

## 內建行為

Scenes 程式庫隨附以下內建行為：

### `ActWhenVariableChanged`

當變數變更時執行副作用。

#### 用法

假設場景中有一個名為 `myVariable` 的 `MultiValueVariable` 變數，您可以設定在變數值變更時要執行的副作用：

```ts
import { behaviors, MultiValueVariable } from '@grafana/scenes';

const logWhenVariableChanges = new behaviors.ActWhenVariableChanged({
  variableName: 'myVariable',
  onChange: (variable) => {
    if (!(variable instanceof MultiValueVariable)) {
      throw new Error('ActWhenVariableChanged 行為的變數類型無效');
    }
    console.log(`myVariable 值已變更: ${variable.state.value}`);
  },
});
```

### `CursorSync`

建立一個共用游標範圍，用於在多個面板之間設定游標同步。

#### 用法

在以下範例中，`CursorSync` 行為用於在場景中的所有面板之間同步游標：

```ts
import {
  behaviors,
  EmbeddedScene,
  PanelBuilders,
  SceneFlexItem,
  SceneFlexLayout,
  SceneQueryRunner,
  SceneTimeRange,
} from '@grafana/scenes';

const httpRequests = new SceneQueryRunner({
  datasource: {
    type: 'prometheus',
    uid: 'gdev-prometheus',
  },
  queries: [
    {
      refId: 'A',
      range: true,
      format: 'time_series',
      expr: 'rate(prometheus_http_requests_total[5m])',
    },
  ],
});

const cpuUsage = new SceneQueryRunner({
  datasource: {
    type: 'prometheus',
    uid: 'gdev-prometheus',
  },
  queries: [
    {
      refId: 'A',
      expr: 'avg by (job, instance, mode) (rate(node_cpu_seconds_total[5m]))',
    },
  ],
});

const scene = new EmbeddedScene({
  $timeRange: new SceneTimeRange({ from: 'now-5m', to: 'now' }),
  $behaviors: [new behaviors.CursorSync({ key: 'cursor-sync-scope', sync: DashboardCursorSync.Tooltip })],
  body: new SceneFlexLayout({
    direction: 'row',
    children: [
      new SceneFlexItem({
        width: '50%',
        height: 300,
        body: PanelBuilders.timeseries().setData(httpRequests).setTitle('HTTP 請求').build(),
      }),
      new SceneFlexItem({
        width: '50%',
        height: 300,
        body: PanelBuilders.timeseries()
          .setTitle('CPU 使用率')
          .setTimeRange(new SceneTimeRange({ from: 'now-6h', to: 'now' }))
          .setData(cpuUsage)
          .build(),
      }),
    ],
  }),
});
```

## 原始碼

[檢視範例原始碼](https://github.com/grafana/scenes/tree/main/docusaurus/docs/advanced-behaviors.tsx)
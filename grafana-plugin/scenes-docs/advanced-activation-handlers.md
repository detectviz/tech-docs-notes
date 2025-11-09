---
id: advanced-activation-handlers
title: 啟動處理常式
---

啟動處理常式是為場景物件提供外部行為的實用工具。當場景物件被掛載時，啟動處理常式會被呼叫。

與 React 的 `useEffect` 類似，啟動處理常式會傳回一個 `function(deactivation handler)`，該函式應用於清理在啟動處理常式中新增的所有行為。當場景物件被卸載時，會呼叫停用處理常式。

:::note
如果您想為核心場景物件新增外部行為，啟動處理常式特別有用。它們減少了實作處理場景物件連線的自訂場景物件的需求。
:::

本主題說明如何建立和使用啟動處理常式。

## 新增啟動處理常式

請依照以下步驟建立啟動處理常式。

### 步驟 1. 建立場景

首先建立一個渲染時間序列面板和文字面板的場景：

```ts
const queryRunner = new SceneQueryRunner({
  datasource: {
    type: 'prometheus',
    uid: '<PROVIDE_GRAFANA_DS_UID>',
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

const timeSeriesPanel = PanelBuilders.timeseries().setTitle('面板標題').setData(queryRunner).build();
const debugView = PanelBuilders.text()
  .setTitle('除錯檢視')
  .setOption('mode', TextMode.HTML)
  .setOption('content', '')
  .build();

const scene = new EmbeddedScene({
  $timeRange: new SceneTimeRange(),
  controls: [new SceneTimePicker({ isOnCanvas: true }), new SceneRefreshPicker({ isOnCanvas: true })],
  body: new SceneFlexLayout({
    direction: 'column',
    children: [
      new SceneFlexItem({
        body: timeSeriesPanel,
      }),
      new SceneFlexItem({
        width: '30%',
        body: debugView,
      }),
    ],
  }),
});
```

### 步驟 2. 新增啟動處理常式

將啟動處理常式新增至 `SceneQueryRunner`，該處理常式會訂閱狀態變更並在文字面板中顯示已執行的查詢。請記住，在 `SceneQueryRunner` 被啟動之前，不會建立對狀態的訂閱：

```ts
queryRunner.addActivationHandler(() => {
  let log = '';

  const sub = queryRunner.subscribeToState((state) => {
    log =
      `${new Date(Date.now()).toLocaleTimeString()} 已執行查詢: <pre>${state.queries.map((q) => q.expr)}</pre>\n` +
      log;
    debugView.setState({
      options: {
        content: log,
      },
    });
  });
});
```

### 步驟 3. 傳回停用處理常式

從啟動處理常式中，傳回一個函式，該函式將在物件停用時取消訂閱 `queryRunner` 狀態變更：

```ts
queryRunner.addActivationHandler(() => {
  let log = '';

  const sub = queryRunner.subscribeToState((state) => {
    log =
      `${new Date(Date.now()).toLocaleTimeString()} 已執行查詢: <pre>${state.queries.map((q) => q.expr)}</pre>\n` +
      log;
    debugView.setState({
      options: {
        content: log,
      },
    });
  });

  // 傳回停用處理常式
  return () => {
    sub.unsubscribe();
  };
});
```

## 原始碼

[檢視範例原始碼](https://github.com/grafana/scenes/tree/main/docusaurus/docs/advanced-activation-handlers.tsx)
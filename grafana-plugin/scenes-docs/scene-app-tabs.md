---
id: scene-app-tabs
title: Scenes 應用程式中的分頁導覽
---

`SceneAppPage` 支援建置具有分頁導覽的 Grafana 應用程式。分頁導覽對於設計複雜 Grafana 應用程式的資訊架構特別有用，其中視覺化會被分組到有意義的區段中。

## 將分頁導覽新增至 Scenes 應用程式

為使用 Scenes 的應用程式定義分頁導覽需要您使用 `SceneAppPage` 的屬性 `tabs`。

### 步驟 1. 建立 Scenes 應用程式

請遵循[使用 Scenes 建置應用程式指南](./scene-app.md)來建置您的應用程式。

### 步驟 2. 為個別分頁建立場景

每個分頁都會渲染自己的場景，類似於 `SceneAppPage`：

```tsx
const getOverviewScene =() => {
    const queryRunner = new SceneQueryRunner({
    $timeRange: new SceneTimeRange()
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

  return new EmbeddedScene({
    $data: queryRunner,
    body: new SceneFlexLayout({
      direction: 'column',
      children: [new SceneFlexItem({
        minHeight: 300,
        body: PanelBuilders.timeseries().setTitle('每個處理常式的 HTTP 請求').build(),
      })],
    }),
  });
}

const getHandlersScene =() => {
  const queryRunner = new SceneQueryRunner({
    $timeRange: new SceneTimeRange()
    datasource: {
      type: 'prometheus',
      uid: '<PROVIDE_GRAFANA_DS_UID>',
    },
    queries: [
      {
        refId: 'A',
        expr: 'sort_desc(avg by(handler) (rate(prometheus_http_request_duration_seconds_sum {}[5m]) * 1e3))',
        format: 'table',
        instant: true,
      },
    ],
  });

    return new EmbeddedScene({
    $data: queryRunner,
    body: new SceneFlexLayout({
      direction: 'column',
      children: [new SceneFlexItem({
        minHeight: 300,
        body: PanelBuilders.table().setTitle('每個處理常式的 HTTP 請求').build(),
      })],
    }),
  });
}
```

### 步驟 3. 為頁面設定分頁

分頁是 `SceneAppPage` 物件的實例。與建立場景頁面類似，您也可以建立分頁。若要渲染分頁，請使用 `SceneAppPage` 物件的 `tabs` 屬性：

```tsx

const overviewTab = new SceneAppPage({
  title: '總覽',
  url: '/a/<PLUGIN_ID>/my-app/overview',
  getScene: getOverviewScene,
});

const handlersTab = new SceneAppPage({
  title: '處理常式',
  url: '/a/<PLUGIN_ID>/my-app/handlers',
  getScene: getHandlersScene,
});


const myAppPage = new SceneAppPage({
  title: 'Grafana Scenes 應用程式',
  url: '`/a/<PLUGIN_ID>/my-app`,
  tabs: [
    overviewTab,
    handlersTab
  ]
});
```

導覽至 `https://your-grafana.url/a/<PLUGIN_ID>/my-app` 將會渲染一個具有兩個分頁的 Scenes 應用程式：**總覽**和**處理常式**。**總覽**分頁包含一個帶有 Prometheus HTTP 請求摘要的時間序列面板。**處理常式**分頁包含一個表格面板，其中包含每個處理常式的 Prometheus HTTP 請求平均持續時間摘要。
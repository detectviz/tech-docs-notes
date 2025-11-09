---
id: scene-app-drilldown
title: Scenes 應用程式中的鑽取頁面
---

鑽取頁面是建置複雜、資料驅動應用程式的強大工具。它們可讓您建立資料的高階概觀，使用者可以與之互動並逐步探索以揭示基礎資料。

## 將鑽取頁面新增至 Scenes 應用程式

`SceneAppPage` 附帶一個 API，可讓您建立深度、巢狀的鑽取頁面。

:::note
**開始之前**：在繼續本指南之前，您必須已經了解 React Router URL 參數、Grafana 欄位設定和資料連結。
:::

若要建立鑽取頁面，請使用 `SceneAppPage` 物件的 `drilldown` 屬性。

### 步驟 1. 建立 Scenes 應用程式

請遵循[使用 Scenes 建置應用程式指南](./scene-app.md)來建置您的應用程式。

### 步驟 2. 建置頂層鑽取頁面

使用以下程式碼來建置一個頁面，該頁面使用 Grafana 的表格面板顯示 Prometheus API 端點的 HTTP 請求平均持續時間摘要：

```ts
function getOverviewScene() {
  const queryRunner = new SceneQueryRunner({
    $timeRange: new SceneTimeRange(),
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

  const tablePanel = PanelBuilders.table().setTitle('HTTP 請求的平均持續時間').setData(queryRunner).build();

  return new EmbeddedScene({
    body: new SceneFlexLayout({
      direction: 'column',
      children: [
        new SceneFlexItem({
          minHeight: 300,
          body: tablePanel,
        }),
      ],
    }),
  });
}

function getSceneApp() {
  return new SceneApp({
    pages: [
      new SceneAppPage({
        title: 'HTTP 處理常式總覽',
        url: '/a/<PLUGIN_ID>/my-app',
        getScene: getOverviewScene,
      }),
    ],
  });
}
```

### 步驟 3. 設定鑽取導覽

若要顯示鑽取頁面，您需要提供導覽。設定表格面板資料連結 (在[官方 Grafana 文件](https://grafana.com/docs/grafana/latest/panels-visualizations/configure-data-links/)中了解有關資料連結的資訊)。然後修改表格面板設定以設定 `handler` 欄位的資料連結：

```tsx
import { sceneUtils, PanelBuilders } from '@grafana/scenes';

// ...

const tablePanel = PanelBuilders.table()
  .setTitle('HTTP 請求的平均持續時間')
  .setData(queryRunner)
  .setOverrides((b) =>
    b.matchFieldsWithName('handler').overrideLinks([
      {
        title: '前往處理常式總覽',
        url: '/a/<PLUGIN_ID>/my-app/${__value.text}${__url.params}',
      },
    ])
  )
  .build();
```

產生的面板將為 `handler` 欄位的所有值提供連結。按一下某個值將重新導向至特定的端點鑽取 URL，該 URL 將顯示「找不到頁面」錯誤。您將在下一步中設定此頁面。

:::note
`fieldConfig` 選項與您在表格面板檢查抽屜中檢視 **面板 JSON** 時在典型儀表板面板中看到的選項相同。若要存取面板檢查抽屜，請在面板編輯選單中按一下 **檢查**。
:::

### 步驟 4. 建置鑽取頁面

修改 `getSceneApp` 函式以設定鑽取場景。使用 `SceneAppPage` 物件的 `drilldowns` 屬性。`drilldowns` 屬性接受 `SceneAppDrilldownView` 物件的陣列。它允許設定鑽取 URL 和要渲染的頁面：

```ts
export interface SceneAppDrilldownView {
  /** 用於提供參數化鑽取 URL，例如 /app/clusters/:clusterId **/
  routePath: string;
  /** 傳回給定鑽取路由符合的頁面物件的函式。使用 parent 透過 getParentPage 方法設定鑽取檢視父 SceneAppPage。 **/
  getPage: (routeMatch: SceneRouteMatch<any>, parent: SceneAppPageLike) => SceneAppPageLike;
}
```

設定 API 端點鑽取檢視：

```tsx
function getSceneApp() {
  return new SceneApp({
    pages: [
      new SceneAppPage({
        title: 'HTTP 處理常式總覽',
        url: '/a/<PLUGIN_ID>/my-app',
        getScene: getOverviewScene,
        drilldowns: [
          {
            routePath: '/a/<PLUGIN_ID>/my-app/:handler',
            getPage: getHandlerDrilldownPage,
          },
        ],
      }),
    ],
  });
}
```

定義一個函式，該函式會為鑽取檢視傳回 `SceneAppPage`。此函式接收兩個引數：

- `routeMatch` - 包含有關 URL 參數的資訊。
- `parentPage` - 包含對父 `SceneAppPage` 的參考，此參考是正確設定麵包屑導覽所必需的。

```ts
function getHandlerDrilldownPage(routeMatch: SceneRouteMatch<{ handler: string }>, parent: SceneAppPageLike) {
  // 從 URL 參數中擷取處理常式
  const handler = decodeURIComponent(routeMatch.params.handler);

  return new SceneAppPage({
    // 設定特定的處理常式鑽取 URL
    url: `/a/<PLUGIN_ID>/my-app/${encodeURIComponent(handler)}`,
    // 重要：設定此項以建置麵包屑導覽
    getParentPage: () => parent,
    title: `${handler} 端點總覽`,
    getScene: () => getHandlerDrilldownScene(handler),
  });
}
```

### 步驟 5. 建置鑽取場景

定義將在鑽取頁面上渲染的場景：

```ts
function getHandlerDrilldownScene(handler: string) {
  const requestsDuration = new SceneQueryRunner({
    datasource: {
      type: 'prometheus',
      uid: '<PROVIDE_GRAFANA_DS_UID>',
    },
    queries: [
      {
        refId: 'A',
        expr: `avg without(job, instance) (rate(prometheus_http_request_duration_seconds_sum{handler="${handler}"}[5m])) * 1e3`,
      },
    ],
  });

  const requestsCount = new SceneQueryRunner({
    datasource: {
      type: 'prometheus',
      uid: '<PROVIDE_GRAFANA_DS_UID>',
    },
    queries: [
      {
        refId: 'A',
        expr: `sum without(job, instance) (rate(prometheus_http_request_duration_seconds_count{handler="${handler}"}[5m])) `,
      },
    ],
  });

  return new EmbeddedScene({
    body: new SceneFlexLayout({
      direction: 'column',
      children: [
        new SceneFlexItem({
          minHeight: 300,
          body: PanelBuilders.timeseries().setTitle('請求持續時間').setData(requestsDuration),
        }),
        new SceneFlexItem({
          minHeight: 300,
          body: PanelBuilders.timeseries().setTitle('請求計數').setData(requestsCount),
        }),
      ],
    }),
  });
}
```

### 完整範例

以下是具有鑽取頁面的 Scenes 應用程式的完整程式碼：

```tsx
function getOverviewScene() {
  const queryRunner = new SceneQueryRunner({
    $timeRange: new SceneTimeRange(),
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

  const tablePanel = PanelBuilders.table()
    .setTitle('HTTP 請求的平均持續時間')
    .setData(queryRunner)
    .setOverrides((b) =>
      b.matchFieldsWithName('handler').overrideLinks([
        {
          title: '前往處理常式總覽',
          url: '/a/<PLUGIN_ID>/my-app/${__value.text}${__url.params}',
        },
      ])
    )
    .build();

  return new EmbeddedScene({
    body: new SceneFlexLayout({
      direction: 'column',
      children: [
        new SceneFlexItem({
          minHeight: 300,
          body: tablePanel,
        }),
      ],
    }),
  });
}

function getHandlerDrilldownPage(routeMatch: SceneRouteMatch<{ handler: string }>, parent: SceneAppPageLike) {
  // 從 URL 參數中擷取處理常式。
  const handler = decodeURIComponent(routeMatch.params.handler);

  return new SceneAppPage({
    // 設定特定的處理常式鑽取 URL
    url: `/a/<PLUGIN_ID>/my-app/${encodeURIComponent(handler)}`,
    // 重要：設定此項以建置麵包屑導覽
    getParentPage: () => parent,
    title: `${handler} 端點總覽`,
    getScene: () => getHandlerDrilldownScene(handler),
  });
}

function getHandlerDrilldownScene(handler: string) {
  const requestsDuration = new SceneQueryRunner({
    datasource: {
      type: 'prometheus',
      uid: '<PROVIDE_GRAFANA_DS_UID>',
    },
    queries: [
      {
        refId: 'A',
        expr: `avg without(job, instance) (rate(prometheus_http_request_duration_seconds_sum{handler="${handler}"}[5m])) * 1e3`,
      },
    ],
  });

  const requestsCount = new SceneQueryRunner({
    datasource: {
      type: 'prometheus',
      uid: '<PROVIDE_GRAFANA_DS_UID>',
    },
    queries: [
      {
        refId: 'A',
        expr: `sum without(job, instance) (rate(prometheus_http_request_duration_seconds_count{handler="${handler}"}[5m])) `,
      },
    ],
  });

  return new EmbeddedScene({
    body: new SceneFlexLayout({
      direction: 'column',
      children: [
        new SceneFlexItem({
          minHeight: 300,
          body: PanelBuilders.timeseries().setTitle('請求持續時間').setData(requestsDuration),
        }),
        new SceneFlexItem({
          minHeight: 300,
          body: PanelBuilders.timeseries().setTitle('請求計數').setData(requestsCount),
        }),
      ],
    }),
  });
}

function getSceneApp() {
  return new SceneApp({
    pages: [
      new SceneAppPage({
        title: 'HTTP 處理常式總覽',
        url: '/a/<PLUGIN_ID>/my-app',
        getScene: getOverviewScene,
        drilldowns: [
          {
            routePath: '/a/<PLUGIN_ID>/my-app/:handler',
            getPage: getHandlerDrilldownPage,
          },
        ],
      }),
    ],
  });
}
```
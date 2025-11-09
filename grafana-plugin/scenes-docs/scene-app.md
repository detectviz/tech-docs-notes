---
id: scene-app
title: 使用 Scenes 建置應用程式
---

:::note
**開始之前**：在繼續本指南之前，您必須已經了解如何建置 Grafana 外掛程式。更多資訊請參閱 [Grafana 官方文件](https://grafana.com/docs/grafana/latest/developers/plugins/)。
:::

Scenes 附帶了以下物件，讓您可以輕鬆建置高互動性的 Grafana 應用程式外掛程式：

- `SceneApp`
- `SceneAppPage`

## SceneApp

`SceneApp` 是您必須使用的根物件，以充分利用 Scenes 和 Grafana 外掛程式的整合。`SceneApp` 為您的 Scenes 應用程式提供路由支援。

### 步驟 1. 建立 Scenes 應用程式

使用 `SceneApp` 物件定義一個新的 Scenes 應用程式：

```tsx
function getSceneApp() {
  return new SceneApp({
    pages: [],
    urlSyncOptions: {
      updateUrlOnInit: true,
      createBrowserHistorySteps: true,
    },
  });
}
```

### 步驟 2. 在外掛程式中渲染 Scenes 應用程式

定義一個將渲染 Scenes 應用程式的元件：

```tsx
function MyApp() {
  const scene = useSceneApp(getSceneApp);

  return <scene.Component model={scene} />;
}
```

:::note
請使用 useSceneApp 掛鉤來記憶和快取您的 `SceneApp` 實例的建立。這對於 URL 同步正常運作非常重要，同時也能確保當使用者離開您的應用程式再回來時，資料和場景應用程式狀態不會遺失。
:::

在應用程式外掛程式中，渲染 Scenes 應用程式：

```tsx
export class App extends React.PureComponent<AppRootProps> {
  render() {
    return (
      <PluginPropsContext.Provider value={this.props}>
        <MyApp />
      </PluginPropsContext.Provider>
    );
  }
}
```

:::note
前面的範例將渲染一個空白頁面，因為 `SceneApp` 建構函式中的 `pages` 屬性是空的。請使用 `SceneAppPage` 物件在您的應用程式中渲染場景。
:::

## SceneAppPage

`SceneAppPage` 物件可讓您輕鬆地在應用程式外掛程式中渲染場景。除了渲染場景外，它還支援：

- 路由
- Grafana 麵包屑導覽整合
- [分頁導覽](./scene-app-tabs.md)
- [鑽取頁面](./scene-app-drilldown.md)

使用 `SceneAppPage` 來建置您的應用程式頁面。它接受以下屬性：

```ts
  /** 頁面標題 */
  title: string;
  /** 頁面副標題 */
  subTitle?: string;
  /** 用於標題前的圖片 */
  titleImg?: string;
  /** 用於標題前的圖示 */
  titleIcon?: IconName;
  /** 用於提供絕對頁面 URL，例如 /app/overview **/
  url: string;
  /** 用於提供參數化頁面 URL，例如 /app/overview/:clusterId **/
  routePath?: string;
  /** 將在頁面標題右側內聯呈現的場景物件陣列 */
  controls?: SceneObject[];
  /** 控制頁面是否應在麵包屑路徑中可見 **/
  hideFromBreadcrumbs?: boolean;
  /** 作為頁面頂部分頁顯示的 SceneAppPage 物件陣列 **/
  tabs?: SceneAppPageLike[];
  /** 回傳頁面場景物件的函式 **/
  getScene?: (routeMatch: SceneRouteMatch) => EmbeddedScene;
  /** 用於鑽取檢視的場景陣列 **/
  drilldowns?: SceneAppDrilldownView[];
  /** 回傳父頁面物件的函式，用於建立麵包屑結構 **/
  getParentPage?: () => SceneAppPageLike;
  /** 將在麵包屑和頁面分頁連結中保留的查詢參數陣列，例如 ['from', 'to', 'var-datacenter',...] **/
  preserveUrlKeys?: string[];
```

### 步驟 1. 建立場景

首先，建立一個要在 `SceneApp` 中渲染的場景：

```tsx
const getScene = () => {
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

  return new EmbeddedScene({
    $data: queryRunner,
    $timeRange: new SceneTimeRange(),
    body: new SceneFlexLayout({
      direction: 'column',
      children: [
        new SceneFlexItem({
          minHeight: 300,
          body: PanelBuilders.timeseries().build(),
        }),
      ],
    }),
  });
};
```

### 步驟 2. 建立 `SceneAppPage`

使用 `SceneAppPage` 物件來設定應用程式頁面：

```tsx
const myAppPage = new SceneAppPage({
  title: 'Grafana Scenes App',
  url: '/a/<PLUGIN_ID>',
  getScene: getScene,
});
```

### 步驟 3. 將頁面新增至 `SceneApp`

```tsx
const getSceneApp = () =>
  new SceneApp({
    pages: [myAppPage],
  });
```

導覽至 `https://your-grafana.url/a/<PLUGIN_ID>` 將會渲染一個 Scenes 應用程式，其中包含一個頁面，該頁面包含一個時間序列面板，用於視覺化 Prometheus HTTP 請求的數量。
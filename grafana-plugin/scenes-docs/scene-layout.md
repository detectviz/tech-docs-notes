---
id: scene-layout
title: 建置場景版面配置
---

Scenes 支援兩種版面配置類型：彈性 (flex) 和網格 (grid) 版面配置。在本指南中，您將學習如何使用和設定 `SceneFlexLayout` 和 `SceneGridLayout`。

## Flexbox 版面配置

`SceneFlexLayout` 可讓您建置由瀏覽器的 CSS flexbox 版面配置驅動的彈性場景。這讓您可以定義非常動態的版面配置，其中面板的寬度和高度可以適應可用空間。

### 步驟 1. 建立場景

透過建立一個 `body` 設定為 `SceneFlexLayout` 的場景來開始使用 flexbox 版面配置：

```ts
const scene = new EmbeddedScene({
  body: new SceneFlexLayout({}),
});
```

### 步驟 2. 設定 flexbox 版面配置

`SceneFlexLayout` 允許設定 flexbox 行為。您可以設定以下屬性：

- `direction` - 設定 flexbox 版面配置的主軸。放置在版面配置中的子項目會遵循其方向。
- `wrap` - 設定版面配置子項目的行為。預設情況下，子項目會嘗試容納在一行中。

預設情況下，`SceneFlexLayout` 使用 `row` 方向。若要建立欄式版面配置，請使用以下程式碼：

```ts
const scene = new EmbeddedScene({
  body: new SceneFlexLayout({
    direction: 'column',
  }),
});
```

### 步驟 3. 新增版面配置子項目

`SceneFlexLayout` 有一個 `children` 屬性。它接受 `SceneFlexItem` 或 `SceneFlexLayout` 物件的陣列。
建立一個在欄中有兩個大小相等的版面配置項目的場景：

```ts
const scene = new EmbeddedScene({
  body: new SceneFlexLayout({
    direction: 'column',
    children: [new SceneFlexItem({ minHeight: 200 }), new SceneFlexItem({ minHeight: 300 })],
  }),
});
```

`SceneFlexLayout` 和 `SceneFlexItem` 物件類型都接受以下設定選項，以允許大小限制和行為：

```ts
  flexGrow?: CSSProperties['flexGrow'];
  alignSelf?: CSSProperties['alignSelf'];
  width?: CSSProperties['width'];
  height?: CSSProperties['height'];
  minWidth?: CSSProperties['minWidth'];
  minHeight?: CSSProperties['minHeight'];
  maxWidth?: CSSProperties['maxWidth'];
  maxHeight?: CSSProperties['maxHeight'];
  xSizing?: 'fill' | 'content';
  ySizing?: 'fill' | 'content';
  // 適用於較小螢幕的大小限制
  md?: SceneFlexItemPlacement;
```

我們強烈建議在使用 `column` 方向的版面配置的所有子項目上設定 `minHeight`。這可確保它們在較小的螢幕上不會過度壓縮。如果您在 `SceneFlexLayout` 物件上設定 `minHeight` 或 `height`，則無需在每個子項目上設定它，因為它們將繼承這些限制。

### 步驟 4. 將面板新增至彈性版面配置項目

前面的範例為您的場景設定了版面配置。若要視覺化資料，請[設定 `SceneQueryRunner`](./core-concepts.md#data-and-time-range) 並將其新增至您的場景：

```ts
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

const scene = new EmbeddedScene({
  $data: queryRunner,
  body: new SceneFlexLayout({
    direction: 'column',
    children: [new SceneFlexItem({}), new SceneFlexItem({})],
  }),
});
```

接下來，將 `VizPanel` 物件新增為版面配置項目的 `body`：

```ts
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

const scene = new EmbeddedScene({
  $data: queryRunner,
  body: new SceneFlexLayout({
    direction: 'column',
    children: [
      new SceneFlexItem({
        body: PanelBuilders.timeseries().setTitle('Time series').build(),
      }),
      new SceneFlexItem({
        body: PanelBuilders.table().setTitle('Table').build(),
      }),
    ],
  }),
});
```

這將渲染兩個面板，一個時間序列面板和一個表格面板。

:::note
對於包含 `VizPanel` 物件的 `SceneFlexItems`，建議您設定 `minHeight` 或 `minWidth` 限制，以免面板因有限的螢幕空間而過度壓縮。
:::

### 響應式彈性版面配置

預設情況下，`SceneFlexLayout` 對於較小的螢幕具有一些響應式行為：

- `SceneFlexLayout` 方向從 `row` 變為 `column`。
- `SceneFlexLayout` 的 `maxWidth`、`maxHeight`、`height` 或 `width` 限制被移除。
- `SceneFlexLayout` 和 `SceneFlexItem` 使用在父版面配置上設定的 `minHeight` 或 `height` (除非直接在其上指定)。這是為了強制在方向為 `row` 的 `SceneFlexLayout` 上設定的 `height` 或 `minHeight` 限制也適用於其子項目，以便當觸發將方向更改為 `column` 的響應式媒體查詢時，這些限制繼續作用於子項目。

這些行為會在螢幕符合 Grafana 主題的 `theme.breakpoints.down('md')` 媒體查詢時觸發。

您可以使用 `SceneFlexLayout` 和 `SceneFlexItem` 上都存在的 `md` 屬性來覆寫這些行為並設定自訂的方向和大小限制。例如：

```ts
new SceneFlexLayout({
  direction: 'row',
  minHeight: 200,
  md: {
    minHeight: 100,
    direction: 'row',
  },
  children: [getStatPanel({}), getStatPanel({})],
}),
```

在前面的範例中，我們使用 `md` 屬性來覆寫將列版面配置更改為欄版面配置的預設響應式行為。我們還應用了更嚴格的 `minHeight` 限制。

## CSS 網格版面配置

作為 `SceneFlexLayout` 的替代方案，`SceneCSSGridLayout` 可用於將場景項目包裝在 CSS 網格中。

```ts
const scene = new EmbeddedScene({
  body: new SceneCSSGridLayout({
    templateColumns: `repeat(auto-fit, minmax(400px, 1fr))`,
    children: [
      PanelBuilders.timeseries().setTitle('Graph 1').build(),
      PanelBuilders.timeseries().setTitle('Graph 2').build(),
    ],
  }),
});
```

`SceneCSSGridLayout` 接受與 `SceneFlexLayout` 相同的 `children`，並具有以下用於應用 CSS 網格樣式的屬性：

```ts
autoRows?: CSSProperties['gridAutoRows'];
templateRows?: CSSProperties['gridTemplateRows'];
templateColumns: CSSProperties['gridTemplateColumns'];
/** 在 Grafana 設計系統網格單位 (8px) 中 */
rowGap: number;
/** 在 Grafana 設計系統網格單位 (8px) 中 */
columnGap: number;
justifyItems?: CSSProperties['justifyItems'];
alignItems?: CSSProperties['alignItems'];
justifyContent?: CSSProperties['justifyContent'];
// 適用於較小螢幕的大小限制
md?: SceneCSSGridLayoutState;
```

使用 CSS Grid 可以輕鬆建置動態的面板網格，其中面板大小限制可以在網格本身上指定，而不是在每個面板上。這對於建置大小相等的面板網格非常有用。

下面的網格版面配置設定為子元素的最小尺寸為 400px，如果還有更多可用空間，則平均分配。高度使用 autoRows 設定。此設定將啟用一個大小相等的面板的非常響應式的版面配置。

```ts
const scene = new EmbeddedScene({
  body: new SceneCSSGridLayout({
    templateColumns: `repeat(auto-fit, minmax(400px, 1fr))`,
    autoRows: '150px',
    rowGap: 2,
    columnGap: 2,
    children: [
      new SceneCSSGridItem({
        body: PanelBuilders.timeseries().setTitle('Time series').build(),
      }),
      new SceneCSSGridItem({
        body: PanelBuilders.table().setTitle('Table').build(),
      }),
      new SceneCSSGridItem({
        body: PanelBuilders.timeseries().setTitle('Time series').build(),
      }),
      new SceneCSSGridItem({
        body: PanelBuilders.table().setTitle('Table').build(),
      }),
    ],
  }),
});
```

每個子項目周圍的 SceneCSSGridItem 包裝器是可選的。

## 網格版面配置

`SceneGridLayout` 可讓您將場景建置為可拖曳和移動的元素網格。這是 Grafana 中核心儀表板體驗使用的預設版面配置。不建議用於場景應用程式外掛程式，除非您需要使用者能夠移動面板。

### 步驟 1. 建立場景

透過建立一個 `body` 設定為 `SceneGridLayout` 的場景來開始使用網格版面配置：

```ts
const scene = new EmbeddedScene({
  $data: queryRunner,
  body: new SceneGridLayout({}),
});
```

### 步驟 2. 設定網格版面配置

`SceneGridLayout` 允許設定網格行為。提供的網格有 24 欄。

您可以設定以下屬性：

- `isDraggable` - 設定網格項目是否可以移動。
- `isLazy` - 設定當網格項目在視窗外時是否應初始化。

```ts
const scene = new EmbeddedScene({
  $data: queryRunner,
  body: new SceneGridLayout({
    isDraggable: false,
    isLazy: true,
  }),
});
```

### 步驟 3. 新增版面配置子項目

`SceneGridLayout` 有一個 `children` 屬性。它接受 `SceneGridItem` 或 `SceneGridRow` 物件的陣列。
建立一個在列中有兩個網格項目的場景：

```ts
const scene = new EmbeddedScene({
  $data: queryRunner,
  body: new SceneGridLayout({
    children: [
      new SceneGridItem({
        x: 0,
        y: 0,
        width: 12,
        height: 10,
        isResizable: false,
        isDraggable: false,
      }),
      new SceneGridItem({
        x: 12,
        y: 0,
        width: 12,
        height: 10,
        isResizable: false,
        isDraggable: false,
      }),
    ],
  }),
});
```

`SceneGridItem` 接受以下設定選項，以 24 欄網格單位表示：

```ts
  x?: number;
  y?: number;
  width?: number;
  height?: number;
```

### 步驟 4. 將面板新增至網格版面配置項目

將 `VizPanel` 新增至 `SceneGridItem` 以顯示視覺化資料：

```ts
const scene = new EmbeddedScene({
  $data: queryRunner,
  body: new SceneGridLayout({
    children: [
      new SceneGridItem({
        x: 0,
        y: 0,
        width: 12,
        height: 10,
        isResizable: false,
        isDraggable: false,
        body: PanelBuilders.timeseries().setTitle('Time series').build(),
      }),
      new SceneGridItem({
        x: 12,
        y: 0,
        width: 12,
        height: 10,
        isResizable: false,
        isDraggable: false,
        body: PanelBuilders.table().setTitle('Table').build(),
      }),
    ],
  }),
});
```

### 步驟 5. 新增網格列

網格列是將其他 `SceneGridItems` 分組為可折疊列的版面配置項目。使用 `SceneGridRow` 將列新增至場景：

:::note
在 `SceneGridRow` 中，`x` 和 `y` 坐標是相對於該列的。
:::

```ts
const row = new SceneGridRow({
  x: 0,
  y: 0,
  children: [
    new SceneGridItem({
      x: 0,
      y: 0,
      width: 12,
      height: 10,
      isResizable: false,
      isDraggable: false,
      body: PanelBuilders.timeseries().setTitle('Time series').build(),
    }),
    new SceneGridItem({
      x: 12,
      y: 0,
      width: 12,
      height: 10,
      isResizable: false,
      isDraggable: false,
      body: PanelBuilders.table().setTitle('Table').build(),
    }),
  ],
});

const scene = new EmbeddedScene({
  $data: queryRunner,
  body: new SceneGridLayout({
    children: [row],
  }),
});
```

## 分割版面配置

`SplitLayout` 可讓您將場景建置為可調整大小的獨立窗格的組合，方向可以是垂直或水平。

### 步驟 1. 建立場景

透過建立一個 `body` 設定為 `SplitLayout` 的場景來開始使用分割版面配置：

```ts
const scene = new EmbeddedScene({
  $data: queryRunner,
  body: new SplitLayout({}),
});
```

### 步驟 2. 設定分割版面配置

`SplitLayout` 允許多個設定選項：

- `direction` - 設定窗格是按列還是按欄定向。
- `primary` - 第一個窗格。
- `secondary` - 第二個窗格。

```ts
const scene = new EmbeddedScene({
  $data: queryRunner,
  body: new SplitLayout({
    direction: 'column',
  }),
});
```

### 步驟 3. 提供 `primary` 和 `secondary` 物件

`primary` 和 `secondary` 都接受 `SceneFlexItemLike` 物件。

```ts
const scene = new EmbeddedScene({
  $data: queryRunner,
  body: new SplitLayout({
    direction: 'column',
    primary: PanelBuilders.timeseries().setTitle('Primary panel').build(),
    secondary: PanelBuilders.table().setTitle('Secondary panel').build(),
  }),
});
```

## 原始碼

[檢視範例原始碼](https://github.com/grafana/scenes/tree/main/docusaurus/docs/scene-layout.tsx)
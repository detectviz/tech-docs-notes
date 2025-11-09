---
id: transformations
title: 轉換
---

:::note
**開始之前**：在繼續本指南之前，您必須已經了解[在 Scenes 應用程式中連接資料](./core-concepts.md#data-and-time-range)。
:::

轉換是在場景渲染視覺化之前，一種強大的方式來操作由 `SceneQueryRunner` 物件傳回的資料。使用轉換，您可以：

- 重新命名欄位
- 合併時間序列資料
- 跨查詢執行數學運算
- 使用一個轉換的輸出作為另一個轉換的輸入

透過轉換，您可以查詢一次資料，對其進行操作，然後在場景中顯示。

在[官方 Grafana 文件](https://grafana.com/docs/grafana/latest/panels-visualizations/query-transform-data/transform-data/)中深入了解 Grafana 轉換。

## 在場景中轉換查詢結果

### 步驟 1. 建立場景

建立一個帶有單一表格面板和 Prometheus 查詢的場景。範例查詢會傳回 Prometheus API 端點的 HTTP 請求平均持續時間。產生的表格包含三欄：`Time`、`handler` 和 `Value`。

```tsx
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

const scene = new EmbeddedScene({
  $data: queryRunner,
  body: new SceneFlexLayout({
    direction: 'column',
    children: [
      new SceneFlexItem({
        body: PanelBuilders.table().setTitle('HTTP 請求的平均持續時間').build(),
      }),
    ],
  }),
});
```

### 步驟 2. 設定資料轉換

上一步驟產生的表格將類似於以下表格：

| 時間 | 處理常式 | 值 |
| --- | --- | --- |
| 2023-05-09 14:00:00.000 | /metrics | 1.10 |
| 2023-05-09 14:00:00.000 | /api/v1/label/:name/values | 0.361 |
| 2023-05-09 14:00:00.000 | /api/v1/metadata | 0.113 |
| 2023-05-09 14:00:00.000 | /api/v1/query_range | 0.0847 |
| 2023-05-09 14:00:00.000 | /api/v1/query | 0.14 |
| 2023-05-09 14:00:00.000 | /api/v1/labels | 0.0194 |
| 2023-05-09 14:00:00.000 | /api/v1/series | 0 |
| 2023-05-09 14:00:00.000 | /api/v1/status/buildinfo | 0 |

新增 [_組織欄位_ 轉換](https://grafana.com/docs/grafana/latest/panels-visualizations/query-transform-data/transform-data/#organize-fields) 以隱藏 `Time` 欄位：

建立一個 `SceneDataTransformer` 物件：

```tsx
const transformedData = new SceneDataTransformer({
  $data: queryRunner,
  transformations: [
    {
      id: 'organize',
      options: {
        excludeByName: {
          Time: true,
        },
        indexByName: {},
        renameByName: {},
      },
    },
  ],
});
```

:::note
在 `transformations` 中使用的物件與您在典型儀表板面板中檢視面板檢查抽屜中的 **JSON** 標籤時看到的轉換設定物件相同。若要存取此標籤，請在面板編輯選單中按一下 **檢查 > 面板 JSON**。
:::

使用新建立的 `transformedData` 物件來取代先前使用的 `SceneQueryRunner`：

```tsx
const scene = new EmbeddedScene({
  $data: transformedData,
  body: new SceneFlexLayout({
    direction: 'column',
    children: [
      new SceneFlexItem({
        body: PanelBuilders.table().setTitle('HTTP 請求的平均持續時間').build(),
      }),
    ],
  }),
});
```

產生的表格將類似於以下表格：

| 處理常式 | 值 |
| --- | --- |
| /metrics | 1.10 |
| /api/v1/label/:name/values | 0.361 |
| /api/v1/metadata | 0.113 |
| /api/v1/query_range | 0.0847 |
| /api/v1/query | 0.14 |
| /api/v1/labels | 0.0194 |
| /api/v1/series | 0 |
| /api/v1/status/buildinfo | 0 |

### 步驟 3. 設定多個轉換

`SceneDataTransformer` 允許您設定多個轉換。轉換會依照它們新增至 `transformations` 陣列的順序執行。

修改 `transformedData` 物件並新增 [_依規則運算式重新命名_ 轉換](https://grafana.com/docs/grafana/latest/panels-visualizations/query-transform-data/transform-data/#rename-by-regex) 以變更欄位名稱：將 `handler` 變更為 `Handler`，將 `Value` 變更為 `Average duration`：

```tsx
const transformedData = new SceneDataTransformer({
  $data: queryRunner,
  transformations: [
    {
      id: 'organize',
      options: {
        excludeByName: {
          Time: true,
        },
        indexByName: {},
        renameByName: {},
      },
    },
    {
      id: 'renameByRegex',
      options: {
        regex: 'handler',
        renamePattern: 'Handler',
      },
    },
    {
      id: 'renameByRegex',
      options: {
        regex: 'Value',
        renamePattern: 'Average duration',
      },
    },
  ],
});
```

產生的表格將類似於以下表格：

| 處理常式 | 平均持續時間 |
| --- | --- |
| /metrics | 1.10 |
| /api/v1/label/:name/values | 0.361 |
| /api/v1/metadata | 0.113 |
| /api/v1/query_range | 0.0847 |
| /api/v1/query | 0.14 |
| /api/v1/labels | 0.0194 |
| /api/v1/series | 0 |
| /api/v1/status/buildinfo | 0 |

## 新增自訂轉換

除了 Grafana 中所有可用的轉換之外，場景還允許您建立自訂轉換。

`SceneDataTransformer` 接受 `CustomTransformOperator` 作為 `transformations` 陣列的項目：

```ts
  transformations: Array<DataTransformerConfig | CustomTransformOperator>;
```

`CustomTransformOperator` 是一個函式，它會傳回轉換資料的 RxJS 運算子：

```ts
type CustomTransformOperator = (context: DataTransformContext) => MonoTypeOperatorFunction<DataFrame[]>;
```

:::note
在 [RxJS 官方文件](https://rxjs.dev/guide/operators)中深入了解 RxJS 運算子。
:::

### 步驟 1. 建立自訂轉換

建立一個自訂轉換，該轉換將應用於 `handler` 欄位，並為所有值加上 URL 前置詞：

:::note
自訂轉換在很大程度上依賴於操作稱為「資料框架」的內部 Grafana 資料物件。在[官方 Grafana 文件](https://grafana.com/docs/grafana/latest/developers/plugins/data-frames/)中深入了解資料框架。
:::

```ts
import { DataFrame } from '@grafana/data';
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';

const prefixHandlerTransformation: CustomTransformOperator = () => (source: Observable<DataFrame[]>) => {
  return source.pipe(
    map((data: DataFrame[]) => {
      return data.map((frame: DataFrame) => {
        return {
          ...frame,
          fields: frame.fields.map((field) => {
            if (field.name === 'handler') {
              return {
                ...field,
                values: field.values.map((v) => 'http://www.my-api.com' + v),
              };
            }
            return field;
          }),
        };
      });
    })
  );
};
```

### 步驟 2. 使用自訂轉換

將自訂轉換新增至先前建立的 `transformedData` 物件：

```tsx
const transformedData = new SceneDataTransformer({
  $data: queryRunner,
  transformations: [
    prefixHandlerTransformation,
    {
      id: 'organize',
      options: {
        excludeByName: {
          Time: true,
        },
        indexByName: {},
        renameByName: {},
      },
    },
    {
      id: 'renameByRegex',
      options: {
        regex: 'handler',
        renamePattern: 'Handler',
      },
    },
    {
      id: 'renameByRegex',
      options: {
        regex: 'Value',
        renamePattern: 'Average duration',
      },
    },
  ],
});
```

:::note
`prefixHandlerTransformation` 自訂轉換會新增為第一個，因為它會應用於在後續轉換中重新命名為 `Handler` 的 `handler` 欄位。您可以修改自訂轉換的實作，使其不必在其他轉換之前使用。
:::

產生的表格將類似於以下表格：

| 處理常式 | 平均持續時間 |
| --- | --- |
| `http://www.my-api.com/metrics` | 1.10 |
| `http://www.my-api.com/api/v1/label/:name/values` | 0.361 |
| `http://www.my-api.com/api/v1/metadata` | 0.113 |
| `http://www.my-api.com/api/v1/query_range` | 0.0847 |
| `http://www.my-api.com/api/v1/query` | 0.14 |
| `http://www.my-api.com/api/v1/labels` | 0.0194 |
| `http://www.my-api.com/api/v1/series` | 0 |
| `http://www.my-api.com/api/v1/status/buildinfo` | 0 |

## 合併與篩選

您可以使用轉換 (自訂和內建) 做的一件強大的事情，是以有趣的方式在面板之間共用查詢結果。這可讓您將大部分查詢放在場景頂端的單一查詢執行器中。然後，您可以在 `VizPanel` 層級使用 `SceneDataTransformer` 物件，以不同的方式合併和篩選產生的資料。有些面板可能需要兩個查詢的結果，而另一個面板可能需要所有查詢的結果。

使用 `DataFrame` 上的 `refId` 屬性，可以輕鬆地依查詢來源篩選產生的 `DataFrame` 陣列。

## 原始碼

[檢視範例原始碼](https://github.com/grafana/scenes/tree/main/docusaurus/docs/transformations.tsx)
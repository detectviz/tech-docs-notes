---
id: variables
title: 變數
---

變數用於參數化場景。它們是值的佔位符，可用於查詢、面板標題，甚至是自訂場景物件。在[官方 Grafana 文件](https://grafana.com/docs/grafana/latest/dashboards/variables/)中深入了解 Grafana 範本變數。

## 支援的變數類型

Scenes 支援以下變數類型：

- 查詢變數 (`QueryVariable`) - 查詢產生的值清單，例如指標名稱、伺服器名稱、感應器 ID、資料中心等。
- 資料來源變數 (`DataSourceVariable`) - 定義給定類型的資料來源清單。
- 自訂變數 (`CustomVariable`) - 使用逗號分隔清單手動定義變數選項。
- 常數變數 (`ConstantVariable`) - 定義一個常數值變數。
- 文字方塊變數 (`TextBoxVariable`) - 顯示一個可選預設值的自由文字輸入欄位。

## 將變數新增至場景

請依照以下步驟將變數新增至場景。

### 步驟 1. 建立並自訂變數物件

從變數定義開始。以下程式碼會建立一個變數，該變數會從 Prometheus 資料來源擷取 `prometheus_http_requests_total` 指標的所有 `handler` 標籤值：

```ts
const handlers = new QueryVariable({
  name: 'handler',
  datasource: {
    type: 'prometheus',
    uid: '<PROVIDE_GRAFANA_DS_UID>',
  },
  query: {
    query: 'label_values(prometheus_http_requests_total,handler)',
  },
});
```

:::note
上述程式碼區塊中使用的 `datasource` 是指核心 Grafana Prometheus 資料來源外掛程式。請確定您的 Grafana 堆疊已安裝並設定此此外掛程式。`query` 屬性與您在儀表板設定中檢視儀表板 JSON 時在典型儀表板範本變數中看到的屬性相同。
:::

### 步驟 2. 設定場景以使用變數

使用 `SceneVariableSet` 物件為您的場景定義一個 `$variables` 屬性：

```ts
const scene = new EmbeddedScene({
  $variables: new SceneVariableSet({
    variables: [handlers],
  }),
  body: new SceneFlexLayout({
    children: [],
  }),
});
```

### 步驟 3. 在場景中顯示變數選擇器

使用 `EmbeddedScene` 的 `controls` 屬性在場景頂部顯示變數值選擇器：

```ts
const scene = new EmbeddedScene({
  $variables: new SceneVariableSet({
    variables: [handlers],
  }),
  body: new SceneFlexLayout({
    children: [],
  }),
  controls: [new VariableValueSelectors({})],
});
```

現在，一個允許您變更變數值的選擇器會顯示在場景頂部。

### 步驟 4. 在查詢中使用變數

建立 `SceneQueryRunner`，它將查詢 Prometheus 資料來源並在查詢中使用已設定的變數：

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
      expr: 'rate(prometheus_http_requests_total{handler="$handler"}[5m])',
    },
  ],
});
```

請注意，Prometheus 查詢的 `expr` 屬性使用了 `$handler` 變數。在[官方 Grafana 文件](https://grafana.com/docs/grafana/latest/dashboards/variables/variable-syntax/)中深入了解 Grafana 的變數語法。

### 步驟 5. 將資料新增至場景

將上一步驟中建立的 `queryRunner` 與場景連接：

```ts
const scene = new EmbeddedScene({
  $variables: new SceneVariableSet({
    variables: [handlers],
  }),
  $data: queryRunner,
  body: new SceneFlexLayout({
    children: [],
  }),
  controls: [new VariableValueSelectors({})],
});
```

### 步驟 6. 將視覺化新增至場景

若要使用 `handler` 變數顯示查詢結果，請使用 `VizPanel` 類別將時間序列視覺化新增至場景：

```ts
const scene = new EmbeddedScene({
  $variables: new SceneVariableSet({
    variables: [handlers],
  }),
  $data: queryRunner,
  body: new SceneFlexLayout({
    children: [
      new SceneFlexItem({
        body: PanelBuilders.timeseries().build(),
      }),
    ],
  }),
  controls: [new VariableValueSelectors({})],
});
```

使用場景頂部的選擇器變更變數值，即可在您的視覺化中看到更新的資料。

以下是使用 `QueryVariable` 的場景的完整程式碼：

```ts
const handlers = new QueryVariable({
  name: 'handler',
  datasource: {
    type: 'prometheus',
    uid: '<PROVIDE_GRAFANA_DS_UID>',
  },
  query: {
    query: 'label_values(prometheus_http_requests_total,handler)',
  },
});

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
      expr: 'rate(prometheus_http_requests_total{handler="$handler"}[5m])',
    },
  ],
});

const scene = new EmbeddedScene({
  $variables: new SceneVariableSet({
    variables: [handlers],
  }),
  $data: queryRunner,
  body: new SceneFlexLayout({
    children: [
      new SceneFlexItem({
        body: PanelBuilders.timeseries().build(),
      }),
    ],
  }),
  controls: [new VariableValueSelectors({})],
});
```

## 宏

變數系統支援各種內建宏，這些宏是變數表達式，無需包含任何其他變數即可使用。

### 全域宏

| 語法 | 說明 |
| --- | --- |
| `${___url}` | 目前的 URL |
| `${__url.path}` | 不含查詢參數的目前 URL |
| `${__url.params}` | 目前的 URL 查詢參數 |
| `${__url.params:exclude:var-handler}` | 不含 `var-handler` 的目前 URL 查詢參數 |
| `${__url.params:include:var-handler,var-instance}` | 僅含 `var-handler` 和 `var-instance` 的目前 URL 查詢參數 |

使用類似以下的字串，從表格建立到另一個頁面的資料連結，並保留所有查詢參數：

- `/scene-x/my-drilldown-view/${__value.raw}${__url.params}`

使用類似以下的字串，以新的查詢參數更新目前的場景 URL，或如果存在則更新它：

- `/my-scene-url${__url.params:exclude:drilldown-id}&drilldown-id=${__value.raw}`

這將產生一個保留 URL 狀態的 URL，但 `drilldown-id` 查詢參數會更新為此特定資料連結的內插值。

### 欄位/序列宏

以下宏可在資料連結和欄位覆寫屬性 (如 displayName) 中使用。

| 語法 | 說明 |
| --- | --- |
| `${__field.name}` | 將內插為欄位/序列名稱 |
| `${__field.labels.cluster}` | 將內插為 cluster 標籤的值 |

### 值/列宏

以下宏可在基於列和值的資料連結中使用。

| 語法 | 說明 |
| --- | --- |
| `${__value.text}` | 對於表格和其他呈現列/值的視覺化中的資料連結很有用 |
| `${__value.raw}` | 未格式化的值 |
| `${__data.fields[0].text}` | 將內插為同一列上第一個欄位/欄的值 |

## 原始碼

[檢視範例原始碼](https://github.com/grafana/scenes/tree/main/docusaurus/docs/variables.tsx)
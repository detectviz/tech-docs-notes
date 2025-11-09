---
id: data-frames
title: 資料框架
description: 學習在 Grafana 插件開發中使用資料框架。
keywords:
  - grafana
  - plugins
  - plugin
  - data frames
  - dataframes
sidebar_position: 40
---

# 資料框架

Grafana 支援各種不同的資料來源，每種資料來源都有自己的資料模型。為了實現這一點，Grafana 將來自每個資料來源的查詢結果合併到一個名為_資料框架_的統一資料結構中。

資料框架結構的概念借鏡了資料分析工具，例如 [R 程式語言](https://www.r-project.org)和 [Pandas](https://pandas.pydata.org/)。

:::note

資料框架自 Grafana 7.0+ 起提供，並以更通用的資料結構取代了時間序列和表格結構，以支援更廣泛的資料類型。

:::

本文件概述了資料框架結構，以及資料在 Grafana 中的處理方式。

## 資料框架欄位

資料框架是_欄位_的集合，其中每個欄位對應一個欄。每個欄位又由一組值和元資料組成，例如這些值的資料類型。

```ts
export interface Field<T = any, V = Vector<T>> {
  /**
   * 欄位（欄）的名稱
   */
  name: string;
  /**
   *  欄位值類型（字串、數字等）
   */
  type: FieldType;
  /**
   *  有關欄位以及如何顯示它的元資訊
   */
  config: FieldConfig;

  /**
   * 原始欄位值
   * 在 Grafana 10 中，這接受簡單陣列和 Vector 介面
   * 在 Grafana 11 中，Vector 介面已被移除
   */
  values: V | T[];

  /**
   * 當 type === FieldType.Time 時，這可以選擇性地將
   * 奈秒精度的分數儲存為 0 到 999999 之間的整數。
   */
  nanos?: number[];

  labels?: Labels;

  /**
   * 具有適當顯示和 id 值的快取值
   */
  state?: FieldState | null;

  /**
   * 轉換要顯示的值
   */
  display?: DisplayProcessor;

  /**
   * 取得已內插變數的值資料連結
   */
  getLinks?: (config: ValueLinkConfig) => Array<LinkModel<Field>>;
}
```

讓我們看一個範例。下表展示了一個具有兩個欄位的資料框架，_time_ 和 _temperature_：

| time                | temperature |
| ------------------- | ----------- |
| 2020-01-02 03:04:00 | 45.0        |
| 2020-01-02 03:05:00 | 47.0        |
| 2020-01-02 03:06:00 | 48.0        |

每個欄位有三個值，且一個欄位中的每個值都必須共用相同的類型。在此情況下，`time` 欄位中的所有值都是時間戳記，而 `temperature` 欄位中的所有值都是數字。

雖然時間欄位表示時間戳記，但值的類型應為 `Number` (TypeScript) 或 `time.Time` (Golang)。

資料框架中時間欄位的另一個限制涉及數字轉換。在插件前端程式碼中，可以使用 `@grafana/data` 套件中的 [`ensureTimeField`](https://github.com/grafana/grafana/blob/3e24a500bf43b30360faf9f32465281cc0ff996d/packages/grafana-data/src/transformations/transformers/convertFieldType.ts#L245-L257) 函式將其他格式轉換為 `Number`。此函式會將遵循 ISO 8601 格式的字串（例如 `2017-07-19 00:00:00.000`）、Javascript `DateTime` 和具有相對時間的字串（例如 `now-10s`）轉換為 `Number`。

資料框架的一個限制是，框架中的所有欄位都必須具有相同的長度才能成為有效的資料框架。

## 欄位設定

資料框架中的每個欄位都包含有關欄位中值的可選資訊，例如單位、縮放等。

透過將欄位設定新增至資料框架，Grafana 可以自動設定視覺化。例如，您可以設定 Grafana 自動設定資料來源提供的單位。

## 資料轉換

我們已經看到欄位設定如何包含類型資訊；此外，資料框架欄位還可以在 Grafana 中啟用_資料轉換_。

資料轉換是任何接受資料框架作為輸入並傳回另一個資料框架作為輸出的函式。透過在您的插件中使用資料框架，您可以免費獲得一系列轉換。

若要深入了解 Grafana 中的資料轉換，請參閱[轉換資料](https://grafana.com/docs/grafana/latest/panels-visualizations/query-transform-data/transform-data)。

## 資料框架作為時間序列

具有至少一個時間欄位的資料框架被視為_時間序列_。

有關時間序列的更多資訊，請參閱我們的[時間序列簡介](https://grafana.com/docs/grafana/latest/fundamentals/timeseries/)。

### 寬格式

當一組時間序列共用相同的_時間索引_——每個時間序列中的時間欄位都相同時——它們可以一起儲存在_寬_格式中。透過重複使用時間欄位，可以減少傳送到瀏覽器的資料量。

在此範例中，來自每個主機的 `cpu` 使用量共用時間索引，因此我們可以將它們儲存在同一個資料框架中：

```text
Name: Wide
Dimensions: 3 fields by 2 rows
+---------------------+-----------------+-----------------+
| Name: time          | Name: cpu       | Name: cpu       |
| Labels:             | Labels: host=a  | Labels: host=b  |
| Type: []time.Time   | Type: []float64 | Type: []float64 |
+---------------------+-----------------+-----------------+
| 2020-01-02 03:04:00 | 3               | 4               |
| 2020-01-02 03:05:00 | 6               | 7               |
+---------------------+-----------------+-----------------+
```

但是，如果兩個時間序列不共用相同的時間值，則它們會被表示為兩個不同的資料框架：

```text
Name: cpu
Dimensions: 2 fields by 2 rows
+---------------------+-----------------+
| Name: time          | Name: cpu       |
| Labels:             | Labels: host=a  |
| Type: []time.Time   | Type: []float64 |
+---------------------+-----------------+
| 2020-01-02 03:04:00 | 3               |
| 2020-01-02 03:05:00 | 6               |
+---------------------+-----------------+

Name: cpu
Dimensions: 2 fields by 2 rows
+---------------------+-----------------+
| Name: time          | Name: cpu       |
| Labels:             | Labels: host=b  |
| Type: []time.Time   | Type: []float64 |
+---------------------+-----------------+
| 2020-01-02 03:04:01 | 4               |
| 2020-01-02 03:05:01 | 7               |
+---------------------+-----------------+
```

寬格式的典型用途是當多個時間序列由同一個程序收集時。在這種情況下，每個測量都是在相同的時間間隔進行的，因此共用相同的時間值。

### 長格式

某些資料來源會以_長_格式（也稱為_窄_格式）傳回資料。這是例如 SQL 資料庫傳回的常見格式。

在長格式中，字串值會表示為單獨的欄位，而不是標籤。因此，長格式的資料框架可能會有重複的時間值。

使用 Grafana plugin SDK for Go，插件可以偵測並將長格式的資料框架轉換為寬格式。

有關偵測和轉換資料框架的範例，請參閱：

```go
		tsSchema := frame.TimeSeriesSchema()
		if tsSchema.Type == data.TimeSeriesTypeLong {
			wideFrame, err := data.LongToWide(frame, nil)
			if err == nil {
				// handle error
			}
			// return wideFrame
		}
```

以下是另一個範例。以下資料框架以長格式顯示：

```text
Name: Long
Dimensions: 4 fields by 4 rows
+---------------------+-----------------+-----------------+----------------+
| Name: time          | Name: aMetric   | Name: bMetric   | Name: host     |
| Labels:             | Labels:         | Labels:         | Labels:        |
| Type: []time.Time   | Type: []float64 | Type: []float64 | Type: []string |
+---------------------+-----------------+-----------------+----------------+
| 2020-01-02 03:04:00 | 2               | 10              | foo            |
| 2020-01-02 03:04:00 | 5               | 15              | bar            |
| 2020-01-02 03:05:00 | 3               | 11              | foo            |
| 2020-01-02 03:05:00 | 6               | 16              | bar            |
+---------------------+-----------------+-----------------+----------------+
```

上表可以轉換為寬格式的資料框架，如下所示：

```text
Name: Wide
Dimensions: 5 fields by 2 rows
+---------------------+------------------+------------------+------------------+------------------+
| Name: time          | Name: aMetric    | Name: bMetric    | Name: aMetric    | Name: bMetric    |
| Labels:             | Labels: host=foo | Labels: host=foo | Labels: host=bar | Labels: host=bar |
| Type: []time.Time   | Type: []float64  | Type: []float64  | Type: []float64  | Type: []float64  |
+---------------------+------------------+------------------+------------------+------------------+
| 2020-01-02 03:04:00 | 2                | 10               | 5                | 15               |
| 2020-01-02 03:05:00 | 3                | 11               | 6                | 16               |
+---------------------+------------------+------------------+------------------+------------------+
```

:::note

並非所有面板都支援寬時間序列資料框架格式。為了保持完全的向後相容性，Grafana 引入了一種轉換，您可以使用它將寬格式轉換為長格式。有關用法資訊，請參閱[準備時間序列轉換](https://grafana.com/docs/grafana/latest/panels-visualizations/query-transform-data/transform-data#prepare-time-series)。

:::

## 技術參考

Grafana 中資料框架的概念借鏡了資料分析工具，例如 [R 程式語言](https://www.r-project.org)和 [Pandas](https://pandas.pydata.org/)。以下提供了其他技術參考。

### Apache Arrow

資料框架結構的靈感來自並使用了 [Apache Arrow 專案](https://arrow.apache.org/)。Javascript 資料框架使用 Arrow Tables 作為底層結構，後端 Go 程式碼將其 Frames 序列化為 Arrow Tables 以進行傳輸。

### Javascript

資料框架的 Javascript 實作位於 [`@grafana/data` 套件](https://github.com/grafana/grafana/tree/main/packages/grafana-data)的 [`/src/dataframe` 資料夾](https://github.com/grafana/grafana/tree/main/packages/grafana-data/src/dataframe)和 [`/src/types/dataframe.ts`](https://github.com/grafana/grafana/blob/main/packages/grafana-data/src/types/dataFrame.ts) 中。

### Go

有關資料框架的 Go 實作文件，請參閱 [github.com/grafana/grafana-plugin-sdk-go/data package](https://pkg.go.dev/github.com/grafana/grafana-plugin-sdk-go/data?tab=doc)。

## 深入了解

有關使用資料框架進行插件開發的指南，請參閱[建立資料框架](../how-to-guides/data-source-plugins/create-data-frames)。

若要了解資料框架與資料平面合約之間的關係，請參閱 [Grafana 資料結構](https://grafana.com/developers/dataplane/dataplane-dataframes)。
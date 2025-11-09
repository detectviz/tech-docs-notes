---
id: sql-requirements
title: 支援 SQL 運算式的要求
sidebar_label: 支援 SQL 運算式
description: 資料來源插件支援 SQL 運算式的要求。
keywords:
  - grafana
  - plugin
  - datasource
  - sql
---

請遵循本指南，以了解您的資料來源插件需要滿足哪些要求才能支援 SQL 運算式。

:::caution
只有具有後端的插件才能支援 SQL 運算式。
:::

要求取決於您查詢的回應類型。SQL 運算式支援兩種回應類型：

- [表格資料回應](#requirements-for-tabular-data-responses)
- [標籤化指標時間資料回應](#requirements-for-labeled-metric-data-responses)

## 表格資料回應的要求

表格資料回應是單一的[資料框架](../../key-concepts/data-frames)，其任何欄位（欄）上都沒有標籤。簡而言之，是一個可以直接對應到 SQL 表格的資料框架。

表格資料回應開箱即用。

## 標籤化指標資料回應的要求

標籤化指標時間資料回應包含符合[資料平面規範](https://grafana.com/developers/dataplane)的資料，且其 `Frame.Meta.Type` 屬性已設定為資料平面類型。它可以是時間序列或數字。

SQL 不支援標籤，因此當 SQL 運算式收到標籤化指標資料時，它會將資料轉換為對應種類（時間序列或數字）的完整長格式。

支援的類型有：

- `timeseries-multi`
- `timeseries-wide`
- `numeric-multi`
- `numeric-wide`

例如，以下回應（`Frame.Meta.Type` 序列化為 JSON 為 `schema.meta.type`）支援 SQL 運算式：

```jsonc
[
  {
    "schema": {
      "meta": {
        "type": "numeric-wide", // SQL 運算式需要為標籤化指標資料設定此屬性
        "typeVersion": [0, 1], // 對於 SQL 運算式是可選的（因此可以是預設的 0.0）
        // TypeVersion > 0.0 應使其他 SSE 操作更具確定性，
        // 但如果不是新的 DS，最安全的路徑是將其作為單獨的任務來做。
        // ...
      }
    },
    "fields": [
        // ...
    ]
  }
]
```

在 Go 中：

```go
import (
    "github.com/grafana/grafana-plugin-sdk-go/data"
)

func main() {
    frame := data.NewFrame("")
    // ... 新增資料和欄位以建立 "NumericWide" 類型。
    frame.Meta = &data.FrameMeta{Type: data.FrameTypeNumericWide}
}
```

### 轉換是什麼樣子的？

以下是 SQL 運算式轉換標籤化資料的方式：

![SQL 轉換](./images/sql-conversion.png)

## 測試您的插件

### 測試您的插件的 SQL 相容性

在儀表板中，並針對每種類型的回應，請遵循以下步驟：

1. 新增您的資料來源查詢。
2. 新增 `Expression -> Type SQL`。
3. 預設查詢是 `SELECT * from A LIMIT 10`（假設 `RefID` 是 `A`）。如果這在該查詢類型的幾個變體上都有效，那麼它就與 SQL 運算式相容。

### 檢查您的資料來源的檢測以追蹤 SQL 運算式支援

使用以下指標來檢查您的資料來源與 SQL 運算式的相容性：

```
sum(rate(grafana_sse_sql_command_input_count[$__rate_interval])) by (status,attempted_conversion,datasource_type,input_frame_type)`
```

## 疑難排解

### 我的資料來源傳送了沒有框架類型的標籤化資料

如果您的資料來源傳送了沒有框架類型的標籤化資料，而您在 SQL 運算式中選取了它，您將會收到類似以下的錯誤：

```
[sse.sql.input_conversion] failed to convert the results of query [A] (Datasource Type: [grafana-mock-datasource]) into a SQL/Tabular format for sql expression [B]: can not convert because the response is missing the data type (frame.meta.type) and has labels in the response that can not be mapped to a table
```

### 我的資料來源傳回了多個沒有框架類型的框架

如果您的資料來源傳回了多個沒有框架類型的框架，您將會收到類似以下的錯誤：

```
[sse.sql.input_conversion] failed to convert the results of query [A] (Datasource Type: [grafana-mock-datasource]) into a SQL/Tabular format for sql expression [B]: can not convert because the response is missing the data type (frame.meta.type) and has more than one dataframe that can not be automatically mapped to a single table
```

### 我的資料來源傳回了多個具有不同類型的框架

目前不支援此功能。

在這種情況下，SQL 運算式會將第一個回應視為所有框架的類型。您很可能會得到一個奇怪的錯誤，或者結果將不可靠。
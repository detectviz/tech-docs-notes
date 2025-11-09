---
id: create-data-frames
title: 建立資料框架
description: 在插件開發中使用資料框架的指南。
keywords:
  - grafana
  - plugins
  - plugin
  - data frames
  - dataframes
---

[資料框架](../../key-concepts/data-frames)是一種欄式資料結構，可讓您有效率地查詢大量資料。由於資料框架是為 Grafana 開發資料來源和其他插件時的核心概念，因此在本指南中，我們將探討一些使用它們的方法。

`DataFrame` 介面包含一個 `name` 和一個 `fields` 陣列，其中每個欄位都包含該欄位的名稱、類型和值。

:::note

如果您想將現有插件遷移為使用資料框架格式，請參閱[遷移至資料框架](../../migration-guides/update-from-grafana-versions/v6.x-v7.x.md)。

:::

## 建立資料框架

如果您建立資料來源插件，那麼您很可能會想將外部 API 的回應轉換為資料框架。讓我們看看如何做到這一點。

讓我們從建立一個表示時間序列的簡單資料框架開始。建立資料框架最簡單的方法是使用 `toDataFrame` 函式。

```ts
// 長度必須相同。
const timeValues = [1599471973065, 1599471975729];
const numberValues = [12.3, 28.6];

// 從值建立資料框架。
const frame = toDataFrame({
  name: 'http_requests_total',
  fields: [
    { name: 'Time', type: FieldType.time, values: timeValues },
    { name: 'Value', type: FieldType.number, values: numberValues },
  ],
});
```

:::note

表示時間序列的資料框架至少包含一個 `time` 欄位和一個 `number` 欄位。依照慣例，內建插件會使用 `Time` 和 `Value` 作為包含時間序列資料的資料框架的欄位名稱。

:::

從範例中可以看出，若要像這樣建立資料框架，您的資料必須已儲存為欄式資料。如果您已將記錄以物件陣列的形式儲存，則可以將其傳遞給 `toDataFrame`。在這種情況下，`toDataFrame` 會嘗試根據陣列中物件的類型和名稱來猜測結構描述。若要以此方式建立複雜的資料框架，請務必驗證您是否得到預期的結構描述。

```ts
const series = [
  { Time: 1599471973065, Value: 12.3 },
  { Time: 1599471975729, Value: 28.6 },
];

const frame = toDataFrame(series);
frame.name = 'http_requests_total';
```

## 另請參閱

- [資料框架簡介](../../key-concepts/data-frames.md)
- [從資料來源讀取資料](../../how-to-guides/panel-plugins/read-data-from-a-data-source.md)
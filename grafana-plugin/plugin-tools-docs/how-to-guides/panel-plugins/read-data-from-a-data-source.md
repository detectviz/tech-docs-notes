---
id: read-data-from-a-data-source
title: 讀取資料來源插件傳回的資料框架
description: 在 Grafana 面板插件開發中讀取資料框架的指南。
keywords:
  - grafana
  - plugins
  - plugin
  - panel
  - data source
  - datasource
  - data frames
  - dataframes
---

[資料框架](../../key-concepts/data-frames)是一種欄式資料結構，可讓您有效率地查詢大量資料。由於資料框架是為 Grafana 開發插件時的核心概念，因此在本指南中，我們將探討一些在您的面板插件中使用它們來視覺化資料來源查詢傳回的資料的方法。

`DataFrame` 介面包含一個 `name` 和一個 `fields` 陣列，其中每個欄位都包含該欄位的名稱、類型和值。

# 從資料框架讀取值

當您建立面板插件時，資料來源傳回的資料框架可從您面板元件中的 `data` prop 取得。

```ts
function SimplePanel({ data: Props }) {
  const frame = data.series[0];

  // ...
}
```

在您開始讀取資料之前，請先思考您預期會得到什麼資料。例如，若要視覺化時間序列，您至少需要一個時間欄位和一個數字欄位。

```ts
const timeField = frame.fields.find((field) => field.type === FieldType.time);
const valueField = frame.fields.find((field) => field.type === FieldType.number);
```

其他類型的視覺化可能需要多個維度。例如，一個氣泡圖使用三個數字欄位：X 軸、Y 軸，以及一個用於每個氣泡半徑的欄位。在這種情況下，我們建議您讓使用者選擇要用於每個維度的欄位，而不是硬式編碼欄位名稱。

```ts
const x = frame.fields.find((field) => field.name === xField);
const y = frame.fields.find((field) => field.name === yField);
const size = frame.fields.find((field) => field.name === sizeField);

for (let i = 0; i < frame.length; i++) {
  const row = [x?.values[i], y?.values[i], size?.values[i]];

  // ...
}
```

或者，您可以使用 `DataFrameView`，它會給您一個物件陣列，其中每個物件都包含框架中每個欄位的屬性。

```ts
const view = new DataFrameView(frame);

view.forEach((row) => {
  console.log(row[options.xField], row[options.yField], row[options.sizeField]);
});
```

## 顯示資料框架中的值

欄位選項可讓使用者控制 Grafana 如何在資料框架中顯示資料。

若要將欄位選項套用至值，請在對應的欄位上使用 `display` 方法。結果包含顯示值時要使用的顏色和後綴等資訊。

```tsx
const valueField = frame.fields.find((field) => field.type === FieldType.number);

return (
  <div>
    {valueField
      ? valueField.values.map((value) => {
          const displayValue = valueField.display!(value);
          return (
            <p style={{ color: displayValue.color }}>
              {displayValue.text} {displayValue.suffix ? displayValue.suffix : ''}
            </p>
          );
        })
      : null}
  </div>
);
```

若要將欄位選項套用至欄位名稱，請使用 `getFieldDisplayName`。

```ts
const valueField = frame.fields.find((field) => field.type === FieldType.number);
const valueFieldName = getFieldDisplayName(valueField, frame);
```
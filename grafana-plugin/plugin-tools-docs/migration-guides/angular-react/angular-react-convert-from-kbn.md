---
id: angular-react-convert-from-kbn
title: 從 kbn 套件遷移
sidebar_position: 1
description: 如何將使用 kbn 套件的插件遷移至目前的方法。
keywords:
  - grafana
  - plugins
  - plugin
  - React
  - ReactJS
  - Angular
  - migration
  - kbn
---

# Angular 至 React：從 kbn 轉換

kbn 套件通常用於在基於 Angular 的 Grafana 插件中提供來自資料來源的格式化輸出。但是，您可能需要從 kbn 套件遷移，因為它已不再由 Grafana 核心提供。

具體來說，您的插件可能使用以下 kbn API 呼叫：

```ts
getUnitFormats();
valueFormats();
roundValue();
```

## 轉換為新方法

### 從 `getUnitFormats` 轉換

對於使用 `kbn.getUnitFormats()` 的插件，要使用的新方法來自 `@grafana/data`，稱為 `getValueFormats()`。

此方法傳回一個具有分類單位格式的物件，而不是單位的平面清單，並且應相應地處理。

一般來說，設定編輯器可以直接使用預設的單位格式提供者。但是，如果您需要一個單位選擇器，您可以使用 `@grafana/ui` 中的 `UnitPicker` 元件。
此元件的一個範例是 [Grafana 設計系統單位選擇器](https://developers.grafana.com/ui/latest/index.html?path=/story/pickers-and-editors-unitpicker--basic)。

### 從 `valueFormats` 轉換

在 Angular 插件中，一個常見的模式是使用 kbn 為特定單位取得格式化函式，然後使用一些參數呼叫該函式，如下所示：

```ts
const formatFunc = kbn.valueFormats[this.panel.format];
data.valueFormatted = formatFunc(data.value, decimalInfo.decimals, decimalInfo.scaledDecimals);
```

有幾種方法可以格式化一個值以包含用於文字輸出的單位，每種方法都針對不同的情境。

迭代框架的欄位以取得所有值欄位，然後處理它們中的每一個，如下列範例所示。這是一個基本的範例；通常，需要更多程式碼來包含 `valueMappings` 和其他覆寫。

```ts
import { formattedValueToString, getFieldDisplayName, getValueFormat, reduceField } from '@grafana/data';

const valueFields: Field[] = [];
for (const aField of frame.fields) {
  if (aField.type === FieldType.number) {
    valueFields.push(aField);
  }
}
for (const valueField of valueFields) {
  const standardCalcs = reduceField({ field: valueField!, reducers: ['bogus'] });
  const result = getValueFormat(valueField!.config.unit)(operatorValue, maxDecimals, undefined, undefined);
  const valueFormatted = formattedValueToString(result);
}
```

在 Grafana 隨附的面板中，有許多將框架處理為格式化文字輸出的範例。在 [GitHub 儲存庫](https://github.com/grafana/grafana)中搜尋這些函式以尋找範例，例如：

- `formattedValueToString`
- `getValueFormat`
- `reduceField`

### 從 `roundValue` 轉換

您的插件可能包含如下程式碼：

```ts
data.valueRounded = kbn.roundValue(data.value, decimalInfo.decimals);
```

像這樣轉換此程式碼：

```ts
import { roundDecimals } from '@grafana/data';
const valueRounded = roundDecimals(data.value, decimalInfo.decimals);
```

## 其他資源

格式化顯示字串的值（包括單位）可能包含前綴、後綴和其他自訂設定，例如文字本身的顏色。

在實作新方法時，可以進行許多自訂。以下是一些您可以參考的範例：

- [BarChart](https://github.com/grafana/grafana/blob/dc6cd4bb296dda4312395aaee0ee491d348f84bc/public/app/plugins/panel/barchart/distribute.ts#L7)
- [GeoMap](https://github.com/grafana/grafana/blob/dc6cd4bb296dda4312395aaee0ee491d348f84bc/public/app/plugins/panel/geomap/utils/measure.ts#L36)
- [PieChart](https://github.com/grafana/grafana/blob/dc6cd4bb296dda4312395aaee0ee491d348f84bc/public/app/plugins/panel/piechart/PieChartPanel.tsx#L118)
- [Polystat](https://github.com/grafana/grafana-polystat-panel/blob/ecc71d54c3e8819e66604f26aa31d72fb0432873/src/data/processor.ts#L278)
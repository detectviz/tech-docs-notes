---
id: build-a-panel-plugin
description: 了解如何為您的儀表板建立自訂視覺化。
title: 建立面板外掛程式
sidebar_position: 30
keywords:
  - grafana
  - plugins
  - plugin
  - visualization
  - custom visualization
  - dashboard
  - dashboards
---

import CreatePlugin from '@shared/create-plugin-frontend.md';
import PluginAnatomy from '@shared/plugin-anatomy.md';

## 簡介

面板可讓您以不同方式將資料視覺化，是 Grafana 的基本建構區塊之一。Grafana 已內建數種類型的面板，[Grafana 外掛程式目錄](https://grafana.com/grafana/plugins/)中還有更多可用的面板。

若要新增對其他視覺化的支援，您可以建立自己的面板外掛程式。面板是 [ReactJS 元件](https://reactjs.org/docs/components-and-props.html)，可以使用 `create-plugin` 工具來建立。

如需有關面板的更多資訊，請參閱[面板](https://grafana.com/docs/grafana/latest/panels/)上的文件。

### 先決條件

- Grafana v10.0 或更新版本
- [LTS](https://nodejs.dev/en/about/releases/) 版本的 Node.js

## 建立新外掛程式

<CreatePlugin pluginType="panel" />

## 外掛程式剖析

<PluginAnatomy />

## 面板外掛程式

### 面板屬性

[PanelProps](https://github.com/grafana/grafana/blob/57960148e47e4d82e899dbfa3cb9b2d474ad56dc/packages/grafana-data/src/types/panel.ts#L74-L122) 介面會公開面板的執行階段資訊，例如面板尺寸和目前的時間範圍。

您可以透過 `props` 引數存取面板屬性，如您的外掛程式中所示。

```js title="src/components/SimplePanel.tsx"
export const SimplePanel: React.FC<Props> = ({ options, data, width, height }) => {
```

## 開發工作流程

接下來，您將學習對面板進行變更、建置面板以及重新載入 Grafana 以反映您所做變更的基本工作流程。

首先，您需要將面板新增至儀表板：

### 將面板新增至儀表板

1. 在瀏覽器中開啟 Grafana。
   預設情況下，Grafana 可在 [http://localhost:3000](http://localhost:3000) 存取。

2. 建立新儀表板。

   - 從選單中，選取 **Dashboards**。
   - 在右上角，選取 **New** -> **Dashboard**。
   - 選取 **Add Visualization** 以開始設定您的新面板。

3. 設定 `TestData` 資料來源。在資料來源設定強制回應中，為此儀表板選取 **TestData DB** 資料來源。

4. 搜尋並選取您的面板外掛程式。在面板設定檢視中，前往右側的 **Visualization** 清單，搜尋您的面板外掛程式，然後選取它。

5. 儲存儀表板。

---

### 替代方案：使用已佈建的範例面板儀表板

您也可以透過預先設定的儀表板檢視面板的運作情形：

1. 前往 **Dashboards** 並選取 **Provisioned Sample Panel Dashboard**。
   - **TestData DB** 資料來源已為此儀表板設定範例資料。
2. 若要編輯，請在此儀表板中找到您的面板，然後按一下面板右上角的選單 (即三個垂直點)。
3. 從下拉式選單中，選取 **Edit** 以自訂或檢查面板的設定。

現在您可以檢視面板，請嘗試對面板外掛程式進行變更：

1. 在 `SimplePanel.tsx` 中，變更圓形的填滿顏色。例如，若要將其變更為綠色：

   ```ts title="src/components/SimplePanel.tsx"
   <circle style={{ fill: theme.visualization.getColorByName('green') }} r={100} />
   ```

2. 儲存檔案。
3. 在瀏覽器中，重新載入 Grafana。變更應該會出現。

## 新增面板選項

有時您會希望為面板的使用者提供一個選項，讓他們可以設定外掛程式的行為。透過為您的外掛程式設定 _panel options_，您的面板將能夠接受使用者輸入。

在上一步中，您在程式碼中變更了圓形的填滿顏色。讓我們變更程式碼，讓外掛程式使用者可以從面板編輯器設定顏色。

#### 新增選項

面板選項定義在 _panel options object_ 中。`SimpleOptions` 是一個描述選項物件的介面。

1. 在 `types.ts` 中，新增一個 `CircleColor` 型別來存放使用者可以選擇的顏色：

   ```
   type CircleColor = 'red' | 'green' | 'blue';
   ```

2. 在 `SimpleOptions` 介面中，新增一個名為 `color` 的新選項：

   ```
   color: CircleColor;
   ```

以下是更新後的選項定義：

```ts title="src/types.ts"
type SeriesSize = 'sm' | 'md' | 'lg';
type CircleColor = 'red' | 'green' | 'blue';

// 定義面板選項型別的介面
export interface SimpleOptions {
  text: string;
  showSeriesCount: boolean;
  seriesCountSize: SeriesSize;
  color: CircleColor;
}
```

#### 新增選項控制項

若要從面板編輯器變更選項，您需要將 `color` 選項繫結至 _option control_。

Grafana 支援一系列選項控制項，例如文字輸入、開關和選項群組。

讓我們建立一個選項控制項並將其繫結至 `color` 選項。

1. 在建構器的結尾新增控制項：

   ```ts title="src/module.ts"
   .addRadio({
     path: 'color',
     name: 'Circle color',
     defaultValue: 'red',
     settings: {
       options: [
         {
           value: 'red',
           label: 'Red',
         },
         {
           value: 'green',
           label: 'Green',
         },
         {
           value: 'blue',
           label: 'Blue',
         },
       ],
     }
   });
   ```

   `path` 用於將控制項繫結至選項。您可以透過指定選項物件內的完整路徑來將控制項繫結至巢狀選項，例如 `colors.background`。

Grafana 會為您建立一個選項編輯器，並將其顯示在面板編輯器側邊欄的 **Display** 區段中。

#### 使用新選項

您快完成了。您已新增一個新選項和一個對應的控制項來變更值。但外掛程式尚未使用該選項。讓我們來改變一下。

1. 若要將選項值轉換為目前主題所使用的顏色，請在 `SimplePanel.tsx` 中的 `return` 陳述式之前新增下列陳述式。

   ```ts title="src/components/SimplePanel.tsx"
   let color = theme.visualization.getColorByName(options.color);
   ```

2. 設定圓形以使用該顏色。

   ```tsx title="src/components/SimplePanel.tsx"
   <g>
     <circle style={{ fill: color }} r={100} />
   </g>
   ```

現在，當您在面板編輯器中變更顏色時，圓形的填滿顏色也會隨之變更。

## 使用資料框架建立動態面板

大多數面板都會視覺化來自 Grafana 資料來源的動態資料。在此步驟中，您將為每個序列建立一個圓形，每個圓形的半徑等於序列中的最後一個值。

:::info
若要在面板中使用查詢的資料，您需要設定資料來源。如果您沒有可用的資料來源，您可以在開發時使用 [TestData](https://grafana.com/docs/grafana/latest/features/datasources/testdata) 資料來源。
:::

將面板新增至儀表板時，請使用資料來源設定它，以動態填入資料。或者，您可以使用 **Provisioned Sample Panel Dashboard**，它已設定範例資料來源。

### 使用已佈建的範例面板儀表板

已佈建的範例面板儀表板已預先設定 `TestData` 資料來源。此設定包含用於測試和開發的範例資料。範例資料位於 [`TestData`](https://grafana.com/docs/grafana/latest/features/datasources/testdata) 資料來源的 `raw_frame` 情境中，包含兩個具有相同時間戳記的時間序列，如下表所示。下表顯示了這兩個由時間戳記連接的時間序列：

| 時間戳記 (Timestamp) | 標籤 1 (Label1) | 值 1 (Value1) | 標籤 2 (Label2) | 值 2 (Value2) |
| ------------------- | ------ | ------ | ------ | ------ |
| 2020-12-31 19:00:00 | A      | 10     | A      | 40     |
| 2020-12-31 20:00:00 | B      | 20     | B      | 50     |
| 2020-12-31 21:00:00 | C      | 15     | C      | 45     |
| 2020-12-31 22:00:00 | D      | 25     | D      | 55     |
| 2020-12-31 23:00:00 | E      | 30     | E      | 60     |

此設定可讓您使用真實的範例資料測試面板的動態元素，該資料結構具有多個序列，可進行更複雜的視覺化。

面板中資料來源查詢的結果可在面板元件內的 `data` 屬性中取得。

```ts
const { data } = props;
```

`data.series` 包含從資料來源查詢傳回的序列。每個序列都表示為一個稱為 _data frame_ 的資料結構。資料框架類似於表格，其中資料按欄或 _fields_ 而非列儲存。欄位中的每個值都共用相同的資料類型，例如字串、數字或時間。

以下是具有時間欄位 `Time` 和數字欄位 `Value` 的資料框架範例：

| 時間 (Time)   | 值 (Value) |
| ------------- | ----- |
| 1589189388597 | 32.4  |
| 1589189406480 | 27.2  |
| 1589189513721 | 15.0  |

已佈建的範例面板儀表板\*\* 範例資料：
| 時間戳記 (Timestamp) | 標籤 1 (Label1) | 值 1 (Value1) | 標籤 2 (Label2) | 值 2 (Value2) |
| ------------------- | ------ | ------ | ------ | ------ |
| 2020-12-31 19:00:00 | A | 10 | A | 40 |
| 2020-12-31 20:00:00 | B | 20 | B | 50 |
| 2020-12-31 21:00:00 | C | 15 | C | 45 |
| 2020-12-31 22:00:00 | D | 25 | D | 55 |
| 2020-12-31 23:00:00 | E | 30 | E | 60 |

讓我們看看如何從資料框架中擷取資料並在您的視覺化中使用它。

1. 透過在 `SimplePanel.tsx` 的 `return` 陳述式之前新增下列程式碼，取得類型為 `number` 的每個欄位的最後一個值：

   ```ts title="src/components/SimplePanel.tsx"
   const radii = data.series
     .map((series) => series.fields.find((field) => field.type === 'number'))
     .map((field) => field?.values.get(field.values.length - 1));
   ```

   `radii` 將包含從資料來源查詢傳回的每個序列中的最後一個值。您將使用這些值來設定每個圓形的半徑。

2. 將 `svg` 元素變更為下列內容：

   ```tsx title="src/components/SimplePanel.tsx"
   <svg
     className={styles.svg}
     width={width}
     height={height}
     xmlns="http://www.w3.org/2000/svg"
     xmlnsXlink="http://www.w3.org/1999/xlink"
     viewBox={`0 -${height / 2} ${width} ${height}`}
   >
     <g fill={color}>
       {radii.map((radius, index) => {
         const step = width / radii.length;
         return <circle r={radius} transform={`translate(${index * step + step / 2}, 0)`} />;
       })}
     </g>
   </svg>
   ```

   請注意我們如何為 `radii` 中的每個值建立一個 `<circle>` 元素：

   ```tsx title="src/components/SimplePanel.tsx"
   {
     radii.map((radius, index) => {
       const step = width / radii.length;
       return <circle r={radius} transform={`translate(${index * step + step / 2}, 0)`} />;
     });
   }
   ```

   我們在此處使用 `transform` 將圓形水平分佈在面板內。

3. 重建您的外掛程式，並透過將多個查詢新增至面板來試用它。重新整理儀表板。

如果您想進一步了解資料框架，請參閱我們的[資料框架](../key-concepts/data-frames.md)簡介。

## 摘要

在本教學中，您學習了如何為您的儀表板建立自訂視覺化。
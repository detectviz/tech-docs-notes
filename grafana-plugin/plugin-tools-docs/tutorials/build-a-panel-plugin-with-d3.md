---
id: build-a-panel-plugin-with-d3
title: 使用 D3.js 建立面板外掛程式
sidebar_position: 7
description: 了解如何在您的面板外掛程式中使用 D3.js。
draft: true
keywords:
  - grafana
  - plugins
  - plugin
  - d3js
  - d3
  - panel
  - panel plugin
---

import CreatePlugin from '@shared/create-plugin-frontend.md';

## 簡介

面板可讓您以不同方式將資料視覺化，是 Grafana 的基本建構區塊之一。Grafana 已內建數種類型的面板，[Grafana 外掛程式目錄](https://grafana.com/grafana/plugins/)中還有更多可用的面板。

若要新增對其他視覺化的支援，您可以建立自己的面板外掛程式。面板是 [ReactJS 元件](https://reactjs.org/docs/components-and-props.html)，可以使用 `create-plugin` 工具來建立。

如需有關面板的更多資訊，請參閱[面板](https://grafana.com/docs/grafana/latest/panels/)上的文件。

本教學將引導您實際操作，使用 [D3.js](https://d3js.org/) 建立您自己的面板。

在本教學中，您將：

- 建立一個簡單的面板外掛程式來視覺化長條圖。
- 了解如何使用 D3.js 建立使用資料驅動轉換的面板。

### 先決條件

- Grafana v10.0 或更新版本
- [LTS](https://nodejs.dev/en/about/releases/) 版本的 Node.js

## 建立新外掛程式

<CreatePlugin pluginType="panel" />

## 資料驅動文件

[D3.js](https://d3js.org/) 是一個 JavaScript 程式庫，用於根據資料操作文件。它可讓您將任意資料轉換為 HTML，通常用於建立視覺化。

等一下。根據資料操作文件？這聽起來很像 React。事實上，您可以使用 React 完成許多 D3 可以完成的事情。因此，在我們開始檢視 D3 之前，讓我們看看如何僅使用 React 從資料建立 SVG。

1. 在本教學中，請移除下列程式碼：

   ```tsx title="src/components/SimplePanel.tsx"
   viewBox={`-${width / 2} -${height / 2} ${width} ${height}`}
   ```

   和

   ```tsx title="src/components/SimplePanel.tsx"
   <div className={styles.textBox}>
     {options.showSeriesCount && <div>Number of series: {data.series.length}</div>}
     <div>Text option value: {options.text}</div>
   </div>
   ```

2. 現在，將 SVG 群組 `g` 變更為傳回 `rect` 元素，而不是圓形。

   ```tsx title="src/components/SimplePanel.tsx"
   <g>
     <rect x={0} y={0} width={30} height={10} fill={theme.visualization.getColorByName('green')} />
   </g>
   ```

一個矩形可能不太令人興奮，所以讓我們看看如何從資料建立矩形。

1.  建立一些我們可以視覺化的資料。

    ```ts title="src/components/SimplePanel.tsx"
    const values = [4, 8, 15, 16, 23, 42];
    ```

2.  根據面板的高度計算每個長條的高度。

    ```ts title="src/components/SimplePanel.tsx"
    const barHeight = height / values.length;
    ```

3.  在 SVG 群組 `g` 內，為資料集中的每個值建立一個 `rect` 元素。每個矩形都使用該值作為其寬度。

    ```tsx title="src/components/SimplePanel.tsx"
    <g>
      {values.map((value, i) => (
        <rect
          key={value}
          x={0}
          y={i * barHeight}
          width={value}
          height={barHeight - 1}
          fill={theme.visualization.getColorByName('green')}
        />
      ))}
    </g>
    ```

如您所見，React 完全能夠動態建立 HTML 元素。事實上，使用 React 建立元素通常比使用 D3 建立元素更快。

那麼，您為什麼還要使用 D3 呢？在下一步中，我們將了解如何利用 D3 的資料轉換。

## 使用 D3.js 轉換資料

在此步驟中，您將了解如何在呈現資料之前使用 D3 轉換資料。

D3 已與 Grafana 捆綁在一起，您可以透過匯入 `d3` 套件來存取它。但是，我們在開發時需要型別定義。

1. 安裝 D3 型別定義：

   ```bash
   npm install --save-dev @types/d3
   ```

2. 匯入 `d3`：

   ```ts title="src/components/SimplePanel.tsx"
   import * as d3 from 'd3';
   ```

在上一步中，我們必須以像素為單位定義每個長條的寬度。相反地，讓我們使用 D3 程式庫中的 _scales_，讓每個長條的寬度取決於面板的寬度。

比例尺是將一個值範圍對應到另一個值範圍的函式。在這種情況下，我們希望將資料集中的值對應到面板中的位置。

1. 建立一個比例尺，將 0 和資料集中最大值之間的值對應到 0 和面板寬度之間的值。我們將使用它來計算長條的寬度。

   ```ts title="src/components/SimplePanel.tsx"
   const scale = d3
     .scaleLinear()
     .domain([0, d3.max(values) || 0.0])
     .range([0, width]);
   ```

2. 將值傳遞給比例尺函式，以像素為單位計算長條的寬度。

   ```tsx title="src/components/SimplePanel.tsx"
   return (
     <svg width={width} height={height}>
       <g>
         {values.map((value, i) => (
           <rect
             key={value}
             x={0}
             y={i * barHeight}
             width={scale(value)}
             height={barHeight - 1}
             fill={theme.visualization.getColorByName('green')}
           />
         ))}
       </g>
     </svg>
   );
   ```

如您所見，即使我們使用 React 來呈現實際元素，D3 程式庫也包含可用於在呈現資料之前轉換資料的實用工具。

## 新增軸

D3 工具箱中的另一個實用工具是產生 _axes_ 的能力。在圖表中新增軸線可讓使用者更容易了解每個長條之間的差異。

讓我們看看如何使用 D3 在長條圖中新增水平軸。

1. 建立一個 D3 軸。請注意，透過使用與之前相同的比例尺，我們可以確保長條寬度與軸上的刻度對齊。

   ```ts title="src/components/SimplePanel.tsx"
   const axis = d3.axisBottom(scale);
   ```

2. 產生軸。雖然 D3 需要產生軸的元素，但我們可以透過在匿名函式中產生它們來封裝它，並將其作為 `ref` 傳遞給群組元素 `g`。

   ```tsx title="src/components/SimplePanel.tsx"
   <g
     ref={(node) => {
       d3.select(node).call(axis as any);
     }}
   />
   ```

預設情況下，軸會呈現在 SVG 元素的頂部。我們希望將其移至底部，但要做到這一點，我們首先需要透過減少每個長條的高度來為其騰出空間。

1. 根據填補高度計算新的長條高度。

   ```ts title="src/components/SimplePanel.tsx"
   const padding = 20;
   const chartHeight = height - padding;
   const barHeight = chartHeight / values.length;
   ```

2. 透過將轉換新增至 `g` 元素來平移軸。

   ```tsx title="src/components/SimplePanel.tsx"
   <g
     transform={`translate(0, ${chartHeight})`}
     ref={(node) => {
       d3.select(node).call(axis as any);
     }}
   />
   ```

恭喜！您已建立一個簡單且回應式的長條圖。

## 完整範例

```tsx title="src/components/SimplePanel.tsx"
import React from 'react';
import { PanelProps } from '@grafana/data';
import { SimpleOptions } from 'types';
import { css, cx } from '@emotion/css';
import { useStyles2, useTheme2 } from '@grafana/ui';
import * as d3 from 'd3';

interface Props extends PanelProps<SimpleOptions> {}

const getStyles = () => {
  return {
    wrapper: css`
      font-family: Open Sans;
      position: relative;
    `,
    svg: css`
      position: absolute;
      top: 0;
      left: 0;
    `,
    textBox: css`
      position: absolute;
      bottom: 0;
      left: 0;
      padding: 10px;
    `,
  };
};

export const SimplePanel: React.FC<Props> = ({ options, data, width, height }) => {
  const theme = useTheme2();
  const styles = useStyles2(getStyles);
  const values = [4, 8, 15, 16, 23, 42];
  const padding = 20;
  const chartHeight = height - padding;
  const barHeight = chartHeight / values.length;
  const scale = d3
    .scaleLinear()
    .domain([0, d3.max(values) || 0.0])
    .range([0, width]);
  const axis = d3.axisBottom(scale);

  return (
    <div
      className={cx(
        styles.wrapper,
        css`
          width: ${width}px;
          height: ${height}px;
        `
      )}
    >
      <svg
        className={styles.svg}
        width={width}
        height={height}
        xmlns="http://www.w3.org/2000/svg"
        xmlnsXlink="http://www.w3.org/1999/xlink"
      >
        <g>
          {values.map((value, i) => (
            <rect
              key={value}
              x={0}
              y={i * barHeight}
              width={scale(value)}
              height={barHeight - 1}
              fill={theme.visualization.getColorByName('green')}
            />
          ))}
        </g>
        <g
          transform={`translate(0, ${chartHeight})`}
          ref={(node) => {
            d3.select(node).call(axis as any);
          }}
        />
      </svg>
    </div>
  );
};
```

## 摘要

在本教學中，您使用 D3.js 建立了一個面板外掛程式。
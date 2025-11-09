---
id: angular-react-convert-from-time_series2
title: 從 time_series2 套件遷移
sidebar_position: 6
description: 如何將使用 app/core/time_series2 套件的插件遷移至目前的方法。
keywords:
  - grafana
  - plugins
  - plugin
  - React
  - ReactJS
  - Angular
  - migration
  - time_series2
---

# Angular 至 React：從 app/core/time_series2 轉換

`app/core/time_series2` 套件通常由 AngularJS 插件用來擷取要由面板呈現的資料。此套件已不再提供，所有插件都必須改用資料框架。

本指南提供一種將舊函式庫轉換為新資料框架格式的方法。

## 使用 AngularJS 方法轉換資料

在 AngularJS 中移除 `app/core/time_series2` 套件之前，資料是使用類似以下的方法由面板呈現的：

```ts
seriesHandler(seriesData: any) {
    const series = new TimeSeries({
      datapoints: seriesData.datapoints,
      alias: seriesData.target,
    });
    series.flotpairs = series.getFlotPairs(this.panel.nullPointMode);
    return series;
  }
```

## 使用資料框架轉換資料

以下程式碼範例顯示一種使用資料框架從面板轉換資料的方法：

```ts
import {
  GrafanaTheme2,
  FieldDisplay,
  getDisplayProcessor,
  getFieldDisplayValues,
  FieldConfig,
  DisplayValue,
  formattedValueToString,
} from '@grafana/data';

const theme2 = useTheme2();

const getValues = (): FieldDisplay[] => {
  for (const frame of data.series) {
    for (const field of frame.fields) {
      // 為 percent 和 percentunit 自動設定最小值/最大值
      if (field.config.unit === 'percent' || field.config.unit === 'percentunit') {
        const min = field.config.min ?? 0;
        const max = field.config.max ?? (field.config.unit === 'percent' ? 100 : 1);
        field.state = field.state ?? {};
        field.state.range = { min, max, delta: max - min };
        field.display = getDisplayProcessor({ field, theme: theme2 });
      }
    }
  }
  return getFieldDisplayValues({
    fieldConfig,
    reduceOptions: {
      calcs: [options.operatorName],
      values: false,
    },
    replaceVariables,
    theme: theme2,
    data: data.series,
    timeZone,
  });
};

const getThresholdForValue = (field: FieldConfig, value: number, theme: GrafanaTheme2) => {
  if (fieldConfig.defaults.thresholds) {
    const result = getActiveThreshold(value, field.thresholds?.steps);
    return result;
  }
  return null;
};

const getFormattedValue = (index: number) => {
  const singleMetric = metrics[index];
  return formattedValueToString(singleMetric.display);
};

const getDisplayValue = (index: number) => {
  const singleMetric = metrics[index];
  if (!isNaN(singleMetric.display.numeric)) {
    return singleMetric.display.numeric;
  }
  return NaN;
};

// 取得格式化的指標
const metrics = getValues();
```

## 其他資源

- 閱讀更多[Angular 至 React 轉換指南](/migration-guides/angular-react/)。
- 深入了解[資料框架](../../key-concepts/data-frames)。
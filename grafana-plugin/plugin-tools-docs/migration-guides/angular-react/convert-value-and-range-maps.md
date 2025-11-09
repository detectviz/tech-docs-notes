---
id: angular-react-convert-mappings
title: 轉換值和範圍對應
sidebar_position: 3
description: 如何將使用值和範圍對應的 Angular 插件遷移至 React
keywords:
  - grafana
  - plugins
  - plugin
  - React
  - ReactJS
  - Angular
  - migration
  - rangemap
  - valuemap
---

# 轉換值和範圍對應

使用 AngularJS SDK 建立的 Grafana 插件可以使用值和範圍對應的編輯器。將您的插件更新至 React 時，需要將這些對應的設定選項轉換為新的格式。

## 新增新的內建編輯器

在您插件的 `module.ts` 檔案中，新增 `Mappings` 選項以啟用新的編輯器：

```ts
.useFieldConfig({
    standardOptions: {
      [FieldConfigProperty.Mappings]: {},
    },
  })
```

有關新元件的描述，請參閱[設定值對應](https://grafana.com/docs/grafana/latest/panels-visualizations/configure-value-mappings/)文件。

## 將對應轉換為新格式

使用下面顯示的輔助函式來更新您插件的設定。

```ts
import { PanelModel, convertOldAngularValueMappings, ValueMapping } from '@grafana/data';

export const PolystatPanelMigrationHandler = (panel: PanelModel<PolystatOptions>): Partial<PolystatOptions> => {
  // 轉換範圍和值對應
  const newMaps = migrateValueAndRangeMaps(panel);
  panel.options.fieldConfig = {
    defaults: {
      mappings: newMaps,
    },
    overrides: [],
  };
  //@ts-ignore
  delete panel.mappingType;
  //@ts-ignore
  delete panel.rangeMaps;
  //@ts-ignore
  delete panel.valueMaps;
  // 傳回新設定
  return panel.options;
};

export const migrateValueAndRangeMaps = (panel: any) => {
  // 先處理值對應
  panel.mappingType = 1;
  let newValueMappings: ValueMapping[] = [];
  if (panel.valueMaps !== undefined) {
    newValueMappings = convertOldAngularValueMappings(panel);
  }
  // 再處理範圍對應
  panel.mappingType = 2;
  let newRangeMappings: ValueMapping[] = [];
  if (panel.rangeMaps !== undefined) {
    newRangeMappings = convertOldAngularValueMappings(panel);
  }
  // 將兩者串接起來
  const newMappings = newValueMappings.concat(newRangeMappings);
  // 只取唯一值
  return [...new Map(newMappings.map((v) => [JSON.stringify(v), v])).values()];
};
```

:::tip

請務必清理舊的設定，以免遷移重複執行。

:::

有關另一個說明，請參閱[此範例](https://github.com/grafana/grafana-polystat-panel/blob/main/src/migrations.ts#L131)。
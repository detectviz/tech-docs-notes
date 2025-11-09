---
id: visualizations
title: 視覺化
---

# 視覺化

您可以使用場景物件類別 `VizPanel` 將視覺化新增至您的場景中。

## 簡單的 `VizPanel` 範例

```ts
new VizPanel({
    pluginId: 'timeseries',
    title: 'Time series',
    options: {
        legend: {
            showLegend: false,
        }
    },
    fieldConfig: {
        defaults: {
            unit: 'bytes',
            min: 0,
            custom: { lineWidth: 2, fillOpacity: 6 },
        },
        overrides: [],
    }
})
```

:::note
上述範例中使用的 pluginId `timeseries` 是指核心 Grafana 面板外掛程式，也就是時間索引資料的標準圖表視覺化。`options` 和 `fieldConfig` 與您在面板檢查抽屜中檢視 **JSON** 標籤時在典型儀表板面板中看到的選項相同。若要存取此標籤，請在面板編輯選單中按一下 **Inspect > Panel JSON**。
:::

## 資料

`VizPanel` 使用 `sceneGraph.getData(model)` 呼叫來尋找並訂閱最接近且具有 `SceneDataProvider` 物件的父物件。這表示 `VizPanel` 使用在其自身層級設定的 `$data`，或者如果 `$data` 設定在任何父層級，則與其他同層級物件和場景物件共用資料。

## 標頭動作

`VizPanel` 有一個名為 `headerActions` 的屬性，可以是 `React.ReactNode` 或自訂的 `SceneObject`。如果您想在面板標頭的右上角放置連結或按鈕，此屬性非常有用。例如：

## 選單

類型為 VizPanelMenu 的 menu 屬性是可選的，設定後會在面板的右上角定義一個選單。選單物件僅在下拉選單本身呈現時才會啟動。因此，新增動態選單動作和連結的最佳方式是將它們新增至附加到選單的[行為](./advanced-behaviors.md)中。

```ts
new VizPanel({
  pluginId: 'timeseries',
  title: 'Time series',
  headerActions: (
    <LinkButton size="sm" variant="secondary" href="scenes/drilldown/url">
      鑽取
    </LinkButton>
  ),
});
```

面板標頭右上角的按鈕可用於：

- 連結到其他場景
- 變更目前場景的按鈕 (例如，新增鑽取頁面)
- 變更視覺化設定的 `RadioButtonGroup`

對於 `LinkButton`、`Button` 和 `RadioButtonGroup`，當您將它們放置在面板標頭中時，請使用 size="sm"。

## 標準 Grafana 視覺化

Scenes 附帶一個輔助 API `PanelBuilders`，用於建置[標準 Grafana 視覺化](https://grafana.com/docs/grafana/latest/panels-visualizations/visualizations/)。這些包括：

- 長條圖
- 長條圖量表
- 資料網格
- 火焰圖
- 量表
- 地理地圖
- 熱圖
- 直方圖
- 日誌
- 新聞
- 節點圖
- 圓餅圖
- 統計
- 狀態時間軸
- 狀態歷史記錄
- 表格
- 文字
- 時間序列
- 趨勢
- 追蹤
- XY 圖

`PanelBuilders` API 支援為上述視覺化建置 `VizPanel` 物件，並支援面板選項和欄位設定。

### 步驟 1. 匯入 `PanelBuilders` API

```ts
import { PanelBuilders } from '@grafana/scenes';
```

### 步驟 2. 設定標準視覺化 `VizPanel` 物件

```ts
const myTimeSeriesPanel = PanelBuilders.timeseries().setTitle('我的第一個面板');
```

### 步驟 3. 設定資料和時間範圍

```ts
const data = new SceneQueryRunner({
  datasource: {
    type: 'prometheus',
    uid: '<PROVIDE_GRAFANA_DS_UID>',
  },
  queries: [
    {
      refId: 'A',
      expr: 'rate(prometheus_http_requests_total{}[5m])',
    },
  ],
  $timeRange: new SceneTimeRange({ from: 'now-5m', to: 'now' }),
});

myTimeSeriesPanel.setData(data);
```

### 步驟 4. 設定面板選項

```ts
myTimeSeriesPanel.setOption('legend', { asTable: true }).setOption('tooltip', { mode: TooltipDisplayMode.Single });
```

### 步驟 5. 設定標準選項

所有 Grafana 視覺化都附帶標準選項。`PanelBuilders` 提供單獨設定每個標準選項的方法。
在官方 [Grafana 文件](https://grafana.com/docs/grafana/latest/panels-visualizations/configure-standard-options/#standard-options-definitions)中閱讀有關標準選項的更多資訊。

```ts
myTimeSeriesPanel.setDecimals(2).setUnit('ms');
```

### 步驟 6. 設定自訂欄位設定

Grafana 視覺化提供稱為「欄位設定」的自訂、特定於視覺化的設定選項。
在官方 [Grafana 文件](https://grafana.com/docs/grafana/latest/developers/plugins/data-frames/#field-configurations)中閱讀有關欄位設定的更多資訊。

使用 `setCustomFieldConfig` 方法設定所需欄位設定屬性的值。

```ts
myTimeSeriesPanel.setCustomFieldConfig('lineInterpolation', LineInterpolation.Smooth);
```

### 步驟 7. 設定覆寫

Grafana 視覺化可讓您為特定欄位或序列自訂視覺化設定。這是透過新增一個針對特定欄位集的覆寫規則來實現的，每個規則都可以定義多個選項。在官方 [Grafana 文件](https://grafana.com/docs/grafana/latest/panels-visualizations/configure-overrides/)中閱讀有關覆寫的更多資訊。

使用 `setOverrides` 方法設定所需的欄位設定覆寫。對於標準選項，請使用 `override<OptionName>` 方法。對於自訂欄位設定，請使用 `overrideCustomConfigProperty` 方法。

```ts
myTimeSeriesPanel.setOverrides((b) =>
  b.matchFieldsWithNameByRegex('/metrics/').overrideDecimals(4).overrideCustomFieldConfig('lineWidth', 5)
);
```

單一覆寫設定以**匹配器**設定開始。 благодаря 匹配器 Grafana 知道應將覆寫應用於結果的哪個部分。可用的匹配器如下：

#### `matchFieldsWithName(name: string)`

根據提供的欄位名稱選取欄位。您使用此選擇器新增至規則的屬性僅適用於此單一欄位。

#### `matchFieldsWithNameByRegex(regex: string)`

使用正規表示式指定要覆寫的欄位。您使用此選擇器新增至規則的屬性會應用於欄位名稱與正規表示式相符的所有欄位。

#### `matchFieldsByType(fieldType: FieldType)`

按類型選取欄位，例如字串、數字等。您使用此選擇-器新增至規則的屬性會應用於與所選類型相符的所有欄位。

#### `matchFieldsByQuery(refId: string)`

選取由特定查詢傳回的所有欄位，例如 A、B 或 C。您使用此選擇器新增至規則的屬性會應用於由所選查詢傳回的所有欄位。

#### `matchFieldsByValue(options: FieldValueMatcherConfig)`

選取符合所提供值條件設定的所有欄位。此匹配器允許根據對序列的簡化值執行的條件進行覆寫設定。例如，您可以為平均值高於所提供值的序列設定覆寫。

#### `matchComparisonQuery(refId: string)`

選取由比較查詢傳回的所有欄位。您使用此選擇器新增至規則的屬性會應用於為所選查詢執行的比較查詢所傳回的所有欄位。閱讀更多關於[時間範圍比較](./advanced-time-range-comparison.md)的資訊。

### 步驟 8. 建置視覺化

使用 `build` 方法產生已設定的 `VizPanel` 物件：

```ts
const myPanel = myTimeSeriesPanel.build();
```

### 步驟 9. 將視覺化新增至場景

建立一個具有版面配置的場景，並將視覺化新增為版面配置子項目：

```ts
const scene = new EmbeddedScene({
  body: new SceneFlexLayout({
    children: [
      new SceneFlexItem({
        body: myPanel,
      }),
    ],
  }),
});
```

這個建置好的面板現在可以在場景中使用了。

### 將常見的視覺化設定提取到混入函式中

```ts
function latencyGraphMixin(builder: ReturnType<typeof PanelBuilders["timeseries"]>) {
  builder.setMin(0);
  builder.setOption('legend', { showLegend: false: true })
}

const panel = PanelBuilders.timeseries().applyMixin(latencyGraphMixin).setData(...)
```

## 自訂視覺化

如果您想決定如何在您的 Grafana 應用程式外掛程式中視覺化資料，您有兩種方法。您始終可以選擇建立自訂的 `SceneObject`，但您將無法獲得 `VizPanel` 提供的具有載入狀態和其他功能的 `PanelChrome`。如果您想要在面板框架內有一個自訂視覺化，且其外觀應與場景中的其他面板相似，那麼最好註冊一個執行時面板外掛程式。

### 步驟 1. 定義自訂面板選項和欄位設定

```ts
interface CustomVizOptions {
  mode: string;
}

interface CustomVizFieldOptions {
  numericOption: number;
}

interface Props extends PanelProps<CustomVizOptions> {}
```

### 步驟 2. 定義渲染自訂 `PanelPlugin` 的 React 元件

```ts
export function CustomVizPanel(props: Props) {
  const { options, data } = props;

  return (
    <div>
      <h4>
        CustomVizPanel options: <pre>{JSON.stringify(options)}</pre>
      </h4>
      <div>
        CustomVizPanel field config: <pre>{JSON.stringify(data.series[0]?.fields[0]?.config)}</pre>
      </div>
    </div>
  );
}
```

### 步驟 3. 建立 `PanelPlugin` 實例並向 Scenes 程式庫註冊

```ts
import { sceneUtils } from '@grafana/scenes';

const myCustomPanel = new PanelPlugin<CustomVizOptions, CustomVizFieldOptions>(CustomVizPanel).useFieldConfig({
  useCustomConfig: (builder) => {
    builder.addNumberInput({
      path: 'numericOption',
      name: 'Numeric option',
      description: 'A numeric option',
      defaultValue: 1,
    });
  },
});

sceneUtils.registerRuntimePanelPlugin({ pluginId: 'my-scene-app-my-custom-viz', plugin: myCustomPanel });
```

### 步驟 4. 在場景中使用自訂面板

您現在可以在任何 `VizPanel` 中使用此 pluginId。請確保您指定的 pluginId 包含您的場景應用程式名稱，並且不太可能與其他 Scenes 應用程式衝突。

```ts
const data = new SceneQueryRunner({
  datasource: {
    type: 'prometheus',
    uid: 'gdev-prometheus',
  },
  queries: [
    {
      refId: 'A',
      expr: 'rate(prometheus_http_requests_total{}[5m])',
    },
  ],
  $timeRange: new SceneTimeRange({ from: 'now-5m', to: 'now' }),
});

return new EmbeddedScene({
  $data: data,
  body: new SceneFlexLayout({
    children: [
      new SceneFlexItem({
        body: new VizPanel({
          pluginId: 'my-scene-app-my-custom-viz',
          options: { mode: 'my-custom-mode' },
          fieldConfig: {
            defaults: {
              unit: 'ms',
              custom: {
                numericOption: 100,
              },
            },
            overrides: [],
          },
        }),
      }),
    ],
  }),
});
```

更多資訊，請參閱官方的[建置面板外掛程式教學](https://grafana.com/tutorials/build-a-panel-plugin)。請記住，對於 Scenes 執行時面板外掛程式，您不需要為面板外掛程式提供 plugin.json 檔案，因為它不會是您可以在儀表板中使用的獨立外掛程式。您只能在您的 Scenes 應用程式中參考該外掛程式。

## 原始碼

[檢視範例原始碼](https://github.com/grafana/scenes/tree/main/docusaurus/docs/visualizations.tsx)
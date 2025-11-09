---
id: advanced-adhoc-filters
title: 臨機操作篩選器
---

臨機操作篩選器提供了一種非常強大的方式，可以動態變更場景檢視中查詢的範圍或篩選。與通常控制預定義標籤或維度的變數不同，使用臨機操作篩選器，使用者可以控制要篩選的鍵 (標籤)。

## 自動套用至查詢

許多資料來源直接支援臨機操作篩選器。這表示它們會自動將篩選器套用至所有與您在 `AdHocFiltersVariable` 上指定的資料來源相符的查詢。

### 步驟 1. 建立 AdHocFiltersVariable

首先定義 `AdHocFiltersVariable`。

```ts
const filtersVar = new AdHocFiltersVariable({
  name: 'Filters',
  datasource: {
    type: 'prometheus',
    uid: '<PROVIDE_GRAFANA_DS_UID>',
  },
  // 您不需要設定 baseFilters，但如果您想將標籤建議限制為您認為與場景相關的建議，它們會很有用。
  // 這些不會顯示在 UI 中。
  baseFilters: [{ key: '__name__', operator: '=', value: 'ALERTS', condition: '' }],
  // 如果您想預設新增任何預設篩選器，可以在此處指定。
  filters: [],
});
```

接下來，將此 `filtersVar` 新增至您的 SceneVariableSet 的 variables 陣列中。

範例：

```ts
const scene = new EmbeddedScene({
  $variables: new SceneVariableSet({ variables: [filterVar] }),
  ...
});
```

### 自動模式如何運作

`AdHocFiltersVariable` 的行為由 `applyMode` 選項控制。當設定為 `auto` (預設值) 時，任何篩選器的任何變更都會自動重新觸發場景中所有設定了與 `AdHocFiltersVariable` 相同資料來源 UID 的 `SceneQueryRunners`。資料來源實作將處理修改所有查詢以包含目前的篩選器。

## 自訂標籤和值建議

預設情況下，標籤建議將來自資料來源實作的 `getTagKeys`。您在擷取建議時應考慮現有的篩選器和 `baseFilters`，以便篩選器也可以影響其他篩選器的建議標籤和值。其他篩選器被考慮在內的這種行為是新的，並非所有資料來源都支援。

值是從資料來源實作的 `getTagValues` 中擷取的。標籤鍵和標籤值都可以使用兩個狀態屬性來自訂：`getTagKeysProvider` 和 `getTagValuesProvider`。

範例：

```ts
const filterSet = new AdHocFiltersVariable({
  name: 'Filters',
  datasource: {
    type: 'prometheus',
    uid: '<PROVIDE_GRAFANA_DS_UID>',
  },
  getTagKeysProvider: () => {
    return Promise.resolve({
      replace: true,
      values: [
        { text: 'service_namespace', value: 'service_namespace' },
        { text: 'technology', value: 'technology' },
      ],
    });
  },
  getTagValuesProvider: (set: AdHocFilterSet, filter: AdHocVariableFilter) => {
    // 自訂值查詢
    return Promise.resolve({ replace: false, values: [] });
  },
});
```

使用這兩個函式，您可以完全自訂鍵和值查詢。使用傳回物件上的 `replace` 屬性，您可以控制結果是應該取代預設實作 (結果) 還是使用例如來自另一個資料來源的鍵/標籤來擴充預設結果。

## 手動模式

如果您不希望將篩選器套用至與 `AdHocFiltersVariable` 相同資料來源的所有查詢，並且希望對套用篩選器的查詢有更多控制權，您可以將 `applyMode` 設定為 `manual`，然後以您想要的任何方式使用篩選器。例如，您可以訂閱 `AdHocFiltersVariable` 狀態，然後使用篩選器以某種有趣的方式修改場景。

另一種方法是在查詢運算式中將篩選器作為普通變數使用。

範例：

```ts
$variables: new SceneVariableSet({
  variables: [
    new AdHocFiltersVariable({
      name: 'filters',
      applyMode: 'manual',
      datasource: { uid: 'gdev-prometheus' },
      filters: [{ key: 'job', operator: '=', value: 'grafana', condition: '' }],
    }),
  ],
}),
```

使用此變數，您現在可以透過將其作為普通變數在特定查詢中輕鬆使用篩選器。

範例：

```ts
new SceneQueryRunner({
  datasource: { uid: 'gdev-prometheus' },
  queries: [
    {
      refId: 'A',
      expr: 'ALERTS{$filters}',
      format: 'table',
      instant: true,
    },
  ],
});
```

如此設定的查詢包含變數運算式 `$filters`。您可以變更變數的名稱。預設情況下，`AdHocFiltersVariable` 會將篩選器呈現為以逗號分隔的有效 Prometheus 標籤篩選器運算式。
---
id: add-support-for-variables
title: 新增對變數的支援
description: 在 Grafana 插件開發中新增對變數的支援。
keywords:
  - grafana
  - plugins
  - plugin
  - queries
  - variables
---

# 新增對變數的支援

變數是值的預留位置，您可以使用它們來建立範本化的查詢，以及儀表板或面板連結。有關變數的更多資訊，請參閱[範本和變數](https://grafana.com/docs/grafana/latest/dashboards/variables)。

在本指南中，您將了解如何將這樣的查詢字串：

```sql
SELECT * FROM services WHERE id = "$service"
```

轉換為

```sql
SELECT * FROM services WHERE id = "auth-api"
```

Grafana 提供了幾個輔助函式來在字串範本中內插變數。讓我們看看如何在您的插件中使用它們。

## 將變數新增至插件

### 在面板插件中內插變數

對於面板，`replaceVariables` 函式可在 `PanelProps` 中使用。

將 `replaceVariables` 新增至引數清單，並將使用者定義的範本字串傳遞給它：

```tsx
export function SimplePanel({ options, data, width, height, replaceVariables }: Props) {
  const query = replaceVariables('Now displaying $service');

  return <div>{query}</div>;
}
```

### 在資料來源插件中內插變數

對於資料來源，您需要使用 `getTemplateSrv`，它會傳回一個 `TemplateSrv` 的執行個體。

1. 從 `runtime` 套件匯入 `getTemplateSrv`：

   ```ts
   import { getTemplateSrv } from '@grafana/runtime';
   ```

2. 在您的 `query` 方法中，使用使用者定義的範本字串呼叫 `replace` 方法：

   ```ts
   async query(options: DataQueryRequest<MyQuery>): Promise<DataQueryResponse> {
     const query = getTemplateSrv().replace('SELECT * FROM services WHERE id = "$service"', options.scopedVars);

     const data = makeDbQuery(query);

     return { data };
   }
   ```

### 從您的插件設定變數

您不僅可以讀取變數的值，還可以從您的插件更新變數。請使用 `locationService.partial(query, replace)`。

以下範例顯示如何更新一個名為 `service` 的變數。

- `query` 包含您要更新的查詢參數。控制變數的查詢參數以 `var-` 為前綴。
- `replace: true` 告訴 Grafana 更新目前的 URL 狀態，而不是建立新的歷史記錄項目。

```ts
import { locationService } from '@grafana/runtime';
```

```ts
locationService.partial({ 'var-service': 'billing' }, true);
```

:::caution

每當您更新變數時，Grafana 都會查詢您的資料來源。過度更新變數可能會減慢 Grafana 的速度，並導致糟糕的使用者體驗。

:::

### 為您的資料來源新增對查詢變數的支援

[查詢變數](https://grafana.com/docs/grafana/latest/dashboards/variables/add-template-variables#add-a-query-variable)是一種變數類型，可讓您查詢資料來源以取得值。透過為您的資料來源插件新增對查詢變數的支援，使用者可以根據您的資料來源中的資料建立動態儀表板。

讓我們從為變數查詢定義一個查詢模型開始：

```ts
export interface MyVariableQuery {
  namespace: string;
  rawQuery: string;
}
```

為了讓資料來源支援查詢變數，請在您的 `DataSourceApi` 類別中覆寫 `metricFindQuery`。`metricFindQuery` 函式會傳回一個 `MetricFindValue` 陣列，該陣列具有單一屬性 `text`：

```ts
async metricFindQuery(query: MyVariableQuery, options?: any) {
  // 根據查詢擷取 DataQueryResponse。
  const response = await this.fetchMetricNames(query.namespace, query.rawQuery);

  // 將查詢結果轉換為 MetricFindValue[]
  const values = response.data.map(frame => ({ text: frame.name }));

  return values;
}
```

:::note

預設情況下，Grafana 為簡單的文字查詢提供了一個基本的查詢模型和編輯器。如果這就是您所需要的，那麼請將查詢類型保留為 `string`：

:::

```ts
async metricFindQuery(query: string, options?: any)
```

讓我們建立一個自訂查詢編輯器，讓使用者可以編輯查詢模型。

1. 建立一個 `VariableQueryEditor` 元件：

   ```tsx title="src/VariableQueryEditor.tsx"
   import React, { useState } from 'react';
   import { MyVariableQuery } from './types';

   interface VariableQueryProps {
     query: MyVariableQuery;
     onChange: (query: MyVariableQuery, definition: string) => void;
   }

   export const VariableQueryEditor = ({ onChange, query }: VariableQueryProps) => {
     const [state, setState] = useState(query);

     const saveQuery = () => {
       onChange(state, `${state.query} (${state.namespace})`);
     };

     const handleChange = (event: React.FormEvent<HTMLInputElement>) =>
       setState({
         ...state,
         [event.currentTarget.name]: event.currentTarget.value,
       });

     return (
       <>
         <div className="gf-form">
           <span className="gf-form-label width-10">Namespace</span>
           <input
             name="namespace"
             className="gf-form-input"
             onBlur={saveQuery}
             onChange={handleChange}
             value={state.namespace}
           />
         </div>
         <div className="gf-form">
           <span className="gf-form-label width-10">Query</span>
           <input
             name="rawQuery"
             className="gf-form-input"
             onBlur={saveQuery}
             onChange={handleChange}
             value={state.rawQuery}
           />
         </div>
       </>
     );
   };
   ```

   每當其中一個文字欄位失去焦點 (`onBlur`) 時，Grafana 就會儲存查詢模型，然後預覽 `metricFindQuery` 傳回的值。

   `onChange` 的第二個引數可讓您設定查詢的文字表示，該表示將出現在變數清單中變數名稱的旁邊。

2. 設定您的插件以使用查詢編輯器：

   ```ts
   import { VariableQueryEditor } from './VariableQueryEditor';

   export const plugin = new DataSourcePlugin<DataSource, MyQuery, MyDataSourceOptions>(DataSource)
     .setQueryEditor(QueryEditor)
     .setVariableQueryEditor(VariableQueryEditor);
   ```

就是這樣！您現在可以透過將[查詢變數](https://grafana.com/docs/grafana/latest/dashboards/variables/add-template-variables#add-a-query-variable)新增至您的儀表板來試用該插件。

## 使用範本變數

[範本變數](https://grafana.com/docs/grafana/latest/dashboards/variables/#templates)可讓使用者建立可根據其輸入動態變更的儀表板。由於變數在 Grafana 中已經存在很長一段時間，因此許多使用者期望他們安裝的任何資料來源都支援它們。

### 內插範本變數

若要內插範本變數，您需要從 `@grafana/runtime` 套件匯入 `getTemplateSrv()` 函式：

```ts
import { getTemplateSrv } from '@grafana/runtime';
```

`getTemplateSrv()` 函式會傳回一個 `TemplateSrv` 的執行個體，它提供了處理範本變數的方法。最重要的一個是 `replace()`，它接受一個包含變數的字串作為輸入，並傳回一個內插的字串，其中變數已被使用者選取的值取代。

例如，如果您有一個名為 `instance` 的變數，以下程式碼會將該變數取代為其對應的值：

```ts
getTemplateSrv().replace("I'd like $instance, please!");

// I'd like server-1, please!
```

`replace()` 甚至可以處理內建變數，例如 `$__from` 和 `$__to`。

就這樣！對於大多數使用案例，這就是您在資料來源中新增對範本變數支援所需做的全部工作。請注意，由您決定哪些欄位將支援範本變數。例如，若要在您的查詢中內插單一屬性 `rawQuery`，請新增以下內容：

```
const interpolatedQuery: MyQuery = {
  ...query,
  rawQuery: getTemplateSrv().replace(query.rawQuery),
};
```

### 格式化多值變數

在前面的範例中，變數只有一個值 `server-1`。但是，如果使用者改為建立一個多值變數，它就可以同時擁有多個值。多值變數帶來了一個新的挑戰：您如何決定如何格式化值的集合？

例如，這些不同的格式中哪一種適合您的使用案例？

```ts
{server-1, server-2, server-3} (Graphite)
["server-1", "server-2", "server-3"] (JSON)
("server-1" OR "server-2" OR "server-3") (Lucene)
```

幸運的是，`replace()` 方法可讓您傳遞第三個引數，讓您可以從一組預定義的格式中進行選擇，例如 CSV 格式：

```ts
getTemplateSrv().replace("I'd like $instance, please!", {}, 'csv');

// I'd like server-1, server-2, server-3, please!
```

:::note

`replace()` 方法的第二個引數可讓您設定自訂變數集或範圍變數，以在內插字串時包含。除非您對此感興趣，否則請隨意傳遞一個空物件 `{}`。

:::

Grafana 支援一系列格式選項。若要瀏覽可用的格式，請查看[進階變數格式選項](https://grafana.com/docs/grafana/latest/dashboards/variables/variable-syntax/#advanced-variable-format-options)。

### 使用內插函式格式化變數

在檢閱了進階變數格式選項後，您可能會發現您想支援一個不可用的格式選項。幸運的是，Grafana 透過使用內插函式，讓您完全控制 `replace()` 如何格式化變數。

您可以將內插函式傳遞給 `replace()`，而不是字串作為第三個引數。以下範例使用自訂格式化程式函式在最後一個元素之前新增一個 `and`：

```ts
const formatter = (value: string | string[]): string => {
  if (typeof value == 'string') {
    return value;
  }

  // 在最後一個元素之前新增 'and'。
  if (value.length > 1) {
    return value.slice(0, -1).join(', ') + ' and ' + value[value.length - 1];
  }

  return value[0];
};

getTemplateSrv().replace("I'd like $instance, please!", {}, formatter);

// I'd like server-1, server-2, and server-3, please!
```

函式的引數可以是字串或字串陣列，例如 `(string | string[])`，取決於變數是否支援多個值，因此在使用它之前請務必檢查值的類型。

## 在範本之外使用變數

在某些情況下，您可能希望在範本之外使用變數。例如，如果您想驗證所選值的數量或將它們新增到下拉式選單中。

此輔助函式使用 `replace()` 方法將值作為陣列傳回：

```ts
function getValuesForVariable(name: string): string[] {
  const values: string[] = [];

  // 將值收集到陣列中。
  getTemplateSrv().replace(`$${name}`, {}, (value: string | string[]) => {
    if (Array.isArray(value)) {
      values.push(...value);
    } else {
      values.push(value);
    }

    // 我們在這裡並不真正關心字串。
    return '';
  });

  return values;
}
const instances = getValuesForVariable('instance');

for (var instance of instances) {
  console.log(instance);
}

// server-1
// server-2
// server-3
```

您甚至可以更進一步，建立一個整齊地包含所有變數及其值的物件：

```ts
function getAllVariables(): Record<string, string[]> {
  const entries = getTemplateSrv()
    .getVariables()
    .map((v) => [v.name, getValuesForVariable(v.name)]);

  return Object.fromEntries(entries);
}
const vars = getAllVariables();

console.log(vars.instance);

// ["server-1", "server-2", "server-3"]
```

在此範例中，使用 `getTemplateSrv().getVariables()` 列出目前儀表板的所有已設定變數。

:::note

您也可以根據可預測的分隔符號分割內插的字串。請隨意根據對您有意義的方式調整這些程式碼片段。

:::
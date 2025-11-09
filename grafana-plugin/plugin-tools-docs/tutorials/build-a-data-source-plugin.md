---
id: build-a-data-source-plugin
title: 建立資料來源外掛程式
sidebar_position: 1
description: 了解如何建立外掛程式以新增對您自己的資料來源的支援。
keywords:
  - grafana
  - plugins
  - plugin
  - data source
  - datasource
---

import CreatePlugin from '@shared/create-plugin-frontend.md';
import PluginAnatomy from '@shared/plugin-anatomy.md';

## 簡介

Grafana 支援多種[資料來源](https://grafana.com/grafana/plugins/data-source-plugins/)，包括 Prometheus、MySQL 和 Datadog。然而，在某些情況下，您可能已經擁有一個內部指標解決方案，並希望將其新增至您的 Grafana 儀表板。本教學將教您如何建立一個新的資料來源外掛程式來查詢資料。

在本教學中，您將：

- 建立一個資料來源以視覺化正弦波
- 使用查詢編輯器建構查詢
- 使用組態編輯器設定您的資料來源

### 先決條件

- Grafana v10.0 或更新版本
- [LTS](https://nodejs.dev/en/about/releases/) 版本的 Node.js

## 建立新外掛程式

<CreatePlugin pluginType="datasource" />

若要了解如何建立後端資料來源外掛程式，請參閱[建立資料來源外掛程式後端元件](./build-a-data-source-backend-plugin.md)

## 外掛程式剖析

<PluginAnatomy />

## 資料來源外掛程式

Grafana 中的資料來源必須擴充 `DataSourceApi` 介面，這需要您定義兩個方法：`query` 和 `testDatasource`。

### `query` 方法

`query` 方法是任何資料來源外掛程式的核心。它接受使用者的查詢、從外部資料庫擷取資料，並以 Grafana 可辨識的格式傳回資料。

```ts
async query(options: DataQueryRequest<MyQuery>): Promise<DataQueryResponse>
```

`options` 物件包含使用者提出的查詢或 _targets_，以及內容資訊，例如目前的時間間隔。使用此資訊查詢外部資料庫。

### 測試您的資料來源

`testDatasource` 為您的資料來源實作健康狀態檢查。例如，每當使用者在變更連線設定後按一下 **Save & Test** 按鈕時，Grafana 都會呼叫此方法。

```ts
async testDatasource()
```

## 傳回資料框架

有數不清種不同的資料庫，每種資料庫都有自己的查詢資料方式。為了能夠支援所有不同的資料格式，Grafana 將資料合併為一個稱為[資料框架](../key-concepts/data-frames)的統一資料結構。

讓我們看看如何從 `query` 方法建立並傳回資料框架。在此步驟中，您將變更入門外掛程式中的程式碼以傳回[正弦波](https://en.wikipedia.org/wiki/Sine_wave)。

1. 在目前的 `query` 方法中，移除 `map` 函式內的程式碼。

   `query` 方法現在看起來像這樣：

   ```ts title="src/datasource.ts"
   async query(options: DataQueryRequest<MyQuery>): Promise<DataQueryResponse> {
     const { range } = options;
     const from = range!.from.valueOf();
     const to = range!.to.valueOf();

     const data = options.targets.map(target => {
       // 您的程式碼放在這裡。
     });

     return { data };
   }
   ```

2. 在 `map` 函式中，使用 `lodash/defaults` 套件為尚未設定的查詢屬性設定預設值：

   ```ts title="src/datasource.ts"
   import defaults from 'lodash/defaults';

   const query = defaults(target, defaultQuery);
   ```

3. 在 datasource.ts 的頂端建立一個預設查詢：

   ```ts title="src/datasource.ts"
   export const defaultQuery: Partial<MyQuery> = {
     constant: 6.5,
   };
   ```

4. 建立一個包含時間欄位和數字欄位的資料框架：

   ```ts title="src/datasource.ts"
   const frame = createDataFrame({
     refId: query.refId,
     fields: [
       { name: 'time', type: FieldType.time },
       { name: 'value', type: FieldType.number },
     ],
   });
   ```

   需要設定 `refId` 以告知 Grafana 是哪個查詢產生了此日期框架。

接下來，我們將實際值新增至資料框架。不用擔心用於計算值的數學運算。

1. 建立幾個輔助變數：

   ```ts title="src/datasource.ts"
   // 時間範圍的持續時間，以毫秒為單位。
   const duration = to - from;

   // step 決定點在時間上彼此的接近程度 (ms)。
   const step = duration / 1000;
   ```

2. 將值新增至資料框架：

   ```ts title="src/datasource.ts"
   for (let t = 0; t < duration; t += step) {
     frame.add({ time: from + t, value: Math.sin((2 * Math.PI * t) / duration) });
   }
   ```

   `frame.add()` 接受一個物件，其中鍵對應於資料框架中每個欄位的名稱。

3. 傳回資料框架：

   ```ts title="src/datasource.ts"
   return frame;
   ```

4. 透過建立新的資料來源執行個體並建立儀表板來試用它。

您的資料來源現在正在傳送 Grafana 可以視覺化的資料框架。接下來，我們將探討如何透過定義 _query_ 來控制正弦波的頻率。

:::info
在此範例中，我們正在從目前的時間範圍產生時間戳記。這表示無論您使用哪個時間範圍，您都會得到相同的圖形。在實務中，您會改用資料庫傳回的時間戳記。
:::

## 定義查詢

大多數資料來源都提供查詢特定資料的方法。MySQL 和 PostgreSQL 使用 SQL，而 Prometheus 有自己的查詢語言，稱為 _PromQL_。無論您的資料庫使用哪種查詢語言，Grafana 都可讓您建立對其的支援。

透過實作您自己的 _query editor_，一個 React 元件，讓使用者能夠透過使用者友善的圖形介面建立自己的查詢，為您的資料來源新增自訂查詢的支援。

查詢編輯器可以像使用者編輯原始查詢文字的文字欄位一樣簡單，也可以提供更使用者友善的表單，其中包含下拉式選單和開關，稍後在傳送至資料庫之前會轉換為原始查詢文字。

### 定義查詢模型

設計查詢編輯器的第一步是定義其 _query model_。查詢模型定義了資料來源的使用者輸入。

我們希望能夠控制正弦波的頻率，所以讓我們新增另一個屬性。

1. 將一個名為 `frequency` 的新數字屬性新增至查詢模型：

   ```ts title="src/types.ts"
   export interface MyQuery extends DataQuery {
     queryText?: string;
     constant: number;
     frequency: number;
   }
   ```

2. 為新的 `frequency` 屬性設定預設值：

   ```ts title="src/types.ts"
   export const defaultQuery: Partial<MyQuery> = {
     constant: 6.5,
     frequency: 1.0,
   };
   ```

### 將模型繫結至表單

現在您已經定義了您希望支援的查詢模型，下一步是將模型繫結至表單。`FormField` 是 `grafana/ui` 中的一個文字欄位元件，可讓您註冊一個偵聽器，每當表單欄位值變更時都會叫用該偵聽器。

1. 從 `query` 物件定義 `frequency`，並將新的表單欄位新增至查詢編輯器，以控制 `render` 方法中的新頻率屬性。

   ```tsx title="src/components/QueryEditor.tsx"
   const { queryText, constant, frequency } = query;

   <InlineField label="Frequency" labelWidth={16}>
     <Input onChange={onFrequencyChange} value={frequency || ''} />
   </InlineField>;
   ```

2. 為新屬性新增事件偵聽器。

   ```tsx title="src/components/QueryEditor.tsx"
   const onFrequencyChange = (event: ChangeEvent<HTMLInputElement>) => {
     onChange({ ...query, frequency: parseFloat(event.target.value) });
     // executes the query
     onRunQuery();
   };
   ```

   註冊的偵聽器 `onFrequencyChange` 會呼叫 `onChange`，以使用表單欄位中的值更新目前的查詢。

   `onRunQuery();` 告知 Grafana 在每次變更後執行查詢。對於快速查詢，建議這樣做以提供更具回應性的體驗。

### 使用屬性

新的查詢模型現在可以在我們的 `query` 方法中使用了。

1. 在 `query` 方法中，使用 `frequency` 屬性來調整我們的方程式。

   ```ts title="src/datasource.ts"
   frame.add({ time: from + t, value: Math.sin((2 * Math.PI * query.frequency * t) / duration) });
   ```

2. 透過變更面板查詢中的頻率來試用它。

## 為您的資料來源啟用組態

若要存取特定的資料來源，您通常需要設定主機名稱、認證或驗證方法等項目。_config editor_ 可讓您的使用者設定您的資料來源外掛程式以符合其需求。

組態編輯器看起來與查詢編輯器類似，因為它定義了一個模型並將其繫結至表單。

由於我們實際上並未在正弦波範例中連線到外部資料庫，因此我們並不需要太多選項。然而，為了向您展示如何新增選項，我們將新增 _wave resolution_ 作為選項。

解析度控制資料點在時間上彼此的接近程度。較高的解析度表示更多點更接近，但代價是處理更多資料。

### 定義選項模型

1. 將一個名為 `resolution` 的新數字屬性新增至選項模型。

   ```ts title="src/types.ts"
   export interface MyDataSourceOptions extends DataSourceJsonData {
     path?: string;
     resolution?: number;
   }
   ```

### 將模型繫結至表單

就像查詢編輯器一樣，組態編輯器中的表單欄位會在值變更時呼叫註冊的偵聽器。

1. 將一個新的表單欄位新增至查詢編輯器，以控制新的解析度選項。

   ```tsx title="src/components/ConfigEditor.tsx"
   <InlineField label="Resolution" labelWidth={12}>
     <Input onChange={onResolutionChange} value={jsonData.resolution || ''} placeholder="輸入數字" width={40} />
   </InlineField>
   ```

2. 為新選項新增事件偵聽器。

   ```ts title="src/components/ConfigEditor.tsx"
   const onResolutionChange = (event: ChangeEvent<HTMLInputElement>) => {
     const jsonData = {
       ...options.jsonData,
       resolution: parseFloat(event.target.value),
     };
     onOptionsChange({ ...options, jsonData });
   };
   ```

   `onResolutionChange` 偵聽器會呼叫 `onOptionsChange`，以使用表單欄位中的值更新目前的選項。

### 使用選項

1. 在 `DataSource` 類別中建立一個名為 `resolution` 的屬性。

   ```ts title="src/datasource.ts"
   export class DataSource extends DataSourceApi<MyQuery, MyDataSourceOptions> {
     resolution: number;

     constructor(instanceSettings: DataSourceInstanceSettings<MyDataSourceOptions>) {
       super(instanceSettings);

       this.resolution = instanceSettings.jsonData.resolution || 1000.0;
     }

     // ...
   ```

2. 在 `query` 方法中，使用 `resolution` 屬性來變更我們計算步階大小的方式。

   ```ts title="src/datasource.ts"
   const step = duration / this.resolution;
   ```

3. 透過設定新的資料來源並變更解析度值來試用它。

## 摘要

在本教學中，您建立了一個完整的 Grafana 資料來源外掛程式，它使用查詢編輯器來控制要視覺化的資料。您已新增一個資料來源選項，通常用於設定連線選項等。

## 了解更多

### 從外部 API 取得資料

Grafana 中的大多數資料來源都會從外部 API 傳回資料。本教學嘗試保持簡單，不需要額外的服務。

此範例展示了如何使用 [`grafana-runtime` 套件](https://github.com/grafana/grafana/tree/main/packages/grafana-runtime)中的 [`getBackendSrv` 函式](https://github.com/grafana/grafana/blob/main/packages/grafana-runtime/src/services/backendSrv.ts)。

雖然您可以使用 [axios](https://github.com/axios/axios) 或 [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) 之類的工具發出請求，但我們建議使用 `getBackendSrv`，因為它會透過 Grafana 伺服器代理請求，而不是從瀏覽器發出請求。在對外部 API 進行已驗證的請求時，我們強烈建議這樣做。如需有關驗證外部請求的更多資訊，請參閱[為資料來源外掛程式新增驗證](../how-to-guides/data-source-plugins/add-authentication-for-data-source-plugins)。

### 改善外掛程式品質

若要了解更多有關進階外掛程式開發主題的資訊，請參閱下列內容：

- [新增對變數的支援](../how-to-guides/data-source-plugins/add-support-for-variables)
- [新增對註釋的支援](../how-to-guides/data-source-plugins/add-support-for-annotation-queries)
- [新增對 Explore 查詢的支援](../how-to-guides/data-source-plugins/add-features-for-explore-queries)
- [建立日誌資料來源](./build-a-logs-data-source-plugin.md)
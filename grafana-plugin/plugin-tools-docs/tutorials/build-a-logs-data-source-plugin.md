---
id: build-a-logs-data-source-plugin
title: 建立日誌資料來源外掛程式
sidebar_position: 20
description: 了解如何建立日誌資料來源外掛程式。
keywords:
  - grafana
  - plugins
  - plugin
  - logs
  - logs data source
  - datasource
---

# 建立日誌資料來源外掛程式

Grafana 資料來源外掛程式支援指標、日誌和其他資料類型。建立日誌資料來源外掛程式的步驟與指標資料來源大致相同，但有一些差異，我們將在本指南中說明。

## 開始之前

本指南假設您已熟悉如何[為指標建立資料來源外掛程式](./build-a-data-source-plugin.md)。我們建議您在繼續之前先複習此資料。

## 將日誌支援新增至您的資料來源

若要將日誌支援新增至現有的資料來源，您需要：

1. 啟用日誌支援
2. 建構日誌資料框架

完成這些步驟後，您可以使用一或多個[選用功能](#enhance-your-logs-data-source-plugin-with-optional-features)來改善使用者體驗。

### 步驟 1：啟用日誌支援

透過將 `"logs": true` 新增至 [plugin.json](../reference/metadata.md) 檔案，告知 Grafana 您的資料來源外掛程式可以傳回日誌資料。

```json title="src/plugin.json"
{
  "logs": true
}
```

### 步驟 2：建構日誌資料框架

### 日誌資料框架格式

日誌資料框架應包含下列欄位：

| 欄位名稱 (Field name) | 欄位類型 (Field type)                                 | 必要欄位 (Required field) | 說明 (Description)                                                                                                                                                                                                                            |
| --------------------- | ------------------------------------------------------- | ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **timestamp**         | `time`                                                  | 必要                      | 時間戳記，不可為空。                                                                                                                                                                                                                          |
| **body**              | `string`                                                | 必要                      | 日誌行的內容，不可為空。                                                                                                                                                                                                                      |
| **severity**          | `string`                                                | 選用                      | 日誌行的嚴重性/層級。如果找不到嚴重性欄位，取用者/用戶端將決定日誌層級。如需有關日誌層級的更多資訊，請參閱[日誌整合](https://grafana.com/docs/grafana/latest/explore/logs-integration/)。 |
| **id**                | `string`                                                | 選用                      | 日誌行的唯一識別碼。                                                                                                                                                                                                                          |
| **labels**            | `json raw message` (Go) 或 `other` (TypeScript)         | 選用                      | 日誌行的其他標籤。其他系統可能會以不同的名稱參照此項目，例如「屬性」。在 JavaScript 中將其值表示為 `Record<string,any>` 類型。                                                              |

日誌資料框架的 `type` 需要在資料框架的元資料中設定為 `type: DataFrameType.LogLines`。

**在 Go 中建構日誌資料框架的範例：**

```go
frame := data.NewFrame(
   "logs",
  data.NewField("timestamp", nil, []time.Time{time.UnixMilli(1645030244810), time.UnixMilli(1645030247027), time.UnixMilli(1645030247027)}),
  data.NewField("body", nil, []string{"message one", "message two", "message three"}),
  data.NewField("severity", nil, []string{"critical", "error", "warning"}),
  data.NewField("id", nil, []string{"xxx-001", "xyz-002", "111-003"}),
  data.NewField("labels", nil, []json.RawMessage{[]byte(`{}`), []byte(`{"hello":"world"}`), []byte(`{"hello":"world", "foo": 123.45, "bar" :["yellow","red"], "baz" : { "name": "alice" }}`)}),
)

frame.SetMeta(&data.FrameMeta{
	Type:   data.FrameTypeLogLines,
})
```

**在 TypeScript 中建構日誌資料框架的範例：**

```ts
import { createDataFrame, DataFrameType, FieldType } from '@grafana/data';

const result = createDataFrame({
  fields: [
    { name: 'timestamp', type: FieldType.time, values: [1645030244810, 1645030247027, 1645030247027] },
    { name: 'body', type: FieldType.string, values: ['message one', 'message two', 'message three'] },
    { name: 'severity', type: FieldType.string, values: ['critical', 'error', 'warning'] },
    { name: 'id', type: FieldType.string, values: ['xxx-001', 'xyz-002', '111-003'] },
    {
      name: 'labels',
      type: FieldType.other,
      values: [{}, { hello: 'world' }, { hello: 'world', foo: 123.45, bar: ['yellow', 'red'], baz: { name: 'alice' } }],
    },
  ],
  meta: {
    type: DataFrameType.LogLines,
  },
});
```

## 使用選用功能增強您的日誌資料來源外掛程式

您可以使用下列選用功能來增強您在「探索」和「日誌」面板中的日誌資料來源外掛程式。

[探索](https://grafana.com/docs/grafana/latest/explore/) 提供了一個有用的介面，用於調查事件和疑難排解日誌。如果資料來源產生了日誌結果，我們強烈建議您實作下列 API，讓您的使用者能充分利用「日誌」UI 及其在「探索」中的功能。

下列步驟說明透過無縫整合在資料來源外掛程式中新增對「探索」功能的支援的程序。實作這些 API 以增強使用者體驗，並利用「探索」強大的日誌調查功能。

### 在「探索」的「日誌」檢視中顯示日誌結果

為確保您的日誌結果顯示在互動式的「日誌」檢視中，您必須在日誌結果資料框架中將 `meta` 屬性新增至 `preferredVisualisationType`。

**Go 中的範例：**

```go
frame.Meta = &data.FrameMeta{
	PreferredVisualization: "logs",
}
```

**TypeScript 中的範例：**

```ts
import { createDataFrame } from '@grafana/data';

const result = createDataFrame({
    fields: [...],
    meta: {
        preferredVisualisationType: 'logs',
    },
});
```

### 反白顯示搜尋的字詞

:::note

此功能必須在資料框架中實作為元屬性。

:::

日誌視覺化可以在日誌項目中[反白顯示特定字詞或字串](https://grafana.com/docs/grafana/latest/explore/logs-integration/#highlight-searched-words)。此功能通常用於反白顯示搜尋字詞，讓使用者更容易在日誌中找到並專注於相關資訊。為了讓反白顯示正常運作，您必須在資料框架的 `meta` 資訊中包含搜尋字詞。

**Go 中的範例：**

```go
frame.Meta = &data.FrameMeta{
	Custom: map[string]interface{}{
    "searchWords": []string{"foo", "bar", "baz"} ,
  }
}
```

**TypeScript 中的範例：**

```ts
import { createDataFrame } from '@grafana/data';

const result = createDataFrame({
    fields: [...],
    meta: {
      custom: {
        searchWords: ["foo", "bar", "baz"],
      }
    },
});
```

### 日誌結果 `meta` 資訊

:::note

此功能必須在資料框架中實作為元屬性，或在資料框架中實作為欄位。

:::

[日誌結果元資訊](https://grafana.com/docs/grafana/latest/explore/logs-integration/#log-result-meta-information)可用於向使用者傳達有關日誌結果的資訊。可以與使用者共用下列資訊：

- **收到的日誌計數與限制** - 顯示收到的日誌計數與指定限制的比較。資料框架應為要求的日誌行數設定一個帶有元屬性的限制。
- **錯誤**：顯示日誌結果中可能的錯誤。資料框架應在 `meta` 屬性中具有 `error`。
- **通用標籤**：顯示 `labels` 資料框架欄位中存在的標籤，這些標籤對於所有顯示的日誌行都是相同的。對於產生具有標籤欄位的日誌資料框架的資料來源，支援此功能。如需更多資訊，請參閱[日誌資料框架格式](#logs-data-frame-format)。

**Go 中的範例：**

```go
frame.Meta = &data.FrameMeta{
	Custom: map[string]interface{}{
    "limit": 1000,
    "error": "Error information",
  }
}
```

**TypeScript 中的範例：**

```ts
import { createDataFrame } from '@grafana/data';

const result = createDataFrame({
    fields: [...],
    meta: {
      custom: {
        limit: 1000,
        error: "Error information"
      }
    },
});
```

### 使用具有 URL 的資料連結將日誌連結至追蹤

如果您的日誌資料包含 **trace ID**，您可以透過新增具有 _trace ID 值_和 _URL 資料連結_ 的欄位來增強您的日誌資料框架。這些連結應使用 trace ID 值來準確地連結至適當的追蹤。此增強功能讓使用者能夠無縫地從日誌行移至相關的追蹤。

**TypeScript 中的範例：**

```ts
import { createDataFrame, FieldType } from '@grafana/data';

const result = createDataFrame({
  fields: [
    ...,
    { name: 'traceID',
      type: FieldType.string,
      values: ['a006649127e371903a2de979', 'e206649127z371903c3be12q' 'k777549127c371903a2lw34'],
      config: {
        links: [
          {
            // 請務必根據您的資料來源邏輯調整此範例。
            title: 'Trace view',
            url: `http://linkToTraceID/${__value.raw}` // ${__value.raw} 是一個變數，將會被實際的 traceID 值取代。
          }
        ]
      }
    }
  ],
  ...,
});
```

### 以顏色標示的日誌層級

:::note

此功能必須在資料框架中實作為欄位。

:::

以顏色標示的[日誌層級](https://grafana.com/docs/grafana/latest/explore/logs-integration/#log-level)會顯示在每個日誌行的開頭。它們讓使用者可以快速評估日誌項目的嚴重性，並有助於日誌分析和疑難排解。日誌層級是從資料框架的 `severity` 欄位決定的。如果 `severity` 欄位不存在，Grafana 會嘗試根據日誌行的內容評估層級。如果無法從內容推斷日誌層級，則日誌層級會設定為 `unknown`。

如需更多資訊，請參閱[日誌資料框架格式](#logs-data-frame-format)。

### 複製日誌行連結

:::note

此功能必須在資料框架中實作為欄位。

:::

[複製日誌行連結](https://grafana.com/docs/grafana/latest/explore/logs-integration/#copy-link-to-log-line)是一項功能，可讓您產生特定日誌行的連結，以便輕鬆共用和參照。Grafana 在產生具有 `id` 欄位的日誌資料框架的資料來源中支援此功能。

如果基礎資料庫未傳回 `id` 欄位，您可以在資料來源中實作一個。例如，在 Loki 資料來源中，會使用奈秒時間戳記、標籤和日誌行內容的組合來建立唯一的 `id`。另一方面，Elasticsearch 會傳回一個對於指定索引而言唯一的 `_id` 欄位。在這種情況下，為確保唯一性，會同時使用 `index name` 和 `_id` 來建立唯一的 `id`。

如需更多資訊，請參閱[日誌資料框架格式](#logs-data-frame-format)。

### 使用日誌詳細資料篩選欄位

:::note

透過資料來源方法實作此功能。

:::

每個日誌行都有一個可展開的部分，稱為「日誌詳細資料」，您可以按一下該行來開啟。在「日誌詳細資料」中，Grafana 會顯示與該日誌項目相關聯的[欄位](https://grafana.com/docs/grafana/latest/explore/logs-integration/#fields)。如果資料來源實作了 `modifyQuery?(query: TQuery, action: QueryFixAction): TQuery;` API，則每個欄位都可以使用篩選功能。對於日誌，目前有兩種篩選選項可用：

- `ADD_FILTER` - 用於篩選包含所選欄位的日誌行。
- `ADD_FILTER_OUT` - 用於篩選不包含所選欄位的日誌行。

```ts
export class ExampleDatasource extends DataSourceApi<ExampleQuery, ExampleOptions> {
  modifyQuery(query: ExampleQuery, action: QueryFixAction): ExampleQuery {
    let queryText = query.query ?? '';
    switch (action.type) {
      case 'ADD_FILTER':
        if (action.options?.key && action.options?.value) {
          // 請務必根據您的資料來源邏輯調整此範例程式碼。
          queryText = addLabelToQuery(queryText, action.options.key, '=', action.options.value);
        }
        break;
      case 'ADD_FILTER_OUT':
        {
          if (action.options?.key && action.options?.value) {
            // 請務必根據您的資料來源邏輯調整此範例程式碼。
            queryText = addLabelToQuery(queryText, action.options.key, '!=', action.options.value);
          }
        }
        break;
    }
    return { ...query, query: queryText };
  }
}
```

### 即時追蹤

:::note

在 `plugin.json` 中實作此功能資料來源方法並啟用

:::

[即時追蹤](https://grafana.com/docs/grafana/latest/explore/logs-integration/#live-tailing)是一項功能，可使用「探索」啟用即時日誌結果串流。若要為您的資料來源啟用即時追蹤，請依照下列步驟操作：

1. **在 `plugin.json` 中啟用串流**：在您的資料來源外掛程式的 `plugin.json` 檔案中，將 `streaming` 屬性設定為 `true`。這可讓「探索」辨識並為您的資料來源啟用即時追蹤控制項。

```json
{
  "type": "datasource",
  "name": "Example",
  "id": "example",
  "logs": true,
  "streaming": true
}
```

2. 確保您的資料來源的 `query` 方法可以處理 `liveStreaming` 設定為 true 的查詢。

```ts
export class ExampleDatasource extends DataSourceApi<ExampleQuery, ExampleOptions> {
  query(request: DataQueryRequest<ExampleQuery>): Observable<DataQueryResponse> {
    // 這是一個模擬實作。請務必根據您的資料來源邏輯進行調整。
    if (request.liveStreaming) {
      return this.runLiveStreamQuery(request);
    }
    return this.runRegularQuery(request);
  }
}
```

### 日誌內容

:::note

透過 `DataSourceWithLogsContextSupport` 介面實作此功能。

:::

[日誌內容](https://grafana.com/docs/grafana/latest/explore/logs-integration/#log-context)是「探索」中的一項功能，可顯示符合特定搜尋查詢的日誌項目周圍的其他內容行。此功能讓使用者可以透過在其相關內容中檢視日誌項目，更深入地了解日誌資料。由於 Grafana 將顯示周圍的日誌行，使用者可以更清楚地了解事件的順序以及日誌項目發生的背景，從而改善日誌分析和疑難排解。

```ts
import {
  DataQueryRequest,
  DataQueryResponse,
  DataSourceWithLogsContextSupport,
  LogRowContextOptions,
  LogRowContextQueryDirection,
  LogRowModel,
} from '@grafana/data';
import { catchError, lastValueFrom, of, switchMap, Observable } from 'rxjs';

export class ExampleDatasource
  extends DataSourceApi<ExampleQuery, ExampleOptions>
  implements DataSourceWithLogsContextSupport<ExampleQuery>
{
  // 擷取指定日誌列的內容
  async getLogRowContext(
    row: LogRowModel,
    options?: LogRowContextOptions,
    query?: ExampleQuery
  ): Promise<DataQueryResponse> {
    // 請務必根據您的資料來源邏輯調整此 createRequestFromQuery 的範例實作。
    // 在傳回您的 `request` 物件之前，請記得使用 `getTemplateSrv` 和傳遞的 `options.scopedVars` 取代變數。
    const request = createRequestFromQuery(row, query, options);
    return lastValueFrom(
      // 請務必根據您的資料來源邏輯調整此 this.query 的範例。
      this.query(request).pipe(
        catchError((err) => {
          const error: DataQueryError = {
            message: '內容查詢期間發生錯誤。請檢查 JS 主控台日誌。',
            status: err.status,
            statusText: err.statusText,
          };
          throw error;
        }),
        // 請務必根據您的資料來源邏輯調整此 processResultsToDataQueryResponse 的範例。
        switchMap((res) => of(processResultsToDataQueryResponse(res)))
      )
    );
  }

  // 擷取指定日誌列的內容查詢物件。目前用於在分割檢視中開啟 LogContext 查詢。
  getLogRowContextQuery(
    row: LogRowModel,
    options?: LogRowContextOptions,
    query?: ExampleQuery
  ): Promise<ExampleQuery | null> {
    // 資料來源內部實作，根據列、選項和原始查詢建立內容查詢
  }
}
```

## 開發中的 API

這些 API 可用於 [`grafana/grafana`](https://github.com/grafana/grafana) 儲存庫中的資料來源。外部外掛程式開發人員不支援這些 API。

### 顯示全範圍日誌量

:::note

它透過實作 `DataSourceWithXXXSupport` 介面在資料來源中實作。

:::

透過[全範圍日誌量](https://grafana.com/docs/grafana/latest/explore/logs-integration/#logs-volume)，「探索」會顯示一個圖表，其中顯示所有輸入的日誌查詢的日誌分佈。若要將全範圍日誌量支援新增至資料來源外掛程式，請使用 `DataSourceWithSupplementaryQueriesSupport` API。

**如何在資料來源中實作 `DataSourceWithSupplementaryQueriesSupport` API：**

:::note

此 API 必須在資料來源中以 typescript 程式碼實作。

:::

```ts
import {
  DataSourceWithSupplementaryQueriesSupport,
  LogLevel,
  SupplementaryQueryOptions,
  SupplementaryQueryType,
} from '@grafana/data';

export class ExampleDatasource
  extends DataSourceApi<ExampleQuery, ExampleOptions>
  implements DataSourceWithSupplementaryQueriesSupport<ExampleQuery>
{
  // 傳回資料來源支援的補充查詢類型。
  getSupportedSupplementaryQueryTypes(): SupplementaryQueryType[] {
    return [SupplementaryQueryType.LogsVolume];
  }

  // 根據提供的類型和原始查詢，傳回要用於擷取補充資料的補充查詢。
  // 如果提供的查詢不適用於提供的補充查詢類型，則應傳回 undefined。
  getSupplementaryQuery(options: SupplementaryQueryOptions, query: ExampleQuery): ExampleQuery | undefined {
    if (!this.getSupportedSupplementaryQueryTypes().includes(options.type)) {
      return undefined;
    }

    switch (options.type) {
      case SupplementaryQueryType.LogsVolume:
        // 這是一個模擬實作。請務必根據您的資料來源邏輯進行調整。
        return { ...query, refId: `logs-volume-${query.refId}`, queryType: 'count' };
      default:
        return undefined;
    }
  }

  // 它會為特定的補充查詢類型產生 DataQueryRequest。
  // @returns 補充查詢的 DataQueryRequest，如果不支援則為 undefined。
  getSupplementaryRequest(
    type: SupplementaryQueryType,
    request: DataQueryRequest<ExampleQuery>,
    options?: SupplementaryQueryOptions
  ): DataQueryRequest<ExampleQuery> | undefined {
    if (!this.getSupportedSupplementaryQueryTypes().includes(type)) {
      return undefined;
    }

    switch (type) {
      case SupplementaryQueryType.LogsVolume:
        const logsVolumeOption: LogsVolumeOption =
          options?.type === SupplementaryQueryType.LogsVolume ? options : { type };
        return this.getLogsVolumeDataProvider(request, logsVolumeOption);
      default:
        return undefined;
    }
  }

  // 請務必根據您的資料來源邏輯調整此範例。
  private getLogsVolumeDataProvider(
    request: DataQueryRequest<ExampleQuery>,
    options: LogsVolumeOption
  ): DataQueryRequest<ExampleQuery> | undefined {
    const logsVolumeRequest = cloneDeep(request);
    const targets = logsVolumeRequest.targets
      .map((query) => this.getSupplementaryQuery(options, query))
      .filter((query): query is ExampleQuery => !!query);

    if (!targets.length) {
      return undefined;
    }

    return { ...logsVolumeRequest, targets };
}
```

### 日誌範例

:::note

透過實作 `DataSourceWithXXXSupport` 介面，在資料來源中實作此 API。

:::

當您的資料來源同時支援日誌和指標時，[日誌範例](https://grafana.com/docs/grafana/latest/explore/logs-integration/#logs-sample)功能是一項很有價值的附加功能。它讓使用者能夠檢視促成視覺化指標的日誌行範例，從而更深入地了解資料。

若要在您的資料來源外掛程式中實作日誌範例支援，您可以使用 `DataSourceWithSupplementaryQueriesSupport` API。

```ts
import {
  DataSourceWithSupplementaryQueriesSupport,
  SupplementaryQueryOptions,
  SupplementaryQueryType,
} from '@grafana/data';

export class ExampleDatasource
  extends DataSourceApi<ExampleQuery, ExampleOptions>
  implements DataSourceWithSupplementaryQueriesSupport<ExampleQuery>
{
  // 傳回資料來源支援的補充查詢類型。
  getSupportedSupplementaryQueryTypes(): SupplementaryQueryType[] {
    return [SupplementaryQueryType.LogsSample];
  }

  // 根據提供的類型和原始查詢，傳回要用於擷取補充資料的補充查詢。
  // 如果提供的查詢不適用於提供的補充查詢類型，則應傳回 undefined。
  getSupplementaryQuery(options: SupplementaryQueryOptions, query: ExampleQuery): ExampleQuery | undefined {
    if (!this.getSupportedSupplementaryQueryTypes().includes(options.type)) {
      return undefined;
    }

    switch (options.type) {
      case SupplementaryQueryType.LogsSample:
        // 請務必根據您的資料來源邏輯調整此範例。
        return { ...query, refId: `logs-sample-${query.refId}`, queryType: 'logs' };
      default:
        return undefined;
    }
  }

  // 它會為特定的補充查詢類型產生 DataQueryRequest。
  // @returns 補充查詢的 DataQueryRequest，如果不支援則為 undefined。
  getSupplementaryRequest(
    type: SupplementaryQueryType,
    request: DataQueryRequest<ExampleQuery>,
    options?: SupplementaryQueryOptions
  ): DataQueryRequest<ExampleQuery> | undefined {
    if (!this.getSupportedSupplementaryQueryTypes().includes(type)) {
      return undefined;
    }

    switch (type) {
      case SupplementaryQueryType.LogsSample:
        const logsSampleOption: LogsSampleOptions =
          options?.type === SupplementaryQueryType.LogsSample ? options : { type };
        return this.getLogsSampleDataProvider(request, logsSampleOption);
      default:
        return undefined;
    }
  }
  
  private getLogsSampleDataProvider(
    request: DataQueryRequest<ExampleQuery>,
    options?: LogsSampleOptions
  ): DataQueryRequest<ExampleQuery> | undefined {
    const logsSampleRequest = cloneDeep(request);
    const targets = logsSampleRequest.targets
      .map((query) => this.getSupplementaryQuery({ type: SupplementaryQueryType.LogsSample, limit: 100 }, query))
      .filter((query): query is ExampleQuery => !!query);

    if (!targets.length) {
      return undefined;
    }
    return { ...logsSampleRequest, targets };
  }
}
```

如需如何在 Elasticsearch 資料來源中實作日誌範例的範例，請參閱 [PR 70258](https://github.com/grafana/grafana/pull/70258/)。

### 使用內部資料連結將日誌連結至追蹤

:::note

此功能目前不支援 Grafana 儲存庫以外的外部外掛程式。`@internal` API 目前正在開發中。

:::

如果您正在開發同時處理日誌和追蹤的資料來源外掛程式，且您的日誌資料包含追蹤 ID，您可以透過新增具有追蹤 ID 值和內部資料連結的欄位來增強您的日誌資料框架。這些連結應使用追蹤 ID 值來準確地建立產生相關追蹤的追蹤查詢。此增強功能讓使用者能夠無縫地從日誌行移至追蹤。

**TypeScript 中的範例：**

```ts
import { createDataFrame } from '@grafana/data';

const result = createDataFrame({
  fields: [
    ...,
    { name: 'traceID',
      type: FieldType.string,
      values: ['a006649127e371903a2de979', 'e206649127z371903c3be12q' 'k777549127c371903a2lw34'],
      config: {
        links: [
          {
            title: 'Trace view',
            url: '',
            internal: {
              // 請務必根據您的資料來源邏輯，使用 datasourceUid、datasourceName 和 query 調整此範例。
              datasourceUid: instanceSettings.uid,
              datasourceName: instanceSettings.name,
              query: {
                { ...query, queryType: 'trace', traceId: '${__value.raw}'}, // ${__value.raw} 是一個變數，將會被實際的 traceID 值取代。
              }
            }

          }
        ]
      }

    }
  ],
  ...,
});
```

### 日誌內容查詢編輯器

:::note

此功能目前不支援 Grafana 儲存庫以外的外部外掛程式。`@alpha` API 目前正在開發中。

:::

它允許外掛程式開發人員透過實作 `getLogRowContextUi?(row: LogRowModel, runContextQuery?: () => void, origQuery?: TQuery): React.ReactNode;` 方法，在內容檢視中顯示自訂 UI。
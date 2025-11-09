---
id: add-anonymous-usage-reporting
title: 新增匿名使用情況報告
description: 如何將匿名使用情況追蹤新增至您的 Grafana 插件。
keywords:
  - grafana
  - plugins
  - plugin
  - anonymous usage
  - reporting
---

# 新增匿名使用情況報告

將匿名使用情況追蹤新增至您的插件，以將描述您的插件使用方式的[報告事件](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana#reporting_enabled)傳送至追蹤系統，例如 Microsoft Application Insights 或 RudderStack。若要設定您的追蹤系統，包括傳送使用情況報告的位置，請聯絡您的 Grafana 伺服器管理員。

## 事件報告

在本節中，我們將展示一個從查詢編輯器追蹤使用情況資料並從分析服務接收報告的範例。

### 查詢編輯器範例

假設您有一個類似以下範例的 `QueryEditor`。它有一個 `CodeEditor` 欄位，您可以在其中撰寫查詢，還有一個查詢類型選取器，以便您可以選取預期傳回的查詢結果類型：

```tsx
import React, { ReactElement } from 'react';
import { InlineFieldRow, InlineField, Select, CodeEditor } from '@grafana/ui';
import type { EditorProps } from './types';

export function QueryEditor(props: EditorProps): ReactElement {
  const { datasource, query, onChange, onRunQuery } = props;
  const queryType = { value: query.value ?? 'timeseries' };
  const queryTypes = [
    {
      label: 'Timeseries',
      value: 'timeseries',
    },
    {
      label: 'Table',
      value: 'table',
    },
  ];

  const onChangeQueryType = (type: string) => {
    onChange({
      ...query,
      queryType: type,
    });
    runQuery();
  };

  const onChangeRawQuery = (rawQuery: string) => {
    onChange({
      ...query,
      rawQuery: type,
    });
    runQuery();
  };

  return (
    <>
      <div>
        <CodeEditor
          height="200px"
          showLineNumbers={true}
          language="sql"
          onBlur={onChangeRawQuery}
          value={query.rawQuery}
        />
      </div>
      <InlineFieldRow>
        <InlineField label="Query type" grow>
          <Select options={queryTypes} onChange={onChangeQueryType} value={queryType} />
        </InlineField>
      </InlineFieldRow>
    </>
  );
}
```

### 使用 `usePluginInteractionReporter` 追蹤使用情況

假設您想追蹤時間序列和表格查詢之間的使用情況。

您要做的是新增 `usePluginInteractionReporter` 以擷取一個報告函式，該函式接受兩個引數：

- 必要：以 `grafana_plugin_` 開頭的事件名稱。它用於識別正在進行的互動。
- 可選：附加的上下文資料。在我們的範例中，即為查詢類型。

```tsx
import React, { ReactElement } from 'react';
import { InlineFieldRow, InlineField, Select, CodeEditor } from '@grafana/ui';
import { usePluginInteractionReporter } from '@grafana/runtime';
import type { EditorProps } from './types';

export function QueryEditor(props: EditorProps): ReactElement {
  const { datasource, query, onChange, onRunQuery } = props;

  const report = usePluginInteractionReporter(); //  取得報告函式

  const queryType = { value: query.value ?? 'timeseries' };
  const queryTypes = [
    {
      label: 'Timeseries',
      value: 'timeseries',
    },
    {
      label: 'Table',
      value: 'table',
    },
  ];

  const onChangeQueryType = (type: string) => {
    onChange({
      ...query,
      queryType: type,
    });
    runQuery();
  };

  const onChangeRawQuery = (rawQuery: string) => {
    onChange({
      ...query,
      rawQuery: type,
    });

    //  傳送此包含兩個引數的報告
    report('grafana_plugin_executed_query', {
      query_type: queryType.value,
    });

    runQuery();
  };

  return (
    <>
      <div>
        <CodeEditor
          height="200px"
          showLineNumbers={true}
          language="sql"
          onBlur={onChangeRawQuery}
          value={query.rawQuery}
        />
      </div>
      <InlineFieldRow>
        <InlineField label="Query type" grow>
          <Select options={queryTypes} onChange={onChangeQueryType} value={queryType} />
        </InlineField>
      </InlineFieldRow>
    </>
  );
}
```

### 從分析服務傳回的資料

當您使用 `usePluginInteractionReporter` 時，傳回給您的報告函式會自動將有關您正在追蹤的插件的上下文資料附加到事件中。

在我們的範例中，以下資訊會傳送至由 Grafana 伺服器管理員設定的分析服務：

```ts
{
  type: 'interaction',
  payload: {
    interactionName: 'grafana_plugin_executed_query',
    grafana_version: '9.2.1',
    plugin_type: 'datasource',
    plugin_version: '1.0.0',
    plugin_id: 'grafana-example-datasource',
    plugin_name: 'Example',
    datasource_uid: 'qeSI8VV7z', // 只會針對資料來源新增
    query_type: 'timeseries'
  }
}
```
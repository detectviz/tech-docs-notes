---
id: add-features-for-explore-queries
title: 為探索查詢新增功能
description: 在 Grafana 插件開發中為探索查詢新增功能。
keywords:
  - grafana
  - plugins
  - plugin
  - queries
  - explore queries
  - explore
---

[探索](https://grafana.com/docs/grafana/latest/explore/)可讓您在不使用儀表板的情況下進行臨時查詢。當您想對資料進行疑難排解或深入了解時，這非常有用。您的資料來源預設支援「探索」，並使用現有的查詢編輯器。

本指南說明如何在資料來源插件中為「探索」查詢擴充功能。

## 新增特定於「探索」的查詢編輯器

若要為您的資料來源擴充「探索」功能，請定義一個特定於「探索」的查詢編輯器。

1. 在您插件的 `src` 目錄中建立一個名為 `ExploreQueryEditor.tsx` 的檔案，其內容類似於此：

   ```tsx title="src/ExploreQueryEditor.tsx"
   import React from 'react';

   import { QueryEditorProps } from '@grafana/data';
   import { QueryField } from '@grafana/ui';
   import { DataSource } from './DataSource';
   import { MyQuery, MyDataSourceOptions } from './types';

   type Props = QueryEditorProps<DataSource, MyQuery, MyDataSourceOptions>;

   export default (props: Props) => {
     return <h2>我的「探索」專用查詢編輯器</h2>;
   };
   ```

2. 修改您在 `QueryEditor.tsx` 中的基礎查詢編輯器，以呈現特定於「探索」的查詢編輯器。例如：

   ```tsx title="src/QueryEditor.tsx"
   // [...]
   import { CoreApp } from '@grafana/data';
   import ExploreQueryEditor from './ExploreQueryEditor';

   type Props = QueryEditorProps<DataSource, MyQuery, MyDataSourceOptions>;

   export default (props: Props) => {
     const { app } = props;

     switch (app) {
       case CoreApp.Explore:
         return <ExploreQueryEditor {...props} />;
       default:
         return <div>我的基礎查詢編輯器</div>;
     }
   };
   ```

## 選取偏好的視覺化類型

預設情況下，「探索」會識別您傳回的資料（時間序列、日誌或其他），並建立正確類型的視覺化。

但是，如果您想要自訂視覺化，可以透過將 `meta` 屬性設定為 `preferredVisualisationType`，為您傳回的資料框架新增提示。

像這樣建構一個具有特定元資料的資料框架：

```ts
const firstResult = createDataFrame({
    fields: [...],
    meta: {
        preferredVisualisationType: 'logs',
    },
});
```

有關可能的選項，請參閱 [PreferredVisualisationType](https://github.com/grafana/grafana/blob/main/packages/grafana-data/src/types/data.ts#L25)。
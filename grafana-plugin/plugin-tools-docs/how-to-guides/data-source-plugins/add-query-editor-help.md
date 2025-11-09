---
id: add-query-editor-help
title: 新增查詢編輯器說明
description: 如何在 Grafana 中為查詢編輯器新增說明元件。
keywords:
  - grafana
  - plugins
  - plugin
  - queries
  - query editor
  - query editor help
---

查詢編輯器支援新增說明元件，以顯示潛在查詢的範例。當使用者點擊其中一個範例時，查詢編輯器會自動更新。這有助於使用者更快地進行查詢。

1. 在您插件的 `src` 目錄中，建立一個名為 `QueryEditorHelp.tsx` 的檔案，其內容如下：

   ```ts
   import React from 'react';
   import { QueryEditorHelpProps } from '@grafana/data';

   export default (props: QueryEditorHelpProps) => {
     return <h2>我的速查表</h2>;
   };
   ```

2. 設定插件以使用 `QueryEditorHelp`：

   ```ts
   import QueryEditorHelp from './QueryEditorHelp';
   ```

   ```ts
   export const plugin = new DataSourcePlugin<DataSource, MyQuery, MyDataSourceOptions>(DataSource)
     .setConfigEditor(ConfigEditor)
     .setQueryEditor(QueryEditor)
     .setQueryEditorHelp(QueryEditorHelp);
   ```

3. 建立一些潛在查詢的範例：

   ```tsx
   import React from 'react';
   import { QueryEditorHelpProps, DataQuery } from '@grafana/data';

   const examples = [
     {
       title: '加法',
       expression: '1 + 2',
       label: '將兩個整數相加',
     },
     {
       title: '減法',
       expression: '2 - 1',
       label: '從另一個整數減去一個整數',
     },
   ];

   export default (props: QueryEditorHelpProps) => {
     return (
       <div>
         <h2>速查表</h2>
         {examples.map((item, index) => (
           <div className="cheat-sheet-item" key={index}>
             <div className="cheat-sheet-item__title">{item.title}</div>
             {item.expression ? (
               <div
                 className="cheat-sheet-item__example"
                 onClick={(e) => props.onClickExample({ refId: 'A', queryText: item.expression } as DataQuery)}
               >
                 <code>{item.expression}</code>
               </div>
             ) : null}
             <div className="cheat-sheet-item__label">{item.label}</div>
           </div>
         ))}
       </div>
     );
   };
   ```
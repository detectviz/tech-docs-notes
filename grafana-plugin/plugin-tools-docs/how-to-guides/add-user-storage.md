---
id: add-user-storage
title: 為您的插件新增使用者儲存空間
description: 如何為您的插件新增個別使用者儲存空間。
keywords:
  - grafana
  - plugins
  - plugin
  - storage
---

# 為您的插件新增使用者儲存空間

使用者儲存空間允許您的 Grafana 插件將使用者特定的資料儲存在 Grafana 資料庫中。此資料僅供個別使用者存取。但是，請記住，該資料未加密，不應用於儲存敏感資訊。使用者儲存空間的典型用途包括儲存使用者偏好或設定。

:::important

- 此功能適用於 Grafana 11.5 及更新版本。
- 它需要啟用 `userStorageAPI` 功能旗標。
- 如果插件使用此功能，但在 Grafana 執行個體中未啟用，則會改用瀏覽器的 `localStorage` 作為儲存機制。

:::

## 範例：為查詢編輯器新增使用者儲存空間

在此範例中，我們將透過整合使用者儲存空間來增強 `QueryEditor` 元件。它有一個 `Select` 欄位，您可以在其中選取您預期傳回的查詢結果類型。目標是記住使用者偏好的查詢類型（例如「時間序列」或「表格」），並在下次開啟查詢編輯器時將其用作預設值。

```tsx
import React, { ReactElement, useEffect } from 'react';
import { InlineFieldRow, InlineField, Select } from '@grafana/ui';
import { QueryEditorProps, SelectableValue } from '@grafana/data';
import { usePluginUserStorage } from '@grafana/runtime';

import { DataSource } from 'datasource';
import { MyDataSourceOptions, MyQuery } from 'types';

type Props = QueryEditorProps<DataSource, MyQuery, MyDataSourceOptions>;

export function QueryEditor(props: Props): ReactElement {
  const { query, onChange } = props;
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
  const storage = usePluginUserStorage();
  useEffect(() => {
    // 從使用者儲存空間載入預設查詢類型
    storage.getItem('queryType').then((value) => {
      if (value && !query.queryType) {
        onChange({
          ...query,
          queryType: value,
        });
      }
    });
  }, []);

  const onChangeQueryType = (type: SelectableValue<string>) => {
    if (type.value) {
      onChange({
        ...query,
        queryType: type.value,
      });
      // 將查詢類型儲存到使用者儲存空間，以便下次預設使用
      storage.setItem('queryType', type.value);
    }
  };

  return (
    <>
      <InlineFieldRow>
        <InlineField label="Query type" grow>
          <Select options={queryTypes} onChange={onChangeQueryType} value={{ value: query.queryType }} />
        </InlineField>
      </InlineFieldRow>
    </>
  );
}
```
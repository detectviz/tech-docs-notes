---
id: fetch-data-from-frontend
title: 使用資料代理從前端程式碼擷取資料
sidebar_label: 使用資料代理擷取前端資料
description: 學習如何在 Grafana 中使用資料代理 API 從資料來源和應用程式插件的前端程式碼擷取資料
keywords:
  - grafana
  - plugins
  - data proxy
  - frontend
  - data source
  - CORS
---

# 從前端資料來源和應用程式插件擷取資料

除了 JavaScript 的 [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) 之外，Grafana 資料代理也可用於從 Grafana 資料來源插件或應用程式插件擷取資料。

資料代理特別適用於

- 克服跨站 (CORS) 限制，或
- 執行已驗證的請求，或
- 將您插件設定中的其他敏感資料傳送至 Grafana。

本指南說明資料代理的運作方式並探討其使用中的常見問題。

## 它是如何運作的？

您不是直接從瀏覽器向伺服器發出請求，而是透過 Grafana 後端伺服器發出請求，後端伺服器會處理該請求並將回應傳回給插件。

- **不使用資料代理**：請求直接從瀏覽器傳送到第三方伺服器。
- **使用資料代理**：請求從瀏覽器傳送到 Grafana 後端，然後再傳送到第三方伺服器。在這種情況下，沒有 CORS 的限制，您可以指示 Grafana 使用儲存在插件設定中的敏感資料來傳送已驗證的請求。

:::note
您只能從資料來源和應用程式插件使用資料代理。_您無法從面板插件使用資料代理。_
:::

## 如何在資料來源插件中使用資料代理

從資料來源插件使用資料代理最簡單的方法是使用 [`DataSourceHttpSettings`](https://developers.grafana.com/ui/latest/index.html?path=/docs/data-source-datasourcehttpsettings--docs) 元件。

### 步驟 1：在您的資料來源插件設定頁面中使用 `DataSourceHttpSettings` 元件

```typescript title="src/ConfigEditor.tsx"
import React from 'react';
import { DataSourceHttpSettings } from '@grafana/ui';
import { DataSourcePluginOptionsEditorProps } from '@grafana/data';

export function ConfigEditor(props: DataSourcePluginOptionsEditorProps) {
  const { onOptionsChange, options } = props;

  return (
    <DataSourceHttpSettings
      defaultUrl="https://jsonplaceholder.typicode.com/"
      dataSourceConfig={options}
      onChange={onOptionsChange}
    />
  );
}
```

`DataSourceHttpSettings` 將會顯示一個表單，其中包含所有選項，供使用者設定 HTTP 端點，包括驗證、TLS、cookie 和逾時。

### 步驟 2：在您的資料來源插件中使用資料代理

使用者在資料來源設定頁面中輸入端點詳細資料後，您可以查詢傳入資料來源執行個體設定 (`DataSourceInstanceSettings.url`) 的資料代理 URL。

```typescript title="src/dataSource.ts"
import {
  DataQueryRequest,
  DataQueryResponse,
  DataSourceApi,
  DataSourceInstanceSettings,
  FieldType,
  PartialDataFrame,
} from '@grafana/data';
import { getBackendSrv } from '@grafana/runtime';
import { lastValueFrom } from 'rxjs';

type TODO = {
  title: string;
  id: number;
};

export class DataSource extends DataSourceApi {
  baseUrl: string;
  constructor(instanceSettings: DataSourceInstanceSettings) {
    super(instanceSettings);
    // 請注意，我們正在從 instanceSettings 儲存 URL
    this.baseUrl = instanceSettings.url!;
  }

  async query(options: DataQueryRequest): Promise<DataQueryResponse> {
    const response = getBackendSrv().fetch<TODO[]>({
      // 您可以在上面看到 `this.baseUrl` 是在建構函式中設定的
      // 在此範例中，我們假設設定的 url 是
      // https://jsonplaceholder.typicode.com
      /// 如果您檢查 `this.baseUrl`，您會看到 Grafana 資料代理 url
      url: `${this.baseUrl}/todos`,
    });
    // backendSrv fetch 傳回一個 observable 物件
    // 我們應該使用 rxjs 解開它
    const responseData = await lastValueFrom(response);
    const todos = responseData.data;

    // 在此範例中，我們將為所有查詢傳回相同的 todos
    // 在真正的資料來源中，每個目標都應視需要擷取資料。
    const data: PartialDataFrame[] = options.targets.map((target) => {
      return {
        refId: target.refId,
        fields: [
          { name: 'Id', type: FieldType.number, values: todos.map((todo) => todo.id) },
          { name: 'Title', type: FieldType.string, values: todos.map((todo) => todo.title) },
        ],
      };
    });

    return { data };
  }

  async testDatasource() {
    return {
      status: 'success',
      message: 'Success',
    };
  }
}
```

:: note
使用者必須先在設定頁面中設定資料來源，然後資料來源才能透過資料來源查詢
端點。如果未設定資料來源，資料代理將不知道要將請求傳送到哪個
端點。
::

## 如何在具有自訂設定頁面的資料來源插件中使用資料代理

如果您不想使用 `DataSourceHttpSettings` 元件，而是建立自己的設定頁面
，則需要在您的插件中進行一些額外的設定。

### 步驟 1：在您的插件元資料中宣告您的路由

您首先需要在您的 `plugin.json` 元資料中設定路由。

```json title="src/plugin.json"
"routes": [
	{
	  "path": "myRoutePath",
	  "url": "{{ .JsonData.apiUrl }}"
	}
],
```

請注意，`url` 值包含 `jsonData.apiUrl` 的內插。您的設定頁面必須負責根據使用者輸入在 `jsonData` 物件中設定 `apiUrl`。

:::note
每次修改 `plugin.json` 檔案時，您都必須建置您的插件並重新啟動 Grafana 伺服器。
:::

### 步驟 2：建立您的設定頁面

```typescript title="src/ConfigEditor.tsx"
import React, { ChangeEvent } from 'react';
import { InlineField, Input } from '@grafana/ui';

export function ConfigEditor(props: Props) {
  const { onOptionsChange, options } = props;
  const { jsonData } = options;

  const onApiUrlChange = (event: ChangeEvent<HTMLInputElement>) => {
    onOptionsChange({
      ...options,
      jsonData: {
        ...jsonData,
        // 請注意，我們在 jsonData 內設定 apiUrl 值
        apiUrl: event.target.value,
      },
    });
  };

  return (
    <InlineField label="apiUrl" labelWidth={12}>
      <Input
        onChange={onApiUrlChange}
        value={jsonData.apiUrl || ''}
        placeholder="json field returned to frontend"
        width={40}
      />
    </InlineField>
    {/* 您設定頁面表單的其餘部分 */}
  );
}
```

### 步驟 3：從您的前端程式碼擷取資料

在您的資料來源插件中，您現在可以使用代理 URL 來擷取資料。

請參閱[上一個範例](#step-2-create-your-configuration-page)，但變更 `query` 函式中的 `url`，以便將[先前宣告的](#step-1-declare-your-route-in-your-plugin-metadata) `path` 包含在 `url` 中：

```typescript title="src/dataSource.ts"
async query(options: DataQueryRequest): Promise<DataQueryResponse> {
  const response = getBackendSrv().fetch<TODO[]>({
      // 您可以在上面看到 `this.baseUrl` 是在建構函式中設定的
      // 在此範例中，我們假設設定的 url 是
      // https://jsonplaceholder.typicode.com
      // 如果您檢查 `this.baseUrl`，您會看到 Grafana 資料代理 url
      // 並且您需要將 myRoutePath 路徑（先前在 plugin.json 中設定）新增到 url
      url: `${this.baseUrl}/myRoutePath/todos`,
  });
  // backendSrv fetch 傳回一個 observable 物件
  // 我們應該使用 rxjs 解開它
  const responseData = await lastValueFrom(response);
  const todos = responseData.data;

  // 在此範例中，我們將為所有查詢傳回相同的 todos
  // 在真正的資料來源中，每個目標都應視需要擷取資料。
  const data: PartialDataFrame[] = options.targets.map((target) => {
    return {
      refId: target.refId,
      fields: [
        { name: 'Id', type: FieldType.number, values: todos.map((todo) => todo.id) },
        { name: 'Title', type: FieldType.string, values: todos.map((todo) => todo.title) },
      ],
    };
  });

  return { data };
}
```

## 在應用程式插件中使用資料代理

`plugin.json` 元資料中的路由設定與資料來源插件中的相同。但是，由於應用程式插件不會在其 props 中接收 URL，因此 URL 的建構方式如下：

```typescript
const dataProxyUrl = `api/plugin-proxy/${PLUGIN_ID}/yourRoutePath`;
```

以下是從應用程式插件中的資料代理擷取資料的函式範例：

在 `src/plugin.json` 中宣告路由。您也可以使用已驗證的請求和 `jsonData` 內插，就像在資料來源插件中一樣。

```json title="src/plugin.json"
"routes": [
{
        "path": "myRoutePath",
        "url": "https://api.example.com",
        // jsonData 內插也是可能的
        //"url": "{{ .JsonData.apiUrl }}",
}]
```

然後，在您的應用程式插件程式碼中，您可以透過如下建構資料代理 URL 來使用資料代理擷取資料：

```typescript title="src/dataproxy-api-example.ts"
import { getBackendSrv } from '@grafana/runtime';
import { lastValueFrom } from 'rxjs';

async function getDataFromApi() {
  const dataProxyUrl = `api/plugin-proxy/${PLUGIN_ID}/myRoutePath`;
  const response = getBackendSrv().fetch<TODO[]>({
    url: dataProxyUrl,
  });
  return await lastValueFrom(response);
}
```

## 將其他 HTTP 方法（例如，POST、PUT、DELETE）與資料代理搭配使用

您可以直接在 `fetch` 方法中指定方法。您在 `src/plugin.json` 中的路由保持不變：

```typescript
const response = getBackendSrv().fetch<TODO[]>({
  url: `${this.baseUrl}`,
  method: 'POST',
  data: dataToSendViaPost,
});
```

## 使用資料代理將驗證新增至您的請求

若要了解如何將驗證新增至資料代理，請參閱我們的[文件](./add-authentication-for-data-source-plugins)。

## 對資料代理的請求進行偵錯

如果您想對從 Grafana 後端傳送到您的 API 的請求進行偵錯，請在[設定](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/#dataproxy)中啟用資料代理記錄。

您還必須在 Grafana 中[啟用偵錯記錄](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/#mode)，才能在您的 Grafana 設定檔中看到資料代理記錄。

**範例：**

```
[log]
level = debug

[dataproxy]
logging = true
```

使用此設定，Grafana 伺服器輸出會顯示從資料代理傳送到您的 API 的請求。

## 使用資料代理傳送特殊標頭

您可以使用資料代理傳送特殊標頭。若要了解如何將標頭新增至資料代理，請參閱我們的[文件](./add-authentication-for-data-source-plugins)。
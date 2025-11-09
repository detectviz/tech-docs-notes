---
id: add-resource-handler
title: 為資料來源插件新增資源處理常式
description: 學習如何為資料來源插件新增資源處理常式。
keywords:
  - grafana
  - plugins
  - plugin
  - data source
  - datasource
  - resource
  - resource handler
---

import ImplementResourceHandler from '@shared/implement-resource-handler.md';

# 為資料來源插件新增資源處理常式

您可以將資源處理常式新增至您的資料來源後端，以使用您自己的資料來源特定路由來擴充 Grafana HTTP API。本指南說明了您可能想要新增[資源](../../key-concepts/backend-plugins/#resources)處理常式的原因以及一些常見的作法。

## 資源處理常式的用途

資料來源從後端擷取資料的主要方式是透過[查詢方法](../../tutorials/build-a-data-source-plugin#define-a-query)。但有時您的資料來源需要隨需請求資料；例如，在資料來源的查詢編輯器中自動提供自動完成功能。

資源處理常式對於建立允許使用者回寫至資料來源的控制面板也很有用。例如，您可以新增一個資源處理常式來更新物聯網裝置的狀態。

<ImplementResourceHandler />

## 存取資料來源資源

實作後，您可以使用 Grafana HTTP API 和從前端存取資源。

### 使用 Grafana HTTP API

您可以透過使用端點 `http://<GRAFANA_HOSTNAME>:<PORT>/api/datasources/uid/<DATASOURCE_UID>/resources{/<RESOURCE>}` 來透過 Grafana HTTP API 存取資源。`DATASOURCE_UID` 是唯一識別您資料來源的資料來源唯一識別碼 (UID)，而 `RESOURCE` 則取決於資源處理常式的實作方式以及支援哪些資源（路由）。

使用上述範例，您可以存取以下資源：

- HTTP GET `http://<GRAFANA_HOSTNAME>:<PORT>/api/datasources/uid/<DATASOURCE_UID>/resources/namespaces`
- HTTP GET `http://<GRAFANA_HOSTNAME>:<PORT>/api/datasources/uid/<DATASOURCE_UID>/resources/projects`

:::tip

若要驗證資料來源 UID，您可以在瀏覽器的開發人員工具主控台中輸入 `window.grafanaBootData.settings.datasources`，以列出您 Grafana 執行個體中所有已設定的資料來源。

:::

### 從前端

您可以使用 `DataSourceWithBackend` 類別中的 `getResource` 和 `postResource` 輔助函式來查詢您的資源。為了為您的元件提供更好、更方便的 API，建議擴充您的資料來源類別和執行個體，為每個路由提供函式，如下列範例所示：

```typescript
export class MyDataSource extends DataSourceWithBackend<MyQuery, MyDataSourceOptions> {
  constructor(instanceSettings: DataSourceInstanceSettings<MyDataSourceOptions>) {
    super(instanceSettings);
  }

  getNamespaces(): Promise<NamespacesResponse> {
    return this.getResource('/namespaces');
  }

  getProjects(): Promise<ProjectsResponse> {
    return this.getResource('/projects');
  }
}
```

例如，在您的查詢編輯器元件中，您可以從 `props` 物件存取資料來源執行個體，並使用 `getNamespaces` 將 HTTP GET 請求傳送至 `http://<GRAFANA_HOSTNAME>:<PORT>/api/datasources/uid/<DATASOURCE_UID>/resources/namespaces`：

```typescript
const namespaces = await props.datasource.getNamespaces();
```

## 其他範例

一些使用資源處理常式和 [`httpadapter`](https://pkg.go.dev/github.com/grafana/grafana-plugin-sdk-go/backend/resource/httpadapter) 套件的其他範例：

- [datasource-basic](https://github.com/grafana/grafana-plugin-examples/tree/main/examples/datasource-basic) 範例。
- Grafana 的內建 TestData 資料來源，[建立資源處理常式](https://github.com/grafana/grafana/blob/5687243d0b3bad06c4da809f925cfdf3d32c5a16/pkg/tsdb/grafana-testdata-datasource/testdata.go#L45)和[註冊路由](https://github.com/grafana/grafana/blob/5687243d0b3bad06c4da809f925cfdf3d32c5a16/pkg/tsdb/grafana-testdata-datasource/resource_handler.go#L17-L28)。
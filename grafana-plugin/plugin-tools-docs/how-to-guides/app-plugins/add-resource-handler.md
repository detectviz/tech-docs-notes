---
id: add-resource-handler
title: 為應用程式插件新增資源處理常式
description: 學習如何為應用程式插件新增資源處理常式。
keywords:
  - grafana
  - plugins
  - plugin
  - app
  - resource
  - resource handler
---

import ImplementResourceHandler from '@shared/implement-resource-handler.md';

# 為應用程式插件新增資源處理常式

您可以將資源處理常式新增至您的應用程式後端，以使用您自己的應用程式特定路由來擴充 Grafana HTTP API。本指南說明了您可能想要新增[資源](../../key-concepts/backend-plugins/#resources)處理常式的原因以及一些常見的作法。

## 資源處理常式的用途

應用程式通常會與某種 HTTP 服務整合，例如第三方服務，以擷取和傳送資料。例如，此服務可能有特定的驗證和授權需求，或其回應格式不適合傳回給 Grafana 和插件前端。

此外，您可能想要[保護您的資源](implement-rbac-in-app-plugins.md#secure-backend-resources)，以便只有具有特定權限的使用者才能存取某些路由。

資源處理常式對於建立允許使用者回寫至應用程式的控制面板也很有用。例如，您可以新增一個資源處理常式來更新物聯網裝置的狀態。

<ImplementResourceHandler />

## 存取應用程式資源

實作後，您可以使用 Grafana HTTP API 和從前端存取資源。

### 使用 Grafana HTTP API

您可以透過使用端點 `http://<GRAFANA_HOSTNAME>:<PORT>/api/plugins/<PLUGIN_ID>/resources{/<RESOURCE>}` 來透過 Grafana HTTP API 存取資源。`PLUGIN_ID` 是唯一識別您應用程式的插件識別碼，而 `RESOURCE` 則取決於資源處理常式的實作方式以及支援哪些資源（路由）。

使用上述範例，您可以存取以下資源：

- HTTP GET `http://<GRAFANA_HOSTNAME>:<PORT>/api/plugins/<PLUGIN_ID>/resources/namespaces`
- HTTP GET `http://<GRAFANA_HOSTNAME>:<PORT>/api/plugins/<PLUGIN_ID>/resources/projects`

### 從前端

您可以在元件中使用 `backendSrv` 執行階段服務的 `get` 函式來存取您的資源，以將 HTTP GET 請求傳送至 `http://<GRAFANA_HOSTNAME>:<PORT>/api/plugins/<PLUGIN_ID>/resources/namespaces`

```typescript
import { getBackendSrv } from '@grafana/runtime';

const namespaces = await getBackendSrv().get(`/api/plugins/<PLUGIN_ID>/resources/namespaces`);
```
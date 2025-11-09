---
id: add-backend-component
title: 為應用程式插件新增後端元件
description: 如何為應用程式插件新增後端元件
keywords:
  - grafana
  - plugins
  - plugin
  - app
  - backend
---

import CreatePlugin from '@shared/create-plugin-backend.md';
import BackendPluginAnatomy from '@shared/backend-plugin-anatomy.md';
import TroubleshootPluginLoad from '@shared/troubleshoot-plugin-doesnt-load.md';

# 為應用程式插件新增後端元件

應用程式插件的後端元件可讓您擴充應用程式插件以取得額外功能，例如自訂驗證方法和與其他服務的整合。

以下是應用程式插件中後端元件的典型使用案例：

- 使用 Grafana 不支援的自訂驗證方法
- 使用 Grafana 不支援的授權檢查
- 在伺服器端執行工作負載
- 連線到通常無法從瀏覽器連線的非 HTTP 服務

## 開始之前

在新增後端元件之前，請先安裝以下先決條件：

- Go ([版本](https://github.com/grafana/plugin-tools/blob/main/packages/create-plugin/templates/backend/go.mod#L3))
- [Mage](https://magefile.org/)
- [LTS](https://nodejs.dev/en/about/releases/) 版本的 Node.js
- [Docker](https://docs.docker.com/get-docker/)

## 建立新的應用程式插件

<CreatePlugin pluginType="app" />

## 具有後端元件的插件結構

<BackendPluginAnatomy pluginType="app" />

## 為您的應用程式插件新增驗證

若要深入了解如何為您的應用程式插件新增驗證（例如，呼叫自訂後端或第三方 API）和處理機密，請參閱[為應用程式插件新增驗證](./add-authentication-for-app-plugins.md)。

## 存取應用程式設定

設定是 `AppInstanceSettings` 結構的一部分。它們會作為第二個引數傳遞給應用程式插件的建構函式。例如：

```go title="src/app.go"
func NewApp(ctx context.Context, settings backend.AppInstanceSettings) (instancemgmt.Instance, error) {
  jsonData := settings.JSONData // json.RawMessage
  secureJsonData := settings.DecryptedSecureJSONData // map[string]string
}
```

您也可以從請求的 `Context` 中取得設定：

```go title="src/resources.go"
func (a *App) handleMyRequest(w http.ResponseWriter, req *http.Request) {
  pluginConfig := backend.PluginConfigFromContext(req.Context())
  jsonData := pluginConfig.AppInstanceSettings.JSONData // json.RawMessage
}
```

## 為您的應用程式插件新增自訂端點

以下是如何為您的應用程式插件新增 `ServeMux` 或 `CallResource` 端點。

### ServeMux (建議)

您建立的應用程式插件已經有一個預設的 `CallResource`，它使用 [`ServeMux`](https://pkg.go.dev/net/http#ServeMux)。它看起來像這樣：

```go title="app.go"
type App struct {
	backend.CallResourceHandler
}

// NewApp 建立一個新的範例 *App 執行個體。
func NewApp(_ context.Context, _ backend.AppInstanceSettings) (instancemgmt.Instance, error) {
	var app App

	// 為資源呼叫使用 httpadapter（由 SDK 提供）。這允許我們
	// 為資源呼叫使用 *http.ServeMux，因此我們可以將多個路由
	// 對應到 CallResource，而無需實作額外的邏輯。
	mux := http.NewServeMux()
	app.registerRoutes(mux)
  // 實作 CallResourceHandler 介面
	app.CallResourceHandler = httpadapter.New(mux)

	return &app, nil
}
```

現在您可以為您的應用程式插件新增自訂端點。

建立的程式碼已包含一個 `resources.go` 檔案，其中包含 `registerRoutes` 函式。

```go title="resources.go"
// 此函式已存在於建立的應用程式插件中
func (a *App) registerRoutes(mux *http.ServeMux) {
	mux.HandleFunc("/myCustomEndpoint", a.handleMyCustomEndpoint)
}

func (a *App) handleMyCustomEndpoint(w http.ResponseWriter, r *http.Request) {
  // 處理請求
  // 例如，呼叫第三方 API
  w.Write([]byte("my custom response"))
  w.WriteHeader(http.StatusOK)
}
```

### CallResource

您也可以透過直接將 `CallResource` 處理常式新增至您的後端元件來為您的應用程式插件新增自訂端點。您必須實作處理多個請求的邏輯。

```go title="app.go"
func (a *App) CallResource(ctx context.Context, req *backend.CallResourceRequest, sender backend.CallResourceResponseSender) error {
	switch req.Path {
	case "myCustomEndpoint":
		sender.Send(&backend.CallResourceResponse{
			Status: http.StatusOK,
			Body:   []byte("my custom response"),
		})
	default:
		return sender.Send(&backend.CallResourceResponse{
			Status: http.StatusNotFound,
		})
	}
	return nil
}
```

您也可以參閱資料來源[關於資源處理常式的文件](../data-source-plugins/add-resource-handler.md)，您也可以將其應用於您的應用程式插件。

### 從前端程式碼呼叫您的自訂端點

若要從前端程式碼呼叫您的自訂端點，您可以使用 `getBackendSrv` 中的 `fetch` 函式。例如：

```ts
import { getBackendSrv } from '@grafana/runtime';
import { lastValueFrom } from 'rxjs';

function getMyCustomEndpoint() {
  const response = await getBackendSrv().fetch({
    // 將 ${PLUGIN_ID} 替換為您的插件 ID
    url: '/api/plugins/${PLUGIN_ID}/myCustomEndpoint',
  });
  return await lastValueFrom(response);
}
```

## 疑難排解

<TroubleshootPluginLoad />
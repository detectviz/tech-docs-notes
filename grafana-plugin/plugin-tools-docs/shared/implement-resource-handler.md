## 實作資源處理常式介面

若要為您的插件後端元件新增資源處理常式，您需要實作 `backend.CallResourceHandler` 介面。

您可以在您的插件中透過兩種方式實作此介面：[使用 `httpadapter` 套件](#using-the-httpadapter-package)或在您的插件中[手動實作它](#manually-implementing-backendcallresourcehandler)。

### 使用 `httpadapter` 套件

由 [Grafana Plugin SDK for Go](../../key-concepts/backend-plugins/grafana-plugin-sdk-for-go) 提供的 [`httpadapter`](https://pkg.go.dev/github.com/grafana/grafana-plugin-sdk-go/backend/resource/httpadapter) 套件是處理資源的建議方式。此套件支援使用 [`http.Handler`](https://pkg.go.dev/net/http#Handler) 介面來處理資源呼叫，並允許以更無關 Go 的方式回應 HTTP 請求，並使其更容易支援多個路由和方法（GET、POST 等）。

使用 [`http.Handler`](https://pkg.go.dev/net/http#Handler) 可讓您同時使用 Go 的內建路由器功能 [`ServeMux`](https://pkg.go.dev/net/http#ServeMux) 或您偏好的 HTTP 路由器函式庫（例如，[`gorilla/mux`](https://github.com/gorilla/mux)）。

:::note

Go 1.22 [包含路由增強功能](https://go.dev/blog/routing-enhancements)，它新增了對使用 [`ServeMux`](https://pkg.go.dev/net/http#ServeMux) 進行方法比對和萬用字元的支援。

:::

在以下範例中，我們示範了如何使用 `httpadapter` 套件、`ServeMux` 和 `http.Handler` 來新增對擷取命名空間 (`/namespaces`)、專案 (`/projects`) 和更新某些裝置狀態 (`/device`) 的支援：

```go
package myplugin

import (
  "context"
  "net/http"

  "github.com/grafana/grafana-plugin-sdk-go/backend"
  "github.com/grafana/grafana-plugin-sdk-go/backend/resource/httpadapter"
)

type MyPlugin struct {
  resourceHandler backend.CallResourceHandler
}

func New() *MyPlugin {
  p := &MyPlugin{}
  mux := http.NewServeMux()
  mux.HandleFunc("/namespaces", p.handleNamespaces)
  mux.HandleFunc("/projects", p.handleProjects)
  p.resourceHandler := httpadapter.New(mux)
  return p
}

func (p *MyPlugin) CallResource(ctx context.Context, req *backend.CallResourceRequest, sender backend.CallResourceResponseSender) error {
  return p.resourceHandler.CallResource(ctx, req, sender)
}

func (p *MyPlugin) handleNamespaces(rw http.ResponseWriter, req *http.Request) {
  rw.Header().Add("Content-Type", "application/json")
  _, err := rw.Write([]byte(`{ "namespaces": ["ns-1", "ns-2"] }`))
  if err != nil {
    return
  }
  rw.WriteHeader(http.StatusOK)
}

func (p *MyPlugin) handleProjects(rw http.ResponseWriter, req *http.Request) {
  rw.Header().Add("Content-Type", "application/json")
  _, err := rw.Write([]byte(`{ "projects": ["project-1", "project-2"] }`))
  if err != nil {
    return
  }
  rw.WriteHeader(http.StatusOK)
}
```

#### 存取插件上下文

您可以使用 [backend.PluginConfigFromContext](https://pkg.go.dev/github.com/grafana/grafana-plugin-sdk-go/backend#PluginConfigFromContext) 函式來存取 [backend.PluginContext](https://pkg.go.dev/github.com/grafana/grafana-plugin-sdk-go/backend#PluginContext)。它包含有關插件請求的上下文資訊，例如執行請求的使用者：

```go
func (p *MyPlugin) handleSomeRoute(rw http.ResponseWriter, req *http.Request) {
	pCtx := backend.PluginConfigFromContext(req.Context())
	bytes, err := json.Marshal(pCtx.User)
	if err != nil {
		return
	}

	rw.Header().Add("Content-Type", "application/json")
	_, err := rw.Write(bytes)
	if err != nil {
		return
	}
	rw.WriteHeader(http.StatusOK)
}
```

### 手動實作 `backend.CallResourceHandler`

對於基本需求，手動實作 `backend.CallResourceHandler` 介面可能就足夠了。若要支援幾個不同的路由來擷取資料，您可以使用帶有 `req.Path` 的 switch：

```go
func (p *MyPlugin) CallResource(ctx context.Context, req *backend.CallResourceRequest, sender backend.CallResourceResponseSender) error {
	switch req.Path {
	case "namespaces":
		return sender.Send(&backend.CallResourceResponse{
			Status: http.StatusOK,
			Body:   []byte(`{ "namespaces": ["ns-1", "ns-2"] }`),
		})
	case "projects":
		return sender.Send(&backend.CallResourceResponse{
			Status: http.StatusOK,
			Body:   []byte(`{ "projects": ["project-1", "project-2"] }`),
		})
	default:
		return sender.Send(&backend.CallResourceResponse{
			Status: http.StatusNotFound,
		})
	}
}
```
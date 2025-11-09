---
id: use-a-service-account
title: 在 Grafana 應用程式插件中使用服務帳戶
description: 如何在 Grafana 應用程式插件中使用服務帳戶以對 Grafana API 進行驗證。
keywords:
  - grafana
  - plugin
  - app
  - service
  - bundling
  - authentication
---

# 在 Grafana 應用程式插件中使用服務帳戶

具有服務帳戶的應用程式插件可以對 Grafana API 進行驗證，而無需使用者介入，從而允許您的插件以特定權限存取 Grafana 資源。服務帳戶為您的插件提供了一種安全的方式來與 Grafana 的後端服務和 API 互動。

當您的插件註冊時，Grafana 會自動管理服務帳戶。與可能需要使用者憑證或手動產生權杖的傳統驗證方法不同。

## 開始之前

請確保您的開發環境符合以下先決條件：

- **Grafana 版本：** 使用 Grafana 10.3 或更新版本
- **功能開關：** 啟用 `externalServiceAccounts` 功能開關。請參閱我們關於[設定 Grafana 功能開關](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/#feature_toggles)的文件
- **設定變數：** 啟用 [auth.managed_service_accounts_enabled](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/#managed_service_accounts_enabled) 設定變數。請參閱我們關於[設定 Grafana](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/#configure-grafana) 的文件
- **部署類型：** 此功能目前**僅支援單一組織部署**

## 新增服務帳戶設定

若要將您的應用程式插件設定為使用服務帳戶，請在您的 `plugin.json` 檔案中新增一個 `iam` 區段：

```json title="plugin.json"
"iam": {
  "permissions": [
    { "action": "dashboards:create", "scope": "folders:uid:*" },
    { "action": "dashboards:read", "scope": "folders:uid:*"},
    { "action": "dashboards:write", "scope": "folders:uid:*"},
    { "action": "folders:read", "scope": "folders:uid:*"},
    { "action": "folders:write", "scope": "folders:uid:*"},
    { "action": "org.users:read", "scope": "users:*"},
    { "action": "teams:read", "scope": "teams:*"},
    { "action": "teams.permissions:read", "scope": "teams:*"}
  ]
}
```

`permissions` 陣列定義了服務帳戶可以存取的特定動作和範圍。有關可用權限，請參閱 [Grafana 存取控制文件](https://grafana.com/docs/grafana/latest/administration/roles-and-permissions/access-control/)。

## 擷取服務帳戶權杖

當您的插件啟動時，Grafana 會自動建立一個具有指定權限的服務帳戶，並為您的插件提供一個權杖。從請求上下文中擷取此權杖：

```go title="plugin.go"
// 從插件上下文中取得服務帳戶權杖
cfg := backend.GrafanaConfigFromContext(req.Context())
saToken, err := cfg.PluginAppClientSecret()
if err != nil {
  http.Error(w, err.Error(), http.StatusInternalServerError)
  return
}
```

## 使用權杖進行 API 請求

### 選項 1：使用權杖設定 HTTP 用戶端

設定您的 HTTP 用戶端以在所有請求中包含權杖：

```go title="plugin.go"
opts, err := settings.HTTPClientOptions(ctx)
if err != nil {
  return nil, fmt.Errorf("http client options: %w", err)
}

opts.Headers = map[string]string{"Authorization": "Bearer " + saToken}

// 用戶端現在已預先設定了 bearer 權杖
client, err := httpclient.New(opts)
if err != nil {
  return nil, fmt.Errorf("httpclient new: %w", err)
}
```

### 選項 2：將權杖新增至個別請求

或者，將權杖新增至特定的 HTTP 請求：

```go title="plugin.go"
req, err := http.NewRequest("GET", grafanaAPIURL, nil)
if err != nil {
  return nil, err
}
req.Header.Set("Authorization", "Bearer " + saToken)
```

## 實作範例

以下是一個資源處理常式的簡單範例，它使用服務帳戶權杖將請求代理至 Grafana API：

```go title="plugin.go"
func (a *App) handleAPI(w http.ResponseWriter, req *http.Request) {
  // 從上下文中取得 Grafana 設定
  cfg := backend.GrafanaConfigFromContext(req.Context())

  // 取得 Grafana 的基礎 URL
  grafanaAppURL, err := cfg.AppURL()
  if err != nil {
    http.Error(w, err.Error(), http.StatusInternalServerError)
    return
  }

  // 取得服務帳戶權杖
  saToken, err := cfg.PluginAppClientSecret()
  if err != nil {
    http.Error(w, err.Error(), http.StatusInternalServerError)
    return
  }

  // 建立對 Grafana API 的請求
  reqURL, err := url.JoinPath(grafanaAppURL, req.URL.Path)
  proxyReq, err := http.NewRequest("GET", reqURL, nil)

  // 將權杖新增至請求
  proxyReq.Header.Set("Authorization", "Bearer " + saToken)

  // 發出請求
  res, err := a.httpClient.Do(proxyReq)
  // 處理回應...
}
```

## 限制

- 服務帳戶會自動在預設組織（ID：`1`）中建立
- 插件只能存取該特定組織內的資料和資源
- 如果您的插件需要與多個組織搭配使用，則此功能不適用

## 安全性考量

- 使用者無法修改或刪除服務帳戶
- 權杖根據您插件中定義的權限提供對 Grafana 資源的存取權
- 請勿將服務帳戶權杖暴露給前端或終端使用者

## 深入了解

- [Grafana 服務帳戶文件](https://grafana.com/docs/grafana/latest/administration/service-accounts/)
- [Grafana 存取控制文件](https://grafana.com/docs/grafana/latest/administration/roles-and-permissions/access-control/)
- [Grafana plugin.json 參考](https://grafana.com/developers/plugin-tools/reference-plugin-json)
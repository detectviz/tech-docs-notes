---
id: implement-rbac-in-app-plugins
title: 在應用程式插件中實作 RBAC
description: 如何為 Grafana 應用程式插件新增基於角色的存取控制 (RBAC)。
keywords:
  - grafana
  - plugins
  - plugin
  - advanced
  - apps
  - app plugins
  - rbac
  - roles
  - access control
---

Grafana 應用程式插件中的基於角色的存取控制 (RBAC) 對於建立安全且量身打造的使用者體驗至關重要。透過實作 RBAC，您可以確保敏感功能和資料僅供具有適當權限的使用者存取，從而增強安全性和可用性。正確的設定至關重要，因為設定錯誤可能會導致安全性漏洞。

有關 RBAC 的運作方式以及每個角色的更多資訊，請參閱[官方文件](https://grafana.com/docs/grafana/latest/administration/roles-and-permissions/access-control/rbac-for-app-plugins/)。

## 開始之前

需要 Grafana 11.6.0 或更新版本。

## 定義角色

若要為您的插件建立角色，請在您的 `plugin.json` 檔案中插入一個 `roles` 區段。例如：

```json
"roles": [
  {
    "role": {
      "name": "Patents Reader",
      "description": "Read patents",
      "permissions": [
        {"action": "grafana-appwithrbac-app.patents:read"}
      ]
    },
    "grants": ["Admin"] // 自動將此角色授予具有 Admin 角色的使用者。
  },
  {
    "role": {
      "name": "Research Papers Reader",
      "description": "Read research papers",
      "permissions": [
        {"action": "grafana-appwithrbac-app.papers:read"}
      ]
    },
    "grants": ["Viewer"] // 自動將此角色授予具有 Viewer 角色的使用者。
  }
]
```

在 `roles` 陣列中，每個角色物件都指定了 `name` 和 `description` 以求清晰和便於治理，而 `permissions` 則定義了該角色可以執行的確切動作，例如 `read` 或 `write`。`grants` 陣列決定了哪些預設使用者角色（例如 `Admin` 或 `Viewer`）應自動獲得這些自訂角色。

例如，在上面的範例中，具有 `Viewer` 角色的使用者將自動被授予 `Research Papers Reader` 角色。

在定義角色時，請確保每個角色都具有唯一的權限以清楚地區分，以避免衝突和意外存取。最好遵循最小權限原則，為任務指派必要的最低權限。

## 保護前端包含項

若要在您的前端頁面上強制執行基於動作的存取控制，請將 `action` 參數納入您 `plugin.json` 檔案中的包含項定義中。以下是應用方法：

```json
"includes": [
  {
    "type": "page",
    "name": "Research documents",
    "path": "/a/%PLUGIN_ID%/research-docs",
    "action": "grafana-appwithrbac-app.papers:read",
    "addToNav": true,
    "defaultNav": false
    // 此頁面僅會針對具有 'papers:read' 權限的使用者顯示
  },
  {
    "type": "page",
    "name": "Patents",
    "path": "/a/%PLUGIN_ID%/patents",
    "action": "grafana-appwithrbac-app.patents:read",
    "addToNav": true,
    "defaultNav": false
    // 此頁面僅會針對具有 'patents:read' 權限的使用者顯示
  }
]
```

## 保護代理路由

若要使用動作檢查來保護您的代理路由，請在您 `plugin.json` 檔案中的路由定義中包含 `reqAction` 參數。以下是如何執行的範例：

```json
"routes": [
  {
    "path": "api/external/patents",
    "method": "*",
    "reqAction": "grafana-appwithrbac-app.patents:read",
    "url": "{{ .JsonData.backendUrl }}/api/external/patents",
    "headers": [
      {
        "name": "Authorization",
        "content": "{{ .SecureJsonData.backendApiToken }}"
      }
    ]
  }
]
```

## 保護後端資源

如果您的後端公開了資源，您可以使用基於動作的檢查來保護它們。

若要啟用此保護，請啟用 `externalServiceAccounts` 功能。這允許使用受管理的服務帳戶來存取 Grafana 使用者權限。

:::note

`externalServiceAccounts` 功能僅支援單一組織設定。
您可以透過如下修改 `docker-compose.yaml` 檔案在您的 Grafana 執行個體中啟用它：

```yaml
environment:
  - GF_FEATURE_TOGGLES_ENABLE=externalServiceAccounts
```

:::

後端服務帳戶和 ID 轉發設定可讓您插件的後端可靠地驗證請求並確定使用者的身分和權限。此設定對於維持對後端資源的安全和受控存取至關重要。

在您的 `plugin.json` 中，新增 `iam` 區段以取得具有所需權限的服務帳戶權杖：

```json
"iam": {
  "permissions": [
    {"action": "users.permissions:read", "scope": "users:*"}
  ]
}
```

## 動作集、資料夾存取層級與權限

動作集系統提供了一種方法，讓插件可以擴充 Grafana 對資料夾的檢視、編輯或管理員存取權限。

注意：

- **僅限資料夾**：僅限於 `folders:view/edit/admin` 動作集
- **已設定範圍**：一旦使用者取得資料夾的檢視/編輯/管理員存取權限，權限就會限定在該特定資料夾的範圍內。
- **僅限附加**：動作集只能擴充，不能修改或限制

擴充動作集以外的任何內容都會導致錯誤。
```bash
logger=plugins.actionsets.registration pluginId=grafana-lokiexplore-app error="[accesscontrol.actionSetInvalid] 
currently only folder and dashboard action sets are supported, provided action set grafana-lokiexplore-app:view is not a folder or dashboard action set"
```

### Grafana 動作集

Grafana 有幾個可以擴充的資料夾動作集：

**資料夾動作集：**
- `folders:view` → `["folders:read", "dashboards:read"]`
- `folders:edit` → `["folders:read", "folders:write", "dashboards:read", "dashboards:write", "folders:create"]`
- `folders:admin` → `["folders:read", "folders:write", "folders:delete", "folders.permissions:read", "folders.permissions:write", ...]`

### 程式碼路徑
[RegistrationsOfActionSets](https://github.com/grafana/grafana/blob/main/pkg/services/accesscontrol/resourcepermissions/service.go#L574
)

### 插件動作集定義

插件可以在其 `plugin.json` 中定義動作集：

```json
{
  "id": "my-plugin",
  "type": "app", 
  "actionSets": [
    {
      "action": "folders:edit",
      "actions": [
        "my-plugin.documents:create",
        "my-plugin.documents:update", 
        "my-plugin.templates:write"
      ]
    },
    {
      "action": "folders:admin", 
      "actions": [
        "my-plugin.settings:write",
        "my-plugin.users:manage",
        "my-plugin.permissions:write"
      ]
    }
  ]
}
```

### 實際範例

**使用動作集（擴充現有集合）：**
```json
{
  "actionSets": [
    {
      "action": "folders:edit",
      "actions": [
        "my-plugin.docs:create",
        "my-plugin.docs:edit"
      ]
    }
  ],
  ...
}
```

**結果：** 當使用者被授予對資料夾的編輯存取權限時，他們會得到：
- 所有原始的 `folders:edit` 動作（folders:read, folders:write, folders:create）
- 加上插件的額外動作（my-plugin.docs:create, my-plugin.docs:edit）

## 實作

接下來，將 `authlib/authz` 函式庫整合到您插件的後端程式碼中，以有效管理授權：

```go
import "github.com/grafana/authlib/authz"
```

若要設定授權用戶端，請先從傳入請求的插件上下文中擷取用戶端密碼。由於用戶端密碼保持不變，您只需要初始化一次授權用戶端。此方法可有效利用用戶端快取。

使用以下函式取得授權用戶端：

```go
// GetAuthZClient 傳回一個透過插件上下文設定的 authz 強制用戶端。
func (a *App) GetAuthZClient(req *http.Request) (authz.EnforcementClient, error) {
	ctx := req.Context()
	ctxLogger := log.DefaultLogger.FromContext(ctx)
	cfg := backend.GrafanaConfigFromContext(ctx)

	saToken, err := cfg.PluginAppClientSecret()
	if err != nil || saToken == "" {
		if err == nil {
			err = errors.New("service account token not found")
		}
		ctxLogger.Error("Service account token not found", "error", err)
		return nil, err
	}

	// 防止兩個並行呼叫更新用戶端
	a.mx.Lock()
	defer a.mx.Unlock()

	if saToken == a.saToken {
		ctxLogger.Debug("Token unchanged returning existing client")
		return a.authzClient, nil
	}

	grafanaURL, err := cfg.AppURL()
	if err != nil {
		ctxLogger.Error("App URL not found", "error", err)
		return nil, err
	}

	// 初始化授權用戶端
	client, err := authz.NewEnforcementClient(authz.Config{
		APIURL: grafanaURL,
		Token:  saToken,
		// Grafana 在本地設定上簽署 JWT
		JWKsURL: strings.TrimRight(grafanaURL, "/") + "/api/signing-keys/keys",
	},
		// 擷取所有以 grafana-appwithrbac-app 為前綴的使用者權限
		authz.WithSearchByPrefix("grafana-appwithrbac-app"),
		// 使用具有較低到期時間的快取
		authz.WithCache(cache.NewLocalCache(cache.Config{
			Expiry:          10 * time.Second,
			CleanupInterval: 5 * time.Second,
		})),
	)
	if err != nil {
		ctxLogger.Error("Initializing authz client", "error", err)
		return nil, err
	}

	a.saToken = saToken
	a.authzClient = client

	return client, nil
}
```

:::note

`WithSearchByPrefix` 選項用於透過根據其前綴篩選動作來最小化對授權伺服器的頻繁查詢。

`WithCache` 選項可讓您自訂函式庫的內部快取，允許您指定替代的快取設定。預設情況下，快取會在 5 分鐘後到期。

:::

完成此設定後，您可以使用用戶端實作存取控制。例如：

```go
func (a *App) HasAccess(req *http.Request, action string) (bool, error) {
	// 擷取 ID 權杖
	idToken := req.Header.Get("X-Grafana-Id")
	if idToken == "" {
		return false, errors.New("id token not found")
	}

	authzClient, err := a.GetAuthZClient(req)
	if err != nil {
		return false, err
	}

	// 檢查使用者存取權限
	hasAccess, err := authzClient.HasAccess(req.Context(), idToken, action)
	if err != nil || !hasAccess {
		return false, err
	}
	return true, nil
}
```

在 `Resources` 端點中使用函式以執行存取控制檢查並驗證使用者是否擁有存取指定資源所需的必要權限。

```go
if hasAccess, err := a.HasAccess(req, "grafana-appwithrbac-app.patents:read"); err != nil || !hasAccess {
  if err != nil {
    log.DefaultLogger.FromContext(req.Context()).Error("Error checking access", "error", err)
  }
  http.Error(w, "permission denied", http.StatusForbidden)
  return
}
```

## 實作前端存取控制檢查

實作前端存取檢查以防止未經授權的使用者導覽至受限制的 UI 區段，並確保與後端權限一致且安全的使用者體驗。

為了防止 UI 損壞，透過僅根據使用者權限註冊路由和顯示連結來實作這些檢查至關重要。這種主動的方法可確保使用者介面反映後端定義的安全性原則，提供無縫且安全的使用者體驗。

若要執行存取控制檢查，請從 Grafana 執行階段套件匯入 `hasPermission` 函式。

```ts
import { hasPermission } from '@grafana/runtime';
```

然後可以如下執行檢查：

```ts
if (hasPermission('grafana-appwithrbac-app.papers:read')) {
  // 範例：註冊路由、顯示連結等
}
```

## 指派角色

您可以透過導覽至使用者管理區段在 Grafana 中指派角色，您可以在其中根據使用者的職責為使用者指派自訂角色。詳細步驟可在我們全面的[角色管理指南](https://grafana.com/docs/grafana/latest/administration/roles-and-permissions/)中找到。

將角色指派給特定使用者需要 Grafana Cloud 或 [Grafana Enterprise 授權](https://grafana.com/docs/grafana/latest/administration/roles-and-permissions/access-control/#role-based-access-control-rbac)。

如果您有 Grafana Enterprise 授權，則可以如下編輯 `docker-compose.yaml` 檔案：

```yaml
environment:
  - GF_ENTERPRISE_LICENSE_TEXT=<your license>
```
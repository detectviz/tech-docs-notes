---
id: add-authentication-for-app-plugins
title: 為應用程式插件新增驗證
description: 如何為應用程式插件請求新增驗證
keywords:
  - grafana
  - plugins
  - plugin
  - authentication
  - app
  - apps
---

import CreatePlugin from '@shared/create-plugin-backend.md';

Grafana 應用程式插件可讓您捆綁面板和資料來源。應用程式還允許您在 Grafana 中建立具有複雜功能的自訂頁面。

## 選擇驗證方法

有兩種方法可以為應用程式插件新增驗證。您可以透過以下兩種方式之一設定您的應用程式插件以對第三方 API 進行驗證：

- 使用[資料來源代理](#authenticate-using-the-data-source-proxy)方法。
- 建立一個[具有後端元件的插件](#authenticate-using-a-backend-component)。

| 情況 | 使用 |
| --- | --- |
| 您是否需要使用基本驗證或 API 金鑰來驗證您的插件？ | 使用資料來源代理。 |
| 您的 API 是否使用資料來源代理不支援的自訂驗證方法？ | 使用後端元件。 |
| 您的 API 是否透過 HTTP 以外的協定進行通訊？ | 使用後端元件。 |

## 加密機密設定

應用程式插件有兩種儲存自訂設定的方式：

- `jsonData`
- `secureJsonFields`

:::warning

請勿將 `jsonData` 用於密碼、權杖和 API 金鑰等敏感資料。如果您需要儲存敏感資訊，請改用 `secureJsonData`。

:::

### 將設定儲存在 `secureJsonData` 中

如果您需要儲存敏感資訊（機密），請使用 `secureJsonData` 而非 `jsonData`。每當使用者儲存應用程式設定時，`secureJsonData` 中的機密都會傳送到 Grafana 伺服器，並在儲存前進行加密。

一旦您加密了安全設定，就無法再從瀏覽器存取該設定。儲存後存取機密的唯一方法是使用[_資料來源代理_](#authenticate-using-the-data-source-proxy)或透過[後端元件](#authenticate-using-a-backend-component)。

### 將機密設定新增至您的應用程式插件

您引導的應用程式插件應具有一個 `AppConfig` 元件，允許使用者設定應用程式。此元件包含將 `apiKey` 儲存在 `secureJsonData` 中的範例程式碼。您可以在 `secureJsonFields` 中檢查 `secureJsonData` 的屬性，該屬性是 `plugin.meta` 的一部分。`secureJsonFields` 物件包含使用者已設定的金鑰。

以下是一些程式碼重點：

1. `secureJsonData` 永遠不會帶有已填入的值，無論使用者是否已設定它。相反地，您可以透過檢查金鑰在 `secureJsonFields` 中是否為 `true` 來判斷屬性是否已設定。例如：

   ```ts
   const { jsonData, secureJsonFields } = plugin.meta;
   const [state, setState] = useState<State>({
     apiUrl: jsonData?.apiUrl || '',
     apiKey: '',
     // 檢查金鑰是 true 還是 false 以判斷它是否已設定
     isApiKeySet: Boolean(secureJsonFields?.apiKey),
   });
   ```

2. 您可以透過向 `/api/plugins/<pluginId>/settings` 端點傳送 POST 來更新 `secureJsonData`。

   如果您要在 `secureJsonData` 中設定金鑰，則只應傳送使用者已修改值的金鑰。傳送任何值（包括空字串）都會覆寫現有設定。

   ```ts
   const secureJsonData = apiKey.length > 0 ? { apiKey } : undefined;
   await getBackendSrv().fetch({
     url: `/api/plugins/${pluginId}/settings`,
     method: 'POST',
     data: {
       secureJsonData,
     },
   });
   ```

## 使用資料來源代理進行驗證

使用者儲存應用程式的設定後，機密設定在瀏覽器中將變得不可用。加密的機密只能在伺服器上存取。那麼您如何將它們新增到您的請求中呢？

Grafana 伺服器附帶一個代理，可讓您為請求定義範本：_代理路由_。Grafana 將代理路由傳送到伺服器，解密機密以及其他設定，並在傳送請求之前將它們新增到請求中。

:::note

請務必不要將資料代理與[驗證代理](https://grafana.com/docs/grafana/latest/setup-grafana/configure-security/configure-authentication/auth-proxy)混淆。資料代理用於驗證插件請求，而驗證代理用於登入 Grafana 本身。

:::

### 將代理路由新增至您的插件

若要透過資料代理轉發請求，您需要設定一個或多個_代理路由_。代理路由是由代理處理的任何傳出請求的範本。您可以在 [plugin.json](../../reference/metadata.md) 檔案中設定代理路由。

1. 將路由新增至 `plugin.json`：

   ```json title="src/plugin.json"
   "routes": [
     {
       "path": "myRoutePath",
       "url": "https://api.example.com"
     }
   ]
   ```

   :::note

   每次變更 `plugin.json` 檔案時，您都需要建置您的插件並重新啟動 Grafana 伺服器。

   :::

2. 在您的應用程式插件中，使用 `@grafana/runtime` 套件中的 `getBackendSrv` 函式從代理路由擷取資料：

   ```ts
   import { getBackendSrv } from '@grafana/runtime';
   import { lastValueFrom } from 'rxjs';

   async function getDataFromApi() {
     const dataProxyUrl = `api/plugin-proxy/${PLUGIN_ID}/myRoutePath`;
     const response = getBackendSrv().fetch({
       url: dataProxyUrl,
     });
     return await lastValueFrom(response);
   }
   ```

### 將動態代理路由新增至您的插件

在 Grafana 將資料代理請求傳送到伺服器後，資料來源代理會解密敏感資料。然後，資料來源代理會在發出請求之前，使用解密的資料內插範本變數。

若要將使用者定義的設定新增至您的路由：

- 對於儲存在 `jsonData` 中的設定，請使用 `.JsonData`。例如，其中 `projectId` 是 `jsonData` 物件中屬性的名稱：

  ```json title="src/plugin.json"
  "routes": [
    {
      "path": "example",
      "url": "https://api.example.com/projects/{{ .JsonData.projectId }}"
    }
  ]
  ```

- 對於儲存在 `secureJsonData` 中的敏感資料，請使用 `.SecureJsonData`。例如，其中 `password` 是 `secureJsonData` 物件中屬性的名稱，請使用 `.SecureJsonData`：

  ```json title="src/plugin.json"
  "routes": [
    {
      "path": "example",
      "url": "https://{{ .JsonData.username }}:{{ .SecureJsonData.password }}@api.example.com"
    }
  ]
  ```

除了將 URL 新增至代理路由外，您還可以新增標頭、URL 參數和請求主體。

#### 將 HTTP 標頭新增至代理路由

以下是將 `name` 和 `content` 新增為 HTTP 標頭的範例：

```json title="src/plugin.json"
"routes": [
  {
    "path": "example",
    "url": "https://api.example.com",
    "headers": [
      {
        "name": "Authorization",
        "content": "Bearer {{ .SecureJsonData.apiToken }}"
      }
    ]
  }
]
```

#### 將請求主體新增至代理路由

以下是將 `username` 和 `password` 新增至請求主體的範例：

```json title="src/plugin.json"
"routes": [
  {
    "path": "example",
    "url": "http://api.example.com",
    "body": {
      "username": "{{ .JsonData.username }}",
      "password": "{{ .SecureJsonData.password }}"
    }
  }
]
```

#### 應用程式插件中資料代理的限制

- 應用程式插件不支援 `urlParams` 設定。
- 應用程式插件不支援 `tokenAuth` 設定（適用於 OAuth 2.0）。

## 使用後端元件進行驗證

雖然資料代理支援 HTTP API 最常見的驗證方法，但使用代理路由存在一些限制：

- 代理路由僅支援 HTTP 或 HTTPS。
- 代理路由不支援自訂權杖驗證。
- 應用程式的代理路由不支援 `urlParams`。
- 應用程式的代理路由不支援 `tokenAuth`。

如果您的插件適用於任何這些限制，您需要為您的插件新增一個後端元件。因為後端元件在伺服器上執行，所以它們可以存取解密的機密，這使得實作自訂驗證方法更容易。

### 在後端元件中存取機密

解密的機密可從應用程式執行個體設定中的 `DecryptedSecureJSONData` 欄位取得。

```go
func (a *App) registerRoutes(mux *http.ServeMux) {
        // ... 其他路由
	mux.HandleFunc("/test", a.handleMyRequest)
}

func (a *App) handleMyRequest(w http.ResponseWriter, req *http.Request) {
	pluginConfig := backend.PluginConfigFromContext(req.Context())
	secureJsonData := pluginConfig.AppInstanceSettings.DecryptedSecureJSONData

        // 使用解密的資料

	w.Header().Add("Content-Type", "application/json")
	if _, err := w.Write([]byte(`{"message": "ok}`)); err != nil {
		http.Error(w, err.Error(), http.StatusInternalServerError)
		return
	}
	w.WriteHeader(http.StatusOK)
}
```

### 使用 Cookie

您的應用程式插件可以讀取 Grafana 轉發給應用程式的 Cookie。

```go
func (a *App) handleMyRequest(w http.ResponseWriter, req *http.Request) {

	cookies := req.Cookies()

        // 迴圈遍歷 cookie 作為範例
	for _, cookie := range cookies {
		log.Printf("cookie: %+v", cookie)
	}
        // 使用 cookie

	w.Header().Add("Content-Type", "application/json")
	if _, err := w.Write([]byte(`{"message": "ok}`)); err != nil {
		http.Error(w, err.Error(), http.StatusInternalServerError)
		return
	}
	w.WriteHeader(http.StatusOK)
}
```

### 為登入的使用者轉發使用者標頭

啟用 [`send_user_header`](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/#send_user_header) 後，Grafana 會使用 `X-Grafana-User` 標頭將使用者標頭傳遞給插件。
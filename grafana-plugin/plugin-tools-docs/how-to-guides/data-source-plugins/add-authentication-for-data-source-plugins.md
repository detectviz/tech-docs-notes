---
id: add-authentication-for-data-source-plugins
title: 為資料來源插件新增驗證
sidebar_label: 新增驗證
description: 如何為資料來源插件新增驗證。
keywords:
  - grafana
  - plugins
  - plugin
  - authentication
  - data source
  - datasource
---

Grafana 插件可以透過使用資料來源代理或自訂插件後端，對第三方 API 執行已驗證的請求。

## 選擇驗證方法

透過以下兩種方式之一，設定您的資料來源插件以對第三方 API 進行驗證：

- 使用[資料來源代理](#authenticate-using-the-data-source-proxy)方法，或
- 建立一個[插件後端元件](#authenticate-using-a-plugin-backend)。

| 情況 | 使用 |
| --- | --- |
| 您是否需要使用基本驗證或 API 金鑰來驗證您的插件？ | 使用資料來源代理。 |
| 您的 API 是否支援使用用戶端憑證的 OAuth 2.0？ | 使用資料來源代理。 |
| 您的 API 是否使用資料來源代理不支援的自訂驗證方法？ | 使用插件後端。 |
| 您的 API 是否透過 HTTP 以外的協定進行通訊？ | 使用插件後端。 |
| 您的插件是否需要警示支援？ | 使用插件後端。 |

## 加密資料來源設定

資料來源插件有兩種儲存自訂設定的方式：`jsonData` 和 `secureJsonData`。

具有檢視者角色的使用者可以以明文形式存取資料來源設定，例如 `jsonData` 的內容。如果您已啟用匿名存取，任何可以在其瀏覽器中存取 Grafana 的人都可以看到 `jsonData` 的內容。

[Grafana Enterprise](https://grafana.com/products/enterprise/grafana/) 的使用者可以將資料來源的存取權限限制給特定的使用者和團隊。更多資訊，請參閱[資料來源權限](https://grafana.com/docs/grafana/latest/enterprise/datasource_permissions)。

您可以透過在瀏覽器的開發人員主控台中輸入 `window.grafanaBootData` 來查看目前使用者有權存取的設定。

:::warning

請勿將 `jsonData` 用於密碼、權杖和 API 金鑰等敏感資料。如果您需要儲存敏感資訊，請改用 `secureJsonData`。

:::

### 將設定儲存在 secureJsonData 中

如果您需要儲存敏感資訊，請使用 `secureJsonData` 而非 `jsonData`。每當使用者儲存資料來源設定時，`secureJsonData` 中的機密都會傳送到 Grafana 伺服器，並在儲存前進行加密。

一旦您加密了安全設定，就無法再從瀏覽器存取它。儲存後存取機密的唯一方法是使用[_資料來源代理_](#authenticate-using-the-data-source-proxy)。

### 將機密設定新增至您的資料來源插件

若要將機密新增至資料來源插件，您可以新增對設定 API 金鑰的支援。

1. 在 `types.ts` 中建立一個新介面以存放 API 金鑰：

   ```ts
   export interface MySecureJsonData {
     apiKey?: string;
   }
   ```

2. 透過更新您的 `ConfigEditor` 的 props 以接受該介面作為第二個類型參數，為您的 `secureJsonData` 物件新增類型資訊。從您的 `ConfigEditor` 內的 `options` prop 存取機密的值：

   ```ts
   interface Props extends DataSourcePluginOptionsEditorProps<MyDataSourceOptions, MySecureJsonData> {}
   ```

   ```ts
   const { secureJsonData, secureJsonFields } = options;
   const { apiKey } = secureJsonData;
   ```

   :::note

   您可以在使用者儲存設定之前執行此操作；當使用者儲存設定時，Grafana 會清除該值。之後，您可以使用 `secureJsonFields` 來判斷該屬性是否已設定。

   :::

3. 若要在您的插件設定編輯器中安全地更新機密，請使用 `onOptionsChange` prop 更新 `secureJsonData` 物件：

   ```ts
   const onAPIKeyChange = (event: ChangeEvent<HTMLInputElement>) => {
     onOptionsChange({
       ...options,
       secureJsonData: {
         apiKey: event.target.value,
       },
     });
   };
   ```

4. 定義一個可以接受使用者輸入的元件：

   ```tsx
   <Input
     type="password"
     placeholder={secureJsonFields?.apiKey ? 'configured' : ''}
     value={secureJsonData.apiKey ?? ''}
     onChange={onAPIKeyChange}
   />
   ```

5. 可選：如果您希望使用者能夠重設 API 金鑰，則需要在 `secureJsonFields` 物件中將該屬性設定為 `false`：

   ```ts
   const onResetAPIKey = () => {
     onOptionsChange({
       ...options,
       secureJsonFields: {
         ...options.secureJsonFields,
         apiKey: false,
       },
       secureJsonData: {
         ...options.secureJsonData,
         apiKey: '',
       },
     });
   };
   ```

一旦使用者可以設定機密，下一步就是看看我們如何將它們新增到我們的請求中。

## 使用資料來源代理進行驗證

使用者儲存資料來源的設定後，機密資料來源設定將不再可在瀏覽器中使用。加密的機密只能在伺服器上存取。那麼您如何將它們新增到您的請求中呢？

Grafana 伺服器附帶一個代理，可讓您為請求定義範本：_代理路由_。Grafana 將代理路由傳送到伺服器，解密機密以及其他設定，並在傳送請求之前將它們新增到請求中。

:::note

請務必不要將資料來源代理與[驗證代理](https://grafana.com/docs/grafana/latest/setup-grafana/configure-security/configure-authentication/auth-proxy)混淆。資料來源代理用於驗證資料來源，而驗證代理用於登入 Grafana 本身。

:::

### 將代理路由新增至您的插件

若要透過 Grafana 代理轉發請求，您需要設定一個或多個_代理路由_。代理路由是由代理處理的任何傳出請求的範本。您可以在 [plugin.json](../../reference/metadata.md) 檔案中設定代理路由。

1. 將路由新增至 `plugin.json`：

   ```json title="src/plugin.json"
   "routes": [
     {
       "path": "example",
       "url": "https://api.example.com"
     }
   ]
   ```

   :::note

   每次變更 `plugin.json` 檔案時，您都需要建置您的插件並重新啟動 Grafana 伺服器。

   :::

2. 在 `DataSource` 中，將代理 URL 從 `instanceSettings` 提取到一個名為 `url` 的類別屬性：

   ```ts
   export class DataSource extends DataSourceApi<MyQuery, MyDataSourceOptions> {
     url?: string;

     constructor(instanceSettings: DataSourceInstanceSettings<MyDataSourceOptions>) {
       super(instanceSettings);

       this.url = instanceSettings.url;
     }

     // ...
   }
   ```

3. 在 `query` 方法中，使用 `BackendSrv` 發出請求。URL 路徑的第一部分需要與您的代理路由的 `path` 相符。資料來源代理會將 `this.url + routePath` 替換為路由的 `url`。根據我們的範例，請求的 URL 將是 `https://api.example.com/v1/users`：

   ```ts
   import { getBackendSrv } from '@grafana/runtime';
   ```

   ```ts
   const routePath = '/example';

   getBackendSrv().datasourceRequest({
     url: this.url + routePath + '/v1/users',
     method: 'GET',
   });
   ```

### 將動態代理路由新增至您的插件

Grafana 將代理路由傳送到伺服器，資料來源代理會在其中解密任何敏感資料，並在發出請求之前使用解密的資料內插範本變數。

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

- 對於儲存在 `secureJsonData` 中的敏感資料，請使用 `.SecureJsonData`。例如，其中 `password` 是 `secureJsonData` 物件中屬性的名稱：

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

#### 將 URL 參數新增至代理路由

以下是將 `name` 和 `content` 新增為 URL 參數的範例：

```json title="src/plugin.json"
"routes": [
  {
    "path": "example",
    "url": "http://api.example.com",
    "urlParams": [
      {
        "name": "apiKey",
        "content": "{{ .SecureJsonData.apiKey }}"
      }
    ]
  }
]
```

:::note

請注意，`urlParams` 設定僅在資料來源插件中受支援。它在[應用程式插件](../app-plugins/add-authentication-for-app-plugins)中_不_受支援。

:::

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

### 將 OAuth 2.0 代理路由新增至您的插件

由於您對每個路由的請求都是在伺服器端使用 OAuth 2.0 驗證發出的，因此僅支援機器對機器的請求。換句話說，如果您需要使用用戶端憑證以外的不同授權類型，您需要自己實作。

若要使用 OAuth 2.0 進行驗證，請在代理路由定義中新增一個 `tokenAuth` 物件。如有必要，Grafana 會向 `tokenAuth` 中定義的 URL 發出請求以擷取權杖，然後再向您的代理路由中的 URL 發出請求。Grafana 會在權杖到期時自動續訂。

在 `tokenAuth.params` 中定義的任何參數都會以 `application/x-www-form-urlencoded` 編碼並傳送到權杖 URL。

```json title="src/plugin.json"
{
  "routes": [
    {
      "path": "api",
      "url": "https://api.example.com/v1",
      "tokenAuth": {
        "url": "https://api.example.com/v1/oauth/token",
        "params": {
          "grant_type": "client_credentials",
          "client_id": "{{ .SecureJsonData.clientId }}",
          "client_secret": "{{ .SecureJsonData.clientSecret }}"
        }
      }
    }
  ]
}
```

:::note

請注意，`tokenAuth` 設定僅在資料來源插件中受支援。它在[應用程式插件](../app-plugins/add-authentication-for-app-plugins)中_不_受支援。

:::

## 使用插件後端進行驗證

雖然資料來源代理支援 HTTP API 最常見的驗證方法，但使用代理路由有一些限制：

- 代理路由僅支援 HTTP 或 HTTPS。
- 代理路由不支援自訂權杖驗證。

如果您的插件適用於任何這些限制，您需要新增一個[後端元件](../../key-concepts/backend-plugins/#caching-and-connection-pooling)。因為插件後端元件在伺服器上執行，所以它們可以存取解密的機密，這使得實作自訂驗證方法更容易。

解密的機密可從執行個體設定中的 `DecryptedSecureJSONData` 欄位取得。

```go
func (ds *dataSource) QueryData(ctx context.Context, req *backend.QueryDataRequest) (*backend.QueryDataResponse, error) {
  instanceSettings := req.PluginContext.DataSourceInstanceSettings

  if apiKey, exists := instanceSettings.DecryptedSecureJSONData["apiKey"]; exists {
    // 使用解密的 API 金鑰。
  }

  // ...
}
```

## 為登入的使用者轉發 OAuth 身分

如果您的資料來源使用與 Grafana 本身相同的 OAuth 提供者，例如，使用[通用 OAuth 驗證](https://grafana.com/docs/grafana/latest/setup-grafana/configure-security/configure-authentication/generic-oauth)，那麼您的資料來源插件可以重複使用登入的 Grafana 使用者的存取權杖。

若要允許 Grafana 將存取權杖傳遞給插件，請更新資料來源設定並將 `jsonData.oauthPassThru` 屬性設定為 `true`。[DataSourceHttpSettings](https://developers.grafana.com/ui/latest/index.html?path=/story/data-source-datasourcehttpsettings--basic) 設定提供了一個切換開關，即 **Forward OAuth Identity** 選項。您也可以在您的資料來源設定頁面 UI 中建立一個適當的切換開關來設定 `jsonData.oauthPassThru`。

設定後，Grafana 可以將 `Authorization` 或 `X-ID-Token` 等授權 HTTP 標頭轉發到後端資料來源。此資訊可在 `QueryData`、`CallResource` 和 `CheckHealth` 請求中取得。

若要讓 Grafana 轉發標頭，請使用 [Grafana plugin SDK for Go](https://pkg.go.dev/github.com/grafana/grafana-plugin-sdk-go/backend/httpclient) 建立一個 HTTP 用戶端，並將 `ForwardHTTPHeaders` 選項設定為 `true`（預設為 `false`）。此套件會公開請求資訊，這些資訊隨後可以轉發到下游或直接在插件中使用，或兩者兼而有之。

```go
func NewDatasource(ctx context.Context, settings backend.DataSourceInstanceSettings) (instancemgmt.Instance, error) {
  opts, err := settings.HTTPClientOptions(ctx)
	if err != nil {
		return nil, fmt.Errorf("http client options: %w", err)
	}

    // 重要：為每個查詢重複使用相同的用戶端，以避免用盡主機上的所有可用連線。

  opts.ForwardHTTPHeaders = true

	cl, err := httpclient.New(opts)
	if err != nil {
		return nil, fmt.Errorf("httpclient new: %w", err)
	}
	return &Datasource{
		httpClient: cl,
	}, nil
}

func (ds *dataSource) QueryData(ctx context.Context, req *backend.QueryDataRequest) (*backend.QueryDataResponse, error) {
    // 必須保留 Context，因為注入的中介軟體是在那裡設定的
    req, err := http.NewRequestWithContext(ctx, http.MethodGet, "https://some-url", nil)
    if err != nil {
      return nil, fmt.Errorf("new request with context: %w", err)
    }
    // 如果設定了 oauthPassThru，則會自動注入 Authorization 標頭
    resp, err := ds.httpClient.Do(req)
    // ...
}
```

### 從 HTTP 請求中提取標頭

如果您需要直接存取 HTTP 標頭資訊，您也可以從請求中提取該資訊：

```go
func (ds *dataSource) CheckHealth(ctx context.Context, req *backend.CheckHealthRequest) (*backend.CheckHealthResult, error) {
  token := strings.Fields(req.GetHTTPHeader(backend.OAuthIdentityTokenHeaderName))
  var (
    tokenType   = token[0]
    accessToken = token[1]
  )
  idToken := req.GetHTTPHeader(backend.OAuthIdentityIDTokenHeaderName) // 如果使用者的權杖包含 ID 權杖，則存在

  // ...
  return &backend.CheckHealthResult{Status: backend.HealthStatusOk}, nil
}

func (ds *dataSource) QueryData(ctx context.Context, req *backend.QueryDataRequest) (*backend.QueryDataResponse, error) {
  token := strings.Fields(req.GetHTTPHeader(backend.OAuthIdentityTokenHeaderName))
  var (
    tokenType   = token[0]
    accessToken = token[1]
  )
  idToken := req.GetHTTPHeader(backend.OAuthIdentityIDTokenHeaderName)

  for _, q := range req.Queries {
    // ...
  }
}

func (ds *dataSource) CallResource(ctx context.Context, req *backend.CallResourceRequest, sender backend.CallResourceResponseSender) error {
  token := req.GetHTTPHeader(backend.OAuthIdentityTokenHeaderName)
  idToken := req.GetHTTPHeader(backend.OAuthIdentityIDTokenHeaderName)

  // ...
}
```

## 使用 Cookie

### 為登入的使用者轉發 Cookie

您的資料來源插件可以為登入的 Grafana 使用者轉發 Cookie 到資料來源。在資料來源的設定頁面上使用 [DataSourceHttpSettings](https://developers.grafana.com/ui/latest/index.html?path=/story/data-source-datasourcehttpsettings--basic) 元件。它提供了 **Allowed cookies** 選項，您可以在其中指定 Cookie 名稱。

設定後，與[授權標頭](#forward-oauth-identity-for-the-logged-in-user)一樣，如果您使用 SDK HTTP 用戶端，這些 Cookie 會被自動注入。

### 為登入的使用者提取 Cookie

如果需要，您也可以在 `QueryData`、`CallResource` 和 `CheckHealth` 請求中提取 Cookie。

**`QueryData`**

```go
func (ds *dataSource) QueryData(ctx context.Context, req *backend.QueryDataRequest) (*backend.QueryDataResponse, error) {
  cookies:= req.GetHTTPHeader(backend.CookiesHeaderName)

  // ...
}
```

**`CallResource`**

```go
func (ds *dataSource) CallResource(ctx context.Context, req *backend.CallResourceRequest, sender backend.CallResourceResponseSender) error {
  cookies:= req.GetHTTPHeader(backend.CookiesHeaderName)

  // ...
}
```

**`CheckHealth`**

```go
func (ds *dataSource) CheckHealth(ctx context.Context, req *backend.CheckHealthRequest) (*backend.CheckHealthResult, error) {
  cookies:= req.GetHTTPHeader(backend.CookiesHeaderName)

  // ...
}
```

## 為登入的使用者轉發使用者標頭

啟用 [`send_user_header`](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/#send_user_header) 後，Grafana 會使用 `X-Grafana-User` 標頭將使用者標頭傳遞給插件。您也可以轉發此標頭以及[授權標頭](#forward-oauth-identity-for-the-logged-in-user)或[設定的 Cookie](#forward-cookies-for-the-logged-in-user)。

### QueryData

像這樣轉發 `QueryData` 標頭：

```go
func (ds *dataSource) QueryData(ctx context.Context, req *backend.QueryDataRequest) (*backend.QueryDataResponse, error) {
  u := req.GetHTTPHeader("X-Grafana-User")

  // ...
}
```

### CallResource

像這樣轉發 `CallResource` 標頭：

```go
func (ds *dataSource) CallResource(ctx context.Context, req *backend.CallResourceRequest, sender backend.CallResourceResponseSender) error {
  u := req.GetHTTPHeader("X-Grafana-User")

  // ...
}
```

### CheckHealth

像這樣轉發 `CheckHealth` 標頭：

```go
func (ds *dataSource) CheckHealth(ctx context.Context, req *backend.CheckHealthRequest) (*backend.CheckHealthResult, error) {
  u := req.GetHTTPHeader("X-Grafana-User")

  // ...
}
```
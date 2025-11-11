# 代理擴充功能

> [!WARNING]
> **Beta 功能 (自 2.7.0 起)**
>
> 此功能處於 [Beta](https://github.com/argoproj/argoproj/blob/main/community/feature-status.md#beta) 階段。
> 它通常被認為是穩定的，但可能存在未處理的邊緣案例。

## 概覽

透過 UI 擴充功能，可以增強 Argo CD 網頁介面，為使用者提供有價值的資料。然而，資料僅限於屬於應用程式的資源。透過代理擴充功能，還可以新增額外的功能，以存取後端服務提供的資料。在這種情況下，Argo CD API 伺服器充當反向代理，在轉發到後端服務之前對傳入的請求進行身份驗證和授權。

## 設定

由於代理擴充功能處於 [Alpha][1] 階段，該功能預設是停用的。要啟用它，需要在 Argo CD 命令參數中設定功能旗標。正確啟用此功能旗標的最簡單方法是在現有的 `argocd-cmd-params-cm` 中新增 `server.enable.proxy.extension` 鍵。例如：

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: argocd-cmd-params-cm
  namespace: argocd
data:
  server.enable.proxy.extension: 'true'
```

啟用代理擴充功能後，可以在主要的 Argo CD configmap ([argocd-cm][2]) 中進行設定。

以下範例展示了代理擴充功能所有可能的設定：

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: argocd-cm
  namespace: argocd
data:
  extension.config: |
    extensions:
    - name: httpbin
      backend:
        connectionTimeout: 2s
        keepAlive: 15s
        idleConnectionTimeout: 60s
        maxIdleConnections: 30
        services:
        - url: http://httpbin.org
          headers:
          - name: some-header
            value: '$some.argocd.secret.key'
          cluster:
            name: some-cluster
            server: https://some-cluster
```

代理擴充功能也可以使用專用的 Argo CD configmap 鍵單獨提供，以實現更好的 GitOps 操作。以下範例展示了如何使用專用鍵來設定上面假設的 httpbin 設定：

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: argocd-cm
  namespace: argocd
data:
  extension.config.httpbin: |
    connectionTimeout: 2s
    keepAlive: 15s
    idleConnectionTimeout: 60s
    maxIdleConnections: 30
    services:
    - url: http://httpbin.org
      headers:
      - name: some-header
        value: '$some.argocd.secret.key'
      cluster:
        name: some-cluster
        server: https://some-cluster
```

注意：擴充功能名稱在 Argo CD configmap 中必須是唯一的。如果發現重複的鍵，Argo CD API 伺服器將會記錄一條錯誤訊息，並且不會註冊任何代理擴充功能。

注意：修改 Argo CD configmap 中的 `extension.config` 項目後，無需重新啟動 Argo CD 伺服器。變更將會自動應用。將會建立一個新的代理註冊表，使得所有新的傳入擴充功能請求 (`<argocd-host>/extensions/*`) 都會遵守新的設定。

每個設定項目說明如下：

#### `extensions` (_list_)

定義所有已啟用擴充功能的設定。

#### `extensions.name` (_string_)

（強制性）

定義將用於註冊擴充功能路由的端點。例如，如果此屬性的值是 `extensions.name: my-extension`，那麼後端服務將在以下 url 下公開：

    <argocd-host>/extensions/my-extension

#### `extensions.backend.connectionTimeout` (_duration string_)

（可選。預設：2s）

是向擴充功能伺服器發起連線時，等待連線完成的最長時間。

#### `extensions.backend.keepAlive` (_duration string_)

（可選。預設：15s）

指定 API 伺服器和擴充功能伺服器之間作用中網路連線的 keep-alive 探測之間的時間間隔。

#### `extensions.backend.idleConnectionTimeout` (_duration string_)

（可選。預設：60s）

是 API 伺服器和擴充功能伺服器之間的閒置 (keep-alive) 連線在關閉自身之前將保持閒置的最長時間。

#### `extensions.backend.maxIdleConnections` (_int_)

（可選。預設：30）

控制 API 伺服器和擴充功能伺服器之間的閒置 (keep-alive) 連線的最大數量。

#### `extensions.backend.services` (_list_)

按叢集定義後端 url 的列表。

#### `extensions.backend.services.url` (_string_)

（強制性）

是擴充功能後端必須可用的位址。

#### `extensions.backend.services.headers` (_list_)

如果提供，標頭列表將被新增到此服務設定的所有傳出請求中。傳入請求中具有相同名稱的現有標頭將被此列表中的標頭覆寫。保留的標頭名稱將被忽略（請參閱下面的[傳入請求標頭](#incoming-request-headers)）。

#### `extensions.backend.services.headers.name` (_string_)

（強制性）

定義標頭的名稱。如果提供了標頭，則此欄位為強制性。

#### `extensions.backend.services.headers.value` (_string_)

（強制性）

定義標頭的值。如果提供了標頭，則此欄位為強制性。該值可以逐字提供，也可以作為對 Argo CD secret 密鑰的參考。為了將其作為參考提供，必須在其前面加上一個美元符號。

範例：

    value: '$some.argocd.secret.key'

在上面的範例中，該值將被替換為來自 argocd-secret 且密鑰為 'some.argocd.secret.key' 的值。

#### `extensions.backend.services.cluster` (_object_)

（可選）

如果提供，並且設定了多個服務，則必須與應用程式目的地名稱或伺服器匹配，才能將請求正確轉發到此服務 URL。如果同一擴充功能有多個後端，則此欄位是必需的。在這種情況下，有必要提供這兩個值，以避免應用程式無法將請求傳送到適當的後端服務的問題。如果只設定了一個後端服務，則此欄位將被忽略，所有請求都將轉發到已設定的服務。

#### `extensions.backend.services.cluster.name` (_string_)

（可選）

它將與 `Application.Spec.Destination.Name` 的值進行匹配。

#### `extensions.backend.services.cluster.server` (_string_)

（可選）

它將與 `Application.Spec.Destination.Server` 的值進行匹配。

## 用法

一旦設定了代理擴充功能，它將在 Argo CD API 伺服器公開的 `/extensions/<extension-name>` 端點下可用。上面的範例會將對 `<apiserver-host>/extensions/httpbin/` 的請求代理到 `http://httpbin.org`。

下圖說明了此設定可能的一種互動：

```
                                              ┌─────────────┐
                                              │ Argo CD UI  │
                                              └────┬────────┘
                                                   │  ▲
  GET <apiserver-host>/extensions/httpbin/anything │  │ 200 OK
            + authn/authz headers                  │  │
                                                   ▼  │
                                            ┌─────────┴────────┐
                                            │Argo CD API 伺服器│
                                            └──────┬───────────┘
                                                   │  ▲
                   GET http://httpbin.org/anything │  │ 200 OK
                                                   │  │
                                                   ▼  │
                                             ┌────────┴────────┐
                                             │ 後端服務        │
                                             └─────────────────┘
```

### 傳入請求標頭

請注意，Argo CD API 伺服器需要傳送額外的 HTTP 標頭，以便在代理到後端服務之前強制執行傳入請求是否經過身份驗證和授權。標頭如下所述：

#### `Cookie`

Argo CD UI 將身份驗證權杖儲存在一個 cookie (`argocd.token`) 中。此值需要在 `Cookie` 標頭中傳送，以便 API 伺服器可以驗證其真實性。

範例：

    Cookie: argocd.token=eyJhbGciOiJIUzI1Ni...

也可以傳送整個 Argo CD cookie 列表。在這種情況下，API 伺服器將只使用 `argocd.token` 屬性。

#### `Argocd-Application-Name` (強制性)

這是正在叫用擴充功能的應用程式的專案名稱。標頭值的格式必須為：
`"<namespace>:<app-name>"`。

範例：

    Argocd-Application-Name: namespace:app-name

#### `Argocd-Project-Name` (強制性)

登入的使用者必須有權存取此專案才能獲得授權。

範例：

    Argocd-Project-Name: default

Argo CD API 伺服器將確保登入的使用者有權存取上述標頭提供的資源。驗證基於預先設定的 [Argo CD RBAC 規則][3]。相同的標頭也會傳送到後端服務。後端服務也必須驗證已驗證的標頭是否與傳入請求的其餘部分相容。

### 傳出請求標頭

傳送到後端服務的請求將會被加上額外的標頭。傳出請求標頭如下所述：

#### `Argocd-Target-Cluster-Name`

如果應用程式資源中的 `app.Spec.Destination.Name` 不是空字串，則會填入該值。

#### `Argocd-Target-Cluster-URL`

如果應用程式資源中的 `app.Spec.Destination.Server` 不是空字串，則會填入該值。

請注意，可以將額外的預先設定的標頭新增到傳出請求中。有關更多詳細資訊，請參閱 [後端服務標頭](#extensionsbackendservicesheaders-list) 部分。

#### `Argocd-Username`

將會填入登入 Argo CD 的使用者名稱。這主要用於顯示目的。
要出於程式化需求識別使用者，`Argocd-User-Id` 可能是更好的選擇。

#### `Argocd-User-Id`

將會填入內部使用者 ID，通常由 `sub` 聲明定義，即登入 Argo CD 的使用者。

#### `Argocd-User-Groups`

將會填入從登入 Argo CD 的使用者設定的 RBAC 範圍，通常是 `groups` 聲明。

### 多後端使用案例

在某些情況下，當 Argo CD 設定為與多個遠端叢集同步時，可能需要呼叫每個叢集中的特定後端服務。可以透過為同一個擴充功能定義多個服務來設定代理擴充功能以解決此使用案例。請考慮以下設定作為範例：

```yaml
extension.config: |
  extensions:
  - name: some-extension
    backend:
      services:
      - url: http://extension-name.com:8080
        cluster
          name: kubernetes.local
      - url: https://extension-name.ppd.cluster.k8s.local:8080
        cluster
          server: user@ppd.cluster.k8s.local
```

在上面的範例中，API 伺服器將檢查應用程式目的地以驗證應使用哪個 URL 來代理傳入的請求。

## 安全性

當對 `/extensions/*` 的請求到達 API 伺服器時，它會首先驗證它是否已使用有效的權杖進行身份驗證。它透過檢查 `Cookie` 標頭是否已從 Argo CD UI 擴充功能正確傳送來實現此目的。

一旦請求被驗證，接著會驗證使用者是否有權限叫用此擴充功能。該權限由 Argo CD RBAC 設定強制執行。有關如何為代理擴充功能設定 RBAC 的詳細資訊，可以在 [RBAC 文件][3] 頁面中找到。

一旦請求由 API 伺服器進行身份驗證和授權，它會在傳送到後端服務之前進行清理。請求清理將從請求中移除敏感資訊，例如 `Cookie` 和 `Authorization` 標頭。

可以透過在 `extensions.backend.services.headers` 設定中將其定義為標頭，將新的 `Authorization` 標頭新增到傳出請求中。請考慮以下範例：

```yaml
extension.config: |
  extensions:
  - name: some-extension
    backend:
      services:
      - url: http://extension-name.com:8080
        headers:
        - name: Authorization
          value: '$some-extension.authorization.header'
```

在上面的範例中，所有傳送到
`http://extension-name.com:8080` 的請求都將有一個額外的
`Authorization` 標頭。此標頭的值將是來自
[argocd-secret](../../operator-manual/argocd-secret-yaml.md) 且密鑰為 `some-extension.authorization.header` 的值。

[1]: https://github.com/argoproj/argoproj/blob/master/community/feature-status.md
[2]: https://argo-cd.readthedocs.io/en/stable/operator-manual/argocd-cm.yaml
[3]: ../../operator-manual/rbac.md#the-extensions-resource

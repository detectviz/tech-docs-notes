---
id: add-support-for-pdc
title: 新增對私有資料來源連線 (PDC) 的支援
description: 為您的 Grafana 插件新增對私有資料來源連線 (PDC) 功能的支援。
keywords:
  - grafana
  - plugins
  - plugin
  - pdc
  - private data source connect
---

# 新增對私有資料來源連線 (PDC) 的支援

## 什麼是私有資料來源連線？

私有資料來源連線 (PDC) 是一種在 Grafana Cloud 執行個體或堆疊與私有網路中受保護的資料來源之間建立私密、安全連線的方式。

深入了解[私有資料來源連線](https://grafana.com/docs/grafana-cloud/connect-externally-hosted/private-data-source-connect/)。

### 何時不支援 PDC

PDC 是僅限 Grafana Cloud 的解決方案，因此如果您的資料來源在 Grafana Cloud 中不可用，則實作 PDC 支援的好處不大。

# 為資料來源新增 PDC

每個資料來源都必須整合 PDC 支援，因為每個 Grafana 插件都負責建立自己與目標資料來源的連線。雖然 Grafana 將代理設定詳細資料（例如 `proxy_address`、`server_address` 和憑證）儲存在其設定中，但每個插件以不同的方式使用此設定。

[`grafana-plugin-sdk-go`](https://github.com/grafana/grafana-plugin-sdk-go) 提供了一個 `httpClientProvider`，它會自動使用代理設定，讓使用插件 SDK 中 HTTP 用戶端的插件更容易實作 PDC 支援。但是，使用其他類型用戶端的插件需要更多手動調整才能使用代理設定。

## 先決條件

:::note

注意：無法為前端資料來源新增 PDC 支援，因為與代理的連線是從後端建立的。
:::

- 需要 [`grafana-plugin-sdk`](https://github.com/grafana/grafana-plugin-sdk-go) 版本 > [`0.229.0`](https://github.com/grafana/grafana-plugin-sdk-go/releases/tag/v0.229.0) 才能取得與遠端插件相容的最新變更。請保持此 SDK 的更新以取得最新的相容變更。
- Grafana 版本 > `10.0.0`

## 概觀

以下是您需要為資料來源插件新增 PDC 所需步驟的簡要概觀：

- 更新插件前端設定區段以包含啟用 SOCKS 代理的切換開關
- 更新插件後端以在設定為這樣做時使用 SOCKS 代理傳輸
- 測試並發布資料來源
- 在 Grafana Cloud 中為資料來源啟用 PDC
- 更新公開文件

## 前端變更

在前端，我們需要新增一個功能切換開關以啟用或停用 PDC。

在通常定義於 `src/types.ts` 的設定介面中新增一個屬性 `enableSecureSocksProxy?: boolean;`，如下所示：

```javascript
export interface MyDataSourceOptions extends DataSourceJsonData {
  path?: string;
  // 新增此行
  enableSecureSocksProxy?: boolean;
}
```

然後在 `components/ConfigEditor.tsx` 中新增一個選項，以便能夠使用切換開關控制此值。它需要新增一些檢查以確保：

- 已設定功能切換開關 `secureSocksDSProxyEnabled`
- Grafana 版本大於 10.0.0

例如，以下是在 Infinity 資料來源中的作法：

```javascript
import { gte } from 'semver';
...
  {config.featureToggles['secureSocksDSProxyEnabled' as keyof FeatureToggles] &&
        gte(config.buildInfo.version, '10.0.0') && (
          <>
            <InlineField
              label="Secure SOCKS Proxy"
              tooltip={
                <>
                  Enable proxying the data source connection through the secure SOCKS proxy to a
                  different network.
                  See{' '}
                  <a
                    href="https://grafana.com/docs/grafana/next/setup-grafana/configure-grafana/proxy/"
                    target="_blank"
                    rel="noopener noreferrer"
                  >
                    Configure a data source connection proxy.
                  </a>
                </>
              }
            >
              <div className={styles.toggle}>
                <Switch
                  value={options.jsonData.enableSecureSocksProxy}
                  onChange={(e) => {
                    onOptionsChange({
                      ...options,
                      jsonData: {
                        ...options.jsonData,
                        enableSecureSocksProxy: e.currentTarget.checked
                      },
                    });
                  }}
                />
              </div>
            </InlineField>
          </>
        )}
```

![資料來源設定畫面中的 PDC 切換選項](./images/pdc-image-editor.png)

## 後端變更

根據插件如何連線至資料來源，有幾種方法可以新增 PDC 支援：

### HTTP 用戶端：使用 grafana-plugin-sdk HTTP 用戶端

此範例涉及使用 grafana-plugin-sdk 儲存庫中 [`http.Client`](https://github.com/grafana/grafana-plugin-sdk-go/blob/main/backend/httpclient/http_client.go#L21) 的插件。由於此 HTTP 用戶端已管理安全 SOCKS 代理，您只需設定其 [transport](https://github.com/grafana/grafana-plugin-sdk-go/blob/main/backend/httpclient/http_client.go#L27) 以套用代理設定。請考慮以下由 [developers/plugin-tools](https://grafana.com/developers/plugin-tools/) 產生的程式碼：

- `HTTPClientOptions(ctx)` 根據從 Grafana 程序傳遞的上下文讀取並建立 HTTP 用戶端設定。
- `httpclient.New(opts)` 呼叫 `GetTransport()`。[Transport](https://github.com/grafana/grafana-plugin-sdk-go/blob/main/backend/httpclient/http_client.go#L90-L94) 物件負責處理 [標準套件](https://pkg.go.dev/net/http#hdr-Clients_and_Transports)中的 TLS、代理和其他設定。
- `GetTransport()` 使用 `ConfigureSecureSocksHTTPProxy()` 將 `Transport` 物件包裝成具有 TLS 的 SOCKS5 代理
- `ConfigureSecureSocksHTTPProxy()` 呼叫 `NewSecureSocksProxyContextDialer()`，它會建立一個 SOCKS 代理撥號器。

這將透過代理（並因此透過 PDC）代理來自用戶端的每個請求，然後到達資料來源。

```go
func NewDatasource(ctx context.Context, s backend.DataSourceInstanceSettings) (instancemgmt.Instance, error) {
    opts, err := s.HTTPClientOptions(ctx)
    if err != nil {
        return nil, err
    }
    httpClient, err := httpclient.New(opts)
    if err != nil {
        return nil, err
    }
    return &Datasource{HTTPClient: httpClient}, nil
}

```

以下是一些針對某些資料來源的作法範例：

- [BigQuery](https://github.com/grafana/google-bigquery-datasource/pull/193/) - 在這些情況下，使用外部函式庫連線至資料來源，但它允許交換預設的 HTTP 用戶端。因此，我們只需傳遞 SDK 中的用戶端即可設定 PDC 代理。
- [VictoriaMetrics Metrics Data Source](https://github.com/VictoriaMetrics/victoriametrics-datasource/releases/tag/v0.15.1)

### 非 HTTP 用戶端

我們這裡有幾個選項，取決於資料來源程式碼庫中可用的內容：

#### 覆寫撥號器

有些套件提供了一種為用戶端傳輸設定 [Dialer](https://pkg.go.dev/golang.org/x/net/proxy#Dialer) 的方法。使用 `NewSecureSocksProxyContextDialer` 傳回的撥號器來設定用戶端中的 `dialer`。

覆寫撥號器比覆寫整個傳輸的風險較小，因為我們變更的東西較少。

例如，我們的 PostgreSQL 用戶端允許將預設撥號器替換為支援安全 SOCKS 代理的撥號器（[程式碼連結](https://github.com/grafana/grafana/blob/da24ad06bd90b6caeaa7ad553e0063f62b0b6c5c/pkg/tsdb/grafana-postgresql-datasource/postgres.go#L71-L80)）：

```go
if proxyClient.SecureSocksProxyEnabled() {
    socksDialer, err := proxyClient.NewSecureSocksProxyContextDialer()
    if err != nil {
        logger.Error("postgres proxy creation failed", "error", err)
        return nil, nil, fmt.Errorf("postgres proxy creation failed")
    }

    d := newPostgresProxyDialer(socksDialer)

    // update the postgres dialer with the proxy dialer
    connector.Dialer(d)
}
```

其他範例 PR：
[MySQL](https://github.com/grafana/grafana/blame/da24ad06bd90b6caeaa7ad553e0063f62b0b6c5c/pkg/tsdb/mysql/mysql.go#L92)

#### 覆寫傳輸

檢查我們是否可以設定 `Transport` 物件（通常是 `http.RoundTripper`）。然後就可以使用 `httpclient.GetTransport(opts ...Options)` 來取得 `Transport` 物件並在函式庫或連接器中設定它。

## 驗證變更是否正常運作

### 使用 Grafana Cloud 執行個體進行測試

**先決條件：** 如果您還沒有 Grafana Cloud 執行個體可供測試，請先[註冊](https://grafana.com/docs/grafana-cloud/get-started/#sign-up-for-a-grafana-cloud-account)一個免費的 Grafana Cloud 帳戶執行個體。

新增 PDC 支援後，在您希望將您的插件發佈到我們的目錄之前，我們要求您在 Grafana Cloud 中驗證它是否如預期般運作，以確保我們的和您的客戶都有最佳體驗。

1. 建立一個具有 PDC 支援的資料來源插件的現成可用版本
2. 透過 `integrations+pdc@grafana.com` 與我們聯繫，將您的插件版本以及您希望在哪個 grafana cloud 執行個體和組織上執行此插件以進行測試傳送給我們。
3. 我們會為您的執行個體佈建此插件版本，並通知您可以進行測試
4. 一旦測試並確認，您就可以繼續進行常規發布並提交審核

### 使用 microsocks 在本地模擬/測試

在這種情況下，我們將使用 [microsocks](https://github.com/rofl0r/microsocks)，這是一個開源的 SOCKS 伺服器。這與 Grafana 在我們的雲端中的執行方式不同，但它展示了一種無需任何內部相依性的輕量級方法。

執行此方法的步驟：

1. 安裝 [microsocks](https://github.com/rofl0r/microsocks)。對於 macOS，可以使用 \`brew install microsocks\`
2. 執行 microsocks \-i 127.0.0.1 \-p 5555。這將啟動 SOCKS 伺服器並等待連線。
3. 使用此設定執行 Grafana。

   ```shell
   [feature_toggles]
   enable = secureSocksDSProxyEnabled

   [secure_socks_datasource_proxy]
   enabled = true
   proxy_address = localhost:5555
   allow_insecure = true
   ```

4. 建立您要測試的資料來源的新執行個體。您應該會看到「Secure SOCKS Proxy」設定區段，其中有一個啟用它的切換開關。
5. 啟用後，按一下「儲存並測試」，Grafana 會顯示一則訊息，表示連線成功。
6. 在 microsocks 記錄中，您應該會看到類似 `client[5] 127.0.0.1: connected to <target>` 的內容，這表示連線是透過伺服器建立的。
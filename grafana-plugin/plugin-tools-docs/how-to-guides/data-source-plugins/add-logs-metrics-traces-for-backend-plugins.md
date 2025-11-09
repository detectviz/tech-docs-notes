---
id: add-logs-metrics-traces-for-backend-plugins
title: 為插件後端新增日誌、指標和追蹤
description: 如何為插件後端元件新增日誌、指標和追蹤。
keywords:
  - grafana
  - plugins
  - plugin
  - instrument
  - instrumentation
  - logs
  - metrics
  - distributed tracing
  - tracing
  - backend
  - back-end
---

為插件後端元件新增[日誌](#logs)、[指標](#metrics)和[追蹤](#traces)，可讓插件開發人員和 Grafana 操作員更容易診斷和解決問題。本文件提供指引、慣例和最佳實務，以協助您有效地檢測您的插件，以及如何在插件安裝後存取這些資料。

:::note

本文件期望您至少使用 [grafana-plugin-sdk-go v0.246.0](https://github.com/grafana/grafana-plugin-sdk-go/releases/tag/v0.246.0)。但是，建議您保持 Grafana plugin SDK for Go 的更新，以取得最新的改進、安全性和錯誤修正。有關更新說明，請參閱[更新 Go SDK](../../key-concepts/backend-plugins/grafana-plugin-sdk-for-go.md#update-the-go-sdk)。

:::

## 日誌

日誌是記錄軟體環境中發生的事件、警告和錯誤的檔案。大多數日誌都包含上下文資訊，例如事件發生的時間以及與之相關的使用者或端點。

### SDK 的自動檢測

SDK 會自動化某些檢測，以簡化開發人員和操作員的體驗。每次方法呼叫（`QueryData`、`CallResource`、`CheckHealth` 等）完成後，都會記錄一則訊息 `Plugin Request Completed`。此外，如果 `QueryData` 回應包含任何錯誤，則會為每個資料回應錯誤記錄一則訊息 `Partial data response error`。以下是一些記錄訊息的範例：

```shell
DEBUG[09-05|17:24:16] Plugin Request Completed  logger=plugin.grafana-test-datasource dsUID=edeuvt04gim0we endpoint=queryData pluginID=grafana-test-datasource statusSource=plugin uname=admin dsName=grafana-test-datasource traceID=604e15b6345c2c0896e6902fa86b82f5 duration=1.482975875s status=ok
DEBUG[09-05|18:24:16] Plugin Request Completed  logger=plugin.grafana-test-datasource dsUID=edeuvt04gim0we endpoint=queryData pluginID=grafana-test-datasource statusSource=plugin uname=admin dsName=grafana-test-datasource traceID=604e15b6345c2c0896e6902fa86b82f5 duration=1.482975875s status=cancelled error=context.Canceled error
ERROR[09-05|19:24:16] Plugin Request Completed  logger=plugin.grafana-test-datasource dsUID=edeuvt04gim0we endpoint=queryData pluginID=grafana-test-datasource statusSource=plugin uname=admin dsName=grafana-test-datasource traceID=604e15b6345c2c0896e6902fa86b82f5 duration=1.482975875s status=error error=something is not working as expected
ERROR[09-06|15:29:47] Partial data response error   logger=plugin.grafana-test-datasource status=500.000 statusSource=plugin dsName=grafana-test-datasource dsUID=edeuvt04gim0we endpoint=queryData refID=A error="no handler found for query type 'noise'" pluginID=grafana-test-datasource traceID=981b7761aa295e371757582c7a4043d1 uname=admin
```

### 在您的插件中實作記錄

使用[後端套件](https://pkg.go.dev/github.com/grafana/grafana-plugin-sdk-go/backend)中的全域記錄器 `backend.Logger`，在任何地方都適用，且適用於大多數使用案例。

**範例：**

以下範例展示了使用不同嚴重性等級和鍵值對的全域記錄器的基本用法。

```go
package plugin

import (
    "errors"

    "github.com/grafana/grafana-plugin-sdk-go/backend"
)

func main() {
    backend.Logger.Debug("Debug msg", "someID", 1)
    backend.Logger.Info("Info msg", "queryType", "default")
    backend.Logger.Warning("Warning msg", "someKey", "someValue")
    backend.Logger.Error("Error msg", "error", errors.New("An error occurred"))
}
```

上述範例會輸出類似以下的內容。

```shell
DEBUG[11-14|15:26:26] Debug msg     logger=plugin.grafana-basic-datasource someID=1
INFO [11-14|15:26:26] Info msg      logger=plugin.grafana-basic-datasource queryType=default
WARN [11-14|15:26:26] Warning msg   logger=plugin.grafana-basic-datasource someKey=someValue
ERROR[11-14|15:26:26] Error msg     logger=plugin.grafana-basic-datasource error=An error occurred
```

:::note

`backend.Logger` 是[日誌套件](https://pkg.go.dev/github.com/grafana/grafana-plugin-sdk-go/backend/log)中 `log.DefaultLogger` 的一個方便的包裝器，您也可以使用它來存取全域記錄器。

:::

#### 重複使用具有特定鍵值對的記錄器

您可以記錄多則訊息並包含特定的鍵值對，而無需在各處重複您的程式碼，例如，當您想要根據資料來源在每個日誌訊息中的設定方式包含一些特定的鍵值對時。為此，請使用您實例化記錄器上的 `With` 方法建立一個帶有引數的新記錄器。

**範例：**

以下範例說明如何為每個[資料來源執行個體](../../key-concepts/plugin-types-usage#usage-of-data-source-plugins)實例化一個記錄器，並使用 `With` 方法在此資料來源執行個體的生命週期內包含特定的鍵值對。

```go
package plugin

import (
    "context"
    "errors"

    "github.com/grafana/grafana-plugin-sdk-go/backend"
    "github.com/grafana/grafana-plugin-sdk-go/backend/instancemgmt"
)

func NewDatasource(ctx context.Context, settings backend.DataSourceInstanceSettings) (instancemgmt.Instance, error) {
    logger := backend.Logger.With("key", "value")

    return &Datasource{
        logger: logger,
    }, nil
}

func (ds *Datasource) QueryData(ctx context.Context, req *backend.QueryDataRequest) (*backend.QueryDataResponse, error) {
    ds.logger.Debug("QueryData", "queries", len(req.Queries))
}
```

每次呼叫 `QueryData` 時，上述範例都會輸出類似以下的內容。

```shell
DEBUG[11-14|15:26:26] QueryData     logger=plugin.grafana-basic-datasource key=value queries=2
```

:::note

您也可以使用[後端套件](https://pkg.go.dev/github.com/grafana/grafana-plugin-sdk-go/backend)中的 `backend.NewLoggerWith`，這是一個呼叫[日誌套件](https://pkg.go.dev/github.com/grafana/grafana-plugin-sdk-go/backend/log)中 `log.New().With(args...)` 的輔助方法。

:::

#### 使用上下文記錄器

使用上下文記錄器以自動包含附加到 `context.Context` 的額外鍵值對。例如，您可以使用 `traceID` 來將日誌與追蹤關聯，並將日誌與一個通用識別碼關聯。您可以透過在您實例化的記錄器上使用 `FromContext` 方法來建立一個新的上下文記錄器；您也可以在[重複使用具有特定鍵值對的記錄器](#reuse-logger-with-certain-keyvalue-pairs)時結合使用此方法。我們建議您在有權存取 `context.Context` 時使用上下文記錄器。

預設情況下，使用上下文記錄器時，以下鍵值對會包含在日誌中：

- **pluginID：** 插件識別碼。例如，`grafana-github-datasource`。
- **endpoint：** 正在處理的請求；即 `callResource`、`checkHealth`、`collectMetrics`、`queryData`、`runStream`、`subscribeStream` 或 `publishStream`。
- **traceID：** 如果可用，則包含分散式追蹤識別碼。
- **dsName：** 如果可用，則為已設定的資料來源執行個體的名稱。
- **dsUID：** 如果可用，則為已設定的資料來源執行個體的唯一識別碼 (UID)。
- **uname：** 如果可用，則為發出請求的使用者的使用者名稱。

**範例：**

以下範例擴充了[重複使用具有特定鍵值對的記錄器](#reuse-logger-with-certain-keyvalue-pairs)範例，以包含上下文記錄器的用法。

```go
package plugin

import (
    "context"
    "errors"

    "github.com/grafana/grafana-plugin-sdk-go/backend"
    "github.com/grafana/grafana-plugin-sdk-go/backend/instancemgmt"
)

func NewDatasource(ctx context.Context, settings backend.DataSourceInstanceSettings) (instancemgmt.Instance, error) {
    logger := backend.Logger.With("key", "value")

    return &Datasource{
        logger:   logger,
    }, nil
}

func (ds *Datasource) QueryData(ctx context.Context, req *backend.QueryDataRequest) (*backend.QueryDataResponse, error) {
    ctxLogger := ds.logger.FromContext(ctx)
    ctxLogger.Debug("QueryData", "queries", len(req.Queries))
}
```

每次使用 2 個查詢呼叫 `QueryData` 時，上述範例都會輸出類似以下的內容。

```shell
DEBUG[11-14|15:26:26] QueryData     logger=plugin.grafana-basic-datasource pluginID=grafana-basic-datasource endpoint=queryData traceID=399c275ebb516a53ec158b4d0ddaf914 dsName=Basic datasource dsUID=kXhzRl7Mk uname=admin key=value queries=2
```

#### 在日誌中包含額外的上下文資訊

如果您想將額外的上下文鍵值對傳播到後續的程式碼/邏輯中，您可以使用 [log.WithContextualAttributes](https://pkg.go.dev/github.com/grafana/grafana-plugin-sdk-go/backend/log#WithContextualAttributes) 函式。

**範例：**

以下範例擴充了[使用上下文記錄器](#use-a-contextual-logger)範例，透過新增額外的上下文鍵值對並允許將這些傳播到其他方法 (`handleQuery`) 來使用 `log.WithContextualAttributes` 函式。

```go
package plugin

import (
    "context"
    "errors"

    "github.com/grafana/grafana-plugin-sdk-go/backend"
    "github.com/grafana/grafana-plugin-sdk-go/backend/instancemgmt"
    "github.com/grafana/grafana-plugin-sdk-go/backend/log"
)

func NewDatasource(ctx context.Context, settings backend.DataSourceInstanceSettings) (instancemgmt.Instance, error) {
    logger := backend.Logger.With("key", "value")

    return &Datasource{
        logger: logger,
    }, nil
}

func (ds *Datasource) QueryData(ctx context.Context, req *backend.QueryDataRequest) (*backend.QueryDataResponse, error) {
    ctxLogger := ds.logger.FromContext(ctx)
    ctxLogger.Debug("QueryData", "queries", len(req.Queries))

    for _, q := range req.Queries {
        childCtx = log.WithContextualAttributes(ctx, []any{"refID", q.RefID, "queryType", q.QueryType})
        ds.handleQuery(childCtx, q)
    }
}

func (ds *Datasource) handleQuery(ctx context.Context, q backend.DataQuery) {
    ctxLogger := ds.logger.FromContext(ctx)
    ctxLogger.Debug("handleQuery")
}
```

每次使用 2 個查詢呼叫 `QueryData` 時，上述範例都會輸出類似以下的內容。

```shell
DEBUG[11-14|15:26:26] QueryData     logger=plugin.grafana-basic-datasource pluginID=grafana-basic-datasource endpoint=queryData traceID=399c275ebb516a53ec158b4d0ddaf914 dsName=Basic datasource dsUID=kXhzRl7Mk uname=admin queries=2
DEBUG[11-14|15:26:26] handleQuery   logger=plugin.grafana-basic-datasource pluginID=grafana-basic-datasource endpoint=queryData traceID=399c275ebb516a53ec158b4d0ddaf914 dsName=Basic datasource dsUID=kXhzRl7Mk uname=admin refID=A queryType=simpleQuery
DEBUG[11-14|15:26:26] handleQuery   logger=plugin.grafana-basic-datasource pluginID=grafana-basic-datasource endpoint=queryData traceID=399c275ebb516a53ec158b4d0ddaf914 dsName=Basic datasource dsUID=kXhzRl7Mk uname=admin refID=B queryType=advancedQuery
```

### 最佳實務

- 日誌訊息以大寫字母開頭；例如，`logger.Info("Hello world")` 而非 `logger.Info("hello world")`。
- 日誌訊息應作為日誌條目的識別碼，盡量避免參數化；例如，`logger.Debug(fmt.Sprintf(“Something happened, got argument %d”, “arg”))`，而應使用鍵值對來提供額外資料；例如，`logger.Info(“Something happened”, “argument”, “arg”)`。
- 在命名日誌鍵時，偏好使用駝峰式命名法；例如，`remoteAddr` 或 `userID`，以與 Go 識別碼保持一致。
- 記錄 Go 錯誤時，請使用 `error` 鍵；例如，`logger.Error("Something failed", "error", errors.New("An error occurred")`。
- 只要有權存取 `context.Context`，就使用上下文記錄器。
- 請勿記錄敏感資訊，例如資料來源憑證或 IP 位址，或其他個人可識別資訊。

#### 驗證和清理來自使用者輸入的資料

如果日誌訊息或鍵值對源自使用者輸入，則應對其進行驗證和清理。請注意不要在日誌訊息中暴露任何敏感資訊（機密、憑證等）。在將 Go 結構作為值包含時，尤其容易出錯。

如果源自使用者輸入的值是有界的，即存在一組固定的預期值，建議驗證其是否為這些值之一，否則傳回錯誤。

如果源自使用者輸入的值是無界的，即值可以是任何內容，建議驗證值的最大長度/大小並傳回錯誤，或透過僅允許特定數量/固定字元集來進行清理。

#### 何時使用哪個日誌等級？

- **Debug：** 正常操作期間的高頻率資訊性訊息和較不重要的訊息。
- **Info：** 低頻率的資訊性訊息和重要訊息。
- **Warning：** 可在不中斷操作的情況下恢復的錯誤/狀態。如果使用，應具有可操作性，以便操作員可以採取措施解決它。
- **Error：** 表示某些操作失敗（出現錯誤）且程式無法處理該錯誤的錯誤訊息。

:::note

對於 `QueryData` 端點，高頻率的傳入請求通常更常見，因為例如，儀表板的性質會為每個面板或查詢產生一個請求。

:::

### 在本地檢查日誌

來自插件後端的日誌會被連接的 Grafana 執行個體消耗，並包含在 Grafana 伺服器日誌中。

來自插件後端元件的每個日誌訊息都包含一個記錄器名稱 `logger=plugin.<plugin id>`。例如：

```shell
DEBUG[11-14|15:26:26] Debug msg     logger=plugin.grafana-basic-datasource someID=1
INFO [11-14|15:26:26] Info msg      logger=plugin.grafana-basic-datasource queryType=default
WARN [11-14|15:26:26] Warning msg   logger=plugin.grafana-basic-datasource someKey=someValue
ERROR[11-14|15:26:26] Error msg     logger=plugin.grafana-basic-datasource error=An error occurred
```

您可以在您的 Grafana 執行個體中啟用[偵錯記錄](https://grafana.com/docs/grafana/latest/troubleshooting/#troubleshoot-with-logs)，這通常會輸出大量資訊，使得難以找到與特定插件相關的日誌。但是，使用具名記錄器可以方便地僅為特定的具名記錄器和插件啟用偵錯記錄：

```
[log]
filters = plugin.<plugin id>:debug
```

有關設定記錄的更多詳細資訊，請參閱[設定 Grafana](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/#log)。

此外，請參閱[如何收集和視覺化日誌、指標和追蹤](#collect-and-visualize-logs-metrics-and-traces)。

## 指標

指標是可量化的測量值，反映了應用程式或基礎設施的健康狀況和效能。

考慮使用指標來提供對資源狀態的即時洞察。如果您想知道您的插件的回應速度如何，或識別可能是效能問題早期跡象的異常情況，指標是可見性的關鍵來源。

### 指標類型

Prometheus 支援四種不同的指標類型，您可以使用：

- **Counter：** 只能增加或在重新啟動時重設為零。例如，您可以使用計數器來表示已服務的請求數、已完成的任務數或錯誤數。
- **Gauge：** 可以任意上升和下降的數值。例如，您可以使用儀表來表示溫度或目前的記憶體使用量。
- **Histogram：** 對觀察值（通常是請求持續時間或回應大小之類的東西）進行取樣，並將其計入可設定的 bucket 中。它還提供所有觀察值的總和。
- **Summary：** 與直方圖類似，摘要對觀察值（通常是請求持續時間和回應大小之類的東西）進行取樣。雖然它也提供觀察值的總數和所有觀察值的總和，但它會在一個滑動的時間視窗內計算可設定的分位數。

有關您可以使用的不同指標類型以及何時使用的列表和詳細說明，請參閱 [Prometheus 指標類型](https://prometheus.io/docs/concepts/metric_types/)。

### SDK 的自動檢測

SDK 會自動化某些檢測，以簡化開發人員和操作員的體驗。本節探討預設收集和公開的指標。

#### Go 執行階段指標

SDK 提供 Go 執行階段、CPU、記憶體和程序指標的自動收集和公開，以簡化開發人員和操作員的體驗。這些指標在 `go_` 和 `process_` 命名空間下公開，僅舉幾例：

- `go_info`：有關 Go 環境的資訊。
- `go_memstats_alloc_bytes`：已分配且仍在使用中的位元組數。
- `go_goroutines`：目前存在的 goroutine 數。
- `process_cpu_seconds_total`：以秒為單位花費的總使用者和系統 CPU 時間。

有關您的插件自動收集和公開哪些指標的更多詳細資訊和最新列表，建議呼叫 Grafana 的 HTTP API `/api/plugins/:pluginID/metrics`。另請參閱[在本地收集和視覺化指標](#collect-and-visualize-metrics-locally)以取得有關如何將指標提取到 Promethus 的進一步說明。

#### 請求指標

SDK 提供一個名為 `grafana_plugin_request_total` 的新計數器指標的自動收集和公開，允許按端點（`QueryData`、`CallResource`、`CheckHealth` 等）、`status`（ok、cancelled、error）、`status_source`（plugin、downstream）追蹤插件請求的成功率。透過呼叫 Grafana HTTP API `/api/plugins/:pluginID/metrics` 的指標輸出範例：

```shell
# HELP grafana_plugin_request_total 插件請求總數
# TYPE grafana_plugin_request_total counter
grafana_plugin_request_total{endpoint="queryData",status="error",status_source="plugin"} 1
grafana_plugin_request_total{endpoint="queryData",status="ok",status_source="plugin"} 4
```

### 在您的插件中實作指標

[Grafana plugin SDK for Go](../../key-concepts/backend-plugins/grafana-plugin-sdk-for-go) 使用 [Prometheus Go 應用程式檢測函式庫](https://github.com/prometheus/client_golang)。向[預設註冊表](https://pkg.go.dev/github.com/prometheus/client_golang/prometheus#pkg-variables)註冊的任何自訂指標都將被 SDK 擷取，並透過[收集指標功能](../../key-concepts/backend-plugins/#collect-metrics)公開。

為方便起見，建議在建立自訂指標時使用 [promauto 套件](https://pkg.go.dev/github.com/prometheus/client_golang/prometheus/promauto)，因為它會自動在[預設註冊表](https://pkg.go.dev/github.com/prometheus/client_golang/prometheus#pkg-variables)中註冊指標並將其公開給 Grafana。

**範例：**

以下範例顯示如何定義和使用一個名為 `grafana_plugin_queries_total` 的自訂計數器指標，該指標按查詢類型追蹤查詢總數。

```go
package plugin

import (
    "context"

    "github.com/prometheus/client_golang/prometheus"
    "github.com/prometheus/client_golang/prometheus/promauto"

    "github.com/grafana/grafana-plugin-sdk-go/backend"
)

var queriesTotal = promauto.NewCounterVec(
    prometheus.CounterOpts{
        Namespace: "grafana_plugin",
        Name:      "queries_total",
        Help:      "Total number of queries.",
    },
    []string{"query_type"},
)

func (ds *Datasource) QueryData(ctx context.Context, req *backend.QueryDataRequest) (*backend.QueryDataResponse, error) {
    for _, q := range req.Queries {
        queriesTotal.WithLabelValues(q.QueryType).Inc()
    }
}
```

### 最佳實務

- 考慮使用 `grafana_plugin` 命名空間，因為這會為任何定義的指標名稱加上 `plugin` 前綴。這將使操作員清楚地知道任何名為 `grafana_plugin` 的指標都源自 Grafana 插件。
- 在命名指標時，請使用蛇形命名法，例如 `http_request_duration_seconds` 而非 `httpRequestDurationSeconds`。
- 在命名指標標籤時，請使用蛇形命名法，例如 `status_code` 而非 `statusCode`。
- 如果指標類型是計數器，請以 `_total` 後綴命名，例如 `http_requests_total`。
- 如果指標類型是直方圖且您正在測量持續時間，請以 `_<unit>` 後綴命名，例如 `http_request_duration_seconds`。
- 如果指標類型是儀表，請將其命名為表示它可以增加和減少的值，例如 `http_request_in_flight`。

#### 驗證和清理來自使用者輸入的資料

如果標籤值源自使用者輸入，則應對其進行驗證和清理。僅允許預定義的標籤集非常重要，以將高基數問題的風險降至最低。使用或允許過多的標籤值可能會導致高基數問題。例如，使用使用者 ID、電子郵件地址或其他無界值集作為標籤，很容易產生高基數問題，並導致 Prometheus 中出現大量時間序列。有關標籤和高基數的更多資訊，請參閱 [Prometheus 標籤命名](https://prometheus.io/docs/practices/naming/#labels)。

請注意不要在標籤值中暴露任何敏感資訊（機密、憑證等）。

如果源自使用者輸入的值是有界的，即存在一組固定的預期值，建議驗證其是否為這些值之一，否則傳回錯誤。

如果源自使用者輸入的值是無界的，即值可以是任何內容，由於前面提到的高基數問題，通常不建議將其用作標籤。如果仍然需要，建議驗證值的最大長度/大小並傳回錯誤，或透過僅允許特定數量/固定字元集來進行清理。

### 在本地收集和視覺化指標

請參閱[將指標從 Grafana 插件後端提取到 Prometheus](https://grafana.com/docs/grafana/latest/setup-grafana/set-up-grafana-monitoring/#pull-metrics-from-grafana-backend-plugin-into-prometheus)。

此外，請參閱[如何收集和視覺化日誌、指標和追蹤](#collect-and-visualize-logs-metrics-and-traces)。

## 追蹤

分散式追蹤允許插件後端開發人員在其插件中建立自訂 span，然後將它們傳送到與主 Grafana 執行個體相同的端點並使用相同的傳播格式。追蹤上下文也會從 Grafana 執行個體傳播到插件，因此插件的 span 將與正確的追蹤相關聯。

### Grafana 中的 OpenTelemetry 設定

Grafana 支援 [OpenTelemetry](https://opentelemetry.io/) 進行分散式追蹤。如果 Grafana 設定為使用已棄用的追蹤系統（Jaeger 或 OpenTracing），則 SDK 提供的插件中的追蹤將被停用，並在呼叫 `datasource.Manage | app.Manage` 時進行設定。

必須為 Grafana 執行個體啟用和設定 OpenTelemetry。有關更多資訊，請參閱[設定 Grafana](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana#tracingopentelemetry)。

有關 OpenTelemetry 提供的所有功能的深入文件，請參閱 [OpenTelemetry Go SDK](https://pkg.go.dev/go.opentelemetry.io/otel)。

:::note

如果在 Grafana 中停用追蹤，`backend.DefaultTracer()` 將傳回一個無操作的追蹤器。

:::

### 在您的插件中實作追蹤

當在主 Grafana 執行個體上啟用 OpenTelemetry 追蹤並為插件啟用追蹤時，OpenTelemetry 端點位址和傳播格式會在啟動期間傳遞給插件。這些參數用於設定全域追蹤器。

1. 使用 `datasource.Manage` 或 `app.Manage` 來執行您的插件以自動設定全域追蹤器。使用 `CustomAttributes` 為預設追蹤器指定任何自訂屬性：

   ```go
   func main() {
       if err := datasource.Manage("MY_PLUGIN_ID", plugin.NewDatasource, datasource.ManageOpts{
           TracingOpts: tracing.Opts{
               // 可選的自訂屬性附加到追蹤器的資源。
               // 追蹤器將已預先填入一些 SDK 和執行階段屬性。
               CustomAttributes: []attribute.KeyValue{
                   attribute.String("my_plugin.my_attribute", "custom value"),
               },
           },
       }); err != nil {
           log.DefaultLogger.Error(err.Error())
           os.Exit(1)
       }
   }
   ```

2. 設定追蹤後，像這樣使用全域追蹤器：

   ```go
   tracing.DefaultTracer()
   ```

   這會傳回一個用於建立 span 的 [OpenTelemetry `trace.Tracer`](https://pkg.go.dev/go.opentelemetry.io/otel/trace#Tracer)。

   **範例：**

   ```go
   func (d *Datasource) query(ctx context.Context, pCtx backend.PluginContext, query backend.DataQuery) (backend.DataResponse, error) {
       ctx, span := tracing.DefaultTracer().Start(
           ctx,
           "query processing",
           trace.WithAttributes(
               attribute.String("query.ref_id", query.RefID),
               attribute.String("query.type", query.QueryType),
               attribute.Int64("query.max_data_points", query.MaxDataPoints),
               attribute.Int64("query.interval_ms", query.Interval.Milliseconds()),
               attribute.Int64("query.time_range.from", query.TimeRange.From.Unix()),
               attribute.Int64("query.time_range.to", query.TimeRange.To.Unix()),
           ),
       )
       defer span.End()

       // ...
   }
   ```

### SDK 的自動檢測

SDK 會自動化某些檢測，以簡化開發人員的體驗。本節探討新增至 gRPC 呼叫和傳出 HTTP 請求的預設追蹤。

#### 追蹤 gRPC 呼叫

啟用追蹤後，會自動為每個 gRPC 呼叫（`QueryData`、`CallResource`、`CheckHealth` 等）建立一個新的 span，無論是在 Grafana 端還是在插件端。插件 SDK 也會將追蹤上下文注入傳遞給這些方法的 `context.Context` 中。

您可以透過將原始 `context.Context` 傳遞給 `tracing.SpanContextFromContext` 來擷取 [trace.SpanContext](https://pkg.go.dev/go.opentelemetry.io/otel/trace#SpanContext)：

```go
func (d *Datasource) query(ctx context.Context, pCtx backend.PluginContext, query backend.DataQuery) (backend.DataResponse, error) {
    spanCtx := trace.SpanContextFromContext(ctx)
    traceID := spanCtx.TraceID()

    // ...
}
```

#### 追蹤方法呼叫

啟用追蹤後，會自動為每個名為 `sdk.<endpoint>` 的方法呼叫建立一個新的 span，其中 endpoint 是 `QueryData`、`CallResource`、`CheckHealth` 等。Span 屬性可能包括 `plugin_id`、`org_id`、`datasource_name`、`datasource_uid`、`user`、`request_status`（ok、cancelled、error）、`status_source`（plugin、downstream）。

#### 追蹤傳出的 HTTP 請求

啟用追蹤後，除非您指定自訂中介軟體，否則 `TracingMiddleware` 也會新增至使用 [`httpclient.New`](https://pkg.go.dev/github.com/grafana/grafana-plugin-sdk-go/backend/httpclient#New) 或 [`httpclient.NewProvider`](https://pkg.go.dev/github.com/grafana/grafana-plugin-sdk-go/backend/httpclient#NewProvider) 建立的所有 HTTP 用戶端的預設中介軟體堆疊中。此中介軟體會為每個傳出的 HTTP 請求建立 span，並提供一些與請求生命週期相關的實用屬性和事件。

### 在本地收集和視覺化追蹤

請參閱[如何收集和視覺化日誌、指標和追蹤](#collect-and-visualize-logs-metrics-and-traces)。

## 收集和視覺化日誌、指標和追蹤

如果您想在開發插件時使用 Loki、Prometheus 和 Tempo 收集和視覺化日誌、指標和追蹤，請參閱 https://github.com/grafana/grafana/tree/main/devenv/docker/blocks/self-instrumentation，這些是 Grafana 維護人員正在使用的。
---
id: backend-plugins
title: 插件後端系統
description: 了解用於擴充 Grafana 功能的 Grafana 插件後端系統。
keywords:
  - grafana
  - plugins
  - plugin
  - backend
  - plugin system
sidebar_position: 1.5
---

# 插件後端系統

自 Grafana v7.0 起，具有後端元件的插件可讓您將 Grafana 與幾乎任何東西整合，並提供自訂視覺化。插件後端系統基於 HashiCorp 的 [Go Plugin System over RPC](https://github.com/hashicorp/go-plugin)，並支援伺服器端插件元素。Grafana 伺服器會將每個插件後端作為一個子程序啟動，並透過 [gRPC](https://grpc.io/) 與其通訊。

:::note
具有後端的插件也需要一個前端元件。例如，所有資料來源插件都需要一個前端的查詢編輯器元件。
:::

## 插件後端開發的好處

為您的插件新增後端元件有以下好處：

- **穩定性**：插件不會讓您的 Grafana 程序崩潰。插件中的 panic 不會讓伺服器 panic。
- **易於開發**：Grafana 提供了官方支援的 Go SDK 和工具來幫助建立插件。
- **安全性**：插件只能存取給它們的介面和引數，而不能存取程序的整個記憶體空間。

## 何時實作具有後端的插件

以下是具有後端元件的插件的一些常見使用案例：

- 支援資料來源的 [Grafana 警示](https://grafana.com/docs/grafana/latest/alerting/)、[記錄查詢](https://grafana.com/docs/grafana/latest/administration/recorded-queries/)和[查詢和資源快取](https://grafana.com/docs/grafana/latest/administration/data-source-management/#query-and-resource-caching)。
- 連接到 SQL 資料庫伺服器和其他通常無法從瀏覽器連線的非 HTTP 服務。
- 在使用者之間保持狀態，例如，透過為資料來源實作自訂快取。
- 使用 Grafana 不支援的自訂驗證方法和/或授權檢查。
- 使用自訂資料來源請求代理（有關更多資訊，請參閱[資源](#resources)）。

## Grafana 插件後端系統的功能

Grafana 的插件後端系統公開了幾個您的後端元件可以實作的關鍵功能或建構區塊：

- [查詢資料](#query-data)
- [資源](#resources)
- [健康狀況檢查](#health-checks)
- [收集指標](#collect-metrics)
- [串流](#streaming)

### 查詢資料

查詢資料功能允許插件的後端處理從[儀表板](https://grafana.com/docs/grafana/latest/dashboards)、[探索](https://grafana.com/docs/grafana/latest/explore)或 [Grafana 警示](https://grafana.com/docs/grafana/latest/alerting)提交的資料來源查詢。回應包含[資料框架](../data-frames)，用於視覺化指標、日誌和追蹤。

:::note

若要實作查詢資料功能，您需要一個資料來源插件後端。

:::

### 資源

資源功能允許插件的後端處理傳送到 Grafana HTTP API 的自訂 HTTP 請求，並以自訂 HTTP 回應來回應。在這裡，請求和回應格式可以有所不同。例如，您可以使用 JSON、純文字、HTML 或靜態資源，例如圖片和檔案等。

與查詢資料功能（其中回應包含資料框架）相比，資源功能為插件開發人員提供了更大的靈活性，可以擴充和開放 Grafana 以實現新的和有趣的用例。

#### 實作資源的使用案例：

- 實作自訂資料來源代理以提供某些驗證、授權或其他 Grafana [內建資料代理](https://grafana.com/docs/grafana/latest/developers/http_api/#data-source-proxy-calls)不支援的要求。
- 以適合在資料來源查詢編輯器中使用的格式傳回資料或資訊，以提供自動完成功能。
- 傳回靜態資源，例如圖片或檔案。
- 將指令傳送到裝置，例如微控制器或物聯網裝置。
- 從裝置（例如微控制器或物聯網裝置）請求資訊。
- 使用自訂資源、方法和動作擴充 Grafana 的 HTTP API。
- 使用[分塊傳輸編碼](https://en.wikipedia.org/wiki/Chunked_transfer_encoding)以分塊方式傳回大型資料回應或啟用某些串流功能。

### 健康狀況檢查

健康狀況檢查功能允許插件的後端傳回插件的狀態。對於資料來源插件的後端，當使用者編輯資料來源並在 UI 中選取_儲存並測試_時，會自動呼叫健康狀況檢查。

插件的健康狀況檢查端點會在 Grafana HTTP API 中公開，並允許外部系統持續輪詢插件的健康狀況，以確保其正常執行和運作。

### 收集指標

插件的後端可以使用基於文字的 Prometheus [公開格式](https://prometheus.io/docs/instrumenting/exposition_formats/)來收集和傳回執行階段、程序和自訂指標。如果您使用 [Grafana Plugin SDK for Go](./grafana-plugin-sdk-for-go) 來實作您的插件後端，那麼 [Prometheus Go 應用程式檢測函式庫](https://github.com/prometheus/client_golang)是內建的。此 SDK 為您提供了開箱即用的 Go 執行階段指標和程序指標。

若要新增自訂指標來檢測您的插件後端，請參閱[在您的插件中實作指標](../../how-to-guides/data-source-plugins/add-logs-metrics-traces-for-backend-plugins.md#implement-metrics-in-your-plugin)。

### 串流

串流功能允許插件的後端處理正在串流的資料來源查詢。有關更多資訊，請參閱[串流資料來源插件](../../tutorials/build-a-streaming-data-source-plugin.md)的教學。

## 資料通訊模型

Grafana 使用一種通訊模型，您可以選擇加入執行個體管理以簡化開發過程。如果您這樣做，則每個對插件後端的請求都會提供所有必要的資訊（設定），從而允許插件完成請求並傳回回應。此模型簡化了插件作者不必追蹤或請求額外狀態來完成請求的過程。

## 快取和連線池

Grafana 提供執行個體管理作為插件 SDK 的一部分，以簡化處理多個已設定的 Grafana 資料來源或應用程式（稱為執行個體）的工作。這可讓插件輕鬆地在執行個體之間乾淨地分離狀態。SDK 確保透過在記憶體中快取所述執行個體直到其在 Grafana 中的設定變更來最佳化插件資源。請參閱[資料來源插件後端教學](/tutorials/build-a-data-source-backend-plugin)或[具有後端的應用程式文件](/how-to-guides/app-plugins/add-backend-component)，其中展示了如何為資料來源和應用程式插件使用執行個體管理。

上述的執行個體狀態對於持有到下游伺服器（例如 HTTP、gRPC、TCP、UDP 等）的用戶端連線特別有用，以啟用連線池的使用，從而最佳化對下游伺服器的使用和連線重複使用。透過使用連線池，插件可以避免用盡機器的所有可用 TCP 連線。
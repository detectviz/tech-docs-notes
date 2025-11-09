---
id: profile-backend-plugin
title: 對插件的後端進行效能分析
description: 如何對插件的後端進行效能分析。
keywords:
  - grafana
  - plugins
  - plugin
  - profile
  - profiling
  - continuos profiling
  - backend
  - back-end
---

本指南提供了設定具有後端的插件的說明，以便在啟動時啟用某些診斷，產生_效能分析資料_。效能分析資料提供了潛在有用的資訊，可用於調查某些效能問題，例如高 CPU 或記憶體使用率，或當您想使用[持續性效能分析](https://grafana.com/oss/pyroscope/)時。

## 設定效能分析資料

[Grafana 設定檔](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/)可讓您在 `[plugin.<plugin ID>]` 下設定效能分析。

在此檔案的區段中，指定：

- 您要進行效能分析的插件的 `<plugin ID>`，一個唯一的識別碼。例如，[grafana-github-datasource](https://grafana.com/grafana/plugins/grafana-github-datasource/)。
- 效能分析設定選項，如下列小節所述。

**設定範例：**

```ini title="custom.ini"
[plugin.<plugin ID>]
profiling_enabled = true
profiling_port = 6060
profiling_block_rate = 5
profiling_mutex_rate = 5
```

套用設定變更後，請重新啟動 Grafana。您應該會看到一則日誌訊息，指出是否已啟用效能分析。例如：

```shell
INFO [07-09|19:15:00] Profiling enabled   logger=plugin.<plugin ID> blockProfileRate=5 mutexProfileRate=5
```

:::note

若要使用 `profiling_block_rate` 和 `profiling_mutex_rate`，您的插件需要至少使用 [`grafana-plugin-sdk-go v0.238.0`](https://github.com/grafana/grafana-plugin-sdk-go/releases/tag/v0.238.0)。有關如何更新 SDK 的說明，請參閱[更新 Go SDK](../../key-concepts/backend-plugins/grafana-plugin-sdk-for-go.md#update-the-go-sdk)。

:::

### `profiling_enabled` 選項

使用此選項啟用/停用效能分析。預設值為 `false`。

### `profiling_port` 選項

可選地，自訂公開效能分析資料的 HTTP 連接埠。例如，如果您想對多個插件進行效能分析，或者預設連接埠已被佔用，請使用此選項。預設值為 `6060`。

### `profiling_block_rate` 選項

使用此選項控制在阻塞設定檔中報告的 `goroutine` 阻塞事件的比例。預設值為 `0`（即不追蹤任何事件）。例如，使用 `5` 來報告所有事件的 20%。有關更詳細的資訊，請參閱 https://pkg.go.dev/runtime#SetBlockProfileRate。

:::note

比例越高（即此值越小），對正常操作造成的開銷就越大。

:::

### `profiling_mutex_rate` 選項

使用此選項控制在互斥鎖設定檔中報告的互斥鎖爭用事件的比例。預設值為 `0`（即不追蹤任何事件）。例如，使用 `5` 來報告所有事件的 20%。有關更詳細的資訊，請參閱 https://pkg.go.dev/runtime#SetMutexProfileFraction。

:::note

比例越高（即此值越小），對正常操作造成的開銷就越大。

:::

## 關於開銷的注意事項

在啟用效能分析且未啟用[區塊](#the-profiling_block_rate-option)和[互斥鎖](#the-profiling_block_rate-option)設定檔的情況下執行插件，應該只會增加一小部分開銷。因此，這些端點適用於生產或持續性效能分析情境。

新增一小部分區塊和互斥鎖設定檔，例如 5 或 10（即 10% 到 20%），通常應該沒問題，但您的體驗可能會因插件而異。

另一方面，也存在潛在問題。例如，如果您遇到請求緩慢或排隊且束手無策的情況，您可以暫時設定效能分析以收集 100% 的區塊和互斥鎖設定檔，以取得全貌。完成後，在收集設定檔後將其關閉。

## 檢查偵錯端點

透過瀏覽 `http://localhost:<profiling_port>/debug/pprof` 來檢查哪些偵錯端點可用。

在此檔案中，使用 `localhost`，表示您已連線到執行 Grafana 和插件的主機。如果從另一台主機連線，請視需要進行調整。

### 其他端點

還有一些額外的 [godeltaprof](https://github.com/grafana/pyroscope-go/tree/main/godeltaprof) 端點可用於效能分析。這些端點更適合持續性效能分析情境。

這些端點是：

- `/debug/pprof/delta_heap`
- `/debug/pprof/delta_block`
- `/debug/pprof/delta_mutex`

## 收集和分析設定檔

一般來說，您可以使用 [Go 指令 `pprof`](https://golang.org/cmd/pprof/) 來收集和分析效能分析資料。您也可以使用 [`curl`](https://curl.se/) 或類似工具來收集設定檔，這在您沒有 Go `pprof` 指令可用的環境中可能很方便。

接下來，讓我們看看一些使用 `curl` 和 `pprof` 來收集和分析記憶體和 CPU 設定檔的範例。

### 分析高記憶體使用率和記憶體洩漏

當遇到高記憶體使用率或潛在的記憶體洩漏時，收集多個堆積設定檔會很有用。然後稍後您可以分析和比較它們。

在收集每個設定檔之間等待一段時間（例如 30 秒）是個好主意，以允許記憶體消耗增加。

在以下範例中，使用 `localhost` 來表示您已連線到執行 Grafana 和插件的主機。如果您從另一台主機連線，請視需要調整指令。

```bash
curl http://localhost:<profiling_port>/debug/pprof/heap > heap1.pprof
sleep 30
curl http://localhost:<profiling_port>/debug/pprof/heap > heap2.pprof
```

然後您可以使用 `pprof` 工具來比較兩個堆積設定檔。例如：

```bash
go tool pprof -http=localhost:8081 --base heap1.pprof heap2.pprof
```

### 分析高 CPU 使用率

當您遇到高 CPU 使用率時，最好在一段時間內（例如 30 秒）收集 CPU 設定檔。

在以下範例中，使用 `localhost` 來表示您已連線到執行 Grafana 和插件的主機。如果您從另一台主機連線，請視需要調整指令。

```bash
curl 'http://localhost:<profiling_port>/debug/pprof/profile?seconds=30' > profile.pprof
```

然後您可以使用 `pprof` 工具來比較兩個堆積設定檔。例如：

```bash
go tool pprof -http=localhost:8081 profile.pprof
```

## 更多資訊

有關如何對 Grafana 進行效能分析的更多資訊和說明，請參閱 [Grafana 效能分析文件](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/configure-tracing/#turn-on-profiling-and-collect-profiles)。
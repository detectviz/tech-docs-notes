---
id: add-router
title: 新增路由器或多工器以查詢不同的資料類型
sidebar_label: 使用路由器或多工器查詢不同的資料
description: 將路由器或多工器新增至您的插件後端，以查詢不同的資料類型。
keywords:
  - grafana
  - plugins
  - plugin
  - router
  - multiplexer
---

# 新增路由器或多工器以查詢不同的資料類型

您插件後端中的 `QueryData` 方法只允許您查詢一種資料類型。若要支援不同種類的查詢（指標、日誌和追蹤），您需要實作查詢路由器（也稱為_多工器_）。

為此，請：

1. 在用戶端填入您查詢模型的 `queryType` 屬性，如 [`DataQuery`](https://github.com/grafana/grafana/blob/a728e9b4ddb6532b9fa2f916df106e792229e3e0/packages/grafana-data/src/types/query.ts#L47) 介面所示。
2. 在查詢中填入 `queryType` 並傳送至您的插件後端元件後，使用 [`datasource.QueryTypeMux`](https://pkg.go.dev/github.com/grafana/grafana-plugin-sdk-go/backend/datasource#QueryTypeMux) 將不同的查詢類型多工或路由至不同的查詢處理常式。
3. 然後，每個查詢處理常式可以將 [`DataQuery`](https://pkg.go.dev/github.com/grafana/grafana-plugin-sdk-go/backend#DataQuery) 中的每個查詢 JSON 欄位 `json.Unmarshal` 至一個特定的 Go 結構，如本範例所示：

```go
package mydatasource

import (
	"context"

	"github.com/grafana/grafana-plugin-sdk-go/backend"
	"github.com/grafana/grafana-plugin-sdk-go/backend/datasource"
)

type MyDatasource struct {
	queryHandler backend.QueryDataHandler
}

func New() *MyDatasource {
	ds := &MyDatasource{}
	queryTypeMux := datasource.NewQueryTypeMux()
	queryTypeMux.HandleFunc("metrics", ds.handleMetricsQuery)
	queryTypeMux.HandleFunc("logs", ds.handleLogsQuery)
	queryTypeMux.HandleFunc("traces", ds.handleTracesQuery)
	queryTypeMux.HandleFunc("", ds.handleQueryFallback)
	ds.queryHandler := queryTypeMux
	return ds
}

func (d *MyDatasource) QueryData(ctx context.Context, req *backend.QueryDataRequest) (*backend.QueryDataResponse, error) {
	return d.queryHandler.QueryData(ctx, req)
}

// handleMetricsQuery 處理查詢類型為 "metrics" 的查詢。
// backend.QueryDataRequest 中的所有查詢保證只包含
// 查詢類型為 "metrics" 的查詢。
func (d *MyDatasource) handleMetricsQuery(ctx context.Context, req *backend.QueryDataRequest) (*backend.QueryDataResponse, error) {
	// 實作...
}

// handleLogsQuery 處理查詢類型為 "logs" 的查詢。
// backend.QueryDataRequest 中的所有查詢保證只包含
// 查詢類型為 "logs" 的查詢。
func (d *MyDatasource) handleLogsQuery(ctx context.Context, req *backend.QueryDataRequest) (*backend.QueryDataResponse, error) {
	// 實作...
}

// handleTracesQuery 處理查詢類型為 "logs" 的查詢。
// backend.QueryDataRequest 中的所有查詢保證只包含
// 查詢類型為 "traces" 的查詢。
func (d *MyDatasource) handleTracesQuery(ctx context.Context, req *backend.QueryDataRequest) (*backend.QueryDataResponse, error) {
	// 實作...
}

// handleQueryFallback 處理沒有註冊相符查詢類型處理常式的查詢。
func (d *MyDatasource) handleQueryFallback(ctx context.Context, req *backend.QueryDataRequest) (*backend.QueryDataResponse, error) {
	// 實作...
}
```

## 進階用法

您可以在 Grafana 的內建 TestData 資料來源程式碼中找到使用 [`QueryTypeMux`](https://pkg.go.dev/github.com/grafana/grafana-plugin-sdk-go/backend/datasource#QueryTypeMux) 的範例：

- [建立查詢類型多工器](https://github.com/grafana/grafana/blob/623ee3a2be5c4cd84c61b6bbe82a32d18cc29828/pkg/tsdb/grafana-testdata-datasource/testdata.go#L22)並[呼叫 `registerScenarios`](https://github.com/grafana/grafana/blob/623ee3a2be5c4cd84c61b6bbe82a32d18cc29828/pkg/tsdb/grafana-testdata-datasource/testdata.go#L44)。
- [`registerScenarios` 方法](https://github.com/grafana/grafana/blob/623ee3a2be5c4cd84c61b6bbe82a32d18cc29828/pkg/tsdb/grafana-testdata-datasource/scenarios.go#L33)使用[輔助方法](https://github.com/grafana/grafana/blob/623ee3a2be5c4cd84c61b6bbe82a32d18cc29828/pkg/tsdb/grafana-testdata-datasource/scenarios.go#L204-L207)來註冊每個查詢類型處理常式。後者也示範了如何將實際的處理常式包裝在另一個處理常式中，以將常見功能或中介軟體應用於所有處理常式，例如記錄和追蹤。
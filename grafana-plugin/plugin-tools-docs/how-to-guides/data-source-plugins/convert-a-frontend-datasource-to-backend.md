---
id: convert-a-frontend-datasource-to-backend
title: 將資料來源插件前端邏輯轉換為後端元件
sidebar_label: 將前端轉換為後端元件
description: 學習如何將僅限前端的資料來源插件程式碼遷移至後端元件。
keywords:
  - grafana
  - plugins
  - plugin
  - backend
  - frontend
  - datasource
---

本指南說明如何將現有的僅限前端的資料來源插件轉換為[具有後端元件的插件](../../key-concepts/backend-plugins)。

若要將前端資料來源插件轉換為具有後端的插件：

1. 使用 `npx @grafana/create-plugin@latest` 建立一個具有後端的新資料來源插件。
2. 使用以下說明將您原始插件的功能複製到新建立的後端元件中。

## 為何要為您的插件新增後端？

有多項功能僅在插件具有後端元件時才可用，例如 Grafana 警示、記錄查詢或外部共用儀表板（先前稱為公開儀表板）。有關實作插件後端的用例，請參閱[插件後端簡介](../../key-concepts/backend-plugins/#when-to-implement-a-plugin-with-a-backend)。

## 開始之前

在深入了解細節之前：

- 熟悉建立具有後端元件的資料來源插件的過程。如果您以前沒有做過，可以遵循我們的[建立具有後端元件的插件](../../tutorials/build-a-data-source-backend-plugin.md)教學。

- 了解資料來源插件的主要元件以及這些元件在前端和後端之間的差異，如以下小節所述：

### 前端 `DataSource` 類別

資料來源插件實作一個新的 `DataSourcePlugin`。此類別接受一個 `DataSource` 類別作為參數，對於前端資料來源，該類別擴充 `DataSourceApi`，對於後端資料來源，則擴充 `DataSourceWithBackend`。由於 `DatasourceWithBackend` 類別已實作大部分所需的方法，您可以遷移至此以大幅簡化您的程式碼。

資料來源插件需要兩個元件：查詢編輯器和設定編輯器。

### 查詢和設定編輯器

將前端資料來源轉換為後端資料來源時，這兩個前端元件無需變更。但是，如果您為資料來源新增後端元件，您可以從中請求 `resources`。資源是插件公開的額外端點，可用於填入或驗證查詢或設定編輯器。在[其他資源請求區段](#other-resource-requests)中了解更多資訊。

## 插件結構比較

以下資料夾說明了在您為插件新增後端時引入的新元件：

```bash
myorg-myplugin-datasource/
├── .config/
├── .eslintrc
├── .github
│   └── workflows
├── .gitignore
├── .nvmrc
├── .prettierrc.js
├── CHANGELOG.md
├── LICENSE
├── Magefile.go # 後端可執行檔的建置定義
├── README.md
│   └── integration
├── docker-compose.yaml
├── go.mod # 相依性
├── go.sum # 總和檢查碼
├── jest-setup.js
├── jest.config.js
├── node_modules
├── package.json
├── pkg
│   ├── main.go # 後端進入點
│   └── plugin # 其他插件套件
├── playwright.config.ts
├── src
│   ├── README.md
│   ├── components
│   ├── datasource.ts
│   ├── img
│   ├── module.ts
│   ├── plugin.json # 已修改以包含 backend=true 和 executable=<建置二進位檔名稱>
│   └── types.ts
├── tsconfig.json
└── tests
```

## 將前端轉換為後端功能

大多數插件只需要實作三個方法即可完全運作：一個執行查詢的函式、一個測試資料來源連線的函式，以及任何用於擷取不同資源（用於填入查詢編輯器或設定編輯器）的額外 GET 請求。所有這三個方法通常都共用相同的對目標資料來源的驗證機制。

現在讓我們討論如何將驗證邏輯從前端移至後端。

### 驗證

Grafana 資料來源通常包含兩種資料：`jsonData` 和 `secureJsonData`。前者用於儲存非敏感資訊，後者用於儲存密碼或 API 金鑰等敏感資訊。

前端和後端類型都使用相同的 JSON 資料來對目標資料來源進行驗證。主要區別在於，前端資料來源應為每個請求讀取和使用憑證，而後端資料來源應在請求之間共用相同的已驗證用戶端。

在僅限前端的資料來源中，任何需要驗證的請求都需要通過插件代理。您需要在 `plugin.json` 檔案中定義一個 `routes` 物件，並在其中指定要用於每個請求的 URL 和憑證。例如，您可以透過設定一個帶有 `SecureJsonData` 憑證的 `Authorization` 標頭來驗證對給定 URL 的請求：

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

若要使用此路由，前端資料來源應呼叫 `DataSourceApi` 類別中的 `fetch` 方法。此方法會代理請求並新增 `Authorization` 標頭：

```typescript title="src/DataSource.ts"
import { getBackendSrv } from '@grafana/runtime';

const routePath = '/example';

const res = getBackendSrv().datasourceRequest({
  url: this.url + routePath + '/v1/users',
  method: 'GET',
});
// 處理回應
```

在後端資料來源中，您應將驗證邏輯移至 `Datasource` 建構函式。此方法在建立資料來源時呼叫，並應用於建立已驗證的用戶端。將此用戶端儲存在 `Datasource` 執行個體中，並用於每個請求。例如：

```go title="pkg/plugin/datasource.go"
package plugin

import (
  ...
	"github.com/grafana/grafana-plugin-sdk-go/backend/httpclient"
  ...
)

func NewDatasource(ctx context.Context, settings backend.DataSourceInstanceSettings) (instancemgmt.Instance, error) {
	opts, err := settings.HTTPClientOptions(ctx)
	if err != nil {
		return nil, fmt.Errorf("http client options: %w", err)
	}
	opts.Header.Add("Authorization", "Bearer " + settings.DecryptedSecureJSONData["token"])

 	cli, err := httpclient.New(opts)
	if err != nil {
		return nil, fmt.Errorf("httpclient new: %w", err)
	}

	return &Datasource{
		httpClient: cl,
	}, nil
}

// 在任何其他方法中
res, err := d.httpClient.Get("https://api.example.com/v1/users")
// 處理回應
```

相同的原則適用於任何其他驗證機制。例如，基於 SQL 的資料來源應使用 `Datasource` 建構函式來建立與資料庫的連線，並將其儲存在 `Datasource` 執行個體中。

您可以參考[我們的文件](/how-to-guides/data-source-plugins/add-authentication-for-data-source-plugins)以取得有關插件驗證的更多資訊。

### 健康狀況檢查

將驗證邏輯移至後端後，您可以在後端進行健康狀況檢查。

:::note

您需要在前端的 `Datasource` 類別中刪除前端實作 `testDatasource`，才能在後端使用健康狀況檢查。

:::

在此前端範例中，健康狀況檢查會向 `https://api.example.com`（如 `plugin.json` 的 `routes` 欄位中所定義）發出 API 請求，如果請求失敗則傳回錯誤：

```typescript title="src/DataSource.ts"
import { getBackendSrv } from '@grafana/runtime';

const routePath = '/example';

export class MyDatasource extends DataSourceApi<MyQuery, MyDataSourceJsonData> {
  ...

  async testDatasource() {
    try {
      await getBackendSrv().datasourceRequest({
        url: this.url + routePath + '/v1/users',
        method: 'GET',
      });
      return {
        status: 'success',
        message: 'Health check passed.',
      };
    } catch (error) {
      return { status: 'error', message: error.message };
    }
  }
}
```

在後端資料來源的情況下，`Datasource` 結構應實作 `CheckHealth` 方法。如果資料來源不健康，此方法會傳回錯誤。例如：

```go title="pkg/plugin/datasource.go"
func NewDatasource(ctx context.Context, settings backend.DataSourceInstanceSettings) (instancemgmt.Instance, error) {
	opts, err := settings.HTTPClientOptions(ctx)
	if err != nil {
		return nil, fmt.Errorf("http client options: %w", err)
	}

	cl, err := httpclient.New(opts)
	if err != nil {
		return nil, fmt.Errorf("httpclient new: %w", err)
	}

return &Datasource{
		settings:   settings,
		httpClient: cl,
	}, nil
}

func (d *Datasource) CheckHealth(ctx context.Context, _ *backend.CheckHealthRequest) (*backend.CheckHealthResult, error) {
	resp, err := d.httpClient.Get(d.settings.URL + "/v1/users")
	if err != nil {
    // 在此處記錄錯誤
  	return &backend.CheckHealthResult{
	  	Status: backend.HealthStatusError,
		  Message: "request error",
	  }, nil
	}
	if resp.StatusCode != http.StatusOK {
  	return &backend.CheckHealthResult{
	  	Status: backend.HealthStatusError,
		  Message: fmt.Sprintf("got response code %d", resp.StatusCode),
	  }, nil
	}
	return &backend.CheckHealthResult{
		Status:  backend.HealthStatusOk,
		Message: "Data source is working",
	}, nil
}
```

:::note

此範例涵蓋僅限 HTTP 的資料來源。因此，如果您的資料來源需要資料庫連線，您可以使用 Go 用戶端來連線資料庫並執行像 `SELECT 1` 或 `ping` 函式這樣的簡單查詢。

:::

### 查詢

下一步是移動查詢邏輯。這將根據插件如何查詢資料來源並將回應轉換為[框架](../../key-concepts/data-frames)而有很大的不同。在本指南中，您將看到如何遷移一個簡單的範例。

我們的資料來源在命中 `/metrics` 端點時會傳回一個帶有 `datapoints` 列表的 JSON 物件。前端 `query` 方法會將這些 `datapoints` 轉換為框架：

```typescript title="src/DataSource.ts"
export class DataSource extends DataSourceApi<MyQuery, MyDataSourceOptions> {
  async query(options: DataQueryRequest<MyQuery>): Promise<DataQueryResponse> {
    const response = await lastValueFrom(
      getBackendSrv().fetch<DataSourceResponse>({
        url: `${this.url}/metrics`,
        method: 'GET',
      })
    );
    const df: DataFrame = {
      length: response.data.datapoints.length,
      refId: options.targets[0].refId,
      fields: [
        { name: 'Time', values: [], type: FieldType.time, config: {} },
        {
          name: 'Value',
          values: [],
          type: FieldType.number,
          config: {},
        },
      ],
    };
    response.data.datapoints.forEach((datapoint: any) => {
      df.fields[0].values.push(datapoint.time);
      df.fields[1].values.push(datapoint.value);
    });
    return { data: [df] };
  }
}
```

現在讓我們看看如何將其轉換到後端。`Datasource` 執行個體應實作 `QueryData` 方法。此方法應傳回一個框架列表。

:::note

與健康狀況檢查一樣，您需要在前端的 `Datasource` 類別中刪除前端實作 `query`。

:::

以下範例顯示了前述方法：

```go title="pkg/plugin/datasource.go"
func (d *Datasource) QueryData(ctx context.Context, req *backend.QueryDataRequest) (*backend.QueryDataResponse, error) {
	res, err := d.httpClient.Get(d.settings.URL + "/metrics")
  // 處理錯誤 (省略)

	// 解碼回應
	var body struct {
    DataPoints []apiDataPoint `json:"datapoints"`
	}
	if err := json.NewDecoder(httpResp.Body).Decode(&body); err != nil {
		return backend.DataResponse{}, fmt.Errorf("%w: decode: %s", errRemoteRequest, err)
	}

	// 建立時間和值的切片。
	times := make([]time.Time, len(body.DataPoints))
	values := make([]float64, len(body.DataPoints))
	for i, p := range body.DataPoints {
		times[i] = p.Time
		values[i] = p.Value
	}

	// 建立框架並將其新增至回應
	dataResp := backend.DataResponse{
		Frames: []*data.Frame{
			data.NewFrame(
				"response",
				data.NewField("time", nil, times),
				data.NewField("values", nil, values),
			),
		},
	}
	return dataResp, err
}
```

### 其他資源請求

最後，有一種可選的請求類型，插件可以實作。這就是我們所說的_資源_。資源是插件公開並用於填入查詢編輯器或設定編輯器的額外端點。例如，您可以使用資源來填入一個下拉式選單，其中包含資料庫中可用表格的列表。

在前端資料來源中，插件應在 `plugin.json` 檔案中將資源定義為 `routes`，並使用 `fetch` 方法來取得資料。例如：

```json title="src/plugin.json"
{
  "routes": [
    {
      "path": "tables",
      "url": "https://api.example.com/api/v1/tables",
      "method": "GET"
    }
  ]
}
```

```typescript title="src/DataSource.ts"
export class DataSource extends DataSourceApi<MyQuery, MyDataSourceOptions> {
  async getTables() {
    const response = await lastValueFrom(
      getBackendSrv().fetch<MetricsResponse>({
        url: `${this.url}/tables`,
        method: 'GET',
      })
    );
    return response.data;
  }
}
```

:::note

為了保持簡單，此範例中省略了驗證。

:::

對於後端資料來源，插件應實作 `CallResourceHandler` 介面。此介面應處理不同的可能資源。例如：

```go title="pkg/plugin/datasource.go"
func NewDatasource(_ context.Context, _ backend.DataSourceInstanceSettings) (instancemgmt.Instance, error) {
	return &Datasource{
		CallResourceHandler: newResourceHandler(),
	}, nil
}

func newResourceHandler() backend.CallResourceHandler {
	mux := http.NewServeMux()
	mux.HandleFunc("/tables", handleTables)

	return httpadapter.New(mux)
}

func handleTables(w http.ResponseWriter, r *http.Request) {
  // 取得表格
  res, err :=	http.DefaultClient.Get("https://api.example.com/api/v1/tables")
  // 處理錯誤 (省略)
	body, err := io.ReadAll(res.Body)
  // 處理錯誤 (省略)

	w.Write(body)
	w.WriteHeader(http.StatusOK)
}
```

若要在前端請求資源，您可以使用基底類別 `DataSourceWithBackend` 中公開的方法（例如，`getResource` 或 `postResource`）：

```typescript title="src/DataSource.ts"
export class DataSource extends DataSourceWithBackend<MyQuery, MyDataSourceOptions> {
  async getTables() {
    const response = await this.getResource('tables');
    return response;
  }
}
```

## 結論

本指南涵蓋了將前端資料來源轉換為後端資料來源的主要步驟。由於插件種類繁多，如果您有任何問題或需要特定案例的協助，我們鼓勵您在我們的[社群論壇](https://community.grafana.com/c/plugin-development/30)中提出。也歡迎對本指南做出貢獻。
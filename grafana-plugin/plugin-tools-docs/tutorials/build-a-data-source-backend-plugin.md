---
id: build-a-data-source-backend-plugin
title: 建立資料來源外掛程式後端元件
sidebar_position: 10
description: 了解如何為您的資料來源外掛程式建立後端。
keywords:
  - grafana
  - plugins
  - plugin
  - backend
  - backend data source
  - datasource
---

import CreatePlugin from '@shared/create-plugin-backend.md';
import BackendPluginAnatomy from '@shared/backend-plugin-anatomy.md';
import TroubleshootPluginLoad from '@shared/troubleshoot-plugin-doesnt-load.md';

## 簡介

Grafana 支援多種[資料來源](https://grafana.com/grafana/plugins/data-source-plugins/)，包括 Prometheus、MySQL 和 Datadog。然而，在某些情況下，您可能已經擁有一個內部指標解決方案，並希望將其新增至您的 Grafana 儀表板。本教學將教您如何建立一個新的資料來源外掛程式來查詢資料。

後端元件為您的外掛程式提供了許多額外功能，例如自訂驗證方法。詳情請參閱[外掛程式後端元件的關鍵概念](../key-concepts/backend-plugins/)。

在本教學中，您將：

- 為您的資料來源建立一個[後端](../key-concepts/backend-plugins/)
- 為您的資料來源實作健康狀態檢查
- 為您的資料來源啟用 [Grafana Alerting](https://grafana.com/docs/grafana/latest/alerting/)

#### 先決條件

- Grafana v10.0 或更新版本
- [Docker](https://docs.docker.com/get-docker/)
- Go ([版本](https://github.com/grafana/plugin-tools/blob/main/packages/create-plugin/templates/backend/go.mod#L3))
- [Mage](https://magefile.org/)
- [LTS](https://nodejs.dev/en/about/releases/) 版本的 Node.js

## 建立新外掛程式

<CreatePlugin />

現在，讓我們驗證您目前為止建立的外掛程式在建立新資料來源時是否可以在 Grafana 中使用：

1. 在側邊選單中，前往 **Connections** > **Data Sources**。
2. 按一下 **Add data source**。
3. 搜尋您新建立的外掛程式名稱並選取它。
4. 輸入名稱，然後按一下 **Save & Test**。如果出現「隨機錯誤」，您可以忽略它 - 這是下面說明的[健康狀態檢查](#add-support-for-health-checks)的結果。

您現在擁有一個新的資料來源執行個體，可以在儀表板中使用。

若要將資料來源新增至儀表板：

1. 建立新儀表板並新增新面板。
2. 在查詢標籤中，選取您剛才建立的資料來源。會呈現一個包含一個序列（由兩個資料點組成）的折線圖。
3. 儲存儀表板。

### 疑難排解

<TroubleshootPluginLoad />

## 外掛程式後端元件剖析

<BackendPluginAnatomy pluginType="data source" />

在下一步中，我們將檢視查詢端點！

## 實作資料查詢

我們首先開啟檔案 `/pkg/plugin/datasource.go`。在此檔案中，您會看到 `Datasource` 結構，它實作了 [backend.QueryDataHandler](https://pkg.go.dev/github.com/grafana/grafana-plugin-sdk-go/backend?tab=doc#QueryDataHandler) 介面。此結構上的 `QueryData` 方法是資料來源外掛程式擷取資料的地方。

每個請求都包含多個查詢，以減少 Grafana 和外掛程式之間的流量。因此，您需要迴圈遍歷查詢的切片，處理每個查詢，然後傳回所有查詢的結果。

在本教學中，我們擷取了一個名為 `query` 的方法來處理每個查詢模型。由於每個外掛程式都有其獨特的查詢模型，Grafana 會將其作為 JSON 傳送至外掛程式後端。因此，外掛程式需要將查詢模型 `Unmarshal` 為更易於使用的格式。

如您所見，範例僅傳回靜態數字。請嘗試擴充外掛程式以傳回其他類型的資料。

例如，若要產生三個時間上等距的浮點數，您可以使用以下程式碼取代產生的兩個靜態數字：

```go
duration := query.TimeRange.To.Sub(query.TimeRange.From)
mid := query.TimeRange.From.Add(duration / 2)

s := rand.NewSource(time.Now().UnixNano())
r := rand.New(s)

lowVal := 10.0
highVal := 20.0
midVal := lowVal + (r.Float64() * (highVal - lowVal))

// add fields.
frame.Fields = append(frame.Fields,
  data.NewField("time", nil, []time.Time{query.TimeRange.From, mid, query.TimeRange.To}),
  data.NewField("values", nil, []float64{lowVal, midVal, highVal}),
)
```

您可以在我們的文件中閱讀更多關於如何[建立資料框架](../key-concepts/data-frames)的資訊。

## 新增健康狀態檢查支援

實作健康狀態檢查處理常式可讓 Grafana 驗證資料來源是否已正確設定。

在 Grafana 的 UI 中編輯資料來源時，您可以 **Save & Test** 以驗證其是否如預期般運作。

在此範例資料來源中，健康狀態檢查有 50% 的機率會成功。請務必向使用者傳回適當的錯誤訊息，告知他們資料來源中設定錯誤的地方。

開啟 `/pkg/plugin/datasource.go`。在此檔案中，您會看到 `Datasource` 結構也實作了 [backend.CheckHealthHandler](https://pkg.go.dev/github.com/grafana/grafana-plugin-sdk-go/backend?tab=doc#CheckHealthHandler) 介面。前往 `CheckHealth` 方法，查看此範例外掛程式的健康狀態檢查是如何實作的。

## 新增驗證

實作驗證可讓您的外掛程式存取受保護的資源，例如資料庫或 API。若要了解更多資訊，請參閱[如何使用外掛程式後端元件進行驗證](../how-to-guides/data-source-plugins/add-authentication-for-data-source-plugins#authenticate-using-a-plugin-backend)。

## 啟用 Grafana Alerting

1. 新增頂層 `alerting` 屬性，其值為 `true`，以指定您的外掛程式支援 Grafana Alerting，例如

   ```json title="src/plugin.json"
   {
     ...
     "backend": true,
     "executable": "gpx_simple_datasource_backend",
     "alerting": true,
     "info": {
     ...
   }
   ```

2. 重新啟動您的 Grafana 執行個體。
3. 在您的網頁瀏覽器中開啟 Grafana。
4. 導覽至您建立的資料來源，驗證現在是否支援警示。您應該會在「設定」檢視中看到「支援警示」訊息。

### 建立警示

:::note

以下說明基於 Grafana v10.1.1，請參閱[警示文件](https://grafana.com/docs/grafana/latest/alerting/)以取得適合版本的指引。

:::

1. 開啟您在「建立新外掛程式」步驟中稍早建立的儀表板。
2. 編輯現有面板。
3. 按一下面板下方的「警示」標籤。
4. 按一下「從此面板建立警示規則」按鈕。
5. 在「運算式」區段中，在「閾值」運算式 `C` 中，將 `IS ABOVE` 設定為 `15`。
6. 在「閾值」運算式 `C` 上按一下「設定為警示條件」。您的警示現在應如下所示。
   ![顯示 B「縮減」的運算式區段，輸入：A，函式：Last，模式：Strict，C 閾值，輸入：B，Is Above：15 和已啟用警示條件指示器](/img/create-alert.png '警示運算式')
7. 在「設定警示評估行為」區段中，按一下「新資料夾」按鈕並建立一個新資料夾來儲存評估規則。
8. 然後，按一下「新評估群組」按鈕並建立一個新評估群組；選擇一個名稱並將「評估間隔」設定為 `10s`。
9. 按一下「儲存規則並結束」按鈕。
10. 儲存儀表板。一段時間後，警示規則會評估並轉換為「警示中」狀態。

## 同時執行多個查詢

:::note

此功能僅適用於 Grafana 外掛程式後端 SDK 0.232.0 版及更新版本。

:::

預設情況下，單一請求中的多個查詢（即在一個面板內）會循序執行。若要同時執行多個查詢，您可以使用 SDK 公開的 `concurrent.QueryData` 函式。

若要使用 `concurrent.QueryData`，請指定如何執行單一查詢以及要執行的並行查詢數量的限制。請注意，最多可執行 10 個並行查詢。

```go
import (
	...
	"github.com/grafana/grafana-plugin-sdk-go/experimental/concurrent"
	...
)

func (d *Datasource) handleSingleQueryData(ctx context.Context, q concurrent.Query) (res backend.DataResponse) {
  // 在此處實作查詢邏輯
}

func (d *Datasource) QueryData(ctx context.Context, req *backend.QueryDataRequest) (*backend.QueryDataResponse, error) {
	return concurrent.QueryData(ctx, req, d.handleSingleQueryData, 10)
}
```

## 摘要

在本教學中，您為您的資料來源外掛程式建立了後端。
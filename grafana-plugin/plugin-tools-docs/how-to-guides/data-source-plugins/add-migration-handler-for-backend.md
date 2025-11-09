---
id: add-migration-handler-for-backend-data-source
title: 為後端資料來源插件新增查詢遷移
sidebar_label: 新增查詢遷移
description: 如何為您的 Grafana 後端資料來源插件新增查詢遷移處理常式，以實現無縫更新。
keywords:
  - grafana
  - plugins
  - plugin
  - migration
  - datasource
---

隨著插件的發展，插件定義的查詢模型可能需要進行重大變更。原因包括插件所依賴的第三方服務的重大變更、重構、新功能等等。變更資料來源查詢模型時，插件應實作遷移邏輯，以將現有查詢從先前的格式轉換為最新的格式。此遷移邏輯應同時包含在後端（適用於非前端發起的查詢，例如警示）和前端（以使查詢適應較新版本的 `QueryEditor`）。

## 為何要新增查詢遷移處理常式

為確保相容性並維持無縫效能，查詢遷移處理常式會將舊版查詢轉換為目前格式。此方法可讓您在不破壞現有查詢或重複程式碼的情況下提供更新，為使用者更新您的插件時提供順暢的轉換體驗。

## 開始之前

根據您在本指南中執行[其中一個步驟](#step-5-use-migration-code-from-the-frontend-using-experimental-apis)所採用的方法，您可能需要滿足某些先決條件。這些先決條件是：

1. Grafana 必須設定為以獨立 API 伺服器的形式執行資料來源，此行為位於功能旗標 [grafanaAPIServerWithExperimentalAPIs](https://github.com/grafana/grafana/blob/7773c658bb3280f0432fc9742109f8eb324c83a3/pkg/services/featuremgmt/registry.go#L474) 之後。
2. 插件必須在 Grafana 11.4 或更新版本上執行。

有關這些先決條件的更多資訊，請參閱[步驟 5](#step-5-use-migration-code-from-the-frontend-using-experimental-apis)，但如果您的插件無法遵守這些要求，則有使用現有 API 的[替代方法](#step-5-alternative-run-migrations-using-legacy-apis)。

## 實作後端遷移處理常式

若要實作後端遷移處理常式，請在您的後端程式碼中新增遷移邏輯。本指南將引導您完成使用最新工具自動遷移查詢並避免重複程式碼所需的步驟。

:::note

本指南中詳述的遷移系統不支援雙向遷移。僅涵蓋向前遷移。查詢遷移不會自動持續存在，因此使用者需要手動儲存變更以確保流程如預期般運作。

:::

### 步驟 1 (可選)：新增查詢結構描述

首先，插件不需要有強型別的查詢。雖然這降低了插件開發的門檻，但未定義型別的插件更難以擴充和維護。本指南的第一步是新增必要的檔案來定義插件查詢。

請參閱以下範例：[grafana-plugin-examples#400](https://github.com/grafana/grafana-plugin-examples/pull/400)。如您所見，需要建立多個檔案。這些檔案將用於產生 OpenAPI 文件和驗證收到的查詢是否有效（但這是一項仍在進行中的功能，尚未提供）。

建立這些檔案：

- `query.go`：此檔案定義了您的查詢的 Golang 型別。為了讓自動遷移正常運作，您的查詢擴充新的 `v0alpha1.CommonQueryProperties` 很重要。之後，只需定義您的查詢自訂屬性。
- `query_test.go`：此測試檔案既用於檢查所有 JSON 檔案是否與查詢模型保持最新，也用於產生它們。第一次執行測試時，它會產生這些檔案（因此請考慮到 `query.types.json` 需要存在，即使它是空的）。
- `query.*.json`：自動產生的檔案。這些結構描述可用於 OpenAPI 文件。

### 步驟 2：變更查詢模型

:::note

有關如何新增查詢遷移的完整範例（步驟 2、3 和 4），請參閱[實驗性 API](https://github.com/grafana/grafana-plugin-examples/pull/403/files) 或[穩定 API](https://github.com/grafana/grafana-plugin-examples/pull/407) 的程式碼。

:::

一旦您的插件有了自己的結構描述，就可以開始引入模型變更。由於主要版本（或相同 API 版本）內的查詢需要相容，因此請保留對舊版資料格式的參考。此參考也有助於實現簡單的遷移路徑。

例如，假設您要變更插件的查詢格式，並且您正在使用的 `Multiplier` 屬性正在變更為 `Multiply`，如下所示：

```diff
 type DataQuery struct {
        v0alpha1.CommonQueryProperties

-       // Multiplier is the number to multiply the input by
+       // Multiply is the number to multiply the input by
+       Multiply int `json:"multiply,omitempty"`
+
+       // Deprecated: Use Multiply instead
        Multiplier int `json:"multiplier,omitempty"`
 }
```

在此範例中，您可以透過在 `query_test.go` 中執行測試來重新產生結構描述，這樣您的新資料類型就可以使用了。

請注意，目前還沒有破壞性變更，因為所有新參數（在本例中為 `Multiply`）都被標記為可選。此外，其他業務邏輯均未修改，因此一切都應像以前一樣使用已棄用的屬性運作。在下一步中，實際上會有一個破壞性變更。

### 步驟 3：使用新的查詢格式

修改插件程式碼以使用新的資料模型，忽略現有儀表板或查詢繼續使用舊模型的事實。請注意，您必須同時修改前端和後端程式碼。

首先將 `Multiplier` 的使用替換為 `Multiply`。

以下是後端範例：

```diff
func (d *Datasource) query(ctx context.Context, pCtx backend.PluginContext, quer
                        return backend.DataResponse{}, fmt.Errorf("unmarshal: %w", err)
                }
                q := req.URL.Query()
-               q.Add("multiplier", strconv.Itoa(input.Multiplier))
+               q.Add("multiplier", strconv.Itoa(input.Multiply))
                req.URL.RawQuery = q.Encode()
        }
        httpResp, err := d.httpClient.Do(req)
```

以下是前端範例：

```diff
 export class QueryEditor extends PureComponent<Props> {
           type="number"
           id="multiplier"
           name="multiplier"
-          value={this.props.query.multiplier}
-          onChange={(e) => this.props.onChange({ ...this.props.query, multiplier: e.currentTarget.valueAsNumber })}
+          value={this.props.query.multiply}
+          onChange={(e) => this.props.onChange({ ...this.props.query, multiply: e.currentTarget.valueAsNumber })}
         />
       </HorizontalGroup>
     );
```

此時，終於出現了破壞性變更。新查詢將使用新格式並如預期般運作，但舊版查詢則不行，因為它們沒有定義新屬性。讓我們來修正這個問題。

### 步驟 4：在後端新增遷移程式碼

在後端建立一個解析函式，它會接收 `QueryData` 函式收到的通用 JSON blob，然後視需要遷移格式。該函式應接收一個 `backend.DataQuery` 並傳回您自己的 `kinds.DataQuery`。

此函式應僅將 JSON 從原始 `DataQuery` 中解組，並將其解析為您自己的 `DataQuery`，並進行任何必要的遷移。在您的插件邏輯中使用此函式。透過我們範例中的此變更，無論查詢使用舊模型 (`Multiplier`) 還是新模型 (`Multiply`)，兩者都能如預期般運作。

範例：

```diff
func (d *Datasource) query(ctx context.Context, pCtx backend.PluginContext, quer
                return backend.DataResponse{}, fmt.Errorf("new request with context: %w", err)
        }
        if len(query.JSON) > 0 {
-               input := &kinds.DataQuery{}
-               err = json.Unmarshal(query.JSON, input)
+               input, err := convertQuery(query)
                if err != nil {
-                       return backend.DataResponse{}, fmt.Errorf("unmarshal: %w", err)
+                       return backend.DataResponse{}, err
                }
                q := req.URL.Query()
                q.Add("multiplier", strconv.Itoa(input.Multiply))
...

+func convertQuery(orig backend.DataQuery) (*kinds.DataQuery, error) {
+       input := &kinds.DataQuery{}
+       err := json.Unmarshal(orig.JSON, input)
+       if err != nil {
+               return nil, fmt.Errorf("unmarshal: %w", err)
+       }
+       if input.Multiplier != 0 && input.Multiply == 0 {
+               input.Multiply = input.Multiplier
+               input.Multiplier = 0
+       }
+       return input, nil
+}
```

### 步驟 5：從前端使用遷移程式碼（使用實驗性 API）

:::note

此功能取決於功能旗標 [grafanaAPIServerWithExperimentalAPIs](https://github.com/grafana/grafana/blob/7773c658bb3280f0432fc9742109f8eb324c83a3/pkg/services/featuremgmt/registry.go#L474)。它還需要 **@grafana/runtime > 11.4** 套件（仍為實驗性功能）。如果您的插件實作此功能，請將其 **grafanaDepencency 提升至 ">=11.4.0"**。如果您的插件無法遵守這些要求，請參閱[使用舊版 API 執行遷移](#step-5-alternative-run-migrations-using-legacy-apis)。

:::

您應該能夠從前端和後端呼叫您的 `convertQuery` 函式，因此我們的 `QueryEditor` 元件應該能夠將查詢轉換為新格式。為了將此函式公開給前端，後端需要實作 `QueryConversionHandler` 介面。這只是 `convertQuery` 函式的包裝器，但適用於多個查詢。

以下是 `convertQuery` 實作的範例：

```go title="convert_query.go"
// convertQuery 解析給定的 DataQuery 並視需要進行遷移。
func convertQuery(orig backend.DataQuery) (*kinds.DataQuery, error) {
	input := &kinds.DataQuery{}
	err := json.Unmarshal(orig.JSON, input)
	if err != nil {
		return nil, fmt.Errorf("unmarshal: %w", err)
	}
	if input.Multiplier != 0 && input.Multiply == 0 {
		input.Multiply = input.Multiplier
		input.Multiplier = 0
	}
	return input, nil
}

// convertQueryRequest 遷移給定的 QueryDataRequest，其中可能包含多個查詢。
func convertQueryRequest(ctx context.Context, req *backend.QueryDataRequest) (*backend.QueryConversionResponse, error) {
	queries := make([]any, 0, len(req.Queries))
	for _, q := range req.Queries {
		input, err := convertQuery(q)
		if err != nil {
			return nil, err
		}
		q.JSON, err = json.Marshal(input)
		if err != nil {
			return nil, fmt.Errorf("marshal: %w", err)
		}
		queries = append(queries, q)
	}
	return &backend.QueryConversionResponse{
		Queries: queries,
	}, nil
}
```

最後，調整前端，讓 `@grafana/runtime` 知道是否應該執行遷移動作。分兩步完成：

1. 在插件的 `DataSource` 類別中實作 `@grafana/runtime` 公開的 `MigrationHandler`。設定屬性 `hasBackendMigration` (為 `true`) 並實作函式 `shouldMigrate`。`shouldMigrate` 函式會接收一個查詢並驗證其是否需要遷移（例如，透過檢查最新的屬性或檢查預期的插件版本，如果它是模型的一部分）。此驗證可避免對後端進行不必要的查詢。
2. 將包裝器 `QueryEditorWithMigration` 與您的 `QueryEditor` 元件一起使用。此包裝器將確保在呈現編輯器之前已遷移查詢。

就是這樣。一旦插件實作了這些步驟，現有和新的查詢將繼續運作，而無需在多個地方重複遷移邏輯。

:::note

若要查看步驟 2 到 5 在完整範例中的作法，請參閱[此範例](https://github.com/grafana/grafana-plugin-examples/pull/403/files)。

:::

### 步驟 5 (替代方案)：使用舊版 API 執行遷移

除了使用實驗性 API 執行遷移外，也可以使用舊版 API 執行遷移。沒有額外的要求。

請遵循以下步驟：

1. 在後端，將 `convertQuery` 公開為[資源](../../how-to-guides/data-source-plugins/add-resource-handler.md)，以便您可以使用像 `/migrate-query` 這樣的資源端點來擷取它。
2. 修正插件 `QueryEditor`，因為舊版查詢會嘗試呈現舊格式，而插件邏輯尚未為此做好準備。為此，請將插件設定為使用剛剛定義的新遷移端點。

:::note

若要查看步驟 2 到 5 在完整範例中的作法，請參閱[此範例](https://github.com/grafana/grafana-plugin-examples/pull/407)。

:::

### 步驟 6 (可選)：新增 AdmissionHandler

:::note

此步驟是可選的。僅當您使用功能旗標 [grafanaAPIServerWithExperimentalAPIs](https://github.com/grafana/grafana/blob/3457f219be1c8bce99f713d7a907ee339ef38229/pkg/services/featuremgmt/registry.go#L519) 執行 Grafana 時才需要。

:::

使用實驗性 API 執行 Grafana 時，每個資料來源都將作為一個獨立的 API 伺服器執行。這意味著查詢將被路由到像 `https://<grafana-host>/apis/<datasource>.datasource.grafana.app/v0alpha1/namespaces/stack-1/connections/<uid>/query` 這樣的伺服器。

在此情境下，並為確保您的插件能與預期的 API 版本（一開始為 `v0alpha1`）搭配使用，請實作一個 `AdmissionHandler`。此 `AdmissionHandler` 可確保給定的資料來源設定滿足正在執行的 API 版本的預期，因此它們可以處理該 API 版本的查詢。

雖然插件在 `v0*` 時此步驟不是強制性的，但一旦插件達到 `v1`，它就是強制性的。目前，它用於[在儲存資料來源設定時進行驗證](https://github.com/grafana/grafana/blob/a46ff09bf91895ee3de0d8f6c4ab88d70b47bfe6/pkg/services/datasources/service/datasource.go#L373)。

`AdmissionHandler` 方法應實作兩個主要函式：

- `ValidateAdmission`：檢查給定的實體是否有效（在此情況下，為資料來源設定）。
- `MutateAdmission`：允許在儲存實體之前對其進行變異。

在[我們的範例](https://github.com/grafana/grafana-plugin-examples/pull/401)中，這兩個函式可以互換，因為它們都執行相同的程式碼（也就是說，只進行驗證，不進行任何變異）。

## 結論

新增查詢遷移可讓您的 Grafana 資料來源插件在不破壞現有使用者功能的情況下發展。透過在後端和前端維護遷移處理常式，您可以確保相容性並在每次更新中提供更順暢的使用者體驗。
---
id: build-a-streaming-data-source-plugin
title: 建立串流資料來源外掛程式
sidebar_position: 9
description: 了解如何建立串流資料來源外掛程式。
keywords:
  - grafana
  - plugins
  - plugin
  - streaming
  - streaming data source
  - datasource
---

# 建立串流資料來源外掛程式

本教學將教您如何使用 `create-plugin` 工具和範例原始碼建立 Grafana 串流資料來源外掛程式。

串流資料來源外掛程式只會在您的資料來源中有新資料可用時才重新整理外掛程式中的資料。此方法不同於儀表板設定為在指定間隔自動重新整理的一般方法。串流的主要優點是它消除了每次需要更新資料時都必須重新執行查詢的額外負荷。

本教學說明如何建立同時具有前端和後端的資料來源外掛程式。完成本教學後，您將建立一個外掛程式，該外掛程式會在後端產生隨機數字，並以視覺化方式傳回前端。

## 範例螢幕擷取畫面和程式碼

下圖顯示使用此資料來源的面板：

![Grafana 串流資料來源。](/img/streaming-data-source.gif)

## 步驟 1：建立外掛程式

只要您遵循 Grafana 外掛程式規格並實作所有必要的介面，就可以從頭開始建立所有外掛程式程式碼。但是，建立外掛程式最簡單的方法是使用我們提供的 `@grafana/create-plugin` 工具。

1. 前往您要建立外掛程式目錄的位置並執行：

```shell
npx @grafana/create-plugin@latest
```

:::note

如需設定開發環境的完整先決條件和建議清單，請參閱[開始使用](/).

:::

Create Plugin 會提示您一些有關外掛程式名稱和類型、您的組織以及許多其他選項的問題。

1. 在本教學中，請為外掛程式類型輸入 `data source`，並指定它具有後端部分。

該工具將建立一個包含所有必要程式碼和相依性的骨架，以執行資料來源外掛程式後端元件。而且，如果您編譯程式碼，您將擁有一個非常簡單的外掛程式後端。但是，此產生的程式碼還不是串流外掛程式，我們需要進行一些修改。

## 步驟 2：設定前端

由於我們希望將一些參數傳遞給外掛程式的後端部分，因此我們先變更 `/src/components/QueryEditor.tsx`。此元件定義了使用者可以提供給查詢的輸入。我們將新增三個數字輸入，用於產生我們的資料：

- **`tickInterval`** - 產生資料的間隔
- **`upperLimit`** - 隨機數字的上限
- **`lowerLimit`** - 隨機數字的下限

若要完成此操作，請依照下列步驟。

1. 將 `/src/components/QueryEditor.tsx` 中的 `QueryEditor` 函式取代為下列程式碼：

```tsx
export function QueryEditor({ query, onChange, onRunQuery }: Props) {
  const onLowerLimitChange = (event: ChangeEvent<HTMLInputElement>) => {
    onChange({ ...query, lowerLimit: event.target.valueAsNumber });
  };

  const onUpperLimitChange = (event: ChangeEvent<HTMLInputElement>) => {
    onChange({ ...query, upperLimit: event.target.valueAsNumber });
  };

  const onTickIntervalChange = (event: ChangeEvent<HTMLInputElement>) => {
    onChange({ ...query, tickInterval: event.target.valueAsNumber });
  };

  const { upperLimit, lowerLimit, tickInterval } = query;

  return (
    <>
      <InlineField label="Lower Limit" labelWidth={16} tooltip="Random numbers lower limit">
        <Input onChange={onLowerLimitChange} onBlur={onRunQuery} value={lowerLimit || ''} type="number" />
      </InlineField>
      <InlineField label="Upper Limit" labelWidth={16} tooltip="Random numbers upper limit">
        <Input onChange={onUpperLimitChange} onBlur={onRunQuery} value={upperLimit || ''} type="number" />
      </InlineField>
      <InlineField label="Tick interval" labelWidth={16} tooltip="Server tick interval">
        <Input onChange={onTickIntervalChange} onBlur={onRunQuery} value={tickInterval || ''} type="number" />
      </InlineField>
    <>
  );
}
```

2. 在 `src/datasource.ts` 檔案中，透過串流通道表示查詢。此檔案是執行外掛程式前端部分查詢的地方。將下列方法新增至 `DataSource` 類別：

```tsx
  query(request: DataQueryRequest<MyQuery>): Observable<DataQueryResponse> {
    const observables = request.targets.map((query, index) => {

      return getGrafanaLiveSrv().getDataStream({
        addr: {
          scope: LiveChannelScope.DataSource,
          namespace: this.uid,
          path: `my-ws/custom-${query.lowerLimit}-${query.upperLimit}-${query.tickInterval}`, // this will allow each new query to create a new connection
          data: {
            ...query,
          },
        },
      });
    });

    return merge(...observables);
  }
```

對 `getGrafanaLiveSrv()` 的呼叫會傳回對 Grafana 後端的參考，而 `getDataStream` 呼叫會建立串流。如此程式碼片段所示，我們需要提供一些資料來建立串流：

- **`scope`** - 定義通道的使用和控制方式 (指定 `data source`)
- **`namespace`** - 我們特定資料來源的唯一識別碼
- **`path`** - 可區分由相同資料來源建立之通道的部分。在此範例中，我們希望為每個查詢建立不同的通道，因此我們將使用查詢參數作為路徑的一部分。
- **`data`** - 用於在前端和後端之間交換資訊

值得一提的是，`path` 也可以用來與後端部分交換資料，但對於複雜的格式，請使用 `data` 屬性。

:::note

串流由參數組合唯一識別，其格式為：`${scope}/${namespace}/${path}`。這只是一個實作細節，除了偵錯之外，您可能不需要知道。

:::

## 步驟 3：設定外掛程式後端

現在我們需要將必要的程式碼新增至後端。為此，我們變更 `pkg/plugin/datasource.go`，這裡是負責處理由前端建立之查詢的後端部分的定義。在我們的案例中，由於我們希望處理串流，因此我們需要實作 `backend.StreamHandler`。

1. 在 `var` 的取代中新增下列部分：

```go
var (
    _ backend.CheckHealthHandler    = (*Datasource)(nil)
    _ instancemgmt.InstanceDisposer = (*Datasource)(nil)
    _ backend.StreamHandler         = (*Datasource)(nil)
)
```

這表示我們的 `DataSource` 將實作 `backend.CheckHealthHandler`、`instancemgmt.InstanceDisposer` 和 `backend.StreamHandler`。前兩者所需的方法已在骨架中提供；因此，我們只需要實作 `backend.StreamHandler` 所需的方法。

2. 讓我們從新增 `SubscribeStream` 方法開始：

```go
func (d *Datasource) SubscribeStream(context.Context, *backend.SubscribeStreamRequest) (*backend.SubscribeStreamResponse, error) {
    return &backend.SubscribeStreamResponse{
        Status: backend.SubscribeStreamStatusOK,
    }, nil
}
```

當使用者嘗試訂閱頻道時，將會呼叫此程式碼。您可以在此處實作權限檢查，但在我們的案例中，我們只希望使用者在每次嘗試時都能成功連線；因此，我們只傳回一個後端：`SubscribeStreamStatusOK`。

3. 實作 `PublishStream` 方法：

```go
func (d *Datasource) PublishStream(context.Context, *backend.PublishStreamRequest) (*backend.PublishStreamResponse, error) {
    return &backend.PublishStreamResponse{
        Status: backend.PublishStreamStatusPermissionDenied,
    }, nil
}
```

每當使用者嘗試發布至頻道時，都會呼叫此程式碼。由於我們不希望在建立第一個連線後從前端接收任何資料，因此我們只會傳回 `backend.PublishStreamStatusPermissionDenied`。

4. 為避免隨機錯誤，請將預設提供的 `CheckHealth` 方法變更為下列程式碼：

```go
  func (d *Datasource) CheckHealth(_ context.Context, req *backend.CheckHealthRequest) (*backend.CheckHealthResult, error) {
    return &backend.CheckHealthResult{
      Status:  backend.HealthStatusOk,
      Message: "Data source is working",
    }, nil
  }
```

請注意，此程式碼與串流外掛程式本身無關。這只是為了避免因不變更預設值而導致的隨機錯誤。

5. 最後，實作 `RunStream` 方法：

```go
func (d *Datasource) RunStream(ctx context.Context, req *backend.RunStreamRequest, sender *backend.StreamSender) error {
    q := Query{}
    json.Unmarshal(req.Data, &q)

    s := rand.NewSource(time.Now().UnixNano())
    r := rand.New(s)

    ticker := time.NewTicker(time.Duration(q.TickInterval) * time.Millisecond)
    defer ticker.Stop()

    for {
        select {
        case <-ctx.Done():
            return ctx.Err()
        case <-ticker.C:
            // we generate a random value using the intervals provided by the frontend
            randomValue := r.Float64()*(q.UpperLimit-q.LowerLimit) + q.LowerLimit

            err := sender.SendFrame(
                data.NewFrame(
                    "response",
                    data.NewField("time", nil, []time.Time{time.Now()}),
                    data.NewField("value", nil, []float64{randomValue})),
                data.IncludeAll,
            )

            if err != nil {
                Logger.Error("Failed send frame", "error", err)
            }
        }
    }
}
```

實作後，此方法將在每個連線時呼叫一次，我們可以在此處剖析由前端傳送的資料，以便 Grafana 可以持續傳回資料。在此特定範例中，我們將使用由前端傳送的資料來定義我們將傳送框架的頻率以及產生的隨機數範圍。

因此，方法的第一部分會剖析資料，而查詢會定義為 `struct`。

6.  將下列程式碼新增至 `./pkg/plugin/query.go`：

```go
  type Query struct {
      UpperLimit   float64 `json:"upperLimit"`
      LowerLimit   float64 `json:"lowerLimit"`
      TickInterval float64 `json:"tickInterval"`
  }
```

在此範例中，我們根據此計時器建立一個計時器和一個無限迴圈。每當計時器逾時時，我們將進入 `select` 的第二種情況並產生一個隨機數 `randomValue`。之後，我們將使用 `data.NewFrame` 建立一個包含 `randomValue` 和目前時間的資料框架，並使用 `sender.SendFrame` 傳送它。此迴圈將無限期執行，直到我們關閉通道。

:::tip

您可以移除由骨架建立的 `QueryData` 方法及其所有相關程式碼，因為我們不再實作後端。`QueryDataHandler`。

:::

## 步驟 4：修改 `plugin.json`

Grafana 需要在首次載入時辨識新外掛程式是串流外掛程式。若要完成此操作，只需將 `"streaming":true` 屬性新增至您的 `src/plugin.json`。它應該看起來像這樣：

```json
{
  ...
  "id": "grafana-streamingbackendexample-datasource",
  "metrics": true,
  "backend": true,
  ...
  "streaming": true
}
```

## 步驟 5：建置外掛程式

外掛程式開發過程的程式碼撰寫部分已完成。現在我們需要產生我們的外掛程式套件，以便能夠在 Grafana 中執行它。由於我們的外掛程式同時具有前端和後端部分，因此我們分兩步驟執行此操作。

1. 建置前端：

```shell
npm install
npm run build
```

這應該會下載所有相依性並在 `./dist` 目錄中建立外掛程式前端檔案。

2. 編譯後端程式碼並產生外掛程式二進位檔。為此，您應該已安裝 `mage`，然後您只需執行：

```shell
mage -v
```

此指令會編譯 Go 程式碼並在 `./dist` 中為所有 Grafana 支援的平台產生二進位檔。

## 步驟 6：執行外掛程式

建置外掛程式後，您可以複製 `./dist` 及其內容，將其重新命名為外掛程式的 `id`，將其放入 Grafana 執行個體的插件路徑中，然後進行測試。

但是，有一個更簡單的方法可以完成所有這些。

1. 只需執行：

```shell
npm run server
```

此指令會使用 docker compose 執行 Grafana 容器，並將建置的外掛程式放置在正確的位置。

2. 若要驗證建置，請前往 `https://localhost:3000` 上的 Grafana。

## 步驟 7：測試外掛程式

1. 從 `https://localhost:3000` 上的 Grafana，使用預設認證：使用者名稱 `admin` 和密碼 `admin`。如果您未看到登入頁面，請按一下頁面頂端的 **Sign in** 並輸入認證。

2. 新增資料來源。由於我們在 `docker compose` 環境中執行，因此我們不需要安裝它，它將直接可用。前往 **Connections > Data sources**，使用左側選單，如下圖所示：

3. 新頁面會開啟，然後按一下 **Add data source**。Grafana 會開啟另一個頁面，您可以在其中搜尋我們剛才建立的資料來源名稱。

4. 按一下資料來源的卡片，然後在下一頁按一下 **Save & test**。

5. 按一下右上角的 **Build a dashboard**。在下一頁，按一下按鈕以 **+ Add visualization**。在對話方塊中，按一下新新增的資料來源。資料開始出現，如下所示：

6. 若要更清楚地視覺化資料，請將視覺化時間範圍變更為類似從現在起 1 分鐘，然後套用它。

此時，您應該會開始即時看到資料。如果您想，可以變更上限和下限或滴答間隔。這將產生一個新查詢，並且面板將會更新。如果您想，也可以新增更多查詢。

![Grafana 串流資料來源。](/img/streaming-data-source.gif)

您已成功建立同時具有前端和後端的 Grafana 串流資料來源外掛程式。此外掛程式已準備好讓您自訂以符合您的特定需求。

```

```
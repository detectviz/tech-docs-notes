---
id: add-support-for-externally-shared-dashboards
title: 新增對外部共用儀表板的支援
sidebar_label: 支援外部儀表板
description: 如何新增對外部共用儀表板（先前稱為公開儀表板）的支援。
keywords:
  - grafana
  - plugins
  - plugin
  - externally shared dashboards
  - public dashboards
  - data source
  - datasource
---

[外部共用儀表板](https://grafana.com/docs/grafana/latest/dashboards/share-dashboards-panels/shared-dashboards/#externally-shared-dashboards)（先前稱為公開儀表板）允許 Grafana 使用者與任何人共用其儀表板的存取權限，而無需將他們新增為其 Grafana 組織中的使用者。當以這種方式存取儀表板時，它會從後端擷取查詢，而不是從前端接收。這是為了避免暴露敏感資料和執行未經授權的查詢。

因此，有必要不將任何經過前端轉換的主體傳遞給請求，因為它不會在外部共用儀表板面板請求中使用。

:::note

前端資料來源與外部共用儀表板不相容。
若要將資料來源插件前端元件轉換為後端元件，請參閱
[將資料來源前端轉換為後端](./convert-a-frontend-datasource-to-backend)。

:::

## 在您的資料來源插件中支援外部共用儀表板

若要讓您的資料來源插件在外部共用儀表板範圍內運作，請遵循以下步驟：

1.  從 `DataSourceWithBackend` 擴充您的 DataSource 類別

    ```ts
    export class MyDataSourceClass extends DataSourceWithBackend<TQuery, TOptions> {
      // 您的邏輯
    }
    ```

2.  如有必要，使用您的自訂程式碼實作 `query` 方法。如果這會變更後端查詢回應（targets 屬性），請不要轉換請求主體。此主體在呼叫共用的外部儀表板端點時不會作為引數傳遞。

    然後，呼叫 `super.query(request)`。
    這就是呼叫外部共用儀表板端點的地方。

          ```ts
        export class MyDataSourceClass extends DataSourceWithBackend<TQuery, TOptions> {

           query(request: DataQueryRequest<TQuery>): Observable<DataQueryResponse> {
             // 您的邏輯
             return super.query(request).pipe(
                map((response) => {
                    // 您的邏輯
                })
             );
           }
        }

3.  在您的 `plugin.json` 中新增 `"backend": true`

    ```json title="src/plugin.json"
    "backend": true
    ```
---
id: advanced-custom-datasource
title: 自訂資料來源
---

如果您想要查詢自訂資源 API，可以註冊一個執行時資料來源。這樣做的好處是，您可以接著利用 `SceneQueryRunner`。

`SceneQueryRunner` 為您完成了許多複雜的工作，例如：

- 等待變數完成 (如果您的查詢相依於它們)
- 當時間範圍變更時重新執行變數
- 判斷變數在非作用中狀態下是否已變更

```typescript
class MyCustomDS extends RuntimeDataSource {
  query(request: DataQueryRequest<DataQuery>): Promise<DataQueryResponse> | Observable<DataQueryResponse> {
    return Promise.resolve({
      state: LoadingState.Done,
      data: [
        {
          fields: [{ name: 'Values', type: FieldType.number, values: [1, 2, 3], config: {} }],
          length: 3,
        },
      ],
    });
  }

  testDatasource(): Promise<TestDataSourceResponse> {
    return Promise.resolve({ status: 'success', message: 'OK' });
  }
}

// 為您的資料來源指定唯一的 pluginId 和 uid，以免與任何其他場景應用程式外掛程式衝突，這點很重要。
sceneUtils.registerRuntimeDataSource({ dataSource: new MyCustomDS('my-custom-ds', 'my-custom-ds-uid') });
```

您現在可以在 `SceneQueryRunner` 查詢中使用這個資料來源，方法是使用相同的 uid。如果您想要在同一個 `SceneQueryRunner` 中混合查詢標準資料來源和您的自訂資料來源，請使用混合資料來源。

範例：

```typescript
$data: new SceneQueryRunner({
  datasource: { uid: '-- Mixed --' },
  queries: [
    { refId: 'A', datasource: { uid: 'my-prometheus' }, expr: '<my prometheus query>' },
    { refId: 'B', datasource: { uid: 'my-custom-ds-uid' }, expr: '<my prometheus query>' },
  ],
});
```
---
id: advanced-callbacks
title: 使用回呼
---

`scenes-ml` 中的許多元件都允許在建構函式中傳入回呼。例如，在 `SceneOutlierDetector` 中：

```ts
const outlierDetector = new SceneOutlierDetector({
  onOutlierDetected: (outlier: Outlier) => {},
});
```

此回呼可用於建立更符合您場景需求的自訂體驗。

例如，您可能有一個自訂場景，顯示給定 Kubernetes 部署的所有 Pod。透過啟用離群值偵測器，您可以使用回呼來儲存所有行為異常的 Pod 和時間戳記，並渲染第二個面板以顯示這些 Pod 和時間戳記的日誌。
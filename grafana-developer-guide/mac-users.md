# macOS 使用者
以下是 macOS 特有的已知問題

## 連接埠 5000

您在 macOS 上可能會收到監聽 5000 連接埠的錯誤：

```text
docker: Error response from daemon: Ports are not available: exposing port TCP 0.0.0.0:5000 -> 0.0.0.0:0: listen tcp 0.0.0.0:5000: bind: address already in use.
```

在這種情況下，您可以在 macOS 系統偏好設定中停用「AirPlay 接收器」。

## 防火牆對話方塊
如果您看到防火牆對話方塊，可以點擊「拒絕」，因為通常不需要從您的電腦外部進行存取。

## 虛擬化工具鏈 make 目標的執行格式錯誤
如果您收到 `/go/src/github.com/argoproj/argo-cd/dist/mockery: cannot execute binary file: Exec format error`，這通常表示您在執行本地 `make` 目標後又執行了虛擬化的 `make` 目標。
要修復此問題並繼續使用虛擬化工具鏈，請刪除 `argo-cd/dist` 資料夾的內容。
如果之後您想再次執行本地工具鏈的 `make` 目標，請執行 `make install-tools-local` 以重新填入 `argo-cd/dist` 資料夾的內容。
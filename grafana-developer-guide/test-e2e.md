# E2E 測試

測試[目錄](https://github.com/argoproj/argo-cd/tree/master/test)包含 E2E 測試和測試應用程式。測試假設 Argo CD 服務已安裝到目前上下文中的 `argocd-e2e` 命名空間或叢集中。在執行測試之前，會建立一個一次性的
`argocd-e2e***` 命名空間。該一次性命名空間用作測試應用程式的目標命名空間。

[/test/e2e/testdata](https://github.com/argoproj/argo-cd/tree/master/test/e2e/testdata) 目錄包含各種 Argo CD 應用程式。在測試執行之前，該目錄會被複製到 `/tmp/argo-e2e***` 暫存目錄中，並在測試中透過檔案 URL 作為
Git 儲存庫使用：`file:///tmp/argo-e2e***`。

> [!NOTE]
> **Rancher Desktop 磁碟區共享**
>
> e2e git 伺服器在容器中執行。如果您正在使用 Rancher Desktop，您需要為
> e2e 容器啟用磁碟區共享，以便存取 testdata 目錄。為此，請將以下內容新增到
> `~/Library/Application\ Support/rancher-desktop/lima/_config/override.yaml` 並重新啟動 Rancher Desktop：
>
> ```yaml
> mounts:
> - location: /private/tmp
>   writable: true
> ```

## 在本機執行測試

### 使用虛擬化鏈
1.  啟動 e2e 版本 `make start-e2e`
2.  執行測試：`make test-e2e`

### 使用本地鏈
1.  啟動 e2e 版本 `make start-e2e-local`
2.  執行測試：`make test-e2e-local`

## 觀察測試結果

您可以使用 UI [http://localhost:4000/applications](http://localhost:4000/applications) 來觀察測試，使用者名稱為 `"admin"`，密碼為 `"password"`。

## E2E 測試執行的設定

Makefile 的 `start-e2e` 目標會在您的本機啟動 ArgoCD 的實例，其中大部分都需要一個網路監聽器。如果出於任何原因，您的機器上已有網路服務在相同的連接埠上監聽，那麼 e2e 測試將無法執行。您可以透過在執行 `make start-e2e` 之前設定以下環境變數來偏離預設值：

*   `ARGOCD_E2E_APISERVER_PORT`：`argocd-server` 的監聽器連接埠（預設：`8080`）
*   `ARGOCD_E2E_REPOSERVER_PORT`：`argocd-reposerver` 的監聽器連接埠（預設：`8081`）
*   `ARGOCD_E2E_DEX_PORT`：`dex` 的監聽器連接埠（預設：`5556`）
*   `ARGOCD_E2E_REDIS_PORT`：`redis` 的監聽器連接埠（預設：`6379`）
*   `ARGOCD_E2E_YARN_CMD`：用於透過 Yarn 啟動 UI 的命令（預設：`yarn`）

如果您變更了 `argocd-server` 的連接埠，請務必在執行 `make test-e2e` 之前也將 `ARGOCD_SERVER` 環境變數設定為指向該連接埠，例如 `export ARGOCD_SERVER=localhost:8888`，以便測試與正確的伺服器元件通訊。


## 測試隔離

在測試隔離與速度之間已做出一些努力來取得平衡。測試隔離如下，每個測試都會得到：

*   一個隨機的 5 個字元的 ID。
*   一個位於 `/tmp/argo-e2e/${id}` 的唯一 Git 儲存庫，其中包含 `testdata`。
*   一個 `argocd-e2e-ns-${id}` 命名空間。
*   一個應用程式的主要名稱 `argocd-e2e-${id}`。

## 故障排除

**測試無法刪除 `argocd-e2e-ns-*` 命名空間。**

這可能是由於指標伺服器造成的，請執行以下命令：

```bash
kubectl api-resources
```

如果它以狀態碼 1 結束，請執行：

```bash
kubectl delete apiservice v1beta1.metrics.k8s.io
```

從命名空間中移除 `/spec/finalizers`

```bash
kubectl edit ns argocd-e2e-ns-*
```
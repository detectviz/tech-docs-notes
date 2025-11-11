# 在本機執行 Argo CD

## 先決條件
1. [開發環境](development-environment.md)
2. [工具鏈指南](toolchain-guide.md)
3. [開發週期](development-cycle.md)

## 前言
在開發過程中，建議從在本機（K8s 叢集外部）執行 Argo CD 開始。這將大大加快開發速度，因為您不必不斷地建置、推送和安裝包含您最新變更的新 Argo CD Docker 映像。

在本機測試後，您可以進入第二階段，即建置 docker 映像、在您的叢集中執行 Argo CD 並進行進一步測試。

對於這兩種情況，您都需要一個可運作的 K8s 叢集，Argo CD 將在其中儲存其所有資源和設定。

為了在您的叢集中擁有所有必要的資源，您將從您的開發分支部署 Argo CD，然後縮減其所有實例。
這將確保您在叢集中擁有所有相關設定（例如 Argo CD Config Map 和 CRD），而實例本身是停止的。

### 將 Argo CD 資源部署到您的叢集

首先將安裝清單推送到 argocd 命名空間：

```shell
kubectl create namespace argocd
kubectl apply -n argocd --force -f manifests/install.yaml
```

您稍後啟動的服務假設您正在安裝 Argo CD 的命名空間中執行。您可以如下設定目前的上下文預設命名空間：

```bash
kubectl config set-context --current --namespace=argocd
```

### 縮減您叢集中的任何 Argo CD 實例

透過縮減部署來確保 Argo CD 沒有在您的開發叢集中執行：

```shell
kubectl -n argocd scale statefulset/argocd-application-controller --replicas 0
kubectl -n argocd scale deployment/argocd-dex-server --replicas 0
kubectl -n argocd scale deployment/argocd-repo-server --replicas 0
kubectl -n argocd scale deployment/argocd-server --replicas 0
kubectl -n argocd scale deployment/argocd-redis --replicas 0
kubectl -n argocd scale deployment/argocd-applicationset-controller --replicas 0
kubectl -n argocd scale deployment/argocd-notifications-controller --replicas 0
```

## 在本機（K8s 叢集外部）執行 Argo CD
#### 先決條件
1. [將 Argo CD 資源部署到您的叢集](running-locally.md#deploy-argo-cd-resources-to-your-cluster)
2. [縮減您叢集中的任何 Argo CD 實例](running-locally.md#scale-down-any-argo-cd-instance-in-your-cluster)

### 啟動本地服務（虛擬化工具鏈）
當您使用虛擬化工具鏈時，啟動本地服務就像執行一樣簡單

```bash
cd argo-cd
make start
```

預設情況下，Argo CD 使用 Docker。要改用 Podman，請在執行 `make` 命令之前將 `DOCKER` 環境變數設定為 `podman`：

```shell
cd argo-cd
DOCKER=podman make start
```

這將在 Docker 容器中啟動所有 Argo CD 服務和 UI，並向您的主機公開以下連接埠：

*   Argo CD API 伺服器，位於 8080 連接埠
*   Argo CD UI 伺服器，位於 4000 連接埠
*   Helm 註冊庫伺服器，位於 5000 連接埠

您現在可以使用網頁 UI（將您的瀏覽器指向 `http://localhost:4000`）或使用 CLI 對 `http://localhost:8080` 的 API 進行操作。請務必對 CLI 使用 `--insecure` 和 `--plaintext` 選項。Webpack 最初需要一些時間來打包資源，因此第一次載入頁面可能需要幾秒鐘或幾分鐘。

作為每次呼叫 `argocd` CLI 時都使用上述命令列參數的替代方案，您可以設定以下環境變數：

```bash
export ARGOCD_SERVER=127.0.0.1:8080
export ARGOCD_OPTS="--plaintext --insecure"
```

### 啟動本地服務（本地工具鏈）
當您使用本地工具鏈時，可以透過 3 種方式啟動本地服務：

#### 使用 "make start-local"
```shell
cd argo-cd
make start-local ARGOCD_GPG_ENABLED=false
```

#### 使用 "make run"
```shell
cd argo-cd
make run ARGOCD_GPG_ENABLED=false
```

#### 使用 "goreman start"
```shell
cd argo-cd
ARGOCD_GPG_ENABLED=false && goreman start
```

任何這些選項都將啟動所有 Argo CD 服務和 UI：

*   Argo CD API 伺服器，位於 8080 連接埠
*   Argo CD UI 伺服器，位於 4000 連接埠
*   Helm 註冊庫伺服器，位於 5000 連接埠


檢查所有程式是否已啟動：

```text
$ goreman run status
*controller
*api-server
[...]
```

如果某些程序無法啟動（未標記為 `*`），請檢查日誌以了解它們未執行的原因。日誌預設為 `DEBUG` 級別。如果日誌太雜亂而無法找到問題，請嘗試編輯 Argo CD repo 根目錄中 `Procfile` 中命令的日誌級別。

您現在可以使用網頁 UI（將您的瀏覽器指向 `http://localhost:4000`）或使用 CLI 對 `http://localhost:8080` 的 API 進行操作。請務必對 CLI 使用 `--insecure` 和 `--plaintext` 選項。Webpack 最初需要一些時間來打包資源，因此第一次載入頁面可能需要幾秒鐘或幾分鐘。

作為每次呼叫 `argocd` CLI 時都使用上述命令列參數的替代方案，您可以設定以下環境變數：

```bash
export ARGOCD_SERVER=127.0.0.1:8080
export ARGOCD_OPTS="--plaintext --insecure"
```
### 在您的機器上執行 Argo CD 時進行程式碼變更

#### 文件變更

修改文件會自動重新載入[文件網站](https://argo-cd.readthedocs.io/)上的變更，該網站可以使用 `make serve-docs-local` 命令在本機建置。
執行後，您可以在 8000 連接埠上檢視您在本機建置的文件。

在這裡閱讀更多相關資訊[https://argo-cd.readthedocs.io/en/latest/developer-guide/docs-site/](https://argo-cd.readthedocs.io/en/latest/developer-guide/docs-site/)。

#### UI 變更

修改使用者介面（透過編輯 .tsx 或 .scss 檔案）會自動在 4000 連接埠上重新載入變更。

#### 後端變更

修改 API 伺服器、repo 伺服器或控制器需要重新啟動目前的 `make start`（對於虛擬化工具鏈）。
對於使用本地工具鏈的 `make start-local`，只需重新建置並重新啟動相應的服務即可：

```sh
# 以在 repo 伺服器 Go 程式碼上工作為例，請參閱 `Procfile` 中的其他服務名稱
goreman run restart repo-server
```

#### CLI 變更

修改 CLI 需要重新啟動目前的 `make start` 或 `make start-local` 工作階段才能反映變更。這些目標也會重新建置 CLI。

要測試大多數 CLI 命令，您需要登入。

首先，取得自動產生的 secret：

```shell
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```

然後使用該密碼和使用者名稱 `admin` 登入：

```shell
dist/argocd login localhost:8080
```

## 在 K8s 叢集內部執行 Argo CD
### 在您的叢集中擴展 Argo CD

當您在本機完成測試變更並希望將 Argo CD 帶回您的開發叢集時，只需再次擴展部署即可：

```bash
kubectl -n argocd scale statefulset/argocd-application-controller --replicas 1
kubectl -n argocd scale deployment/argocd-applicationset-controller --replicas 1
kubectl -n argocd scale deployment/argocd-dex-server --replicas 1
kubectl -n argocd scale deployment/argocd-repo-server --replicas 1
kubectl -n argocd scale deployment/argocd-server --replicas 1
kubectl -n argocd scale deployment/argocd-redis --replicas 1
kubectl -n argocd scale deployment/argocd-notifications-controller --replicas 1
```

### 在您的叢集上執行您自己的 Argo CD 映像

對於您的最終測試，可能需要建置您自己的映像並在您的開發叢集中執行它們。

#### 建立 Docker 帳戶並登入

如果您還沒有 [Docker Hub](https://hub.docker.com) 帳戶，您可能需要建立一個。建立帳戶後，從您的開發環境登入：

```bash
docker login
```

#### 建立並推送 Docker 映像

您需要將建置的映像推送到您自己的 Docker 命名空間：

```bash
export IMAGE_NAMESPACE=youraccount
```

如果您未在環境中設定 `IMAGE_TAG`，則將使用預設的 `:latest`。要更改標籤，請在環境中匯出變數：

```bash
export IMAGE_TAG=1.5.0-myrc
```

然後您可以一步完成建置和推送映像：

```bash
DOCKER_PUSH=true make image
```

#### 為您的映像設定清單

在 `IMAGE_NAMESPACE` 和 `IMAGE_TAG` 仍然設定的情況下，執行：

```bash
make manifests
```

或

```bash
make manifests-local
```

（取決於您的工具鏈）以建置一組新的安裝清單，其中包含您特定的映像參考。

> [!NOTE]
> 不要將這些清單提交到您的儲存庫。如果您想還原變更，最簡單的方法是從您的環境中取消設定 `IMAGE_NAMESPACE` 和 `IMAGE_TAG`，然後再次執行 `make manifests`。這將重新建立預設清單。

#### 使用自訂清單設定您的叢集

最後一步是將清單推送到您的叢集，以便它會拉取並執行您的映像：

```bash
kubectl apply -n argocd --force -f manifests/install.yaml
```
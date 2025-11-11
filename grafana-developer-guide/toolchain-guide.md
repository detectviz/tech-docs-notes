# 開發工具鏈

## 先決條件
[開發環境](development-environment.md)

## 本地 vs 虛擬化工具鏈

Argo CD 提供了一個使用 Docker 映像的完全虛擬化的開發和測試工具鏈。這些映像提供了與最終產品相同的執行環境，並且更容易與工具鏈和依賴項的變更保持同步。虛擬化工具鏈使用測試工具映像在 Docker 容器內執行建置和程式。這使得一切都是可重複的。需求的動態性是選擇此工具鏈的另一個原因。此設定可能還需要設定安裝本地 K8s 叢集時附帶的預設 K8s API URL。

本地工具鏈可以加快開發和測試週期。特別是在 Docker 和 Linux 核心在 VM 內執行的 macOS 上，您可能想嘗試完全在本機開發。本地工具鏈還需要在您的機器上安裝額外的工具。此工具鏈是使用 IDE 偵錯器的不錯選擇。

`Makefile` 中與建置和測試週期最相關的目標提供了兩種變體，其中一種以 `-local` 為後綴。例如，`make test` 將在 Docker 容器（虛擬化工具鏈）中執行單元測試，`make test-local`（本地工具鏈）將在本機系統上原生執行它。

### 設定虛擬化工具鏈

如果您要使用虛擬化工具鏈，請記住以下幾點：

*   您的 Kubernetes API 伺服器必須在您的本地機器或 VM 的介面上監聽，而不能僅在 `127.0.0.1` 或 `localhost` 上監聽。
*   您的 Kubernetes 客戶端設定 (`~/.kube/config`) 不得使用指向 `localhost`、`127.0.0.1` 或 `0.0.0.0` 的 API URL。

虛擬化工具鏈的 Docker 容器將使用您工作站的以下本地掛載，並可能修改其內容：

*   `~/go/src` - 您的 Go 工作區的原始碼目錄（預期會修改）
*   `~/.cache/go-build` - 您的 Go 建置快取（預期會修改）
*   `~/.kube` - 您的 Kubernetes 客戶端設定（不修改）

#### macOS 上的已知問題
[已知問題](mac-users.md)

#### Docker 權限

如果您選擇使用虛擬化工具鏈，您將需要具有與 Docker 守護程序互動的適當權限。不建議以 root 使用者身份工作，如果您的使用者沒有與 Docker 使用者通訊的權限，但您在系統上設定了 `sudo`，您可以將環境變數 `SUDO` 設定為 `sudo`，以便建置指令碼使用 sudo 執行對 `docker` CLI 的任何呼叫，而不影響建置指令碼的其他部分（應以您的普通使用者權限執行）。

您可以在呼叫 `make` 之前設定此項，例如：

```
SUDO=sudo make sometarget
```

或者您可以選擇將其永久匯出到您的環境中，例如
```
export SUDO=sudo
```

#### 使用 Podman
為了使用 podman 在本機執行和測試 Argo CD，請在執行 `make` 之前將 `DOCKER` 環境變數設定為 `podman`，例如：

```
DOCKER=podman make start
```
如果您已安裝 podman，則可以利用其無根模式。

#### 建置必要的 Docker 映像

執行 `make test-tools-image` 來建置必要的 Docker 映像。此映像提供了虛擬化工具鏈的環境。

用於建置這些映像的 `Dockerfile` 可以在 `test/container/Dockerfile` 中找到。

#### 為從建置容器連線設定您的 K8s 叢集
##### K3d
K3d 是一個最小的 Kubernetes 發行版，在 docker 中。因為它在 docker 容器中執行，所以在您使用 k3d 時，您正在處理 docker 的內部網路規則。在您的本地機器上執行的典型 Kubernetes 叢集是您所在網路的一部分，因此您可以使用 **kubectl** 存取它。然而，在 docker 容器（在本例中是由 make 啟動的那個）中執行的 Kubernetes 叢集無法從容器內部存取 0.0.0.0，當 0.0.0.0 是容器本身（和/或容器的網路）之外的網路資源時。這是一個完全自包含、可拋棄的 Kubernetes 叢集的代價。

您將需要為 Argo CD 虛擬化工具鏈進行的設定：

1.  在 Mac/Linux 上執行 `ifconfig`，在 Windows 上執行 `ipconfig` 來找到您的主機 IP。對於大多數使用者，以下命令可以找到 IP 位址。

    *   對於 Mac：

    ```
    IP=`ifconfig en0 | grep inet | grep -v inet6 | awk '{print $2}'`
    echo $IP
    ```

    *   對於 Linux：

    ```
    IP=`ifconfig eth0 | grep inet | grep -v inet6 | awk '{print $2}'`
    echo $IP
    ```

    請記住，此 IP 是由路由器動態分配的，因此如果您的路由器因任何原因重新啟動，您的 IP 可能會變更。

2.  編輯您的 ~/.kube/config 並將 0.0.0.0 替換為上述 IP 位址，刪除叢集憑證並新增 `insecure-skip-tls-verify: true`

3.  執行 `kubectl version` 以確保您仍然可以透過此新 IP 連線到 Kubernetes API 伺服器。

##### Minikube

預設情況下，minikube 將會建立使用檔案中驗證資料的 Kubernetes 客戶端設定。這與虛擬化工具鏈不相容。因此，如果您打算使用虛擬化工具鏈，則必須將此驗證資料嵌入到客戶端設定中。為此，請使用 `minikube start --embed-certs` 啟動 minikube。另請注意，目前不支援使用 Docker 驅動程式的 minikube 與虛擬化工具鏈，因為 Docker 驅動程式會在硬式編碼的 127.0.0.1 上公開 API 伺服器。

#### 測試從建置容器到您的 K8s 叢集的連線

您可以透過執行 `make verify-kube-connect` 來測試虛擬化工具鏈是否可以存取您的 Kubernetes 叢集，該命令將在用於執行所有測試的 Docker 容器內執行 `kubectl version`。


如果您收到類似以下的錯誤：

```
The connection to the server 127.0.0.1:6443 was refused - did you specify the right host or port?
make: *** [Makefile:386: verify-kube-connect] Error 1
```

您應該編輯您的 `~/.kube/config` 並修改 `server` 選項以指向您正確的 K8s API（如上所述）。

### 設定本地工具鏈

#### 安裝 `node`

<https://nodejs.org/en/download>

#### 安裝 `yarn`

<https://classic.yarnpkg.com/lang/en/docs/install/>

#### 安裝 `goreman`

<https://github.com/mattn/goreman#getting-started>

Goreman 用於啟動所有必要的程序以取得一個可運作的 Argo CD 開發環境（在 `Procfile` 中定義）

#### 安裝必要的依賴項和建置工具

> [!NOTE]
> 安裝說明僅適用於 Linux 主機。Mac 說明將很快提供。

為了在您的本地系統上安裝建置和測試 Argo CD 所需的工具，我們提供了方便的安裝程式指令碼。預設情況下，它們會將二進位檔安裝到您系統的 `/usr/local/bin` 中，這可能需要 `root` 權限。

您可以在執行安裝程式指令碼之前透過設定 `BIN` 環境來更改目標位置。例如，您可以將二進位檔安裝到 `~/go/bin` 中（然後該目錄應為您 `PATH` 環境中的第一個元件，即 `export PATH=~/go/bin:$PATH`）：

```shell
BIN=~/go/bin make install-tools-local
```

此外，您必須透過作業系統的套件管理器至少安裝以下工具（此列表可能並非總是最新）：

*   Git LFS 外掛程式
*   GnuPG 第 2 版
# 設定開發環境

## 必要工具概覽

您需要安裝以下工具，並符合指定的最低版本：

*   Git (v2.0.0+)
*   Go (`go.mod` 中指定的版本 - 使用 `go version` 檢查)
*   Docker (v20.10.0+) 或 Podman (v3.0.0+)
*   Kind (v0.11.0+) 或 Minikube (v1.23.0+) 或 K3d (v5.7.3+)



## 安裝必要工具

### 安裝 Git

顯然，您需要一個 `git` 客戶端來拉取原始碼並推送您的變更。

<https://github.com/git-guides/install-git>


### 安裝 Go

您需要在您的開發環境中安裝並運作 Go SDK 及相關工具（例如 GNU `make`）。

<https://go.dev/doc/install/>

安裝 Go 的版本需等於或大於 `go.mod` 中列出的版本（使用 `go version` 驗證 go 版本）。
我們將假設您的 Go 工作區位於 `~/go`。

驗證：執行 `go version`

### 安裝 Docker 或 Podman

#### Docker 安裝指南

<https://docs.docker.com/engine/install/>

您將需要一個可運作的 Docker 執行環境，以便能夠建置和執行映像。Argo CD 使用多階段建置。

驗證：執行 `docker version`

#### Podman 安裝指南

<https://podman.io/docs/installation>

### 安裝本地 K8s 叢集

您不需要一個功能齊全的多主節點、多節點叢集，但您需要像 K3S、K3d、Minikube、Kind 或 microk8s 這樣的東西。您還需要在您的開發環境中有一個可運作的 Kubernetes 客戶端 (`kubectl`) 設定。該設定必須位於 `~/.kube/config`。

#### Kind

##### [安裝指南](https://kind.sigs.k8s.io/docs/user/quick-start)

您可以使用 `kind` 在 Docker 內執行 Kubernetes。但指向任何其他開發叢集也同樣可行，只要 Argo CD 能夠連線到它。

##### 啟動叢集
```shell
kind create cluster
```

#### Minikube

##### [安裝指南](https://minikube.sigs.k8s.io/docs/start)

##### 啟動叢集
```shell
minikube start
```

或者，如果您正在使用帶有 podman 驅動程式的 minikube：

```shell
minikube start --driver=podman
```

#### K3d

##### [安裝指南](https://k3d.io/stable/#quick-start)

### 驗證叢集安裝

*   執行 `kubectl version`

## Fork 並複製儲存庫
1.  將 Argo CD 儲存庫 Fork 到您的個人 GitHub 帳戶
2.  複製已 Fork 的儲存庫：
```shell
git clone https://github.com/YOUR-USERNAME/argo-cd.git
```
   請注意，本地建置過程使用 GOPATH，除非 Argo CD 儲存庫直接複製到其中，否則不應使用該路徑。

3.  雖然每個人都有自己的 Git 工作流程，但本文件的作者建議在您的本地副本中建立一個名為 `upstream` 的遠端，指向原始的 Argo CD 儲存庫。這樣，您可以透過將 Argo CD 儲存庫的最新變更合併到您的本地分支中來輕鬆地保持您的本地分支為最新狀態，例如，在您本地簽出的分支中執行 `git pull upstream master`。
   要建立遠端，請執行：
   ```shell
   cd argo-cd
   git remote add upstream https://github.com/argoproj/argo-cd.git
   ```

## 安裝額外的必要開發工具

```shell
make install-go-tools-local
make install-codegen-tools-local
```

## 在您的本地叢集上安裝最新的 Argo CD

```shell
kubectl create namespace argocd &&
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/master/manifests/install.yaml
```

設定 kubectl 設定以避免在每個 kubectl 命令中指定命名空間。

```shell
kubectl config set-context --current --namespace=argocd
```
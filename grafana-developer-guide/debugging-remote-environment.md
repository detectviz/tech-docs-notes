# 對遠端 ArgoCD 環境進行偵錯

在本指南中，我們將描述如何使用 [Telepresence](https://telepresence.io/) 來對遠端 ArgoCD 環境進行偵錯。

Telepresence 允許您連接並偵錯部署在遠端環境中的服務，並「挑選」一個服務在本機執行，同時保持與遠端叢集的連接。這將：

*   減少本機的資源佔用
*   縮短反饋迴圈時間
*   對交付的程式碼更有信心。

要了解更多資訊，請參閱官方文件 [telepresence.io](https://telepresence.io/) 或 [Medium](https://medium.com/containers-101/development-environment-using-telepresence-634bd7210c26)。

## 安裝 ArgoCD
首先，在您的叢集上安裝 ArgoCD
```shell
kubectl create ns argocd
curl -sSfL https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml | kubectl apply -n argocd -f -
```

## 連接
連接到其中一個服務，例如，要偵錯主要的 ArgoCD 伺服器，請執行：
```shell
kubectl config set-context --current --namespace argocd
telepresence helm install --set-json agent.securityContext={} # 將 telepresence 安裝到您的叢集中
telepresence connect # 開始連接到您的叢集（綁定到目前的命名空間）
telepresence intercept argocd-server --port 8080:http --env-file .envrc.remote # 開始攔截
```
*   `--port` 將遠端 http 連接埠的流量轉發到本機的 8080（如果 argocd-server 終止 TLS，請使用 `--port 8080:https`）
*   `--env-file` 將遠端 pod 的所有環境變數寫入一個本地檔案，這些變數也會設定在 `--run` 命令的子程序上

如此一來，任何到達您叢集中 argocd-server 服務的流量（例如，透過 LB / ingress）都將被轉發到您筆記型電腦的 8080 連接埠。這樣您現在可以在本機啟動 argocd-server 來偵錯或測試新程式碼。如果您使用 `.envrc.remote` 中的環境變數啟動 argocd-server，它能夠從叢集中取得所有的 configmap、secret 等，並透明地連接到其他微服務，因此應該不需要進一步的設定，其行為與在叢集中完全相同。

使用以下命令列出 Telepresence 的目前狀態：
```shell
telepresence status
```

使用以下命令停止攔截：
```shell
telepresence leave argocd-server
```

並從您的叢集中卸載 telepresence：
```shell
telepresence helm uninstall
```

有關如何使用 Telepresence 攔截服務的更多資訊，請參閱[此快速入門](https://www.telepresence.io/docs/latest/quick-start/)。

### 連接 (telepresence v1)
請改用以下命令：
```shell
telepresence --swap-deployment argocd-server --namespace argocd --env-file .envrc.remote --expose 8080:8080 --expose 8083:8083 --run bash
```
*   `--swap-deployment` 更改 argocd-server 的部署
*   `--expose` 將遠端 8080 和 8083 連接埠的流量轉發到本地相同的連接埠
*   `--env-file` 將遠端 pod 的所有環境變數寫入一個本地檔案，這些變數也會設定在 `--run` 命令的子程序上
*   `--run` 定義建立連接後要執行的命令，可使用 `bash`、`zsh` 或其他

## 偵錯
建立連接後，使用您喜歡的工具在本機啟動伺服器。

### 終端機
*   編譯 `make server`
*   執行 `./dist/argocd-server`

### VSCode
在 VSCode 中，使用以下啟動設定來執行 argocd-server：

```json
        {
            "name": "Launch argocd-server",
            "type": "go",
            "request": "launch",
            "mode": "auto",
            "program": "${workspaceFolder}/cmd/main.go",
            "envFile": [
                "${workspaceFolder}/.envrc.remote",
            ],
            "env": {
                "ARGOCD_BINARY_NAME": "argocd-server",
                "CGO_ENABLED": "0",
                "KUBECONFIG": "/path/to/kube/config"
            }
        }
```
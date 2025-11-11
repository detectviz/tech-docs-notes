# Tilt 開發

[Tilt](https://tilt.dev/) 提供了一個即時的網頁 UI，可以更好地查看日誌、健康狀態和依賴項，與僅依賴終端輸出相比，使偵錯更容易。只需一個 `tilt up` 命令，開發人員就可以啟動所有必要的服務，而無需手動管理多個程序，從而簡化了本地開發工作流程。Tilt 還與 Docker 和 Kubernetes 無縫整合，實現了高效的基於容器的開發。與缺乏動態設定重載的 goreman 不同，Tilt 可以檢測並應用對 Kubernetes YAML 和 Helm chart 的變更，而無需完全重新啟動，使其在迭代開發中更有效率。

### 先決條件
*   kubernetes 環境（kind、minikube、k3d 等）
*   tilt (`brew install tilt`)
*   kustomize
*   kubectl

### 執行
1.  在 repo 的根目錄中執行 `tilt up` 來啟動環境
    *   資源將被部署到您 `kubeconfig` 目前指向的叢集中的 `argocd` 命名空間。

2.  使用 `ctrl+c` 來關閉 tilt，這會停止監看檔案變更並關閉通訊埠轉發。部署到本地叢集的所有內容將保持不變並繼續執行。再次執行 `tilt up` 以啟動另一個工作階段並從您上次離開的地方繼續。

### 清理
要移除您本地叢集中所有已部署的資源，包括 CRD，請從 repo 的根目錄執行 `tilt down`。

### 通訊埠轉發
通訊埠轉發會自動從叢集設定到 localhost 主機，用於以下連接埠：

| 部署 | API | 指標 | Webhook | 偵錯 |
|------------|-----|---------|---------|-------|
| argocd-server | 8080 | 8083 | | 9345 |
| argocd-repo-server | 8081 | 8084 | | 9346 |
| argocd-redis | 6379 | | | |
| argocd-applicationset-controller | | 8085 | 7000 | 9347 |
| argocd-application-controller | | 8086 | | 9348 |
| argocd-notifications-controller | | 8087 | | 9349 |
| argocd-commit-server | 8089 | 8088 | | 9350 |

### 偵錯 ArgoCD
每個執行 ArgoCD 元件的已部署 pod 都使用 delve 來公開一個偵錯連接埠。Tilt 被設定為將這些連接埠中的每一個都本地轉發到 `localhost`。IDE 可以附加到相應的應用程式以設定中斷點並偵錯在叢集內部執行的程式碼。

| 部署 | 偵錯主機連接埠 |
|-----------|------------|
| argocd-server | localhost:9345 |
| argocd-repo-server | localhost:9346 |
| argocd-applicationset-controller | localhost:9347 |
| argocd-application-controller | localhost:9348 |
| argocd-notifications-controller | localhost:9349 |
| argocd-commit-server | localhost:9350 |


#### VS Code
新增一個 `.vscode/launch.json` 檔案，其中包含這些設定，以支援附加到與服務對應的正在執行的 pod。


```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Connect to server",
            "type": "go",
            "request": "attach",
            "mode": "remote",
            "remotePath": "${workspaceFolder}",
            "port": 9345,
            "host": "127.0.0.1"
        },
        {
            "name": "Connect to repo-server",
            "type": "go",
            "request": "attach",
            "mode": "remote",
            "remotePath": "${workspaceFolder}",
            "port": 9346,
            "host": "127.0.0.1"
        },
        {
            "name": "Connect to applicationset-controller",
            "type": "go",
            "request": "attach",
            "mode": "remote",
            "remotePath": "${workspaceFolder}",
            "port": 9347,
            "host": "127.0.0.1"
        },
        {
            "name": "Connect to application-controller",
            "type": "go",
            "request": "attach",
            "mode": "remote",
            "remotePath": "${workspaceFolder}",
            "port": 9348,
            "host": "127.0.0.1"
        },
        {
            "name": "Connect to notifications-controller",
            "type": "go",
            "request": "attach",
            "mode": "remote",
            "remotePath": "${workspaceFolder}",
            "port": 9349,
            "host": "127.0.0.1"
        },
        {
            "name": "Connect to commit-server",
            "type": "go",
            "request": "attach",
            "mode": "remote",
            "remotePath": "${workspaceFolder}",
            "port": 9350,
            "host": "127.0.0.1"
        }
    ]
}
```

#### Goland
新增一個 `.run/remote-debugging.run.xml` 檔案，其中包含這些設定，以支援附加到與服務對應的正在執行的 pod。

```xml
<component name="ProjectRunConfigurationManager">
    <configuration default="false" name="Connect to server" type="GoRemoteDebugConfigurationType" factoryName="Go Remote" focusToolWindowBeforeRun="true" port="9345">
        <option name="disconnectOption" value="LEAVE" />
        <disconnect value="LEAVE" />
        <method v="2" />
    </configuration>
    <configuration default="false" name="Connect to repo-server" type="GoRemoteDebugConfigurationType" factoryName="Go Remote" focusToolWindowBeforeRun="true" port="9346">
        <option name="disconnectOption" value="LEAVE" />
        <disconnect value="LEAVE" />
        <method v="2" />
    </configuration>
    <configuration default="false" name="Connect to applicationset-controller" type="GoRemoteDebugConfigurationType" factoryName="Go Remote" focusToolWindowBeforeRun="true" port="9347">
        <option name="disconnectOption" value="LEAVE" />
        <disconnect value="LEAVE" />
        <method v="2" />
    </configuration>
    <configuration default="false" name="Connect to application-controller" type="GoRemoteDebugConfigurationType" factoryName="Go Remote" focusToolWindowBeforeRun="true" port="9348">
        <option name="disconnectOption" value="LEAVE" />
        <disconnect value="LEAVE" />
        <method v="2" />
    </configuration>
    <configuration default="false" name="Connect to notifications-controller" type="GoRemoteDebugConfigurationType" factoryName="Go Remote" focusToolWindowBeforeRun="true" port="9349">
        <option name="disconnectOption" value="LEAVE" />
        <disconnect value="LEAVE" />
        <method v="2" />
    </configuration>
    <configuration default="false" name="Connect to commit-server" type="GoRemoteDebugConfigurationType" factoryName="Go Remote" focusToolWindowBeforeRun="true" port="9350">
        <option name="disconnectOption" value="LEAVE" />
        <disconnect value="LEAVE" />
        <method v="2" />
    </configuration>
</component>
```
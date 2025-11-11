# 在本機對 Argo CD 實例進行偵錯

## 先決條件
1. [開發環境](development-environment.md)
2. [工具鏈指南](toolchain-guide.md)
3. [開發週期](development-cycle.md)
4. [在本機執行](running-locally.md)

## 前言
請確保您熟悉使用[本地工具鏈](running-locally.md#start-local-services-local-toolchain)在本機執行 Argo CD。

當在本機執行 Argo CD 進行手動測試時，最快的方法是將所有 Argo CD 元件一起執行，如[在本機執行](running-locally.md)中所述。

然而，當您需要對單個 Argo CD 元件（例如 `api-server`、`repo-server` 等）進行偵錯時，您需要在您的 IDE 中單獨執行此元件，使用您的 IDE 啟動和偵錯設定，而其他元件將如前所述，使用本地工具鏈執行。

在接下來的步驟中，我們將以 Argo CD `api-server` 為例，說明如何在 IDE 中執行一個元件。

## 設定您的 IDE

### 在 `Procfile` 中找到您的元件設定
當使用本地工具鏈在本機執行 Argo CD 時，Goreman 會使用 `Procfile`。該檔案位於您複製的 Argo CD repo 資料夾的頂層目錄中，您可以在[這裡](https://github.com/argoproj/argo-cd/blob/master/Procfile)查看其最新版本。它包含了所有需要的元件執行設定，您需要將此設定的一部分複製到您的 IDE 中。

`Procfile` 中 `api-server` 設定的範例：
``` text
api-server: [ "$BIN_MODE" = 'true' ] && COMMAND=./dist/argocd || COMMAND='go run ./cmd/main.go' && sh -c "GOCOVERDIR=${ARGOCD_COVERAGE_DIR:-/tmp/coverage/api-server} FORCE_LOG_COLORS=1 ARGOCD_FAKE_IN_CLUSTER=true ARGOCD_TLS_DATA_PATH=${ARGOCD_TLS_DATA_PATH:-/tmp/argocd-local/tls} ARGOCD_SSH_DATA_PATH=${ARGOCD_SSH_DATA_PATH:-/tmp/argocd-local/ssh} ARGOCD_BINARY_NAME=argocd-server $COMMAND --loglevel debug --redis localhost:${ARGOCD_E2E_REDIS_PORT:-6379} --disable-auth=${ARGOCD_E2E_DISABLE_AUTH:-'true'} --insecure --dex-server http://localhost:${ARGOCD_E2E_DEX_PORT:-5556} --repo-server localhost:${ARGOCD_E2E_REPOSERVER_PORT:-8081} --port ${ARGOCD_E2E_APISERVER_PORT:-8080} --otlp-address=${ARGOCD_OTLP_ADDRESS} --application-namespaces=${ARGOCD_APPLICATION_NAMESPACES:-''} --hydrator-enabled=${ARGOCD_HYDRATOR_ENABLED:='false'}"
```
此設定範例將作為後續步驟的基礎。

> [!NOTE]
> 元件的 Procfile 可能會隨著時間而改變。請仔細閱讀 Procfile 並確保您使用最新的設定進行偵錯。

### 設定元件環境變數
您將在 IDE 中執行以進行偵錯的元件（在我們的例子中是 `api-server`）將需要環境變數。從您開發分支的 `argo-cd` 根資料夾中的 `Procfile` 複製環境變數。環境變數位於元件執行命令的 `sh -c` 部分中的 `$COMMAND` 部分之前。
您可以將它們保存在 `.env` 檔案中，然後讓 IDE 啟動設定指向該檔案。顯然，在偵錯特定設定時，您可以根據需要調整環境變數。

`api-server.env` 檔案的範例：
``` bash
ARGOCD_BINARY_NAME=argocd-server
ARGOCD_FAKE_IN_CLUSTER=true
ARGOCD_GNUPGHOME=/tmp/argocd-local/gpg/keys
ARGOCD_GPG_DATA_PATH=/tmp/argocd-local/gpg/source
ARGOCD_GPG_ENABLED=false
ARGOCD_LOG_FORMAT_ENABLE_FULL_TIMESTAMP=1
ARGOCD_SSH_DATA_PATH=/tmp/argocd-local/ssh
ARGOCD_TLS_DATA_PATH=/tmp/argocd-local/tls
ARGOCD_TRACING_ENABLED=1
FORCE_LOG_COLORS=1
KUBECONFIG=/Users/<YOUR_USERNAME>/.kube/config # 必須是絕對完整路徑
...
# 等等，例如：當您測試 app-in-any-namespace 功能時，
# 您需要將 ARGOCD_APPLICATION_NAMESPACES 加入到此列表中
# 僅用於測試此功能，之後再將其移除。
```

### 安裝 DotENV / EnvFile 外掛程式
使用您 IDE 的市集/外掛程式管理員。以下範例設定需要安裝該外掛程式。


### 設定元件 IDE 啟動設定
#### VSCode 範例
接下來，您需要建立一個帶有相關參數的啟動設定。從您開發分支的 `argo-cd` 根資料夾中的 `Procfile` 複製參數。參數位於元件執行命令的 `sh -c` 部分中的 `$COMMAND` 部分之後。
`api-server` 啟動設定的範例，基於我們上面 `Procfile` 中 `api-server` 設定的範例：
``` json
    {
      "name": "api-server",
      "type": "go",
      "request": "launch",
      "mode": "auto",
      "program": "YOUR_CLONED_ARGO_CD_REPO_PATH/argo-cd/cmd",
      "args": [
        "--loglevel",
        "debug",
        "--redis",
        "localhost:6379",
        "--repo-server",
        "localhost:8081",
        "--dex-server",
        "http://localhost:5556",
        "--port",
        "8080",
        "--insecure"
      ],
      "envFile": "YOUR_ENV_FILES_PATH/api-server.env", # 假設您已安裝 DotENV 外掛程式
    }
```

#### Goland 範例
接下來，您需要建立一個帶有相關參數的啟動設定。從您開發分支的 `argo-cd` 根資料夾中的 `Procfile` 複製參數。參數位於元件執行命令的 `sh -c` 部分中的 `$COMMAND` 部分之後。
`api-server` 啟動設定片段的範例，基於我們上面 `Procfile` 中 `api-server` 設定的範例：
``` xml
<component name="ProjectRunConfigurationManager">
  <configuration default="false" name="api-server" type="GoApplicationRunConfiguration" factoryName="Go Application">
    <module name="argo-cd" />
    <working_directory value="$PROJECT_DIR$" />
    <parameters value="--loglevel debug --redis localhost:6379 --insecure --dex-server http://localhost:5556 --repo-server localhost:8081 --port 8080" />
    <EXTENSION ID="net.ashald.envfile"> <!-- 假設您已安裝 EnvFile 外掛程式-->
      <option name="IS_ENABLED" value="true" />
      <option name="IS_SUBST" value="false" />
      <option name="IS_PATH_MACRO_SUPPORTED" value="false" />
      <option name="IS_IGNORE_MISSING_FILES" value="false" />
      <option name="IS_ENABLE_EXPERIMENTAL_INTEGRATIONS" value="false" />
      <ENTRIES>
        <ENTRY IS_ENABLED="true" PARSER="runconfig" IS_EXECUTABLE="false" />
        <ENTRY IS_ENABLED="true" PARSER="env" IS_EXECUTABLE="false" PATH="<YOUR_ENV_FILES_PATH>/api-server.env" />
      </ENTRIES>
    </EXTENSION>
    <kind value="DIRECTORY" />
    <directory value="$PROJECT_DIR$/cmd" />
    <filePath value="$PROJECT_DIR$" />
    <method v="2" />
  </configuration>
</component>
```

> [!NOTE]
> 作為將上述檔案匯入 Goland 的替代方案，您可以使用官方的 [Goland 文件](https://www.jetbrains.com/help/go/go-build.html) 建立一個執行/偵錯設定，並僅從上面的範例中複製 `parameters`、`directory` 和 `PATH` 部分（在執行/偵錯設定精靈中將 `Run kind` 指定為 `Directory`）

## 在沒有被偵錯元件的情況下執行 Argo CD
接下來，我們需要執行除了被偵錯元件之外的所有 Argo CD 元件（因為我們將在 IDE 中單獨執行此元件）。
執行其他元件的方法可以混合搭配——您可以在您的 K8s 叢集中執行它們，也可以使用本地工具鏈在本機執行它們。
以下是不同的選項。

### 在本機執行其他元件
#### 使用 "make start-local" 執行
`make start-local` 預設會執行所有元件，但也可以使用白名單來執行，從而實現我們需要的分離。

因此，在偵錯 `api-server` 的情況下，請執行：
`make start-local ARGOCD_START="notification applicationset-controller repo-server redis dex controller ui"`

#### 使用 "make run" 執行
`make run` 預設會執行所有元件，但也可以使用黑名單來執行，從而實現我們需要的分離。

因此，在偵錯 `api-server` 的情況下，請執行：
`make run exclude=api-server`

#### 使用 "goreman start" 執行
`goreman start` 預設會執行所有元件，但也可以使用白名單來執行，從而視需要進行分離。

要偵錯 `api-server`，請執行：
`goreman start notification applicationset-controller repo-server redis dex controller ui`

## 從您的 IDE 執行 Argo CD 被偵錯的元件
最後，從您的 IDE 執行您希望偵錯的元件，並確保它沒有任何錯誤。

## 重要事項
當單獨執行 Argo CD 元件時，請確保元件之間不會產生衝突——每個元件都必須且只能啟動一次，無論是在本地工具鏈中執行還是在您的 IDE 中執行。否則，您可能會收到有關連接埠不可用或甚至正在偵錯不包含您程式碼變更的程序的錯誤。
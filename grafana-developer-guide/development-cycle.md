# 開發週期

## 先決條件
1. [開發環境](development-environment.md)
2. [工具鏈指南](toolchain-guide.md)

## 前言
當您開發完並可能手動測試了您想要貢獻的程式碼後，您應該確保一切都能正確建置。在本機提交您的變更，並執行以下步驟，每一步都列出了本地和虛擬化工具鏈的命令。

### 虛擬化工具鏈使用者的 Docker 權限
[這些說明](toolchain-guide.md#docker-privileges)與以下大部分步驟相關。

### 虛擬化工具鏈使用者使用 Podman
[這些說明](toolchain-guide.md#using-podman)與以下大部分步驟相關。

## 開發週期步驟
### 將 kubectl 上下文設定為 argocd 命名空間

要讓這些步驟成功，需要將 kubectl 設定為 argocd 命名空間。
本指南中所有後續命令都假設命名空間已經設定。

```shell
kubectl config set-context --current --namespace=argocd
```

### 拉取所有建置依賴項

隨著建置依賴項隨時間變更，您必須將您的開發環境與目前的規格同步。為了拉取所有必要的依賴項，請執行：

*   `make dep-ui` 或 `make dep-ui-local`

### 產生 API 黏合程式碼和其他資產

Argo CD 依賴 Google 的 [Protocol Buffers](https://developers.google.com/protocol-buffers) 作為其 API，這大量使用了自動產生的黏合程式碼和存根。每當您接觸到 API 程式碼的某些部分時，都必須重新產生自動產生的程式碼。

*   執行 `make codegen` 或 `make codegen-local`，這可能需要一些時間
*   透過執行 `git status` 或 `git diff` 來檢查是否有任何變更
*   將任何可能的變更提交到您的本地 Git 分支，一個適當的提交訊息可以是 `Changes from codegen`，例如。

> [!NOTE]
> 有一些不明顯的資產是自動產生的。您不應該變更自動產生的資產，因為它們會在後續的 `make codegen` 執行中被覆寫。相反，您應該變更它們的來源檔案。不明顯的自動產生程式碼的顯著例子是 `swagger.json` 或安裝清單 YAML。

### 建置您的程式碼並執行單元測試

在程式碼黏合產生後，您的程式碼應該能夠建置，並且單元測試應該能夠在沒有任何錯誤的情況下執行。執行以下語句：

*   `make build` 或 `make build-local`
*   `make test` 或 `make test-local`

這些步驟是非修改性的，因此之後無需檢查變更。

### 對您的程式碼庫進行 Lint

為了在我們的原始碼樹中保持一致的程式碼風格，您的程式碼必須符合一些被廣泛接受的規則，這些規則由 Linter 應用。

Linter 可能會對您的程式碼進行一些自動變更，例如縮排修正。Linter 回報的其他一些錯誤必須手動修復。

*   執行 `make lint` 或 `make lint-local` 並觀察 Linter 回報的任何錯誤
*   修復回報的任何錯誤並提交到您的本地分支
*   最後，在 Linter 沒有回報任何錯誤後，執行 `git status` 或 `git diff` 來檢查 Lint 自動進行的任何變更
*   如果有自動變更，請將它們提交到您的本地分支

如果您接觸了 UI 程式碼，您也應該在其上執行 Yarn linter：

*   執行 `make lint-ui` 或 `make lint-ui-local`
*   修復它回報的任何錯誤

### 執行端對端測試

最後一步是執行端對端測試套件，以確保您的 Kubernetes 依賴項正常運作。這將涉及在您的電腦上啟動所有 Argo CD 元件。端對端測試由兩部分組成：一個伺服器元件和一個客戶端元件。

*   首先，啟動端對端伺服器：`make start-e2e` 或 `make start-e2e-local`。這將在您的系統上產生許多程序和服務。
*   當所有元件都啟動後，執行 `make test-e2e` 或 `make test-e2e-local` 來對您的本地服務執行端對端測試。

要使用本地工具鏈執行單個測試，您可以使用 `TEST_FLAGS="-run TestName" make test-e2e-local`。

有關端對端測試的更多資訊，請參閱[端對端測試文件](test-e2e.md)。

## 常用的 Make 目標

以下是一些常用的 make 目標（所有都將在您的機器上執行）：

### 本地工具鏈 Make 目標

*   `make install-tools-local` - 為本地工具鏈安裝測試和建置工具
*   `make build-local` - 建置 Argo CD 二進位檔
*   `make test-local` - 執行單元測試
*   `make codegen-local` - 重新產生自動產生的 Swagger 和 Protobuf（在變更 API 程式碼後）
*   `make lint-local` - 執行 linting
*   `make pre-commit-local` - 執行 pre-commit 檢查
*   `make start-e2e-local` - 啟動端對端測試伺服器
*   `make test-e2e-local` - 執行端對端測試
*   `make serve-docs-local` - 提供文件服務
*   `make start-local` - 啟動 Argo CD
*   `make cli-local` - 建置 Argo CD CLI 二進位檔

### 虛擬化工具鏈 Make 目標

*   `make verify-kube-connect` - 測試虛擬化工具鏈是否可以存取您的 K8s 叢集
*   `make test-tools-image` - 準備虛擬化鏈的環境
*   `make build` - 建置 Argo CD 二進位檔
*   `make test` - 執行單元測試
*   `make codegen` - 重新產生自動產生的 Swagger 和 Protobuf（在變更 API 程式碼後）
*   `make lint` - 執行 linting
*   `make pre-commit` - 執行 pre-commit 檢查
*   `make start-e2e` - 啟動端對端測試伺服器
*   `make test-e2e` - 執行端對端測試
*   `make serve-docs` - 提供文件服務
*   `make start` - 啟動 Argo CD

---
恭喜您完成本手冊！🚀

若要了解更多關於 Argo CD 的資訊，請在 Slack 中找到我們 - <https://slack.cncf.io/> [#argo-contributors](https://cloud-native.slack.com/archives/C020XM04CUW)
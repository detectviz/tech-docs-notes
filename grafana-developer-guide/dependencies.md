# 管理依賴項

## 通知引擎 (`github.com/argoproj/notifications-engine`)

### 儲存庫

[notifications-engine](https://github.com/argoproj/notifications-engine)

### 從 `notifications-engine` 拉取變更

在您的 Notifications Engine PR 合併後，需要更新 ArgoCD 以拉取包含您變更的通知引擎版本。以下是步驟：

-   取得您提交的 SHA 雜湊值。您將在下一步中使用它。
-   在 `argo-cd` 資料夾中，執行以下命令

    `go get github.com/argoproj/notifications-engine@<git-commit-sha>`

    如果您收到錯誤訊息 `invalid version: unknown revision`，那麼您的 SHA 雜湊值是錯誤的。

-   執行：

    `go mod tidy`

-   以下檔案將會被變更：

    -   `go.mod`
    -   `go.sum`

-   如果您的通知引擎 PR 包含文件變更，請執行 `make codegen` 或 `make codegen-local`。

-   為上述檔案變更建立一個 ArgoCD PR，並在其標題中使用 `refactor:` 類型。

## Argo UI 元件 (`github.com/argoproj/argo-ui`)
### 為 Argo CD UI 做出貢獻

Argo CD 與 Argo Workflows 一起使用來自 [Argo UI](https://github.com/argoproj/argo-ui) 的共享 React 元件。其中一些元件的範例包括按鈕、容器、表單控制項等。雖然您可以對這些檔案進行變更並在本機執行它們，但為了將這些變更新增到 Argo CD repo，您需要遵循以下步驟。

1.  Fork 並複製 [Argo UI 儲存庫](https://github.com/argoproj/argo-ui)。

2.  `cd` 進入您的 `argo-ui` 目錄，然後執行 `yarn install`。

3.  進行您的檔案變更。

4.  執行 `yarn start` 以啟動一個 [storybook](https://storybook.js.org/) 開發伺服器，並在您的瀏覽器中檢視元件。確保您的所有變更都如預期般運作。

5.  使用 [yarn link](https://classic.yarnpkg.com/en/docs/cli/link/) 將 Argo UI 套件連結到您的 Argo CD 儲存庫。（以下命令假設 `argo-ui` 和 `argo-cd` 都位於同一個父資料夾中）

    *   `cd argo-ui`
    *   `yarn link`
    *   `cd ../argo-cd/ui`
    *   `yarn link argo-ui`

    一旦 `argo-ui` 套件成功連結，請在您的本地開發環境中測試變更。

6.  提交變更並向 [Argo UI](https://github.com/argoproj/argo-ui) 開啟一個 PR。

7.  一旦您的 PR 在 Argo UI 中合併，請 `cd` 進入您的 `argo-cd/ui` 資料夾並執行 `yarn add git+https://github.com/argoproj/argo-ui.git`。這將更新 `ui/yarn.lock` 檔案中的提交 SHA，以使用 argo-ui 的最新 master 提交。

8.  在向 Argo CD 的 PR 中提交對 `ui/yarn.lock` 的變更。
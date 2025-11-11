# 提交 PR

## 先決條件
1. [開發環境](development-environment.md)
2. [工具鏈指南](toolchain-guide.md)
3. [開發週期](development-cycle.md)

## 前言

> [!NOTE]
> **在您開始之前**
>
> Argo CD 專案在功能和社群規模方面都在不斷增長。越來越多的組織採用它，並信任 Argo CD 來處理其關鍵的生產工作負載。因此，我們需要非常謹慎地對待任何影響 Argo CD 相容性、效能、可擴展性、穩定性和安全性的變更。因此，每一項新功能或較大的增強功能都必須在被接受到程式碼庫之前經過適當的設計和討論。
>
> 我們歡迎並鼓勵每個人參與 Argo CD 專案，但請理解，由於各種原因，我們無法接受社群的每一項貢獻。如果您想為一個很棒的新功能或增強功能提交程式碼，我們懇請您在開始編寫程式碼或提交 PR 之前，先看一下
> [程式碼貢獻指南](code-contributions.md#)。

如果您想提交 PR，請仔細閱讀本文件，因為它包含了指導您通過我們 PR 品質關卡的重要資訊。

如果您在提交 PR 方面需要指導，或有任何其他關於 Argo CD 開發的問題，請隨時[加入我們的 Slack](https://argoproj.github.io/community/join-slack) 並在 `#argo-cd-contributors` 頻道與我們聯繫！

## 提交 PR 之前

1.  將您的分支 rebase 到上游 main：
```shell
git fetch upstream
git rebase upstream/main
```

2.  執行 pre-commit 檢查：
```shell
make pre-commit-local
```

## 持續整合流程

當您向 Argo CD 的 GitHub 儲存庫提交 PR 時，會自動執行一些 CI 檢查，以確保您的變更可以順利建置並符合某些品質標準。您的貢獻需要通過這些檢查才能被合併到儲存庫中。

> [!NOTE]
> 請確保您始終從與 Argo CD 的 master 分支最新變更保持同步的分支建立 PR。根據維護者審查和合併您的 PR 所需的時間，可能需要再次將最新的變更拉取到您的分支中。

請理解，作為一個開源專案，我們審查和合併 Argo CD PR 的能力有限。我們會盡力審查您的 PR 並盡快給您回饋，但如果花費的時間比預期長，請您耐心等待。

以下閱讀內容將幫助您提交符合我們 CI 測試標準的 PR：

## PR 的標題

請為您的 PR 使用一個有意義且簡潔的標題。這將有助於我們快速挑選 PR 進行審查，並且 PR 標題最終也會出現在變更日誌中。

我們使用 [PR 標題檢查器](https://github.com/marketplace/actions/pr-title-checker) 將您的 PR 分類為以下類別之一：

*   `fix` - 您的 PR 包含一個或多個程式碼錯誤修復
*   `feat` - 您的 PR 包含一個新功能
*   `docs` - 您的 PR 改善了文件
*   `chore` - 您的 PR 改善了 Argo CD 的任何內部部分，例如建置過程、單元測試等

請在您的 PR 標題前加上一個有效的類別。例如，如果您選擇的 PR 標題是 `Add documentation for GitHub SSO integration`，請改用 `docs: Add documentation for GitHub SSO integration`。

## PR 範本檢查清單

開啟 PR 時，詳細資訊將包含一個來自範本的檢查清單。請閱讀檢查清單，並勾選適用於您的項目。

## 自動化建置與測試

在您提交 PR 後，以及每當您向該分支推送新的提交時，GitHub 都會對您的程式碼執行一些持續整合檢查。它將執行以下操作，並且每一項都必須通過：

*   建置 Go 程式碼 (`make build`)
*   產生 API 黏合程式碼和清單 (`make codegen`)
*   對程式碼執行 Go linter (`make lint`)
*   執行單元測試 (`make test`)
*   執行端對端測試 (`make test-e2e`)
*   建置並對 UI 程式碼進行 lint 檢查 (`make lint-ui`)
*   建置 `argocd` CLI (`make cli`)

如果 CI 管線中的任何這些測試失敗，這意味著您的某些貢獻被認為是有問題的（或者某個測試可能不穩定，見下文）。

## 程式碼測試覆蓋率

我們在 CI 管線中使用 [CodeCov](https://codecov.io) 來檢查測試覆蓋率，一旦您提交 PR，它將會執行並在您的 PR 中以評論的形式報告覆蓋率差異。如果差異為負值過大，即您的提交導致程式碼覆蓋率顯著下降，CI 檢查將會失敗。

每當您開發新功能或提交錯誤修復時，請也為其編寫適當的單元測試。如果您編寫一個全新的模組，請以至少 80% 的覆蓋率為目標。
如果您想查看某個特定模組（即您的新模組）的覆蓋率，您可以將 `TEST_MODULE` 設定為該模組的（完全合格）名稱，並使用 `make test`，即：

```bash
 make test TEST_MODULE=github.com/argoproj/argo-cd/server/cache
...
ok      github.com/argoproj/argo-cd/server/cache        0.029s  coverage: 89.3% of statements
```

## Cherry-picking 修復

如果您的 PR 包含一個錯誤修復，並且您希望將該修復反向移植到先前的發行分支，請為您的
PR 加上 `cherry-pick/x.y` 標籤（例如：`cherry-pick/3.1`）。如果您沒有權限新增標籤，請要求維護者為您
新增。

如果您在 PR 合併之前新增標籤，cherry-pick 機器人將在您的 PR 合併時開啟反向移植的 PR。

在 PR 合併後新增標籤也會導致機器人開啟反向移植的 PR。
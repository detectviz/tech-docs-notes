# 概覽

> [!WARNING]
> **作為 Argo CD 的使用者，您可能不想閱讀本節文件。**
>
> 本手冊的這一部分旨在幫助人們為 Argo CD、文件做出貢獻，或開發與 Argo CD 互動的第三方應用程式，例如：
>
> *   聊天機器人
> *   Slack 整合

## 前言
#### 了解[程式碼貢獻指南](code-contributions.md)
#### 了解[程式碼貢獻前言](submit-your-pr.md#preface)

## 為 Argo CD 文件做出貢獻

本指南將幫助您快速開始貢獻文件變更，執行您所需的最低限度設定。
對於需要完整建置-測試-在本機執行週期的後端和前端貢獻，請參閱[為 Argo CD 後端和前端做出貢獻](index.md#contributing-to-argo-cd-backend-and-frontend)

### Fork 並複製 Argo CD 儲存庫
- [Fork 並複製 Argo CD 儲存庫](development-environment.md#fork-and-clone-the-repository)

### 提交您的 PR
- [在提交 PR 之前](submit-your-pr.md#before-submitting-a-pr)
- [為您的 PR 選擇一個正確的標題](submit-your-pr.md#choose-a-correct-title-for-your-pr)
- [執行 PR 範本檢查清單](submit-your-pr.md#perform-the-PR-template-checklist)

## 為 Argo CD 通知文件做出貢獻

本指南將幫助您快速開始貢獻文件變更，執行您所需的最低限度設定。
通知文件位於 [notifications-engine](https://github.com/argoproj/notifications-engine) Git 儲存庫中，需要 2 個 pull request：一個用於 `notifications-engine` repo，另一個用於 `argo-cd` repo。
對於需要完整建置-測試-在本機執行週期的後端和前端貢獻，請參閱[為 Argo CD 後端和前端做出貢獻](index.md#contributing-to-argo-cd-backend-and-frontend)

### Fork 並複製 Argo CD 儲存庫
- [Fork 並複製 Argo CD 儲存庫](development-environment.md#fork-and-clone-the-repository)

### 將您的 PR 提交到 notifications-engine
- [在提交 PR 之前](submit-your-pr.md#before-submitting-a-pr)
- [為您的 PR 選擇一個正確的標題](submit-your-pr.md#choose-a-correct-title-for-your-pr)
- [執行 PR 範本檢查清單](submit-your-pr.md#perform-the-PR-template-checklist)

### 在您的機器上安裝 Go
- [安裝 Go](development-environment.md#install-go)

### 將您的 PR 提交到 argo-cd
- [為 notifications-engine 做出貢獻](dependencies.md#notifications-engine-githubcomargoprojnotifications-engine)
- [在提交 PR 之前](submit-your-pr.md#before-submitting-a-pr)
- [為您的 PR 選擇一個正確的標題](submit-your-pr.md#choose-a-correct-title-for-your-pr)
- [執行 PR 範本檢查清單](submit-your-pr.md#perform-the-PR-template-checklist)

## 為 Argo CD 後端和前端做出貢獻

本指南將幫助您設定您的建置和測試環境，以便您可以開始開發和測試錯誤修復和功能增強，而無需花費太多精力來設定本地工具鏈。

與開發過程一樣，本文件也在不斷變更。如果您發現任何錯誤，或者您認為本文件已過時，或者您認為它缺少某些內容：歡迎隨時向我們的 GitHub 議題追蹤器提交 PR 或提交錯誤。

### 設定您的開發環境
- [安裝必要的工具（Git、Go、Docker 等）](development-environment.md#install-required-tools)
- [安裝並啟動一個本地 K8s 叢集（Kind、Minikube 或 K3d）](development-environment.md#install-a-local-k8s-cluster)
- [Fork 並複製 Argo CD 儲存庫](development-environment.md#fork-and-clone-the-repository)
- [安裝額外的必要開發工具](development-environment.md#install-additional-required-development-tools)
- [在您的本地叢集上安裝最新的 Argo CD](development-environment.md#install-latest-argo-cd-on-your-local-cluster)

### 設定開發工具鏈（本地或虛擬化）
- [了解工具鏈之間的差異](toolchain-guide.md#local-vs-virtualized-toolchain)
- 選擇一個開發工具鏈

    -   [設定本地工具鏈](toolchain-guide.md#setting-up-a-local-toolchain)
    -   或者 [設定虛擬化工具鏈](toolchain-guide.md#setting-up-a-virtualized-toolchain)

### 執行開發週期
- [將 kubectl 上下文設定為 argocd 命名空間](development-cycle.md#set-kubectl-context-to-argocd-namespace)
- [拉取所有建置依賴項](development-cycle.md#pull-in-all-build-dependencies)
- [產生 API 黏合程式碼和其他資產](development-cycle.md#generate-API-glue-code-and-other-assets)
- [建置您的程式碼並執行單元測試](development-cycle.md#build-your-code-and-run-unit-tests)
- [對您的程式碼庫進行 Lint](development-cycle.md#lint-your-code-base)
- [執行 e2e 測試](development-cycle.md#run-end-to-end-tests)
- 如何為文件做出貢獻：在您的機器上[建置並執行文件網站](docs-site/)以進行手動測試

### 在本機執行和偵錯 Argo CD
- [在您的機器上執行 Argo CD 以進行手動測試](running-locally.md)
- [在您機器的 IDE 中偵錯 Argo CD](debugging-locally.md)

### 提交您的 PR
- [在提交 PR 之前](submit-your-pr.md#before-submitting-a-pr)
- [了解持續整合流程](submit-your-pr.md#understand-the-continuous-integration-process)
- [為您的 PR 選擇一個正確的標題](submit-your-pr.md#choose-a-correct-title-for-your-pr)
- [執行 PR 範本檢查清單](submit-your-pr.md#perform-the-PR-template-checklist)
- [了解 CI 自動化建置和測試](submit-your-pr.md#automated-builds-&-tests)
- [了解並確保您的 PR 符合 CI 程式碼測試覆蓋率要求](submit-your-pr.md#code-test-coverage)

需要幫助嗎？從[貢獻者常見問題](faq/)開始

## 為 Argo CD 依賴項做出貢獻
- [為 argo-ui 做出貢獻](dependencies.md#argo-ui-components-githubcomargoprojargo-ui)
- [為 gitops-engine 做出貢獻](dependencies.md#gitops-engine-githubcomargoprojgitops-engine)
- [為 notifications-engine 做出貢獻](dependencies.md#notifications-engine-githubcomargoprojnotifications-engine)

## 擴充功能和第三方應用程式
* [UI 擴充功能](extensions/ui-extensions.md)
* [代理擴充功能](extensions/proxy-extensions.md)
* [設定管理外掛程式](../operator-manual/config-management-plugins/)

## 為 Argo 網站做出貢獻
Argo 網站維護在 [argo-site](https://github.com/argoproj/argo-site) 儲存庫中。
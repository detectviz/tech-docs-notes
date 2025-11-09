---
id: set-up-github
title: 為您的開發環境設定 GitHub 工作流程
sidebar_label: 設定您的 GitHub 工作流程
description: 為 Grafana 插件開發設定 GitHub。
keywords:
  - grafana
  - plugins
  - plugin
  - create-plugin
  - GitHub
  - setup
  - CI
  - continuous integration
  - automation
  - configuration
sidebar_position: 20
---

自動化您的開發流程以最小化錯誤並使其更快、更具成本效益。`create-plugin` 工具可協助您設定 GitHub actions 工作流程以協助自動化您的開發流程。

## CI 工作流程

CI (`ci.yml`) 工作流程旨在對前端和後端進行程式碼檢查、類型檢查和建置。它也用於在您每次將變更推送到您的儲存庫時對您的插件執行測試。`create-plugin` 工具有助於在開發過程的早期發現任何問題，以免它們變成更大的問題。有關作為 CI 工作流程一部分的端對端測試的更多資訊，請參閱我們的[文件](/e2e-test-a-plugin/ci.md)。

## 發布工作流程

若要了解如何自動化發布流程並設定發布工作流程，請參閱我們的[使用 GitHub CI 自動化打包和簽署](/publish-a-plugin/build-automation)文件。

:::warning

此工作流程需要一個 Grafana Cloud API 金鑰。在開始之前，請遵循[產生存取原則權杖](/publish-a-plugin/sign-a-plugin#generate-an-access-policy-token)的說明。

:::

### 將您的存取原則權杖儲存為 GitHub 中的儲存庫密碼

1. 存取儲存庫設定：

- 前往您的 GitHub 儲存庫。
- 導覽至「設定」標籤。

2. 在左側側邊欄中，按一下「密碼和變數」->「動作」
3. 按一下「新增儲存庫密碼」按鈕。
4. 新增密碼資訊：

- 為您的密碼輸入名稱 - GRAFANA_ACCESS_POLICY_TOKEN。
- 將存取原則權杖值貼到「密碼」欄位中。

5. 按一下「新增密碼」按鈕以儲存密碼。

儲存密碼後，您可以在您的 GitHub Actions 工作流程中存取它：

```yaml title="release.yml"
name: Release

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: grafana/plugin-actions/build-plugin@main
        with:
          grafana_token: ${{ secrets.GRAFANA_ACCESS_POLICY_TOKEN }}
```

在此範例中，`secrets.GRAFANA_ACCESS_POLICY_TOKEN` 變數用於在您的 GitHub Actions 工作流程中安全地存取儲存的權杖。請務必根據您的特定需求以及您正在使用的語言/環境來調整工作流程。

### 觸發工作流程

若要觸發發布工作流程，請推送您要發布的插件版本的 Git 標籤：

```bash
git tag v1.0.0
git push origin v1.0.0
```

## 相容性檢查工作流程

相容性檢查 (`is-compatible.yml`) 工作流程旨在在您每次將變更推送到您的儲存庫時檢查您插件的 Grafana API 相容性。這有助於在潛在的前端執行階段問題發生之前發現它們。

工作流程包含以下步驟：

1. 在您的插件中尋找 `@grafana` npm 套件。
2. 擷取指定版本的匯出類型。
3. 比較該版本與最新版本之間的差異。
4. 在您的插件中尋找那些已變更 API 的用法。
5. 報告任何潛在的不相容性。

## create plugin update 工作流程

create plugin update (`cp-update.yml`) 工作流程旨在自動化讓您的插件開發環境和相依性保持最新的過程。它會定期檢查 npm 註冊表上列出的最新版本的 create-plugin，並將其與您的插件使用的版本進行比較。如果有較新的版本可用，工作流程將會執行 `create-plugin update` 指令，更新前端相依性鎖定檔案，然後建立一個包含變更的 PR 以供審核。

此工作流程需要對您的插件儲存庫具有內容和拉取請求寫入權限，才能推送變更和開啟 PR。請從以下兩個選項中擇一：

### 使用預設存取權杖

若要使用此選項，您必須在您的儲存庫設定中允許 [github actions 建立和核准拉取請求](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/managing-github-actions-settings-for-a-repository#preventing-github-actions-from-creating-or-approving-pull-requests)，並在工作流程中使用 `permissions` 屬性來提升預設存取權杖的權限，如下所示：

```yaml
name: Create Plugin Update

on:
  workflow_dispatch:
  schedule:
    - cron: '0 0 1 * *' # 每月第一天執行一次

permissions:
  contents: write
  pull-requests: write

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: grafana/plugin-actions/create-plugin-update@main
```

### 使用個人存取權杖

若要使用此選項，您必須建立一個具有存取插件儲存庫權限的 Github [細粒度個人存取權杖](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)，並具有讀取和寫入 `contents` 和 `pull requests` 的權限。建立後，將權杖新增至插件儲存庫動作密碼，然後將其傳遞給動作，如下所示：

```yaml
name: Create Plugin Update

on:
  workflow_dispatch:
  schedule:
    - cron: '0 0 1 * *' # 每月第一天執行一次

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: grafana/plugin-actions/create-plugin-update@main
        with:
          token: ${{ secrets.GH_PAT_TOKEN }}
```

## bundle stats 工作流程

bundle stats (`bundle-stats.yml`) 工作流程旨在幫助開發人員關注其插件前端資產的大小。PR 中的變更會觸發此工作流程，該工作流程將比較兩個 webpack 統計檔案；一個來自預設分支，另一個來自 PR。然後它會計算這些資產大小之間的差異，並將格式化的評論張貼到 PR 中，以提供任何大小差異的概觀。

```yaml title="bundle-stats.yml"
name: Bundle Stats

on:
  workflow_dispatch:
  pull_request:
    branches:
      - main
  push:
    branches:
      - main

permissions:
  contents: write
  pull-requests: write
  actions: read

jobs:
  compare:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - uses: grafana/plugin-actions/bundle-size@main
```

### 疑難排解

#### 找不到主要統計成品

如果您在 bundle size 工作流程執行期間看到此警告，表示工作流程無法找到包含主要分支統計檔案的 github 成品。可以透過將 PR 合併到 main、將提交推送到 main 或使用 workflow_dispatch 手動執行工作流程來產生該檔案。
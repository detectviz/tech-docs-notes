---
id: build-automation
title: 自動化插件建置
sidebar_position: 1
description: 自動化 Grafana 插件建置和發布
keywords:
  - grafana
  - plugins
  - plugin
  - automation
  - build
  - automate
  - builds
---

# 使用 GitHub CI 自動化打包和簽署

我們建議您將插件設定為使用 [create-plugin](../get-started.md) 提供的[GitHub 工作流程](../set-up/set-up-github)，以確保您的插件將以正確的格式進行建置和打包。

此外，我們建議使用此工作流程產生的 zip 檔案來測試插件。

如果您在 [Github 儲存庫機密](https://docs.github.com/en/codespaces/managing-codespaces-for-your-organization/managing-development-environment-secrets-for-your-repository-or-organization)中包含 Grafana 存取原則權杖，則會自動建立已簽署的建置，您可以使用它在提交前於本地測試插件。[簽署插件](./sign-a-plugin.md#generate-an-access-policy-token)文件包含有關如何建立此權杖的指南。

透過建立發布標籤，整個過程將會自動化，最終產生一個 zip 檔案，您可以將其提交至 [Grafana 插件目錄](https://grafana.com/plugins)進行發布。

您可以使用發布頁面中的封存和 zip 檔案連結來提交您的插件。

## 設定發布工作流程

請確保您的儲存庫包含一個 `.github/workflows/release.yml` 檔案，其內容如下：

```yaml title=".github/workflows/release.yml"
name: Release

on:
  push:
    tags:
      - 'v*' # 在版本標籤上執行工作流程，例如 v1.0.0。

jobs:
  release:
    permissions:
      id-token: write
      contents: write
      attestations: write
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: grafana/plugin-actions/build-plugin@main
        with:
          # 請參閱 https://grafana.com/developers/plugin-tools/publish-a-plugin/sign-a-plugin#generate-an-access-policy-token 以產生它
          # 將值儲存在您的儲存庫機密中
          policy_token: ${{ secrets.GRAFANA_ACCESS_POLICY_TOKEN }}
          # 建立一個已簽署的建置來源證明以驗證插件建置的真實性
          attestation: true
```

## 如何觸發發布工作流程

若要觸發發布工作流程，您需要將格式為 `vX.X.X` 的標籤推送到儲存庫。通常，您所有的變更都會合併到 `main`，並且標籤會套用至 `main`。

### 使用您的套件管理器建立 `vX.X.X` 標籤（建議）

建立版本標籤最簡單的方法是使用您的套件管理器。

在以下範例中，會建立一個修補程式版本（遵循[語意版本控制](https://semver.org/)）：

使用 [npm](https://docs.npmjs.com/cli/v7/commands/npm-init)：

```
npm version patch
```

使用 [yarn](https://yarnpkg.com/lang/en/docs/cli/version/)：

```
yarn version patch
```

使用 [pnpm](https://pnpm.io/)：

```
pnpm version patch
```

這會更新您 `package.json` 檔案中的版本，並建立一個格式為 `vX.X.X` 的新 Git 標籤。如果您想建立一個新的次要或主要版本，可以將 `patch` 變更為 `minor` 或 `major`。

建立標籤後，將其推送到儲存庫：

```bash
git push origin main --follow-tags
```

## 在 GitHub 上發布您的版本

在您[建立並推送標籤](#how-to-trigger-the-release-workflow)後，發布工作流程將會執行，產生一個包含提交您的插件至 [Grafana 插件目錄](https://grafana.com/plugins)所需的所有成品的版本。

工作流程會建立一個**草稿版本**。您可以在 GitHub 中編輯該版本，視需要更新描述，然後發布它。有關管理儲存庫版本的更多詳細資訊，請參閱 [GitHub 文件](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository)。

## 使用您的版本資產進行插件提交

草稿版本發布後，您可以使用版本資產將您的插件提交至 [Grafana 插件目錄](https://grafana.com/plugins)。只需複製封存 (zip) 檔案和 sha1 總和的連結即可。在插件提交表單中使用這些連結。

## 下載版本 zip 檔案

直接從 GitHub 儲存庫發布路徑存取版本 zip 檔案（例如，`https://github.com/org/plugin-id/releases`）。

## 自動簽署您的插件

您可以使用 GitHub Action 自動簽署您的插件版本。

首先，[產生一個存取原則權杖](./sign-a-plugin.md#generate-an-access-policy-token)並將其[儲存在您的儲存庫機密](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions#creating-secrets-for-a-repository)中，名稱為 `GRAFANA_ACCESS_POLICY_TOKEN`。

預設情況下，create-plugin 會將以下 `release.yml` 新增至您建立的插件中，其內容如下。如果您的插件儲存庫中缺少此檔案，請複製以下內容以新增工作流程：

```yaml title=".github/workflows/release.yml"
name: Release

on:
  push:
    tags:
      - 'v*' # 在版本標籤上執行工作流程，例如 v1.0.0。

jobs:
  release:
    permissions:
      id-token: write
      contents: write
      attestations: write
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: grafana/plugin-actions/build-plugin@main
        with:
          # 請參閱 https://grafana.com/developers/plugin-tools/publish-a-plugin/sign-a-plugin#generate-an-access-policy-token 以產生它
          # 將值儲存在您的儲存庫機密中
          policy_token: ${{ secrets.GRAFANA_ACCESS_POLICY_TOKEN }}
          attestation: true
          use_changelog_generator: true # 啟用自動變更日誌產生
```

接下來，遵循常規流程以[觸發](#how-to-trigger-the-release-workflow)發布工作流程。您的插件將會被自動簽署，您可以使用版本資產進行插件提交。

## 證明插件建置的來源

來源證明是一項功能，可產生建置來源和過程的可驗證記錄，從而增強插件建置的安全性。透過此功能，使用者可以確認他們正在安裝的插件是透過您的官方建置管線建立的。

目前，此功能僅適用於公開儲存庫中的 GitHub Actions。雖然我們建議使用具有來源證明的 GitHub Actions 以提高安全性，但您仍然可以使用其他 CI/CD 平台或手動方法來建置和分發插件。

### 啟用來源證明

若要在您現有的 GitHub Actions 工作流程中啟用來源證明：

1. 為您的工作流程工作新增必要的權限：

```yaml
permissions:
  id-token: write
  contents: write
  attestations: write
```

2. 在 `build-plugin` 動作中啟用證明：

```yaml
- uses: grafana/plugin-actions/build-plugin@main
  with:
    policy_token: ${{ secrets.GRAFANA_ACCESS_POLICY_TOKEN }}
    attestation: true
```

工作流程會在建置您的插件 zip 檔案時自動產生證明。

### 來源證明疑難排解

如果您在插件驗證器或插件提交中遇到如下錯誤：

- 「沒有來源證明。此插件是在沒有建置驗證的情況下建置的。」
- 「無法驗證插件建置。」

請遵循上述步驟，在您的 GitHub Actions 工作流程中啟用來源證明。

## 自動產生變更日誌

維護詳細的變更日誌對於向使用者傳達更新至關重要，並且會在 Grafana 插件詳細資訊頁面中顯著顯示。為了簡化此過程，我們的插件建置工作流程支援自動產生變更日誌。

### 使用 GitHub Actions 工作流程產生變更日誌

build-plugin GitHub Action 可以使用 [github-changelog-generator](https://github.com/github-changelog-generator/github-changelog-generator) 工具自動產生和維護您插件的變更日誌。此功能：

1. 建立一個按版本組織的綜合性 CHANGELOG.md 檔案
2. 按類型（功能、錯誤修正等）對變更進行分組
3. 包含指向拉取請求和問題的連結
4. 致謝貢獻者
5. 將更新後的變更日誌提交至您的儲存庫

若要在您的工作流程中啟用自動變更日誌產生，請將 `use_changelog_generator: true` 參數新增至您的 build-plugin 動作：

```yaml
- uses: grafana/plugin-actions/build-plugin@main
  with:
    policy_token: ${{ secrets.GRAFANA_ACCESS_POLICY_TOKEN }}
    attestation: true
    use_changelog_generator: true # 啟用自動變更日誌產生
```

### 要求

若要使用此功能，請確保您的工作流程具有必要的權限：

```yaml
permissions:
  contents: write
```

變更日誌產生器需要寫入權限才能將更新後的 `CHANGELOG.md` 檔案提交至您的儲存庫。

如果您的目標分支受到保護，則預設的 github.token 無法直接推送變更，即使具有寫入權限也是如此。在這種情況下，您需要：

1. 建立具有適當權限的個人存取權杖 (PAT)
2. 將其儲存為儲存庫機密（例如，CHANGELOG_PAT）
3. 設定動作以使用此權杖：

```yaml
- name: Build plugin
  uses: grafana/plugin-actions/build-plugin@main
  with:
    use_changelog_generator: true
    token: ${{ secrets.CHANGELOG_PAT }} # 取代預設的 github.token
```

### 產生的變更日誌格式

產生的變更日誌遵循標準化格式，可清楚地對變更進行分類：

```markdown
## [1.2.0](https://github.com/user/plugin-name/tree/1.2.0) (2025-03-15)

**已實作的增強功能：**

- 新增深色主題支援 [\#138](https://github.com/user/plugin-name/pull/138) ([username](https://github.com/username))
- 新增工具提示自訂選項 [\#135](https://github.com/user/plugin-name/pull/135) ([username](https://github.com/username))

**已修正的錯誤：**

- 修正在切換儀表板時面板崩潰的問題 [\#139](https://github.com/user/plugin-name/pull/139) ([username](https://github.com/username))
- 修正時區處理不一致的問題 [\#134](https://github.com/user/plugin-name/pull/134) ([username](https://github.com/username))

**已關閉的問題：**

- 文件需要更多範例 [\#130](https://github.com/user/plugin-name/issues/130)

**已合併的拉取請求：**

- 更新相依性以提高安全性 [\#140](https://github.com/user/plugin-name/pull/140) ([username](https://github.com/username))
```

## 後續步驟

打包好您的插件後，請繼續[發布插件](./publish-or-update-a-plugin.md)或[安裝已打包的插件](https://grafana.com/docs/grafana/latest/administration/plugin-management/#install-a-packaged-plugin)。
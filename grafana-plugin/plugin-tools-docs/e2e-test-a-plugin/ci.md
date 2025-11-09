---
id: ci
title: CI 工作流程
description: 如何在 CI 中執行端對端測試。
keywords:
  - grafana
  - plugins
  - plugin
  - e2e
  - end-to-end
  - ci
sidebar_position: 30
---

import BEPluginWorkflowNPM from '@snippets/plugin-e2e-ds-workflow.npm.md';
import BEPluginWorkflowYarn from '@snippets/plugin-e2e-ds-workflow.yarn.md';
import BEPluginWorkflowPNPM from '@snippets/plugin-e2e-ds-workflow.pnpm.md';
import FEPluginNPM from '@snippets/plugin-e2e-fe-plugin-workflow.npm.md';
import FEPluginYarn from '@snippets/plugin-e2e-fe-plugin-workflow.yarn.md';
import FEPluginPNPM from '@snippets/plugin-e2e-fe-plugin-workflow.pnpm.md';

本文將逐步說明如何針對 Grafana 版本矩陣執行端對端測試。

## 為何要針對 Grafana 版本矩陣執行端對端測試

由於 Grafana 的[相依性共用機制](../key-concepts/manage-npm-dependencies.md)，許多與外掛程式相關的問題只有在執行階段才會出現。例如，如果外掛程式呼叫了 Grafana 執行階段環境中不可用的函式、元件或類別，任何載入該部分外掛程式的頁面都會崩潰。這些執行階段特有的問題超出了單元測試的範圍，但可以透過端對端測試有效地識別出來。

為了維持可靠性和相容性，外掛程式開發人員必須定期在所有支援的 Grafana 版本上執行端對端測試。`e2e-versions` GitHub Action 透過根據您外掛程式的 `grafanaDependency` 自動解析支援的 Grafana 版本，同時也包含 Grafana 的主要開發分支，簡化了這個過程。將此 Action 整合到您的 CI 工作流程中，可確保您的外掛程式在舊版和新版的 Grafana 中都能保持穩定和相容，讓您對其跨版本的功能充滿信心。

## e2e-versions Action

`e2e-versions` GitHub Action 會產生一個 Grafana 映像檔名稱和版本的矩陣，用於在 GitHub 工作流程中對 Grafana 外掛程式進行端對端測試。此 Action 支援兩種模式：

- **`plugin-grafana-dependency:`** 此模式會解析最新的 `grafana-dev` 映像檔，並傳回自 `plugin.json` 檔案中指定為 `grafanaDependency` 的版本以來的所有最新修補程式版本的 Grafana Enterprise。為防止啟動過多的工作，輸出上限為 6 個版本。這是預設模式。
- **`version-support-policy:`** 在此模式下，action 會根據 Grafana 的外掛程式相容性支援政策來解析版本。它會擷取目前主要 Grafana 版本中每個次要版本的最新修補程式版本。此外，它還會包含前一個主要 Grafana 版本最新次要版本的最新版本。

有關設定 `inputs` 的詳細資訊，請造訪 `e2e-versions` [GitHub 頁面](https://github.com/grafana/plugin-actions/tree/main/e2e-version)。

## 工作流程範例

所有使用 `grafana/create-plugin` 4.7.0 版或更新版本建立的 Grafana 外掛程式，都會自動在其 `ci.yml` 工作流程中包含針對 Grafana 版本矩陣的端對端測試。如果您的外掛程式是使用較早版本建立的，您可以使用下列工作流程範例，在一個獨立的 GitHub 工作流程中設定並執行具有 Grafana 版本矩陣的端對端測試：

:::note

下列範例為通用範例，適用於任何類型的外掛程式。根據您外掛程式的具體情況，您可能需要在將 `playwright-tests` 工作中的某些步驟整合到您外掛程式的工作流程之前，修改或移除這些步驟。

:::

<details>
  <summary> <h3>具有後端工作流程的外掛程式</h3> </summary>
  <CodeSnippets
snippets={[
{ component: BEPluginWorkflowNPM, label: 'npm' },
{ component: BEPluginWorkflowYarn, label: 'yarn' },
{ component: BEPluginWorkflowPNPM, label: 'pnpm' }
]}
groupId="package-manager"
queryString="current-package-manager"
/>
</details>

<details>
  <summary> <h3>僅限前端的外掛程式工作流程</h3> </summary>
  <CodeSnippets
snippets={[
{ component: FEPluginNPM, label: 'npm' },
{ component: FEPluginYarn, label: 'yarn' },
{ component: FEPluginPNPM, label: 'pnpm' }
]}
groupId="package-manager"
queryString="current-package-manager"
/>
</details>

## 將 Playwright 報告發佈至 GitHub Pages

Playwright [HTML 報告](https://playwright.dev/docs/test-reporters#html-reporter)以及 [Trace Viewer](https://playwright.dev/docs/trace-viewer)，為解決端對端測試執行期間發現的問題提供了強大的工具。本節說明如何將這些報告部署到 GitHub 的靜態網站託管服務 GitHub Pages，使其在測試完成後可立即存取以供檢閱。

本指南基於本文件前面提供的範例工作流程。

### 啟用報告發佈的步驟

1. 在執行測試的步驟之後，立即新增一個步驟，使用 `upload-report-artifacts` Action 將報告和測試摘要上傳為 GitHub artifacts。

```yml
- name: Run Playwright tests
  id: run-tests
  run: npx playwright test

- name: Upload e2e test summary
  uses: grafana/plugin-actions/playwright-gh-pages/upload-report-artifacts@main
  if: ${{ (always() && !cancelled()) }}
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
    test-outcome: ${{ steps.run-tests.outcome }}
```

2. 在 `playwright-tests` 工作之後，新增一個新工作來下載報告 artifacts、將其部署到 GitHub Pages，並發佈一個 PR 註解，總結測試結果，包括指向報告的連結。

```yml
publish-report:
  if: ${{ (always() && !cancelled()) }}
  needs: [playwright-tests]
  runs-on: ubuntu-latest
  steps:
    - uses: actions/checkout@v4
    - name: Publish report
      uses: grafana/plugin-actions/playwright-gh-pages/deploy-report-pages@main
      with:
        github-token: ${{ secrets.GITHUB_TOKEN }}
```

3. 修改工作流程權限，以允許其推送變更、查詢 GitHub API 和更新 PR 註解。

```yml
permissions:
  contents: write
  id-token: write
  pull-requests: write
```

4. 如果您的儲存庫尚未啟用 GitHub Pages，請設定一個用於部署的來源分支。請遵循[此處](https://github.com/grafana/plugin-actions/tree/main/playwright-gh-pages#github-pages-branch-configuration)的詳細說明進行設定。

有關其他設定選項和範例，請參閱 `playwright-gh-pages` [文件](https://github.com/grafana/plugin-actions/blob/main/playwright-gh-pages/README.md)。

### 重要考量

- **公開可見性**：預設情況下，GitHub Pages 網站可在網際網路上公開存取。如果您的端對端測試包含敏感資料或機密，請注意潛在的曝險風險。
- **企業存取控制**：如果您有 GitHub Enterprise 帳戶，您可以設定存取控制以限制可見性。有關詳細資訊，請參閱 [GitHub 文件](https://docs.github.com/en/enterprise-cloud@latest/pages/getting-started-with-github-pages/changing-the-visibility-of-your-github-pages-site)。

### 報告摘要

`publish-report` 工作會新增一個 PR 註解，總結作為矩陣一部分執行的所有測試。對於失敗的測試，註解會包含指向 GitHub Pages 網站的連結，您可以在其中瀏覽詳細報告。

![](/img/e2e-report-summary.png)

```

```
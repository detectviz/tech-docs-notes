---
id: migrate-from-grafana-e2e
title: 從 @grafana/e2e 遷移
description: 從 @grafana/e2e 遷移至 @grafana/plugin-e2e。
keywords:
  - grafana
  - plugins
  - plugin
  - testing
  - e2e
  - end-to-end
  - migrate
sidebar_position: 90
---

import ScaffoldPluginE2InstallNPM from '@shared/plugin-e2e-install.md';

:::danger
隨著 Grafana 11.0.0 的發布，[`@grafana/e2e`](https://www.npmjs.com/package/@grafana/e2e) 套件已被棄用且不再支援。我們建議所有插件作者將其端對端測試遷移至使用 Playwright 和 `@grafana/plugin-e2e`，而非 Cypress 和 `@grafana/e2e`。
:::

在本指南中，您將學習：

- 如何在您的插件中手動設定 `@grafana/plugin-e2e`
- 如何遷移測試
- 如何在 CI 中執行 Playwright 測試
- 如何解除安裝 Cypress 和 `@grafana/e2e`

## 手動安裝 @grafana/plugin-e2e

使用 create-plugin v4.6.0 及更新版本建立的插件會自動包含 `@grafana/plugin-e2e` 和 `@playwright/test` 的設定。若要手動新增此設定，請遵循以下步驟：

### 步驟 1：安裝 Playwright

`@grafana/plugin-e2e` 工具擴充了 Playwright API，因此您需要在插件的 `package.json` 檔案中將最低版本為 1.41.2 的 `@playwright/test` 安裝為開發依賴項。有關如何安裝的說明，請參閱 [Playwright 文件](https://playwright.dev/docs/intro#installing-playwright)。請確保您可以執行安裝期間產生的範例測試。如果範例測試成功執行，您可以繼續並刪除它們，因為它們不再需要了。

### 步驟 2：安裝 `@grafana/plugin-e2e`

開啟終端機並在您的插件專案目錄中執行以下指令：

<ScaffoldPluginE2InstallNPM />

### 步驟 3：設定 Playwright

開啟安裝 Playwright 時產生的 Playwright 設定檔。

1. 取消註解 `baseUrl` 並將其變更為 `'http://localhost:3000'`。

```ts title="playwright.config.ts"
  baseURL: 'http://localhost:3000',
```

2. Playwright 使用[專案](https://playwright.dev/docs/test-projects)來邏輯地分組具有相同設定的測試。我們將新增兩個專案：

   1. `auth` 是一個設定專案，它將登入 Grafana 並將已驗證的狀態儲存在磁碟上。
   2. `run-tests` 會在您選擇的瀏覽器中執行所有測試。透過新增對 `auth` 專案的依賴，我們確保登入只會發生一次，且 `run-tests` 專案中的所有測試都將以已驗證的狀態開始。

   您的 Playwright 設定應具有以下專案設定：

```ts title="playwright.config.ts"
import { dirname } from 'path';
import { defineConfig, devices } from '@playwright/test';
import type { PluginOptions } from '@grafana/plugin-e2e';

const pluginE2eAuth = `${dirname(require.resolve('@grafana/plugin-e2e'))}/auth`;

export default defineConfig<PluginOptions>({
    ...
    projects: [
    {
      name: 'auth',
      testDir: pluginE2eAuth,
      testMatch: [/.*\.js/],
    },
    {
      name: 'run-tests',
      use: {
        ...devices['Desktop Chrome'],
        // @grafana/plugin-e2e 會將驗證狀態寫入此檔案，
        // 路徑不應修改
        storageState: 'playwright/.auth/admin.json',
      },
      dependencies: ['auth'],
    }
  ],
});
```

已驗證的狀態會以以下檔案名稱模式儲存在磁碟上：`<plugin-root>/playwright/.auth/<username>.json`。

為防止這些檔案被版本控制，您可以將以下行新增至您的 `.gitignore` 檔案：

```shell title=".gitignore"
/test-results/
/playwright-report/
/blob-report/
/playwright/.cache/
/playwright/.auth/
```

## 從 `@grafana/e2e` 遷移

一旦您安裝並設定好 Playwright 和 `@grafana/plugin-e2e`，您可以遵循以下步驟從 `@grafana/e2e` 遷移。

### 遷移測試

目前沒有工具可以自動將現有的基於 `@grafana/e2e` 的 Cypress 測試遷移到基於 `@grafana/plugin-e2e` 的 Playwright 測試。這意味著您必須逐一轉換您的測試，或用一組新的基於 Playwright 的測試取代它們。請參閱以下資源以獲取有關如何編寫 playwright 測試的靈感：

- [如何測試資料來源插件](./test-a-data-source-plugin/index.md)
- [如何測試面板插件](./test-a-panel-plugin.md)
- [有關測試隔離的最佳實踐](./setup-resources.md#test-isolation)
- [如何選取 UI 元素](./selecting-ui-elements.md)
- [插件範例儲存庫](https://github.com/grafana/grafana-plugin-examples)

### 在 CI 中執行測試

若要在 CI 中針對多個 Grafana 版本執行 Playwright 測試，請使用 [CI](./ci.md) 指南中的其中一個範例工作流程。

:::note

請注意，Grafana 不提供任何支援的方式在其他 CI 平台（例如 Drone 或 CircleCI）中針對多個 Grafana 版本執行端對端測試。但是您可以輕鬆地設定您的 CI 來複製所參考的 Github Action 正在做的事情，因為我們沒有做任何無法在其他 CI 系統中完成的特定事情。

:::

### 解除安裝 Cypress 和 @grafana/e2e

雖然我們建議您及時從 `@grafana/e2e` 遷移到 `@grafana/plugin-e2e`，但在過渡階段，沒有什麼能阻止您將兩者並存。

當所有 Cypress 測試都已遷移後，開啟終端機並從您本地的插件開發目錄中執行以下腳本：

#### 1. 移除 Cypress 測試和設定檔

```shell
rm ./cypress.json
rm -rf ./cypress
```

#### 2. 解除安裝依賴項

```shell
npm uninstall --save-dev @grafana/e2e @grafana/e2e-selectors
```

#### 3. 更新腳本

在 `package.json` 檔案中，完全移除 `e2e:update` 腳本，並將 `e2e` 腳本變更為以下內容：

`"e2e": "playwright test",`
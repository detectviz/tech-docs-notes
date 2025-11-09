---
id: get-started
title: 開始使用
description: 開始使用插件端對端測試。
keywords:
  - grafana
  - plugins
  - plugin
  - testing
  - e2e
  - end-to-end
  - get-started
sidebar_position: 2
---

import ScaffoldPluginE2EStartGrafanaNPM from '@shared/plugin-e2e-start-grafana.md';
import ScaffoldPluginE2ERunTestsNPM from '@shared/plugin-e2e-run-tests.md';

[create-plugin](https://www.npmjs.com/package/@grafana/create-plugin?activeTab=readme) 工具會自動為 `@grafana/plugin-e2e` 建立基本設定，幫助您輕鬆開始在您的插件中進行端對端測試。本指南將引導您完成使用 `@grafana/plugin-e2e` 執行 Playwright 測試的基本用法。

:::note
如果您是使用 4.6.0 之前的 create-plugin 版本建立插件，請遵循[遷移指南](./migrate-from-grafana-e2e.md)的說明手動安裝和設定 `@grafana/plugin-e2e`。
:::

## 開始之前

您需要具備以下條件：

- Grafana [插件開發環境](/set-up/)。
- Node.js 18 或更新版本。
- Playwright 的基本知識。如果您以前沒有使用過 Playwright，我們建議您遵循其文件中的[入門](https://playwright.dev/docs/intro)部分。

### 步驟 1：啟動 Grafana

在您的本機上啟動最新版本的 Grafana，如下所示：

<ScaffoldPluginE2EStartGrafanaNPM />

如果您想啟動特定版本的 Grafana，可以透過指定 `GRAFANA_VERSION` 環境變數來達成。例如：

```bash
GRAFANA_VERSION=10.4.1 npm run server
```

### 步驟 2：執行測試

開啟一個新的終端機，並從您本地的插件開發目錄中執行測試腳本。

<ScaffoldPluginE2ERunTestsNPM />

### 步驟 3：在 CI 中執行測試

`plugin.json` 檔案中的 [`grafanaDependency`](../reference/metadata.md#dependencies) 屬性指定了插件與哪些 Grafana 版本相容。最佳實踐是，針對所有支援的版本執行您的 Playwright 端對端測試。使用 `create-plugin` 建立插件時可以包含的 GitHub 工作流程確保了這一點。

如果您在建立插件時選擇不新增 GitHub 工作流程，最佳實踐是遵循 [CI](./ci.md) 指南中的說明，針對您的插件支援的所有 Grafana 版本執行 Playwright 端對端測試。

## 接下來呢？

接下來，我們建議您查看以下指南：

- [在端對端測試中選取 UI 元素](./selecting-ui-elements.md)
- [設定您需要的資源](./setup-resources.md)
- [如何測試資料來源插件](./test-a-data-source-plugin/index.md)
- [如何測試面板插件](./test-a-panel-plugin.md)
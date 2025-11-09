---
id: updating-a-plugin
title: 自動化您的插件更新
description: 學習如何使用 create-plugin update 來自動更新設定檔、工作流程和相依性。
keywords:
  - grafana
  - plugin
  - update
  - config
  - dependencies
---

import UpdateNPM from '@shared/createplugin-update.md';
import UpdateNPMCommit from '@shared/createplugin-update-commit.md';
import UpdateNPMForce from '@shared/createplugin-update-force.md';

# 自動化您的插件更新

保持您的 Grafana 插件工具的更新對其健康至關重要。然而，這樣做可能是一項費力的任務。為了解決這個問題，`create-plugin` 提供了 `update` 指令來幫助您自動化工具更新，包括：

- **設定檔變更**以利用 Grafana、開發工具和 create-plugin 的更新。
- **相依性更新**以應對開發工具的主要版本更新。
- **程式碼重構**以配合設定檔變更或主要相依性更新。

## 更新您的插件

:::info[Git 分支狀態]

在繼續之前建立一個 Git 分支。如果有未提交的變更，請先 stash 或提交它們，否則更新指令將提早結束。

:::

若要更新您的插件，請執行：

<UpdateNPM />

更新完成後，執行您的開發和建置腳本，以驗證您的插件環境是否仍如預期般運作。

### 選項和旗標

#### `--commit`

`--commit` 旗標會在每次成功遷移時將變更提交到目前的分支。這對於偵錯和檢閱更新指令所做的變更很有用。

<UpdateNPMCommit />

#### `--force`

`--force` 旗標可用於繞過所有與未提交變更相關的安全檢查。請謹慎使用。

<UpdateNPMForce />

### 更新時會發生什麼事

更新指令會對您的插件套用一系列稱為遷移的變更，以使其與最新的 `create-plugin` 標準保持一致。

執行時，它會：

- 偵測目前的 create-plugin 版本。
- 決定需要執行哪些遷移才能讓您的插件保持最新。
- 依序執行每個遷移。

每個遷移執行時，其名稱和描述都會輸出到終端機， साथ ही遷移已變更的任何檔案清單。如果遷移更新了任何相依性，它也會安裝和更新任何鎖定檔。

如果您傳遞 `--commit` 旗標，在每個遷移完成後，它會使用遷移的名稱在目前的分支上新增一個 Git 提交。

## 透過 CI 自動化更新

為了讓您的插件更容易保持最新，請使用提供的 GitHub 工作流程，它會執行更新指令，並在有任何變更時自動開啟一個 PR。請遵循[這些步驟](/set-up/set-up-github#the-create-plugin-update-workflow)在您的儲存庫中啟用它。

## 自動化相依性更新

`create-plugin` 只有在其他變更需要它們才能正常運作時才會更新相依性。除了定期執行更新指令外，請使用 [dependabot](https://docs.github.com/en/code-security/getting-started/dependabot-quickstart-guide) 或 [renovatebot](https://docs.renovatebot.com/) 來讓所有相依性保持最新。

## 取得協助

如果您遇到問題，請[開啟一個錯誤報告](https://github.com/grafana/plugin-tools/issues/new?template=bug_report.yml)。
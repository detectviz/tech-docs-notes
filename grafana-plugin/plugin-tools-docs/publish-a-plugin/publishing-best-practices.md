---
id: publishing-best-practices
title: 發布最佳實務
sidebar_position: 0
description: 發布您的插件的最佳實務
keywords:
  - grafana
  - plugins
  - plugin
  - publish
  - best practices
---

# 插件發布最佳實務

發布 Grafana 插件時，請遵循最佳實務，以確保順利的提交、審核流程，並為使用者提供更高品質的體驗。無論您是微調插件的功能還是準備文件，遵循既定指南都有助於提高插件在 Grafana 生態系統中的效能、安全性和可發現性。
本文件中的建議將幫助您避免常見的陷阱、簡化審核流程，並建立一個能無縫整合至使用者工作流程的插件。

## 開始之前

在繼續之前，請確保您已完成以下操作：

- [建立您的初始插件](../)
- 檢閱[插件開發最佳實務指南](../key-concepts/best-practices)
- 熟悉[插件簽章等級](../publish-a-plugin/sign-a-plugin#public-or-private-plugins)

## 填寫您插件的元資料

使用元資料讓您的 Grafana 插件更容易被發現且更方便使用。正確地建構[您 `plugin.json` 檔案中的元資料](../reference/metadata.md)不僅有助於使用者在 [Grafana 的插件目錄](https://grafana.com/grafana/plugins/)中找到您的插件，還提供了有關插件功能和相容性的基本詳細資訊。

請專注於以下關鍵要素：

**[插件名稱](../reference/plugin-json)**

`name`

您插件的名稱必須清晰、簡潔且具描述性。這是潛在使用者的第一個互動點，因此請避免過於通用或隱晦的名稱。目標是取一個能反映插件主要功能並讓人一眼就能了解其用途的名稱。

**[描述](../reference/plugin-json#info)**

`info.description`

描述欄位總結了您的插件的功能以及使用者為何應該安裝它。將描述限制在兩句話以內，突顯核心功能和使用案例。寫得好的描述不僅能告知使用者，還有助於在目錄中獲得更好的搜尋結果。

**[關鍵字](../reference/plugin-json#info)**

`info.keywords`

關鍵字可提高您插件在 Grafana 目錄中的可搜尋性。請選擇能準確描述您插件功能及其支援的資料類型的詞彙，例如「JSON」、「SQL」或「視覺化」。

:::caution

避免關鍵字堆砌。不相關的關鍵字會在審核過程中被標記，可能會延遲發布。

:::

**[標誌](../reference/plugin-json#info)**

`info.logos`

新增標誌可改善您插件在插件目錄中的整體外觀，並為您的插件增添合法性和專業性。

**[螢幕截圖](../reference/plugin-json#info)**

`info.screenshots`

使用螢幕截圖欄位提供一個或多個螢幕截圖影像陣列，以顯示在插件目錄中。影像為使用者提供了您插件的視覺化表示，並可以幫助他們在安裝前決定您的插件是否能解決他們的問題。

請確保您的螢幕截圖：

- 展示您的插件的實際操作，突顯其突出功能
- 具有合適的解析度和檔案類型，例如 png、jpeg 或 gif

**[贊助連結](../reference/plugin-json#infolinks)**

`info.links`

贊助連結為使用者提供了一種支援您工作並為其開發做出貢獻的方式。它會出現在您插件詳細資訊頁面的「連結」區段中，並支援各種資助平台，例如 GitHub Sponsors 或 Patreon。

範例：

```
{
  info: {
    links: [
      {
        name: "sponsorship",
        url: "https://github.com/sponsors/pluginDeveloper"
      }
    ]
  }
}
```

**[Grafana 版本相容性](../reference/plugin-json#dependencies)**

`dependencies.grafanaDependency`

指定您的插件相容的最低 Grafana 版本，以便執行不同版本 Grafana 的使用者知道您的插件是否適用於他們。請務必[執行端對端測試](../e2e-test-a-plugin/index.md)以確認與您支援的版本的相容性。

## 建立一份全面的 README

您插件的 README 檔案既是第一印象，也是為您的使用者提供的詳細指南。它結合了店面廣告和說明手冊——展示了您的插件的功能、如何安裝，以及使用者如何在他們的 Grafana 執行個體中充分利用它。

使用[README 範本](https://raw.githubusercontent.com/grafana/plugin-tools/main/packages/create-plugin/templates/common/src/README.md)（作為 `create-plugin` 工具產生的插件結構的一部分），為您的使用者提供他們自信地使用和貢獻您的插件所需的一切。

該範本涵蓋了基本元件，您可以新增更具體的詳細資訊來幫助使用者了解您插件的價值和功能，例如：

- **螢幕截圖或螢幕錄製：** 包含螢幕截圖甚至影片示範，以便使用者可以快速掌握插件的功能和設定過程，讓他們有信心有效地使用它。
- **動態徽章：** 徽章提供有關您插件的快速資訊，例如最新版本或是否已通過安全性和程式碼檢查。使用 [shields.io](https://shields.io/) 等工具搭配 Grafana.com API，在您每次發布新版本時自動更新這些徽章，為您的插件增添透明度和可信度。
- **貢獻指南：** 維護一個插件可能很費力，特別是對於個人開發者而言。清楚地概述使用者如何提供回饋和回報錯誤，並將潛在的程式碼貢獻者引導至您的 `contributing.md`。這可以促進社群參與，並使隨著時間的推移維護和改進您的插件變得更容易。

## 維護詳細的變更日誌

維護良好的變更日誌對於插件的透明度至關重要，並有助於使用者了解版本之間的變更。Grafana 會在插件詳細資訊頁面中顯示您的變更日誌，以便使用者可以評估是否要更新。

:::info

使用 Grafana 的自動變更日誌產生功能來簡化維護變更日誌的過程。請在[自動產生變更日誌](../publish-a-plugin/build-automation.md#generate-changelogs-automatically)指南中了解如何操作。

:::

### 變更日誌最佳實務

在您的儲存庫根目錄中使用一個專用的 CHANGELOG.md 檔案，並包含以下資訊：

1. 遵循語意版本控制 (MAJOR.MINOR.PATCH) 並按版本組織條目
2. 為每個版本加上日期以提供時間順序上下文
3. 按類型對變更進行分組，例如「功能」、「錯誤修正」、「破壞性變更」...
4. 參考帶有連結的拉取請求以提供額外的上下文
5. 突顯破壞性變更以提醒使用者需要採取的措施

### 範例

```markdown
### [1.10.0](https://github.com/user/plugin-name/tree/1.10.0) (2025-04-05)

**已實作的增強功能：**

- 新增深色主題支援 [\#138](https://github.com/user/plugin-name/pull/138) ([username](https://github.com/username))
- 新增自訂工具提示格式的選項 [\#135](https://github.com/user/plugin-name/pull/135) ([username](https://github.com/username))
- 支援 PostgreSQL 資料來源 [\#129](https://github.com/user/plugin-name/pull/129) ([username](https://github.com/username))

**已修正的錯誤：**

- 修正在切換儀表板時面板崩潰的問題 [\#139](https://github.com/user/plugin-name/pull/139) ([username](https://github.com/username))
- 修正不一致的時區處理 [\#134](https://github.com/user/plugin-name/pull/134) ([username](https://github.com/username))

**已關閉的問題：**

- 文件需要 PostgreSQL 查詢的範例 [\#130](https://github.com/user/plugin-name/issues/130)

**已合併的拉取請求：**

- 更新相依性以解決安全性漏洞 [\#140](https://github.com/user/plugin-name/pull/140) ([username](https://github.com/username))

**破壞性變更：**

- 遷移設定儲存格式 [\#115](https://github.com/user/plugin-name/pull/115) ([username](https://github.com/username))
```

使用此格式，您的變更日誌將成為一個透明的資源，清楚地傳達變更、致謝貢獻，並提供指向更詳細資訊的連結。它有助於使用者就更新您的插件做出明智的決定，並展示您維護高品質 Grafana 插件的承諾。

## 端對端測試

端對端 (E2E) 測試可確保您的 Grafana 插件在各種環境和支援的 Grafana 版本中都能正常運作。它透過在類似於最終使用者設定的環境中測試插件來複製真實世界的使用情況。實作 E2E 測試有助於在提交前發現問題，從而節省審核過程中的時間並確保更順暢的使用者體驗。

**關鍵點：**

- **跨版本測試相容性：** 透過設定針對多個版本的 E2E 測試，確保您的插件能與各種版本的 Grafana 無縫運作。
- **自動化測試：** 將 E2E 測試整合至您的持續整合 (CI) 管線中，以及早且頻繁地發現問題，從而減少審核期間的潛在問題。

有關設定 E2E 測試的綜合指南，請參閱我們的 [E2E 測試插件](../e2e-test-a-plugin/index.md)文件。

## 驗證您的插件

在提交您的插件進行審核之前，請使用插件驗證器檢查可能導致您的插件被拒絕的潛在問題，例如安全性漏洞或結構性問題。

**關鍵點：**

- **在本地或 CI 中執行：** 您可以在本地執行驗證器，或將其整合至您的 CI 工作流程中以自動化驗證過程。請注意，驗證器會在預設的發布工作流程中自動執行。
- **驗證報告：** 該工具會產生一份報告，突顯在提交前需要解決的任何錯誤或警告。

有關使用驗證器的更多資訊，請參閱[插件驗證器文件](https://github.com/grafana/plugin-validator)。

## 提供一個已佈建的測試環境

為您的插件佈建一個測試環境可以顯著縮短審核時間，並讓其他人更容易測試和貢獻您的插件。一個已佈建的環境包含一個預先設定的 Grafana 執行個體，其中包含範例儀表板和資料來源，可展示您插件的功能。

**關鍵點：**

- **為何佈建很重要：** 它確保審核人員和貢獻者都能快速驗證您插件的行為，而無需手動設定，從而加快審核和協作過程。
- **自動化設定：** 您可以使用 Docker 來佈建測試環境，以建立一個複製典型 Grafana 設定的開箱即用體驗。

若要深入了解如何設定已佈建的環境，請查看我們的[佈建指南](../publish-a-plugin/provide-test-environment.md)。

## 使用 GitHub Actions 自動化發布

為了簡化您的插件開發工作流程，請使用 GitHub Actions 自動化發布。這可確保您的插件在每次發布時都能正確地建置、簽署和打包，從而減少人為錯誤並加快發布過程。

**關鍵點：**

- **持續整合 (CI)：** 使用 GitHub Actions 在每次提交或拉取請求時自動建置和測試您的插件，以及早發現問題。
- **發布工作流程：** 在您準備好發布時自動化插件的簽署和打包，確保其符合提交至 Grafana 插件目錄的必要標準。

有關更多詳細資訊，請參閱 Grafana 的[使用 GitHub 自動化打包和簽署](../publish-a-plugin/build-automation.md)指南。

## 後續步驟

遵循這些最佳實務可以提高插件提交成功的機會。它們旨在確保您的插件不僅能通過我們的審核流程，還能為使用者提供卓越的體驗。採用這些實務將簡化您的工作流程，並有助於建立在 Grafana 生態系統中脫穎而出的插件。

一旦您的插件準備好發布，請遵循我們的[提交您的插件進行審核](../publish-a-plugin/publish-or-update-a-plugin.md)指南。我們期待看到您的創作！
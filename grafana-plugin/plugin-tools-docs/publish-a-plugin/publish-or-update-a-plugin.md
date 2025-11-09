---
id: publish-a-plugin
title: 發布或更新插件
sidebar_position: 4
description: 如何打包和分享您的 Grafana 插件。
keywords:
  - grafana
  - plugins
  - plugin
  - publish plugin
  - update plugin
  - provision
---

# 發布或更新插件

您剛剛建立了您的插件，現在您想與全世界分享它！

將您的插件發布到 [Grafana 插件目錄](https://grafana.com/plugins)可讓數百萬 Grafana 使用者輕鬆發現它。請繼續閱讀以了解如何管理目錄中插件的生命週期，從發布和更新到可能的棄用。

## 開始之前

- [檢閱我們的指南](https://grafana.com/legal/plugins/#plugin-publishing-and-signing-criteria) - 了解 Grafana Labs 發布和簽署插件的標準。
- [檢閱我們的發布最佳實務](../publish-a-plugin/publishing-best-practices.md) - 在提交審核之前，確保您的插件處於最佳狀態。
- [打包插件](./package-a-plugin.md) - 建置插件並準備好以 ZIP 封存的形式分享。
- 請參閱 [plugin-examples](https://github.com/grafana/grafana-plugin-examples) 以檢閱建置插件的最佳實務。

**為加快審核您的插件所需的時間：**

- 使用[插件驗證器](https://github.com/grafana/plugin-validator)檢查您的插件是否已準備好進行審核。
- 在您的儲存庫中提供範例儀表板和測試資料，以便可以驗證插件的功能。使用提供的[佈建](./provide-test-environment.md)流程來簡化此步驟。

## 發布您的插件

請遵循以下步驟首次發布您的插件。

1. [登入](https://grafana.com/auth/sign-in)您的 Grafana Cloud 帳戶。請注意，您需要是用於發布插件的 Grafana Cloud 組織的管理員。
2. 在左側選單的「組織設定」下，按一下「我的插件」。
3. 按一下「提交新插件」。此時會出現「建立插件提交」對話方塊。

   ![提交插件。](/img/plugins-submission-create.png)

4. 輸入表單要求的資訊。
   - **作業系統與架構：**
     - 如果您的插件封存包含多個架構的二進位檔，請選取**單一**。
     - 如果您想為每個架構提交單獨的插件封存，請選取**多個**。
       這可以加快下載速度，因為使用者可以選取他們想要安裝插件的特定架構。
   - **URL：** 指向您打包插件的 ZIP 封存的 URL。
   - **原始碼 URL：** 指向您完整插件原始碼的公開 Git 儲存庫或 ZIP 封存的 URL。
   - **SHA1：** 由 **URL** 指定的插件的 SHA1 雜湊值。
   - **測試指南：** 涵蓋您插件的安裝、設定和用法的概觀。
   - **為測試環境提供佈建：** 如果您已[設定佈建](./provide-test-environment.md)，請勾選此方塊。如果您已完成此操作，請放心，審核期間會識別出來，您無需採取任何其他動作。
   - 其餘問題有助於我們確定您插件的[簽章等級](https://grafana.com/legal/plugins/#what-are-the-different-classifications-of-plugins)。
5. 按一下 **Submit**。
   提交您的插件後，我們會執行自動化驗證，以確保其符合我們的指南。
   一旦您的提交通過驗證，它就會被放入審核佇列。
   所有提交都由插件審核員手動檢查。
   對於每個新插件，我們都會執行手動審核，其中包括以下檢查：

   - **程式碼審核：** 出於品質和安全目的，我們會審核插件的原始碼。
   - **測試：** 我們會在我們的其中一個 Grafana 執行個體上安裝您的插件，以測試其基本用法。
     我們可能會要求您協助我們為插件設定測試環境。
     每當您提交插件更新時，我們都會使用該測試環境。

## 更新您的插件

若要更新插件，請遵循與[發布您的插件](#publish-your-plugin)相同的指南，但在步驟 3 中，您現在可以按一下您要更新的插件的 **Submit Update**。

所有插件提交，無論是新的還是更新，都會經過相同的自動化和嚴格的手動審核流程。由於我們可能已經為現有插件設定了測試環境，因此插件更新審核可能會更快。

## 棄用插件

如果插件不再相關或無法維護，插件開發人員可以請求將該插件棄用並從目錄中移除。同樣地，Grafana Labs 可能會作為策劃目錄和確保插件符合我們的安全、品質和相容性標準的一部分，棄用並下架插件。

有關插件棄用的更多資訊以及如何請求棄用您的插件，請參閱 Grafana Labs [插件棄用政策](https://grafana.com/legal/plugin-deprecation/)。

## 常見問題

### 我需要提交私有插件嗎？

- 不用。請只提交您希望向 Grafana 社群公開提供的插件。

### 審核提交需要多長時間？

- 我們目前無法提供預估時間，但我們一直在努力縮短審核插件所需的時間。提供[已佈建](./provide-test-environment.md)的測試環境可以大幅加快您的審核速度。

### 我可以決定我的插件何時發布嗎？

- 不行。我們無法保證特定的發布日期，因為插件在根據我們的內部優先順序進行審核後會立即發布。

### 我可以查看我的插件安裝、下載或使用量的指標嗎？

- 不行。我們目前不向插件作者提供此資訊。

### 如何更新我的插件目錄頁面？

- 插件目錄頁面的內容是從插件 README 檔案中擷取的。
  若要更新插件的目錄頁面，請提交一個更新的插件，並在 README 檔案中包含新內容。

### 我可以下架插件嗎？

- 如果發生錯誤，在特殊情況下（例如安全問題），可以從我們的目錄中下架插件。但是，我們無法控制已安裝插件的執行個體。

- 此外，請參閱 Grafana Labs [插件棄用政策](https://grafana.com/legal/plugin-deprecation/)以深入了解插件棄用。

### 我可以在 Grafana 插件目錄以外的其他地方分發我的插件嗎？

- 分發 Grafana 插件的官方方法是透過我們的目錄。根據[本指南](https://grafana.com/docs/grafana/latest/administration/plugin-management#install-plugin-on-local-grafana)中提供的指南，可以使用其他方法，例如在本地 Grafana 執行個體上安裝私有或開發插件。

### 我還可以使用 Angular 來開發插件嗎？

- 不行。我們將不接受任何以 Angular 撰寫的新插件提交。有關更多資訊，請參閱我們的[Angular 支援棄用文件](https://grafana.com/docs/grafana/latest/developers/angular_deprecation/)。

### 我可以提交使用 Toolkit 建立的插件嗎？

- @grafana/toolkit 工具已棄用。請[遷移至 `create-plugin`](../migration-guides/migrate-from-toolkit.mdx)。未來，我們將拒絕基於 @grafana/toolkit 的提交，因為它會越來越過時。

### 所有插件都需要簽章嗎？

- 所有插件都需要簽章，除非它們處於開發階段或首次提交審核。

### 插件簽章會過期嗎？

- 插件簽章目前不會過期。

### 支援哪些原始碼 URL 格式？

- 使用標籤或分支：`https://github.com/grafana/clock-panel/tree/v2.1.3`
- 使用標籤或分支且程式碼位於子目錄中（對於單一儲存庫很重要）：`https://github.com/grafana/clock-panel/tree/v2.1.3/plugin/`（此處，插件包含插件程式碼）
- 使用最新的 main 或 master 分支提交：`https://github.com/grafana/clock-panel/`（不建議，最好傳遞標籤或分支）
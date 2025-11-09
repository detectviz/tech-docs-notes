---
id: include-dashboards
title: 在 Grafana 資料來源插件中包含儀表板
sidebar_label: 新增儀表板
description: 在 Grafana 資料來源插件中包含儀表板。
keywords:
  - grafana
  - plugin
  - dashboards
  - datasource
  - bundling
---

本指南說明如何將預先設定的儀表板新增至 Grafana 資料來源插件中。透過將預先設定的儀表板整合至您的插件，您可以為您的使用者提供一個立即可用的範本，讓他們不必從頭開始建立儀表板。

我們將引導您完成將儀表板捆綁至插件的過程。此過程包括建立儀表板、將其新增至您的插件，然後將其匯入插件。

## 步驟 1：建立儀表板

首先建立您要與插件捆綁的儀表板。create-plugin 提供的[開發環境](/set-up/)有助於建立和測試儀表板。

### 匯出儀表板

在此步驟中，我們將儀表板匯出為 JSON 檔案，以便將其與您的插件原始碼一起放置在檔案中：

1. 在 Grafana 中開啟您的儀表板。
2. 按一下儀表板左上角的 **Share** 圖示。
3. 按一下 **Export**。
4. 選取 **Export for sharing externally**，然後按一下 **Save to file**。

使用此選項匯出會將直接的資料來源參考取代為預留位置。這可確保在匯入時儀表板可以使用使用者的資料來源執行個體。

## 步驟 2：將儀表板新增至您的插件

1. 在您插件專案的 `src` 目錄中建立一個 `dashboards` 資料夾。

2. 將您匯出的儀表板 JSON 檔案移至新的 `dashboards` 資料夾。
   ```shell
   myorg-myplugin-datasource/
   └── src/
   // addition-highlight-start
       ├── dashboards/
       │   └── overview.json
   // addition-highlight-end
       ├── module.ts
       └── plugin.json
   ```
3. 更新您的 `plugin.json` 檔案以包含對新儀表板資源的參考，並指定 `src` 資料夾中儀表板檔案的相對路徑。

   ```json title="src/plugin.json"
   {
     "includes": [
       {
         "name": "overview",
         "path": "dashboards/overview.json",
         "type": "dashboard"
       }
     ]
   }
   ```

   :::info

   確保路徑相對於 `src` 目錄。這對於插件在您建置前端後能正確地從 `dist` 目錄參考儀表板 JSON 檔案是必要的。

   :::

4. 將儀表板新增至您的插件後，重新建置插件並重新啟動 Grafana 以套用新設定。

## 步驟 3：將儀表板匯入您的插件

若要測試您新建立的儀表板，請匯入儀表板：

1. 建立或編輯您資料來源的現有執行個體。
2. 按一下 **Dashboards** 以列出所有包含的儀表板。
3. 在您要匯入的儀表板旁邊按一下 **Import**。儀表板隨即匯入您的插件。

## 讓您的儀表板保持最新

Grafana 儀表板結構描述會隨著時間的推移而演變，未定期更新的儀表板可能會過時。過時的儀表板在載入時可能需要耗時的遷移，或者可能無法與較新的 Grafana 功能正常運作。以下是確保您的儀表板保持最新狀態的方法：

1. 遵循上述[步驟 3](#step-3-import-the-dashboard-into-your-plugin) 中的步驟匯入您的儀表板。此過程會自動執行任何必要的遷移以將儀表板更新至最新的結構描述。
2. 載入後，按一下儀表板頂部選單中的 **Export** 按鈕，然後選取 **Export as JSON**。
3. 請務必勾選 **Export the dashboard to use in another instance** 選項。
4. 透過以下任一方式儲存更新後的儀表板：
   - 按一下 **Download file** 並取代您現有的 JSON 檔案
   - 使用 **Copy to clipboard** 並將內容貼到您現有的 JSON 檔案中
5. （可選）為了幫助使用者識別最新的儀表板版本，請在 JSON 檔案的根層級增加 `version` 號碼（例如，從 1 到 2）。這使得儀表板更新了較新的功能或修正時一目了然。

## 結論

透過將儀表板與您的插件捆綁，您可以顯著改善使用者入門體驗。預先設定的儀表板讓使用者無需從頭開始設定常見的變數、面板或查詢。這可以大大提高使用者的滿意度和效率！
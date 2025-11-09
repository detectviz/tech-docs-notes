---
id: include-dashboards-in-app-plugins
title: 在應用程式插件中包含儀表板
description: 在 Grafana 應用程式插件中包含儀表板。
keywords:
  - grafana
  - plugin
  - dashboards
  - app
  - datasource
  - bundling
---

# 在應用程式插件中包含儀表板

本指南說明如何將預先設定的儀表板新增至應用程式插件中。透過將預先設定的儀表板整合至您的插件，您可以為您的使用者提供一個立即可用的範本，讓他們不必從頭開始建立儀表板。

我們將引導您完成將儀表板捆綁至插件的過程。此過程包括建立儀表板、將其新增至您的插件，然後將其匯入插件。您也可以選擇性地新增導覽連結，讓使用者更容易發現其所有功能。

## 步驟 1：建立儀表板

首先建立您要與插件捆綁的儀表板。create-plugin 提供的[開發環境](/set-up/)有助於建立和測試儀表板。

### 設定資料來源變數

為了方便使用者自訂，請建立一個儀表板資料來源變數。這可讓使用者在匯入後輕鬆連結自己的資料來源執行個體。

1. 透過選擇資料來源變數類型並為其命名來建立資料來源變數。
   ![資料來源變數](/img/app-dashboard-ds-variable.png)
2. 為您建立的每個面板選取資料來源變數作為資料來源。
   ![資料來源選取](/img/app-dashboard-ds-select.png)

### 匯出儀表板

1. 在 Grafana 應用程式中前往您的儀表板。
2. 按一下儀表板左上角的 **Share** 圖示。
3. 按一下 **Export**，然後按一下 **Save to file**。
4. 在您的程式碼編輯器中開啟儀表板 JSON 檔案，並將其 `id` 屬性設定為 `null`。

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

5. Grafana 伺服器會自動匯入應用程式插件中包含的儀表板。導覽至您的儀表板以驗證其是否如預期般運作。

## 步驟 3：在應用程式插件中新增導覽連結（可選）

應用程式插件可以透過在 `plugin.json` 中新增導覽連結來增強使用者導覽。包含項的路徑應參考捆綁的儀表板 `uid` 屬性。

```json title="src/plugin.json"
{
  "includes": [
    {
      "name": "My App Dashboard",
      "path": "dashboards/overview.json",
      "type": "dashboard"
    },
    // addition-highlight-start
    {
      "addToNav": true,
      "name": "My App Dashboard",
      "path": "d/ffb13c35-2f2f-4f36-99b1-bde7244e8de3",
      "type": "page"
    }
    // addition-highlight-end
  ]
}
```

## 結論

透過將儀表板與您的應用程式插件捆綁，您可以顯著改善使用者入門體驗。預先設定的儀表板讓使用者無需從頭開始設定常見的變數、面板或查詢。這可以大大提高使用者的滿意度和效率！
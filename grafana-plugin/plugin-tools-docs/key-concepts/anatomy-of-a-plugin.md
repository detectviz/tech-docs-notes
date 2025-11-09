---
id: anatomy-of-a-plugin
title: 插件的剖析
description: 本指南描述了插件的剖析，包括構成每種類型插件的各個元件。
keywords:
  - grafana
  - plugins
  - plugin
  - create-plugin
  - folders
  - anatomy
  - components
  - apps
  - data sources
  - panels
sidebar_position: 0
---

# 插件的剖析

Grafana 插件可讓您透過新增自訂功能（例如新的資料來源、視覺化或整個應用程式）來擴充 Grafana 的核心功能。

本指南將引導您了解 Grafana 插件的基本元件，以及如何建構和組織它們。您將了解 `create-plugin` 工具產生的資料夾結構，以及下一步該去哪裡學習如何使用建立的範本來建置和擴充插件。

**觀看下面的影片**以取得 Grafana 插件結構和元件的視覺化概觀。此影片補充了本指南中的書面內容，並提供了對 Grafana 插件剖析的實用見解。

<YouTubeEmbed videoId="dzFkEAVwjGI" title="Learn the Anatomy of a Grafana Plugin" />

## 開始之前

在繼續之前，請檢閱[插件類型和用法指南](/key-concepts/plugin-types-usage)以對可用的不同插件類型有基本的了解。

每個 Grafana 插件都由幾個基本元件組成，這些元件以不同的方式擴充 Grafana 的功能。首先，我們將探討三種主要插件類型的核心部分：應用程式、資料來源和面板。

![可用的不同插件類型：應用程式、資料來源和面板](./images/plugin-types.png)

## 應用程式插件

應用程式插件提供了最大的靈活性，讓開發人員可以建立超越基本視覺化或資料互動的自訂體驗。它們可以包含自訂頁面、用於伺服器端邏輯的後端，以及可掛接到 Grafana 核心功能的 UI 擴充功能。

![應用程式插件的元件](./images/app-plugin.png)

### 頁面

應用程式可以新增可從 Grafana 導覽列存取的自訂頁面。這些頁面基本上是 React 元件，可讓開發人員建立自訂使用者介面。若要新增頁面，開發人員可以使用 `@grafana/runtime` 套件中的 `PluginPage` 元件。您可以在我們的應用程式教學中[深入了解如何將頁面新增至應用程式](/tutorials/build-an-app-plugin#add-a-page-in-the-navigation-menu)。

![自訂應用程式頁面範例](./images/app-pages.png)

### 設定

應用程式插件通常包含設定頁面，使用者可以在其中輸入必要的設定，例如 API 憑證或其他參數。您可以在我們的應用程式教學中[深入了解如何將設定頁面新增至應用程式](/tutorials/build-an-app-plugin#configuration-page)。

![應用程式設定頁面範例](./images/app-configuration.png)

### UI 擴充功能

應用程式插件可以註冊和公開可掛接到核心 Grafana 功能的 UI 擴充功能，提供額外的功能或互動點。這些擴充點允許與 Grafana 的 UI 進行強大的整合。您可以在我們的操作指南中[深入了解 UI 擴充功能](/how-to-guides/ui-extensions/)。

![面板視覺化範例](./images/ui-extension.png)

### 健康狀況檢查

應用程式可以定義健康狀況檢查，以確保插件已正確設定且可正常運作。您可以根據插件的後端邏輯自訂這些檢查。有關實作詳細資訊，請參閱我們的[文件](/how-to-guides/data-source-plugins/convert-a-frontend-datasource-to-backend#health-check)。

### 呼叫資源

應用程式可以有後端來處理伺服器端功能，例如進行外部 API 呼叫或處理更進階的驗證方法。`CallResourceHandler` 方法通常用於此目的。有關實作詳細資訊，請參閱我們的[文件](/how-to-guides/app-plugins/add-backend-component#add-a-custom-endpoint-to-your-app-plugin)。

### 巢狀插件

應用程式插件可以將多個插件（例如資料來源或面板）捆綁到一個可安裝的套件中。對於需要結合多個插件才能完整運作的服務，此方法很有用。您可以在我們的文件中[深入了解如何使用巢狀插件](/how-to-guides/app-plugins/work-with-nested-plugins)。

## 資料來源插件

資料來源插件可讓 Grafana 連線到外部服務、設定查詢和顯示資料。它們可以包含僅限前端或全端元件（含後端）。

![資料來源插件的元件](./images/data-source-plugin.png)

### 設定編輯器

設定編輯器是使用者在設定資料來源的特定執行個體時提供外部服務的連線詳細資料（例如，API 金鑰、URL）的地方。若要定義設定編輯器，請使用 `setConfigEditor()` 並傳遞一個自訂設定元件。您可以在我們的資料來源教學中[了解如何定義設定編輯器](/tutorials/build-a-data-source-plugin#enable-configuration-for-your-datasource)。

請確保使用 `secureJson` 安全地儲存您的敏感資料。有關更多詳細資訊，請閱讀我們關於[為資料來源插件新增驗證](/how-to-guides/data-source-plugins/add-authentication-for-data-source-plugins#store-configuration-in-securejsondata)的指南。

![資料來源設定編輯器範例](./images/datasource-configeditor.png)

### 查詢編輯器

查詢編輯器可讓使用者針對連線的服務建構查詢。在儀表板中新增面板、使用「探索」以及建立新的警示規則時，都會使用此編輯器。查詢編輯器可以自訂以提供[程式碼編輯器](https://github.com/grafana/grafana/blob/main/packages/grafana-ui/src/components/Monaco/CodeEditor.tsx)以及引導式查詢產生器。您可以在我們的資料來源插件教學中[了解如何定義查詢編輯器](/tutorials/build-a-data-source-plugin#define-a-query)。

![資料來源查詢編輯器範例](./images/datasource-queryeditor.png)

### 健康狀況檢查

資料來源設定頁面中的「儲存並測試」按鈕可讓使用者驗證連線是否正常。插件可以透過[新增自訂健康狀況檢查](/how-to-guides/data-source-plugins/convert-a-frontend-datasource-to-backend#health-check)來自訂此行為。

![正在執行資料來源健康狀況檢查](./images/datasource-healthcheck.png)

### 查詢資料

`QueryData` 方法會處理多個查詢並傳回對應的回應。每個查詢都包含一個 `RefID`，它會對應到 `QueryDataResponse` 中的回應。該方法會迴圈遍歷查詢、個別處理它們，並傳回結果或帶有適當狀態碼的錯誤。

此方法可有效率地處理多個查詢，並內建記錄和錯誤管理功能，以確保順暢運作。

請參閱[後端資料來源教學](/tutorials/build-a-data-source-backend-plugin#run-multiple-queries-concurrently)。

### 呼叫資源

自訂端點可讓資料來源插件公開自己的 HTTP API 路由以處理伺服器端功能。這在處理驗證、進階查詢或處理大型資料集時特別有用。您可以使用 `CallResourceHandler` 方法在後端建立自訂端點，以處理請求並回應資料或狀態資訊。

有關如何實作自訂端點的範例，請參閱[文件](/how-to-guides/app-plugins/add-backend-component#add-a-custom-endpoint-to-your-app-plugin)。

## 面板插件

面板插件透過提供自訂元件來增強 Grafana，這些元件可在儀表板中提供獨特的資料視覺化或其他實用的小工具功能。

![面板插件的元件](./images/panel-plugin.png)

### 視覺化

面板插件在 Grafana 儀表板中提供資料的視覺化表示。若要建立自訂視覺化，開發人員使用 React 元件來定義資料將如何在儀表板上呈現。此視覺化可以是任何東西，從簡單的圖表到複雜的互動式小工具。面板的 `render()` 函式定義了資料如何傳遞到視覺化中，以及在資料或選項變更時如何處理更新。

有關面板視覺化的更多詳細資訊，請參閱[面板插件教學](/tutorials/build-a-panel-plugin)。

![面板視覺化範例](./images/panel-visualization.png)

### 面板選項

面板選項可讓使用者自訂面板插件的行為和外觀。您可以透過實作 `OptionsEditor` 元件來定義這些選項，該元件可以公開與視覺化相關的選項。這些選項會傳遞到面板的 `render()` 函式中，從而可以根據使用者輸入進行動態更新。

您可以在[基本面板教學](/tutorials/build-a-panel-plugin)中看到如何實作面板選項的範例。

![右側自訂面板選項範例](./images/panel-options.png)

## 插件資料夾結構

執行 `create-plugin` 工具以產生插件的新資料夾。插件資料夾遵循標準命名慣例（例如，`organization-pluginName-pluginType`），並包含建置、執行和測試插件所需的所有檔案。

以下是資料夾版面配置和主要檔案的概觀：

```
myorg-myplugin-datasource/
├── pkg/
│   ├── main.go
│   └── plugin/
├── src/
│   ├── module.ts
│   └── plugin.json
└── tests/
├── CHANGELOG.md
├── docker-compose.yaml
├── go.mod
├── package.json
├── LICENSE
├── Magefile.go
├── README.md
```

:::note

`create-plugin` CLI 工具不斷改進，因此本文件與您建立的插件專案之間可能存在細微差異。

:::

### 主要插件檔案

以下檔案對您的插件開發和功能至關重要：

- **前端程式碼** (`src/`)：必要。此目錄包含您插件的所有前端程式碼。這裡要注意的主要檔案是 `plugin.json` 和 `module.ts`。
  - `plugin.json`：儲存[有關您插件的元資料](/reference/plugin-json)，包括其描述、支援的 Grafana 版本和相依性等資訊。
  - `module.ts`：您插件前端邏輯的進入點。
- **後端程式碼** (`pkg/`)：如果您的插件具有後端元件，則為必要。如果您的插件包含後端功能，程式碼將位於此目錄中，通常在 `pkg/plugin/` 內。插件後端元件是以 Go 撰寫的，`main.go` 作為您後端邏輯的進入點。
- **測試檔案** (`tests/`)：可選，但強烈建議以確保插件品質。此資料夾包含您插件的測試檔案，通常以 `.spec.ts` 為後綴，用於前端測試。您可以在我們的 E2E 測試指南中[深入了解如何測試您的插件](/e2e-test-a-plugin/index.md)。
- **其他檔案**：
  - `docker-compose.yaml`：僅 Docker 環境需要。包含用於執行 Grafana 本地開發執行個體的 Docker 設定。
  - `CHANGELOG.md`：必要。記錄插件的變更和更新歷史。
  - `README.md`：必要。提供插件的概觀，包括安裝說明和用法指南。

## 後續步驟

現在您已經了解了 Grafana 插件的基本元件和專案結構，我們推薦一些資源：

- **教學**：透過我們的 [Grafana 插件教學](/tutorials)深入了解特定的插件開發任務。這些指南將幫助您建立和自訂資料來源、面板和應用程式插件。
- **插件範例**：查看 GitHub 上的 [Grafana 插件範例儲存庫](https://github.com/grafana/grafana-plugin-examples)，以取得展示各種插件類型的範例專案。
- **社群**：加入 [Grafana 社群](https://community.grafana.com/c/plugin-development/30)以取得建議、分享經驗並向其他插件開發人員尋求協助。
- **插件發布**：當您準備好分享您的插件時，[了解如何發布 Grafana 插件](/publish-a-plugin/publish-a-plugin)。

這些資源將引導您完成建置、測試和最終發布插件的更精細細節，確保您擁有順暢的開發流程。
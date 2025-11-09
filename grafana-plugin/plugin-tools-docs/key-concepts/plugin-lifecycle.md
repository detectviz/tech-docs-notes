---
id: plugin-lifecycle
title: 插件的生命週期
description: 了解 Grafana 插件的生命週期。
keywords:
  - grafana
  - plugins
  - plugin
  - lifecycle
  - life cycle
  - loading
  - unloading
  - installation
sidebar_position: 20
---

# 插件的生命週期

本文件描述了插件的各個階段，例如安裝和載入。插件生命週期的差異取決於其[類型](./plugin-types-usage)以及是否具有[後端元件](./backend-plugins/)。

## 安裝和解除安裝插件

有關安裝或解除安裝插件的說明，請參閱[插件管理](https://grafana.com/docs/grafana/latest/administration/plugin-management/#install-grafana-plugins)。如果您正在使用 Grafana Cloud，請參閱[尋找和使用 Grafana 插件](https://grafana.com/docs/grafana-cloud/introduction/find-and-use-plugins/)。

安裝後，插件會被解壓縮到您檔案系統中的_插件目錄_。同樣地，解除安裝插件會從同一個目錄中刪除檔案。

:::note

使用 Grafana CLI 安裝或解除安裝插件需要您重新啟動 Grafana 才能使變更生效。為避免重新啟動 Grafana，您可以在執行階段直接從 Grafana [插件目錄](https://grafana.com/plugins/)安裝插件。

:::

## 載入插件

插件會在 Grafana 啟動時或在執行階段安裝/解除安裝插件時載入。

了解 Grafana 載入插件時涉及的不同階段，可能有助於您更了解插件的用法並[對任何非預期行為進行疑難排解](#troubleshooting)。例如，為什麼即使您已安裝某個插件，它在插件目錄中仍未標示為已安裝或無法在 Grafana 中使用。

對於具有[後端元件](./backend-plugins)的插件，Grafana 會執行[階段 3](#phase-3-plugin-backend-initialization) 中描述的額外初始化程序。

:::note

插件的生命週期是在記憶體中追蹤的，不會保存在 Grafana 的資料庫中。這意味著每次重新啟動伺服器時都會發生以下描述的階段。

:::

### 階段 1. 插件探索

Grafana 透過掃描檔案系統中每個 `plugin.json` 的目錄來探索已安裝的插件。

### 階段 2. 插件載入

在探索階段發現的所有插件都會被檢查以確保它們是有效的。一些自動化檢查包括：

- 插件必須具有有效的[簽章](https://grafana.com/docs/grafana/latest/administration/plugin-management/#plugin-signatures)。有效的插件被稱為_已驗證的插件_。
- Angular 偵測。鑑於 [Angular 已被棄用](https://grafana.com/docs/grafana/latest/developers/angular_deprecation/)，如果停用了 Angular 支援並在插件中偵測到 Angular，則我們會記錄一個錯誤並且不允許載入該插件。

### 階段 3. 插件後端初始化

對於任何具有後端元件的已驗證插件，Grafana 會設定後端用戶端以透過 RPC 使用 HashiCorp 的 Go 插件系統。

### 階段 4. 註冊

所有已驗證的插件都會在記憶體中的註冊表中註冊，並在 Grafana 中變得可用。

已註冊的插件在目錄中會顯示為已安裝，並出現在儀表板中選取面板或資料來源的檢視中。

### 階段 5. 啟動插件後端

對於具有後端元件的已註冊插件，Grafana 會使用 HashiCorp 的 Go 插件系統透過 RPC 將後端二進位檔案作為一個獨立的程序執行。Grafana（用戶端）和插件（伺服器）之間會協商支援的插件協定和版本，以讓 Grafana 了解插件的功能。

Grafana 插件後端元件有其自己獨立的生命週期。只要插件後端正在執行，Grafana 就會確保在後端崩潰或終止時重新啟動它。當 Grafana 關閉時，後端程序隨後會被終止。

### 階段 6. 用戶端載入

Grafana 啟動且 [HTTP API](https://grafana.com/docs/grafana/latest/developers/http_api/) 執行後，您將會收到包含引導資料的伺服器端呈現的索引頁面。此資料包含可用插件的列表以及 Grafana 用於實例化插件的 `module.js` 檔案的 URI。

當您與需要插件的 UI 互動時，Grafana 將_延遲載入_插件的 `module.js` 檔案：

- 對於**面板插件**，當您開啟帶有面板的儀表板（或與任何需要插件的 UI 互動）時，Grafana 會透過一個擷取請求延遲載入必要的插件程式碼。每個插件只會載入一次，但其物件會被多次初始化。

- **資料來源插件**有多種載入方式。例如，如果您在「探索」頁面的下拉式選單中選取資料來源，或者如果您載入使用它的儀表板。

- **應用程式插件**有兩種不同的載入模式：_延遲_和_預先載入_。延遲應用程式插件僅在您直接存取應用程式選單項目時才會載入。預先載入的應用程式插件會與 Grafana 應用程式一起載入，並可以在頁面載入後立即執行程式碼。

:::note

雖然每個插件只會載入一次，但其物件可能會被多次初始化。例如，一個包含 10 個不同面板插件的儀表板將會載入 10 個插件執行個體，每個插件一個。一個包含 10 個相同插件面板的儀表板將會載入該插件一次，該插件將有 10 個執行個體。

:::

## 疑難排解

您可以檢查 [Grafana 伺服器日誌](https://grafana.com/docs/grafana/latest/troubleshooting/#troubleshoot-with-logs)以取得任何非預期的錯誤或與載入插件相關的詳細資訊。此外，您可以透過將[日誌等級變更為偵錯](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/#log)來啟用更多詳細資訊。
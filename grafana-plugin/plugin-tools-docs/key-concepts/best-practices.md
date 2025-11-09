---
id: best-practices
title: 最佳實務
description: Grafana Labs 建議的插件實務索引。
keywords:
  - grafana
  - plugins
  - plugin
  - best practices
sidebar_position: 30
---

# 插件開發的最佳實務

我們彙整了一份我們認為有益的建立和發布高品質 Grafana 插件的實務清單。在建立您自己的插件時，請參考它們以獲得最佳效果。

此清單中是否缺少某些內容？[讓我們知道](https://github.com/grafana/plugin-tools/issues/new)。

## 一般

- **驗證您的資料來源或應用程式插件可以被佈建** - 有關更多資訊，請參閱[佈建](https://grafana.com/docs/grafana/latest/administration/provisioning/#data-sources)。
- **在您的資料來源或應用程式插件中包含預設儀表板** - 有關更多資訊，請參閱[儀表板的捆綁](../key-concepts/plugin-types-usage.md#bundling-of-dashboards)。
- **確保 Grafana 的最低版本是正確的** - 確保您 `plugin.json` 中的 `grafanaDependency` 指向您的插件完全支援的最早版本的 Grafana。
- **不要暴露敏感資訊** - 出於安全原因，請避免暴露敏感資訊，例如機密。確保正確利用日誌等級，避免過度記錄，並且絕不記錄憑證或其他敏感資訊。
- **不允許使用者輸入任意程式碼** - 出於安全原因，請勿讓您的插件使用者輸入潛在的惡意程式碼，以防止任意程式碼執行。
- **避免在您的插件中使用 `console.log`** - 主控台訊息通常用於偵錯目的，因此不適合傳送給用戶端。
- **新增程式碼檢查和自動完成** - 透過在 VS Code 中新增像[這個](https://grafana.com/blog/2021/01/21/6-tips-for-improving-your-grafana-plugin-before-you-publish/#tip-3-add-linting-and-auto-completion-to-your-pluginjson)這樣的程式碼片段來為您的插件取得程式碼檢查，以減少您插件中的錯誤。
- **包含一份寫得好的 README** - 讓使用者更深入地了解如何設定和使用您的插件，但不要讓它成為必讀的。您希望使用者在可能的情況下能夠直觀地了解您的插件，而無需參考文件。
- **允許漸進式學習** - 使用開關或類別隱藏進階選項，讓使用者在準備好時學習進階功能。
- **尋找 Beta 測試人員** - 在您提交插件之前，招募您的目標受眾中的使用者來試用您的插件。取得回饋以幫助您在發布前改進您的插件。
- **保持工具的更新** - 利用 [create-plugin update](/how-to-guides/updating-a-plugin.md) 指令來讓您的插件保持最新。

## 面板插件

- **不要儲存或使用憑證** - 面板插件沒有安全儲存憑證的方法。如果您的插件需要使用憑證，請考慮改用資料來源或應用程式插件，並使用面板插件來顯示資料來源傳回的資訊。

- **考慮建立自訂選項** - 如果預設的面板選項不適合您想提供給使用者的功能，請使用[自訂選項](../how-to-guides/panel-plugins/custom-panel-option-editors)。
- **記錄資料框架結構描述** - 考慮在 README 中[記錄插件的結構描述](https://grafana.com/blog/2021/01/21/6-tips-for-improving-your-grafana-plugin-before-you-publish/#tip-2-document-the-data-frame-schema-for-panel-plugins)（預期欄位、欄位類型、欄位名稱的命名慣例等）。

## 資料來源插件

- **如果您的插件需要儲存憑證，請使用 `secureJsonData` 而非 `jsonData`** - 前者在靜態時會加密，而後者則不會。有關更多資訊，請參閱[安全 JSON 資料](../how-to-guides/data-source-plugins/add-authentication-for-data-source-plugins#store-configuration-in-securejsondata)。
- **實作查詢產生器** - 這對於不熟悉資料來源查詢語言的使用者非常有用。例如，請參閱 [Microsoft SQL Server 的查詢產生器](https://grafana.com/docs/grafana/latest/datasources/mssql/query-editor/#builder-mode)，它有助於為該服務撰寫 SQL 查詢。
- **為您的插件新增健康狀況檢查** - [健康狀況檢查](../key-concepts/backend-plugins/#health-checks)用於驗證資料來源是否正常運作。如何實作取決於插件是否具有後端。對於 `backend` 的情況，只要它擴充了 `@grafana/runtime` 中的 `DataSourceWithBackend` 類別，就不需要修改其前端程式碼。
- **新增[儀表板變數](https://grafana.com/docs/grafana/latest/dashboards/variables/)支援** - 儀表板（或範本）變數可讓使用者建立動態儀表板。在您的插件中新增對變數的支援有兩個方面。首先是允許對您的資料來源進行查詢，並傳回要用作變數的值。其次是在其他查詢中取代現有變數。有關更多資訊，請參閱我們的[文件](../how-to-guides/data-source-plugins/add-support-for-variables#add-support-for-query-variables-to-your-data-source)。在選取「所有值」時要特別注意，因為它可能需要特定的邏輯來連接變數值。
- **新增註釋支援** - 註釋可讓使用者為其儀表板新增上下文資訊，並且可以使用查詢來定義它們。有關更多資訊，請參閱[啟用註釋](../how-to-guides/data-source-plugins/add-support-for-annotation-queries)。
- **實踐良好的前端設計** - 在建立前端元件時，請確保使用 [Grafana 元件](https://developers.grafana.com/ui/latest/index.html?path=/docs/docs-overview-intro--page)作為基礎，並遵循 [Saga 設計系統](https://grafana.com/developers/saga/about/overview)。
- **新增查詢編輯器說明** - 查詢編輯器可能很複雜，為使用者提供說明很有用。有關更多資訊，請參閱[新增查詢編輯器說明](../how-to-guides/data-source-plugins/add-query-editor-help)。
- **略過隱藏或空的查詢** - 這可以避免執行不必要或錯誤的查詢。實作 `DataSourceWithBackend` 的資料來源只需要實作 `filterQuery` 方法。請參閱此[範例](https://github.com/grafana/grafana/blob/fd5f66083c91b9759ae7772f99b80c9342b93290/public/app/plugins/datasource/loki/datasource.ts#L1085)。
- **指定預設查詢** - 預設查詢可以幫助使用者發現如何為插件撰寫查詢。

### 僅限前端的插件

- **僅在前端執行的資料來源通常使用 [Grafana 代理](../how-to-guides/data-source-plugins/add-authentication-for-data-source-plugins#add-a-proxy-route-to-your-plugin)來存取外部服務** - 這是在您的插件中新增對查詢支援的簡單方法，而且不需要 Golang 知識。但是，在某些使用案例中，有必要撰寫插件的後端。有關這些使用案例的更多資訊，請參閱[插件後端](../key-concepts/backend-plugins/#when-to-implement-a-plugin-with-a-backend)。

### 具有後端的插件

- **新增對警示的支援** - 具有後端元件的插件原生支援 [Grafana 警示](https://grafana.com/docs/grafana/latest/alerting/)，但需要啟用此支援。只需在您的 `plugin.json` 檔案中新增 `"alerting": true`。
- **使用 `CallResourceHandler` 介面來處理自訂 HTTP 請求**。有關更多資訊，請參閱[資源處理常式](../key-concepts/backend-plugins/#resources)。例如，在提供查詢產生器時，這很有用。
- **為您的資料來源新增日誌、指標和追蹤。** 讓插件開發人員和 Grafana 操作員更容易診斷和解決問題。在我們的[文件](../how-to-guides/data-source-plugins/add-logs-metrics-traces-for-backend-plugins)中尋找更多資訊。
- **保留快取連線** - 這是一項重要的最佳化。若要深入了解，請參閱我們的[文件](../key-concepts/backend-plugins/#caching-and-connection-pooling)。
- **新增巨集支援** - 巨集類似於變數，但它們通常在後端進行評估，並且可以根據環境資料（例如目前的時間選擇）傳回值。例如，在動態期間評估警示時，這可能很有用。這些通常使用 `$__macroName` 語法定義（例如 `$__timeFilter`）。[插件 SDK `macros` 套件](https://github.com/grafana/grafana-plugin-sdk-go/tree/main/experimental/macros)中提供了一些預定義的巨集。
- **對於 SQL 資料來源，請參閱 [SDK 中的 `sqlutil` 套件](https://pkg.go.dev/github.com/grafana/grafana-plugin-sdk-go/data/sqlutil)** - 它包含多個用於處理 SQL 資料來源的輔助程式，例如資料框架轉換、預設巨集等。此外，請考慮使用 [`sqlds` 套件](https://pkg.go.dev/github.com/grafana/sqlds)，它極大地簡化了 SQL 資料來源的實作。
- **不要使用本地檔案系統** - 不同的插件共用相同的環境。出於安全原因，插件不應依賴本地檔案。
- **不要使用環境變數** - 環境變數也是一種安全風險，應避免使用。對於特定資料來源的設定，請使用 `plugin.json` 檔案中的 `jsonData` 或 `secureJsonData` 欄位。如果插件需要與資料來源共用的設定，請使用 [`plugin` 設定](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/#pluginplugin_id)。
- **插件不應在後端執行任意程式碼** - 同樣，這是一種安全風險，應避免使用。如果您的插件需要執行程式碼，請提供一個允許的指令列表，並在執行前驗證輸入。
- **一般來說，發生的任何錯誤都應以 `error` 等級記錄。**
- **不要使用 `info` 等級：改用 `debug` 等級。**
- **啟用並行查詢執行** - 這可讓您並行執行多個查詢。若要深入了解，請參閱我們的[文件](/tutorials/build-a-data-source-backend-plugin#run-multiple-queries-concurrently)。

## 應用程式插件

- **為您的應用程式指定一個根頁面** - 如果您的應用程式定義了多個頁面，請確保選取一個預設頁面，該頁面將用作您插件的登陸頁面。
- **對您的應用程式進行程式碼分割** - 如果您的應用程式包含多個頁面，請確保使用程式碼分割技術來改善前端載入效能。預設情況下，如果任何前端資產大於 250kb，Webpack 將在建置期間在終端機中顯示警告。有關更多資訊，請參閱以下連結：
  - [SurviveJs 程式碼分割概觀](https://survivejs.com/books/webpack/building/code-splitting)
  - [官方 React lazy 文件](https://react.dev/reference/react/lazy)
- **若要產生動態應用程式，請考慮使用 [Grafana Scenes](https://grafana.com/developers/scenes/)。**
- **考慮貢獻一個 [UI 擴充功能](../key-concepts/ui-extensions)** - UI 擴充功能可以幫助使用者在上下文中發現您的應用程式並繼續給定的工作流程。此外，如果您的應用程式提供了可用於其他應用程式的上下文，請建立一個擴充點以允許這些應用程式這樣做，而無需在您的應用程式中進行進一步的變更。

## UI 擴充功能

權力越大，責任越大。UI 擴充功能是一個強大的工具，但在使用它們時需要記住一些事情。

- **Grafana 核心不應擴充插件** - Grafana 核心絕不應擴充位於插件中的擴充點。我們不希望相依性朝那個方向流動，因為它可能會產生不必要的副作用。

- **從 Grafana 核心公開元件** - 如果您想從 Grafana 核心公開一個元件，您可以使用 UI 擴充功能來公開它，但前提是它是公開可用的，並且透過 `@grafana/ui` 公開它沒有意義。如果您想將其公開給受限數量的插件，您應該考慮使用「公開受限 API」功能。

- **對 UI 擴充功能元件使用延遲載入** - 透過 UI 擴充功能公開元件時，請考慮使用延遲載入來改善初始載入效能並減少套件大小。這對於不一定需要的大型元件尤其有益。有關實作詳細資訊，請參閱[公開延遲載入的元件](../how-to-guides/ui-extensions/expose-a-lazy-loaded-component)。

## 發布插件

- **新增 GitHub 徽章** - 遵循[這些步驟](https://grafana.com/blog/2024/06/06/6-tips-to-improve-your-grafana-plugin-before-you-publish/#tip-4-add-dynamic-badges-to-your-readme)以幫助使用者使用 GitHub 徽章找到您的插件。
- **新增工作流程自動化** - 如果您的插件在 GitHub 上可用，請考慮將用於插件開發的 [GitHub 工作流程](../set-up/set-up-github)新增至您的儲存庫。

## 管理插件相容性

在 Grafana 插件生態系統中，插件與特定 Grafana 版本的相容性由插件 `plugin.json` 檔案的 `grafanaDependency` 屬性中定義的語義版本控制範圍決定。插件作者必須仔細選擇一個版本範圍，以在廣泛的相容性與可管理的維護工作之間取得平衡。

:::note

我們強烈建議您驗證您在 `grafanaDependency` 中定義的範圍是否設定正確。驗證此範圍的最佳方法是使用 [`semver.satisfies`](https://www.npmjs.com/package/semver) 函式，並將 `includePrerelease` 選項設定為 `true`，以檢查特定版本是否符合您定義的範圍。

如果您想針對下一個版本（或目前的 `main`）並且能夠在 Grafana Cloud 中安裝插件，請記得在您的 `grafanaDependency` 中新增一個預發行版本。例如，如果您想針對 `12.1.0`，請將其設定為 `>=12.1.0-0`，因為 Grafana Cloud 版本看起來像 `12.1.0-123123`。

```
/*  您可以在此頁面的 chrome DevTools 主控台中執行的範例 */

const { satisfies } = await import("https://esm.sh/semver");
console.log(satisfies("12.1.0-3212123", ">=12.1.0", { includePrerelease: true }));
> false
```

在 [semver.org](https://semver.org) 上閱讀有關 semver 規範的更多資訊

:::

### 插件開發中的獨特挑戰

Grafana 插件在某種程度上是一種不尋常的軟體，因為在編譯期間使用的許多 npm 相依性在執行階段會被不同版本取代。有關更多詳細資訊，請參閱[Grafana 插件中的前端 NPM 相依性](../key-concepts/manage-npm-dependencies.md)。如果插件依賴於在作用中的 Grafana 環境中不可用的 API，此執行階段替換可能會導致崩潰。因此，管理相容性是插件開發中一個關鍵且複雜的方面。

### 管理插件相容性的最佳實務

為確保插件穩健可靠，請遵循以下最佳實務：

- **採用最新的插件 API：** 使用最新的插件 API 版本可讓開發人員利用新的 Grafana 功能，並確保與平台不斷發展的功能保持一致。它還鼓勵對插件及其相依性進行定期維護和更新。
- **維持單一開發分支：** 旨在為插件支援的整個 Grafana 版本範圍（如 `grafanaDependency` 中所指定）維持單一分支。此方法可減少維護負擔，並與 Grafana 插件目錄中使用的實務保持一致。
- **使用執行階段檢查管理向後相容性：** 若要利用新的 Grafana 功能，同時保持與舊版本的相容性，請實作在執行階段驗證功能可用性的條件式邏輯。有關指導，請參閱[使用執行階段檢查管理向後相容性](../how-to-guides/runtime-checks.md)。
  <!-- 當本文撰寫完成後取消註解 - **使用相容性套件管理向後相容性：** -->
  <!-- 當本文撰寫完成後取消註解 - **透過捆綁 `grafana/ui` 管理向後相容性：** -->
- **使用 Grafana 版本矩陣執行端對端測試：** Grafana 的相依性共用機制可能導致許多與插件相關的問題僅在執行階段出現。透過在插件 `plugin.json` 檔案中定義的 Grafana 版本矩陣上執行端對端 smoke 測試，可以有效地識別這些問題。定期針對舊版支援的版本和 Grafana 的主要開發分支測試插件，可確保向後和向前相容性。此方法可讓插件維護人員驗證目前和即將推出的 Grafana 版本的功能，維持可靠性並為未來的更新做好準備。
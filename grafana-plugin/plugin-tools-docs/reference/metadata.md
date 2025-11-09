---
id: plugin-json
title: 元資料 (plugin.json)
description: Grafana plugin.json 元資料檔案的參考。
keywords:
  - grafana
  - plugins
  - documentation
  - plugin.json
  - API reference
  - API
sidebar_position: 10
---

# 插件元資料 (plugin.json)

所有插件都需要 `plugin.json` 檔案。Grafana 啟動時，它會掃描插件資料夾並掛載每個包含 `plugin.json` 檔案的資料夾，除非該資料夾包含名為 `dist` 的子資料夾。在這種情況下，Grafana 會改為掛載 `dist` 資料夾。

**屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| **id** | `string` | 插件的唯一名稱。如果插件在 grafana.com 上發布，則插件 `id` 必須遵循命名慣例。<br/>模式：`^[0-9a-z]+\-([0-9a-z]+\-)?(app\|panel\|datasource)$`<br/> | ✅ |
| **type** | `string` | 插件類型。<br/>可能的值為：`"app"`、`"datasource"`、`"panel"`、`"renderer"`<br/> | ✅ |
| [**info**](#info) | `object` | 插件的元資料。某些欄位用於 Grafana 中的插件頁面，如果插件已發布，則其他欄位用於 grafana.com。<br/> | ✅ |
| **name** | `string` | 在 UI 中向使用者顯示的插件的人類可讀名稱。<br/> | ✅ |
| [**dependencies**](#dependencies) | `object` | 與 Grafana 和其他插件相關的相依性資訊。<br/> | ✅ |
| **$schema** | `string` | plugin.json 檔案的結構描述定義。主要用於結構描述驗證。<br/> | |
| **alerting** | `boolean` | 對於資料來源插件，如果插件支援警示。需要將 `backend` 設定為 `true`。<br/> | |
| **annotations** | `boolean` | 對於資料來源插件，如果插件支援註釋查詢。<br/> | |
| **autoEnabled** | `boolean` | 對於應在所有組織中啟用並固定到導覽列的應用程式插件，請設定為 true。<br/> | |
| **backend** | `boolean` | 如果插件具有後端元件。<br/> | |
| **buildMode** | `string` | 插件的建置模式。此欄位在建置時自動設定，因此不應手動提供。<br/> | |
| **builtIn** | `boolean` | [僅限內部] 表示插件是否作為 Grafana 的一部分開發和發布。也稱為「核心插件」。<br/> | |
| **category** | `string` | 用於「新增資料來源」頁面的插件類別。<br/>可能的值為：`"tsdb"`、`"logging"`、`"cloud"`、`"tracing"`、`"profiling"`、`"sql"`、`"enterprise"`、`"iot"`、`"other"`<br/> | |
| [**enterpriseFeatures**](#enterprisefeatures) | `object` | Grafana Enterprise 特定功能<br/> | |
| **executable** | `string` | 後端元件可執行檔檔案名稱的第一部分。可以為不同的作業系統和架構建置多個可執行檔。Grafana 將會檢查名為 `<executable>_<$GOOS>_<lower case $GOARCH><.exe for Windows>` 的可執行檔，例如 `plugin_linux_amd64`。$GOOS 和 $GOARCH 的組合可以在這裡找到：https://golang.org/doc/install/source#environment。<br/> | |
| **hideFromList** | `boolean` | [僅限內部] 將插件從 Grafana 的 UI 列表中排除。僅允許 `builtIn` 插件。<br/> | |
| [**includes**](#includes) | `object[]` | 要包含在插件中的資源。<br/> | |
| **logs** | `boolean` | 對於資料來源插件，如果插件支援日誌。它可用於僅篩選日誌功能。<br/> | |
| **metrics** | `boolean` | 對於資料來源插件，如果插件支援指標查詢。用於在面板編輯器中啟用插件。<br/> | |
| **multiValueFilterOperators** | `boolean` | 對於資料來源插件，如果插件支援 adhoc 篩選器中的多值運算子。<br/> | |
| **pascalName** | `string` | [僅限內部] 插件的 PascalCase 名稱。用於建立機器友善的識別碼，通常在程式碼產生中使用。如果未提供，則預設為名稱，但會轉換為標題大小寫並進行清理（僅允許字母字元）。<br/>模式：`^([A-Z][a-zA-Z]{1,62})$`<br/> | |
| **preload** | `boolean` | 在啟動時初始化插件。預設情況下，插件在首次使用時初始化，但當 preload 設定為 true 時，插件會在 Grafana Web 應用程式首次載入時載入。僅適用於應用程式插件。設定為 `true` 時，請實作[前端程式碼分割](https://grafana.com/developers/plugin-tools/key-concepts/best-practices#app-plugins)以將效能影響降至最低。<br/> | |
| [**queryOptions**](#queryoptions) | `object` | 對於資料來源插件。插件的查詢編輯器中有一個查詢選項區段，可以視需要開啟這些選項。<br/> | |
| [**routes**](#routes) | `object[]` | 對於資料來源插件。用於插件驗證和將標頭新增至插件發出的 HTTP 請求的代理路由。有關更多資訊，請參閱[資料來源插件的驗證](https://grafana.com/developers/plugin-tools/how-to-guides/data-source-plugins/add-authentication-for-data-source-plugins)。<br/> | |
| **skipDataQuery** | `boolean` | 對於面板插件。隱藏查詢編輯器。<br/> | |
| **state** | `string` | 將插件標示為預發行版本。<br/>可能的值為：`"alpha"`、`"beta"`<br/> | |
| **streaming** | `boolean` | 對於資料來源插件，如果插件支援串流。用於在「探索」中啟動即時串流。<br/> | |
| **tracing** | `boolean` | 對於資料來源插件，如果插件支援追蹤。例如，用於將日誌（例如 Loki 日誌）與追蹤插件連結。<br/> | |
| [**iam**](#iam) | `object` | Grafana 會讀取「身分與存取管理」區段並為插件初始化一個服務帳戶，該服務帳戶具有一組量身打造的 [Grafana RBAC 權限](https://grafana.com/docs/grafana/latest/administration/roles-and-permissions/access-control/custom-role-actions-scopes/#rbac-permissions-actions-and-scopes)。Grafana 將使用 `GF_PLUGIN_APP_CLIENT_SECRET` 環境變數與插件後端共用服務帳戶的 bearer 權杖。需要 Grafana 10.3.0 或更新版本。目前，這位於 `externalServiceAccounts` 功能旗標之後。若要試用此功能，請遵循此[指南](https://grafana.com/developers/plugin-tools/how-to-guides/app-plugins/implement-rbac-in-app-plugins)。<br/> | |
| [**roles**](#roles) | `object[]` | 由插件定義的 RBAC 角色及其對基本角色（`Viewer`、`Editor`、`Admin`、`Grafana Admin`）的預設指派。需要 Grafana 9.4.0 或更新版本。<br/> | |
| [**extensions**](#extensions) | `object` | 插件擴充功能是擴充核心 Grafana 或其他插件的 UI 的一種方式。<br/> | |
| [**languages**](#languages) | `string[]` | 插件支援的語言列表。每個條目都應為 `language-COUNTRY` 格式的地區設定識別碼（例如 `en-US`、`fr-FR`、`es-ES`）。<br/> | |

<a name="info"></a>

## info

插件的元資料。某些欄位用於 Grafana 中的插件頁面，如果插件已發布，則其他欄位用於 grafana.com。

**屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| [**author**](#infoauthor) | `object` | 有關插件作者的資訊。<br/> | |
| [**build**](#infobuild) | `object` | 建置資訊<br/> | |
| **description** | `string` | 插件的描述。用於 Grafana 中的插件頁面和 grafana.com 上的搜尋。<br/> | |
| [**keywords**](#infokeywords) | `string[]` | 插件關鍵字陣列。用於 grafana.com 上的搜尋。<br/> | ✅ |
| [**links**](#infolinks) | `object[]` | 要在此插件的專案頁面上顯示的連結物件陣列，格式為 `{name: 'foo', url: 'http://example.com'}`<br/> | |
| [**logos**](#infologos) | `object` | 用作插件圖示的 SVG 影像。<br/> | ✅ |
| [**screenshots**](#infoscreenshots) | `object[]` | 螢幕截圖物件陣列，格式為 `{name: 'bar', path: 'img/screenshot.png'}`<br/> | |
| **updated** | `string` | 此插件的建置日期。<br/>模式：`^(\d{4}-\d{2}-\d{2}\|\%TODAY\%)$`<br/> | ✅ |
| **version** | `string` | 此提交的 [SemVer](https://semver.org/) 版本，例如 `6.7.1`。<br/>模式：`^(0\|[1-9]\d*)\.(0\|[1-9]\d*)\.(0\|[1-9]\d*)\|(\%VERSION\%)$`<br/> | ✅ |

<a name="infoauthor"></a>

### info.author

有關插件作者的資訊。

**屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| **name** | `string` | 作者姓名。<br/> | |
| **email** | `string` | 作者姓名。<br/>格式：`"email"`<br/> | |
| **url** | `string` | 作者網站連結。<br/>格式：`"uri"`<br/> | |

<a name="infobuild"></a>

### info.build

建置資訊

**屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| **time** | `number` | 插件建置時間，為 Unix 時間戳記。<br/> | |
| **repo** | `string` | | |
| **branch** | `string` | 插件建置時的 Git 分支。<br/> | |
| **hash** | `string` | 插件建置時的提交的 Git 雜湊值<br/> | |
| **number** | `number` | | |
| **pr** | `number` | 插件建置時的 GitHub 拉取請求<br/> | |
| **build** | `number` | 用於建置此插件的建置工作編號。<br/> | |

<a name="infokeywords"></a>

### info.keywords[]

插件關鍵字陣列。用於 grafana.com 上的搜尋。

**項目**

**項目類型：** `string`
**最少項目數：** 1
<a name="infolinks"></a>

### info.links[]

要在此插件的專案頁面上顯示的連結物件陣列，格式為 `{name: 'foo', url: 'http://example.com'}`

**項目**

**項目屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| **name** | `string` | 連結的顯示名稱。具有預定義行為的特殊名稱：<br/>• `documentation` - 在插件詳細資訊頁面上設定「文件」連結<br/>• `repository` - 用於確定並連結至插件的儲存庫<br/>• `license` - 在插件詳細資訊頁面上設定「授權」連結<br/>• `raise issue` - 在插件詳細資訊頁面上設定「提出問題」連結<br/>• `sponsorship` - 在插件詳細資訊頁面上設定「贊助此開發人員」連結，以引導使用者如何支援您的工作<br/> | |
| **url** | `string` | 用於此特定連結的 URL 值。<br/>格式：`"uri"`<br/> | |

<a name="infologos"></a>

### info.logos

用作插件圖示的 SVG 影像。

**屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| **small** | `string` | 指向插件標誌的「小」版本的連結，該連結必須是 SVG 影像。「大」和「小」標誌可以是相同的影像。<br/> | ✅ |
| **large** | `string` | 指向插件標誌的「大」版本的連結，該連結必須是 SVG 影像。「大」和「小」標誌可以是相同的影像。<br/> | ✅ |

<a name="infoscreenshots"></a>

### info.screenshots[]

螢幕截圖物件陣列，格式為 `{name: 'bar', path: 'img/screenshot.png'}`

**項目**

**項目屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| **name** | `string` | | |
| **path** | `string` | | |

<a name="dependencies"></a>

## dependencies

與 Grafana 和其他插件相關的相依性資訊。

**屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| **grafanaVersion** | `string` | (已棄用) 此插件所需的 Grafana 版本，例如 `6.x.x 7.x.x` 表示插件需要 Grafana v6.x.x 或 v7.x.x。<br/>模式：`^([0-9]+)(\.[0-9x]+)?(\.[0-9x])?$`<br/> | |
| **grafanaDependency** | `string` | 此插件所需的 Grafana 版本。使用 https://github.com/npm/node-semver 進行驗證。<br/>模式：`^(<=\|>=\|<\|>\|=\|~\|\^)?([0-9]+)(\.[0-9x\*]+)?(\.[0-9x\*]+)?(-[0-9A-Za-z-.]+)?(\s(<=\|>=\|<\|=>)?([0-9]+)(\.[0-9x\*]+)?(\.[0-9x\*]+)?(-[0-9A-Za-z-.]+)?)?$`<br/> | ✅ |
| [**plugins**](#dependenciesplugins) | `object[]` | 此插件所依賴的必要插件陣列。此列表中只需指定非核心（即外部插件）插件。<br/> | |
| [**extensions**](#dependenciesextensions) | `object` | 此插件所依賴的插件擴充功能。<br/> | |

<a name="dependenciesplugins"></a>

### dependencies.plugins[]

此插件所依賴的必要插件陣列。此列表中只需指定非核心（即外部插件）插件。

**項目**

插件相依性。用於在 Grafana UI 中顯示有關插件相依性的資訊。

**項目屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| **id** | `string` | 模式：`^[0-9a-z]+\-([0-9a-z]+\-)?(app\|panel\|datasource)$`<br/> | ✅ |
| **type** | `string` | 可能的值為：`"app"`、`"datasource"`、`"panel"`<br/> | ✅ |
| **name** | `string` | | ✅ |

<a name="dependenciesextensions"></a>

### dependencies.extensions

此插件所依賴的插件擴充功能。

**屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| [**exposedComponents**](#dependenciesextensionsexposedcomponents) | `string[]` | 此插件所依賴的公開元件 ID 陣列。<br/> | |

<a name="dependenciesextensionsexposedcomponents"></a>

#### dependencies.extensions.exposedComponents[]

此插件所依賴的公開元件 ID 陣列。

**項目**

**項目類型：** `string`
<a name="enterprisefeatures"></a>

## enterpriseFeatures

Grafana Enterprise 特定功能

**屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| **healthDiagnosticsErrors** | `boolean` | 啟用/停用健康診斷錯誤。需要 Grafana >=7.5.5。<br/>預設值：`false`<br/> | |

<a name="includes"></a>

## includes[]

要包含在插件中的資源。

**項目**

**項目屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| **uid** | `string` | 包含資源的唯一識別碼<br/> | |
| **type** | `string` | 可能的值為：`"dashboard"`、`"page"`、`"panel"`、`"datasource"`<br/> | |
| **name** | `string` | | |
| **component** | `string` | (舊版) 用於頁面的 Angular 元件。<br/> | |
| **role** | `string` | 使用者必須具有的最低角色才能在導覽選單中看到此頁面。<br/>可能的值為：`"Admin"`、`"Editor"`、`"Viewer"`<br/> | |
| **action** | `string` | 使用者必須具有的 RBAC 動作才能在導覽選單中看到此頁面。**警告**：除非動作針對插件，否則只會驗證動作，而不會驗證其適用對象。<br/> | |
| **path** | `string` | 用於應用程式插件。<br/> | |
| **addToNav** | `boolean` | 將包含項新增至導覽選單。<br/> | |
| **defaultNav** | `boolean` | 當使用者按一下側邊選單中的圖示時的頁面或儀表板。<br/> | |
| **icon** | `string` | 在側邊選單中使用的圖示。有關可用圖示的資訊，請參閱[圖示概觀](https://developers.grafana.com/ui/latest/index.html?path=/story/docs-overview-icon--icons-overview)。<br/> | |

<a name="queryoptions"></a>

## queryOptions

對於資料來源插件。插件的查詢編輯器中有一個查詢選項區段，可以視需要開啟這些選項。

**屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| **maxDataPoints** | `boolean` | 對於資料來源插件。如果應在查詢編輯器的查詢選項區段中顯示 `max data points` 選項。<br/> | |
| **minInterval** | `boolean` | 對於資料來源插件。如果應在查詢編輯器的查詢選項區段中顯示 `min interval` 選項。<br/> | |
| **cacheTimeout** | `boolean` | 對於資料來源插件。如果應在查詢編輯器的查詢選項區段中顯示 `cache timeout` 選項。<br/> | |

<a name="routes"></a>

## routes[]

對於資料來源插件。用於插件驗證和將標頭新增至插件發出的 HTTP 請求的代理路由。有關更多資訊，請參閱[資料來源插件的驗證](https://grafana.com/developers/plugin-tools/how-to-guides/data-source-plugins/add-authentication-for-data-source-plugins)。

**項目**

**項目屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| **path** | `string` | 對於資料來源插件。代理呼叫時由路由 URL 欄位取代的路由路徑。<br/> | |
| **method** | `string` | 對於資料來源插件。路由方法符合 HTTP 動詞，例如 `GET` 或 `POST`。可以提供以逗號分隔的多個方法。<br/> | |
| **url** | `string` | 對於資料來源插件。請求代理至的路由 URL。<br/> | |
| **reqSignedIn** | `boolean` | | |
| **reqRole** | `string` | | |
| **reqAction** | `string` | 使用者必須具有的 RBAC 動作才能使用此路由。**警告**：除非動作針對插件（或巢狀資料來源插件），否則只會驗證動作，而不會驗證其適用對象。<br/> | |
| [**headers**](#routesheaders) | `array` | 對於資料來源插件。路由標頭會將 HTTP 標頭新增至代理的請求。<br/> | |
| [**body**](#routesbody) | `object` | 對於資料來源插件。路由標頭會將主體內容和長度設定為代理的請求。<br/> | |
| [**tokenAuth**](#routestokenauth) | `object` | 對於資料來源插件。與 OAuth API 搭配使用的權杖驗證區段。<br/> | |
| [**jwtTokenAuth**](#routesjwttokenauth) | `object` | 對於資料來源插件。與 JWT OAuth API 搭配使用的權杖驗證區段。<br/> | |
| [**urlParams**](#routesurlparams) | `object[]` | 將 URL 參數新增至代理路由<br/> | |

<a name="routesheaders"></a>

### routes[].headers[]

對於資料來源插件。路由標頭會將 HTTP 標頭新增至代理的請求。

<a name="routesbody"></a>

### routes[].body

對於資料來源插件。路由標頭會將主體內容和長度設定為代理的請求。

<a name="routestokenauth"></a>

### routes[].tokenAuth

對於資料來源插件。與 OAuth API 搭配使用的權杖驗證區段。

**屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| **url** | `string` | 用於擷取驗證權杖的 URL。<br/> | |
| [**scopes**](#routestokenauthscopes) | `string[]` | 您的應用程式應被授予存取權限的範圍列表。<br/> | |
| [**params**](#routestokenauthparams) | `object` | 權杖驗證請求的參數。<br/> | |

<a name="routestokenauthscopes"></a>

#### routes[].tokenAuth.scopes[]

您的應用程式應被授予存取權限的範圍列表。

**項目**

**項目類型：** `string`
<a name="routestokenauthparams"></a>

#### routes[].tokenAuth.params

權杖驗證請求的參數。

**屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| **grant_type** | `string` | OAuth 授權類型<br/> | |
| **client_id** | `string` | OAuth 用戶端 ID<br/> | |
| **client_secret** | `string` | OAuth 用戶端密碼。通常透過解密 SecureJson blob 中的密碼來填入。<br/> | |
| **resource** | `string` | OAuth 資源<br/> | |

<a name="routesjwttokenauth"></a>

### routes[].jwtTokenAuth

對於資料來源插件。與 JWT OAuth API 搭配使用的權杖驗證區段。

**屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| **url** | `string` | 用於擷取 JWT 權杖的 URL。<br/>格式：`"uri"`<br/> | |
| [**scopes**](#routesjwttokenauthscopes) | `string[]` | 您的應用程式應被授予存取權限的範圍列表。<br/> | |
| [**params**](#routesjwttokenauthparams) | `object` | JWT 權杖驗證請求的參數。<br/> | |

<a name="routesjwttokenauthscopes"></a>

#### routes[].jwtTokenAuth.scopes[]

您的應用程式應被授予存取權限的範圍列表。

**項目**

**項目類型：** `string`
<a name="routesjwttokenauthparams"></a>

#### routes[].jwtTokenAuth.params

JWT 權杖驗證請求的參數。

**屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| **token_uri** | `string` | | |
| **client_email** | `string` | | |
| **private_key** | `string` | | |

<a name="routesurlparams"></a>

### routes[].urlParams[]

將 URL 參數新增至代理路由

**項目**

**項目屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| **name** | `string` | URL 參數的名稱<br/> | |
| **content** | `string` | URL 參數的值<br/> | |

<a name="iam"></a>

## iam

Grafana 會讀取「身分與存取管理」區段並為插件初始化一個服務帳戶，該服務帳戶具有一組量身打造的 [Grafana RBAC 權限](https://grafana.com/docs/grafana/latest/administration/roles-and-permissions/access-control/custom-role-actions-scopes/#rbac-permissions-actions-and-scopes)。Grafana 將使用 `GF_PLUGIN_APP_CLIENT_SECRET` 環境變數與插件後端共用服務帳戶的 bearer 權杖。需要 Grafana 10.3.0 或更新版本。目前，這位於 `externalServiceAccounts` 功能旗標之後。若要試用此功能，請遵循此[指南](https://grafana.com/developers/plugin-tools/how-to-guides/app-plugins/implement-rbac-in-app-plugins)。

**屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| [**permissions**](#iampermissions) | `object[]` | 權限是插件需要其關聯的服務帳戶具有才能查詢 Grafana 的權限。<br/> | |

<a name="iampermissions"></a>

### iam.permissions[]

權限是插件需要其關聯的服務帳戶具有才能查詢 Grafana 的權限。

**項目**

**項目屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| **action** | `string` | 動作，例如：`teams:read`。<br/> | |
| **scope** | `string` | 插件需要存取的範圍，例如：`teams:*`。<br/> | |

<a name="roles"></a>

## roles[]

由插件定義的 RBAC 角色及其對基本角色（`Viewer`、`Editor`、`Admin`、`Grafana Admin`）的預設指派。需要 Grafana 9.4.0 或更新版本。

**項目**

**項目屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| [**role**](#rolesrole) | `object` | 角色會將您插件的相關 RBAC 權限分組（例如：`Projects Admin` 會將建立、讀取、寫入和刪除專案的權限分組）。您角色中定義的 RBAC 動作必須以您的插件 `id` 開頭（例如：`grafana-test-app.projects:read`）。<br/> | |
| [**grants**](#rolesgrants) | `string[]` | 對 Grafana 基本角色（`Viewer`、`Editor`、`Admin`、`Grafana Admin`）的角色預設指派。<br/> | |

<a name="rolesrole"></a>

### roles[].role

角色會將您插件的相關 RBAC 權限分組（例如：`Projects Admin` 會將建立、讀取、寫入和刪除專案的權限分組）。您角色中定義的 RBAC 動作必須以您的插件 `id` 開頭（例如：`grafana-test-app.projects:read`）。

**屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| **name** | `string` | 角色的顯示名稱。<br/> | |
| **description** | `string` | 描述角色的目的。<br/> | |
| [**permissions**](#rolesrolepermissions) | `object[]` | 插件的 RBAC 權限。<br/> | |

<a name="rolesrolepermissions"></a>

#### roles[].role.permissions[]

插件的 RBAC 權限。

**項目**

**項目屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| **action** | `string` | | |
| **scope** | `string` | | |

<a name="rolesgrants"></a>

### roles[].grants[]

對 Grafana 基本角色（`Viewer`、`Editor`、`Admin`、`Grafana Admin`）的角色預設指派。

**項目**

**項目類型：** `string`
<a name="extensions"></a>

## extensions

插件擴充功能是擴充核心 Grafana 或其他插件的 UI 的一種方式。

**屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| [**addedComponents**](#extensionsaddedcomponents) | `object[]` | 此列表必須包含您的插件使用 [`.addComponent()`](https://grafana.com/developers/plugin-tools/reference/ui-extensions-reference/ui-extensions#addcomponent) 註冊到其他擴充點的所有元件擴充功能。**未在此處列出的元件將無法運作。**<br/> | |
| [**addedLinks**](#extensionsaddedlinks) | `object[]` | 此列表必須包含您的插件使用 [`.addLink()`](https://grafana.com/developers/plugin-tools/reference/ui-extensions-reference/ui-extensions#addlink) 註冊到其他擴充點的所有連結擴充功能。**未在此處列出的連結將無法運作。**<br/> | |
| [**exposedComponents**](#extensionsexposedcomponents) | `object[]` | 此列表必須包含您的插件使用 [`.exposeComponent()`](https://grafana.com/developers/plugin-tools/reference/ui-extensions-reference/ui-extensions#exposecomponent) 公開的所有元件。**未在此處列出的元件將無法運作。**<br/> | |
| [**extensionPoints**](#extensionsextensionpoints) | `object[]` | 此列表必須包含您的插件使用 [`usePluginLinks()`](https://grafana.com/developers/plugin-tools/reference/ui-extensions-reference/ui-extensions#usepluginlinks) 或 [`usePluginComponents()`](https://grafana.com/developers/plugin-tools/reference/ui-extensions-reference/ui-extensions#useplugincomponents) 定義的所有擴充點。**未在此處列出的擴充點將無法運作。**<br/> | |

<a name="extensionsaddedcomponents"></a>

### extensions.addedComponents[]

此列表必須包含您的插件使用 [`.addComponent()`](https://grafana.com/developers/plugin-tools/reference/ui-extensions-reference/ui-extensions#addcomponent) 註冊到其他擴充點的所有元件擴充功能。**未在此處列出的元件將無法運作。**

**項目**

**項目屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| [**targets**](#extensionsaddedcomponentstargets) | `string[]` | 您的插件將擴充功能註冊到的擴充點 ID，例如 `["grafana/user/profile/tab"]`<br/> | ✅ |
| **title** | `string` | 您的元件擴充功能的標題。<br/>最小長度：`10`<br/> | ✅ |
| **description** | `string` | 有關您的元件擴充功能的其他資訊。<br/> | |

<a name="extensionsaddedcomponentstargets"></a>

#### extensions.addedComponents[].targets[]

您的插件將擴充功能註冊到的擴充點 ID，例如 `["grafana/user/profile/tab"]`

**項目**

**項目類型：** `string`
<a name="extensionsaddedlinks"></a>

### extensions.addedLinks[]

此列表必須包含您的插件使用 [`.addLink()`](https://grafana.com/developers/plugin-tools/reference/ui-extensions-reference/ui-extensions#addlink) 註冊到其他擴充點的所有連結擴充功能。**未在此處列出的連結將無法運作。**

**項目**

**項目屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| [**targets**](#extensionsaddedlinkstargets) | `string[]` | 您的插件將擴充功能註冊到的擴充點 ID，例如 `["grafana/dashboard/panel/menu"]`<br/> | ✅ |
| **title** | `string` | 您的連結擴充功能的標題。<br/>最小長度：`10`<br/> | ✅ |
| **description** | `string` | 有關您的連結擴充功能的其他資訊。<br/> | |

<a name="extensionsaddedlinkstargets"></a>

#### extensions.addedLinks[].targets[]

您的插件將擴充功能註冊到的擴充點 ID，例如 `["grafana/dashboard/panel/menu"]`

**項目**

**項目類型：** `string`
<a name="extensionsexposedcomponents"></a>

### extensions.exposedComponents[]

此列表必須包含您的插件使用 [`.exposeComponent()`](https://grafana.com/developers/plugin-tools/reference/ui-extensions-reference/ui-extensions#exposecomponent) 公開的所有元件。**未在此處列出的元件將無法運作。**

**項目**

**項目屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| **id** | `string` | 您公開元件的唯一識別碼。這用於在其他插件中參考該元件。它必須採用以下格式：`{PLUGIN_ID}/name-of-component/v1`。建議新增版本後綴以防止未來的破壞性變更。例如：`myorg-extensions-app/exposed-component/v1`。<br/>模式：`^[0-9a-z]+-([0-9a-z]+-)?(app\|panel\|datasource)\/[a-zA-Z0-9_-]+\/v[0-9_.-]+$`<br/> | ✅ |
| **title** | `string` | 您公開元件的標題。<br/> | |
| **description** | `string` | 有關您公開元件的其他資訊。<br/> | |

<a name="extensionsextensionpoints"></a>

### extensions.extensionPoints[]

此列表必須包含您的插件使用 [`usePluginLinks()`](https://grafana.com/developers/plugin-tools/reference/ui-extensions-reference/ui-extensions#usepluginlinks) 或 [`usePluginComponents()`](https://grafana.com/developers/plugin-tools/reference/ui-extensions-reference/ui-extensions#useplugincomponents) 定義的所有擴充點。**未在此處列出的擴充點將無法運作。**

**項目**

**項目屬性**

| 名稱 | 類型 | 說明 | 必要 |
| --- | --- | --- | :---: |
| **id** | `string` | 您擴充點的唯一識別碼。這用於在其他插件中參考該擴充點。它必須採用以下格式：`{PLUGIN_ID}/name-of-my-extension-point/v1`。建議新增版本後綴以防止未來的破壞性變更。例如：`myorg-extensions-app/extension-point/v1`。<br/>模式：`^[0-9a-z]+-([0-9a-z]+-)?(app\|panel\|datasource)\/[a-zA-Z0-9_-]+\/v[0-9_.-]+$`<br/> | ✅ |
| **title** | `string` | 您擴充點的標題。<br/> | |
| **description** | `string` | 有關您擴充點的其他資訊。<br/> | |

<a name="languages"></a>

## languages[]

插件支援的語言列表。每個條目都應為 `language-COUNTRY` 格式的地區設定識別碼（例如 `en-US`、`fr-FR`、`es-ES`）。

**項目**

**項目類型：** `string`
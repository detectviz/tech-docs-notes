# Grafana MCP 伺服器

[![Unit Tests](https://github.com/grafana/mcp-grafana/actions/workflows/unit.yml/badge.svg)](https://github.com/grafana/mcp-grafana/actions/workflows/unit.yml)
[![Integration Tests](https://github.com/grafana/mcp-grafana/actions/workflows/integration.yml/badge.svg)](https://github.com/grafana/mcp-grafana/actions/workflows/integration.yml)
[![E2E Tests](https://github.com/grafana/mcp-grafana/actions/workflows/e2e.yml/badge.svg)](https://github.com/grafana/mcp-grafana/actions/workflows/e2e.yml)
[![Go Reference](https://pkg.go.dev/badge/github.com/grafana/mcp-grafana.svg)](https://pkg.go.dev/github.com/grafana/mcp-grafana)
[![MCP Catalog](https://archestra.ai/mcp-catalog/api/badge/quality/grafana/mcp-grafana)](https://archestra.ai/mcp-catalog/grafana__mcp-grafana)

一個用於 Grafana 的 [Model Context Protocol][mcp] (MCP) 伺服器。

這提供了對您的 Grafana 執行個體及周邊生態系統的存取。

## 需求

- **需要 Grafana 9.0 或更新版本**才能獲得完整功能。某些功能，特別是與資料來源相關的操作，可能無法在較早版本中正常運作，因為缺少 API 端點。

## 功能

_以下是 MCP 伺服器目前可用的功能。此列表僅供參考，不代表未來功能的路線圖或承諾。_

### 儀表板

- **搜尋儀表板：** 按標題或其他元資料尋找儀表板
- **依 UID 取得儀表板：** 使用其唯一識別碼擷取完整的儀表板詳細資料。_警告：大型儀表板可能會消耗大量的上下文視窗空間。_
- **取得儀表板摘要：** 取得儀表板的精簡概觀，包括標題、面板數量、面板類型、變數和元資料，而無需完整的 JSON，以最小化上下文視窗的使用
- **取得儀表板屬性：** 使用 JSONPath 運算式（例如 `$.title`, `$.panels[*].title`）提取儀表板的特定部分，以僅擷取所需資料並減少上下文視窗的消耗
- **更新或建立儀表板：** 修改現有儀表板或建立新儀表板。_警告：需要完整的儀表板 JSON，這可能會消耗大量的上下文視窗空間。_
- **修補儀表板：** 對儀表板應用特定變更，而無需完整的 JSON，從而顯著減少針對性修改的上下文視窗使用量
- **取得面板查詢和資料來源資訊：** 從儀表板中的每個面板取得標題、查詢字串和資料來源資訊（包括 UID 和類型，如果可用）

#### 上下文視窗管理

儀表板工具現在包含多種策略來有效管理上下文視窗的使用（[問題 #101](https://github.com/grafana/mcp-grafana/issues/101)）：

- **使用 `get_dashboard_summary`** 進行儀表板概觀和規劃修改
- **當您只需要特定的儀表板部分時，請使用 `get_dashboard_property`** 搭配 JSONPath
- **避免使用 `get_dashboard_by_uid`**，除非您特別需要完整的儀表板 JSON

### 資料來源

- **列出和擷取資料來源資訊：** 檢視所有已設定的資料來源並擷取每個資料來源的詳細資訊。
  - _支援的資料來源類型：Prometheus, Loki._

### Prometheus 查詢

- **查詢 Prometheus：** 針對 Prometheus 資料來源執行 PromQL 查詢（支援即時和範圍指標查詢）。
- **查詢 Prometheus 元資料：** 從 Prometheus 資料來源擷取指標元資料、指標名稱、標籤名稱和標籤值。

### Loki 查詢

- **查詢 Loki 日誌和指標：** 使用 LogQL 針對 Loki 資料來源執行日誌查詢和指標查詢。
- **查詢 Loki 元資料：** 從 Loki 資料來源擷取標籤名稱、標籤值和串流統計資料。

### 事件

- **搜尋、建立和更新事件：** 在 Grafana Incident 中管理事件，包括搜尋、建立和向事件新增活動。

### Sift 調查

- **列出 Sift 調查：** 擷取 Sift 調查列表，支援 limit 參數。
- **取得 Sift 調查：** 依其 UUID 擷取特定 Sift 調查的詳細資料。
- **取得 Sift 分析：** 從 Sift 調查中擷取特定分析。
- **在日誌中尋找錯誤模式：** 使用 Sift 在 Loki 日誌中偵測升高的錯誤模式。
- **尋找緩慢請求：** 使用 Sift (Tempo) 偵測緩慢請求。

### 警報

- **列出和擷取警報規則資訊：** 在 Grafana 中檢視警報規則及其狀態（觸發/正常/錯誤等）。
- **列出聯絡點：** 在 Grafana 中檢視已設定的通知聯絡點。

### Grafana OnCall

- **列出和管理排程：** 在 Grafana OnCall 中檢視和管理待命排程。
- **取得輪班詳細資料：** 擷取特定待命輪班的詳細資訊。
- **取得目前待命使用者：** 查看目前有哪些使用者正在排程中待命。
- **列出團隊和使用者：** 檢視所有 OnCall 團隊和使用者。
- **列出警報群組：** 依各種條件（包括狀態、整合、標籤和時間範圍）從 Grafana OnCall 檢視和篩選警報群組。
- **取得警報群組詳細資料：** 依其 ID 擷取特定警報群組的詳細資訊。

### 管理

- **列出團隊：** 在 Grafana 中檢視所有已設定的團隊。
- **列出使用者：** 在 Grafana 中檢視組織中的所有使用者。

### 導覽

- **產生深層連結：** 為 Grafana 資源建立準確的深層連結 URL，而不是依賴 LLM 的 URL 猜測。
  - **儀表板連結：** 使用其 UID 產生儀表板的直接連結（例如 `http://localhost:3000/d/dashboard-uid`）
  - **面板連結：** 使用 viewPanel 參數建立儀表板內特定面板的連結（例如 `http://localhost:3000/d/dashboard-uid?viewPanel=5`）
  - **探索連結：** 產生 Grafana Explore 的連結，並預先設定資料來源（例如 `http://localhost:3000/explore?left={"datasource":"prometheus-uid"}`）
  - **時間範圍支援：** 將時間範圍參數新增至連結（`from=now-1h&to=now`）
  - **自訂參數：** 包含其他查詢參數，如儀表板變數或重新整理間隔

工具列表是可設定的，因此您可以選擇要提供給 MCP 用戶端的工具。
如果您不使用某些功能，或者不想佔用太多上下文視窗，這非常有用。
若要停用某一類別的工具，請在啟動伺服器時使用 `--disable-<category>` 旗標。例如，若要停用
OnCall 工具，請使用 `--disable-oncall`，或若要停用導覽深層連結產生，請使用 `--disable-navigation`。

#### RBAC 權限

每個工具都需要特定的 RBAC 權限才能正常運作。為 MCP 伺服器建立服務帳戶時，請確保它具有根據您計劃使用的工具所需的權限。列出的權限是所需的最低操作 - 您可能還需要適當的範圍（例如 `datasources:*`, `dashboards:*`, `folders:*`），具體取決於您的使用案例。

**注意：** Grafana Incident 和 Sift 工具使用基本的 Grafana 角色，而不是細微性的 RBAC 權限：
- **Viewer 角色：** 唯讀操作所需（列出事件、取得調查）
- **Editor 角色：** 寫入操作所需（建立事件、修改調查）

有關 Grafana RBAC 的更多資訊，請參閱[官方文件](https://grafana.com/docs/grafana/latest/administration/roles-and-permissions/access-control/)。

#### RBAC 範圍

範圍定義了權限適用的特定資源。每個操作都需要適當的權限和範圍組合。

**常見的範圍模式：**

- **廣泛存取：** 使用 `*` 萬用字元進行組織範圍的存取

  - `datasources:*` - 存取所有資料來源
  - `dashboards:*` - 存取所有儀表板
  - `folders:*` - 存取所有資料夾
  - `teams:*` - 存取所有團隊

- **有限存取：** 使用特定的 UID 或 ID 來限制對個別資源的存取
  - `datasources:uid:prometheus-uid` - 僅存取特定的 Prometheus 資料來源
  - `dashboards:uid:abc123` - 僅存取 UID 為 `abc123` 的儀表板
  - `folders:uid:xyz789` - 僅存取 UID 為 `xyz789` 的資料夾
  - `teams:id:5` - 僅存取 ID 為 `5` 的團隊
  - `global.users:id:123` - 僅存取 ID 為 `123` 的使用者

**範例：**

- **完整的 MCP 伺服器存取：** 為所有工具授予廣泛的權限

  ```
  datasources:* (datasources:read, datasources:query)
  dashboards:* (dashboards:read, dashboards:create, dashboards:write)
  folders:* (用於儀表板建立和警報規則)
  teams:* (teams:read)
  global.users:* (users:read)
  ```

- **有限的資料來源存取：** 僅查詢特定的 Prometheus 和 Loki 執行個體

  ```
  datasources:uid:prometheus-prod (datasources:query)
  datasources:uid:loki-prod (datasources:query)
  ```

- **儀表板特定存取：** 僅讀取特定的儀表板
  ```
  dashboards:uid:monitoring-dashboard (dashboards:read)
  dashboards:uid:alerts-dashboard (dashboards:read)
  ```

### 工具

| 工具 | 類別 | 說明 | 必要 RBAC 權限 | 必要範圍 |
| --- | --- | --- | --- | --- |
| `list_teams` | Admin | 列出所有團隊 | `teams:read` | `teams:*` 或 `teams:id:1` |
| `list_users_by_org` | Admin | 列出組織中的所有使用者 | `users:read` | `global.users:*` 或 `global.users:id:123` |
| `search_dashboards` | Search | 搜尋儀表板 | `dashboards:read` | `dashboards:*` 或 `dashboards:uid:abc123` |
| `get_dashboard_by_uid` | Dashboard | 依 uid 取得儀表板 | `dashboards:read` | `dashboards:uid:abc123` |
| `update_dashboard` | Dashboard | 更新或建立新儀表板 | `dashboards:create`, `dashboards:write` | `dashboards:*`, `folders:*` 或 `folders:uid:xyz789` |
| `get_dashboard_panel_queries` | Dashboard | 從儀表板取得面板標題、查詢、資料來源 UID 和類型 | `dashboards:read` | `dashboards:uid:abc123` |
| `get_dashboard_property` | Dashboard | 使用 JSONPath 運算式提取儀表板的特定部分 | `dashboards:read` | `dashboards:uid:abc123` |
| `get_dashboard_summary` | Dashboard | 取得不含完整 JSON 的儀表板精簡摘要 | `dashboards:read` | `dashboards:uid:abc123` |
| `list_datasources` | Datasources | 列出資料來源 | `datasources:read` | `datasources:*` |
| `get_datasource_by_uid` | Datasources | 依 uid 取得資料來源 | `datasources:read` | `datasources:uid:prometheus-uid` |
| `get_datasource_by_name` | Datasources | 依名稱取得資料來源 | `datasources:read` | `datasources:*` 或 `datasources:uid:loki-uid` |
| `query_prometheus` | Prometheus | 針對 Prometheus 資料來源執行查詢 | `datasources:query` | `datasources:uid:prometheus-uid` |
| `list_prometheus_metric_metadata` | Prometheus | 列出指標元資料 | `datasources:query` | `datasources:uid:prometheus-uid` |
| `list_prometheus_metric_names` | Prometheus | 列出可用的指標名稱 | `datasources:query` | `datasources:uid:prometheus-uid` |
| `list_prometheus_label_names` | Prometheus | 列出符合選擇器的標籤名稱 | `datasources:query` | `datasources:uid:prometheus-uid` |
| `list_prometheus_label_values` | Prometheus | 列出特定標籤的值 | `datasources:query` | `datasources:uid:prometheus-uid` |
| `list_incidents` | Incident | 在 Grafana Incident 中列出事件 | Viewer 角色 | N/A |
| `create_incident` | Incident | 在 Grafana Incident 中建立事件 | Editor 角色 | N/A |
| `add_activity_to_incident` | Incident | 將活動項目新增至 Grafana Incident 中的事件 | Editor 角色 | N/A |
| `get_incident` | Incident | 依 ID 取得單一事件 | Viewer 角色 | N/A |
| `query_loki_logs` | Loki | 使用 LogQL 查詢和擷取日誌（日誌或指標查詢） | `datasources:query` | `datasources:uid:loki-uid` |
| `list_loki_label_names` | Loki | 列出日誌中所有可用的標籤名稱 | `datasources:query` | `datasources:uid:loki-uid` |
| `list_loki_label_values` | Loki | 列出特定日誌標籤的值 | `datasources:query` | `datasources:uid:loki-uid` |
| `query_loki_stats` | Loki | 取得有關日誌串流的統計資料 | `datasources:query` | `datasources:uid:loki-uid` |
| `list_alert_rules` | Alerting | 列出警報規則 | `alert.rules:read` | `folders:*` 或 `folders:uid:alerts-folder` |
| `get_alert_rule_by_uid` | Alerting | 依 UID 取得警報規則 | `alert.rules:read` | `folders:uid:alerts-folder` |
| `list_contact_points` | Alerting | 列出通知聯絡點 | `alert.notifications:read` | 全域範圍 |
| `list_oncall_schedules` | OnCall | 從 Grafana OnCall 列出排程 | `grafana-oncall-app.schedules:read` | 插件特定範圍 |
| `get_oncall_shift` | OnCall | 取得特定 OnCall 輪班的詳細資料 | `grafana-oncall-app.schedules:read` | 插件特定範圍 |
| `get_current_oncall_users` | OnCall | 取得特定排程目前待命的使用者 | `grafana-oncall-app.schedules:read` | 插件特定範圍 |
| `list_oncall_teams` | OnCall | 從 Grafana OnCall 列出團隊 | `grafana-oncall-app.user-settings:read` | 插件特定範圍 |
| `list_oncall_users` | OnCall | 從 Grafana OnCall 列出使用者 | `grafana-oncall-app.user-settings:read` | 插件特定範圍 |
| `list_alert_groups` | OnCall | 從 Grafana OnCall 列出具有篩選選項的警報群組 | `grafana-oncall-app.alert-groups:read` | 插件特定範圍 |
| `get_alert_group` | OnCall | 依其 ID 從 Grafana OnCall 取得特定警報群組 | `grafana-oncall-app.alert-groups:read` | 插件特定範圍 |
| `get_sift_investigation` | Sift | 依其 UUID 擷取現有的 Sift 調查 | Viewer 角色 | N/A |
| `get_sift_analysis` | Sift | 從 Sift 調查中擷取特定分析 | Viewer 角色 | N/A |
| `list_sift_investigations` | Sift | 擷取 Sift 調查列表，可選用 limit | Viewer 角色 | N/A |
| `find_error_pattern_logs` | Sift | 在 Loki 日誌中尋找升高的錯誤模式。 | Editor 角色 | N/A |
| `find_slow_requests` | Sift | 從相關的 tempo 資料來源中尋找緩慢的請求。 | Editor 角色 | N/A |
| `list_pyroscope_label_names` | Pyroscope | 列出符合選擇器的標籤名稱 | `datasources:query` | `datasources:uid:pyroscope-uid` |
| `list_pyroscope_label_values` | Pyroscope | 列出符合選擇器之標籤名稱的標籤值 | `datasources:query` | `datasources:uid:pyroscope-uid` |
| `list_pyroscope_profile_types` | Pyroscope | 列出可用的設定檔類型 | `datasources:query` | `datasources:uid:pyroscope-uid` |
| `fetch_pyroscope_profile` | Pyroscope | 以 DOT 格式擷取設定檔以供分析 | `datasources:query` | `datasources:uid:pyroscope-uid` |
| `get_assertions` | Asserts | 取得給定實體的斷言摘要 | 插件特定權限 | 插件特定範圍 |
| `generate_deeplink` | Navigation | 為 Grafana 資源產生準確的深層連結 URL | 無（唯讀 URL 產生） | N/A |

## CLI 旗標參考

`mcp-grafana` 二進位檔支援各種命令列旗標進行設定：

**傳輸選項：**
- `-t, --transport`：傳輸類型（`stdio`、`sse` 或 `streamable-http`）- 預設：`stdio`
- `--address`：SSE/streamable-http 伺服器的主機和連接埠 - 預設：`localhost:8000`
- `--base-path`：SSE/streamable-http 伺服器的基礎路徑
- `--endpoint-path`：streamable-http 伺服器的端點路徑 - 預設：`/`

**偵錯和記錄：**
- `--debug`：啟用偵錯模式以進行詳細的 HTTP 請求/回應記錄

**工具設定：**
- `--enabled-tools`：以逗號分隔的啟用工具列表 - 預設：所有工具皆啟用
- `--disable-search`：停用搜尋工具
- `--disable-datasource`：停用資料來源工具
- `--disable-incident`：停用事件工具
- `--disable-prometheus`：停用 prometheus 工具
- `--disable-loki`：停用 loki 工具
- `--disable-alerting`：停用警報工具
- `--disable-dashboard`：停用儀表板工具
- `--disable-oncall`：停用 oncall 工具
- `--disable-asserts`：停用 asserts 工具
- `--disable-sift`：停用 sift 工具
- `--disable-admin`：停用 admin 工具
- `--disable-pyroscope`：停用 pyroscope 工具
- `--disable-navigation`：停用導覽工具

**用戶端 TLS 設定（用於 Grafana 連線）：**
- `--tls-cert-file`：用於用戶端驗證的 TLS 憑證檔案路徑
- `--tls-key-file`：用於用戶端驗證的 TLS 私密金鑰檔案路徑
- `--tls-ca-file`：用於伺服器驗證的 TLS CA 憑證檔案路徑
- `--tls-skip-verify`：略過 TLS 憑證驗證（不安全）

**伺服器 TLS 設定（僅限 streamable-http 傳輸）：**
- `--server.tls-cert-file`：用於伺服器 HTTPS 的 TLS 憑證檔案路徑
- `--server.tls-key-file`：用於伺服器 HTTPS 的 TLS 私密金鑰檔案路徑

## 用法

此 MCP 伺服器可與本地 Grafana 執行個體和 Grafana Cloud 搭配使用。對於 Grafana Cloud，請在下方的設定範例中使用您的執行個體 URL（例如 `https://myinstance.grafana.net`）而非 `http://localhost:3000`。

1. 如果使用服務帳戶權杖驗證，請在 Grafana 中建立一個具有足夠權限以使用您想用之工具的服務帳戶，
   產生一個服務帳戶權杖，並將其複製到剪貼簿以在設定檔中使用。
   請遵循 [Grafana 服務帳戶文件][service-account] 以取得建立服務帳戶權杖的詳細資訊。

   > **注意：** 環境變數 `GRAFANA_API_KEY` 已被棄用，並將在未來版本中移除。請遷移至使用 `GRAFANA_SERVICE_ACCOUNT_TOKEN`。舊的變數名稱將繼續運作以保持向後相容性，但會顯示棄用警告。

2. 您有數種安裝 `mcp-grafana` 的選項：

   - **Docker 映像檔**：使用 Docker Hub 上預先建置的 Docker 映像檔。

     **重要**：Docker 映像檔的進入點預設設定為在 SSE 模式下執行 MCP 伺服器，但大多數使用者會希望使用 STDIO 模式以便與像 Claude Desktop 這類的 AI 助理直接整合：

     1. **STDIO 模式**：對於 stdio 模式，您必須使用 `-t stdio` 明確覆寫預設值，並包含 `-i` 旗標以保持 stdin 開啟：

     ```bash
     docker pull mcp/grafana
     # 對於本地 Grafana：
     docker run --rm -i -e GRAFANA_URL=http://localhost:3000 -e GRAFANA_SERVICE_ACCOUNT_TOKEN=<your service account token> mcp/grafana -t stdio
     # 對於 Grafana Cloud：
     docker run --rm -i -e GRAFANA_URL=https://myinstance.grafana.net -e GRAFANA_SERVICE_ACCOUNT_TOKEN=<your service account token> mcp/grafana -t stdio
     ```

     2. **SSE 模式**：在此模式下，伺服器作為一個 HTTP 伺服器執行，用戶端會連線到此伺服器。您必須使用 `-p` 旗標暴露 8000 連接埠：

     ```bash
     docker pull mcp/grafana
     docker run --rm -p 8000:8000 -e GRAFANA_URL=http://localhost:3000 -e GRAFANA_SERVICE_ACCOUNT_TOKEN=<your service account token> mcp/grafana
     ```

     3. **Streamable HTTP 模式**：在此模式下，伺服器作為一個獨立的程序運作，可以處理多個用戶端連線。您必須使用 `-p` 旗標暴露 8000 連接埠：對於此模式，您必須使用 `-t streamable-http` 明確覆寫預設值

     ```bash
     docker pull mcp/grafana
     docker run --rm -p 8000:8000 -e GRAFANA_URL=http://localhost:3000 -e GRAFANA_SERVICE_ACCOUNT_TOKEN=<your service account token> mcp/grafana -t streamable-http
     ```

     對於使用伺服器 TLS 憑證的 HTTPS streamable HTTP 模式：

     ```bash
     docker pull mcp/grafana
     docker run --rm -p 8443:8443 \
       -v /path/to/certs:/certs:ro \
       -e GRAFANA_URL=http://localhost:3000 \
       -e GRAFANA_SERVICE_ACCOUNT_TOKEN=<your service account token> \
       mcp/grafana \
       -t streamable-http \
       -addr :8443 \
       --server.tls-cert-file /certs/server.crt \
       --server.tls-key-file /certs/server.key
     ```

   - **下載二進位檔**：從 [releases 頁面](https://github.com/grafana/mcp-grafana/releases)下載最新版的 `mcp-grafana` 並將其放置在您的 `$PATH` 中。

   - **從原始碼建置**：如果您已安裝 Go 工具鏈，您也可以從原始碼建置並安裝它，使用 `GOBIN` 環境變數
     來指定應安裝二進位檔的目錄。此目錄也應在您的 `PATH` 中。

     ```bash
     GOBIN="$HOME/go/bin" go install github.com/grafana/mcp-grafana/cmd/mcp-grafana@latest
     ```

   - **使用 Helm 部署至 Kubernetes**：使用 [Grafana helm-charts 儲存庫中的 Helm chart](https://github.com/grafana/helm-charts/tree/main/charts/grafana-mcp)

     ```bash
     helm repo add grafana https://grafana.github.io/helm-charts
     helm install --set grafana.apiKey=<Grafana_ApiKey> --set grafana.url=<GrafanaUrl> my-release grafana/grafana-mcp
     ```


3. 將伺服器設定新增至您的用戶端設定檔。例如，對於 Claude Desktop：

   **如果使用二進位檔：**

   ```json
   {
     "mcpServers": {
       "grafana": {
         "command": "mcp-grafana",
         "args": [],
         "env": {
           "GRAFANA_URL": "http://localhost:3000",  // 或 "https://myinstance.grafana.net" for Grafana Cloud
           "GRAFANA_SERVICE_ACCOUNT_TOKEN": "<your service account token>",
           // 如果使用使用者名稱/密碼驗證
           "GRAFANA_USERNAME": "<your username>",
           "GRAFANA_PASSWORD": "<your password>"
         }
       }
     }
   }
   ```

> 注意：如果您在 Claude Desktop 中看到 `Error: spawn mcp-grafana ENOENT`，您需要指定 `mcp-grafana` 的完整路徑。

**如果使用 Docker：**

```json
{
  "mcpServers": {
    "grafana": {
      "command": "docker",
      "args": [
        "run",
        "--rm",
        "-i",
        "-e",
        "GRAFANA_URL",
        "-e",
        "GRAFANA_SERVICE_ACCOUNT_TOKEN",
        "mcp/grafana",
        "-t",
        "stdio"
      ],
      "env": {
        "GRAFANA_URL": "http://localhost:3000",  // 或 "https://myinstance.grafana.net" for Grafana Cloud
        "GRAFANA_SERVICE_ACCOUNT_TOKEN": "<your service account token>",
        // 如果使用使用者名稱/密碼驗證
        "GRAFANA_USERNAME": "<your username>",
        "GRAFANA_PASSWORD": "<your password>"
      }
    }
  }
}
```

> 注意：此處的 `-t stdio` 參數至關重要，因為它會覆寫 Docker 映像檔中的預設 SSE 模式。

**在 VSCode 中使用遠端 MCP 伺服器**

如果您正在使用 VSCode 並在 SSE 模式下執行 MCP 伺服器（這是使用 Docker 映像檔而不覆寫傳輸時的預設模式），請確保您的 `.vscode/settings.json` 包含以下內容：

```json
"mcp": {
  "servers": {
    "grafana": {
      "type": "sse",
      "url": "http://localhost:8000/sse"
    }
  }
}
```

對於使用伺服器 TLS 憑證的 HTTPS streamable HTTP 模式：

```json
"mcp": {
  "servers": {
    "grafana": {
      "type": "sse",
      "url": "https://localhost:8443/sse"
    }
  }
}
```

### 偵錯模式

您可以透過在指令中新增 `-debug` 旗標來為 Grafana 傳輸啟用偵錯模式。這將提供 MCP 伺服器與 Grafana API 之間 HTTP 請求和回應的詳細記錄，有助於疑難排解。

若要搭配 Claude Desktop 設定使用偵錯模式，請如下更新您的設定：

**如果使用二進位檔：**

```json
{
  "mcpServers": {
    "grafana": {
      "command": "mcp-grafana",
      "args": ["-debug"],
      "env": {
        "GRAFANA_URL": "http://localhost:3000",  // 或 "https://myinstance.grafana.net" for Grafana Cloud
        "GRAFANA_SERVICE_ACCOUNT_TOKEN": "<your service account token>"
      }
    }
  }
}
```

**如果使用 Docker：**

```json
{
  "mcpServers": {
    "grafana": {
      "command": "docker",
      "args": [
        "run",
        "--rm",
        "-i",
        "-e",
        "GRAFANA_URL",
        "-e",
        "GRAFANA_SERVICE_ACCOUNT_TOKEN",
        "mcp/grafana",
        "-t",
        "stdio",
        "-debug"
      ],
      "env": {
        "GRAFANA_URL": "http://localhost:3000",  // 或 "https://myinstance.grafana.net" for Grafana Cloud
        "GRAFANA_SERVICE_ACCOUNT_TOKEN": "<your service account token>"
      }
    }
  }
}
```

> 注意：與標準設定一樣，需要 `-t stdio` 參數來覆寫 Docker 映像檔中的預設 SSE 模式。

### TLS 設定

如果您的 Grafana 執行個體位於 mTLS 之後或需要自訂 TLS 憑證，您可以設定 MCP 伺服器使用自訂憑證。伺服器支援以下 TLS 設定選項：

- `--tls-cert-file`：用於用戶端驗證的 TLS 憑證檔案路徑
- `--tls-key-file`：用於用戶端驗證的 TLS 私密金鑰檔案路徑
- `--tls-ca-file`：用於伺服器驗證的 TLS CA 憑證檔案路徑
- `--tls-skip-verify`：略過 TLS 憑證驗證（不安全，僅供測試使用）

**使用用戶端憑證驗證的範例：**

```json
{
  "mcpServers": {
    "grafana": {
      "command": "mcp-grafana",
      "args": [
        "--tls-cert-file",
        "/path/to/client.crt",
        "--tls-key-file",
        "/path/to/client.key",
        "--tls-ca-file",
        "/path/to/ca.crt"
      ],
      "env": {
        "GRAFANA_URL": "https://secure-grafana.example.com",
        "GRAFANA_SERVICE_ACCOUNT_TOKEN": "<your service account token>"
      }
    }
  }
}
```

**使用 Docker 的範例：**

```json
{
  "mcpServers": {
    "grafana": {
      "command": "docker",
      "args": [
        "run",
        "--rm",
        "-i",
        "-v",
        "/path/to/certs:/certs:ro",
        "-e",
        "GRAFANA_URL",
        "-e",
        "GRAFANA_SERVICE_ACCOUNT_TOKEN",
        "mcp/grafana",
        "-t",
        "stdio",
        "--tls-cert-file",
        "/certs/client.crt",
        "--tls-key-file",
        "/certs/client.key",
        "--tls-ca-file",
        "/certs/ca.crt"
      ],
      "env": {
        "GRAFANA_URL": "https://secure-grafana.example.com",
        "GRAFANA_SERVICE_ACCOUNT_TOKEN": "<your service account token>"
      }
    }
  }
}
```

TLS 設定適用於 MCP 伺服器使用的所有 HTTP 用戶端，包括：

- 主要的 Grafana OpenAPI 用戶端
- Prometheus 資料來源用戶端
- Loki 資料來源用戶端
- 事件管理用戶端
- Sift 調查用戶端
- 警報用戶端
- Asserts 用戶端

**直接使用 CLI 的範例：**

用於測試自我簽署憑證：

```bash
./mcp-grafana --tls-skip-verify -debug
```

使用用戶端憑證驗證：

```bash
./mcp-grafana \
  --tls-cert-file /path/to/client.crt \
  --tls-key-file /path/to/client.key \
  --tls-ca-file /path/to/ca.crt \
  -debug
```

僅使用自訂 CA 憑證：

```bash
./mcp-grafana --tls-ca-file /path/to/ca.crt
```

**程式化用法：**

如果您以程式化方式使用此程式庫，您也可以建立啟用 TLS 的上下文函式：

```go
// 使用結構體字面值
tlsConfig := &mcpgrafana.TLSConfig{
    CertFile: "/path/to/client.crt",
    KeyFile:  "/path/to/client.key",
    CAFile:   "/path/to/ca.crt",
}
grafanaConfig := mcpgrafana.GrafanaConfig{
    Debug:     true,
    TLSConfig: tlsConfig,
}
contextFunc := mcpgrafana.ComposedStdioContextFunc(grafanaConfig)

// 或內嵌
grafanaConfig := mcpgrafana.GrafanaConfig{
    Debug: true,
    TLSConfig: &mcpgrafana.TLSConfig{
        CertFile: "/path/to/client.crt",
        KeyFile:  "/path/to/client.key",
        CAFile:   "/path/to/ca.crt",
    },
}
contextFunc := mcpgrafana.ComposedStdioContextFunc(grafanaConfig)
```

### 伺服器 TLS 設定（僅限 Streamable HTTP 傳輸）

當使用 streamable HTTP 傳輸（`-t streamable-http`）時，您可以設定 MCP 伺服器提供 HTTPS 而非 HTTP。當您需要保護 MCP 用戶端與伺服器本身之間的連線時，這非常有用。

伺服器支援以下 streamable HTTP 傳輸的 TLS 設定選項：

- `--server.tls-cert-file`：用於伺服器 HTTPS 的 TLS 憑證檔案路徑（TLS 必需）
- `--server.tls-key-file`：用於伺服器 HTTPS 的 TLS 私密金鑰檔案路徑（TLS 必需）

**注意**：這些旗標與上述的用戶端 TLS 旗標完全無關。用戶端 TLS 旗標設定 MCP 伺服器如何連線到 Grafana，而這些伺服器 TLS 旗標設定用戶端在使用 streamable HTTP 傳輸時如何連線到 MCP 伺服器。

**使用 HTTPS streamable HTTP 伺服器的範例：**

```bash
./mcp-grafana \
  -t streamable-http \
  --server.tls-cert-file /path/to/server.crt \
  --server.tls-key-file /path/to/server.key \
  -addr :8443
```

這將在 HTTPS 連接埠 8443 上啟動 MCP 伺服器。用戶端接著將連線到 `https://localhost:8443/` 而非 `http://localhost:8000/`。

**使用伺服器 TLS 的 Docker 範例：**

```bash
docker run --rm -p 8443:8443 \
  -v /path/to/certs:/certs:ro \
  -e GRAFANA_URL=http://localhost:3000 \
  -e GRAFANA_SERVICE_ACCOUNT_TOKEN=<your service account token> \
  mcp/grafana \
  -t streamable-http \
  -addr :8443 \
  --server.tls-cert-file /certs/server.crt \
  --server.tls-key-file /certs/server.key
```

## 疑難排解

### Grafana 版本相容性

如果您在使用與資料來源相關的工具時遇到以下錯誤：

```
get datasource by uid : [GET /datasources/uid/{uid}][400] getDataSourceByUidBadRequest {"message":"id is invalid"}
```

這通常表示您使用的 Grafana 版本早於 9.0。`/datasources/uid/{uid}` API 端點是在 Grafana 9.0 中引入的，資料來源操作在較早版本上會失敗。

**解決方案：** 將您的 Grafana 執行個體升級至 9.0 或更新版本以解決此問題。

## 開發

歡迎貢獻！如果您有任何建議或改進，請開啟一個 issue 或提交一個 pull request。

本專案以 Go 語言撰寫。請依照您平台的指示安裝 Go。

若要在本地以 STDIO 模式執行伺服器（這是本地開發的預設模式），請使用：

```bash
make run
```

若要在本地以 SSE 模式執行伺服器，請使用：

```bash
go run ./cmd/mcp-grafana --transport sse
```

您也可以在自訂建置的 Docker 映像檔中使用 SSE 傳輸執行伺服器。就像已發布的 Docker 映像檔一樣，這個自訂映像檔的進入點預設為 SSE 模式。若要建置映像檔，請使用：

```
make build-image
```

若要以 SSE 模式執行映像檔（預設），請使用：

```
docker run -it --rm -p 8000:8000 mcp-grafana:latest
```

如果您需要改以 STDIO 模式執行，請覆寫傳輸設定：

```
docker run -it --rm mcp-grafana:latest -t stdio
```

### 測試

有三種類型的測試可用：

1. 單元測試（無需外部依賴）：

```bash
make test-unit
```

您也可以使用以下指令執行單元測試：

```bash
make test
```

2. 整合測試（需要 docker 容器啟動並執行）：

```bash
make test-integration
```

3. 雲端測試（需要雲端 Grafana 執行個體和憑證）：

```bash
make test-cloud
```

> 注意：雲端測試在 CI 中會自動設定。對於本地開發，您需要設定自己的 Grafana Cloud 執行個體和憑證。

更全面的整合測試將需要一個在本地 3000 連接埠上執行的 Grafana 執行個體；您可以使用 Docker Compose 啟動一個：

```bash
docker-compose up -d
```

整合測試可以透過以下指令執行：

```bash
make test-all
```

如果您要新增更多工具，請為它們新增整合測試。現有的測試應該是一個很好的起點。

### 程式碼檢查

若要檢查程式碼，請執行：

```bash
make lint
```

這包含一個自訂的 linter，它會檢查 `jsonschema` 結構體標籤中未逸出的逗號。`description` 欄位中的逗號必須使用 `\\,` 逸出，以防止靜默截斷。您可以使用以下指令單獨執行此 linter：

```bash
make lint-jsonschema
```

請參閱 [JSONSchema Linter 文件](internal/linter/jsonschema/README.md)以取得更多詳細資訊。

## 授權

本專案採用 [Apache License, Version 2.0](LICENSE) 授權。

[mcp]: https://modelcontextprotocol.io/
[service-account]: https://grafana.com/docs/grafana/latest/administration/service-accounts/#add-a-token-to-a-service-account-in-grafana
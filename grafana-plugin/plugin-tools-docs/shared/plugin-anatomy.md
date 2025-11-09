您建立的每個插件至少需要兩個檔案：`plugin.json` 和 `src/module.ts`。

### `plugin.json`

Grafana 啟動時，它會掃描[插件目錄](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/#plugins)中任何包含 `plugin.json` 檔案的子目錄。`plugin.json` 檔案包含有關您插件的資訊，並告知 Grafana 您的插件需要哪些功能和相依性。

雖然某些插件類型可以有特定的設定選項，但讓我們看看強制性的選項：

- `type` 告訴 Grafana 預期哪種類型的插件。Grafana 支援三種類型的插件：`panel`、`datasource` 和 `app`。
- `name` 是使用者在插件列表中會看到的名稱。如果您正在建立一個資料來源，這通常是它所連接的資料庫的名稱，例如 Prometheus、PostgreSQL 或 Stackdriver。
- `id` 唯一地識別您的插件，並且應遵循此命名慣例：`<$organization-name>-<$plugin-name>-<$plugin-type>`。create-plugin 工具會根據您對其提示的回應正確地設定此項。

若要查看 `plugin.json` 的所有可用設定，請參閱 [plugin.json 結構描述](../reference/metadata.md)。

### `module.ts`

發現您的插件後，Grafana 會載入 `module.js` 檔案，這是您插件的進入點。`module.js` 公開了您插件的實作，這取決於您正在建立的插件類型。

具體來說，`src/module.ts` 需要匯出一個擴充 [GrafanaPlugin](https://github.com/grafana/grafana/blob/f900098cc9f5771c02b6189ba5138547b4f5e6c2/packages/grafana-data/src/types/plugin.ts#L175) 的類別，並且可以是以下任何一種：

- [PanelPlugin](https://github.com/grafana/grafana/blob/f900098cc9f5771c02b6189ba5138547b4f5e6c2/packages/grafana-data/src/panel/PanelPlugin.ts#L95)
- [DataSourcePlugin](https://github.com/grafana/grafana/blob/f900098cc9f5771c02b6189ba5138547b4f5e6c2/packages/grafana-data/src/types/datasource.ts#L33)
- [AppPlugin](https://github.com/grafana/grafana/blob/f900098cc9f5771c02b6189ba5138547b4f5e6c2/packages/grafana-data/src/types/app.ts#L58)
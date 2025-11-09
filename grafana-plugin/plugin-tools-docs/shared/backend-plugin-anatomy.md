用於為 {props.pluginType} 建置後端的資料夾和檔案如下：

| 檔案/資料夾 | 說明 |
| --- | --- |
| `Magefile.go` | 使用 mage 建置檔案並非必要，但我們強烈建議您使用它們，以便您可以使用插件 SDK 提供的建置目標。 |
| `/go.mod ` | Go [模組相依性](https://golang.org/cmd/go/#hdr-The_go_mod_file)。 |
| `/src/plugin.json` | 描述插件的 JSON 檔案。 |
| `/pkg/main.go` | 插件二進位檔的起點。 |

#### plugin.json 檔案

所有插件都需要 [`plugin.json`](../reference/metadata.md) 檔案。在建置插件後端元件時，請特別注意以下屬性：

| 屬性 | 說明 |
| --- | --- |
| `backend` | 對於具有後端元件的插件，請設定為 `true`。這會告知 Grafana 在載入插件時應啟動一個二進位檔。 |
| `executable` | 這是 Grafana 預期啟動的可執行檔的名稱。有關詳細資訊，請參閱 [plugin.json 參考](../reference/metadata.md)。 |
| `alerting` | 如果您的後端資料來源支援警示，請設定為 `true`。需要將 `backend` 設定為 `true`。 |
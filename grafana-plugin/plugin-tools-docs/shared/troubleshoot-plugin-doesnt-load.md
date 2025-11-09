### Grafana 未載入我的插件

請確保 Grafana 已在開發模式下啟動。如果您是從原始碼執行 Grafana，請將以下行新增至您的 `conf/custom.ini` 檔案：

```ini
app_mode = development
```

:::note

如果您還沒有 `conf/custom.ini` 檔案，請在繼續之前建立它。

:::

然後，您可以透過在 Grafana 儲存庫根目錄中執行 `make run & make run-frontend` 來以開發模式啟動 Grafana。

如果您是從二進位檔案或在 Docker 容器內執行 Grafana，您可以透過將環境變數 `GF_DEFAULT_APP_MODE` 設定為 `development` 來以開發模式啟動它。

:::note

預設情況下，Grafana 要求插件經過簽署。若要載入未簽署的插件，您需要將 Grafana 設定為[允許未簽署的插件](https://grafana.com/docs/grafana/latest/administration/plugin-management/#allow-unsigned-plugins)。如需更多資訊，請參閱[插件簽章驗證](https://grafana.com/docs/grafana/latest/administration/plugin-management/#backend-plugins)。

:::
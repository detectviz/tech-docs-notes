Grafana [create-plugin 工具](https://www.npmjs.com/package/@grafana/create-plugin)是一個 CLI 應用程式，可簡化 Grafana 插件開發，讓您可以專注於程式碼。該工具會為您建立一個入門插件、所有必要的設定，以及一個使用 [Docker Compose](https://docs.docker.com/compose/) 的開發環境。

1. 在一個新目錄中，使用 create-plugin 工具從範本建立一個插件。當提示您選擇插件類型時，請選取 {props.pluginType}：

   ```shell
   npx @grafana/create-plugin@latest
   ```

2. 前往您新建立的插件目錄：

   ```shell
   cd <your-plugin>
   ```

3. 安裝相依性：

   ```shell
   npm install
   ```

4. 建置插件：

   ```shell
   npm run dev
   ```

5. 啟動 Grafana：

   ```shell
   docker compose up
   ```

6. 開啟 Grafana，預設為 [http://localhost:3000](http://localhost:3000)，然後前往 **管理** > **插件**。請確保您的 {props.pluginType} 插件在那裡。

您也可以透過檢查日誌來驗證 Grafana 是否已發現您的插件：

```
INFO[01-01|12:00:00] Plugin registered       logger=plugin.loader pluginID=<your-plugin>
```
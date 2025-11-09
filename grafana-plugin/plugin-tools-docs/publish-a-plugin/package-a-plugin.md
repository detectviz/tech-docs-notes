---
id: package-a-plugin
title: 打包插件
sidebar_position: 2
description: 如何打包 Grafana 插件。
keywords:
  - grafana
  - plugins
  - plugin
  - links
  - package
  - packaging
  - packages
---

# 打包插件

打包插件以組織插件程式碼，並使其準備好在您的組織中使用。請遵循以下步驟將插件打包成 ZIP 檔案。

1. 建置插件

   ```shell npm2yarn
    npm install
    npm run build
   ```

2. 可選：如果您的插件有後端，也請建置它。

   ```
   mage
   ```

   請確保所有二進位檔案都是可執行的，並具有 `0755` (`-rwxr-xr-x`) 權限。

3. 簽署插件。若要深入了解，請參閱[簽署插件](./sign-a-plugin.md)。

4. 將 `dist` 目錄重新命名以符合您的插件 ID，然後建立一個 ZIP 封存。

   ```
   mv dist/ myorg-simple-panel
   zip myorg-simple-panel-1.0.0.zip myorg-simple-panel -r
   ```

5. 可選：使用 [zipinfo](https://linux.die.net/man/1/zipinfo) 驗證您的插件是否已正確打包。
   它應該看起來像這樣：

   ```shell
   $ zipinfo grafana-clickhouse-datasource-1.1.2.zip

   Archive:  grafana-clickhouse-datasource-1.1.2.zip
   Zip file size: 34324077 bytes, number of entries: 22
   drwxr-xr-x          0 bx stor 22-Mar-24 23:23 grafana-clickhouse-datasource/
   -rw-r--r--       1654 bX defN 22-Mar-24 23:23 grafana-clickhouse-datasource/CHANGELOG.md
   -rw-r--r--      11357 bX defN 22-Mar-24 23:23 grafana-clickhouse-datasource/LICENSE
   -rw-r--r--       2468 bX defN 22-Mar-24 23:23 grafana-clickhouse-datasource/MANIFEST.txt
   -rw-r--r--       8678 bX defN 22-Mar-24 23:23 grafana-clickhouse-datasource/README.md
   drwxr-xr-x          0 bx stor 22-Mar-24 23:23 grafana-clickhouse-datasource/dashboards/
   -rw-r--r--      42973 bX defN 22-Mar-24 23:23 grafana-clickhouse-datasource/dashboards/cluster-analysis.json
   -rw-r--r--      56759 bX defN 22-Mar-24 23:23 grafana-clickhouse-datasource/dashboards/data-analysis.json
   -rw-r--r--      39406 bX defN 22-Mar-24 23:23 grafana-clickhouse-datasource/dashboards/query-analysis.json
   -rwxr-xr-x   16469136 bX defN 22-Mar-24 23:23 grafana-clickhouse-datasource/gpx_clickhouse_darwin_amd64
   -rwxr-xr-x   16397666 bX defN 22-Mar-24 23:23 grafana-clickhouse-datasource/gpx_clickhouse_darwin_arm64
   -rwxr-xr-x   14942208 bX defN 22-Mar-24 23:23 grafana-clickhouse-datasource/gpx_clickhouse_linux_amd64
   -rwxr-xr-x   14155776 bX defN 22-Mar-24 23:23 grafana-clickhouse-datasource/gpx_clickhouse_linux_arm
   -rwxr-xr-x   14548992 bX defN 22-Mar-24 23:23 grafana-clickhouse-datasource/gpx_clickhouse_linux_arm64
   -rwxr-xr-x   15209472 bX defN 22-Mar-24 23:23 grafana-clickhouse-datasource/gpx_clickhouse_windows_amd64.exe
   drwxr-xr-x          0 bx stor 22-Mar-24 23:23 grafana-clickhouse-datasource/img/
   -rw-r--r--        304 bX defN 22-Mar-24 23:23 grafana-clickhouse-datasource/img/logo.png
   -rw-r--r--       1587 bX defN 22-Mar-24 23:23 grafana-clickhouse-datasource/img/logo.svg
   -rw-r--r--     138400 bX defN 22-Mar-24 23:23 grafana-clickhouse-datasource/module.js
   -rw-r--r--        808 bX defN 22-Mar-24 23:23 grafana-clickhouse-datasource/module.js.LICENSE.txt
   -rw-r--r--     487395 bX defN 22-Mar-24 23:23 grafana-clickhouse-datasource/module.js.map
   -rw-r--r--       1616 bX defN 22-Mar-24 23:23 grafana-clickhouse-datasource/plugin.json
   22 files, 92516655 bytes uncompressed, 34319591 bytes compressed:  62.9%
   ```

打包好您的插件後，您可以繼續：

- [發布插件](./publish-or-update-a-plugin.md)以與全世界分享，或
- [安裝已打包的插件](https://grafana.com/docs/grafana/latest/administration/plugin-management/#install-a-packaged-plugin)並將其解壓縮到您的插件目錄中。
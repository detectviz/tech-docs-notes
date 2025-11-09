---
id: add-support-for-annotation-queries
title: 新增對註釋查詢的支援
description: 在您的資料來源插件中新增對註釋查詢的支援。
keywords:
  - grafana
  - plugins
  - plugin
  - annotations
  - annotation query
  - annotation queries
---

您可以為您的插件新增註釋查詢支援，這將會將資訊插入 Grafana 警示中。本指南說明如何為資料來源插件新增對[註釋查詢](https://grafana.com/docs/grafana/latest/dashboards/build-dashboards/annotate-visualizations/)的支援。

## 在您的資料來源插件中支援註釋查詢

若要啟用註釋，只需在您的插件中新增兩行程式碼。Grafana 會使用您的預設查詢編輯器來編輯註釋查詢。

1. 在 [plugin.json](../../reference/metadata.md) 檔案中新增 `"annotations": true`，讓 Grafana 知道您的插件支援註釋。

   ```json title="src/plugin.json"
   {
     "annotations": true
   }
   ```

2. 在 `datasource.ts` 中，覆寫 `DataSourceApi`（或後端資料來源的 `DataSourceWithBackend`）中的 `annotations` 屬性。對於預設行為，請將 `annotations` 設定為一個空物件。

   ```ts title="src/datasource.ts"
   annotations: {
   }
   ```
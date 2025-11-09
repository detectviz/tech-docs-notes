---
id: work-with-nested-plugins
title: 使用巢狀插件
description: 如何使用巢狀插件，Grafana 應用程式插件。
keywords:
  - grafana
  - plugins
  - plugin
  - advanced
  - apps
  - app plugins
  - nested
---

Grafana 應用程式插件可以將前端和後端的資料來源與面板插件巢狀在一起，以便您提供完整的使用者體驗。

## 開始之前

建立一個應用程式插件。有關說明，請參閱[建立應用程式插件](../../tutorials/build-an-app-plugin)的教學。

## 巢狀插件的結構

巢狀插件位於應用程式插件的 `src` 資料夾內。它們通常遵循與插件相同的結構，並擁有自己的 `plugin.json`，但它們沒有自己的 `package.json` 或 `.config` 資料夾。

以下是巢狀資料來源插件的範例：

```diff bash
./src
 ├── README.md
 ├── components
+├── nested-datasource
+│   ├── components
+│   │   ├── ConfigEditor.tsx
+│   │   └── QueryEditor.tsx
+│   ├── datasource.ts
+│   ├── img
+│   ├── module.ts
+│   ├── plugin.json
+│   └── types.ts
 ├── img
 │   └── logo.svg
 ├── module.ts
 └── plugin.json
```

## 何時使用巢狀插件

當您希望將資料來源或面板插件與您的應用程式插件一起分發時，

每個巢狀資料來源都可以有自己的後端，獨立於應用程式插件的後端。

:::note

請注意，`nested-datasource` 插件沒有自己的 `package.json`。巢狀插件資料夾的名稱並不重要。

:::

## 如何將巢狀插件新增至應用程式插件

1. 建立一個將成為巢狀插件的新插件：

   :::important

   請在您的應用程式插件目錄之外開始。

   :::

   使用 `create-plugin` 工具產生一個新插件：

   ```bash
   npx @grafana/create-plugin@latest
   ```

   選取所需的插件類型（資料來源或面板），提供一個名稱，並使用與您的應用程式插件相同的組織。

2. 準備巢狀插件：

   將您新產生的插件的 `src` 資料夾重新命名為能反映其特定用途的名稱（例如，`nested-datasource`）。

3. 整合至您的應用程式插件：

   將重新命名的 `src` 資料夾直接複製到您的應用程式插件的 `src` 資料夾內。
   您可以安全地忽略其他產生的檔案（例如 `package.json`、`.config` 等）。您的應用程式中不需要這些檔案。

4. （可選）將您的資料來源新增至您的佈建資料來源：

   如果您要新增巢狀資料來源，請記得將其新增至佈建的資料來源 YAML 設定檔中。有關更多詳細資訊，請參閱[佈建 Grafana](https://grafana.com/docs/grafana/latest/administration/provisioning/#data-sources) 的文件。

5. （可選）清理您的目錄：

   您現在可以刪除最初產生的巢狀插件的整個目錄。

完成這些步驟後，您的應用程式插件現在就包含了您的巢狀資料來源或面板的原始碼，可以進行進一步的開發。
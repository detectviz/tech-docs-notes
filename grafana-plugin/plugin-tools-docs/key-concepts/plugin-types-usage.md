---
id: plugin-types-usage
title: 插件類型與用法
description: 了解不同類型的 Grafana 插件、其用法和設定。
keywords:
  - grafana
  - plugins
  - plugin
  - usage
  - provisioning
  - provision
  - configuration
  - configure
sidebar_position: 10
---

# Grafana 插件類型與用法

:::note

本文件討論與插件開發人員相關的插件設定、佈建和用法。有關插件管理的一般資訊，請參閱[插件管理](https://grafana.com/docs/grafana/latest/administration/plugin-management/)。

:::

## 概觀

Grafana 插件開發根據您想建立的使用者體驗類型提供多種選項。無論您的需求為何，都有支援的插件類型可供您使用：

- **面板插件** - 一種新的資料視覺化方式。
- **資料來源插件** - 連接到新的資料庫或其他資料來源。
- **應用程式插件** - 一種開箱即用的整合體驗。

有關如何快速建立[每種類型](../reference/cli-commands.mdx#select-a-plugin-type)插件的說明，請參閱[開始使用](../get-started.md)。

:::note

請勿為與插件開發相關的主題開啟支援工單。如需插件開發方面的協助，請洽詢[社群論壇](https://community.grafana.com/c/plugin-development/30)。

:::

## 面板（視覺化）插件

面板插件（也稱為視覺化）的開發可讓 Grafana 使用自訂視覺化，類似於現有的插件，例如 [Polystat](https://grafana.com/grafana/plugins/grafana-polystat-panel/) 面板。

### 面板插件的用法

任何已安裝的 `panel` 類型插件都可以在儀表板中作為視覺化來使用、選取和設定。

與資料來源和應用程式相比，目前不支援在 [Grafana 組織](https://grafana.com/docs/grafana/latest/administration/organization-management/#about-organizations)層級設定視覺化。

:::note

隨著最近推出的 [Grafana Scenes](https://grafana.com/developers/scenes)，您可以在 Scenes 實作中使用任何已安裝的 `panel` 類型插件作為視覺化。

:::

## 資料來源插件

在插件開發中，您可以建立新的資料來源插件，以將 Grafana 與新的資料庫搭配使用，類似於現有的插件，例如 [MongoDB](https://grafana.com/grafana/plugins/grafana-mongodb-datasource/) 或 [Google BigQuery](https://grafana.com/grafana/plugins/grafana-bigquery-datasource/)。資料來源插件可以在前端和[後端](../key-concepts/backend-plugins/)新增。

### 資料來源插件的用法

當您想使用第三方服務提供的資料，並在 Grafana 儀表板、探索、警示等中使用時，請建立並設定 Grafana 資料來源。

對於任何已安裝的 `datasource` 類型插件，您可以為每個 [Grafana 組織](https://grafana.com/docs/grafana/latest/administration/organization-management/#about-organizations)建立和設定任意數量的資料來源。在您建立零到無限個資料來源後，它們會保存在 Grafana 的資料庫中。

:::note

為了區分 Grafana 資料來源和資料來源插件，我們有時會將後者稱為_資料來源執行個體_，即一個已設定的 Grafana 資料來源，其 `plugin id` 類型為 `datasource`。

:::

### 資料來源插件的全域設定

使用 Grafana 設定檔來設定您的應用程式 [`plugin_id`](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/#pluginplugin_id)。

:::info

並非所有插件都支援此類型的設定。有關詳細資訊，請參閱特定插件的 readme。

:::

### 資料來源的佈建

資料來源也可以使用 Grafana 的[佈建功能](https://grafana.com/docs/grafana/latest/administration/provisioning/#data-sources)進行佈建，以便您可以在 Grafana 啟動時或[隨需](https://grafana.com/docs/grafana/latest/developers/http_api/admin/#reload-provisioning-configurations)將 Grafana 定義為程式碼，作為 GitOps 方法的一部分。

請注意，必須先安裝 `datasource` 類型的插件，然後才能對其進行佈建。

### 儀表板的捆綁

資料來源插件可以透過在 `plugin.json` 檔案中參考儀表板 JSON 檔案（包括 `property` 和 `type=dashboard`）來[包含儀表板](../reference/metadata.md#includes)。Grafana 在匯入儀表板時會將其放置在 `General` 資料夾中。

## 應用程式插件

應用程式插件（也稱為應用程式）的開發可讓您建立開箱即用的解決方案，例如 [Redis](https://grafana.com/grafana/plugins/redis-app/) 應用程式。您可以選擇性地捆綁資料來源和面板，以及提供自訂頁面、[Scenes](https://grafana.com/developers/scenes) 和 [UI 擴充功能](../key-concepts/ui-extensions.md)。

### 應用程式插件的用法

當您想利用或建立針對第三方服務的量身打造的監控檢視，並選擇性地使用自訂頁面或 UI 擴充功能時，請設定 Grafana 應用程式。對於任何已安裝的 `app` 類型插件，您可以為每個 [Grafana 組織](https://grafana.com/docs/grafana/latest/administration/organization-management/#about-organizations)啟用一次，它們會保存在 Grafana 的資料庫中。

:::note

為了區分 Grafana 應用程式和應用程式插件，我們有時會將後者稱為_應用程式執行個體_，即一個已設定的 Grafana 應用程式，其 `plugin id` 類型為 `app`。

:::

### 應用程式插件的全域設定

使用 [Grafana 設定檔](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/#configuration-file-location)來設定您的應用程式 [`plugin_id`](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/#pluginplugin_id)。

:::info

並非所有插件都支援此類型的設定。有關詳細資訊，請參閱特定插件的 readme。

:::

### 應用程式插件的佈建

應用程式也可以使用 Grafana 的[佈建功能](https://grafana.com/docs/grafana/latest/administration/provisioning/#plugins)進行佈建，以便您可以在 Grafana 啟動時或[隨需](https://grafana.com/docs/grafana/latest/developers/http_api/admin/#reload-provisioning-configurations)將 Grafana 定義為程式碼，作為 GitOps 方法的一部分。

請注意，必須先安裝插件，然後才能成功佈建具有 `app` 類型的 `plugin id`。

### 應用程式的捆綁

應用程式插件類型可讓您[在其內部巢狀其他插件](../how-to-guides/app-plugins/work-with-nested-plugins)；換句話說，在同一個套件中捆綁或[包含](../reference/metadata.md#includes)多個插件。

### 儀表板的捆綁

應用程式插件可以透過在 `plugin.json` 中參考儀表板 JSON 檔案（包括 `property` 和 `type=dashboard`）來包含儀表板。Grafana 在匯入儀表板時會將其放置在 `General` 資料夾中，這在啟用應用程式時會自動發生。
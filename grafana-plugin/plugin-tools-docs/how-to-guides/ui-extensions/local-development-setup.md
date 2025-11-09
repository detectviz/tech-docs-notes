---
title: 在多個插件之間測試 UI 擴充功能
description: 學習如何設定您的本地環境以進行 UI 擴充功能開發。
---

# 在多個插件之間測試 UI 擴充功能

在開發與另一個插件的 UI 擴充功能互動的插件時，您需要一種方法讓兩個插件都在您的本地 Grafana 執行個體中執行。本指南將引導您設定一個本地開發環境，以便在多個插件之間測試 UI 擴充功能。

此過程涉及兩個主要步驟：

1.  安裝您要測試的目標插件。
2.  使用 Grafana 的佈建系統設定目標插件。

## 步驟 1：安裝目標插件

若要測試您的插件與另一個插件之間的互動，您首先需要在您的本地開發環境中安裝目標插件。您有兩種選擇。

### 選項 1：從 URL 安裝（建議）

建議的方法是在您的 `docker-compose.yml` 檔案中使用 `GF_INSTALL_PLUGINS` 環境變數。這可讓您從 URL 安裝插件的預先建置版本。此方法很靈活，只需變更 URL 即可輕鬆切換不同版本的插件。

以下是如何在您的 `docker-compose.yml` 中使用 `GF_INSTALL_PLUGINS` 的範例：

```yaml
environment:
  - GF_INSTALL_PLUGINS=https://example.com/path/to/your-plugin.zip;your-plugin-id
```

### 選項 2：複製、建置和連結插件

或者，您可以複製目標插件的原始碼並將其連結至您的本地 Grafana 執行個體。但是，此方法更複雜，因為它需要您在執行插件之前安裝所有相依性並建置插件。因此，通常不建議使用此方法。

## 步驟 2：使用佈建設定目標插件

插件安裝後，您需要進行設定。最好的方法是使用 Grafana 的佈建系統，它會在您的本地 Grafana 執行個體啟動時自動化設定過程。

:::note

如果您使用 [create-plugin 工具](../../../plugin-tools) 來建立您的插件，則佈建設定已包含在您的專案中。

:::

若要設定插件，請遵循以下步驟：

1.  **建立佈建檔案**：在您的插件根目錄中，您應該有一個 `provisioning` 資料夾。在此資料夾內，您可以新增 YAML 檔案來設定您正在測試的插件所需的資料來源、儀表板和其他資源。有關佈建檔案的範例，請參閱 [grafana-plugin-examples 儲存庫](https://github.com/grafana/grafana-plugin-examples/tree/main/examples/app-basic/provisioning/plugins)。有關佈建的更多資訊，請參閱 [Grafana 佈建文件](https://grafana.com/docs/grafana/latest/administration/provisioning/)。

2.  **掛載佈建資料夾**：確保您的 `docker-compose.yml` 檔案將 `provisioning` 資料夾掛載為 `/etc/grafana/provisioning` 的磁碟區：

    ```yaml
    volumes:
      - ./provisioning:/etc/grafana/provisioning
    ```

### 處理後端相依性

如果您正在測試的插件具有後端元件，則可能需要額外的設定。在這種情況下，我們建議您與插件的作者聯繫。他們或許可以提供一個測試環境或測試資料，您可以在您的佈建檔案中使用。

透過遵循這些步驟，您可以建立一個穩健的本地開發環境，用於開發和測試與其他插件互動的 UI 擴充功能。

## 測試您的 UI 擴充功能

設定好您的本地開發環境後，請考慮編寫端對端 (e2e) 測試來驗證您的 UI 擴充功能是否正常運作。E2e 測試有助於確保您的擴充功能與其他插件正確整合，並提供一種可靠的方式來捕捉迴歸。有關 e2e 測試的更多資訊，請參閱 [e2e 測試文件](../../e2e-test-a-plugin/)。
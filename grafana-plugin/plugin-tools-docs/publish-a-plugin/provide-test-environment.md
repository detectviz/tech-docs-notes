---
id: provide-test-environment
title: 提供測試環境
description: 如何為您的插件新增佈建以加快您的插件審核流程。
keywords:
  - github
  - provisioning
  - provision
  - review
sidebar_position: 5
---

# 提供測試環境

:::note

佈建並非插件提交流程的必要部分，但會加快審核速度。

:::

開發人員經常問我們，審核插件以發布到 Grafana [插件目錄](https://grafana.com/plugins)需要多長時間。雖然我們[無法提供預估時間](/publish-a-plugin/publish-a-plugin#how-long-does-it-take-to-review-a-submission)，但有辦法縮短週期時間。

到目前為止，插件審核中最耗時的方面是建立一個合適的測試環境，以便我們驗證其行為。此步驟通常涉及插件開發人員和審核團隊之間的多次來回對話。

為了縮短審核時間，請為您的插件新增[_佈建_](https://grafana.com/docs/grafana/latest/administration/provisioning/#provision-grafana)。佈建是指在插件的 [Docker 開發環境](/set-up/)中準備和設定測試環境的過程。

## 為何要佈建測試環境？

佈建有幾個好處：

- **審核時間大幅縮短。** 如果您在提交前佈建您的插件，您的審核等待時間將會短得多。
- **更容易貢獻。** 透過提供一個開箱即用的工作範例，潛在的插件貢獻者可以輕鬆地對插件進行新增實驗並提出拉取請求。
- **更容易設定端對端測試。** 透過佈建儀表板，端對端測試可以在本地開發和 CI 中針對特定情境執行。
- **提高清晰度。** 我們發現，佈建的插件讓精通技術的使用者更容易了解插件的運作方式。

## 如何提供測試環境

您可以將 Grafana 設定為透過一種稱為[佈建](https://grafana.com/docs/grafana/latest/administration/provisioning/#provision-grafana)的機制來安裝和啟用資源，其中資源是在 `/provisioning` 目錄下的 YAML 檔案中設定的。

從 v2.8.0 開始，`create-plugin` 會為所有插件類型（應用程式、場景應用程式、資料來源、面板）產生佈建功能，並包含一個範例儀表板。

### 您需要做什麼？

若要佈建，請遵循以下步驟：

1. 執行 `create-plugin` 工具以根據所選的插件類型產生包含其他檔案的 `provisioning` 資料夾。
2. 當您執行 Docker 開發環境時，這些檔案會被用來自動安裝（並在適用時_啟用_）您的插件和一個範例儀表板。

注意事項：

- 使用並更新範例儀表板，以作為您開發過程的一部分持續驗證行為。如果適用，請設定您的插件以便它可以傳回資料。

- 如果您是使用舊版 `create-plugin` 建立您的插件，您可以執行一個新指令來新增遺失的佈建檔案。
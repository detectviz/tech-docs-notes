---
id: debug-logs
title: 使用日誌偵錯您的擴充點
sidebar_label: 偵錯您的擴充點
description: 使用日誌偵錯您的擴充點。
keywords:
  - grafana
  - plugins
  - plugin
  - extensions
  - ui-extensions
  - debug
  - logs
  - troubleshooting
sidebar_position: 90
---

:::info
此功能僅在以[開發模式](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/#app_mode)執行 Grafana 時可用。
:::

「擴充功能」日誌檢視是一個管理頁面，它會顯示 Grafana 在您開發擴充點時收集的所有日誌。

若要存取它，請在開發模式下前往 **Grafana > 管理 > 插件和資料 > 擴充功能**，以查看您瀏覽器中所有作用中分頁的日誌。這樣一來，您就可以輕鬆地在一個瀏覽器或分頁中開啟擴充功能日誌檢視，並在另一個分頁中偵錯您的擴充功能。

![在開發擴充功能時使用日誌偵錯。](/img/extension-debug.gif)
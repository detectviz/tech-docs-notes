---
id: e2e-test-a-plugin
title: 對您的插件進行端對端測試
description: 如何為 Grafana 插件執行端對端測試。
keywords:
  - grafana
  - plugins
  - plugin
  - test
  - e2e
  - end-to-end
---

您可以使用 [`@grafana/plugin-e2e`](https://www.npmjs.com/package/@grafana/plugin-e2e?activeTab=readme) 來測試您的插件，這是一個專為 Grafana 插件開發人員設計的工具。它擴充了 [`@playwright/test`](https://playwright.dev/) 的功能，提供了相關的 fixtures、模型和 expect 匹配器；從而能夠在多個 Grafana 版本上對 Grafana 插件進行全面的端對端測試。此套件簡化了測試流程，確保您的插件在各種 Grafana 環境中都能穩健且相容。

## 為何使用 `@grafana/plugin-e2e`？

作為插件作者，您希望您的插件能與一系列 Grafana 版本相容。這可能具有挑戰性，因為環境、API 和 UI 元件等因素在不同的 Grafana 版本之間可能有所不同。因此，手動在多個 Grafana 版本上測試插件是一個繁瑣的過程，所以在大多數情況下，端對端測試提供了更好的解決方案。

`@grafana/plugin-e2e` 工具提供了一種與 Grafana UI 互動的一致方式，無需在插件測試程式碼中處理 UI 的偏差。`@grafana/plugin-e2e` 的 API 保證能與自 8.5.0 以來的所有最新次要版本的 Grafana 搭配使用。除了跨版本的相容性外，該工具還提供了一系列簡化端對端測試體驗的功能：

- **預定義的 fixtures：** 提供了一組專為 Grafana 插件測試量身打造的預定義 fixtures。
- **自訂模型：** 提供代表 Grafana 中頁面和元件的自訂模型，簡化了維護並建立了可重複使用的程式碼以避免重複。
- **Expect 匹配器：** 包含一系列專為 Grafana 插件斷言而設的 expect 匹配器，幫助您更有效地驗證插件行為。
- **與 Playwright 整合：** 與 Playwright 測試框架無縫整合，利用其強大的瀏覽器自動化功能。

## 端對端測試指南

請參閱以下指南以了解更多有關端對端測試的資訊：

<DocLinkList />
---
id: npm-dependencies
title: NPM 相依性
description: 了解 Grafana 插件中的前端 NPM 相依性。
keywords:
  - grafana
  - plugins
  - plugin
  - github
  - npm
  - dependencies
sidebar_position: 50
---

# Grafana 插件中的前端 NPM 相依性

Grafana 中的插件前端元件有其自己獨特的相依性，以及在執行階段與 Grafana 應用程式共用的相依性。本文件著重於如何處理這些共用相依性，特別是 `@grafana` npm 套件。

重要的是要了解，雖然插件在其 `package.json` 檔案中指定了這些相依性的預期版本，但它們在執行階段會動態連結至 Grafana 版本。

## 動態相依性連結

插件的 `package.json` 可能會參考特定版本的 `@grafana` npm 套件，例如 `@grafana/ui: 9.5.1`。在開發環境中（例如開發人員的 IDE 或執行單元測試時），將會使用此版本的 `@grafana/ui`。

但是，當插件安裝並在 Grafana 執行個體中執行時，它會繼承 Grafana 應用程式正在使用的 `@grafana` 套件的版本。例如，如果 Grafana 版本為 10.0.0，則插件會使用 Grafana 應用程式中版本為 10.0.0 的共用 `@grafana` 相依性。

:::info

此動態相依性連結也適用於 create-plugin 提供的 [docker 開發環境](/set-up/)。當插件在 Grafana 內部執行時，它將繼承 Grafana 應用程式中的 `@grafana` 相依性版本。

:::

## 相依性共用機制

為了促進此動態相依性連結，Grafana 使用 SystemJS 來載入插件的前端程式碼，並與插件共用 Grafana 應用程式的某些 NPM 相依性。

Grafana 出於以下兩個原因之一決定共用相依性：

- **單例相依性要求：** 在某些情況下，執行階段只能存在單一的相依性執行個體。
- **效能最佳化：** 共用相依性可以提高效能，尤其是在處理大型相依性程式碼庫時。

## 共用相依性的要求

若要共用相依性，Grafana 定義了兩個關鍵元件：

- **Grafana 中的 [SystemJS](https://github.com/systemjs/systemjs) 匯入對應：** 相依性必須列在 Grafana 應用程式的 SystemJS 匯入對應中。
- **插件建置工具設定：** 相依性必須在插件的建置工具設定中外部化，這主要是使用 Webpack 完成的。

:::danger

不支援自訂建置工具設定以變更外部相依性，這很可能會導致插件載入失敗或錯誤。

:::

## 編譯和執行階段

當 Grafana 應用程式在前端載入時，SystemJS 會註冊在匯入對應中找到的所有共用相依性。編譯前端程式碼時，Grafana 會確保外部化的相依性存在於插件執行階段環境的範圍內。

當使用者導覽至需要特定插件的 Grafana 頁面時，會發生以下步驟：

1. [SystemJS](https://github.com/systemjs/systemjs) _延遲載入_插件的 `module.js` 檔案。
2. SystemJS 實例化 `module.js` 檔案中的程式碼，確保在執行程式碼之前將任何共用相依性與插件中的外部相依性參考連結起來。

此過程可讓 Grafana 有效率地管理和共用各種插件之間的相依性，同時確保在執行階段使用正確且相容的共用相依性版本。
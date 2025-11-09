---
id: plugin-internationalization
title: 翻譯您的插件
description: 翻譯您的插件
keywords:
  - grafana
  - plugins
  - plugin
  - frontend
  - tooling
  - configuration
  - translation
  - localization
  - internationalization
  - webpack
---

import PluginInternationalizationShared from '@shared/plugin-internationalization-shared.md';

預設情況下，插件僅提供英文版本，當您在 [Grafana UI](https://grafana.com/docs/grafana/latest/administration/organization-preferences/#change-grafana-language) 中變更語言設定時，插件不會被翻譯。

如果您希望您的插件可以被翻譯成其他語言，您需要執行本文件中描述的變更。您可以在 GitHub 上找到[可用語言列表](https://github.com/grafana/grafana/blob/main/packages/grafana-i18n/src/constants.ts)。

:::note
雖然此範例基於面板插件，但資料來源和應用程式插件的流程是相同的。
:::

## 開始之前

:::info
翻譯功能自 Grafana 12.1.0 起提供。如果您使用的是 Grafana 11.0.0 及更新版本，請遵循[在 Grafana 12.1.0 之前翻譯您的插件](plugin-internationalization-grafana-11.md)中的步驟。如果您使用的是較舊版本的 Grafana，該插件將無法運作。
:::

建議具備以下條件：

- Grafana 插件開發的基礎知識
- 對 [`t` 函式](https://www.i18next.com/overview/api#t)的基本理解
- 對 [`Trans` 元件](https://react.i18next.com/latest/trans-component)的基本理解

## 受翻譯影響的檔案概觀

如果您使用 `create-plugin` 鷹架工具建立您的插件，啟用插件翻譯涉及更新以下檔案：

- `docker-compose.yaml`
- `plugin.json`
- `module.ts`
- `eslint.config.mjs`
- `package.json`

在翻譯過程結束時，您將擁有如下的檔案結構：

```
myorg-myplugin-plugintype/
├── src/
│   ├── locales
│   │  ├── en-US
│   │  │  └── myorg-myplugin-plugintype.json
│   │  └── es-ES
│   │     └── myorg-myplugin-plugintype.json
│   ├── module.ts
│   └── plugin.json
├── tests/
├── docker-compose.yaml
├── eslint.config.mjs
└── package.json
```

## 為您的插件設定翻譯

請遵循以下步驟更新您的插件並為其設定翻譯。

### 在您的 Grafana 執行個體中啟用翻譯（僅限 12.1.0）

若要翻譯您的插件，您需要在您的 Grafana 執行個體中[啟用功能開關](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/feature-toggles/) `localizationForPlugins`。

為此，請使用功能開關 `localizationForPlugins` 更新您插件中的 `docker-compose.yaml`：

```yaml title="docker-compose.yaml"
services:
  grafana:
    extends:
      file: .config/docker-compose-base.yaml
      service: grafana
    environment:
      GF_FEATURE_TOGGLES_ENABLE: localizationForPlugins
```

### 定義語言和 Grafana 相依性

為您的插件設定翻譯語言和 Grafana 的翻譯相依性。

為此，請在 `plugin.json` 檔案中新增相關的 `grafanaDependency` 和您要翻譯的 `languages`。例如，如果您想新增英文（美國）和西班牙文（西班牙）：

```json title="plugin.json"
"dependencies": {
    "grafanaDependency": ">=12.1.0", // @grafana/i18n 從 11.0.0 及更高版本開始運作
    "plugins": []
  },
"languages": ["en-US", "es-ES"] // 插件支援的語言
```

### 更新至最新版的 `create-plugin`

使用以下指令將您的 `create-plugin` 設定更新至最新版本：

```shell npm2yarn
npx @grafana/create-plugin@latest update
```

### 在 `module.ts` 中初始化翻譯

將插件翻譯新增至 `module.ts`：

```ts title="module.ts"
import { initPluginTranslations } from '@grafana/i18n';
import pluginJson from 'plugin.json';

await initPluginTranslations(pluginJson.id);
```

<PluginInternationalizationShared />
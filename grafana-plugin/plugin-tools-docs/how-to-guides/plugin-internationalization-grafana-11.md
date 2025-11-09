---
id: plugin-internationalization-grafana-11
title: 在 Grafana 12.1.0 之前翻譯您的插件
description: 在 Grafana 12.1.0 之前翻譯您的插件
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
這適用於需要支援 Grafana 版本 >= 11.0.0 並使用翻譯的插件。如果您的插件只需要支援 Grafana 12.1.0 及更新版本，則請改為遵循[翻譯您的插件](plugin-internationalization)中的步驟。如果您使用的是較舊版本的 Grafana，該插件將無法運作。
:::

## 受翻譯影響的檔案概觀

如果您使用 `create-plugin` 鷹架工具建立您的插件，啟用插件翻譯涉及更新以下檔案：

- `docker-compose.yaml`
- `plugin.json`
- `module.ts`
- `loadResources.ts`
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
│   ├── loadResources.ts
│   └── plugin.json
├── tests/
├── docker-compose.yaml
├── eslint.config.mjs
└── package.json
```

## 為您的插件設定翻譯

請遵循以下步驟更新您的插件並為其設定翻譯。

### 將 Grafana 11.0.0 設為您的預設映像檔版本

為此，請使用正確的 `grafana_version` 更新您插件中的 `docker-compose.yaml`：

```yaml title="docker-compose.yaml"
services:
  grafana:
    extends:
      file: .config/docker-compose-base.yaml
      service: grafana
    build:
      args:
        grafana_version: ${GRAFANA_VERSION:-11.0.0}
```

### 定義語言和 Grafana 相依性

為您的插件設定翻譯語言和 Grafana 的翻譯相依性。

為此，請在 `plugin.json` 檔案中新增相關的 `grafanaDependency` 和您要翻譯的 `languages`。例如，如果您想新增英文（美國）和西班牙文（西班牙）：

```json title="plugin.json"
"dependencies": {
    "grafanaDependency": ">=11.0.0",
    "plugins": []
  },
"languages": ["en-US", "es-ES"] // 插件支援的語言
```

### 更新至最新版的 `create-plugin`

使用以下指令將您的 `create-plugin` 設定更新至最新版本：

```shell npm2yarn
npx @grafana/create-plugin@latest update
```

### 將 `semver` 更新為一般相依性

更新 semver 套件以啟用基於版本的行為切換：

```shell npm2yarn
npm uninstall semver
npm install --save semver
npm install --save-dev @types/semver
```

### 新增 `loadResources` 檔案

為了處理翻譯資源的載入，讓我們新增 `src/loadResources.ts`

```ts title="src/loadResources.ts"
import { LANGUAGES, ResourceLoader, Resources } from '@grafana/i18n';
import pluginJson from 'plugin.json';

const resources = LANGUAGES.reduce<Record<string, () => Promise<{ default: Resources }>>>((acc, lang) => {
  acc[lang.code] = async () => await import(`./locales/${lang.code}/${pluginJson.id}.json`);
  return acc;
}, {});

export const loadResources: ResourceLoader = async (resolvedLanguage: string) => {
  try {
    const translation = await resources[resolvedLanguage]();
    return translation.default;
  } catch (error) {
    // 這可確保當 Grafana 中解析的語言不受插件支援時，插件不會崩潰
    console.error(`The plugin '${pluginJson.id}' doesn't support the language '${resolvedLanguage}'`, error);
    return {};
  }
};
```

### 在 `module.ts` 中初始化翻譯

將插件翻譯和載入器邏輯新增至 `module.ts`：

```ts title="module.ts"
import { initPluginTranslations } from '@grafana/i18n';
import pluginJson from 'plugin.json';
import { config } from '@grafana/runtime';
import semver from 'semver';
import { loadResources } from './loadResources';

// 在 Grafana 12.1.0 版之前，插件負責載入翻譯資源
// 在 Grafana 12.1.0 版及更新版本中，Grafana 負責載入翻譯資源
const loaders = semver.lt(config?.buildInfo?.version, '12.1.0') ? [loadResources] : [];

await initPluginTranslations(pluginJson.id, loaders);
```

<PluginInternationalizationShared />
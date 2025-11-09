---
id: get-started
title: 開始使用
description: 使用 create-plugin 工具開始 Grafana 插件開發。
keywords:
  - grafana
  - plugins
  - plugin
  - create-plugin
  - getting started
slug: /
---

import ScaffoldNPM from '@shared/createplugin-scaffold.md';
import InstallNPM from '@shared/createplugin-install.md';
import BuildFEDevNPM from '@shared/createplugin-build-fe-dev.md';
import BuildFEProdNPM from '@shared/createplugin-build-fe-prod.md';

# 開始使用

歡迎來到 Grafana 插件開發的世界，在這裡您可以增強 Grafana 的基礎功能。在本指南中，您將學習如何透過建立插件模板、在高效的開發環境中運行它，以及使用其基本功能來開始。

<YouTubeEmbed videoId="AARrATeVEQY" title="Getting Started with Grafana Plugin Development" />

**觀看我們的介紹影片**，了解如何開始您的第一個 Grafana 插件的逐步指南。這個視覺化的教學影片補充了下方的詳細說明，並提供了實用的見解來幫助您。

## 快速入門

只需一個指令即可建立新插件的模板！執行以下指令並回答提示：

<ScaffoldNPM />

## 為什麼要建立 Grafana 插件？

Grafana 插件開發讓您可以創建許多不同類型的使用者體驗。例如，您可以製作：

- **Panel plugins** - 視覺化資料的新方法
- **Data source plugins** - 連接到新的資料庫或其他資料來源
- **App plugins** - 開箱即用的整合體驗

:::tip

如果這是您第一次建立插件，我們建議您先熟悉插件類型、前端和後端元件、資料框架及其他基本要素的基礎知識。深入了解 [Grafana 插件開發的關鍵概念](/key-concepts/)。

:::

## Grafana 插件的簽章分類

請熟悉 Grafana 插件的簽章分類，例如私有和公有插件之間的區別。

請注意，如果您想將與商業產品相關的插件發布到官方的 Grafana 目錄中，通常需要付費訂閱。深入了解 [Grafana 的插件政策](https://grafana.com/legal/plugins/)。

## 使用插件工具加速您的插件開發

Grafana 的插件工具提供了一種官方支援的方式來擴展 Grafana 的核心功能。我們設計這些工具是為了幫助您透過現代化的建置設定，更快地開發您的插件，且無需額外設定。

插件工具包含兩個套件：

- `create-plugin`：一個用於建立新插件模板或遷移使用 `@grafana/toolkit` 建立的插件的 CLI。
- `sign-plugin`：一個用於簽署插件以供分發的 CLI。

:::info

如果您之前曾使用 `@grafana/toolkit` 建置插件，您可以使用我們的插件工具來轉換到我們最新的工具。更多資訊，請參閱[從 toolkit 遷移](/migration-guides/migrate-from-toolkit.mdx)。

:::

## 開始之前

請確保您使用的是支援的作業系統、Grafana 版本和工具。

### 支援的作業系統

Grafana 插件工具適用於以下作業系統：

- Linux
- macOS
- Windows 10+ 搭配 WSL (Windows Subsystem for Linux)

### 支援的 Grafana 版本

我們通常建議您為 Grafana v10.0 以後的版本進行建置。有關使用 Grafana 開發時的需求和依賴項的更多資訊，請參閱 [Grafana 開發人員指南](https://github.com/grafana/grafana/blob/main/contribute/developer-guide.md)。

### 必要工具

您需要設定好以下工具：

- Go ([版本](https://github.com/grafana/plugin-tools/blob/main/packages/create-plugin/templates/backend/go.mod#L3))
- [Mage](https://magefile.org/)
- [LTS](https://nodejs.dev/en/about/releases/) 版本的 Node.js
- [Docker](https://docs.docker.com/get-docker/)
- （可選）[Yarn](https://yarnpkg.com/getting-started/install) 或 [PNPM](https://pnpm.io/installation)

#### 支援的套件管理器

當您第一次執行 `@grafana/create-plugin` 時，請選擇您的套件管理器：`npm`、`pnpm` 或 `yarn`。

:::note
本網站上的 Yarn 指令與 Yarn Berry (>=2.0.0) 相容。如果您使用的是 Yarn 1.x.x，我們建議您升級到 [Yarn Berry](https://yarnpkg.com/migration/guide)。或者，您可以使用 `yarn create @grafana/plugin` 來執行 Yarn 1.x.x 的指令。
:::

## 建立插件模板

### 執行 `create-plugin` 工具

執行以下指令並回答提示：

<ScaffoldNPM />

有關提示的幫助，請參閱 [CLI 指令](./reference/cli-commands.mdx)。

### 開啟產生的資料夾結構

開啟插件資料夾以瀏覽產生的插件：

目錄名稱 `<orgName>-<pluginName>-<pluginType>` 是根據您對提示的回答而定的。當出現提示時，請使用產生的資料夾名稱。此目錄包含啟動您插件開發的初始專案結構。

檔案結構應如下所示：

```
<orgName>-<pluginName>-<pluginType>
├── .config/
├── .eslintrc
├── .github
│   └── workflows
├── .gitignore
├── .nvmrc
├── .prettierrc.js
├── CHANGELOG.md
├── LICENSE
├── Magefile.go
├── README.md
├── cypress
│   └── integration
├── docker-compose.yaml
├── go.mod
├── go.sum
├── jest-setup.js
├── jest.config.js
├── node_modules
├── package.json
├── pkg
│   ├── main.go
│   └── plugin
├── src
│   ├── README.md
│   ├── components
│   ├── datasource.ts
│   ├── img
│   ├── module.ts
│   ├── plugin.json
│   └── types.ts
└── tsconfig.json
```

有關這些檔案的更多資訊，請參閱[插件的結構](/key-concepts/anatomy-of-a-plugin/)。

## 在 Docker 中建置並執行您的插件

使用 `create-plugin` 工具，您可以利用 Docker 容器來簡化設定、載入和開發流程。更多資訊，請參閱[設定開發環境](/set-up/)。

請參考[建立新插件模板](#scaffold-a-plugin)後終端機輸出的「後續步驟」，以安裝依賴項、建置並執行您的插件。

輸出範例：
```
## What's next?

Run the following commands to get started:

    * cd ./orgName-pluginName-app
    * npm install to install frontend dependencies.
    * npm exec playwright install chromium to install e2e test dependencies.
    * npm run dev to build (and watch) the plugin frontend code.
    * mage -v build:backend to build the plugin backend code. Rerun this command every time you edit your backend files.
    * docker compose up to start a grafana development server.
    * Open http://localhost:3000 in your browser to create a dashboard to begin developing your plugin.

Note: We strongly recommend creating a new Git repository by running git init in ./org-pluginname-app before continuing.

    * Learn more about Grafana Plugin Development at https://grafana.com/developers/plugin-tools

```

### 安裝依賴項

<InstallNPM />

### 建置前端

要在開發時以觀察模式建置插件並持續監控變更，請執行：

<BuildFEDevNPM />

要為生產環境建置，請執行：

<BuildFEProdNPM />

### 建置後端

如果您的插件包含[後端](./key-concepts/backend-plugins/index.md)元件，您可以使用 mage 進行建置：

```shell
mage -v build:linux
```

#### 建置目標

| 選項 | 描述 | 範例 |
| --- | --- | --- |
| `build:[arch]` | 為特定架構建置二進位檔。 | `mage -v build:Linux` |

列出所有可用的 Mage 目標以獲取更多指令：

```bash
mage -l
```

### 執行 Grafana 伺服器

要使用 Docker 啟動 Grafana 開發伺服器，請執行：

```shell
docker compose up --build
```

恭喜！您剛剛建立了您的第一個插件模板，現在可以透過 [http://localhost:3000](http://localhost:3000) 存取它。

## 後續步驟

- 透過我們的[插件開發教學](/tutorials/)開始您的插件之旅。
- 學習如何[擴展](/how-to-guides)其功能。
- 檢閱[插件範例](https://github.com/grafana/grafana-plugin-examples)以了解良好實踐。
- 學習如何[打包](/publish-a-plugin/package-a-plugin)、[簽署](/publish-a-plugin/sign-a-plugin)和[發布](/publish-a-plugin/publish-or-update-a-plugin.md)您的插件到 Grafana [插件目錄](https://grafana.com/plugins)。
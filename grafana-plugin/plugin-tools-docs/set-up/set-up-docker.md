---
id: set-up-docker
title: 設定您的 Docker 開發環境
sidebar_label: 設定您的 Docker 環境
description: 為 Grafana 插件開發設定 Docker。
keywords:
  - grafana
  - plugins
  - plugin
  - create-plugin
  - Docker
  - setup
  - CI
  - continuous integration
  - automation
  - configuration
sidebar_position: 20
---

import DockerNPM from '@shared/docker-grafana-version.md';

[`create-plugin` 工具](/get-started.md#use-plugin-tools-to-develop-your-plugins-faster)包含一個以 [Docker](https://docs.docker.com/get-docker/) 為特色的開發環境。它可讓您啟動一個供 Grafana 插件開發人員使用的 Grafana 應用程式執行個體，您可以在其上進行編碼。

:::info

在開發期間無需[簽署插件](/publish-a-plugin/sign-a-plugin.md)。使用 `@grafana/create-plugin` 建立的 Docker 開發環境預設設定為在[開發模式](https://github.com/grafana/grafana/blob/main/contribute/developer-guide.md#configure-grafana-for-development)下執行，這可讓您在沒有簽章的情況下載入插件。

:::

## 為何使用 Docker 環境

我們選擇使用 Docker 是因為它簡化了建立、部署和執行應用程式的過程。它可讓您為您的插件建立一致且隔離的環境。這使得管理相依性並確保插件在不同機器上以相同方式執行變得容易。

使用 `create-plugin` 工具，Docker 容器會設定必要的變數，以允許輕鬆存取 Grafana 並載入插件，而無需簽署它們。插件工具還新增了一個即時重載功能，可讓您對前端程式碼進行變更以觸發瀏覽器中的重新整理，而變更後端程式碼將使插件二進位檔自動重新載入。

docker 環境還允許您將偵錯器附加到插件後端程式碼，從而使開發過程更容易。

## 開始使用 Docker

若要開始您的插件開發專案，請按所列順序執行以下指令：

1. <SyncCommand cmd="install" />：安裝前端相依性。
2. <SyncCommand cmd="run dev" />：建置並監看插件前端程式碼。
3. `mage -v build:linux`：建置插件後端程式碼。每次編輯後端檔案時，請重新執行此指令。
4. <SyncCommand cmd="run server" />：啟動一個在
   [http://localhost:3000](http://localhost:3000) 上執行的 Grafana 開發伺服器。

## 設定 Grafana 版本

若要在不同版本的 Grafana 中測試插件，請設定一個環境變數。使用 `GRAFANA_VERSION` 來設定 Grafana 版本：

<DockerNPM />

## 設定 Grafana 映像檔

插件工具中的預設 Docker 映像檔是 `grafana-enterprise`。如果您想覆寫此映像檔，請透過變更 `grafana_image` 建置引數來修改 `docker-compose.yaml`，如下所示：

```yaml title="docker-compose.yaml"
version: '3.7'

services:
  grafana:
    extends:
      file: .config/docker-compose-base.yaml
      service: grafana
    build:
      args:
        grafana_version: ${GRAFANA_VERSION:-9.1.2}
        grafana_image: ${GRAFANA_IMAGE:-grafana}
```

此範例將環境變數 `GRAFANA_IMAGE` 指派給建置引數 `grafana_image`，預設值為 `grafana`。這讓您可以選擇在執行 `docker compose` 指令時設定該值。

## 偵錯插件的後端

如果您正在開發具有後端部分的插件，請使用 `DEVELOPMENT=true` 執行 `npm run server`。docker compose 檔案會公開用於偵錯的連接埠 `2345`，來自將在 docker 環境中執行的無頭 delve 執行個體。
您可以將偵錯器用戶端附加到此連接埠以偵錯您的後端程式碼。
例如，在 VSCode 中，您可以新增一個 `launch.json` 設定，如下所示：

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Attach to plugin backend in docker",
      "type": "go",
      "request": "attach",
      "mode": "remote",
      "port": 2345,
      "host": "127.0.0.1",
      "showLog": true,
      "trace": "log",
      "logOutput": "rpc",
      "substitutePath": [
        {
          "from": "${workspaceFolder}",
          "to": "/root/<your-plugin-id>"
        }
      ]
    }
  ]
}
```
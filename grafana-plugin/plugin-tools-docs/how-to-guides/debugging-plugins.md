---
id: debugging-plugins
title: 使用 React 工具偵錯插件
description: 如何偵錯您基於 React 的 Grafana 插件。
keywords:
  - grafana
  - plugins
  - plugin
  - debugging
---

# 使用 React 工具偵錯您的插件

使用 [React Tools 偵錯器](https://react.dev/learn/react-developer-tools)來開發新的 Grafana 插件和對現有插件進行疑難排解。

## 設定 Grafana 以進行效能分析和偵錯

使用 Grafana 的偵錯版本可讓您輕鬆地逐步執行未經最小化的程式碼，並在 React 開發人員工具中使用 `Profiler`。

預設情況下，Grafana Docker 映像檔不包含 React 偵錯版本。若要使用偵錯版本，請使用在 [hub.docker.com](https://hub.docker.com/) 上找到的對應版本更新您的 `docker-compose.yml`。如果您的插件支援較舊的（受支援的）版本，請使用搜尋選項找到標籤。

### 設定您的 Grafana OSS Docker 版本

* 對於 Grafana OSS 11.5 版或更新版本，請參閱 [v11.5+ OSS](https://hub.docker.com/repository/docker/grafana/grafana-oss-dev/tags?name=11.5)。

* 對於 Grafana OSS 12.1 版或更新版本，請參閱 [v12.1+ OSS](https://hub.docker.com/repository/docker/grafana/grafana-oss-dev/tags?name=12.1)。

例如：

```YAML
services:
  grafana:
    image: grafana/grafana-oss-dev:12.1.0-255911
    ...
```

或者，您可以使用環境變數而無需修改現有的 `docker-compose.yml` 檔案：

```SHELL
export GRAFANA_IMAGE=grafana-oss-dev
export GRAFANA_VERSION=12.1.0-255911
```

### 設定您的 Grafana Enterprise Docker 版本

由於 Grafana Enterprise 支援額外的 API 呼叫，因此當您的插件依賴 Enterprise 功能時，請使用此類型的映像檔。

* 對於 Grafana Enterprise 11.5 版或更新版本，請參閱 [v11.5+ Enterprise](https://hub.docker.com/repository/docker/grafana/grafana-enterprise-dev/tags?name=11.5)。

* 對於 Grafana Enterprise 12.1 版或更新版本，請參閱 [v12.1+ Enterprise](https://hub.docker.com/repository/docker/grafana/grafana-enterprise-dev/tags?name=12.1)。

例如：

```YAML
services:
  grafana:
    image: grafana/grafana-enterprise-dev:12.1.0-92854
    ...
```

或者，您可以使用環境變數而無需修改現有的 `docker-compose.yml` 檔案：

```SHELL
export GRAFANA_IMAGE=grafana-enterprise-dev
export GRAFANA_VERSION=12.1.0-92854
```

## 使用 React Tools

若要使用 React 開發工具來偵錯 Grafana，請遵循以下步驟：

1. 啟動您的 Docker 環境。
2. 開啟 Chrome 或 Firefox 並導覽至您的 Grafana 執行個體。
  - 在 Chrome 中，選取 **View > Developer > Developer Tools**。
  - 在 Firefox 中，選取 **Tools > Browser Tools > Web Developer Tools**。請注意，Firefox 需要安裝 React Developer Tools 擴充功能。

您現在可以在偵錯工具中使用 **Profiler** 標籤，它提供了 Flamegraph、Ranked 和 Timeline 選項。
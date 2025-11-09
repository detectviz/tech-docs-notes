---
id: plugin-protocol
title: 插件協定
description: 了解用於插件開發的 Grafana 插件協定。
keywords:
  - grafana
  - plugins
  - plugin
  - backend
  - plugin protocol
  - protocol
  - protobufs
  - protocal buffers
sidebar_position: 3
---

# 插件後端通訊協定

Grafana 伺服器使用有線通訊協定在 Grafana 和插件的後端之間建立合約，以允許它們彼此通訊。插件協定位於 [Grafana Plugin SDK for Go](grafana-plugin-sdk-for-go.md) 中，因為 Grafana 本身使用 SDK 的一部分作為相依性。

## 使用插件協定進行開發

:::note

請勿直接針對協定開發您的插件後端。請改用[Grafana Plugin SDK for Go](grafana-plugin-sdk-for-go)，它實作了此協定並提供更高階的 API。

:::

如果您選擇直接針對插件協定進行開發，您可以使用 [Protocol Buffers](https://developers.google.com/protocol-buffers)（即 protobufs）搭配 [gRPC](https://grpc.io/) 來進行。Grafana 的插件協定 protobufs 可在 [GitHub 儲存庫](https://github.com/grafana/grafana-plugin-sdk-go/blob/master/proto/backend.proto)中取得。

## 版本控制

Grafana 會不時在最新版本的插件協定中提供服務、訊息和欄位的附加功能。我們不預期這些更新會引入任何破壞性變更。但是，如果我們必須對插件協定引入破壞性變更，我們將會建立一個新的主要版本的插件協定。

Grafana 將會隨著新的主要 Grafana 版本發布新的主要版本的插件協定。屆時，我們將在一段時間內同時支援舊的和新的插件協定，以確保現有的插件後端元件繼續運作。

插件協定試圖遵循 Grafana 的版本控制。但是，這並不表示當 Grafana 發布新的主要版本時，我們會自動建立一個新的主要版本的插件協定。
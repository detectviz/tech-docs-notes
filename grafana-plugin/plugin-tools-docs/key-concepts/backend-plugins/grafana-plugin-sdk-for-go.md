---
id: grafana-plugin-sdk-for-go
title: Grafana plugin SDK for Go
description: 了解用於 Go 開發的 Grafana 插件 SDK。
keywords:
  - grafana
  - plugins
  - plugin
  - backend
  - SDK
  - Go
sidebar_position: 2
---

# Grafana plugin SDK for Go

[Grafana plugin SDK for Go](https://pkg.go.dev/mod/github.com/grafana/grafana-plugin-sdk-go?tab=overview) 是一個 [Go](https://golang.org/) 模組，提供了一組[套件](https://pkg.go.dev/mod/github.com/grafana/grafana-plugin-sdk-go?tab=packages)，您可以使用它們來實作插件後端。

該插件 SDK 提供了一個高階框架，包含 API、公用程式和工具。透過使用 SDK，您可以避免學習[插件協定](./plugin-protocol.md)和 RPC 通訊協定的詳細資訊，因此您無需管理任何一個。

## 版本控制

Grafana plugin Go SDK 仍處於開發階段。它基於[插件協定](./plugin-protocol.md)，該協定是單獨版本控制的，並且被認為是穩定的。但是，我們有時可能會在 SDK 中引入破壞性變更。

當我們更新插件 SDK 時，使用較舊版本 SDK 的插件應該仍然可以與 Grafana 搭配使用。但是，這些較舊的插件可能無法使用我們在更新的 SDK 版本中引入的新功能和能力。

## 更新 Go SDK

若要讓 Grafana plugin SDK for Go 保持最新狀態：

```bash
go get -u github.com/grafana/grafana-plugin-sdk-go
go mod tidy
```

## 另請參閱

- [SDK 原始碼](https://github.com/grafana/grafana-plugin-sdk-go)
- [Go 參考文件](https://pkg.go.dev/github.com/grafana/grafana-plugin-sdk-go)
---
id: targeting-older-releases
title: 針對較舊版本
sidebar_position: 8
description: 如何為插件針對較舊版本。
keywords:
  - grafana
  - plugins
  - plugin
  - React
  - ReactJS
  - Angular
  - migration
  - targeting
---

# Angular 至 React：針對較舊版本

Angular 插件通常擁有龐大的安裝基礎和執行較舊版本 Grafana 的使用者。

本文件提供如何針對較舊（甚至不受支援）版本的一般性指導，讓使用者可以視需要遷移至較新版本的 Grafana。

最可靠的最低版本是 `8.4.11` 和 `9.3.16`，取決於使用了多少 UI 元件。

使用 `create-plugin` 公用程式時，無論是更新現有插件還是進行遷移，套件清單通常都會包含最新版本的 Grafana。若要為較舊版本的 Grafana 提供插件，只需將 `dependencies` 和 `devDependencies` 設定為符合較舊版本即可。

## 針對 v8.4

polystat 面板能夠針對 v8.4.11，因為它使用的 UI 元件非常少。它還使用最新的 plugin-e2e 套件，以及最新的 plugin-tools 設定。

```json
"dependencies": {
    "@grafana/data": "8.4.11",
    "@grafana/runtime": "8.4.11",
    "@grafana/schema": "10.3.3",
    "@grafana/ui": "8.4.11",
    "react": "17.0.2",
    "react-dom": "17.0.2",
    "react-redux": "7.2.6",
    ...
}
```

更新 `src/plugin.json` 檔案以對應相同的版本：

```json
    "dependencies": {
        "grafanaVersion": "8.4.x",
        "grafanaDependency": ">=8.4.11"
    }
```

## 針對 v9.3

由於在先前版本中不可用的資料轉換，D3 Gauge 面板必須至少針對 v9.3.16。它還使用最新的 plugin-tools 設定，並搭配 git 工作流程和 webpack 設定。

```json
"dependencies": {
    "@grafana/data": "9.3.16",
    "@grafana/runtime": "9.3.16",
    "@grafana/ui": "9.3.16",
    "react": "17.0.2",
    "react-dom": "17.0.2",
    ...
}
```

更新 `src/plugin.json` 檔案以對應相同的版本：

```json
    "dependencies": {
        "grafanaVersion": "9.3.x",
        "grafanaDependency": ">=9.3.16"
    }
```

## 特別注意事項

請務必使用最低版本和目前版本之間的所有版本來測試插件，以確保不會發生崩潰。

## 其他資源

這些面板針對較舊的 Grafana 版本，並能與最新版本正常運作。

[Polystat Panel](https://github.com/grafana/grafana-polystat-panel/blob/main/package.json)

[D3 Gauge Panel](https://github.com/briangann/grafana-gauge-panel/blob/main/package.json)
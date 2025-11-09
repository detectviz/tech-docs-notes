---
id: custom-config-components
title: 自訂設定元件
sidebar_position: 4
description: 如何將具有自訂欄位的 Grafana 插件從 AngularJS 遷移至具有自訂元件的 React。
keywords:
  - grafana
  - plugins
  - plugin
  - React
  - ReactJS
  - Angular
  - AngularJS
  - migration
---

# AngularJS 至 React：自訂設定元件

使用本指南來學習如何將面板的自訂設定元件從 AngularJS 遷移至 React。

## 背景

Grafana SDK 為開發人員提供了許多有用的設定元件，例如輸入數字範圍、閾值、單位選擇和顏色選擇。您可以在 [@grafana/ui](https://developers.grafana.com/ui/latest/index.html) 中查看這些元素的描述和範例。

某些面板比其他面板具有更複雜的設定選項。這些面板需要您建立一個自訂元件以支援從 AngularJS 移植到 React。

## Angular 自訂設定範例

本文件以 `grafana-polystat-panel` 為例，說明一個大型複雜插件如何從 AngularJS 轉換為 React。

AngularJS 版本的 `polystat` 有一個用於[複合指標](https://en.wikipedia.org/wiki/Composite_measure)的自訂編輯器。使用複合指標可以將一組指標一起處理，以確定整體狀態。

複合指標的一個範例是具有 CPU 使用率、記憶體使用率、磁碟使用率、網路頻寬和 API 回應率查詢的伺服器。單一指標被組合以代表所有這些不同的指標，每個指標都有其獨特的閾值，並以「最差」狀態（即最低值）顯示一個多邊形。

複合編輯器包含全域選項，以及許多適用於特定複合指標的自訂選項。例如：

![AngularJS 複合指標](/img/migration-screenshots/composite-editor-angular.png)

當您展開一個複合指標 (COMPOSITE1) 時，它會顯示詳細資訊。新增一個新的複合指標會提供合理的預設值，並在左側顯示一個有序的名稱。

此面板第 1 版的原始碼可在此處查看：`Polystat Panel v1.x`。

AngularJS 複合編輯器是 [HTML](https://github.com/grafana/grafana-polystat-panel/blob/v1.2.11/src/partials/editor.composites.html) 和用於[管理 UI](https://github.com/grafana/grafana-polystat-panel/blob/v1.2.11/src/composites_manager.ts) 的程式碼的組合。例如：

![AngularJS 複合編輯器展開](/img/migration-screenshots/composite-editor-angular-expanded.png)

此 AngularJS 插件可在 Grafana 10 版中運作，但編輯器難以使用，因為它需要左右捲動才能看到所有值。此外，您必須將編輯器側邊面板展開到幾乎全尺寸才能看到所有選項。一旦停用 AngularJS，該插件將不再運作。

## React 元件和設定選項

將您的元件移植到 React 需要建立一個自訂編輯器元件，如下所示：

![React 複合編輯器](/img/migration-screenshots/composite-react-component.png)

![React 複合編輯器展開](/img/migration-screenshots/composite-component-react-expanded.png)

![React 複合編輯器新增指標](/img/migration-screenshots/composite-react-component-add-metric.png)

複合編輯器現在垂直顯示，不需要左右捲動即可看到所有選項。您也可以像篩選其餘設定項目一樣篩選編輯器。

該元件的原始碼現在是自包含的，可以在 [Grafana GitHub 儲存庫](https://github.com/grafana/grafana-polystat-panel/tree/main/src/components/composites)中查看。

新的編輯器比 AngularJS 版本有許多優點：

- 支援重新排序複合指標並為其命名以便於識別。編輯器還利用了內建的輸入欄位和驗證器，減少了支援面板所需的程式碼量。
- 允許許多複雜的設定選項，這些選項在 React 中更容易實作。例如，選項包括從規則運算式、範本變數或兩者中衍生複合指標名稱的功能。
- 包含用於閾值和覆寫的其他自訂編輯器，這些編輯器與常見的 `@grafana/ui` 模式不同。這些編輯器可以作為如何使用複雜邏輯實作這些類型的編輯器的參考。
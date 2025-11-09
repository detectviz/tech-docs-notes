---
id: migrate-angularjs-configuration-settings-to-react
title: 遷移設定
sidebar_position: 4
description: 如何將 AngularJS 插件設定遷移至 React。
keywords:
  - grafana
  - plugins
  - plugin
  - React
  - ReactJS
  - Angular
  - migration
  - migrate
---

# 遷移設定

本指南探討了在將您的 Grafana 插件從已棄用的 AngularJS 或 Angular 升級至我們目前建立插件的方法 React 時，如何轉換設定。在我們討論此過程中可能出現的各種問題時，您將看到 Grafana 遷移處理工具如何讓轉換變得更容易。

## 遷移處理常式

### 背景

面板插件有兩種插件設定遷移類型：

- AngularJS 至 React
- 插件版本更新

當您載入的面板中，其 JSON 中指定的插件版本與目前的執行階段版本不同時，就會呼叫遷移處理常式。此處理常式需要傳回一個有效的物件，且不得擲回任何錯誤。

我們強烈建議您在升級插件可能導致現有面板出現問題時使用遷移處理常式。透過使用遷移處理常式，您將提供更好的使用者體驗。

### 使用指定的區段

當您將面板插件從 AngularJS 轉換為 React 時，JSON 中有一個用於編輯器自訂選項的特定位置。以前，使用 Angular 插件時，插件可以將自訂物件儲存在設定中的任何位置，但現在這些物件必須使用指定的區段。

Grafana 預設會為其所有元件使用相同的指定區段。您新增的任何自訂元件都必須使用相同的位置。

當載入面板時，Grafana 會在向使用者顯示任何內容之前呼叫一個遷移處理常式。此呼叫允許將舊的面板設定自動轉換為正在載入的新版本。如果您不提供遷移處理常式，那麼使用者會得到所有面板的預設值，並且他們必須手動修正每個面板。

## 深入探討 Angular 至 React 的遷移

當儀表板中的面板使用較舊的 AngularJS 版本的插件，但實際執行的卻是最新版的 React 版本時，就需要進行修改。在這種情況下，請盡可能輕鬆地修改舊設定以與新插件搭配使用。理想情況下，使用者不需要重新設定他們的面板。

### 介紹 Polystat 範例

Angular 插件通常有一個 `panel.config` 物件，其中包含特定於插件的設定。例如，Polystat 面板插件，稱為 `grafana-polystat-panel`，是一個範例插件，它最初是一個 AngularJS 面板，後來被移植到 React。該插件的 React 版本在 `module.ts` 中使用了 `.setMigrationHandler`，如下所示：

```TYPESCRIPT
.setMigrationHandler(PolystatPanelMigrationHandler)
```

基於 Angular 的 Polystat 面板 (v1.x) 將大部分設定儲存在「panel.polystat」物件中。您的插件應在遷移處理常式中偵測此物件是否存在，以便您可以觸發轉換至新的基於 React 的插件設定。

React 面板將所有內容儲存在 `panel.options` 中。如果此物件不存在，則遷移處理常式至少應傳回一個有效的空物件。如果此物件存在，則它應僅傳回目前的 `panel.options`。在此階段，有機會修改 React 設定，以防較新版本已移除或新增了新功能。

:::note

`panel.options` 是一個名為 `PanelModel` 的介面，其類型是您面板插件的自訂類型。

:::

### 在 Polystat 範例中變更字型

當安裝新版本的插件時，Grafana 伺服器會呼叫遷移處理常式以新增或移除設定項目。這些變更不會保存在儀表板中，因此您必須「儲存」它們，以防止遷移在每次載入時都必須修改面板。

例如：當 Polystat 插件以 AngularJS 編寫時，它有一個硬式編碼的字型 Roboto，在較新版本的 Grafana 中已被移除。這導致當插件在較新版本的 Grafana 中執行時，輸出會不正確地呈現。為了解決此問題，Grafana 新增了一個新的選取器，讓使用者可以選擇字型，但之前的版本中沒有此設定的全域設定。

在這種情況下，遷移處理常式應偵測是否不存在該選項，然後插入一個預設值。該預設值會根據正在使用的 Grafana 版本傳回一個可運作的設定。

### 步驟 1：偵測 Grafana 的執行階段版本

您的插件可以存取 `config.buildInfo.version` 變數以確定正在執行的 Grafana 版本。遷移處理常式可以使用此值來設定有效的預設值。

多個版本的 Grafana 可能都已進行了向後移植修補，因此它們可能已移除了您的插件預期的功能。在前面的範例中，就是 Roboto 字型。

遷移處理常式會取得執行階段版本，並使用 semver 來決定要使用哪種字型。較舊的版本沒有較新的 Inter 作為字型，因此載入 Roboto 是最安全的。較新的版本已移除 Roboto，因此插件應改為載入 Inter。

這裡有兩種情況：

#### 情況 1：使用者執行選取了 Roboto 的面板

在這種情況下，使用者正在執行目前的 Grafana (9.4.3)，並且有一個選取了 Roboto 的面板。插件可以根據執行階段提供不同的選取選項。

Polystat 面板在其 `module.ts` 中有一個根據執行階段的條件式檢查：

```TYPESCRIPT
     .addSelect({
       path: 'globalTextFontFamily',
       name: 'Font Family',
       description: 'Font used for rendered text',
       category: ['Text'],
       defaultValue: GLOBAL_TEXT_FONT_FAMILY,
       settings: {
         options: FontFamilyOptions,
       },
       showIf: () => hasRobotoFont() === false,
     })
     .addSelect({
       path: 'globalTextFontFamily',
       name: 'Font Family',
       description: 'Font used for rendered text',
       category: ['Text'],
       defaultValue: GLOBAL_TEXT_FONT_FAMILY_LEGACY,
       settings: {
         options: FontFamilyOptionsLegacy,
       },
       showIf: () => hasRobotoFont() === true,
     })
```

#### 情況 2：Grafana 自動將字型切換為 Inter

在這種情況下，使用者升級 Grafana（從 v9.3.10 到 v9.4.3），遷移會自動切換為使用 Inter，並且不會在字型選取器中顯示 Roboto。如果升級是 9.4.0 或更高版本，則使用 Inter；否則，使用 Roboto。

Polystat 中的 `MigrationHandler` 包含此程式碼：

```TYPESCRIPT
import { config } from "@grafana/runtime";
import { satisfies, coerce } from "semver";

export const PolystatPanelMigrationHandler = (panel: PanelModel<PolystatOptions>): Partial<PolystatOptions> => {
  // 將預設字型設定為 inter，並檢查其是否可用，如果不可用則設定為 roboto
  options.globalTooltipsFontFamily = FontFamilies.INTER;
    if (hasRobotoFont()) {
      options.globalTooltipsFontFamily = FontFamilies.ROBOTO;
    }
}

export const hasRobotoFont = () => {
  const version = coerce(config.buildInfo.version);
  if (version !== null) {
    if (satisfies(version, "<9.4.0")) {
      return true;
    }
  }
  return false;
};
```

### 步驟 2：偵測遺失的設定

新版本的插件可能會新增面板未定義的新設定選項。遷移處理常式可用於新增具有「安全」預設值的新選項。

```TYPESCRIPT
options.globalTooltipsFontFamily = FontFamilies.INTER;
  if (hasRobotoFont()) {
    options.globalTooltipsFontFamily = FontFamilies.ROBOTO;
  }

export const PolystatPanelMigrationHandler = (panel: PanelModel<PolystatOptions>): Partial<PolystatOptions> => {
  if (panel.options.NewFeature === undefined) {
    // 為新功能新增預設值
    panel.options.NewFeature = 5.0;
  }
  ...
  return panel.options;
}
```

### 步驟 3：偵測無效的設定

由於插件正在載入並接收整個設定，因此可以迭代設定並確保值是合法的。

```TYPESCRIPT
export const PolystatPanelMigrationHandler = (panel: PanelModel<PolystatOptions>): Partial<PolystatOptions> => {
  // 迭代並驗證

  let validConfigOptions = {
    fontSize: 2,
    fontFamily: 3,
    defaultInvalid: 'a'
  };

  for (const anOption in panel.options) {
    if (!validConfigOptions.includes(anOption)) {
      // 移除此選項
      console.log(`removing ${anOption}`);
      delete panel.options.anOption;
    }
  }

  return panel.options;
}
```

### 步驟 4：設定一個安全的預設值

有時插件會移除某項功能或修改選項的有效選項。遷移處理常式可用於視需要調整設定。

```TYPESCRIPT
export const PolystatPanelMigrationHandler = (panel: PanelModel<PolystatOptions>): Partial<PolystatOptions> => {
  const featureOptions = [
    'SelectionA',
    'SelectionB',
    'SelectionC'
  ];

  // 偵測設定是否正在使用已被移除的值，
  //  並設定一個安全的預設值
  const removedOption = 'a removed option in selector';
  if (!featureOptions.includes(panel.options.aSelectionSetting) {
    panel.options.aSelectionSetting = featureOptions[0];
  }
  // 新增了新功能，為其設定一個安全的預設值
  if (panel.options.aSelectionSetting === undefined) {
    // 為新功能新增預設值
    panel.options.aSelectionSetting = featureOptions[0];
  }

  return panel.options;
}
```
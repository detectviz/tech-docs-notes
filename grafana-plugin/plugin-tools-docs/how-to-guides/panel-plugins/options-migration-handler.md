---
id: migration-handler-for-panels
title: 為您的面板插件新增遷移處理常式
description: 如何為您的 Grafana 面板插件新增遷移處理常式，以實現無縫更新。
keywords:
  - grafana
  - plugins
  - plugin
  - panel
  - migration
  - migration handler
  - version update
---

# 為您的面板插件新增遷移處理常式

在您開發和維護 Grafana 面板插件時，您可能需要變更面板選項結構。這些變更可能會破壞現有的儀表板設定。

當您對插件引入破壞性變更時，您應該增加插件版本的主要版本號（例如，從 1.2.1 到 2.0.0）。我們鼓勵您盡可能減少插件版本之間的破壞性變更，但在必要的情況下，為確保使用者更新您的插件時能順利轉換，您可以實作遷移處理常式。

## 遷移處理常式基礎知識

遷移處理常式是一個函式，當儀表板儲存的面板版本與目前安裝的面板版本不同時執行。它可讓您更新面板的選項以符合新的結構，而無需使用者手動介入。

若要將遷移處理常式新增至您的面板插件，請在 `PanelPlugin` 物件上使用 `setMigrationHandler` 方法：

```ts title="module.ts"
import { PanelPlugin } from '@grafana/data';
import { SimplePanel } from './components/SimplePanel';
import { SimpleOptions } from './types';
import { migrationHandler } from './migrationHandler';

export const plugin = new PanelPlugin<SimpleOptions>(SimplePanel)
  .setPanelOptions((builder) => {
    // ... 面板選項設定
  })
  // 定義遷移處理常式
  .setMigrationHandler(migrationHandler);
```

:::note

遷移處理常式僅在安裝的插件版本與用於產生面板的版本不同時才會被呼叫。對面板所做的變更不會自動持續存在，使用者需要在面板遷移後手動儲存儀表板。
:::

## 實作遷移處理常式

遷移處理常式函式會接收整個面板模型作為引數，並應傳回更新後的面板選項。以下是一個基本結構：

```ts title="migrationHandler.ts"
import { PanelModel } from '@grafana/data';
import { SimpleOptions } from './types';

function migrationHandler(panel: PanelModel<Partial<SimpleOptions>>) {
  // panel.options 包含儲存在儀表板中的目前面板選項
  const options = Object.assign({}, panel.options);

  // 在此處執行選項遷移

  return options;
}
```

## 常見的遷移情境

### 處理新選項

新增新的面板設定選項時，請為其設定預設值：

```ts
if (options.newFeature === undefined) {
  panel.options.newFeature = 'defaultValue';
}
```

### 重新命名選項

如果您重新命名了選項，請將舊選項的值轉移到新選項：

```ts
if (panel.options.oldOptionName) {
  panel.options.newOptionName = panel.options.oldOptionName;
  // 確保移除舊選項
  delete panel.options.oldOptionName;
}
```

### 調整已變更的選項

移除有效選項或變更有效選項時，請設定安全的預設值：

```ts
const validOptions = ['option1', 'option2', 'option3'];
if (!validOptions.includes(panel.options.someOption)) {
  panel.options.someOption = validOptions[0];
}
```

或將現有值遷移至新選項：

```ts
// 例如，displayType 的選項已重新命名
// 從 bar、line、pie 到 barChart、linePlot 和 pieChart
if (options.displayType) {
  switch (options.displayType) {
    case 'bar':
      options.displayType = 'barChart';
      break;
    case 'line':
      options.displayType = 'linePlot';
      break;
    case 'pie':
      options.displayType = 'pieChart';
      break;
  }
}
```

### 特定版本的調整

您可能希望根據用於編寫面板的版本來決定遷移。為此，您可以使用 `pluginVersion` 屬性。此屬性在遷移處理常式首次使用時為空，但之後將設定為用於儲存面板的插件版本。

例如，想像一個插件有以下歷史記錄：

 - 在 v1 中，插件沒有任何遷移程式碼。
 - 在 v2 中，插件引入了第一個遷移程式碼。
 - 在 v3 中，插件再次變更，並新增了新的遷移步驟。
 
在該情境下，遷移處理常式會像這樣：

```ts title="migrationHandler.ts"
function migrationHandler(panel: PanelModel<SimpleOptions>) {
  const options = Object.assign({}, panel.options);
  const pluginVersion = panel?.pluginVersion ?? '';

  if (pluginVersion === '') {
    // 插件版本為 v1.x
    // 需要將 v1 -> v3 的邏輯遷移
    options.displayMode = 'compact';
    options.displayType = 'linePlot';
  }
  
  if (pluginVersion.startsWith('2.') {
    // 面板上次使用 v2.x 版本儲存
    // 需要將 v2 -> v3 的邏輯遷移
    options.displayMode = 'compact';
  }

  return options;
}
```

#### 使用字串比較

```ts title="migrationHandler.ts"
import { config } from '@grafana/runtime';

function migrationHandler(panel: PanelModel<SimpleOptions>) {
  const options = Object.assign({}, panel.options);

  // 如果插件先前未實作遷移處理常式，pluginVersion 將為空
  // 或包含上次呼叫遷移處理常式後儲存面板時的插件版本。
  const pluginVersion = panel?.pluginVersion ?? '';

  if (pluginVersion === '' || pluginVersion.startsWith('1.')) {
    options.displayMode = 'compact';
  }

  return options;
}
```

## 最佳實務

1. 始終建立選項物件的副本，以避免修改原始物件。
2. 使用類型檢查以確保您只遷移存在的選項。
3. 處理所有可能的情境，以避免因意外設定而破壞儀表板。
4. 使用各種面板設定徹底測試您的遷移處理常式。
5. 在您的插件變更日誌中記錄變更，以幫助使用者了解更新了什麼。

請記住，遷移處理常式會在每次面板載入時執行，直到使用者手動編輯並儲存面板。請確保您的遷移是冪等的，並且如果多次執行不會產生意外的副作用。
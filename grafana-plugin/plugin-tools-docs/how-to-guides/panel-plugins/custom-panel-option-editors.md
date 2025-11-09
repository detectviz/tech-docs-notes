---
id: custom-panel-option-editors
title: 新增自訂面板選項編輯器
description: 如何新增自訂面板選項編輯器。
keywords:
  - grafana
  - plugins
  - plugin
  - custom panel option editor
  - customizing panel options
  - panel options
---

# 建立自訂面板選項編輯器

Grafana 插件平台提供了一系列編輯器，讓您的使用者可以自訂面板。標準編輯器涵蓋了最常見的選項類型，例如文字輸入和布林值開關。如果您找不到您要的編輯器，您可以建立自己的編輯器。

## 面板選項編輯器基礎知識

最簡單的編輯器是一個 React 元件，它接受兩個 props：

- **`value`**：選項的目前值
- **`onChange`**：更新選項的值

以下範例中的編輯器可讓使用者透過點擊按鈕來切換布林值：

```tsx title="src/SimpleEditor.tsx"
import React from 'react';
import { Button } from '@grafana/ui';
import { StandardEditorProps } from '@grafana/data';

export const SimpleEditor = ({ value, onChange }: StandardEditorProps<boolean>) => {
  return <Button onClick={() => onChange(!value)}>{value ? 'Disable' : 'Enable'}</Button>;
};
```

若要使用自訂面板選項編輯器，請在您的 `module.ts` 檔案中使用 `OptionsUIBuilder` 物件上的 `addCustomEditor`，並將 `editor` 屬性設定為您自訂編輯器元件的名稱。

```ts title="src/module.ts"
export const plugin = new PanelPlugin<SimpleOptions>(SimplePanel).setPanelOptions((builder) => {
  return builder.addCustomEditor({
    id: 'label',
    path: 'label',
    name: 'Label',
    editor: SimpleEditor,
  });
});
```

## 為您的面板選項編輯器新增設定

您可以使用您的自訂編輯器來自訂多個可能的設定。若要為您的編輯器新增設定，請將 `StandardEditorProps` 的第二個範本變數設定為包含您要設定之設定的介面。透過 `item` prop 存取編輯器設定。

以下是一個編輯器的範例，它會用一系列數字填入下拉式選單。`Settings` 介面定義了 `from` 和 `to` 屬性的範圍。

```tsx title="src/SimpleEditor.tsx"
interface Settings {
  from: number;
  to: number;
}

type Props = StandardEditorProps<number, Settings>;

export const SimpleEditor = ({ item, value, onChange }: Props) => {
  const options: Array<SelectableValue<number>> = [];

  // 預設值
  const from = item.settings?.from ?? 1;
  const to = item.settings?.to ?? 10;

  for (let i = from; i <= to; i++) {
    options.push({
      label: i.toString(),
      value: i,
    });
  }

  return <Select options={options} value={value} onChange={(selectableValue) => onChange(selectableValue.value)} />;
};
```

您現在可以透過將 `settings` 屬性設定為呼叫 `addCustomEditor` 來為每個選項設定編輯器：

```ts title="src/module.ts"
export const plugin = new PanelPlugin<SimpleOptions>(SimplePanel).setPanelOptions((builder) => {
  return builder.addCustomEditor({
    id: 'index',
    path: 'index',
    name: 'Index',
    editor: SimpleEditor,
    settings: {
      from: 1,
      to: 10,
    },
  });
});
```

## 在您的面板選項編輯器中使用查詢結果

選項編輯器可以存取上次查詢的結果。這可讓您根據資料來源傳回的資料動態更新您的編輯器。

編輯器上下文可透過 `context` prop 取得。資料來源傳回的資料框架可在 `context.data` 下取得。

```tsx title="src/SimpleEditor.tsx"
export const SimpleEditor = ({ item, value, onChange, context }: StandardEditorProps<string>) => {
  const options: SelectableValue<string>[] = [];

  if (context.data) {
    const frames = context.data;

    for (let i = 0; i < frames.length; i++) {
      options.push({
        label: frames[i].name,
        value: frames[i].name,
      });
    }
  }

  return <Select options={options} value={value} onChange={(selectableValue) => onChange(selectableValue.value)} />;
};
```
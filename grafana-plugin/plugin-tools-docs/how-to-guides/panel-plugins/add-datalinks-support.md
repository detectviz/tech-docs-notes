---
id: add-datalinks-support
title: 為面板插件新增資料連結支援
description: 如何為面板插件新增資料連結支援。
keywords:
  - grafana
  - plugins
  - plugin
  - datalinks
---

# 如何在面板插件中實作資料連結

資料連結可讓使用者從面板視覺化導覽至其他儀表板、外部系統或任何 URL。本指南將說明如何在您的 Grafana 面板插件中實作資料連結。

## 先決條件

- Grafana 面板插件開發的基礎知識
- 熟悉 React 和 TypeScript

## 了解資料連結

在 Grafana 中，資料連結是由使用者在面板的欄位選項中設定的。您的面板插件需要：

1. 從欄位資料存取這些已設定的連結
2. 檢查特定資料點是否存在連結
3. 呈現一個可點擊的元素以開啟連結選單
4. 使用 `DataLinksContextMenu` 元件來顯示和處理連結

## 運作方式：逐步說明

### 1. 存取具有連結的欄位資料

`getFieldDisplayValues` 函式會處理您的面板資料並套用欄位設定，包括資料連結：

```tsx title="YourPanel.tsx"
const fieldDisplayValues = getFieldDisplayValues({
  fieldConfig, // 包含欄位設定，包括資料連結
  reduceOptions: options.reduceOptions,
  data: data.series,
  theme,
  replaceVariables, // 用於連結中的變數內插
  timeZone,
});
```

### 2. 檢查資料連結

對於每個欄位值，請使用 `hasLinks` 和 `getLinks` 屬性檢查其是否具有連結：

```tsx
if (displayValue.hasLinks && displayValue.getLinks) {
  // 此欄位值已設定資料連結
}
```

### 3. 使用 DataLinksContextMenu 元件

將您的視覺化元素包裝在 `DataLinksContextMenu` 元件中：

```tsx title="YourPanel.tsx"
<DataLinksContextMenu links={displayValue.getLinks} config={displayValue.field}>
  {(api) => (
    <YourVisualizationElement
      onClick={api.openMenu} // 點擊時會開啟連結選單
      // 其他 props
    />
  )}
</DataLinksContextMenu>
```

## 幕後花絮

1. **使用者設定**：使用者在您面板的欄位選項中設定資料連結
2. **資料處理**：Grafana 處理這些連結並將其附加到欄位值
3. **呈現**：您的面板會檢查連結並呈現可點擊的元素
4. **互動**：當使用者點擊具有連結的元素時，會開啟內容選單
5. **導覽**：使用者選取一個連結並導覽至目標

## 注意事項

- 資料連結必須在您面板的欄位選項中設定
- `getLinks` 函式會傳回一個函式，該函式會傳回一個 `LinkModel` 物件陣列
- 資料連結支援使用 `replaceVariables` 函式進行變數內插
- 內容選單會處理所有顯示和導覽連結的 UI

透過實作資料連結，您可以增強面板的互動性，讓使用者可以從您的視覺化導覽至相關的儀表板或外部資源。

## 完整實作範例

以下是一個完整的範例，展示了所有部分如何在面板元件中組合在一起：

```tsx title="YourPanel.tsx"
import React from 'react';
import { PanelProps, getFieldDisplayValues, LinkModel, FieldConfig } from '@grafana/data';
import { DataLinksContextMenu, useStyles2, useTheme2 } from '@grafana/ui';

interface Props extends PanelProps<YourPanelOptions> {}

export const YourPanel = ({ data, width, height, options, replaceVariables, fieldConfig, timeZone }: Props) => {
  const theme = useTheme2();

  // 處理與不同 Grafana 版本的相容性
  const ContextMenu = DataLinksContextMenu as React.FC<{
    links: () => LinkModel[];
    config: FieldConfig;
    children: (api: { openMenu: React.MouseEventHandler<HTMLElement> }) => React.ReactNode;
  }>;

  // 1. 取得包含連結資訊的欄位顯示值
  const fieldDisplayValues = getFieldDisplayValues({
    fieldConfig,
    reduceOptions: options.reduceOptions,
    data: data.series,
    theme,
    replaceVariables,
    timeZone,
  });

  return (
    <div style={{ width, height }}>
      {/* 2. 呈現您的視覺化與資料連結 */}
      <div className="visualization-container">
        {fieldDisplayValues.map((displayValue, index) => {
          // 3. 檢查此欄位是否具有資料連結
          if (displayValue.hasLinks && displayValue.getLinks) {
            // 4. 使用 DataLinksContextMenu 來處理連結
            return (
              <ContextMenu key={index} links={displayValue.getLinks} config={displayValue.field}>
                {(api) => (
                  <div
                    className="data-point"
                    onClick={api.openMenu} // 5. 附加 openMenu 處理常式
                    style={{
                      cursor: 'pointer',
                      // 視覺化樣式
                    }}
                  >
                    {displayValue.display.text}
                  </div>
                )}
              </ContextMenu>
            );
          }

          // 呈現沒有連結的元素
          return (
            <div key={index} className="data-point">
              {displayValue.display.text}
            </div>
          );
        })}
      </div>
    </div>
  );
};
```
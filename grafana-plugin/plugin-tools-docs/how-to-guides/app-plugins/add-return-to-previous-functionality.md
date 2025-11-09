---
id: add-return-to-previous-functionality
title: 允許使用者輕鬆返回特定情境
description: 允許使用者輕鬆返回特定情境。
keywords:
  - grafana
  - plugins
  - plugin
  - return to previous
  - return to
  - context
---

# 簡介

使用者在瀏覽 Grafana 時的_情境_可能會發生巨大變化，有時他們的麵包屑導覽與他們的歷史記錄不符。在這些情況下，使用者無法輕鬆返回，可能會對他們的體驗感到沮喪。本指南定義了_情境_，並說明了如何將 `ReturnToPrevious` 功能新增至您的插件，以最少的程式碼解決此問題。

## Grafana 中的情境

就 Grafana 插件開發而言，_情境_一詞指的是使用者從一個根 URL 到另一個根 URL 的路徑上的位置。請注意，在 Grafana 中，此術語也可能與其他 Grafana 功能（例如 Explore 或儀表板）相關。

使用麵包屑導覽來注意使用者情境的變化。例如：

- 您是否從 **首頁 > 儀表板** 轉到 **首頁 > 探索**？那麼您就改變了您所在的情境。

- 您是否從 **首頁 > 儀表板 > 播放清單 > 編輯播放清單** 轉到 **首頁 > 儀表板 > 報告 > 設定**？那麼您與之前處於相同的情境。

如您所見，關鍵在於 URL 從根層級的變化。因為「探索」和「儀表板」都位於根層級，所以使用者的情境發生了變化。但使用者到「報告設定」的路徑情況並非如此。

## 新增功能以允許使用者返回其先前的情境

1. 選取一個互動式項目，例如連結或按鈕，以觸發 `ReturnToPrevious` 功能。此元素將會引導使用者到 Grafana 內部的另一個情境。例如，警示規則的 `Go to dashboard` 按鈕。

2. 若要設定所需的值，您可以使用來自 `@grafana/runtime` 的 `useReturnToPrevious` hook：

- 指定要顯示在按鈕中的 `title`。
- （可選）如果 `href` 與目前 URL 不同，請傳遞第二個引數來設定它。

例如：

```tsx
import { config, useReturnToPrevious } from '@grafana/runtime';

const setReturnToPrevious = useReturnToPrevious();

[...]

<LinkButton
size="sm"
       key="dashboard"
       variant="primary"
       icon="apps"
       href={`d/${encodeURIComponent(dashboardUID)}`}
       onClick={() => {
       	setReturnToPrevious(rule.name);
       }}
>
      	Go to dashboard
</LinkButton>
```

3. 驗證其是否如預期般運作。當您透過該互動式元素從您的應用程式插件轉到 Grafana 的另一個區域時，應會出現「返回上一頁」按鈕。

## 使用指南

### 請這樣做

- 請透過連結或具有 `onClick` 事件的互動式元素來觸發「ReturnToPrevious」功能。
- 請僅在將使用者傳送到另一個情境時使用「ReturnToPrevious」功能，例如從「警示」到「儀表板」。
- 請指定一個能以最易懂的方式識別要返回頁面的按鈕標題。

### 請不要這樣做

- 請勿透過連結或按鈕以外的元素觸發「ReturnToPrevious」功能。
- 在相同情境中從一個頁面轉到另一個頁面時，請勿使用此功能。
- 請勿使用「返回上一頁」之類的文字。請具體說明。
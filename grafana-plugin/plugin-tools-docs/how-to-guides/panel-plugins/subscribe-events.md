---
id: subscribe-events
title: 訂閱 Grafana 事件
description: 使用事件匯流排訂閱 Grafana 應用程式事件
keywords:
  - grafana
  - plugins
  - plugin
  - panel
  - properties
  - eventbus
  - event bus
  - subscribe
  - events
---

# 訂閱 Grafana 事件

如果您正在建立[面板插件](../../key-concepts/plugin-types-usage.md#panel-visualization-plugins)，在某些情況下，您可能會希望您的插件對發生在插件外部的變更做出反應。例如，您可能希望您的插件在使用者放大或縮小另一個面板時做出反應。在本指南中，您將學習如何讓您的插件對 Grafana 中的事件做出反應。

:::tip

有關製作您自己的面板插件的逐步指南，請參閱我們的[面板插件教學](../../tutorials/build-a-panel-plugin.md)。

:::

## 存取事件匯流排

用於訂閱 Grafana 事件的事件匯流排位於 [`PanelProps`](https://github.com/grafana/grafana/blob/57960148e47e4d82e899dbfa3cb9b2d474ad56dc/packages/grafana-data/src/types/panel.ts#L74-L122) 介面中。此介面公開了有關面板的執行階段資訊，例如面板的尺寸和計時測量。有關屬性定義，請參閱程式碼註解。

您可以透過插件的 `props` 引數存取事件匯流排和其他面板屬性。例如：

```js title="src/components/SimplePanel.tsx"
export const SimplePanel: React.FC<Props> = ({ options, data, width, height }) => {
```

## 訂閱 Grafana 應用程式事件

Grafana 使用事件匯流排將應用程式事件發佈到 Grafana 的不同部分，以便在使用者執行操作時通知它們。您的插件可以透過訂閱一個或多個事件來對這些操作做出反應。

事件由唯一的字串識別；此外，它們可以有可選的承載。在以下範例中，`ZoomOutEvent` 由 `zoom-out` 字串識別，並攜帶一個數字作為承載。

```tsx
class ZoomOutEvent extends BusEventWithPayload<number> {
  static type = 'zoom-out';
}
```

以下是您可以訂閱的其他一些事件：

- `RefreshEvent` from `@grafana/runtime`
- `DataHoverEvent` from `@grafana/data`

您可以存取面板 props 中可用的事件匯流排，並使用 `getStream()` 方法訂閱特定類型的事件。傳遞給 subscribe 方法的回呼將針對每個新事件被呼叫，如下列範例所示：

```tsx
import React, { useEffect } from 'react';
import { RefreshEvent } from '@grafana/runtime';

// ...

interface Props extends PanelProps<MyOptions> {}

export const MyPanel: React.FC<Props> = ({ eventBus }) => {
  useEffect(() => {
    const subscriber = eventBus.getStream(RefreshEvent).subscribe((event) => {
      console.log(`Received event: ${event.type}`);
    });

    return () => {
      subscriber.unsubscribe();
    };
  }, [eventBus]);

  return <div>Event bus example</div>;
};
```

:::important

請記得在您的訂閱者上呼叫 `unsubscribe()` 以避免記憶體洩漏。

:::

## 支援哪些事件？

雖然目前沒有關於支援事件的官方文件，但您或許可以根據它們在其他插件中的用法以及它們提供的功能來提取事件。

請注意，雖然許多事件類型可用但尚未匯出，例如 `PanelEditEnteredEvent`，但您仍然可以透過自己重新實作事件類型來訂閱它們：

```tsx
class MyPanelEditEnteredEvent extends BusEventWithPayload<number> {
  static type = 'panel-edit-started';
}
```

我們將在未來改進事件匯流排並新增更多事件。請在[我們的社群論壇](https://community.grafana.com/c/plugin-development/30)中讓我們知道您如何使用事件匯流排，以及您認為對您的插件有用的任何事件！
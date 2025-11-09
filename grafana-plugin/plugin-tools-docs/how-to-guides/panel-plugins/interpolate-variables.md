---
id: interpolate-variables
title: 在面板插件中內插變數
description: 在 Grafana 面板插件中新增對變數的支援。
keywords:
  - grafana
  - plugins
  - plugin
  - panel
  - queries
  - variables
---

變數是值的預留位置，您可以使用它們來建立範本化的查詢，以及儀表板或面板連結。有關變數的更多資訊，請參閱[範本和變數](https://grafana.com/docs/grafana/latest/dashboards/variables)。

Grafana 提供了輔助函式來在字串範本中內插變數。`replaceVariables` 函式可在 `PanelProps` 中使用。

將 `replaceVariables` 新增至引數清單，並將使用者定義的範本字串傳遞給它：

```tsx
export function SimplePanel({ options, data, width, height, replaceVariables }: Props) {
  const query = replaceVariables('Now displaying $service');

  return <div>{query}</div>;
}
```
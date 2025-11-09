---
id: extension-points
title: Grafana 中的擴充點
description: Grafana 中可用的擴充點。
keywords:
  - grafana
  - plugins
  - documentation
  - plugin.json
  - UI extensions
  - extension points
sidebar_position: 50
---

使用由 `@grafana/data` 套件公開的 [`PluginExtensionPoints`](https://github.com/grafana/grafana/blob/main/packages/grafana-data/src/types/pluginExtensions.ts#L121) 列舉來存取 Grafana 中的擴充點。

```typescript
import { PluginExtensionPoints } from '@grafana/data';

const extensionPointId = PluginExtensionPoints.DashboardPanelMenu;
```

可用的擴充點如下：

| 擴充點 ID (Extension Point ID)      | 類型 (Type) | 描述 (Description)                                                 |
| --------------------------------- | --------- | -------------------------------------------------------------------- |
| **`AlertingAlertingRuleAction`**  | Link      | 使用自訂操作擴充警示規則選單，用於警示規則。                         |
| **`AlertingHomePage`**            | Component | 使用自訂的警示建立體驗擴充警示首頁。                                 |
| **`AlertingRecordingRuleAction`** | Link      | 使用自訂操作擴充警示規則選單，用於記錄規則。                         |
| **`AlertInstanceAction`**         | Link      | 使用自訂操作擴充警示實例表格。                                       |
| **`CommandPalette`**              | Link      | 使用自訂操作擴充命令面板。                                           |
| **`DashboardPanelMenu`**          | Link      | 使用自訂操作擴充面板選單。                                           |
| **`ExploreToolbarAction`**        | Link      | 在「探索」頁面上使用自訂操作擴充「新增」按鈕。                       |
| **`UserProfileTab`**              | Component | 使用自訂分頁擴充使用者個人資料頁面。                                 |
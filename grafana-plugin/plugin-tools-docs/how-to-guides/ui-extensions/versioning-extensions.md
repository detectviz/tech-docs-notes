---
id: versioning-extensions
title: 版本化擴充功能
sidebar_label: 版本化擴充點
description: 查看版本化 UI 擴充功能的最佳實務，以確保穩定性、相容性和順暢的轉換。
keywords:
  - grafana
  - plugins
  - plugin
  - extensions
  - ui-extensions
sidebar_position: 40
---

# 版本化 UI 擴充功能的最佳實務

為了在更新 UI 擴充功能時維持穩定性並確保順暢的轉換，請在擴充點或公開元件的 ID 中包含_版本後綴_。此作法可在允許開發人員以受控的方式管理破壞性變更的同時，保留相容性。

## 在 ID 中使用版本後綴

每個擴充點 ID/元件 ID 都應包含一個表示擴充功能主要版本的後綴。

**範例：**

```typescript
// 初始版本
export const EXTENSION_POINT_OR_COMPONENT_ID_V1 = 'my-plugin-id/feature/v1';

// 引入破壞性變更
export const EXTENSION_POINT_OR_COMPONENT_ID_V2 = 'my-plugin-id/feature/v2';
```

- 非破壞性變更（例如，新增可選屬性）_不_需要新的版本後綴。
- 破壞性變更（例如，修改行為或移除屬性）_必須_引入新的版本後綴。

## 在轉換期間支援多個版本

在引入新的主要版本時，應用程式應在轉換期間同時提供新舊版本。這讓消費者有時間遷移，而不會立即中斷。

**範例：**

- `my-plugin-id/feature/v1` 在引入 `my-plugin-id/feature/v2` 時繼續運作。
- 消費者逐漸遷移至 `v2`。
- 在 `v1` 的棄用期過後，您可以安全地將其移除。

## 清楚地傳達棄用資訊

應清楚地向消費者傳達棄用資訊，以確保順暢的轉換。

- 在已發布的類型中使用 `@deprecated` 關鍵字，並參考變更日誌或遷移指南。

  **範例：**

  ```typescript
  /**
   * @deprecated 請改用 FeatureConfigV2。請參閱遷移指南：https://example.com/migration-guide
   */
  export type FeatureContextV1 = {
    /* ... */
  };
  ```

- 在變更日誌或遷移指南中記錄變更。
- 提供棄用舊版本的時間表。
- 通知消費者即將發生的變更，以防止意外中斷。

## 使用版本後綴發布類型

:::note
此選項目前僅適用於在 grafana 組織內開發的插件。
:::

為了支援同時使用多個版本，請使用相同的版本後綴將類型發布至 `@grafana/plugin-types`。這可讓開發人員在不發生衝突的情況下從不同版本匯入類型。

**範例：**

```typescript
// 擴充點上下文
import { FeatureContextV1 } from '@grafana/plugin-types/my-plugin-id';
import { FeatureContextV2 } from '@grafana/plugin-types/my-plugin-id';

// 公開的元件 props
import { ComponentPropsV1 } from '@grafana/plugin-types/my-plugin-id';
import { ComponentPropsV2 } from '@grafana/plugin-types/my-plugin-id';
```

- 在處理不同擴充功能版本時確保類型安全。
- 在引入變更時避免破壞現有的消費者。

## 摘要

透過遵循此方法對擴充功能和擴充點進行版本控制，您可以確保它們在允許迭代改進、順暢遷移和更安全的類型管理的同時保持穩定。
---
id: runtime-checks
title: 使用執行階段檢查管理向後相容性
description: 如何使用執行階段檢查管理向後相容性。
keywords:
  - grafana
  - plugins
  - plugin
  - compatibility
---

# 使用執行階段檢查管理向後相容性

如[管理 NPM 相依性](../key-concepts/manage-npm-dependencies.md)文章中所述，插件中使用的大多數 Grafana NPM 相依性在執行階段都與 Grafana 應用程式共用。為了在插件中利用新的 Grafana 功能，同時保持與舊版本的相容性，插件作者需要實作條件式邏輯，在執行階段檢查功能的可用性。未能考慮到向後相容性可能會導致插件崩潰和糟糕的使用者體驗。

執行這些執行階段檢查的方法因功能及其向插件開發人員提供的方式而異。以下範例展示了有效處理這些情境的最佳實踐。

## 範例：有條件地呼叫函式

Grafana 10.1.0 在 `@grafana/data` 套件中引入了 `createDataFrame` 函式，並棄用了 `MutableDataFrame` 類別。為了保持與 10.1.0 之前的 Grafana 版本的相容性，插件必須實作條件式邏輯，以確定這些 API 在執行階段是否可用。

```tsx
import { createDataFrame, DataFrameDTO, MutableDataFrame } from '@grafana/data';

function getDataFrame(data: DataFrameDTO) {
  if (typeof createDataFrame === 'undefined') {
    // 對於較舊版本，退回到已棄用的類別
    return new MutableDataFrame(data);
  } else {
    // 如果可用，則使用新的 API
    return createDataFrame(data);
  }
}
```

## 範例：有條件地使用 React hooks

在 Grafana 11.1.0 中，同步的 `getPluginLinkExtensions` 函式被棄用，並由反應式的 `usePluginLinks` hook 取代。以下範例展示了如何根據其可用性動態地在兩個 API 之間切換。

```tsx
import { useMemo } from 'react';
import { PluginExtensionLink } from '@grafana/data';
import {
  GetPluginExtensionsOptions,
  getPluginLinkExtensions,
  usePluginLinks as usePluginLinksOriginal,
} from '@grafana/runtime';

function useLegacyLinkExtensions({ context, extensionPointId }: GetPluginExtensionsOptions): {
  links: PluginExtensionLink[];
  isLoading: boolean;
} {
  const { extensions } = useMemo(
    () =>
      getPluginLinkExtensions({
        extensionPointId,
        context,
      }),
    [context, extensionPointId]
  );

  return {
    links: extensions,
    isLoading: false,
  };
}

// 動態決定使用哪個 API
const usePluginLinks = usePluginLinksOriginal !== undefined ? usePluginLinksOriginal : useLegacyLinkExtensions;

export function ToolbarExtensionPoint() {
  const { links, isLoading } = usePluginLinks({ extensionPointId: 'myorg-foo-app/toolbar/v1' });

   // 您的實作在此處
  ...
}
```

## 範例：有條件地呈現 React 元件

`UserIcon` 元件是在 Grafana 10.1.0 中引入的，在較早的版本中沒有對應的元件。為了保持相容性，僅當 UserIcon 元件在目前的執行階段環境中可用時才呈現它。

```tsx
import React from 'react';
import { Card, UserIcon, UserView } from '@grafana/ui';

export const Profile = ({ userView }: { userView: UserView }) => {
  return (
    <Card>
      <Card.Heading>Profile</Card.Heading>
      <Card.Meta>{['Tag 1']}</Card.Meta>
      {/* 如果 UserIcon 元件存在，則有條件地呈現它 */}
      {UserIcon && <UserIcon userView={userView} />}
    </Card>
  );
};
```

## 範例：在端對端測試中涵蓋條件式呈現

當某個功能僅在特定的 Grafana 版本中可用時，透過端對端 (E2E) 測試來驗證其條件式呈現是一個很好的做法。這些測試可確保插件在功能存在的新環境和功能不可用的舊環境中都能正確運作。

以下範例測試 `UserIcon` 元件是否僅在 Grafana 10.1.0 或更新版本中呈現，同時確保使用者設定檔的其餘部分始終顯示。

```tsx
import * as semver from 'semver';
import { test, expect } from '@grafana/plugin-e2e';

test('應呈現設定檔', async ({ page, grafanaVersion }) => {
  const userProfile = page.getByTestId('user-profile');

  // 驗證共用元件的可見性
  await expect(userProfile.getByText('Heading')).toBeVisible();
  await expect(userProfile.getByText('Tag 1')).toBeVisible();

  // 有條件地驗證 UserIcon 元件的呈現
  if (semver.gte(grafanaVersion, '10.1.0')) {
    await expect(userProfile.getByText('Jane Doe')).toBeVisible();
  }
});
```

### 延伸閱讀

- **插件的端對端測試**：有關為 Grafana 插件編寫和執行 E2E 測試的全面指南，請參閱[文件](../e2e-test-a-plugin/index.md)。
- **跨多個 Grafana 版本執行端對端測試**：若要了解如何設定您的工作流程以針對不同的 Grafana 版本測試插件，請參閱[範例工作流程](../e2e-test-a-plugin/ci.md)。
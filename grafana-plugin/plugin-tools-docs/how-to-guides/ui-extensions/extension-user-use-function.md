---
id: extension-user-use-function
title: 將擴充點用於一般函式
sidebar_label: 將擴充點用於一般函式
sidebar_position: 10
description: 學習如何提供一個擴充點，以便其他應用程式可以貢獻其擴充功能。
keywords:
  - grafana
  - plugins
  - plugin
  - links
  - extensions
  - extension point
  - app plugins
  - apps
  - function extension
---

擴充點是您插件或 Grafana UI 的一部分，您可以在其中呈現來自其他插件的內容（連結、函式或 React 元件）。使用它們來根據擴充點公開的情境擴充您的使用者體驗。

:::note
在[關鍵概念](../../key-concepts/ui-extensions.md)下閱讀有關擴充功能的更多資訊。<br />
有關參考文件，包括 API，請參閱 [UI 擴充功能參考指南](../../reference/ui-extensions-reference)。
:::

## 函式擴充功能的最佳實務

- **分享情境資訊** <br /> 思考哪些情境資訊對其他插件可能有用，並將其作為參數傳遞給函式。
- **處理錯誤** <br /> 確保處理函式擴充功能可能擲回的任何錯誤。

## 為函式建立一個擴充點

```tsx
import { usePluginFunctions } from '@grafana/runtime';

export const MyComponent = () => {
  // 這是您擴充點的唯一 ID。
  // 這也是其他插件（內容提供者）在呼叫 `AppPlugin.addFunction({...})` 時使用的
  // - 以版本號碼為 ID 後綴（在此範例中為 "/v1"）
  // - 以您的插件 ID 為 ID 前綴（在此範例中為 "myorg-foo-app/"），
  //   或對於核心 Grafana 擴充點，則為 "grafana/"
  const extensionPointId = 'myorg-foo-app/myfunction/v1';
  const { functions, isLoading } = usePluginFunctions({ extensionPointId });
  const onClick = useCallback(() => {
    functions.forEach((fn) => {
      try {
        fn();
      } catch (err) {
        // 在此處處理錯誤
      }
    });
  }, [functions]);

  if (isLoading) {
    return <div>載入中...</div>;
  }

  return <Button onClick={onClick}>點擊</Button>;
};
```

## 將資料傳遞給函式

我們在先前的範例基礎上進行建構，但這次我們也將一些情境資料傳遞給函式：

```tsx
import { usePluginFunctions } from '@grafana/runtime';

// 公開類型以獲得更好的開發人員體驗
// （查看下一節了解如何與其他插件共用此類型）
export type Fn = (params: { activeProjectId: string }) => void;

export const MyComponent = () => {
  const extensionPointId = 'myorg-foo-app/myfunction/v1';
  const { projectId } = useActiveProject();
  // 使用 `Fn` 類型作為泛型
  const { functions, isLoading } = usePluginFunctions<Fn>({ extensionPointId });
  const onClick = useCallback(() => {
    functions.forEach((fn) => {
      try {
        fn({ activeProjectId: projectId });
      } catch (err) {
        // 在此處處理錯誤
      }
    });
  }, [functions, projectId]);

  if (isLoading) {
    return <div>載入中...</div>;
  }

  return <Button onClick={onClick}>點擊</Button>;
};
```

## 限制哪些插件可以在您的擴充點中註冊函式

```tsx
import { usePluginFunctions } from '@grafana/runtime';

export type Fn = (params: { activeProjectId: string }) => void;

export const MyComponent = () => {
  const extensionPointId = 'myorg-foo-app/myfunction/v1';
  const allowedPluginIds = ['myorg-a-app', 'myorg-b-app'];
  const { projectId } = useActiveProject();
  const { functions, isLoading } = usePluginFunctions<Fn>({ extensionPointId });
  const onClick = useCallback(() => {
    functions
      .filter(({ pluginId }) => allowedPluginIds.includes(pluginId))
      .forEach((fn) => {
        try {
          fn({ activeProjectId: projectId });
        } catch (err) {
          // 在此處處理錯誤
        }
      });
  }, [functions, projectId]);

  if (isLoading) {
    return <div>載入中...</div>;
  }

  return <Button onClick={onClick}>點擊</Button>;
};
```
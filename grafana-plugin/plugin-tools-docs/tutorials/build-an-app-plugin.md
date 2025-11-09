---
id: build-an-app-plugin
title: 建立應用程式外掛程式
sidebar_position: 10
description: 了解如何建立應用程式外掛程式。
keywords:
  - grafana
  - plugins
  - plugin
  - app
---

import CreatePlugin from '@shared/create-plugin-frontend.md';
import PluginAnatomy from '@shared/plugin-anatomy.md';

## 簡介

在本教學中，您將學習如何建立一個 _app plugin_。應用程式外掛程式是 Grafana 外掛程式，可讓您將資料來源和面板外掛程式捆綁在一個套件中。它們還可讓您在 Grafana 中建立自訂頁面，為文件、註冊表單、透過 HTTP 與其他服務整合提供專用空間。您也可以使用應用程式外掛程式來建立 [Scenes apps](https://grafana.com/developers/scenes/)。

應用程式外掛程式可以顯示在[導覽選單](#add-a-page-in-the-navigation-menu)中，並提供定義[UI 擴充](../key-concepts/ui-extensions.md)的彈性。

### 先決條件

- Grafana v10.0 或更新版本
- [LTS](https://nodejs.dev/en/about/releases/) 版本的 Node.js

## 建立新的應用程式外掛程式

<CreatePlugin pluginType="app" />

:::note

如果您選擇為您的應用程式外掛程式設定後端，請在啟動 Grafana 與 Docker 之前執行 `mage -v` 來建置二進位檔。

:::

## 外掛程式剖析

<PluginAnatomy />

## 開發工作流程

接下來，您將學習對您的應用程式進行變更、建置它，以及重新載入 Grafana 以反映您所做變更的基本工作流程。

第一步是檢視您已建立鷹架的外掛程式的運作情形。

1. 使用 `docker compose up` 啟動您的 Grafana 執行個體。
2. 在您的瀏覽器中開啟 Grafana。前往 [http://localhost:3000](http://localhost:3000)。
3. 前往 **Apps** -> **Your App Name**。

現在您可以檢視您的應用程式根頁面 (範例中的第一頁)，請嘗試對應用程式外掛程式進行變更：

1. 在 `PageOne.tsx` 中，變更一些頁面文字內容：

   ```tsx title="src/pages/PageOne.tsx"
   <PluginPage>New page content</PluginPage>
   ```

2. 儲存檔案。
3. 在您的瀏覽器中重新載入 Grafana。您的變更應該會出現。

## 在導覽選單中新增頁面

若要在您的應用程式選單項目下的導覽選單中新增頁面，請修改 `plugin.json` 檔案的 [`includes` 區段](../reference/metadata.md#includes)。

當您建立外掛程式的鷹架時，`create-plugin` 工具會將一些範例頁面新增至導覽選單。每個範例頁面都遵循 `/a/%PLUGIN_ID%/PAGE_NAME` 之類的路徑。任何傳送至 /a/%PLUGIN_ID% 的請求，例如 `/a/myorgid-simple-app/`，都會路由至應用程式外掛程式的根頁面。根頁面是一個 React 元件，它會傳回指定路由的內容。

讓我們在導覽選單中新增一個新頁面：

1. 修改 `plugin.json` 以新增新頁面。

   ```json title="src/plugin.json"
   // ...
   "includes": [
       // ...
       {
           "type": "page",
           "name": "New Page",
           "path": "/a/%PLUGIN_ID%/new-page",
           "addToNav": true,
           "defaultNav": false
       }
   ]
   ```

2. 儲存 `src/plugin.json` 檔案。
3. 重新啟動您的 Grafana 執行個體。

:::note

儲存 `plugin.json` 檔案後，您需要重新啟動您的 Grafana 執行個體，才能在導覽選單中看到新頁面。

:::

新頁面會出現在導覽選單中。您現在可以在 `src/components/App/App.tsx` 中編輯 React 路由器，並將自訂元件指向它。

1. 建立一個名為 `src/pages/NewPage.tsx` 的新檔案，並新增下列程式碼：

   ```tsx title="src/pages/NewPage.tsx"
   import React from 'react';
   import { PluginPage } from '@grafana/runtime';

   export function NewPage() {
     return <PluginPage>New Page</PluginPage>;
   }
   ```

2. 修改 `src/components/App/App.tsx` 中的路由以辨識新頁面：

   ```tsx title="src/components/App/App.tsx"
   {
     /* .... */
   }
   <Route path="new-page" element={<NewPage />} />;
   ```

3. 儲存檔案。
4. 重新載入 Grafana 以檢視新頁面。

您不需要在 `plugin.json` 的 `includes` 中註冊所有頁面。僅註冊您要新增至導覽選單的頁面。

:::tip

您可以使用 [`role`](/reference/plugin-json#includes) 屬性來限制哪些使用者可以存取導覽選單中的頁面。

:::

:::note

您只能在導覽選單中擁有一層頁面。不支援子選單項目。

:::

## 組態頁面

您可以將組態頁面新增至您的應用程式外掛程式，讓使用者可以設定您的外掛程式所需的任何設定。您的外掛程式應該已經有一個範例組態頁面，其原始碼位於 `src/components/AppConfig/AppConfig.tsx`。

### 儲存使用者設定

若要儲存使用者設定，請將 POST 請求傳送至 `/api/plugins/%PLUGIN_ID%/settings`，並將 `jsonData` 和 `secureJsonData` 作為資料。

```ts
export const updatePluginSettings = async (pluginId: string, data: Partial<PluginMeta>) => {
  const response = await getBackendSrv().fetch({
    url: `/api/plugins/${pluginId}/settings`,
    method: 'POST',
    data, // data: { jsonData: { ... }, secureJsonData: { ... } }
  });

  return lastValueFrom(response);
};
```

### 擷取使用者設定

使用者設定是外掛程式 `meta` 的一部分。您可以使用 `usePluginContext` 掛鉤在 React 元件內擷取它們。例如：

```tsx
import React from 'react';
import usePluginContext from '@grafana/data';

function MyComponent() {
  const context = usePluginContext();
  // user settings
  const jsonData = context.meta.jsonData;
}
```

## 使用資料代理從前端程式碼擷取資料

如果您想從您的應用程式前端程式碼擷取資料 (例如，從第三方 API)，而不會發生 [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) 問題或使用已驗證的請求，那麼您可以使用[資料代理](../how-to-guides/data-source-plugins/fetch-data-from-frontend)。

## 在您的應用程式中新增巢狀外掛程式

您可以在您的應用程式外掛程式中巢狀化資料來源和面板外掛程式。請參閱[使用巢狀外掛程式](../how-to-guides/app-plugins/work-with-nested-plugins)。

## 包含外部外掛程式

如果您想讓使用者知道您的應用程式需要現有的外掛程式，您可以將其作為相依性新增至 `plugin.json`。請注意，他們仍然需要自行安裝。

```json title="src/plugin.json"
"dependencies": {
  "plugins": [
    {
      "type": "panel",
      "name": "Clock Panel",
      "id": "grafana-clock-panel",
      "version": "^2.1.0"
    }
  ]
}
```

## 後續步驟

- [簽署您的外掛程式](../publish-a-plugin/sign-a-plugin.md)
- [發布您的外掛程式](../publish-a-plugin/publish-or-update-a-plugin.md)
- 為您的外掛程式撰寫 [e2e 測試](../e2e-test-a-plugin/get-started.md)
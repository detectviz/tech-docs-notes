# UI 擴充功能

Argo CD 網頁使用者介面可以透過額外的 UI 元素進行擴充。擴充功能應以 javascript 檔案的形式提供，放置在 `argocd-server` Pod 的 `/tmp/extensions` 目錄中，並以 `extension` 前綴開頭（符合 `^extension(.*)\.js$` 正規表示法）。

```
/tmp/extensions
├── example1
│   └── extension-1.js
└── example2
    └── extension-2.js
```

擴充功能在初始頁面渲染期間載入，並應使用 `extensionsAPI` 全域變數中公開的 API 進行註冊。（有關更多資訊，請參閱對應的擴充功能類型詳細資訊）。

擴充功能應提供一個負責渲染 UI 元素的 React 元件。擴充功能不應捆綁 React 函式庫。相反，擴充功能應使用 `react` 全域變數。如果您正在使用 webpack，可以利用 `externals` 設定：

```js
externals: {
  react: "React";
}
```

## 資源標籤擴充功能

資源標籤擴充功能是一種為 Argo CD 應用程式詳細資訊頁面上的資源滑動面板提供額外標籤的擴充功能。

資源標籤擴充功能應使用 `extensionsAPI.registerResourceExtension` 方法进行註冊：

```typescript
registerResourceExtension(component: ExtensionComponent, group: string, kind: string, tabTitle: string)
```

-   `component: ExtensionComponent` 是一個 React 元件，它接收以下屬性：

    -   application: Application - Argo CD 應用程式資源；
    -   resource: State - Kubernetes 資源物件；
    -   tree: ApplicationTree - 包括構成應用程式的所有資源的列表；

    請參閱 [models.ts](https://github.com/argoproj/argo-cd/blob/master/ui/src/app/shared/models.ts) 中的屬性介面

-   `group: string` - 符合資源群組的 glob 運算式；注意：使用 globstar (`**`) 來符合所有群組，包括空字串；
-   `kind: string` - 符合資源種類的 glob 運算式；
-   `tabTitle: string` - 擴充功能標籤的標題。
-   `opts: Object` - 附加選項：
    -   `icon: string` - 代表 [https://fontawesome.com/](https://fontawesome.com/) 函式庫中圖示的類別名稱（例如 'fa-calendar-alt'）；

以下是資源標籤擴充功能的範例：

```javascript
((window) => {
  const component = () => {
    return React.createElement("div", {}, "Hello World");
  };
  window.extensionsAPI.registerResourceExtension(
    component,
    "*",
    "*",
    "Nice extension"
  );
})(window);
```

## 系統層級擴充功能

Argo CD 允許您向側邊欄新增新項目，點擊後將會以帶有自訂元件的新頁面顯示。系統層級擴充功能應使用 `extensionsAPI.registerSystemLevelExtension` 方法進行註冊：

```typescript
registerSystemLevelExtension(component: ExtensionComponent, title: string, options: {icon?: string})
```

以下是一個簡單的系統層級擴充功能的範例：

```javascript
((window) => {
  const component = () => {
    return React.createElement(
      "div",
      { style: { padding: "10px" } },
      "Hello World"
    );
  };
  window.extensionsAPI.registerSystemLevelExtension(
    component,
    "Test Ext",
    "/hello",
    "fa-flask"
  );
})(window);
```

## 應用程式標籤擴充功能

由於 Argo CD 應用程式是一個 Kubernetes 資源，因此應用程式標籤可以與任何其他資源標籤相同。
請確保使用 'argoproj.io'/'Application' 作為群組/種類，擴充功能將用於渲染應用程式層級的標籤。

## 應用程式狀態面板擴充功能

狀態面板是應用程式檢視頂部的長條，其中顯示同步狀態。Argo CD 允許您向應用程式的狀態面板新增新項目。擴充功能應使用 `extensionsAPI.registerStatusPanelExtension` 方法進行註冊：

```typescript
registerStatusPanelExtension(component: StatusPanelExtensionComponent, title: string, id: string, flyout?: ExtensionComponent)
```

以下是一個簡單擴充功能的範例：

```javascript
((window) => {
  const component = () => {
    return React.createElement(
      "div",
      { style: { padding: "10px" } },
      "Hello World"
    );
  };
  window.extensionsAPI.registerStatusPanelExtension(
    component,
    "My Extension",
    "my_extension"
  );
})(window);
```

### 彈出式小工具

也可以為您的擴充功能新增一個可選的彈出式小工具。可以透過從您的擴充功能元件中呼叫 `openFlyout()` 來開啟它。然後，您的彈出式元件將在一個滑動面板中渲染，類似於點擊 `歷史和回滾` 時開啟的面板。

以下是使用彈出式小工具的擴充功能範例：


```javascript
((window) => {
  const component = (props: {
    openFlyout: () => any
  }) => {
    return React.createElement(
            "div",
            {
              style: { padding: "10px" },
              onClick: () => props.openFlyout()
            },
            "Hello World"
    );
  };
  const flyout = () => {
    return React.createElement(
            "div",
            { style: { padding: "10px" } },
            "This is a flyout"
    );
  };
  window.extensionsAPI.registerStatusPanelExtension(
          component,
          "My Extension",
          "my_extension",
          flyout
  );
})(window);
```

## 頂部操作列選單擴充功能

頂部操作列面板是應用程式檢視頂部的操作選單，其中顯示了諸如詳細資訊、同步、重新整理等操作按鈕。Argo CD 允許您向應用程式的頂部操作列選單新增新按鈕。
當點擊擴充功能按鈕時，自訂小工具將在一個彈出式面板中渲染。

擴充功能應使用 `extensionsAPI.registerTopBarActionMenuExt` 方法進行註冊：

```typescript
registerTopBarActionMenuExt(
  component: TopBarActionMenuExtComponent,
  title: string,
  id: string,
  flyout?: ExtensionComponent,
  shouldDisplay: (app?: Application) => boolean = () => true,
  iconClassName?: string,
  isMiddle = false
)
```

如果擴充功能應該顯示，`shouldDisplay` 回呼函式應返回 true，否則返回 false：

```typescript
const shouldDisplay = (app: Application) => {
  return application.metadata?.labels?.['application.environmentLabelKey'] === "prd";
};
```

以下是一個帶有彈出式小工具的簡單擴充功能範例：

```javascript
((window) => {
  const shouldDisplay = () => {
    return true;
  };
  const flyout = () => {
    return React.createElement(
            "div",
            { style: { padding: "10px" } },
            "This is a flyout"
    );
  };
  const component = () => {
    return React.createElement(
            "div",
            {
              onClick: () => flyout()
            },
            "Toolbar Extension Test"
    );
  };
  window.extensionsAPI.registerTopBarActionMenuExt(
          component,
          "Toolbar Extension Test",
          "Toolbar_Extension_Test",
          flyout,
          shouldDisplay,
          '',
          true
  );
})(window);
```
---
id: url-sync
title: URL 同步
---

Scenes 附帶一個 URL 同步系統，可啟用場景物件狀態與 URL 之間的雙向同步。

## UrlSyncContextProvider

若要啟用 URL 同步，您必須將您的根場景包裝在一個 UrlSyncContextProvider 中

```tsx
<UrlSyncContextProvider scene={scene} updateUrlOnInit={true} createBrowserHistorySteps={true} />
```

## SceneApp

對於使用 SceneApp 的場景應用程式，URL 同步會為您初始化，但您仍然可以在 SceneApp 狀態上設定 URL 同步選項。

```tsx
function getSceneApp() {
  return new SceneApp({
    pages: [],
    urlSyncOptions: {
      updateUrlOnInit: true,
      createBrowserHistorySteps: true,
    },
  });
}
```

## SceneObjectUrlSyncHandler

設定其 `_urlSync` 屬性的場景物件將可以選擇將其部分狀態同步到 URL 或從 URL 同步。

此屬性具有此介面類型：

```tsx
export interface SceneObjectUrlSyncHandler {
  getKeys(): string[];
  getUrlState(): SceneObjectUrlValues;
  updateFromUrl(values: SceneObjectUrlValues): void;
  shouldCreateHistoryStep?(values: SceneObjectUrlValues): boolean;
}
```

updateFromUrl 的目前行為有點奇怪，因為它只會傳遞與 getUrlState 傳回的值相比不同的 URL 值。

## 瀏覽器歷史記錄

如果啟用了 createBrowserHistorySteps，那麼對於 shouldCreateHistoryStep 傳回 true 的狀態變更，將會傳回新的瀏覽器歷史記錄狀態。

## SceneObjectUrlSyncConfig

此類別實作 SceneObjectUrlSyncHandler 介面，是一個公用程式類別，可讓場景物件更容易實作 URL 同步行為。

範例：

```tsx
export class SomeObject extends SceneObjectBase<SomeObjectState> {
  protected _urlSync = new SceneObjectUrlSyncConfig(this, { keys: ['from', 'to'] });

  public getUrlState() {
    return { from: this.state.from, to: this.state.to };
  }

  public updateFromUrl(values: SceneObjectUrlValues) {
    const update: Partial<SomeObjectState> = {};

    if (typeof values.from === 'string') {
      update.from = values.from;
    }

    if (typeof values.to === 'string') {
      update.to = values.to;
    }

    this.setState(update);
  }

  onUserUpdate(from: string, to: string) {
    // 對於應新增瀏覽器歷史記錄的狀態動作，請將其包裝在此回呼中
    this._urlSync.performBrowserHistoryAction(() => {
      this.setState({ from, to });
    });
  }
}
```
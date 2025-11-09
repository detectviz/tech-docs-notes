---
id: app-pages
title: 測試由應用程式新增的頁面
description: 如何測試由應用程式新增的頁面
keywords:
  - grafana
  - plugins
  - plugin
  - testing
  - e2e
  - app
sidebar_position: 10
---

# 測試由應用程式新增的頁面

`/a/${pluginId}/` 路由可用於存取由應用程式新增的頁面。為避免重複並無需在您的測試中匯入 `plugin.json`，我們提供了一組頁面類別和導覽函式。

本指南將說明如何建立一個特定於應用程式的 fixture，以簡化編寫測試並在不同測試之間共用邏輯。

## 具有基本 UI 的頁面

如果您想測試一個具有基本 UI 且易於透過標準 Playwright API 互動的頁面，那麼請使用像 `gotoPage` 這樣的導覽函式。

例如：

```ts title="fixtures.ts"
import { AppPage, test as base } from '@grafana/plugin-e2e';
import pluginJson from '../src/plugin.json';

type AppTestFixture = {
  gotoPage: (path?: string) => Promise<AppPage>;
};

export const test = base.extend<AppTestFixture>({
  gotoPage: async ({ gotoAppPage }, use) => {
    await use((path) =>
      gotoAppPage({
        path,
        pluginId: pluginJson.id,
      })
    );
  },
});

export { expect } from '@grafana/plugin-e2e';
```

若要使用此函式，只需從您的 fixture 匯入 `test` 和 `expect`，而不是從 `@grafana/plugin-e2e` 匯入，然後像平常一樣編寫您的測試。

例如：

```ts title="startPage.spec.ts"
import { test, expect } from './fixtures.ts';

test('起始頁面應歡迎使用者使用應用程式', async ({ gotoPage, page }) => {
  await gotoPage('/start');
  await expect(page.getByRole('heading', { name: 'Welcome to my app' })).toBeVisible();
});
```

## 具有複雜 UI 的頁面

另一方面，如果您有一個具有複雜 UI 的頁面，可能不易於透過標準 Playwright API 互動，那麼您需要一種不同的方法。最佳實踐是建立一個頁面物件，它可以將頁面的邏輯封裝在函式中。

如果您想在多個測試中重複使用該選擇器邏輯，這特別有幫助。以下範例展示了如何使用頁面物件擴充標準 fixture。我們簡化了 `getWelcomeText` 邏輯，以突顯頁面物件模式，而不會為此範例帶來不必要的複雜性。

例如：

```ts title="fixtures.ts"
import { AppPage, PluginTestCtx, PluginPageArgs, test as base } from '@grafana/plugin-e2e';
import pluginJson from '../src/plugin.json';

class StartPage extends AppPage {
  private path: string;

  constructor(ctx: PluginTestCtx, args: PluginPageArgs & { path: string }) {
    super(ctx, args);
    this.path = args.path;
  }

  goto(): Promise<void> {
    return super.goto({ path: this.path });
  }

  getWelcomeText(): Locator {
    const { page } = this.ctx;
    return page.getByRole('heading', { name: 'Welcome to my app' });
  }
}

type AppTestFixture = {
  startPage: StartPage;
};

export const test = base.extend<AppTestFixture>({
  startPage: async ({ page, selectors, grafanaVersion, request }, use, testInfo) => {
    const startPage = new StartPage(
      { page, selectors, grafanaVersion, request, testInfo },
      {
        pluginId: pluginJson.id,
        path: '/start',
      }
    );
    await startPage.goto();
    await use(startPage);
  },
});

export { expect } from '@grafana/plugin-e2e';
```

若要使用此程式碼，只需從您的 fixture 匯入 `test` 和 `expect`，而不是從 `@grafana/plugin-e2e` 匯入，然後像平常一樣編寫您的測試。當您在測試函式中解構 `startPage` 時，測試會自動導覽至該頁面。

例如：

```ts title="startPage.spec.ts"
import { test, expect } from './fixtures.ts';

test('起始頁面應歡迎使用者使用應用程式', async ({ startPage }) => {
  await expect(startPage.getWelcomeText()).toBeVisible();
});
```
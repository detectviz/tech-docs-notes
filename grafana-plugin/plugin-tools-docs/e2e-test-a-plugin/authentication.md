---
id: use-authentication
title: 驗證
description: 使用 plugin-e2e 工具為端對端測試新增驗證。
keywords:
  - grafana
  - plugins
  - plugin
  - testing
  - e2e
  - end-to-end
  - authentication
sidebar_position: 40
---

## 簡介

為了能夠與 Grafana UI 互動，您需要登入 Grafana。`@grafana/plugin-e2e` 提供了一種宣告式的方式來處理驗證和建立使用者。在本指南中，您將看到如何在您的插件中使用和不使用角色型存取控制 (RBAC) 的情況下執行此操作的範例。

## 不使用 RBAC 的插件

如果您的插件不使用 RBAC，您可以使用預設的伺服器管理員憑證登入。

在以下範例中，有一個名為 `auth` 的[設定專案](https://playwright.dev/docs/test-global-setup-teardown#setup-example)。此專案會呼叫 `@grafana/plugin-e2e` 套件中的一個函式，該函式會使用 `admin:admin` 登入 Grafana。已驗證的狀態會以此檔案名稱模式儲存在磁碟上：`<plugin-root>/playwright/.auth/<username>.json`。

第二個專案 `run-tests` 會執行 `./tests` 目錄中的所有測試。此專案會重複使用 `auth` 專案的驗證狀態。因此，登入只會發生一次，且 `run-tests` 專案中的所有測試都會在已驗證的狀態下開始。

```ts title="playwright.config.ts"
import { dirname } from 'path';
import { defineConfig, devices } from '@playwright/test';

const pluginE2eAuth = `${dirname(require.resolve('@grafana/plugin-e2e'))}/auth`;

export default defineConfig({
    ...
    projects: [
    {
      name: 'auth',
      testDir: pluginE2eAuth,
      testMatch: [/.*\.js/],
    },
    {
      name: 'run-tests',
      use: {
        ...devices['Desktop Chrome'],
        // @grafana/plugin-e2e 會將驗證狀態寫入此檔案，
        // 路徑不應修改
        storageState: 'playwright/.auth/admin.json',
      },
      dependencies: ['auth'],
    }
  ],
});
```

## 使用 RBAC 的插件

如果您的插件使用 RBAC，您可能會想編寫測試來驗證某些插件功能是否基於角色。

`@grafana/plugin-e2e` 工具可讓您在 Playwright 設定檔中定義具有角色的使用者。在以下範例中，在 `createViewerUserAndAuthenticate` 設定專案中建立了一個具有 `Viewer` 角色的新使用者。在下一個專案中，執行測試時會重複使用具有檢視者角色的使用者的驗證狀態。請注意，已將特定於 `Viewer` 角色的測試新增至專用的 `testDir`。

```ts title="playwright.config.ts"
import { dirname } from 'path';
import { defineConfig, devices } from '@playwright/test';

const pluginE2eAuth = `${dirname(require.resolve('@grafana/plugin-e2e'))}/auth`;

export default defineConfig<PluginOptions>({
  ...
  projects: [
      {
        name: 'createViewerUserAndAuthenticate',
        testDir: pluginE2eAuth,
        testMatch: [/.*auth\.setup\.ts/],
        use: {
          user: {
            user: 'viewer',
            password: 'password',
            role: 'Viewer',
          },
        },
      },
      {
        name: 'run-tests-for-viewer',
        testDir: './tests/viewer',
        use: {
          ...devices['Desktop Chrome'],
          // @grafana/plugin-e2e 會將驗證狀態寫入此檔案，
          // 路徑不應修改
          storageState: 'playwright/.auth/viewer.json',
        },
        dependencies: ['createViewerUserAndAuthenticate'],
      },
  ]
})
```

## 管理使用者

當在設定專案中定義了 `user`（如上述 RBAC 範例所示）時，`plugin-e2e` 將使用 Grafana HTTP API 建立使用者帳戶。此操作需要較高的權限，因此預設會使用伺服器管理員憑證 `admin:admin`。如果端對端測試的目標是使用 `create-plugin` 建立的[開發環境](../set-up/)，這將可以正常運作。但是對於其他測試環境，伺服器管理員密碼可能會不同。在這種情況下，我們會搜尋 GRAFANA_ADMIN_USER 和 GRAFANA_ADMIN_PASSWORD 環境變數。此外，您可以透過在全域選項中設定 `grafanaAPICredentials` 來提供正確的憑證。

```ts title="playwright.config.ts"
import { dirname } from 'path';
import { defineConfig, devices } from '@playwright/test';

const pluginE2eAuth = `${dirname(require.resolve('@grafana/plugin-e2e'))}/auth`;

export default defineConfig<PluginOptions>({
  testDir: './tests',
  use: {
    baseURL: process.env.GRAFANA_URL || 'http://localhost:3000',
    grafanaAPICredentials: {
      user: process.env.GRAFANA_ADMIN_USER || 'admin',
      password: process.env.GRAFANA_ADMIN_PASSWORD || 'admin',
    },
  },
  projects: [
    ...
  ]
})
```
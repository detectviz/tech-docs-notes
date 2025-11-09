---
id: feature-toggles
title: Grafana 功能開關
description: 如何在端對端測試中使用 Grafana 功能開關。
keywords:
  - grafana
  - plugins
  - plugin
  - testing
  - e2e
  - end-to-end
  - feature toggles
sidebar_position: 50
---

## 簡介

Grafana 使用一種稱為[功能開關](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/feature-toggles/)的機制，讓程式碼可以在執行階段「開啟」或「關閉」。插件可以選擇性地對功能開關的狀態做出反應，以適當地改變其行為；如果它們這樣做，您可以在您的端對端測試中涵蓋這一點。本指南描述了 `@grafana/plugin-e2e` 的功能，這些功能使處理功能開關變得更加容易。

## 將功能開關設定傳遞給 Grafana

在整個 Grafana 堆疊中設定功能開關最簡單的方法是，在啟動您的 Grafana 執行個體時指定環境變數。有關如何操作的說明，請參閱[我們的文件](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/#override-configuration-with-environment-variables)。

## 在端對端測試中覆寫前端功能開關

`@grafana/plugin-e2e` 工具可讓您覆寫 Grafana 設定使用的前端功能開關。您可以在 Playwright 設定檔中指定自訂選項 `featureToggles` 來執行此操作。

```typescript
// playwright.config.ts
import { defineConfig, devices } from '@playwright/test';
import { PluginOptions } from '@grafana/plugin-e2e';

export default defineConfig<PluginOptions>({
  testDir: './tests',
  reporter: 'html',
  use: {
    baseURL: 'http://localhost:3000',
    featureToggles: {
      experimentalPluginQueryEditor: false,
      experimentalPluginConfigEditor: true,
    },
  },
  ...
}
```

:::note

以這種方式定義的功能開關會傳播到 `window.grafanaBootData.settings.featureToggles`。這表示它們只會對 Grafana 的前端產生影響。如果您需要功能開關在整個堆疊中產生影響，請參閱[上一節](#passing-feature-toggle-configuration-to-grafana)。

:::

## 在特定的測試檔案中覆寫功能開關

若要為特定測試檔案中的測試覆寫功能開關，請使用如下程式碼。

```typescript
import { test, expect } from '@grafana/plugin-e2e';

test.use({
  featureToggles: {
    experimentalPluginQueryEditor: true,
  },
});
```

## 在您的測試中檢查功能是否已啟用

使用 `isFeatureToggleEnabled` fixture 來判斷某個功能開關是否已啟用。在底層，`isFeatureToggleEnabled` 會檢查給定的功能是否在 `window.grafanaBootData.settings.featureToggles` 物件中定義並啟用。

```typescript
import { test, expect } from '@grafana/plugin-e2e';
import * as semver from 'semver';

test('valid credentials should return a 200 status code', async ({
  createDataSourceConfigPage,
  page,
  isFeatureToggleEnabled,
}) => {
  const configPage = await createDataSourceConfigPage({ type: 'grafana-snowflake-datasource' });
  await configPage.getByGrafanaSelector('Data source connection URL').fill('http://localhost:9090');
  const isSecureSocksDSProxyEnabled = await isFeatureToggleEnabled('secureSocksDSProxyEnabled');
  if (isSecureSocksDSProxyEnabled) {
    page.getByLabel('Enabled').check();
  }
  await expect(configPage.saveAndTest()).toBeOK();
});
```
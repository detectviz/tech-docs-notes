---
id: annotation-queries
title: 測試註釋查詢
description: 測試自訂註釋編輯器和註釋查詢的執行。
keywords:
  - grafana
  - plugins
  - plugin
  - testing
  - e2e
  - data-source
  - annotation queries
sidebar_position: 50
---

## 簡介

註釋用於在視覺化圖表上標記事件，例如「AB 測試開始」或「行銷活動開始」。支援註釋的資料來源插件可用於查詢註釋事件。您可以選擇性地為資料來源插件實作自訂註釋編輯器，以協助使用者撰寫註釋查詢。

在許多情況下，註釋查詢的執行需要與一般資料查詢不同的處理方式，在這些情況下，我們建議您編寫端對端測試，以驗證資料來源註釋是否如預期般運作。

### 測試註釋編輯器

如果您的資料來源插件實作了自訂註釋編輯器，您可以編寫測試來驗證編輯器是否如預期般運作。如果您沒有實作自訂編輯器，那麼插件會使用內建編輯器。在這種情況下，就不需要編寫測試。

以下範例是一個簡單的 smoke 測試，用於驗證自訂註釋編輯器是否能載入：

```ts title="annotations.spec.ts"
import { expect, test } from '@grafana/plugin-e2e';

test('應呈現註釋編輯器', async ({ annotationEditPage, page, readProvisionedDataSource }) => {
  const ds = await readProvisionedDataSource({ fileName: 'datasources.yml' });
  await annotationEditPage.datasource.set(ds.name);
  await expect(page.getByRole('textbox', { name: 'Query Text' })).toBeVisible();
});
```

### 測試整個註釋查詢執行流程

在以下範例中，一個整合測試涵蓋了插件的整個註釋查詢資料流程：

:::note
請注意，從 Grafana 11.0.0 開始，註釋查詢結果會呈現在一個 Alert 元件中，因此在較早的版本中使用 `toHaveAlert` 匹配器將無法運作。
:::

```ts title="annotations.spec.ts"
import * as semver from 'semver';
import { expect, test } from '@grafana/plugin-e2e';

test('當查詢有效時，應成功執行並顯示成功警示框', async ({
  annotationEditPage,
  page,
  selectors,
  readProvisionedDataSource,
  grafanaVersion,
}) => {
  const ds = await readProvisionedDataSource({ fileName: 'datasources.yml' });
  await annotationEditPage.datasource.set(ds.name);
  await page.waitForFunction(() => window.monaco);
  await annotationEditPage.getByGrafanaSelector(selectors.components.CodeEditor.container).click();
  await page.keyboard.insertText(`select time as time, humidity as text
  from dataset
  where $__timeFilter(time) and humidity > 95`);
  await expect(annotationEditPage.runQuery()).toBeOK();
  if (semver.gte(grafanaVersion, '11.0.0')) {
    await expect(annotationEditPage).toHaveAlert('success');
  }
});
```

#### 測試錯誤情境

如果插件中發生錯誤，或者上游 API 傳回錯誤，您可能會想捕捉該錯誤並向使用者傳回有意義的錯誤訊息。

```ts title="annotations.spec.ts"
test('當回應中缺少時間欄位時，應失敗並顯示錯誤警示框', async ({
  annotationEditPage,
  page,
  selectors,
  readProvisionedDataSource,
  grafanaVersion,
}) => {
  const ds = await readProvisionedDataSource({ fileName: 'datasources.yml' });
  await annotationEditPage.datasource.set(ds.name);
  await page.waitForFunction(() => window.monaco);
  await annotationEditPage.getByGrafanaSelector(selectors.components.CodeEditor.container).click();
  await page.keyboard.insertText(`select humidity as text
  from dataset
  where humidity > 95`);
  await expect(annotationEditPage.runQuery()).not.toBeOK();
  if (semver.gte(grafanaVersion, '11.0.0')) {
    await expect(annotationEditPage).toHaveAlert('error', { hasText: 'Time field is missing' });
  }
});
```

### 在已佈建的儀表板中測試註釋

有時您可能會想為一個已存在的儀表板開啟註釋編輯頁面，並執行註釋查詢以確保一切如預期般運作。

```ts
test('已佈建儀表板中的註釋查詢應傳回 200 回應', async ({
  readProvisionedDashboard,
  gotoAnnotationEditPage,
  grafanaVersion,
}) => {
  const dashboard = await readProvisionedDashboard({ fileName: 'dashboard.json' });
  const annotationEditPage = await gotoAnnotationEditPage({ dashboard, id: '1' });
  await expect(annotationEditPage.runQuery()).toBeOK();
  if (semver.gte(grafanaVersion, '11.0.0')) {
    await expect(annotationEditPage).toHaveAlert('success', { hasText: /2 events.*/ });
  }
});
```
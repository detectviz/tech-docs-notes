## 決定要翻譯的文字

設定好插件的翻譯後，您可以繼續標記您要翻譯的語言字串。每個可翻譯的字串都會被指派一個唯一的索引鍵，最終會出現在 `locales/<locale>/<plugin id>.json` 下的每個翻譯檔案中。
以下範例使用 `t` 函式：

```diff
export const plugin = new PanelPlugin<SimpleOptions>(SimplePanel).setPanelOption
   return builder
     .addTextInput({
       path: 'text',
-      name: 'Simple text option',
-      description: 'Description of panel option',
-      defaultValue: 'Default value of text input option',
+      name: t('panel.options.text.name', 'Simple text option'),
+      description: t('panel.options.text.description', 'Description of panel option'),
+      defaultValue: t('panel.options.text.defaultValue', 'Default value of text input option'),
     })
     .addBooleanSwitch({
       path: 'showSeriesCount',
-      name: 'Show series counter',
+      name: t('panel.options.showSeriesCount.name', 'Show series counter'),
       defaultValue: false,
     })
     .addRadio({
       path: 'seriesCountSize',
       defaultValue: 'sm',
-      name: 'Series counter size',
+      name: t('panel.options.seriesCountSize.name', 'Series counter size'),
       settings: {
         options: [
           {
             value: 'sm',
-            label: 'Small',
+            label: t('panel.options.seriesCountSize.options.sm', 'Small'),
           },
           {
             value: 'md',
-            label: 'Medium',
+            label: t('panel.options.seriesCountSize.options.md', 'Medium'),
           },
           {
             value: 'lg',
-            label: 'Large',
+            label: t('panel.options.seriesCountSize.options.lg', 'Large'),
           },
         ],
       },
```

### 使用 `Trans` 元件的範例：

```diff
 import { SimpleOptions } from 'types';
 import { css, cx } from '@emotion/css';
 import { useStyles2, useTheme2 } from '@grafana/ui';
 import { PanelDataErrorView } from '@grafana/runtime';
+import { Trans } from '@grafana/i18n';

 interface Props extends PanelProps<SimpleOptions> {}

@@ -60,9 +61,15 @@ export const SimplePanel: React.FC<Props> = ({ options, data, width, height, fie

       <div className={styles.textBox}>
         {options.showSeriesCount && (
-          <div data-testid="simple-panel-series-counter">Number of series: {data.series.length}</div>
+          <div data-testid="simple-panel-series-counter">
+            <Trans i18nKey="components.simplePanel.options.showSeriesCount">
+              Number of series: {{ numberOfSeries: data.series.length }}
+            </Trans>
+          </div>
         )}
-        <div>Text option value: {options.text}</div>
+        <Trans i18nKey="components.simplePanel.options.textOptionValue">
+          Text option value: {{ optionValue: options.text }}
+        </Trans>
       </div>
     </div>
   );
```

## 取得翻譯後的文字

使用 `i18next` [剖析器](https://github.com/i18next/i18next-parser#readme)和 `i18n-extract` 來掃描所有輸入檔案、擷取標記的 `i18n` 鍵，並儲存翻譯。

### 剖析以進行翻譯

安裝 `i18next` 剖析器：

```shell npm2yarn
npm install --save-dev i18next-parser
```

接下來，建立一個設定檔 `src/locales/i18next-parser.config.js` 並進行設定，讓剖析器掃描您的插件並將翻譯擷取至 `src/locales/[$LOCALE]/[your-plugin].json`：

:::warning
路徑 `src/locales/[$LOCALE]/[your-plugin-id].json` 是強制性的。如果您修改它，翻譯將無法運作。
:::

```js title="src/locales/i18next-parser.config.js"
const pluginJson = require('../plugin.json');

module.exports = {
  locales: pluginJson.languages, // 您的插件支援的地區設定陣列
  sort: true,
  createOldCatalogs: false,
  failOnWarnings: true,
  verbose: false,
  resetDefaultValueLocale: 'en-US', // 當程式碼中的值變更時，更新擷取的值
  defaultNamespace: pluginJson.id,
  input: ['../**/*.{tsx,ts}'],
  output: 'src/locales/$LOCALE/$NAMESPACE.json',
};
```

### 取得您的翻譯檔案

將翻譯腳本 `i18n-extract` 新增至 `package.json`：

```json title="package.json"
  "scripts": {
    "i18n-extract": "i18next --config src/locales/i18next-parser.config.js"
  },
```

執行腳本以翻譯檔案：

```shell npm2yarn
npm run i18n-extract
```

翻譯檔案將類似於此：

```json title="src/locales/en-US/[your-plugin-id].json"
{
  "components": {
    "simplePanel": {
      "options": {
        "showSeriesCount": "Number of series: {{numberOfSeries}}",
        "textOptionValue": "Text option value: {{optionValue}}"
      }
    }
  },
  "panel": {
    "options": {
      "seriesCountSize": {
        "name": "Series counter size",
        "options": {
          "lg": "Large",
          "md": "Medium",
          "sm": "Small"
        }
      },
      "showSeriesCount": {
        "name": "Show series counter"
      },
      "text": {
        "defaultValue": "Default value of text input option",
        "description": "Description of panel option",
        "name": "Simple text option"
      }
    }
  }
}
```

## 測試已翻譯的插件

若要測試插件，請遵循[設定您的開發環境](../set-up/)中的步驟，以在本地執行您的插件。

然後，您可以透過[變更語言](https://grafana.com/docs/grafana/latest/administration/organization-preferences/#change-grafana-language)來驗證您的插件是否顯示適當的文字。

## 為翻譯設定 ESLint 規則

在 `eslint.config.mjs` 中新增 `@grafana/i18n` 規則：

```js title="eslint.config.mjs"
/* 現有匯入 */
import grafanaI18nPlugin from '@grafana/i18n/eslint-plugin';

export default defineConfig([
  /* 現有設定 */
  {
    name: 'grafana/i18n-rules',
    plugins: { '@grafana/i18n': grafanaI18nPlugin },
    rules: {
      '@grafana/i18n/no-untranslated-strings': ['error', { calleesToIgnore: ['^css$', 'use[A-Z].*'] }],
      '@grafana/i18n/no-translation-top-level': 'error',
    },
  },
]);
```

您可以在[此處](https://github.com/grafana/grafana/blob/main/packages/grafana-i18n/src/eslint/README.md)找到規則的更詳細說明。
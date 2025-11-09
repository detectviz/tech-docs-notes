---
id: extend-configurations
title: 擴充預設設定
description: 擴充您的開發環境工具設定 (webpack, eslint, prettier, jest)
keywords:
  - grafana
  - plugins
  - plugin
  - frontend
  - tooling
  - configuration
  - webpack
---

`.config/` 目錄存放了用於開發、測試和建置 Grafana 插件的不同工具的偏好設定。雖然您可以進行變更，但我們不建議這樣做。請遵循本主題中的指引來自訂您的工具設定。

:::danger

請勿編輯 `.config/` 目錄中的檔案。`create-plugin` 的 `update` 指令會覆寫此目錄中的任何變更。編輯這些檔案可能會導致您的插件無法編譯或在 Grafana 中載入。

請遵循本頁面上的說明進行進階設定，而不是直接變更檔案。

:::

### 擴充 ESLint 設定

編輯專案根目錄中的 `eslint.config.mjs` 檔案以擴充 ESLint 設定。以下範例停用了原始程式檔的棄用通知。

**範例：**

```javascript title="eslint.config.mjs"
import { defineConfig } from 'eslint/config';
import baseConfig from './.config/eslint.config.mjs';

export default defineConfig([
  {
    ignores: [
      //...
    ],
  },
  ...baseConfig,
  {
    files: ['src/**/*.{ts,tsx}'],
    rules: {
      '@typescript-eslint/no-deprecated': 'off',
    },
  },
]);
```

### 擴充 Prettier 設定

編輯專案根目錄中的 `.prettierrc.js` 檔案以擴充 Prettier 設定：

**範例：**

```js title=".prettierrc.js"
module.exports = {
  // @grafana/create-plugin 提供的 Prettier 設定
  ...require('./.config/.prettierrc.js'),
  semi: false,
};
```

### 擴充 Jest 設定

專案根目錄中有兩個屬於 Jest 的檔案：`jest-setup.js` 和 `jest.config.js`。

**`jest-setup.js`：** 此檔案會在測試套件中的每個測試檔案執行之前執行。它為測試函式庫設定 Jest DOM 並套用一些 polyfills。更多資訊，請參閱 [Jest 文件](https://jestjs.io/docs/configuration#setupfilesafterenv-array)。

**`jest.config.js`：** 這是擴充 Grafana 設定的 Jest 設定檔。更多資訊，請參閱 [Jest 設定文件](https://jestjs.io/docs/configuration)。

#### Jest 的 ESM 錯誤

如果您在執行 Jest 或 `npm run test` 時看到 `SyntaxError: Cannot use import statement outside a module`，請參閱[疑難排解](/troubleshooting#i-get-syntaxerror-cannot-use-import-statement-outside-a-module-when-running-jest-or-npm-run-test)。

### 擴充 TypeScript 設定

若要擴充 TS 設定，請編輯專案根目錄中的 `tsconfig.json` 檔案：

**範例：**

```json title="tsconfig.json"
{
  // @grafana/create-plugin 提供的 TypeScript 設定
  "extends": "./.config/tsconfig.json",
  "compilerOptions": {
    "preserveConstEnums": true
  }
}
```

### 擴充 Webpack 設定

請遵循以下步驟以擴充位於 `.config/` 中的 Webpack 設定：

#### 1. 建立一個新的 Webpack 設定檔

在專案根目錄中建立一個 `webpack.config.ts` 檔案。此檔案會擴充 `create-plugin` 提供的 Webpack 設定。

#### 2. 將 Grafana 設定與您的自訂設定合併

使用 [webpack-merge](https://github.com/survivejs/webpack-merge) 套件來擴充 `create-plugin` 設定：

```ts title="webpack.config.ts"
import type { Configuration } from 'webpack';
import { merge } from 'webpack-merge';
import grafanaConfig, { Env } from './.config/webpack/webpack.config';
import { BundleAnalyzerPlugin } from 'webpack-bundle-analyzer';

const config = async (env: Env): Promise<Configuration> => {
  const baseConfig = await grafanaConfig(env);

  return merge(baseConfig, {
    // 將一個 webpack 插件新增至設定中
    plugins: [new BundleAnalyzerPlugin()],
  });
};

export default config;
```

#### 3. 更新 `package.json` 以使用新的 Webpack 設定

更新 `package.json` 中的 `scripts` 以使用擴充的 Webpack 設定：

```diff title="package.json"
-"build": "webpack -c ./.config/webpack/webpack.config.ts --env production",
+"build": "webpack -c ./webpack.config.ts --env production",
-"dev": "webpack -w -c ./.config/webpack/webpack.config.ts --env development",
+"dev": "webpack -w -c ./webpack.config.ts --env development",
```

#### 自訂 Webpack 設定範例

以下範例將「libs」目錄從 typescript/javascript 編譯中排除，以防止在原始程式碼中直接匯入已打包的函式庫時發生建置或執行階段失敗。

```ts title="webpack.config.ts"
import type { Configuration } from 'webpack';
import { mergeWithRules } from 'webpack-merge';
import grafanaConfig, { Env } from './.config/webpack/webpack.config';

const config = async (env: Env): Promise<Configuration> => {
  const baseConfig = await grafanaConfig(env);
  const customConfig = {
    module: {
      rules: [
        {
          exclude: /(node_modules|libs)/,
          test: /\.[tj]sx?$/,
        },
      ],
    },
  };
  return mergeWithRules({
    module: {
      rules: {
        exclude: 'replace',
      },
    },
  })(baseConfig, customConfig);
};

export default config;
```

Webpack 5 不會自動 polyfill [Node.js 核心模組](https://webpack.js.org/configuration/resolve/#resolvefallback)。以下範例顯示如果您的插件使用 Node.js polyfills，如何新增它們。

```ts title="webpack.config.ts"
import type { Configuration } from 'webpack';
import { merge } from 'webpack-merge';
import grafanaConfig, { Env } from './.config/webpack/webpack.config';

const config = async (env: Env): Promise<Configuration> => {
  const baseConfig = await grafanaConfig(env);

  return merge(baseConfig, {
    resolve: {
      fallback: {
        crypto: require.resolve('crypto-browserify'),
        fs: false,
        path: require.resolve('path-browserify'),
        stream: require.resolve('stream-browserify'),
        util: require.resolve('util'),
      },
    },
  });
};

export default config;
```
---
id: plugin-spellcheck
title: 插件拼字檢查（內部）
draft: true
description: 關於如何設定插件拼字檢查的內部文件。
keywords:
  - grafana
  - plugins
  - spellcheck
---

# 設定插件拼字檢查

:::info
此流程僅適用於 Grafana 維護的插件，且僅當插件在 drone.grafana.net 中為 CI 流程啟動時適用。
:::

## 它是什麼以及為何需要它

拼字檢查 CI 步驟會對插件程式碼和文件執行基本的拼字檢查。
它幫助我們避免向客戶顯示如下所示的內容：

![image](https://user-images.githubusercontent.com/1436174/208397307-4270cb57-b538-4c68-8b0f-67ab5d3b8dad.png)

目前，所有內部插件都必須進行拼字檢查。

在底層，管線使用 [cspell npm 套件](https://www.npmjs.com/package/cspell)來執行拼字檢查。

## 設定拼字檢查的步驟

如果您是從失敗的 CI 中的連結來到這裡，那麼很可能您尚未為您的插件設定拼字檢查。請遵循以下步驟進行設定。

1. 將 cspell 套件安裝到您的插件儲存庫：

   ```bash
   npm install --save-dev cspell@6.13.3
   ```

2. 在您插件的 `package.json` 中的 `scripts` 區段新增 `spellcheck` 指令：

   ```json title="package.json"
   "spellcheck": "cspell -c cspell.config.json \"**/*.{ts,tsx,js,go,md,mdx,yml,yaml,json,scss,css}\""
   ```

3. 在儲存庫根目錄中建立一個 `cspell.config.json` 檔案，並在其中新增基本設定：

   ```json title="cspell.config.json"
   {
     "ignorePaths": [
       "coverage/**",
       "cypress/**",
       "dist/**",
       "go.sum",
       "mage_output_file.go",
       "node_modules/**",
       "provisioning/**/*.yaml",
       "src/dashboards/*.json",
       "**/testdata/**/*.json",
       "**/testdata/**/*.jsonc",
       "vendor/**",
       "cspell.config.json",
       "package.json",
       "yarn.lock",
       "docker-compose*.yaml",
       "docker-compose*.yml"
     ],
     "ignoreRegExpList": [
       // 忽略多行匯入
       "import\\s*\\((.|[\r\n])*?\\)",
       // 忽略單行匯入
       "import\\s*.*\".*?\""
     ],
     "words": ["grafana", "datasource", "datasources"]
   }
   ```

4. 執行 `npm run spellcheck` 以查看是否有任何拼寫錯誤
5. 如果發現錯誤，請修正它們或將其新增至先前建立的 `cspell.config.json` 的 `ignorePaths` 或 `words` 區段

將拼字檢查新增至您的儲存庫的範例 PR：https://github.com/grafana/athena-datasource/pull/185/files
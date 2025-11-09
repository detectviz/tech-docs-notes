---
id: troubleshooting
title: 疑難排解
description: 解決您在 Grafana 插件開發中遇到的問題。
keywords:
  - grafana
  - plugins
  - documentation
  - troubleshooting
  - Windows
  - WSL
sidebar_position: 55
---

# 疑難排解

如果您在使用 `create-plugin` 或 `sign-plugin` 工具時遇到問題，可以嘗試以下方法進行檢查。

### 我收到：`Unsupported operating system 'Windows' detected. Please use WSL with create-plugin`

`create-plugin` 工具不支援原生 Windows。如果您使用 Windows，則必須安裝 WSL 2 才能使用 `create-plugin`。WSL 2 是 Windows Subsystem for Linux 架構的新版本，可讓您在 Windows 上執行 ELF64 Linux 二進位檔案。

#### 我正在使用 Windows 搭配 WSL，但仍然收到 `Unsupported operating system 'Windows' detected.`

請確保您正在使用 WSL 終端機。僅在 Windows 上安裝 WSL 是不夠的；您需要設定您的 WSL 環境。

若要檢查您是否正在使用 WSL 終端機，請執行以下指令：`uname -a`。此指令應傳回類似以下的內容：

`Linux ....-microsoft-standard-WSL2 #... GNU/Linux`

如果您收到錯誤或不同的輸出，表示您未使用 WSL 終端機。

#### 我可以確認我正在 WSL 終端機內，但我仍然收到 `Unsupported operating system 'Windows' detected.`

您必須在您的 WSL 環境中安裝 Node.js。您可以在 Microsoft 的[文件](https://learn.microsoft.com/en-us/windows/dev-environment/javascript/nodejs-on-wsl)中找到安裝指南。我們建議安裝最新 LTS 版本的 Node.js。

#### 我已在 WSL 內安裝 Node.js，但我仍然收到 `Unsupported operating system 'Windows' detected.`

您可能已在 WSL 中安裝 Node.js，但未使用最新的 LTS 版本。此外，WSL 有一個問題，它可能會拾取 Windows 二進位檔案並嘗試執行它們。這表示即使您在 WSL 終端機內，您可能仍在執行 `create-plugin` 不支援的 Windows 二進位檔案。

##### 您可以怎麼做：

- 執行 `node --version` 檢查您是否正在使用 Node.js 18 或更新版本。
- 執行以下指令檢查您是否正在使用 Linux 二進位檔案的 Node.js 和 npx：`which node` 和 `which npx`（請注意，指令是 `which`，而不是 `node --version`）。您應該會收到類似 `/usr/bin/node` 和 `/usr/bin/npx` 的輸出。如果這些指令的輸出是類似 `..../Program Files/nodejs/npx` 的內容，表示您正在使用錯誤的二進位檔案，應安裝或重新安裝 Node.js。
- 您可以遵循 [Microsoft 指南](https://learn.microsoft.com/en-us/windows/dev-environment/javascript/nodejs-on-wsl)來正確安裝 Node.js。

:::note

如果您在未遵循任何指南或變更 APT 儲存庫的情況下使用 `sudo apt install nodejs` 安裝 Node.js，那麼您很可能未使用最新的 Node.js LTS。

:::

### 當我使用我新建立的插件開啟 Grafana 時，Grafana 無法載入該插件。

- 首先，嘗試重新啟動 Grafana，以確保它能偵測到新的插件。
- 如果您正在使用原生 Windows（不含 WSL），您需要使用 WSL。`create-plugin` 工具不支援原生 Windows。

### 當我在我的插件中執行 `npm run build` 或 `npm run dev` 時，我看不到我的變更反映出來。

如果您正在使用掛載來存取您的檔案，很可能是 webpack 沒有偵測到您的檔案變更。跨 WSL 和 Windows 檔案系統處理檔案儲存和效能可能會出現問題。此問題與 `create-plugin` 無關，而是 WSL 和 Windows 的運作方式所致。

##### 您可以怎麼做：

- 如果您正在從原生 Windows 應用程式（例如 VS Code）編輯您的程式碼，您需要在每次想要看到插件變更時手動重新執行 `yarn build`。
- 在您的專案中使用 [webpack `watchOption` 搭配 `poll`](https://webpack.js.org/configuration/watch/#watchoptionspoll)。嘗試執行包含輪詢的 [create-plugin update 指令](/reference/cli-commands#update)。

### 當我執行 Jest 或 `npm run test` 時，收到 `SyntaxError: Cannot use import statement outside a module`。

目前 Jest 設定的一個常見問題是匯入一個只提供 ESM 組建的 npm 套件。這些套件會導致 Jest 錯誤，並顯示 `SyntaxError: Cannot use import statement outside a module`。

為了解決這個問題，我們提供了一個已知套件的列表，以傳遞給 `[transformIgnorePatterns](https://jestjs.io/docs/configuration#transformignorepatterns-arraystring)` Jest 設定屬性。

如果需要，可以透過以下方式擴充：

```javascript
process.env.TZ = 'UTC';
const { grafanaESModules, nodeModulesToTransform } = require('./.config/jest/utils');

module.exports = {
  // @grafana/create-plugin 提供的 Jest 設定
  ...require('./.config/jest.config'),
  // 通知 Jest 僅轉換特定的 node_module 套件。
  transformIgnorePatterns: [nodeModulesToTransform([...grafanaESModules, 'packageName'])],
};
```

### 執行 `docker compose up` 或 `npm run server` 後，我收到 `"image with reference <plugin-name> was found but does not match the specified platform: wanted linux/amd64, actual: linux/arm64/v8"`。

此錯誤最有可能影響使用配備 Apple 晶片的 Mac 電腦使用者。如果您先前曾為使用 v1.12.2 之前的 `create-plugin` 建立的插件建置映像檔，那麼如果舊的映像檔尚未移除，執行 `docker compose up` 可能會失敗並顯示上述訊息。

##### 您可以怎麼做：

- 執行 `docker compose down` 以停止並移除容器。
- 使用 `docker rmi <plugin-name>` 移除映像檔。
- 執行 `docker compose up` 或 `npm run server`。
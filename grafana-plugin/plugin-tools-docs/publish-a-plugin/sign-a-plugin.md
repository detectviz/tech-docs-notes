---
id: sign-a-plugin
title: 簽署插件
sidebar_position: 3
description: 如何簽署 Grafana 插件。
keywords:
  - grafana
  - plugins
  - plugin
  - sign plugin
  - signing plugin
---

# 簽署插件

所有 Grafana Labs 撰寫的插件，包括企業版插件，都經過簽署，以便我們可以使用[簽章驗證](https://grafana.com/docs/grafana/latest/administration/plugin-management#plugin-signatures)來驗證其真實性。預設情況下，Grafana [要求](https://grafana.com/docs/grafana/latest/administration/plugin-management#allow-unsigned-plugins)所有插件都必須經過簽署才能載入。

:::info

在開發期間或首次提交插件進行審核時，您無需簽署插件。使用 `@grafana/create-plugin` 建立的 [Docker 開發環境](../set-up/)預設設定為在[開發模式](https://github.com/grafana/grafana/blob/main/contribute/developer-guide.md#configure-grafana-for-development)下執行，這可讓您在沒有簽章的情況下載入插件。

:::

## 公開或私有插件

插件可以根據其作者、相關技術和預期用途具有不同的[簽章等級](https://grafana.com/legal/plugins/#what-are-the-different-classifications-of-plugins)。

插件可以是_公開的_或_私有的_：

- 公開插件以社群或商業形式簽署。它們在 [Grafana 插件目錄](https://grafana.com/plugins)中分發，可供其他人安裝。
- 私有插件僅可在您的組織內使用。

在簽署您的插件之前，請檢閱[插件政策](https://grafana.com/legal/plugins/)以確定適合您插件的簽章。

## 產生存取原則權杖

為了驗證您插件的所有權，請產生一個存取原則權杖，您每次需要簽署新版插件時都會用到它。

1. [建立一個 Grafana Cloud 帳戶](https://grafana.com/signup)。

2. 登入您的帳戶，然後前往 **我的帳戶 > 安全性 > 存取原則**。

3. 按一下 **建立存取原則**。

   領域：必須是 **your-org-name** (all-stacks)
   範圍：**plugins:write**

   ![建立存取原則。](/img/create-access-policy-v2.png)

4. 按一下 **建立權杖** 以建立新權杖。

   **到期日**是可選的，但您應定期變更權杖以提高安全性。

   ![建立存取原則權杖。](/img/create-access-policy-token.png)

5. 按一下 **建立**，然後將權杖的副本儲存在安全的地方以備將來參考。

6. 繼續簽署您的[公開插件](#sign-a-public-plugin)或[私有插件](#sign-a-private-plugin)。

## 簽署公開插件

公開插件需要經過 Grafana 團隊的審核，然後您才能簽署它們。

1. 提交您的插件以進行[審核](./publish-or-update-a-plugin.md)。
2. 如果我們核准您的插件，您將獲得一個插件簽章等級。您需要此簽章等級才能繼續。
3. 在您的插件目錄中，使用您剛建立的權杖將存取原則權杖匯出為環境變數。

   ```bash
   export GRAFANA_ACCESS_POLICY_TOKEN=<YOUR_ACCESS_POLICY_TOKEN>
   ```

4. 接下來，簽署插件。Grafana sign-plugin 工具會在您插件的 `dist` 目錄中建立一個 [MANIFEST.txt](#add-a-plugin-manifest-for-verification) 檔案：

   ```shell npm2yarn
   npx @grafana/sign-plugin@latest
   ```

## 簽署私有插件

1. 在您的插件目錄中，使用您剛建立的權杖將存取原則權杖匯出為環境變數。

   ```bash
   export GRAFANA_ACCESS_POLICY_TOKEN=<YOUR_ACCESS_POLICY_TOKEN>
   ```

2. 接下來，簽署插件。Grafana sign-plugin 工具會在您插件的 `dist` 目錄中建立一個 [MANIFEST.txt](#add-a-plugin-manifest-for-verification) 檔案。在 `rootUrls` 旗標之後，輸入您打算安裝插件的 Grafana 執行個體的 URL 的逗號分隔清單：

   ```shell npm2yarn
   npx @grafana/sign-plugin@latest --rootUrls https://example.com/grafana
   ```

## 新增插件資訊清單以進行驗證

為了讓 Grafana 驗證插件的數位簽章，請包含一個已簽署的資訊清單檔案 `MANIFEST.txt`。已簽署的資訊清單檔案包含兩個部分：

- **已簽署的訊息 -** 包含插件元資料和帶有各自校驗和 (SHA256) 的插件檔案。
- **數位簽章 -** 透過使用私密金鑰加密已簽署的訊息而建立。Grafana 內建了一個公開金鑰，可用於驗證數位簽章是否已使用預期的私密金鑰進行加密。

**範例**

```txt
-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA512

{
  "manifestVersion": "2.0.0",
  "signatureType": "community",
  "signedByOrg": "myorgid",
  "signedByOrgName": "My Org",
  "plugin": "myorgid-simple-panel",
  "version": "1.0.0",
  "time": 1602753404133,
  "keyId": "7e4d0c6a708866e7",
  "files": {
    "LICENSE": "12ab7a0961275f5ce7a428e662279cf49bab887d12b2ff7bfde738346178c28c",
    "module.js.LICENSE.txt": "0d8f66cd4afb566cb5b7e1540c68f43b939d3eba12ace290f18abc4f4cb53ed0",
    "module.js.map": "8a4ede5b5847dec1c6c30008d07bef8a049408d2b1e862841e30357f82e0fa19",
    "plugin.json": "13be5f2fd55bee787c5413b5ba6a1fae2dfe8d2df6c867dadc4657b98f821f90",
    "README.md": "2d90145b28f22348d4f50a81695e888c68ebd4f8baec731fdf2d79c8b187a27f",
    "module.js": "b4b6945bbf3332b08e5e1cb214a5b85c82557b292577eb58c8eb1703bc8e4577"
  }
}
-----BEGIN PGP SIGNATURE-----
Version: OpenPGP.js v4.10.1
Comment: https://openpgpjs.org

wqEEARMKAAYFAl+IE3wACgkQfk0ManCIZudpdwIHTCqjVzfm7DechTa7BTbd
+dNIQtwh8Tv2Q9HksgN6c6M9nbQTP0xNHwxSxHOI8EL3euz/OagzWoiIWulG
7AQo7FYCCQGucaLPPK3tsWaeFqVKy+JtQhrJJui23DAZLSYQYZlKQ+nFqc9x
T6scfmuhWC/TOcm83EVoCzIV3R5dOTKHqkjIUg==
=GdNq
-----END PGP SIGNATURE-----
```

## 疑難排解

### 為何我會收到「已修改的簽章」錯誤？

在某些情況下，由於在 Windows 上簽署插件時出現問題，會產生無效的 `MANIFEST.txt`。您可以透過將 `MANIFEST.txt` 檔案中的所有雙反斜線 `\\` 替換為正斜線 `/` 來修正此問題。每次簽署您的插件時都需要執行此操作。

### 為何我的公開插件會收到「欄位為必填：`rootUrls`」錯誤？

對於_公開_插件，您的插件尚未指派插件簽章等級。Grafana 團隊成員會在審核並核准您的插件後為其指派一個簽章等級。有關更多資訊，請參閱[簽署公開插件](#sign-a-public-plugin)。

### 為何我的私有插件會收到「欄位為必填：`rootUrls`」錯誤？

對於_私有_插件，您需要在 `plugin:sign` 指令中新增一個 `rootUrls` 旗標。`rootUrls` 必須符合 [`root_url`](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana#root_url) 設定。有關更多資訊，請參閱[簽署私有插件](#sign-a-private-plugin)。

如果您仍然收到此錯誤，請確保存取原則權杖是由與插件 ID 的第一部分相符的 Grafana Cloud 帳戶產生的。
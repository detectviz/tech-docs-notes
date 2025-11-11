# 發行

## 簡介

Argo CD 使用 GitHub actions 以兩步驟自動化方式發行。發行過程大約需要 60 分鐘，
有時會少一點，取決於 GitHub Actions 執行器的效能。

目標發行分支必須已存在於 GitHub 儲存庫中。例如，如果您
想建立一個 `v2.7.0` 版本，則對應的發行分支
`release-2.7` 必須存在，否則無法建置該版本。此外，
觸發標籤應始終在您本地儲存庫副本中簽出的發行分支中建立。

在觸發發行自動化之前，應將 `CHANGELOG.md` 更新
為最新資訊，並將此變更提交並推送到
GitHub 儲存庫的發行分支。之後，即可觸發自動化。
這在不久的將來將會自動化。

**版本建立前的手動步驟：**

*   使用此版本的變更更新 `CHANGELOG.md`
*   提交並推送對 `CHANGELOG.md` 的變更
*   在 `goreleaser.yaml` 的 `Release Notes Blog Post` 部分更新部落格文章連結。

**`Init ArgoCD Release` 工作流程將執行以下步驟：**

*   在發行分支中更新 `VERSION` 檔案
*   在發行分支中以新版本的映像標籤更新清單
*   建立一個 pull request 以提交上述變更

**`Publish ArgoCD Release` 工作流程將執行以下步驟：**

*   建置、推送並簽署容器映像到 Quay.io
*   為容器映像產生一個來源證明
*   建置 CLI 二進位檔、發行說明，然後建立一個 GitHub 版本並附加必要的資產。
*   為 CLI 二進位檔產生一個來源證明
*   產生並簽署一個 sbom
*   在適用時更新穩定標籤
*   當新版本為 GA 時，在 master 分支中更新 `VERSION` 檔案

## 步驟

### 步驟 1 - 更新版本和清單

1.  確保 TARGET_BRANCH 已存在。
2.  訪問 [Release GitHub Action](https://github.com/argoproj/argo-cd/actions/workflows/init-release.yaml)
    並選擇您要從哪個分支開始工作。
3.  輸入要簽出的 TARGET_BRANCH。
4.  輸入將用於建置清單和 `VERSION` 檔案的 TARGET_VERSION。（例如 `2.7.0-rc1`）

![GitHub Release Action](../assets/release-action.png)

當 action 完成後，將會產生一個包含已更新清單和 `Version` 檔案的 pull request。

5.  合併 pull request 並繼續步驟 2。

### 步驟 2 - 標記發行分支

以下步驟需要由在 Argo CD 上游 repo 中具有寫入權限的人執行。

1.  簽出發行分支。例如：`git fetch upstream && git
    checkout release-2.7`
2.  如下所示執行位於 `hack/trigger-release.sh` 的指令碼：

```shell
./hack/trigger-release.sh <version> <remote name>
```

範例：
```shell
./hack/trigger-release.sh v2.7.2 upstream
```

> [!TIP]
> 標籤必須是以下格式之一才能觸發 GH 工作流程：<br>
> *   GA: `v<MAJOR>.<MINOR>.<PATCH>`<br>
> *   預發行版: `v<MAJOR>.<MINOR>.<PATCH>-rc<RC#>`

指令碼成功執行後，一個 GitHub 工作流程將開始
執行。您可以在 [Actions](https://github.com/argoproj/argo-cd/actions/workflows/release.yaml) 標籤下追蹤其進度，該 action 的名稱是 `Publish ArgoCD Release`。

> [!WARNING]
> 您不能在同一個發行分支上同時執行多個發行。

### 驗證自動化發行

自動化版本建立完成後，您應該執行手動
檢查以查看版本是否正確發行：

*   檢查 GitHub action 的狀態和輸出
*   檢查 [https://github.com/argoproj/argo-cd/releases](https://github.com/argoproj/argo-cd/releases)
    以查看版本是否已正確建立，以及所有必要的資產
    是否已附加。
*   檢查映像是否已正確發布到 Quay.io

### 如果出了問題

如果出了問題，損害應該是有限的。根據已
執行的步驟，您將需要手動清理。

*   如果容器映像已推送到 Quay.io，請將其刪除
*   從 GitHub 的 `Releases` 頁面刪除版本（如果已建立）

### 手動發行

發行流程不允許手動發行過程。映像簽章和來源證明需要使用 GitHub Actions 建立。

## 涉及發行流程的重要檔案

| 檔案                               | 說明                                            |
|------------------------------------|--------------------------------------------------------|
|goreleaser.yaml                     |用於建置 CLI 二進位檔、校驗和、發行說明的設定  |
|.github/workflows/image-reuse.yaml  |用於產生容器映像的可重複使用的工作流程     |
|.github/workflows/init-release.yaml |用於產生清單和 `VERSION` 檔案            |
|.github/workflows/release.yaml      |建置映像、CLI 二進位檔、來源證明、sbom、後續工作 |
|./hack/trigger-release.sh           |確保所有先決條件都已滿足並推送標籤    |
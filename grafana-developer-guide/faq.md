# 貢獻常見問題

## 一般

### 我可以在哪裡討論我的貢獻想法？

當然可以！您可以在我們的 GitHub 議題追蹤器中開啟一個增強提案，或者您可以[在 Slack 上加入我們](https://argoproj.github.io/community/join-slack)，在 #argo-contributors 頻道中討論您的想法並獲得提交 PR 的指導。

> [!NOTE]
> 每週都會舉行定期的[貢獻者會議](https://argo-cd.readthedocs.io/en/latest/developer-guide/code-contributions/#regular-contributor-meeting)。請點擊連結以獲取更多詳細資訊。

### 還沒有人看我的 PR。為什麼？

由於我們的資源有限，有時可能需要一段時間才有人回應您的 PR。特別是當您的 PR 包含複雜或不明顯的變更時。請耐心等待，我們會盡力查看收到的每一個 PR。請確保您的 PR 清單中已符合所有適用的要求。

### 如何讓我的 PR 被標記為 `ready-for-review`？

按照慣例，初步審查由 Argo 成員或審查員執行。一旦初步審查通過，就可以將其標記為 `ready-for-review`，然後新增到 [Argo CD 審查](https://github.com/orgs/argoproj/projects/28) Github 專案中。專案儀表板的詳細資訊可以在[這裡](https://github.com/orgs/argoproj/projects/28?pane=info)找到。

我們非常鼓勵社群進行高品質的審查。成員/審查員可以與社群審查員合作，讓 PR 被標記為 `ready-for-review`。然後可以將其新增到專案儀表板並標記為 `社群審查`。

### 為什麼我的 PR 被拒絕了？我投入了這麼多心血！

我們感謝您將寶貴的時間和知識投入到貢獻中。然而，有些變更不符合 ArgoCD 的整體理念，因此無法合併到官方的 ArgoCD 原始碼樹中。

為保險起見，請確保在開始處理您的 PR 之前，您已為您的變更建立了一個增強提案，並從社群和維護者那裡收集了足夠的回饋。

### 我的 PR 有一個檢查失敗了。
請參閱[失敗的 CI 檢查](ci.md#troubleshooting-ci-checks)。

### 哪些簽入的程式碼是產生的，以及它是如何產生的？
本儲存庫下的以下檔案是產生的，必須保持最新。另請參閱[為什麼 codegen 步驟會失敗？](ci.md#why-does-the-codegen-step-fail)。

請參閱 Makefile 以了解也可以執行這些指令碼的目標，以及執行所有這些指令碼的 `codegen` 目標。

| 檔名 | 目的 | 產生者 |
| -------- | ------- | ------------ |
| `*.pb.go`, `*.pb.gw.go` | [Protobuf](https://developers.google.com/protocol-buffers/docs/gotutorial) 介面 | `hack/generate-proto.sh` |
| `assets/swagger.json` | Swagger 2 API 規格 | `hack/update-openapi.sh` |
| `manifests/` | k8s 安裝清單 | `hack/update-manifests.sh` |
| `docs/user-guide/commands` | CLI 文件 | `tools/cmd-docs/main.go` |
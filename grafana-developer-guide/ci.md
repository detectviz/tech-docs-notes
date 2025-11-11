# 持續整合 (CI)

## 排除 CI 檢查故障

您可以點擊失敗步驟旁的「詳細資訊」連結，以獲取有關失敗的更多資訊。

![失敗的 GitHub Action](ci-pipeline-failed.png)

若要閱讀更多關於 GitHub actions 的設定，請參閱 [`ci-build.yaml`](https://github.com/argoproj/argo-cd/blob/master/.github/workflows/ci-build.yaml)。

### 我可以在不推送新提交的情況下重新觸發檢查嗎？

由於 CI 管線是在 Git 提交時觸發的，因此目前（已知）沒有辦法在不向您的分支推送新提交的情況下重新觸發 CI 檢查。

如果您絕對確定失敗是由於管線中的故障，而不是您提交的變更中的錯誤，您可以向您的分支推送一個空的提交，從而在沒有任何程式碼變更的情況下重新觸發管線。為此，請執行：

```bash
git commit -s --allow-empty -m "Retrigger CI pipeline"
git push origin <yourbranch>
```

### 為什麼建置步驟會失敗？

首先，請確保失敗的建置步驟在您的機器上可以成功。請記住，也可以使用容器化的建置工具鏈。

如果建置在 `Ensure Go modules synchronicity` 步驟失敗，您需要先透過 `go mod download` 在本機下載所有 Go 依賴模組，然後執行 `go mod tidy` 以確保依賴的 Go 模組已整理乾淨。最後，將 `go.mod` 和 `go.sum` 的變更提交並推送到您的分支。

如果建置在 `Build & cache Go code` 步驟失敗，您需要確保 `make build-local` 在您的本機上成功執行。

### 為什麼 codegen 步驟會失敗？

如果 codegen 步驟失敗並顯示「Check nothing has changed...」，很有可能是您沒有執行 `make codegen`，或者沒有提交它所做的變更。您應該透過在您分支的本地工作副本中執行 `make codegen` 後接著執行 `git status` 來再次檢查。提交任何變更並將它們推送到您的 GH 分支，以讓 CI 再次檢查它。

另一個常見的情況是，當您修改了任何自動產生的資產時，因為這些資產會在執行 `make codegen` 時被覆寫。

一般來說，此步驟會執行 `codegen` 並將結果與其簽出的 Git 分支進行比較。如果存在差異，該步驟將會失敗。

有關更多資訊，請參閱[哪些簽入的程式碼是產生的，以及它來自何處？](faq.md#what-checked-in-code-is-generated-and-how-is-it-generated)。

### 為什麼 lint 步驟會失敗？

您的程式碼未能正確通過 lint 檢查，或者 `golangci-lint` 程序執行了修改。

*   您應該在您的本地分支上執行 `make lint` 或 `golangci-lint run`，並修復所有問題。

*   如果您收到類似 ```File is not `goimports`-ed (goimports)``` 的錯誤，表示檔案格式不正確。執行 `gofmt -w $file.go` 來解決此 linter 錯誤。

### 為什麼 test 或 e2e 步驟會失敗？

您應該如上所述，在檢查的詳細資訊頁面中檢查失敗的原因。這將為您提供失敗的測試名稱以及失敗原因的詳細資訊。如果您的測試在本機（使用虛擬化工具鏈）通過，那麼該測試很可能是 flaky 的，下次執行時可能會通過。請如上所述重新觸發 CI 管線，看看測試步驟現在是否通過。

## 更新建構器映像

登入 Docker Hub：

```bash
docker login
```

建置映像：

```bash
make builder-image IMAGE_NAMESPACE=argoproj IMAGE_TAG=v1.0.0
```

## 公開 CD

每次對 master 的提交都會被建置並發布到 `ghcr.io/argoproj/argo-cd/argocd:<version>-<short-sha>`。映像列表可在
[https://github.com/argoproj/argo-cd/packages](https://github.com/argoproj/argo-cd/packages) 取得。

> [!NOTE]
> GitHub docker registry [要求](https://github.community/t5/GitHub-Actions/docker-pull-from-public-GitHub-Package-Registry-fail-with-quot/m-p/32888#M1294) 即使是公開可用的套件也需要進行驗證才能讀取。
> 如果您想使用 `ghcr.io/argoproj/argo-cd/argocd` 映像，請遵循 Kubernetes [文件](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry)
> 中的步驟來設定映像提取 secret。

該映像會自動部署到開發 Argo CD 實例：[https://cd.apps.argoproj.io/](https://cd.apps.argoproj.io/)
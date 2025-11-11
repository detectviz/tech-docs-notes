# 靜態程式碼分析

我們使用以下靜態程式碼分析工具：

*   `golangci-lint` 和 `eslint` 用於編譯時的程式碼檢查
*   [codecov.io](https://codecov.io/gh/argoproj/argo-cd) - 用於程式碼覆蓋率
*   [snyk.io](https://app.snyk.io/org/argoproj/projects) - 用於映像掃描
*   [sonarcloud.io](https://sonarcloud.io/organizations/argoproj/projects) - 用於程式碼掃描和安全性警報

這些工具至少每天或在每個 pull request 上執行一次。
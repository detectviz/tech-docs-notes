# 文件網站

## 開發與測試

[文件網站](https://argo-cd.readthedocs.io/)是使用 `mkdocs` 和 `mkdocs-material` 建置的。

若要測試：

```bash
make serve-docs
```
執行後，您可以在 [http://0.0.0.0:8000/](http://0.0.0.0:8000/) 檢視您在本機建置的文件。
對文件進行變更將會自動重建並重新整理檢視。

在提交 PR 之前，請先建置網站，以驗證建置網站時沒有任何錯誤。
```bash
make build-docs
```

如果您想在不使用 docker 容器的情況下直接在本機建置和測試網站，請遵循以下步驟：

1.  使用 `pip` 命令安裝 `mkdocs`
    ```bash
    pip install mkdocs
    ```
2.  使用以下命令安裝必要的依賴項
    ```bash
    pip install $(mkdocs get-deps)
    ```
3.  從根目錄在本機建置文件網站
    ```bash
    make build-docs-local
    ```
4.  在本機啟動文件網站
    ```bash
    make serve-docs-local
    ```

## 分析

> [!TIP]
> 測試時別忘了停用您的廣告攔截器。

我們收集 [Google Analytics](https://analytics.google.com/analytics/web/#/report-home/a105170809w198079555p192782995)。
Argo CD UI 會為各種 Kubernetes 資源類型顯示圖示，以幫助使用者快速識別它們。Argo CD
包含了一組用於常見資源類型的內建圖示。

您可以按照以下步驟為自訂資源類型貢獻額外的圖示：

1. 確保授權與 Apache 2.0 相容。
2. 將圖示檔案新增到 Argo CD 儲存庫中的 `ui/src/assets/images/resources/<group>/icon.svg` 路徑。
3. 修改 SVG 以使用正確的顏色 `#8fa4b1`。
4. 執行 `make resourceiconsgen` 來更新產生的 typescript 檔案，該檔案列出了所有可用的圖示。
5. 建立一個包含您的變更的 pull request 到 Argo CD 儲存庫。

`<group>` 是自訂資源的 API 群組。例如，如果您要為 API 群組為 `example.com` 的自訂資源新增圖示，您會將圖示放置在 `ui/src/assets/images/resources/example.com/icon.svg`。

如果您希望將相同的圖示應用於具有相同後綴的多個 API 群組中的資源，您可以建立一個以前置底線為首的目錄。底線將被解釋為萬用字元。例如，要將相同的圖示應用於 `example.com` 和 `another.example.com` API 群組中的資源，您會將圖示放置在 `ui/src/assets/images/resources/_.example.com/icon.svg`。
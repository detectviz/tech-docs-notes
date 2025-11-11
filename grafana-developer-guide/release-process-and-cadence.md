# 發行流程與節奏

## 發行週期

### 時程表

以下是即將到來的發行日期：

| 發行版本 | 候選版本 1   | 正式發行 | 發行負責人                                        | 發行批准人                                      | 檢查清單                                                     |
|---------|-----------------------|----------------------|---------------------------------------------------------|-------------------------------------------------------|---------------------------------------------------------------|
| v2.6    | 2022 年 12 月 19 日，星期一 | 2023 年 2 月 6 日，星期一 | [William Tam](https://github.com/wtam2018)              | [William Tam](https://github.com/wtam2018)            | [檢查清單](https://github.com/argoproj/argo-cd/issues/11563) |
| v2.7    | 2023 年 3 月 20 日，星期一 | 2023 年 5 月 1 日，星期一  | [Pavel Kostohrys](https://github.com/pasha-codefresh)   | [Pavel Kostohrys](https://github.com/pasha-codefresh) | [檢查清單](https://github.com/argoproj/argo-cd/issues/12762) |
| v2.8    | 2023 年 6 月 26 日，星期一 | 2023 年 8 月 7 日，星期一 | [Keith Chong](https://github.com/keithchong)            | [Keith Chong](https://github.com/keithchong)          | [檢查清單](https://github.com/argoproj/argo-cd/issues/13742) |
| v2.9    | 2023 年 9 月 18 日，星期一 | 2023 年 11 月 6 日，星期一 | [Leonardo Almeida](https://github.com/leoluz)           | [Leonardo Almeida](https://github.com/leoluz)         | [檢查清單](https://github.com/argoproj/argo-cd/issues/14078) |
| v2.10   | 2023 年 12 月 18 日，星期一 | 2024 年 2 月 5 日，星期一 | [Katie Lamkin](https://github.com/kmlamkin9)            |                                                       | [檢查清單](https://github.com/argoproj/argo-cd/issues/16339) |
| v2.11   | 2024 年 4 月 5 日，星期五 | 2024 年 5 月 6 日，星期一  | [Pavel Kostohrys](https://github.com/pasha-codefresh)   | [Pavel Kostohrys](https://github.com/pasha-codefresh) | [檢查清單](https://github.com/argoproj/argo-cd/issues/17726) |
| v2.12   | 2024 年 6 月 17 日，星期一 | 2024 年 8 月 5 日，星期一 | [Ishita Sequeira](https://github.com/ishitasequeira)    | [Pavel Kostohrys](https://github.com/pasha-codefresh) | [檢查清單](https://github.com/argoproj/argo-cd/issues/19063) |
| v2.13   | 2024 年 9 月 16 日，星期一 | 2024 年 11 月 4 日，星期一 | [Regina Voloshin](https://github.com/reggie-k)          | [Pavel Kostohrys](https://github.com/pasha-codefresh) | [檢查清單](https://github.com/argoproj/argo-cd/issues/19513) |
| v2.14   | 2024 年 12 月 16 日，星期一 | 2025 年 2 月 3 日，星期一 | [Ryan Umstead](https://github.com/rumstead)             | [Pavel Kostohrys](https://github.com/pasha-codefresh) | [檢查清單](https://github.com/argoproj/argo-cd/issues/20869) |
| v3.0    | 2025 年 3 月 17 日，星期一 | 2025 年 5 月 6 日，星期二 | [Regina Voloshin](https://github.com/reggie-k)          |                                                       | [檢查清單](https://github.com/argoproj/argo-cd/issues/21735) |
| v3.1    | 2025 年 6 月 16 日，星期一 | 2025 年 8 月 4 日，星期一 | [Christian Hernandez](https://github.com/christianh814) | [Alexandre Gaudreault](https://github.com/agaudreault) | [檢查清單](#) |
| v3.2    | 2025 年 9 月 15 日，星期一 | 2025 年 11 月 3 日，星期一 | [Nitish Kumar](https://github.com/nitishfy)             |                                                       | [檢查清單](#) |
| v3.3    | 2025 年 12 月 15 日，星期一 | 2026 年 2 月 2 日，星期一 |                                                         |                                                       |

實際發行日期可能與計畫相差幾天。

### 發行流程

#### 次要版本發行 (例如 2.x.0)

Argo CD 的次要版本每年發行四次，每三個月一次。每個正式發行 (GA) 版本之前都會有幾個候選版本 (RC)。第一個 RC 在預定 GA 日期前七週發行。這實際上意味著有七週的功能凍結期。

以下是大概的發行日期：

*   二月的第一個星期一
*   五月的第一個星期一
*   八月的第一個星期一
*   十一月的第一個星期一

日期可能會因應假日而略有調整。這些調整應該是最小的。

#### 修補程式版本發行 (例如 2.5.x)

Argo CD 修補程式版本會視需要發行。只有最新的三個次要版本才有資格獲得修補程式版本。比最新三個次要版本更舊的版本將被視為生命週期結束 (EOL)，將不會收到錯誤修復或安全性更新。

#### 發行負責人

為了協助管理發行中涉及的所有步驟，我們將設立一位發行負責人。發行負責人將負責其發行版本的一份檢查清單。該檢查清單是 Argo CD 儲存庫中的一個議題範本。

發行負責人可以是 Argo CD 社群中的任何人。某些任務（例如 cherry-pick 錯誤修復和發行版本）需要[批准人](https://github.com/argoproj/argoproj/blob/master/community/membership.md#community-membership)的成員資格。發行負責人可以在必要時委派任務，並將負責與批准人協調。

### 功能接受標準

要符合納入次要版本發行的資格，新功能必須在該版本的 RC 日期之前符合以下標準。

如果它是一個涉及重大設計決策的大型功能，則該功能必須在提案中進行描述，並且該提案必須經過審查和合併。

功能 PR 必須包括：

*   測試（通過）
*   文件
*   如有必要，在計畫的次要版本的升級文件中附註
*   PR 必須由批准人審查、批准和合併。

如果這些標準在 RC 日期之前未得到滿足，該功能將沒有資格納入該次要版本的 RC 系列或 GA。它將必須等到下一個次要版本。

### 安全性修補程式策略

Argo CD 程式碼中的 CVE 將會為所有支援的版本進行修補。有關支援版本的更多資訊，請閱讀[Argo CD 的安全性政策](https://github.com/argoproj/argo-cd/security/policy#supported-versions)。

### 依賴項生命週期策略

在引入依賴項之前會對其進行評估，以確保它們：

1) 正在積極維護
2) 由值得信賴的維護者維護

這些評估因依賴項而異。

如果專案已被棄用或不再維護，依賴項也將被安排移除。

如果 CVE 適用且被 Snyk 評估為高或嚴重嚴重性，則依賴項中的 CVE 將會為所有支援的版本進行修補。自動化每週會產生一次[新的 Snyk 掃描](../snyk/index.md)。
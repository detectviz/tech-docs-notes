# Pro WS W680-ACE 系列 BIOS 使用手冊

**T21328 | 第一版 | 2022 年 12 月發行**

## 版權說明

© ASUSTeK Computer Inc. All rights reserved. 華碩電腦股份有限公司保留所有權利 本使用手冊包括但不限於其所包含的所有資訊皆受到著作權法之保護，未經華碩 電腦股份有限公司（以下簡稱「華碩」）許可，不得任意地仿製、拷貝、謄抄、轉 譯或為其他利用。

## 免責聲明

本使用手冊是以「現況」及「以目前明示的條件下」的狀態提供給您。在法律允 許的範圍內，華碩就本使用手冊，不提供任何明示或默示的擔保及保證，包括但不 限於商業適銷性、特定目的之適用性、未侵害任何他人權利及任何得使用本使用手 冊或無法使用本使用手冊的保證，且華碩對因使用本使用手冊而獲取的結果或透過 本使用手冊所獲得任何資訊之準確性或可靠性不提供擔保。

台端應自行承擔使用本使用手冊的所有風險。 台端明確了解並同意，華碩、華 碩之授權人及其各該主管、董事、員工、代理人或關係企業皆無須為您因本使用手 冊、或因使用本使用手冊、或因不可歸責於華碩的原因而無法使用本使用手冊或其 任何部分而可能產生的衍生、附隨、直接、間接、特別、懲罰或任何其他損失（包 括但不限於利益損失、業務中斷、資料遺失或其他金錢損失）負責，不論華碩是否 被告知發生上開損失之可能性。

由於部分國家或地區可能不允許責任的全部免除或對前述損失的責任限制，所以 前述限制或排除條款可能對您不適用。

台端知悉華碩有權隨時修改本使用手冊。本產品規格或驅動程式一經改變，本使 用手冊將會隨之更新。本使用手冊更新的詳細說明請您造訪華碩的客戶服務網 http:// support.asus.com，或是直接與華碩資訊產品技術支援專線 0800-093-456 聯絡。

於本使用手冊中提及之第三人產品名稱或內容，其所有權及智慧財產權皆為各別 產品或內容所有人所有且受現行智慧財產權相關法令及國際條約之保護。 當下列兩種情況發生時，本產品將不再受到華碩之保固及服務：

1.  本產品曾經過非華碩授權之維修、規格更改、零件替換或其他未經過華碩授權 的行為。
2.  本產品序號模糊不清或喪失。

本產品的名稱與版本都會印在主機板/顯示卡上，版本數字的編碼方式是用三個數 字組成，並有一個小數點做間隔，如 1.02G、2.03G 等...數字愈大表示版本愈新，而 愈左邊位數的數字更動表示更動幅度也愈大。更新的詳細說明請您到華碩的全球資 訊網瀏覽或是直接與華碩聯絡。

## 目錄

- [1. 認識 BIOS 程式](#1-認識-bios-程式)
- [2. BIOS 程式設定](#2-bios-程式設定)
- [3. 管理、更新您的 BIOS 程式](#3-管理更新您的-bios-程式)
  - [3.1 華碩 CrashFree BIOS 3 程式](#31-華碩-crashfree-bios-3-程式)
  - [3.2 使用華碩 EzFlash 更新程式](#32-使用華碩-ezflash-更新程式)
- [4. BIOS 選單畫面](#4-bios-選單畫面)
  - [4.1 功能表列說明](#41-功能表列說明)
  - [4.2 選單項目](#42-選單項目)
  - [4.3 子選單](#43-子選單)
  - [4.4 操作功能鍵說明](#44-操作功能鍵說明)
  - [4.5 一般說明](#45-一般說明)
  - [4.6 設定值](#46-設定值)
  - [4.7 設定視窗](#47-設定視窗)
  - [4.8 捲軸](#48-捲軸)
- [5. 主選單（Main Menu）](#5-主選單main-menu)
- [6. Ai Tweaker 選單（Ai Tweaker menu）](#6-ai-tweaker-選單ai-tweaker-menu)
- [7. 進階選單（Advanced menu）](#7-進階選單advanced-menu)
  - [7.1 平台各項設定（Platform Misc Configuration）](#71-平台各項設定platform-misc-configuration)
  - [7.2 CPU 設定（CPU Configuration）](#72-cpu-設定cpu-configuration)
  - [7.3 系統代理設定（System Agent Configuration）](#73-系統代理設定system-agent-configuration)
  - [7.4 PCH 設定（PCH Configuration）](#74-pch-設定pch-configuration)
  - [7.5 PCH 儲存裝置設定（PCH Storage Configuration）](#75-pch-儲存裝置設定pch-storage-configuration)
  - [7.6 PCH-FW 設定（PCH-FW Configuration）](#76-pch-fw-設定pch-fw-configuration)
  - [7.7 AMT 設定（AMT Configuration）](#77-amt-設定amt-configuration)
  - [7.8 Thunderbolt(TM) 設定（Thunderbolt(TM) Configuration）](#78-thunderbolttm-設定thunderbolttm-configuration)
  - [7.9 Redfish Host 介面設定（Redfish Host Interface Settings）](#79-redfish-host-介面設定redfish-host-interface-settings)
  - [7.10 序列埠控制面板重新定向（Serial Port Console Redirection）](#710-序列埠控制面板重新定向serial-port-console-redirection)
  - [7.11 Intel TXT 資訊（Intel TXT Information）](#711-intel-txt-資訊intel-txt-information)
  - [7.12 PCI 子系統設定（PCI Subsystem Settings）](#712-pci-子系統設定pci-subsystem-settings)
  - [7.13 USB 設定（USB Configuration）](#713-usb-設定usb-configuration)
  - [7.14 網路協定堆疊設定（Network Stack Configuration）](#714-網路協定堆疊設定network-stack-configuration)
  - [7.15 NVMe 設定（NVMe Configuration）](#715-nvme-設定nvme-configuration)
  - [7.16 HDD/SSD SMART 資訊（HDD/SSD SMART Information）](#716-hddssd-smart-資訊hddssd-smart-information)
  - [7.17 APM 設定（APM Configuration）](#717-apm-設定apm-configuration)
  - [7.18 內建裝置設定（OnBoard Devices Configuration）](#718-內建裝置設定onboard-devices-configuration)
  - [7.19 Intel(R) 快速儲存技術（Intel(R) Rapid Storage Technology）](#719-intelr-快速儲存技術intelr-rapid-storage-technology)
- [8. 監控選單（Monitor menu）](#8-監控選單monitor-menu)
- [9. 啟動選單（Boot menu）](#9-啟動選單boot-menu)
- [10. 工具選單（Tool menu）](#10-工具選單tool-menu)
  - [10.1 華碩 User Profile](#101-華碩-user-profile)
  - [10.2 華碩 SPD 資訊（ASUS SPD Information）](#102-華碩-spd-資訊asus-spd-information)
  - [10.3 華碩 Armoury Crate](#103-華碩-armoury-crate)
- [11. IPMI 選單（IPMI menu）](#11-ipmi-選單ipmi-menu)
  - [11.1 系統事件記錄（System Event Log）](#111-系統事件記錄system-event-log)
  - [11.2 BMC 網路設定（BMC network configuration）](#112-bmc-網路設定bmc-network-configuration)
  - [11.3 檢視系統事件記錄（View System Event Log）](#113-檢視系統事件記錄view-system-event-log)
- [12. 離開 BIOS 程式（Exit menu）](#12-離開-bios-程式exit-menu)

---

## 1. 認識 BIOS 程式

華碩全新的 UEFI BIOS 是可延伸韌體介面，符合最新的 UEFI 架構，這個友善的使用介面，跳脫傳統使用鍵盤輸入 BIOS 方式，提供更有彈性 與更便利的滑鼠控制操作。您可以輕易地使用新的 UEFI BIOS，如同操 作您的作業系統般順暢。在本使用手冊中的「BIOS」一詞除非特別說 明，所指皆為「UEFI BIOS」。

BIOS（Basic Input and Output System；基本輸出入系統）用來儲存系統開機時所 需要的硬體設定，例如儲存裝置設定、超頻設定、進階電源管理與開機設定等，這 些設定會儲存在主機板的 CMOS 中。在正常情況下，預設的 BIOS 程式設定提供大 多數使用情況下可以獲得最佳的運作效能。建議您不要變更預設的 BIOS 設定，除了 以下幾種狀況：

- 在系統啟動期間，螢幕上出現錯誤訊息，並要求您執行 BIOS 程式設定。
- 安裝新的系統元件，需要進一步的 BIOS 設定或更新。

**注意：**
- 不適當的 BIOS 設定可能會導致系統不穩定或開機失敗。強烈建議您只 有在受過訓練專業人士的協助下，才可以執行 BIOS 程式設定的變更。
- 下載或更新 BIOS 檔案時，請將檔案名稱變更為 XXXXX.CAP 或是開 啟 BIOSRenamer.exe 應用程式以自動將檔案重新命名給本主機板使 用。請參考主機板隨附的使用手冊中的相關資訊以獲得檔案名稱。 CAP 檔案名稱會依型號而異，正確名稱請參考主機板使用手冊。
- 本章節畫面僅供參考，請以實際的 BIOS 選項為準。
- BIOS 設定選項會因版本而異，請確認已更新至最新的 BIOS 版本。

## 2. BIOS 程式設定

使用 BIOS Setup（BIOS 設定）功能可以更新 BIOS 或設定其參數。BIOS 設定畫面 包含導覽鍵與簡要的畫面輔助說明，以指示您使用 BIOS 設定程式。

**當開機時進入 BIOS 設定程式：**

- 當進入開機自我測試（POST）過程時，按下 `<Delete>` 或 `<F2>` 鍵可以進入 BIOS 設定畫面。若您未按下 `<Delete>` 或 `<F2>` 鍵，則 開機自我測試（POST）功能會繼續進行。

**當 POST 結束後才進入 BIOS 設定程式：**

- 按下 `<Ctrl>`+`<Alt>`+`<Delete>` 鍵。
- 或是按下機殼上的 RESET（重置）鍵重新開機。
- 或是將按下機殼上的電源按鈕，將電腦關閉後再重新開機。如果前兩種方式無 效，再選用最後一種方式。

然後再於開機自我測試（POST）過程時按下 `<Delete>` 鍵進入 BIOS 設定畫面。

**注意：**
- 在本章節的 BIOS 程式畫面僅供參考，將可能與您所見到的畫面有所 差異。
- BIOS 程式的出廠預設值可讓系統運作處於最佳效能，但是若系統因 您改變 BIOS 程式而導致不穩定，請讀取出廠預設值來保持系統的 穩定。請選擇 Exit 選單中的 Load Optimized Defaults 項目或按下 `<F5>` 鍵。請參閱 [12. 離開 BIOS 程式](#12-離開-bios-程式exit-menu) 中的詳細說明。
- 若是變更 BIOS 設定後開機失敗，請試著使用清除 CMOS，然後將主 機板的設定值回復為預設值。請參考主機板使用手冊中的相關說明 以了解 Clear CMOS 按鈕並清除 CMOS 即時時鐘（RTC）記憶體資 料。
- BIOS 設定程式不支援藍牙裝置。

## 3. 管理、更新您的 BIOS 程式

以下的工具程式項目為提供您管理與更新主機板 BIOS 設定程式:
1.  **華碩 CrashFree BIOS 3**：當 BIOS 程式毀損時，使用可開機的 USB 隨身碟來更新 BIOS 程式。
2.  **ASUS EzFlash**：使用 USB 隨身碟更新 BIOS。
3.  **BUPDATER**：使用可開機的 USB 隨身碟在 DOS 環境下更新 BIOS 程式。

上述軟體請參考相關章節的詳細使用說明。

**建議：** 建議您先將主機板原始的 BIOS 程式備份到可開機的 USB 隨身碟中，以 備您往後需要再度安裝原始的 BIOS 程式。使用 BUPDATER 程式來拷貝 主機板原始的 BIOS 程式。

### 3.1 華碩 CrashFree BIOS 3 程式

華碩最新自行研發的 CrashFree BIOS 3 工具程式，讓您在當 BIOS 程式和資料被 病毒入侵或毀損時，可以輕鬆的從含有最新或原始的 BIOS 檔案的 USB 隨身碟中回 復 BIOS 程式的資料。

在執行更新 BIOS 程式之前，請準備存有 BIOS 檔案的USB 隨身碟。

**使用 USB 隨身碟回復 BIOS 程式**

請依照以下步驟，使用 USB 隨身碟回復 BIOS 程式。
1.  將儲存有原始或更新的 BIOS 程式檔案的 USB 隨身碟插入 USB 埠，並啟動系 統。
2.  程式會自動開始進行更新，並在完成後重新啟動系統。

**警告：**
- 請勿在更新 BIOS 程式檔案時關閉或重新啟動系統！此舉將會導致系統 損毀！
- 在驅動及公用程式光碟中的 BIOS 程式檔案，也許並非為最新的 BIOS 檔 案，請至華碩網站（http://www.asus.com/tw）下載最新的 BIOS 版本檔 案。

### 3.2 使用華碩 EzFlash 更新程式

華碩 EzFlash 程式讓您能輕鬆的更新 BIOS 程式，可以不必再透過開機片的冗長程 序或是到 DOS 模式下執行。

請至華碩網站 http://www.asus.com/tw/ 下載最新的 BIOS 程式檔案。 以下的 BIOS 畫面僅供參考，請依您所見的實際 BIOS 畫面為準。

請依照下列步驟，使用 EzFlash 來更新 BIOS：
1.  將儲存有最新的 BIOS 檔案的 USB 隨身碟插入 USB 連接埠。
2.  進入 BIOS 設定程式。來到 Tool 選單，選擇 Start EzFlash 後並按下`<Enter>` 鍵 將其開啟。
3.  按左方向鍵來切換至 Drive 欄位。
4.  按上/下方向鍵來選擇儲存最新 BIOS 版本的 USB 隨身碟，然後按下 `<Enter>` 鍵。
5.  按右方向鍵來切換至 Folder Info 欄位。
6.  按上/下方向鍵來選擇 BIOS 檔案，並按下 `<Enter>` 鍵執行 BIOS 更新作業。
7.  當 BIOS 更新作業完成後請重新啟動電腦。

## 4. BIOS 選單畫面

| 選單項目 | 功能表列 | 設定視窗 | 項目說明 |
| :--- | :--- | :--- | :--- |
| **Main** | 本項目提供系統基本設定。 | | |
| **Ai Tweaker** | 本項目提供超頻設定。 | | |
| **Advanced** | 本項目提供系統進階功能設定。 | | |
| **Monitor** | 本項目提供溫度、電源及風扇功能設定。 | | |
| **Boot** | 本項目提供開機磁碟設定。 | | |
| **Tool** | 本項目提供特殊功能的設定。 | | |
| **IPMI** | 本項目提供 IPMI 設定。 | | |
| **Exit** | 本項目提供離開 BIOS 設定程式與出廠預設值還原功能。 | | |

### 4.1 功能表列說明

BIOS 設定程式最上方各選單功能說明如下：
- **Main**: 本項目提供系統基本設定。
- **Ai Tweaker**: 本項目提供超頻設定。
- **Advanced**: 本項目提供系統進階功能設定。
- **Monitor**: 本項目提供溫度、電源及風扇功能設定。
- **Boot**: 本項目提供開機磁碟設定。
- **Tool**: 本項目提供特殊功能的設定。
- **IPMI**: 本項目提供 IPMI 設定。
- **Exit**: 本項目提供離開 BIOS 設定程式與出廠預設值還原功能。

使用左右方向鍵移動選項，可切換至另一個選單畫面。

### 4.2 選單項目

於功能表列選定選項時，被選擇的功能將會反白。假設您選擇 Main 功能，則 會顯示 Main 選單的項目。點選選單中的其他項目（如：Event Logs、Advanced、 Monitor、Boot、Tool 與 Exit 等）也會出現該項目不同的選項。

### 4.3 子選單

在選單畫面中，若功能選項前面有一個小三角形標記（`>`），代表此為子選單，您 可利用方向鍵來選擇，並按下 `<Enter>` 鍵來進入子選單。

### 4.4 操作功能鍵說明

在選單畫面的右下方為操作功能鍵說明，請參照功能鍵說明來選擇及改變各項功 能。

### 4.5 一般說明

在選單畫面的右上方為目前所選擇的作用選項的功能說明，此說明會依選項的不 同而自動變更。

### 4.6 設定值

此區域顯示選單項目的設定值。這些項目中，有的功能選項僅為告知使用者目前 執行狀態，並無法更改，此類項目就會以淡灰色顯示。而可更改的項目，當您使用 方向鍵移動項目時，被選擇的項目以反白顯示，代表這是可更改的項目。要改變設 定值請選擇此項目，並按下 `<Enter>` 鍵以顯示設定值列表。

### 4.7 設定視窗

在選單中請選擇功能項目，然後按下 `<Enter>` 鍵，程式將會顯示包含此功能所提供 的選項小視窗，您可以利用此視窗來設定您所想要的設定。

### 4.8 捲軸

在選單畫面的右方若出現捲軸，即代表此頁選項超過可顯示的畫面，您可利用上/ 下方向鍵或是 PageUp/PageDown 鍵來切換畫面。

## 5. 主選單（Main Menu）

當進入 BIOS 設定程式的進階模式（Advanced Mode）時，首先出現的第一個畫面 即為主選單。主選單顯示系統資訊概要，用來設定系統日期、時間、語言與安全設 定。

### 安全性選單（Security）

本選單可以讓您改變系統安全設定。

**注意：**
- 若您忘記設定的 BIOS 密碼，可以採用清除 CMOS 即時鐘（RTC） 記憶體。請參考主機板使用手冊中 跳線選擇區 的說明。
- Administrator 或 User Password 項目預設值為 `[Not Installed]`。當您 設定密碼之後將顯示為 `[Installed]`。

#### Administrator Password（設定系統管理員密碼）

當您設定系統管理員密碼後，建議您先登入您的帳戶，以免 BIOS 設定程式中的某 些資訊無法檢視或變更設定。

請依照以下步驟設定系統管理員密碼（Administrator Password）：
1.  請選擇 Administrator Password 項目並按下 `<Enter>`。
2.  由 Create New Password 視窗輸入欲設定的密碼，輸入完成時，請按下 `<Enter>`。
3.  請再一次輸入密碼並選擇 OK。

請依照以下步驟變更系統管理員密碼（Administrator Password）：
1.  請選擇 Administrator Password 項目並按下 `<Enter>`。
2.  由 Enter Current Password 視窗輸入密碼並按下 `<Enter>`。
3.  由 Create New Password 視窗輸入新密碼，輸入完成按下 `<Enter>`。
4.  請再一次輸入密碼並選擇 OK。

欲刪除系統管理員密碼時，請依照變更系統管理員密碼之步驟，但請在輸入/確認 密碼視窗出現時選擇 OK。當您刪除系統管理員密碼後，Administrator Password 項 目將顯示為 `[Not Installed]`。

#### User Password（設定使用者密碼）

當您設定使用者密碼後，你必需登入您的帳戶才能使用 BIOS 設定程式。User Password 項目預設值為 `[Not Installed]`。當您設定密碼之後將顯示為 `[Installed]`。

請依照以下步驟設定使用者密碼（User Password）：
1.  請選擇 User Password 項目並按下 `<Enter>`。
2.  由 Create New Password 視窗輸入欲設定的密碼，輸入完成時，請按下 `<Enter>`。
3.  請再一次輸入密碼並選擇 OK。

請依照以下步驟變更使用者密碼（User Password）：
1.  請選擇 User Password 項目並按下 `<Enter>`。
2.  由 Enter Current Password 視窗輸入密碼並按下 `<Enter>`。
3.  由 Create New Password 視窗輸入新密碼，輸入完成按下 `<Enter>`。
4.  請再一次輸入密碼並選擇 OK。

欲刪除使用者密碼時，請依照變更使用者密碼之步驟，但請在輸入/確認密碼視 窗出現時選擇 OK。當您刪除使用者密碼後，User Password 項目將顯示為 `[Not Installed]`。

## 6. Ai Tweaker 選單（Ai Tweaker menu）

本選單可讓您設定超頻功能的相關選項。

**注意！** 在您設定本進階選單的設定時，不正確的設定值將導致系統功能 異常。

**注意！** 以下項目中的設定值，可能會隨安裝在主機板上的 CPU 與記憶體模組而 異。

### Ai Tweaker 參數補充說明

此章節的設定主要用於系統超頻與效能最佳化。不當的設定可能導致系統不穩定、無法開機或甚至損壞硬體。建議僅由熟悉超頻的進階使用者進行手動調整。初學者建議使用 `Ai Overclock Tuner` 中的自動設定檔（如 EXPO/DOCP/AEMP）以獲得穩定且安全的效能提升。

- **核心超頻概念**：CPU 的最終頻率由 `基頻 (BCLK Frequency)` 乘以 `核心倍頻 (Core Ratio)` 決定。例如，100MHz BCLK * 50x Ratio = 5000MHz (5.0GHz)。最常見的超頻方式是調整核心倍頻。
- **記憶體超頻概念**：記憶體效能由 `頻率 (DRAM Frequency)` 與 `時序 (DRAM Timings)` 共同決定。高頻率和低時序能帶來更好的資料傳輸效能。`Ai Overclock Tuner` 中的設定檔 (EXPO/DOCP) 是最簡單的記憶體超頻方式。
- **電壓與散熱**：提高頻率通常需要增加電壓以維持穩定，但這會大幅增加功耗與廢熱。請確保您的散熱系統（CPU 散熱器、機殼風扇）足以應付超頻後產生的額外熱量，並密切監控溫度。

- **Ai Overclock Tuner**
  - **功能**：這是系統效能調校的起點，決定了 CPU 和記憶體頻率的控制方式。
  - **`[Auto]`**：最安全的選項，所有設定均由 BIOS 自動根據硬體規格決定，不進行超頻。
  - **`[EXPO I / EXPO II]`** (適用於 AMD 平台)：讀取並套用記憶體模組中儲存的 `AMD EXtended Profiles for Overclocking` 設定檔。這是 AMD 官方認證的一鍵記憶體超頻技術。`EXPO I` 通常是廠商預設的最佳化設定，而 `EXPO II` 則是相容性更高的設定。
  - **`[DOCP I / DOCP II]`** (Direct Over Clock Profile, 華碩的 XMP 實作)：讀取並套用記憶體模組中儲存的 `Intel Xtreme Memory Profile (XMP)` 設定檔。這是 Intel 平台最普及的一鍵記憶體超頻技術。`DOCP I` 和 `DOCP II` 提供不同層級的最佳化。
  - **`[AEMP]`** (ASUS Enhanced Memory Profile)：當您的記憶體模組沒有內建 EXPO 或 XMP 設定檔時，華碩會根據記憶體晶片資訊，提供一組經過測試的最佳化超頻設定。
  - **建議**：若您購買了標示有 EXPO 或 XMP 功能的超頻記憶體，請務必在此處啟用對應的設定檔，以發揮記憶體的完整效能。
  - *此選單項目會依安裝的記憶體模組而異。*

- **AEMP** (只有在 Ai Overclock Tuner 設為 `[AEMP]` 時才會出現)
  - 本項目可以用來選擇 ASUS Enhanced Memory Profile（AEMP）。每個設定檔都有 專屬動態隨機存取記憶體（DRAM）頻率、時間與電壓。

- **EXPO** (只有在 Ai Overclock Tuner 設為 `[EXPO I]` 或 `[EXPO II]` 時才會 出現)
  - 本項目用來選擇 EXPO 設定檔。每個設定檔都有專屬動態隨機存取記憶體 （DRAM）頻率、時間與電壓。

- **D.O.C.P.** (只有在 Ai Overclock Tuner 設為 `[DOCP I]` 或 `[DOCP II]` 時才會 出現)
  - 本項目可以選擇 D.O.C.P.設定檔。每個設定檔都有專屬動態隨機存取記憶體 （DRAM）頻率、時間與電壓。

- **ASUS Performance Enhancement 3.0**
  - **功能**：此為華碩獨家的 CPU 效能增強技術，主要透過放寬 Intel 原廠的功耗牆限制 (PL1/PL2)，讓 CPU 在高負載下能夠維持更長時間的最高 Turbo Boost 頻率，從而提升多核心效能。
  - **`[Disabled]`**：完全遵循 Intel CPU 的官方功耗規範。
  - **`[Enabled]`**：解鎖功耗限制，讓 CPU 發揮最大潛能。此模式下 CPU 功耗與溫度會顯著上升，對散熱系統要求較高。
  - **`[Enabled(limit CPU temp. at 90°C)]`**：在解鎖功耗限制的同時，增加一道溫度保護牆。當 CPU 溫度達到 90°C 時，系統會自動介入以防止過熱，在效能與安全之間取得平衡。
  - **建議**：若您擁有高階的 CPU 散熱器（如大型塔式風冷或 240mm 以上的一體式水冷），可以考慮啟用此功能以獲得免費的效能提升。若對散熱沒有信心，選擇 90°C 溫度限制模式是更穩妥的選擇。
  - *實際設定值會因型號而異。*

- **BCLK Frequency DRAM Frequency Ratio**
  - **功能**：設定 `基頻 (BCLK)` 與 `記憶體頻率 (DRAM Frequency)` 之間的傳輸速率比例。BCLK 是主機板上多數元件的參考時脈，預設為 100MHz。
  - **`[Auto]`**：由 BIOS 自動選擇最佳比例。
  - **`[100:133]`**：BCLK 與 DRAM 頻率為 100:133 的比例。這允許記憶體頻率達到更高的非對稱速率，通常能提供更好的記憶體超頻潛力。
  - **`[100:100]`**：BCLK 與 DRAM 頻率為 1:1 的同步比例。
  - **建議**：一般情況下維持 `[Auto]` 或 `[100:133]` 即可。手動調整 BCLK 是非常進階的超頻技巧，會同時影響 CPU、記憶體、PCIe 等多個元件的頻率，容易導致不穩定，不建議初學者嘗試。

- **Memory Controller : DRAM Frequency Ratio**
  - **功能**：設定 `記憶體控制器 (IMC)` 與 `DRAM 實際頻率` 之間的運作模式，通常稱為 `Gear Mode`。
  - **`[Auto]`**：由 BIOS 自動判斷。
  - **`[1:1]` (Gear 1)**：記憶體控制器與 DRAM 同步運作。此模式延遲最低，能提供最佳的遊戲與應用程式效能，但對記憶體控制器和 RAM 體質要求極高，通常只能在較低的記憶體頻率下達成 (例如 DDR5-6000 或更低)。
  - **`[1:2]` (Gear 2)**：記憶體控制器的運作頻率為 DRAM 的一半。此模式大幅放寬了對硬體的要求，讓記憶體能衝刺到更高的頻率，從而獲得更大的頻寬，但在延遲方面會有所犧牲。
  - **`[1:4]` (Gear 4)**：更進一步降低記憶體控制器的運作頻率，僅為 DRAM 的四分之一，用於挑戰極限記憶體頻率。
  - **建議**：對於追求最低延遲與最佳綜合效能的使用者，應優先嘗試在 `[1:1]` 模式下找到記憶體能穩定運作的最高頻率。若您使用超高頻率的記憶體 (如 DDR5-7200+)，則 `[1:2]` 模式是更現實且穩定的選擇。

- **DRAM Frequency**
  - **功能**：手動設定記憶體的目標運作頻率。此處顯示的頻率是 `資料傳輸率 (Data Rate)`，例如 DDR5-6000MHz。
  - **`[Auto]`**：若未啟用 EXPO/XMP，則會以 JEDEC 的標準預設頻率運作 (例如 DDR5-4800)。若已啟用 EXPO/XMP，則會自動套用設定檔中的頻率。
  - **手動設定**：您可以從列表中選擇一個目標頻率來手動超頻記憶體。手動超頻通常需要搭配調整電壓與時序才能穩定。
  - **建議**：除非您是經驗豐富的超頻玩家，否則建議將此項保留為 `[Auto]`，並透過 `Ai Overclock Tuner` 中的 EXPO/XMP 設定檔來進行超頻。
  - *以下項目中的設定值，可能會隨安裝在主機板上的 CPU 與記憶體模組而 異。*
  - *呈灰色的設定值不建議使用，請使用呈白色的設定值。*

- **Performance Core Ratio**
  - **功能**：設定 CPU `效能核心 (P-Core)` 的倍頻。CPU 的最終頻率由 BCLK (通常為 100MHz) 乘以倍頻決定。
  - **`[Auto]`**：由 CPU 根據負載狀況自動管理 Turbo Boost 行為。
  - **`[Sync All Cores]`**：鎖定所有 P-Core 使用同一個最高倍頻。這是最常見的手動超頻方式，例如設定為 `55`，所有 P-Core 在高負載時都會嘗試達到 5.5GHz。
  - **`[By Core Usage]`**：允許您根據當前有多少個核心處於活動狀態，來設定不同的倍頻上限。例如，您可以設定在只有 1-2 個核心負載時跑更高的頻率 (如 5.7GHz)，而在 8 個核心滿載時跑較低的頻率 (如 5.3GHz)，以在效能和功耗/溫度之間取得平衡。
  - **`[AI Optimized]`**：華碩的 AI 超頻功能，會根據您的 CPU 體質和散熱器效能評分，自動生成一組最佳化的 `By Core Usage` 超頻曲線。這是最智慧、最適合初學者的自動超頻方式。
  - **建議**：對於追求穩定且想發揮 CPU 潛能的使用者，`[AI Optimized]` 是最佳選擇。手動超頻玩家則常使用 `[Sync All Cores]` 來挑戰極限。
  - *`[AI Optimized]` 項目只有在安裝沒有鎖頻的處理器時才會顯示。*

- **ALL-Core Ratio Limit** (只有在 Performance Core Ratio 設為 `[Sync All Cores]` 時才 會出現)
  - 選擇 `[Auto]` 以套用 CPU 預設的 Turbo 倍頻設定或手動指定 Core Ratio Limit 數 值。請使用 `<+>` 與 `<->` 鍵調整數值。設定值有：`[Auto]` `[8]` - `[45]`

- **1-Core Ratio Limit / 2-Core Ratio Limit / ... / 8-Core Ratio Limit** (只有在 Performance Core Ratio 設為 `[By Core Usage]` 時才會出 現)
  - N-core 比率限制需高於或等於（N+1）-core 比率限制。（N 代表 CPU 核心數量） 當核心數量低於 N 時，核心比率限制無法設定為 `[Auto]` 。最大核心比率限制需低於 或等於第二大核心比率限制。請使用 `<+>` 與 `<->` 鍵調整數值。設定值有：`[Auto]` `[21]` - `[49]`

- **Optimized AVX Frequency** (只有在 Performance Core Ratio 設為 `[AI Optimized]` 時才會出 現)
  - 標準用例選擇 `[Normal Use]`，或是 extreme 負載如 Prime 95 AVX 時選擇 `[Heavy AVX]`。設定值有：`[Normal Use]` `[Heavy AVX]`

- **Efficient Core Ratio**
  - **功能**：設定 CPU `效率核心 (E-Core)` 的倍頻。E-Core 主要負責處理背景任務與多執行緒工作，超頻 E-Core 能提升系統整體的處理能力。
  - **`[Auto]`**：由 CPU 自動管理 E-Core 的頻率。
  - **`[Sync All Cores]`**：鎖定所有 E-Core 使用同一個最高倍頻。
  - **`[By Core Usage]`**：與 P-Core 設定類似，但較少使用於 E-Core。
  - **`[AI Optimized]`**：AI 超頻也會一併最佳化 E-Core 的頻率曲線。
  - **建議**：手動超頻時，通常會為 E-Core 設定一個比 P-Core 低的全核倍頻 (例如 4.2GHz - 4.5GHz)，以確保穩定性。若使用 `AI Optimized`，則讓 AI 自動處理即可。
  - *`[AI Optimized]` 項目只有在安裝沒有鎖頻的處理器時才會顯示。*

- **ALL-Core Ratio Limit** (只有在 Efficient Core Ratio 設為 `[Sync All Cores]` 時才會 出現)
  - 本項目可以設定效率核心的 Ratio Limit。請使用 `<+>` 與 `<->` 鍵調整數值。設定值 有：`[Auto]` `[8]` - `[34]`

- **Efficient 1-Core Ratio Limit / ... / Efficient 4-Core Ratio Limit** (只有在 Performance Core Ratio 設為 `[By Core Usage]` 時才會出 現)
  - 設定值有：`[Auto]` `[16]` - `[36]`

- **AVX Related Controls**
  - **功能**：控制 CPU 的 `Advanced Vector Extensions (AVX)` 指令集。AVX 主要用於科學計算、影像處理等高強度運算，會讓 CPU 產生極高的功耗與溫度。
  - **AVX2**: 啟用或關閉 AVX2 指令集的支援。
  - **建議**：一般使用者保持 `[Auto]` 或 `[Enabled]` 即可。在進行極限超頻穩定性測試時，部分玩家會暫時關閉 AVX 以降低 CPU 負載，但这在日常使用中並不建議。

- **DRAM Timing Control**
  - **功能**：手動微調記憶體的詳細時序參數 (俗稱「小參」)。記憶體時序決定了資料在記憶體顆粒中讀寫的延遲，更低的時序 (更小的數值) 代表更快的反應速度。
  - **重要參數**：
    - `DRAM CAS# Latency (tCL)`: 核心時序之一，代表從發出讀取命令到資料開始傳輸的延遲。
    - `DRAM RAS# to CAS# Delay (tRCD)`: 行位址到列位址的延遲。
    - `DRAM RAS# PRE Time (tRP)`: 預充電時間。
    - `DRAM RAS# ACT Time (tRAS)`: 行位址啟動時間。
    - `DRAM Command Rate (CMD)`: 命令速率，`[1N]` (或 1T) 比 `[2N]` (2T) 更快，但對穩定性要求更高。
  - **建議**：手動調整記憶體時序是一項極為複雜且耗時的工作，需要反覆測試以確保穩定性。強烈建議初學者直接使用 EXPO/XMP 設定檔，而非手動修改此處的參數。進階玩家可以從 `Primary Timings` 開始，嘗試在穩定範圍內逐步降低數值以最佳化記憶體效能。
  - *自行更改數值將會導致系統的不穩定與硬體損毀，當系統出現不穩定的 狀況時，建議您使用預設值。*

- **Primary Timings**
  - **DRAM CAS# Latency**: `[Auto]` `[2]` - `[126]`
  - **DRAM RAS# to CAS# Delay**: `[Auto]` `[0]` - `[255]`
  - **DRAM RAS# PRE Time**: `[Auto]` `[0]` - `[255]`
  - **DRAM RAS# ACT Time**: `[Auto]` `[1]` - `[511]`
  - **DRAM Command Rate**: `[Auto]` `[1N]` `[2N]` `[3N]` `[N:1]`

- **N to 1 ratio** (只有在 DRAM Command Rate 設為 `[N:1]` 時才會出現)
  - 每個有效命令周期之間的數值。設定值有：`[1]` - `[7]`

- **Secondary Timings**
  - **DRAM RAS# to RAS# Delay L**: `[Auto]` `[1]` - `[63]`
  - **DRAM RAS# to RAS# Delay S**: `[Auto]` `[1]` - `[127]`
  - **DRAM REF Cycle Time**: `[Auto]` `[1]` - `[65535]`
  - **DRAM REF Cycle Time 2**: `[Auto]` `[1]` - `[65535]`
  - **DRAM REF Cycle Time Same Bank**: `[Auto]` `[0]` - `[2047]`
  - **DRAM Refresh Interval**: `[Auto]` `[1]` - `[262143]`
  - **DRAM WRITE Recovery Time**: `[Auto]` `[1]` - `[234]`
  - **DRAM READ to PRE Time**: `[Auto]` `[1]` - `[255]`
  - **DRAM FOUR ACT WIN Time**: `[Auto]` `[1]` - `[511]`
  - **DRAM WRITE to READ Delay**: `[Auto]` `[1]` - `[31]`
  - **DRAM WRITE to READ Delay L**: `[Auto]` `[1]` - `[31]`
  - **DRAM WRITE to READ Delay S**: `[Auto]` `[1]` - `[31]`
  - **DRAM CKE Minimum Pulse Width**: `[Auto]` `[0]` - `[127]`
  - **DRAM Write Latency**: `[Auto]` `[1]` - `[255]`

- **Skew Control**
  - **DDRCRCOMPCTL0/1/2**
    - **Ctl0 dqvrefup**: `[Auto]` `[0]` - `[255]`
    - **Ctl0 dqvrefdn**: `[Auto]` `[0]` - `[255]`
    - **Ctl0 dqodtvrefup**: `[Auto]` `[0]` - `[255]`
    - **Ctl0 dqodtvrefdn**: `[Auto]` `[0]` - `[255]`
    - **Ctl1 cmdvrefup**: `[Auto]` `[0]` - `[255]`
    - **Ctl1 ctlvrefup**: `[Auto]` `[0]` - `[255]`
    - **Ctl1 clkvrefup**: `[Auto]` `[0]` - `[255]`
    - **Ctl1 ckecsvrefup**: `[Auto]` `[0]` - `[255]`
    - **Ctl2 cmdvrefdn**: `[Auto]` `[0]` - `[255]`
    - **Ctl2 ctlvrefdn**: `[Auto]` `[0]` - `[255]`
    - **Ctl2 clkvrefdn**: `[Auto]` `[0]` - `[255]`

- **Tc Odt Control**
  - **ODT_READ_DURATION**: `[Auto]` `[0]` - `[15]`
  - **ODT_READ_DELAY**: `[Auto]` `[0]` - `[15]`
  - **ODT_WRITE_DURATION**: `[Auto]` `[0]` - `[15]`
  - **ODT_WRITE_DELAY**: `[Auto]` `[0]` - `[15]`

- **MC0 Dimm0 / MC0 Dimm1 / MC1 Dimm0 / MC1 Dimm1**
  - **DQ RTT WR**: `[Auto]` `[0 DRAM Clock]` `[34 DRAM Clock]` `[40 DRAM Clock]` `[48 DRAM Clock]` `[60 DRAM Clock]` `[80 DRAM Clock]` `[120 DRAM Clock]` `[240 DRAM Clock]`
  - **DQ RTT NOM RD**: `[Auto]` `[0 DRAM Clock]` `[34 DRAM Clock]` `[40 DRAM Clock]` `[48 DRAM Clock]` `[60 DRAM Clock]` `[80 DRAM Clock]` `[120 DRAM Clock]` `[240 DRAM Clock]`
  - **DQ RTT NOM WR**: `[Auto]` `[0 DRAM Clock]` `[34 DRAM Clock]` `[40 DRAM Clock]` `[48 DRAM Clock]` `[60 DRAM Clock]` `[80 DRAM Clock]` `[120 DRAM Clock]` `[240 DRAM Clock]`
  - **DQ RTT PARK**: `[Auto]` `[0 DRAM Clock]` `[34 DRAM Clock]` `[40 DRAM Clock]` `[48 DRAM Clock]` `[60 DRAM Clock]` `[80 DRAM Clock]` `[120 DRAM Clock]` `[240 DRAM Clock]`
  - **DQ RTT PARK DQS**: `[Auto]` `[0 DRAM Clock]` `[34 DRAM Clock]` `[40 DRAM Clock]` `[48 DRAM Clock]` `[60 DRAM Clock]` `[80 DRAM Clock]` `[120 DRAM Clock]` `[240 DRAM Clock]`
  - **GroupA CA ODT**: `[Auto]` `[0 DRAM Clock]` `[40 DRAM Clock]` `[60 DRAM Clock]` `[80 DRAM Clock]` `[120 DRAM Clock]` `[240 DRAM Clock]` `[480 DRAM Clock]`
  - **GroupA CS ODT**: `[Auto]` `[0 DRAM Clock]` `[40 DRAM Clock]` `[60 DRAM Clock]` `[80 DRAM Clock]` `[120 DRAM Clock]` `[240 DRAM Clock]` `[480 DRAM Clock]`
  - **GroupA CK ODT**: `[Auto]` `[0 DRAM Clock]` `[40 DRAM Clock]` `[60 DRAM Clock]` `[80 DRAM Clock]` `[120 DRAM Clock]` `[240 DRAM Clock]` `[480 DRAM Clock]`
  - **GroupB CA ODT**: `[Auto]` `[0 DRAM Clock]` `[40 DRAM Clock]` `[60 DRAM Clock]` `[80 DRAM Clock]` `[120 DRAM Clock]` `[240 DRAM Clock]` `[480 DRAM Clock]`
  - **GroupB CS ODT**: `[Auto]` `[0 DRAM Clock]` `[40 DRAM Clock]` `[60 DRAM Clock]` `[80 DRAM Clock]` `[120 DRAM Clock]` `[240 DRAM Clock]` `[480 DRAM Clock]`
  - **GroupB CK ODT**: `[Auto]` `[0 DRAM Clock]` `[40 DRAM Clock]` `[60 DRAM Clock]` `[80 DRAM Clock]` `[120 DRAM Clock]` `[240 DRAM Clock]` `[480 DRAM Clock]`
  - **Pull-up Output Driver Impedance**: `[Auto]` `[34 DRAM Clock]` `[40 DRAM Clock]` `[48 DRAM Clock]`
  - **Pull-Down Output Driver Impedance**: `[Auto]` `[34 DRAM Clock]` `[40 DRAM Clock]` `[48 DRAM Clock]`

- **RTL IOL Control**
  - **Round Trip Latency Init Value MC0-1 CHA-B**: `[Auto]` `[0]` - `[255]`
  - **Round Trip Latency Max Value MC0-1 CHA-B**: `[Auto]` `[0]` - `[255]`
  - **Round Trip Latency Offset Value Mode Sign MC0-1 CHA-B**: `[-]` `[+]`
  - **Round Trip Latency Offset Value MC0-1 CHA-B**: `[Auto]` `[0]` - `[255]`
  - **Round Trip Latency MC0-1 CHA-B R0-7**: `[Auto]` `[0]` - `[255]`

- **Memory Training Algorithms**
  - *This section allows you to enable or disable different memory training algorithms.*
  - **Early Command Training**: `[Auto]` `[Disabled]` `[Enabled]`
  - **SenseAmp Offset Training**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Early ReadMPR Timing Centering 2D**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Read MPR Training**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Receive Enable Training**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Jedec Write Leveling**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Early Write Timing Centering 2D**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Early Read Timing Centering 2D**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Write Timing Centering 1D**: `[Disabled]` `[Enabled]`
  - **Write Voltage Centering 1D**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Read Timing Centering 1D**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Read Timing Centering with JR**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Dimm ODT Training***: `[Auto]` `[Disabled]` `[Enabled]`
  - **Max RTT_WR**: `[ODT OFF]` `[120 Ohms]`
  - **DIMM RON Training***: `[Auto]` `[Disabled]` `[Enabled]`
  - **Write Drive Strength/Equalization 2D***: `[Auto]` `[Disabled]` `[Enabled]`
  - **Write Slew Rate Training***: `[Auto]` `[Disabled]` `[Enabled]`
  - **Read ODT Training***: `[Auto]` `[Disabled]` `[Enabled]`
  - **Comp Optimization Training**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Read Equalization Training***: `[Auto]` `[Disabled]` `[Enabled]`
  - **Read Amplifier Training***: `[Auto]` `[Disabled]` `[Enabled]`
  - **Write Timing Centering 2D**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Read Timing Centering 2D**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Command Voltage Centering**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Early Command Voltage Centering**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Write Voltage Centering 2D**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Read Voltage Centering 2D**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Late Command Training**: `[Disabled]` `[Enabled]` `[Auto]`
  - **Round Trip Latency**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Turn Around Timing Training**: `[Auto]` `[Disabled]` `[Enabled]`
  - **CMD CTL CLK Slew Rate**: `[Auto]` `[Disabled]` `[Enabled]`
  - **CMD/CTL DS & E 2D**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Read Voltage Centering 1D**: `[Auto]` `[Disabled]` `[Enabled]`
  - **TxDqTCO Comp Training***: `[Auto]` `[Disabled]` `[Enabled]`
  - **ClkTCO Comp Training***: `[Auto]` `[Disabled]` `[Enabled]`
  - **TxDqsTCO Comp Training***: `[Auto]` `[Disabled]` `[Enabled]`
  - **VccDLL Bypass Training***: `[Auto]` `[Disabled]` `[Enabled]`
  - **CMD/CTL Drive Strength Up/Dn 2D**: `[Auto]` `[Disabled]` `[Enabled]`
  - **DIMM CA ODT Training**: `[Auto]` `[Disabled]` `[Enabled]`
  - **PanicVttDnLp Training***: `[Auto]` `[Disabled]` `[Enabled]`
  - **Read Vref Decap Traning***: `[Auto]` `[Disabled]` `[Enabled]`
  - **Vddq Training**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Duty Cycle Correction Training**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Rank Margin Tool Per Bit**: `[Auto]` `[Disabled]` `[Enabled]`
  - **DIMM DFE Training**: `[Auto]` `[Disabled]` `[Enabled]`
  - **EARLY DIMM DFE Training**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Tx Dqs Dcc Training**: `[Auto]` `[Disabled]` `[Enabled]`
  - **DRAM DCA Training**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Write Driver Strength Training**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Rank Margin Tool**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Memory Test**: `[Auto]` `[Disabled]` `[Enabled]`
  - **DIMM SPD Alias Test**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Receive Enable Centering 1D**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Retrain Margin Check**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Write Drive Strength Up/Dn independently**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Margin Check Limit**: `[Disabled]` `[L1]` `[L2]` `[Both]`
  - **Margin Limit Check L2** (Only if Margin Check Limit is `[L2]` or `[Both]`): `[1]` - `[300]`

- **Third Timings**
  - **tRDRD_sg_Training**: `[Auto]` `[0]` - `[127]`
  - **tRDRD_sg_Runtime**: `[Auto]` `[0]` - `[127]`
  - **tRDRD_dg_Training**: `[Auto]` `[0]` - `[127]`
  - **tRDRD_dg_Runtime**: `[Auto]` `[0]` - `[127]`
  - **tRDWR_sg**: `[Auto]` `[0]` - `[255]`
  - **tRDWR_dg**: `[Auto]` `[0]` - `[255]`
  - **tWRWR_sg**: `[Auto]` `[0]` - `[127]`
  - **tWRWR_dg**: `[Auto]` `[0]` - `[127]`
  - **tWRRD_sg**: `[Auto]` `[0]` - `[511]`
  - **tWRRD_dg**: `[Auto]` `[0]` - `[511]`
  - **tRDRD_dr**: `[Auto]` `[0]` - `[255]`
  - **tRDRD_dd**: `[Auto]` `[0]` - `[255]`
  - **tRDWR_dr**: `[Auto]` `[0]` - `[255]`
  - **tRDWR_dd**: `[Auto]` `[0]` - `[255]`
  - **tWRWR_dr**: `[Auto]` `[0]` - `[127]`
  - **tWRWR_dd**: `[Auto]` `[0]` - `[255]`
  - **tWRRD_dr**: `[Auto]` `[0]` - `[127]`
  - **tWRRD_dd**: `[Auto]` `[0]` - `[127]`
  - **tRPRE**: `[Auto]` `[0]` - `[4]`
  - **tWPRE**: `[Auto]` `[0]` - `[4]`
  - **tWRPRE**: `[Auto]` `[0]` - `[1023]`
  - **tPRPDEN**: `[Auto]` `[0]` - `[31]`
  - **tRDPDEN**: `[Auto]` `[0]` - `[255]`
  - **tWRPDEN**: `[Auto]` `[0]` - `[1023]`
  - **tCPDED**: `[Auto]` `[0]` - `[31]`
  - **tREFIX9**: `[Auto]` `[0]` - `[255]`
  - **Ref Interval**: `[Auto]` `[0]` - `[8191]`
  - **tXPDLL**: `[Auto]` `[0]` - `[127]`
  - **tXP**: `[Auto]` `[0]` - `[127]`
  - **tPPD**: `[Auto]` `[0]` - `[15]`
  - **tCCD_L_tDLLK**: `[Auto]` `[0]` - `[15]`

- **Misc.**
  - **MRC Fast Boot**: `[Disabled]` `[Enabled]`
  - **MCH Full Check**: `[Auto]` `[Enabled]` `[Disabled]`
  - **Mem Over Clock Fail Count**: `[Auto]` `[1]` - `[254]`
  - **Training Profile**: `[Auto]` `[Standard Profile]` `[ASUS User Profile]`
  - **RxDfe**: `[Auto]` `[Enabled]` `[Disabled]`
  - **Mrc Training Loop Count**: `[Auto]` `[0]` - `[32]`
  - **DRAM CLK Period**: `[Auto]` `[0]` - `[161]`
  - **Dll_bwsel**: `[Auto]` `[0]` - `[63]`
  - **Controller 0, Channel 0 Control**: `[Enabled]` `[Disabled]`
  - **Controller 0, Channel 1 Control**: `[Enabled]` `[Disabled]`
  - **Controller 1, Channel 0 Control**: `[Enabled]` `[Disabled]`
  - **Controller 1, Channel 1 Control**: `[Enabled]` `[Disabled]`
  - **MC_Vref0-2**: `[Auto]` `[0]` - `[65533]`
  - **Fine Granularity Refresh mode**: `[Auto]` `[Disabled]` `[Enabled]`

- **DRAM SPD Configuration**
  - **SDRAM Density Per Die**: `[Auto]` `[4 Gb]` `[8 Gb]` `[12 Gb]` `[16 Gb]` `[24 Gb]` `[32 Gb]` `[48 Gb]` `[64 Gb]`
  - **SDRAM Banks Per Bank Group**: `[Auto]` `[1 bank per bank group]` `[2 bank per bank group]` `[4 bank per bank group]`
  - **SDRAM Bank Groups**: `[Auto]` `[1 bank group]` `[2 bank groups]` `[4 bank groups]` `[8 bank groups]`

- **Configure Memory Dynamic Frequency Switching**
  - **Dynamic Memory Boost** (Only if Realtime Memory Frequency is `[Disabled]`): `[Disabled]` `[Enabled]`
  - **Realtime Memory Frequency** (Only if Dynamic Memory Boost is `[Disabled]`): `[Disabled]` `[Enabled]`
  - **SA GV**: `[Disabled]` `[Enabled]` `[Fixed to 1st Point]` `[Fixed to 2nd Point]` `[Fixed to 3rd Point]` `[Fixed to 4th Point]`
  - **First Point Frequency**: `[0]` - `[65535]`
  - **First Point Gear**: `[0]` - `[4]`
  - **Second Point Frequency**: `[0]` - `[65535]`
  - **Second Point Gear**: `[0]` - `[4]`
  - **Third Point Frequency**: `[0]` - `[65535]`
  - **Third Point Gear**: `[0]` - `[4]`
  - **Fourth Point Gear**: Please set in the main menu.

- **Digi+ VRM**
  - **功能**：此區塊的設定用於調整主機板的 `數位電壓調節模組 (Digital Voltage Regulation Module, VRM)`。VRM 負責將電源供應器的 12V 電壓轉換為 CPU 所需的精確電壓。正確設定 VRM 是超頻成功的關鍵。
  - **VRM Intialization Check**: `[Disabled]` `[Enabled]`
  - **CPU Input Voltage Load-line Calibration**: `[Auto]` `[Level 1]` `[Level 2]` `[Level 3]`
  - **CPU Load-line Calibration (LLC)**:
    - **功能**：補償 CPU 在高負載時發生的 `電壓下降 (Vdroop)` 現象。當 CPU 負載劇增時，電壓會自然下降，可能導致超頻不穩定。LLC 會主動提高電壓來抵銷這種下降。
    - **等級**：Level 1 的補償幅度最小，Level 7 (或更高) 的補償幅度最大，甚至可能產生 `電壓上升 (overshoot)`。
    - **建議**：超頻時，建議從中間的等級 (如 Level 4 或 Level 5) 開始嘗試。過高的 LLC 等級雖然能讓電壓在高負載下更穩定，但在負載變化瞬間可能產生過高的尖峰電壓，長期下來可能損害 CPU。請務必在調整後使用燒機軟體監控實際的 CPU 電壓 (Vcore)。
  - **Synch ACDC Loadline with VRM Loadline**: `[Disabled]` `[Enabled]`
  - **CPU Current Capability**: `[Auto]` `[100%]` `[110%]` `[120%]`
  - **CPU VRM Switching Frequency**: `[Auto]` `[Manual]`
  - **VRM Spread Spectrum** (Only if CPU VRM Switching Frequency is `[Auto]`): `[Auto]` `[Disabled]` `[Enabled]`
  - **Fixed CPU VRM Switching Frequency(KHz)** (Only if CPU VRM Switching Frequency is `[Manual]`): 250kHz to 500kHz in 50kHz intervals.
  - **CPU Power Duty Control**: `[Auto]` `[T. Probe]` `[Extreme]`
  - **CPU Power Phase Control**: `[Auto]` `[Standard]` `[Extreme]`
  - **CPU Graphics Load-line Calibration** (Only with integrated graphics): `[Auto]` `[Level 1]` `[Level 2]` `[Level 3]` `[Level 4:Recommended for OC]` `[Level 5]` `[Level 6]` `[Level 7]`
  - **CPU Graphics VRM Switching Frequency**: `[Auto]` `[Manual]`
  - **Fixed CPU Graphics Switching Frequency(KHz)** (Only if CPU Graphics VRM Switching Frequency is `[Manual]`): 250KHz to 500KHz in 50KHz intervals.

- **Boot Voltages**
  - **CPU Core/Cache Boot Voltage**: `[Auto]` `[0.60000]` - `[1.70000]`
  - **CPU Input Boot Voltage**: `[Auto]` `[1.50000]` - `[2.10000]`
  - **PLL Input Boot Voltage**: `[Auto]` `[0.80000]` - `[1.80000]`
  - **CPU Standby Boot Voltage**: `[Auto]` `[0.80000]` - `[1.80000]`
  - **Memory Controller Boot Voltage**: `[Auto]` `[1.00000]` - `[2.00000]`

- **Auto Voltage Caps**
  - **CPU Core Auto Voltage Cap**: `[Auto]` `[0.60000]` - `[1.70000]`
  - **CPU Input Auto Voltage Cap**: `[Auto]` `[1.50000]` - `[2.10000]`
  - **Memory Controller Auto Voltage Cap**: `[Auto]` `[1.00000]` - `[2.00000]`

- **Internal CPU Power Management**
  - **功能**：此區塊直接控制 CPU 內部的電源管理機制，對超頻的穩定性、功耗及溫度有決定性的影響。此處的設定多數非常進階，不當的修改可能導致 CPU 效能下降、不穩定甚至損壞。
  - **重要參數**：
    - `Short Duration Package Power Limit (PL2)`: CPU 在短時間內 (通常是幾十秒) 能夠達到的最大功耗上限。這是決定 CPU Turbo Boost 最高頻率能維持多久的關鍵。超頻時通常會大幅提高此數值 (例如設為 4095W，等同於無限制)。
    - `Long Duration Package Power Limit (PL1)`: CPU 在長時間負載下所能維持的功耗上限，通常等同於 CPU 的 TDP (熱設計功耗)。
    - `Package Power Time Window`: PL2 能夠維持的時間長度。
    - `CPU Core/Cache Current Limit Max. (IccMax)`: CPU 核心與快取所能承受的最大電流上限。解鎖此限制是極限超頻的必要步驟，但同時也帶來更高的風險。
    - `IA AC/DC Load Line`: 調整 CPU 內部回報給主機板 VRM 的電阻值，讓 VRM 能更精準地供給電壓。調整此項需要搭配 `CPU Load-line Calibration` 進行，屬於非常精細的電壓調校。
  - **建議**：對於大多數超頻玩家，建議將 `Short Duration Package Power Limit`、`Long Duration Package Power Limit` 和 `CPU Core/Cache Current Limit Max.` 設定為最大值，以解除功耗牆與電流牆的限制，讓 CPU 的超頻潛力可以完全發揮。其他選項建議維持 `[Auto]`，除非您非常清楚其運作原理與潛在後果。
  - **Tcc Activation Offset**: `[Auto]` `[0]` - `[63]`
  - **IVR Transmitter VDDQ ICCMAX**: `[Auto]` `[0]` - `[15]`
  - **CPU Core/Cache Current Limit Max.**: `[Auto]` `[0.00]` - `[511.75]`
  - **CPU Graphics Current Limit**: `[Auto]` `[0.00]` - `[511.75]`
  - **Short Duration Package Power Limit**: `[Auto]` `[1]` - `[4095]`
  - **Package Power Time Window**: `[Auto]` `[1]` - `[448]`
  - **Short Duration Package Power Limit**: `[Auto]` `[1]` - `[4095]`
  - **Dual Tau Boost**: `[Disabled]` `[Enabled]`
  - **IA AC Load Line**: `[Auto]` `[0.01]` - `[62.49]`
  - **IA DC Load Line**: `[Auto]` `[0.01]` - `[62.49]`
  - **IA CEP Enable**: `[Auto]` `[Disabled]` `[Enabled]`
  - **GT CEP Enable**: `[Auto]` `[Disabled]` `[Enabled]`
  - **SA CEP Enable**: `[Auto]` `[Disabled]` `[Enabled]`
  - **IA SoC Iccmax Reactive Protector**: `[Auto]` `[Disabled]` `[Enabled]`
  - **Inverse Temperature Dependency Throttle**: `[Auto]` `[Disabled]` `[Enabled]`
  - **IA VR Voltage Limit**: `[Auto]` `[0]` - `[7999]`
  - **CPU SVID Support**: `[Auto]` `[Disabled]` `[Enabled]`

- **Tweaker’s Paradise**
  - **功能**：此區塊集合了最為進階且細微的調整選項，主要供極限超頻玩家使用，用於榨乾系統的最後一絲效能。此處的設定影響層面廣泛，且多數選項沒有明確的指引，需要大量的嘗試與驗證。
  - **重要參數**：
    - `Realtime Memory Timing`: 允許在作業系統內透過軟體即時調整記憶體時序，方便快速測試不同參數的效能影響，而無需頻繁重啟進入 BIOS。
    - `FLL OC mode`: 調整 CPU 內部 `頻率鎖相環 (Frequency Locked Loop)` 的運作模式，更高的等級 (Elevated) 有助於在極限超頻下維持 BCLK 的穩定性。
    - `Core PLL Voltage`, `GT PLL Voltage`, etc.: 手動調整各個鎖相環 (PLL) 的電壓。PLL 是生成 CPU 各部分時脈信號的關鍵元件，微調其電壓有時能幫助系統在更高的頻率下穩定下來，但不當設定也可能直接導致無法開機。
    - `UnderVolt Protection`: 禁用此選項可以讓您設定更低的 CPU 電壓，是降壓超頻 (Undervolting) 以降低功耗與溫度的關鍵步驟。
  - **建議**：強烈建議一般使用者不要修改此區塊的任何設定。對於希望挑戰極限頻率的專家級使用者，可以從 `FLL OC mode` 和各種 `PLL Voltage` 開始嘗試，並搭配專業的示波器等工具來監控信號品質，以進行科學化的調校。
  - **Realtime Memory Timing**: `[Disabled]` `[Enabled]`
  - **SPD Write Disable**: `[TRUE]` `[FALSE]`
  - **PVD Ratio Threshold**: `[Auto]` `[1]` - `[40]`
  - **SA PLL Frequency Override**: `[Auto]` `[3200 MHz]` `[1600 MHz]`
  - **BCLK TSC HW Fixup**: `[Enabled]` `[Disabled]`
  - **FLL OC mode**: `[Auto]` `[Disabled]` `[Normal]` `[Elevated]` `[Extreme Elevated]`
  - **UnderVolt Protection**: `[Disabled]` `[Enabled]`
  - **Core PLL Voltage**: `[Auto]` `[0.90000]` - `[1.84500]`
  - **GT PLL Voltage**: `[Auto]` `[0.90000]` - `[1.84500]`
  - **Ring PLL Voltage**: `[Auto]` `[0.90000]` - `[1.84500]`
  - **System Agent PLL Voltage**: `[Auto]` `[0.90000]` - `[1.84500]`
  - **Memory Controller PLL Voltage**: `[Auto]` `[0.90000]` - `[1.84500]`
  - **CPU 1.8V Small Rail**: `[Auto]` `[1.50000]` - `[2.30000]`
  - **PLL Termination Voltage**: `[Auto]` `[0.80000V]` - `[1.80000V]`
  - **CPU Standby Voltage**: `[Auto]` `[0.80000]` - `[1.80000]`
  - **PCH 1.05V Voltage**: `[Auto]` `[0.80000]` - `[1.60000]`
  - **PCH 0.82V Voltage**: `[Auto]` `[0.70000]` - `[1.00000]`
  - **CPU Input Voltage Reset Voltage**: `[Auto]` `[1.50000]` - `[2.10000]`

## 7. 進階選單（Advanced menu）

在進階選單（Advanced menu）裡的項目，為提供您變更 CPU 與其他系統裝置的 設定。

**注意！** 在您設定本進階選單的設定時，不正確的設定值將導致系統功能 異常。

### 7.1 平台各項設定（Platform Misc Configuration）

本項目用來設定與平台相關的功能。

- **PCI Express Native Power Management**
  - **功能**：啟用或關閉由作業系統（OS）控制的 PCI Express 主動狀態電源管理（ASPM）。
  - **`[Enabled]`**：允許作業系統根據裝置的使用狀況，動態地將 PCIe 裝置切換到 L0s（低延遲待機）或 L1（高延遲、更省電）等低功耗狀態，以節省電力。這是現代作業系統建議的設定。
  - **`[Disabled]`**：BIOS 將不向作業系統提供 ASPM 的控制權限，PCIe 裝置將保持在 L0（全速運作）狀態，功耗較高但可避免某些敏感裝置因狀態切換而產生延遲或相容性問題。
  - **建議**：一般情況下建議保持 `[Enabled]` 以獲得最佳的電源效率。若您遇到特定的擴充卡（如音效卡、擷取卡）出現不穩定、喚醒失敗或效能異常的問題，可以嘗試設為 `[Disabled]` 來進行故障排除。

### 7.2 CPU 設定（CPU Configuration）

本項目可讓您得知中央處理器的各項資訊與變更中央處理器的相關設定。此處的設定直接影響 CPU 的核心功能、效能表現與安全性。

- **Intel (VMX) Virtualization Technology**
  - **功能**：啟用或關閉 CPU 的硬體虛擬化支援。這是執行虛擬機器（VM）、模擬器（如 Android Studio）、或是 Windows 功能（如 Hyper-V、Windows Sandbox、適用於 Linux 的 Windows 子系統 WSL2）的必要條件。
  - **`[Enabled]`**：開啟硬體虛擬化功能。
  - **`[Disabled]`**：關閉此功能。
  - **建議**：強烈建議保持 `[Enabled]`。關閉此功能會導致絕大多數虛擬化應用程式無法運作。僅在極少數特定的舊版軟體或安全環境要求下才需考慮關閉。

- **Hyper-Threading**
  - **功能**：啟用或關閉 Intel 的同步多執行緒（SMT）技術。啟用後，每個實體 CPU 核心（Performance Core）可以模擬成兩個邏輯核心，讓作業系統能夠同時分派兩個執行緒給它，從从而提升多工處理和多執行緒應用程式的效能。
  - **`[Enabled]`**：開啟超執行緒功能。
  - **`[Disabled]`**：關閉此功能，每個實體核心只會被視為一個邏輯核心。
  - **建議**：對於絕大多數應用場景，包括日常使用、遊戲、影音編輯等，建議保持 `[Enabled]` 以獲得最佳的多工效能。在極少數對延遲極度敏感或受特定安全漏洞（如 MDS）影響的伺服器環境中，管理員可能會考慮關閉此功能。

- **Active Performance Cores / Active Efficient Cores**
  - **功能**：手動設定要啟用的 P-Core（效能核心）與 E-Core（效率核心）的數量。這可以用於特定的測試場景或故障排除。
  - **建議**：除非有特殊需求（例如，模擬不同等級的 CPU 或測試軟體在特定核心數下的表現），否則應一律保持為 `[All]`，以確保 CPU 的所有核心都能正常運作，發揮完整效能。

- **Intel Trusted Execution Technology (TXT)**
  - **功能**：提供一個更安全的硬體執行環境，用於保護系統免受軟體層面的攻擊。它透過建立一個受信任的啟動鏈和執行空間，確保關鍵程式碼和資料的完整性與機密性。
  - **建議**：此功能主要用於有高度安全需求的企業或伺服器環境。對於一般個人使用者，通常不需要啟用此功能，保持 `[Disabled]` 即可，啟用它可能需要搭配特定的軟硬體設定（如 TPM），設定不當可能導致開機問題。

#### CPU - Power Management Control

此區塊的設定控制著 CPU 的節能技術與效能狀態切換，對於系統的功耗、溫度與反應速度有著直接影響。

- **Intel(R) SpeedStep(tm)**
  - **功能**：Intel 的動態頻率調整技術。啟用後，CPU 會在低負載時自動降低核心倍頻與電壓，以節省電力並降低發熱；當需要處理運算任務時，則會瞬間拉高至應有效能。
  - **建議**：務必保持 `[Enabled]`。關閉此功能會讓 CPU 持續運行在較高的基礎頻率，導致不必要的功耗與發熱，百害而無一利。

- **Intel(R) Speed Shift Technology**
  - **功能**：這是 SpeedStep 的進化版，將頻率與電壓的控制權從作業系統層級轉移到 CPU 硬體層級。這使得 CPU 能夠以更快的速度（通常在毫秒級）回應效能需求，提升系統的反應速度與能效。
  - **建議**：務必保持 `[Enabled]` 以獲得最佳的系統反應性。

- **Intel(R) Turbo Boost Max Technology 3.0**
  - **功能**：此技術會自動識別 CPU 中體質最好的一或兩個核心（稱為 "Favored Cores"），並在單執行緒或雙執行緒負載下，將這幾個核心的頻率提升到比標準 Turbo Boost 更高的水平，以最大化單核效能。
  - **建議**：保持 `[Enabled]`，特別對於遊戲或對單核效能敏感的應用程式，能帶來顯著的幫助。

- **Turbo Mode**
  - **功能**：這是 CPU 自動超頻功能的總開關。啟用後，CPU 會在散熱與功耗允許的範圍內，自動將核心頻率提升至超過基礎頻率的 Turbo 頻率，以應對高運算負載。
  - **建議**：務必保持 `[Enabled]`。關閉此功能將使 CPU 只能運行在基礎頻率，損失大量的潛在效能。

- **CPU C-states**
  - **功能**：CPU 的閒置電源狀態。C-states 定義了 CPU 在不同閒置程度下可以進入的睡眠模式。C0 是正常運作狀態，數字越大的狀態（如 C6, C7, C10）代表 CPU 進入越深層的睡眠，關閉越多的內部單元，從从而節省越多的電力。
  - **建議**：建議保持 `[Enabled]`。這能大幅降低系統在待機或低負載時的功耗。僅在極少數情況下，如進行極限超頻穩定性調校、或使用對延遲極度敏感的即時系統（Real-time System）時，才需要考慮關閉 C-states 以避免喚醒延遲。

### 7.3 系統代理設定（System Agent Configuration）

本選單可讓您變更系統代理（System Agent, SA）的各項相關設定。系統代理是 CPU 中負責管理記憶體控制器、PCI Express 控制器、內建顯示晶片等核心 I/O 功能的區塊。

- **VT-d**
  - **功能**：Intel 的 `虛擬化技術 Direct I/O (Virtualization Technology for Directed I/O)`。它允許虛擬機器（VM）直接、獨佔地存取特定的硬體裝置（如網卡、儲存控制器、GPU），而無需透過 Hypervisor（虛擬層）進行轉譯。
  - **優點**：大幅提升 VM 的 I/O 效能，降低延遲，並增強安全性，因為它可以將硬體隔離給指定的 VM 使用。這是建立高效能虛擬化伺服器（例如，建立 NAS、軟路由或遊戲 VM）的關鍵技術。
  - **建議**：如果您有使用虛擬機器的需求，特別是需要高效能 I/O 的場景（如 Proxmox VE, ESXi），務必將此項設為 `[Enabled]`。對於不使用虛擬機的一般使用者，保持預設值即可。

#### Memory Configuration

- **Memory Remap**:
  - **功能**：處理 32 位元作業系統的 4GB 記憶體定址限制。啟用後，如果系統安裝了超過 4GB 的記憶體，此功能會將被舊式 32 位元硬體 I/O 位址空間遮蔽的記憶體區域，重新映射到 4GB 以上的位址空間，以確保作業系統能存取到全部的實體記憶體。
  - **建議**：在現今 64 位元作業系統普及的時代，此選項應永遠保持 `[Enabled]`。關閉它可能導致系統無法識別並使用全部安裝的記憶體。

#### Graphics Configuration

- **Primary Display**:
  - **功能**：設定系統開機時優先初始化的顯示輸出裝置。
  - **`[Auto]`**：BIOS 自動偵測連接的顯示器，通常優先級為 PCIe 獨立顯示卡 > 內建顯示晶片。
  - **`[CPU Graphics]`**：強制使用 CPU 的內建顯示晶片作為主要輸出。
  - **`[PEG Slot]` / `[PCIE]`**: 強制使用安裝在主要 PCIe x16 插槽上的獨立顯示卡作為主要輸出。
  - **建議**：一般使用者保持 `[Auto]` 即可。若您同時使用內顯與獨顯，並希望指定 BIOS POST 畫面顯示在哪個螢幕上，可以在此手動設定。

- **iGPU Multi-Monitor**:
  - **功能**：啟用或關閉內建顯示晶片（iGPU）與獨立顯示卡（dGPU）同時運作的功能。
  - **`[Enabled]`**：即使安裝了獨立顯示卡，內顯依然保持啟用狀態，允許您連接更多螢幕，或使用 Intel Quick Sync Video 等內顯加速功能來進行影片編碼/解碼，同時用獨顯玩遊戲。
  - **`[Disabled]`**：當偵測到獨立顯示卡時，自動停用內顯以節省資源。
  - **建議**：若您有多螢幕需求，或需要使用內顯的特定加速功能，請設為 `[Enabled]`。否則，可以設為 `[Disabled]` 以簡化系統配置。

#### VMD setup menu

- **Enable VMD controller**:
  - **功能**：啟用 Intel 的 `Volume Management Device (VMD)` 控制器。VMD 是一種先進的儲存管理技術，它允許您將多個直接連接到 CPU PCIe 通道的 NVMe SSD 組合成一個 RAID 陣列，而無需透過晶片組（PCH）。這能提供更低延遲、更高頻寬的 NVMe RAID 效能。它也是實現 NVMe SSD 熱插拔（Hot-Plug）功能的關鍵。
  - **建議**：若您計畫使用多個 NVMe SSD 組建 RAID 0/1/5 陣列以追求極致速度或資料備援，請將此項設為 `[Enabled]`。啟用後，安裝作業系統時可能需要手動載入 Intel RST VMD 驅動程式才能識別到磁碟。對於僅使用單一 NVMe SSD 的使用者，保持 `[Disabled]` 可以簡化安裝過程。

### 7.4 PCH 設定（PCH Configuration）

本項目可以管理與設定 PCH（Platform Controller Hub，平台控制器中樞，即晶片組）的相關功能。PCH 負責處理主機板上絕大多數的 I/O 功能，例如 SATA、USB、網路以及額外的 PCIe 插槽。

#### PCI Express Configuration

此處的設定讓您能手動調整連接到 PCH 的 PCIe 插槽與 M.2 插槽的運作速率。

- **M.2_x / PCIEX_x Link Speed**
  - **功能**：手動設定特定插槽的 PCIe 世代（Generation）。較高的世代意味著更高的頻寬（例如，Gen4 的頻寬是 Gen3 的兩倍）。
  - **`[Auto]`**：主機板會自動與安裝的裝置協商，以該裝置所能支援的最高速率運作。
  - **手動設定 (Gen1/Gen2/Gen3...)**：強制插槽以指定的較低速率運作。
  - **建議**：絕大多數情況下應保持 `[Auto]`，以確保所有裝置都能發揮最佳效能。僅在遇到特定擴充卡或 NVMe SSD 的相容性或穩定性問題時，才需要嘗試手動將其降速至 Gen3 或 Gen2 來進行故障排除。這在PCIe延長線品質不佳時也可能是解決方案之一。

### 7.5 PCH 儲存裝置設定（PCH Storage Configuration）

當您進入 BIOS 設定程式時，BIOS 設定程式將自動偵測已安裝的 SATA 裝置。此選單用於管理連接到晶片組（PCH）的 SATA 硬碟與 SSD。

- **SATA Controller(s)**
  - **功能**：主機板上 SATA 控制器的總開關。
  - **`[Enabled]`**：啟用 SATA 控制器，允許系統偵測並使用連接到 SATA 連接埠上的裝置。
  - **`[Disabled]`**：完全關閉 SATA 控制器。所有 SATA 裝置將無法被偵測。
  - **建議**：只要您有使用任何 SATA 硬碟或 SSD，就必須保持 `[Enabled]`。只有在您的系統完全使用 NVMe SSD 且沒有任何 SATA 裝置時，才可以考慮關閉此選項以節省微量資源並加快開機速度。

- **Aggressive LPM support**
  - **功能**：啟用 `積極連結電源管理 (Aggressive Link Power Management, ALPM)`。這是一種 SATA 節能技術，允許 SATA 控制器在磁碟閒置時，主動將 SATA 連結置於低功耗狀態，以節省電力。
  - **建議**：對於筆記型電腦或希望最大化節能的系統，可以設為 `[Enabled]`。然而，此功能在某些舊款 SSD 或 HDD 上可能存在相容性問題，可能導致磁碟隨機失去回應或系統卡頓。如果遇到儲存裝置不穩定的問題，將此項設為 `[Disabled]` 是一個重要的故障排除步驟。

- **SATA6G_x / SLIMSAS_x Hot Plug**
  - **功能**：為指定的 SATA 或 SlimSAS 連接埠啟用熱插拔功能。
  - **`[Enabled]`**：允許您在作業系統運行中，安全地插入或移除該連接埠上的硬碟，類似於 USB 隨身碟。作業系統會將其識別為可移除裝置。
  - **`[Disabled]`**：停用熱插拔功能。硬碟被視為固定裝置，不應在開機狀態下移除。
  - **建議**：對於需要頻繁更換硬碟的伺服器或外接硬碟抽取盒，啟用此功能非常方便。對於安裝在機殼內部、作為系統碟或固定資料碟的硬碟，建議保持 `[Disabled]`，以避免誤操作或被作業系統誤判為可移除裝置。

### 7.6 PCH-FW 設定（PCH-FW Configuration）

本選單用來設定與晶片組韌體（Firmware）及安全性相關的功能。

- **PTT (Platform Trust Technology)**
  - **功能**：啟用 Intel 的平台信任技術，這是一種內建於晶片組韌體中的 `信賴平台模組 (Trusted Platform Module, TPM)` 解決方案。它提供了與實體 TPM 晶片相同的功能，例如安全地儲存加密金鑰、憑證和密碼。
  - **用途**：啟用 PTT 是使用 Windows BitLocker 磁碟加密、安全開機（Secure Boot）以及安裝 Windows 11 的必要條件。
  - **建議**：強烈建議設為 `[Enable]` 以符合現代作業系統的安全要求。只有在您另外安裝了獨立的 TPM 2.0 模組並希望使用它時，才需要考慮關閉 PTT。

### 7.7 AMT 設定（AMT Configuration）

本選單用來設定 Intel® Active Management Technology (AMT) 的相關功能。AMT 是一項強大的遠端管理技術，主要應用於企業環境。

- **功能**：AMT 允許 IT 管理員透過網路，對電腦進行頻外（Out-of-band）管理。這意味著即使電腦處於關機狀態（但連接電源）、作業系統當機或尚未安裝，管理員依然可以遠端：
    - 開機、關機、重新啟動電腦。
    - 存取 BIOS 設定。
    - 重新導向開機磁碟（例如，從遠端的 ISO 檔案開機並安裝系統）。
    - 透過 `Keyboard-Video-Mouse (KVM)` 功能，直接遠端遙控桌面。
- **建議**：
    - **企業使用者**：請根據公司 IT 部門的政策進行設定。通常需要啟用此功能以納入中央管理。
    - **個人使用者**：強烈建議保持 `[Disabled]`。AMT 功能複雜，且若設定不當或密碼強度不足，可能成為潛在的安全漏洞，讓攻擊者有機會遠端控制您的系統。對於家庭使用者來說，幾乎沒有需要使用此功能的場景。

### 7.12 PCI 子系統設定（PCI Subsystem Settings）

本項目提供您設定 PCI、PCI-X 和 PCI Express 相關的底層功能，這些設定對於相容性與效能至關重要。

- **Above 4G Decoding**
  - **功能**：啟用或關閉對 64 位元 PCI 裝置的定址支援。當系統中安裝了需要大量 PCI 位址空間的裝置（例如高階顯示卡、多個 NVMe SSD、專業運算卡）時，可能會需要超過傳統 32 位元定址所能提供的 4GB 上限。啟用此選項允許系統將這些裝置的記憶體映射到 4GB 以上的位址空間。
  - **建議**：強烈建議設為 `[Enabled]`。這是啟用 `Re-Size BAR` 的先決條件，並且是確保現代高效能硬體正常運作的基礎。只有在遇到極少數舊版 32 位元擴充卡相容性問題時，才需考慮關閉。

- **Re-Size BAR Support**
  - **功能**：啟用 `Resizable Base Address Register (可調整大小的基底定址暫存器)` 功能，也稱為 `Smart Access Memory (SAM)`。啟用後，CPU 可以一次性地存取顯示卡的全部視訊記憶體（VRAM），而不是傳統的以 256MB 區塊進行分次存取。
  - **優點**：消除了 CPU 與 GPU 之間的資料傳輸瓶頸，在許多遊戲和專業應用中可以帶來明顯的效能提升（通常在 5% - 15% 之間）。
  - **建議**：若您的 CPU（Intel 10 代及更新 / AMD Ryzen 5000 系列及更新）和 GPU（NVIDIA RTX 30 系列及更新 / AMD RX 6000 系列及更新）都支援此技術，務必設為 `[Enabled]` 以獲得免費的效能提升。啟用前需先開啟 `Above 4G Decoding`。

- **SR-IOV Support**
  - **功能**：啟用 `單根 I/O 虛擬化 (Single Root I/O Virtualization)` 支援。這是一種硬體標準，允許單一的實體 PCIe 裝置（例如高階網卡或 GPU）將自己分割成多個輕量化的虛擬功能（Virtual Functions, VFs）。
  - **用途**：在虛擬化環境中，每個虛擬功能可以被直接指派給一個虛擬機器，讓 VM 能夠近乎無效能損失地共享該實體裝置。這比軟體模擬的裝置共享效率高得多。
  - **建議**：此為高階虛擬化應用功能。對於一般使用者，保持 `[Disabled]` 即可。對於需要建置高效能虛擬化平台的專業人士，當使用支援 SR-IOV 的硬體時，應在此處將其 `[Enabled]`。

### 7.13 USB 設定（USB Configuration）

本選單可讓您變更 USB 裝置的各項相關設定，特別是關於舊版作業系統的相容性。

- **Legacy USB Support**
  - **功能**：在不原生支援 USB 的環境中（例如 DOS、UEFI/BIOS 選單、Windows 7 安裝過程），提供對 USB 鍵盤和滑鼠的模擬支援。BIOS 會將 USB 裝置模擬成舊式的 PS/2 裝置，讓這些環境能夠識別並使用它們。
  - **`[Enabled]`**：完全啟用模擬功能。
  - **`[Auto]`**：系統會自動偵測環境，僅在需要時才啟用模擬。
  - **`[Disabled]`**：完全關閉模擬。在 UEFI 環境或 modern OS（Win 10/11）中，USB 裝置仍可正常使用，但在 BIOS 選單或舊版系統中可能會失效。
  - **建議**：建議設定為 `[Enabled]` 或 `[Auto]` 以確保在任何情況下（包括進入 BIOS、使用開機選單）USB 鍵盤和滑鼠都能正常運作。

- **XHCI Hand-off**
  - **功能**：將 USB 3.0 (XHCI) 控制器的控制權從 BIOS/UEFI 轉交給作業系統。
  - **`[Enabled]`**：對於不原生支援 XHCI 的作業系統（如 Windows 7），由 BIOS 接管控制，待系統載入驅動後再交接。這能確保在這些舊系統的安裝和使用過程中，USB 3.0 連接埠依然可用。
  - **`[Disabled]`**：BIOS 不會介入，完全由作業系統負責控制 XHCI 控制器。
  - **建議**：對於現代作業系統（Windows 10, Windows 11, Linux），建議設為 `[Disabled]`，因為它們都有原生的 XHCI 驅動程式。若您需要安裝或使用 Windows 7，則必須將此項設為 `[Enabled]`。

### 7.17 APM 設定（APM Configuration）

本選單中的項目可用來調整進階電源管理（Advanced Power Management, APM）設定，主要控制系統在不同關機狀態下的行為。

- **Restore AC Power Loss**
  - **功能**：設定當外部電力供應中斷後又恢復時，電腦的反應。
  - **`[Power Off]`**：電力恢復後，電腦將保持關機狀態，需要手動按下電源鈕才能開機。這是最安全的設定。
  - **`[Power On]`**：電力恢復後，電腦將自動開機。這對於需要 7x24 小時運作的伺服器或遠端主機非常有用，可確保在意外斷電後服務能自動恢復。
  - **`[Last State]`**：電力恢復後，電腦將回復到斷電前的狀態。如果斷電時電腦是開著的，它會自動開機；如果當時是關機的，則會保持關機。
  - **建議**：一般家用電腦建議設為 `[Power Off]`。伺服器或無人值守的設備可根據需求設為 `[Power On]`。

- **ErP Ready**
  - **功能**：設定是否符合歐盟的 `能源相關產品 (Energy-related Products, ErP)` 指令規範。啟用此功能會讓系統在 S4（休眠）和 S5（關機）狀態下，將耗電量降至極低的水平（通常小於 0.5W）。
  - **影響**：為了達到此超低功耗標準，啟用 ErP 會關閉一些關機狀態下的功能，例如 `Power On By PCI-E`（網路喚醒）、`Power On By RTC`（定時開機）以及 USB 埠的關機供電。
  - **建議**：如果您希望最大化節能，並且不需要使用網路喚醒或定時開機等功能，可以將其 `[Enabled]`。若您需要使用這些喚醒功能，則必須將 ErP 設為 `[Disabled]`。

- **Power On By PCI-E**
  - **功能**：啟用或關閉 `網路喚醒 (Wake-on-LAN, WOL)` 功能。允許來自網路的特殊封包（稱為 "Magic Packet"）將處於睡眠或關機狀態的電腦喚醒。
  - **建議**：若您需要遠端喚醒此電腦，請將此項設為 `[Enabled]`，並確保 `ErP Ready` 為 `[Disabled]`。

- **Power On By RTC**
  - **功能**：啟用或關閉 `即時時鐘 (Real-time Clock, RTC)` 喚醒功能，也就是俗稱的「定時開機」。啟用後，您可以設定一個特定的日期和時間，讓電腦自動開機。
  - **建議**：若您有排程備份、定時執行任務等需求，可將此項設為 `[Enabled]`，並同樣確保 `ErP Ready` 為 `[Disabled]`。

### 7.18 內建裝置設定（OnBoard Devices Configuration）

本選單可讓您變更主機板上各種內建晶片與裝置的開關和運作模式。

- **PCIe Bandwidth Bifurcation Configuration**
  - **功能**：`PCIe 通道拆分`。允許您將一個實體的 PCIe x16 插槽的頻寬，拆分成多個較小的組合。例如，將 `[x16]` 模式改為 `[x8/x8]` 模式。
  - **用途**：主要用於搭配特殊的轉接卡（Riser Card），讓您可以在一個 x16 插槽上同時安裝兩張 x8 的擴充卡，或者安裝四張 M.2 NVMe SSD（需要 `[x4/x4/x4/x4]` 模式）。
  - **建議**：除非您確定正在使用需要通道拆分的特殊轉接卡，否則請務必保持在預設的 `[Auto Mode]` 或 `[x16]` 模式。不正確的設定會導致安裝在該插槽上的裝置降速或無法被識別。

- **HD Audio / Intel 2.5G LAN1/2**
  - **功能**：內建音效卡與網路卡的總開關。
  - **建議**：若您不使用內建的音效或網路功能（例如，您安裝了獨立的音效卡或萬兆網卡），可以在此將其 `[Disabled]`，以釋放系統資源（中斷、I/O 位址等）並可能減少潛在的驅動衝突。

- **SlimSAS Configuration**
  - **功能**：設定 SlimSAS 連接埠的運作模式。SlimSAS 是一種高密度接口，可以根據設定傳輸不同的訊號。
  - **`[SATA mode]`**：讓 SlimSAS 接口作為多個 SATA 連接埠使用，通常一條 SlimSAS 線可轉接成四個標準 SATA 裝置。
  - **`[PCIE mode]`**：讓 SlimSAS 接口作為 PCIe 通道使用，用於連接高效能的 NVMe 儲存裝置（如 U.2/U.3 SSD）。
  - **建議**：請根據您所連接的裝置類型來選擇正確的模式。

## 8. 監控選單（Monitor menu）

監控選單可讓您即時檢視系統各項重要的硬體感測器數據，並對散熱風扇的行為進行精細的控制。這是確保系統在各種負載下都能維持穩定運作的關鍵頁面。

- **Temperature / Fan Speed / Voltage Monitor**
  - **功能**：此區域以唯讀方式顯示來自全板各處感測器的即時數據，包括 CPU 溫度、主機板各區域溫度（VRM、晶片組）、風扇轉速、CPU 核心電壓以及電源供應器的各路電壓（+12V, +5V, +3.3V）等。
  - **用途**：這是監控系統健康狀況最重要的工具。當您進行超頻、壓力測試或感覺系統不穩定時，應密切關注此處的溫度與電壓讀數，確保它們都維持在安全的範圍內。例如，CPU 溫度在高負載下不應長時間超過 90-95°C，各路電壓的波動範圍應在 ±5% 以內。

#### Q-Fan Configuration

此區塊是華碩智慧風扇控制系統的核心，讓您能夠為每個連接到主機板的風扇，自訂其轉速與溫度的對應曲線。

- **Q-Fan Tuning**
  - **功能**：這是一個自動偵測與校準的程序。執行後，BIOS 會讓所有風扇從最低轉速運轉至最高轉速，以偵測每個風扇的 PWM 控制範圍與最低啟動電壓。
  - **建議**：在首次設定或更換風扇後，**務必**執行一次 `Q-Fan Tuning`。這能確保後續的風扇曲線設定（不論是自動還是手動）都是基於準確的風扇性能數據，避免風扇在低轉速下停轉或無法啟動。

- **CPU/Chassis Fan Profile**
  - **功能**：提供多種預設的風扇運轉策略。
  - **`[Standard]`**：在效能與噪音之間取得平衡，適用於大多數日常使用場景。
  - **`[Silent]`**：以靜音為優先，風扇轉速會盡可能地維持在較低水平，適合文書處理、上網等低負載工作。
  - **`[Turbo]`**：以散熱效能為優先，風扇轉速會對溫度變化做出更積極的反應，適合遊戲或高強度運算。
  - **`[Full Speed]`**：忽略溫度監控，所有風扇直接以 100% 全速運轉，噪音極大，僅用於壓力測試或極限超頻。
  - **`[Manual]`**：允許您手動設定風扇轉速曲線的三個溫度/轉速對應點。

- **Chassis Fan Q-Fan Source**
  - **功能**：設定機殼風扇的轉速要依據哪個感測器的溫度來進行調整。預設是 `[CPU]` 溫度。
  - **建議**：您可以根據機殼風扇的位置來設定。例如，負責吹拂顯示卡的下方進氣風扇，可以將其溫度來源改為 `[PCH]` 或 `[T_Sensor]`（如果您有外接溫度探測線並貼在顯示卡附近）；負責吹拂 VRM 供電模組的後方排氣風扇，可以將溫度來源設為 `[VRM]`。精確的設定能讓機殼風道更智慧地應對熱源變化。

## 9. 啟動選單（Boot menu）

本選單可讓您變更系統啟動裝置、開機順序以及與啟動模式和安全性相關的關鍵功能。

#### CSM (Compatibility Support Module)

CSM 是一個在現代 UEFI 韌體中提供的相容性層，其唯一目的是為了能夠啟動舊版的作業系統或使用不支援 UEFI 的舊款硬體。

- **Launch CSM**
  - **功能**：CSM 功能的總開關。
  - **`[Enabled]`**：啟用相容性模式。這將允許系統辨識並啟動使用傳統 MBR 分割區格式的硬碟，並載入舊式的 Option ROM（例如，沒有 UEFI GOP 驅動的舊顯示卡）。
  - **`[Disabled]`**：完全關閉相容性模式，系統將以純粹的 UEFI 模式運作。這是現代作業系統的標準啟動方式。
  - **建議**：對於安裝與使用現代作業系統（如 Windows 10, Windows 11），強烈建議**務必**將此項設為 `[Disabled]`。純 UEFI 模式提供更快的開機速度、更好的安全性，並且是使用 Secure Boot、Re-Size BAR 等新技術的必要前提。只有在您需要安裝 Windows 7 或其他舊版 Legacy 作業系統時，才需要啟用 CSM。

#### Secure Boot

安全啟動是一項重要的安全標準，它透過驗證開機過程中所有軟體（包括韌體、驅動程式、作業系統開機載入器）的數位簽章，來防止惡意軟體（如 Rootkit）在作業系統啟動前載入，從而保護系統的完整性。

- **OS Type**
  - **功能**：選擇要載入的安全啟動金鑰資料庫。
  - **`[Windows UEFI Mode]`**：載入微軟官方發布的標準金鑰。這將允許所有經過微軟認證的作業系統（包括 Windows）和第三方驅動程式（如主流 Linux 發行版 Ubuntu、Fedora 的開機載入器）正常啟動。
  - **`[Other OS]`**：清除預設金鑰，允許您手動管理金鑰資料庫，適用於需要安裝自訂金鑰或不支援微軟金鑰的作業系統。
  - **建議**：對於絕大多數使用者（包括 Windows 和主流 Linux 用戶），應選擇 `[Windows UEFI Mode]`。

- **Secure Boot Mode**
  - **功能**：設定金鑰的管理方式。
  - **`[Standard]`**：最簡單的模式，自動載入主機板內建的標準安全金鑰。
  - **`[Custom]`**：允許進階使用者手動匯入或刪除平台金鑰（PK）、金鑰交換金鑰（KEK）、已授權簽章資料庫（db）與已撤銷簽章資料庫（dbx）。
  - **建議**：一般使用者應使用 `[Standard]` 模式。`[Custom]` 模式僅供需要簽署自有驅動程式或開機載入器的開發者或安全專家使用。

**重要提示**：要成功啟用 Secure Boot，`Launch CSM` 必須設定為 `[Disabled]`。Windows 11 的安裝與運行強制要求系統支援並啟用 Secure Boot。

#### Boot Configuration

- **Fast Boot**
  - **功能**：透過在開機自我測試（POST）過程中跳過部分硬體檢測（例如，部分 USB 裝置的初始化），來縮短開機時間。
  - **建議**：可以設為 `[Enabled]` 以加快開機速度。但如果您發現在開機時無法使用 USB 鍵盤進入 BIOS，或者某些週邊裝置需要較長時間才能被辨識，應將此項設為 `[Disabled]`。

## 10. 工具選單（Tool menu）

工具選單整合了多種華碩獨家的實用工具程式，讓您能更方便地管理與維護您的 BIOS。

- **Start ASUS EzFlash**
  - **功能**：啟動華碩的 `EzFlash` BIOS 更新工具。這是一個圖形化的工具程式，內建於 BIOS ROM 中，允許您直接從 USB 隨身碟讀取 BIOS 檔案（通常是 `.CAP` 檔案）並進行更新，而無需進入作業系統。
  - **建議**：這是更新 BIOS 最推薦、最安全的方式。在更新前，請確保已從華碩官方網站下載適用於您主機板型號的正確 BIOS 檔案，並將其存放在 USB 隨身碟的根目錄。更新過程中**切勿**斷電或重置系統。

### 10.1 華碩 User Profile

- **功能**：此功能允許您將目前所有 BIOS 設定儲存成一個設定檔，並可隨時載入。您最多可以儲存多組不同的設定檔。
- **用途**：這對於超頻玩家來說極為有用。您可以將一組穩定的日常使用設定儲存起來，同時嘗試另一組更激進的效能設定。當超頻失敗導致無法開機時，只需清除 CMOS 後，即可輕鬆載回之前儲存的穩定設定檔，省去重新設定數十個選項的麻煩。

### 10.3 華碩 Armoury Crate

- **Download & Install ARMOURY CRATE app**
  - **功能**：設定是否在首次進入 Windows 作業系統時，自動提示並協助您下載安裝 `Armoury Crate` 整合式軟體。
  - **Armoury Crate 功能**：它是一個控制中心，整合了驅動程式自動更新、AURA SYNC 燈效控制、風扇轉速監控、獨家軟體下載等多種功能。
  - **建議**：若您希望簡化驅動程式的安裝流程並使用華碩的燈效同步等功能，可以將其設為 `[Enabled]`。若您偏好手動安裝所有驅動程式，不希望任何軟體自動安裝，則可以將其設為 `[Disabled]`。

## 11. IPMI 選單（IPMI menu）

本選單可以設定 `智慧平台管理介面 (Intelligent Platform Management Interface, IPMI)`。IPMI 是一套應用於伺服器電腦的工業標準，它允許系統管理員進行頻外管理（Out-of-Band Management）。

- **核心功能**：透過主機板上一個獨立的微控制器——`基板管理控制器 (Baseboard Management Controller, BMC)`，IPMI 提供了一套獨立於主 CPU、BIOS 和作業系統的監控與管理系統。BMC 擁有自己專屬的網路接口（通常與 LAN1共享，或有獨立的管理埠），只要伺服器接著電源線，即使在關機、當機或未安裝作業系統的狀態下，管理員也能遠端執行以下操作：
    - **電源控制**：遠端開機、關機、重啟。
    - **硬體監控**：監控溫度、電壓、風扇轉速等感測器數據。
    - **KVM over IP**：透過網路遠端操作鍵盤、滑鼠，並觀看螢幕畫面，就像坐在機器前面一樣。可以遠端進入 BIOS、安裝作業系統。
    - **虛擬媒體**：將本地端的 ISO 檔案或光碟機掛載到遠端伺服器上，作為開機磁碟。
    - **事件記錄**：檢視 BMC 記錄的系統事件日誌（System Event Log, SEL），用於故障排除。

- **BMC 網路設定 (BMC network configuration)**
  - **功能**：設定 BMC 專用網路接口的 IP 位址。
  - **`[Static]`**：手動為 BMC 設定一個固定的 IP 位址。這是伺服器管理的標準做法，以確保管理位址不會改變。
  - **`[DynamicBmcDhcp]`**：讓 BMC 透過 DHCP 自動從網路中的 DHCP 伺服器（通常是路由器）獲取 IP 位址。
  - **建議**：在正式的伺服器環境中，強烈建議使用 `[Static]` IP 以方便管理。在家庭或測試環境中，可以使用 `[DynamicBmcDhcp]`，但您需要登入路由器查看分配給 BMC 的 IP 位址才能進行連線。連線通常是透過在瀏覽器輸入 `https://<BMC 的 IP 位址>` 來存取其 Web 管理介面。

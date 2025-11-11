**Pro WS**  

**W680-ACE** 系列 

**BIOS** 使用手冊   
**Motherboar~~d~~**  
T21328 

第一版 

2022 年 12 月發行

版權說明 

© ASUSTeK Computer Inc. All rights reserved. 華碩電腦股份有限公司保留所有權利 本使用手冊包括但不限於其所包含的所有資訊皆受到著作權法之保護，未經華碩 電腦股份有限公司（以下簡稱「華碩」）許可，不得任意地仿製、拷貝、謄抄、轉 譯或為其他利用。 

免責聲明 

本使用手冊是以「現況」及「以目前明示的條件下」的狀態提供給您。在法律允 許的範圍內，華碩就本使用手冊，不提供任何明示或默示的擔保及保證，包括但不 限於商業適銷性、特定目的之適用性、未侵害任何他人權利及任何得使用本使用手 冊或無法使用本使用手冊的保證，且華碩對因使用本使用手冊而獲取的結果或透過 本使用手冊所獲得任何資訊之準確性或可靠性不提供擔保。 

台端應自行承擔使用本使用手冊的所有風險。 台端明確了解並同意，華碩、華 碩之授權人及其各該主管、董事、員工、代理人或關係企業皆無須為您因本使用手 冊、或因使用本使用手冊、或因不可歸責於華碩的原因而無法使用本使用手冊或其 任何部分而可能產生的衍生、附隨、直接、間接、特別、懲罰或任何其他損失（包 括但不限於利益損失、業務中斷、資料遺失或其他金錢損失）負責，不論華碩是否 被告知發生上開損失之可能性。 

由於部分國家或地區可能不允許責任的全部免除或對前述損失的責任限制，所以 前述限制或排除條款可能對您不適用。 

台端知悉華碩有權隨時修改本使用手冊。本產品規格或驅動程式一經改變，本使 用手冊將會隨之更新。本使用手冊更新的詳細說明請您造訪華碩的客戶服務網 http:// support.asus.com，或是直接與華碩資訊產品技術支援專線 0800-093-456 聯絡。 

於本使用手冊中提及之第三人產品名稱或內容，其所有權及智慧財產權皆為各別 產品或內容所有人所有且受現行智慧財產權相關法令及國際條約之保護。 當下列兩種情況發生時，本產品將不再受到華碩之保固及服務： 

（1）本產品曾經過非華碩授權之維修、規格更改、零件替換或其他未經過華碩授權 的行為。 

（2）本產品序號模糊不清或喪失。 

本產品的名稱與版本都會印在主機板/顯示卡上，版本數字的編碼方式是用三個數 字組成，並有一個小數點做間隔，如 1.02G、2.03G 等...數字愈大表示版本愈新，而 愈左邊位數的數字更動表示更動幅度也愈大。更新的詳細說明請您到華碩的全球資 訊網瀏覽或是直接與華碩聯絡。   
目錄 

1\. 認識 BIOS 程式........................................................................................................................... 5 2\. BIOS 程式設定 ............................................................................................................................ 6 3\. 管理、更新您的 BIOS 程式................................................................................................... 7 

3.1 華碩 CrashFree BIOS 3 程式.............................................................................. 7 3.2 使用華碩 EzFlash 更新程式............................................................................... 8 4\. BIOS 選單畫面 ............................................................................................................................ 9 4.1 功能表列說明........................................................................................................... 9 4.2 選單項目 ..................................................................................................................10 4.3 子選單.......................................................................................................................10 4.4 操作功能鍵說明....................................................................................................10 4.5 一般說明 ..................................................................................................................10 4.6 設定值.......................................................................................................................10 4.7 設定視窗 ..................................................................................................................10 4.8 捲軸 .........................................................................................................................10 5\. 主選單（Main Menu）...........................................................................................................11 6\. Ai Tweaker 選單（Ai Tweaker menu）...........................................................................14 7\. 進階選單（Advanced menu）..............................................................................................35 7.1 平台各項設定（Platform Misc Configuration）......................................36 7.2 CPU 設定（CPU Configuration）...................................................................37 7.3 系統代理設定（System Agent Configuration） .......................................42 7.4 PCH 設定（PCH Configuration）...................................................................44 7.5 PCH 儲存裝置設定（PCH Storage Configuration）...............................45 7.6 PCH-FW 設定（PCH-FW Configuration）...................................................46 7.7 AMT 設定（AMT Configuration）.................................................................46 7.8 Thunderbolt(TM) 設定（Thunderbolt(TM) Configuration）.................48 7.9 Redfish Host 介面設定（Redfish Host Interface Settings）..............50 7.10 序列埠控制面板重新定向（Serial Port Console Redirection）.....50 7.11 Intel TXT 資訊（Intel TXT Information）...............................................52 7.12 PCI 子系統設定（PCI Subsystem Settings）..........................................53 7.13 USB 設定（USB Configuration）................................................................53 7.14 網路協定堆疊設定（Network Stack Configuration）.........................54 7.15 NVMe 設定（NVMe Configuration）.........................................................55 7.16 HDD/SSD SMART 資訊（HDD/SSD SMART Information）..............55 7.17 APM 設定（APM Configuration）..............................................................56 7.18 內建裝置設定（OnBoard Devices Configuration）..............................57 7.19 Intel(R) 快速儲存技術（Intel(R) Rapid Storage Technology）.......59 8\. 監控選單（Monitor menu）.................................................................................................60 9\. 啟動選單（Boot menu）........................................................................................................67

Pro WS W680-ACE BIOS 使用手冊 3   
10\. 工具選單（Tool menu）.....................................................................................................73 10.1 華碩 User Profile ...............................................................................................74 10.2 華碩 SPD 資訊（ASUS SPD Information）............................................75 10.3 華碩 Armoury Crate ...........................................................................................75 

11\. IPMI 選單（IPMI menu）...................................................................................................76 11.1 系統事件記錄（System Event Log）.........................................................77 11.2 BMC 網路設定（BMC network configuration）....................................78 11.3 檢視系統事件記錄（View System Event Log）....................................80 12\. 離開 BIOS 程式（Exit menu）.........................................................................................81

4 Pro WS W680-ACE BIOS 使用手冊   
**BIOS** 程式設定 

1\. 認識 BIOS 程式

華碩全新的 UEFI BIOS 是可延伸韌體介面，符合最新的 UEFI 架構，這 個友善的使用介面，跳脫傳統使用鍵盤輸入 BIOS 方式，提供更有彈性 與更便利的滑鼠控制操作。您可以輕易地使用新的 UEFI BIOS，如同操 作您的作業系統般順暢。在本使用手冊中的「BIOS」一詞除非特別說 明，所指皆為「UEFI BIOS」。 

BIOS（Basic Input and Output System；基本輸出入系統）用來儲存系統開機時所 需要的硬體設定，例如儲存裝置設定、超頻設定、進階電源管理與開機設定等，這 些設定會儲存在主機板的 CMOS 中。在正常情況下，預設的 BIOS 程式設定提供大 多數使用情況下可以獲得最佳的運作效能。建議您不要變更預設的 BIOS 設定，除了 以下幾種狀況： 

• 在系統啟動期間，螢幕上出現錯誤訊息，並要求您執行 BIOS 程式設定。 • 安裝新的系統元件，需要進一步的 BIOS 設定或更新。 

不適當的 BIOS 設定可能會導致系統不穩定或開機失敗。強烈建議您只 有在受過訓練專業人士的協助下，才可以執行 BIOS 程式設定的變更。 

• 下載或更新 BIOS 檔案時，請將檔案名稱變更為 XXXXX.CAP 或是開 啟 BIOSRenamer.exe 應用程式以自動將檔案重新命名給本主機板使 用。請參考主機板隨附的使用手冊中的相關資訊以獲得檔案名稱。 CAP 檔案名稱會依型號而異，正確名稱請參考主機板使用手冊。 • 本章節畫面僅供參考，請以實際的 BIOS 選項為準。 

• BIOS 設定選項會因版本而異，請確認已更新至最新的 BIOS 版本。 

Pro WS W680-ACE BIOS 使用手冊 5   
2\. BIOS 程式設定 

使用 BIOS Setup（BIOS 設定）功能可以更新 BIOS 或設定其參數。BIOS 設定畫面 包含導覽鍵與簡要的畫面輔助說明，以指示您使用 BIOS 設定程式。 

當開機時進入 BIOS 設定程式： 

• 當進入開機自我測試（POST）過程時，按下  \<Delete\> 或 \<F2\> 鍵可以進入 BIOS 設定畫面。若您未按下 \<Delete\> 或 \<F2\> 鍵，則 開機自我測試（POST）功能會繼續進行。 

當 POST 結束後才進入 BIOS 設定程式： 

當 POST 結束後才進入 BIOS 設定程式： 

• 按下 \<Ctrl\>+\<Alt\>+\<Delete\> 鍵。 

• 或是按下機殼上的 RESET（重置）鍵重新開機。 

• 或是將按下機殼上的電源按鈕，將電腦關閉後再重新開機。如果前兩種方式無 效，再選用最後一種方式。 

然後再於開機自我測試（POST）過程時按下 \<Delete\> 鍵進入 BIOS 設定畫面。

• 在本章節的 BIOS 程式畫面僅供參考，將可能與您所見到的畫面有所 差異。 

• BIOS 程式的出廠預設值可讓系統運作處於最佳效能，但是若系統因 您改變 BIOS 程式而導致不穩定，請讀取出廠預設值來保持系統的 穩定。請選擇 Exit 選單中的 Load Optimized Defaults 項目或按下  \<F5\> 鍵。請參閱 12\. 離開 BIOS 程式 中的詳細說明。 

• 若是變更 BIOS 設定後開機失敗，請試著使用清除 CMOS，然後將主 機板的設定值回復為預設值。請參考主機板使用手冊中的相關說明 以了解 Clear CMOS 按鈕並清除 CMOS 即時時鐘（RTC）記憶體資 料。 

• BIOS 設定程式不支援藍牙裝置。 

6 Pro WS W680-ACE BIOS 使用手冊   
3\. 管理、更新您的 BIOS 程式 

以下的工具程式項目為提供您管理與更新主機板 BIOS 設定程式: 1\. 華碩 CrashFree BIOS 3 

當 BIOS 程式毀損時，使用可開機的 USB 隨身碟來更新 BIOS 程式。 2\. ASUS EzFlash 

使用 USB 隨身碟更新 BIOS。 

3\. BUPDATER 

使用可開機的 USB 隨身碟在 DOS 環境下更新 BIOS 程式。 

上述軟體請參考相關章節的詳細使用說明。 

建議您先將主機板原始的 BIOS 程式備份到可開機的 USB 隨身碟中，以 備您往後需要再度安裝原始的 BIOS 程式。使用 BUPDATER 程式來拷貝 主機板原始的 BIOS 程式。 

3.1 華碩 CrashFree BIOS 3 程式 

華碩最新自行研發的 CrashFree BIOS 3 工具程式，讓您在當 BIOS 程式和資料被 病毒入侵或毀損時，可以輕鬆的從含有最新或原始的 BIOS 檔案的 USB 隨身碟中回 復 BIOS 程式的資料。 

在執行更新 BIOS 程式之前，請準備存有 BIOS 檔案的USB 隨身碟。 

使用 USB 隨身碟回復 BIOS 程式 

請依照以下步驟，使用 USB 隨身碟回復 BIOS 程式。 

1\. 將儲存有原始或更新的 BIOS 程式檔案的 USB 隨身碟插入 USB 埠，並啟動系 統。 

2\. 程式會自動開始進行更新，並在完成後重新啟動系統。 

請勿在更新 BIOS 程式檔案時關閉或重新啟動系統！此舉將會導致系統 損毀！ 

在驅動及公用程式光碟中的 BIOS 程式檔案，也許並非為最新的 BIOS 檔 案，請至華碩網站（http://www.asus.com/tw）下載最新的 BIOS 版本檔 案。

Pro WS W680-ACE BIOS 使用手冊 7   
3.2 使用華碩 EzFlash 更新程式 

華碩 EzFlash 程式讓您能輕鬆的更新 BIOS 程式，可以不必再透過開機片的冗長程 序或是到 DOS 模式下執行。 

請至華碩網站 http://www.asus.com/tw/ 下載最新的 BIOS 程式檔案。 以下的 BIOS 畫面僅供參考，請依您所見的實際 BIOS 畫面為準。 

請依照下列步驟，使用 EzFlash 來更新 BIOS： 

1\. 將儲存有最新的 BIOS 檔案的 USB 隨身碟插入 USB 連接埠。 2\. 進入 BIOS 設定程式。來到 Tool 選單，選擇 Start EzFlash 後並按下\<Enter\> 鍵 將其開啟。 

**ASUSTek. EzFlash Utility** 

 **Current Platform**   
 **New Platform**   
**Platform : Pro-WS-W680-ACE-IPMI Platform : Pro-WS-W680-ACE-IPMI**   
**Version : 0101**   
**Build Date :09/14/2022**   
**Version : 0105**   
**Build Date :10/24/2022**

**FS0 Pro-WS-W680-ACE-IPMI\-ASUS-0104.cap 33558528 Bytes \[Up/Down/Left/Right\]:Switch \[Enter\]:Choose \[q\]:Exit** 

3\. 按左方向鍵來切換至 Drive 欄位。 

4\. 按上/下方向鍵來選擇儲存最新 BIOS 版本的 USB 隨身碟，然後按下 \<Enter\>  鍵。  

5\. 按右方向鍵來切換至 Folder Info 欄位。 

6\. 按上/下方向鍵來選擇 BIOS 檔案，並按下 \<Enter\> 鍵執行 BIOS 更新作業。  7\. 當 BIOS 更新作業完成後請重新啟動電腦。 

8 Pro WS W680-ACE BIOS 使用手冊   
4\. BIOS 選單畫面 

選單項目 功能表列 設定視窗 項目說明 ![][image1]  
4.1 功能表列說明 

BIOS 設定程式最上方各選單功能說明如下： 

Main 本項目提供系統基本設定。 Ai Tweaker 本項目提供超頻設定。 

Advanced 本項目提供系統進階功能設定。 Monitor 本項目提供溫度、電源及風扇功能設定。 Boot 本項目提供開機磁碟設定。 Tool 本項目提供特殊功能的設定。 IPMI 本項目提供 IPMI 設定。   
操作功能鍵 

Exit 本項目提供離開 BIOS 設定程式與出廠預設值還原功能。 使用左右方向鍵移動選項，可切換至另一個選單畫面。

Pro WS W680-ACE BIOS 使用手冊 9   
4.2 選單項目 

於功能表列選定選項時，被選擇的功能將會反白。假設您選擇 Main 功能，則 會顯示 Main 選單的項目。點選選單中的其他項目（如：Event Logs、Advanced、 Monitor、Boot、Tool 與 Exit 等）也會出現該項目不同的選項。 

4.3 子選單 

在選單畫面中，若功能選項前面有一個小三角形標記（\>），代表此為子選單，您 可利用方向鍵來選擇，並按下 \<Enter\> 鍵來進入子選單。 

![][image2]4.4 操作功能鍵說明 

在選單畫面的右下方為操作功能鍵說明，請參照功能鍵說明來選擇及改變各項功 能。 

4.5 一般說明 

在選單畫面的右上方為目前所選擇的作用選項的功能說明，此說明會依選項的不 同而自動變更。 

4.6 設定值 

此區域顯示選單項目的設定值。這些項目中，有的功能選項僅為告知使用者目前 執行狀態，並無法更改，此類項目就會以淡灰色顯示。而可更改的項目，當您使用 方向鍵移動項目時，被選擇的項目以反白顯示，代表這是可更改的項目。要改變設 定值請選擇此項目，並按下 \<Enter\> 鍵以顯示設定值列表。 

4.7 設定視窗 

在選單中請選擇功能項目，然後按下 \<Enter\> 鍵，程式將會顯示包含此功能所提供 的選項小視窗，您可以利用此視窗來設定您所想要的設定。 

4.8 捲軸 

在選單畫面的右方若出現捲軸，即代表此頁選項超過可顯示的畫面，您可利用上/ 下方向鍵或是 PageUp/PageDown 鍵來切換畫面。

10 Pro WS W680-ACE BIOS 使用手冊   
5\. 主選單（Main Menu） 

當進入 BIOS 設定程式的進階模式（Advanced Mode）時，首先出現的第一個畫面 即為主選單。主選單顯示系統資訊概要，用來設定系統日期、時間、語言與安全設 定。

![][image3]Pro WS W680-ACE BIOS 使用手冊 11   
安全性選單（Security） 

本選單可以讓您改變系統安全設定。

![][image4]  
• 若您忘記設定的 BIOS 密碼，可以採用清除 CMOS 即時鐘（RTC） 記憶體。請參考主機板使用手冊中 跳線選擇區 的說明。 

• Administrator 或 User Password 項目預設值為 \[Not Installed\]。當您 設定密碼之後將顯示為 \[Installed\]。 

Administrator Password（設定系統管理員密碼） 

當您設定系統管理員密碼後，建議您先登入您的帳戶，以免 BIOS 設定程式中的某 些資訊無法檢視或變更設定。 

請依照以下步驟設定系統管理員密碼（Administrator Password）： 1\. 請選擇 Administrator Password 項目並按下 \<Enter\>。   
2\. 由 Create New Password 視窗輸入欲設定的密碼，輸入完成時，請按下  \<Enter\>。 

3\. 請再一次輸入密碼並選擇 OK。 

請依照以下步驟變更系統管理員密碼（Administrator Password）： 1\. 請選擇 Administrator Password 項目並按下 \<Enter\>。   
2\. 由 Enter Current Password 視窗輸入密碼並按下 \<Enter\>。 

3\. 由 Create New Password 視窗輸入新密碼，輸入完成按下 \<Enter\>。 4\. 請再一次輸入密碼並選擇 OK。 

欲刪除系統管理員密碼時，請依照變更系統管理員密碼之步驟，但請在輸入/確認 密碼視窗出現時選擇 OK。當您刪除系統管理員密碼後，Administrator Password 項 目將顯示為 \[Not Installed\]。 

12 Pro WS W680-ACE BIOS 使用手冊   
User Password（設定使用者密碼） 

當您設定使用者密碼後，你必需登入您的帳戶才能使用 BIOS 設定程式。User  Password 項目預設值為 \[Not Installed\]。當您設定密碼之後將顯示為 \[Installed\]。 

請依照以下步驟設定使用者密碼（User Password）： 

1\. 請選擇 User Password 項目並按下 \<Enter\>。 

2\. 由 Create New Password 視窗輸入欲設定的密碼，輸入完成時，請按下  \<Enter\>。 

3\. 請再一次輸入密碼並選擇 OK。 

請依照以下步驟變更使用者密碼（User Password）： 

1\. 請選擇 User Password 項目並按下 \<Enter\>。 

2\. 由 Enter Current Password 視窗輸入密碼並按下 \<Enter\>。 

3\. 由 Create New Password 視窗輸入新密碼，輸入完成按下 \<Enter\>。 4\. 請再一次輸入密碼並選擇 OK。 

欲刪除使用者密碼時，請依照變更使用者密碼之步驟，但請在輸入/確認密碼視 窗出現時選擇 OK。當您刪除使用者密碼後，User Password 項目將顯示為 \[Not  Installed\]。

Pro WS W680-ACE BIOS 使用手冊 13   
6\. Ai Tweaker 選單（Ai Tweaker menu） 

本選單可讓您設定超頻功能的相關選項。 

注意！在您設定本進階選單的設定時，不正確的設定值將導致系統功能 異常。 

以下項目中的設定值，可能會隨安裝在主機板上的 CPU 與記憶體模組而 異。 

![][image5]將捲軸往下捲動來顯示以下項目。 Ai Overclock Tuner   
\[Auto\] 載入系統最佳化設定值。 

\[EXPO I\] 選擇此項目以使用記憶體模組預設的 EXPO I 記憶體時脈（CL、 TRCD、TRP、TRAS）以及經由華碩最佳化的其他記憶體參數設定。 \[EXPO II\] 選擇此項目以使用記憶體模組的預設 EXPO 檔。未偵測到其他設定 檔時，選擇此項目以載入華碩最佳化的記憶體參數設定。 

\[DOCP I\] 選擇此項目以使用記憶體模組預設的 DOCP I 記憶體時脈（CL、 TRCD、TRP、TRAS）以及經由華碩最佳化的其他記憶體參數設定。 \[DOCP II\] 選擇此項目以使用記憶體模組的預設 DOCP 檔。未偵測到其他設定 檔時，選擇此項目以載入華碩最佳化的記憶體參數設定。 

\[AEMP\] 未偵測到其他設定檔時，選擇此項目以載入華碩最佳化的記憶體參 數設定。 

此選單項目會依安裝的記憶體模組而異。

14 Pro WS W680-ACE BIOS 使用手冊   
以下項目只有在 Ai Overclock Tuner 設為 \[AEMP\] 時才會出現。 

AEMP 

本項目可以用來選擇 ASUS Enhanced Memory Profile（AEMP）。每個設定檔都有 專屬動態隨機存取記憶體（DRAM）頻率、時間與電壓。 

以下項目只有在 Ai Overclock Tuner 設為 \[EXPO I\] 或 \[EXPO II\] 時才會 出現。 

EXPO 

本項目用來選擇 EXPO 設定檔。每個設定檔都有專屬動態隨機存取記憶體 （DRAM）頻率、時間與電壓。 

以下項目只有在 Ai Overclock Tuner 設為 \[DOCP I\] 或 \[DOCP II\] 時才會 出現。 

D.O.C.P. 

本項目可以選擇 D.O.C.P.設定檔。每個設定檔都有專屬動態隨機存取記憶體 （DRAM）頻率、時間與電壓。 

ASUS Performance Enhancement 3.0 

\[Disabled\] 本項目可以使用 Intel 預設的 CPU 設定 （Intel 庫存的 CPU 核心頻率與功率限 

制）。 

\[Enabled\] 本項目可以開啟 ASUS 優化的 CPU 設定 （解鎖功率限制以提高 CPU 效能）。 

\[Enabled(limit CPU temp. at 90°C)\] 本項目可以開啟 ASUS 優化的 CPU 設 定（解鎖功率限制）並限制 CPU 溫度為  

90°C 以提升效能。 

實際設定值會因型號而異。 

BCLK FrequencyDRAM Frequency Ratio 

\[Auto\] 自動最佳化 BCLK 頻率與 DRAM 頻率。 

\[100:133\] 本項目將 BCLK 頻率與 DRAM 頻率的比值設為 100:133。 \[100:100\] 本項目將 BCLK 頻率與 DRAM 頻率的比值設為 100:100。 

Memory Controller : DRAM Frequency Ratio 

BCLK Frequency: DRAM Frequency Ratio 選擇 100:133 時有較好的超頻性能，同時  1:2 Memory Controller: DRAM Frequency Ratio 僅在 DRAM 比值為偶數時運作。設定 值有：\[Auto\] \[1:1\] \[1:2\] \[1:4\]

Pro WS W680-ACE BIOS 使用手冊 15   
DRAM Frequency 

本項目可設定記憶體的運作頻率。設定選項會隨著 BCLK Frequency 設定值變 動。選擇自動模式以套用最佳化設定。設定值有：\[Auto\] \[DDR5-800MHz\] \- \[DDR5- 13333MHz\] 

以下項目中的設定值，可能會隨安裝在主機板上的 CPU 與記憶體模組而 異。 

呈灰色的設定值不建議使用，請使用呈白色的設定值。 

Performance Core Ratio 

\[Auto\] 系統將自動調整所有核心比率。 

\[Sync All Cores\] 設定核心比率限制以同步所有核心。 

\[By Core Usage\] 依據正在使用的核心數量配置活動核心的比率限制。 \[AI Optimized\] 使用動態機器學習算法使活動核心比率最佳化。 

• \[AI Optimized\] 項目只有在安裝沒有鎖頻的處理器時才會顯示。 • 以下項目只有在 Performance Core Ratio 設為 \[Sync All Cores\] 時才 會出現。 

ALL-Core Ratio Limit 

選擇 \[Auto\] 以套用 CPU 預設的 Turbo 倍頻設定或手動指定 Core Ratio Limit 數 值。請使用 \<+\> 與 \<-\> 鍵調整數值。設定值有：\[Auto\] \[8\] \- \[45\] 

以下項目只有在 Performance Core Ratio 設為 \[By Core Usage\] 時才會出 現。 

1-Core Ratio Limit / 2-Core Ratio Limit / 3-Core Ratio Limit / 4-Core Ratio  Limit / 5-Core Ratio Limit / 6-Core Ratio Limit / 7-Core Ratio Limit / 8-Core  Ratio Limit 

N-core 比率限制需高於或等於（N+1）-core 比率限制。（N 代表 CPU 核心數量） 當核心數量低於 N 時，核心比率限制無法設定為 \[Auto\] 。最大核心比率限制需低於 或等於第二大核心比率限制。請使用 \<+\> 與 \<-\> 鍵調整數值。設定值有：\[Auto\] \[21\]  \- \[49\] 

以下項目只有在 Performance Core Ratio 設為 \[AI Optimized\] 時才會出 現。 

Optimized AVX Frequency 

標準用例選擇 \[Normal Use\]，或是極端負載如 Prime 95 AVX 時選擇 \[Heavy  AVX\]。設定值有：\[Normal Use\] \[Heavy AVX\]

16 Pro WS W680-ACE BIOS 使用手冊   
Efficient Core Ratio 

\[Auto\] 系統將自動調整所有效率核心比率。 

\[Sync All Cores\] 設定核心比率限制以同步所有效率核心。 

\[By Core Usage\] 依據正在使用的效率核心數量配置活動核心的比率限制。 \[AI Optimized\] 使用動態機器學習算法使效率核心比率最佳化。 

• \[AI Optimized\] 項目只有在安裝沒有鎖頻的處理器時才會顯示。 • 以下項目只有在 Efficient Core Ratio 設為 \[Sync All Cores\] 時才會 出現。 

ALL-Core Ratio Limit 

本項目可以設定效率核心的 Ratio Limit。請使用 \<+\> 與 \<-\> 鍵調整數值。設定值 有：\[Auto\] \[8\] \- \[34\] 

以下項目只有在 Performance Core Ratio 設為 \[By Core Usage\] 時才會出 現。 

Efficient 1-Core Ratio Limit / Efficient 2-Core Ratio Limit / Efficient 3-Core  Ratio Limit / Efficient 4-Core Ratio Limit 

設定值有：\[Auto\] \[16\] \- \[36\] 

AVX Related Controls 

AVX2 

啟用或關閉 AVX 2 控制器。設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

DRAM Timing Control 

本項目用來管理與設定 DRAM 電力。請使用 \<+\> 與 \<-\> 鍵調整數值。當您要回復 預設值時，請使用鍵盤輸入 \[Auto\] 並按下 \<Enter\> 鍵。您可以選擇 Memory Presets 以載入適合用於某些記憶體模組的設定值。 

自行更改數值將會導致系統的不穩定與硬體損毀，當系統出現不穩定的 狀況時，建議您使用預設值。 

Primary Timings 

DRAM CAS\# Latency 

設定值有：\[Auto\] \[2\] \- \[126\] 

DRAM RAS\# to CAS\# Delay 

設定值有：\[Auto\] \[0\] \- \[255\] 

DRAM RAS\# PRE Time 

設定值有：\[Auto\] \[0\] \- \[255\] 

DRAM RAS\# ACT Time 

設定值有：\[Auto\] \[1\] \- \[511\]

Pro WS W680-ACE BIOS 使用手冊 17   
DRAM Command Rate 

設定值有：\[Auto\] \[1N\] \[2N\] \[3N\] \[N:1\] 

以下項目只有在 DRAM Command Rate 設為 \[N:1\] 時才會出現。 

N to 1 ratio 

每個有效命令周期之間的數值。設定值有：\[1\] \- \[7\] 

Secondary Timings 

DRAM RAS\# to RAS\# Delay L 

設定值有：\[Auto\] \[1\] \- \[63\] 

DRAM RAS\# to RAS\# Delay S 

設定值有：\[Auto\] \[1\] \- \[127\] 

DRAM REF Cycle Time 

設定值有：\[Auto\] \[1\] \- \[65535\] 

DRAM REF Cycle Time 2 

設定值有：\[Auto\] \[1\] \- \[65535\] 

DRAM REF Cycle Time Same Bank 

設定值有：\[Auto\] \[0\] \- \[2047\] 

DRAM Refresh Interval 

設定值有：\[Auto\] \[1\] \- \[262143\] 

DRAM WRITE Recovery Time 

設定值有：\[Auto\] \[1\] \- \[234\] 

DRAM READ to PRE Time 

設定值有：\[Auto\] \[1\] \- \[255\] 

DRAM FOUR ACT WIN Time 

設定值有：\[Auto\] \[1\] \- \[511\] 

DRAM WRITE to READ Delay 

設定值有：\[Auto\] \[1\] \- \[31\] 

DRAM WRITE to READ Delay L 

設定值有：\[Auto\] \[1\] \- \[31\] 

DRAM WRITE to READ Delay S 

設定值有：\[Auto\] \[1\] \- \[31\] 

DRAM CKE Minimum Pulse Width 

設定值有：\[Auto\] \[0\] \- \[127\] 

DRAM Write Latency 

設定值有：\[Auto\] \[1\] \- \[255\] 

Skew Control 

DDRCRCOMPCTL0/1/2 

Ctl0 dqvrefup 

設定值有：\[Auto\] \[0\] \- \[255\]

18 Pro WS W680-ACE BIOS 使用手冊   
Ctl0 dqvrefdn   
設定值有：\[Auto\] \[0\] \- \[255\] 

Ctl0 dqodtvrefup   
設定值有：\[Auto\] \[0\] \- \[255\] 

Ctl0 dqodtvrefdn   
設定值有：\[Auto\] \[0\] \- \[255\] 

Ctl1 cmdvrefup   
設定值有：\[Auto\] \[0\] \- \[255\] 

Ctl1 ctlvrefup   
設定值有：\[Auto\] \[0\] \- \[255\] 

Ctl1 clkvrefup   
設定值有：\[Auto\] \[0\] \- \[255\] 

Ctl1 ckecsvrefup   
設定值有：\[Auto\] \[0\] \- \[255\] 

Ctl2 cmdvrefdn   
設定值有：\[Auto\] \[0\] \- \[255\] 

Ctl2 ctlvrefdn   
設定值有：\[Auto\] \[0\] \- \[255\] 

Ctl2 clkvrefdn   
設定值有：\[Auto\] \[0\] \- \[255\] 

Tc Odt Control 

ODT\_READ\_DURATION   
設定值有：\[Auto\] \[0\] \- \[15\] 

ODT\_READ\_DELAY   
設定值有：\[Auto\] \[0\] \- \[15\] 

ODT\_WRITE\_DURATION   
設定值有：\[Auto\] \[0\] \- \[15\] 

ODT\_WRITE\_DELAY   
設定值有：\[Auto\] \[0\] \- \[15\] 

MC0 Dimm0 / MC0 Dimm1 / MC1 Dimm0 / MC1 Dimm1 

DQ RTT WR 

設定值有：\[Auto\] \[0 DRAM Clock\] \[34 DRAM Clock\] \[40 DRAM Clock\] \[48  DRAM Clock\] \[60 DRAM Clock\] \[80 DRAM Clock\] \[120 DRAM Clock\] \[240  DRAM Clock\] 

DQ RTT NOM RD 

設定值有：\[Auto\] \[0 DRAM Clock\] \[34 DRAM Clock\] \[40 DRAM Clock\] \[48  DRAM Clock\] \[60 DRAM Clock\] \[80 DRAM Clock\] \[120 DRAM Clock\] \[240  DRAM Clock\] 

DQ RTT NOM WR 

設定值有：\[Auto\] \[0 DRAM Clock\] \[34 DRAM Clock\] \[40 DRAM Clock\] \[48  DRAM Clock\] \[60 DRAM Clock\] \[80 DRAM Clock\] \[120 DRAM Clock\] \[240  DRAM Clock\] 

DQ RTT PARK 

設定值有：\[Auto\] \[0 DRAM Clock\] \[34 DRAM Clock\] \[40 DRAM Clock\] \[48  DRAM Clock\] \[60 DRAM Clock\] \[80 DRAM Clock\] \[120 DRAM Clock\] \[240  DRAM Clock\]

Pro WS W680-ACE BIOS 使用手冊 19   
DQ RTT PARK DQS 

設定值有：\[Auto\] \[0 DRAM Clock\] \[34 DRAM Clock\] \[40 DRAM Clock\] \[48  DRAM Clock\] \[60 DRAM Clock\] \[80 DRAM Clock\] \[120 DRAM Clock\] \[240  DRAM Clock\] 

GroupA CA ODT 

設定值有：\[Auto\] \[0 DRAM Clock\] \[40 DRAM Clock\] \[60 DRAM Clock\] \[80  DRAM Clock\] \[120 DRAM Clock\] \[240 DRAM Clock\] \[480 DRAM Clock\] 

GroupA CS ODT 

設定值有：\[Auto\] \[0 DRAM Clock\] \[40 DRAM Clock\] \[60 DRAM Clock\] \[80  DRAM Clock\] \[120 DRAM Clock\] \[240 DRAM Clock\] \[480 DRAM Clock\] 

GroupA CK ODT 

設定值有：\[Auto\] \[0 DRAM Clock\] \[40 DRAM Clock\] \[60 DRAM Clock\] \[80  DRAM Clock\] \[120 DRAM Clock\] \[240 DRAM Clock\] \[480 DRAM Clock\] 

GroupB CA ODT 

設定值有：\[Auto\] \[0 DRAM Clock\] \[40 DRAM Clock\] \[60 DRAM Clock\] \[80  DRAM Clock\] \[120 DRAM Clock\] \[240 DRAM Clock\] \[480 DRAM Clock\] 

GroupB CS ODT 

設定值有：\[Auto\] \[0 DRAM Clock\] \[40 DRAM Clock\] \[60 DRAM Clock\] \[80  DRAM Clock\] \[120 DRAM Clock\] \[240 DRAM Clock\] \[480 DRAM Clock\] 

GroupB CK ODT 

設定值有：\[Auto\] \[0 DRAM Clock\] \[40 DRAM Clock\] \[60 DRAM Clock\] \[80  DRAM Clock\] \[120 DRAM Clock\] \[240 DRAM Clock\] \[480 DRAM Clock\] 

Pull-up Output Driver Impedance 

設定值有：\[Auto\] \[34 DRAM Clock\] \[40 DRAM Clock\] \[48 DRAM Clock\] Pull-Down Output Driver Impedance   
設定值有：\[Auto\] \[34 DRAM Clock\] \[40 DRAM Clock\] \[48 DRAM Clock\] RTL IOL Control   
Round Trip Latency Init Value MC0-1 CHA-B 

設定值有：\[Auto\] \[0\] \- \[255\] 

Round Trip Latency Max Value MC0-1 CHA-B 

設定值有：\[Auto\] \[0\] \- \[255\] 

Round Trip Latency Offset Value Mode Sign MC0-1 CHA-B 

設定值有：\[-\] \[+\] 

Round Trip Latency Offset Value MC0-1 CHA-B 

設定值有：\[Auto\] \[0\] \- \[255\] 

Round Trip Latency MC0-1 CHA-B R0-7 

設定值有：\[Auto\] \[0\] \- \[255\] 

Memory Training Algorithms 

本選單的項目用來讓您開啟或關閉不同的記憶體訓練演算法。 Early Command Training   
設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

SenseAmp Offset Training 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Early ReadMPR Timing Centering 2D 

設定值有：\[Auto\] \[Disabled\] \[Enabled\]

20 Pro WS W680-ACE BIOS 使用手冊   
Read MPR Training 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Receive Enable Training 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Jedec Write Leveling 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Early Write Timing Centering 2D 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Early Read Timing Centering 2D 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Write Timing Centering 1D 

設定值有：\[Disabled\] \[Enabled\] 

Write Voltage Centering 1D 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Read Timing Centering 1D 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Read Timing Centering with JR 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Dimm ODT Training\* 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Max RTT\_WR 

在電力訓練時可限制 RTT\_WR 的最大值。設定值有：\[ODT OFF\] \[120  Ohms\] 

DIMM RON Training\* 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Write Drive Strength/Equalization 2D\* 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Write Slew Rate Training\* 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Read ODT Training\* 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Comp Optimization Training 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Read Equalization Training\* 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Read Amplifier Training\* 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Write Timing Centering 2D 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Read Timing Centering 2D 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Command Voltage Centering 

設定值有：\[Auto\] \[Disabled\] \[Enabled\]

Pro WS W680-ACE BIOS 使用手冊 21   
Early Command Voltage Centering 設定值有：\[Auto\] \[Disabled\] \[Enabled\] Write Voltage Centering 2D   
設定值有：\[Auto\] \[Disabled\] \[Enabled\] Read Voltage Centering 2D   
設定值有：\[Auto\] \[Disabled\] \[Enabled\] Late Command Training   
設定值有：\[Disabled\] \[Enabled\] \[Auto\] Round Trip Latency   
設定值有：\[Auto\] \[Disabled\] \[Enabled\] Turn Around Timing Training   
設定值有：\[Auto\] \[Disabled\] \[Enabled\] CMD CTL CLK Slew Rate   
設定值有：\[Auto\] \[Disabled\] \[Enabled\] CMD/CTL DS & E 2D   
設定值有：\[Auto\] \[Disabled\] \[Enabled\] Read Voltage Centering 1D   
設定值有：\[Auto\] \[Disabled\] \[Enabled\] TxDqTCO Comp Training\*   
設定值有：\[Auto\] \[Disabled\] \[Enabled\] ClkTCO Comp Training\*   
設定值有：\[Auto\] \[Disabled\] \[Enabled\] TxDqsTCO Comp Training\*   
設定值有：\[Auto\] \[Disabled\] \[Enabled\] VccDLL Bypass Training\*   
設定值有：\[Auto\] \[Disabled\] \[Enabled\] CMD/CTL Drive Strength Up/Dn 2D 設定值有：\[Auto\] \[Disabled\] \[Enabled\] DIMM CA ODT Training   
設定值有：\[Auto\] \[Disabled\] \[Enabled\] PanicVttDnLp Training\*   
設定值有：\[Auto\] \[Disabled\] \[Enabled\] Read Vref Decap Traning\*   
設定值有：\[Auto\] \[Disabled\] \[Enabled\] Vddq Training   
設定值有：\[Auto\] \[Disabled\] \[Enabled\] Duty Cycle Correction Training 設定值有：\[Auto\] \[Disabled\] \[Enabled\] Rank Margin Tool Per Bit   
設定值有：\[Auto\] \[Disabled\] \[Enabled\] DIMM DFE Training   
設定值有：\[Auto\] \[Disabled\] \[Enabled\]

22 Pro WS W680-ACE BIOS 使用手冊   
EARLY DIMM DFE Training 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Tx Dqs Dcc Training 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

DRAM DCA Training 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Write Driver Strength Training 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Rank Margin Tool 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Memory Test 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

DIMM SPD Alias Test 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Receive Enable Centering 1D 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Retrain Margin Check 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Write Drive Strength Up/Dn independently 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Margin Check Limit 

本項目可以檢視開機記憶體是否需要重設。 

設定值有：\[Disabled\] \[L1\] \[L2\] \[Both\] 

以下項目只有在 Margin Check Limit 設為 \[L2\] 或 \[Both\] 時才會出現。 

Margin Limit Check L2 

設定值有：\[1\] \- \[300\] 

Third Timings 

tRDRD\_sg\_Training 

設定值有：\[Auto\] \[0\] \- \[127\] 

tRDRD\_sg\_Runtime 

設定值有：\[Auto\] \[0\] \- \[127\] 

tRDRD\_dg\_Training 

設定值有：\[Auto\] \[0\] \- \[127\] 

tRDRD\_dg\_Runtime 

設定值有：\[Auto\] \[0\] \- \[127\] 

tRDWR\_sg 

設定值有：\[Auto\] \[0\] \- \[255\] 

tRDWR\_dg 

設定值有：\[Auto\] \[0\] \- \[255\]

Pro WS W680-ACE BIOS 使用手冊 23   
tWRWR\_sg 

設定值有：\[Auto\] \[0\] \- \[127\] tWRWR\_dg   
設定值有：\[Auto\] \[0\] \- \[127\] tWRRD\_sg   
設定值有：\[Auto\] \[0\] \- \[511\] tWRRD\_dg   
設定值有：\[Auto\] \[0\] \- \[511\] tRDRD\_dr   
設定值有：\[Auto\] \[0\] \- \[255\] tRDRD\_dd   
設定值有：\[Auto\] \[0\] \- \[255\] tRDWR\_dr   
設定值有：\[Auto\] \[0\] \- \[255\] tRDWR\_dd   
設定值有：\[Auto\] \[0\] \- \[255\] tWRWR\_dr   
設定值有：\[Auto\] \[0\] \- \[127\] tWRWR\_dd   
設定值有：\[Auto\] \[0\] \- \[255\] tWRRD\_dr   
設定值有：\[Auto\] \[0\] \- \[127\] tWRRD\_dd   
設定值有：\[Auto\] \[0\] \- \[127\] tRPRE   
設定值有：\[Auto\] \[0\] \- \[4\] tWPRE   
設定值有：\[Auto\] \[0\] \- \[4\] tWRPRE   
設定值有：\[Auto\] \[0\] \- \[1023\] tPRPDEN   
設定值有：\[Auto\] \[0\] \- \[31\] tRDPDEN   
設定值有：\[Auto\] \[0\] \- \[255\] tWRPDEN   
設定值有：\[Auto\] \[0\] \- \[1023\] tCPDED   
設定值有：\[Auto\] \[0\] \- \[31\]

24 Pro WS W680-ACE BIOS 使用手冊   
tREFIX9 

設定值有：\[Auto\] \[0\] \- \[255\] 

Ref Interval 

設定值有：\[Auto\] \[0\] \- \[8191\] 

tXPDLL 

設定值有：\[Auto\] \[0\] \- \[127\] 

tXP 

設定值有：\[Auto\] \[0\] \- \[127\] 

tPPD 

設定值有：\[Auto\] \[0\] \- \[15\] 

tCCD\_L\_tDLLK 

設定值有：\[Auto\] \[0\] \- \[15\] 

Misc. 

MRC Fast Boot 

啟用或關閉 MRC 控制器。設定值有：\[Disabled\] \[Enabled\] 

MCH Full Check 

本項目用來增強 DRAM 超頻能力或降低由 BCLK 產生的 EMI 電磁波干擾。設 定為 \[Enabled\] 可以降低 EMI 干擾，設定為 \[Disabled\] 則可以增強 BCLK 超頻能 力。設定值有：\[Auto\] \[Enabled\] \[Disabled\] 

Mem Over Clock Fail Count 

設定值有：\[Auto\] \[1\] \- \[254\] 

Training Profile 

本項目用來選擇 DIMM training 資料。設定值有：\[Auto\] \[Standard Profile\]  \[ASUS User Profile\] 

RxDfe 

本項目可以設定 SOC Rx 上的 DFE。設定值有：\[Auto\] \[Enabled\] \[Disabled\] Mrc Training Loop Count   
本項目用來設定循環的指數以執行測試。設定值有：\[Auto\] \[0\] \- \[32\] DRAM CLK Period   
本項目用來設定動態隨機存取記憶體的時間週期。設定值有：\[Auto\] \[0\] \-  \[161\] 

Dll\_bwsel 

可嘗試 OC 範圍為 22+。設定值有：\[Auto\] \[0\] \- \[63\] 

Controller 0, Channel 0 Control 

本項目用來開啟或關閉 Controller 0 與 Channel 0。設定值有：\[Enabled\]  \[Disabled\]

Pro WS W680-ACE BIOS 使用手冊 25   
Controller 0, Channel 1 Control 

本項目用來開啟或關閉 Controller 0 與 Channel 1。設定值有：\[Enabled\]  \[Disabled\] 

Controller 1, Channel 0 Control 

本項目用來開啟或關閉 Controller 1 與 Channel 0。設定值有：\[Enabled\]  \[Disabled\] 

Controller 1, Channel 1 Control 

本項目用來開啟或關閉 Controller 1 與 Channel 1。設定值有：\[Enabled\]  \[Disabled\] 

MC\_Vref0-2 

設定值有：\[Auto\] \[0\] \- \[65533\] 

Fine Granularity Refresh mode 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

DRAM SPD Configuration 

SDRAM Density Per Die 

設定值有：\[Auto\] \[4 Gb\] \[8 Gb\] \[12 Gb\] \[16 Gb\] \[24 Gb\] \[32 Gb\] \[48 Gb\]  \[64 Gb\] 

SDRAM Banks Per Bank Group 

設定值有：\[Auto\] \[1 bank per bank group\] \[2 bank per bank group\] \[4 bank  per bank group\] 

SDRAM Bank Groups 

設定值有：\[Auto\] \[1 bank group\] \[2 bank groups\] \[4 bank groups\] \[8 bank  groups\] 

Configure Memory Dynamic Frequency Switching 

以下項目只有在 Realtime Memory Frequency 設為 \[Disabled\] 時才會出 現。 

Dynamic Memory Boost 

本項目用來開啟或關閉 Dynamic Memory Boost。允許自動切換預設 SPD  Profile 頻率與選擇的 XMP Profile 頻率。僅選擇 XMP Profile 才有效。設定 值有：\[Disabled\] \[Enabled\] 

以下項目只有在 Dynamic Memory Boost 設為 \[Disabled\] 時才會出現。 

Realtime Memory Frequency 

本項目可以啟用或關閉記憶體頻率功能。允許執行時手動切換預設 SPD  Profile 頻率與選擇的 XMP Profile 頻率。僅選擇 XMP Profile 才有效。設定 值有：\[Disabled\] \[Enabled\] 

SA GV 

系統代理程式。本項目可以關閉、調整至特定點或啟用頻率切換。啟用時 建議將選項保留至停放值以獲得最佳相容性。啟用此功能需要更長的開機時 間。設定值有：\[Disabled\] \[Enabled\] \[Fixed to 1st Point\] \[Fixed to 2nd Point\]  \[Fixed to 3rd Point\] \[Fixed to 4th Point\]

26 Pro WS W680-ACE BIOS 使用手冊   
以下項目只有在 SA GV 設為 \[Enabled\]、\[Fixed to 1st Point\]、\[Fixed to  2nd Point\]、 \[Fixed to 3rd Point\] 或 \[Fixed to 4th Point\] 時才會出現。 

First Point Frequency 

本項目可以指定頻率。0-MRC 自動，或是指定為整數的特定頻率： 2000Mhz。設定值有：\[0\] \- \[65535\] 

First Point Gear 

本項目可以設定 SAGV 點的齒輪比。0-Auto、1-G1、2-G2 與 4-G4。設定 值有：\[0\] \- \[4\] 

Second Point Frequency 

本項目可以指定頻率。0-MRC 自動，或是指定為整數的特定頻率： 2000Mhz。設定值有：\[0\] \- \[65535\] 

Second Point Gear 

本項目可以設定 SAGV 點的齒輪比。0-Auto、1-G1、2-G2 與 4-G4。設定 值有：\[0\] \- \[4\] 

Third Point Frequency 

本項目可以指定頻率。0-MRC 自動，或是指定為整數的特定頻率： 2000Mhz。設定值有：\[0\] \- \[65535\] 

Third Point Gear 

本項目可以設定 SAGV 點的齒輪比。0-Auto、1-G1、2-G2 與 4-G4。設定 值有：\[0\] \- \[4\] 

Fourth Point Gear 請在主選單裡設定。 

Digi+ VRM 

VRM Intialization Check 

若本功能啟用時，當 VRM 初始化發生錯誤，系統會顯示 POST 代碼 76/77。 設定值有：\[Disabled\] \[Enabled\] 

CPU Input Voltage Load-line Calibration 

設定值有：\[Auto\] \[Level 1\] \[Level 2\] \[Level 3\] 

CPU Load-line Calibration 

Load-line 是根據 Intel 所訂立之 VRM 規格，其設定值將影響繪圖處理器電 壓。CPU 運作電壓將依 CPU 的負載呈比例性遞減，當您將此項目的設定值 設定越高時，將可提高電壓值與超頻能力，但會增加 CPU 及 VRM 的溫度。 從 1-7 中選擇一個等級來調整負載線斜率。等級 1 代表更大的 VDroop，等 級 7 代表最小 VDroop。設定值有：\[Auto\] \[Level 1\] \[Level 2\] \[Level 3\] \[Level  4:Recommended for OC\] \[Level 5\] \[Level 6\] \[Level 7\] 

實際提升的效能視 CPU 型號而異。 

請勿將散熱系統移除，散熱環境需受到監控。

Pro WS W680-ACE BIOS 使用手冊 27   
Synch ACDC Loadline with VRM Loadline 

開啟本項目來自動調整 VRM 負載線以匹配 AC/DC 負載線。設定值有： \[Disabled\] \[Enabled\] 

CPU Current Capability 

本項目用來設定較高的數值提供更大的總電力範圍，同時擴展超頻頻率的範 圍。當系統超頻，或是 CPU 在較高負載需要獲得額外的電力支援時，請選擇較 高的數值。設定值有： \[Auto\] \[100%\] \[110%\] \[120%\] 

當 CPU 超頻或是需負載額外的電力時，請設置較高的數值。 

CPU VRM Switching Frequency 

本項目用來設定 VRM 開關頻率。VRM 開關頻率影響瞬態回應與 VRM 元件溫 度。選擇 \[Manual\] 設定較高的頻率可以獲得較快的暫態響應速度。當處理器運 作於高電壓與高負載線校準值時，建議使用 VRM 散熱器主動冷卻。設定值有： \[Auto\] \[Manual\] 

請勿將散熱系統移除，散熱環境需受到監控。 

以下項目只有在 CPU VRM Switching Frequency 設為 \[Auto\] 時才會出 現。 

VRM Spread Spectrum 

本項目可讓您啟動 VRM Spread Spectrum 項目以增加系統穩定性。啟用本項目 可減少噪音峰值。超頻時請關閉本項目。設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

以下項目只有在 CPU VRM Switching Frequency 設為 \[Manual\] 時才會 出現。 

Fixed CPU VRM Switching Frequency(KHz) 

本項目可讓您設定固定的 VRM 頻率。數值以 50kHz 為間隔，變更的範圍由  250kHz 至 500kHz。 

CPU Power Duty Control 

本項目用來調整每個元件相數的電流與散熱環境。 

\[Auto\] 設為預設值。 

\[T. Probe\] 設定降壓控制器以平衡 VRM FET 溫度。 

\[Extreme\] 維持各相電流平衡。 

 當本項目設定為 \[Extreme\] 時請勿將散熱系統移除，散熱環境需受到監 控。

28 Pro WS W680-ACE BIOS 使用手冊   
CPU Power Phase Control 

本項目提供 CPU 電源相數控制設定。 

\[Auto\] 系統自動選擇。 

\[Standard\] 由 CPU 選擇。 

\[Extreme\] 全相數模式。 

 當本項目設定為 \[Extreme\] 時請勿將散熱系統移除，散熱環境需受到監 控。 

以下項目只有使用內建顯示卡時才會出現。 

CPU Graphics Load-line Calibration 

Load-line 是根據 Intel 所訂立之 VRM 規格，其設定值將影響繪圖處理器電 壓。CPU 顯示卡運作電壓將依 CPU 顯示卡的負載呈比例性遞減。繪圖處理器運 行電壓將依繪圖處理器的負載呈比例性遞減。從 Level 1 至 Level 7 中選擇，以 將處理器圖形電源電壓從 100％ 調整至 0%。設定值有：\[Auto\] \[Level 1\] \[Level  

2\] \[Level 3\] \[Level 4:Recommended for OC\] \[Level 5\] \[Level 6\] \[Level 7\] 實際提升的效能將視 CPU 型號而異。請勿將散熱系統移除， 

CPU Graphics VRM Switching Frequency 

本項目讓您設定更高的頻率以獲得較快的暫態響應速度。選擇 \[Manual\] 設定 較高的頻率可以獲得較快的暫態響應速度。設定值有：\[Auto\] \[Manual\] 

當本項目設為 \[Manual\] 時請勿將散熱系統移除，散熱環境需受到監 控。 

以下項目只有在 CPU Graphics VRM Switching Frequency 設為 \[Manual\] 時才會出現。 

Fixed CPU Graphics Switching Frequency(KHz) 

本項目讓您設定更高的頻率以獲得較快的暫態響應速度。請使用 \<+\> 與 \<-\>  鍵調整數值。數值以 50KHz 為間隔，變更的範圍由 250KHz 至 500KHz。 

Boot Voltages 

CPU Core/Cache Boot Voltage 

本項目讓您在首次啟動時設定 CPU 電壓。請使用 \<+\> 與 \<-\> 鍵調整數值。 數值以 0.005V 為間隔，變更的範圍由 0.600V 至 1.700V。設定值有：\[Auto\]  \[0.60000\] \- \[1.70000\]

Pro WS W680-ACE BIOS 使用手冊 29   
CPU Input Boot Voltage 

本項目讓您在首次啟動時設定 CPU 輸入電壓。請使用 \<+\> 與 \<-\> 鍵調整數 值。數值以 0.010V 為間隔，變更的範圍由 1.500V 至 2.100V。設定值有： \[Auto\] \[1.50000\] \- \[2.10000\] 

PLL Input Boot Voltage 

本項目讓您在首次啟動時設定 PLL 輸入電壓。請使用 \<+\> 與 \<-\> 鍵調整數 值。數值以 0.010V 為間隔，變更的範圍由 0.800V 至 1.800V。設定值有： \[Auto\] \[0.80000\] \- \[1.80000\] 

CPU Standby Boot Voltage 

本項目讓您在首次啟動時設定 CPU 待機電壓。請使用 \<+\> 與 \<-\> 鍵調整數 值。數值以 0.010V 為間隔，變更的範圍由 0.800V 至 1.800V。設定值有： \[Auto\] \[0.80000\] \- \[1.80000\] 

Memory Controller Boot Voltage 

本項目讓您在首次啟動時設定記憶體控制器電壓。請使用 \<+\> 與 \<-\> 鍵調整 數值。數值以 0.00625V 為間隔，變更的範圍由 1.000V 至 2.000V。設定值有： \[Auto\] \[1.00000\] \- \[2.00000\] 

Auto Voltage Caps 

CPU Core Auto Voltage Cap 

將此設定為特定值將為 CPU 核心自動電壓設定上限。未處於手動模式時，其 有效性受其他因素影響，如 AC/DC 負載線值與 CPU 的本機 VID。請使用 \<+\> 與  \<-\> 鍵調整數值。數值以 0.005V 為間隔，變更的範圍由 0.600V 至 1.700V。設 定值有：\[Auto\] \[0.60000\] \- \[1.70000\] 

CPU Input Auto Voltage Cap 

將此設定為特定值為 CPU 輸入自動電壓設定上限。請使用 \<+\> 與 \<-\> 鍵調整 數值。數值以 0.010V 為間隔，變更的範圍由 1.500V 至 2.100V。 設定值有： \[Auto\] \[1.50000\] \- \[2.10000\] 

Memory Controller Auto Voltage Cap 

將此設定為特定值為記憶體控制器自動電壓設定上限。請使用 \<+\> 與 \<-\> 鍵 調整數值。數值以 0.00625V 為間隔，變更的範圍由 1.000V 至 2.000V。設定值 有：\[Auto\] \[1.00000\] \- \[2.00000\] 

Internal CPU Power Management 

本項目用來管理與設定 CPU 電力。 

Tcc Activation Offset 

與預設值的 TCC 啟動溫度的偏移量，在該溫度下須啟動熱控制電路。Tcc 將 於以下啟動：TCC Activation Temp（TCC 啟動溫度）- Tcc Activation Offset （TCC 啟動偏移量）。請使用 \<+\> 與 \<-\> 鍵調整數值。設定值有：\[Auto\] \[0\] \-  \[63\] 

IVR Transmitter VDDQ ICCMAX 

設定值有：\[Auto\] \[0\] \- \[15\]

30 Pro WS W680-ACE BIOS 使用手冊   
CPU Core/Cache Current Limit Max.  

本項目可讓您設定更高的電流限制以防止超頻時的頻率或功率節流。可以設定 為最大值（511.75）以防止超頻時節流。請使用 \<+\> 與 \<-\> 鍵調整數值。設定值 有：\[Auto\] \[0.00\] \- \[511.75\] 

CPU Graphics Current Limit 

本項目可讓您設定更高的電流限制以防止超頻時的頻率或功率節流。請使用  \<+\> 與 \<-\> 鍵調整數值。設定值有：\[Auto\] \[0.00\] \- \[511.75\] 

Short Duration Package Power Limit 

本項目為 Intel 參數，稱為 \[power limit 1\]，以瓦特為單位表示。預設值由處 理器的 TDP（散熱設計功耗）定義。增加此值可讓 Turbo 倍頻在更高的電流負 載下維持更長時間。 設定值有：\[Auto\] \[1\] \- \[4095\] 

Package Power Time Window 

本項目為 Intel 參數 \[power limit 1\]，以秒為單位表示。套用的值表示當 TDP  超過限制時，Turbo 倍頻可以保持多長時間。設定值有：\[Auto\] \[1\] \[2\] \[3\] \[4\]  \[5\] \[6\] \[7\] \[8\] \[10\] \[12\] \[14\] \[16\] \[20\] \[24\] \[28\] \[32\] \[40\] \[48\] \[56\] \[64\] \[80\]  \[96\] \[112\] \[128\] \[160\] \[192\] \[224\] \[256\] \[320\] \[384\] \[448\] 

Short Duration Package Power Limit 

本項目為 Intel 參數，稱為 \[power limit 2\]，以瓦特為單位表示。這是第二項 電源限制，當封包電源超過 Power Limit 1 時，提供您快速的防護。預設值為  1.25 乘以 power limit 1。依照 Intel 的定義，當功耗超過 power limit 2 時，平台 必須支援此值達 10 毫秒。華碩主機板經過精心設計，可按照需要支援此值更長 時間，以執行超頻。 設定值有：\[Auto\] \[1\] \- \[4095\] 

Dual Tau Boost 

本項目可讓您開啟 Dual Tau Boost 功能。僅適用於桌機 35W/65W/12W sku。 啟用 DPTF 後，此功能將被忽略。設定值有：\[Disabled\] \[Enabled\] 

IA AC Load Line 

本項目用來設定 AC 負載線，以毫歐姆為單位。請使用 \<+\> 與 \<-\> 鍵調整數 值。設定值有：\[Auto\] \[0.01\] \- \[62.49\] 

IA DC Load Line 

本項目用來設定 DC 負載線，以毫歐姆為單位。請使用 \<+\> 與 \<-\> 鍵調整數 值。設定值有：\[Auto\] \[0.01\] \- \[62.49\] 

IA CEP Enable 

本項目為啟用或關閉 IA CEP 支援功能。使用 pCode Mailbox 指令 0x37，Sub command 0x1。將 Databit2 設定為 1。 

設定值有：\[Auto\] \[Disabled\] \[Enabled\]

Pro WS W680-ACE BIOS 使用手冊 31   
GT CEP Enable 

本項目為啟用或關閉 GT CEP 支援功能。使用 pCode Mailbox 指令 0x37， Sub-command 0x1。將 Databit3 設定為 1。 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

SA CEP Enable 

本項目為啟用或關閉 SA CEP 支援功能。使用 pCode Mailbox 指令 0x37， Sub-command 0x1。將 Databit3 設定為 1。 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

IA SoC Iccmax Reactive Protector 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

Inverse Temperature Dependency Throttle 

設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

IA VR Voltage Limit 

Voltage Limit (VMAX).此數值表示最大瞬間電壓。範圍為 0 \- 7999mV。使用  BIOS VR mailbox 指令 0x8。設定值有：\[Auto\] \[0\] \- \[7999\] 

CPU SVID Support 

關閉 SVID 支援可使 CPU 停止與外部電壓調節器通訊。設定值有：\[Auto\]  \[Disabled\] \[Enabled\] 

Tweaker’s Paradise 

Realtime Memory Timing 

本項目用來開啟或關閉即時記憶體時序。當設為 \[Enabled\] 時，系統將允許在  MRC\_DONE 後執行即時記憶體時序變更。設定值有：\[Disabled\] \[Enabled\] 

SPD Write Disable 

本項目用來開啟或關閉設定 SPD Write Disable。為了安全起見，您必須設定 禁止寫入 SPD。設定值有：\[TRUE\] \[FALSE\] 

PVD Ratio Threshold 

對於 Core Domain PLL，切換至較低後分頻器的臨界值預設為 15。當推高  BCLK 時，您可以設定一個低於 15 的值，以例數位控制振盪器（DCO）維持在 合理的頻率。設定值有：\[Auto\] \[1\] \- \[40\] 

SA PLL Frequency Override 

本項目用來設定 Sa PLL 頻率。設定值有：\[Auto\] \[3200 MHz\] \[1600 MHz\] BCLK TSC HW Fixup   
本項目用來在 TSC 由 PMA 複製至 APIC 時啟用或關閉 BCLK TSC HW Fixup。 設定值有：\[Enabled\] \[Disabled\]

32 Pro WS W680-ACE BIOS 使用手冊   
FLL OC mode 

設定值有：\[Auto\] \[Disabled\] \[Normal\] \[Elevated\] \[Extreme Elevated\] UnderVolt Protection   
啟用本項目時，在作業系統執行期使用者將無法指定低電壓。建議維持啟用的 預設值。 

\[Disabled\] 無低電壓保護。 

\[Enabled\] 允許 BIOS 設定低電壓，但執行期啟用低電壓保護。 Core PLL Voltage   
本項目可以用來設定 Core PLL VCC Trim。數值以 0.015V 為間隔，變更的範 圍由 0.900V 至 1.845V。設定值有：\[Auto\] \[0.90000\] \- \[1.84500\] 

GT PLL Voltage 

本項目可以用來設定 GT PLL VCC Trim。數值以 0.015V 為間隔，變更的範圍 由 0.900V 至 1.845V。設定值有：\[Auto\] \[0.90000\] \- \[1.84500\] 

Ring PLL Voltage 

本項目可以用來設定 Ring PLL VCC Trim。數值以 0.015V 為間隔，變更的範 圍由 0.900V 至 1.845V。設定值有：\[Auto\] \[0.90000\] \- \[1.84500\] 

System Agent PLL Voltage 

本項目可以用來設定 System Agent PLL VCC Trim。數值以 0.015V 為間隔， 變更的範圍由 0.900V 至 1.845V。設定值有：\[Auto\] \[0.90000\] \- \[1.84500\] 

Memory Controller PLL Voltage 

本項目可以用來設定 Memory Controller PLL VCC Trim。數值以 0.015V 為間 隔，變更的範圍由 0.900V 至 1.845V。 

設定值有：\[Auto\] \[0.90000\] \- \[1.84500\] 

CPU 1.8V Small Rail 

本項目可以用來設定 CPU 1.8V Small Rail 的電壓。數值以 0.010V 為間隔， 變更的範圍由 1.500V 至 2.300V。 

設定值有：\[Auto\] \[1.50000\] \- \[2.30000\] 

PLL Termination Voltage 

本項目可以用來設定 PLL Termination 的電壓。數值以 0.010V 為間隔，變更 的範圍由 0.800V 至 1.800V。設定值有：\[Auto\] \[0.80000V\] \- \[1.80000V\] 

CPU Standby Voltage 

本項目可以用來設定 CPU 待機電壓。請使用 \<+\> 與 \<-\> 鍵調整數值。數值以  0.010V 為間隔，變更的範圍由 0.800V 至 1.800V。設定值有：\[Auto\] \[0.80000\]  \- \[1.80000\] 

PCH 1.05V Voltage 

本項目可以用來設定 PCH 待機電壓。請使用 \<+\> 與 \<-\> 鍵調整數值。數值以  0.010V 為間隔，變更的範圍由 0.800V 至 1.600V。設定值有：\[Auto\] \[0.80000\]  \- \[1.60000\]

Pro WS W680-ACE BIOS 使用手冊 33   
PCH 0.82V Voltage 

本項目可以用來設定 PCH 0.82V 的電壓。請使用 \<+\> 與 \<-\> 鍵調整數值。 數值以 0.010V 為間隔，變更的範圍由 0.700V 至 1.000V。設定值有：\[Auto\]  \[0.70000\] \- \[1.00000\] 

CPU Input Voltage Reset Voltage 

本項目可以用來設定 CPU 輸入重置時的電壓。請使用 \<+\> 與 \<-\> 鍵調整數 值。數值以 0.010V 為間隔，變更的範圍由 1.500V 至 2.100V。設定值有： \[Auto\] \[1.50000\] \- \[2.10000\]

34 Pro WS W680-ACE BIOS 使用手冊   
7\. 進階選單（Advanced menu） 

在進階選單（Advanced menu）裡的項目，為提供您變更 CPU 與其他系統裝置的 設定。將捲軸往下捲動來顯示以下項目。 

注意！在您設定本進階選單的設定時，不正確的設定值將導致系統功能 異常。

![][image6]Pro WS W680-ACE BIOS 使用手冊 35   
7.1 平台各項設定（Platform Misc Configuration） 

本項目用來設定與平台相關的功能。 

![][image7]PCI Express Native Power Management   
將本項目設定為 \[Enabled\]時，可提升 PCI Express 的省電功能及作業系統的 ASPM  功能。設定值有：\[Disabled\] \[Enabled\] 

以下項目只有在 PCI Express Native Power Management 設為 \[Enabled\] 時才會出現。 

Native ASPM 

將本項目設為 \[Enabled\] 使用 OS 控制 ASPM，或是設為 \[Disabled\] 使用 BIOS 控 制 ASPM。設定值有：\[Auto\] \[Enabled\] \[Disabled\] 

PCH \- PCI Express 

DMI Link ASPM Control 

本項目可讓您控制 DMI Link 上北橋（NB）與南橋（SB）的 Active State Power  Management（ASPM）。設定值有：\[Disabled\] \[L1\] \[Auto\] 

ASPM 

本項目用來選擇 ASPM state 的節能狀態。設定值有：\[Disabled\] \[L1\] \[Auto\] 

L1 Substates 

本項目可選擇設定 PCI Express L1 Substates。設定值有：\[Disabled\] \[L1.1\] \[L1.1 &  L1.2\] 

SA \- PCI Express 

DMI ASPM Control 

本項目用來設定 DMI ASPM 支援。設定值有：\[Disabled\] \[Auto\] \[ASPM L1\] 

36 Pro WS W680-ACE BIOS 使用手冊   
DMI Gen3 ASPM 

本項目用來設定 DMI Gen3 ASPM 支援。設定值有：\[Disabled\] \[Auto\] \[ASPM L1\] 

PEG \- ASPM 

本項目用來控制 PEG 0 的 ASPM 支援。若 PEG 不是目前使用的裝置則無效。設 定值有：\[Disabled\] \[L0s\] \[L1\] \[L0sL1\] 

PCI Express Clock Gating 

本項目用來開啟或關閉每個連接埠的 PCI Express Clock Gating。 設定值有：\[Disabled\] \[Enabled\] 

7.2 CPU 設定（CPU Configuration） 

本項目可讓您得知中央處理器的各項資訊與變更中央處理器的相關設定。將捲軸 往下捲動來顯示以下項目。 

以下畫面所顯示項目可能會因您所安裝處理器不同而有所差異。 

![][image8]  
Efficient Core Information 

此子選單顯示效率核心訊息。 

Performance Core Information 

此子選單顯示效能核心訊息。

Pro WS W680-ACE BIOS 使用手冊 37   
Hardware Prefetcher 

啟用或關閉 MLC 控制器。設定值有：\[Disabled\] \[Enabled\] 

Adjacent Cache Line Prefetch 

本項目可以讓處理器在 L2 Cache 進行預取反饋和資料，從而降低記憶體負荷時 間，改善系統效能。設定值有：\[Disabled\] \[Enabled\] 

Intel (VMX) Virtualization Technology 

當本項目設為 \[Enabled\] 時，啟動 Intel 虛擬技術（Virtualization Technology）， 讓硬體平台可以同時執行多個作業系統。設定值有：\[Disabled\] \[Enabled\] 

以下項目只有在 Intel Trusted Execution Technology 設為 \[Disabled\] 時 才會出現。 

Active Performance Cores 

本項目可讓您設定在每個處理封包中啟用的處理器核心數量。設定值有：\[All\] \[1\]  \- \[7\] 

Active Efficient Cores 

本項目可讓您設定在每個處理封包中啟用的效率核心數量。設定值有：\[All\] \[0\] \-  \[3\] 

核心數與效率核心數同時檢視，當兩者都是 {0,0} 時，Pcode 將啟用所有 核心。 

Hyper-Threading 

啟動本項目可以讓高速執行緒處理器在作業系統內作為兩個邏輯處理器，允許作 業系統同時處理二個執行緒或處理器。 

\[Enabled\] 每個啟動核心可二個執行緒。 

\[Disabled\] 每個啟動核心僅可一個執行緒。 

以下項目只有在 Intel (VMX) Virtualization Technology 設為 \[Enabled\] 時才會出現。 

Intel Trusted Execution Technology 

本項目可以啟用由 Intel Trusted Execution Technology 提供的額外硬體功能應用。 設定值有：\[Disabled\] \[Enabled\] 

變更會在完整電源週期後生效。

38 Pro WS W680-ACE BIOS 使用手冊   
以下項目只有在 Intel Trusted Execution Technology 設為 \[Enabled\] 時 才會出現。 

Alias Check Request 

本項目可以啟用 Txt Alias 測試。 設定值有：\[Disabled\] \[Enabled\] 

• 變更需要完整 Txt 能力才會生效。 

• 此為單次變更，會在下次開機後重置。 

DPR Memory Size (MB) 

預留 DPR 記憶體大小（0-255）MB。設定值有：\[0\] \- \[255\] 

Reset AUX Content 

重置 AUX Content。AUX 內容重置後 Txt 可能無法發揮功能。 

Total Memory Encryption 

本項目用來設定全記憶體加密（TME）以保護動態隨機存取記憶體資料免受物理 攻擊。設定值有：\[Disabled\] \[Enabled\] 

Legacy Game Compatibility Mode 

本項目設定為 \[Enabled\] 時，按下捲軸鎖定鍵可切換為效率核心, 同時捲軸鎖定指 示燈會亮起。設定值有：\[Disabled\] \[Enabled\] 

CPU \- Power Management Control 

本項目用來管理與設定處理器電力。 

Boot performance mode 

在作業系統切換前允許您選擇 BIOS 的效能狀態。設定值有：\[Max Battery\]  \[Max Non-Turbo Performance\] \[Turbo Performance\] \[Auto\] 

Intel(R) SpeedStep(tm) 

本項目可以支援兩個以上的頻率。設定值有：\[Disabled\] \[Enabled\] Intel(R) Speed Shift Technology   
本項目用來開啟或關閉 Intel(R) Speed Shift Technology 的支援。當開啟時， CPPC v2 介面可以讓硬體控制 P-states。設定值有：\[Disabled\] \[Enabled\] 

Intel(R) Turbo Boost Max Technology 3.0 

本項目用來啟動或關閉 Intel(R) Turbo Boost Max Technology 3.0 支援。當關閉 本功能時，將會在 \_CPC 回報最大核心比值的最慢核心。設定值有：\[Disabled\]  \[Enabled\]

Pro WS W680-ACE BIOS 使用手冊 39   
Turbo Mode 

本項目用來設定核心處理器的速度，使其在運作電力、電流與溫度條件限制 下，可以比基本運作頻率更快的速度運作。設定值有：\[Disabled\] \[Enabled\] 

Acoustic Noise Settings 

本子選單中的項目可以讓您為 IA、GT 與 SA 網域進行噪音設定。 Acoustic Noise Settings   
Acoustic Noise Mitigation 

當 CPU 處於更深的 C 狀態時，啟用此選項將有助於減輕某些 SKU 上的噪 音。設定值有：\[Disabled\] \[Enabled\] 

以下項目只有在 Acoustic Noise Mitigation 設為 \[Enabled\] 時才會出現。 

Pre Wake Time 

本項目讓您以 micro ticks 為單位設定最大 Pre Wake 隨機分派時間。這是 為了減輕噪音動態週期性改變（DPA）調整。請使用 \<+\> 與 \<-\> 鍵調整數 值。設定值有：\[0\] \- \[255\] 

Ramp Up Time 

本項目讓您以 micro ticks 為單位設定最大 Ramp Up 隨機分派時間。這是 為了減輕噪音動態週期性改變（DPA）調整。請使用 \<+\> 與 \<-\> 鍵調整數 值。設定值有：\[0\] \- \[255\] 

Ramp Down Time 

本項目讓您以 micro ticks 為單位設定最大 Ramp Up 隨機分派時間。這是 為了減輕噪音動態週期性改變（DPA）調整。請使用 \<+\> 與 \<-\> 鍵調整數 值。設定值有：\[0\] \- \[255\] 

IA VR Domain 

Disable Fast PKG C State Ramp for IA Domain 

需要配置此選項以在更深的 C-state 期間減少噪音。 

\[FALSE\] 請勿在更深的 C-state 期間停用 Fast ramp。 

\[TRUE\] 在更深的 C-state 期間停用 Fast ramp。 

Slow Slew Rate for IA Domain 

為深封裝 C-state 緩衝時間設定 VR IA 慢速電壓轉換速率；慢速電壓轉換 速率等於快速除以數字，數字為 2、4、8 以減慢擺率以幫助最大限度地減 少噪音。設定值有：\[Fast/2\] \[Fast/4\] \[Fast/8\] 

GT VR Domain 

Disable Fast PKG C State Ramp for GT Domain 

需要配置此選項以在更深的 C-state 期間減少噪音。 

\[FALSE\] 請勿在更深的 C-state 期間停用 Fast ramp。 

\[TRUE\] 在更深的 C-state 期間停用 Fast ramp。

40 Pro WS W680-ACE BIOS 使用手冊   
Slow Slew Rate for GT Domain 

為深封裝 C-state 緩衝時間設定 VR GT 慢速電壓轉換速率；慢速電壓轉換 速率等於快速除以數字，數字為 2、4、8 以減慢擺率以幫助最大限度地減 少噪音。設定值有：\[Fast/2\] \[Fast/4\] \[Fast/8\] 

CPU C-states 

本項目用來開啟或關閉 CPU 電源節能。允許 CPU 在未 100% 使用時進入  C-state。設定值有：\[Auto\] \[Disabled\] \[Enabled\] 

以下項目只有在 CPU C-states 設為 \[Enabled\] 時才會出現。 

Enhanced C-States 

本項目用來開啟或關閉 C1E。啟用後，當所有核心進入 C-state 時，CPU 將切 換至最低速度。設定值有：\[Enabled\] \[Disabled\] 

Package C State Limit 

本項目可讓您設定處理器封包的 C-state 限制。設定為 \[CPU Default\] 會將其 保留為出廠預設值。設定為 \[Auto\] 將初始化最深的可用封包 C-State 限制。設定 值有：\[C0/C1\] \[C2\] \[C3\] \[C6\] \[C7\] \[C7s\] \[C8\] \[C9\] \[C10\] \[CPU Default\] \[Auto\] 

Thermal Monitor 

本項目可讓您啟用或停用 Thermal Monitor。設定值有：\[Disabled\] \[Enabled\] Dual Tau Boost   
本項目可讓您開啟 Dual Tau Boost 功能。僅適用於桌機 35W/65W/125W sku。 啟用 DPTF 後，此功能將被忽略。設定值有：\[Disabled\] \[Enabled\]

Pro WS W680-ACE BIOS 使用手冊 41   
7.3 系統代理設定（System Agent Configuration） 

本選單可讓您變更系統代理的各項相關設定。 

![][image9]VT-d   
本項目用來在記憶體控制中心開啟虛擬化技術。設定值有：\[Enabled\] \[Disabled\] 以下項目只有在 Vt-d 設為 \[Enabled\] 時才會出現。 

Control Iommu Pre-boot Behavior 

本項目可讓您在預啟動環境中開啟或關閉 IOMMU（若 DXE 中安裝了 DMAR  列表，而 PEI 中安裝了 VTD\_INFO\_PPI）。設定值有：\[Disable IOMMU\] \[Enable  IOMMU during boot\] 

Memory Configuration 

本項目用來設定記憶體配置參數。  

Memory Remap 

本項目用來開啟或關閉 4GB 以上的記憶體 remap。設定值有：\[Enabled\]  \[Disabled\] 

Graphics Configuration 

本項目用來選擇以處理器、PEG 顯示裝置或 PCIe 顯示裝置作為優先使用的顯示裝 置。 

Primary Display 

本項目用來選擇以處理器、PEG 顯示裝置或 PCIe 顯示裝置作為優先使用的顯 示裝置。設定值有：\[Auto\] \[CPU Graphics\] \[PEG Slot\] \[PCIE\] 

iGPU Multi-Monitor 

將此項目設定為 \[Enabled\] 以啟用整合與獨立顯示卡的多顯示器輸出。iGPU  共享系統記憶體大小將固定為 64M。設定值有：\[Disabled\] \[Enabled\] 

DVMT Pre-Allocated 

本項目可以讓您選擇內部顯示裝置使用的 DVMT 5.0 預置（固定）顯示記憶 體大小。設定值有：\[32M\] \[64M\] \[96M\] \[128M\] \[160M\] \[192M\] \[224M\] \[256M\]  \[288M\] \[320M\] \[352M\] \[384M\] \[416M\] \[448M\] \[480M\] \[512M\]

42 Pro WS W680-ACE BIOS 使用手冊   
RC6(Render Standby) 

本項目用來開啟 Render Standby 支援。設定值有：\[Disabled\] \[Enabled\] 

VMD setup menu 

本項目用來管理與設定 VMD 電力。 

Enable VMD controller 

啟用或關閉 VMD 控制器。設定值有：\[Disabled\] \[Enabled\] 

將 Enable VMD controller 設為 \[Disabled\] 可能會導致資料遺失。 

以下項目只有在 Enable VMD controller 設為 \[Enabled\] 時才會出現。 

Map PCIE Storage under VMD 

本項目用來 map 或 unmap PCIE 儲存至 VMD。設定值有：\[Disabled\] \[Enabled\] 

如果將 Map PCIE Storage under VMD 設為 \[Enabled\]，請確認 Map  SATA Controller under VMD 設為 \[Disabled\]。 

Map SATA Controller under VMD 

本項目用來 map 或 unmap 本啟動連接埠至 VMD。設定值有：\[Disabled\]  \[Enabled\] 

如果將 Map SATA Controller under VMD 設為 \[Enabled\]，請確認 Map  PCIE Storage under VMD 設為 \[Disabled\]。 

PCI Express Configuration 

本子選單中的項目可以讓您為不同的內建插槽配定 PCIe 速度。  M.2\_1 Link Speed   
本項目用來設定此插槽的 PCIe 運作速度。設定值有：\[Auto\] \[Gen1\] \[Gen2\]  \[Gen3\] \[Gen4\] 

PCIEX16(G5)\_1 Link Speed 

本項目用來設定此插槽的 PCIe 運作速度。設定值有：\[Auto\] \[Gen1\] \[Gen2\]  \[Gen3\] \[Gen4\] \[Gen5\] 

PCIEX16 (G5)\_2 Link Speed 

本項目用來設定此插槽的 PCIe 運作速度。設定值有：\[Auto\] \[Gen1\] \[Gen2\]  \[Gen3\] \[Gen4\] \[Gen5\]

Pro WS W680-ACE BIOS 使用手冊 43   
7.4 PCH 設定（PCH Configuration） 

本項目可以管理與設定 PCH PCI 插槽。 

![][image10]PCI Express Configuration   
本項目可以管理與設定 PCH PCI 插槽速度。 

PCIEX16(G3) Link Speed 

本項目用來設定此插槽的 PCIe 運作速度。設定值有：\[Auto\] \[Gen1\] \[Gen2\]  \[Gen3\] 

PCIEX16(G3)\_1 Link Speed 

本項目用來設定此插槽的 PCIe 運作速度。設定值有：\[Auto\] \[Gen1\] \[Gen2\]  \[Gen3\] 

PCIEX16(G3)\_2 Link Speed 

本項目用來設定此插槽的 PCIe 運作速度。設定值有：\[Auto\] \[Gen1\] \[Gen2\]  \[Gen3\] 

M.2\_2 Link Speed 

本項目用來設定此插槽的 PCIe 運作速度。設定值有：\[Auto\] \[Gen1\] \[Gen2\]  \[Gen3\] \[Gen4\] 

M.2\_3 Link Speed 

本項目用來設定此插槽的 PCIe 運作速度。設定值有：\[Auto\] \[Gen1\] \[Gen2\]  \[Gen3\] \[Gen4\]

44 Pro WS W680-ACE BIOS 使用手冊   
7.5 PCH 儲存裝置設定（PCH Storage Configuration） 

當您進入 BIOS 設定程式時，BIOS 設定程式將自動偵測已安裝的 SATA 裝置。當 未偵側到 SATA 裝置時將顯示 Empty。將捲軸往下捲動來顯示其他 BIOS 項目。 

![][image11]SATA Controller(s)    
啟用或關閉 SATA 控制器。設定值有：\[Disabled\] \[Enabled\] 

以下項目只有在 SATA Controller(s) 設為 \[Enabled\] 時才會出現。 

Aggressive LPM support 

當本項目設定為 \[Enabled\] 時，可以讓 PCH 主動進入連接電源狀態。設定值有： \[Disabled\] \[Enabled\] 

SMART Self Test 

S.M.A.R.T.（自動偵測、分析、報告技術，Self-Monitoring, Analysis and Reporting  Technology）是一個監控軟體，可以監控您的硬碟，並在發生錯誤時於開機自我檢測 （POST）時顯示錯誤訊息。設定值有：\[Disabled\] \[Enabled\]  

SLIMSAS\_1 \- SLIMSAS\_4 

本項目可以啟動或關閉選擇的連接埠。設定值有：\[Disabled\] \[Enabled\] SLIMSAS\_1 \- SLIMSAS\_4 Hot Plug    
指定此連接埠支援熱抽換功能。設定值有：\[Disabled\] \[Enabled\]

Pro WS W680-ACE BIOS 使用手冊 45   
SATA6G\_1 \- SATA6G\_4 

本項目可以啟動或關閉選擇的連接埠。設定值有：\[Disabled\] \[Enabled\] 

SATA6G\_1 \- SATA6G\_4 Hot Plug  

指定此連接埠支援熱抽換功能。設定值有：\[Disabled\] \[Enabled\] 

7.6 PCH-FW 設定（PCH-FW Configuration） 

本選單用來設定韌體 TPM 相關的功能。 

![][image12]PTT   
本項目用來開啟或關閉 SkuMgr 中的 PTT。設定值有：\[Disable\] \[Enable\] 

Extend CSME Measurement to TPM-PCR 

本項目用來開啟或關閉 Extend CSME Measurements 為 TPM-PCR\[0\]，並設定 AMR  為 TPM-PCR\[1\]。設定值有：\[Disabled\] \[Enabled\] 

7.7 AMT 設定（AMT Configuration） 

本選單用來設定 Intel(R) Active Management Technology 的相關功能。 ![][image13]USB Provisioning of AMT   
本項目用來開啟或關閉 AMT-USB。設定值有：\[Disabled\] \[Enabled\] 

MAC Pass Through 

本項目用來開啟或關閉 MAC（SERR Reporting Enable）。設定值有：\[Disabled\]  \[Enabled\]

46 Pro WS W680-ACE BIOS 使用手冊   
Activate Remote Assistance Process 

觸發 CIRA 開機。設定值有：\[Disabled\] \[Enabled\] 

由 MEBx Setup 啟用時需要網路連線。 

Unconfigure ME 

下次開機時 Unconfigure ME 重設 MEBx 密碼為預設值。設定值有：\[Disabled\]  \[Enabled\] 

ASF Configuration 

此選單中的選項用來設定 Alert Standard Format 參數。 

PET Progress 

本項目用來開啟或關閉 PET Events Progress。設定值有：\[Disabled\] \[Enabled\] WatchDog   
本項目用來開啟或關閉看門狗計時器。設定值有：\[Disabled\] \[Enabled\] 以下項目只有在 WatchDog 設為 \[Enabled\] 時才會出現。 

OS Timer 

設定系統的看門狗計時器。設定值有：\[0\] \- \[65535\] 

BIOS Timer 

設定 BIOS 的看門狗計時器。設定值有：\[0\] \- \[65535\] 

ASF Sensors Table 

新增 ASF Sensor Table 至ASF。ACPI 表格。設定值有：\[Disabled\] \[Enabled\] 

Secure Erase Configuration 

此選單中的選項用來設定安全清除。 

Secure Erase mode 

變更安全清除模式。 

\[Simulated\] 執行安全清除不清除固態硬碟。 

\[Real\] 清除固態硬碟。 

Force Secure Erase 

在下次開機時強制執行安全清除。設定值有：\[Disabled\] \[Enabled\] 

One Click Recovery (OCR) Configuration 

此選單中的選項用來設定 One Click Recovery。將會存取 AMT 以執行還原 OS 應 用程式。

Pro WS W680-ACE BIOS 使用手冊 47   
OCR Https Boot 

啟用或關閉 One Click Recovery Https Boot。設定值有：\[Disabled\] \[Enabled\] OCR PBA Boot   
本項目用來開啟或關閉 One Click Recovery PBA Boot。設定值有：\[Disabled\]  \[Enabled\] 

OCR Windows Recovery Boot 

本項目用來開啟或關閉 One Click Recovery Windows Recovery Boot。設定值 有：\[Disabled\] \[Enabled\] 

OCR Disable Secure Boot 

允許 CSME 關閉 Secure Boot。設定值有：\[Disabled\] \[Enabled\] 

7.8 Thunderbolt(TM) 設定（Thunderbolt(TM) Configuration） 本項目用來設定 Thunderbolt 的相關功能。 

![][image14]PCIE Tunneling over USB4   
本項目用來開啟或關閉 USB4 的 PCIE Tunneling。設定值有：\[Disabled\] \[Enabled\] 

Discrete Thunderbolt(TM) Support 

本項目用來開啟或關閉 Discrete Thunderbolt 支援。設定值有：\[Disabled\] \[Enabled\] 

• 請在 System Agent(SA) Configuration 頁面將 Control Iommu Pre boot Behavior 設為 \[Enabled\] 以支援 DMA Protection 功能。 

• 以下項目只有在 Discrete Thunderbolt(TM) Support 設為 \[Enabled\] 時才會出現。 

Wake From Thunderbolt(TM) Devices 

本項目可以開啟或關閉 Thunderbolt(TM) 裝置的系統喚醒。設定值有：\[Disabled\]  \[Enabled\] 

Discrete Thunderbolt(TM) Configuration 

本項目用來設定 Discrete Thunderbolt(TM) 的相關功能。 

DTBT Go2Sx Command 

本項目用來啟用在系統進入 Sx 時將 DTBT 置於 Sx 狀態的指令。設定值有： \[Disabled\] \[Enabled\]

48 Pro WS W680-ACE BIOS 使用手冊   
Windows 10 Thunderbolt Support 

本項目可以讓您指定 Windows 10 Thunderbolt 支援等級。 

\[Enable \+ RTD3\] OS 原生支援 RTD3。 

\[Disabled\] 無 OS 原生支援。 

DTBT Controller 0 Configuration 

DTBT Contorller 0 

設定值有：\[Disabled\] \[Enabled\] 

TBT Host Router 

可以依據可用連接埠啟用主機路由器。設定值有：\[One Port\] \[Two Port\] Extra Bus Reserved   
本項目用來選擇 TBT Root Port Type。 

\[56\] One port Host。 

\[106\] Two port Host. 

Reserved Memory 

本項目可以讓您為此根橋接器設定保留記憶體。請使用 \<+\> 與 \<-\> 鍵調整 數值。設定值有：\[1\] \- \[4096\] 

Memory alignment 

本項目用來設定記憶體對齊位元。請使用 \<+\> 與 \<-\> 鍵調整數值。設定值 有：\[0\] \- \[31\] 

Reserved PMemory 

本項目可以讓您為此根橋接器設定保留預取記憶體。請使用 \<+\> 與 \<-\> 鍵 調整數值。設定值有：\[1\] \- \[4096\] 

PMemory alignment 

本項目用來設定 PMemory 對齊位元。請使用 \<+\> 與 \<-\> 鍵調整數值。設 定值有：\[0\] \- \[31\] 

Reserved I/O 

請使用 \<+\> 與 \<-\> 鍵調整數值。請使用 \<+\> 與 \<-\> 鍵調整數值。數值以 4  為間隔，變更的範圍由 0 至 60。設定值有：\[0\] \- \[60\]

Pro WS W680-ACE BIOS 使用手冊 49   
7.9 Redfish Host 介面設定（Redfish Host Interface Settings） 本選單的項目可以進行 Redfish Host 介面設定。

Redfish   
本項目可以啟用或關閉 Redfish。設定值有：\[Disabled\] \[Enabled\] 以下項目只有在 Redfish 設為 \[Enabled\] 時才會出現。 

Authentication mode 

本項目可以選擇認證模式。設定值有：\[Basic Authentication\] \[Session  Authentication\] 

7.10 序列埠控制面板重新定向（Serial Port Console Redirection） 本項目用來設定控制台重新導向功能。 

![][image15]COM0 / COM1 (Pci Bus0, Dev22, Func3, Port0) 

Console Redirection 

啟用或關閉控制台重新導向功能。設定值有：\[Disabled\] \[Enabled\] 

50 Pro WS W680-ACE BIOS 使用手冊   
以下項目只有在 Console Redirection for COM0 或 COM1 (Pci Bus0,  Dev22, Func3, Port0) 設為 \[Enabled\] 時才會出現。 

Console Redirection Settings 

當 Console Redirection 項目設為 \[Enabled\] 時，本項目才可以設定。本設定用 來指定主機電腦與遠程電腦（用戶所使用的電腦）之間如何交換資料。兩台電腦 之間必須採用相同或相容的設定。 

Terminal Type 

提供您設定終端類型。 

\[VT100\] ASCII 字元設定。 

\[VT100+\] 延伸 VT100 支援顏色、功能鍵等等。 

\[VT-UTF8\] 使用 UTF8 加密以映像 Unicode（萬國碼）字元在 1 或更多位元 組以上。 

\[ANSI\] 延伸 ASCII 字元設定。 

Bits per second 

選擇序列埠傳輸速度。此速度必須與另一側相符。較長或有噪音的線路需要較 低的速度。 設定值有：\[9600\] \[19200\] \[38400\] \[57600\] \[115200\] 

Data Bits 

設定值有：\[7\] \[8\] 

Parity 

同位位元能與資料位元一起發送，以檢測一些傳輸錯誤。不能使用 \[Mark\] 與  \[Space\] 來檢測錯誤。會被視為額外資料位元。 

\[None\] 無 

\[Even\] 同位位元為 0，表示 N 個位元裡，1 出現的總次數為偶數。 \[Odd\] 同位位元為 0，表示 N 個位元裡，1 出現的總次數為奇數。 \[Mark\] 同位位元總是 1。 

\[Space\] 同位位元總是 0。 

Stop Bits 

停止位元為序列資料封包的終點（開始位元表示起始）。標準設定是 1 Stop  bit。使用較慢的裝置通訊可能會需要超過 1 stop bit。設定值有：\[1\] \[2\] 

Flow Control 

Flow control（流量控制）能預防在緩衝區溢滿時的資料流失。當傳送資料 時，若接收的緩衝區已經滿了，此時會送出停止訊號來停止傳送資料流（data  flow）。當緩衝區空出時，會再送出開始訊號以重新開始傳送資料流。硬體流量 控制使用兩條金屬線來傳送開始 / 停止訊號。設定值有：\[None\] \[Hardware RTS/ CTS\] 

VT \-UTF8 Combo Key Support 

當 Terminal Type 項目設定為 \[ANSI\] 或 \[VT100\] 時，本項目才會顯示，並可 以讓您啟動或關閉在 ANSI 或 VT100 終端器下所支援的 VT-UTF8 組合碼。設定 值有：\[Disabled\] \[Enabled\]

Pro WS W680-ACE BIOS 使用手冊 51   
Recorder Mode  

若啟用此模式僅會傳送文字，此為擷取終端資料。設定值有：\[Disabled\]  \[Enabled\] 

Resolution 100x31 

本項目用來開啟或關閉延伸終端的解析度。設定值有：\[Disabled\] \[Enabled\] Putty Keypad   
本項目提供您選擇 FunctionKey 與在 Putty 上面的 Keypad。設定值有： \[VT100\] \[LINUX\] \[XTERMR6\] \[SCO\] \[ESCN\] \[VT400\] 

Legacy Console Redirection Settings 

Redirection COM Port 

可讓您選擇一個 COM 連接埠以顯示 Legacy 作業系統與 Legacy OPROM 訊息 的重新導向。設定值有： \[COM0\] \[COM1(Pci Bus0, Dev22, Func3, Port0)\] 

Resolution 

設定支援傳統作業系統的行、列數。設定值有：\[80x24\] \[80x25\] 

Redirection After POST 

本項目預設值為 \[Always Enable\]。 

\[Always Enable\] 啟用此功能。 

\[Bootloader\] 關閉此功能。 

7.11 Intel TXT 資訊（Intel TXT Information） 

本選單提供 Intel TXT 資訊。

52 Pro WS W680-ACE BIOS 使用手冊   
7.12 PCI 子系統設定（PCI Subsystem Settings） 

本項目提供您設定 PCI、 PCI-X 和 PCI Express。 

Above 4G Decoding   
若您的系統支援 64-bit PCI 解碼能力，則可以啟用或關閉 64 位元運算能力的裝 置，來解碼超過 4G 以上的 Address Space（位址空間）。僅適用於系統支援 64-bit  PCI 解碼能力。設定值有：\[Disabled\] \[Enabled\] 

• 僅適用於 64 位元的作業系統。 

• 以下項目只有在 Above 4G Decoding 設為 \[Enabled\] 時才會出現。 

Re-Size BAR Support 

當系統具備 Resizable BAR 功能的 PCIe 裝置時，本項目可以啟用或關閉 Resizable  BAR 支援。設定值有：\[Disabled\] \[Enabled\] 

SR-IOV Support 

若系統有具備 SR-IOV 的 PCIe 裝置，本項目可以啟用或關閉支援 SIngle Root IO  Virtualization 功能。設定值有：\[Disabled\] \[Enabled\] 

7.13 USB 設定（USB Configuration） 

本選單可讓您變更 USB 裝置的各項相關設定。 

在 Mass Storage Devices 項目中會顯示自動偵測到的數值或裝置。若無 連接任何裝置，則會顯示 None。

Pro WS W680-ACE BIOS 使用手冊 53   
Legacy USB Support 

\[Enabled\] 啟用 USB 支援。 

\[Disabled\] USB 裝置僅在 BIOS 設定程式中可用，在開機裝置列表中無法被識 別。 

\[Auto\] 系統在開機時偵測是否有無 USB 裝置。若沒有偵測到 USB 裝置， 便會關閉 USB 向下相容功能。 

XHCI Hand-off 

此項目為不支援 XHCI hand-off 之作業系統的替代方法。XHCI 所有權變更需由  XHCI 驅動程式提出。 

\[Disabled\] 關閉本功能。  

\[Enabled\] 啟動支援沒有 XHCI hand-off 功能的作業系統。  

Mass Storage Devices: 

本項目用來設定主機板上安裝的大容量儲存裝置的模擬類型。\[Auto\] 按照裝置的 媒體格式來模擬裝置。光碟機會被模擬為 \[CD-ROM\]，無媒體的磁碟將依照磁碟類型 進行模擬。設定值有：\[Auto\] \[Floppy\] \[Forced FDD\] \[Hard Disk\] \[CD-ROM\] 

USB Single Port Control 

本項目用來啟動或關閉個別 USB 連接埠。 

USB 連接埠的位置請參考主機板手冊 後側面板連接埠 的說明。 

7.14 網路協定堆疊設定（Network Stack Configuration） 本選單可讓您變更網路協定堆疊的各項相關設定。 

Network stack   
設定值有：\[Disable\] \[Enable\]  

以下項目只有在 Network Stack 設為 \[Enabled\] 時才會出現。 

Ipv4/Ipv6 PXE Support 

本項目用來啟動或關閉 Ipv4/Ipv6 PXE 開機選項。設定值有：\[Disabled\]  \[Enabled\]

54 Pro WS W680-ACE BIOS 使用手冊   
7.15 NVMe 設定（NVMe Configuration） 

本選單顯示已連結裝置的 NVMe 控制器與驅動資訊。請按下 \<Enter\> 選擇本選單 顯示之已連結的 NVMe 裝置以檢視更多資訊。 

本選單的顯示選項會依連接至主機板的裝置而異。關於實際設定與選 項，請參考主機板的 BIOS。 

7.16 HDD/SSD SMART 資訊（HDD/SSD SMART Information） 本選單顯示已連結裝置的 SMART 資訊。 

本選單的顯示選項會依連接至主機板的裝置而異。關於實際設定與選 項，請參考主機板的 BIOS。 

NVM Express 裝置不支援 SMART 資訊。

Pro WS W680-ACE BIOS 使用手冊 55   
7.17 APM 設定（APM Configuration） 

本選單中的項目可用來調整進階電源管理（APM）設定。

Restore AC Power Loss   
本項目讓您的系統在 AC 電源中斷後可進入 ON 狀態、OFF 狀態或是同時進入這 兩種狀態。若您的系統設定 \[Last State\]，則將系統設定回復到電源未中斷之前的狀 態。設定值有：\[Power Off\] \[Power On\] \[Last State\] 

Max Power Saving 

設定值有：\[Disabled\] \[Enabled\] 

ErP Ready 

在 S4+S5 或 S5 休眠模式下關閉某些電源，減少待機模式下電力的流失，以符合 歐盟能源使用產品（Energy Related Product）的規範。當設定為 \[Enabled\] 時，其他  PME 選項將被關閉。RGB LED 指示燈與 RGB/可定址燈條接頭也會被關閉。設定值 有：\[Disabled\] \[Enabled (S4+S5)\] \[Enabled (S5)\] 

Power On By PCI-E 

本項目用來啟動或關閉內建網路控制器或其他安裝的 PCI-E 網路卡的喚醒功能。 設定值有：\[Disabled\] \[Enabled\] 

Power On By RTC 

本項目用來關閉或開啟即時時鐘（RTC）喚醒功能，啟用時您可自行設定天、小 時、分、秒以安排時間讓系統自動開機。設定值有：\[Disabled\] \[Enabled\] 

56 Pro WS W680-ACE BIOS 使用手冊   
7.18 內建裝置設定（OnBoard Devices Configuration） 本選單可讓您變更內建裝置的各項相關設定。將捲軸往下捲動來顯示以下項目。 

PCIe Bandwidth Bifurcation Configuration 

\[Auto Mode\] 執行完整 PCIe X16 模式。 

\[X8/X8 mode\] 將在 X16 上執行的 PCIEX16(G5)\_1 拆分為 X8/X8。 

HD Audio 

\[Disabled\] 高傳真音效無條件關閉。 

\[Enabled\] 高傳真音效無條件開啟。 

Intel 2.5G LAN1/2 

設定值有：\[Disabled\] \[Enabled\] 

Connectivity mode (Wi-Fi & Bluetooth) 

設定值有：\[Disabled\] \[Enabled\] 

Onboard LED 

本項目可以開啟或關閉 HDD 與 PLED LED 指示燈。設定值有：\[Disabled\]  \[Enabled\] 

Q-Code LED Function 

\[Disabled\] 關閉 Q-Code LED 指示燈。 

\[POST Code Only\] 在 Q-Code LED 指示燈上顯示開機自我檢測（Power-On Self Test）代碼。 

\[Auto\] 在 Q-Code LED 指示燈上自動顯示開機自我檢測（Power-On  Self-Test）代碼與 CPU 溫度。 

SlimSAS Configuration 

\[SATA mode\] 僅支援 SlimSAS SATA 裝置。 

\[PCIE mode\] 僅支援 SlimSAS PCIE 裝置。

Pro WS W680-ACE BIOS 使用手冊 57   
U32G2\_C1 Type C Power Mode 

\[Auto\] 偵測到裝置時電力會自動提供至 USB 3.2 Gen 2 Type-C 連接埠。 \[Enabled\] 電力會持續提供至 USB 3.2 Gen 2 Type-C 連接埠。 

不正確的裝置連接可能會永久損壞系統。 

GNA Device 

設定值有：\[Enabled\] \[Disabled\] 

Serial Port Configuration 

以下的項目可以讓您進行序列埠設定。 

Serial Port 

設定值有：\[Enabled\] \[Disabled\] 

以下項目只有在 Serial Port 設為 \[Enabled\] 時才會出現。 

Change settings 

本項目用來為 Super I/O 裝置選擇最佳設定。設定值有：\[IO=3F8h; IRQ=4\]  \[IO=2F8h; IRQ=3\] \[IO=3E8h; IRQ=4\] \[IO=2E8h; IRQ=3\] 

Parallel Port Configuration 

以下的項目可以讓您進行並列埠（LPT/LPTE）設定。 

Parallel Port 

設定值有：\[Disabled\] \[Enabled\] 

以下項目只有在 Parallel Port 設為 \[Enabled\] 時才會出現。 

Change settings 

本項目用來為 Super I/O 裝置選擇最佳設定。設定值有： \[Auto\] \[IO=378h;  IRQ=5\] \[IO=378h; IRQ=5,6,7,9,10,11,12\] \[IO=278h; IRQ=5,6,7,9,10,11,12\]  

Device Mode 

本項目用來設定印表機埠的模式。設定值有：\[STD Printer Mode\] \[SPP Mode\]  \[EPP-1.9 and SPP Mode\]\[EPP-1.7 and SPP Mode\] \[ECP Mode\] \[ECP and EPP-1.9  Mode\] \[ECP and EPP-1.7 Mode\]

58 Pro WS W680-ACE BIOS 使用手冊   
7.19 Intel(R) 快速儲存技術（Intel(R) Rapid Storage Technology） 本項目可以管理 Intel(R) RAID 控制器上的 RAID volumes。 

• 此選單中的項目可能因連接的儲存裝置而異。關於實際設定與選 項，請參考主機板的 BIOS。 

• 使用 Intel(R) Rapid Storage Technology 建立 RAID 前，請務必進行  VMD 設定。

Pro WS W680-ACE BIOS 使用手冊 59   
8\. 監控選單（Monitor menu） 

監控選單可讓您檢視系統溫度/電力狀態，並可用來變更風扇設定。將捲軸往下捲 動來顯示其他 BIOS 項目。 

Temperature Monitor 

CPU Temperature, CPU Package Temperature, MotherBoard Temperature,  VRM Temperature, Chipset Temperature, T\_Sensor Temperature, DIMM A1-2  Temperature, DIMM B1-2 Temperature \[xxx°C/xxx°F\] 

本系列主機板可自動偵測並顯示目前主機板與其他元件的溫度。若是您不想顯 示偵測的溫度，請選擇 \[Ignore\]。 

Fan Speed Monitor 

CPU Fan Speed, CPU Optional Fan Speed, Chassis Fan 1-3 Speed, Water Pump+  Speed \[xxxx RPM\] 

為了避免系統因為過熱而造成損壞，本系列主機板備有風扇的轉速 RPM （Rotations Per Minute）監控，所有的風扇都設定了轉速安全範圍，一旦風扇轉 速低於安全範圍，華碩智慧型主機板就會發出警訊，通知使用者注意。如果風 

扇並未連接至主機板，本項目會顯示 N/A。若是您不想顯示偵測的速度，請選擇  \[Ignore\]。

60 Pro WS W680-ACE BIOS 使用手冊   
Voltage and Current Monitor 

CPU Core Voltage, 12V Voltage, 5V Voltage, 3.3V Voltage, Memory Controller  Voltage \[x.xxx V\] 

本系列主機板具有電壓監控的功能，用來確保主機板以及 CPU 接受正確的電 壓準位，以及穩定的電流供應。若是您不想偵測這些項目，請選擇 \[Ignore\]。 

CPU Core Current \[xx A\] 

內建硬體監視器自動檢測電流輸出。若是您不想偵測這些項目，請選擇  \[Ignore\]。 

Q-Fan Configuration 

Q-Fan Tuning 

點選本項目會自動偵測最低速度並設定每個風扇的最小工作週期。 調整過程可能需要 2-5 分鐘，在此過程中請不要關閉或重新啟動系統。 

CPU Q-Fan Control 

本項目用來設定 CPU Q-Fan 運作模式。 

\[Auto Detect\] 偵測安裝的風扇/水泵類型並自動切換控制模式。 \[DC Mode\] 在 DC 模式啟動 Q-Fan Control 來使用 3-pin 風扇/水泵。 \[PWM Mode\] 在 PWM 模式啟動 Q-Fan Control 來使用 4-pin 風扇/水泵。 CPU Fan Profile   
本項目用來設定風扇/水泵適當的效能。選擇 \[Manual\] 時，當處理器溫度超 過 75°C 時，建議將風扇/水泵的占空比設為 100%。請留意當風扇/水泵占空比 不足導致過熱，會造成處理器效能限制。設定值有：\[Standard\] \[Silent\] \[Turbo\]  \[Full Speed\] \[Manual\] 

以下項目只有在 CPU Fan Profile 設為 \[Standard\]、\[Silent\]、\[Turbo\] 或  \[Manual\] 時才會出現。 

CPU Fan Step Up 

本項目用來設定處理器風扇的加速。等級 0 是速度的瞬時變化。等級越高， 速度變化越慢，也可能導致噪音更小，但這也會導致散熱速度更慢。設定值有： \[Level 0\] \[Level 1\] \[Level 2\] \[Level 3\] \[Level 4\] \[Level 5\] 

CPU Fan Step Down 

本項目用來設定處理器風扇的減速。等級 0 是速度的瞬時變化。等級越高， 速度變化越慢，可能導致噪音持續期間較長。設定值有：\[Level 0\] \[Level 1\]  \[Level 2\] \[Level 3\] \[Level 4\] \[Level 5\]

Pro WS W680-ACE BIOS 使用手冊 61   
CPU Fan Speed Low Limit 

本項目用來設定風扇/水泵的最低速度警告值。當達到這個最低速度時，會出 現警告訊息。若設定為 \[Ignore\]，將不會再出現警告訊息。設定值有：\[Ignore\]  \[200 RPM\] \[300 RPM\] \[400 RPM\] \[500 RPM\] \[600 RPM\] 

以下項目只有在 CPU Fan Profile 設為 \[Manual\] 時才會出現。 

CPU Fan Upper Temperature 

本項目用來設定風扇/水泵溫度上限。當處理器溫度超過 75°C 時，建議將風 扇/水泵的占空比設為 100%。請留意當風扇/水泵占空比不足導致過熱，會造成 處理器效能限制。請使用 \<+\> 與 \<-\> 鍵調整數值。  

風扇/水泵溫度上限不能低於溫度下限。 

CPU Fan Max. Duty Cycle (%)  

本項目用來設定當溫度源溫度高於上限時，風扇的最大占空比。當處理器溫度 超過 75°C 時，建議將風扇/水泵的占空比設為 100%。請留意當風扇/水泵占空 比不足導致過熱，會造成處理器效能限制。使用 \<+\> 或 \<-\> 鍵調整風扇/水泵的 最大工作週期。 

CPU Fan Middle Temperature 

本項目用來設定風扇/水泵的中間溫度。當處理器溫度超過 75°C 時，建議將 風扇/水泵的占空比設為 100%。請留意當風扇/水泵占空比不足導致過熱，會造 成處理器效能限制。使用 \<+\> 與 \<-\> 鍵設定風扇/水泵的中間溫度。 

CPU Fan Middle.Duty Cycle (%) 

本項目用來設定當溫度源溫度高於中間溫度時，風扇的中間占空比。當處理器 溫度超過 75°C 時，建議將風扇/水泵的占空比設為 100%。請留意當風扇/水泵 占空比不足導致過熱，會造成處理器效能限制。請使用 \<+\> 或 \<-\> 鍵調整風扇/ 水泵的中間工作週期。 

CPU Fan Lower Temperature 

本項目用來設定處理器最低溫度。當處理器溫度超過 75°C 時，建議將風扇/ 水泵的占空比設為 100%。請留意當風扇/水泵占空比不足導致過熱，會造成處理 器效能限制。使用 \<+\> 或 \<-\> 鍵調整風扇/水泵溫度的最小值。 

CPU Fan Min. Duty Cycle(%)  

本項目用來設定最小風扇占空比。當處理器溫度超過 75°C 時，建議將風扇/ 水泵的占空比設為 100%。請留意當風扇/水泵占空比不足導致過熱，會造成處理 器效能限制。使用 \<+\> 或 \<-\> 鍵調整風扇/水泵的最小占空比。

62 Pro WS W680-ACE BIOS 使用手冊   
Chassis Fan(s) Configuration 

Chassis Fan 1-3 Q-Fan Control 

本項目用來設定 Chassis Fan 1-3 運作模式。 

\[Auto Detect\] 偵測安裝的風扇/水泵類型並自動切換控制模式。  \[DC Mode\] 在 DC 模式啟動 Q-Fan Control 來使用 3-pin 風扇/水泵。 \[PWM Mode\] 在 PWM 模式啟動 Q-Fan Control 來使用 4-pin 風扇/水 泵。 

Chassis Fan 1-3 Profile 

本項目用來設定風扇/水泵適當的效能。選擇 \[Manual\] 時，當處理器溫 度超過 75°C 時，建議將風扇/水泵的占空比設為 100%。請留意當風扇/ 水泵占空比不足導致過熱，會造成處理器效能限制。設定值有：\[Standard\]  \[Silent\] \[Turbo\] \[Full Speed\] \[Manual\] 

以下項目只有在 CPU Fan 1-3 Profile 設為 \[Standard\]、\[Silent\]、\[Turbo\] 或 \[Manual\] 時才會出現。 

Chassis Fan 1-3 Q-Fan Source 

指 派 的 風 扇 / 水 泵 會 依 選 擇 的 溫 度 來 源 運 作 。 設 定 值 有 ： \[ C P U \]  \[MotherBoard\] \[VRM\] \[Chipset\] \[T\_Sensor\] \[Multiple Sources\] 

Chassis Fan 1-3 Step Up 

本項目用來設定處理器風扇的加速。等級 0 是速度的瞬時變化。等級越 高，速度變化越慢，也可能導致噪音更小，但這也會導致散熱速度更慢。設 定值有：\[Level 0\] \[Level 1\] \[Level 2\] \[Level 3\] \[Level 4\] \[Level 5\] 

Chassis Fan 1-3 Step Down 

本項目用來設定處理器風扇的減速。等級 0 是速度的瞬時變化。等級 越高，速度變化越慢，可能導致噪音持續期間較長。設定值有：\[Level 0\]  \[Level 1\] \[Level 2\] \[Level 3\] \[Level 4\] \[Level 5\] 

Chassis Fan 1-3 Speed Low Limit 

本項目用來設定風扇/水泵的最低速度警告值。當達到這個最低速度時， 會出現警告訊息。若設定為 \[Ignore\]，將不會再出現警告訊息。設定值有： \[Ignore\] \[200 RPM\] \[300 RPM\] \[400 RPM\] \[500 RPM\] \[600 RPM\] 

以下項目只有在 Chassis Fan 1-3 Profile 設為 \[Manual\] 時才會出現。 

Chassis Fan 1-3 Upper Temperature 

本項目用來設定風扇/水泵溫度上限。當處理器溫度超過 75°C 時，建議 將風扇/水泵的占空比設為 100%。請留意當風扇/水泵占空比不足導致過熱， 會造成處理器效能限制。請使用 \<+\> 與 \<-\> 鍵調整數值。  

風扇/水泵溫度上限不能低於溫度下限。

Pro WS W680-ACE BIOS 使用手冊 63   
Chassis Fan 1-3 Max. Duty Cycle (%)  

本項目用來設定當溫度源溫度高於上限時，風扇的最大占空比。當處理器 溫度超過 75°C 時，建議將風扇/水泵的占空比設為 100%。請留意當風扇/ 水泵占空比不足導致過熱，會造成處理器效能限制。使用 \<+\> 或 \<-\> 鍵調整 風扇/水泵的最大工作週期。 

Chassis Fan 1-3 Middle Temperature 

本項目用來設定風扇/水泵的中間溫度。當處理器溫度超過 75°C 時， 建議將風扇/水泵的占空比設為 100%。請留意當風扇/水泵占空比不足導致 過熱，會造成處理器效能限制。使用 \<+\> 與 \<-\> 鍵設定風扇/水泵的中間溫 度。 

Chassis Fan 1-3 Middle.Duty Cycle (%) 

本項目用來設定當溫度源溫度高於中間溫度時，風扇的中間占空比。當處 理器溫度超過 75°C 時，建議將風扇/水泵的占空比設為 100%。請留意當 風扇/水泵占空比不足導致過熱，會造成處理器效能限制。請使用 \<+\> 或 \<-\>  鍵調整風扇/水泵的中間工作週期。 

Chassis Fan 1-3 Lower Temperature 

本項目用來設定處理器最低溫度。當處理器溫度超過 75°C 時，建議將 風扇/水泵的占空比設為 100%。請留意當風扇/水泵占空比不足導致過熱，會 造成處理器效能限制。使用 \<+\> 或 \<-\> 鍵調整風扇/水泵溫度的最小值。 

Chassis Fan 1-3 Min. Duty Cycle(%)  

本項目用來設定最小風扇占空比。當處理器溫度超過 75°C 時，建議將 風扇/水泵的占空比設為 100%。請留意當風扇/水泵占空比不足導致過熱，會 造成處理器效能限制。使用 \<+\> 或 \<-\> 鍵調整風扇/水泵的最小占空比。 

Allow Fan Stop 

本項目用來設定風扇在來源溫度掉到最低溫以下時可以 0% 工作週期運 行。設定值有：\[Disabled\] \[Enabled\] 

Water Pump+ Q-Fan Control 

本項目用來設定水泵運作模式。 

\[Auto Detect\] 偵測安裝的風扇/水泵類型並自動切換控制模式。 \[DC Mode\] 在 DC 模式啟動 Q-Fan Control 來使用 3-pin 風扇/水泵。 \[PWM Mode\] 在 PWM 模式啟動 Q-Fan Control 來使用 4-pin 風扇/水泵。 

Water Pump+ Profile 

本項目用來設定風扇/水泵適當的效能。選擇 \[Manual\] 時，當處理器溫度超 過 75°C 時，建議將風扇/水泵的占空比設為 100%。請留意當風扇/水泵占空比 不足導致過熱，會造成處理器效能限制。設定值有：\[Standard\] \[Silent\] \[Turbo\]  \[Full Speed\] \[Manual\]

64 Pro WS W680-ACE BIOS 使用手冊   
以下項目只有在 Water Pump+ Profile 設為 \[Standard\]、 \[Silent\]、 \[Turbo\] 或 \[Manual\] 時才會出現。 

Water Pump+ Q-Fan Source 

指派的風扇/水泵會依選擇的溫度來源運作。設定值有：\[CPU\] \[MotherBoard\]  \[VRM\] \[Chipset\] \[T\_Sensor\] \[Multiple Sources\] 

Water Pump+ Step Up 

本項目用來設定處理器風扇的加速。等級 0 是速度的瞬時變化。等級越高， 速度變化越慢，也可能導致噪音更小，但這也會導致散熱速度更慢。設定值有： \[Level 0\] \[Level 1\] \[Level 2\] \[Level 3\] \[Level 4\] \[Level 5\] 

Water Pump+ Step Down 

本項目用來設定處理器風扇的減速。等級 0 是速度的瞬時變化。等級越高， 速度變化越慢，可能導致噪音持續期間較長。設定值有：\[Level 0\] \[Level 1\]  \[Level 2\] \[Level 3\] \[Level 4\] \[Level 5\] 

Water Pump+ Speed Low Limit 

本項目用來設定風扇/水泵的最低速度警告值。當達到這個最低速度時，會出 現警告訊息。若設定為 \[Ignore\]，將不會再出現警告訊息。設定值有：\[Ignore\]  \[200 RPM\] \[300 RPM\] \[400 RPM\] \[500 RPM\] \[600 RPM\] 

以下項目只有在 Water Pump+ Profile 設為 \[Manual\] 時才會出現。 

Water Pump+ Upper Temperature 

本項目用來設定風扇/水泵溫度上限。當處理器溫度超過 75°C 時，建議將風 扇/水泵的占空比設為 100%。請留意當風扇/水泵占空比不足導致過熱，會造成 處理器效能限制。請使用 \<+\> 與 \<-\> 鍵調整數值。  

風扇/水泵溫度上限不能低於溫度下限。 

Water Pump+ Max. Duty Cycle (%)  

本項目用來設定當溫度源溫度高於上限時，風扇的最大占空比。當處理器溫度 超過 75°C 時，建議將風扇/水泵的占空比設為 100%。請留意當風扇/水泵占空 比不足導致過熱，會造成處理器效能限制。使用 \<+\> 或 \<-\> 鍵調整風扇/水泵的 最大工作週期。

Pro WS W680-ACE BIOS 使用手冊 65   
Water Pump+ Middle Temperature 

本項目用來設定風扇/水泵的中間溫度。當處理器溫度超過 75°C 時，建議將 風扇/水泵的占空比設為 100%。請留意當風扇/水泵占空比不足導致過熱，會造 成處理器效能限制。使用 \<+\> 與 \<-\> 鍵設定風扇/水泵的中間溫度。 

Water Pump+ Middle. Duty Cycle (%) 

本項目用來設定當溫度源溫度高於中間溫度時，風扇的中間占空比。當處理器 溫度超過 75°C 時，建議將風扇/水泵的占空比設為 100%。請留意當風扇/水泵 占空比不足導致過熱，會造成處理器效能限制。請使用 \<+\> 或 \<-\> 鍵調整風扇/ 水泵的中間工作週期。 

Water Pump+ Lower Temperature 

本項目用來設定處理器最低溫度。當處理器溫度超過 75°C 時，建議將風扇/ 水泵的占空比設為 100%。請留意當風扇/水泵占空比不足導致過熱，會造成處理 器效能限制。使用 \<+\> 或 \<-\> 鍵調整風扇/水泵溫度的最小值。 

Water Pump+ Min. Duty Cycle(%)  

本項目用來設定最小風扇占空比。當處理器溫度超過 75°C 時，建議將風扇/ 水泵的占空比設為 100%。請留意當風扇/水泵占空比不足導致過熱，會造成處理 器效能限制。使用 \<+\> 或 \<-\> 鍵調整風扇/水泵的最小占空比。

66 Pro WS W680-ACE BIOS 使用手冊   
9\. 啟動選單（Boot menu） 

本選單可讓您變更系統啟動裝置與相關功能。 

CSM (Compatibility Support Module)   
本項目用來設定 CSM（相容性支援模組）項目來完全支援各種 VGA、啟動裝置和 附加裝置，藉以獲得更佳的相容性。 

Launch CSM 

\[Enabled\] 為獲得更好的相容性，開啟 CSM 以完全支援非 UEFI 驅動的附 加裝置或 Windows UEFI 模式。 

\[Disabled\] 關閉此功能。 

以下項目只有在 Launch CSM 設為 \[Enabled\] 時才會出現。 

Boot Device Control 

本項用來選擇想要啟動的裝置類型。 

設定值有：\[UEFI and Legacy OPROM\] \[Legacy OPROM only\] \[UEFI only\] Boot from Network Devices   
本項目用來選擇想要執行的網路裝置。 

設定值有：\[Ignore\] \[UEFI only\] \[Legacy only\] 

Boot from Storage Devices 

本項用來選擇想要執行的儲存裝置類型。 

設定值有：\[Ignore\] \[UEFI only\] \[Legacy only\]

Pro WS W680-ACE BIOS 使用手冊 67   
Boot from PCI-E/PCI Expansion Devices 

本項目用來選擇想要執行的 PCIe/PCI 擴充裝置類型。設定值有： \[UEFI  only\] \[Legacy only\]  

Secure Boot 

本項目用來設定 Windows® 安全開機的相關參數以及管理系統金鑰，以提升系統在 開機自我檢測（POST）時的安全性，避免受到未授權的使用者與惡意軟體的危害 

OS Type   
\[Windows UEFI Mode\] 本項目用來選擇安裝的作業系統。執行 Microsoft ® 安全開機檢查。只有在 Windows® UEFI 模式或其他  

Microsoft® 安全開機相容作業系統中開機時選擇此項   
目。 

\[Other OS\] 在 Windows® 非 UEFI 模式中開機時獲得最佳功能。 Microsoft® 安全開機功能僅支援 Windows® UEFI 模 

式。 

Microsoft 安全開機功能僅可在 Windows UEFI 模式下正確運作。 

Secure Boot Mode 

本項目用來選擇安全開機模式。在自定義（Custom）模式下，安全開機策 略變數可以由實際存在的使用者設定，而無需進行完全身份驗證。設定值有： \[Standard\] \[Custom\] 

以下項目只有在 Secure Boot Mode 設為 \[Custom\] 時才會出現。 

Key Management 

Install Default Secure Boot keys 

本項目用來立即載入預設的安全啟動金鑰、平台金鑰（PK）、金鑰加密金鑰 （KEK）、認證簽名資料庫（db）和撤銷簽名資料庫（dbx）。當載入預設的安 全啟動金鑰後，PK 狀態會變為載入模式。 

Clear Secure Boot keys 

本項目只有在載入預設的安全開機金鑰時才會出現。本項目可以清除所有預設 安全開機金鑰。 

Save all Secure Boot variables 

本項目用來將安全開機金鑰儲存至 USB 儲存裝置。 

PK Management 

平台金鑰（PK）鎖定並保護韌體遭到未授權的變更。在進入作業系統前需先 驗證平台金鑰（PK）。  

Save To File 

本項目用來將平台金鑰（PK）儲存至 USB 儲存裝置。 

Set New key 

本項目用來由 USB 儲存裝置載入已下載平台金鑰（PK）。

68 Pro WS W680-ACE BIOS 使用手冊   
Delete key 

本項目用來移除系統中的 PK。當平台金鑰刪除後即無法使用安全啟動金 鑰。 

設定值有：\[Yes\] \[No\] 

PK 檔必須格式化為一個基於時間認證變量的 UEFI 變量構造。 

KEK Management 

KEK（金鑰交換金鑰 \[Key-exchange Key\] 或金鑰註冊金鑰 \[Key-Enrollment  Key\]）用來管理簽名資料庫（db）與撤銷簽名資料庫（dbx）。 

Key-exchange Key（KEK）指的是 Microsoft® Secure Boot Key-Enrollment  Key（KEK）。 

Save to file 

本項目用來將平台金鑰（KEK）儲存至 USB 儲存裝置。 

Set New key 

本項目用來由 USB 儲存裝置載入已下載平台金鑰（KEK）。 

Append Key 

本項目用來由儲存裝置載入附加的 KEK，以管理附加的簽名資料庫（db） 與撤銷簽名資料庫（dbx）。 

Delete key 

本項目用來移除系統中的 KEK。 

設定值有：\[Yes\] \[No\] 

KEK 檔必須格式化為一個基於時間認證變量的 UEFI 變量構造。 

DB Management 

db（認證簽名資料庫）列出可以在單一電腦載入之 UEFI 應用程式、作業系統 載入器與 UEFI 驅動程式的簽名者或圖片影像。 

Save to file 

本項目用來儲存 db 至 USB 儲存裝置。 

Set New key 

本項目用來由 USB 儲存裝置載入已下載的認證簽名資料庫（db）。 Append Key   
本項目用來由儲存裝置載入附加的認證簽名資料庫（db），以安全的載入 更多的圖片影像。 

Delete key 

本項目用來移除系統中的 db 檔。設定值有：\[Yes\] \[No\] 

db 檔必須格式化為一個基於時間認證變量的 UEFI 變量構造。

Pro WS W680-ACE BIOS 使用手冊 69   
DBX Management 

dbx（撤銷簽名資料庫）列出 db 項目中不再被信任且無法被載入之被禁止的 圖片影像。 

Save to file 

本項目用來儲存 dbx 至 USB 儲存裝置。 

Set New key 

本項目用來由 USB 儲存裝置載入已下載的 dbx。 

Append Key 

本項目用來由儲存裝置載入附加的 dbx，以管理附加的簽名資料庫（db） 與撤銷簽名資料庫（dbx）。 

Delete key 

本項目用來移除系統中的 dbx 檔。設定值有：\[Yes\] \[No\] 

dbx 檔必須格式化為一個基於時間認證變量的 UEFI 變量構造。 

Boot Configuration 

Fast Boot 

本項目可以啟用或關閉開機時僅執行最少所需裝置。不會對 BBS 開機選項造 成影響。設定值有：\[Disabled\] \[Enabled\] 

以下項目只有在 Fast Boot 設為 \[Enabled\] 時才會出現。 

Next Boot after AC Power Loss 

\[Normal Boot\] 電源中斷後，在下一次啟動時恢復至正常啟動速度。 \[Normal Boot\] 電源中斷後，在下一次啟動時加快啟動速度。 

Boot Logo Display 

\[Auto\] 依 Windows 要求自動調整開機自我檢測（POST）過程中的開機 畫面。 

\[Full Screen\] 設定在開機自我檢測（POST）過程中的開機畫面為全螢幕。 \[Disabled\] 隱藏開機自我檢測（POST）過程中的開機畫面。 

以下項目只有在 Boot Logo Display 設為 \[Auto\] 或 \[Full Screen\] 時才會 出現。 

Post Delay Time 

本項目可以讓您選擇 POST 的等候時間，以更快進入 BIOS。您可以在正常啟 動下僅執行 POST 延後。設定值有：\[0 sec\] \- \[10 sec\] 

本功能僅支援正常啟動時使用。

70 Pro WS W680-ACE BIOS 使用手冊   
以下項目只有在 Boot Logo Display 設為 \[Disabled\] 時才會出現。 

Post Report 

本項目可以選擇 POST 的等候時間或直到按下 ESC。設定值有：\[1 sec\] \- \[10  sec\] \[Until Press ESC\] 

Boot up NumLock State 

本項目可以選擇鍵盤 NumLock 狀態。設定值有：\[On\] \[Off\] 

Wait For ‘F1’ If Error 

系統開機過程出現錯誤訊息時，本項目可讓系統等待您按下 \<F1\> 鍵確認才會 繼續進行開機程序。設定值有：\[Disabled\] \[Enabled\] 

Option ROM Messages 

\[Force BIOS\] 選購裝置韌體資訊會在開機自我檢測時顯示。 \[Keep Current\] 在開機自我檢測時只顯示華碩開機圖示。  Interrupt 19 Capture   
本項目用來開啟或關閉選購裝置韌體資訊以進行 Interrupt 19 Capture。設定值 有：\[Enabled\] \[Disabled\] 

AMI Native NVMe Driver Support 

本項目用來開啟或關閉 AMI Native NVMe 驅動程式。設定值有：\[Disabled\]  \[Enabled\] 

Boot Sector (MBR/GPT) Recovery Policy 

\[Auto Recovery\] 依照 UEFI 規則。 

\[Local User Control\] 您可以進入設定頁面後選擇 Boot Sector（MBR/ GPT）Recovery Policy，以在下次開機時還原  

MBR/GPT。 

以下項目只有在 Boot Sector (MBR/GPT) Recovery Policy 設為 \[Local  User Control\] 時才會出現。 

Next Boot Recovery Action 

本項目可以選擇下次開機時的（MBR/GPT）還原動作。設定值有：\[Skip\]  \[Recovery\] 

Boot Option Priorities 

本項目讓您自行選擇開機磁碟並排列開機裝置順序。依照 1st、2nd、3rd 順序分 別代表其開機裝置順序，而裝置的名稱將因使用的硬體裝置不同而有所差異。 

• 欲進入 Windows® 安全模式時，請在開機自我檢測（POST）時按下  \<F8\>（Windows® 8 不支援這項功能）。  

• 開機時您可以在 ASUS Logo 出現時按下 \<F8\> 選擇啟動裝置。

Pro WS W680-ACE BIOS 使用手冊 71   
Boot Override 

這些項目會顯示可用的裝置。依照 1st、2nd、3rd 順序分別代表其開機裝置順 序，而裝置的名稱將因使用的硬體裝置不同而有所差異。點選任一裝置可將該將置 設定為開機裝置。

72 Pro WS W680-ACE BIOS 使用手冊   
10\. 工具選單（Tool menu） 

工具選單可以針對特別功能進行設定。請選擇選單中的選項並按下 \<Enter\> 鍵來顯 示子選單。 

BIOS Image Rollback Support   
\[Enabled\] 支援將 BIOS 回滾到上一版本，但是此設定違反了 NIST SP 800-147 的要求。 

\[Disabled\] 僅支援將 BIOS 升級到更新的版本，此設定符合 NIST SP 800-147  的要求。 

Publish HII Resources 

設定值有：\[Disabled\] \[Enabled\] 

FlexKey 

\[Reset\] 重新啟動系統。 

\[DirectKey\] 啟動後直接進入 BIOS。 

Start ASUS EzFlash 

當按下 \<Enter\> 鍵後，本項目可以執行華碩 EzFlash BIOS ROM 工具程式，請參考  使用華碩 EzFlash 更新 BIOS 程式 的說明。 

IPMI Hardware Monitor 

本項目可以按下 \<Enter\> 鍵執行 IPMI 硬體監控。

Pro WS W680-ACE BIOS 使用手冊 73   
10.1 華碩 User Profile 

本選單可以讓您儲存以及載入多種 BIOS 設定檔。  

Load from Profile   
本項目可以讓您載入先前儲存在 BIOS Flash 中的 BIOS 設定。輸入一個儲存在  BIOS 設定中的設定檔編號，然後按下 \<Enter\> 鍵並 選擇 Yes 來載入檔案。 

• 當進行 BIOS 升級時，請勿關閉或重新啟動系統以免造成系統開機失 敗。 

• 建議您只在相同的記憶體/處理器設定與相同的 BIOS 版本狀態下， 更新 BIOS 程式。 

Profile Name 

本項目用來輸入設定檔名稱。  

Save to Profile 

本項目可以讓您儲存目前的 BIOS 檔案至 BIOS Flash 中，並建立一個設定檔。從  1 至 8 選擇一個設定檔編號並輸入該編號，然後按下 \<Enter\> 鍵，接著選擇 Yes。

74 Pro WS W680-ACE BIOS 使用手冊   
10.2 華碩 SPD 資訊（ASUS SPD Information） 

本選單顯示記憶體插槽的相關資訊。 

10.3 華碩 Armoury Crate 

本項目用來讓您在 Windows® 作業系統中開啟或關閉 Armoury Crate 應用程式的下 載與安裝。Armoury Crate 應用程式可以幫助您管理與下載主機板最新的驅動程式與 公用程式。 

Download & Install ARMOURY CRATE app 

設定值有：\[Disabled\] \[Enabled\]

Pro WS W680-ACE BIOS 使用手冊 75   
11\. IPMI 選單（IPMI menu） 

本選單可以設定 IPMI。 

OS Watchdog Timer   
本項目用來開啟或關閉 BIOS 計時器，開啟後僅能透過管理軟體進行關閉，以幫 助決定作業系統是否成功載入或依循 OS Boot Watchdog Timer 政策。設定值有： \[Enabled\] \[Disabled\] 

以下項目只有在 OS Watchdog Timer 設為 \[Enabled\] 時才會出現。 

OS Wtd Timer Timeout 

本項目提供您設定 OS Boot Watchdog Timer（開機關門狗計時器）的時間長短。 設定值有：\[1\] \- \[30\] 

OS Wtd Timer Policy 

本項目可以在 OS Boot Watchdog Timer 到期後，提供您設定系統應該如何回應。 設定值有：\[Do Nothing\] \[Reset\] \[Power Down\] \[Power Cycle\] 

MLED light Synchronizing 

本項目可以設定同步訊息指示燈與 IPMI 擴充卡上左側 RJ45 指示燈。設定值有： \[Disabled\] \[Enabled\]

76 Pro WS W680-ACE BIOS 使用手冊   
BMC\_LED light synchronizing 

本項目可以設定同步 BMC 指示燈與 IPMI 擴充卡上右側 RJ45 指示燈。設定值 有：\[Disabled\] \[Enabled\] 

In-Band driver type 

設定值有： \[Windows\] \[Linux\] 

11.1 系統事件記錄（System Event Log） 

提供您變更 SEL 事件記錄設定。 

變更選項後需重新開機才會生效。

Erase SEL   
本項目用來選擇如何清除 SEL。設定值有：\[No\] \[Yes, On next reset\] \[Yes, On  every reset\] 

Pro WS W680-ACE BIOS 使用手冊 77   
11.2 BMC 網路設定（BMC network configuration） 

此選單中的選項用來設定 BMC 網路參數。

Configure IPV4 support 

LAN channel 1 

Configuration Address source 

選擇設定的網路通道的參數為靜態或動態（透過 BIOS 或 BMC）。\[Unspecified\]  選項在 BIOS 階段不會修改任何 BMC 網路參數。設定值有：\[Unspecified\] \[Static\]  \[DynamicBmcDhcp\] 

以下項目只有在 Configuration Address source 設為 \[Static\] 時才會出 現。 

Station IP address 

本項目用來設定 IP 位址。 

Subnet mask 

本項目用來設定子網路遮罩。建議您設定與作業系統的網路相同的子網路遮罩。 

Router IP Address 

本項目可以設定路由器 IP 位址。 

Router MAC Address 

本項目可以設定路由器 MAC 位址。 

78 Pro WS W680-ACE BIOS 使用手冊   
Configure IPV6 support 

LAN channel 1 

IPV6 support  

本項目用來開啟或關閉 IPV6 支援。設定值有：\[Enabled\] \[Disabled\] 以下項目只有在 IPV6 support 設為 \[Enabled\] 時才會出現。 

Configuration Address source 

選擇設定的網路通道的參數為靜態或動態。\[Unspecified\] 選項在 BIOS 階段不會 修改任何 BMC 網路參數。 設定值有：\[Unspecified\] \[Static\] \[DynamicBmcDhcp\] 

以下項目只有在 Configuration Address source 設為 \[Static\] 時才會出 現。 

Station IPV6 address 

本項目用來設定 IPV6 位址。 

Prefix Length 

本項目可以設定前置位元長度（最多為 128）。 

Configuration Router Lan1 Address source 

選擇設定的網路通道的參數為靜態或動態。\[Unspecified\] 選項在 BIOS  階段不會修改任何 BMC 網路參數。 設定值有：\[Unspecified\] \[Static\]  \[DynamicBmcDhcp\] 

以下項目只有在 Configuration Router Lan1 Address source 設為 \[Static\] 時才會出現。 

IPV6 Router1 IP address 

本項目可以設定 IPV6 Router1 IP 位址。 

IPV6 Router1 Prefix Length Lan1 

本項目可以設定前置位元長度（最多為 128）。 

IPV6 Router1 Prefix Value Lan1 

本項目可以變更 IPV6 路由器 1 前置位元數值。

Pro WS W680-ACE BIOS 使用手冊 79   
11.3 檢視系統事件記錄（View System Event Log） 

本選單可以檢視系統事件記錄。

80 Pro WS W680-ACE BIOS 使用手冊 
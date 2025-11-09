**Pro WS**  

**W680-ACE Series** 

**BIOS Manual**   
**Motherboar~~d~~**  
E23138   
Revised Edition   
December 2023 

**Copyright© 2023 ASUSTeK COMPUTER INC. All Rights Reserved.**   
No part of this manual, including the products and software described in it, may be reproduced,  transmitted, transcribed, stored in a retrieval system, or translated into any language in any form or by  any means, except documentation kept by the purchaser for backup purposes, without the express  written permission of ASUSTeK COMPUTER INC. (“ASUS”).   
Product warranty or service will not be extended if: (1) the product is repaired, modified or altered, unless  such repair, modification of alteration is authorized in writing by ASUS; or (2) the serial number of the  product is defaced or missing.   
ASUS PROVIDES THIS MANUAL “AS IS” WITHOUT WARRANTY OF ANY KIND, EITHER EXPRESS  OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE IMPLIED WARRANTIES OR CONDITIONS OF  MERCHANTABILITY OR FITNESS FOR A PARTICULAR PURPOSE. IN NO EVENT SHALL ASUS, ITS  DIRECTORS, OFFICERS, EMPLOYEES OR AGENTS BE LIABLE FOR ANY INDIRECT, SPECIAL,  INCIDENTAL, OR CONSEQUENTIAL DAMAGES (INCLUDING DAMAGES FOR LOSS OF PROFITS,  LOSS OF BUSINESS, LOSS OF USE OR DATA, INTERRUPTION OF BUSINESS AND THE LIKE),  EVEN IF ASUS HAS BEEN ADVISED OF THE POSSIBILITY OF SUCH DAMAGES ARISING FROM  ANY DEFECT OR ERROR IN THIS MANUAL OR PRODUCT.   
SPECIFICATIONS AND INFORMATION CONTAINED IN THIS MANUAL ARE FURNISHED FOR  INFORMATIONAL USE ONLY, AND ARE SUBJECT TO CHANGE AT ANY TIME WITHOUT NOTICE,  AND SHOULD NOT BE CONSTRUED AS A COMMITMENT BY ASUS. ASUS ASSUMES NO  RESPONSIBILITY OR LIABILITY FOR ANY ERRORS OR INACCURACIES THAT MAY APPEAR IN  THIS MANUAL, INCLUDING THE PRODUCTS AND SOFTWARE DESCRIBED IN IT. Products and corporate names appearing in this manual may or may not be registered trademarks or  copyrights of their respective companies, and are used only for identification or explanation and to the  owners’ benefit, without intent to infringe.  
**Contents** 

**1---. Knowing BIOS ............................................................................................... 5 2---. BIOS setup program ..................................................................................... 6 3---. Managing and updating your BIOS ............................................................. 7** 

3.1 ASUS CrashFree BIOS 3 utility...................................................... 7 3.2 ASUS EzFlash Utility...................................................................... 8 **4---. BIOS menu screen ........................................................................................ 9** 4.1 Menu bar........................................................................................ 9 4.2 Menu items................................................................................... 10 4.3 Submenu items ............................................................................ 10 4.4 Navigation keys............................................................................ 10 4.5 General help................................................................................. 10 4.6 Configuration fields ...................................................................... 10 4.7 Pop-up window............................................................................. 10 4.8 Scroll bar...................................................................................... 10 **5---. Main menu ................................................................................................... 11 6---. Ai Tweaker menu......................................................................................... 14 7---. Advanced menu .......................................................................................... 35** 7.1 Platform Misc Configuration......................................................... 36 7.2 CPU Configuration ....................................................................... 37 7.3 System Agent (SA) Configuration ................................................ 42 7.4 PCH Configuration ....................................................................... 44 7.5 PCH Storage Configuration.......................................................... 45 7.6 PCH-FW Configuration ................................................................ 46 7.7 AMT Configuration ....................................................................... 46 7.8 Thunderbolt(TM) Configuration.................................................... 48 7.9 Trusted Computing....................................................................... 50 7.10 Redfish Host Interface Settings.................................................... 51 7.11 Serial Port Console Redirection................................................... 52 7.12 Intel TXT Information.................................................................... 54 7.13 PCI Subsystem Settings .............................................................. 54 7.14 USB Configuration ....................................................................... 55 7.15 Network Stack Configuration........................................................ 56 7.16 NVMe Configuration..................................................................... 56 7.17 HDD/SSD SMART Information .................................................... 57 7.18 APM Configuration....................................................................... 57 7.19 Onboard Devices Configuration................................................... 58 7.20 Intel(R) Rapid Storage Technology.............................................. 60

Pro WS W680-ACE Series BIOS Manual 3   
**8---. Monitor menu .............................................................................................. 61 9---. Boot menu ................................................................................................... 68 10---. Tool menu.................................................................................................... 74** 

10.1 ASUS User Profile........................................................................ 75 10.2 ASUS SPD Information................................................................ 76 10.3 ASUS Armoury Crate................................................................... 76 

**11---. IPMI menu .................................................................................................... 77** 11.1 System Event Log........................................................................ 78 11.2 BMC network configuration.......................................................... 79 11.3 View System Event Log ............................................................... 81 **12---. Exit menu..................................................................................................... 82**

4 Pro WS W680-ACE Series BIOS Manual   
**BIOS Setup** 

**1---. Knowing BIOS**

The new ASUS UEFI BIOS is a Unified Extensible Interface that complies with UEFI  architecture, offering a user-friendly interface that goes beyond the traditional keyboard only BIOS controls to enable a more flexible and convenient mouse input. You can easily  navigate the new UEFI BIOS with the same smoothness as your operating system. The  term “BIOS” in this user manual refers to “UEFI BIOS” unless otherwise specified. 

BIOS (Basic Input and Output System) stores system hardware settings such as storage  device configuration, overclocking settings, advanced power management, and boot  device configuration that are needed for system startup in the motherboard CMOS. In  normal circumstances, the default BIOS settings apply to most conditions to ensure  optimal performance. **DO NOT change the default BIOS settings** except in the following  circumstances:  

• An error message appears on the screen during the system bootup and requests you  to run the BIOS Setup. 

• You have installed a new system component that requires further BIOS settings or  update. 

Inappropriate BIOS settings may result to instability or boot failure. **We strongly  recommend that you change the BIOS settings only with the help of a trained  service personnel**. 

• When downloading or updating the BIOS file for your motherboard, rename it as  **XXXXX.CAP** or launch the **BIOSRenamer.exe** application to automatically rename  the file. The name of the CAP file varies depending on models. Refer to the user  manual that came with your motherboard for the name. 

• The screenshots in this manual are for reference only, please refer to the latest BIOS  version for settings and options. 

• BIOS settings and options may vary due to different BIOS release versions or CPU  installed. Please refer to the latest BIOS version for settings and options. 

Pro WS W680-ACE Series BIOS Manual 5   
**2---. BIOS setup program** 

Use the BIOS Setup to update the BIOS or configure its parameters. The BIOS screen  include navigation keys and brief onscreen help to guide you in using the BIOS Setup  program. 

**Entering BIOS at startup** 

To enter BIOS Setup at startup, press ---<Delete---> or ---<F2---> during the Power-On Self Test  (POST). If you do not press ---<Delete---> or ---<F2--->, POST continues with its routines. 

**Entering BIOS Setup after POST** 

To enter BIOS Setup after POST: 

• Press ---<Ctrl--->+---<Alt--->+---<Delete---> simultaneously. 

• Press the reset button on the system chassis. 

• Press the power button to turn the system off then back on. Do this option only if you  failed to enter BIOS Setup using the first two options. 

After doing either of the three options, press ---<Delete---> key to enter BIOS.

• The BIOS setup screens shown in this section are for reference purposes only, and  may not exactly match what you see on your screen. 

• If the system becomes unstable after changing any BIOS setting, load the default  settings to ensure system compatibility and stability. Select the **Load Optimized  Defaults** item under the **Exit** menu or press hotkey **---<F5--->**. See section **Exit menu** for  details. 

• If the system fails to boot after changing any BIOS setting, try to clear the CMOS  and reset the motherboard to the default value. See your motherboard manual for  information on how to erase the RTC RAM. 

• The BIOS setup program does not support Bluetooth devices. 

6 Pro WS W680-ACE Series BIOS Manual   
**3---. Managing and updating your BIOS** 

The following utilities allow you to manage and update the motherboard Basic Input/Output  System (BIOS) setup: 

1---. ASUS CrashFree BIOS 3 

To recover the BIOS using a bootable USB flash disk drive when the BIOS file fails or  gets corrupted. 

2---. ASUS EzFlash 

Updates the BIOS using a USB flash disk. 

Refer to the corresponding sections for details on these utilities. 

**3.1 ASUS CrashFree BIOS 3 utility** 

The ASUS CrashFree BIOS 3 is an auto recovery tool that allows you to restore the BIOS  file when it fails or gets corrupted during the updating process. You can update a corrupted  BIOS file using a USB flash drive that contains the updated BIOS file. 

Prepare a USB flash drive containing the updated motherboard BIOS before using this  utility. 

**Recovering the BIOS from a USB flash drive** 

To recover the BIOS from a USB flash drive: 

1---. Insert the USB flash drive with the original or updated BIOS file to one USB port on  the system. 

2---. The utility will automatically recover the BIOS. It resets the system when the BIOS  recovery finished. 

DO NOT shut down or reset the system while recovering the BIOS---! Doing so would cause  system boot failure---! 

The recovered BIOS may not be the latest BIOS version for this motherboard. Visit the  ASUS website at www.asus.com to download the latest BIOS file.

Pro WS W680-ACE Series BIOS Manual 7   
**3.2 ASUS EzFlash Utility** 

The ASUS EzFlash Utility feature allows you to update the BIOS using a USB flash disk  without having to use a DOS‑based utility. 

Download the latest BIOS from the ASUS website at www.asus.com before using this  utility. 

The succeeding BIOS screens are for reference only. The actual BIOS screen displays  may not be the same as shown. 

To update the BIOS using EzFlash Utility: 

1---. Insert the USB flash disk that contains the latest BIOS file to the USB port. 

2---. Enter the BIOS setup program. Go to the **Tool** menu to select **Start EzFlash** and  press ---<Enter---> to enable it. 

**ASUSTek. EzFlash Utility** 

 **Current Platform**   
 **New Platform**   
**Platform : Pro-WS-W680-ACE-IPMI Platform : Pro-WS-W680-ACE-IPMI**   
**Version : 0101**   
**Build Date :09/14/2022**   
**Version : 0105**   
**Build Date :10/24/2022**

**FS0 Pro-WS-W680-ACE-IPMI-ASUS-0104.cap 33558528 Bytes ---[Up/Down/Left/Right---]:Switch ---[Enter---]:Choose ---[q---]:Exit** 

3---. Press the Left arrow key to switch to the **Drive** field. 

4---. Press the Up/Down arrow keys to find the USB flash disk that contains the latest BIOS  then press ---<Enter--->.  

5---. Press the Right arrow key to switch to the **Folder Info** field. 

6---. Press the Up/Down arrow keys to find the BIOS file then press ---<Enter--->.  7---. Reboot the system when the update process is done. 

8 Pro WS W680-ACE Series BIOS Manual   
**4---. BIOS menu screen**   
**Menu items Menu bar Configuration fields General help ![][image1]Navigation keys** 

**4.1 Menu bar** 

The menu bar on top of the screen has the following main items: 

**Main** For changing the basic system configuration 

**Ai Tweaker** For changing the overclocking settings 

**Advanced** For changing the advanced system settings 

**Monitor** For displaying the system temperature, power status, and changing  the fan settings 

**Boot** For changing the system boot configuration 

**Tool** For configuring options for special functions 

**IPMI** For configuring IPMI options 

**Exit** For selecting the save & exit options 

To select an item on the menu bar, press the right or left arrow key on the keyboard until the  desired item is highlighted.

Pro WS W680-ACE Series BIOS Manual 9   
**4.2 Menu items** 

The highlighted item on the menu bar displays the specific items for that menu. For example,  selecting Main shows the Main menu items. The other items on the menu bar have their  respective menu items. 

**4.3 Submenu items** 

A solid triangle before each item on any menu screen means that the item has a submenu.  To display the submenu, select the item and press ---<Enter--->. 

![][image2]**4.4 Navigation keys**   
At the bottom right corner of a menu screen are the navigation keys for the BIOS setup  program. Use the navigation keys to select items in the menu and change the settings. 

**4.5 General help** 

At the top right corner of the menu screen is a brief description of the selected item. 

**4.6 Configuration fields** 

These fields show the values for the menu items. If an item is user-configurable, you can  change the value of the field opposite the item. You cannot select an item that is not user configurable. A configurable field is enclosed in brackets, and is highlighted when selected.  To change the value of a field, select it and press ---<Enter---> to display a list of options. 

**4.7 Pop-up window** 

Select a menu item and press ---<Enter---> to display a pop-up window with the configuration  options for that item. 

**4.8 Scroll bar** 

A scroll bar appears on the right side of a menu screen when there are items that do not fit  on the screen. Press the Up/Down arrow keys or ---<Page Up---> /---<Page Down---> keys to display  the other items on the screen.

10 Pro WS W680-ACE Series BIOS Manual   
**5---. Main menu** 

The Main menu screen appears when you enter the Advanced Mode of the BIOS Setup  program. The Main menu provides you an overview of the basic system information, and  allows you to set the system date, time, language, and security settings.

![][image3]Pro WS W680-ACE Series BIOS Manual 11   
**Security** 

The Security menu items allow you to change the system security settings.![][image4]

• If you have forgotten your BIOS password, erase the CMOS Real Time Clock (RTC)  RAM to clear the BIOS password. See the motherboard for information on how to  erase the RTC RAM via the Clear CMOS jumper. 

• The Administrator or User Password items on top of the screen show the default **---[Not  Installed---]**. After you set a password, these items show **---[Installed---]**. 

**Administrator Password** 

If you have set an administrator password, we recommend that you enter the administrator  password for accessing the system. Otherwise, you might be able to see or change only  selected fields in the BIOS setup program. 

**To set an administrator password:** 

1---. Select the **Administrator Password** item and press ---<Enter--->. 

2---. From the **Create New Password** box, key in a password, then press ---<Enter--->. 3---. Re-type to confirm the password then select **OK**.   
**To change an administrator password:** 

1---. Select the **Administrator Password** item and press ---<Enter--->. 

2---. From the **Enter Current Password** box, key in the current password, then press  ---<Enter--->. 

3---. From the **Create New Password** box, key in a new password, then press ---<Enter--->. 4---. Re-type to confirm the password then select **OK**.   
To clear the administrator password, follow the same steps as in changing an administrator  password, but leave other fields blank then select **OK** to continue. After you clear the  password, the **Administrator Password** item on top of the screen shows **---[Not Installed---]**. 

12 Pro WS W680-ACE Series BIOS Manual   
**User Password** 

If you have set a user password, you must enter the user password for accessing the  system. The User Password item on top of the screen shows the default **---[Not Installed---]**.  After you set a password, this item shows **---[Installed---]**. 

**To set a user password:** 

1---. Select the **User Password** item and press ---<Enter--->. 

2---. From the **Create New Password** box, key in a password, then press ---<Enter--->. 3---. Re-type to confirm the password then select **OK**.   
**To change a user password:** 

1---. Select the **User Password** item and press ---<Enter--->. 

2---. From the **Enter Current Password** box, key in the current password, then press  ---<Enter--->. 

3---. From the **Create New Password** box, key in a new password, then press ---<Enter--->. 4---. Re-type to confirm the password then select **OK**.   
To clear the user password, follow the same steps as in changing a user password, but  leave other fields blank then select **OK** to continue. After you clear the password, the **User  Password** item on top of the screen shows **---[Not Installed---]**.

Pro WS W680-ACE Series BIOS Manual 13   
**6---. Ai Tweaker menu** 

The Ai Tweaker menu items allow you to configure overclocking-related items. 

Be cautious when changing the settings of the Ai Tweaker menu items. Incorrect field  values can cause the system to malfunction. 

The configuration options for this section vary depending on the CPU and DIMM model  you installed on the motherboard. 

Scroll down to display other BIOS items. 

![][image5]  
**Ai Overclock Tuner** 

---[Auto---] Loads the optimal settings for the system. 

---[AEMP---] Loads the memory parameters profile which is optimized by ASUS if no  DIMM profiles are detected. 

---[XMP I---] Load the DIMM’s default XMP memory timings (CL, TRCD, TRP, TRAS)  and other memory parameters optimized by ASUS. 

---[XMP II---] Load the DIMM’s complete default XMP profile. Load the memory  parameters profile optimized by ASUS if no DIMM profiles detected. 

The configuration options for this item depends on the DIMM installed.

14 Pro WS W680-ACE Series BIOS Manual   
The following item appears only when **Ai Overclock Tuner** is set to **---[AEMP---]**. 

**AEMP** 

Allows you to select your ASUS Enhanced Memory Profile (AEMP). Each profile has its own  DRAM frequency, timing and voltage. 

The following item appears only when **Ai Overclock Tuner** is set to **---[XMP I---]** or **---[XMP II---]**. 

**XMP** 

Allows you to select your XMP Profile. Each profile has its own DRAM frequency, timing and  voltage. 

**Intel(R) Adaptive Boost Technology** 

Allows you to enable or disable IABT to improve performance by allowing higher multi-core  turbo frequencies. Operating within system power and temperature specifications when  current, power and thermal headroom exists, please ensure quality cooling for the CPU  before enabling the ABT function. 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] 

**ASUS Performance Enhancement 3.0** 

---[Disabled---] Keep to Intel default settings (Intel stock CPU core  frequency and power limits). 

---[Enabled---] Enable ASUS optimized CPU settings (unlock  power limits to increase CPU performance). 

---[Enabled(limit CPU temp. at 90°C)---] Enable ASUS optimized CPU settings (unlock  power limits) and limit CPU temperature at 90°C for  

better performance balance. 

Actual settings may vary depending on different models. 

**BCLK Frequency : DRAM Frequency Ratio** 

---[Auto---] The BCLK frequency to DRAM frequency ratio will be set to the optimized  setting. 

---[100:133---] The BCLK frequency to DRAM frequency ratio will be set to 100:133. ---[100:100---] The BCLK frequency to DRAM frequency ratio will be set to 100:100. 

**Memory Controller : DRAM Frequency Ratio** 

BCLK Frequency: DRAM Frequency Ratio of 100:133 tends to overclock better and 1:2  Memory Controller: DRAM Frequency Ratio only works with even numbered DRAM Ratios  and not odd numbered ratios. 

Configuration options: ---[Auto---] ---[1:1---] ---[1:2---] ---[1:4---]

Pro WS W680-ACE Series BIOS Manual 15   
**DRAM Frequency** 

Allows you to set the memory operating frequency. The configurable options vary with the  BCLK (base clock) frequency setting. Select the auto mode to apply the optimized setting. Configuration options: ---[Auto---] ---[DDR5-800MHz---] ---- ---[DDR5-13333MHz---] 

The configuration options for this item vary depending on the DIMM model you installed on  the motherboard. 

The frequency ratios in grey are not recommended, use BCLK ---+ ratios in white to reach  your target frequency if needed. 

**Performance Core Ratio** 

---[Auto---] The system will adjust all Performance core ratios automatically. ---[Sync All Cores---] Configure a core ratio limit to synchronize all Performance cores. ---[By Core Usage---] Configure the ratio limits for active cores depending on how many  Performance cores are being utilized. 

The following item appears only when **Performance Core Ratio** is set to **---[Sync All  Cores---]**. 

**ALL-Core Ratio Limit** 

Enter **---[Auto---]** to apply the CPU default Turbo Ratio setting or manually assign a Core ratio  limit to synchronize all cores. Use the ---<+---> or ---<----> to adjust the value. 

Configuration options: ---[Auto---] ---[8---] ---- ---[45---] 

The following items appear only when **Performance Core Ratio** is set to **---[By Core  Usage---]**. 

**1-Core Ratio Limit / 2-Core Ratio Limit / 3-Core Ratio Limit / 4-Core Ratio  Limit / 5-Core Ratio Limit / 6-Core Ratio Limit / 7-Core Ratio Limit / 8-Core  Ratio Limit** 

The N-core ratio limit must be higher than or equal to the (N+1)-core ratio limit. (N stands for  the number of CPU cores) The core ratio limit cannot be set to **---[Auto---]** when the core number  is lower than N. The biggest core’s ratio limit must be lower than or equal to the second  biggest core’s ratio limit. Use the ---<+---> or ---<----> to adjust the value. 

Configuration options: ---[Auto---] ---[21---] ---- ---[49---] 

**Efficient Core Ratio** 

---[Auto---] The system will adjust all Efficient core ratios automatically. ---[Sync All Cores---] Configure a core ratio limit to synchronize all Efficient cores. ---[By Core Usage---] Configure the ratio limits for active cores depending on how many Efficient  cores are being utilized. 

The following item appears only when **Efficient Core Ratio** is set to **---[Sync All Cores---]**.

16 Pro WS W680-ACE Series BIOS Manual   
**ALL-Core Ratio Limit** 

Allows you to set the ratio limit for Efficient cores when N Efficient cores are loaded. Use the  ---<+---> or ---<----> to adjust the value. 

Configuration options: ---[Auto---] ---[8---] ---- ---[34---] 

The following items appear only when **Performance Core Ratio** is set to **---[By Core  Usage---]**. 

**Efficient 1-Core Ratio Limit / Efficient 2-Core Ratio Limit / Efficient 3-Core  Ratio Limit / Efficient 4-Core Ratio Limit** 

Configuration options: ---[Auto---] ---[16---] ---- ---[36---] 

**AVX Related Controls** 

**AVX2** 

Allows you to enable or disable the AVX 2 Instructions.   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] 

**DRAM Timing Control** 

The sub-items in this menu allow you to set the DRAM timing control features. Use the  ---<+---> and ---<----> keys to adjust the value. To restore the default setting, type **---[Auto---]** using the  keyboard and press the ---<Enter---> key. You can also select various **Memory Presets** to load  settings suitably tuned for some memory modules. 

Changing the values in this menu may cause the system to become unstable---! If this  happens, revert to the default settings. 

**Primary Timings** 

**DRAM CAS---# Latency** 

Configuration options: ---[Auto---] ---[2---] ---- ---[126---] 

**DRAM RAS---# to CAS---# Delay** 

Configuration options: ---[Auto---] ---[0---] ---- ---[255---] 

**DRAM RAS---# PRE Time** 

Configuration options: ---[Auto---] ---[0---] ---- ---[255---] 

**DRAM RAS---# ACT Time** 

Configuration options: ---[Auto---] ---[1---] ---- ---[511---] 

**DRAM Command Rate** 

Configuration options: ---[Auto---] ---[1N---] ---[2N---] ---[3N---] ---[N:1---] 

The following item appears only when **DRAM Command Rate** is set to **---[N:1---]**. 

**N to 1 ratio** 

Number of bubbles between each valid command cycle. 

Configurations: ---[1---] ---- ---[7---]

Pro WS W680-ACE Series BIOS Manual 17   
**Secondary Timings** 

**DRAM RAS---# to RAS---# Delay L** 

Configuration options: ---[Auto---] ---[1---] ---- ---[63---] 

**DRAM RAS---# to RAS---# Delay S** 

Configuration options: ---[Auto---] ---[1---] ---- ---[127---] **DRAM REF Cycle Time**   
Configuration options: ---[Auto---] ---[1---] ---- ---[65535---] **DRAM REF Cycle Time 2**   
Configuration options: ---[Auto---] ---[1---] ---- ---[65535---] **DRAM REF Cycle Time Same Bank**   
Configuration options: ---[Auto---] ---[0---] ---- ---[2047---] **DRAM Refresh Interval**   
Configuration options: ---[Auto---] ---[1---] ---- ---[262143---] **DRAM WRITE Recovery Time**   
Configuration options: ---[Auto---] ---[1---] ---- ---[234---] **DRAM READ to PRE Time**   
Configuration options: ---[Auto---] ---[1---] ---- ---[255---] **DRAM FOUR ACT WIN Time**   
Configuration options: ---[Auto---] ---[1---] ---- ---[511---] **DRAM WRITE to READ Delay**   
Configuration options: ---[Auto---] ---[1---] ---- ---[31---] 

**DRAM WRITE to READ Delay L** 

Configuration options: ---[Auto---] ---[1---] ---- ---[31---] 

**DRAM WRITE to READ Delay S** 

Configuration options: ---[Auto---] ---[1---] ---- ---[31---] 

**DRAM CKE Minimum Pulse Width** 

Configuration options: ---[Auto---] ---[0---] ---- ---[127---] **DRAM Write Latency**   
Configuration options: ---[Auto---] ---[1---] ---- ---[255---] **Skew Control** 

**DDRCRCOMPCTL0/1/2** 

**Ctl0 dqvrefup**   
Configuration options: ---[Auto---] ---[0---] ---- ---[255---]   
v Ctl0 dqvrefdn   
Configuration options: ---[Auto---] ---[0---] ---- ---[255---] 

**Ctl0 dqodtvrefup**   
Configuration options: ---[Auto---] ---[0---] ---- ---[255---] 

**Ctl0 dqodtvrefdn** 

Configuration options: ---[Auto---] ---[0---] ---- ---[255---]

18 Pro WS W680-ACE Series BIOS Manual   
**Ctl1 cmdvrefup**   
Configuration options: ---[Auto---] ---[0---] ---- ---[255---] 

**Ctl1 ctlvrefup**   
Configuration options: ---[Auto---] ---[0---] ---- ---[255---] 

**Ctl1 clkvrefup**   
Configuration options: ---[Auto---] ---[0---] ---- ---[255---] 

**Ctl1 ckecsvrefup**   
Configuration options: ---[Auto---] ---[0---] ---- ---[255---] 

**Ctl2 cmdvrefdn**   
Configuration options: ---[Auto---] ---[0---] ---- ---[255---] 

**Ctl2 ctlvrefdn**   
Configuration options: ---[Auto---] ---[0---] ---- ---[255---] 

**Ctl2 clkvrefdn**   
Configuration options: ---[Auto---] ---[0---] ---- ---[255---] 

**Tc Odt Control** 

**ODT---_READ---_DURATION**   
Configuration options: ---[Auto---] ---[0---] ---- ---[15---] 

**ODT---_READ---_DELAY**   
Configuration options: ---[Auto---] ---[0---] ---- ---[15---] 

**ODT---_WRITE---_DURATION**   
Configuration options: ---[Auto---] ---[0---] ---- ---[15---] 

**ODT---_WRITE---_DELAY**   
Configuration options: ---[Auto---] ---[0---] ---- ---[15---] 

**MC0 Dimm0 / MC0 Dimm1 / MC1 Dimm0 / MC1 Dimm1** 

**DQ RTT WR** 

Configuration options: ---[Auto---] ---[0 DRAM Clock---] ---[34 DRAM Clock---] ---[40 DRAM Clock---] ---[48  DRAM Clock---] ---[60 DRAM Clock---] ---[80 DRAM Clock---] ---[120 DRAM Clock---] ---[240 DRAM Clock---] **DQ RTT NOM RD**   
Configuration options: ---[Auto---] ---[0 DRAM Clock---] ---[34 DRAM Clock---] ---[40 DRAM Clock---] ---[48  DRAM Clock---] ---[60 DRAM Clock---] ---[80 DRAM Clock---] ---[120 DRAM Clock---] ---[240 DRAM Clock---] **DQ RTT NOM WR**   
Configuration options: ---[Auto---] ---[0 DRAM Clock---] ---[34 DRAM Clock---] ---[40 DRAM Clock---] ---[48  DRAM Clock---] ---[60 DRAM Clock---] ---[80 DRAM Clock---] ---[120 DRAM Clock---] ---[240 DRAM Clock---] **DQ RTT PARK**   
Configuration options: ---[Auto---] ---[0 DRAM Clock---] ---[34 DRAM Clock---] ---[40 DRAM Clock---] ---[48  DRAM Clock---] ---[60 DRAM Clock---] ---[80 DRAM Clock---] ---[120 DRAM Clock---] ---[240 DRAM Clock---] **DQ RTT PARK DQS**   
Configuration options: ---[Auto---] ---[0 DRAM Clock---] ---[34 DRAM Clock---] ---[40 DRAM Clock---] ---[48  DRAM Clock---] ---[60 DRAM Clock---] ---[80 DRAM Clock---] ---[120 DRAM Clock---] ---[240 DRAM Clock---] **GroupA CA ODT**   
Configuration options: ---[Auto---] ---[0 DRAM Clock---] ---[40 DRAM Clock---] ---[60 DRAM Clock---] ---[80  DRAM Clock---] ---[120 DRAM Clock---] ---[240 DRAM Clock---] ---[480 DRAM Clock---] **GroupA CS ODT**   
Configuration options: ---[Auto---] ---[0 DRAM Clock---] ---[40 DRAM Clock---] ---[60 DRAM Clock---] ---[80  DRAM Clock---] ---[120 DRAM Clock---] ---[240 DRAM Clock---] ---[480 DRAM Clock---]

Pro WS W680-ACE Series BIOS Manual 19   
**GroupA CK ODT** 

Configuration options: ---[Auto---] ---[0 DRAM Clock---] ---[40 DRAM Clock---] ---[60 DRAM Clock---] ---[80  DRAM Clock---] ---[120 DRAM Clock---] ---[240 DRAM Clock---] ---[480 DRAM Clock---] 

**GroupB CA ODT** 

Configuration options: ---[Auto---] ---[0 DRAM Clock---] ---[40 DRAM Clock---] ---[60 DRAM Clock---] ---[80  DRAM Clock---] ---[120 DRAM Clock---] ---[240 DRAM Clock---] ---[480 DRAM Clock---] 

**GroupB CS ODT** 

Configuration options: ---[Auto---] ---[0 DRAM Clock---] ---[40 DRAM Clock---] ---[60 DRAM Clock---] ---[80  DRAM Clock---] ---[120 DRAM Clock---] ---[240 DRAM Clock---] ---[480 DRAM Clock---] 

**GroupB CK ODT** 

Configuration options: ---[Auto---] ---[0 DRAM Clock---] ---[40 DRAM Clock---] ---[60 DRAM Clock---] ---[80  DRAM Clock---] ---[120 DRAM Clock---] ---[240 DRAM Clock---] ---[480 DRAM Clock---] 

**Pull-up Output Driver Impedance** 

Configuration options: ---[Auto---] ---[34 DRAM Clock---] ---[40 DRAM Clock---] ---[48 DRAM Clock---] **Pull-Down Output Driver Impedance**   
Configuration options: ---[Auto---] ---[34 DRAM Clock---] ---[40 DRAM Clock---] ---[48 DRAM Clock---] **RTL IOL Control** 

**Round Trip Latency Init Value MC0-1 CHA-B** 

Configuration options: ---[Auto---] ---[0---] ---- ---[255---] 

**Round Trip Latency Max Value MC0-1 CHA-B** 

Configuration options: ---[Auto---] ---[0---] ---- ---[255---] 

**Round Trip Latency Offset Value Mode Sign MC0-1 CHA-B** 

Configuration options: ---[----] ---[+---] 

**Round Trip Latency Offset Value MC0-1 CHA-B** 

Configuration options: ---[Auto---] ---[0---] ---- ---[255---] 

**Round Trip Latency MC0-1 CHA-B R0-7** 

Configuration options: ---[Auto---] ---[0---] ---- ---[255---] 

**Memory Training Algorithms** 

The items in this menu allows you to enable or disable different Memory Training  Algorithms. 

**Early Command Training** 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] 

**SenseAmp Offset Training** 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] 

**Early ReadMPR Timing Centering 2D** 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] 

**Read MPR Training** 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] 

**Receive Enable Training** 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] 

**Jedec Write Leveling** 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] 

**Early Write Timing Centering 2D** 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---]

20 Pro WS W680-ACE Series BIOS Manual   
**Early Read Timing Centering 2D** 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Write Timing Centering 1D**   
Configuration options: ---[Disabled---] ---[Enabled---] 

**Write Voltage Centering 1D** 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Read Timing Centering 1D**   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Read Timing Centering with JR**   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Dimm ODT Training---***   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Max RTT---_WR**   
Allows you to cap the maximum RTT---_WR in power training. Configuration options: ---[ODT OFF---] ---[120 Ohms---] **DIMM RON Training---***   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Write Drive Strength/Equalization 2D---***   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Write Slew Rate Training---***   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Read ODT Training---***   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Comp Optimization Training**   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Read Equalization Training---***   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Read Amplifier Training---***   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Write Timing Centering 2D**   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Read Timing Centering 2D**   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Command Voltage Centering**   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Early Command Voltage Centering**   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Write Voltage Centering 2D**   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Read Voltage Centering 2D**   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Late Command Training** 

Configuration options: ---[Disabled---] ---[Enabled---] ---[Auto---]

Pro WS W680-ACE Series BIOS Manual 21   
**Round Trip Latency** 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Turn Around Timing Training**   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **CMD CTL CLK Slew Rate**   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **CMD/CTL DS & E 2D**   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Read Voltage Centering 1D**   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **TxDqTCO Comp Training---***   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **ClkTCO Comp Training---***   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **TxDqsTCO Comp Training---***   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **VccDLL Bypass Training---***   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **CMD/CTL Drive Strength Up/Dn 2D** Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **DIMM CA ODT Training**   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **PanicVttDnLp Training---***   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Read Vref Decap Traning---***   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Vddq Training**   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Duty Cycle Correction Training**   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Rank Margin Tool Per Bit**   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **DIMM DFE Training**   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **EARLY DIMM DFE Training**   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Tx Dqs Dcc Training**   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **DRAM DCA Training**   
Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] **Write Driver Strength Training** 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---]

22 Pro WS W680-ACE Series BIOS Manual   
**Rank Margin Tool** 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] 

**Memory Test** 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] 

**DIMM SPD Alias Test** 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] 

**Receive Enable Centering 1D** 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] 

**Retrain Margin Check** 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] 

**Write Drive Strength Up/Dn independently** 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] 

**Margin Check Limit** 

Checks Margin to Limit to see if next boot memory needs to be retrained. Configuration options: ---[Disabled---] ---[L1---] ---[L2---] ---[Both---] 

The following item appears only when **Margin Check Limit** is set to **---[L2---]** or **---[Both---]**. 

**Margin Limit Check L2** 

L2 check threshold is scale of L1 check.   
Configuration options: ---[1---] ---- ---[300---] 

**Third Timings** 

**tRDRD---_sg---_Training** 

Configuration options: ---[Auto---] ---[0---] ---- ---[127---] 

**tRDRD---_sg---_Runtime** 

Configuration options: ---[Auto---] ---[0---] ---- ---[127---] 

**tRDRD---_dg---_Training** 

Configuration options: ---[Auto---] ---[0---] ---- ---[127---] 

**tRDRD---_dg---_Runtime** 

Configuration options: ---[Auto---] ---[0---] ---- ---[127---] 

**tRDWR---_sg** 

Configuration options: ---[Auto---] ---[0---] ---- ---[255---] 

**tRDWR---_dg** 

Configuration options: ---[Auto---] ---[0---] ---- ---[255---] 

**tWRWR---_sg** 

Configuration options: ---[Auto---] ---[0---] ---- ---[127---] 

**tWRWR---_dg** 

Configuration options: ---[Auto---] ---[0---] ---- ---[127---] 

**tWRRD---_sg** 

Configuration options: ---[Auto---] ---[0---] ---- ---[511---] 

**tWRRD---_dg** 

Configuration options: ---[Auto---] ---[0---] ---- ---[511---]

Pro WS W680-ACE Series BIOS Manual 23   
**tRDRD---_dr** 

Configuration options: ---[Auto---] ---[0---] ---- ---[255---] **tRDRD---_dd**   
Configuration options: ---[Auto---] ---[0---] ---- ---[255---] **tRDWR---_dr**   
Configuration options: ---[Auto---] ---[0---] ---- ---[255---] **tRDWR---_dd**   
Configuration options: ---[Auto---] ---[0---] ---- ---[255---] **tWRWR---_dr**   
Configuration options: ---[Auto---] ---[0---] ---- ---[127---] **tWRWR---_dd**   
Configuration options: ---[Auto---] ---[0---] ---- ---[255---] **tWRRD---_dr**   
Configuration options: ---[Auto---] ---[0---] ---- ---[127---] **tWRRD---_dd**   
Configuration options: ---[Auto---] ---[0---] ---- ---[127---] **tRPRE**   
Configuration options: ---[Auto---] ---[0---] ---- ---[4---] **tWPRE**   
Configuration options: ---[Auto---] ---[0---] ---- ---[4---] **tWRPRE**   
Configuration options: ---[Auto---] ---[0---] ---- ---[1023---] **tPRPDEN**   
Configuration options: ---[Auto---] ---[0---] ---- ---[31---] **tRDPDEN**   
Configuration options: ---[Auto---] ---[0---] ---- ---[255---] **tWRPDEN**   
Configuration options: ---[Auto---] ---[0---] ---- ---[1023---] **tCPDED**   
Configuration options: ---[Auto---] ---[0---] ---- ---[31---] **tREFIX9**   
Configuration options: ---[Auto---] ---[0---] ---- ---[255---] **Ref Interval**   
Configuration options: ---[Auto---] ---[0---] ---- ---[8191---] **tXPDLL**   
Configuration options: ---[Auto---] ---[0---] ---- ---[127---] **tXP** 

Configuration options: ---[Auto---] ---[0---] ---- ---[127---]

24 Pro WS W680-ACE Series BIOS Manual   
**tPPD** 

Configuration options: ---[Auto---] ---[0---] ---- ---[15---] 

**tCCD---_L---_tDLLK** 

Configuration options: ---[Auto---] ---[0---] ---- ---[15---] 

**Misc.** 

**MRC Fast Boot** 

Allows you to enable or disable the MRC fast boot.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**MCH Full Check** 

Enable this item to enhance the system stability. Setting this item to **---[Disabled---]** may  enhance the DRAM overclocking capability. 

Configuration options: ---[Auto---] ---[Enabled---] ---[Disabled---] 

**Mem Over Clock Fail Count** 

Configuration options: ---[Auto---] ---[1---] ---- ---[254---] 

**Training Profile** 

Allows you to select the DIMM training profile.   
Configuration options: ---[Auto---] ---[Standard Profile---] ---[ASUS User Profile---] **RxDfe**   
Allows you to set the DFE on SOC Rx.   
Configuration options: ---[Auto---] ---[Enabled---] ---[Disabled---] 

**Mrc Training Loop Count** 

Allows you to set the exponential number of loops to run the test. Configuration options: ---[Auto---] ---[0---] ---- ---[32---] 

**DRAM CLK Period** 

Allows you to set the DRAM clock period.   
Configuration options: ---[Auto---] ---[0---] ---- ---[161---] 

**Dll---_bwsel** 

Try range of 22+ for OC.   
Configuration options: ---[Auto---] ---[0---] ---- ---[63---] 

**Controller 0, Channel 0 Control** 

Allows you to enable or disable Controller 0, Channel 0---.   
Configuration options: ---[Enabled---] ---[Disabled---] 

**Controller 0, Channel 1 Control** 

Allows you to enable or disable Controller 0, Channel 1---.   
Configuration options: ---[Enabled---] ---[Disabled---] 

**Controller 1, Channel 0 Control** 

Allows you to enable or disable Controller 1, Channel 0---.   
Configuration options: ---[Enabled---] ---[Disabled---] 

**Controller 1, Channel 1 Control** 

Allows you to enable or disable Controller 1, Channel 1---. 

Configuration options: ---[Enabled---] ---[Disabled---]

Pro WS W680-ACE Series BIOS Manual 25   
**MC---_Vref0-2** 

Configuration options: ---[Auto---] ---[0---] ---- ---[65533---] 

**Fine Granularity Refresh mode** 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] 

**DRAM SPD Configuration** 

**SDRAM Density Per Die** 

Configuration options: ---[Auto---] ---[4 Gb---] ---[8 Gb---] ---[12 Gb---] ---[16 Gb---] ---[24 Gb---] ---[32 Gb---] ---[48 Gb---] ---[64  Gb---] 

**SDRAM Banks Per Bank Group** 

Configuration options: ---[Auto---] ---[1 bank per bank group---] ---[2 bank per bank group---] ---[4 bank per  bank group---] 

**SDRAM Bank Groups** 

Configuration options: ---[Auto---] ---[1 bank group---] ---[2 bank groups---] ---[4 bank groups---] ---[8 bank  groups---] 

**Configure Memory Dynamic Frequency Switching** 

The following item appears only when **Realtime Memory Frequency** is set to **---[Disabled---]**. 

**Dynamic Memory Boost** 

Allows you to enable or disable Dynamic Memory Boost Feature. Allows automatic  switching between default SPD Profile frequency and selected XMP profile frequency.  Only valid if an XMP Profile is selected.   
Configuration options: ---[Disabled---] ---[Enabled---] 

The following item appears only when **Dynamic Memory Boost** is set to **---[Disabled---]**. 

**Realtime Memory Frequency** 

Allows you to enable or disable Memory Frequency feature. Allows manual switching  between in runtime between default SPD Profile frequency and selected XMP profile  frequency. Only valid if an XMP Profile is selected.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**SA GV** 

System Agent Geyserville. Can disable, fix to a specific point, or enable frequency  switching. If enabled, we recommend you to leave options at parked values for best  compatibility. Enabling this feature requires a longer boot time.   
Configuration options: ---[Disabled---] ---[Enabled---] ---[Fixed to 1st Point---] ---[Fixed to 2nd Point---] ---[Fixed to 3rd Point---] ---[Fixed to 4th Point---] 

The following items appear only when **SA GV** is set to **---[Enabled---]**, **---[Fixed to 1st Point---]**, **---[Fixed to 2nd Point---]**, **---[Fixed to 3rd Point---]**, or **---[Fixed to 4th Point---]**. 

**First Point Frequency** 

Allows you to specify the frequency for the given point. 0-MRC auto, else a specific  frequency as an integer: 2000Mhz.   
Configuration options: ---[0---] ---- ---[65535---] 

**First Point Gear** 

Allows you to set the gear ratio for this SAGV point. 0-Auto, 1-G1, 2-G2, 4-G4. Configuration options: ---[0---] ---- ---[4---]

26 Pro WS W680-ACE Series BIOS Manual   
**Second Point Frequency** 

Allows you to specify the frequency for the given point. 0-MRC auto, else a specific  frequency as an integer: 2000Mhz.   
Configuration options: ---[0---] ---- ---[65535---] 

**Second Point Gear** 

Allows you to set the gear ratio for this SAGV point. 0-Auto, 1-G1, 2-G2, 4-G4.   
Configuration options: ---[0---] ---- ---[4---] 

**Third Point Frequency** 

Allows you to specify the frequency for the given point. 0-MRC auto, else a specific  frequency as an integer: 2000Mhz.   
Configuration options: ---[0---] ---- ---[65535---] 

**Third Point Gear** 

Allows you to set the gear ratio for this SAGV point. 0-Auto, 1-G1, 2-G2, 4-G4.   
Configuration options: ---[0---] ---- ---[4---] 

The Fourth Point Gear will always be the settings you set in the main menu, so configure  the Fourth Point Gear there. 

**Digi+ VRM** 

**VRM Intialization Check** 

When any error occurs during VRM initialization, the system will hang at POST code  76/77 if this function is enabled. 

Configuration options: ---[Disabled---] ---[Enabled---] 

**CPU Input Voltage Load-line Calibration** 

Configuration options ---[Auto---] ---[Level 1---] ---[Level 2---] ---[Level 3---] 

**CPU Load-line Calibration** 

The load-line is defined by the Intel VRM specification and affects the level of voltage  supplied to the processor. Higher load-line calibration settings result in reduced  VDroop at the expense of voltage overshoot and will increase CPU temperatures due  to higher voltage under load. Select from level 1 to 7 to adjust the load-line slope.  Level 1 ---= greater VDroop, Level 7 ---= minimum VDroop. 

Configuration options ---[Auto---] ---[Level 1---] ---[Level 2---] ---[Level 3---] ---[Level 4:Recommended for  OC---] ---[Level 5---] ---[Level 6---] ---[Level 7---] 

The actual performance boost may vary depending on your CPU specification. 

DO NOT remove the thermal module. The thermal conditions should be monitored. 

**Synch ACDC Loadline with VRM Loadline** 

Enable this item to allow the VRM Loadline to be adjusted automatically to match the  AC/DC Loadline. 

Configuration options: ---[Disabled---] ---[Enabled---]

Pro WS W680-ACE Series BIOS Manual 27   
**CPU Current Capability** 

Allows you to set the shut-off current limit for external voltage regulator. A higher  setting will allow the voltage regulator to supply more current while a lower setting will  cause the voltage regulator to shut off the system when the supplied current is higher  than the set value. 

Configuration options: ---[Auto---] ---[100%---] ---[110%---] ---[120%---] 

Configure higher values when overclocking or under a high loading for extra power  support. 

**CPU VRM Switching Frequency** 

This item affects the VRM transient response speed and the component thermal  production. Select ---[**Manual**---] to configure a higher frequency for a quicker transient  response speed. Setting a higher switching frequency will result in better transient  response at the expense of higher VRM temperatures. Active cooling of the VRM  heatsink is recommended when running high CPU voltage and high load-line  calibration values. 

Configuration options: ---[Auto---] ---[Manual---] 

DO NOT remove the thermal module. The thermal conditions should be monitored. 

The following item appears only when **CPU VRM Switching Frequency** is set to **---[Auto---]**. 

**VRM Spread Spectrum** 

Allows you to reduce the magnitude of peak noise from the VRM. Enable to reduce  peak noise. Disable this setting when overclocking. 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] 

The following item appears only when **CPU VRM Switching Frequency** is set to  **---[Manual---]**. 

**Fixed CPU VRM Switching Frequency(KHz)** 

Allows you to set a higher frequency for a quicker transient response speed. The  values range from 250 KHz to 500 KHz with an interval of 50 KHz. 

**CPU Power Duty Control** 

CPU power duty control adjusts the duty cycle of each VRM phase based upon  current and/or temperature. 

---[Auto---] Sets to the default setting.   
---[T. Probe---] Sets the buck controller to balance VRM FET temperatures ---[Extreme---] Select to set the VRM current balance mode. 

DO NOT remove the thermal module when setting this item to **---[Extreme---]**. The thermal  conditions should be monitored.

28 Pro WS W680-ACE Series BIOS Manual   
**CPU Power Phase Control** 

Allows you to set the power phase control of the CPU.   
---[Auto---] Automatically selects the power phase control. 

---[Standard---] The number of active phases is controlled by the CPU. ---[Extreme---] Sets full phase mode. 

DO NOT remove the thermal module when setting this item to **---[Extreme---]**. The thermal  conditions should be monitored. 

The following items appear only when using the onboard graphics. 

**CPU Graphics Load-line Calibration** 

The load-line is defined by the Intel VRM specification and affects the CPU Graphics  power voltage. The CPU Graphics working voltage will decrease proportionally  depending on the CPU Graphics loading. Higher levels of the load-line calibration  can get a higher voltage and a better overclocking performance but increase the CPU  Graphics and VRM thermal production. Select from level 1 to 7 to adjust the CPU  Graphics power voltage from 100% to 0%. 

Configuration options ---[Auto---] ---[Level 1---] ---[Level 2---] ---[Level 3---] ---[Level 4:Recommended for  OC---] ---[Level 5---] ---[Level 6---] ---[Level 7---] 

Boosted performance may vary depending on the CPU Graphics specification. DO NOT  remove the thermal module. 

**CPU Graphics VRM Switching Frequency** 

The switching frequency will affect the CPU Graphics transient response speed  and the component thermal production. Select manual mode to configure a higher  frequency to get a quicker transient response speed. 

Configuration options: ---[Auto---] ---[Manual---] 

DO NOT remove the thermal module when setting this item to **---[Manual---]**. The thermal  conditions should be monitored. 

The following item appears only when **CPU Graphics VRM Switching Frequency** is set  to **---[Manual---]**. 

**Fixed CPU Graphics Switching Frequency(KHz)** 

The switching frequency will affect the CPU Graphics transient response speed and  the component thermal production. Use the ---<+---> or ---<----> to adjust the value. The values  range from 250 KHz to 500 KHz with an interval of 50 KHz. 

**Boot Voltages** 

**CPU Core/Cache Boot Voltage** 

Allows you to set the CPU voltage at initial boot up. Use the ---<+---> or ---<----> to adjust the  value. The values range from 0.600V to 1.700V with an interval of 0.005V. Configuration options: ---[Auto---] ---[0.60000---] ---- ---[1.70000---]

Pro WS W680-ACE Series BIOS Manual 29   
**CPU Input Boot Voltage** 

Allows you to set the CPU Input Voltage at initial boot up. Use the ---<+---> or ---<----> to adjust  the value. The values range from 1.500V to 2.100V with an interval of 0.010V. Configuration options: ---[Auto---] ---[1.50000---] ---- ---[2.10000---] 

**PLL Termination Boot Voltage** 

Allows you to set the PLL Termination voltage at initial boot up. Use the ---<+---> or ---<----> to  adjust the value. The values range from 0.800V to 1.800V with an interval of 0.010V. Configuration options: ---[Auto---] ---[0.80000---] ---- ---[1.80000---] 

**CPU Standby Boot Voltage** 

Allows you to set the CPU Standby voltage at initial bootup. Use the ---<+---> or ---<----> to  adjust the value. The values range from 0.800V to 1.800V with an interval of 0.010V. Configuration options: ---[Auto---] ---[0.80000---] ---- ---[1.80000---] 

**Memory Controller Boot Voltage** 

Allows you to set the Memory Controller voltage at initial bootup. Use the ---<+---> or  ---<----> to adjust the value. The values range from 1.000V to 2.000V with an interval of  0.00625V. 

Configuration options: ---[Auto---] ---[1.00000---] ---- ---[2.00000---] 

**Auto Voltage Caps** 

**CPU Core Auto Voltage Cap** 

Setting this to a specific value will set a ceiling for CPU Core Auto Voltage. When not  in manual mode, it’s effectiveness is subject to other factors such as AC/DC Loadline  values and CPU’s native VID. Use the ---<+---> or ---<----> to adjust the value. The values  range from 0.600V to 1.700V with an interval of 0.005V. 

Configuration options: ---[Auto---] ---[0.60000---] ---- ---[1.70000---] 

**CPU Input Auto Voltage Cap** 

Setting this to a specific value will set a ceiling for CPU Input Auto Voltage. Use the  ---<+---> or ---<----> to adjust the value. The values range from 1.500V to 2.100V with an  interval of 0.010V.  

Configuration options: ---[Auto---] ---[1.50000---] ---- ---[2.10000---] 

**Memory Controller Auto Voltage Cap** 

Setting this to a specific value will set a ceiling for Memory Controller Auto Voltage.  Use the ---<+---> or ---<----> to adjust the value. The values range from 1.000V to 2.000V with  an interval of 0.00625V. 

Configuration options: ---[Auto---] ---[1.00000---] ---- ---[2.00000---] 

**Internal CPU Power Management** 

The items in this submenu allow you to set the CPU ratio and features. **Tcc Activation Offset**   
Offse from factory set Tcc activation temperature at which the Thermal Control Circuit  must be activated. Tcc will be activated at: Tcc Activation Temp ---- Tcc Activation  Offset. Use the ---<+---> or ---<----> to adjust the value. 

Configuration options: ---[Auto---] ---[0---] ---- ---[63---] 

**IVR Transmitter VDDQ ICCMAX** 

Configuration options: ---[Auto---] ---[0---] ---- ---[15---]

30 Pro WS W680-ACE Series BIOS Manual   
**CPU Core/Cache Current Limit Max.**  

Allows you to configure a current limit for frequency or power throttling when  overclocking. Can be set to maximum value (511.75) to prevent throttling when  overclocking. Use the ---<+---> and ---<----> keys to adjust the value. 

Configuration options: ---[Auto---] ---[0.00---] ---- ---[511.75---] 

**CPU Graphics Current Limit** 

Allows you to configure a high current limit to prevent frequency or power throttling  when overclocking. Use the ---<+---> and ---<----> keys to adjust the value. Configuration options: ---[Auto---] ---[0.00---] ---- ---[511.75---] 

**Long Duration Package Power Limit** 

An Intel parameter known as ---[power limit 1---] and specified in Watts. The defualt value  is defined by TDP of the processor. Increasing the value will allow the Turbo ratio to  be maintained for a longer duration under higher current loads.  

Configuration options: ---[Auto---] ---[1---] ---- ---[4095---] 

**Package Power Time Window** 

An Intel parameter of ---[power limit 1---] and specified in seconds. The applied value  indicates how long the Turbo ratio can be active when TDP is exceeded. Configuration options: ---[Auto---] ---[1---] ---[2---] ---[3---] ---[4---] ---[5---] ---[6---] ---[7---] ---[8---] ---[10---] ---[12---] ---[14---] ---[16---] ---[20---] ---[24---] ---[28---]  ---[32---] ---[40---] ---[48---] ---[56---] ---[64---] ---[80---] ---[96---] ---[112---] ---[128---] ---[160---] ---[192---] ---[224---] ---[256---] ---[320---] ---[384---] ---[448---] 

**Short Duration Package Power Limit** 

An Intel parameter known as ---[power limit 2---] and specified in Watts. It is the second  Intel power limit which provides protection when package power exceeds power limit  1---. The default setting is 1.25 times power limit 1---. According to Intel, the platform must  support this value for up to 10msec when power consumption exceeds power limit  2---. ASUS motherboards are engineered to support this duration for a longer time as  required to facilitate overclocking.  

Configuration options: ---[Auto---] ---[1---] ---- ---[4095---] 

**Dual Tau Boost** 

Allows you to enable or disable Dual Tau Boost feature. This is only applicable for  Desktop 35W/65W/12W sku. When DPTF is enabled this feature is ignored. Configuration options: ---[Disabled---] ---[Enabled---] 

**IA AC Load Line** 

Allows you to set the AC loadline defined in mOhms. Use the ---<+---> and ---<----> keys to  adjust the value. 

Configuration options: ---[Auto---] ---[0.01---] ---- ---[62.49---] 

**IA DC Load Line** 

Allows you to set the DC loadline defined in mOhms. Use the ---<+---> and ---<----> keys to  adjust the value. 

Configuration options: ---[Auto---] ---[0.01---] ---- ---[62.49---] 

**IA CEP Enable** 

Allows you to enable or disable IA CEP (Current Excursion Protection) Support. Uses  pCode Mailbox Command 0x37, Sub-command 0x1. Set Databit2 to 1---. Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---]

Pro WS W680-ACE Series BIOS Manual 31   
**GT CEP Enable** 

Allows you to enable or disable GT CEP (Current Excursion Protection) Support. Uses  pCode Mailbox Command 0x37, Sub-command 0x1. Set Databit3 to 1---. Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] 

**SA CEP Enable** 

Allows you to enable or disable SA CEP (Current Excursion Protection) Support. Uses  pCode Mailbox Command 0x37, Sub-command 0x1. Set Databit3 to 1---. Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] 

**IA SoC Iccmax Reactive Protector** 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] 

**Inverse Temperature Dependency Throttle** 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] 

**IA VR Voltage Limit** 

Voltage Limit (VMAX). This value represents the Maximum instantaneous voltage  allowed at any given time. Range is 0 ---- 7999mV. Uses BIOS VR mailbox command  0x8. 

Configuration options: ---[Auto---] ---[0---] ---- ---[7999---] 

**CPU DLVR Bypass Mode Enable** 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] 

**CPU SVID Support** 

Disable this item to stop the CPU from communicating with the external voltage  regulator. 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] 

**Tweaker’s Paradise** 

**Realtime Memory Timing** 

Allows you to enable or disable realtime memory timing. When set to **---[Enabled---]**, the  system will allow performing realtime memory timing changes after MRC---_DONE. Configuration options: ---[Disabled---] ---[Enabled---] 

**SPD Write Disable** 

Allows you to enable or disable setting SPD Write Disable. For security  recommendations, SPD write disable bit must be set. 

Configuration options: ---[TRUE---] ---[FALSE---] 

**PVD Ratio Threshold** 

For the Core Domain PLL, the threshold to switch to lower post divider is 15 by  default. You can set a value lower than 15 when pushing high BCLK so that Digitally  Controlled Oscillator (DCO) remains at reasonable frequency. 

Configuration options: ---[Auto---] ---[1---] ---- ---[40---] 

**SA PLL Frequency Override** 

Allows you to configure Sa PLL Frequency. 

Configuration options: ---[Auto---] ---[3200 MHz---] ---[1600 MHz---]

32 Pro WS W680-ACE Series BIOS Manual   
**BCLK TSC HW Fixup** 

Allows you to enable or disable BCLK TSC HW Fixup disable during TSC copy from  PMA to APIC. 

Configuration options: ---[Enabled---] ---[Disabled---] 

**FLL OC mode** 

Configuration options: ---[Auto---] ---[Disabled---] ---[Normal---] ---[Elevated---] ---[Extreme Elevated---] **UnderVolt Protection**   
When UnderVolt Protection is enabled, user will not be able to program under voltage  in OS runtime. Recommended to keep it enabled by default. 

---[Disabled---] No UnderVolt Protection in Runtime. 

---[Enabled---] Allow BIOS undervolting, but enable UnderVolt Protection in  Runtime. 

**Core PLL Voltage** 

Allows you to configure the offset for the Core PLL VCC Trim. The values range from  0.900V to 1.845V with an interval of 0.015V. 

Configuration options: ---[Auto---] ---[0.90000---] ---- ---[1.84500---] 

**GT PLL Voltage** 

Allows you to configure the offset for the GT PLL VCC Trim. The values range from  0.900V to 1.845V with an interval of 0.015V. 

Configuration options: ---[Auto---] ---[0.90000---] ---- ---[1.84500---] 

**Ring PLL Voltage** 

Allows you to configure the offset for the Ring PLL VCC Trim. The values range from  0.900V to 1.845V with an interval of 0.015V. 

Configuration options: ---[Auto---] ---[0.90000---] ---- ---[1.84500---] 

**System Agent PLL Voltage** 

Allows you to configure the offset for the System Agent PLL VCC Trim. The values  range from 0.900V to 1.845V with an interval of 0.015V. 

Configuration options: ---[Auto---] ---[0.90000---] ---- ---[1.84500---] 

**Memory Controller PLL Voltage** 

Allows you to configure the offset for the Memory Controller PLL VCC Trim. The  values range from 0.900V to 1.845V with an interval of 0.015V. 

Configuration options: ---[Auto---] ---[0.90000---] ---- ---[1.84500---] 

**CPU 1.8V Small Rail** 

Allows you to configure the voltage for the CPU 1.8V Small Rail. The values range  from 1.500V to 2.300V with an interval of 0.010V. 

Configuration options: ---[Auto---] ---[1.50000---] ---- ---[2.30000---] 

**PLL Termination Voltage** 

Allows you to configure the voltage for the PLL Termination. The values range from  0.800V to 1.800V with an interval of 0.010V. 

Configuration options: ---[Auto---] ---[0.80000V---] ---- ---[1.80000V---] 

**CPU Standby Voltage** 

Allows you to configure the voltage for the CPU Standby. Use the ---<+---> and ---<----> keys to  adjust the value. The values range from 0.800V to 1.800V with an interval of 0.010V. Configuration options: ---[Auto---] ---[0.80000---] ---- ---[1.80000---]

Pro WS W680-ACE Series BIOS Manual 33   
**PCH 1.05V Voltage** 

Allows you to configure the voltage for the PCH 1.05V. Use the ---<+---> and ---<----> keys to  adjust the value. The values range from 0.800V to 1.600V with an interval of 0.010V. Configuration options: ---[Auto---] ---[0.80000---] ---- ---[1.60000---] 

**PCH 0.82V Voltage** 

Allows you to configure the voltage for the PCH 0.82V. Use the ---<+---> and ---<----> keys to  adjust the value. The values range from 0.700V to 1.000V with an interval of 0.010V. Configuration options: ---[Auto---] ---[0.70000---] ---- ---[1.00000---] 

**CPU Input Voltage Reset Voltage** 

Allows you to configure the voltage for the CPU Input when reset. Use the ---<+---> and  ---<----> keys to adjust the value. The values range from 1.500V to 2.100V with an interval  of 0.010V. 

Configuration options: ---[Auto---] ---[1.50000---] ---- ---[2.10000---]

34 Pro WS W680-ACE Series BIOS Manual   
**7---. Advanced menu** 

The Advanced menu items allow you to change the settings for the CPU and other system  devices. Scroll down to display other BIOS items. 

Be cautious when changing the settings of the Advanced menu items. Incorrect field  

values can cause the system to malfunction.

![][image6]Pro WS W680-ACE Series BIOS Manual 35   
**7.1 Platform Misc Configuration** 

The items in this menu allow you to configure the platform-related features. ![][image7]  
**PCI Express Native Power Management** 

Allows you to enhance the power saving feature of PCI Express and perform Active State  Power Management (ASPM) operations in the operating system when set to **---[Enabled---]**. Configuration options: ---[Disabled---] ---[Enabled---] 

The following item appears only when PCI Express Native Power Management is set to **---[Enabled---]**. 

**Native ASPM** 

Set this item to **---[Enabled---]** for OS Controlled ASPM, or set this item to ---[Disabled---] for BIOS  controlled ASPM. 

Configuration options: ---[Auto---] ---[Enabled---] ---[Disabled---] 

**PCH ---- PCI Express** 

**DMI Link ASPM Control** 

Allows you to control the Active State Power Management of the DMI Link. Configuration options: ---[Disabled---] ---[L1---] ---[Auto---] 

**ASPM** 

Allows you to select the ASPM state for energy-saving conditions.   
Configuration options: ---[Disabled---] ---[L1---] ---[Auto---] 

**L1 Substates** 

Allows you to select the PCI Express L1 Substates settings.   
Configuration options: ---[Disabled---] ---[L1.1---] ---[L1.1 & L1.2---] 

**SA ---- PCI Express** 

**DMI ASPM** 

Allows you to set the DMI ASPM Support. 

Configuration options: ---[Disabled---] ---[Auto---] ---[ASPM L1---] 

36 Pro WS W680-ACE Series BIOS Manual   
**DMI Gen3 ASPM** 

Allows you to set the DMI Gen3 ASPM Support.   
Configuration options: ---[Disabled---] ---[Auto---] ---[ASPM L1---] 

**PEG ---- ASPM** 

Allows you to control the ASPM support for the PEG 0---. This has no effect if PEG is not the  currently active device. 

Configuration options: ---[Disabled---] ---[L0s---] ---[L1---] ---[L0sL1---] 

**PCI Express Clock Gating** 

Allows you to enable or disable PCI Express Clock Gating for each root port. Configuration options: ---[Disabled---] ---[Enabled---] 

**7.2 CPU Configuration** 

The items in this menu show the CPU-related information that the BIOS automatically  detects. Scroll down to display other BIOS items. 

The items in this menu may vary based on the CPU installed. 

![][image8]  
**Efficient Core Information** 

This submenu displays the Efficient Core Information. 

**Performance Core Information** 

This submenu displays the Performance Core Information.

Pro WS W680-ACE Series BIOS Manual 37   
**Hardware Prefetcher** 

Allows you to enable or disable the MLC streamer prefetcher.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**Adjacent Cache Line Prefetch** 

Allows you to prefetch adjacent cache lines, reducing the DRAM loading time and improving  the system performance. 

Configuration options: ---[Disabled---] ---[Enabled---] 

**Intel (VMX) Virtualization Technology** 

When set to **---[Enabled---]**, VMM can utilize the additional hardware capabilities provided by  Vanderpool Technology. 

Configuration options: ---[Disabled---] ---[Enabled---] 

The following items appear only when **Intel Trusted Execution Technology** is set to **---[Disabled---]**. 

**Active Performance Cores** 

Allows you to select the number of CPU cores to activate in each processor package. Configuration options: ---[All---] ---[1---] ---- ---[7---] 

**Active Efficient Cores** 

Allows you to select the number of Efficient cores to activate in each processor package. Configuration options: ---[All---] ---[0---] ---- ---[3---] 

Number of Cores and Efficient Cores are looked at together. When both are {0,0}, Pcode  will enable all cores. 

**Hyper-Threading** 

Allows a hyper-threading processor to appear as two logical processors, allowing the  operating system to schedule two threads or processes simultaneously. ---[Enabled---] For two threads per activated core. 

---[Disabled---] For only one thread per activated core. 

The following item appears only when **Intel (VMX) Virtualization Technology** is set to **---[Enabled---]**. 

**Intel Trusted Execution Technology** 

Allows you to enable utilization of additional hardware capabilities provided by Intel(R)  Trusted Execution Technology. 

Configuration options: ---[Disabled---] ---[Enabled---] 

Changes require a full power cycle to take effect.

38 Pro WS W680-ACE Series BIOS Manual   
The following items appear only when **Intel Trusted Execution Technology** is set to **---[Enabled---]**. 

**Alias Check Request** 

Enables Txt Alias Checking capability.    
Configuration options: ---[Disabled---] ---[Enabled---] 

• Changes require full Txt capability before it will take effect. 

• This is a one time only change, and will be reset on the next reboot. 

**DPR Memory Size (MB)** 

Reserve DPR memory size (0-255) MB.   
Configuration options: ---[0---] ---- ---[255---] 

**Reset AUX Content** 

Reset TPM Aux Content. Txt may not be functional after AUX content gets reset. 

**Total Memory Encryption** 

Allows you to configure the Total Memory Encryption (TME) to protect DRAM data from  physical attacks. 

Configuration options: ---[Disabled---] ---[Enabled---] 

**Legacy Game Compatibility Mode** 

When set to ---[Enabled---], pressing the scroll lock key will toggle the Efficient-cores between  being parked when Scroll Lock LED is on and un-parked when LED is off. Configuration options: ---[Disabled---] ---[Enabled---] 

**CPU ---- Power Management Control** 

The items in this submenu allow you to manage and configure the CPU’s power. **Boot performance mode**   
Allows you to select the performance state that the BIOS will set starting from the  reset vector. 

Configuration options: ---[Max Battery---] ---[Max Non-Turbo Performance---]  ---[Turbo Performance---] ---[Auto---] 

**Intel(R) SpeedStep(tm)** 

Allows more than two frequency to be supported.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**Intel(R) Speed Shift Technology** 

Allows you to disable or enable Intel(R) Speed Shift Technology support. When  enabled, CPPC v2 interface allows hardware controlled P-states. 

Configuration options: ---[Disabled---] ---[Enabled---] 

**Intel(R) Turbo Boost Max Technology 3.0** 

Allows you to disable or enable Intel(R) Turbo Boost Max Technology 3.0 support.  Disabling will report the maximum ratio of the slowest core in ---_CPC object. Configuration options: ---[Disabled---] ---[Enabled---]

Pro WS W680-ACE Series BIOS Manual 39   
**Turbo Mode** 

Allows you to automatically set the CPU cores to run faster than the base operating  frequency when it is below the operating power, current and temperature specification  limit. 

Configuration options: ---[Disabled---] ---[Enabled---] 

**Acoustic Noise Settings** 

The items in this submenu allow you to configure Acoustic Noise Settings for IA, GT,  and SA domains. 

**Acoustic Noise Settings** 

**Acoustic Noise Mitigation** 

Enabling this option will help mitigate acoustic noise on certain SKUs when the CPU is in  deeper C state.   
Configuration options: ---[Disabled---] ---[Enabled---] 

The following items appear only when **Acoustic Noise Mitigation** is set to **---[Enabled---]**. 

**Pre Wake Time** 

Allows you to set the maximum Pre Wake randomization time in micro ticks. This is for  acoustic noise mitigation Dynamic Periodicity Alteration (DPA) tuning. Use the ---<+---> or ---<---->  to adjust the value.   
Configuration options: ---[0---] ---- ---[255---] 

**Ramp Up Time** 

Allows you to set the maximum Ramp Up randomization time in micro ticks. This is for  acoustic noise mitigation Dynamic Periodicity Alteration (DPA) tuning. Use the ---<+---> or ---<---->  to adjust the value.   
Configuration options: ---[0---] ---- ---[255---] 

**Ramp Down Time** 

Allows you to set the maximum Ramp Down randomization time in micro ticks. This is for  acoustic noise mitigation Dynamic Periodicity Alteration (DPA) tuning. Use the ---<+---> or ---<---->  to adjust the value.   
Configuration options: ---[0---] ---- ---[255---] 

**IA VR Domain** 

**Disable Fast PKG C State Ramp for IA Domain** 

This option needs to be configured to reduce acoustic noise during deeper C states. ---[FALSE---] Don’t disable Fast ramp during deeper C states. 

---[TRUE---] Disable Fast ramp during deeper C state. 

**Slow Slew Rate for IA Domain** 

Set VR IA Slow Slew Rate for Deep Package C State ramp time; Slow slew rate equals  to Fast divided by number, the number is 2, 4, 8 to slow down the slew rate to help  minimize acoustic noise.   
Configuration options: ---[Fast/2---] ---[Fast/4---] ---[Fast/8---] 

**GT VR Domain** 

**Disable Fast PKG C State Ramp for GT Domain** 

This option needs to be configured to reduce acoustic noise during deeper C states. ---[FALSE---] Don’t disable Fast ramp during deeper C states. 

---[TRUE---] Disable Fast ramp during deeper C state.

40 Pro WS W680-ACE Series BIOS Manual   
**Slow Slew Rate for GT Domain** 

Set VR GT Slow Slew Rate for Deep Package C State ramp time; Slow slew rate equals  to Fast divided by number, the number is 2, 4, 8 to slow down the slew rate to help  minimize acoustic noise.   
Configuration options: ---[Fast/2---] ---[Fast/4---] ---[Fast/8---] 

**CPU C-states** 

Allows you to enable or disable CPU Power Management. Allows CPU to go to C  states when it’s not 100% utilized. 

Configuration options: ---[Auto---] ---[Disabled---] ---[Enabled---] 

The following items appear only when **CPU C-states** is set to **---[Enabled---]**. 

**Enhanced C-States** 

Allows you to enable or disable C1E. When enabled, CPU will switch to minimum  speed when all cores enter C-State. 

Configuration options: ---[Enabled---] ---[Disabled---] 

**Package C State Limit** 

Allows you to set the C-state limit for the CPU package. Setting to **---[CPU Default---]** will leave it as the Factory default value. Setting to **---[Auto---]** will initialize the deepest  available Package C State Limit. 

Configuration options: ---[C0/C1---] ---[C2---] ---[C3---] ---[C6---] ---[C7---] ---[C7s---] ---[C8---] ---[C9---] ---[C10---] ---[CPU Default---]  ---[Auto---] 

**Thermal Monitor** 

Allows you to enable or disable the Thermal Monitor.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**Dual Tau Boost** 

Allows you to enable Dual Tau Boost feature. This is only applicable for Desktop  35W/65W/125W sku. When DPTF is enabled this feature is ignored. Configuration options: ---[Disabled---] ---[Enabled---]

Pro WS W680-ACE Series BIOS Manual 41   
**7.3 System Agent (SA) Configuration** 

The items in this menu allow you to change the System Agent (SA) parameters. ![][image9]  
**VT-d** 

Allows you to enable virtualization technology function on memory control hub. Configuration options: ---[Enabled---] ---[Disabled---] 

The following item appears only when **VT-d** is set to **---[Enabled---]**. 

**Control Iommu Pre-boot Behavior** 

Allows you to enable IOMMU in Pre-boot environment (if DMAR table is installed in DXE and  if VTD---_INFO---_PPI is installed in PEI). 

Configuration options: ---[Disable IOMMU---] ---[Enable IOMMU during boot---] 

**Memory Configuration** 

The items in this submenu allow you to set memory configuration parameters.  **Memory Remap**   
Allows you to enable or disable memory remap above 4GB.   
Configuration options: ---[Enabled---] ---[Disabled---] 

**Graphics Configuration** 

The items in this submenu allow you to select a primary display from CPU Graphics, PEG  Graphics devices, or PCIe Graphics devices. 

**Primary Display** 

Allows you to select the primary display from CPU Graphics / PEG Graphics / PCIe  Graphics device. 

Configuration options: ---[Auto---] ---[CPU Graphics---] ---[PEG Slot---] ---[PCIE---] 

**iGPU Multi-Monitor** 

Set this item to ---[Enabled---] to empower both integrated and discrete graphics for multi monitor output. iGPU shared system memory size will be fixed at 64M. Configuration options: ---[Disabled---] ---[Enabled---] 

**DVMT Pre-Allocated** 

Allows you to select the DVMT 5.0 Pre-Allocated (Fixed) Graphics Memory size used  by the Internal Graphics Device. 

Configuration options: ---[32M---] ---[64M---] ---[96M---] ---[128M---] ---[160M---] ---[192M---] ---[224M---] ---[256M---] ---[288M---]  ---[320M---] ---[352M---] ---[384M---] ---[416M---] ---[448M---] ---[480M---] ---[512M---]

42 Pro WS W680-ACE Series BIOS Manual   
**RC6(Render Standby)** 

Allows you to enable render standby support.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**VMD setup menu** 

The items in this submenu allow you to set the VMD configuration settings. **Enable VMD controller**   
Allows you to enable or disable the VMD controller.   
Configuration options: ---[Disabled---] ---[Enabled---] 

Setting Enable VMD controller to **---[Disabled---]** may result in data loss. 

The following items appear only when **Enable VMD controller** is set to **---[Enabled---]**. 

**Map PCIE Storage under VMD** 

Allows you to map or unmap PCIE Storage to VMD.   
Configuration options: ---[Disabled---] ---[Enabled---] 

Ensure to set **Map SATA Controller under VMD** to **---[Disabled---]** if you set **Map PCIE  Storage under VMD** to **---[Enabled---]**. 

**Map SATA Controller under VMD** 

Allows you to map or unmap this Root Port to VMD.   
Configuration options: ---[Disabled---] ---[Enabled---] 

Ensure to set **Map PCIE Storage under VMD** to **---[Disabled---]** if you set **Map SATA  Controller under VMD** to **---[Enabled---]**. 

**PCI Express Configuration** 

The items in this submenu allow you to configure the PCIe Speeds for the different onboard  slots.  

**M.2---_1 Link Speed** 

Allows you to configure the PCIe speed for this slot.   
Configuration options: ---[Auto---] ---[Gen1---] ---[Gen2---] ---[Gen3---] ---[Gen4---] 

**PCIEX16(G5)---_1 Link Speed** 

Allows you to configure the PCIe speed for this slot.   
Configuration options: ---[Auto---] ---[Gen1---] ---[Gen2---] ---[Gen3---] ---[Gen4---] ---[Gen5---] 

**PCIEX16 (G5)---_2 Link Speed** 

Allows you to configure the PCIe speed for this slot. 

Configuration options: ---[Auto---] ---[Gen1---] ---[Gen2---] ---[Gen3---] ---[Gen4---] ---[Gen5---]

Pro WS W680-ACE Series BIOS Manual 43   
**7.4 PCH Configuration** 

The items in this menu allow you to change the PCIe configurations for slots supported by  the PCH. 

![][image10]

**PCI Express Configuration** 

The items in this submenu allow you to configure the PCIe Speeds for the different onboard  slots supported by the PCH. 

**PCIEX16(G3) Link Speed** 

Allows you to configure the PCIe speed for this slot.   
Configuration options: ---[Auto---] ---[Gen1---] ---[Gen2---] ---[Gen3---] 

**PCIEX16(G3)---_1 Link Speed** 

Allows you to configure the PCIe speed for this slot.   
Configuration options: ---[Auto---] ---[Gen1---] ---[Gen2---] ---[Gen3---] 

**PCIEX16(G3)---_2 Link Speed** 

Allows you to configure the PCIe speed for this slot.   
Configuration options: ---[Auto---] ---[Gen1---] ---[Gen2---] ---[Gen3---] 

**M.2---_2 Link Speed** 

Allows you to configure the PCIe speed for this slot.   
Configuration options: ---[Auto---] ---[Gen1---] ---[Gen2---] ---[Gen3---] ---[Gen4---] 

**M.2---_3 Link Speed** 

Allows you to configure the PCIe speed for this slot. 

Configuration options: ---[Auto---] ---[Gen1---] ---[Gen2---] ---[Gen3---] ---[Gen4---]

44 Pro WS W680-ACE Series BIOS Manual   
**7.5 PCH Storage Configuration** 

While entering Setup, the BIOS automatically detects the presence of SATA devices. The  SATA Port items show Empty if no SATA device is installed to the corresponding SATA port.  Scroll down to display the other BIOS items. 

![][image11]  
**SATA Controller(s)**  

Allows you to enable or disable the SATA Device.   
Configuration options: ---[Disabled---] ---[Enabled---] 

The following items appear only when **SATA Controller(s)** is set to **---[Enabled---]**. 

**Aggressive LPM support** 

Allows PCH to aggressively enter link power state.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**SMART Self Test** 

S.M.A.R.T. (Self-Monitoring, Analysis and Reporting Technology) is a monitoring system  that shows a warning message during POST (Power-on Self Test) when an error occurs in  the hard disks. 

Configuration options: ---[Disabled---] ---[Enabled---]  

**SLIMSAS---_1 ---- SLIMSAS---_4** 

Allows you to enable or disable the selected port.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**SLIMSAS---_1 ---- SLIMSAS---_4 Hot Plug**  

Designates this port as Hot Pluggable. 

Configuration options: ---[Disabled---] ---[Enabled---]

Pro WS W680-ACE Series BIOS Manual 45   
**SATA6G---_1 ---- SATA6G---_4** 

Allows you to enable or disable the selected port.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**SATA6G---_1 ---- SATA6G---_4 Hot Plug**  

Designates this port as Hot Pluggable.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**7.6 PCH-FW Configuration** 

The items in this menu allows you to configure the firmware TPM. 

![][image12]  
**PTT** 

Allows you to enable or disable PTT in SkuMgr.   
Configuration options: ---[Disable---] ---[Enable---] 

**Extend CSME Measurement to TPM-PCR** 

Allows you to enable or disable Extend CSME Measurements to TPM-PCR---[0---] and AMR  Config to TPM-PCR---[1---]. 

Configuration options: ---[Disabled---] ---[Enabled---] 

**7.7 AMT Configuration** 

The items in this menu allow you to configure Intel(R) Active Management Technology  parameters. 

![][image13]  
**USB Provisioning of AMT** 

Allows you to enable or disable AMT USB provisioning.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**MAC Pass Through** 

Allows you to enable or disable MAC Pass Through function. 

Configuration options: ---[Disabled---] ---[Enabled---]

46 Pro WS W680-ACE Series BIOS Manual   
**Activate Remote Assistance Process** 

Allows you to trigger CIRA boot.   
Configuration options: ---[Disabled---] ---[Enabled---] 

Network Access must be activated first from MEBx Setup. 

**Unconfigure ME** 

Unconfigure ME with resetting MEBx password to default on next boot. Configuration options: ---[Disabled---] ---[Enabled---] 

**ASF Configuration** 

The items in this submenu allow you to configure Alert Standard Format parameters. **PET Progress**   
Allows you to enable or disable PET Events Progress to receive PET Events. Configuration options: ---[Disabled---] ---[Enabled---] 

**WatchDog** 

Allows you to enable or disable WatchDog Timer.   
Configuration options: ---[Disabled---] ---[Enabled---] 

The following items appear only when **WatchDog** is set to **---[Enabled---]**. 

**OS Timer** 

Allows you to set OS watchdog timer.   
Configuration options: ---[0---] ---- ---[65535---] 

**BIOS Timer** 

Allows you to set BIOS watchdog timer.   
Configuration options: ---[0---] ---- ---[65535---] 

**ASF Sensors Table** 

Adds ASF Sensor Table into ASF---! ACPI Table.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**Secure Erase Configuration** 

The items in this submenu allow you to configure secure erase. 

**Secure Erase mode** 

Change the Secure Erase module behavior.   
---[Simulated---] Performs SE flow without erasing SSD.   
---[Real---] Erase SSD. 

**Force Secure Erase** 

Allows you to force Secure Erase on next boot.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**One Click Recovery (OCR) Configuration** 

The items in this submenu allow you to configure settings for One Click Recovery. This will  allow access for AMT to boot a recovery OS application.

Pro WS W680-ACE Series BIOS Manual 47   
**OCR Https Boot** 

Allows you to enable or disable One Click Recovery Https Boot.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**OCR PBA Boot** 

Allows you to enable or disable One Click Recovery PBA Boot.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**OCR Windows Recovery Boot** 

Allows you to enable or disable One Click Recovery Windows Recovery Boot. Configuration options: ---[Disabled---] ---[Enabled---] 

**OCR Disable Secure Boot** 

Allows CSME to request Secure Boot to be disabled for One Click Recovery. Configuration options: ---[Disabled---] ---[Enabled---] 

**7.8 Thunderbolt(TM) Configuration** 

The items in this menu allow you to configure Thunderbolt settings. 

![][image14]  
**PCIE Tunneling over USB4** 

Allows you to enable or disable PCIE Tunneling over USB4.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**Discrete Thunderbolt(TM) Support** 

Allows you to enable or disable Discrete Thunderbolt(TM) Support.   
Configuration options: ---[Disabled---] ---[Enabled---] 

• Please set Control Iommu Pre-boot Behavior in the System Agent(SA) Configuration  page to **---[Enabled---]** to support DMA Protection Feature. 

• The following items appear only when **Discrete Thunderbolt(TM) Support** is set to **---[Enabled---]**. 

**Wake From Thunderbolt(TM) Devices** 

Allows you to enable or disable system wake from Thunderbolt(TM) devices. Configuration options: ---[Disabled---] ---[Enabled---] 

**Discrete Thunderbolt(TM) Configuration** 

The items in this submenu allow you to configure Discrete Thunderbolt(TM) related  configurations. 

**DTBT Go2Sx Command** 

Allows you to enable the command to put DTBT into Sx state while system is going  into Sx. 

Configuration options: ---[Disabled---] ---[Enabled---]

48 Pro WS W680-ACE Series BIOS Manual   
**Windows 10 Thunderbolt Support** 

Allows you to specify Windows 10 Thunderbolt support level.   
---[Enable ---+ RTD3---] OS Native support plus RTD3.   
---[Disabled---] No OS native support. 

**DTBT Controller 0 Configuration** 

**DTBT Contorller 0** 

Configuration options: ---[Disabled---] ---[Enabled---] 

**TBT Host Router** 

Allows you to enable host router based on ports available.   
Configuration options: ---[One Port---] ---[Two Port---] 

**Extra Bus Reserved** 

Allows you to select the TBT Root Port Type.   
---[56---] One port Host.   
---[106---] Two port Host. 

**Reserved Memory** 

Allows you to set the Reserved Memory for this Root Bridge. Use the ---<+---> and ---<----> keys  to adjust the value.   
Configuration options: ---[1---] ---- ---[4096---] 

**Memory alignment** 

Allows you to set the memory alignment bits. Use the ---<+---> and ---<----> keys to adjust the  value.   
Configuration options: ---[0---] ---- ---[31---] 

**Reserved PMemory** 

Allows you to set the Reserved Prefetchable Memory for this Root Bridge. Use the ---<+--->  and ---<----> keys to adjust the value.   
Configuration options: ---[1---] ---- ---[4096---] 

**PMemory alignment** 

Allows you to set the PMemory alignment bits. Use the ---<+---> and ---<----> keys to adjust the  value.   
Configuration options: ---[0---] ---- ---[31---] 

**Reserved I/O** 

Use the ---<+---> and ---<----> keys to adjust the value. Use the ---<+---> and ---<----> keys to adjust the  value. The values range from 0 to 60 with an interval of 4---. 

Configuration options: ---[0---] ---- ---[60---]

Pro WS W680-ACE Series BIOS Manual 49   
**7.9 Trusted Computing** 

The items in this menu allow you to configure the Trusted Computing settings. ![][image15]  
**Security Device Support** 

Allows you to enable or disable the BIOS support for security device. O.S. will not show  Security Device. TCG EFI protocol and INT1A interface will not be available. Configuration options: ---[Disable---] ---[Enable---]  

The following items appear only when **Security Device Support** is set to **---[Enable---]**. 

**SHA256 PCR Bank** 

Allows you to enable or disable the SHA256 PCR Bank.   
Configuration options: ---[Disabled---] ---[Enabled---]  

**Pending operation** 

Allows you to schedule an Operation for the Security Device.   
Configuration options: ---[None---] ---[TPM Clear---]  

Your computer will reboot during restart in order to change the State of the Security  Device. 

**Platform Hierarchy** 

Allows you to enable or disable the Platform Hierarchy.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**Storage Hierarchy** 

Allows you to enable or disable the Storage Hierarchy.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**Endorsement Hierarchy** 

Allows you to enable or disable the Endorsement Hierarchy. 

Configuration options: ---[Disabled---] ---[Enabled---]

50 Pro WS W680-ACE Series BIOS Manual   
**Physical Presence Spec Version** 

Allows you to select to Tell O.S. to support PPI Version 1.2 or 1.3.   
Configuration options: ---[1.2---] ---[1.3---] 

Some HCK tests might not support 1.3. 

**PH Randomization** 

Allows you enable or disable Platform Hierarchy randomization.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**7.10 Redfish Host Interface Settings** 

The items in this menu allow you to configure Redfish Host Interface Settings.  
**Redfish** 

Allows you to enable or disable Redfish.   
Configuration options: ---[Disabled---] ---[Enabled---] 

The following items appear only when **Redfish** is set to **---[Enabled---]**. 

**Authentication mode** 

Allows you to select the authentication mode.   
Configuration options: ---[Basic Authentication---] ---[Session Authentication---] 

Pro WS W680-ACE Series BIOS Manual 51   
**7.11 Serial Port Console Redirection** 

The items in this menu allow you to configure serial port console redirection settings. **COM0 / COM1 (Pci Bus0, Dev22, Func3, Port0)** 

**Console Redirection** 

Allows you to enable or disable the console redirection feature.   
Configuration options: ---[Disabled---] ---[Enabled---] 

The following item appears only when **Console Redirection** for COM0 or COM1 (Pci  Bus0, Dev22, Func3, Port0) is set to **---[Enabled---]**. 

**Console Redirection Settings** 

These items become configurable only when you enable the Console Redirection  item. The settings specify how the host computer and the remote computer (which  the user is using) will exchange data. Both computers should have the same or  compatible settings. 

**Terminal Type** 

Allows you to set the terminal type.   
---[VT100---] ASCII char set.   
---[VT100+---] Extends VT100 to support color, function keys, etc. ---[VT-UTF8---] Uses UTF8 encoding to map Unicode chars onto 1 or more  bytes. 

---[ANSI---] Extended ASCII char set. 

**Bits per second** 

Selects serial port transmission speed. The speed must be matched on the other side.  Long or noisy lines may require lower speeds.  

Configuration options: ---[9600---] ---[19200---] ---[38400---] ---[57600---] ---[115200---] 

**Data Bits** 

Configuration options: ---[7---] ---[8---]

52 Pro WS W680-ACE Series BIOS Manual   
**Parity** 

A parity bit can be sent with the data bits to detect some transmission errors. ---[Mark---]  and ---[Space---] parity do not allow for error detection. They can be used as an additional  data bit. 

---[None---] None   
---[Even---] Parity bit is 0 if the num of 1’s in the data bits is even. ---[Odd---] Parity bit is 0 if num of 1’s in the data bits is odd. 

---[Mark---] Parity bit is always 1---.   
---[Space---] Parity bit is always 0---. 

**Stop Bits** 

Stop bits indicate the end of a serial data packet. (A start bit indicates the beginning.)  The standard setting is 1 stop bit. Communication with slow devices may require more  than 1 stop bit. 

Configuration options: ---[1---] ---[2---] 

**Flow Control** 

Flow control can prevent data loss from buffer overflow. When sending data, if the  receiving buffers are full, a “stop” signal can be sent to stop the data flow. Once the  buffers are empty, a “start” signal can be sent to re-start the flow. Hardware flow  control uses two wires to send start/stop signals. 

Configuration options: ---[None---] ---[Hardware RTS/CTS---] 

**VT ----UTF8 Combo Key Support** 

This allows you to enable the VT ----UTF8 Combination Key Support for ANSI/VT100  terminals. 

Configuration options: ---[Disabled---] ---[Enabled---] 

**Recorder Mode**  

With this mode enabled only text will be sent. This is to capture Terminal data. Configuration options: ---[Disabled---] ---[Enabled---] 

**Resolution 100x31** 

This allows you enable or disable extended terminal solution.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**Putty Keypad** 

This allows you to select the FunctionKey and Keypad on Putty.   
Configuration options: ---[VT100---] ---[LINUX---] ---[XTERMR6---] ---[SCO---] ---[ESCN---] ---[VT400---] **Legacy Console Redirection Settings** 

**Redirection COM Port** 

Allows you to select a COM port to display redirection of Legacy OS and Legacy  OPROM Messages. 

Configuration options: ---[COM0---] ---[COM1(Pci Bus0, Dev22, Func3, Port0)---] **Resolution**   
This allows you to set the number of rows and columns supported on the Legacy OS. Configuration options: ---[80x24---] ---[80x25---]

Pro WS W680-ACE Series BIOS Manual 53   
**Redirection After POST** 

The default setting for this option is set to ---[Always Enable---].   
---[Always Enable---] Legacy Console Redirection is enabled for legacy OS. ---[Bootloader---] The legacy Console Redirection is disabled before booting to  legacy OS. 

**7.12 Intel TXT Information** 

You may view the Intel TXT information in this menu. 

**7.13 PCI Subsystem Settings** 

The items in this menu allows you to configure PCI, PCI-X, and PCI Express Settings.   
**Above 4G Decoding** 

Allows you to enable or disable 64-bit capable devices to be decoded in above 4G address  space. It only works if the system supports 64-bit PCI decoding. 

Configuration options: ---[Disabled---] ---[Enabled---] 

• Only enabled under 64bit operating system. 

• The following item appears only when **Above 4G Decoding** is set to **---[Enabled---]**. 

**Re-Size BAR Support** 

If system has Resizable BAR capable PCIe Devices, this option enables or disables  Resizable BAR Support. 

Configuration options: ---[Disabled---] ---[Enabled---]------

54 Pro WS W680-ACE Series BIOS Manual   
**SR-IOV Support** 

Allows you to enable or disable Single Root IO Virtualization Support if the system has SR IOV capable PCIe devices. 

Configuration options: ---[Disabled---] ---[Enabled---] 

**7.14 USB Configuration** 

The items in this menu allow you to change the USB-related features. 

The **Mass Storage Devices** item shows the auto-detected values. If no USB device is  detected, the item shows **None**. 

**Legacy USB Support** 

---[Enabled---] Enabled the Legacy USB support. 

---[Disabled---] USB devices are available only for EFI applications. 

---[Auto---] Automatically disabled the Legacy USB support if no USB devices are  connected. 

**XHCI Hand-off** 

This is a workaround for OSes without XHCI hand-off support. The XHCI ownership change  should be claimed by XHCI driver. 

---[Disabled---] Support XHCI by XHCI drivers for operating systems with XHCI support.  ---[Enabled---] Support XHCI by BIOS for operating systems without XHCI support.  

**Mass Storage Devices:** 

Allows you to select the mass storage device emulation type for devices connected. **---[Auto---]** enumerates devices according to their media format. Optical drives are emulated as **---[CD ROM---]**, drives with no media will be emulated according to a drive type. Configuration options: ---[Auto---] ---[Floppy---] ---[Forceg/4vu84d FDD---] ---[Hard Disk---] ---[CD-ROM---] 

**USB Single Port Control** 

Allows you to enable or disable the individual USB ports. 

Refer to section **Rear panel features** in your motherboard’s user manual for the location  of the USB ports.

Pro WS W680-ACE Series BIOS Manual 55   
**7.15 Network Stack Configuration** 

The items in this menu allow you to change the Network Stack Configuration.   
**Network stack** 

Configuration options: ---[Disable---] ---[Enable---]  

The following items appear only when **Network Stack** is set to **---[Enabled---]**. 

**Ipv4/Ipv6 PXE Support** 

Allows you to enable or disable the Ipv4/Ipv6 PXE wake event.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**7.16 NVMe Configuration** 

This menu displays the NVMe controller and Drive information of the connected devices.  You may press ---<Enter---> on a connected NVMe device which appears in this menu to view  more information on the NVMe device. 

The options displayed in this menu may vary depending on the devices connected to your  motherboard. Please refer to the BIOS of your motherboard for the actual settings and  options.

56 Pro WS W680-ACE Series BIOS Manual   
**7.17 HDD/SSD SMART Information** 

The items in this menu allow you to view the SMART information for connected storage  devices. 

The options displayed in this menu may vary depending on the devices connected to your  motherboard. Please refer to the BIOS of your motherboard for the actual settings and  options. 

NVM Express devices do not support SMART information. 

**7.18 APM Configuration** 

The items in this menu allow you to change the advanced power management settings.  
**Restore AC Power Loss** 

Allows your system to go to ON state, OFF state, or both states after an AC power loss.  When setting your system to **---[Last State---]**, it goes to the previous state before the AC power  loss. 

Configuration options: ---[Power Off---] ---[Power On---] ---[Last State---] 

**Max Power Saving** 

Configuration options: ---[Disabled---] ---[Enabled---] 

**ErP Ready** 

Allows you to switch off some power at S4+S5 or S5 to get the system ready for ErP  requirement. When set to **---[Enabled---]**, all other PME options are switched off. RGB LEDs and  RGB/Addressable RGB Headers will also be disabled. 

Configuration options: ---[Disabled---] ---[Enabled (S4+S5)---] ---[Enabled (S5)---] 

**Power On By PCI-E** 

Allows you to enable or disable the Wake-on-LAN function of the onboard LAN controller or  other installed PCI-E LAN cards. 

Configuration options: ---[Disabled---] ---[Enabled---] 

Pro WS W680-ACE Series BIOS Manual 57   
**Power On By RTC** 

Allows you to enable or disable the RTC (Real-Time Clock) to generate a wake event and  configure the RTC alarm date. When enabled, you can set the days, hours, minutes, or  seconds to schedule an RTC alarm date. 

Configuration options: ---[Disabled---] ---[Enabled---]

**7.19 Onboard Devices Configuration** 

The items in this menu allow you to change the onboard devices settings. Scroll down to  view the other BIOS items. 

**PCIe Bandwidth Bifurcation Configuration** 

---[Auto Mode---] Run full PCIe X16 mode. 

---[X8/X8 mode---] Split up PCIEX16(G5)---_1 that runs at X16 into X8/X8. 

**HD Audio** 

---[Disabled---] HDA will be unconditionally disabled. 

---[Enabled---] HDA will be unconditionally enabled. 

**Intel 2.5G LAN1/2** 

Configuration options: ---[Disabled---] ---[Enabled---] 

**Connectivity mode (Wi-Fi & Bluetooth)** 

Configuration options: ---[Disabled---] ---[Enabled---] 

**Onboard LED** 

Allows you to turn on or off the HDD and PLED LEDs.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**Q-Code LED Function** 

---[Disabled---] Turn off Q-Code LED. 

---[POST Code Only---] Show POST (Power-On Self-Test) code on Q-Code LED. ---[Auto---] Automatically display POST (Power-On Self-Test) code and  CPU temperature on Q-Code LED. 

58 Pro WS W680-ACE Series BIOS Manual   
**SlimSAS Configuration** 

---[SATA mode---] Only supports SlimSAS SATA devices. 

---[PCIE mode---] Only supports SlimSAS PCIE devices. 

**U32G2---_C1 Type C Power Mode** 

---[Auto---] Power will be provided to USB 3.2 Gen 2 Type-C ports automatically when  a device is detected. 

---[Enabled---] Power will always be on for USB 3.2 Gen 2 Type-C ports. Improper connection may damage the system permanently. 

**GNA Device** 

Configuration options: ---[Enabled---] ---[Disabled---] 

**Serial Port Configuration** 

This submenu allows you to set parameters for Serial Port. 

**Serial Port** 

Configuration options: ---[Enabled---] ---[Disabled---] 

The following item appears only when **Serial Port** is set to **---[Enabled---]**. 

**Change settings** 

Allows you to select an optimal setting for super IO device.   
Configuration options: ---[IO=3F8h; IRQ=4---] ---[IO=2F8h; IRQ=3---] ---[IO=3E8h; IRQ=4---]  ---[IO=2E8h; IRQ=3---] 

**Parallel Port Configuration** 

This submenu allows you to set parameters for Parallel Port (LPT/LPTE). **Parallel Port**   
Configuration options: ---[Disabled---] ---[Enabled---] 

The following item appears only when **Parallel Port** is set to **---[Enabled---]**. 

**Change settings** 

Allows you to select an optimal setting for super IO device.   
Configuration options: ---[Auto---] ---[IO=378h; IRQ=5---] ---[IO=378h; IRQ=5,6,7,9,10,11,12---]  ---[IO=278h; IRQ=5,6,7,9,10,11,12---]  

**Device Mode** 

Allows you to change the Printer Port mode.   
Configuration options: ---[STD Printer Mode---] ---[SPP Mode---] ---[EPP-1.9 and SPP Mode---] ---[EPP-1.7 and SPP Mode---] ---[ECP Mode---] ---[ECP and EPP-1.9 Mode---]  

---[ECP and EPP-1.7 Mode---]

Pro WS W680-ACE Series BIOS Manual 59   
**7.20 Intel(R) Rapid Storage Technology** 

The items in this menu allow you manage RAID volumes on the Intel(R) RAID Controller. 

• The settings and options of this menu may vary depending on the storage devices  connected. Please refer to the BIOS of your motherboard for the actual settings and    
options. 

• Ensure to set the VMD configuration settings before using Intel(R) Rapid Storage  Technology to create a RAID set.

60 Pro WS W680-ACE Series BIOS Manual   
**8---. Monitor menu** 

The Monitor menu displays the system temperature/power status, and allows you to change  the fan settings. Scroll down to display the other BIOS items. 

**Temperature Monitor**   
**CPU Temperature, CPU Package Temperature, MotherBoard Temperature,  VRM Temperature, Chipset Temperature, T---_Sensor Temperature, DIMM A1-2  Temperature, DIMM B1-2 Temperature ---[xxx°C/xxx°F---]** 

The onboard hardware monitor automatically detects and displays the temperatures  for the different components. Select **---[Ignore---]** if you do not wish to display the detected  temperatures. 

**Fan Speed Monitor** 

**CPU Fan Speed, CPU Optional Fan Speed, Chassis Fan 1-3 Speed, Water  Pump+ Speed ---[xxxx RPM---]** 

The onboard hardware monitor automatically detects and displays the fan speeds in  rotations per minute (RPM). If the fan is not connected to the motherboard, the field  shows N/A. Select **---[Ignore---]** if you do not wish to display the detected speed.

Pro WS W680-ACE Series BIOS Manual 61   
**Voltage and Current Monitor** 

**CPU Core Voltage, 12V Voltage, 5V Voltage, 3.3V Voltage, Memory Controller  Voltage ---[x.xxx V---]** 

The onboard hardware monitor automatically detects the voltage output through the  onboard voltage regulators. Select **---[Ignore---]** if you do not want to detect this item. 

**CPU Core Current ---[xx A---]** 

The onboard hardware monitor automatically detects the current output. Select  **---[Ignore---]** if you do not want to detect this item. 

**Q-Fan Configuration** 

**Q-Fan Tuning** 

Click this item to automatically detect the lowest speed and configure the minimum  duty cycle for each fan. 

The process may take 2 to 5 minutes. DO NOT shut down or reset your system during the  tuning process. 

**CPU Q-Fan Control** 

Allows you to set the CPU Q-Fan operating mode. 

---[Auto Detect---] Detects the type of installed fan/pump and automatically switches  the control modes. 

---[DC Mode---] Enables the Q-Fan Control feature in DC mode for 3-pin fan/pump. ---[PWM Mode---] Enables the Q-Fan Control feature in PWM mode for 4-pin fan/ pump. 

**CPU Fan Profile** 

Allows you to set the appropriate performance level of the assigned fan/pump. When  selecting **---[Manual---]**, we suggest raising your fan/pump duty to 100% if your CPU  temperature exceeds 75°C. Please be noted CPU performance will throttle due to  overheating with inefficient fan/pump duty. 

Configuration options: ---[Standard---] ---[Silent---] ---[Turbo---] ---[Full Speed---] ---[Manual---] 

The following items appear only when **CPU Fan Profile** is set to **---[Standard---]**, **---[Silent---]**,  **---[Turbo---]**, or **---[Manual---].** 

**CPU Fan Step Up** 

Step up allows you to adjust how quickly the fan rotation speed changes, with level 0  being an instantaneous change in speed. The higher the level, the slower the change  in speed, and may also result in less noise, but this will also cause slower heat  dissipation. 

Configuration options: ---[Level 0---] ---[Level 1---] ---[Level 2---] ---[Level 3---] ---[Level 4---] ---[Level 5---] **CPU Fan Step Down**   
Step down allows you to adjust how quickly the fan rotation speed changes, with  level 0 being an instantaneous change in speed. The higher the level, the slower the  change in speed, which may result in longer period with more noise. 

Configuration options: ---[Level 0---] ---[Level 1---] ---[Level 2---] ---[Level 3---] ---[Level 4---] ---[Level 5---]

62 Pro WS W680-ACE Series BIOS Manual   
**CPU Fan Speed Low Limit** 

Allows you to set the lower speed limit for assigned fan/pump. A warning message will  appear when the limit is reached; the warning message will not appear if **---[Ignore---]** is  selected. 

Configuration options: ---[Ignore---] ---[200 RPM---] ---[300 RPM---] ---[400 RPM---] ---[500 RPM---] ---[600  RPM---] 

The following items appear only when **CPU Fan Profile** is set to **---[Manual---].** 

**CPU Fan Upper Temperature** 

Configure the fan/pump upper temperature to make assigned fan/pump operate at the  max. duty cycle when the source temperature reaches the limit. We suggest raising  your fan/pump duty to 100% if your CPU temperature exceeds 75°C. Please be noted  CPU performance will throttle due to overheating with inefficient fan/pump duty. Use  the ---<+---> or ---<----> keys to adjust the upper temperature.  

The fan/pump upper temperature cannot be lower than the fan/pump lower temperature. 

**CPU Fan Max. Duty Cycle (%)**  

Set the maximum fan/pump duty cycle of the assigned fan/pump for when the source  temp. reaches the upper limit. We suggest raising your fan/pump duty to 100% if your  CPU temperature exceeds 75°C. Please be noted CPU performance will throttle due  

to overheating with inefficient fan/pump duty. Use the ---<+---> or ---<----> keys to adjust the  fan/pump maximum duty cycle. 

**CPU Fan Middle Temperature** 

Configure the fan/pump middle temperature to make assigned fan/pump operate at  the mid. duty cycle when the source temperature is higher than the limit. We suggest  raising your fan/pump duty to 100% if your CPU temperature exceeds 75°C. Please  be noted CPU performance will throttle due to overheating with inefficient fan/pump  duty. Use the ---<+---> or ---<----> keys to adjust the middle temperature. 

**CPU Fan Middle. Duty Cycle (%)** 

Set the middle fan/pump duty cycle of the assigned fan/pump for when the source  temp. exceeds the middle temperature. We suggest raising your fan/pump duty to  100% if your CPU temperature exceeds 75°C. Please be noted CPU performance will  throttle due to overheating with inefficient fan/pump duty. Use the ---<+---> or ---<----> keys to  adjust the fan/pump middle duty cycle. 

**CPU Fan Lower Temperature** 

Configure the fan/pump lower temperature to make assigned fan/pump operate at  the min. duty cycle when the source temperature is lower than the limit. We suggest  raising your fan/pump duty to 100% if your CPU temperature exceeds 75°C. Please  be noted CPU performance will throttle due to overheating with inefficient fan/pump  duty. Use the ---<+---> or ---<----> keys to adjust the lower temperature. 

**CPU Fan Min. Duty Cycle(%)**  

Set the middle fan/pump duty cycle of the assigned fan/pump for when the source  temp. is lower than the lower temperature. We suggest raising your fan/pump duty to  100% if your CPU temperature exceeds 75°C. Please be noted CPU performance will  throttle due to overheating with inefficient fan/pump duty. Use the ---<+---> or ---<----> keys to  adjust the fan/pump minimum duty cycle.

Pro WS W680-ACE Series BIOS Manual 63   
**Chassis Fan(s) Configuration** 

**Chassis Fan 1-3 Q-Fan Control** 

Allows you to set the Chassis Fan 1-3 operating mode. 

---[Auto Detect---] Detects the type of installed fan/pump and automatically  switches the control modes.  

---[DC Mode---] Enables the Q-Fan Control feature in DC mode for 3-pin fan/ pump. 

---[PWM Mode---] Enables the Q-Fan Control feature in PWM mode for 4-pin fan/ pump. 

**Chassis Fan 1-3 Profile** 

Allows you to set the appropriate performance level of the assigned fan/pump. When  selecting **---[Manual---]**, we suggest raising your fan/pump duty to 100% if your CPU  temperature exceeds 75°C. Please be noted CPU performance will throttle due to  overheating with inefficient fan/pump duty.   
Configuration options: ---[Standard---] ---[Silent---] ---[Turbo---] ---[Full Speed---] ---[Manual---] 

The following items appear only when **CPU Fan 1-3 Profile** is set to **---[Standard---]**, **---[Silent---]**,  **---[Turbo---]**, or **---[Manual---].** 

**Chassis Fan 1-3 Q-Fan Source** 

The assigned fan/pump will be controlled according to the selected temperature source. Configuration options: ---[CPU---] ---[MotherBoard---] ---[VRM---] ---[Chipset---] ---[T---_Sensor---] ---[Multiple  Sources---] 

**Chassis Fan 1-3 Step Up** 

Step up allows you to adjust how quickly the fan rotation speed changes, with level 0  being an instantaneous change in speed. The higher the level, the slower the change in  speed, and may also result in less noise, but this will also cause slower heat dissipation. Configuration options: ---[Level 0---] ---[Level 1---] ---[Level 2---] ---[Level 3---] ---[Level 4---] ---[Level 5---] **Chassis Fan 1-3 Step Down** 

Step down allows you to adjust how quickly the fan rotation speed changes, with level 0  being an instantaneous change in speed. The higher the level, the slower the change in  speed, which may result in longer period with more noise.   
Configuration options: ---[Level 0---] ---[Level 1---] ---[Level 2---] ---[Level 3---] ---[Level 4---] ---[Level 5---] **Chassis Fan 1-3 Speed Low Limit**   
Allows you to set the lower speed limit for assigned fan/pump. A warning message will  appear when the limit is reached; the warning message will not appear if **---[Ignore---]** is  selected.   
Configuration options: ---[Ignore---] ---[200 RPM---] ---[300 RPM---] ---[400 RPM---] ---[500 RPM---] ---[600 RPM---] The following items appear only when **Chassis Fan 1-3 Profile** is set to **---[Manual---]**. 

**Chassis Fan 1-3 Upper Temperature** 

Configure the fan/pump upper temperature to make assigned fan/pump operate at the  max. duty cycle when the source temperature reaches the limit. We suggest raising your  fan/pump duty to 100% if your CPU temperature exceeds 75°C. Please be noted CPU  performance will throttle due to overheating with inefficient fan/pump duty. Use the ---<+---> or  ---<----> keys to adjust the upper temperature.  

The fan/pump upper temperature cannot be lower than the fan/pump lower temperature.

64 Pro WS W680-ACE Series BIOS Manual   
**Chassis Fan 1-3 Max. Duty Cycle (%)**  

Set the maximum fan/pump duty cycle of the assigned fan/pump for when the source  temp. reaches the upper limit. We suggest raising your fan/pump duty to 100% if your  CPU temperature exceeds 75°C. Please be noted CPU performance will throttle due to  overheating with inefficient fan/pump duty. Use the ---<+---> or ---<----> keys to adjust the fan/ pump maximum duty cycle. 

**Chassis Fan 1-3 Middle Temperature** 

Configure the fan/pump middle temperature to make assigned fan/pump operate at the  mid. duty cycle when the source temperature is higher than the limit. We suggest raising  your fan/pump duty to 100% if your CPU temperature exceeds 75°C. Please be noted  CPU performance will throttle due to overheating with inefficient fan/pump duty. Use the  ---<+---> or ---<----> keys to adjust the middle temperature. 

**Chassis Fan 1-3 Middle. Duty Cycle (%)** 

Set the middle fan/pump duty cycle of the assigned fan/pump for when the source temp.  exceeds the middle temperature. We suggest raising your fan/pump duty to 100% if your  CPU temperature exceeds 75°C. Please be noted CPU performance will throttle due to  overheating with inefficient fan/pump duty. Use the ---<+---> or ---<----> keys to adjust the fan/ pump middle duty cycle. 

**Chassis Fan 1-3 Lower Temperature** 

Configure the fan/pump lower temperature to make assigned fan/pump operate at the  min. duty cycle when the source temperature is lower than the limit. We suggest raising  your fan/pump duty to 100% if your CPU temperature exceeds 75°C. Please be noted  CPU performance will throttle due to overheating with inefficient fan/pump duty. Use the  ---<+---> or ---<----> keys to adjust the lower temperature. 

**Chassis Fan 1-3 Min. Duty Cycle(%)**  

Set the middle fan/pump duty cycle of the assigned fan/pump for when the source temp.  is lower than the lower temperature. We suggest raising your fan/pump duty to 100% if  your CPU temperature exceeds 75°C. Please be noted CPU performance will throttle due  to overheating with inefficient fan/pump duty. Use the ---<+---> or ---<----> keys to adjust the fan/ pump minimum duty cycle. 

**Allow Fan Stop** 

This function allows the fan to run at 0% duty cycle when the temperature of the source is  dropped below the lower temperature.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**Water Pump+ Q-Fan Control** 

Allows you to set the Water Pump+ operating mode. 

---[Auto Detect---] Detects the type of installed fan/pump and automatically switches  the control modes. 

---[DC Mode---] Enables the Q-Fan Control feature in DC mode for 3-pin fan/pump. ---[PWM Mode---] Enables the Q-Fan Control feature in PWM mode for 4-pin fan/ pump. 

**Water Pump+ Profile** 

Allows you to set the appropriate performance level of the assigned fan/pump. When  selecting **---[Manual---]**, we suggest raising your fan/pump duty to 100% if your CPU  temperature exceeds 75°C. Please be noted CPU performance will throttle due to  overheating with inefficient fan/pump duty. 

Configuration options: ---[Standard---] ---[Silent---] ---[Turbo---] ---[Full Speed---] ---[Manual---]

Pro WS W680-ACE Series BIOS Manual 65   
The following items appear only when **Water Pump+ Profile** is set to **---[Standard---]**, **---[Silent---]**,  **---[Turbo---]**, or **---[Manual---].** 

**Water Pump+ Q-Fan Source** 

The assigned fan/pump will be controlled according to the selected temperature  source. 

Configuration options: ---[CPU---] ---[MotherBoard---] ---[VRM---] ---[Chipset---] ---[T---_Sensor---] ---[Multiple  Sources---] 

**Water Pump+ Step Up** 

Step up allows you to adjust how quickly the fan rotation speed changes, with level 0  being an instantaneous change in speed. The higher the level, the slower the change  in speed, and may also result in less noise, but this will also cause slower heat  dissipation. 

Configuration options: ---[Level 0---] ---[Level 1---] ---[Level 2---] ---[Level 3---] ---[Level 4---] ---[Level 5---] **Water Pump+ Step Down**   
Step down allows you to adjust how quickly the fan rotation speed changes, with  level 0 being an instantaneous change in speed. The higher the level, the slower the  change in speed, which may result in longer period with more noise. Configuration options: ---[Level 0---] ---[Level 1---] ---[Level 2---] ---[Level 3---] ---[Level 4---] ---[Level 5---] 

**Water Pump+ Speed Low Limit** 

Allows you to set the lower speed limit for assigned fan/pump. A warning message will  appear when the limit is reached; the warning message will not appear if **---[Ignore---]** is  selected. 

Configuration options: ---[Ignore---] ---[200 RPM---] ---[300 RPM---] ---[400 RPM---] ---[500 RPM---] ---[600  RPM---] 

The following items appear only when **Water Pump+ Profile** is set to **---[Manual---].** 

**Water Pump+ Upper Temperature** 

Configure the fan/pump upper temperature to make assigned fan/pump operate at the  max. duty cycle when the source temperature reaches the limit. We suggest raising  your fan/pump duty to 100% if your CPU temperature exceeds 75°C. Please be noted  CPU performance will throttle due to overheating with inefficient fan/pump duty. Use  the ---<+---> or ---<----> keys to adjust the upper temperature.  

The fan/pump upper temperature cannot be lower than the fan/pump lower temperature. 

**Water Pump+ Max. Duty Cycle (%)**  

Set the maximum fan/pump duty cycle of the assigned fan/pump for when the source  temp. reaches the upper limit. We suggest raising your fan/pump duty to 100% if your  CPU temperature exceeds 75°C. Please be noted CPU performance will throttle due  

to overheating with inefficient fan/pump duty. Use the ---<+---> or ---<----> keys to adjust the  fan/pump maximum duty cycle.

66 Pro WS W680-ACE Series BIOS Manual   
**Water Pump+ Middle Temperature** 

Configure the fan/pump middle temperature to make assigned fan/pump operate at  the mid. duty cycle when the source temperature is higher than the limit. We suggest  raising your fan/pump duty to 100% if your CPU temperature exceeds 75°C. Please  be noted CPU performance will throttle due to overheating with inefficient fan/pump  duty. Use the ---<+---> or ---<----> keys to adjust the middle temperature. 

**Water Pump+ Middle. Duty Cycle (%)** 

Set the middle fan/pump duty cycle of the assigned fan/pump for when the source  temp. exceeds the middle temperature. We suggest raising your fan/pump duty to  100% if your CPU temperature exceeds 75°C. Please be noted CPU performance will  throttle due to overheating with inefficient fan/pump duty. Use the ---<+---> or ---<----> keys to  adjust the fan/pump middle duty cycle. 

**Water Pump+ Lower Temperature** 

Configure the fan/pump lower temperature to make assigned fan/pump operate at  the min. duty cycle when the source temperature is lower than the limit. We suggest  raising your fan/pump duty to 100% if your CPU temperature exceeds 75°C. Please  be noted CPU performance will throttle due to overheating with inefficient fan/pump  duty. Use the ---<+---> or ---<----> keys to adjust the lower temperature. 

**Water Pump+ Min. Duty Cycle(%)**  

Set the middle fan/pump duty cycle of the assigned fan/pump for when the source  temp. is lower than the lower temperature. We suggest raising your fan/pump duty to  100% if your CPU temperature exceeds 75°C. Please be noted CPU performance will  throttle due to overheating with inefficient fan/pump duty. Use the ---<+---> or ---<----> keys to  adjust the fan/pump minimum duty cycle.

Pro WS W680-ACE Series BIOS Manual 67   
**9---. Boot menu** 

The Boot menu items allow you to change the system boot options. 

**CSM (Compatibility Support Module)** 

Allows you to configure the CSM (Compatibility Support Module) items to fully support the  various VGA, bootable devices and add-on devices for better compatibility. 

**Launch CSM** 

---[Enabled---] For better compatibility, enable the CSM to fully support the non UEFI driver add-on devices or the Windows® UEFI mode. 

---[Disabled---] Disable the CSM to fully support the non-UEFI driver add-on devices  or the Windows® UEFI mode. 

The following items appear only when **Launch CSM** is set to **---[Enabled---]**. 

**Boot Device Control** 

Allows you to select the type of devices that you want to boot.   
Configuration options: ---[UEFI and Legacy OPROM---] ---[Legacy OPROM only---] ---[UEFI only---] **Boot from Network Devices**   
Allows you to select the type of network devices that you want to launch.   
Configuration options: ---[Ignore---] ---[UEFI only---] ---[Legacy only---] 

**Boot from Storage Devices** 

Allows you to select the type of storage devices that you want to launch. 

Configuration options: ---[Ignore---] ---[UEFI only---] ---[Legacy only---]

68 Pro WS W680-ACE Series BIOS Manual   
**Boot from PCI-E/PCI Expansion Devices** 

Allows you to select the type of PCI-E/PCI expansion devices that you want to launch. Configuration options: ---[UEFI only---] ---[Legacy only---]  

**Secure Boot** 

Allows you to configure the Windows® Secure Boot settings and manage its keys to protect  the system from unauthorized access and malwares during POST. 

**OS Type** 

---[Windows UEFI Mode---] This item allows you to select your installed operating  system. Execute the Microsoft® Secure Boot check. Only  

select this option when booting on Windows® UEFI mode    
or other Microsoft® Secure Boot compliant OS. 

---[Other OS---] Get the optimized function when booting on Windows® non-UEFI mode. Microsoft® Secure Boot only supports  

Windows® UEFI mode. 

The Microsoft secure boot can only function properly on Windows UEFI mode. 

**Secure Boot Mode** 

This option allows you to select the Secure Boot mode from between Standard or  Custom. In Custom mode, Secure Boot Policy variables can be configured by a  physically present user without full authentication. 

Configuration options: ---[Standard---] ---[Custom---] 

The following item appears only when **Secure Boot Mode** is set to **---[Custom---]**. 

**Key Management** 

**Install Default Secure Boot keys** 

Allows you to immediately load the default Security Boot keys, Platform key (PK), Key exchange Key (KEK), Signature database (db), and Revoked Signatures (dbx). When  the default Secure boot keys are loaded, the PK state will change from Unloaded  mode to loaded mode. 

**Clear Secure Boot keys** 

This item appears only when you load the default Secure Boot keys. Allows you to  clear all default Secure Boot keys. 

**Save all Secure Boot variables** 

Allows you to save all secure boot keys to a USB storage device. 

**PK Management** 

The Platform Key (PK) locks and secures the firmware from any permissible changes.  The system verifies the PK before your system enters the OS.  

**Save To File** 

Allows you to save the PK to a USB storage device. 

**Set New key** 

Allows you to load the downloaded PK from a USB storage device.

Pro WS W680-ACE Series BIOS Manual 69   
**Delete key** 

Allows you to delete the PK from your system. Once the PK is deleted, all the system’s  Secure Boot keys will not be active.   
Configuration options: ---[Yes---] ---[No---] 

The PK file must be formatted as a UEFI variable structure with time-based authenticated  variable. 

**KEK Management** 

The KEK (Key-exchange Key or Key Enrollment Key) manages the Signature  database (db) and Revoked Signature database (dbx). 

Key-exchange Key (KEK) refers to Microsoft® Secure Boot Key-Enrollment Key (KEK). 

**Save to file** 

Allows you to save the KEK to a USB storage device. 

**Set New key** 

Allows you to load the downloaded KEK from a USB storage device. 

**Append Key** 

Allows you to load the additional KEK from a storage device for an additional db and dbx  loaded management. 

**Delete key** 

Allows you to delete the KEK from your system.   
Configuration options: ---[Yes---] ---[No---] 

The KEK file must be formatted as a UEFI variable structure with time-based  authenticated variable. 

**DB Management** 

The db (Authorized Signature database) lists the signers or images of UEFI  applications, operating system loaders, and UEFI drivers that you can load on the  single computer. 

**Save to file** 

Allows you to save the db to a USB storage device. 

**Set New key** 

Allows you to load the downloaded db from a USB storage device. 

**Append Key** 

Allows you to load the additional db from a storage device for an additional db and dbx  loaded management. 

**Delete key** 

Allows you to delete the db file from your system.   
Configuration options: ---[Yes---] ---[No---] 

The db file must be formatted as a UEFI variable structure with time-based authenticated  variable.

70 Pro WS W680-ACE Series BIOS Manual   
**DBX Management** 

The dbx (Revoked Signature database) lists the forbidden images of db items that are  no longer trusted and cannot be loaded. 

**Save to file** 

Allows you to save the dbx to a USB storage device. 

**Set New key** 

Allows you to load the downloaded dbx from a USB storage device. 

**Append Key** 

Allows you to load the additional dbx from a storage device for an additional db and dbx  loaded management. 

**Delete key** 

Allows you to delete the dbx file from your system.   
Configuration options: ---[Yes---] ---[No---] 

The dbx file must be formatted as a UEFI variable structure with time-based authenticated  variable. 

**Boot Configuration** 

**Fast Boot** 

Allows you to enable or disable boot with initialization of a minimal set of devices  required to launch active boot option. Has no effect for BBS boot options. Configuration options: ---[Disabled---] ---[Enabled---] 

The following item appears only when **Fast Boot** is set to **---[Enabled---]**. 

**Next Boot after AC Power Loss** 

---[Normal Boot---] Returns to normal boot on the next boot after an AC power loss. ---[Fast Boot---] Accelerates the boot speed on the next boot after an AC power  loss. 

**Boot Logo Display** 

---[Auto---] Automatically adjust the boot logo size for Windows  requirements. 

---[Full Screen---] Maximize the boot logo size.   
---[Disabled---] Hide the logo during POST. 

The following item appears only when **Boot Logo Display** is set to **---[Auto---]** or **---[Full  Screen---]**. 

**Post Delay Time** 

Allows you to select a desired additional POST waiting time to easily enter the BIOS  Setup. You can only execute the POST delay time during normal boot. Configuration options: ---[0 sec---] ---- ---[10 sec---] 

This feature only works when set under normal boot.

Pro WS W680-ACE Series BIOS Manual 71   
The following item appears only when **Boot Logo Display** is set to **---[Disabled---]**. 

**Post Report** 

Allows you to select a desired POST report waiting time or until ESC is pressed. Configuration options: ---[1 sec---] ---- ---[10 sec---] ---[Until Press ESC---] 

**Boot up NumLock State** 

Allows you to select the keyboard NumLock state.   
Configuration options: ---[On---] ---[Off---] 

**Wait For ‘F1’ If Error** 

Allows your system to wait for the ---<F1---> key to be pressed when error occurs. Configuration options: ---[Disabled---] ---[Enabled---] 

**Option ROM Messages** 

---[Force BIOS---] The Option ROM Messages will be shown during the POST. ---[Keep Current---] Only the ASUS logo will be shown during the POST.  **Interrupt 19 Capture** 

Enable this item to allow the option ROMs to trap the interrupt 19---.   
Configuration options: ---[Enabled---] ---[Disabled---] 

**AMI Native NVMe Driver Support** 

Allows you to enable or disable AMI Native NVMe driver.   
Configuration options: ---[Disabled---] ---[Enabled---] 

**Boot Sector (MBR/GPT) Recovery Policy** 

---[Auto Recovery---] Follow UEFI Rule.   
---[Local User Control---] You can enter setup page and select Boot Sector (MBR/GPT)  Recovery Policy to recovery MBR/GPT on the next boot time. 

The following item appears only when **Boot Sector (MBR/GPT) Recovery Policy** is set to  **---[Local User Control---]**. 

**Next Boot Recovery Action** 

Choose the (MBR/GPT) recovery action on the next boot.   
Configuration options: ---[Skip---] ---[Recovery---] 

**Boot Option Priorities** 

These items specify the boot device priority sequence from the available devices. The  number of device items that appears on the screen depends on the number of devices  installed in the system. 

• To access Windows® OS in Safe Mode, press ---<F8 ---> after POST (Windows® 8 not  supported).  

• To select the boot device during system startup, press ---<F8---> when ASUS Logo  appears.

72 Pro WS W680-ACE Series BIOS Manual   
**Boot Override** 

These item displays the available devices. The number of device items that appear on the  screen depends on the number of devices installed in the system. Click an item to start  booting from the selected device.

Pro WS W680-ACE Series BIOS Manual 73   
**10---. Tool menu** 

The Tool menu items allow you to configure options for special functions. Select an item  then press ---<Enter---> to display the submenu. 

**BIOS Image Rollback Support** 

---[Enabled---] Support roll back your BIOS to a previous version, but this setting violates  the NIST SP 800-147 requirement. 

---[Disabled---] Only support updating your BIOS to a newer version, and this setting  meets the NIST SP 800-147 requirement. 

**Publish HII Resources** 

Configuration options: ---[Disabled---] ---[Enabled---] 

**Flexkey** 

---[Reset---] Reboots the system. 

---[DirectKey---] Boot directly into the BIOS. 

**Start ASUS EzFlash** 

Allows you to run ASUS EzFlash BIOS ROM Utility when you press ---<Enter--->. Refer to the  **ASUS EzFlash Utility** section for details. 

**IPMI Hardware Monitor** 

Allows you to view the IPMI Hardware Monitor when you press ---<Enter--->.

74 Pro WS W680-ACE Series BIOS Manual   
**10.1 ASUS User Profile** 

This item allows you to store or load multiple BIOS settings.  

**Load from Profile** 

Allows you to load the previous BIOS settings saved in the BIOS Flash. Key in the profile  number that saved your BIOS settings, press ---<Enter--->, and then select **Yes**. 

• DO NOT shut down or reset the system while updating the BIOS to prevent the  system boot failure---! 

• We recommend that you update the BIOS file only coming from the same memory/ CPU configuration and BIOS version. 

**Profile Name** 

Allows you to key in a profile name.  

**Save to Profile** 

Allows you to save the current BIOS settings to the BIOS Flash, and create a profile. Key in  a profile number from one to eight, press ---<Enter--->, and then select **Yes**.

Pro WS W680-ACE Series BIOS Manual 75   
**10.2 ASUS SPD Information** 

This item allows you to view the DRAM SPD information. 

**10.3 ASUS Armoury Crate** 

This item allows you to enable or disable downloading and installing of the Armoury Crate  app in the Windows® OS. The Armoury Crate app can help you manage and download the  latest drivers and utilities for your motherboard. 

**Download & Install ARMOURY CRATE app**   
Configuration options: ---[Disabled---] ---[Enabled---]

76 Pro WS W680-ACE Series BIOS Manual   
**11---. IPMI menu** 

The IPMI menu items allow you to configure IPMI settings. 

**OS Watchdog Timer** 

When this option is set to **---[Enabled---]** it starts a BIOS timer which can only be shut off by  Management Software after the OS loads. Helps determine if the OS successfully loaded or  follows the OS Boot Watchdog Timer policy. 

Configuration options: ---[Enabled---] ---[Disabled---] 

The following items appear only when **OS Watchdog Timer** is set to **---[Enabled---]**. 

**OS Wtd Timer Timeout** 

Enter a value between 1 and 30 min for OS Boot Watchdog Expiration, Not available if OS  Boot Watchdog Timer is disabled. 

Configuration options: ---[1---] ---- ---[30---] 

**OS Wtd Timer Policy** 

This item allows you to configure the how the system should respond if the OS Boot Watch  Timer expires. 

Configuration options: ---[Do Nothing---] ---[Reset---] ---[Power Down---] ---[Power Cycle---] 

**MLED light Synchronizing** 

Allows you to synchronize the message LED light with the left RJ45 LED of the IPMI  Expansion card. 

Configuration options: ---[Disabled---] ---[Enabled---]

Pro WS W680-ACE Series BIOS Manual 77   
**BMC---_LED light synchronizing** 

Allows you to synchronize the BMC LED light with the right RJ45 LED of the IPMI Expansion  card. 

Configuration options: ---[Disabled---] ---[Enabled---] 

**In-Band driver type** 

Configuration options: ---[Windows---] ---[Linux---] 

**11.1 System Event Log** 

Allows you to change the SEL event log configuration. 

All values changed here do not take effect until computer is restarted.

**Erase SEL**   
Allows you to choose options for erasing SEL.   
Configuration options: ---[No---] ---[Yes, On next reset---] ---[Yes, On every reset---] 

78 Pro WS W680-ACE Series BIOS Manual   
**11.2 BMC network configuration** 

The sub-items in this configuration allow you to configure the BMC network parameters.**Configure IPV4 support** 

**LAN channel 1** 

**Configuration Address source** 

This item allows you to configure LAN channel parameters statistically or dynamically (by  BIOS or BMC). **---[Unspecified---]** option will not modify any BMC network parameters during  BIOS phase. 

Configuration options: ---[Unspecified---] ---[Static---] ---[DynamicBmcDhcp---] 

The following items are available only when **Configuration Address source** is set to  **---[Static---]**. 

**Station IP address** 

Allows you to set the station IP address. 

**Subnet mask** 

Allows you to set the subnet mask. We recommend that you use the same Subnet Mask you  have specified on the operating system network for the used network card. 

**Router IP Address** 

Allows you to set the router IP address. 

**Router MAC Address** 

Allows you to set the router MAC address. 

Pro WS W680-ACE Series BIOS Manual 79   
**Configure IPV6 support** 

**LAN channel 1** 

**IPV6 support**  

Allows you to enable or disable IPV6 support.   
Configuration options: ---[Enabled---] ---[Disabled---] 

The following items appear only when **IPV6 support** is set to **---[Enabled---]**. 

**Configuration Address source** 

Allows you to set the LAN channel parameters statically or dynamically (by BIOS or by  BMC). **---[Unspecified---]** option will not modify any BMC network parameters during BIOS  phase.  

Configuration options: ---[Unspecified---] ---[Static---] ---[DynamicBmcDhcp---] 

The following items are available only when **Configuration Address source** is set to  ---[Static---]. 

**Station IPV6 address** 

Allows you to set the station IPV6 address. 

**Prefix Length** 

Allows you to set the prefix length (maximum of Prefix Length is 128). 

**Configuration Router Lan1 Address source** 

Allows you to set the LAN channel parameters statically or dynamically (by BIOS or  by BMC). **---[Unspecified---]** option will not modify any BMC network parameters during  BIOS phase.  

Configuration options: ---[Unspecified---] ---[Static---] ---[DynamicBmcDhcp---] 

The following items are available only when **Configuration Router Lan1 Address source** is set to **---[Static---]**. 

**IPV6 Router1 IP address** 

Allows you to set the IPV6 Router1 IP address. 

**IPV6 Router1 Prefix Length Lan1** 

Allows you to set the IPV6 Router1 prefix length (maximum of Prefix Length is 128). **IPV6 Router1 Prefix Value Lan1**   
Allows you to set the IPV6 Router1 prefix value.

80 Pro WS W680-ACE Series BIOS Manual 
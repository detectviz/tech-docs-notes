**Pro WS**  

**W680-ACE** 

**Pro WS**  

**W680-ACE IPMI**  
**Motherboard**   
E21247   
First Edition   
October 2022 

**Copyright © 2022 ASUSTeK COMPUTER INC. All Rights Reserved.**   
No part of this manual, including the products and software described in it, may be reproduced,  transmitted, transcribed, stored in a retrieval system, or translated into any language in any form or by  any means, except documentation kept by the purchaser for backup purposes, without the express  written permission of ASUSTeK COMPUTER INC. (“ASUS”).   
Product warranty or service will not be extended if: (1) the product is repaired, modified or altered, unless  such repair, modification of alteration is authorized in writing by ASUS; or (2) the serial number of the  product is defaced or missing.   
ASUS PROVIDES THIS MANUAL “AS IS” WITHOUT WARRANTY OF ANY KIND, EITHER EXPRESS  OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE IMPLIED WARRANTIES OR CONDITIONS OF  MERCHANTABILITY OR FITNESS FOR A PARTICULAR PURPOSE. IN NO EVENT SHALL ASUS, ITS  DIRECTORS, OFFICERS, EMPLOYEES OR AGENTS BE LIABLE FOR ANY INDIRECT, SPECIAL,  INCIDENTAL, OR CONSEQUENTIAL DAMAGES (INCLUDING DAMAGES FOR LOSS OF PROFITS,  LOSS OF BUSINESS, LOSS OF USE OR DATA, INTERRUPTION OF BUSINESS AND THE LIKE),  EVEN IF ASUS HAS BEEN ADVISED OF THE POSSIBILITY OF SUCH DAMAGES ARISING FROM  ANY DEFECT OR ERROR IN THIS MANUAL OR PRODUCT.   
SPECIFICATIONS AND INFORMATION CONTAINED IN THIS MANUAL ARE FURNISHED FOR  INFORMATIONAL USE ONLY, AND ARE SUBJECT TO CHANGE AT ANY TIME WITHOUT NOTICE,  AND SHOULD NOT BE CONSTRUED AS A COMMITMENT BY ASUS. ASUS ASSUMES NO  RESPONSIBILITY OR LIABILITY FOR ANY ERRORS OR INACCURACIES THAT MAY APPEAR IN  THIS MANUAL, INCLUDING THE PRODUCTS AND SOFTWARE DESCRIBED IN IT. Products and corporate names appearing in this manual may or may not be registered trademarks or  copyrights of their respective companies, and are used only for identification or explanation and to the  owners’ benefit, without intent to infringe.

ii   
**Contents** 

**Safety information....................................................................................................... v About this guide......................................................................................................... vi Pro WS W680-ACE specifications summary .......................................................... vii Package contents...................................................................................................... xii** 

**Chapter 1: Product Introduction** 

**1.1 Before you proceed ...................................................................................1-1 1.2 Motherboard layout....................................................................................1-2** 

**Chapter 2: Basic Installation** 

**2.1 Building your PC system...........................................................................2-1** 2.1.1 CPU installation...........................................................................2-1 2.1.2 Cooling system installation..........................................................2-3 2.1.3 DIMM installation.........................................................................2-6 2.1.4 M.2 installation ............................................................................2-7 2.1.5 Motherboard installation............................................................2-13 2.1.6 ATX power connection..............................................................2-14 2.1.7 SATA device connection...........................................................2-16 2.1.8 SlimSAS connection..................................................................2-16 2.1.9 Front I/O connector...................................................................2-17 2.1.10 Expansion card installation .......................................................2-18 2.1.11 M.2 Wi-Fi module and antenna installation...............................2-20 

**2.2 BIOS update utility...................................................................................2-21 2.3 Motherboard rear and audio connections .............................................2-23** 2.3.1 Rear I/O connection ..................................................................2-23 2.3.2 Audio I/O connections...............................................................2-25 **2.4 Starting up for the first time....................................................................2-27 2.5 Turning off the computer ........................................................................2-27** 

**Chapter 3: BIOS and RAID Support** 

**3.1 Knowing BIOS ............................................................................................3-1 3.2 BIOS setup program ..................................................................................3-2 3.3 ASUS EZ Flash 3 ........................................................................................3-3 3.4 ASUS CrashFree BIOS 3............................................................................3-4 3.5 RAID configurations ..................................................................................3-5**

iii   
**Appendix** 

**Q-Code table............................................................................................................ A-1 Notices .................................................................................................................... A-5 Warranty................................................................................................................. A-10 ASUS contact information.................................................................................... A-12 Service and Support ............................................................................................. A-12**

iv   
**Safety information** 

**Electrical safety** 

• To prevent electrical shock hazard, disconnect the power cable from the electrical  outlet before relocating the system. 

• When adding or removing devices to or from the system, ensure that the power cables  for the devices are unplugged before the signal cables are connected. If possible,  disconnect all power cables from the existing system before you add a device. 

• Before connecting or removing signal cables from the motherboard, ensure that all  power cables are unplugged. 

• Seek professional assistance before using an adapter or extension cord. These  devices could interrupt the grounding circuit. 

• Ensure that your power supply is set to the correct voltage in your area. If you are not  sure about the voltage of the electrical outlet you are using, contact your local power  company. 

• If the power supply is broken, do not try to fix it by yourself. Contact a qualified service  technician or your retailer. 

**Operation safety** 

• Before installing the motherboard and adding devices on it, carefully read all the  manuals that came with the package. 

• Before using the product, ensure all cables are correctly connected and the power  cables are not damaged. If you detect any damage, contact your dealer immediately. 

• To avoid short circuits, keep paper clips, screws, and staples away from connectors,  slots, sockets and circuitry. 

• Avoid dust, humidity, and temperature extremes. Do not place the product in any area  where it may become wet. 

• Place the product on a stable surface. 

• If you encounter technical problems with the product, contact a qualified service  technician or your retailer. 

• Your motherboard should only be used in environments with ambient temperatures  between 0°C and 40°C. 

**Button/Coin Batteries Safety Information**

**![][image1]**

| WARNING  KEEP OUT OF REACH OF CHILDREN Swallowing can lead to chemical burns,  perforation of soft tissue, and death. Severe  burns can occur within 2 hours of ingestion.  Seek medical attention immediately. |  |
| ----- | :---- |

v   
**About this guide** 

This user guide contains the information you need when installing and configuring the  motherboard. 

**How this guide is organized** 

This guide contains the following parts: 

**• Chapter 1: Product Introduction** 

This chapter describes the features of the motherboard and the new technology it  supports. It includes description of the switches, jumpers, and connectors on the  motherboard. 

**• Chapter 2: Basic Installation** 

This chapter lists the hardware setup procedures that you have to perform when  installing system components. 

**• Chapter 3: BIOS and RAID Support** 

This chapter tells how to boot into the BIOS, upgrade BIOS using the EZ Flash Utility  and support on RAID. 

**Where to find more information** 

Refer to the following sources for additional information and for product and software  updates. 

**1---. ASUS website** 

The ASUS website (www.asus.com) provides updated information on ASUS hardware  and software products. 

**2---. Optional documentation** 

Your product package may include optional documentation, such as warranty flyers,  that may have been added by your dealer. These documents are not part of the  standard package. 

**Conventions used in this guide** 

To ensure that you perform certain tasks properly, take note of the following symbols used  throughout this user guide. 

**CAUTION:** Information to prevent damage to the components and injuries to  yourself when trying to complete a task. 

**IMPORTANT:** Instructions that you MUST follow to complete a task. 

**NOTE:** Tips and additional information to help you complete a task.

vi   
**Pro WS W680-ACE specifications summary** 

| CPU | Intel® Socket LGA1700 for 13th Gen Intel® Core™ Processors & 12th Gen Intel® Core™, Pentium® Gold and Celeron® Processors  Supports Intel® Turbo Boost Technology 2.0 and Intel® Turbo Boost Max  Technology 3.0---*---*  ---* Refer to www.asus.com for CPU support list.  ---*---* Intel® Turbo Boost Max Technology 3.0 support depends on the CPU types. |
| :---- | ----- |
| **Chipset**  | Intel® W680 Chipset |
| **Memory**  | 4 x DIMM, Max. 128GB, DDR5 6000(OC) / 5800(OC) / 5600(OC) / 5400(OC) / 5200(OC) / 5000(OC) / 4800 Non-ECC, Un-buffered  Memory---*  4 x DIMM, Max. 128GB, DDR5 4800 ECC Memory  Dual Channel Memory Architecture  Supports Intel® Extreme Memory Profile (XMP)  OptiMem II  **---* Supported memory types, data rate(Speed), and number of DRAM modules  vary depending on the CPU and memory configuration, for more information  refer to www.asus.com for memory support list.  ---* Non-ECC, un-buffered DDR5 memory supports On-Die ECC function.** |
| **Graphics** | 1 x DisplayPort---*---* 1 x HDMI® port---*---*---*  1 x VGA port   **---* Graphics specifications may vary between CPU types. Please refer to  www.intel.com for any updates.  ---*---* Supports max. 8K@60Hz as specified in DisplayPort 1.4.  ---*---*---* Supports 4K@60Hz as specified in HDMI 2.1.** |
| **Expansion Slots** | **Intel® 13th & 12th Gen Processors---***  ---- 2 x PCIe 5.0 x16 slots (support x16 or x8/x8 mode)  **Intel® W680 Chipset**   ---- 2 x PCIe 3.0 x16 slots (supports x4 mode)   ---- 1 x PCIe 3.0 x1 slot  **---* Please check the PCIe bifurcation table at the support site   (https://www.asus.com/support/FAQ/1037507/).**  To ensure compatibility of the device installed, please   refer to https://www.asus.com/support/ for the list of   supported peripherals. |
| **Storage** | **Total supports 3 x M.2 slots and 8 x SATA 6Gb/s ports---*Intel® 13th & 12th Gen Processors**   ---- M.2---_1 slot (Key M), type 2242/2260/2280 (supports PCIe 4.0 x4 mode) |

(continued on the next page)

vii   
**Pro WS W680-ACE specifications summary** 

| Storage | Intel® W680 Chipset   ---- M.2---_2 slot (Key M), type 2242/2260/2280 (supports PCIe 4.0 x4 mode)  ---- M.2---_3 slot (Key M), type 2242/2260/2280/22110   (supports PCIe 4.0 x4 mode)   ---- 4 x SATA 6Gb/s ports   ---- 1 x SlimSAS Slot Support SlimSAS NVMe device (supports PCIe 4.0  x4 mode), up to 4 SATA devices---*---*---*  ---* Intel® Rapid Storage Technology supports PCIe RAID 0/1/5/10, SATA RAID  0/1/5/10.  ---*---* Intel® Rapid Storage Technology supports Intel® Optane Memory H Series  on PCH attached M.2 slots.  ---*---*---* SlimSAS slot can support up to 4 SATA devices via a transfer cable. The  cable is purchased separately. |
| ----- | :---- |
| **Ethernet** | 2 x Intel® 2.5Gb Ethernet ASUS LANGuard |
| **Wireless & Bluetooth®**  | M.2 slot only (Key E, CNVi & PCIe)---***---* Wi-Fi module is sold separately.** |
| **USB** | **Rear USB (Total 8 ports)**  ---- 2 x USB 3.2 Gen 2 ports (1 x Type-A, 1 x Type-C®)   ---- 4 x USB 3.2 Gen 1 ports (4 x Type-A)   ---- 2 x USB 2.0 ports (2 x Type-A)  **Front USB (Total 7 ports)**   ---- 1 x USB 3.2 Gen 2x2 connector (supports USB Type-C®)  ---- 1 x USB 3.2 Gen 1 header supports additional 2 USB 3.2 Gen 1 ports  ---- 2 x USB 2.0 headers support additional 4 USB 2.0 ports |
| **Audio** | **Realtek 7.1 Surround Sound High Definition Audio CODEC** ---- Supports: Jack-detection, Multi-streaming, Front Panel Jack-retasking  ---- Supports up to 24-Bit/192 kHz playback  **Audio Features**   ---- Audio Shielding   ---- Premium audio capacitors   ---- Dedicated audio PCB layers   ---- Unique de-pop circuit |
| **Back Panel I/O Ports**  | 2 x USB 3.2 Gen 2 ports (1 x Type-A, 1 x Type-C®) 4 x USB 3.2 Gen 1 ports (4 x Type-A)  2 x USB 2.0 ports (2 x Type-A)  1 x DisplayPort  1 x HDMI® port  1 x VGA port  2 x Intel® 2.5Gb Ethernet ports  5 x Audio jacks  1 x BIOS FlashBack™ button |

(continued on the next page)

viii   
**Pro WS W680-ACE specifications summary** 

| Internal I/O connectors | Fan and Cooling related   1 x 4-pin CPU Fan header  1 x 4-pin CPU OPT Fan header  1 x 4-pin W---_PUMP+ header  3 x 4-pin Chassis Fan headers  Power related   1 x 24-pin Main Power connector  1 x 8-pin ---+12V Power connector  1 x 4-pin ---+12V Power connector  1 x 6-pin PCIe Power connector  Storage related  3 x M.2 slots (Key M)   4 x SATA 6Gb/s ports  1 x SlimSAS port  USB  1 x USB 3.2 Gen 2x2 connector (supports USB Type-C®)  1 x USB 3.2 Gen 1 header supports 2 additional USB 3.2 Gen 1 ports 2 x USB 2.0 headers support 4 additional USB 2.0 ports  Miscellaneous  1 x BMC Header  1 x Clear CMOS header  1 x COM Port header  1 x FlexKey button  1 x Front Panel Audio header (AAFP)  1 x LPT header  1 x LPT---_P80 switch  1 x M.2 slot (Key E)  1 x SPI TPM header  1 x Start button  1 x 20-3 pin System Panel header with Chassis intrude function 1 x Thermal Sensor header  1 x Thunderbolt™ header |
| ----- | :---- |
| **Special Features** | **ASUS 5X PROTECTION III**  ---- DIGI+ VRM (Digital power design with DrMOS)   ---- ESD Guards    ---- LANGuard   ---- Overvoltage Protection   ---- SafeSlot   ---- Stainless-Steel Back I/O |

(continued on the next page)

ix   
**Pro WS W680-ACE specifications summary**

| Special Features | ASUS Q-Design    ---- M.2 Q-Latch   ---- Q-Code   ---- Q-Connector   ---- Q-DIMM   ---- Q-LED (CPU ---[red---], DRAM ---[yellow---], VGA ---[white---], Boot Device ---[yellow  green---])   ---- Q-Slot  ASUS Thermal Solution   ---- M.2 heatsink   ---- VRM heatsink design  ASUS EZ DIY   ---- CPU Socket lever protector   ---- SafeDIMM   ---- SafeSlot   ---- BIOS FlashBack™ button   ---- BIOS FlashBack™ LED  Bespoke Motherboard Design & Business Focused Features  ---- 24/7 Reliability |
| :---- | :---- |
| **Software Features** | **ASUS Exclusive Software** Armoury Crate   ---- Fan Xpert 4    ---- Power Saving  ASUS CPU-Z  Norton 360 Deluxe (60 Days Free Trial)  WinRAR  IT Management software supported   ---- ASUS Control Center Express(ACCE)   **UEFI BIOS**  ASUS EZ DIY    ---- ASUS CrashFree BIOS 3    ---- ASUS EZ Flash 3   FlexKey |
| **BIOS**  | 256 Mb Flash ROM, UEFI AMI BIOS |
| **Manageability**  | WOL by PME, PXE  |
| **Operating System**  | Windows® 11 Windows® 10 64-bit  |
| **Form Factor**  | ATX Form Factor 12 inch x 9.6 inch (30.5 cm x 24.4 cm) |

x   
• Specifications are subject to change without notice. Please refer to the ASUS website  for the latest specifications. 

• For more information on downloading and installing drivers and utilities for your  motherboard, please scan the QR code below:

![][image2]

xi   
**Package contents** 

Check your motherboard package for the following items. 

Motherboard 1 x Pro WS W680-ACE IPMI motherboard or  1 x Pro WS W680-ACE motherboard 

Cables 2 x SATA 6Gb/s cables  

1 x IPMI expansion card 

IPMI Expansion Card and  related accessories---* 

**---* This item or package is only  available for Pro WS W680-ACE  IPMI.** 

Miscellaneous   
1 x BMC cable 

1 x Power On/Off cable 

1 x Reset cable 

1 x USB 2.0 cable 

1 x IPMI Expansion Card user guide 1 x Q-connector   
1 x M.2 rubber package 

2 x Screw packages for M.2 SSD 

Documentation 1 x User guide   
1 x ACC Express Activation Key Card 

If any of the above items is damaged or missing, contact your retailer.

xii   
**Chapter 1: Product Introduction**   
**Product Introduction** 1 **Chapter 1**   
**1.1 Before you proceed** 

Take note of the following precautions before you install motherboard components or  change any motherboard settings.

• Unplug the power cord from the wall socket before touching any component. 

• Before handling components, use a grounded wrist strap or touch a safely grounded    
object or a metal object, such as the power supply case, to avoid damaging them due    
to static electricity. 

• Hold components by the edges to avoid touching the ICs on them. 

• Whenever you uninstall any component, place it on a grounded antistatic pad or in    
the bag that came with the component. 

• Before you install or remove any component, ensure that the ATX power supply is    
switched off or the power cord is detached from the power supply. Failure to do so    
may cause severe damage to the motherboard, peripherals, or components. 

• The pin definitions in this chapter are for reference only. The pin names depend on    
the location of the header/jumper/connector. 

• For more information on installing your motherboard, please scan the QR code    
below: 

![][image3]

Pro WS W680-ACE Series 1-1   
**1.2 Motherboard layout**

**Chapter 1** 

|  |
| :---- |
|  |

|  |
| :---- |
|  |

1-2 Chapter 1: Product Introduction 

| Layout contents  | Page |
| :---- | :---: |
| 1---. CPU socket  | 1-4 |
| 2---. DIMM slots  | 1-5 |
| 3---. Expansion slots  | 1-7 |
| 4---. Fan headers  | 1-9 |
| 5---. Power connectors  | 1-10 |
| 6---. M.2 slot  | 1-11 |
| 7---. SATA 6GB/s port  | 1-12 |
| 8---. SlimSAS port  | 1-13 |
| 9---. USB 3.2 Gen 2x2 Type-C® Front Panel connector  | 1-13 |
| 10---. USB 3.2 Gen 1 header  | 1-14 |
| 11---. USB 2.0 connector  | 1-15 |
| 12---. BMC header  | 1-16 |
| 13---. Clear CMOS header  | 1-17 |
| 14---. COM Port header  | 1-18 |
| 15---. FlexKey button (Reset)  | 1-19 |
| 16---. Front Panel Audio header  | 1-20 |
| 17---. LPT header  | 1-20 |
| 18---. LPT and Q-Code Switch header  | 1-21 |
| 19---. M.2 slot (Key E)  | 1-21 |
| 20---. Start button  | 1-22 |
| 21---. System Panel header  | 1-23 |
| 22---. Thermal Sensor header  | 1-24 |
| 23---. Thunderbolt™ header  | 1-25 |
| 24---. TPM header  | 1-26 |
| 25---. Q-Code LED  | 1-26 |
| 26---. Q LEDs  | 1-27 |
| 27---. BIOS FlashBack™ LED  | 1-2 |
| 28---. 8-pin Power Plug LED  | 1-28 |

**Chapter 1**

Pro WS W680-ACE Series 1-3   
**1---. CPU socket** 

The motherboard comes with a LGA1700 socket designed for 13th Gen Intel® Core™  Processors & 12th Gen Intel® Core™, Pentium® Gold and Celeron® Processors. 

**Chapter 1** 

• Ensure that you install the correct CPU designed for LGA1700 socket only. DO NOT    
install a CPU designed for other sockets on the LGA1700 socket. 

• The CPU fits in only one correct orientation. DO NOT force the CPU into the socket    
to prevent bending the connectors on the socket and damaging the CPU. 

• Ensure that all power cables are unplugged before installing the CPU. 

• Upon purchase of the motherboard, ensure that the PnP cap is on the socket and    
the socket contacts are not bent. Contact your retailer immediately if the PnP cap    
is missing, or if you see any damage to the PnP cap/socket contacts/motherboard    
components. ASUS will shoulder the cost of repair only if the damage is shipment/   
transit-related. 

• Keep the cap after installing the motherboard. ASUS will process Return    
Merchandise Authorization (RMA) requests only if the motherboard comes with the    
cap on the socket. 

• The product warranty does not cover damage to the socket contacts resulting from    
incorrect CPU installation/removal, or misplacement/loss/incorrect removal of the  

PnP cap.

1-4 Chapter 1: Product Introduction   
**2---. DIMM slots** 

The motherboard comes with Dual Inline Memory Modules (DIMM) slots designed for DDR5  (Double Data Rate 5---) memory modules. 

**Chapter 1**   
A DDR5 memory module is notched differently from a DDR, DDR2, DDR3, or DDR4    
module. DO NOT install a DDR, DDR2, DDR3, or DDR4 memory module to the DDR5    
slot. 

**Recommended memory configurations**

Pro WS W680-ACE Series 1-5   
**Memory configurations** 

You may install 8GB, 16GB, and 32GB unbuffered and non‑ECC DDR5 DIMMs into the  DIMM sockets. 

**Chapter 1**   
You may install varying memory sizes in Channel A and Channel B. The system maps    
the total size of the lower-sized channel for the dual-channel configuration. Any excess    
memory from the higher-sized channel is then mapped for single-channel operation. 

• The default memory operation frequency is dependent on its Serial Presence Detect    
(SPD), which is the standard way of accessing information from a memory module.    
Under the default state, some memory modules for overclocking may operate at a    
lower frequency than the vendor-marked value. 

• For system stability, use a more efficient memory cooling system to support a full    
memory load or overclocking condition. 

• Always install the DIMMS with the same CAS Latency. For an optimum compatibility,  we recommend that you install memory modules of the same version or data code    
(D/C) from the same vendor. Check with the vendor to get the correct memory    
modules. 

• Visit the ASUS website for the latest QVL.

1-6 Chapter 1: Product Introduction   
**3---. Expansion slots** 

Unplug the power cord before adding or removing expansion cards. Failure to do so may    
cause you physical injury and damage motherboard components. 

**Chapter 1** 

|  |
| :---- |
|  |

|  |
| :---- |
|  |

• Additional PCIe bifurcation and M.2 settings for RAID function are also supported  when a Hyper M.2 x16 series card is installed. 

• For full details on the PCIe bifurcation, you may visit the support site at    
https://www.asus.com/support/FAQ/1037507/. 

• The Hyper M.2 X16 series card is sold separately. 

• Adjust the PCIe bifurcation under BIOS settings.

Please refer to the following tables for the recommended VGA configuration and PCIe  bifurcation configuration. 

Pro WS W680-ACE Series 1-7   
**Recommended VGA configuration** 

| Slot Description  |  | Single VGA  | Dual VGA |  Triple VGA |  Quad VGA |
| :---- | :---- | :---: | :---: | :---: | :---: |
| 2 |  PCIEX16(G5)---_1  | x16  | x8  | x8  | x8 |
| 3 |  PCIEX16(G5)---_2  | ----  | x8  | x8  | x8 |
| 4 |  PCIEX16(G3)---_1  | ----  | ----  | x4  | x4 |
| 5 |  PCIEX16(G3)---_2  | ----  | ----  | ----  | x4 |

**Chapter 1**   
• Connect a chassis fan to the chassis fan connectors when using multiple graphics    
cards for better thermal environment. 

• When installing a dual VGA card, we recommend selecting a chassis case which    
supports 7 or more expansion slots 

**PCIe bifurcation & M.2 settings in PCIe x16 slots (from CPU)**

| Slot Description  |  | Quantity of identifiable Intel M.2 SSD (pcs) |  |  |
| ----- | :---- | :---: | :---: | :---: |
|  |  | **Situation 1** |  **Situation 2** |  **Situation 3** |
| 2 |  PCIEX16(G5)---_1  | 1 (x16)  | 2 (x8 ---+ x8)  | 1 (x8) |
| 3 |  PCIEX16(G5)---_2  | ----  | ----  | 1 (x8) |
| 4 |  PCIEX16(G3)---_1  | 1 (x4)  | 1 (x4)  | 1 (x4) |
| 5 |  PCIEX16(G3)---_2  | 1 (x4)  | 1 (x4)  | 1 (x4) |

1-8 Chapter 1: Product Introduction   
**4---. Fan and Pump headers** 

The Fan and Pump headers allow you to connect fans or pumps to cool the system. **Chapter 1** 

• DO NOT forget to connect the fan cables to the fan headers. Insufficient air flow    
inside the system may damage the motherboard components. These are not    
jumpers---! Do not place jumper caps on the fan headers---! 

• Ensure the cable is fully inserted into the header. 

For water cooling kits, connect the pump connector to the **W---_PUMP+** header.

| Header |  Max. Current |  Max. Power |  Default Speed |  Shared Control |
| :---- | :---: | :---: | :---: | :---: |
| CPU---_FAN  | 1A  | 12W |  Q-Fan Controlled  | A |
| CPU---_OPT  | 1A  | 12W |  Q-Fan Controlled  | A |
| CHA---_FAN1  | 1A  | 12W |  Q-Fan Controlled  | ---- |
| CHA---_FAN2  | 1A  | 12W |  Q-Fan Controlled  | ---- |
| CHA---_FAN3  | 1A  | 12W |  Q-Fan Controlled  | ---- |
| W---_PUMP+  | 3A  | 36W  | Full Speed  | ---- |

Pro WS W680-ACE Series 1-9   
**5---. Power connectors** 

These Power connectors allow you to connect your motherboard to a power supply.  The power supply plugs are designed to fit in only one orientation, find the proper  orientation and push down firmly until the power supply plugs are fully inserted.   
**Chapter 1** 

Ensure to connect the 8-pin power plug, or both the 8-pin and 4-pin power plugs. 

• We recommend that you use a PSU with a higher power output when configuring a    
system with more power-consuming devices. The system may become unstable or    
may not boot up if the power is inadequate. 

• For a fully configured system, we recommend that you use a power supply unit    
(PSU) that complies with ATX 12 V Specification 2.0 (or later version) and provides a    
minimum power of 350W. 

• If you want to use two or more high-end PCI Express x16 cards, use an appropriate    
PSU which can supply the required power to ensure the system stability. 

The **PCIE---_6P---_PWR** provides additional power for your PCIe X16 slots.

1-10 Chapter 1: Product Introduction   
**6---. M.2 slot** 

The M.2 slot allows you to install M.2 SSD modules. 

**Chapter 1** 

• **Intel® 13th & 12th Gen Processors:** 

 ---- M.2---_1 supports PCIE 4.0 x4 mode M Key design and type 2242 / 2260 / 2280    
storage devices. 

• **Intel® W680 Chipset:** 

 ---- M.2---_2 supports PCIE 4.0 x4 mode M Key design and type 2242 / 2260 / 2280    
storage devices. 

 ---- M.2---_3 supports PCIE 4.0 x4 mode M Key design and type 2242 / 2260 / 2280 /    
22110 storage devices. 

• Intel® Rapid Storage Technology supports PCIe RAID 0/1/5/10, SATA RAID 0/1/5/10. 

The M.2 SSD module is purchased separately.

Pro WS W680-ACE Series 1-11   
**7---. SATA 6Gb/s ports** 

The SATA 6Gb/s ports allows you to connect SATA devices such as optical disc  drives and hard disk drives via a SATA cable. 

**Chapter 1** 

If you installed SATA storage devices to the **SATA6G---_1-4** ports, you can create a RAID 0,    
1, 5, and 10 configuration with the Intel® Rapid Storage Technology through the onboard    
Intel® W680 chipset. 

Before creating a RAID set, refer to the **RAID Configuration Guide**. You can download  

the **RAID Configuration Guide** from the ASUS website.

1-12 Chapter 1: Product Introduction   
**8---. SlimSAS port** 

The SlimSAS port allows you to connect PCIe 4.0 x4 NVM Express storage, and can  support up to 4 SATA devices using a transfer cable. 

**Chapter 1** 

Cables are purchased separately. 

**9---. USB 3.2 Gen 2x2 Type-C® Front Panel connector** 

The USB 3.2 Gen 2x2 Type-C® connector allows you to connect a USB 3.2 Gen 2x2  Type-C® module for additional USB 3.2 Gen 2x2 ports on the front panel. The USB 3.2  Gen 2x2 Type-C® connector provides data transfer speeds of up to 20 Gb/s. 

The USB 3.2 Gen 2x2 Type-C® module is purchased separately.

Pro WS W680-ACE Series 1-13   
**10---. USB 3.2 Gen 1 header** 

The USB 3.2 Gen 1 header allows you to connect a USB 3.2 Gen 1 module for  additional USB 3.2 Gen 1 ports. The USB 3.2 Gen 1 header provides data transfer  speeds of up to 5 Gb/s.   
**Chapter 1** 

The USB 3.2 Gen 1 module is purchased separately.

1-14 Chapter 1: Product Introduction   
**11---. USB 2.0 header** 

The USB 2.0 header allows you to connect a USB module for additional USB    
2.0 ports. The USB 2.0 header provides data transfer speeds of up to 480 MB/s  connection speed. 

**Chapter 1** 

DO NOT connect a 1394 cable to the USB connectors. Doing so will damage the    
motherboard---! 

The USB 2.0 module is purchased separately.

Pro WS W680-ACE Series 1-15   
**12---. BMC header** 

The BMC header allows you to connect and support an IPMI card. 

**Chapter 1** 

• The IPMI card is purchased separately. 

• For more information on the installation and information regarding the IPMI card,  

please visit www.asus.com.

1-16 Chapter 1: Product Introduction   
**13---. Clear CMOS header** 

The Clear CMOS header allows you to clear the Real Time Clock (RTC) RAM in  the CMOS, which contains the date, time, system passwords, and system setup  parameters. 

**Chapter 1** 

To erase the RTC RAM: 

1---. Turn OFF the computer and unplug the power cord. 

2---. Short-circuit pin 1-2 with a metal object or jumper cap for about 5-10 seconds. 3---. Plug the power cord and turn ON the computer. 

4---. Hold down the ---<Del---> key during the boot process and enter BIOS setup to  re-enter data. 

DO NOT short-circuit the pins except when clearing the RTC RAM. Short-circuiting or    
placing a jumper cap will cause system boot failure---! 

If the steps above do not help, remove the onboard button cell battery and short the two    
pins again to clear the CMOS RTC RAM data. After clearing the CMOS, reinstall the  

button cell battery.

Pro WS W680-ACE Series 1-17   
**14---. COM Port connector** 

The COM (Serial) Port connector allows you to connect a COM port module. Connect  the COM port module cable to this connector, then install the module to a slot opening  on the system chassis.   
**Chapter 1** 

The COM port module is purchased separately.

1-18 Chapter 1: Product Introduction   
**15---. FlexKey button (Reset)** 

Press the FlexKey button to reboot the system. You may also configure the button and  assign a quick access feature such as activating Safe Boot to the button. 

**Chapter 1** 

This button set to **---[Reset---]** by default. You can assign a different function to this button in  

the BIOS settings.

Pro WS W680-ACE Series 1-19   
**16---. Front Panel Audio header** 

The Front Panel Audio header is for a chassis-mounted front panel audio I/O module  that supports HD Audio. Connect one end of the front panel audio I/O module cable to  this header.   
**Chapter 1** 

We recommend that you connect a high-definition front panel audio module to this    
connector to avail of the motherboard’s high-definition audio capability. 

**17---. LPT header** 

The LPT (Line Printing Terminal) connector supports devices such as a printer. LPT  standardizes as IEEE 1284, which is the parallel port interface on IBM PC-compatible  computers.

1-20 Chapter 1: Product Introduction   
**18---. LPT and Q-Code Switch header** 

The LPT and Q-Code Switch header allows you to enable either LPT (Line Printing  Thermal) connector or Q-Code. 

**Chapter 1** 

**19---. M.2 slot (Key E)** 

The M.2 Wi-Fi slot allows you to install an M.2 Wi-Fi module (E-key, type 2230; PCIe  interface). 

The M.2 Wi-Fi module is purchased separately.

Pro WS W680-ACE Series 1-21   
**20---. Start button** 

Press the Power button to power up the system, or put the system into sleep or soft off mode (depending on the operating system settings).

**Chapter 1** 

1-22 Chapter 1: Product Introduction   
**21---. System Panel header** 

The System Panel header supports several chassis-mounted functions. 

**Chapter 1** 

**• System Power LED header (PLED)** 

The 2-pin header allows you to connect the System Power LED. The System Power  LED lights up when the system is connected to a power source, or when you turn on  the system power, and blinks when the system is in sleep mode. 

**• Storage Device Activity LED header (HDD---_LED)** 

The 2-pin header allows you to connect the Storage Device Activity LED. The Storage  Device Activity LED lights up or blinks when data is read from or written to the storage  device or storage device add-on card. 

**• System Warning Speaker header (SPEAKER)** 

The 4-pin header allows you to connect the chassis-mounted system warning    
speaker. The speaker allows you to hear system beeps and warnings. 

**• Power Button/Soft-off Button header (PWRSW)** 

The 3-1 pin header allows you to connect the system power button. Press the    
power button to power up the system, or put the system into sleep or soft-off mode  (depending on the operating system settings). 

**• Reset button header (RESET)** 

The 2-pin header allows you to connect the chassis-mounted reset button. Press the  reset button to reboot the system. You may also set this header to other functions. 

This header is set to **---[Reset---]** by default. You can assign a different function to this header    
in the BIOS settings. 

**• Chassis intrusion connector (CHASSIS)** 

The 2-pin connector allows you to connect the chassis-mounted intrusion detection  sensor or switch. The chassis intrusion sensor or switch sends a high-level signal to  the connector when a chassis component is removed or replaced, the signal is then  generated as a chassis intrusion event.

Pro WS W680-ACE Series 1-23   
**22---. Thermal Sensor header** 

The Thermal Sensor header allows you to connect a sensor to monitor the  temperature of the devices and the critical components inside the motherboard.  Connect the thermal sensor and place it on the device or the motherboard’s    
**Chapter 1**   
component to detect its temperature. 

The thermal sensor is purchased separately.

1-24 Chapter 1: Product Introduction   
**23---. Thunderbolt**™ **header** 

The Thunderbolt™ header allows you to connect an add-on Thunderbolt™ I/O    
card that supports Intel®’s Thunderbolt™ Technology, allowing you to connect    
Thunderbolt™-enabled devices to form a daisy-chain configuration. 

**Chapter 1** 

• The add-on Thunderbolt™ I/O card and Thunderbolt™ cables are purchased    
separately. 

• Please visit the official website of your purchased Thunderbolt™ card for more details  on compatibility. 

The Thunderbolt™ card can only be used when installed to the PCIEX16(G3)---_2 slot.  

Ensure to install your Thunderbolt™ card to the PCIEX16(G3)---_2 slot.

Pro WS W680-ACE Series 1-25   
**24---. TPM header** 

The TPM header allows you to connect a TPM module, which securely stores keys,  digital certificates, passwords, and data. A TPM system also helps enhance network  security, protect digital identities, and ensures platform integrity.   
**Chapter 1** 

The TPM module is purchased separately.

**25---. Q-Code LED** 

The Q-Code LED design provides you with a 2-digit error code that displays the  system status. 

|  |
| :---- |
|  |

|  |
| :---- |
|  |

• The Q-Code LEDs provide the most probable cause of an error code as a starting  point for troubleshooting. The actual cause may vary from case to case. 

• Please refer to the Q-Code table in the **Appendix** section for more details. 

1-26 Chapter 1: Product Introduction   
**26---. Q-LEDs** 

The Q-LEDs check key components (CPU, DRAM, VGA, and booting devices) during  the motherboard booting process. If an error is found, the critical component’s LED  stays lit up until the problem is solved. 

**Chapter 1** 

The Q-LEDs provide the most probable cause of an error code as a starting point for    
troubleshooting. The actual cause may vary from case to case. 

**27---. BIOS FlashBack**™ **LED** 

The BIOS FlashBack™ LED lights up or blinks to indicate the status of the BIOS  FlashBack™. 

Refer to the **BIOS update utility** section for more information on using the BIOS  

FlashBack™ feature.

Pro WS W680-ACE Series 1-27   
**28---. 8-pin Power Plug LED** 

The 8-pin Power Plug LED lights up to indicate that the 8-pin power plug is not  connected.

**Chapter 1** 

1-28 Chapter 1: Product Introduction   
**Chapter 2: Basic Installation** 

**Basic Installation 2.1 Building your PC system** 

2 

The diagrams in this section are for reference only. The motherboard layout may vary with  models, but the installation steps are the same for all models. 

**2.1.1 CPU installation** 

• Ensure that you install the correct CPU designed for LGA1700 socket only. DO NOT    
install a CPU designed for LGA1155, LGA1156, LGA1151, and LGA1200 sockets on    
the LGA1700 socket.   
**Chapter 2**   
• ASUS will not cover damages resulting from incorrect CPU installation/removal,    
incorrect CPU orientation/placement, or other damages resulting from negligence by    
the user. 

Take caution when lifting the load    
lever, ensure to hold onto the load    
lever when releasing the load    
lever. Letting go of the load lever    
immediately after releasing it may    
cause the load lever to spring    
back and cause damage to your  

motherboard.

Pro WS W680-ACE Series 2-1   
**Chapter 2** 

Ensure to remove the CPU    
Socket lever protector on the    
lever latch before locking the    
lever latch under the retention    
tab. Failure to do so may cause    
damages to your system when  

installing the cooling system.

2-2 Chapter 2: Basic Installation   
**2.1.2 Cooling system installation To install a CPU heatsink and fan assembly** 

• Apply Thermal Interface Material to    
the CPU cooling system and CPU    
before you install the cooling system,    
if necessary. 

• Ensure to remove the CPU Socket    
lever protector on the lever latch    
before installing the cooling system,    
failure to do so may cause damages  

to your system.

**Chapter 2** 

Pro WS W680-ACE Series 2-3 

**Chapter 2**   
**Bottom side of motherboard**

LGA1200 

LGA1700   
• We recommend using a  LGA1700 compatible cooling  system on an Intel® 600 series  motherboard. 

• Additional holes for LGA1200  compatible cooling systems    
are also available on ASUS’  Intel® 600 series motherboards,  however, we still strongly advise  consulting with your cooling    
system vendor or manufacturer  on the compatibility and    
functionality of the cooling    
system. 

• Push-pin type LGA1200  compatible cooling systems    
cannot be installed to this    
motherboard. 

Make sure a click is heard when  pushing the push-pins. 

2-4 Chapter 2: Basic Installation   
**To install an AIO cooler** 

• We recommend using a LGA1700 compatible cooling system on an Intel® 600 series    
motherboard. 

• Additional holes for LGA1200 compatible cooling systems are also available on    
ASUS’ Intel® 600 series motherboards, however, we still strongly advise consulting    
with your cooling system vendor or manufacturer on the compatibility and    
functionality of the cooling system. 

• If you wish to install an AIO cooler, we recommend installing the AIO cooler after    
installing the motherboard into the chassis. 

**Chapter 2** 

CPU---_FAN

CPU---_OPT 

|  |
| :---- |

W---_PUMP+/  AIO---_PUMP 

Pro WS W680-ACE Series 2-5 

**Chapter 2**   
**2.1.3 DIMM installation To remove a DIMM**  
2-6 Chapter 2: Basic Installation   
**2.1.4 M.2 installation** 

Supported M.2 type varies per motherboard. 

If the thermal pad on the M.2 heatsink becomes damaged, we recommend replacing it    
with a thermal pad with a thickness of 1.25mm. 

• The illustrations only show the installation steps for a single M.2 slot, the steps are    
the same for the other M.2 slots if you wish to install an M.2 to another M.2 slot. 

• Use a Phillips screwdriver when removing or installing the screws or screw stands    
mentioned in this section. 

• The M.2 is purchased separately.

**Chapter 2**   
1---. Completely loosen the screws on the heatsinks. 

2---. Lift and remove the heatsinks. 

1 1 

2 

Pro WS W680-ACE Series 2-7   
3---. Install your M.2 to your M.2 slot. The steps may differ between installing M.2 of  different lengths, please refer to the different types and their installation steps below: 

**• To install an M.2 to M.2---_1 slot** 

For 2280 length 

A. (optional) Install the bundled rubber for M.2 if you are installing a    
single sided M.2 storage device. DO NOT install the bundled rubber    
for M.2 when installing a double-sided M.2 storage device. The rubber    
installed by default is compatible with double sided M.2 storage    
devices. 

B. Rotate and adjust the M.2 Q-latch so that the handle points away    
from the M.2 slot. 

C. Install your M.2 to the M.2 slot. 

D. Rotate the M.2 Q-Latch clockwise to secure the M.2 in place.   
**Chapter 2**   
**Bundled rubber for M.2**

OPTIONAL 

2-8 Chapter 2: Basic Installation   
For 2242, 2260 length 

A. (optional) Remove the M.2 rubber. 

Follow this step only if you wish to install an M.2 to type 2242---. 

B. Install the bundled screw stand to the M.2 length screw hole you wish  to install your M.2 to. 

C. Install your M.2 to the M.2 slot. 

D. Secure your M.2 using the bundled screw stand’s screw.

**Chapter 2** 

Pro WS W680-ACE Series 2-9   
**• To install an M.2 to M.2---_2 slot** 

For 2242, 2260, 2280 length 

A. Install the bundled screw stand to the M.2 length screw hole you wish  to install your M.2 to. 

B. Install your M.2 to the M.2 slot. 

C. Secure your M.2 using the bundled screw stand’s screw.

**Chapter 2** 

2-10 Chapter 2: Basic Installation   
For 2242, 2260, 2280, 22110 length 

A. (optional) Remove the pre-installed removable M.2 Q-Latch screw at  the 2280 length screw hole. 

B. (optional) Install the M.2 Q-Latch to the M.2 length screw hole you  wish to install your M.2 to. 

Follow step A and B only when you wish to install an 2242, 2260, or 22110 length M.2. 

C. Rotate and adjust the M.2 Q-latch so that the handle points away  from the M.2 slot. 

D. Install your M.2 to the M.2 slot. 

E. Rotate the M.2 Q-Latch clockwise to secure the M.2 in place.**Chapter 2** 

Pro WS W680-ACE Series 2-11   
4---. Remove the plastic film from the thermal pads on the bottom of the heatsinks. 

If the thermal pad on the M.2 heatsink becomes damaged, we recommend replacing it    
with a thermal pad with a thickness of 1.25mm. 

5---. Replace the heatsinks. 

6---. Secure the heatsinks using the screws on the heatsink. 

6 6 

5 

**Chapter 2**   
4

2-12 Chapter 2: Basic Installation   
**2.1.5 Motherboard installation**

1---. Place the motherboard into the chassis, ensuring that its rear I/O ports are aligned to  the chassis’ rear I/O panel. 

**Chapter 2**   
2---. Place nine (9) screws into the holes indicated by circles to secure the motherboard to  the chassis. 

|  |
| :---- |

DO NOT over tighten the screws---! Doing so can damage the motherboard. 

Pro WS W680-ACE Series 2-13   
**2.1.6 ATX power connection** 

**Chapter 2** 

**OR AND**

Ensure to connect the 8-pin power plug, or both the 8-pin and 4-pin power plugs. 

2-14 Chapter 2: Basic Installation   
**Chapter 2**   
The **PCIE---_6P---_PWR** connector provides additional power for your PCIe X16 slots.

Pro WS W680-ACE Series 2-15 

**Chapter 2**   
**2.1.7 SATA device connection OR**

**2.1.8 SlimSAS connection** 

2-16 Chapter 2: Basic Installation   
**2.1.9 Front I/O connector To install front panel connector** 

**To install USB 3.2 Gen 1 connector** 

**USB 3.2 Gen 1** 

**To install front panel audio connector AAFP**   
**To install USB 3.2 Gen 2x2 Type-C®  connector** 

**USB 3.2 Gen 2x2**    
**Type-C®** 

This connector will only fit in one    
orientation. Push the connector until it  clicks into place.

**To install USB 2.0 connector USB 2.0**   
**Chapter 2** 

Pro WS W680-ACE Series 2-17 

**Chapter 2**   
**2.1.10 Expansion card installation To install PCIe x16 cards** 

**To install PCIe x1 cards**

2-18 Chapter 2: Basic Installation   
**To install Thunderbolt**™ **series card** 

**USB Type-C®**   
**port connects**    
**to Thunderbolt**    
**devices** 

**MiniDP in port**    
**connects to DP**    
**out port on the**    
**motherboard**    
**or a VGA card** 

**6-pin PCIe power connector** 

**USB 2.0 header** 

**Thunderbolt™ header** 

**Chapter 2** 

The Thunderbolt™ card can only be used when installed to the PCIEX16(G3)---_2 slot.  Ensure to install your Thunderbolt™ card to the PCIEX16(G3)---_2 slot. 

• Step 6 is optional, please connect a 6-pin PCIe power connector when you wish to  use the USB Type-C® port Thunderbolt™ quick charge feature to charge a 5V or  more device. 

• The TypeC---_1 port can support up to 20V devices, and the TypeC---_2 port can support  up to 9V devices when the 6-pin PCIe power connector is connected. 

• Please visit the official website of your purchased Thunderbolt™ card for more details  on compatibility.

Pro WS W680-ACE Series 2-19   
**2.1.11 M.2 Wi-Fi module and antenna installation** 

**Installing the M.2 W-Fi Module** 

2 

1 

4   
**Chapter 2**   
3 

• Ensure that the    
ASUS Wi-Fi moving    
antenna is securely    
installed to the Wi-Fi    
ports. 

5 

• Ensure that the  antenna is at least    
20 cm away from all  persons. 

• The illustration  to the left is for    
reference only. The  I/O port layout may    
vary with models,    
but the Wi-Fi    
antenna installation  procedure is the    
same for all models. 

• The M.2 Wi-Fi  module and antenna  are purchased  

separately.

2-20 Chapter 2: Basic Installation   
**2.2 BIOS update utility** 

**BIOS FlashBack**™ 

BIOS FlashBack™ allows you to easily update the BIOS without entering the existing BIOS  or operating system.  

**To use BIOS FlashBack™:** 

1---. Visit https://www.asus.com/support/ and download the latest BIOS version for this  motherboard. 

2---. Manually rename the file as follows depending on your motherboard model: 

| Pro WS W680-ACE IPMI:  | PWW680AI.CAP |
| :---- | :---- |
| **Pro WS W680-ACE:**  | PWW680A.CAP |

or launch the **BIOSRenamer.exe** application to automatically rename the file, then  copy it to your USB storage device. 

The **BIOSRenamer.exe** application is zipped together with your BIOS file when you    
**Chapter 2**   
download a BIOS file for a BIOS FlashBack™ compatible motherboard.  

3---. Plug the 24-pin power connector to the motherboard and turn on the power supply  (no need to power on the system). Insert the USB storage device to the BIOS  

FlashBack™ port. 

4---. Press the BIOS FlashBack™ button for three (3) seconds until the BIOS FlashBack™  LED blinks three times, indicating that the BIOS FlashBack™ function is enabled. 

**BIOS FlashBack™ port BIOS FlashBack™ button** 

5---. Wait until the light goes out, indicating that the BIOS updating process is completed. 

For more BIOS update utilities in BIOS setup, refer to the section **Updating BIOS** in    
Chapter 3---. 

• Do not unplug portable disk, power system, or short the CLR---_CMOS header while    
BIOS update is ongoing, otherwise update will be interrupted. In case of interruption,    
please follow the steps again. 

• If the light flashes for five seconds and turns into a solid light, this means that the    
BIOS FlashBack™ is not operating properly. This may be caused by improper    
installation of the USB storage device and filename/file format error. If this scenario    
happens, please restart the system to turn off the light.  

• Updating BIOS may have risks. If the BIOS program is damaged during the process    
and results to the system’s failure to boot up, please contact your local ASUS Service  

Center.

Pro WS W680-ACE Series 2-21   
For more information on using the BIOS FlashBack™ feature, please refer to  https://www.asus.com/support/, or by scanning the QR code below.

![][image4]

**Chapter 2** 

2-22 Chapter 2: Basic Installation   
**2.3 Motherboard rear and audio connections** 

**2.3.1 Rear I/O connection** 

**Chapter 2** 

| Rear panel connectors  1---. DisplayPort |
| :---- |
| 2---. USB 2.0 ports 11 and 12 |
| 3---. Intel® 2.5Gb Ethernet ports---* |
| 4---. HDMI® port |
| 5---. VGA port |
| 6---. USB 3.2 Gen 1 Type-A ports 5 and 6 |
| 7---. USB 3.2 Gen 1 Type-A ports 3 and 4 |
| 8---. USB 3.2 Gen 2 Type-A port 2 |
| 9---. USB 3.2 Gen 2 Type-C® port C1 |
| 10---. BIOS FlashBack™ button |
| 11---. Audio jacks---*---* |

**---* and ---*---* : Refer to the tables on the next page for LAN port LEDs, and audio port definitions.** 

We strongly recommend that you connect your devices to ports with matching data    
transfer rate. For example connecting your USB 3.2 Gen 1 devices to USB 3.2 Gen 1  

ports for faster and better performance for your devices.

Pro WS W680-ACE Series 2-23   
**---* Intel® 2.5Gb Ethernet port LED indications** 

**ACT/LINK**  

| Activity Link LED |  |  | Speed LED |  |
| :---- | ----- | :---- | :---- | :---- |
| **Status**  | **Description** |  | **Status** |  **Description** |
| OFF  | No link |  | OFF |  No link |
| GREEN  | Linked |  | OFF  | 100 Mbps / 10 Mbps  connection |
| BLINKING |  Data activity |  |  |  |
|  |  |  | GREEN |  2.5 Gbps connection |
|  |  |  | ORANGE |  1 Gbps connection |

**---*---* Audio 2, 4, 5.1 or 7.1-channel configuration**  
**LED SPEED  LED** 

**LAN port** 

| Port |  2-channel |  4-channel |  5.1-channel |  7.1-channel |
| ----- | :---: | :---: | :---: | :---: |
| Light Blue (Rear panel)  | ----  | ----  | ----  | Side Speaker Out |
| Lime  (Rear panel) | Front Speaker Out | Front Speaker Out | Front Speaker Out | Front Speaker Out |
| Pink(Rear panel)  | ----  | ----  | ----  | ---- |
| Black(Rear panel)  | ----  | Rear Speaker Out | Rear Speaker Out | Rear Speaker Out |
| Orange (Rear panel)  | ----  | ----  | Center/ Subwoofer | Center/ Subwoofer |

**Chapter 2** 

2-24 Chapter 2: Basic Installation   
**2.3.2 Audio I/O connections Audio I/O ports** 

**Connect to Headphone and Mic Connect to 2-channel Speakers** 

**Connect to 4-channel Speakers**

**Chapter 2** 

Pro WS W680-ACE Series 2-25 

**Chapter 2**   
**Connect to 5.1-channel Speakers Connect to 7.1-channel Speakers**

2-26 Chapter 2: Basic Installation   
**2.4 Starting up for the first time** 

1---. After making all the connections, replace the system case cover.  

2---. Ensure that all switches are off. 

3---. Connect the power cord to the power connector at the back of the system chassis. 4---. Connect the power cord to a power outlet that is equipped with a surge protector. 5---. Turn on the devices in the following order: 

a. Monitor 

b. External storage devices (starting with the last device on the chain) 

c. System power 

6---. After applying power, the system power LED on the system front panel case lights  up. For systems with ATX power supplies, the system LED lights up when you press  the ATX power button. If your monitor complies with the “green” standards or if it has    
**Chapter 2**   
a “power standby” feature, the monitor LED may light up or change from orange to  green after the system LED turns on. 

The system then runs the power-on self tests (POST). While the tests are running,  the BIOS beeps (refer to the BIOS beep codes table) or additional messages appear  on the screen. If you do not see anything within 30 seconds from the time you turned  on the power, the system may have failed a power-on test. Check the jumper settings  and connections or call your retailer for assistance. 

| BIOS Beep  One short beep | Description  VGA detected  Quick boot set to disabled   No keyboard detected  |
| :---- | :---- |
| One continuous beep followed by two short  beeps then a pause (repeated)  | No memory detected |
| One continuous beep followed by three short  beeps  | No VGA detected |
| One continuous beep followed by four short  beeps  | Hardware component failure |

7---. At power on, hold down the ---<Delete---> key to enter the BIOS Setup. Follow the  instructions in Chapter 3---. 

**2.5 Turning off the computer** 

While the system is ON, press the power button for less than four seconds to put the system  on sleep mode or soft-off mode, depending on the BIOS setting. Press the power button  for more than four seconds to let the system enter the soft-off mode regardless of the BIOS  setting.

Pro WS W680-ACE Series 2-27   
**Chapter 2**

2-28 Chapter 2: Basic Installation   
**Chapter 3: BIOS and RAID Support**   
**BIOS and RAID Support** 3 

**3.1 Knowing BIOS** 

The new ASUS UEFI BIOS is a Unified Extensible Interface that complies with UEFI  

architecture, offering a user-friendly interface that goes beyond the traditional keyboard   
only BIOS controls to enable a more flexible and convenient mouse input. You can easily    
navigate the new UEFI BIOS with the same smoothness as your operating system. The    
term “BIOS” in this user manual refers to “UEFI BIOS” unless otherwise specified. 

BIOS (Basic Input and Output System) stores system hardware settings such as storage  device configuration, overclocking settings, advanced power management, and boot  device configuration that are needed for system startup in the motherboard CMOS. In  normal circumstances, the default BIOS settings apply to most conditions to ensure  optimal performance. **DO NOT change the default BIOS settings** except in the following  circumstances:  

• An error message appears on the screen during the system bootup and requests you  to run the BIOS Setup. 

• You have installed a new system component that requires further BIOS settings or  update. 

Inappropriate BIOS settings may result to instability or boot failure. **We strongly**    
**recommend that you change the BIOS settings only with the help of a trained**    
**Chapter 3**   
**service personnel**. 

BIOS settings and options may vary due to different BIOS release versions. Please refer    
to the latest BIOS version for settings and options. 

For more information on BIOS configurations, please refer to    
https://www.asus.com/support, or download the BIOS manual  

by scanning the QR code.

Pro WS W680-ACE Series 3-1   
**3.2 BIOS setup program** 

Use the BIOS Setup to update the BIOS or configure its parameters. The BIOS screen  include navigation keys and brief onscreen help to guide you in using the BIOS Setup  program. 

**Entering BIOS at startup** 

To enter BIOS Setup at startup, press ---<Delete---> or ---<F2---> during the Power-On Self Test  (POST). If you do not press ---<Delete---> or ---<F2--->, POST continues with its routines. 

**Entering BIOS Setup after POST** 

To enter BIOS Setup after POST: 

• Press ---<Ctrl--->+---<Alt--->+---<Delete---> simultaneously. 

• Press the reset button on the system chassis. 

• Press the power button to turn the system off then back on. Do this option only if you  failed to enter BIOS Setup using the first two options. 

After doing either of the three options, press ---<Delete---> key to enter BIOS. 

• Ensure that a USB mouse is connected to your motherboard if you want to use the    
mouse to control the BIOS setup program. 

• If the system becomes unstable after changing any BIOS setting, load the default    
settings to ensure system compatibility and stability. Select the **Load Optimized**    
**Defaults** item under the **Exit** menu or press hotkey **---<F5--->**. 

• If the system fails to boot after changing any BIOS setting, try to clear the CMOS and  reset the motherboard to the default value. 

• The BIOS setup program does not support Bluetooth devices.   
**Chapter 3**   
**BIOS menu screen** 

The BIOS Setup program can be used under two modes: **EZ Mode** and **Advanced Mode**.  You can change modes from **Setup Mode** in **Boot menu** or by pressing the ---<F7---> hotkey.

3-2 Chapter 3: BIOS Setup   
**3.3 ASUS EZ Flash 3** 

The ASUS EZ Flash 3 feature allows you to update the BIOS without using an OS‑based  utility. 

Ensure to load the BIOS default settings to ensure system compatibility and stability.    
Select the **Load Optimized Defaults** item under the **Exit** menu or press hotkey **---<F5--->**. 

**To update the BIOS:** 

• This function can support devices such as a USB flash disk with FAT 32/16 format    
and single partition only. 

• DO NOT shut down or reset the system while updating the BIOS to prevent system    
boot failure---! 

1---. Insert the USB flash disk that contains the latest BIOS file to the USB port. 

2---. Enter the Advanced Mode of the BIOS setup program. Go to the **Tool** menu to select  **ASUS EZ Flash 3 Utility** and press ---<Enter--->. 

3---. Press the Left arrow key to switch to the **Drive** field. 

4---. Press the Up/Down arrow keys to find the USB flash disk that contains the latest  BIOS, and then press ---<Enter--->.  

5---. Press the Right arrow key to switch to the **Folder** field. 

6---. Press the Up/Down arrow keys to find the BIOS file, and then press ---<Enter---> to  perform the BIOS update process. Reboot the system when the update process is  done.

**Chapter 3** 

Pro WS W680-ACE Series 3-3   
**3.4 ASUS CrashFree BIOS 3** 

The ASUS CrashFree BIOS 3 utility is an auto recovery tool that allows you to restore the  BIOS file when it fails or gets corrupted during the updating process. You can restore a  corrupted BIOS file using a USB flash drive that contains the BIOS file. 

**Recovering the BIOS** 

1---. Download the latest BIOS version for this motherboard from https://www.asus.com/ support/. 

2---. Rename the BIOS file as **asus.cap** or **PWW680AI.cap** and copy the renamed BIOS  file to a USB flash drive. 

3---. Turn on the system. 

4---. Insert the USB flash drive containing the BIOS file to a USB port. 

5---. The utility automatically checks the devices for the BIOS file. When found, the utility  reads the BIOS file and enters ASUS EZ Flash 3 automatically. 

6---. The system requires you to enter BIOS Setup to recover the BIOS setting. To ensure  system compatibility and stability, we recommend that you press ---<F5---> to load default  BIOS values. 

DO NOT shut down or reset the system while updating the BIOS---! Doing so can cause  

system boot failure---!

**Chapter 3** 

3-4 Chapter 3: BIOS Setup   
**3.5 RAID configurations** 

The motherboard comes with the Intel® Rapid Storage Technology that supports PCIe RAID  0/1/5/10 and SATA RAID 0/1/5/10 configurations. 

For more information on configuring your RAID sets, please    
refer to the **RAID Configuration Guide** which you can find at  

https://www.asus.com/support, or by scanning the QR code.

**RAID definitions** 

**RAID 0 (Data striping)** optimizes two identical hard disk drives to read and write data in  parallel, interleaved stacks. Two hard disks perform the same work as a single drive but at a  sustained data transfer rate, double that of a single disk alone, thus improving data access  and storage. Use of two new identical hard disk drives is required for this setup. **RAID 1 (Data mirroring)** copies and maintains an identical image of data from one drive to  a second drive. If one drive fails, the disk array management software directs all applications  to the surviving drive as it contains a complete copy of the data in the other drive. This RAID  configuration provides data protection and increases fault tolerance to the entire system.  Use two new drives or use an existing drive and a new drive for this setup. The new drive  must be of the same size or larger than the existing drive. 

**RAID 5** stripes both data and parity information across three or more hard disk drives.  Among the advantages of RAID 5 configuration include better HDD performance, fault  tolerance, and higher storage capacity. The RAID 5 configuration is best suited for  transaction processing, relational database applications, enterprise resource planning, and  other business systems. Use a minimum of three identical hard disk drives for this setup. **RAID 10** is data striping and data mirroring combined without parity (redundancy data)  **Chapter 3**   
having to be calculated and written. With the RAID 10 configuration you get all the benefits  of both RAID 0 and RAID 1 configurations. Use four new hard disk drives or use an existing  drive and three new drives for this setup. 

Pro WS W680-ACE Series 3-5   
**Chapter 3**

3-6 Chapter 3: BIOS Setup   
**Appendix** 

**Appendix** 

**Q-Code table** 

| Code |  Description |
| :---- | :---- |
| **00**  | Not used |
| **01**  | Power on. Reset type detection (soft/hard). |
| **02**  | AP initialization before microcode loading |
| **03**  | System Agent initialization before microcode loading |
| **04**  | PCH initialization before microcode loading |
| **06**  | Microcode loading |
| **07**  | AP initialization after microcode loading |
| **08**  | System Agent initialization after microcode loading |
| **09**  | PCH initialization after microcode loading |
| **0B**  | Cache initialization |
| **0C – 0D**  | Reserved for future AMI SEC error codes |
| **0E**  | Microcode not found |
| **0F**  | Microcode not loaded |
| **10**  | PEI Core is started |
| **11 – 14**  | Pre-memory CPU initialization is started |
| **15 – 18**  | Pre-memory System Agent initialization is started  |
| **19 – 1C**  | Pre-memory PCH initialization is started |
| **2B – 2F**  | Memory initialization |
| **30**  | Reserved for ASL (see ASL Status Codes section below) |
| **31**  | Memory Installed |
| **32 – 36**  | CPU post-memory initialization |
| **37 – 3A**  | Post-Memory System Agent initialization is started |
| **3B – 3E**  | Post-Memory PCH initialization is started |
| **4F**  | DXE IPL is started |
| **50 – 53**  | Memory initialization error. Invalid memory type or incompatible memory speed |
| **54**  | Unspecified memory initialization error |
| **55**  | Memory not installed |
| **56**  | Invalid CPU type or Speed |
| **57**  | CPU mismatch |
| **58**  | CPU self test failed or possible CPU cache error |
| **59**  | CPU micro-code is not found or micro-code update is failed |
| **5A**  | Internal CPU error |
| **5B**  | Reset PPI is not available |
| **5C – 5F**  | Reserved for future AMI error codes |

**Appendix** 

(continued on the next page)

Pro WS W680-ACE Series A-1   
**Q-Code table** 

| Code |  Description |
| :---- | :---- |
| **E0**  | S3 Resume is stared (S3 Resume PPI is called by the DXE IPL) |
| **E1**  | S3 Boot Script execution |
| **E2**  | Video repost |
| **E3**  | OS S3 wake vector call |
| **E4 – E7**  | Reserved for future AMI progress codes |
| **E8**  | S3 Resume Failed |
| **E9**  | S3 Resume PPI not Found |
| **EA**  | S3 Resume Boot Script Error |
| **EB**  | S3 OS Wake Error |
| **EC – EF**  | Reserved for future AMI error codes |
| **F0**  | Recovery condition triggered by firmware (Auto recovery) |
| **F1**  | Recovery condition triggered by user (Forced recovery) |
| **F2**  | Recovery process started |
| **F3**  | Recovery firmware image is found |
| **F4**  | Recovery firmware image is loaded |
| **F5 – F7**  | Reserved for future AMI progress codes |
| **F8**  | Recovery PPI is not available |
| **F9**  | Recovery capsule is not found |
| **FA**  | Invalid recovery capsule |
| **FB – FF**  | Reserved for future AMI error codes |
| **60**  | DXE Core is started |
| **61**  | NVRAM initialization |
| **62**  | Installation of the PCH Runtime Services |
| **63 – 67**  | CPU DXE initialization is started |
| **68**  | PCI host bridge initialization |
| **69**  | System Agent DXE initialization is started |
| **6A**  | System Agent DXE SMM initialization is started |
| **6B – 6F**  | System Agent DXE initialization (System Agent module specific) |
| **70**  | PCH DXE initialization is started |
| **71**  | PCH DXE SMM initialization is started |
| **72**  | PCH devices initialization |
| **73 – 77**  | PCH DXE Initialization (PCH module specific) |
| **78**  | ACPI module initialization |
| **79**  | CSM initialization |
| **7A – 7F**  | Reserved for future AMI DXE codes |

**Appendix**   
(continued on the next page)

A-2 Appendix   
**Q-Code table** 

| Code |  Description |
| :---- | :---- |
| **90**  | Boot Device Selection (BDS) phase is started |
| **91**  | Driver connecting is started |
| **92**  | PCI Bus initialization is started |
| **93**  | PCI Bus Hot Plug Controller Initialization |
| **94**  | PCI Bus Enumeration |
| **95**  | PCI Bus Request Resources |
| **96**  | PCI Bus Assign Resources |
| **97**  | Console Output devices connect |
| **98**  | Console input devices connect |
| **99**  | Super IO Initialization |
| **9A**  | USB initialization is started |
| **9B**  | USB Reset |
| **9C**  | USB Detect |
| **9D**  | USB Enable |
| **9E – 9F**  | Reserved for future AMI codes |
| **A0**  | IDE initialization is started |
| **A1**  | IDE Reset |
| **A2**  | IDE Detect |
| **A3**  | IDE Enable |
| **A4**  | SCSI initialization is started |
| **A5**  | SCSI Reset |
| **A6**  | SCSI Detect |
| **A7**  | SCSI Enable |
| **A8**  | Setup Verifying Password |
| **A9**  | Start of Setup |
| **AA**  | Reserved for ASL (see ASL Status Codes section below) |
| **AB**  | Setup Input Wait |
| **AC**  | Reserved for ASL (see ASL Status Codes section below) |
| **AD**  | Ready To Boot event |
| **AE**  | Legacy Boot event |
| **AF**  | Exit Boot Services event |
| **B0**  | Runtime Set Virtual Address MAP Begin |
| **B1**  | Runtime Set Virtual Address MAP End |
| **B2**  | Legacy Option ROM Initialization |
| **B3**  | System Reset |

**Appendix**   
(continued on the next page)

Pro WS W680-ACE Series A-3   
**Q-Code table** 

| Code |  Description |
| :---- | :---- |
| **B4**  | USB hot plug |
| **B5**  | PCI bus hot plug |
| **B6**  | Clean-up of NVRAM |
| **B7**  | Configuration Reset (reset of NVRAM settings) |
| **B8– BF**  | Reserved for future AMI codes |
| **D0**  | CPU initialization error |
| **D1**  | System Agent initialization error |
| **D2**  | PCH initialization error |
| **D3**  | Some of the Architectural Protocols are not available |
| **D4**  | PCI resource allocation error. Out of Resources |
| **D5**  | No Space for Legacy Option ROM |
| **D6**  | No Console Output Devices are found |
| **D7**  | No Console Input Devices are found |
| **D8**  | Invalid password |
| **D9**  | Error loading Boot Option (LoadImage returned error) |
| **DA**  | Boot Option is failed (StartImage returned error) |
| **DB**  | Flash update is failed |
| **DC**  | Reset protocol is not available |

**ACPI/ASL Checkpoints (under OS)**

| Code |  Description |
| :---- | :---- |
| **03**  | System is entering S3 sleep state |
| **04**  | System is entering S4 sleep state |
| **05**  | System is entering S5 sleep state |
| **30**  | System is waking up from the S3 sleep state |
| **40**  | System is waking up from the S4 sleep state |
| **AC**  | System has transitioned into ACPI mode. Interrupt controller is in PIC mode. |
| **AA**  | System has transitioned into ACPI mode. Interrupt controller is in APIC mode. |

**Appendix** 

A-4 Appendix   
**Notices** 

**FCC Compliance Information** 

Responsible Party: Asus Computer International 

Address: 48720 Kato Rd., Fremont, CA 94538, USA 

Phone / Fax No: (510)739-3777 / (510)608-4555 

This device complies with part 15 of the FCC Rules. Operation is subject to the following  two conditions: (1) This device may not cause harmful interference, and (2) this device must  accept any interference received, including interference that may cause undesired operation. This equipment has been tested and found to comply with the limits for a Class B digital  device, pursuant to part 15 of the FCC Rules. These limits are designed to provide  reasonable protection against harmful interference in a residential installation. This  equipment generates, uses and can radiate radio frequency energy and, if not installed  and used in accordance with the instructions, may cause harmful interference to radio  communications. However, there is no guarantee that interference will not occur in a  particular installation. If this equipment does cause harmful interference to radio or television  reception, which can be determined by turning the equipment off and on, the user is  encouraged to try to correct the interference by one or more of the following measures:  ---- Reorient or relocate the receiving antenna. 

 ---- Increase the separation between the equipment and receiver. 

 ---- Connect the equipment into an outlet on a circuit different from that to which the receiver  is connected. 

 ---- Consult the dealer or an experienced radio/TV technician for help. 

**HDMI Compliance Statement** 

The terms HDMI, HDMI High-Definition Multimedia Interface, and the HDMI Logo are  trademarks or registered trademarks of HDMI Licensing Administrator, Inc.

**Appendix** 

Pro WS W680-ACE Series A-5   
**Compliance Statement of Innovation, Science and Economic  Development Canada (ISED)** 

This device complies with Innovation, Science and Economic Development Canada licence  exempt RSS standard(s). Operation is subject to the following two conditions: (1) this device  may not cause interference, and (2) this device must accept any interference, including  interference that may cause undesired operation of the device. 

CAN ICES-003(B)/NMB-003(B) 

**Déclaration de conformité de Innovation, Sciences et**    
**Développement économique Canada (ISED)** 

Le présent appareil est conforme aux CNR d’Innovation, Sciences et Développement  économique Canada applicables aux appareils radio exempts de licence. L’exploitation est  autorisée aux deux conditions suivantes : (1) l’appareil ne doit pas produire de brouillage,  et (2) l’utilisateur de l’appareil doit accepter tout brouillage radioélectrique subi, même si le  brouillage est susceptible d’en compromettre le fonctionnement. 

CAN ICES-003(B)/NMB-003(B) 
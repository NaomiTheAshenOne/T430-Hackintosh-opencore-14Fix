# Thinkpad T430 - Hackintosh - OpenCore
[![T430](https://img.shields.io/badge/ThinkPad-T430-blueviolet.svg)](https://psref.lenovo.com/syspool/Sys/PDF/withdrawnbook/ThinkPad_T430.pdf)
[![OC](https://img.shields.io/badge/OpenCore-0.9.2-informational.svg)](https://github.com/acidanthera/OpenCorePkg/releases/tag/0.9.2)
[![10.13](https://img.shields.io/badge/macOS-10.13-yellowgreen.svg)]()
[![10.14](https://img.shields.io/badge/macOS-10.14-blue.svg)]()
[![10.15](https://img.shields.io/badge/macOS-10.15-9cf.svg)]()
[![11](https://img.shields.io/badge/macOS-11-red.svg)]()
[![12](https://img.shields.io/badge/macOS-12-blueviolet.svg)]()
[![13](https://img.shields.io/badge/macOS-13-yellow.svg)]()
[![14](https://img.shields.io/badge/macOS-14-orange.svg)]()
[![download](https://img.shields.io/badge/Download-latest-success.svg)](https://github.com/jozews321/T430-Hackintosh-Opencore/releases/latest)
<img align="left" src="/resources/T430-new.png" alt="Lenovo Thinkpad T430" width="300">
<img align="right" src="/resources/homepage.png" alt="Opencore" width="200">
<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
## WHAT DOES THIS FORK CHANGE?
This fork is an updated version of jozews preconfigured EFI, in MacOS 14.4+ some breaking API changes were introduced that causes the orignal EFI to crash when trying to install Sonoma. Huge thanks to 
  
## IMPORTANT NOTES ABOUT THIS FORK
* By default this config is for 768p displays, in config.plist change APPL,ig-platform-id from 03006601 to 04006601 for 900p displays.
* Delete all data on your drive before installing (if not already using Mac)
* Make sure to read the entire guide even if you have read the orginal as some things differ
* Both fresh install and upgrade from MacOS have been tested and work

## SUMMARY 
This guide will provide a EFI folder configured to install macOS 10.13 to 14 on the Thinkpad T430 using OpenCore Bootloader with every device working with some exceptions depending on your particular T430 model (see below)

|:warning: This guide assumes prior knowledge on how to do basic Hackintosh stuff |
|:--------------------------------------------------------------------|
If you are new to this i suggest you to go to [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/) first
There will be references to the linked guide throught this proccess for those that are new to this

## REQUIREMENTS
- Lenovo Thinkpad T430 (T430s is similar enough to work with this config, but i can't verify)
- Intel Core i3 3110M or better (Celeron and Pentium CPUs are not supported)
- If using nvidia GPU disable before following the guide
- At least 4GB RAM 
- Nearly Any intel wifi card should work
- Integrated Fingerprint reader and WWAN card are not supported
- Dual Booting is discouraged in this guide as there is many boot scenarios to cover properly here
- SSD (HDDs suck)
- 32GB USB drive

## ABOUT
The provided ACPI tables in this OC EFI folder are made using a DSDT-less method via hotpatching to increase compatibility with different BIOS versions.

## HARDWARE TESTED
Do a pull request to add more Hardware configs to this list
<details>
<summary><strong>NaomiTheAshenOne's T430</strong></summary>

### ThinkPad T430 Specs 
| Component           | Details                                       |
| ------------------: | :-------------------------------------------- |
| Model               | Lenovo ThinkPad T430                          |
| BIOS Version        | 1vyRain                                       |
| Processor           | Intel Core i7 3740QM                          |
| Memory              | 16GB DDR3 1600MHz in Dual-Channel             |
| SSD                 | 256GB SSD                                     |
| Graphics            | Intel HD Graphics 4000                        |
| Display             | 15.6" 1366x768                                |
| Audio               | Realtek ALC269VC                              |
| Ethernet            | N/A                                           |
| WIFI                | Intel ax200                                   |
| Bluetooth           | Intel ax200                                   |
  
</details>


## INSTALL GUIDE
<details>
<summary><strong>Getting the EFI ready</strong></summary>
	
Download the latest release of the EFI 
	
</details>
<details>
<summary><strong>BIOS Settings</strong></summary>

### BIOS Settings
Latest BIOS Version: `2.77` stock or ivyrain

**CONFIG TAB**

* USB UEFI BIOS Support: `Enabled`
* USB 3.0 Mode: `Enabled`
* Display > Boot Display Device: `ThinkPad LCD`
* SATA > SATA Controller Mode: `XHCI`
* CPU > Core Multi-Processing: `Enabled`
* CPU > Intel (R) Hyper-Threading: `Enabled`

**SECURITY TAB**

* Security Chip: `Disabled`
* UEFI BIOS Update Options > Flash BIOS Updating by End-Users: `Enabled`
* UEFI BIOS Update Options > Secure Rollback Prevention: `Enabled`
* Memory Protection: `Enabled`
* Virtualization > Intel (R) Virtualization Technology: `Enabled` 
* I/O Port Access (`Disable` the following:):
	* Wireless WAN
	* ExpressCard Slot
	* eSATA Port
	* Fingerprint Reader
	* Antitheft and Computrace
	* Secure Boot: `Disabled`

**STARTUP TAB**

* Boot (HDD/SSD as first device)
* UEFI/Legacy Boot: `UEFI only`
* CSM Support: `Disabled`
* Boot Mode: `Quick`

</details>

<details>
<summary><strong>USB Installation media</strong></summary>

### Creating the USB installer 
In this step you will create a macOS installation media.
* You will need a Mac with opencore legacy patcher installed on it, if you dont own a Mac first follow the original guide and install MacOS 12 on the t430
* Create a offline MacOS installer with opencore legacy and install it to a 32GB or higher USB drive

</details> 

<details>
<summary><strong>Adding the EFI folder to the USB</strong></summary>

* Use MountEFI (https://github.com/corpnewt/MountEFI) to mount the EFI of the USB drive
* Copy the EFI folder from releases into the EFI parition

</details> 

<details>
<summary><strong>Installing macOS</strong></summary>
<br /> 
Boot from the USB by pressing F12 on the Thinkpad BIOS and choose your USB

- You will see the OpenCore Boot Picker and choose to boot from your installation media

- After that select Disk Utility and format your HDD/SSD in APFS
	
- Install as normal
	
You can consult [OpenCore Guide - Installation Process](https://dortania.github.io/OpenCore-Install-Guide/installation/installation-process.html) to get some instructions if you need them.
</details> 

## POST INSTALL
Follow the next steps carefully to get a fully working system 

<details>
<summary><strong>Generating SMBIOS serial</strong></summary>
<br />
	
Now it's time to generate the Serial, MLB, UUID and ROM to the config.plist (you will need to have ProperTree installed)

- Download [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS/)
  <br /> <br /> 
- Open config.plist with ProperTree in the EFI folder
  <br /> <br /> 
- Open GenSMBIOS
  <br /> <br /> 
- Choose 1 to install MacSerial
  <br /> <br /> 
- Choose 3 to generate some new serials
  <br /> <br /> 
- Write MacBookPro12,1
  <br /> <br /> 
- You will get something like this
  <br /> <br />  
<img src="/resources/gensmbios.png" width="600">
  <br /> <br /> 
  
- If you care about iServices you will need to try the generated serial in [Apple Coverage](https://checkcoverage.apple.com)
  and try to get this message (use a VPN or TOR to get around the rate limits) 
<img src="/resources/notvalidated.png" width="600">
   <br /> <br /> 
  
- Add the generated serials in the config.plist at /PlatformInfo/Generic
  (Serial to SystemSerialNumber, Board Serial to MLB, SmUUID to SystemUUID, AppleRom to ROM)
<img src="/resources/configsmbios.png" width="600">
   <br /> <br /> 

- Save and continue
</details> 

<details>
<summary><strong>macOS 12 to 14 - HD 4000 system patch</strong></summary>
<br /> 
Apple dropped the HD 4000 iGPU with macOS 12. If you dont install this you won't have any kind of graphics acceleration and your macOS 12-14 experience will be completely miserable

- Download [OpenCore Legacy Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher/releases)

- Run OLCP
	
- Choose Post Install Root Patch and follow instructions
	
- Reboot
	
If everything went right, now you would be able to control the brightness and enjoy fully Metal accelerated UI
</details> 


<details>
<summary><strong>Boot without USB</strong></summary>
<br /> 

- Download [MountEFI](https://github.com/corpnewt/MountEFI)

- Choose your macOS drive and it should be mounted in Finder 
	
- Copy your EFI folder to the root of the EFI partition on your macOS drive
	
- Reboot and disconnect your USB drive
	
- Boot from disk
	
</details> 

<details>
<summary><strong>Optimizing CPU power management</strong></summary>
<br /> 
Follow this guide(Don't skip or your cpu will be locked at minimum or base clock and everything will be very slow):
https://dortania.github.io/OpenCore-Post-Install/universal/pm.html#sandy-and-ivy-bridge-power-management

	
</details> 

## Credits

Thanks to:
* [jozews321] (https://github.com/jozews321) (Original EFI creator)

* [positivew] (https://github.com/positivew) (The idea of using MacbookPro12,1 instead of 10,1) 
* [Apple](https://www.apple.com) (macOS)
* [Acidanthera](https://github.com/acidanthera) (OpenCore, VirtualSMC, Lilu, WhateverGreen and a lot more)
* [Dortania](https://dortania.github.io) (Opencore Install Guide, Opencore Legacy Patcher)
* [OpenIntelWireless](https://github.com/OpenIntelWireless/itlwm) (Airportitlwm)
* [5T33Z0](https://github.com/5T33Z0/Lenovo-T530-Hackinosh-OpenCore) (T530 ACPI fixes)
* [zhen-zen](https://github.com/zhen-zen/YogaSMC) (YogaSMC)

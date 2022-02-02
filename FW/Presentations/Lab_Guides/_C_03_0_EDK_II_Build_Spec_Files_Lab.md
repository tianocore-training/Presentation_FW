---
## ## Slide  1   Platform Build Lab
<br><br><br><br><br>
##  UEFI & EDK II Training 

#### EDK II Build Specification Files - Windows and Linux

<br>
<a href='http://www.tianocore.org'>tianocore.org</a>

<!---
 Lab_Guide.md for UEFI / EDK II Training  EDK II Build Specification Files - Windows and Linux

  Copyright (c) 2020-2022, Intel Corporation. All rights reserved.<BR>

  Redistribution and use in source (original document form) and 'compiled'
  forms (converted to PDF, epub, HTML and other formats) with or without
  modification, are permitted provided that the following conditions are met:

  1) Redistributions of source code (original document form) must retain the
     above copyright notice, this list of conditions and the following
     disclaimer as the first lines of this file unmodified.

  2) Redistributions in compiled form (transformed to other DTDs, converted to
     PDF, epub, HTML and other formats) must reproduce the above copyright
     notice, this list of conditions and the following disclaimer in the
     documentation and/or other materials provided with the distribution.

  THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR
  IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
  MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO
  EVENT SHALL TIANOCORE PROJECT  BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
  SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
  PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS;
  OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
  WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR
  OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF
  ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

-->

---  
## Slide  2  Lesson Objective

###  Lesson Objective  <br>

<!---  Add bullets using https://fontawesome.com/cheatsheet certificate
-->

- Examine the Build components and  build text files DSC, DEC, & FDF 

  

---
## Slide  3  EDK II Build Text Files

###  EDK ii Build Text Files

EDK II tools use INI-style text-based files to describe components, platforms and firmware volumes.


---
## Slide  4  Build Description File Types
 
 EDK II Spec files
 * INF
 * DEC
 * DSC
 * FDF


 
 
 
 [https://github.com/tianocore/tianocore.github.io/wiki/Build-Description-Files](https://github.com/tianocore/tianocore.github.io/wiki/Build-Description-Files)
 
 [https://github.com/tianocore/tianocore.github.io/wiki/EDK-II-Specifications](https://github.com/tianocore/tianocore.github.io/wiki/EDK-II-Specifications)

--- 
## Slide 5 Package Declaration File (DEC)



### **Declare**

Syntax:
```
     <DECfile> ::=	<Defines> 
				Include 
				[<LibraryClass>] 
				[<Guids>] 
				[<Protocols>] 
				[<Ppis>] 
				[<Pcd>] 
				[<UserExtensions>] 

```



* One DEC file per package
* Required for EDK II modules using extended INF and extended DSC format files 

* The “ D” in DEC stands for “declaration”, as in package declaration file (DEC).
* There is one DEC file for each package.
* This file is required for using EDK II modules using the extended INF and extended DSC format files. 
* If you make a new package you must have a DEC file for it.
* SYNTAX -
* The DEC has a defines section that says what this package is. It gives it a GUID and a name. Every other section described here is optional. 
* It can have an includes section words saying “the include directories that this package has are as follows”. Then you would have some include directories. For example, you might be able to say this is my IA64 include and this is my X64 include, etc.
* It has a library class section (optional). This exposes the library classes are defined in this package.
* It has GUID section if there are any GUIDs that you have. Certain structures have GUIDs defines for them. If that structure is defined in this package, it would be listed there.
* Each protocol for every protocol header file that is in your package you list.  You list the GUID of that protocol in the protocol section.
* The same is true for PPIs; they are also identified by GUID.
* PCD, if any module contained in your package defines a new PCD, this is where you tp look it up.
* It is possible to reference a PCD from another package, but do not list it here. This location is for new PCDs.
* Coincidentally,  as soon as you make a new PCD, you must make a new token space GUID, because all the PCDs are defined by a token space GUILD, followed by the PCD name. A new token space means you must have a GUID for the token space. So, any new PCDs are also going to have a GUID.
* Finally, user extensions are rarely used but are optionally present. 

--- 
## Slide 6 Example DEC File
### Declare
```
Defines]
  DEC_SPECIFICATION              = 0x00010005
  PACKAGE_NAME                   = OvmfPkg
  PACKAGE_GUID                   = 2daf5f34-50e5-4b9d-b8e3-5562334d87e5
  PACKAGE_VERSION                = 0.1

[Includes]
  Include

[LibraryClasses]
  ##  @libraryclass  Loads and boots a Linux kernel image
  #
  LoadLinuxLib|Include/Library/LoadLinuxLib.h

[Guids]
  gUefiOvmfPkgTokenSpaceGuid          = {0x93bb96af, 0xb9f2, 0x4eb8, {0x94, 0x62, 0xe0, 0xba, 0x74, 0x56, 0x42, 0x36}}
  gEfiXenInfoGuid                     = {0xd3b46f3b, 0xd441, 0x1244, {0x9a, 0x12, 0x0, 0x12, 0x27, 0x3f, 0xc1, 0x4d}}

[Protocols]
  gVirtioDeviceProtocolGuid           = {0xfa920010, 0x6785, 0x4941, {0xb6, 0xec, 0x49, 0x8c, 0x57, 0x9f, 0x16, 0x0a}}
  gXenBusProtocolGuid                 = {0x3d3ca290, 0xb9a5, 0x11e3, {0xb7, 0x5d, 0xb8, 0xac, 0x6f, 0x7d, 0x65, 0xe6}}

[PcdsFixedAtBuild]
  gUefiOvmfPkgTokenSpaceGuid.PcdOvmfPeiMemFvBase|0x0|UINT32|0x00001014
  gUefiOvmfPkgTokenSpaceGuid.PcdOvmfPeiMemFvSize|0x0|UINT32|0x00001015

```

NOTE: Tokens need to beunique to the DEC file (1 per PCD) 


--- 
## Slide 7 Examine the Dec File Details
### Declare

Follow the following Links and examine the examples of the EmulatorPkg.dec file


Next open the same EmulatorPkg.dec in the %WORKSPACE% and become familiar with the different sections

[EmulatorPkg.dec.md#dec-file-for-emulatorpkg](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.dec.md#dec-file-for-emulatorpkg)
* [Link:](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.dec.md#list-of-list-of-defines--package-name-guild-version-) List of List of Defines, Package Name, GUILD, Version ...
* [Link:](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.dec.md#the-include-sectionthere-is-an-include-directory-for-this-package) The Include section
* [Link:](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.dec.md#library-classes-section-these-are-the-libraries-created-by-this-package-notice-each-is-pointing-to-a-h-file) Library classes section
* [Link:](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.dec.md#protocols-section-defined-by-this-package) Protocols Section
* [Link:](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.dec.md#guids-section-guids-defined-by-this-package) GUIDs section
* [Link:](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.dec.md#pcds-section-declared-by-this-package) PCDs Section
* [Link:](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.dec.md#patchable-pcds-section) Patchable PCDs Section 

--- 
## Slide 8 Platform Description File (DSC)

### **Description**

Syntax :

```
DSCfile ::= [<Header>]
			<Defines>
			[<SkuIds>] 
			[<Libraries>]
			[<LibraryClasses>]
			[<Pcds>] 
			[<Components>]
			[<UserExtensions>] 
```

Notes:

To build a platform the build requires a .DSC file

List the Library implementation with the library names

Change the different settings of the PCD defaults 

List the components for the build to compile and link

Build options for the platform

--- 
## Slide 9 Platform Description File (DSC)

### **Description**

* DSC file is the recipe for creating a package
* Definitions for the package build
* EDK II Library Class Instance Mappings (for EDK II Modules)
* EDK II PCD Entry Settings  
* Components / Modules to build (list of .inf files)

Note:
DSC file must define all libraries, components and/or modules that will be used by one package

--- 
## Slide 10  Example: DSC File

### **Description**
```
Defines]
  PLATFORM_NAME                  = Ovmf
  PLATFORM_GUID                  = 5a9e7754-d81b-49ea-85ad-69eaa7b1539b
  PLATFORM_VERSION               = 0.1
  DSC_SPECIFICATION              = 0x00010005
  OUTPUT_DIRECTORY               = Build/OvmfX64
  SUPPORTED_ARCHITECTURES        = X64
  BUILD_TARGETS                  = NOOPT|DEBUG|RELEASE
  SKUID_IDENTIFIER               = DEFAULT
  FLASH_DEFINITION               = OvmfPkg/OvmfPkgX64.fdf

  #
  # Defines for default states.  These can be changed on the command line.
  # -D FLAG=VALUE
. . .
[BuildOptions.common.EDKII.DXE_RUNTIME_DRIVER]
  GCC:*_*_*_DLINK_FLAGS = -z common-page-size=0x1000
  XCODE:*_*_*_DLINK_FLAGS =
[LibraryClasses]
  PcdLib|MdePkg/Library/BasePcdLibNull/BasePcdLibNull.inf
  TimerLib|OvmfPkg/Library/AcpiTimerLib/BaseAcpiTimerLib.inf
.  .  .
#############################
# Pcd Section 
#############################
 . . .
#############################
#
# Components Section - list of all EDK II Modules needed by this Platform.
#
#############################
[Components]
  OvmfPkg/ResetVector/ResetVector.inf

. . .  

```
DSC must contain a [Components] Section

---

## Slide  11  Examine : DSC File Details
### Examine : DSC File Details

Follow the following Links and examine the examples of the EmulatorPkg.dsc file


Next open the same EmulatorPkg.dsc in the %WORKSPACE% and become familiar with the different sections

[EmulatorPkg.dsc.md#dsc-file-for-emulatorpkg](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.dsc.md#dsc-file-for-emulatorpkg)
* [Link](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.dsc.md#list-of-defines--package-name-guild-version-): List of Defines
* [Link](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.dsc.md#define-switches-to-determine-some-configurations): Define Switches to determine some configurations
* [Link](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.dsc.md#library-classes---global-notice-the-dsc-file-is-pointing-to-the-inf-file): Library Classes – Global
* [Link](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.dsc.md#library-classes-for-the-the-uefi-boot-phases): Library Classes for UEFI Boot phases
* [Link](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.dsc.md#pcds-section-changing-the-default-value-by-this-package): PCDs Section, changing the default
* [Link](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.dsc.md#dynamic-pcds-section-changing-the-default-value-by-this-package): Dynamic PCDs Section
* [Link](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.dsc.md#finally-the-components-section-which-every-dsc-must-have): Components Section
* [Link](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.dsc.md#build-options-section): Build Options Section
* [Link](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.dsc.md#adding-more-modules-etc-done-by-adding-more-sections-to-the-end-of-the-file): Adding More

---

## Slide  12  Flash Description File(FDF)
### **Flash Layout**

Syntax:
```
FDFfile ::= [<Header>]
    [<Defines>] 
    <FD> 
    <FV> 
    [<Capsule>] 
    [<VTF>] 
    [<Rules>] 
    [<OptionRom>]
    [<UserExtensions>] 

```

Must have a FD and FV Section

FD section defines for Flash devices must be in the FDF file
FV – section defines firmware Volumes must be in FDF file


The FDF file is used to describe the content and layout of binary images. Binary images described in this file may be any combination of boot images, capsule images or PCI Options ROMs.



The EDK II FDF file is used in conjunction with an EDK II DSC file to generate bootable images, option ROM images, and update capsules for bootable images that comply with the UEFI specifications

A firmware volume is part of the UEFI Spec in the Platform Initialization (PI) Volume 3 Specification. The PI Spec Volume 3 will describe the details of the UEFI Firmware volume.

---

## Slide  13  Flash Description File(FDF)
### **Flash Layout**

* Describes information about flash parts
* Used to create firmware images, Option ROM images or bootable images
* Rules for combining binaries (Firmware Image) built from a DSC file

Note:
The FDF file describes information about the flash part.
It has rules for combining binaries built from a DSC file.
You can create firmware images option ROM images for nearly anything you needed.
It is possible to have the PCD information used in the definition, and also in some of the PCDs. The patchable ones will be stored at specific places inside the FV file. 

---

## Slide  14  FLASH DEVICE CONFIGURATION COMMON LAYOUT FILE (.FDF)



| Flash Area | Description |
| ----------- | ----------- |
| FV Recover | Used to store the SEC/PEI Phase |
| FTW Spare Space  | Fault Tolerant Write (FTW) regions |
| FTW Working | FTW Working |
| Event Log |  NVRAM storage for event logs|
| Microcode | CPU Microcode |
| Variable Region | Variables & Plaftorm settings  |
|  FV Main| Contents of DXE/BDS/ Etc. Phase Drivers |


Note:  some of these areas are Firmware Volumes and other areas will be data 

---

## Slide  15   Example: FDF File

```
[Defines]
!include OvmfPkg.fdf.inc

#
# Build the variable store and the firmware code as one unified flash device
# image.
#
[FD.OVMF]
BaseAddress   = $(FW_BASE_ADDRESS)
Size          = $(FW_SIZE)
ErasePolarity = 1
BlockSize     = $(BLOCK_SIZE)
NumBlocks     = $(FW_BLOCKS)


$(VARS_SIZE)|$(FVMAIN_SIZE)
FV = FVMAIN_COMPACT

$(SECFV_OFFSET)|$(SECFV_SIZE)
FV = SECFV

```

Note:
Offset | Size are used to descript where in the flash items will be placed


---

## Slide  16 Examine : FDF File Details

Follow the following Links and examine the examples of the EmulatorPkg.fdf file

Next open the same EmulatorPkg.fdf in the %WORKSPACE% and become familiar with the different sections


[EmulatorPkg.fdf.md#fdf-file-for-the-emulatorpkg](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.fdf.md#fdf-file-for-the-emulatorpkg)

* [Link](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.fdf.md#the-fd-section): FD Section
* [Link](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.fdf.md#fvrecovery-firmware-volume): Firmware Volume – FvRecovery
* [Link](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.fdf.md#begin-the-variable-storage-regions--): Begin Firmware Layout Regions
* [Link](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.fdf.md#firmware-volumes-that-are-going-to-be-created): Declaring each Firmware Volumes
* [Link](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.fdf.md#the-apriori-section): Apriori Section
* [Link](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.fdf.md#example-including-another-file-with-another-list-of-modules): Example: #include of fdf file
* [Link](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/EmulatorPkg.fdf.md#rules-section): Rules Section

Following are for the Whiskey Lake UPX (these examples will be used in later projects
* [Link](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/OpenBoardPkg.fdf.md#fdf-for-whiskey-lake-up-xtreme-board): FDF For Whiskey Lake Up Xtreme
* [Link](https://github.com/tianocore-training/Lab_Material_FW/blob/main/FW/Documentation/Application_examples/AppExamplesMd/FlashMapInclude.fdf.md): Flash Map of Up Xtreme


---
## Slide 17 Add a Simple Driver to the Build
Note: this requires Building of the Platform Lab First

* Windows Build Emulator Platform Lab [Link](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_C_01_Platform_Build_Win_Emulator_Lab_guide.md)
* Linux Build Ovmf Platform Lab [Link](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_L_C_01_Platform_Build_OVMF-QEMU_Lab_guide.md)

#### Steps
1. Copy the LabSampleCode/LabSolutions/BlankDrv directory to WORKSPACE edk2
2. Edit the Platform .DSC file and add the BlankDrv component at the end
3. Edit the Platform .FDF file and add the BlankDrv Driver to the DXE section of Firmware Volumes

**Windows Build**
* Copy the LabSampleCode/LabSolutions/BlankDrv directory to C:/FW/edk2-ws/edk2
* Edit EmulatorPkg.dsc and EmlatorPkg.fdf

```shell
  C:/FW/edk2-ws/edk2> Build –D ADD_SHELL_STRING 
  C:/FW/edk2-ws/edk2> RunEmulator.bat
```


**Linux Build**
* Copy the LabSampleCode/SampleApp directory to ~/src/edk2-ws/edk2
* Edit OvmfPkg/OvmfPkgX64.dsc and OvmfPkg/OvmfPkgX64.fdf 
```shell
bash$> cd ~/src/edk-ws/edk2
bash$> build -D ADD_SHELL_STRING -a X64
bash$ cd $HOME/run-ovmf
bash$ cp ~/src/edk2-ws/Build/OvmfX64/DEBUG_GCC5/FV/OVMF.fd bios.bin
bash$ . RunQemu.sh
```
---
## Slide 18 Verify the Driver was Added
At the Shell prompt
```shell
 Shell> Exit
```

Go to the "Device Manager" menu and Verify the "Blank Driver Configuration page" is available

Enter into the BlankDrv Setup Page

Exit by pressing ESC key twice

Then use the "Reset"

On Linux Exit QEMU

---

## Slide  19  Summary
### Summary  <br>

- Examine the Build components and  build text files DSC, DEC, & FDF 


 

---
## Slide  20  Questions
### Questions


---
## Slide  22  Logo Slide

### logo


---
## Slide  23  Acknowledgements
#### Acknowledgements 

```
/**
Redistribution and use in source (original document form) and 'compiled' forms (converted
to PDF, epub, HTML and other formats) with or without modification, are permitted provided
that the following conditions are met:

Redistributions of source code (original document form) must retain the above copyright 
notice, this list of conditions and the following disclaimer as the first lines of this 
file unmodified.

Redistributions in compiled form (transformed to other DTDs, converted to PDF, epub, HTML
and other formats) must reproduce the above copyright notice, this list of conditions and 
the following disclaimer in the documentation and/or other materials provided with the 
distribution.

THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR IMPLIED 
WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND 
FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL TIANOCORE PROJECT BE 
LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES 
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, 
DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, 
WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) 
ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF ADVISED OF THE POSSIBILITY 
OF SUCH DAMAGE.

Copyright (c) 2020-2022, Intel Corporation. All rights reserved.
**/

```

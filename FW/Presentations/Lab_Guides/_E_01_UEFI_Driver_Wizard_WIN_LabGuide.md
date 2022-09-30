# UEFI Driver Wizard Lab
# Windows

---
## Slide 1 UEFI & EDK II Training
### UEFI Driver Wizar Lab - Windows


[tianocore.org](https://www.tianocore.org/)
<!---
LabGuide.md for UEFI / EDK II Training  UEFI Driver Wizard Windows Lab

  Copyright (c) 2021-2022, Intel Corporation. All rights reserved.<BR>

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
## Slide 2 Lesson Objective

- Setup the UEFI Driver Wizard
- Create a UEFI Driver Templated

---
## Slide 3 UEFI Driver Wizard

Creating a Template UEFI Driver with the UEFI Driver Wizard

---
## Slide 4 UEFI Driver Wizard Overview

- Open source tool
- Based on Driver Writer's Guide for UEFI 2.3.1 content
- Intel engineers contributed
- Located on [tianocore.org](https://www.tianocore.org/)

---
## Slide 5 Installing UEFI Driver Wizard 

**Requirements and Options**

- Workspace must contain `BaseTools` , `MdePkg` , `MdeModulePkg` Packages from [tianocore edk2](https://github.com/tianocore/edk2) for Driver development on [tianocore.org](https://www.tianocore.org/)
- Uses previous lab's setup w/ Windows `C:\FW\edk2-ws\`
- Python* scripts from [GitHub](https://github.com/tianocore/edk2-share/tree/master/DriverDeveloper/UefiDriverWizard) then use instructions from README for Python and wxPython versions to install then run

```bash
$ python launch.py
```

---
## Slide 6 Requirements for Your Driver

**Using UEFI Driver Wizard**

- UEFI Device Driver
- UEFI Version 2.7 (0x00020046)

```
#define EFI_2_70_SYSTEM_TABLE_REVISION ((2<<16) | (70DEC)) 
```

- Unloadable driver
- Support IA32 & x64 CPUs
- Returns component name information
- Byte stream device (i.e. UART / Serial I/O)
- Option to produce HII strings & forms for setup

---
## Slide 7 Template File Contents

**Proper UEFI driver entry point**

**Basic driver libraries/headers**

**Skeletons for common driver functions**

**Error values until ported EFI_UNSUPPORTED, EFI_DEVICE_ERROR**


---
## Slide 8 Lab 1: Create a UEFI Driver with the UEFI Driver Wizard

- In this lab, you'll create a new UEFI driver using the UEFI Driver Wizard.
- This will create a set of "c" code files to be used as a template UEFI Driver used in the subsequent driver labs

---
## Slide 9 Lab 1: Install UEFI Driver Wizard

First setup for Building EDK II Windows, See [Lab Setup](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/__C_01_Build_Setup_Download_EDK_II_Win_LabGuide.md)

Install UEFI Driver Wizard

1. **Open** and **Run** `/FW/DriverWizard/UefiDriverWizard.msi`
2. **Click** through "Next" until install finishes

**Open** the UEFI Driver Wizard

---
## Slide 10 Lab 1: UefiDriverWizard - Select Work Space

Click on File and Select "Open WORKSPACE" Or Or *Control+O*

Browse to `C:/FW/edk2-ws/edk2`

Select "OK"

Should say 

"WORKSPACE C:\FW\edk2-ws\edk2 selected"

Note: the environment for EDK II must be setup with edksetup.bat

---
## Slide 11 Lab 1: Create a New UEFI Driver

Control+**N** - to Open Menu

(See PDF for screenshots)

---
## Slide 12 Lab 1: New UEFI Driver Menu

- UEFI Driver Path - Type: "`MyPkg/MyWizardDriver`"

*Note:* "UEFI Driver Name" is filled in.

- **Ensure** all the forms, radio buttons, and boxes are filled in and selected *exactly* like the image to the right. (except GUID)

(See PDF for screenshot)


- "UEFI Driver Version" – Type: "1.0"
- "UEFI Driver Type" – Select: "UEFI Driver Model Device Driver"
- "Optional Features …" – Select:
  - Unloadable
  - Driver Supporte EFI Version Protocol
  - HII Packages for Strings, Fonts, or images
- "CPU Architecture" – Select: "IA32" and "X64"


- **Note:** A new, specific driver GUID will populate, so it will be different than this image

Click **Next**

---
## Slide 13 Lab 1: UEFI Driver Model Optional Features

**Ensure** all the forms, radio buttons, and boxes are filled in and selected *exactly* like the image to the right.

- "Component Name 2 Protocol"
- "Component Name Protocol"
- "HII Packages for Forms ..."

(See PDF for screenshot)

Click **Next**

---
## Slide 14 Lab 1: UEFI Driver Consumed Protocol

**Select**

- "USB Driver that consumes the USB I/O Protocol"

(See PDF for screenshot)

Click **Next**


---
## Slide 15 Lab 1: UEFI Driver Produced Protocols

**Select**

- "Byte stream device (i.e.UART) producing Serial I/O Protocol"

(See PDF for screenshot)

Click **Finish**

---
## Slide 16 Lab 1: UEFI Driver Created

UEFI Driver template created
```
Create UEFI Driver MyWizardDriver
  Create file C:\FW\edk2-ws\edk2\MyPkg\MyWizardDriver\MyWizardDriver.inf
  Create file C:\FW\edk2-ws\edk2\MyPkg\MyWizardDriver\MyWizardDriver.c
  Create file C:\FW\edk2-ws\edk2\MyPkg\MyWizardDriver\MyWizardDriver.h
  Create file C:\FW\edk2-ws\edk2\MyPkg\MyWizardDriver\DriverBinding.h
  Create file C:\FW\edk2-ws\edk2\MyPkg\MyWizardDriver\ComponentName.c
  Create file C:\FW\edk2-ws\edk2\MyPkg\MyWizardDriver\ComponentName.h
  Create file C:\FW\edk2-ws\edk2\MyPkg\MyWizardDriver\MyWizardDriver.uni
  Create file C:\FW\edk2-ws\edk2\MyPkg\MyWizardDriver\MyWizardDriver.vfr
  Create file C:\FW\edk2-ws\edk2\MyPkg\MyWizardDriver\HiiConfigAccess.c
  Create file C:\FW\edk2-ws\edk2\MyPkg\MyWizardDriver\HiiConfigAccess.h
  Create file C:\FW\edk2-ws\edk2\MyPkg\MyWizardDriver\SerialIo.c
  Create file C:\FW\edk2-ws\edk2\MyPkg\MyWizardDriver\SerialIo.h
```

(See PDF for screenshots)

Close the UEFI Wizard

---
## Slide 17 Summary

- Setup the UEFI Driver Wizard
- Create a UEFI Driver Template

---
## Slide 18 Questions?

<br>

---
## Slide 19 Return to Main Training Page

Return to Training Table of Contents for next presentation [link](https://github.com/tianocore-training/Tianocore_Training_Contents/wiki)

---
## Slide 20 Logo Slide

<br>

---
## Slide 21 Acknowledgements

Redistribution and use in source (original document form) and 'compiled‘ forms (converted to PDF, epub, HTML and other formats) with or without modification, are permitted provided that the following conditions are met:

Redistributions of source code (original document form) must retain the above copyright notice, this list of conditions and the following disclaimer as the first lines of this file unmodified.

Redistributions in compiled form (transformed to other DTDs, converted to PDF, epub, HTML and other formats) must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL TIANOCORE PROJECT BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,  BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

Copyright (c) 2021-2022, Intel Corporation. All rights reserved.

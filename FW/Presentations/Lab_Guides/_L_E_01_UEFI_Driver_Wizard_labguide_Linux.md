# UEFI Driver Wizard Lab
# Linux

---
## Slide 1 UEFI & EDK II Training
### UEFI Driver Wizard Lab
**Currently only available in Ubuntu 16.04**

[tianocore.org](https://www.tianocore.org/)

---
## Slide 2 Lesson Objective

Linux Ubuntu 16.04 is only supported

Python Version 2.7 and python-wxgtk

Non-Ubuntu - Continue to [Porting UEFI Driver Lab](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_L_E_02_UEFI_Driver_Port_Linux_LabGuide.md)

- Setup the UEFI Driver Wizard
- Create a UEFI Driver Template

---
## Slide 3 UEFI Driver Wizard

Creating a Template UEFI Driver with the UEFI Driver Wizard


---
## Slide 4 UEFI Driver Wizard Overview

- Open source tool
- Based on Driver Writer's Guide for UEFI 2.3.1 content
- Intel SSG engineers contributed
- Located on [tianocore.org](https://www.tianocore.org/)

---
## Slide 5 Installing Python for UEFI Driver Wizard

### Requirements and Options

- Work space must contain BaseTools, MdePkg & MdeModulePkg Packages from [UDK2017](https://github.com/tianocore/tianocore.github.io/wiki/Driver-Developer) for Driver development on tianocore.org
- Uses previous lab's setup `$HOME/src/edk2-ws`
- Python* scripts from [GitHub Link](https://github.com/tianocore/edk2-share/tree/master/DriverDeveloper/UefiDriverWizard) then use instructions from README for Python and wxPython versions to install then run

```bash
$ python launch.py
```

---
## Slide 6 Requirements for Your Driver
### Using UEFI Driver Wizard

- UEFI Device Driver
- UEFI Version 2.7 (0x00020046)

```
#define EFI_2_70_SYSTEM_TABLE_REVISION ((2<<16)|(70DEC))
```

- Unloadable driver
- Support IA32 x64 CPUs
- Returns component name information
- Byte stream device (i.e. UART / Serial I/O)
- Option to produce HII strings & forms for setup


---
## Slide 7 Template File Contents

Proper UEFI driver entry point

Basic driver libraries/headers

Skeletons for common driver functions

Error values until ported `EFI_UNSUPPORTED`, `EFI_DEVICE_ERROR`

---
## Slide 8 Lab 1: Create a UEFI Driver with the UEFI Driver Wizard

- In this lab, you'll create a new UEFI driver using the UEFI Driver Wizard.
- This will create a set of "c" code files to be used as a template UEFI Driver used in the subsequent driver labs

---
## Slide 9 Lab 1: Install UEFI Driver Wizard, Python & wxPython

1. Perform [Lab Setup - tentative link](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_L_C_01_Platform_Build_OVMF-QEMU_Lab_guide.md) from previous labs
2. From the `~/FW/DriverWizard` folder, copy and paste folder `~/FW/DriverWizard/UefiDriverWizard` to `~$HOME`
3. Check if version 2.7.x is the default of Python from Terminal Prompt

```bash
$ python -V
Python 2.7.12
```

4. Install the wxPython (Version 3.0)

```bash
$ sudo apt-get install python-wxgtk3.0
```

---
## Slide 10 Lab 1: UefiDriverWizard - Select Work Space

Terminal Prompt (Ctrl-Alt-T)

```bash
$ cd ~/UefiDriverWizard
$ python launch.py
```

Select a Work Space

Control+O - to browse for a directory

Browse to `~/src/edk2-ws/edk2`

Select **Open**

(See screenshots in PDF file for more details)

---
## Slide 11 Lab 1: Create a New UEFI Driver

Control+N - to Open Menu

(See screenshots in PDF file for more details)


---
## Slide 12 Lab 1: New UEFI Driver Menu

- UEFI Driver Path - Type: "`MyWizardDriver`"	
	
	**Note:** "UEFI Driver Name" is filled in.

- **Ensure** all the forms, radio buttons, and boxes are filled in and selected *exactly* like the image to the right. (See PDF for screenshot)
- **Note:** A new, specific driver GUID will populate, so it will be different than this image

Click **Next**


---
## Slide 13 Lab 1: UEFI Driver Model Optional Features

**Ensure** all the forms, radio buttons and boxes are filled in and selected *exactly* like the image to the right. (See PDF for screenshot)

- "Component Name 2 Protocol"
- "Component Name Protocol"
- "HII Packages for forms..."

Click **Next**

---
## Slide 14 Lab 1: UEFI Driver Consumed Protocol

**Select**

- "USB Driver that consumes the USB I/O Protocol"

(See screenshot in PDF for clarification)

Click **Next**

---
## Slide 15 Lab 1: UEFI Driver Produced Protocols

**Select**

- "Byte stream device (i.e. UART) producing Serial I/O Protocol"

(See screenshot in PDF for clarification)

Click **Finish**

---
## Slide 16 Lab 1: UEFI Driver Created

UEFI Driver template created

(See PDF for screenshots)

Note: The new driver will be created in the following folder:

```
/home/u-uefi/fw/edk2-ws/edk2/MyPkg/MyWizardDriver
```

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

Copyright (c) 2021, Intel Corporation. All rights reserved
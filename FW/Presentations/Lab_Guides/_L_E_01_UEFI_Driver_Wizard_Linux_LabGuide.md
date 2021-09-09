
## slide 01 [UEFI_Driver_Wizard_Linux_Lab]
<br><br><br><br><br>
### UEFI & EDK II Training

#### UEFI Driver Wizard Lab - Linux
<br>
<span style="font-size:0.75em" ><a href='http://www.tianocore.org'>tianocore.org</a></span>
<!---
LabGuide.md for UEFI / EDK II Training  UEFI Driver Wizard Linux Lab

  Copyright (c) 2021, Intel Corporation. All rights reserved.<BR>

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
## slide 02 [Lesson Objective]
<BR>
### <p align="center"<span class="gold"   >Lesson Objective </span></p><br>

- Linux Ubuntu 16.04 is only supported
  -	Python Version 2.7 and 
  -	python-wxgtk V3.0


- Non-Ubuntu - Continue to <a href="https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_L_E_02_UEFI_Driver_Port_Linux_LabGuide.md">Porting UEFI Driver Lab</a>

#### Ubuntu 16.04 

- Setup the UEFI Driver Wizard
- Create a UEFI Driver Template


## slide 03 [UEFI Driver Wizard Section ]
<br>
### UEFI Driver Wizard
- Creating a Template UEFI Driver with the 
- UEFI Driver Wizard



## slide 04 [UEFI Driver Wizard  Overview]
<br>
### UEFI Driver Wizard  Overview
<br>
- Open source tool
- Based on Driver Writer’s Guide for UEFI 2.3.1 content
- Intel  engineers contributed
- Located on www.TianoCore.org 


Note:

## slide 05 [Installing UEFI Driver Wizard]
### Installing UEFI Driver Wizard

### Requirements and Options

- Workspace  must contain BaseTools, MdePkg & MdeModulePkg Packages from tianocore.org edk2 for Driver development on Tianocore.org 

- Uses previous lab’s setup w/ Windows $HOME/src/edk2-ws

- Python* scripts from <a href="https://github.com/tianocore/edk2-share/tree/master/DriverDeveloper/UefiDriverWizard"> Github Link </a> then use instructions from README for Python and wxPython versions to install then run
```
	 bash$ python launch.py
		
```
Note:

Same as slide


## slide 06 [Requirements for Your Driver ]

### Requirements for Your Driver 
- Using UEFI Driver Wizard
- UEFI Device Driver
- UEFI Version 2.7 (0x00020046)  
	#define EFI_2_70_SYSTEM_TABLE_REVISION ((2<<16) | (70DEC)) 
- Unloadable driver
- Support IA32 & x64 CPUs
- Returns component name information
- Byte stream device (i.e.UART / Serial I/O)
- Option to produce HII strings & forms for setup


Note:



---
## slide 07 [Template File Contents]
<p align="right"><span class="gold" ><b>Template File Contents </b></span></p>

- Proper UEFI driver entry point 
- Basic driver libraries/headers
- Skeletons for common driver functions



- Error values until ported EFI_UNSUPPORTED, EFI_DEVICE_ERROR<br>&nbsp;</span></p>)




Note:
- Establishes a proper UEFI Driver Entry Point
- References to basic driver libraries/headers based on Driver Wizard form input
- Inserted in .INF, .H and .C files
- Skeletons for common driver functions
- Includes comments based on information from the Driver Writer’s Guide for UEFI 2.3.1
- Functions may return error values until ported : (EFI_UNSUPPORTED, EFI_DEVICE_ERROR)



## slide 08 [Lab 1: Create a UEFI Driver section]


### Lab 1: Create a UEFI Driver with the UEFI Driver Wizard

- In this lab, you'll create a new UEFI driver using the UEFI Driver Wizard.
- This will create a set of "c" code files to be used as a template UEFI Driver used in the subsequent driver labs


 
Note:


## slide 09 [Lab 1: Install UEFI Driver Wizard ]

### Lab 1: Install UEFI Driver Wizard

1. First setup for Building EDK II for Emulator, See <a href="https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_L_C_01_Platform_Build_OVMF-QEMU_Lab_guide.md"> 
Lab Setup </a>
2. From the ~FW/DriverWizard folder, copy and paste folder "~FW/DriverWizard/UefiDriverWizard" to ~$Home 
3. Check  if version 2.7.x is the default of Python from Terminal Prompt 
```
  bash$ python –V
  Python 2.7.12
```
4. Install the wxPython  (Version 3.0 )
```
 bash$ sudo apt-get install python-wxgtk3.0 
```

## slide 10 [Lab 1: UefiDriverWizard -Select Work Space]

### Lab 1: UefiDriverWizard -Select Work Space

- Terminal Prompt (Cnt-Alt-T)
```
bash$ cd ~UefiDriverWizard
bash$ python launch.py
```
- Click on File and Select 
```
"Open WORKSPACE"
   Or 
Control+O  
```
- Browse to ~src/edk2-ws/edk2 
- Select  "OK"
- Should say 
```
"WORKSPACE /home/. . ./src/edk2-ws/edk2 selected"
```
- Select Open

Note: the environment for EDK II must be setup with edksetup.bat 


## slide 11 [Lab 1: -Create a New UEFI Driver]

### Lab 1: -Create a New UEFI Driver

- Control+N – to Open Menu 

- select "New UEFI Driver"

---
## slide 12 [Lab 1: New UEFI Driver Menu]


### Lab 1: New UEFI Driver Menu

- UEFI Driver Path" – Type: "MyWizardDriver"
      Note:  "UEFI Driver Name" is filled in.


- "UEFI Driver Version" – Type: "1.0"
- "UEFI Driver Type" – Select: "UEFI Driver Model Device Driver"
- "Optional Features …" – Select:
  - 	Unloadable
  - 	Driver Supporte EFI Version Protocol
  - 	HII Packages for Strings, Fonts, or images
- "CPU Architecture" – Select: "IA32" and "X64"
 Note:  A new, specific driver GUID will populate, so it will be different than this image

- Click Next
---


## slide 13 [Lab 1: UEFI Driver Model Optional Features]

### Lab 1: UEFI Driver Model Optional Features

- UEFI Driver Path" – Type: "MyWizardDriver"
      Note:  "UEFI Driver Name" is filled in.

- UEFI Driver Model Optional Features  – Select:
  -	Componnt Name 2 Prorocol
  -	Componnt Name Prorocol
  -	HII Packages for Forms and HII based configuruation


- Click Next


## slide 14 [Lab 1: UEFI Driver Consumed Protocol]
<br>
### Lab 1: UEFI Driver Consumed Protocol

- Select  "PCI Driver that consumes the PCI I/O Protocol"

- Click Next
Note:

Same as slide





## slide 15 [Lab 1: UEFI Driver Produced Protocols]

### Lab 1: UEFI Driver Produced Protocols

- Select  "Byte stream device (i.e.UART)producing Serial I/O Protocol"

- Click Finish
Note:

Same as slide



## slide 16 [Lab 1: UEFI Driver Created]
<br>
### Lab 1: UEFI Driver Created
- UEFI Driver template created

Several files should have been  created in the .. edk2/MyWizardDriver directory

- click OK

- Close the wizard

Note:

Same as slide



---  
## slide 17 [Summary]
<BR>
### Summary

- Setup the UEFI Driver Wizard
- Create a UEFI Driver Template

--
## Slide 18 [Questions]
<br>


---
## Slide 19 [Return to Training schedule main page]
<br>

Return to Training schedule main page
https://github.com/tianocore-training/Tianocore_Training_Contents/wiki 

---

## Slide 20 [Logo Slide]
<br><br><br>



---
## Slide 21 [Acknowledgements]

```
Redistribution and use in source (original document form) and 'compiled‘ forms (converted to PDF, epub, HTML and other formats) with or without modification, are permitted provided that the following conditions are met:
Redistributions of source code (original document form) must retain the above copyright notice, this list of conditions and the following disclaimer as the first lines of this file unmodified.
Redistributions in compiled form (transformed to other DTDs, converted to PDF, epub, HTML and other formats) must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.
THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL TIANOCORE PROJECT BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,  BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
Copyright (c) 2021, Intel Corporation. All rights reserved.
```




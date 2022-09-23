# Platform Build Lab Simics® Quick Start Platform (QSP)  
# Windows (Part 2)

---

## Slide 1  Platform Build Lab Simics® Quick Start Platform (QSP)

## UEFI & EDK II Training

## Platform Build Lab Simics® Quick Start Platform (QSP)  

 
[tianocore.org](http://www.tianocore.org)



<!---
 Lab_Guide.md for  Platform Build Lab Simics QSP Lab Windows

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
## Slide 2  Lesson Objective

### Platform Build Labs 

First Setup for Building EDK II, See [Lab Setup](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_C_01_Build_Setup_Download_EDK_II_Win_LabGuide.md)


- Build and Run the EmulatorPkg
- Build a EDK II Platform using Simics Open Source QSP Board
- Run Simics with the QSP Board



---
## Slide 3 Build the EmulatorPkg


### **Build the EmulatorPkg**

Note: May need to update conf/target.txt for other labs

---
## Slide 4  Build EDK II EmulatorPkg
 
Open VS Command prompt & Cd to work space directory 
```bash
$> cd C:\FW\edk2-ws
```
Setup the local environment: (see batch file setenv.bat )
```bash
$> setenv.bat
```
OR
```bash
$> set WORKSPACE=%CD%
$> set PACKAGES_PATH=%WORKSPACE%\edk2;%WORKSPACE%\edk2-libc
```
Invoke Edksetup.bat from directory C:/FW/edk2-ws/edk2 to Build BaseTools 
```bash
$> cd edk2
$> edksetup.bat 
```



### Build EmulatorPkg
```bash
$> build -D ADD_SHELL_STRING -a X64 -p EmulatorPkg\EmulatorPkg.dsc
```
---
## Slide 5 Build EDK II
### Build EDK II
- Inside the VS Prompt

build will take approximately 6 Minutes

Build will end at VS Prompt

---
## Slide 6 Run the Emulator
### Run the Emulator

---
## Slide 7 Invoke Emulation
### Invoke Emulation



From the command prompt

```bash
$> RunEmulator.bat
```

Or 
run WinHost.exe  from:
```bash
  C:\FW\edk2-ws\Build\EmulatorPkg\ . . .\X64 directory
```

Notice 2  "GOP Window n" opened 


---
## Slide 8 Emulator at Shell Prompt
### Emulator at Shell Prompt

Type : "Reset" to exit


---
## Slide 9 Build Simics QSP Open board
### Build Simics QSP Open board

---
## Slide 10 Build Simics QSP Open board
### Build Simics QSP Open board

The Simics QSP is part of the MinPlatformPkg

How to Download  & Build: Open Source MinPlatform [Readme.md](https://github.com/tianocore/edk2-platforms/blob/master/Platform/Intel/Readme.md)





Note: done in Lab 1

---
## Slide 11 MinPlatform Open Board Tree Structure
### MinPlatform Open Board Tree Structure

Below the Build script is invoked form the `edk2-platforms/Platform/Intel/`

The Platform DSC & FDF file are in `edk2-platforms/Platform/Intel/SimicsOpenBoardPkg/BoardX58Ich10`

```
edk2/ https://github.com/tianocore/edk2
 . . .
edk2-platforms/ https://github.com/tianocore/edk2-platforms 
  Platform/
      Intel/                     # Invoke the Build .py from here
		  BoardModulePkg /
		  SimicsOpenBoardPkg/
   			 BoardX58Ich10/      # Platform DSC & FDF here
		  MinPlatformPkg/
  Silicon/
	  Intel/
		  SimicsIch10Pkg/
		  SimicsX58ktPkg/
. . .
 Features/Intel
		    AdvancedFeaturePkg/
edk2-non-osi/ https://github.com/tianocore/edk2-non-osi
   Silicon/
	  Intel/
		   SimicsIch10BinPkg/
FSP/  https://github.com/IntelFsp/FSP

```

---
## Slide 12 Open a VS Command Prompt
### Open a VS Command Prompt

1. Right Click on the Task tray VS Command prompt Icon to Open another Visual Studio Command Prompt 
2. Left Click on "Developer . . ."       (this opens another window)
3. Then CD to:
```bash
> cd C:\fw\edk2-ws\edk2-platforms\Platform\Intel
```

---
## Slide 13 Build Environment
### Build Environment

Check if Python okay  (may also need to set PYTHON_HOME)
```bash
$>  python --version
Python 3.8.8
```
Check for available MinPlatform Boards
```bash
$> python build_bios.py -l
Platforms:
    BoardMtOlympus
    BoardX58Ich10
    AspireVn7Dash572G
    GalagoPro3
    KabylakeRvp3
    UpXtreme
    WhiskeylakeURvp
    CometlakeURvp
    TigerlakeURvp
    CooperCityRvp
    WilsonCityRvp
    BoardTiogaPass
    JunctionCity
    Aowanda
```



---
## Slide 14 Invoke the Build
### Invoke the Build

Invoke the Python Build script for Simics QSP
```bash
$> python build_bios.py -p BoardX58Ich10  -t VS20XX
```
- Where XX is `15x86` or `17` or `19`

Takes about 8 minutes

Rebuild takes about 2 minutes



**NOTE:** May require `PYTHON_HOME` to be set:
```bash
$ Set PYTHON_HOME=%USERPROFILE%\AppData\Local\Programs\Python\Python38-32
```
Where `38-32` is the version of Python you have installed


---
## Slide 15 Examine Build Parameters

```bash
Python build_bios.py -p BoardX58Ich10 -t VS2015x86

. . .
Calling build -n 0 --log=Build.log --report-file=BuildReport.log and from \edk2-ws\conf\target.txt
```

| PARAMETER | VALUE | PURPOSE | 
| --- | --- | --- |
| TARGET | = DEBUG | Build Mode |
| TARGET_ARCH | = IA32 X64 | CPU Architecture |
| TOOL_CHAIN_TAG | = VS2015x86 | VS Tool Chain |
| ACTIVE_PLATFORM | = ... /SimicsOpenBoardPkg/BoardX58Ich10/OpenBoardPkg.dsc | Platform DSC file
| Report file created (via python script) | = BuildReport.log | PCDs, Libs, etc. |


---
## Slide 16 Build EDK II
### Inside VS Prompt

Finished Build (see image on PDF)
Similar to: 
```bash
 C:\FW\edk2-ws\Build\SimicsOpenBoardPkg\BoardX58Ich10\DEBUG_VS2015x86\FV\BOARDX58ICH10.fd
```
Note the location of the final .fd file

---
## Slide 17 Invoke QSP with BOARDX58ICH10
### Invoke QSP with BOARDX58ICH10
---
## Slide 18 Open Simics Environment
### Open Simics Environment

Open the "Intel Simics Package Manager"
1.  Using the Start menu in Windows , Left Click on "Windows Key" Lower Left
2. Scroll down from the scroll bar on the right until "Intel Simics Package Manager"
3. Select "My Projects"

See PDF for a snapshot of Intel Simics Package Manager with "My Simics Project 1" created

Alternatively, Open a Windows CMD prompt and Cd to simics-projects\my-simics-project-1

---
## Slide 19 Copy BoardX85Ich10.fd to Simics
### Copy BoardX85Ich10.fd to Simics

```bash
Copy  C:\fw\edk2-ws\Build\SimicsOpenBoardPkg\BoardX58Ich10\DEBUG_VS20XX\FV\BOARDX58ICH10.fd
```
*To*
```bash
%USERPROFILE%\AppData\Local\Programs\Simics\simics-qsp-x86-6.0.57\targets\qsp-x86\images
```
Where XX is `15x86` or `17` or `19`


---
## Slide 20 Update the Simics Script
### Update the Simics Script

Update the Simics Script to Use the BoardX85Ich10.fd image just built

Edit the file:
```bash
%USERPROFILE%\AppData\Local\Programs\Simics\simics-qsp-x86-6.0.57\targets\qsp-x86\qsp-uefi.include
```

Replace `SIMICSX58IA32X64_1_0_0_bp_r.fd` with `BOARDX58ICH10.fd`

File: gsp-uefi.include

```bash
decl {
	params from "gsp-images.include"
	default bios image =
		%simics%/targets/qsp-x86/images/BOARDX58ICH10.fd
#		%simics%/targets/qsp-x86/images/SIMICSX58IA32X64_1_0_0_bp_r.fd
	default enable_efi = TRUE
}
```
Save qsp-uefi.include

---
## Slide 21 Invoke Simics QSP Script 
### Invoke Simics QSP Script 

Open a Simics Command Prompt: double click on `>_`

Invoke the qsp-modern-core script:

```bash
$> .\simics targets/qsp-x86/qsp-modern-core.simics
```



---
## Slide 22 Simics Windows

After invoking the First Steps script, **many** Simics windows will have opened:

**Simics Control Console**
- Stop
- Run

**Simics Command Prompt**
- Issue Simics Commands
	- Stop
	- Quit
	- Run
	- etc.

**Target Graphics Console**
- Video from Target

**Target COM1 Console**
- Debug Output from Target

Simics Getting Started: [Link](https://www.intel.com/content/www/us/en/developer/articles/guide/simics-simulator-get-started.html)

(see PDF for Simic windows examples)
---
## Slide 23 Run Simics to Boot Target

1. Next type "run" in the Simics command line
2. Be ready to press "F2" in the Target Graphics Console when tianocore logo is displayed

---

## Slide 24 QSP Setup - Boot to UEFI Shell

From QSP Setup

1. Click on "Boot Manager"
2. Click on "EFI Internal Shell"

Boots to UEFI Shell


---
## Slide 25 Exit QSP UEFI Shell & Simics

- To Stop the QSP Simulation, from the Simics Command Line Prompt winow type: "`stop`"
	- This will stop the Simics simulation of the QSP board
	- To continue, type "`run`"
- To exit this simulation, type "`quit`"
	- This will remove all other Simics windows
- To restart, reissue the command:

```bash
$ .\simics targets/qsp-x86/qsp-modern-core.simics
```

---
## Slide 26 Summary
Build and Run the EmulatorPkg
Build a EDK II Platform using Simics Open Source QSP Board
Run Simics with the QSP Board

---
## Slide 27 Questions?

<br>

---
## Slide 28 Return to Main Training Page

Return to training table of contents for next presentation: [link](https://github.com/tianocore-training/Tianocore_Training_Contents/wiki)


---
## Slide 29 Logo Slide

<br>

---
## Slide 30 Acknowledgements

Redistribution and use in source (original document form) and 'compiled' forms (converted to PDF, epub, HTML and other formats) with or without modification, are permitted provided that the following conditions are met:

Redistributions of source code (original document form) must retain the above copyright notice, this list of conditions and the following disclaimer as the first lines of this file unmodified.
Redistributions in compiled form (transformed to other DTDs, converted to PDF, epub, HTML and other formats) must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL TIANOCORE PROJECT BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,  BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.


Copyright (c) 2021-2022, Intel Corporation. All rights reserved.

---
## Slide 31 Backup


---  
## Slide 32  Build Errors
<br>
<br>
<br>
<br>

### Build Errors


---
## Slide 33  Build Error- RC.exe 

<b>Build Error- RC.exe </b>

Because RC.Exe is not found, Error Message:

```bash
   "c:\Program Files (x86)\Windows Kits\8.0\bin\x64\rc.exe" 
/Foc:\edkii.svn\Build\NT32IA32\DEBUG_VS2013x86\IA32\MdeModulePkg\Application\HelloWorld\HelloWorld\OUTPUT
\HelloWorldhii.lib 
c:\edkii.svn\Build\NT32IA32\DEBUG_VS2013x86\IA32\MdeModulePkg\Application\HelloWorld\HelloWorld\OUTPUT\He
lloWorldhii.rc
'c:\Program' is not recognized as an internal or external command,
operable program or batch file.
 
NMAKE : fatal error U1077: '"c:\Program Files (x86)\Windows Kits\8.0\bin\x64\rc.exe' : return code '0x1'
Stop.

```

Find where the RC.EXE is located on your VS Installation:  


Example (VS 2013):  The RC.exe is located on this machine: <br>
```bash
C:\Program Files (x86)\Windows Kits\8.1\bin\x64
```

Edit `Conf/tools_def.txt` 



---
## Slide 34  Build Error- RC.exe 02
<b>Build Error- RC.exe Cont...</b>

Edit `Conf/tools_def.txt` 
Search for your installation of Visual Studio (2013 or 2015)<br>
Update according to the path for where the RC.EXE is found 

```bash
# Microsoft Visual Studio 2013 Professional Edition
DEFINE WINSDK8_BIN       = c:\Program Files\Windows Kits\8.1\bin\x86\
DEFINE WINSDK8x86_BIN    = c:\Program Files (x86)\Windows Kits\8.1\bin\x64
 
# Microsoft Visual Studio 2015 Professional Edition
DEFINE WINSDK81_BIN       = c:\Program Files\Windows Kits\8.1\bin\x86\
DEFINE WINSDK81x86_BIN    = c:\Program Files (x86)\Windows Kits\8.1\bin\x64


```

### RC FIX for VS 2017 at command prompt type 

```bash
> Set
```
Then  See what the value of WINSDK10_PREFIX is.  This is probably the problem. 
Search your directory "C:\Program Files (x86)\" for the file rc.exe
The one you want will be in  C:\Program Files (x86)\Windows Kits\10\bin\x86
 
 
see below for the define: 
```bash 
  From : DEFINE WINSDK10_BIN       = ENV(WINSDK10_PREFIX)DEF(VS2017_HOST)
  to : DEFINE WINSDK10_BIN       = C:\Program Files (x86)\Windows Kits\10\bin\x86
```

---
## Slide 35  Build Error- C1041 
<b>Build Error: fatal error C1041: </b>
Build Error from fatal error C1041: cannot open program database

This Error is usually because the location you are building is being shared by another application in Windows.  Example: Syncplicity may cause this
<br>

Error Message:

```bash
k:\fw\edk2\MdePkg\Library\BaseLib\LinkedList.c : fatal error C1041: cannot open program 
database 
'k:\fw\edk2\build\nt32ia32\debug_vs2013x86\ia32\mdepkg\library\baselib\baselib\vc120.pdb'; if 
multiple CL.EXE write to the same .PDB file, please use /FS
NMAKE : fatal error U1077: '"C:\Program Files (x86)\Microsoft Visual Studio 
12.0\Vc\bin\cl.exe"' : return code '0x2'
Stop.

```

<b>Solution:</b>  Try using a Workspace that is not shared



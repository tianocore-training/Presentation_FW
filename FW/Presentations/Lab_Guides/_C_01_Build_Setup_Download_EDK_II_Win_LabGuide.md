# Build Setup and Download EDK II Lab Guide
# Windows (Part 1)

---
## Slide 1  Build Setup and Download EDK II Lab

## UEFI & EDK II Training

 Platform Build Windows Emulator Lab – Windows

[tianocore.org](http://www.tianocore.org)


<!---
 Lab_Guide.md for Build Setup and Download EDK II  Windows

  Copyright (c) 2020-2022, Intel Corporation. All rights reserved –

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

### Build Setup Labs
   
- Pin Visual Studio Command Prompt to Windows Task Bar 
- Pre-requisite Setup / Install Build Application Tools
- Download EDK II Source
- Build BaseTools



---
## Slide 3  Pin VS CMD Prompt Section

### Pin VS Command Prompt 
 
Pin the Visual Studio Command prompt to WindowsTask Bar


---
## Slide 4  Pin VS CMD Prompt
### Pin  VS Command Prompt

Windows 10 

Steps to Pin Visual Studio Command Prompt to task bar for Windows 10
1. Using the Start menu in Windows 10, Left Click on "Windows Key" Lower Left
2. Scroll down from the scroll bar on the right until "Visual Studio 201n"
3. Left Click "Visual Studio 201n"


---
## Slide 5  Pin VS CMD Prompt 02
### Pin  VS Command Prompt


4. Select "Developer Command Prompt for VS201n"
5. Right Click to open Windows dialog box
   - Do not use any of the other "... Command Prompts"


---
## Slide 6  Pin VS CMD Prompt 03
### Pin  VS Command Prompt


6. Left Click on "Pin to taskbar"



---
## Slide 7  Pin VS CMD Prompt 04
### Pin  VS Command Prompt

7. Open VS Command Prompt

All Windows Labs use this short-cut to Build Edk II platforms and projects using Windows Visual Studio: 2015/2017 or 2019


---
## Slide 8  Prerequisite Download / Install Tools   
<br>

---
## Slide 9  Prerequisites
### Done Before Class

- Windows 10:
   - Continuous Integration (CI) - Stuart CI Build with  Visual Studio VS2017 or VS2019
   - Non Stuart CI - Visual Studio VS2015, VS2017 or VS2019
- Python 3.8.x or greater and /Scripts directories on Path: [Link](https://www.python.org/) to download 
- Git for Windows on Path: [Link](http://git-scm.com/download/win)
- NASM  2.15.x or greater for Win64: [Link](https://www.nasm.us/pub/nasm/releasebuilds/2.15.05/win64/)
- IASL Compiler Install iasl from [Link](https://acpica.org/node/199) (iasl-win-20220331.zip)


---
## Slide 10 Setup Simics Environment

Download and Install the Simics Simulator (both Package Manager & Simics-6 Packages) [Simics® Simulator Public Release](https://www.intel.com/content/www/us/en/developer/articles/tool/simics-simulator.html)

Setup the Simics Simulator

[Simics® Simulator Installation and Get Started](https://www.intel.com/content/www/us/en/developer/articles/guide/simics-simulator-installation.html)

Open the "Intel Simics Package Manager"

1. Using the Start menu in Windows, Left Click on "Windows Key" Lower Left
2. Scroll down from the scroll bar on the right until "Intel Simics Package Manager"

Here is a snapshot of Intel Simics Package Manager with "My Simics Project 1" created (see PDF for picture)


---
## Slide 11  Download EDK II Open Source Repos

Use Git Bash to download EDK II


---
## Slide 12  Create Workspace Directory

Open a Windows Command Prompt for VS
Make new directory for Workspace:

```bash
$ cd /
$ Mkdir FW
$ cd FW
$ Mkdir edk2-ws
$ cd edk2-ws
```

---
## Slide 13 Download the EDK II Source Code
### Download the open source EDK II from GitHub

Note:  if behind a firewall, set PROXYS FIRST (example shows for Intel Corp. -- Maybe different for your Corp. or GEO)
```bash
$ git config --global https.proxy proxy-dmz.intel.com:912
$ git config --global http.proxy proxy-dmz.intel.com:911
```

From the command prompt use "git clone" to download following repos
- Edk2 - main core code
```bash
$ git clone -b Edk2Lab_22Q3 https://github.com/tianocore-training/edk2.git
```
- EDK II "C" Library Repo
```bash
$ git clone https://github.com/tianocore/edk2-libc.git
```
- EDK II Platforms Repo
```bash
$ git clone https://github.com/tianocore/edk2-platforms.git
```
- EDK II Non-OSI (Stand alone Binaries)
```bash
$ git clone https://github.com/tianocore/edk2-non-osi.git
```
- Intel FSP
```bash
$ git clone https://github.com/intel/FSP.git
```

---
## Slide 14 Download the EDK II Source Code
### Download the open source EDK II from GitHub

Download the Submodules and Checkout the Lab Branch
```bash
$ C:\fw\edk2-ws> cd edk2
$ C:\fw\edk2-ws\edk2> git submodule update --init
$ C:\fw\edk2-ws> cd ..
```

Download Checkout the Sha tag for edk2-platforms repo
```bash
$ C:\fw\edk2-ws> cd edk2-platforms
$ C:\fw\edk2-ws\edk2-platforms> git reset --hard c546cc01f1517b42470f3ae44d67efcb8ee257fc
$ C:\fw\edk2-ws\edk2-platforms> cd ..
```

(reset to this commit since this is used with all the labs)

---
## Slide 15  Download Lab_Material_FW - getting the Source 
Download the Lab_Material_FW.zip from: [github.com Lab_Matrial_FW.zip](https://github.com/tianocore-training/Lab_Material_FW/archive/refs/heads/main.zip)

OR

Use `git clone` to download the Lab_Material_FW
```bash
C:\> cd C:\
C:\> git clone https://github.com/tianocore-training/Lab_Material_FW.git
```
Directory Lab_Material_FW will be created

```
   FW 
    - Documentation 
   - DriverWizard 
   - edk2-ws      
   - edk2Linux 
   - LabSampleCode 
   - Nasm
```

---
## Slide 16 Build EDK II
### Extract the Lab Material

Extract the Downloaded Lab_Material_FW-main.zip to C:\


Download the Lab_Material_FW.zip from [link](https://github.com/tianocore-training/Lab_Material_FW/archive/refs/heads/main.zip)


---
## Slide 17  Build  Edk II
### Copy edk2-ws


From the downloaded Lab_Material_FW folder **copy** and **paste** folder `"..Lab_Material_FW\FW\edk2-ws"` to `"C:\FW"`


*Note:* Overwrite existing files and directories


---
## Slide 18  Build  Edk II
### Get Nasm

Copy `Nasm` directory to `C:\`

(creating `C:\Nasm` directory)

*Note: If only Readme.txt exist in the Directory from the   Lab_Material_FW folder then follow Readme.txt instructions to download the Nasm Executable

---
## Slide 19  Build EDK II
### Get IASL ACPI Compiler

Copy `asl` directory to `C:\`

(creating `C:\asl` directory)

*Note: If only Readme.txt exist in the Directory from the   Lab_Material_FW folder then follow Readme.txt instructions to download the Iasl Executable.



---
## Slide 20 Build BaseTools

Note: will need to update conf/target.txt for other labs


---
## Slide 21 Build EDK II
### build BaseTools

Open VS Command prompt & cd to workspace directory
```bash
$> cd C:\fw\edk2-ws
```

Setup the local environment: (see batch file `setenv.bat`)
Sets WORKSPACE and PACKAGES_PATH env variables
```bash
$> setenv.bat
```

Invoke Edksetup.bat from directory C:/fw/edk2-ws/edk2 to Build BaseTools
```bash
$> cd edk2
$> edksetup.bat Rebuild
```

**Important NOTE:** only use "Rebuild" on the first time - there after just use "edkseup.bat"

Building BaseTools only needs to be done once but setting up local environment and edksetup.bat needs to be done each new VS prompt session

---
## Slide 22 Update Conf/Target.txt

**Invoke** Edksetup.bat

```bash
$> cd C:\fw\edk2-ws\edk2
$> edksetup.bat
```

**Edit** the file Conf/target.txt (**change** TOOL_CHAIN_TAG)
```bash
$> notepad Conf/target.txt
```

```
TARGET_ARCH        = X64
. . .
TOOL_CHAIN_TAG     = VS2015x86 // Change this depending on VS version
```

VS Version Guide

|VS version| TOOL_CHAIN_TAG |
| --- | --- |
| 2015 | VS2015x86 |
| 2017 | VS2017 |
| 2019 | VS2019 |

Save and exit

<br>

---
## Slide 23 Summary

- Pin Visual Studio Command Prompt to Windows Task Bar
- Pre-requisite Setup / Install Build Application Tools
- Download EDK II Source
- Build BaseTools

---
## Slide 24 Questions?   
<br>

---
## Slide 25 Return to Main Training Page

Return to the Training Table of contents for the next presentation: [link](https://github.com/tianocore-training/Tianocore_Training_Contents/wiki)


---
## Slide 26 Logo Slide
<br>

---
## Slide 27 Acknowledgements

Redistribution and use in source (original document form) and 'compiled‘ forms (converted to PDF, epub, HTML and other formats) with or without modification, are permitted provided that the following conditions are met:

Redistributions of source code (original document form) must retain the above copyright notice, this list of conditions and the following disclaimer as the first lines of this file unmodified.
Redistributions in compiled form (transformed to other DTDs, converted to PDF, epub, HTML and other formats) must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL TIANOCORE PROJECT BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,  BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.


Copyright (c) 2021-2022, Intel Corporation. All rights reserved.
# Build Setup and Download EDK II Lab Guide
# Linux (Part 2)

---
## Slide 1 UEFI & EDK II Training
### Platform Build Lab - Simics - Linux

[tianocore.org](https://www.tianocore.org/)

---
## Slide 2 Platform Build Labs
### First Setup for Building EDK II, See [TENTATIVE LINK](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_C_01_Platform_Build_Win_Emulator_Lab_guide.md#build-emulator)

- Build a EDK II Platform using OVMF package
- Run OVMF using Qemu
- Build a EDK II Platform using Simics Open Source QSP Board
- Run Simics with the QSP Board

---
## Slide 3 Build the OVMF Platform

<br>

---
## Slide 4 Build EDK II OVMF
### Update Target.txt

**What is OVMF?**

OpenV Virtual Machine Firmware - Build with edk2

```bash
$ cd ~/fw/edk-ws/edk2
. edksetup.sh
```

Edit the file Conf/target.txt

```bash
$ gedit Conf/target.txt
```

Write the following in target.txt
```
ACTIVE_PLATFORM			= OvmfPkg/OvmfPkgX64.dsc
#. . .
TARGET_ARCH				= X64
#. . .
TOOL_CHAIN_TAG			= GCC5
```
Save and build

```bash
$ build -D ADD_SHELL_STRING
```

More info: [tianocore-wiki/OVMF](https://github.com/tianocore/tianocore.github.io/wiki/OVMF)

--
## Slide 5 Build EDK II OVMF
### Inside Terminal

```bash
$ build -D ADD_SHELL_STRING -a X64
```

---
## Slide 6 Build EDK II OVMF
### Verify Build Succeeded

OVMF.fd should be in the Build directory
- For GCC5 with x64, it should be located at

```
~/fw/edk2-ws/Build/OvmfX64/DEBUG_GCC5/FV/OVMF.fd
```

---
## Slide 7 Invoke QEMU

Change to run-ovmf directory under the home directory

```bash
$ cd $HOME/run-ovmf
```

Copy the OVMF.fd BIOS image created from the build to the run-ovmf directory naming it bios.bin

```bash
$ cp ~/fw/edk2-ws/Build/OvmfX64/DEBUG_GCC5/FV/OVMF.fd bios.bin
```

Run the RunQemu.sh Linux shell script

```bash
$ . RunQemu.sh
```

Exit QEMU

---
## Slide 8 Build QSP Simics Open board

<br>

---
## Slide 9 Where to get Open Source Simics

How to Download & Build: Open Source MiniPlatform [Readme.md](https://github.com/tianocore/edk2-platforms/blob/master/Platform/Intel/Readme.md)



---
## Slide 10 MinPlatform Open Board Tree Structure

```
edk2/ https://github.com/tianocore/edk2
 . . .
edk2-platforms/ https://github.com/tianocore/edk2-platforms 
  Platform/
      Intel/				# Invoke the Build .py from here
		  BoardModulePkg 
		  SimicsOpenBoardPkg
   			 BoardX58Ich10           # Platform DSC & FDF here
		  MinPlatformPkg
  Silicon/
	  Intel/
		  SimicsIch10Pkg
		  SimicsX58ktPkg
. . .
 Features/Intel
		    AdvancedFeaturePkg
edk2-non-osi/ https://github.com/tianocore/edk2-non-osi
   Silicon/
	  Intel/
		   SimicsIch10BinPkg
FSP/  https://github.com/IntelFsp/FSP

```

---
## Slide 11 Open Another Terminal Prompt

1. Open another Terminal Prompt in $HOME/fw/edk2-ws
2. Then CD to edk2 to do edksetup.sh

```bash
$ cd ~/fw/edk2-ws/edk2
$ . edksetup.sh
```

3. Then CD to:

```bash
$ cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel
```

---
## Slide 12 Build Environment

Check if Python is okay (may also need to set PYTHON_HOME)

```bash
$ python --version
Python 3.8.2
```

Check for available MinPlatform Boards

```
$ python build_bios.py -l
```

---
## Slide 13 Invoke the build

Invoke the Python Build script for UP Xtreme (Takes about 8 minutes)

```
$ python build_bios.py -p BoardX58Ich10 -t GCC5
```

Rebuild takes about 2 minutes

---
## Slide 14 Examine Build Parameters

```
Python build_bios.py -p BoardX58Ich10 -t GCC5

. . .
Calling build -n 0 --log=Build.log --report-file=BuildReport.log and from \edk2-ws\conf\target.txt
```

| PARAMETER | VALUE | PURPOSE | 
| --- | --- | --- |
| TARGET | = DEBUG | Build Mode |
| TARGET_ARCH | = IA32 X64 | CPU Architecture |
| TOOL_CHAIN_TAG | = GCC5 | VS Tool Chain |
| ACTIVE_PLATFORM | = ... /SimicsOpenBoardPkg/BoardX58Ich10/OpenBoardPkg.dsc | Platform DSC file
| Report file created (via python script) | = BuildReport.log | PCDs, Libs, etc. |

---
## Slide 15 Build EDK II
### Inside VS Prompt

Finished Build (see image on ppt)

Note the location of the final .fd file

---
## Slide 16 Invoke QSP wit BOARDX58ICH10

<br>

---
## Slide 17 Open Intel Simics Project Manager

Open a terminal prompt in the Un-tar directory of the Simics e.g. intel-simics-package-manager-1.2.3-linux64/

```bash
$ ./ispm-gui
```

In the GUI select **My** Projects

---
## Slide 18 Copy BoardX85Ich10.fd to Simics

Copy \~/fw/edk2-ws/Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/FV/BOARDX58ICH10.fd

*To*

Simics-Install-Directory/simics-qsp-x86-6.0.57/targets/qsp-x86/images


---
## Slide 19 Update the Simics Script

Update the Simics Script to Use the BoardX85Ich10.fd image just built

Edit the file:

*<InstallDir>/simics-qsp-x86-6.0.57/targets/qsp-x86/qsp-uefi.include*

Where *InstallDir* is the directory selected to install simics, e.g., *usr/bin/simics*

Replace `SIMICSX58IA32X64_1_0_0_bp_r.fd` with `BOARDX58ICH10.fd`

File: gsp-uefi.include

```
decl {
	params from "gsp-images.include"
	default bios image =
		%simics%/targets/qsp-x86/images/BOARDX58ICH10.fd
#		%simics%/targets/qsp-x86/images/SIMICSX58IA32X64_1_0_0_bp_r.fd
	default enable_efi = TRUE
}
```

---
## Slide 20 Invoke Simics First Steps Script

Open a Simics Command Prompt: double click on `>_`

Invoke the firststeps script:

```bash
$> ./simics targets/qsp-x86/firststeps.simics
```

---
## Slide 21 Simics Windows

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


---
## Slide 22 Run Simics to Boot Target

1. Next type "run" in the Simics command line
2. Be ready to press "F2" in the Target Graphics Console when tianocore logo is displayed

---
## Slide 23 QSP Setup - Boot to UEFI Shell

From QSP Setup

1. Click on "Boot Manager"
2. Click on "EFI Internal Shell"

Boots to UEFI Shell


---
## Slide 24 Exit QSP UEFI Shell & Simics

- To Stop the QSP Simulation, from the Simics Command Line Prompt winow type: "`stop`"
	- This will stop the Simics simulation of the QSP board
	- To continue, type "`run`"
- To exit this simulation, type "`quit`"
	- This will remove all other Simics windows
- To restart, reissue the command:

```bash
$ ./simics targets/qsp-x86/firststeps.simics
```

---
## Slide 25 Summary

- Build EDK II Platform using OVMF package
- Run OVMF using Qemu
- Build a EDK II Platform using Simics Open Source QSP Board
- Run Simics with the QSP Board

---
## Slide 26 Questions?

<br>

---
## Slide 27 Return to Main Training Page

Return to training table of contents for next presentation: [link](https://github.com/tianocore-training/Tianocore_Training_Contents/wiki)


---
## Slide 28 Logo Slide

<br>

---
## Slide 29 Acknowledgements

Redistribution and use in source (original document form) and 'compiled‘ forms (converted to PDF, epub, HTML and other formats) with or without modification, are permitted provided that the following conditions are met:

Redistributions of source code (original document form) must retain the above copyright notice, this list of conditions and the following disclaimer as the first lines of this file unmodified.
Redistributions in compiled form (transformed to other DTDs, converted to PDF, epub, HTML and other formats) must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL TIANOCORE PROJECT BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,  BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.


Copyright (c) 2021-2022, Intel Corporation. All rights reserved.
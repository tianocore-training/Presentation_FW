# EDK II Debugging with Linux Lab


---
## Slide 1 UEFI & EDK II Training
### EDK II Debugging with Linux Lab

[tianocore.org](https://www.tianocore.org/)

---
## Slide 2 Lesson Objective

- Using PCDs to Configure `DebugLib` - LAB
- Change the `DebugLib` instance to modify the debug output - LAB
- Debug EDK II Boot Flow - LAB

---
## Slide 3 Catch up Lab

In this lab, you'll start where the previous Writing UEFI Applications left off.

---
## Slide 4 Lab 0: Catch up from previous lab (1)

Skip to next slide if Writing UEFI App Lab completed

- Perform Lab Setup from previous Labs - [Lab Guide](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_L_C_01_Build_Setup_Download_EDK_II_Linux_LabGuide.md)
- Copy contents of `../FW/LabSampleCode/SampleAppDebug/MyPkg` to `~/fw/edk2-ws/edk2/`
- Open `edk2-platforms/Platform/Intel/SimicsOpenBoardPkg/BoardX58Ich10/OpenBoardPkg.dsc` and add the following in the [Components...] section, Hint: add after comment:

```
# Add new modules here
MyPkg/SampleApp/SampleApp.inf
```

- Save and close the file `OpenBoard.dsc`

---
## Slide 5 Lab 0: Catch up from previous lab (2)

Copy the `UefiAppLab.vhd`

From: `.../Lab_Material_FW/FW/LabSampleCode/AppLab/UefiAppLab.vhd`

To `Simics-Install-Directory/simics-qsp-x86-6.0.57/targets/qsp-x86/images`


---
## Slide 6 Lab 0: Catch up from previous lab (3)

Update the Simics Script to Use the `UefiAppLab.vhd` image as a file system

Edit the file: qsp-modern-core.simics from `Simics-Install-Directory/simics-qsp-cpu-6.0.4/targets/qsp-x86/qsp-modern-core.simics`

Add the following Line:

```
$disk1_image="%simics%/targets/qsp-x86/images/UefiAppLab.vhd"
```

Before the `run-command-file` line

Save `qsp-modern-core.simics`

**File: qsp-modern-core.simics**

```
Decl{
decl {
! Script that runs the Quick Start Platform (QSP) with a modern 
!   processor core. 

params from "%simics%/targets/qsp-x86/qsp-clear-linux.simics"  
default cpu_comp_class = "x86QSP2"  
default num_cores = 2  
default num_threads = 2
} 
$disk1_image="%simics%/targets/qsp-x86/images/UefiAppLab.vhd“

run-command-file "%simics%/targets/qsp-x86/qsp-clear-linux.simics"
```

---
## Slide 7 Lab 1 - Adding Debug Statements

In this lab, you'll add debug statements to the previous lab's SampleApp UEFI Shell application


---
## Slide 8 Lab 1: Add debug statements to SampleApp

Mount the UefiAppLab.vhd using GuestMount: [How To Link](https://www.tianocore.org/)

- Open `~/fw/edk2-ws/edk2/MyPkg/SampleApp/SampleApp.c`
- Add the following to the include statements at the top of the file below the last "include" statement

```
#include <Library/DebugLib.h>
```


---
## Slide 9 Lab 1: Add debug statements to SampleApp

Locate the UefiMain function. Then copy and paste the following code after the `EFI_INPUT_KEY  KEY;` statement: and before the first `Print()` statement as shown in the code block below:

```
DEBUG ((0xffffffff, "/n/nUEFI Base Training DEBUG DEMO/n") );
DEBUG ((0xffffffff, "0xffffffff USING DEBUG ALL Mask Bits Set/n") );

DEBUG ((DEBUG_INIT,     " 0x%08x USING DEBUG DEBUG_INIT/n" , (UINTN)(DEBUG_INIT))  );
DEBUG ((DEBUG_WARN,     " 0x%08x USING DEBUG DEBUG_WARN/n", (UINTN)(DEBUG_WARN))  );
DEBUG ((DEBUG_LOAD,     " 0x%08x USING DEBUG DEBUG_LOAD/n", (UINTN)(DEBUG_LOAD))  );
DEBUG ((DEBUG_FS,       " 0x%08x USING DEBUG DEBUG_FS/n", (UINTN)(DEBUG_FS))  );
DEBUG ((DEBUG_POOL,     " 0x%08x USING DEBUG DEBUG_POOL/n", (UINTN)(DEBUG_POOL))  );
DEBUG ((DEBUG_PAGE,     " 0x%08x USING DEBUG DEBUG_PAGE/n", (UINTN)(DEBUG_PAGE))  );
DEBUG ((DEBUG_INFO,     " 0x%08x USING DEBUG DEBUG_INFO/n", (UINTN)(DEBUG_INFO))  );
DEBUG ((DEBUG_DISPATCH, " 0x%08x USING DEBUG DEBUG_DISPATCH/n",(UINTN)(DEBUG_DISPATCH)));
DEBUG ((DEBUG_VARIABLE, " 0x%08x USING DEBUG DEBUG_VARIABLE/n",(UINTN)(DEBUG_VARIABLE)));
DEBUG ((DEBUG_BM,       " 0x%08x USING DEBUG DEBUG_BM/n", (UINTN)(DEBUG_BM))  );
DEBUG ((DEBUG_BLKIO,    " 0x%08x USING DEBUG DEBUG_BLKIO/n", (UINTN)(DEBUG_BLKIO))  );
DEBUG ((DEBUG_NET,      " 0x%08x USING DEBUG DEBUG_NET/n", (UINTN)(DEBUG_NET))  );
DEBUG ((DEBUG_UNDI,     " 0x%08x USING DEBUG DEBUG_UNDI/n", (UINTN)(DEBUG_UNDI))  );
DEBUG ((DEBUG_LOADFILE, " 0x%08x USING DEBUG DEBUG_LOADFILE/n",(UINTN)(DEBUG_LOADFILE)));
DEBUG ((DEBUG_EVENT,    " 0x%08x USING DEBUG DEBUG_EVENT/n", (UINTN)(DEBUG_EVENT))  );
DEBUG ((DEBUG_GCD,      " 0x%08x USING DEBUG DEBUG_GCD/n", (UINTN)(DEBUG_EVENT)) );
DEBUG ((DEBUG_CACHE,    " 0x%08x USING DEBUG DEBUG_CACHE/n", (UINTN)(DEBUG_CACHE)) );
DEBUG ((DEBUG_VERBOSE,  " 0x%08x USING DEBUG DEBUG_VERBOSE/n", (UINTN)(DEBUG_VERBOSE)) );
DEBUG ((DEBUG_ERROR,    " 0x%08x USING DEBUG DEBUG_ERROR/n", (UINTN)(DEBUG_ERROR))  );
```

SAVE and CLOSE SampleApp.c

---
## Slide 10 Update UefiAppLab.vhd File

Build the Simics Board

At the Terminal Prompt, Build BoardX58Ich10

```bash
$  cd ~/fw/edk2-ws/edk2
$  . edksetup.sh
$  cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel/
$  python build_bios.py -p BoardX58Ich10  -t GCC5

```

Copy `SampleApp.efi` from the build directory to the `VHD Disk`

```bash
$ cp ~/fw/edk2-ws/edk2/Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/X64/SampleApp.efi ~/VHD
```

(See PDF for screenshot)


---
## Slide 11 Lab 1: Build, Run and Test Result

Open another Terminal Command Prompt

```bash
$ cd  simics-projects/my-simics-project-1
```

Run the firststeps script:

```
$ ./simics targets/qsp-x86/qsp-modern-core.simics
simics> run
```

(Press "F2" at the logo, then select "Boot Manager" followed by "EFI Internal Shell")

At the Shell prompt

```
Shell> Fs1:
FS1:/> SampleApp
```

See that the output from the Debug statements goes to the Simics Serial Console

(See PDF for screenshot)

Exit Simics

```
simics> Stop
simics> quit
```

---
## Slide 12 Lab 2 - Changing PCD Value

In this lab, you'll learn how to use PCD values to change debugging capabilities.

---
## Slide 13 Lab 2: Change PCDs for SampleApp

Open `~/fw/edk2-platforms/Platform/Intel/SimicsOpenBoardPkg/BoardX58Ich10/OpenBoardPkg.dsc`

Replace `SampleApp/SampleApp.inf` with the following;

```
SampleApp/SampleApp.inf {
   <PcdsFixedAtBuild>
    gEfiMdePkgTokenSpaceGuid.PcdDebugPropertyMask|0xff
    gEfiMdePkgTokenSpaceGuid.PcdDebugPrintErrorLevel|0xffffffff
 }
```

**Save** and **close** OpenBoardPkg.dsc

---
## Slide 14 Lab 2: Build and Test SampleApp

1. At the Terminal Command Prompt, Re-Build BoardX58Ich10

```bash
$ cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel/
$ python build_bios.py -p BoardX58Ich10  -t GCC5
```

2. Copy `SampleApp.efi` from the build directory to the `VHD Disk`

```bash
$ cp  ../Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/X64/SampleApp.efi ~/VHD
```

3. Run the firststeps script from Terminal Command Prompt:

```bash
$ ./simics targets/qsp-x86/qsp-modern-core.simics
simics> run   
```

4. At the UEFI Shell prompt

```
Shell> Fs1:
FS1:/> SampleApp.efi
```

See that the output from ALL the Debug statements goes to the Simics Serial Console

(See PDF for screenshot)

5. Exit Simics

```
simics> stop
simics> quit
```

---
## Slide 15 Lab 3 - Library Instances for Debugging

In this lab, you'll learn how to add specific debug library instances.

---
## Slide 16 Lab 3: Using Library Instances for Debugging

Open `~/fw/edk2-platforms/Platform/Intel/SimicsOpenBoardPkg/BoardX58Ich10/OpenBoardPkg.dsc`

Replace `SampleApp/SampleApp.inf { . . .}` with the following:

```
SampleApp/SampleApp.inf {
   <LibraryClasses>
    DebugLib|MdePkg/Library/UefiDebugLibConOut/UefiDebugLibConOut.inf
 }
```

**Save** and **close** OpenBoardPkg.dsc

---
## Slide 17 Lab 3: Build and Test SampleApp

1. At the Terminal Command Prompt, Re-Build BoardX58Ich10

```bash
$ cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel/
$ python build_bios.py -p BoardX58Ich10  -t GCC5
```

2. Copy `SampleApp.efi` from the build directory to the `VHD Disk` 

```
Copy ../Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/X64/SampleApp.efi UefiAppLab Disk
```

3. Run the `firststeps` script from Terminal Command Prompt:

```bash
$ ./simics targets/qsp-x86/qsp-modern-core.simics
simics> run
```

4. At the UEFI Shell prompt

```
Shell> Fs1:
FS1:/> SampleApp.efi
```

See that the output from the Debug statements now goes to the Simics VGA console

(See PDF for screenshot)

5. Exit Simics

```
simics> stop
simics> quit
```

---
## Slide 18 Lab 4: Null Instance of DebugLib

In this lab, you'll change the `DebugLib` to the Null instance.


---
## Slide 19 Lab 4: Using Null Library Instances

Open `~/fw/edk2-platforms/Platform/Intel/SimicsOpenBoardPkg/BoardX58Ich10/OpenBoardPkg.dsc`

Replace `SampleApp/SampleApp.inf { . . .}` with the following:

```
SampleApp/SampleApp.inf {
   <LibraryClasses>    	
   DebugLib|MdePkg/Library/BaseDebugLibNull/BaseDebugLibNull.inf 
}
```

**Save** and **close** `OpenBoardPkg.dsc`

---
## Slide 20 Lab 4: Build and Test SampleApp

1. At the Terminal Command Prompt, Re-Build BoardX58Ich10

```bash
$ cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel/
$ python build_bios.py -p BoardX58Ich10  -t GCC5
```

2. Copy `SampleApp.efi` from the build directory to the `VHD Disk`

```
Copy ../Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/X64/SampleApp.efi UefiAppLab Disk
```

3. Run the `firststeps` script from Terminal Command Prompt:

```bash
$ ./simics targets/qsp-x86/qsp-modern-core.simics
simics> run   
```

4. At the UEFI Shell prompt

```
Shell> Fs1:
FS1:/> SampleApp.efi
```

See that there is **NO** Debug output

(See PDF for screenshot)

5. Exit Simics

```
simics> stop
simics> quit
```

---
## Slide 21 Lab 5: Debugging EDK II add Debug to Boot Flow

In this lab, you'll learn how add Debug statements to the EDK II Boot flow and check the debug log output

---
## Slide 22 Lab 5: Debug Boot Flow

Edit the `MdeModulePkg/Core/Pei/PeiMain/PeiMain.c` and add a "DEBUG" print \~line 489 before the call to the PeiDispatcher:

```
DEBUG((DEBUG_INFO, "***** ***** *****Before call to Pei Dispatcher ***** ***** *****/n"));
```

Save PeiMain.c

```
// Call PEIM dispatcher
//
DEBUG((DEBUG_INFO, "***** ***** *****Before call to Pei Dispatcher ***** ***** *****\n"));
PeiDispatcher (SecCoreData, &PrivateData);
```

---
## Slide 23 Lab 5: Build and Test SampleApp

1. At the Terminal Command Prompt, Re-Build BoardX58Ich10

```bash
$ cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel/
$ python build_bios.py -p BoardX58Ich10  -t GCC5
```

2. Copy the Simics QSP Board .FD file `~/fw/edk2-ws/Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/FV/BOARDX58ICH10.fd` To `%USERPROFILE%/AppData/Local/Programs/Simics/simics-qsp-x86-6.0.57/targets/qsp-x86/images`

3. Run the firststeps script from Terminal Command Prompt:

```bash
$ ./simics targets/qsp-x86/qsp-modern-core.simics
simics> run   
```

4. Scroll back in the Simics Serial Console to find the  Debug statement before the PEI Dispatcher. This would be a place to debug a PEIM

(See PDF for screenshot)

5. Exit Simics

```
simics> stop
simics> quit
```

---
## Slide 24 Summary

- Using PCDs to Configure DebugLib - LAB
- Change the DebugLib instance to modify the debug output - LAB
- Debug EDK II Boot flow - LAB

---
## Slide 25 Questions?

<br>

---
## Slide 26 Return to Main Training Page

Return to Training Table of contents for next presentation [link](https://github.com/tianocore-training/Tianocore_Training_Contents/wiki)


---
## Slide 27 Logo Slide

<br>

---
## Slide 28 Acknowledgements


Redistribution and use in source (original document form) and 'compiled‘ forms (converted to PDF, epub, HTML and other formats) with or without modification, are permitted provided that the following conditions are met:

Redistributions of source code (original document form) must retain the above copyright notice, this list of conditions and the following disclaimer as the first lines of this file unmodified.

Redistributions in compiled form (transformed to other DTDs, converted to PDF, epub, HTML and other formats) must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL TIANOCORE PROJECT BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,  BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

Copyright (c) 2022, Intel Corporation. All rights reserved.

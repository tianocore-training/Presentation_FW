# EDK II Debugging  using Simics® QSP
# with Windows & Linux


---
## Slide 1 UEFI & EDK II Training
### EDK II Debugging w/ Windows or Linux Lab  - Simics® QSP

[tianocore.org](https://www.tianocore.org/)


<!---
Note:

  LabGuid.md for UEFI / EDK II Training  EDK II Debugging w/ Windows or Linux Lab  - Simics® QSP

  Copyright (c) 2022, Intel Corporation. All rights reserved.<BR>

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

<!---  
-->

---

## Slide 2  Lesson Objective


### Lesson Objective <br>



- Using PCDs to Configure DebugLib - LAB
- Change the DebugLib instance to modify the debug output - LAB
- Debug EDK II Boot Flow - LAB

**Note**: the following guide the directory  `<WORKSPACE>` is:
- **Windows** :  `C:\FW\edk2-ws\` 
- **Linux** : `~/fw/edk2-ws/`


---
## Slide 3 Lab 0 Catch up Lab

In this lab, you'll start where the previous Writing UEFI Applications left off.


---
## Slide 4 Lab 0: Catch Up Catch up from previous (1)

### <b>Lab 0: Catch up from previous lab</b>

**Skip** to next slide if Lab Writing UEFI App Lab completed [UEFI App WIN Lab Guide](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_C_04_Writing_UEFI_App_Simics_Win_LabGuide.md)
 or 
[UEFI App Linux Lab Guide](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_L_C_04_Writing_UEFI_App_Simics_Linux_LabGuide.md)

- **Perform** Lab Setup from previous Labs 
[Setup WinLab Guide](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_C_01_Build_Setup_Download_EDK_II_Win_LabGuide.md)
Or 
[Setup Linux Guide](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_L_C_01_Build_Setup_Download_EDK_II_Linux_LabGuide.md)
- **Copy** contents of `../FW/LabSampleCode/SampleAppDebug/`, directory `MyPkg`,  to `../edk2-ws/edk2/`, **creating** the directory `MyPkg/SampleApp` in `<WORKSPACE>/edk2/`
- **Open** `<WORKSPACE>/edk2-platforms/Platform/Intel/SimicsOpenBoardPkg/BoardX58Ich10/OpenBoardPkg.dsc`
- **Add** the following to the `[Components]` section: 

```
 # Add new modules here
 MyPkg/SampleApp/SampleApp.inf
```

- **Save** and close the file `<WORKSPACE>/edk2-platforms/Platform/Intel/SimicsOpenBoardPkg/BoardX58Ich10/OpenBoardPkg.dsc `


---
## Slide 5 Lab 0: Catch Up SampleApp (2)
**Copy** the UefiApplLab.vhd 

From: `.../Lab_Material_FW/FW/LabSampleCode/AppLab/UefiAppLab.vhd `

**to**

**Windows** : 
`%USERPROFILE%\AppData\Local\Programs\Simics\simics-qsp-x86-6.0.57\targets\qsp-x86\images`

**Linux** :  `<SimicsInstallDir>/simics-qsp-x86-6.0.57/targets/qsp-x86/images`

---
## Slide 6 Lab 0: Catch Up SampleApp (3)

**Update** the Simics Script to Use the UefiAppLab.vhd image as a file system


**Edit** the file: `qsp-modern-core.simics` from:

**Windows** : 
`%USERPROFILE%\AppData\Local\Programs\Simics\simics-qsp-cpu-6.0.4\targets\qsp-x86\qsp-modern-core.simics`

**Linux** :
`<SimicsInstallDir>/simics-qsp-cpu-6.0.4/targets/qsp-x86/qsp-modern-core.simics`




**Add** the following Line:
```
$disk1_image="%simics%/targets/qsp-x86/images/UefiAppLab.vhd"
```
Before the "`run-command-file`" line

**Save** `qsp-modern-core.simics`

File: 	`qsp-modern-core.simics`

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
#$disk1_image="%simics%/targets/qsp-x86/images/ShellLab.vhd"
$disk1_image="%simics%/targets/qsp-x86/images/UefiAppLab.vhd"

run-command-file "%simics%/targets/qsp-x86/qsp-clear-linux.simics"

```


---
## Slide 7 Lab 1: Add debug statements 

In this lab, you'll add debug statements to the previous lab's SampleApp UEFI Shell application


---
## Slide 8 Lab 1: Add debug statements SampleApp 01


1. Mount the UefiAppLab.vhd 

**Windows** : using Disk Manager: 
[How to Mount VHD LINK](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_C_04_Writing_UEFI_App_Simics_Win_LabGuide.md#how-to-mount-vhd)

**Linux** : using GuestMount:
[How to Mount VHD LINK](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_L_C_04_Writing_UEFI_App_Simics_Linux_LabGuide.md#mounting-a-vhd-file-disk)


2. Open `<WORKSPACE>/edk2/MyPkg/SampleApp/SampleApp.c`

Add the following to the include statements at the top of the file after below the last "`include`" statement:
```
	#include <Library/DebugLib.h>
```



---
## Slide 9 Lab 1: Add debug statements SampleApp 02
### **Lab 1: Add debug statements to SampleApp**

- Locate the `UefiMain` function. Then copy and paste the following 
code after the &nbsp;"`EFI_INPUT_KEY  KEY;`" statement: and before the first &nbsp;`Print()` statement 


```c
DEBUG ((0xffffffff, "\n\nUEFI Base Training DEBUG DEMO\n") );
DEBUG ((0xffffffff, "0xffffffff USING DEBUG ALL Mask Bits Set\n") );

DEBUG ((DEBUG_INIT,     " 0x%08x USING DEBUG DEBUG_INIT\n" , (UINTN)(DEBUG_INIT))  );
DEBUG ((DEBUG_WARN,     " 0x%08x USING DEBUG DEBUG_WARN\n", (UINTN)(DEBUG_WARN))  );
DEBUG ((DEBUG_LOAD,     " 0x%08x USING DEBUG DEBUG_LOAD\n", (UINTN)(DEBUG_LOAD))  );
DEBUG ((DEBUG_FS,       " 0x%08x USING DEBUG DEBUG_FS\n", (UINTN)(DEBUG_FS))  );
DEBUG ((DEBUG_POOL,     " 0x%08x USING DEBUG DEBUG_POOL\n", (UINTN)(DEBUG_POOL))  );
DEBUG ((DEBUG_PAGE,     " 0x%08x USING DEBUG DEBUG_PAGE\n", (UINTN)(DEBUG_PAGE))  );
DEBUG ((DEBUG_INFO,     " 0x%08x USING DEBUG DEBUG_INFO\n", (UINTN)(DEBUG_INFO))  );
DEBUG ((DEBUG_DISPATCH, " 0x%08x USING DEBUG DEBUG_DISPATCH\n",(UINTN)(DEBUG_DISPATCH)));
DEBUG ((DEBUG_VARIABLE, " 0x%08x USING DEBUG DEBUG_VARIABLE\n",(UINTN)(DEBUG_VARIABLE)));
DEBUG ((DEBUG_BM,       " 0x%08x USING DEBUG DEBUG_BM\n", (UINTN)(DEBUG_BM))  );
DEBUG ((DEBUG_BLKIO,    " 0x%08x USING DEBUG DEBUG_BLKIO\n", (UINTN)(DEBUG_BLKIO))  );
DEBUG ((DEBUG_NET,      " 0x%08x USING DEBUG DEBUG_NET\n", (UINTN)(DEBUG_NET))  );
DEBUG ((DEBUG_UNDI,     " 0x%08x USING DEBUG DEBUG_UNDI\n", (UINTN)(DEBUG_UNDI))  );
DEBUG ((DEBUG_LOADFILE, " 0x%08x USING DEBUG DEBUG_LOADFILE\n",(UINTN)(DEBUG_LOADFILE)));
DEBUG ((DEBUG_EVENT,    " 0x%08x USING DEBUG DEBUG_EVENT\n", (UINTN)(DEBUG_EVENT))  );
DEBUG ((DEBUG_GCD,      " 0x%08x USING DEBUG DEBUG_GCD\n", (UINTN)(DEBUG_EVENT))  );
DEBUG ((DEBUG_CACHE,    " 0x%08x USING DEBUG DEBUG_CACHE\n", (UINTN)(DEBUG_EVENT))  );
DEBUG ((DEBUG_VERBOSE,  " 0x%08x USING DEBUG DEBUG_VERBOSE\n", (UINTN)(DEBUG_EVENT))  );
DEBUG ((DEBUG_ERROR,    " 0x%08x USING DEBUG DEBUG_ERROR\n", (UINTN)(DEBUG_ERROR))  );
```

**Save** and Close SampleApp.c

---
## Slide 10 Lab 1: Update UefiAppLab.vhd File
**Lab 1: Update UefiAppLab.vhd File**

### **Windows**
1. **Build** the Simics QSP Board

- At the Visual Studio Command Prompt, Build  BoardX58Ich10 
```
   $> cd C:\FW\edk2-ws\edk2-platforms\Platform\Intel\
   $> python build_bios.py -p BoardX58Ich10  -t VS20XX
```
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; - Where XX is `15x86` or `17` or `19`

2. **Copy**
`C:\FW\edk2-ws\Build\SimicsOpenBoardPkg\BoardX58Ich10\DEBUG_VS20XX\X64\SampleApp.efi `
**To** 
`X:\UEFIAPPLAB\` 
    - where X is the VHD Drive

---
### **Linux**

1. **Build** the Simics Board
- At the Another Terminal Prompt to Build  BoardX58Ich10 
```
   $  cd ~/fw/edk2-ws/edk2
   $  . edksetup.sh
   $  cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel/
   $  python build_bios.py -p BoardX58Ich10  -t GCC5
```
2. **Copy**  SampleApp.efi from the build directory to the VHD Disk
```
$ cp ~/fw/edk2-ws/edk2/Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/X64/SampleApp.efi ~/VHD
```

---
## Slide 11 Lab 1: Build,Run and Test Result 
### **Lab 1: Build, Run and Test Result**

### **Windows**
1. **Open** a Windows Command prompt (Note: not the Visual Studio Command Prompt )

```bash
$> cd simics-projects\my-simics-project-1
```

2. **Run** the qsp-modern-core script:

```bash
$> .\simics targets/qsp-x86/qsp-modern-core.simics 
simics> run
```


---

### **Linux**

1. **Open** another Terminal Command Prompt
```
$ cd  simics-projects/my-simics-project-1
```
2. **Run** the qsp-modern-core  script :
```
$ ./simics targets/qsp-x86/qsp-modern-core.simics
simics> run   
```

---

### **Both Windows & Linux**

Press "F2" at the logo, then select "Boot Manager" followed by "EFI Internal Shell"

3. **Invoke** SampleApp at the UEFI Shell prompt
```
Shell> Fs1:
FS1:/> SampleApp
FS1:/>
```

- **Observe**
See that the output from the Debug 
statements goes to the Simics Serial Console 



4. **Exit Simics**

```
simics> stop
simics> quit
```

Note:

Notice the changes in DEBUG output for SampleApp in the Simics Serial Console.  Since the new PCD definitions were only applied to SampleApp, the DEBUG output properties are not changed for other parts of the Emulator project.
At the VS Command Prompt


---
## Slide 12 Lab 2: Changing PCD Value


In this lab, you’ll learn how to use PCD values to change debugging capabilities

<br>
Note:

In this lab, you'll learn how to use PCD values to change debugging capabilities.  The previous lab,  Adding Debug Statements, did not display all the DEBUG messages added to SampleApp.c.  This lab shows how to change this behavior.




---
## Slide 13  Lab 2: Change PCDs for SampleApp
### **Lab 2: Change PCDs for SampleApp**

**Open**  `<WORKSPACE>/edk2-platforms/Platform/Intel/SimicsOpenBoardPkg/BoardX58Ich10/OpenBoardPkg.dsc`

Replace `MyPkg/SampleApp/SampleApp.inf` with the following:
```
MyPkg/SampleApp/SampleApp.inf {
   <PcdsFixedAtBuild>
    gEfiMdePkgTokenSpaceGuid.PcdDebugPropertyMask|0xff
    gEfiMdePkgTokenSpaceGuid.PcdDebugPrintErrorLevel|0xffffffff
 }
```

**Save** and close `OpenBoardPkg.dsc`


Note:

---
## Slide 14 Lab 2: Build,Run and Test Result 
### **Lab 2: Build, Run and Test Result**

### **Windows**

1. **Build** the Simics QSP Board

- At the Visual Studio Command Prompt, Build  BoardX58Ich10 
```
   $> cd C:\FW\edk2-ws\edk2-platforms\Platform\Intel\
   $> python build_bios.py -p BoardX58Ich10  -t VS20XX
```
&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Where XX is `15x86` or `17` or `19`

2. **Copy** 
`C:\FW\edk2-ws\Build\SimicsOpenBoardPkg\BoardX58Ich10\DEBUG_VS20XX\X64\SampleApp.efi `
**To** 
`X:\UEFIAPPLAB\` 
    - where X is the VHD Drive


3. **Run** the Simics qsp-modern-core script  from the  Windows Command prompt 

```bash
$> .\simics targets/qsp-x86/qsp-modern-core.simics 
simics> run
```

---

### **Linux**
1. **Build** - At the Terminal Command Prompt, Re-Build  BoardX58Ich10 
```
	$ cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel/
	$ python build_bios.py -p BoardX58Ich10  -t GCC5
```
2. **Copy** SampleApp.efi from the build directory to the VHD Disk
```
$ cp  ../Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/X64/SampleApp.efi ~/VHD
```

3. **Run** the qsp-modern-core  script from Terminal Command Prompt:
```
     $ ./simics targets/qsp-x86/qsp-modern-core.simics
     simics> run   
```

---

### **Both Windows & Linux**

- Press "F2" at the logo, then select "Boot Manager" followed by "EFI Internal Shell"

4. **Invoke** SampleApp at the UEFI Shell prompt

```
Shell> Fs1:
FS1:/> SampleApp
FS1:/>
```

- **Observe**

See that the output from ALL the Debug 
statements goes to the Simics Serial Console 



5. **Exit Simics**

```
simics> stop
simics> quit
```


Notice the changes in DEBUG output for SampleApp in the Simics Serial Console.  


---
## Slide 15 Lab 3: Library Instances for Debugging


In this lab, you’ll learn how to add specific debug library instances

Note:

---
## Slide 16 Lab 3: Using Library Instances for Debugging
### **Lab 3: Using Library Instances for Debugging**

- **Open** `<WORKSPACE>/edk2-platforms/Platform/Intel/SimicsOpenBoardPkg/BoardX58Ich10/OpenBoardPkg.dsc` 
- **Replace** `MyPkg/SampleApp/SampleApp.inf { . . .}` with the following:

```
MyPkg/SampleApp/SampleApp.inf {
   <LibraryClasses>
    DebugLib|MdePkg/Library/UefiDebugLibConOut/UefiDebugLibConOut.inf
 }
```
- **Save** and close `OpenBoardPkg.dsc`




---
## Slide 17 Lab 3: Build,Run and Test Result 
### **Lab 3: Build, Run and Test Result**

### **Windows**
1. **Build** the Simics QSP Board

- At the Visual Studio Command Prompt, Build  BoardX58Ich10 
```
   $> cd C:\FW\edk2-ws\edk2-platforms\Platform\Intel\
   $> python build_bios.py -p BoardX58Ich10  -t VS20XX
```
 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  - Where XX is `15x86` or `17` or `19`

2. **Copy**
`C:\FW\edk2-ws\Build\SimicsOpenBoardPkg\BoardX58Ich10\DEBUG_VS201XX\X64\SampleApp.efi ` 
**To** 
`X:\UEFIAPPLAB\` 

    - where X is the VHD Drive

3. **Run** the Simics qsp-modern-core script  from the  Windows Command prompt 

```bash
$> .\simics targets/qsp-x86/qsp-modern-core.simics 
simics> run
```


---

### **Linux**
1. **Build** - At the Terminal Command Prompt, Re-Build  BoardX58Ich10 
```
	$ cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel/
	$ python build_bios.py -p BoardX58Ich10  -t GCC5
```
2. **Copy** SampleApp.efi from the build directory to the VHD Disk
```
$ cp  ../Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/X64/SampleApp.efi ~/VHD
```

3. **Run** the qsp-modern-core  script from Terminal Command Prompt:
```
     $ ./simics targets/qsp-x86/qsp-modern-core.simics
     simics> run   
```

---

### **Both Windows & Linux**




- Press "F2" at the logo, then select "Boot Manager" followed by "EFI Internal Shell"

4. **Invoke** SampleApp at the UEFI Shell prompt

```
Shell> Fs1:
FS1:/> SampleApp
FS1:/>
```

- **Observe**

See that the output from the Debug statements 
now goes to the Simics VGA console 




5. **Exit Simics**

```
simics> stop
simics> quit
```



---

## Slide 18 Lab 4: Serial port Instance of DebugLib

### **Lab 4: Null Instance of `DebugLib`**

In this lab,  you'll change the `DebugLib` to the Null instance. 



---
## Slide 19 Lab 4: Using Null Library Instances
### **Lab 4: Using Null Library Instances**


- **Open** `<WORKSACE>/edk2-platforms/Platform/Intel/SimicsOpenBoardPkg/BoardX58Ich10/OpenBoardPkg.dsc` 
- **Replace** `MyPkg/SampleApp/SampleApp.inf { . . .}` with the following:

```
MyPkg/SampleApp/SampleApp.inf {
   <LibraryClasses>
    DebugLib|MdePkg/Library/UefiDebugLibConOut/UefiDebugLibConOut.inf
 }
```
- **Save** and close `OpenBoardPkg.dsc`



---
## Slide 20 Lab 4: Build, Run and Test Result
### **Lab 4: Build, Run and Test Result**

### **Windows**
1. **Build** the Simics QSP Board

- At the Visual Studio Command Prompt, Build  BoardX58Ich10 
```
   $> cd C:\FW\edk2-ws\edk2-platforms\Platform\Intel\
   $> python build_bios.py -p BoardX58Ich10  -t VS20XX
```
 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  - Where XX is `15x86` or `17` or `19`

2. **Copy**
`C:\FW\edk2-ws\Build\SimicsOpenBoardPkg\BoardX58Ich10\DEBUG_VS201XX\X64\SampleApp.efi `
**To** 
`X:\UEFIAPPLAB\` 

   - where X is the VHD Drive

3. **Run** the Simics qsp-modern-core script  from the  Windows Command prompt 

```bash
$> .\simics targets/qsp-x86/qsp-modern-core.simics 
simics> run
```


---

### **Linux**
1. **Build** - At the Terminal Command Prompt, Re-Build  BoardX58Ich10 
```
	$ cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel/
	$ python build_bios.py -p BoardX58Ich10  -t GCC5
```
2. **Copy** SampleApp.efi from the build directory to the VHD Disk
```
$ cp  ../Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/X64/SampleApp.efi ~/VHD
```

3. **Run** the qsp-modern-core  script from Terminal Command Prompt:
```
     $ ./simics targets/qsp-x86/qsp-modern-core.simics
     simics> run   
```

---

### **Both Windows & Linux**

Press "F2" at the logo, then select "Boot Manager" followed by "EFI Internal Shell"

4. **Invoke** SampleApp at the UEFI Shell prompt

```
Shell> Fs1:
FS1:/> SampleApp
FS1:/>
```

- **Observe**

See that there is NO Debug output on either output connsoles




5. **Exit Simics**

```
simics> stop
simics> quit
```

---
## Slide 21 Lab 5: Debugging EDK II add Debug to Boot Flow
### **Lab 5: Debugging EDK II add Debug to Boot Flow**

In this lab, you’ll learn how add Debug statements to the EDK II Boot flow and check the debug log output



---
## Slide 22 Lab 5: Debug Boot Flow
### **Lab 5: Debug Boot Flow**

**Edit** the `<WORKSPACE>/edk2/MdeModulePkg/Core/Pei/PeiMain/PeiMain.c` 


**Add** a "`DEBUG`"  print ~line 489 before the call to the `PeiDispatcher`:

```
	 DEBUG((DEBUG_INFO, "***** ***** *****Before call to Pei Dispatcher ***** ***** *****\n"));
	 
```


**Save** PeiMain.c


---
## Slide 23 Lab 5: Build and Test the Boot Flow
### **Lab 5: Build and Test the Boot Flow**

### **Windows**

1. **Build** the Simics QSP Board

- At the Visual Studio Command Prompt, Build  BoardX58Ich10 
```
   $> cd C:\FW\edk2-ws\edk2-platforms\Platform\Intel\
   $> python build_bios.py -p BoardX58Ich10  -t VS20XX
```
 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  - Where XX is `15x86` or `17` or `19`

2. **Copy**
`C:\fw\edk2-ws\Build\SimicsOpenBoardPkg\BoardX58Ich10\DEBUG_VS20XX\FV\BOARDX58ICH10.fd` 	

&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; **To** 

&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; `%USERPROFILE%\AppData\Local\Programs\Simics\simics-qsp-x86-6.0.57\targets\qsp-x86\images`


3. **Run** the Simics qsp-modern-core script  from the  Windows Command prompt 

```bash
$> .\simics targets/qsp-x86/qsp-modern-core.simics 
simics> run
```


---

### **Linux**
1. **Build** - At the Terminal Command Prompt, Re-Build  BoardX58Ich10 
```
	$ cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel/
	$ python build_bios.py -p BoardX58Ich10  -t GCC5
```
2. **Copy** SampleApp.efi from the build directory to the VHD Disk
```
$ cp  ../Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/X64/SampleApp.efi ~/VHD
```

3. **Run** the qsp-modern-core  script from Terminal Command Prompt:
```
     $ ./simics targets/qsp-x86/qsp-modern-core.simics
     simics> run   
```

---

### **Both Windows & Linux**

Press "F2" at the logo, then select "Boot Manager" followed by "EFI Internal Shell"

4. **Observe** Scroll back in the Simics Serial Console to find the   Debug statement before the PEI Dispatcher.  
   - This would be a place to debug a PEIM with a Debugger and add a "`CpuDeadLoop`" or "`CpuBreakPoint`"


5. **Exit Simics**

```
simics> stop
simics> quit
```

---  
## Slide  24 Summary

### Summary 

- Using PCDs to Configure DebugLib - LAB
- Change the DebugLib instance to modify the debug
        output - LAB
- Debug EDK II EDK II Boot Flow - LAB


---
## Slide 25 Questions
<br>

---
## Slide 26 return to main
### <b>Return to Main Training Page</b>
<br>
<br>
Return to Training Table of contents for next presentation <a href="https://github.com/tianocore-training/Tianocore_Training_Contents/wiki#schedule--outline">link</a>

---
## Slide 27 Logo Slide



---
## Slide 28 Acknowledgements
#### Acknowledgements

```c++
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

Copyright (c) 2022, Intel Corporation. All rights reserved.
**/

```


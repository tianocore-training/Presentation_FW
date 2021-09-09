## Slide 1  EDK II Debugging]
<br><br><br>

## UEFI & EDK II Training

#### EDK II Debugging w/ Linux Lab

<br>

<a href='http://www.tianocore.org'>tianocore.org</a>

<!---
Note:

  LabGuid.md for UEFI / EDK II Training  EDK II Debugging Pres-lab

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

<!---  
-->

---

## Slide 2  Lesson Objective
<BR>

### Lesson Objective <br>



- Using PCDs to Configure DebugLib - LAB
- Change the DebugLib instance to modify the debug
        output - LAB
- Debug EDK II using GDB - LAB


---
## Slide 3 Lab 1: Adding Debug Statements

In this lab, you’ll add debug statements to the previous lab's SampleApp UEFI Shell application


Note:
In this lab, you'll learn how to add debug statements.  
This lab uses code from a previous exercise as a starting point (refer to  Writing Simple UEFI Applications).  Before proceeding, verify that the SampleApp code is present in your workspace and that the code references the OvmfPkgX64.dsc file.

---
## Slide 4 Lab 1: Catch Up SampleApp

### <b>Lab 1: Catch up from previous lab</b>

Skip to next slide if Lab Writing UEFI App Lab</a> completed - <a href="https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_L_C_03_Writing_UEFI_App_Linux_LabGuide.md"> Labguide </a> <BR>

- Perform Lab Setup from previous Labs  <a href="https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_L_C_01_Platform_Build_OVMF-QEMU_Lab_guide.md">LabGuide</a>  
- Create a Directory under the workspace `~/src/edk2-ws/edk2 : "SampleApp"`
- Copy contents of `~/FW/LabSampleCode/SampleAppDebug to ~src/edk2-ws/edk2/SampleApp`
- Open `~src/edk2-ws/edk2/OvmfPkg/OvmfPkgX64.dsc`
- Add the following to the `[Components]` section: 

```
 # Add new modules here
SampleApp/SampleApp.inf
```
<br>
- Save and close the file `~src/edk2-ws/edk2/OvmfPkg/OvmfPkgX64.dsc`



---
## Slide 5 Lab 1: Add debug statements SampleApp

Open a Terminal Command Prompt
```
    bash$ cd ~/src/edk2-ws
    bash$ export WORKSPACE=$PWD
    bash$ export PACKAGES_PATH=$WORKSPACE/edk2:$WORKSPACE/edk2-libc
    bash$ cd edk2
    bash$ . edksetup.sh
```
Open `~/src/edk2/SampleApp/SampleApp.c`

Add the following to the include statements at the top of the file after below the last "include" statement:
```
	#include <Library/DebugLib.h>
```



---
## Slide 6 Lab 1: Add debug statements SampleApp 02]
### <b>Lab 1: Add debug statements to SampleApp</b>
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

---
## Slide 7 Lab 1: Update the Qemu Script
### <b>Lab 1: Update the Qemu Script</b>

Edit  the Linux shell script to run the QEMU from the ~/run-ovmf directory and add  the option for a serial log
```
bash$ gedit RunQemu.sh
```
Add to the RunQemu.sh file
```
qemu-system-x86_64 -pflash bios.bin -hda fat:rw:hda-contents 
-net none -debugcon file:debug.log 
-global isa-debugcon.iobase=0x402  -serial file:serial.log
```

Note: See contents of ~/FW/edk2Linux/SRunQemu.sh 
(copy and paste to ~/run-ovmf/RunQemu.sh)

Save and Exit



---
## Slide 8 Lab 1: Build,Run and Test Result 01
### <b>Lab 1: Build, Run and Test Result 01</b>

Build SampleApp – Cd to ~/src/edk2 dir
```
bash$ cd ~/src/edk2-ws/edk2
bash$ build
```
Copy the OVMF.fd to the run-ovmf directory naming it bios.bin
```
bash$ cd ~/run-ovmf
bash$ cp ~/src/edk2-ws/Build/OvmfX64/DEBUG_GCC5/FV/OVMF.fd bios.bin
```
Copy SampleApp.efi to hda-contents
```
bash$ cd ~/run-ovmf/hda-contents 
bash$ cp ~/src/edk2-ws/Build/OvmfX64/DEBUG_GCC5/X64/SampleApp.efi .
```

---
## Slide 9 Lab 1: Build,Run and Test Result 02
### <b>Lab 1: Build, Run and Test Result 02</b>


Test by Invoking Qemu
```
bash$ cd ~/run-ovmf 
bash$ . RunQemu.sh
```
Run the application from the shell
```
Shell> SampleApp
```
Check the contents of the debug.log file
```
bash$ cat debug.log
```
Exit QEMU


Note:

Notice the changes in DEBUG output for SampleApp in the Visual Studio Command Prompt.  Since the new PCD definitions were only applied to SampleApp, the DEBUG output properties are not changed for other parts of the Emulator project.
At the debug.log file


## Slide 10 Lab 2: Changing PCD Value
<br>

In this lab, you’ll learn how to use PCD values to change debugging capabilities

<br>
Note:

In this lab, you'll learn how to use PCD values to change debugging capabilities.  The previous lab,  Adding Debug Statements, did not display all the DEBUG messages added to SampleApp.c.  This lab shows how to change this behavior.




---
## Slide 11  Lab 2: Change PCDs for SampleApp
### <b>Lab 2: Change PCDs for SampleApp</b>

Open  `~src/edk2-ws/OvmfPkg/OvmfPkgX64.dsc`

Replace `SampleApp/SampleApp.inf` with the following:
```
SampleApp/SampleApp.inf {
   <PcdsFixedAtBuild>
    gEfiMdePkgTokenSpaceGuid.PcdDebugPropertyMask|0xff
    gEfiMdePkgTokenSpaceGuid.PcdDebugPrintErrorLevel|0xffffffff
 }
```
Save and close `~src/edk2-ws/OvmfPkg/OvmfPkgX64.dsc`


Build SampleApp :   
```
bash$ build  
```
Copy SampleApp.efi to hda-contents
```
  bash$ cd ~/run-ovmf/hda-contents
  bash$ cp ~/src/edk2/Build/OvmfX64/DEBUG_GCC5/X64/SampleApp.efi .
```

Note:

---
## Slide 12 Lab 2: Build,Run and Test Result 
### <b>Lab 2: Build, Run and Test Result</b>


Test by Invoking Qemu
```
bash$ cd ~/run-ovmf 
bash$ . RunQemu.sh
```
Run the application from the shell
```
Shell> SampleApp
```
Check the contents of the debug.log file
```
bash$ cat debug.log
```
Exit QEMU



Notice the changes in DEBUG output for SampleApp in the debug.log file.  Since the new PCD definitions were only applied to SampleApp, the DEBUG output properties are not changed for other parts of the  OVMF project.



---
## Slide 13 Lab 3: Library Instances for Debugging


In this lab, you’ll learn how to add specific debug library instances

Note:

---
## Slide 14 Lab 3: Using Library Instances for Debugging
### <b>Lab 3: Using Library Instances for Debugging</b>

- Open `~src/edk2-ws/edk2/OvmfPkg/OvmfPkgX64.dsc`
- Replace `SampleApp/SampleApp.inf { . . .}` with the following:

```
SampleApp/SampleApp.inf {
   <LibraryClasses>
    DebugLib|MdePkg/Library/UefiDebugLibConOut/UefiDebugLibConOut.inf
 }
```
- Save and close OvmfPkgX64.dsc

Build SampleApp – 
```
bash$ cd ~/src/edk2-ws/edk2     
bash$ build
```
Copy SampleApp.efi to hda-contents
```
bash$ cd ~/run-ovmf/hda-contents 
bash$ cp ~/src/edk2-ws/Build/OvmfX64/DEBUG_GCC5/X64/SampleApp.efi .
```


---
## Slide 15 Lab 3: Run the Qemu Script
### <b>Lab 3: Run the Qemu Script</b>
Test by Invoking Qemu
```
bash$ cd ~/run-ovmf 
bash$ . RunQemu.sh
```
Run the application from the shell
```
Shell> SampleApp
```
See that the output from the Debug statements now goes to the QEMU console 

Exit QEMU



## Slide 16 Lab 4: Serial port Instance of DebugLib

### <b>Lab 4: Serial port instance of `DebugLib`</b>
<br>
In this lab, you’ll change the DebugLib to the Serial port instance.


Note:

The DEBUG output for SampleApp is to serial port


---
## Slide 17 Lab 4: Using Serial port Library Instances
### <b>Lab 4: Using Serial port Library Instances</b>
<br>


Open `~src/edk2-ws/edk2/OvmfPkg/OvmfPkgX64.dsc`
Replace SampleApp/SampleApp.inf { . . .} with the following:
```
SampleApp/SampleApp.inf {
   <LibraryClasses>    	DebugLib|MdePkg/Library/BaseDebugLibSerialPort/BaseDebugLibSerialPort.inf
 }
```
Save and close `~src/edk2-ws/edk2/OvmfPkg/OvmfPkgX64.dsc`
Build SampleApp – 
```
bash$ cd ~/src/edk2-ws/edk2     
bash$ build
```
Copy SampleApp.efi to hda-contents
```
bash$ cd ~/run-ovmf/hda-contents 
bash$ cp ~/src/edk2-ws/Build/OvmfX64/DEBUG_GCC5/X64/SampleApp.efi .
```


---
## Slide 18 Lab 4: Run the Qemu Script
### <b>Lab 4: Run the Qemu Script</b>

Test by Invoking Qemu
```
bash$ cd ~/run-ovmf 
bash$ . RunQemu.sh
```
Run the application from the shell
```
Shell> SampleApp
```
See that the output from the Debug statements now goes to the QEMU console 

Exit QEMU


Note:
Notice  Debug messages output to the Serial.log file now





## Slide 19 Lab 5: Debugging EDK II with GDB
<br>

### <b>Lab 5: Debugging EDK II with GDB</b>
<br>
In this lab, you’ll learn how setup the Linux GDB to use with EDK II and Qemu 
See also the tianocore.org wiki page: <br>
<A href="https://github.com/tianocore/tianocore.github.io/wiki/How-to-debug-OVMF-with-QEMU-using-GDB">How to use GDB with QEMU</a>

---
## Slide 20 Lab 5.1: Lab 5.1: Update the Qemu Script
### <b>Lab 5.1: Lab 5.1: Update the Qemu Script </b> 
<br>

Edit the Linux shell script to run the QEMU from the run-ovmf directory and add the option for GDB "-s" to generate a symbol file and also use IA32 instead of x86_64
```
 bash$ cd ~/run-ovmf
 bash$ gedit RunQemu.sh
```
add the "-s" to the following to RunQemu.sh (Note, this is for the IA32 and the –serial is not there.)
```
  qemu-system-i386  -s  -pflash bios.bin -hda fat:rw:hda-contents -net none -debugcon file:debug.log -global isa-debugcon.iobase=0x402
```

Save and Exit

Note: the "-s" is to invoke the GDB Debugger

---
## Slide 21 Lab 5.2: 
### <b>Lab 5.2:  </b> 
<br>
Open ~src/edk2-ws/edk2/OvmfPkg/OvmfPkgIa32.dsc and add the application to the end of the [Components] section:
```
[Components]
# add at the end of the components section OvmfPkgIa32.dsc
  SampleApp/SampleApp.inf 
```

Save and close `~src/edk2-ws/edk2/OvmfPkg/OvmfPkgIa32.dsc`

Build OVMF for IA32
```
bash$ cd ~src/edk2-ws/edk2
bash$ build -a IA32 -p OvmfPkg/OvmfPkgIa32.dsc 
```
Copy the the OVMF.fd to the run-ovmf directory renaming it bios.bin: 
```
bash$ cd ~/run-ovmf/ 
bash$ cp ~/src/edk2-ws/Build/OvmfIa32/DEBUG_GCC5/FV/OVMF.fd bios.bin
```




---
## Slide 22 Lab 5.3: Lab 5.3: Build Ovmf for IA32
### <b>Lab 5.3: Lab 5.3: Build Ovmf for IA32 </b> 
<br>

Copy the output of SampleApp to the hda-contents directory:
```
   bash$ cd ~/run-ovmf/hda-contents
   bash$ cp ~/src/edk2-ws/Build/OvmfIa32/DEBUG_GCC5/IA32/SampleApp  .
```
The following will be in the `~/run-ovmf/hda-contents/`
```
	SampleApp.efi
	SampleApp.debug
   	SampleApp (Directory)
```

Open a Terminal(1) Prompt and Invoke Qemu
```
bash$ cd ~/run-ovmf
bash$ . RunQemu.sh
```
Run the application from the shell
```
 Shell> SampleApp 
```


---
## Slide 23 Lab 5.4: Lab 5.4: Check debug.log
### <b>Lab 5.4: Lab 5.4: Check debug.log </b> 
<br>

Open another Terminal(2) Prompt in the run-ovmf directory and check the debug.log file.
```
bash$ cd ~/run-ovmf
bash$ cat debug.log
```
See the line: Loading driver at `0x00006AEE000` is the memory location where your UEFI Application is loaded.

```
 InstallProtocolInterface: 5B1B31A1-9562-11D2-8E3F-00A0C969723B 6F0F028
 Loading driver at 0x00006AEE000 EntryPoint=0x00006AEE756 SampleApp.efi
 InstallProtocolInterface: BC62157E-3E33-4FEC-9920-2D3B36D750DF 6F0FF10
```

Note: the above memory location maybe different on different labs

---
## Slide 24 Lab 5.5: Add a Debug Print
### <b>Lab 5.5: Add a Debug Print </b> 
<br>


Add a DEBUG statement to your SampleApp.c application to get the entry point of your code.<br> 
Add the following DEBUG line just before the DEBUG statements from the previous lab:
```
UefiMain (
// . . .
    EFI_INPUT_KEY       Key;
    // ADD the following line
    DEBUG ((EFI_D_INFO, "My Entry point: 0x%p\r\n", (CHAR16*)UefiMain )  );
```

When you print out the debug.log again, the exact entry point for your code will show.<br>
This is useful to double check symbols are fixed up to the correct line numbers in the source file.

```
   Loading driver at 0x00006AEE000 EntryPoint=0x00006AEE756 SampleApp.efi
  InstallProtocolInterface: BC62157E-3E33-4FEC-9920-2D3B36D750DF 6F0FF10
  ProtectUefiImageCommon - 0x6F0F028
    - 0x0000000006AEE000 - 0x0000000000002B00
  InstallProtocolInterface: 752F3136-4E16-4FDC-A22A-E5F46812F4CA 7EA4B00
  My Entry point: 0x06AEE496 
```
Note: the above memory location maybe different on different labs

---
## Slide 25 Lab 5.6: Invoking GDB
### <b>Lab 5.6: Invoking GDB </b> 
<br>




---
## Slide 25 Lab 5.6: Invoking GDB
### <b>Lab 5.6: Invoking GDB </b> 
<br>

In the terminal(2) prompt Invoke GDB  (note - at first there will be nothing in the source window)
```
bash$ cd ~/run-ovmf/hda-contents
bash$ gdb --tui
```
Load your UEFI Application SampleApp.efi with the "file" command.

```
(gdb) file SampleApp.efi
Reading symbols from SampleApp.efi...(no debugging symbols found)...done.
```
Check where GDB has for ".text" and ".data" offsets with "info files" command.
```
 (gdb) info files
 Symbols from "/home/u-mypc/run-ovmf/hda-contents/SampleApp.efi".
 Local exec file:
        `/home/u-mypc/run-ovmf/hda-contents/SampleApp.efi',
        file type pei-i386.
        Entry point: 0x756
        0x00000240 - 0x000028c0 is .text
        0x000028c0 - 0x00002980 is .data
        0x00002980 - 0x00002b00 is .reloc
```

---
## Slide 26 Lab 5.7: Calculate Addresses
### <b>Lab 5.7: Calculate Addresses </b>
<br>

We need to calculate our addresses for ".text" and ".data" section. <br>
The application is loaded under 0x00006AEE000 (loading driver point - NOT Entrypoint) and we know text and data offsets.
```
text = 0x00006AEE000  +  0x00000240 = 0x06AEE240
data = 0x00006AEE000  +  0x00000240 + 0x000028c0 = 0x06AF0B00
```

Unload the .efi file

```
(gdb) file
No executable file now.
No symbol file now.
```

---
## Slide 27 Lab 5.8: Load the Symbols for SampleApp
### <b>Lab 5.8: Load the Symbols for SampleApp</b>
<br>

Load the symbols with the fixed up address using SampleApp output .debug file using the "add-symbol-file" command:
```
(gdb) add-symbol-file SampleApp.debug 0x06AEE240 -s .data 0x06AF0B00 
add symbol table from file "SampleApp.debug" at
        .text_addr = 0x6aee240
        .data_addr = 0x6af0b00
(y or n) y
Reading symbols from SampleApp.debug...done.
```
Set a break point at UefiMain
```
(gdb) break UefiMain
Breakpoint 1 at 0x6aee496: file /home/u-uefi/src/edk2/SampleApp/SampleApp.c, line 40.
```



---
## Slide 28 Lab 5.9: Attach GDB to QEMU
### <b>Lab Lab 5.9: Attach GDB to QEMU </b>
<br>
Attach the GDB debugger to QEMU
```
(gdb) target remote localhost:1234
Remote debugging using localhost:1234
0x07df6ba4 in ?? ()
```
Continue in GDB
```
(gdb) c
Continuing.
```
In the QEMU Window Invoke your application again
```
Fs0:\> SampleApp.efi
```
The GDB will hit your break point in your UEFI application's entry point, and you can begin to debug with source code debugging



---
## Slide 29 Lab 5: GBD and QEMU Windows
### <b>Lab Lab 5: GBD and QEMU Windows</b>
<br>

picture  of GDB Debugger stoped at the entry point


---  
## Slide  30 Summary

### Summary 

- Using PCDs to Configure DebugLib - LAB
- Change the DebugLib instance to modify the debug
        output - LAB
- Debug EDK II using GDB Debugger - LAB


---
## Slide 26 Questions
<br>

---
## Slide 27 return to main
### <b>Return to Main Training Page</b>
<br>
<br>
Return to Training Table of contents for next presentation <a href="https://github.com/tianocore-training/Tianocore_Training_Contents/wiki#schedule--outline">link</a>

---
## Slide 28 Logo Slide]



---
## Slide 29 Acknowledgements
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

Copyright (c) 2021, Intel Corporation. All rights reserved.
**/

```


---
## Slide 30 Backup Section


---
## Slide 31 Issue: Debugging in Emulation with Windows 7 and VS
### Issue:<br>

Debugging in  Emulation with Windows 7 <br>and Visual Studio does not work?


- Symptom:  With Windows 7 a CpuBreakpoint() or ASSERT  just exits with an error from the "Build Run" command. 


- Link to fix this issue: 
https://github.com/tianocore/tianocore.github.io/wiki/NT32#Debugging_in_Nt32_Emulation_with_Windows_7_and_Visual_Studio_does_not_work

1. Run the RegEdt32
2. Navigate to the HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows NT\CurrentVersion\AeDebug
3. Add a string value entry called "Auto" with a value of "1"

- Windows 10  Visual Studio does not seem to have this issue 

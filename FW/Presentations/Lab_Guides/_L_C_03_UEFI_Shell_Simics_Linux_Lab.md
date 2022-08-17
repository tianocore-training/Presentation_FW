# UEFI Shell Lab with Simics
# Linux

---
## Slide 1 UEFI & EDK II Training
### UEFI Shell Lab with Simics - Linux

See also Lab Guide (this document) for Copy & Paste examples in labs

[tianocore.org](https://www.tianocore.org/)

---
## Slide 2 Lesson Objectives

- Run UEFI Shell with Simics
- Run UEFI Shell Commands
- Run UEFI Shell Scripts

---
## Slide 3 UEFI Shell Lab with Simics

<br>

---
## Slide 4 Invoke Simics with Platform BoardX58Ich10

First Setup for Building EDK II, See [Lab Setup](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_C_01_Platform_Build_Win_Emulator_Lab_guide.md#build-emulator) then [Platform Build Lab for Simics Linux](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_L_C_02_Platform_Build_Linux_Simics_LabGuide.md)

To Build BoardX58Ich10, from a Terminal Command prompt

```bash
$ cd ~/fw/edk2-ws/edk2
$ . edksetup.sh
$ cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel
$ python build_bios.py -p BoardX58Ich10 -t GCC5
```

Copy the .../Build/.../FV/BOARDX58ICH10.fd to \<SimicsInstallDir\>/simics-qsp-x86-6.0.57/targets/qsp-x86/images

Open Terminal Prompt in the Simics project directory e.g. `$ HOME/simics-projects/my-simics-project-1`

Run the qsp-modern-core script:

```bash
$> ./simics targets/qsp-x86/qsp-modern-core.simics
simics> run (Press "F2" at the logo)
```

---
## Slide 5 UEFI Shell Commands
### Commands from the Command Line Interface

<br>

---
## Slide 6 Common Shell Commands for Debugging

```
help
mm
mem
memmap
drivers
devices
devtree
dh
load
dmpstore
pci
stall
bcfg
```

"-b" is the command line parameter for breaking after each page.

---
## Slide 7 Shell Help

```
Shell> help -b
```

---
## Slide 8 Shell "memmap"

```
Shell> memmap
```

Displays the memory map maintained by the UEFI environment

---
## Slide 9 Shell "mm"

```
Shell> mm -? -b
```

Help for "mm" command shows options for different types of memory and I/O that can be modified.

---
## Slide 10 Shell "mm"

```
Shell> mm df33d000**
```

** Pick a location from the MemMap command on the Previous Slide

MM in can display / modify any location

Try 

```
Shell> mm 0000
```

"q" to quit

---
## Slide 11 Shell "mem"

```
Shell> mem
```

Displays the contents of the system or device memory 

without arguments, displays the system memory configuration. 

---
## Slide 12 Shell "Drivers"

```
Shell> drivers -b
```

Displays the UEFI driver list.

To get a description of each section in the list, (top header)

Use:

```
Shell> drivers -?
```

---
## Slide 13 Shell "Devices"

```
Shell> devices -b
```

Displays a list of devices that UEFI drivers manage.

---
## Slide 14 Shell "Devtree"

```
Shell> devtree -b
```

Displays tree of devices currently managed by UEFI drivers.

---
## Slide 15 Shell Handle Database - "Dh"

```
Shell> dh -b
```

Dump Handle - Displays the device handles associated with UEFI drivers

Also try `dh - d` with handle number to get more information on that handle.

---
## Slide 16 Shell Handle Database - "Dh -d"

```
Shell> dh -d 76
```

Dump Handle of Device "76" - Dumps UEFI Driver Model Information

Also try `dh -d ##` with handle number of the device this driver is handling, e.g. 

```
dh -d b5
```

to get more information on that device being managed

---
## Slide 17 Shell "Load"

```
Shell> load -?
```

Loads a UEFI driver into memory

---
## Slide 18 Shell "dmpstore"

```
Shell> dmpstore -all -b
```

Display the contents of the NVRAM variables

---
## Slide 19 Shell "pci"

```
Shell> pci -? -b
```

Display the help for the PCI command

---
## Slide 20 Shell "pci"

```
Shell> pci -b
```

Display the list of PCI devices

---
## Slide 21 "pci 00 00 00 -i"

```
Shell> pci 00 00 00 -i
```

Display the configuration space of Bus 0, Device 0, Function 0.

---
## Slide 22 Shell "Stall"

```
Shell> stall 10000000
```

Stalls the operation for a specified number of microseconds

```
Shell> stall 10000000
Shell> 
```

---
## Slide 23 UEFI Shell Scripts
### Use Scripting with UEFI Shell

<br>

---
## Slide 24 UEFI Shell Scripts

The UEFI Shell can execute commands from a file, which is called a batch script file (.nsh files)

**Benefits:** These files allow users to simplify routine or repetitive tasks.

- Perform basic flow control.
- Allow branching and looping in a script.
- Allow users to control input and output and call other batch programs (known as script nesting).

---
## Slide 25 Exit QSP UEFI Shell & Simics

- To stop the QSP simulation, from the Simics command line prompt window, type "stop"
	- This will stop the Simics simulation of the QSP board
	- To continue, type: "run"
- To exit this situation, type: "quit"
	- This will remove all other Simics windows

---
## Slide 26 Copy ShellLab.vhd file

Copy the ShellLab.vhd

From:

.../Lab_Material_FW/FW?LabSampleCode/ShellScripts/ShellLab.vhd

to

\<SimicsInstallDir\>/simics-qsp-x86-6.0.57/targets/qsp-x86/images

---
## Slide 27 Update the Simics Script

Update the Simics Script to Use the ShellLab.vhd image as a file system

Edit the file: qsp-modern-core.simics from

`\<SimicsInstallDir\>/simics-qsp-x86-6.0.57/targets/qsp-x86/qsp-modern-core.simics`

Add the following line:

```
$disk1_image="%simics%/targets/qsp-x86/images/ShellLab.vhd"
```

Before the "run-command-file" line

Save qsp-modern-core.simics

File: qsp-modern-core.simics

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
$disk1_image="%simics%/targets/qsp-x86/images/ShallLab.vhd“

run-command-file "%simics%/targets/qsp-x86/qsp-clear-linux.simics"

```

---
## Slide 28 Run Simics First Steps Script

Re-run the qsp-modern-core script from the Simics Command Prompt:

```bash
$> ./simics targets/qsp-x86/qsp-modern-core.simics
simics> run (Press "F2" at the logo)
```

Note: now there is a "FS1" file system

---
## Slide 29 Writing UEFI Shell Scripts

At the shell prompt

```
Shell> fs1:
FS1:\> edit HelloScript.nsh
```

Type:

```
echo Hello World
```

Press "F2"

Enter

Press "F3" to exit

(See Powerpoint for Help Menu - Shell)

---
## Slide 30 Hello World Script

In the shell, type HelloScript for the following result:

```
FS1:\> HelloScript.nsh
FS1:\> echo Hello World
Hello World
FS1:\> 
```

---
## Slide 31 UEFI Shell Script Example

Script1.nsh

```
# Simple UEFI Shell script file
echo -off
script2.nsh
if exist %cwd%Mytime.log then
	type Mytime.log
endif
echo "%HThank you." "%VByeBye:) %N"
```

Script2.sh

```
# Show nested scripts
time > Mytime.log
for %a run (3 1 -1)
	echo %a counting down
endfor
```

---
## Slide 32 Run UEFI Shell Scripts

At the shell prompt type:

```
Shell> fs1:
FS1:\> cd ShellScripts
FS1:\ShellScripts\> Script1
```

The shell will then display the following

```
FS1:\ShellScripts\> script2.nsh
FS1:\ShellScripts\> time > Mytime.log
FS1:\ShellScripts\> for %a run (3 1 -1)
FS1:\ShellScripts\>		echo %a counting down
3 counting down
FS1:\ShellScripts\> endfor
FS1:\ShellScripts\> for %a run (3 1 -1)
FS1:\ShellScripts\>		echo %a counting down
2 counting down
FS1:\ShellScripts\> endfor
FS1:\ShellScripts\> for %a run (3 1 -1)
FS1:\ShellScripts\>		echo %a counting down
1 counting down
FS1:\ShellScripts\> endfor
FS1:\ShellScripts\> for %a run (3 1 -1)
FS1:\ShellScripts\>		echo %a counting down
FS1:\ShellScripts\> if exist %cwd%Mytime.log then
FS1:\ShellScripts\> echo " Thank you." " ByeBye" " :)" " " " Done"
 Thank you.  ByeBye  :)  Done
```

---
## Slide 33 Run UEFI Shell Scripts

Remove the "#" on the first line


**UEFI EDIT Script1.nsh**
```
echo -off
script2.nsh
if exist %cwd%/Mytime.log then
	type Mytime.log
endif
echo "%HThank yoy. %VByeBye:) %N"
```

Press "F2"
Enter
Press "F3" to exit
Type

```bash
FS1:\ShellScripts\> Script1
```

---
## Slide 34 UEFI Shell Global Variables
### Use BCFG and DmpStore

<br>


---
## Slide 35 Show the UEFI Boot Variables

At the Shell Prompt:

```
Shell> FS1:
FS1:> BCFG Boot Dump
```

---
## Slide 36 Use the Dmpstore to Show the Boot Order

At the Shell Prompt:

```
FS1:> Dmpstore BootOrder
```

---
## Slide 37 Use the BCFG to Move a boot item

Use BCFG to Move the 8th boot item too 1st location (location 0).

Then verify using the "dmpstore"

(Hint: use `BCFG - ? -b` for help menu)

The dmpstore output should look like the screen shot (see powerpoint)

---
## Slide 38 Use the BCFG to Add a boot item

Use the file from on FS1 `/OldShell.Shell_FullX64.efi` and use BCFG to Add a 08 entry for a new boot option with Shell_FullX64.efi

(hint: check bcfg -h for adding a boot entry)

Then verify using the "BCFG Boot Dump"

After the bcfg add, the output should look like (see screenshot on powerpoint)

---
## Slide 39 Verify Results from BCFG Commands

At the Shell Prompt Type "exit" to get back to the BIOS Setup

Press escape

Select "Boot Manager"

Verify:

- EFI Internal Shell - item 1
- Old EFI Shell 1.0 - item 8

---
## Slide 40 Exit QSP UEFI Shell & Simics

- To stop the QSP Simulation, from the Simics Command Line Prompt Window, Type: "stop"
	- This will stop the Simics simulation of the QSP board
	- To continue, type "run"
- To Exit this Simulation, type: "quit"
	- This will remove all other Simics windows

---
## Slide 41 Summary

- Run UEFI Shell (Windows Emulation)
- Run UEFI Shell Commands
- Run UEFI Shell Scripts

---
## Slide 42 Questions?

<br>

---
## Slide 43 Return to Main Training Page

Return to Training Table of Contents for next presentation [link](https://github.com/tianocore-training/Tianocore_Training_Contents/wiki)

---
## Slide 44 Logo Slide

<br>

---
## Slide 45 Ackowleedgements 

Redistribution and use in source (original document form) and 'compiled‘ forms (converted to PDF, epub, HTML and other formats) with or without modification, are permitted provided that the following conditions are met:


Redistributions of source code (original document form) must retain the above copyright notice, this list of conditions and the following disclaimer as the first lines of this file unmodified.


Redistributions in compiled form (transformed to other DTDs, converted to PDF, epub, HTML and other formats) must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.


THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL TIANOCORE PROJECT BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,  BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.


Copyright (c) 2021-2022, Intel Corporation. All rights reserved.

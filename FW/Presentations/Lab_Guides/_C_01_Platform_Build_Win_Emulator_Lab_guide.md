## Slide 1  @title[Platform Build Win Emulator Lab]

## UEFI & EDK II Training

 Platform Build Windows Emulator Lab 

<br>
<a href='http://www.tianocore.org'>tianocore.org</a>


<!---
 Lab_Guide.md for Platform Build Win Emulator Lab

  Copyright (c) 2020-2021, Intel Corporation. All rights reserved.<BR>

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
## Slide 2  @title[Lesson Objective]

### Platform Build Labs 
<br>
   
- Pin Visual Studio Command Prompt to Windows Task Bar 
- Build a EDK II Platform using Emulator package
- Run the Emulator in Windows



---
## Slide 3  @title[Pin VS CMD Prompt Section]
<br>

### Pin VS Command Prompt 
 
Pin the Visual Studio Command prompt to Windows <br>Task Bar


---
## Slide 4  @title[Pin VS CMD Prompt]
### Pin  VS Command Prompt

Windows 10 
<br>

Steps to Pin Visual Studio Command Prompt to task bar for Windows 10
1. Using the Start menu in Windows 10, Left Click on “Windows Key” Lower Left
2. Scroll down from the scroll bar on the right until “Visual Studio 201n”
3. Left Click “Visual Studio 201n”


---
## Slide 5  @title[Pin VS CMD Prompt 02]
### Pin  VS Command Prompt


4. Left Click <b>"Visual Studio Tools"</b>

This will open another Windows file explorer window <br>

Note: <i>VS 2013 example, other version of VS maybe different</i>  

<br>

---
## Slide 6  @title[Pin VS CMD Prompt 03]
### Pin  VS Command Prompt


5. Select <b>"Developer Command Prompt for VS201<i>n</i>"</b> 

6. Right Click to open Windows dialog box 

<font color="Red"><b>Do not use any of the other</b><br> ".. Command Prompts" </font>





---
## Slide 7  @title[Pin VS CMD Prompt 04]
### Pin  VS Command Prompt

7. Left Click  on <br>"Pin to to taskbar" 




---
## Slide 8  @title[Pin VS CMD Prompt 05]
### Pin  VS Command Prompt

8. Open the VS Command Prompt 
<br>


All Windows Labs use this short-cut to Build Edk II platforms and projects using Windows Visual Studio<br>
(2010 / 2012 / 2013 / 2015/ 2017 or 2019)



---
## Slide 9  @title[End of Pin VS Section]
<br>
## End Pin  VS Prompt




---
## Slide 10  @title[Lab 1 -Build Emulator Section]


## Build Emulator

 Setup EmulatorPkg to build and run emulation <br> w/ Windows



---
## Slide 11  @title[Prerequisites - Done Before Class]
<b>Prerequisites<br><i>- Done Before Class</i></b>
- Windows 10:
   - Continuous Integration (CI) - Stuart CI Build with  Visual Studio VS2017 or VS2019
   - Non Stuart CI - Visual Studio VS2015, VS2017 or VS2019
   - Windows SDK (for rc)
   - Windows WDK (for Capsules)
- Python 3.7.x or greater and /Scripts directories on Path: <a href="https://www.python.org/">Link</a> to download 
- Git for Windows on Path : <a href="http://git-scm.com/download/win">Link</a>
- NASM  for Win64 : <a href="https://www.nasm.us/pub/nasm/releasebuilds/2.12.02/win64/">Link</a>




## Slide 12  @title[Create Work Space Directory]
<b>Create Work Space Directory</b>


Open a  Windows command prompt <br>
Make new directory for Work Space:

```
$ cd /
$ Mkdir FW
$ cd FW
$ Mkdir edk2-ws
$ cd edk2-ws
```

---
## Slide 13  @title[Download the EDK II Source Code ]

### Download the EDK II Source Code
Note:  if behind a firewall, set PROXYS FIRST
```
$ git config --global https.proxy proxy.jf.intel.com:911
$ git config --global http.proxy proxy.jf.intel.com:911
```

```
C:\FW\edk2-WS> git clone -b Edk2Lab_21Q1 https://github.com/tianocore-training/edk2.git
C:\FW\edk2-WS> git clone -b LabBranch https://github.com/tianocore-training/edk2-libc.git

```

Download the Submodules and Checkout the Lab Branch
```
C:\FW\edk2-wS> cd edk2
C:\FW\edk2-wS\edk2> git submodule update --init
C:\FW\edk2-wS> cd ..
```

---
## Slide 14  @title[Download Lab_Material_FW -getting the Source ]
### Download Lab Material<br>
Download the Lab_Material_FW.zip from :  <a href="https://github.com/tianocore-training/Lab_Material_FW/archive/master.zip">github.com Lab_Matrial_FW.zip</a><br>
<br>
OR<br>
Use `git clone` to download the Lab_Material_FW<span>
```bash
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
## Slide 15  @title[Build  Edk2 -getting the Source ]
<b>Build EDK II  </b><br>

– Extract the Source  

Extract the Downloaded `Lab_Material_FW-master.zip to C:\ `


Note:
Extract the Downloaded Lab_Material_FW.zip to C:\ (this will create a directory FW )

---
## Slide 16  @title[Build  Edk2 -getting the Source 02]
<b>Build EDK II  </b><br>
 - Copy edk2-ws 


From the downloaded Lab_Material_FW folder,<br> <b>copy</b> and <b>paste</b> folder `"..\edk2-ws"` to `"C:/FW"`


<i>Note:</i> Overwrite existing files and directories

---
## Slide 17  @title[Build  Edk2 -get Nasm]
<b>Build EDK II  </b><br>
 - Get Nasm 

Copy `Nasm` directory to `C:\`

(creating `C:\Nasm` directory)



---
## Slide 18  @title[CI Stuart Build  sub Section]
<br>

### CI Stuart Build EmulatorPkg
<b>SKIP</b> if doing Non-Stuart CI Build

---
## Slide 19  @title[Stuart CI Build Edk II]
### <b>Stuart CI Build EDK II  </b><br>
1. CD C:\FW\Edk2-ws and run setup script to setup `WORKSPACE` and Packages path
```
$ cd C:\FW\edk2-ws
$ Setenv.bat
$ cd edk2
```

The following are invoked in C:\FW\edk2-ws\edk2 directory

2. Install the pip requirements (Note, Proxy option needed behind a firewall) 
```
$ pip install --upgrade -r pip-requirements.txt --proxy http://proxy-chain.intel.com:911 
```
3. Get the code dependencies (done only when submodules change)
```
$ stuart_setup -c EmulatorPkg/PlatformCI/PlatformBuild.py TOOL_CHAIN_TAG=<Your TAG> -a X64
```
4. Update other dependencies (done on new VS Command Prompt)
```
$ stuart_update -c EmulatorPkg/PlatformCI/PlatformBuild.py TOOL_CHAIN_TAG=<Your TAG> -a X64
```
5. Build the BaseTools (done only when BaseTools change and first time)
```
$ python BaseTools\Edk2ToolsBuild.py -t <Your TAG>
```
6. Compile the EmulatorPkg
```
$ stuart_build -c EmulatorPkg/PlatformCI/PlatformBuild.py TOOL_CHAIN_TAG=<Your TAG> -a X64 BLD_*_ADD_SHELL_STRING=1 BLD_*_WORKSPACE=%WORKSPACE%

```
Where "`<Your TAG>`" is either `VS2017` or `VS2019`


---
## Slide 20  @title[Output from CI Stuart Build]
<b>Output from CI Stuart Build </b><br>

```
INFO - - Done -
INFO - Build end time: 13:17:07, Jul.27 2020
INFO - Build total time: 00:01:33
INFO -
INFO - ------------------------------------------------
INFO - --------------Cmd Output Finished---------------
INFO - --------- Running Time (mm:ss): 01:33 ----------
INFO - ----------- Return Code: 0x00000000 ------------
INFO - ------------------------------------------------
PROGRESS - Running Post Build
DEBUG - Plugin Success: Windows RC Path Support
DEBUG - Plugin Success: Windows Visual Studio Tool Chain Support
INFO - Writing BuildToolsReports to C:\FW\edk2-ws\edk2\Build\EmulatorX64\DEBUG_VS2017\BUILD_TOOLS_REPORT
DEBUG - Plugin Success: Build Tools Report Generator
PROGRESS - End time: 2020-07-27 13:17:07.515485  Total time Elapsed: 0:01:37
SECTION - Log file is located at: C:\FW\edk2-ws\edk2\Build\BUILDLOG_EmulatorPkg.txt
SECTION - Summary
PROGRESS - Success
```
### Finshed Build

---
## Slide 21  @title[Non CI Stuart Build  sub Section]
<br>

### Non Stuart CI Build EmulatorPkg

<b>SKIP</b> if doing Stuart CI Build


---
## Slide 22  @title[Build  Edk2 -build BaseTools]
<b>Non Stuart CI Build EDK II  </b><br>
 – build `BaseTools`
 
Open VS Command prompt & Cd to work space directory 
```
$> cd C:\FW\edk2-ws
```
Setup the local environment: (see batch file setenv.bat )
```
$> set WORKSPACE=%CD%
$> set PACKAGES_PATH=%WORKSPACE%\edk2;%WORKSPACE%\edk2-libc
```
Invoke Edksetup.bat from directory C:/FW/edk2-ws/edk2 to Build BaseTools 
```
$> cd edk2
$> edksetup.bat Rebuild
```

Building BaseTools only needs to be done once but setting up local environment and edksetup.bat needs to be done each new VS prompt session

<font color="Red"><B> SKIP</b></font> if doing Stuart CI Build with VS2017 or VS2018


---
## Slide 23  @title[Non Stuart CI Build Edk2 -update target.txt]
<b>Non Stuart CI Build EDK II  </b><br>
 - Update `Target.txt`

### <b>EmulatorPkg</b>
Invoke Edksetup.bat 
```
$> cd C:\FW\edk2-ws\edk2
$> edksetup.bat
```

### Edit the file `Conf/target.txt` (change TOOL_CHAIN_TAG)
```
 notepad Conf/target.txt 
```
Update the following define statements
```
    TARGET_ARCH           = X64
	. . .
    TOOL_CHAIN_TAG        = <see table below>
```

Use the following for TOOL_CHAIN_TAG:

| VS Version | TOOL_CHAIN_TAG |  |
| ---------- | ------------------ | - |
| 2015 | VS2015x86 | |
| 2017 | VS2017 | |
| 2019 | VS2019 | |
|  |  |  |

### Save and Exit

### Build EmulatorPkg
```
$> build -D ADD_SHELL_STRING -a X64
```


---
## Slide 24  @title[Possible Build Errors]
<b>Possible Build Errors </b>

1. If you get a BUILD Error:  Error "C:/Program " not found
   - First check that you have opened Visual Studio and installed the “C++”  
   - Open Visual Studio and create a "C++" project 
   - (This will take some time to install)
2.  If you get a BUILD Error: Check if  RC.Exe compiler not found is the error -<a href="https://gitpitch.com/tianocore-training/Platform_Build_Win_Emulator_Lab/master#/31" > here</a>
3.  If you get a BUILD Error: fatal error C1041: cannot open program database . . . Check <a href="https://gitpitch.com/tianocore-training/Platform_Build_Win_Emulator_Lab/master#/32"> here</a> 


---
## Slide 25  @title[Build Edk2 -build inside VS Prompt]
<b>Build EDK II  </b><br>
- Inside VS Prompt



Finished build

---
## Slide 26  @title[Run the Emulator sub Section]
<br>

### Run the  EmulatorPkg

---
## Slide 27  @title[Build Edk2 -invoke emulator]
<b>Invoke Emulation  </b>

<br>

From the command prompt
```
$>  RunEmulator.bat
```

<br>
<br>
OR<br>
run <font color="red">`WinHost.exe`</font> from: 

```
%WORKSPACE%/Build/EmulatorX64/DEBUG_VS20nn/X64
```
directory (where "nn" is the Visual Studio Version)

<br>
Notice 2 "GOP Window n" opened 




---
## Slide 28  @title[Build Edk2 -exit emulator]
<b>Emulator at Shell Prompt  </b>

Type: "Reset" to exit


---  
## Slide 29  @title[Summary]
### Summary <br>

- Pin Visual Studio Command Prompt to Windows Task Bar 
- Build a EDK II Platform using Emulator package
- Run the Emulator in Windows

---
## Slide 30  @title[Questions]
<br>
Questions

---
## Slide 31  @title[return to main]
<b>Return to Main Training Page</b>
<br>
<br>
<br>
<br>
<br>
Return to Training Table of contents for next presentation <a href="https://github.com/tianocore-training/Tianocore_Training_Contents/wiki#schedule--outline">link</a>



---
## Slide 32  @title[Logo Slide]
<br><br><br>



---
## Slide 33  @title[Acknowledgements]
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

Copyright (c) 2020, Intel Corporation. All rights reserved.
**/

```


---
## Slide 34  @title[Backup Section]
<br><br><br><br><br>
## Back up


---  
## Slide 35  @title[Build Errors]
<br>
<br>
<br>
<br>
##### Build Errors<br>


---
## Slide 36  @title[Build Error- RC.exe ]
<b>Build Error- RC.exe </b>
Because RC.Exe is not found, Error Message:

```
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
```
C:\Program Files (x86)\Windows Kits\8.1\bin\x64
```

Edit `Conf/tools_def.txt` 



---
## Slide 37  @title[Build Error- RC.exe 02]
<b>Build Error- RC.exe Cont...</b>

Edit `Conf/tools_def.txt` 
Search for your installation of Visual Studio (2013 or 2015)<br>
Update according to the path for where the RC.EXE is found 

```
# Microsoft Visual Studio 2013 Professional Edition
DEFINE WINSDK8_BIN       = c:\Program Files\Windows Kits\8.1\bin\x86\
DEFINE WINSDK8x86_BIN    = c:\Program Files (x86)\Windows Kits\8.1\bin\x64
 
# Microsoft Visual Studio 2015 Professional Edition
DEFINE WINSDK81_BIN       = c:\Program Files\Windows Kits\8.1\bin\x86\
DEFINE WINSDK81x86_BIN    = c:\Program Files (x86)\Windows Kits\8.1\bin\x64


```

### RC FIX for VS 2017 at command prompt type 

```
> Set
```
Then  See what the value of WINSDK10_PREFIX is.  This is probably the problem. 
Search your directory "C:\Program Files (x86)\" for the file rc.exe
The one you want will be in  C:\Program Files (x86)\Windows Kits\10\bin\x86
 
 
see below for the define: 
``` 
  From : DEFINE WINSDK10_BIN       = ENV(WINSDK10_PREFIX)DEF(VS2017_HOST)
  to : DEFINE WINSDK10_BIN       = C:\Program Files (x86)\Windows Kits\10\bin\x86
```

---
## Slide 38  @title[Build Error- C1041 ]
<b>Build Error: fatal error C1041: </b>
Build Error from fatal error C1041: cannot open program database

This Error is usually because the location you are building is being shared by another application in Windows.  Example: Syncplicity may cause this
<br>

Error Message:

```
k:\fw\edk2\MdePkg\Library\BaseLib\LinkedList.c : fatal error C1041: cannot open program 
database 
'k:\fw\edk2\build\nt32ia32\debug_vs2013x86\ia32\mdepkg\library\baselib\baselib\vc120.pdb'; if 
multiple CL.EXE write to the same .PDB file, please use /FS
NMAKE : fatal error U1077: '"C:\Program Files (x86)\Microsoft Visual Studio 
12.0\Vc\bin\cl.exe"' : return code '0x2'
Stop.

```

<b>Solution:</b>  Try using a Workspace that is not shared



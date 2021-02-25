## Slide 1  @title[Platform Build Win Up Xtreme Lab]

## UEFI & EDK II Training

Platform Build Lab Up Xtreme - Windows

<br>
<a href='http://www.tianocore.org'>tianocore.org</a>


<!---
 Lab_Guide.md for Platform Build Lab Up Xtreme - Windows

  Copyright (c) 2020, Intel Corporation. All rights reserved.<BR>

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
## Slide 2  @title[Platform Build Labs]
### Platform Build Labs
- Download Minplatform Using Git Bash 
- Build a EDK II Platform using Up Xtreme Aaeon board

---
## Slide 3  @title[Download MinPlatform]
### Download MinPlatform

---
## Slide 4  @title[EDK II Platform – Up Xtreme by Aaeon]
### EDK II Platform – Up Xtreme by Aaeon

---
## Slide 5  @title[Git Bash]
### Git Bash

- Open “Git Bash” 
- Linux like commands “/” for dirs.
- Use “/c” to go to C: in Windows, etc.
- Cd to the Work Space: 

```
$ cd /c/fw
$ mkdir UpX
$ cd UpX
```

---
## Slide 6  @title[Download the source for Edk II, MinPlatform and FSP]
### Download the source for Edk II, MinPlatform and FSP

In the Git Bash command line window Do the following:<br>

May need to Set PROXYS FIRST  (below is an example for Intel OR)
```
$ git config --global https.proxy=proxy.hf.intel.com:911
$ git config --global http.proxy=proxy.hf.intel.com:911

```

**Edk2** Download with tag
```
$ git clone https://github.com/tianocore/edk2.git
$ cd edk2
$ git checkout ea56ebf67dd55483105aa9f9996a48213e78337e
$ git submodule update --init
$ cd ..
```
**Edk2-platforms** Download with tag
```
$ git clone https://github.com/tianocore/edk2-platforms.git
$ cd edk2-platforms
$ git checkout  fcf693c9e8d68abbbc98d344720bc56a70ce7ba4
$ cd ..
```
**Edk2-non-osi**
```
$ git clone https://github.com/tianocore/edk2-non-osi.git
```
**FSP**
```
$ git clone https://github.com/Intel/FSP.git
```
---
## Slide 7  @title[Download MinPlatform Lab Material]
### Download MinPlatform Lab Material

Download the PlatformBuildLab_MinPlatform_FW.zip from :        [github.com PlatformBuildLab2_FW.zip](https://github.com/tianocore-training/PlatformBuildLab_MinPlatform_FW/archive/master.zip) <br>
OR <br>
Use git clone to download the PlatformBuildLab_MinPlatform_FW
```
$ git clone https://github.com/tianocore-training/PlatformBuildLab_MinPlatform_FW.git

Directory PlatformBuildLab_MinPlatform_FW will be created
/FW 
 /MinPlatformBuild
	- asl              - Asl Compiler               - Readme has download info
	- FTDI-Driver	   - Serial / USB cable         - Readme has download info
	- UpX_Lab          - Lab Material and Lab Guide
	- TeraTerm         - Terminal app               - Readme has download info
	- Nasm             - Nasm Assembler             - Readme has download info

```

---
## Slide 8  @title[Build Up Xtreme]
###  Build Up Xtreme

---
## Slide 9  @title[Where to get Open Source Up Xtreme]
###  Where to get Open Source Up Xtreme

How to Download  & Build: Open Source MinPlatform [Readme.md](https://github.com/tianocore/edk2-platforms/blob/master/Platform/Intel/Readme.md)

---
## Slide 10  @title[Preparing to Build]
### Get the ASL compiler

Directory C:\MinPlatformBuildLab_FW\FW\MinPlatformBuildLab from Download or zip 

or Download Asl compiler described in the Readme.txt (Windows)

Copy \asl Folder to C:\


---
## Slide 11  @title[Preparing to Build]
### Get the Nasm Assembler

Directory C:\MinPlatformBuildLab_FW\FW\MinPlatformBuildLab from Download or zip 

or Download Nasm Assembler described in the Readme.txt (Windows)

Copy \Nasm Folder to C:\



---
## Slide 12  @title[MinPlatform Open Board Tree Structure]
### MinPlatform Open Board Tree Structure


```
edk2/ https://github.com/tianocore/edk2
 . . .
edk2-platforms/ https://github.com/tianocore/edk2-platforms 
  Platform/
	Intel/
      BoardModulePkg
      WhiskeylakeOpenBoardPkg
        UpXtreme
      MinPlatformPkg
  Silicon/
	Intel/
     CoffeelakeSiliconPkg
. . .
  Features/Intel
      AdvancedFeaturePkg

edk2-non-osi/ https://github.com/tianocore/edk2-non-osi
  Silicon/
	Intel/
      CoffeelakeSiliconBinPkg

FSP/  https://github.com/IntelFsp/FSP
	CoffeelakeFspBinPkg

```




---
## Slide 13  @title[Open a VS Command Prompt]
### Open a VS Command Prompt

Follow Steps from [here](https://gitpitch.com/tianocore-training/Platform_Build_Win_Emulator_Lab/master#/) to Pin the Visual Studio Command Prompt to the Windows Task Bar




**Open a Visual Studio Command prompt**<br>
Cd to the Min Platform Build directory
```
$> cd \
$> cd Fw\UpX\edk2-platforms\Platform\Intel
```


---
## Slide 14  @title[Build Environment]
###  Build Environment
Check the python version
```
$> python --version
```

Make sure that the `PYTHON_HOME` is in the environment settings

If it is not, use the following (Windows) (Note for Python v 3.8.n)
```
set PYTHON_HOME=%USERPROFILE%\AppData\Local\Programs\Python\Python38-32
```


Check for platforms that are MinPlatform capable
```
$> python build_bios.py -l
```


---
## Slide 15  @title[Invoke the Build]
### Invoke the Build

**Build the Up Xtreme**

Invoke the Python Build script for Up Xtreme (example with VS2017)
```
$> python build_bios.py -p UpXtreme  -t VS2017
```
- note use "`-t VS2017`" or "`-t VS2019`" at end of next command  if VS is not VS2015

VS2015 is the default so the following will work
```
$> python build_bios.py -p UpXtreme
```
---
## Slide 16  @title[Platform Build Scripts]
### Platform Build Scripts
#### **Platform Config**


Many Platforms have a bash, bat  or Python script file to pre or post process the EDK II build process

For MinPlatform platform specific config
Build processing:
Build_config.cfg – Lists directories required for the build and build settings



Link to Up Xtreme [Build_config.cfg](https://github.com/tianocore/edk2-platforms/blob/master/Platform/Intel/WhiskeylakeOpenBoardPkg/UpXtreme/build_config.cfg)


---
## Slide 17  @title[Examine Build Parameters]
### Examine Build Parameters

The build command was the following Python script to preform the build

```
Python build_bios.py -p UpXtreme
```

The build script will call the following executable to perform the build

```
. . .
 Calling $  build -n 0 --log=Build.log --report-file=BuildReport.log
         and from UpX\conf\target.txt
```

| Variable | Value | Description |
| ---------- | ------------------ | ----------- |
| TARGET |      DEBUG|  Build Mode|
| TARGET_ARCH|  IA32  X64 |  CPU Architecture|
| TOOL_CHAIN_TAG |  VS2015| VS Tool Chain |
| ACTIVE_PLATFORM | \WhiskylakeOpenBoardPkg\ UpXtreme\OpenBoardPkg.dsc  | Platform DSC file |
| Repor file created(via Python script| BuildReport.log | PCDs, Libs, etc.|
| | | |



---
## Slide 18  @title[Platform Build and PCD Parameters]
### Platform Build and PCD Parameters
#### **Platform  Parameters**

Many Platform Parameters are defined in a top .DSC file that controls PCD and build switches

For Up Xtreme : `edk2-platforms\Platform\Intel\WhiskeylakeOpenBoardPkg\UpXtreme OpenBoardPkgPcd.dsc and OpenBoardPkgBuildOption.dsc`

Example:
``` 
 # Define Build Options both for EDK and EDKII drivers.

  DEFINE DSC_S3_BUILD_OPTIONS =
  DEFINE DSC_CSM_BUILD_OPTIONS =

!if gSiPkgTokenSpaceGuid.PcdAcpiEnable == TRUE
  DEFINE DSC_ACPI_BUILD_OPTIONS = -DACPI_SUPPORT=1
!else
  DEFINE DSC_ACPI_BUILD_OPTIONS =
!endif

  DEFINE BIOS_GUARD_BUILD_OPTIONS =
  DEFINE OVERCLOCKING_BUILD_OPTION =
```



---
## Slide 19  @title[Build Process for RELEASE Target]
### Build Process for RELEASE Target


Invoke the Python Build script for Up Xtreme for building a RELEASE version of the IFWI. 

Only the `-r` is added to the previous DEBUG build.

DEBUG Build is the default

```
$> python build_bios.py -p UpXtreme -r -t VS2017
```

Note: `-t VS2017`, use the correct VS Version




---
## Slide 20  @title[DEBUG & RELEASE Differences]
### DEBUG & RELEASE Differences

#### DEBUG build …
- Contains detailed debug strings that show the boot process, along with various ASSERT/TRACE errors
- Uses the serial port for debug string output
- Larger image than RELEASE, due to the embedded debug info
- Slower boot than RELEASE, due to the time it takes to display the debug info


#### RELEASE build …
- Does not contain the debug strings
- Does not use the serial port for debug output
- Smaller image than DEBUG
- Faster boot than DEBUG


---
## Slide 21  @title[Make a Change]
### Make a Change
#### Change the logo

In the Directory `C:\MinPlatformBuildLab_FW\FW\MinPlatformBuildLab\UpX_Lab`

There is a file: `Logo.bmp`

**OR**

Create a .BMP with Windows Paint and save to the name `Logo.bmp

Copy `Logo.bmp` to `C:\FW\UpX\edk2\MdeModulePkg\Logo` and overwrite existing file

See . . .` WhiskeylakeOpenBoardPkg\UpXtreme\OpenBoardPkg.fdf` Approx. line 285  where the logo file is directly included into the IFWI image.


---
## Slide 22  @title[Build with new logo]
### Build with new logo

Invoke the Python Build script for Up Xtreme for building a new Logo.bmp


```
$> python build_bios.py -p UpXtreme  -t VS2017
```

Note: `-t VS2017`, use the correct VS Version

---
## Slide 23  @title[Build Process Completed]
### Build Process Completed

Locate the build .fd images

Note that the script displays the location of the final .fd files

```
Done
Fd file can be found at C:\FW\UpX\Build\WhiskeylakeOpenBoardPkg\UpXtreme\DEBUG_VS2017\FV\UPXTREME.fd
```

This file is what will be used to flash into the Up Xtreme board.


After the build is complete use dediprog to flash into the Up Xtreme system

Note:  This lab does not have these details at this time.

To flash the .fd file on the Up Xtreme a tool similar to a Dediprog can be used to flash the image into the upper region of the flash device on the board.  

IT IS IMPORTANT not to overwrite the lower sections of the flash device since it contains the IFWI Flash layout descriptor and if it is overwriten the board will not execute ANY of the Firmware image.



---
## Slide 24  @title[Summary]
### Summary

- Download Minplatform Using Git Bash 
- Build a EDK II Platform using Up Xtreme Aaeon board


---
## Slide 25  @title[Questions]
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
## Slide 26  @title[Logo Slide]
<br><br><br>



---
## Slide 27  @title[Acknowledgements]
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



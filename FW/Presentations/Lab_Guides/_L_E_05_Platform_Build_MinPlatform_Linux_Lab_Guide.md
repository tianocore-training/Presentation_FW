## Slide 1   [Platform Build Win Up Xtreme Lab]

## UEFI & EDK II Training

Platform Build Lab Up Xtreme - Linux

<br>
<a href='http://www.tianocore.org'>tianocore.org</a>


<!---
 Lab_Guide.md for Platform Build Lab Up Xtreme - Linux

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
## Slide 2   [Platform Build Labs]
### Platform Build Labs
- Download Minplatform Using Git from Tianocore.org 
- Build a EDK II Platform using Up Xtreme Aaeon board

---
## Slide 3   [Download MinPlatform]
### Download MinPlatform

---
## Slide 4   [EDK II Platform – Up Xtreme by Aaeon]
### EDK II Platform – Up Xtreme by Aaeon

---
## Slide 5   [Linux setup for Up Xtreme Lab]
### Linux setup for Up Xtreme Lab


Lab Setup Requirements – Ubuntu 16.04
```
  bash$ sudo apt-get install build-essential uuid-dev iasl git gcc-5 nasm 
  bash$ sudo apt-get install screen
  bash$ sudo apt-get install gcab 
```
Lab Setup Requirements – Clear Linux* Project
```  
  bash$ sudo swupd bundle-add devpkg-util-linux
  bash$ sudo swupd bundle-add devpkg-gcab
```
Open Terminal Prompt.


Cd to the Workspace and create the Up Xtreme build directory "UpX"
```
  bash$ cd ~/src
  bash$ mkdir UpX
  bash$ cd UpX
```
---
## Slide 6   [Download the source for Edk II, MinPlatform and FSP]
### Download the source for Edk II, MinPlatform and FSP

From a terminal prompt at ~/src/Upx , do the following <BR>

May need to Set PROXYS FIRST  (below is an example for Intel OR)
```
  bash$ git config --global https.proxy=proxy.hf.intel.com:911
  bash$ git config --global http.proxy=proxy.hf.intel.com:911

```

**Edk2** Download with tag  edk2-stable202108
```
  bash$ git clone https://github.com/tianocore/edk2.git
  bash$ cd edk2
  bash$ git checkout 7b4a99be8a39c12d3a7fc4b8db9f0eab4ac688d5
  bash$ git submodule update --init
  bash$ cd ..
```
**Edk2-platforms** Download with tag  Aug 2021
```
  bash$ git clone https://github.com/tianocore/edk2-platforms.git
  bash$ cd edk2-platforms
  bash$ git checkout  40609743565da879078e6f91da76fc58a35ecaf7
  bash$ cd ..
```
**Edk2-non-osi**
```
  bash$ git clone https://github.com/tianocore/edk2-non-osi.git
```
**FSP**
```
  bash$ git clone https://github.com/Intel/FSP.git
```
---
## Slide 7   [Download MinPlatform Lab Material]
### Download MinPlatform Lab Material

Download the PlatformBuildLab_MinPlatform_FW.zip from :        [github.com PlatformBuildLab2_FW.zip](https://github.com/tianocore-training/PlatformBuildLab_MinPlatform_FW/archive/refs/heads/main.zip)
<br>
OR <br>
Use git clone to download the PlatformBuildLab_MinPlatform_FW
```
$ git clone https://github.com/tianocore-training/PlatformBuildLab_MinPlatform_FW.git

Directory PlatformBuildLab_MinPlatform_FW will be created
/FW 
 /MinPlatformBuild
	- UpX_Lab          - Lab Material 
     -  . . .	
```

---
## Slide 8   [Build Up Xtreme]
###  Build Up Xtreme

---
## Slide 9   [Where to get Open Source Up Xtreme]
###  Where to get Open Source Up Xtreme

How to Download  & Build: Open Source MinPlatform [Readme.md](https://github.com/tianocore/edk2-platforms/blob/master/Platform/Intel/Readme.md)


---
## Slide 10   [MinPlatform Open Board Tree Structure]
### MinPlatform Open Board Tree Structure


```
edk2/ https://github.com/tianocore/edk2
 . . .
edk2-platforms/ https://github.com/tianocore/edk2-platforms 
  Platform/
	Intel/    -----------------------------Invoke the build_bios.py form here
      BoardModulePkg
      WhiskeylakeOpenBoardPkg
        UpXtreme      -------------------- Platform DSC & FDF here
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
## Slide 11   [Open a VS Command Prompt]
### Open a VS Command Prompt


Open a Terminal Command Prompt
```
  bash$ cd ~/src/UpX/edk2
  bash$ source edksetup.sh
  bash$ cd ..
  bash$ cd edk2-platforms/Platform/Intel
```
Check if Python okay  (may also need to set PYTHON_HOME)
```
  bash$  python --version
  Python 3.8.2
```
Check for available MinPlatform Boards
``` 
 bash$ python build_bios.py -l
   
```


---
## Slide 12   [Invoke the Build]
### Invoke the Build

**Build the Up Xtreme**

Invoke the Python Build script for Up Xtreme
```
$> python build_bios.py -p UpXtreme  -t GCC5
```


---
## Slide 13   [Platform Build Scripts]
### Platform Build Scripts
#### **Platform Config**


Many Platforms have a bash, bat  or Python script file to pre or post process the EDK II build process

For MinPlatform platform specific config<br>

Build processing:<br>
- Build_config.cfg – Lists directories required for the build and build settings



Link to Up Xtreme [Build_config.cfg](https://github.com/tianocore/edk2-platforms/blob/master/Platform/Intel/WhiskeylakeOpenBoardPkg/UpXtreme/build_config.cfg)


---
## Slide 14   [Examine Build Parameters]
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
| TOOL_CHAIN_TAG |  GCC5| VS Tool Chain |
| ACTIVE_PLATFORM | /WhiskylakeOpenBoardPkg/ UpXtreme/OpenBoardPkg.dsc  | Platform DSC file |
| Repor file created(via Python script| BuildReport.log | PCDs, Libs, etc.|
| | | |



---
## Slide 15  [Platform Build and PCD Parameters]
### Platform Build and PCD Parameters
#### **Platform  Parameters**

Many Platform Parameters are defined in a top .DSC file that controls PCD and build switches

For Up Xtreme : `edk2-platforms/Platform/Intel/WhiskeylakeOpenBoardPkg/UpXtreme`<br>
&nbsp;&nbsp;`OpenBoardPkgPcd.dsc and OpenBoardPkgBuildOption.dsc`

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
## Slide 16   [Build Process for RELEASE Target]
### Build Process for RELEASE Target


Invoke the Python Build script for Up Xtreme for building a RELEASE version of the IFWI. 

Only the `-r` is added to the previous DEBUG build.

DEBUG Build is the default

```
$> python build_bios.py -p UpXtreme -r -t GCC
```





---
## Slide 17   [DEBUG & RELEASE Differences]
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
## Slide 18   [Make a Change]
### Make a Change
#### Change the logo

In the Directory `..~/MinPlatformBuildLab_FW/FW/MinPlatformBuildLab/UpX_Lab`

There is a file: `Logo.bmp`

**OR**

Create a .BMP with Windows Paint and save to the name `Logo.bmp

Copy `Logo.bmp` to `~/src/UpX/edk2/MdeModulePkg/Logo` and overwrite existing file

See . . .` WhiskeylakeOpenBoardPkg/UpXtreme/OpenBoardPkg.fdf` Approx. line 285  where the logo file is directly included into the IFWI image.


---
## Slide 19   [Build with new logo]
### Build with new logo

Invoke the Python Build script for Up Xtreme for building a new Logo.bmp


```
$> python build_bios.py -p UpXtreme  -t GCC5
```


---
## Slide 20   [Build Process Completed]
### Build Process Completed

Locate the build .fd images

Note that the script displays the location of the final .fd files

```
Done
Fd file can be found at ~/src/UpX/Build/WhiskeylakeOpenBoardPkg/UpXtreme/DEBUG_GCC5/FV/UPXTREME.fd

```

This file is what will be used to flash into the Up Xtreme board.


After the build is complete use dediprog to flash into the Up Xtreme system

Note:  This lab does not have these details at this time.

To flash the .fd file on the Up Xtreme a tool similar to a Dediprog can be used to flash the image into the upper region of the flash device on the board.  

IT IS IMPORTANT not to overwrite the lower sections of the flash device since it contains the IFWI Flash layout descriptor and if it is overwriten the board will not execute ANY of the Firmware image.



---
## Slide 24   [Summary]
### Summary

- Download Minplatform Using Git 
- Build a EDK II Platform using Up Xtreme Aaeon board


---
## Slide 25   [Questions]
<br>
Questions

---
## Slide 31   [return to main]
<b>Return to Main Training Page</b>
<br>
<br>
<br>
<br>
<br>
Return to Training Table of contents for next presentation <a href="https://github.com/tianocore-training/Tianocore_Training_Contents/wiki#schedule--outline">link</a>



---
## Slide 26   [Logo Slide]
<br><br><br>



---
## Slide 27   [Acknowledgements]
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



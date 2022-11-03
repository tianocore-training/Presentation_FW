
# How to Write a UEFI Driver Porting Lab - Simics 
# Linux

---
## Slide 1
# UEFI & EDK II Training

## How to Write a UEFI Driver Porting Lab - Simics & Linux

<!---
 LabGuide.md for UEFI Driver Porting Win Lab - Simics & Linux

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

-->

---  
## Slide 2   Lesson Objective
### Lesson Objective 

First Seup for Building EDK II, See 
[Lab Setup](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_L_C_01_Build_Setup_Download_EDK_II_Linux_LabGuide.md)
then 
[Platform Build Lab for Simics](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_L_C_02_Platform_Build_Linux_Simics_LabGuide.md)

- Compile a UEFI driver template created from UEFI Driver Wizard 
- Test driver w/ Simics QSP using UEFI Shell 
- Port code into the template driver 


---
## Slide 3   Lab 1: UEFI Driver Template
### Lab 1: UEFI Driver Template

Use this lab, if you’re not able to create a UEFI Driver Template using the UEFI Driver Wizard. 

**Skip** this lab if LAB 1 UEFI Driver Wizard completed successfully



---


## Slide 4   Lab 1: Get UEFI Driver Template

If  UEFI Driver Wizard does not work: 

**Copy** the directory `MyPkg` from    
    `. . ./FW/LabSampleCode/UefiDriverTemplate` to  `~/fw/edk2-ws/edk2 ` 



Review UEFI Driver Wizard Lab: [UEFI Driver Wizard Lab](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_L_E_01_UEFI_Driver_Wizard_Linux_LabGuide.md)
 for protocols produced and which are being consumed 

---
## Slide 5   Lab 2: Building a UEFI Driver

### Lab 2: Building a UEFI Driver


In this lab, you’ll build a UEFI Driver created by the UEFI Driver Wizard.<br>
You will include the driver in the EmulatorPkg project. <br>
Build the UEFI Driver from the Driver Wizard 

---

## Slide 6   Compile a UEFI Driver?
### Compile a UEFI Driver 


**Two Ways to Compile a Driver**
| Standalone | In a Project |  |
| ---------- | ------------------ | ---- |
| The build command directly compiles the .INF file | Include the .INF file in the project’s .DSC file | |
| Results:  The driver’s  .EFI file is located in the Build directory  |  Results:  The driver’s .EFI file is a part of the project in the Build directory  |    |
| | |




---
## Slide 7   Lab2: Build the UEFI Driver
### Lab 2: Build the UEFI Driver

Perform 
[Lab Setup](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_L_C_01_Build_Setup_Download_EDK_II_Linux_LabGuide.md)
and then 
[Platform Build Lab for Simics](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_L_C_02_Platform_Build_Linux_Simics_LabGuide.md)
from previous Labs

- **Open** `edk2-platforms/Platform/Intel/SimicsOpenBoardPkg/BoardX58Ich10/OpenBoardPkg.dsc`

- **Add** the following to the `[Components]` section: 
  - Hint: add to the last module in the `[Components]` section
```
# Add new modules here
   MyPkg/MyWizardDriver/MyWizardDriver.inf
```    
- **Save** and close the file OpenBoardPkg.dsc



---
## Slide 8   Lab2: Build the UEFI Driver -2
### Lab 2: Build the UEFI Driver
- Open another  Terminal  command prompt in `$HOME/fw/edk2-ws`

Then CD to edk2 to do edksetup.sh
```
$ cd ~/fw/edk2-ws/edk2
$ . edksetup.sh
```

Then CD to: 
```
$ cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel
```
Invoke the Python Build script for Simics OpenBoard QSP
```
$ python build_bios.py -p BoardX58Ich10  -t GCC5
```
Copy
  
`~/fw/edk2-ws/Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/FV/BOARDX58ICH10.fd`

**To**
 
`<SimicsInstallDir>/simics-qsp-x86-6.0.57/targets/qsp-x86/images`

- Build ERRORS: Copy the solution files from `/FW/LabSampleCode/LabSolutions/LessonC.1` to 
`~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver`




---
## Slide 9  Lab2: Copy UefiAppLab.vhd File

Copy the UefiAppLab.vhd

From:

`..../Lab_Material_FW/FW/LabSampleCode/AppLab/UefiAppLab.vhd ` 

**TO**

`<SimicsInstallDir>/simics-qsp-x86-6.0.57/targets/qsp-x86/images `



---
## Slide 10 Lab2: Update the Simics Script

Update the Simics script to use the UefiAppLab.vhd image as a file system
Edit the file: `gsp-modern-core.simics` from

`<SimicsInstallDir>/simics-qsp-cpu-6.0.4/targets/qsp-x86/qsp-modern-core.simics`


Add the following line:

```
$disk1_image="%simics%/targets/qsp-x86/images/UefiAppLab.vhd"
```


Before the `run-command-file` line

Comment if `$disk1_image` was added from a previous lab using "#" at the line beginning

Save `qsp-modern-core.simics`


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
## Slide 11 Lab2: Update UefiAppLab.vhd File

- Mount the UefiAppLab.vhd using Disk Manager:
[How to Mount VHD LINK](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_L_C_04_Writing_UEFI_App_Simics_Linux_LabGuide.md#slide-87-mounting-a-vhd-file-disk)


Copy & Paste `MyWizardDriver.efi`  to the VHD Disk

```bash
$ cp ~/fw/edk2-ws/Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_VS2015x86/X64/MyWizardDriver.efi ~/VHD
```



---
## Slide 12   Lab 2: Load Driver 



Run the `qsp-modern-core.simics` or `firststeps.simics` script from Simics Command Prompt :
```bash
$> ./simics target/qsp-x86/qsp-modern-core.simics 
simics> run
```

or
```bash
$> ./simics target/qsp-x86/firststeps.simics 
simics> run
```

Press "F2" at the logo then "`Boot Manager`" then "`EFI Internal Shell`"


At the Shell prompt
```
Shell> Fs1:
FS1:\> Load MyWizardDriver.efi
```


---
## Slide 13   Lab 2 Test Driver -drivers
### Lab 2: Test Driver -drivers

At the shell prompt Type: 
```
FS1:\> drivers
```

Verify the UEFI Shell loaded the new driver. 
The `drivers` command will display the driver information and a driver handle number ("FF" in the example screenshot )

It will be the last driver listed and have the name "MyWizardDriver"




---
## Slide 14   Lab 2: Test Driver -Dh
### <span class="gold" >Lab 2: Test Driver  


At the shell prompt using the handle from the `drivers` command, Type: 
```
FS1:\> dh -d ff
```

*Note:*  The value `ff` is the driver handle for `MyWizardDriver`.  The handle value may change based on your system configuration.(see example screenshot )


---
## Slide 15   Lab 2: Test Driver -unload
### Lab 2: Test Driver 

At the shell prompt using the handle from the `drivers` command, Type:
```
FS1:\> unload ff
```

See example screenshot
Type:&nbsp; `drivers` again

Notice results of `unload` command

Exit Simics

```
simics> stop
simics> quit
```


---
### End of Lab 2
---
## Slide 16   Lab 3: Component Name Section


### Lab 3: Component Name 

In this lab, you’ll change the information reported to the drivers command using the`ComponentName` and `ComponentName2` protocols.


---
## Slide 17   Lab 3: Component Name 
### Lab 3: Component Name 
<br>

- **Open** &nbsp;&nbsp;`~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver/ComponentName.c`
- **Change** &nbsp;&nbsp; the string returned by the driver from `MyWizardDriver` to: &nbsp;&nbsp;&nbsp; `UEFI Sample Driver`

   
```c++
  /// Table of driver names
  ///
 GLOBAL_REMOVE_IF_UNREFERENCED 
 EFI_UNICODE_STRING_TABLE mMyWizardDriverDriverNameTable[] = {
   { "eng;en", (CHAR16 *)L"UEFI Sample Driver" },
   { NULL, NULL }
 };
```

- **Save** and close the file: `~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver/ComponentName.c`  


Note:

The localization is using both the RFC 4646 and the ISO 639-2 Lanuage codes
as seen with the "`eng;en`"

---
## Slide 18   Lab 3 Build and Test Driver
### Lab 3: Build and Test Driver - Build the MyWizardDriver 


1. At the Terminal Command Prompt, Re-Build BoardX58ICH10

```bash
$> cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel
$> python build_bios.py -p BoardX58Ich10  -t GCC5
```

2. Copy MyWizardDriver.efi from the build directory to the VHD Disk

```bash
$> cp ~/fw/edk2-ws/Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/X64/MyWizardDriver.efi ~/VHD
```

3. Run the `qsp-modern-core.simics` or `firststeps.simics` script from the Simics Terminal Command Prompt

```bash
$> ./simics targets/qsp-x86/qsp-modern-core.simics
simics> run
```

Press "F2" at the logo then "`Boot Manager`" then "`EFI Internal Shell`"


4. At the UEFI Shell prompt Load the UEFI Driver from the shell

```
Shell> Fs1:
FS1:\> load MyWizardDriver.efi
```
5. Type Drivers  
```
FS1:\> Drivers
```

6. Exit Simics
```
simics> stop
simics> quit
```

Notice tha the Name has changed in the list of drivers

---
### Endo of Lab 3 


---
## Slide 19   Lab 4: Port Supported Section


### Lab 4: Porting the Supported & Start Functions 


The UEFI Driver Wizard produced a starting point for driver porting … so now what?<br><br>
In this lab, you’ll port the "Supported" and "Start" functions for the UEFI driver



---
## Slide 20   Lab 4: Port Supported-Start
### Lab 4: Porting Supported and Start

**Review the Driver Binding Protocol** 


- Port Supported() Determines if a driver supports a controller
   - to check for a specific protocol before returning ‘Success’
- Port Start() Starts a driver on a controller & Installs Protocols

   - to allocate a memory buffer and fill it with a specific value
- Stop() 
   - Stops a driver from managing a controller

---
## Slide 21   Lab 4: Supported Port
### Lab 4: The `Supported()` Port 

The UEFI Driver Wizard produced a `Supported()` function but it only returns `EFI_UNSUPPORTED`   

**Supported Goals:**

- Checks if the driver supports the device for the specified controller handle 
- Associates the driver with the Serial I/O protocol 
- Helps locate a protocol’s specific GUID through  UEFI Boot Services’ function 


---

## Slide 22   Lab 4: Help from Robust Libraries
### Lab 4: Help from Robust Libraries 
EDK II has libraries to help with porting UEFI Drivers <br>

- `AllocateZeroPool()` include - `[MemoryAllocationLib.h]`  
- `SetMem16()`   include - `[BaseMemoryLib.h]`  


<br>
Check the MdePkg with libraries help file (.chm format) 



---
## Slide 23   Lab 4: Update Supported 
### Lab 4: Update Supported 


**Open** &nbsp;&nbsp;`~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver/MyWizardDriver.c` 

**Locate** &nbsp;&nbsp; ` MyWizardDriverDriverBindingSupported()`, 
       the supported function for this driver and comment out the "`//`" in the line: "`return EFI_UNSUPPORTED;` " 

```c++
EFI_STATUS
EFIAPI
MyWizardDriverDriverBindingSupported (
  IN EFI_DRIVER_BINDING_PROTOCOL  *This,
  IN EFI_HANDLE                   ControllerHandle,
  IN EFI_DEVICE_PATH_PROTOCOL     *RemainingDevicePath OPTIONAL
  )
{
  // return EFI_UNSUPPORTED;
}
```

**copy** and paste (next slide)

Note: 

This code checks for a specific protocol before returning a status for the supported function (EFI_SUCCESS if the protocol GUID exists).

---
## Slide 24   Lab 4: Update Supported 02 
### Lab 4: Update Supported Add Code 
**Copy & Paste** &nbsp;&nbsp; the following code for the supported function `MyWizardDriverDriverBindingSupported()`:


```c++
	EFI_STATUS                Status;
	EFI_USB_IO_PROTOCOL           *UsbIo;


	Status = gBS->OpenProtocol(
		ControllerHandle,
		&gEfiUsbIoProtocolGuid,
		(VOID **)&UsbIo,
		This->DriverBindingHandle,
		ControllerHandle,
		EFI_OPEN_PROTOCOL_BY_DRIVER
		);

	if (EFI_ERROR(Status)) {
		return Status; // Bail out if OpenProtocol returns an error
	}


	// We're here because OpenProtocol was a success, so clean up
	gBS->CloseProtocol(
		ControllerHandle,
		&gEfiUsbIoProtocolGuid,
		This->DriverBindingHandle,
		ControllerHandle
		);

	return Status;

```

Note:
  end of copy & paste

---

## Slide 25   Lab 4: Notice UEFI Driver Wizard Includes  
### Lab 4: Notice UEFI Driver Wizard Includes

- **Open** &nbsp;&nbsp;`~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver/MyWizardDriver.h`
- **Notice** &nbsp;&nbsp; the following include statement is already added by the driver wizard: 

```c++
// Produced Protocols
//
#include <Protocol/UsbIo.h>

```

- **Review** &nbsp;&nbsp; the Libraries section and see that UEFI Driver Wizard automatically includes library headers based on the form information. Also other common library headers were included 

```c++

// Libraries
//
#include <Library/UefiBootServicesTableLib.h>
#include <Library/MemoryAllocationLib.h>
#include <Library/BaseMemoryLib.h>
#include <Library/BaseLib.h>
#include <Library/UefiLib.h>
#include <Library/DevicePathLib.h>
#include <Library/DebugLib.h>
```


Note that the UEFI Runtime Services Table is not listed but is also often used,  this will be added in a later lab
- **Close** &nbsp;&nbsp;`~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver/MyWizardDriver.h`
---

## Slide 26   Lab 4: Update the Start  
### Lab 4: Update the `Start()`  

**Copy & Paste** &nbsp;&nbsp; the following in  `MyWizardDriver.c` after the `#include "MyWizardDriver.h"` line

```c++
#define  DUMMY_SIZE 100*16		// Dummy buffer
CHAR16	*DummyBufferfromStart = NULL;

```

**Locate** &nbsp;&nbsp;` MyWizardDriverDriverBindingStart()`,  the start function for this driver and comment out the "`//`" in the line "`return EFI_UNSUPPORTED;` "

```c++
EFI_STATUS
EFIAPI
MyWizardDriverDriverBindingStart (
  IN EFI_DRIVER_BINDING_PROTOCOL  *This,
  IN EFI_HANDLE                   ControllerHandle,
  IN EFI_DEVICE_PATH_PROTOCOL     *RemainingDevicePath OPTIONAL
  )
{
  // return EFI_UNSUPPORTED;
}
```

---

## Slide 27   Lab 4: Update Start 02 
### Lab 4: Update Start Add Code 
**Copy & Paste** &nbsp;&nbsp; the following code for the start function ` MyWizardDriverDriverBindingStart()`:

```c++

    BOOLEAN FirstAlloc = FALSE;

    if (DummyBufferfromStart == NULL) {     // was buffer already allocated?
        DummyBufferfromStart = (CHAR16*)AllocateZeroPool (DUMMY_SIZE * sizeof(CHAR16));
	  FirstAlloc = TRUE;
    }
 
    if (DummyBufferfromStart == NULL) {
        return EFI_OUT_OF_RESOURCES;    // Exit if the buffer isn’t there
    } 

    if (FirstAlloc) { 
       SetMem16 (DummyBufferfromStart, (DUMMY_SIZE * sizeof(CHAR16)), 0x004A);  // Fill buffer
    }
 
    return EFI_SUCCESS;
```
- Notice the Library calls to `AllocateZeroPool()` and `SetMem16()`
- The `Start()` function is where there would be calls to "`gBS->InstallMultipleProtocolInterfaces()`"


Note:

- This code checks for an allocated memory buffer. If the buffer doesn’t exist, memory will be allocated and filled with an initial value (0x004A). 
- this lab's start does **not** do anything useful but if it did it would make calls to gBS->InstallMultipleProtocolInterfaces() to produce potocols and manage other handle devices

---

## Slide 28   Lab 4: Debugging before Testing the Driver 
### Lab 4: Debugging before Testing the Driver 

UEFI drivers can use the EDK II debug library 

- `DEBUG()`	 include - `[DebugLib.h]`<br>
- `DEBUG()` Macro statements can show status progress interest points throughout  the driver code


The Simics Serial Console Output  will show Debug Messages

---

## Slide 29   Lab 4: Add Debug statements supported 
### Lab 4: Add Debug Statements `Supported()` 
**Copy & Paste** &nbsp;&nbsp; the following  `DEBUG ()`  macros for the supported function:

```c++

	Status = gBS->OpenProtocol(
		ControllerHandle,
		&gEfiUsbIoProtocolGuid,
		(VOID **)&UsbIo,
		This->DriverBindingHandle,
		ControllerHandle,
		EFI_OPEN_PROTOCOL_BY_DRIVER
		);

	if (EFI_ERROR(Status)) {
		DEBUG((DEBUG_INFO, "[MyWizardDriver] Not Supported  \n"));
		return Status; // Bail out if OpenProtocol returns an error
	}


	// We're here because OpenProtocol was a success, so clean up
	gBS->CloseProtocol(
		ControllerHandle,
		&gEfiUsbIoProtocolGuid,
		This->DriverBindingHandle,
		ControllerHandle
		);
	DEBUG((DEBUG_INFO, "[MyWizardDriver] *** Supported SUCCESS  *** \n"));
	return Status;

```	




---
## Slide 30   Lab 4: Add Debug statements start 
### Lab 4: Add Debug Statements `Start()` 

**Copy & Paste** the following `DEBUG` macro for the Start function just after the `SetMem16` function call; 

```c++
if (FirstAlloc) {
		SetMem16(DummyBufferfromStart, (DUMMY_SIZE * sizeof(CHAR16)), 0x004A);  // Fill buffer with J's
		DEBUG((DEBUG_INFO, "\n*******\n***[MyWizardDriver] Buffer 0x%p  ***\n*******\n", DummyBufferfromStart));
  }
```	

*Note:* This debug macro displays the memory address of the allocated buffer on the debug console<br>


**Save**  &nbsp;&nbsp; `~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver/MyWizardDriver.c`


---

## Slide 31   Lab 4 Build and Test Driver
### Lab 4: Build and Test Driver 

Build the MyWizardDriver 

1. At the Terminal Command Prompt, Re-Build BoardX58ICH10

```bash
$> cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel/
$> python build_bios.py -p BoardX58Ich10  -t GCC5
```

2. Copy MyWizardDriver.efi  from the build directory to the VHD Disk

```bash
$> cp ~/fw/edk2-ws/Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/X64/MyWizardDriver.efi ~/VHD
```

3. Run the `qsp-modern-core.simics` or `firststeps.simics` script  from Simics Command Prompt

```bash
$> ./simics targets/qsp-x86/qsp-modern-core.simics
simics> run
```

Press "F2" at the logo then "`Boot Manager`" then "`EFI Internal Shell`"


4. At the UEFI Shell prompt

```
Shell> Fs1:
FS1:\> Load MyWizardDriver.efi 
```


---
## Slide 32   Lab 4 Build and Test Driver 02
### Lab 4: Build and Test Driver 

- Check the VS console output.
- Notice Debug messages indicate the driver did **not** return EFI_SUCCESS from the "`Supported()`" function **most** of the time.
```
[MyWizardDriver] Not Supported 
[MyWizardDriver] Not Supported 
[MyWizardDriver] Not Supported 
[MyWizardDriver] Not Supported 
. . .
```

- See that the "`Start()`" function did get called and a Buffer was allocated.
```
[MyWizardDriver] *** Supported SUCCESS ***

********
[MyWizardDriver] Buffer 0xDDF43018  
```


<br>
<br>

Exit Simics  
```
simics> stop 
simics> quit 
```

Note:

use the right-side scroll bar with mouse to scroll back to see the "Supported SUCCESS"

---
### End of Lab 4

---
## Slide 33  Lab 5: Create NV Var Section
<br>

### Lab 5: Create a NVRAM Variable 
<br>


- In this lab you’ll create a non-volatile UEFI variable (NVRAM), and set and get the variable in the `Start()` function 
- Use Runtime services to "`SetVariable()`" and "`GetVariable()`"


 
Note:

On systems without a serial port, the code from previous lab will not work since the Serial Protocol GUID does not exist. 

For Simics the USB is used to claim an added USB device being added.

With QEMU and the EmulatorPkg there is a serial device so the driver’s start function would then start to manage the serial port by creating child handles. 


---

## Slide 34   Lab 5: Create NV Var steps

### Lab 5: Adding a NVRAM Variable Steps  


1. Create .h file with new `typedef` definition and its own `GUID`
2. Include the new .h file in the driver's top .h file 
3. In the `Start()` make a call to a new function to set/get the new NVRam Variable
4. Before EntryPoint() add the new function `CreateNVVariable()` to the driver.c file
 

---

## Slide 35   Lab 5: Create a new .h file 
### Lab 5: Create a new .h file 
**Create** &nbsp;&nbsp; a new file in your editor called: "`MyWizardDriverNVDataStruc.h`"<br>

**Copy, Paste** and then **Save** this file in the `C:\FW\edk2-ws\edk2\MyPkg\MyWizardDriver` directory&nbsp;&nbsp;  

File:  `MyWizardDriverNVDataStruc.h`
```c++
#ifndef _MYWIZARDDRIVERNVDATASTRUC_H_
#define _MYWIZARDDRIVERNVDATASTRUC_H_
#include <Guid/HiiPlatformSetupFormset.h>
#include <Guid/HiiFormMapMethodGuid.h>

#define MYWIZARDDRIVER_VAR_GUID   { 0x363729f9, 0x35fc, 0x40a6,{ 0xaf, 0xc8, 0xe8, 0xf5, 0x49, 0x11, 0xf1, 0xd6} } 

#define CONFIGURATION_VARSTORE_ID    0x1234
#define MYWIZARDDRIVER_STRING_SIZE   0x1A

#pragma pack(1)
typedef struct {

    UINT16  MyWizardDriverStringData[MYWIZARDDRIVER_STRING_SIZE];
    UINT16  MyWizardDriverHexData;
    UINT8   MyWizardDriverBaseAddress;
    UINT8   MyWizardDriverChooseToEnable;
	CHAR16  *MyWizardDriverNvRamAddress;
} MYWIZARDDRIVER_CONFIGURATION;

#pragma pack()

#endif
```

Note:

- In order to set, retrieve, and use the UEFI variable, it requires the GUID reference that you just added.

- another Note:  For this lab, you were provided a GUID file.  You can also generate a GUID through the UEFI Driver Wizard or Guidgenerator.com.  But for the purposes of the lab, the one above can be used


---

## Slide 36   Lab 5: Add New Vars to .c 
### Lab 5: Update MyWizardDriver.c  

**Open** &nbsp;&nbsp; "`~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver/MyWizardDriver.c`"<br>

**Copy & Paste** &nbsp;&nbsp; the following  4 lines after the `#include "MyWizardDriver.h"` statement: 

```c++
#include "MyWizardDriver.h"

EFI_GUID   mMyWizardDriverVarGuid = MYWIZARDDRIVER_VAR_GUID;

CHAR16     mVariableName[] = L"MWD_NVData";  // Use Shell "Dmpstore" to see 
MYWIZARDDRIVER_CONFIGURATION   mMyWizDrv_Conf_buffer;
MYWIZARDDRIVER_CONFIGURATION   *mMyWizDrv_Conf = &mMyWizDrv_Conf_buffer;  //use the pointer 

```

---

## Slide 37   Lab 5: Update Suppport for new function 
### Lab 5: Update MyWizardDriver.c 

**Locate** &nbsp;&nbsp; "`MyWizardDriverDriverBindingStart ()`" function<br>

**Copy & Paste** &nbsp;&nbsp; the following line at the beginning of the start function to declare a local variable
```
  EFI_STATUS                Status;
```
**Copy & Paste** &nbsp;&nbsp; the below 6 lines just before the line "` } return EFI_SUPPORTED`": 1) new call to "`CreateNVVariable();`" 2-6) `if` statement with DEBUG and inside the “if (FirstAlloc)” as below:  

```C
	// Only init our NVRam and Data structures on first pass
	if (FirstAlloc) {
		SetMem16(DummyBufferfromStart, (DUMMY_SIZE * sizeof(CHAR16)), 0x004A);  // Fill buffer with J's
		DEBUG((DEBUG_INFO, "\n*******\n***[MyWizardDriver] Buffer 0x%p  ***\n*******\n", DummyBufferfromStart));

    //start  of code to add
    Status = CreateNVVariable();
	if (EFI_ERROR(Status)) {
		DEBUG((EFI_D_ERROR, "[MyWizardDriver] NV Variable already created \n"));
	}
	else {
		DEBUG((EFI_D_ERROR, "[MyWizardDriver] Created NV Variable in the Start \n"));
	}
	//end of code to add
  }
  return EFI_SUCCESS;
```

---

## Slide 38   Lab 5: Add new function 
### Lab 5: Update MyWizardDriver.c 
**Copy & Paste** &nbsp;&nbsp; the new function `CreateNVVariable()` before the call to function  `MyWizardDriverDriverEntryPoint()`



```c++
EFI_STATUS
EFIAPI
CreateNVVariable()
{
	EFI_STATUS            	Status;
	UINTN                  BufferSize;

	BufferSize = sizeof (MYWIZARDDRIVER_CONFIGURATION);
	Status = gRT->GetVariable(
		mVariableName,
		&mMyWizardDriverVarGuid,
		NULL,
		&BufferSize,
		mMyWizDrv_Conf
		);
	if (EFI_ERROR(Status)) {  // Not definded yet so add it to the NV Variables.
		if (Status == EFI_NOT_FOUND) {
			Status = gRT->SetVariable(
				mVariableName,
				&mMyWizardDriverVarGuid,
				EFI_VARIABLE_NON_VOLATILE | EFI_VARIABLE_BOOTSERVICE_ACCESS,
				sizeof (MYWIZARDDRIVER_CONFIGURATION),
				mMyWizDrv_Conf   //  buffer is is init before call
				);
			DEBUG((DEBUG_INFO, "***[MyWizardDriver] Variable %s created in NVRam Var\n", mVariableName));
			return EFI_SUCCESS;
		}
	}
	// already defined once so this time through unsupported
	return EFI_UNSUPPORTED;
}
```	  

- Note: the `gRT->GetVariable` and `gRT->SetVariable` use Runtime services table 
- The Runtime Services Table was not automatically included with the Driver Wizard
- Thus,  the Runtime Services Table will need to be included in MyWizardDriver/MyWizardDriver.h"

---

## Slide 39   Lab 5: Update .h
### Lab 5: Update MyWizardDriver.h
**Open** &nbsp;&nbsp; "`~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver/MyWizardDriver.h`"

**Copy & Paste** &nbsp;&nbsp; the following "`#include`" after the list of library include statements:

```C++
 // Libraries
 // . . .
 #include <Library/UefiRuntimeServicesTableLib.h>
```
<br>
**Copy & Paste** &nbsp;&nbsp; the following  "`#include`" after the list of protocol include statements: 

```C++
 // Produced Protocols
 // . . .
 #include "MyWizardDriverNVDataStruc.h"	
```

**Save** &nbsp;&nbsp; "`~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver/MyWizardDriver.h`"<br>

---

## Slide 40   Lab 5: - Improvements(1) MyWizardDriver.c
### Lab 5: - Improvements(1) MyWizardDriver.c

In Lab 4 every time the Supported function was called a Debug message was printed to the Serial port resulting in many messages to examine.  Instead, use a different Debug message type for the "Not Supported"  Debug message

In the `MyWizardDriverDriverBindingSupported`
function after the call to "`OpenProtocol`" fails use "`DEBUG_VERBOSE`" instead of "`DEBUG_INFO`"

This can be changed by setting the PCD message flag in the DSC file.
```
Status = gBS->OpenProtocol(  . . . 
// . . .
if (EFI_ERROR(Status)) {
   DEBUG((DEBUG_VERBOSE, "[MyWizardDriver] Not Supported  \n" ));
   return Status; // Bail out if OpenProtocol returns an error
}
```


---

## Slide 41   Lab 5: - Improvements(2) MyWizardDriver.c
### Lab 5: - Improvements(2) MyWizardDriver.c

It is hard to find the Buffer address in the Debug Message.

Before the call to `CreateNVVariable()` in the `MyWizardDriverDriverBindingStart()` Function add the following:
1. Store the address of the Dummy Buffer in the NVRAM Variable
2. Use "`StrCpyS`" to store the string: "`UEFI-Training-Class-MWD`" to the NVRAM Variable String

```c
	  // store the address and string value in the NvRam Variable  - this ALlows DMPSTORE to display our buffer address
	  mMyWizDrv_Conf_buffer.MyWizardDriverNvRamAddress = DummyBufferfromStart;
	  StrCpyS(mMyWizDrv_Conf_buffer.MyWizardDriverStringData, 
	         (MYWIZARDDRIVER_STRING_SIZE * sizeof(CHAR16)), 
	         L"UEFI-Training-Class-MWD");

	  // Create the NVRAM Variable MWD_NVData
	  Status = CreateNVVariable();
```


Save   "`~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver/MyWizardDriver.c`"




---

## Slide 42   Lab 5 Build and Test Driver
### Lab 5: Build and Test Driver

Build the MyWizardDriver 

1. At the Terminal Command Prompt, Re-Build BoardX58ICH10

```bash
$> cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel/
$> python build_bios.py -p BoardX58Ich10  -t GCC5
```

2. Copy MyWizardDriver.efi  from the build directory to the VHD Disk

```bash
cp ~/fw/edk2-ws/Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/X64/MyWizardDriver.efi ~/VHD
```

3. Run the `qsp-modern-core.simics` or `firststeps.simics` script from Simics Command Prompt

```bash
$> ./simics targets/qsp-x86/qsp-modern-core.simics
simics> run
```

Press "F2" at the logo then "`Boot Manager`" then "`EFI Internal Shell`"


4. At the UEFI Shell prompt

```
Shell> Fs1:
FS1:\> Load MyWizardDriver.efi 

```


---

## Slide 43   Lab 5 Verify the Output
### Lab 5: Verify the Output

Observe the Buffer address returned by the debug statement in the Simics Serial Console window and the new NV Variable was created

Also note, the "`[MyWizardDriver] Not Supported`" Messages are no longer displayed.

To display these, Set the `PcdDebugPrintErrorLevel|0x80400040`
In the DSC file

Note: use the right-side scroll bar with mouse to scroll back to see the "`Supported SUCCESS`"


---


## Slide 44   Lab 5 Verify Driver
### Lab 5: Verify Driver 


Use the Buffer address pointer in the previous slide then use the "`mem`" command


At the Shell prompt, use the `mem` command with the address of the buffer observed from the VS 
Command window then type&nbsp;&nbsp;  `FS1:\>  mem 0ddf43018` 

Observe the Buffer is filled with the letter "J" or 0x004A <br>

---

## Slide 45   Lab 5 Verify NVRAM Driver
### Lab 5: Verify NVRAM Created by Driver 


At the Shell prompt, type &nbsp;&nbsp; `FS1:\> dmpstore -all MWD_NVData`

Observe new the NVRAM variable "`MWD_NVData`" was created 
and filled with the address of the buffer and the string "`UEFI-Training-Class-MWD`"

Note the buffer address similar to:  00 00 DD F4 30 18 (doing little-endian swap) 

Exit Simics

```
simics> stop
simics> quit
```


---

## Slide 46   Lab 5 More Porting Needed for the Start
### Lab 5: More Porting Needed for the Start

At this point the MyWizardDriver does not manage anything.

The next steps would be to install a protocol to manage the Buffer and NVRAM variable.


- The Start function Install Protocols to Handle database 
- System firmware returns the Protocol Interface for the protocol that is then used to invoke the protocol specific service.
- The UEFI Driver keeps private, device specific context with the protocol interfaces.
    - Examples: Device Path, PCI I/O, Disk I/O, GOP, UNDI


---
### End of Lab 5



---
## Slide 47   Lab 6: Port Stop and Unload


### Lab 6: Port Stop and Unload 


In this lab, you’ll port the driver’s "Unload" and "Stop" functions to free any resources the driver allocated when it was loaded and started.

 
---
## Slide 48   Lab 6: Port the Unload
### Lab 6: Port the Unload function

**Open** &nbsp;&nbsp; "`~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver/MyPkg/MyWizardDriver.c`"


**Locate**&nbsp;&nbsp;"`MyWizardDriverUnload ()`" function<br>
**Copy & Paste**&nbsp;&nbsp;the following "`if`"  and "`DEBUG`" statements before the "`return EFI_SUCCESS;`" statement.

```C++

  // Do any additional cleanup that is required for this driver
  //
  if (DummyBufferfromStart != NULL) {
	  FreePool(DummyBufferfromStart);
	  DEBUG((EFI_D_INFO, "[MyWizardDriver] Unload, clear buffer\r\n"));
  }
  DEBUG((EFI_D_INFO, "[MyWizardDriver] Unload success\r\n"));

  return EFI_SUCCESS;
```

Note:

- The code will deallocate the buffer using the FreePool library function.
- Notice that the FreePool is called since there was an allocate call to get memory in the start function
- for the Unload similar "unwind" calls such as
  - free memory
  - Uninstall Protocol Interfaces
  - Disconnect Controller calls are made
  

---
## Slide 49   Lab 6: Port the Stop
### Lab 6: Port the Stop function 
**Locate** &nbsp;&nbsp;"`MyWizardDriverDriverBindingStop()`" function

**Comment out** &nbsp;&nbsp; with "`//`" before the "`return EFI_UNSUPPORTED;`" statement.

**Copy & Paste** &nbsp;&nbsp; the following "`if`"  and "`DEBUG`" statements in place of the "`return EFI_UNSUPPORTED;`" statement. 

```C++
	if (DummyBufferfromStart != NULL) {
		FreePool(DummyBufferfromStart);
		DEBUG((EFI_D_INFO, " MyWizardDriver] Stop, clear buffer\r\n"));
	}

	DEBUG((EFI_D_INFO, " MyWizardDriver] Stop, EFI_SUCCESS\r\n"));
	return EFI_SUCCESS;
  //  return EFI_UNSUPPORTED;
}
```

**Save & Close** &nbsp;&nbsp; "`MyWizardDriverDriver.c`"

Note:

- The code will deallocate the buffer using the FreePool library function.
- This a duplicate of the check performed in the unload function, in case the stop function was executed prior to unload.
- Notice that the FreePool is called since there was an allocate call to get memory in the start function
- for the stop similar "unwind" calls to mimic same functions in the start
  - free memory for Alocate memory
  - Uninstall Protocol Interfaces  for Install interfaces
  
    
---

## Slide 50   Lab 6 Build and Test Driver
### Lab 6: Build and Test Driver  

Build the MyWizardDriver 
1. At the Terminal Command Prompt, Re-Build BoardX58ICH10

```bash
$> cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel/
$> python build_bios.py -p BoardX58Ich10  -t GCC5
```

2. Copy MyWizardDriver.efi  from the build directory to the VHD Disk

```bash
cp ~/fw/edk2-ws/Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/X64/MyWizardDriver.efi ~/VHD
```

3. Run the `qsp-modern-core.simics` or `firststeps.simics` script  from Simics Terminal Command Prompt

```bash
$> ./simics targets/qsp-x86/qsp-modern-core.simics
simics> run
```

Press "F2" at the logo then "`Boot Manager`" then "`EFI Internal Shell`"

4. At the UEFI Shell prompt

```
Shell> Fs1:
FS1:\> Load MyWizardDriver.efi 

```
Observe the Buffer address is at `0xDDF43018` as this slide example<br>

---
## Slide 51   Lab 6 Verify Driver
### Lab 6: Verify Driver 


At the Shell prompt, type &nbsp; `FS1:\>  drivers`


Observe the handle is "`FF`" as this slide example 

Type: &nbsp;` mem  0x0xDDF43018 `

Observe the buffer was filled with the "0x004A"  or "J" 


---
## Slide 52   Lab 6 Verify Unload
### Lab 6: Verify Unload 


At the Shell prompt, type &nbsp; `FS0:\> unload FF`


Observe the DEBUG messages from the Unload

Type `Drivers` again to verify

---
## Slide 53   Lab 6 Verify Unload
### Lab 6: Verify Unload 


At the Shell prompt, type &nbsp; `FS1:\>  mem 0xDDF43018`


Observe the buffer is now NOT filled 


Exit Simics

```
simics> stop
simics> quit
```


---
### End of Lab 6

---


---
## Slide 54   Lab 7: Add Driver to the Platform

### Lab 7: Add Driver to the Platform

In this lab, you’ll add the My Wizard Driver to the Platform Build.



---
## Slide 55   Lab 7: Build the UEFI Driver
 
**Open**  `~/fw/edk2-ws/edk2-platforms/Platform/Intel/SimicsOpenBoardPkg/BoardX58Ich10/OpenBoardPkg.fdf'

**Add** MyWizardDriver to the following in section `[FV.DXEFV]` and after the Shell.inf: 
```
INF  ShellPkg/Application/Shell/Shell.inf
INF  MyPkg/MyWizardDriver/MyWizardDriver.inf
```
    
**Save** and **close** the file OpenBoardPkg.fdf

*Optional* -  Update file `~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver.uni` for `FORM_SET_TITLE` and `FORM1_TITLE`, strings , 
Then Save.  ( Be Creative)


**Build**  the Simics BoardX58Ich10 from the Terminal command prompt
```
$> cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel/
$> python build_bios.py -p BoardX58Ich10  -t GCC5
```
**Copy**

`~/fw/edk2-ws/Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/FV/BOARDX58ICH10.fd`

**To** 

`<SimicsInstallDir>/simics-qsp-x86-6.0.57/targets/qsp-x86/images`



---
## Slide 56   Lab 7: Verify Driver Got Installed

Run the `qsp-modern-core.simics` or `firststeps.simics` script  from Simics Command Prompt :
```
$> ./simics targets/qsp-x86/qsp-modern-core.simics
simics> run   
```

Press "F2" at the logo then "`Boot Manager`" then "`EFI Internal Shell`"

At the Shell prompt, type  `Shell> drivers`


Observe the handle is “98" as this slide example 



---
## Slide 57  Lab 7: Verify NVRAM Created by Driver

At the Shell prompt, type   `FS1:\> dmpstore -all MWD_NVData`

Observe new the NVRAM variable "`MWD_NVData`" was created and filled with the address of the buffer and the string "`UEFI-Training-Class-MWD`"




Buffer address is:  `00 00 DD FF 50 18` (doing little-endian swap) 

At the Shell prompt, type   `FS1:\> Mem DDFF5018` to Verify Buffer is still set to "J"s



---
## Slide 58  Lab 7: Verify Driver Form Menu in Setup

At the Shell prompt, type   `FS1:\> Exit`

This will exit back to setup, Then type "`Escape`", then Select "`Device Manager`" and then "`Sample Formset . . .`"

This is the Form for the `MyDriverWizard`


This can be updated to get user data for configuration of your driver that then gets stored in the `NVRAM MWD_NVRam` date

 
---
### End of Lab 7

---


## Slide 59   Additional Porting
### Additional Porting    


- Adding strings and forms to setup (HII)
- Publish & consume protocols
- Hardware initialization


Refer to the UEFI Drivers Writer’s Guide for more tips –
[PDF Link](https://tianocore-docs.github.io/edk2-UefiDriverWritersGuide/draft/edk2-UefiDriverWritersGuide-draft.pdf)
 

Note:
Use the UEFI Driver Wizard to create a starting point for new drivers on EDK II


---  
## Slide 60   Summary

### Summary 

- Compile a UEFI driver template created from UEFI Driver Wizard 
- Test driver w/ Simics QSP using UEFI Shell 
- Port code into the template driver 


---
## Slide 61   Questions



---
## Slide 62   Return to Training schedule main page


Return to Training schedule main page
https://github.com/tianocore-training/Tianocore_Training_Contents/wiki 

---

## Slide 63   Logo Slide




---
## Slide 64   Acknowledgements

```
Redistribution and use in source (original document form) and 'compiled‘ forms (converted to PDF, epub, HTML and other formats) with or without modification, are permitted provided that the following conditions are met:

Redistributions of source code (original document form) must retain the above copyright notice, this list of conditions and the following disclaimer as the first lines of this file unmodified.

Redistributions in compiled form (transformed to other DTDs, converted to PDF, epub, HTML and other formats) must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL TIANOCORE PROJECT BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,  BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

Copyright (c) 2022, Intel Corporation. All rights reserved.
```


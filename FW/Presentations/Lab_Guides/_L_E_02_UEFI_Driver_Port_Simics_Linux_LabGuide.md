# How to Write a UEFI Driver - Porting Lab
# Linux

---
## Slide 1 UEFI & EDK II Training
### How to Write a UEFI Driver - Porting Lab - Linux

[tianocore.org](https://www.tianocore.org/)

---
## Slide 2 Lesson Objective 

First Setup for Building EDK II, See [Lab Setup](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_C_01_Build_Setup_Download_EDK_II_Win_LabGuide.md) then TENTATIVE LINK

- Compile a UEFI driver template created from UEFI Driver Wizard
- Test driver w/ Simics QSP Board using UEFI Shell 2.0
- Port code in the template driver

---
## Slide 3 Lab 1: UEFI Driver Template

Use this lab, if you''re not able to create a UEFI Driver Template using the UEFI Driver Wizard.

**Note:** Skip if LAB 1 UEFI Driver Wizard completed successfully

---
## Slide 4 Lab 1: Get UEFI Driver Template

If UEFI Driver Wizard does not work:

1. Copy the directory MkPkg from `.../FW/LabSampleCode/UefiDriverTemplate` to `~/fw/edk2-ws/edk2`


(See PDF for screenshots)

Overwrite if SampleApp Lab Completed

Review [UEFI Driver Wizard Lab](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_E_01_UEFI_Driver_Wizard_WIN_LabGuide.md) for which protocols are being produced and which are being consumed

---
## Slide 5 Lab 2: Building a UEFI Driver

In this lab, you'll build a UEFI Driver created by the UEFI Driver Wizard. You will include the driver in the Emulator project. Build the UEFI Driver from the Driver Wizard.

---
## Slide 6 Compile a UEFI Driver

**Two Ways to Compile a Driver**


| Standalone | In a Project |
| --- | --- |
| The build command directly compiles the .INF file | Include the .INF file in the project's .DSC file |
| Results: The driver's .EFI file is located in the Build directory | Results: The driver's .EFI file is a part of the project in the Build directory |

---
##  Slide 7 Lab 2: Build the UEFI Driver

- Perform [Lab Setup - tentative link]() and platform (TENTATIVE LINK) from previous labs
- Open `edk2-platforms/Platform/Intel/SimicsOpenBoardPkg/BoardX58Ich10/OpenBoard.dsc`
- Add the following to the [Components] section:

Hint: add to the last module in the [Components] section

```
# Add new modules here
	MkPkg/MyWizardDriver/MyWizardDriver.inf
```

- Save and close the file OpenBoard.dsc


---
## Slide 8 Copy UefiAppLab.vhd file

Copy the UefiApplLab.vhd 

From `.../Lab_Material_FW/FW/LabSampleCode/AppLab/UefiAppLab.vhd` To `Simics-Install-Directory/simics-qsp-x86-6.0.57/targets/qsp-x86/images`

(See PDF for screenshots)

---
## Slide 9 Update the Simics Script

Update the Simics script to use the UefiAppLab.vhd image as a file system

Edit the file: qsp-modern-core.simics from `Simics-Install-Directory/simics-qsp-cpu-6.0.4/targets/qsp-modern-core.simics`

Add the following line:

```
$disk1_image="%simics%/targets/qsp-x86/images/UefiAppLab.vhd"
```

Before the "run-command-file" line

Save qsp-modern-core.simics

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
## Slide 10 Build Platform BoardX58Ich10

Open another Terminal Prompt in `$HOME/fw/edk2-ws` then cd to edk2 to do edksetup.sh

```bash
$ cd ~/fw/edk2-ws/edk2 
$ . edksetup.sh
```

Then cd to:

```bash
$ cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel
```

Invoke the Python Build script for Simics OpenBoard QSP

```bash
$ python build_bios.py -p BoardX58Ich10 -t GCC5
```

Copy `~/fw/edk2-ws/Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/FV/BOARDX58ICH10.fd` To `Simics-Install-Directory/simics-qsp-x86-6.0.57/targets/qsp-x86/images`

---
## Slide 11 Update UefiAppLab.vhd File

Mount the UefiAppLab.vhd using GuestMount: [How to - tentative link](tianocore.org)

Copy MyWizardDriver.efi to the VHD Disk

```bash
$ cp ~/fw/edk2-ws/Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_VS2015x86/X64/MyWizardDriver.efi ~/VHD
```

---
## Slide 12 Lab 2: Build and Test Driver

Run the firststeps script from Terminal Command Prompt:

```bash
$> ./simics targets/qsp-x86/qsp-modern-core.simics 
simics> run
```
Press "F2" at the logo, then Select "Boot Manager" followed by "EFI Internal Shell"

At the UEFI Shell prompt

```
Shell> Fs1:
FS1:/> Load MyWizardDriver.efi
```

```
Shell> FS1:
FS1:\> load MyWizardDriver.efi
Image 'FS1:\MyWizardDriver.efi' loaded at DDD3B000 - Success
FS1:\> 
```

Build ERRORS: Copy the solution files from `/FW/LabSampleCode/LabSolutions/LessonC.1` to `~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver`

---
## Slide 13 Lab 2: Test Driver

At the shell prompt type:

```
FS1:/> drivers
```

Verify the UEFI Shell loaded the new driver. The drivers command will display the driver information and a driver handle number ("ff" in the example screenshot)

(See PDF for screenshot)

---
## Slide 14 Lab 2: Test Driver

At the shell prompt using the handle from the drivers command, type:

```
dh -d ff
```

**Note:** The value ff is the driver handle for MyWizardDriver. The handle vlaue may change based on your system configuration. 

(See PDF for screenshot)

---
## Slide 15 Lab 2: Test Driver

At the shell prompt using the handle from the drivers command, type:

```
FS1:/ > unload ff
```

See example screenshot (in PDF), type: `drivers` again

Notice the results of unload command

Exit Simics

```
simics> stop
simics> quit
```

---
## Slide 16 Lab 3: Component Name

In this lab, you'll change the information reported to the drivers command using the ComponentName and ComponentName2 protocols.

---
## Slide 17 Lab 3: Component Name

- **Open** `~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver/ComponentName.c`

- **Change** the string returned by the driver from MyWizardDriver to: UEFI Sample Driver

```
/// Table of driver names
///
  GLOBAL_REMOVE_IF_UNREFERENCED
  EFI_UNICODE_STRING_TABLE mMyWizardDriverDriverNameTable[] = {
    { "eng;en", (CHAR16 *)L"UEFI Sample Driver" },
    { NULL, NULL }
  };
```

- **Save** and close the file: `~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver/ComponentName.c`

---
## Slide 18 Lab 3: Build and Test Driver

1. At the Terminal Command Prompt, Re-Build BoardX58Ich10

```bash
$> cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel/ 
$> python build_bios.py -p BoardX58Ich10 -t GCC5
```

2. Copy MyWizardDriver.efi from the build directory to the VHD Disk

```bash
$ cp ../Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/X64/MyWizardDriver.efi UefiAppLab
```

3. Run the firststeps script from Terminal Command Prompt:

```bash
$> ./simics targets/qsp-x86.qsp-modern-core.simics 
simics> run
```
4. At the Shell, Load Driver

```
Shell> fs1:
FS1:/> load MyWizardDriver.efi
```

5. Type Drivers

```
FS1:/> Drivers
```

(See PDF for results screenshot)

6. Exit Simics

```
simics> stop
simics> quit
```


---
## Slide 19 Lab 4: Porting the Supported & Start Functions

The UEFI Driver Wizard produced a starting point for driver porting ... so now what?

In this lab, you'll port the "Supported" and "Start" functions for the UEFI driver


---
## Slide 20 Lab 4: Porting Supported and Start

### Review the Driver Binding Protocol

- **Supported()** - Determines if a driver supports a controller
- **Start()** - Starts a driver on a controller & Installs Protocols
- **Stop()** - Stops a driver from managing a controller


---
## Slide 21 Lab 4: The Supported() Port

The UEFI Driver Wizard produced a Supported() function, but it only returns `EFI_UNSUPPORTED`

**Supported Goals:**

- Checks if the driver supports the device for the specified controller handle
- Associates the driver with the Serial I/O protocol
- Helps locate a protocol's specific GUID through UEFI Boot Services' function

---
## Slide 22 Lab 4: Help from Robust Libraries 

EDK II has libraries to help with porting UEFI Drivers

- `AllocateZeroPool()` include - \[Memory/AllocationLib.h\]
- `SetMem16()` include - \[BaseMemoryLib.h\]

Check the MdePkg with libraries help file (.chm format)

---
## Slide 23 Lab 4: Update Supported 

- **Open** `~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver/MyWizardDriver.c`
- **Locate** `MyWizardDriverDriverBindingSupported()`, the supported function for this driver and comment out the "//" in the line: "return EFI_UNSUPPORTED; "

```
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

- copy and paste (next slide)

---
## Slide 24 Lab 4: Update Supported Add Code

**Copy & Paste** the following code for the supported function

`MyWizardDriverDriverBindingSupported():`

```
EFI_STATUS                Status;
EFI_PCI_IO_PROTOCOL       *UsbIo;
Status = gBS->OpenProtocol (
                ControllerHandle,
                &gEfiUsbIoProtocolGuid,
		    (VOID **)&UsbIo,
                This->DriverBindingHandle,
                ControllerHandle,
                EFI_OPEN_PROTOCOL_BY_DRIVER EFI_OPEN_PROTOCOL_EXCLUSIVE
                );

if (EFI_ERROR (Status)) {
   return Status; // Bail out if OpenProtocol returns an error
}
 
  // We're here because OpenProtocol was a success, so clean up
   gBS->CloseProtocol (
      ControllerHandle,
      &gEfiUsbIoProtocolGuid,
      This->DriverBindingHandle,
      ControllerHandle
      );

   return EFI_SUCCESS;
```

---
## Slide 25 Lab 4: Notice UEFI Driver Wizard Includes

- **Open** `~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver/MyWizardDriver.h
`
- **Notice** the following include statement is already added by the driver wizard:

```
// Consumed Protocols
//
#include <Protocol/UsbIo.h>
```

- **Review** the Libraries section and see that UEFI Driver Wizard automatically includes library headers based on the form information. Also, other common library headers were included

```
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

---
## Slide 26 Lab 4: Update the Start()

- **Copy & Paste** the following in MyWizardDriver.c after the `#include "MyWizardDriver.h"` line:

```
#define  DUMMY_SIZE 100*16      // Dummy buffer
CHAR16   *DummyBufferfromStart = NULL;
```

**Locate** MyWizardDriverDriverBindingStart(), the start function for this driver and comment out the "//" in the line "`return EFI_UNSUPPORTED;`"

```
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
## Slide 27 Lab 4: Update Start Add Code

**Copy & Paste** the following code for the start function

`MyWizardDriverDriverBindingStart()`

```
BOOLEAN FirstAlloc = FALSE;

if (DummyBufferfromStart == NULL) {     // was buffer already allocated?
    DummyBufferfromStart = (CHAR16*)AllocateZeroPool (DUMMY_SIZE * sizeof(CHAR16));
    FirstAlloc = TRUE;
}
 
if (DummyBufferfromStart == NULL) {
    return EFI_OUT_OF_RESOURCES;    // Exit if the buffer is not there
} 

if (FirstAlloc) { 
   SetMem16 (DummyBufferfromStart, (DUMMY_SIZE * sizeof(CHAR16)), 0x0042);  // Fill buffer
)
 
return EFI_SUCCESS;
```

- Notice the Library calls to `AllocateZeroPool()` and `SetMem16()`
- The `Start()` function is where there would be calls to `gBS-InstallMultipleProtocolInterfaces()`

---
## Slide 28 Lab 4: Debugging before Testing the Driver

UEFI drivers can use the EDK II debug library

- `DEBUG()` inlcude - [DebugLib.h]

**DEBBUG()** Macro statements can show status progress interest points throughout the driver code

Simics Serial Console Output Debug Messages:

(See PDF for screenshot)

---
## Slide 29 Lab 4: Add Debug Statements Supported()

**Copy & Paste** the following `DEBUG()` macros for the supported function:

```
Status = gBS->OpenProtocol(
      ControllerHandle,
      &gEfiUsbIoProtocolGuid,
      (VOID **)&UsbIo,
      This->DriverBindingHandle,
      ControllerHandle,
      EFI_OPEN_PROTOCOL_BY_DRIVER | EFI_OPEN_PROTOCOL_EXCLUSIVE
      );
 
  if (EFI_ERROR(Status)) {
     DEBUG((DEBUG_INFO, "[MyWizardDriver] Not Supported /n"));
     return Status; // Bail out if OpenProtocol returns an error
  }
 
  // We're here because OpenProtocol was a success, so clean up
  gBS->CloseProtocol(
      ControllerHandle,
      &gEfiUsbIoProtocolGuid,
      This->DriverBindingHandle,
      ControllerHandle
      );
  DEBUG((DEBUG_INFO, "[MyWizardDriver] *** Supported SUCCESS ***/n"));
  return EFI_SUCCESS;
```

---
## Slide 30 Lab 4: Add Debig Statements Start()

**Copy & Paste** the following DEBUG macro for the Start function just before the `return EFI_SUCCESS;` statement

```c
DEBUG ((DEBUG_INFO, "/n*******/n***[MyWizardDriver] Buffer 0x%p  ***/n*******/n", DummyBufferfromStart));
return EFI_SUCCESS;
```

*Note:* This debug macro displays the memory address of the allocated buffer on the debug console

**Save** `~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver/MyWizardDriver.c`

---
## Slide 31 Lab 4: Build and Test Driver

1. At the Terminal Command Prompt, Re-Build BoardX58Ich10

```bash
$> cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel/
$> python build_bios.py -p BoardX58Ich10  -t GCC5
```

2. Copy `MyWizardDriver.efi` from the build directory to the `VHD Disk`

```bash
$> cp ../Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/X64/MyWizardDriver.efi ~/VHD
```

3. Run the firststeps script from Terminal Command Prompt:

```bash
$> ./simics targets/qsp-x86/qsp-modern-core.simics 
simics> run
```

4. At the Shell, Load Driver

```
Shell> fs1:
FS1:/> load MyWizardDriver.efi
```

```
Shell> fs1:
FS1:\> load MyWizardDriver.efi
Image 'FS1:\MyWizardDriver.efi' loaded at DDD3B000 - Success
FS1:\>
```

---
## Slide 32 Lab 4: Build and Test Driver

- Check the Simics Com[0] output.
- Notice Debug messages indicate the driver did not return EFI_SUCCESS from the "Supported()" function most of the time.
- See that the "Start()" function did get called and a Buffer was allocated.

(See PDF for screenshots)

*Note:* use the right-side scroll bar with mouse to scroll back to see the "Supported SUCCESS"

Exit Simics

```
simics> stop
simics> quit
```

---
## Slide 33 Lab 5: Create a NVRAM Variable

In this lab you'll create a non-volatile UEFI variable (NVRAM), and set and get the variable in the Start function

Use Runtime services to "`SetVariable()`" and "`GetVariable()`"

---
## Slide 34 Lab 5: Adding a NVRAM Variable Steps

1. Create .h file with new `typedef` definition and its own `GUID`
2. Include the new .h file in the driver's top .h file
3. In the `Start()` make a call to a new function to set/get the new NVRam Variable
4. Before `EntryPoint()` add the new function `CreateNVVariable()` to the driver.c file.

---
## Slide 35 Lab 5: Create a new .h file

**Create** a new file in your editor called: "`MyWizardDriverNVDataStruc.h`"

**Copy, Paste** and then **Save** this file

```
#ifndef _MYWIZARDDRIVERNVDATASTRUC_H_
#define _MYWIZARDDRIVERNVDATASTRUC_H_
#include <Guid/HiiPlatformSetupFormset.h>
#include <Guid/HiiFormMapMethodGuid.h>
 
#define MYWIZARDDRIVER_VAR_GUID /
  { /
    0x363729f9, 0x35fc, 0x40a6, 0xaf, 0xc8, 0xe8, 0xf5, 0x49, 0x11, 0xf1, 0xd6 /
  }
#define MYWIZARDDRIVER_STRING_SIZE   0x1A

#pragma pack(1)
typedef struct {
    UINT16  MyWizardDriverStringData[MYWIZARDDRIVER_STRING_SIZE];
    UINT8   MyWizardDriverHexData;
    UINT8   MyWizardDriverBaseAddress;
    UINT8   MyWizardDriverChooseToEnable;
    CHAR16  *MyWizardDriverNvRamAddress;
 } MYWIZARDDRIVER_CONFIGURATION;

#pragma pack()
#endif
```

---
## Slide 36 Lab 5: Update MyWizardDriver.c

**Open** `~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver/MyWizardDriver.c`

**Copy & Paste** the following 4 lines after the `#include "MyWizardDriver.h"` statement:

```
#include "MyWizardDriver.h"
 
EFI_GUID   mMyWizardDriverVarGuid = MYWIZARDDRIVER_VAR_GUID;
 
CHAR16     mVariableName[] =   L"MWD_NVData";  				// Use Shell "Dmpstore" to see 
MYWIZARDDRIVER_CONFIGURATION   mMyWizDrv_Conf_buffer;
MYWIZARDDRIVER_CONFIGURATION   *mMyWizDrv_Conf = &mMyWizDrv_Conf_buffer;  //use the pointer
```

---
## Slide 37 Lab 5: Update MyWizardDriver.c

**Locate** "`MyWizardDriverDriverBindingStart()`" function

**Copy & Paste** at the beginning of the start function to declare a local variable

```
EFI_STATUS        Status;  // Declare a local variable Status
```

**Copy & Paste** the 6 lines: 1) new call to "`CreateNVVariable();`", 2-6) `if` statement with DEBUG just before the line `return EFI_SUPPORTED` as below:

```
Status = CreateNVVariable();
if (EFI_ERROR(Status)) {
  DEBUG((DEBUG_INFO, "***[MyWizardDriver] NV Variable already created /n"));
}
else {
  DEBUG((DEBUG_INFO, "***[MyWizardDriver] Created NV Variable in the Start /n"));
}

return EFI_SUCCESS;
```

---
## Slide 38 Lab 5: Update MyWizardDriver.c

**Copy & Paste** the new function before the call to "`MyWizardDriverDriverEntryPoint()`"

- **Note:** the `gRT->GetVariable` and `gRT->SetVariable` use Runtime services table
- The Runtime Services Table was not automatically included with the Driver Wizard

```
EFI_STATUS
EFIAPI
CreateNVVariable()
{
    EFI_STATUS                Status;
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
                mMyWizDrv_Conf   //  buffer is init before call
                );
            DEBUG((DEBUG_INFO, “***[MyWizardDriver] Variable %s created in NVRam Var/n", mVariableName));
            return EFI_SUCCESS;
        }
    }
    // already defined once 
    return EFI_UNSUPPORTED;
}
```

---
## Slide 39 Lab 5: Update MyWizardDriver.h

**Open** "\~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver/MyWizardDriver.h"

**Copy & Paste** the following "`#include`" after the list of library include statements:

```
// Libraries
// . . .
#include <Library/UefiRuntimeServicesTableLib.h>
```

**Copy & Paste** the following "`#include`" after the list of protocol include statements:

```
// Produced Protocols
// . . .
#include "MyWizardDriverNVDataStruc.h"
```

**Save** "\~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver/MyWizardDriver.h"

---
## Slide 40 Lab 5 - Improvements(1) MyWizardDriver.c

In Lab 4 every time the Supported function was called a Debug message was printed to the Serial port resulting in many messages to examine. Instead, use a different Debug message type for the "Not Supported" Debug message

In the `MyWizardDriverDriverBindingSuppported` function after the call to "OpenProtocol" fails use "`DEBUG_VERBOSE`" instead of "`DEBUG_INFO`" This can be changed by setting the PCD message flag in the DSC file.

```
Status = gBS->OpenProtocol(  . . . 
// . . .
if (EFI_ERROR(Status)) {
   DEBUG((DEBUG_VERBOSE, "[MyWizardDriver] Not Supported  /n" ));
   return Status; // Bail out if OpenProtocol returns an error
}
```

---
## Slide 41 Lab 5 - Improvements(2) MyWizardDriver.c

It is hard to find the Buffer address in the Debug Message

Before the call to `CreateNVVariable()` in the `MyWizardDriverDriverBindingStart()` Function add the following:

1. Store the address of the Dummy Buffer in the NVRAM Variable
2. Use "`StrCpyS`" to store the string: `UEFI-Training-Class-MWD` to the NVRAM Variable String

```
//  store the address and string value in the NvRam Variable  - 
//  this Allows DMPSTORE to display our buffer address
  mMyWizDrv_Conf_buffer.MyWizardDriverNvRamAddress = DummyBufferfromStart;
  StrCpyS(mMyWizDrv_Conf_buffer.MyWizardDriverStringData, 
		(MYWIZARDDRIVER_STRING_SIZE * sizeof(CHAR16)), 
		L"UEFI-Training-Class-MWD");
```

**Save** "\~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver/MyWizardDriver.c"

---
## Slide 42 Lab 5: Build and Test Driver

1. At the Terminal Command Prompt, Re-Build BoardX58Ich10

```
$> cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel/
$> python build_bios.py -p BoardX58Ich10 -t GCC5
```

2. Copy `MyWizardDriver.efi` from the build directory to the `VHD Disk`

```
$> cp ../Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/X64/MyWizardDriver.efi ~/VHD
```

3. Run the firststeps script from Terminal Command Prompt

```
$> ./simics targets/qsp-x86/qsp-modern-core.simics
simics> run
```

4. At the Shell, Load Driver

```
Shell> fs1:
FS1:/> load MyWizardDriver.efi
```

```
Shell> fs1:
FS1:\> load MyWizardDriver.efi
Image 'FS1:\MyWizardDriver.efi' loaded at DDD3B000 - Success
FS1:\>
```

---
## Slide 43 Lab 5: Verify the Output

Observe the Buffer address returned by the debug statement in the Simics Serial Console window and the new NV Variable was created

Also note, the "\[MyWizardDriver\] Not Supported" Messages are no longer displayed.

To display these, Set the `PcdDebugPrintErrorLevel|0x8040004F` in the DSC file

(See PDF for screenshot)

**Note:** use the right-side scroll bar with mouse to scroll back to see the "Supported SUCCESS"


---
## Slide 44 Lab 5: Verify Driver

Use the Buffer address pointer in the previous slide then use the "mem" command

At the Shell prompt, type `FS0:/> mem 0ddf43018`

Observe the Buffer is filled with the letter "J" ir 0x004A

(See PDF for screenshot)

---
## Slide 45 Lab 5: Verify NVRAM Created by Driver

At the Shell prompt, type `FS1:/> dmpstore -all MWD_NVData`

Observe new the NVRAM variable "MWD_NVData" was created and filled with the address of the buffer and the string "UEFI-Training-Class-MWD"

(See PDF for screenshot)

Buffer address is: `00 00 DD F4 30 18`

Exit Simics

```
simics> stop
simics> quit
```

---
## Slide 46 Lab 5: More Porting Needed for the Start

At this point the MyWizardDriver does not manage anything.

The next steps would be to install a protocol to manage the Buffer and NVRAM variable.

---
## Slide 47 Lab 6: Port Stop and Unload

In this lab, you'll port the driver's "Unload" and "Stop" functions to free any resources the driver allocated when it was loaded and started.

---
## Slide 48 Lab 6: Port the Unload function

**Open** "`~/fw/edk2-ws/edk2/MyPkg/MyWizardDriver/MyWizardDriver.c`""

**Locate** "`MyWizardDriverUnload()`"" function

**Copy & Paste** the following "`if`" and "`DEBUG`" statements before the `return EFI_SUCCESS;` statement.

```
// Do any additional cleanup that is required for this driver
//
if (DummyBufferfromStart != NULL) {
    FreePool(DummyBufferfromStart);
    DEBUG((EFI_D_INFO, "[MyWizardDriver] Unload, clear buffer/n"));
}
DEBUG((DEBUG_INFO, "[MyWizardDriver] Unload success/n"));
 
return EFI_SUCCESS;
```

---
## Slide 49 Lab 6: Port the Stop function

**Locate** "`MyWizardDriverDriverBindingStop()`" function

**Comment out** with "//" before the `return EFI_UNSUPPORTED;` statement.

**Copy & Paste** the following "if" and "DEBUG" statements before the `return EFI_SUCCESS;` statement.

```
if (DummyBufferfromStart != NULL) {
      FreePool(DummyBufferfromStart);
      DEBUG((DEBUG_INFO, "[MyWizardDriver] Stop, clear buffer/n"));
  }
  DEBUG((DEBUG_INFO, "[MyWizardDriver] Stop, EFI_SUCCESS/n"));
 
  return EFI_SUCCESS;
//  return EFI_UNSUPPORTED;
}
```

**Save & Close** "MyWizardDriverDriver.c"


---
## Slide 50 Lab 6: Build and Test Driver

1. At the Terminal Command Prompt, Re-Buid BoardX58Ich10

```bash
$> cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel/
$> python build_bios.py -p BoardX58Ich10 -t GCC5 
```

2. Copy `MyWizardDriver.efi` from the build directory to the `VHD Disk`

```bash
$ cp ../Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/X64/MyWizardDriver.efi ~/VHD
```

3. Run the firststeps script from Terminal Command Prompt:

```bash
$> ./simics targets/qsp-x86/qsp-x86/qsp-modern-core.simics 
simics> run
```

4. At the Shell, Load Driver

```
Shell> fs1:
FS1:/> load MyWizardDriver.efi
```

(See PDF for screenshot)

Observe the Buffer address is at 0xDDF43018 as this slide example

---
## Slide 51 Lab 6: Verify Driver

At the Shell prompt, type `FS1:/> drivers`

(See PDF for screenshots)

Observe the handle is "FF" as this slide example

Type: `mem 0xDDF43018`

Observe the buffer was filled with the "0x004A" or "J"

(See PDF for screenshots)

---
## Slide 52 Lab 6: Verify Unload

At the Shell prompt, type `FS1:/> unload FF`

(See PDF for screenshots)

Observe the DEBUG messages from the Unload in the VS Command Window

Type `Drivers` again to verify

(See PDF for screenshots)


---
## Slide 53 Lab 6: Verify Unload

At the Shell prompt, type `FS1:/> mem 0xDDF43018`

Observe the buffer is now NOT filled

(See PDF for screenshots)

Exit Simics

```
simics> stop
simics> quit
```

---
## Slide 54 Lab 7: Add Driver to the Platform

In this lab, you'll add the My Wizard Driver to the Platform Build.

---
## Slide 55 Lab 7: Build the UEFI Driver

- Open `edk2-platforms/Platform/Intel/SimicsOpenBoardPkg/BoardX58Ich10/OpenBoard.fdf`
- Add the following in section \[FV.DXEFV\] and after the Shell.inf:

```
INF ShellPkg/Application/Shell/Shell.inf

INF MyPkg/MyWizardDriver/MyWizardDriver.inf
```

- Save and close the file OpenBoard.fdf
- Optional - Update file `C:/fw/edk2-ws/edk2/MyPkg/MyWizardDriver.uni` for `FORM_SET_TITLE` and `FORM1_TITLE`, strings, then save. (Be Creative)
- Open the Visual Studio command prompt
- Build the Simics BoardX58Ich10

```bash
$> cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel
$> python build_bios.py -p BoardX58Ich10 -t GCC5
```

Copy `~/fw/edk2-ws/Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/FV/BOARDX58ICH10.fd` To `Simics-Install-Directory/simics-qsp-x86-6.0.57/targets/qsp-x86/images`

---
## Slide 56 Lab 7: Verify Driver Got Installed

Run the firststeps script from Terminal Command Prompt:

```bash
$> ./simics targets/qsp-x86/qsp-modern-core.simics 
simics> run
```

At the Shell prompt, type `Shell> drivers`

(See PDF for screenshots)

Observe the handle is "98" as this slide example

---
## Slide 57 Lab 7: Verify NVRAM Created by Driver

At the Shell prompt, type `FS1:/> dmpstore -all MWD_NVData`

Observe new the NVRAM variable "`MWD_NVData`" was created and filled with the address of the buffer and the string "UEFI-Training-Class-MWD"

(See PDF for screenshot)

Buffer address is: `00 00 DD FF 50 18`

At the Shell prompt, type `FS1:/> Mem DDFF5018` to Verify Buffer is still set to "J"s


---
## Slide 58 Lab 7: Verify Driver Form Menu in Setup

At Shell prompt, type `FS1:/> Exit` 

This will exit back to setup. Then type "Escape" , then select "Device Manager" and then "Sample Formset..."

This is the Form for MyDriverWizard

This can be updated to get user data for configuration of your driver that then gets stored in the NVRAM MWD-NVRam date

---
## Slide 59 Additional Porting

**Adding strings and forms to setup (HII)**

**Install produced protocols**

**Hardware initialization**

Refer to the UEFI Drivers Writer's Guide for more tips - [PDF link](https://tianocore-docs.github.io/edk2-UefiDriverWritersGuide/draft/edk2-UefiDriverWritersGuide-draft.pdf)

---
## Slide 60 Summary

- Compile a UEFI driver template created from UEFI Driver Wizard
- Test driver w/ Simics QSP Board using UEFI Shell 2.0
- Port code into the template driver

---
## Slide 61 Questions?

<br>

---
## Slide 62 Return to Main Training Page

Return to the Training Table of Contents for next presentation [link](https://github.com/tianocore-training/Tianocore_Training_Contents/wiki)


---
## Slide 63 Logo Slide

<br>

---
## Slide 64 Acknowledgements

Redistribution and use in source (original document form) and 'compiled‘ forms (converted to PDF, epub, HTML and other formats) with or without modification, are permitted provided that the following conditions are met:

Redistributions of source code (original document form) must retain the above copyright notice, this list of conditions and the following disclaimer as the first lines of this file unmodified.

Redistributions in compiled form (transformed to other DTDs, converted to PDF, epub, HTML and other formats) must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL TIANOCORE PROJECT BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,  BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

Copyright (c) 2022, Intel Corporation. All rights reserved.

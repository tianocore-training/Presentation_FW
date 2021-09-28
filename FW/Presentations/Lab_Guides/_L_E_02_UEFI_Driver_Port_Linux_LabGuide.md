
## Slide 1
# UEFI & EDK II Training

## How to Write a UEFI Driver Lab - Linux

<!---
 LabGuide.md for UEFI Driver Porting Linux Lab

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
## Slide 2  [Lesson Objective]
### Lesson Objective 

- Compile a UEFI driver template created from UEFI Driver Wizard 
- Test driver in QEMU using UEFI Shell 2.0 
- Port code in  the template driver 

---
## Slide 3  [Lab 1: UEFI Driver Template]
### Lab 1: UEFI Driver Template

Use this lab, if you’re not able to create a UEFI Driver Template using the UEFI Driver Wizard. 

Skip if LAB 1 UEFI Driver Wizard completed successfully
---
## Slide 4  [Lab 1: Get UEFI Driver Template]

If  UEFI Driver Wizard does not work: 

Copy the directory `UefiDriverTemplate` from    
    . . ./FW/LabSampleCode/ to  C:/FW/edk2-ws/edk2 

Rename Directory `UefiDriverTemplate` to `MyWizardDriver`

Review UEFI Driver Wizard Lab:
https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_L_E_01_UEFI_Driver_Wizard_Linux_LabGuide.md
 for protocols produced and which are being consumed 

---
## Slide 5  [Lab 2: Building a UEFI Driver]

### Lab 2: Building a UEFI Driver
<br>

In this lab, you’ll build a UEFI Driver created by the UEFI Driver Wizard.<br>
You will include the driver in the OvmfPkg project. <br>
Build the UEFI Driver from the Driver Wizard 



## Slide 6  [Compile a UEFI Driver?]
### Compile a UEFI Driver 


<b>Two Ways to Compile a Driver</b>
| Standalone | In a Project |  |
| ---------- | ------------------ | ---- |
| The build command directly compiles the .INF file | Include the .INF file in the project’s .DSC file | |
| Results:  The driver’s  .EFI file is located in the Build directory  |  Results:  The driver’s .EFI file is a part of the project in the Build directory  |    |
| | |




---
## Slide 7  [Lab2: Build the UEFI Driver?]
### Lab 2: Build the UEFI Driver

<ul>
   <li>Perform <a href="https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_L_C_01_Platform_Build_OVMF-QEMU_Lab_guide.md">Lab Setup</a> from previous EmulatorPkg Labs </li>
   <li>Open `~src/edk2-ws/edk2/OvmfPkg/OvmfPkgX64.dsc`</li>
   <li>Add the following to the `[Components]` section:  <br>
   
   
   *Hint:*add to the last module in the `[Components]` section  </li>

```
    # Add new modules here
     MyWizardDriver/MyWizardDriver.inf{
      <LibraryClasses>    DebugLib|MdePkg/Library/UefiDebugLibConOut/UefiDebugLibConOut.inf
}

```

   <li>Save and close the file `~src/edk2-ws/edk2/OvmfPkg/OvmfPkgX64.dsc`  </li>
</ul>


---

## Slide 8  [Lab 2 Build and Test Driver]
### Lab 2: Build and Test Driver 1

Build MyWizardDriver – Cd to ~/src/edk2-ws/edk2 dir
```
  bash$ cd ~/src/edk2-ws/edk2
  bash$ . edksetup.sh
  bash$ build
```

- Build error Known issue from UEFI Driver Wizard: 
  -  `ComponentName.c` Line 148 col 74 needs "//" in front of "## TO_START" 
   
```
   bash$ build
```
Build ERRORS: Copy the solution files from 
`~/. . ./FW/LabSampleCode/LabSolutions/LessonC.1` to `~/src/edk2-ws/edk2/MyWizardDriver`

---

## Slide 9  [Lab 2 Build and Test Driver]
### Lab 2: Build and Test Driver 2

Copy MyWizardDriver.efi to hda-contents
```
 bash$ cd ~/run-ovmf/hda-contents
 bash$ cp ~/src/edk2-ws/Build/OvmfX64/DEBUG_GCC5/X64/MyWizardDriver.efi .
```
Test by Invoking Qemu
```
  bash$ cd ~/run-ovmf
  bash$ . RunQemu.sh
```
- Load the UEFI Driver from the shell<br>

- &nbsp;&nbsp;&nbsp; At the Shell prompt, type &nbsp; `Shell> fs0:`
- &nbsp;&nbsp;&nbsp; Type:&nbsp;  `FS0:\> `&nbsp;`load MyWizardDriver.efi`  


---
## Slide 10  [Lab 2 Test Driver Drivers]
### Lab 2: Test Driver 
<br>
At the shell prompt Type: `drivers`<br>

Verify the UEFI Shell loaded the new driver. 
The `drivers` command will display the driver information and a driver handle number ("a9" in the example screenshot )

It will be the last driver listed and have the name "MyWizardDriver"




---
## Slide 11  [Lab 2 Test Driver -Dh]
### <span class="gold" >Lab 2: Test Driver  
<br>
At the shell prompt using the handle from the `drivers` command, Type:&nbsp; `dh -d a9`


<i>Note:</i>  The value `a9` is the driver handle for 'MyWizardDriver'.  The handle value may change based on your system configuration.(see example screenshot - right)


---
## Slide 12  [Lab 2 Test Driver -unload]
### Lab 2: Test Driver 
<br>
At the shell prompt using the handle from the `drivers` command, Type:&nbsp; `unload a9`


See example screenshot - right<br>
Type:&nbsp; `drivers` again<br><br>
Notice results of `unload` command<br><br>

Exit QEMU

Note:


END of Lab 2


---
## Slide 13  [Lab 3: Component Name Section]
<br>

### Lab 3: Component Name

In this lab, you’ll change the information reported to the drivers command using the`ComponentName` and `ComponentName2` protocols.


---
## Slide 14  [Lab 3: Component Name ]
### Lab 3: Component Name
<br>

- <b>Open</b>&nbsp;&nbsp;`~/src/edk2-ws/edk2/MyWizardDriver/ComponentName.c`
- <b>Change</b>&nbsp;&nbsp; the string returned by the driver from `MyWizardDriver` to: &nbsp;&nbsp;&nbsp; `UEFI Sample Driver`

   
```c++
  /// Table of driver names
  ///
 GLOBAL_REMOVE_IF_UNREFERENCED 
 EFI_UNICODE_STRING_TABLE mMyWizardDriverDriverNameTable[] = {
   { "eng;en", (CHAR16 *)L"UEFI Sample Driver" },
   { NULL, NULL }
 };
```

- <b>Save</b> and close the file: `~/src/edk2-ws/edk2/MyWizardDriver/ComponentName.c`  


Note:

The localization is using both the RFC 4646 and the ISO 639-2 Lanuage codes
as seen with the "`eng;en`"

---
## Slide 15 [Lab 3 Build and Test Driver]
### Lab 3: Build and Test Driver 1
<br>
Build the MyWizardDriver 
```
bash$ cd ~/src/edk2-ws/edk2
bash$ build
```
Copy MyWizardDriver.efi to hda-contents
```
 bash$ cd ~/run-ovmf/hda-contents
 bash$ cp ~/src/edk2-ws/Build/OvmfX64/DEBUG_GCC5/X64/MyWizardDriver.efi .
```
Test by Invoking Qemu
```
  bash$ cd ~/run-ovmf
  bash$ . RunQemu.sh
```

---
## Slide 16 [Lab 3 Build and Test Driver]
### Lab 3: Build and Test Driver 2
<br>

- Load the UEFI Driver from the shell<br>

- &nbsp;&nbsp;&nbsp; At the Shell prompt, type &nbsp; `Shell> fs0:`
- &nbsp;&nbsp;&nbsp; Type:&nbsp;  `FS0:\> `&nbsp;`load MyWizardDriver.efi`  

Type:&nbsp; `drivers`<br>
Observe the change in the string that the driver returned <br>
<br>
Exit QEMU


Note: 
Lab 3 finished


---
## Slide 17  [Lab 4: Port Supported Section]
<br>

### Lab 4: Porting the Supported & Start Functions 
<br>
The UEFI Driver Wizard produced a starting point for driver porting … so now what?<br><br>
In this lab, you’ll port the "Supported" and "Start" functions for the UEFI driver



---
## Slide 18  [Lab 4: Port Supported-Start]
### Lab 4: Porting Supported and Start
<span style="font-size:01.1em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>Review the Driver Binding Protocol</b> 


- Port Supported() to check for a specific protocol before returning ‘Success’
- Port Start() to allocate a memory buffer and fill it with a specific value
- Stop() Stops a driver from managing a controller

---
## Slide 19  [Lab 4: Supported Port]
### Lab 4: The `Supported()` Port 
The UEFI Driver Wizard produced a `Supported()` function but it only returns `EFI_UNSUPPORTED`   

<Font color="Green"><b>Supported Goals: </b></font> 

- Checks if the driver supports the device for the specified controller handle 
- Associates the driver with the Serial I/O protocol 
- Helps locate a protocol’s specific GUID through  UEFI Boot Services’ function 


---

## Slide 20  [Lab 4: Help from Robust Libraries]
### Lab 4: Help from Robust Libraries 
EDK II has libraries to help with porting UEFI Drivers <br>

- `AllocateZeroPool()` include - `[MemoryAllocationLib.h]`  
- `SetMem16()`   include - `[BaseMemoryLib.h]`  
</ul>
<br>
<br>
<br>
Check the MdePkg with libraries help file (.chm format) 



---
## Slide 21  [Lab 4: Update Supported ]
### Lab 4: Update Supported


<b>Open</b>&nbsp;&nbsp;`~/src/edk2-ws/edk2/MyWizardDriver/MyWizardDriver.c` <br>
<b>Locate</b>&nbsp;&nbsp; ` MyWizardDriverDriverBindingSupported()`, 
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

<b>copy</b> and past (next slide)

Note: 

This code checks for a specific protocol before returning a status for the supported function (EFI_SUCCESS if the protocol GUID exists).

---
## Slide 22  [Lab 4: Update Supported 02 ]
### Lab 4: Update Supported Add Code 
<b>Copy & Paste</b>&nbsp;&nbsp; the following code for the supported function `MyWizardDriverDriverBindingSupported()`:



```c++
  EFI_STATUS Status;
  EFI_SERIAL_IO_PROTOCOL *SerialIo;
  Status = gBS->OpenProtocol (
                  ControllerHandle,
                  &gEfiSerialIoProtocolGuid,
                  (VOID **) &SerialIo,
                  This->DriverBindingHandle,
                  ControllerHandle,
                  EFI_OPEN_PROTOCOL_BY_DRIVER | EFI_OPEN_PROTOCOL_EXCLUSIVE
                  );

  if (EFI_ERROR (Status)) {
	 return Status; // Bail out if OpenProtocol returns an error
  }

    // We're here because OpenProtocol was a success, so clean up
     gBS->CloseProtocol (
        ControllerHandle,
        &gEfiSerialIoProtocolGuid,
        This->DriverBindingHandle,
        ControllerHandle
        );
  	
     return EFI_SUCCESS; 
	 
```

Note:
  end of copy & paste

---

## Slide 23  [Lab 4: Notice UEFI Driver Wizard Includes  ]
### Lab 4: Notice UEFI Driver Wizard Includes

- <b>Open</b>&nbsp;&nbsp;`~/src/edk2-ws/edk2/MyWizardDriver/MyWizardDriver.h`
- <b>Notice</b>&nbsp;&nbsp; the following include statement is already added by the driver wizard: 

```c++
// Produced Protocols
//
#include <Protocol/SerialIo.h>

```

- <b>Review</b>&nbsp;&nbsp; the Libraries section and see that UEFI Driver Wizard automatically includes library headers based on the form information. Also other common library headers were included 

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

- <b>Close</b>&nbsp;&nbsp;`~/src/edk2-ws/edk2/MyWizardDriver/MyWizardDriver.h`

---

## Slide 24  [Lab 4: Update the Start  ]
### Lab 4: Update the `Start()`  

<b>Copy & Paste</b>&nbsp;&nbsp; the following in  `MyWizardDriver.c` after the <br>`#include "MyWizardDriver.h"` line

```c++
#define  DUMMY_SIZE 100*16		// Dummy buffer
CHAR16	*DummyBufferfromStart = NULL;

```

<b>Locate</b>&nbsp;&nbsp;` MyWizardDriverDriverBindingStart()`,  the start function for this driver and comment out the "`//`" in the line "`return EFI_UNSUPPORTED;` "

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

## Slide 25  [Lab 4: Update Start 02 ]
### Lab 4: Update Start Add Code 
<b>Copy & Paste</b>&nbsp;&nbsp; the following code for the start function ` MyWizardDriverDriverBindingStart()`:

```c++
	if (DummyBufferfromStart == NULL) {     // was buffer already allocated?
		DummyBufferfromStart = (CHAR16*)AllocateZeroPool (DUMMY_SIZE * sizeof(CHAR16));
	}

	if (DummyBufferfromStart == NULL) {
		return EFI_OUT_OF_RESOURCES;    // Exit if the buffer isn’t there
	}

	SetMem16 (DummyBufferfromStart, (DUMMY_SIZE * sizeof(CHAR16)), 0x0042);  // Fill buffer

	return EFI_SUCCESS;
```
- Notice the Library calls to `AllocateZeroPool()` and `SetMem16()`
- The `Start()` function is where there would be calls to "`gBS->InstallMultipleProtocolInterfaces()`"


Note:

- This code checks for an allocated memory buffer. If the buffer doesn’t exist, memory will be allocated and filled with an initial value (0x0042). 
- this lab's start does **not** do anything useful but if it did it would make calls to gBS->InstallMultipleProtocolInterfaces() to produce potocols and manage other handle devices

---

## Slide 26  [Lab 4: Debugging before Testing the Driver ]
### Lab 4: Debugging before Testing the Driver 
<br>
UEFI drivers can use the EDK II debug library 

- `DEBUG()`	 include - `[DebugLib.h]`<br>
- `DEBUG()` Macro statements can show status progress interest points throughout  the driver code


## Slide 27  [Lab 4: Add Debug statements supported ]
### Lab 4: Add Debug Statements `Supported()` 
<b>Copy & Paste</b>&nbsp;&nbsp; the following  `DEBUG ()`  macros for the supported function:

```c++
	Status = gBS->OpenProtocol(
		ControllerHandle,
		&gEfiSerialIoProtocolGuid,
		(VOID **)&SerialIo,
		This->DriverBindingHandle,
		ControllerHandle,
		EFI_OPEN_PROTOCOL_BY_DRIVER | EFI_OPEN_PROTOCOL_EXCLUSIVE
		);

	if (EFI_ERROR(Status)) {
	   DEBUG((EFI_D_INFO, "[MyWizardDriver] Not Supported \r\n"));  // Copy this line
	   return Status; // Bail out if OpenProtocol returns an error
	}
	
	// We're here because OpenProtocol was a success, so clean up
	gBS->CloseProtocol(
		ControllerHandle,
		&gEfiSerialIoProtocolGuid,
		This->DriverBindingHandle,
		ControllerHandle
		);
	DEBUG((EFI_D_INFO, "[MyWizardDriver] Supported SUCCESS\r\n")); // Copy this line
	return EFI_SUCCESS;

```	




---
## Slide 28  [Lab 4: Add Debug statements start ]
### Lab 4: Add Debug Statements `Start()` 
<br>
<b>Copy & Paste</b>&nbsp;&nbsp; the following `DEBUG` macro for the Start function just before the `return EFI_SUCCESS;` statement

```c++
  DEBUG ((EFI_D_INFO, "\r\n***\r\n[MyWizardDriver] Buffer 0x%p\r\n", DummyBufferfromStart));
  return EFI_SUCCESS;
```	

<i>Note: </i>This debug macro displays the memory address of the allocated buffer on the debug console<br>
<br>

<b>Save</b>&nbsp;&nbsp; `~/src/edk2-ws/edk2/MyWizardDriver/MyWizardDriver.c`


---

## Slide 29  [Lab 4 Build and Test Driver]
### Lab 4: Build and Test Driver 1
<br>
Build the MyWizardDriver 



```
bash$ cd ~/src/edk2-ws/edk2
bash$ build
```
Copy MyWizardDriver.efi to hda-contents
```
 bash$ cd ~/run-ovmf/hda-contents
 bash$ cp ~/src/edk2-ws/Build/OvmfX64/DEBUG_GCC5/X64/MyWizardDriver.efi .
```
Test by Invoking Qemu
```
  bash$ cd ~/run-ovmf
  bash$ . RunQemu.sh
```

---
## Slide 30 [Lab 4 Build and Test Driver 02]
### Lab 4: Build and Test Driver 2
<br>

- Load the UEFI Driver from the shell<br>

- &nbsp;&nbsp;&nbsp; At the Shell prompt, type &nbsp; `Shell> fs0:`
- &nbsp;&nbsp;&nbsp; Type:&nbsp;  `FS0:\> `&nbsp;`load MyWizardDriver.efi`  


---
## Slide 31  [Lab 4 Build and Test Driver 03]
### Lab 4: Build and Test Driver 3

- Check the QEMU console output.
- Notice Debug messages indicate the driver did not return EFI_SUCCESS from the "`Supported()`" function <b>most</b> of the time.
- See that the "`Start()`" function did get called and a Buffer was allocated.
<br>
<br>
Exit QEMU

Note:

use the right-side scroll bar with mouse to scroll back to see the "Supported SUCCESS"


Finished Lab 4

---
## Slide 32  [Lab 5: Create NV Var Section]
<br>

### Lab 5: Create a NVRAM Variable 
<br>


- In this lab you’ll create a non-volatile UEFI variable (NVRAM), and set and get the variable in the `Start()` function 
- Use Runtime services to "`SetVariable()`" and "`GetVariable()`"


 
Note:

On systems without a serial port, the code from previous lab will not work since the Serial Protocol GUID does not exist. 

With QEMU and the EmulatorPkg there is a serial device so the driver’s start function would then start to manage the serial port by creating child handles. 


---

## Slide 33  [Lab 5: Create NV Var steps]

### Lab 5: Adding a NVRAM Variable Steps  
<br>

 1. Create .h file with new `typedef` definition and its own `GUID`
 2. Include the new .h file in the driver's top .h file 
 3. `Supported()` make a call to a new function to set/get the new NVRam Variable  
 4. Before `EntryPoint()` add the new function `CreateNVVariable()` to the driver.c file.



---

## Slide 34  [Lab 5: Create a new .h file ]
### Lab 5: Create a new .h file 
<b>Create</b>&nbsp;&nbsp; a new file in your editor called: "`MyWizardDriverNVDataStruc.h`"<br>

<b>Copy, Paste</b> and then <b>Save</b> this file in the `~src/edk2-ws/edk2/MyWizardDriver` directory&nbsp;&nbsp;  

<hr>

```c++
#ifndef _MYWIZARDDRIVERNVDATASTRUC_H_
#define _MYWIZARDDRIVERNVDATASTRUC_H_
#include <Guid/HiiPlatformSetupFormset.h>
#include <Guid/HiiFormMapMethodGuid.h>

#define MYWIZARDDRIVER_VAR_GUID \
  { \
	0x363729f9, 0x35fc, 0x40a6, {0xaf, 0xc8, 0xe8, 0xf5, 0x49, 0x11, 0xf1, 0xd6} \
  }

#pragma pack(1)
typedef struct {

    UINT16  MyWizardDriverStringData[20];
    UINT8   MyWizardDriverHexData;
    UINT8   MyWizardDriverBaseAddress;
    UINT8   MyWizardDriverChooseToEnable;
 
} MYWIZARDDRIVER_CONFIGURATION;

#pragma pack()

#endif

```

Note:

- In order to set, retrieve, and use the UEFI variable, it requires the GUID reference that you just added.

- another Note:  For this lab, you were provided a GUID file.  You can also generate a GUID through the UEFI Driver Wizard or Guidgenerator.com.  But for the purposes of the lab, the one above can be used


---

## Slide 35  [Lab 5: Add New Vars to .c ]
### Lab 5: Update MyWizardDriver.c  
<br>
<b>Open</b>&nbsp;&nbsp; "`~/src/edk2-ws/edk2/MyWizardDriver/MyWizardDriver.c`"<br>

<b>Copy & Paste</b>&nbsp;&nbsp; the following  4 lines after the `#include "MyWizardDriver.h"` statement: 

```c++
#include "MyWizardDriver.h"

EFI_GUID   mMyWizardDriverVarGuid = MYWIZARDDRIVER_VAR_GUID;

CHAR16     mVariableName[] = L"MWD_NVData";  // Use Shell "Dmpstore" to see 
MYWIZARDDRIVER_CONFIGURATION   mMyWizDrv_Conf_buffer;
MYWIZARDDRIVER_CONFIGURATION   *mMyWizDrv_Conf = &mMyWizDrv_Conf_buffer;  //use the pointer 

```

---

## Slide 36  [Lab 5: Update Suppport for new function ]
### Lab 5: Update MyWizardDriver.c 
<br>
<b>Locate</b>&nbsp;&nbsp; "`MyWizardDriverDriverBindingStart ()`" function<br>

<b>Copy & Paste</b>&nbsp;&nbsp; the following line at the beginning of the start function to declare a local variable
```
  EFI_STATUS                Status;
```
<b>Copy & Paste</b>&nbsp;&nbsp; the below 6 lines just before the line "`return EFI_SUPPORTED`": 1) new call to "`CreateNVVariable();`" 2-6) `if` statement with DEBUG and 7) "`return`"  as below: 

```C++
  Status = CreateNVVariable();
	if (EFI_ERROR(Status)) {
		DEBUG((EFI_D_ERROR, "[MyWizardDriver] NV Variable already created \r\n"));
	}
	else {
		DEBUG((EFI_D_ERROR, "[MyWizardDriver] Created NV Variable in the Start \r\n"));
	}


	return EFI_SUCCESS;
```

---

## Slide 37  [Lab 5: Add new function ]
### Lab 5: Update MyWizardDriver.c 
<b>Copy & Paste</b>&nbsp;&nbsp; the new function `CreateNVVariable()` before the call to function  `MyWizardDriverDriverEntryPoint()`
<br>


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
				mMyWizDrv_Conf   //  buffer is 000000  now for first time set
				);
			DEBUG((EFI_D_INFO, "[MyWizardDriver] Variable %s created in NVRam Var\r\n", mVariableName));
			return EFI_SUCCESS;
		}
	}
	// already defined once 
	return EFI_UNSUPPORTED;
 }

```	  

- Note: the gRT->GetVariable and gRT->SetVariable use Runtime services table 
- The Runtime Services Table was not automatically included with the Driver Wizard
- Thus,  the Runtime Services Table will need to be included in MyWizardDriver/MyWizardDriver.h"

---

## Slide 38  [Lab 5: Update .h]
### Lab 5: Update MyWizardDriver.h
<b>Open</b>&nbsp;&nbsp; "`~/src/edk2/MyWizardDriver/MyWizardDriver.h`"<br>
<b>Copy & Paste</b>&nbsp;&nbsp; the following "#include" after the list of library include statements:

```C++

 // Libraries
 // . . .
 #include <Library/UefiRuntimeServicesTableLib.h>
```
<br>
<b>Copy & Paste</b>&nbsp;&nbsp; the following  "#include" after the list of protocol include statements: 

```C++
 // Produced Protocols
 // . . .
 #include "MyWizardDriverNVDataStruc.h"	

 ```
<b>Save</b>&nbsp;&nbsp; "`~/src/edk2-ws/edk2/MyWizardDriver/MyWizardDriver.h`"<br>

<b>Save</b>&nbsp;&nbsp;`"`~/src/edk2-ws/edk2/MyWizardDriver/MyWizardDriver.c`"

---

## Slide 39  [Lab 5 Build and Test Driver]
### Lab 5: Build and Test Driver 1

Build the MyWizardDriver 

```
bash$ cd ~/src/edk2-ws/edk2
bash$ build
```
Copy MyWizardDriver.efi to hda-contents
```
 bash$ cd ~/run-ovmf/hda-contents
 bash$ cp ~/src/edk2-ws/Build/OvmfX64/DEBUG_GCC5/X64/MyWizardDriver.efi .
```
Test by Invoking Qemu
```
  bash$ cd ~/run-ovmf
  bash$ . RunQemu.sh
```

---
## Slide 40 [Lab 5 Build and Test Driver]
### Lab 5: Build and Test Driver 2
<br>

- Load the UEFI Driver from the shell<br>

- &nbsp;&nbsp;&nbsp; At the Shell prompt, type &nbsp; `Shell> fs0:`
- &nbsp;&nbsp;&nbsp; Type:&nbsp;  `FS0:\> `&nbsp;`load MyWizardDriver.efi`  

Observe the Buffer address returned by the debug statement



---

## Slide 41  [Lab 5 Verify Driver]
### Lab 5: Verify Driver 
<br>

Use the Buffer address pointer in the previous slide then use the "`mem`" command


At the Shell prompt, use the `mem` command with the address of the buffer observed from the VS Command window then type&nbsp;&nbsp;  `FS0:\>`&nbsp;`mem 0x1be2d978018` <br>

Observe the Buffer is filled with the letter "B" or 0x0042 <br>

---

## Slide 42  [Lab 5 Verify NVRAM Driver]
### Lab 5: Verify NVRAM Created by Driver 
<br>

At the Shell prompt, type &nbsp;&nbsp; `FS0:\> dmpstore -all -b`<br>

Observe new the NVRAM variable "`MWD_NVData`" was created and filled with 0x00s<br>

<br>

Exit QEMU

Note:

End of Lab 5



---
## Slide 43  [Lab 6: Port Stop and Unload]
<br>

### Lab 6: Port Stop and Unload 
<br>

In this lab, you’ll port the driver’s "Unload" and "Stop" functions to free any resources the driver allocated when it was loaded and started.

 
---
## Slide 44  [Lab 6: Port the Unload]
### Lab 6: Port the Unload function

<b>Open</b>&nbsp;&nbsp; "`~src/edk2-ws/edk2/MyWizardDriver/MyWizardDriver.c`"<br>
<b>Locate</b>&nbsp;&nbsp;"`MyWizardDriverUnload ()`" function<br>
<b>Copy & Paste</b>&nbsp;&nbsp;the following "`if`"  and "`DEBUG`" statements before the "`return EFI_SUCCESS;`" statement.

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
## Slide 45  [Lab 6: Port the Stop]
### Lab 6: Port the Stop function 
<b>Locate</b>&nbsp;&nbsp;"`MyWizardDriverDriverBindingStop()`" function<br>
<b>Comment out</b>&nbsp;&nbsp; with "`//`" before the "`return EFI_UNSUPPORTED;`" statement.<br>
<b>Copy & Paste</b>&nbsp;&nbsp; the following "`if`"  and "`DEBUG`" statements in place of the "`return EFI_UNSUPPORTED;`" statement. 

```C++
	if (DummyBufferfromStart != NULL) {
		FreePool(DummyBufferfromStart);
		DEBUG((EFI_D_INFO, "[MyWizardDriver] Stop, clear buffer\r\n"));
	}

	DEBUG((EFI_D_INFO, "[MyWizardDriver] Stop, EFI_SUCCESS\r\n"));
	return EFI_SUCCESS;
  //  return EFI_UNSUPPORTED;
}
```

<b>Save & Close</b>&nbsp;&nbsp;"`~src/edk2-ws/edk2/MyWizardDriver/MyWizardDriverDriver.c`"

Note:

- The code will deallocate the buffer using the FreePool library function.
- This a duplicate of the check performed in the unload function, in case the stop function was executed prior to unload.
- Notice that the FreePool is called since there was an allocate call to get memory in the start function
- for the stop similar "unwind" calls to mimic same functions in the start
  - free memory for Alocate memory
  - Uninstall Protocol Interfaces  for Install interfaces
  
    
---

## Slide 46  [Lab 6 Build and Test Driver 01]
### Lab 6: Build and Test Driver 1 

Build the MyWizardDriver 


```
bash$ cd ~/src/edk2-ws/edk2
bash$ build
```
Copy MyWizardDriver.efi to hda-contents
```
 bash$ cd ~/run-ovmf/hda-contents
 bash$ cp ~/src/edk2-ws/Build/OvmfX64/DEBUG_GCC5/X64/MyWizardDriver.efi .
```
Test by Invoking Qemu
```
  bash$ cd ~/run-ovmf
  bash$ . RunQemu.sh
```

---
## Slide 47 [Lab 6 Build and Test Driver 02]
### Lab 6: Build and Test Driver 2
<br>

- Load the UEFI Driver from the shell<br>

- &nbsp;&nbsp;&nbsp; At the Shell prompt, type &nbsp; `Shell> fs0:`
- &nbsp;&nbsp;&nbsp; Type:&nbsp;  `FS0:\> `&nbsp;`load MyWizardDriver.efi`  

Observe the Buffer address returned by the debug statement


## Slide 48  [Lab 6 Verify Driver]
### Lab 6: Verify Driver 
<br>

At the Shell prompt, type &nbsp; `FS0:\> `&nbsp;`drivers`<br>
<br>

Observe the handle is "`A9`" as this slide example <br>
Type: &nbsp;` mem  0x25DE4F5C018`<br>
Observe the buffer was filled with the "0x0042" <br>

NOTE: the MyWizardDriver handle maybe different in your lab


## Slide 49  [Lab 6 Verify Unload]
### Lab 6: Verify Unload 


At the Shell prompt, type &nbsp; `FS0:\> `&nbsp;`unload a9`<br>
<br>

Observe the DEBUG messages from the Unload<br><br>
Type `Drivers` again to verify<br>


NOTE: the MyWizardDriver handle maybe different in your lab
---
## Slide 50  [Lab 6 Verify Unload]
### Lab 6: Verify Unload 
<br>

At the Shell prompt, type &nbsp; `FS0:\> `&nbsp;`mem 0x25DE4F5C018 -b`<br>
<br>

Observe the buffer is now NOT filled <br>
<br>

Exit QEMU

Note:
End of Lab 6

---

## Slide 51  [Additional Porting]
### Additional Porting    
<br>

- Adding strings and forms to setup (HII)
- Publish & consume protocols
- Hardware initialization


Refer to the UEFI Drivers Writer’s Guide for more tips – <a href="https://tianocore-docs.github.io/edk2-UefiDriverWritersGuide/draft/edk2-UefiDriverWritersGuide-draft.pdf">Pdf link</a> 

Note:
Use the UEFI Driver Wizard to create a starting point for new drivers on EDK II


---  
## Slide 47  [Summary]

### Summary <br>

- Compile a UEFI driver template created fromUEFI Driver Wizard 
- Test driver QEMU using UEFI Shell 2.0
- Port code into the template driver 


---
## Slide 48  [Questions]
<br>


---
## Slide 49  [Return to Training schedule main page]
<br>

Return to Training schedule main page
https://github.com/tianocore-training/Tianocore_Training_Contents/wiki 

---

## Slide 50  [Logo Slide]
<br><br><br>



---
## Slide 51  [Acknowledgements]

```
Redistribution and use in source (original document form) and 'compiled‘ forms (converted to PDF, epub, HTML and other formats) with or without modification, are permitted provided that the following conditions are met:
Redistributions of source code (original document form) must retain the above copyright notice, this list of conditions and the following disclaimer as the first lines of this file unmodified.
Redistributions in compiled form (transformed to other DTDs, converted to PDF, epub, HTML and other formats) must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.
THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL TIANOCORE PROJECT BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,  BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
Copyright (c) 2021, Intel Corporation. All rights reserved.
```


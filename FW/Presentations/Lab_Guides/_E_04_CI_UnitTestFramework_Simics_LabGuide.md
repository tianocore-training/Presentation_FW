# Unit Test Framework Lab with Simics 
# Windows & Linux
The following  is a lab for using the Unit Test Framework 


---
## Slide 1  CI Unit Test Framework for Developer Validation Lab

## UEFI & EDK II Training

 Continuous Integration (CI) Unit Test Framework for Developer Validation Lab - Simics 

<br>
<a href='http://www.tianocore.org'>tianocore.org</a>


<!---
Note:

  LabGuid.md for UEFI / EDK II Training  CI Unit Test Framework for Developer Validation Lab - Simics

  Copyright (c) 2021-2022, Intel Corporation. All rights reserved.<BR>

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


## Slide 2 **About**
### **About**

This lab will show how to build and run a unit test sample code in the host-based environment.

- Step by step guide for the Stuart CI build and run for the Sample Unit Test from `UnitTestFrameworkPkg`
- Steps to build for the Non-Stuart CI build and run
- Create a Host Unit Test Framework for a simple function
- Add a UEFI Shell Unit Test Framework using the Simics® Quick Start Platform (QSP)`

---

## Slide 3 **Prerequisites**
### **Prerequisites**
* Windows 10:
  * Stuart CI Visual Studio VS2017 or VS2019
  * Non-Stuart CI - Visual Studio VS2015, VS2017 or VS2019
  * Windows SDK (for rc)
  * Windows WDK (for Capsules)
* Ubuntu 18.04 or Fedora
  * GCC5 or greater
* Python 3.7.x or greater on Path and Scripts on path
* Git on Path

---
## Slide 4 Steps for this Lab
### **Steps for this Lab**

1. [Lab Setup](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_E_04_CI_UnitTestFramework_Simics_LabGuide.md#lab-setup)
2. [Build and Run for Stuart CI Locally](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_E_04_CI_UnitTestFramework_Simics_LabGuide.md#how-to-build)
3. Or [Build and Run for Windows | Linux Non-Stuart CI](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_E_04_CI_UnitTestFramework_Simics_LabGuide.md#how-to-build-non-stuart)
4. [Run the Host Unit test locally](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_E_04_CI_UnitTestFramework_Simics_LabGuide.md#4-run-the-host-unit-test-locally)
5. [Create and Add a unit test case to test a function](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_E_04_CI_UnitTestFramework_Simics_LabGuide.md#5-create-and-add-a-unit-test-case-to-test-a-function)
6. [Add the Unit test case the UEFI Shell](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_E_04_CI_UnitTestFramework_Simics_LabGuide.md#6-add-the-unit-test-case-the-uefi-shell)


Solutions: See in the LabMaterial_FW/FW/LabSampleCode/LabSolutions/LessonU_Unit_Test

---

## Slide 5 Lab Setup
### **Lab Setup**

## **1. Setup Lab material from previous lab**

**SKIP** to  [**How to build**](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_E_04_CI_UnitTestFramework_Simics_LabGuide.md#how-to-build) if Lab Setup and build was done from the previous labs


### 1.1 Windows

First Seup for Building EDK II, See [Lab Setup](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_C_01_Build_Setup_Download_EDK_II_Win_LabGuide.md)
then [Platform Build Lab for Simics](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_C_02_Platform_Build_Win_Simics_LabGuide.md)

### 1.2 Linux

First Seup for Building EDK II, See [Lab Setup](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_L_C_01_Build_Setup_Download_EDK_II_Linux_LabGuide.md) 
then [Platform Build Lab for Simics](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_L_C_02_Platform_Build_Linux_Simics_LabGuide.md)





---
## Slide 6 How to Build
## **How to build**
## **2. Build and Run for Stuart CI Locally**
The following steps are for Building with the CI Pytool locally for the UnitTestFrameworkPkg Host based Unit Tests. Note that the "`<Your tag>`" in the examples below needs to be one of the prerequisite compilers above:
* Windows - Visual Studio "`2017`" or "`2019`"
* Ubuntu 18.04 or Fedora - "`GCC5`" 

### **2.1. How to Build using Stuart CI** (Skip doing **How to Build Non-Stuart** if not doing Stuart CI)

#### 2.1.1. Set up environment
  - Windows
     - Open commamd Prompt - CD C:\FW\Edk2-ws and run setup script to setup `WORKSPACE` and Packages path
```shell
        $ cd C:\FW\edk2-ws
        $ Setenv.bat
        $ cd edk2
```
  - Linux
    - Open a terminal prompt
```shell
        bash$ cd ~fw/edk2-ws
        bash$ export WORKSPACE=$PWD
        bash$ export PACKAGES_PATH=$WORKSPACE/edk2:$WORKSPACE/edk2-libc
        bash$ cd edk2
```
#### 2.1.2 (**Stuart CI** Skip 2-6 if doing Non-Stuart)Install the pip requirements  (Pip is in the `Pythonxx/Scripts` Directory Also Note, Proxy option  needed behind a firewall)
```shell
 $ pip install --upgrade -r pip-requirements.txt

:: With proxy:

 $ pip install --upgrade -r pip-requirements.txt --proxy http://proxy-chain.intel.com:911
```

3. Get the code dependencies (done only when submodules change)
```shell
   $ stuart_setup -c .pytool\CISettings.py TOOL_CHAIN_TAG=<Your TAG>
```
4. Update other dependencies (done on new command prompt)
```shell
   $ stuart_update -c .pytool\CISettings.py TOOL_CHAIN_TAG=<Your TAG>
```
5. Build the BaseTools (done only when BaseTools change and first time)
```shell
   $ python BaseTools\Edk2ToolsBuild.py -t <Your TAG>
```
6. Compile and Run the Host based Unit Test modules
```shell
   $ stuart_ci_build -c .pytool\CISettings.py TOOL_CHAIN_TAG=<Your TAG> -p UnitTestFrameworkPkg -t NOOPT -a X64
```

### 2.1.3 **Output from Stuart CI Build and Run**

The return from the CI Build will be `ErrorLevel==0` on successful Unit test passing

```shell
SECTION - Init SDE
SECTION - Loading Plugins
SECTION - Start Invocable Tool
SECTION - Getting Environment
SECTION - Loading plugins
SECTION - Building UnitTestFrameworkPkg Pac
PROGRESS - --Running UnitTestFrameworkPkg: 
PROGRESS - Start time: 2020-07-08 14:20:08.
PROGRESS - Setting up the Environment
PROGRESS - Running Pre Build
PROGRESS - Running Build NOOPT
PROGRESS - Running Post Build
SECTION - Run Host based Unit Tests
SUBSECTION - Testing for architecture: X64
PROGRESS - End time: 2020-07-08 14:21:40.24
PROGRESS - --->Test Success: Host Unit Test
PROGRESS - Overall Build Status: Success
SECTION - Summary
PROGRESS - Success
### **Build **
Build the `BaseTools`
```
---
## **3. Build and Run Non-Stuart CI**
## **How to Build Non-Stuart**

### **3.1 Windows**

Open A Visual Studio Command Prompt
* From the "Windows" key scroll down in the applications menu to **folder** of "Visual Studio 20xx" where xx is either 15, 17 or 19. (make sure you click on the folder and not the application itself)
* Then click on the command prompt for "Developer Command Prompt for VS20xx"
* This will open a black Command line window for you to invoke the build and run commands
* At the command prompt Change directgory (Cd) to the WorkSpace/edk2 directory 
#### 3.1.1. Set up environment
```
    $ cd C:\FW\edk2-ws
    $ Setenv.bat
    $ cd edk2
```

#### 3.1.2. Build the BaseTools
At the command prompt in the directory of your Workspace/edk2 invoke the following:

```shell 
    $ edksetup.bat Rebuild
```
#### 3.1.3. Build the `UnitTestFrameworkPkg` Unit Test Host, below builds using Visual Studio 2015, 
```shell
    $ build -b NOOPT -t VS2015x86 -a X64 -p UnitTestFrameworkPkg\Test\UnitTestFrameworkPkgHostTest.dsc 
```
For other Visual Studio versions change the "`-t`" option to:
* 2017 use `-t VS2017` 
* 2019 use `-t VS2019`

### **3.2 Linux**

#### 3.2.1. Run Make

```shell
    bash$ cd ~/fw/edk-ws/edk2
    bash$ make –C BaseTools/
```
* Make sure the tests pass OK

#### 3.2.2. Run edksetup
```shell
    bash$ . edksetup.sh
```

#### 3.2.3. Build the `UnitTestFrameworkPkg` Unit Test Host
```shell
    $ build -b NOOPT -t GCC5 -a X64 -p UnitTestFrameworkPkg\Test\UnitTestFrameworkPkgHostTest.dsc 
```

----
## **4. Run the Host Unit test locally**

#### Run test results with Manual test results 

First CD to the build directory where the Host-based application was built.
For example, the following is the directory if using VS 2017
- Windows
```
%WORKSPACE%\Build\UnitTestFrameworkPkg\HostTest\NOOPT_VS2017\X64\SampleUnitTestHost.exe
```
- Linux

```
%WORKSPACE%/Build/UnitTestFrameworkPkg/HostTest/NOOPT_GCC5/X64/SampleUnitTestHost.exe
```
Then run the host application (note for Linx no .exe extention)

```shell

    $ SampleUnitTestHost.exe
```
Result from running Unit test
```
Before call  SampleUnitTestHost.exe
Sample Unit Test v0.1
---------------------------------------------------------
------------     RUNNING ALL TEST SUITES   --------------
---------------------------------------------------------
---------------------------------------------------------
RUNNING TEST SUITE: Simple Math Tests
---------------------------------------------------------
[==========] Running 1 test(s).
[ RUN      ] Adding 1 to 1 should produce 2
[       OK ] Adding 1 to 1 should produce 2
[==========] 1 test(s) run.
[  PASSED  ] 1 test(s).
---------------------------------------------------------
RUNNING TEST SUITE: Global Variable Tests
---------------------------------------------------------
[==========] Running 2 test(s).
[ RUN      ] You should be able to change a global BOOLEAN
[       OK ] You should be able to change a global BOOLEAN
[ RUN      ] You should be able to change a global pointer
UnitTest: Pointer - You should be able to change a global pointer
Log Output Start
[WARNING]     About to change a global pointer! Current value is 0x0
Log Output End
[       OK ] You should be able to change a global pointer
[==========] 2 test(s) run.
[  PASSED  ] 2 test(s).
---------------------------------------------------------
RUNNING TEST SUITE: Macro Tests with ASSERT() enabled
---------------------------------------------------------
[==========] Running 13 test(s).
[ RUN      ] Test UT_ASSERT_TRUE() macro
[       OK ] Test UT_ASSERT_TRUE() macro
[ RUN      ] Test UT_ASSERT_FALSE() macro
[       OK ] Test UT_ASSERT_FALSE() macro
[ RUN      ] Test UT_ASSERT_EQUAL() macro
[       OK ] Test UT_ASSERT_EQUAL() macro
[ RUN      ] Test UT_ASSERT_MEM_EQUAL() macro
[       OK ] Test UT_ASSERT_MEM_EQUAL() macro
[ RUN      ] Test UT_ASSERT_NOT_EQUAL() macro
[       OK ] Test UT_ASSERT_NOT_EQUAL() macro
[ RUN      ] Test UT_ASSERT_NOT_EFI_ERROR() macro
[       OK ] Test UT_ASSERT_NOT_EFI_ERROR() macro
[ RUN      ] Test UT_ASSERT_STATUS_EQUAL() macro
[       OK ] Test UT_ASSERT_STATUS_EQUAL() macro
[ RUN      ] Test UT_ASSERT_NOT_NULL() macro
[       OK ] Test UT_ASSERT_NOT_NULL() macro
[ RUN      ] Test UT_EXPECT_ASSERT_FAILURE() macro
UnitTest: MacroUtExpectAssertFailure - Test UT_EXPECT_ASSERT_FAILURE() macro
Log Output Start
[INFO]        Detected expected ASSERT: c:\fw\ut\lj_unittest\3\edk2\UnitTestFrameworkPkg\Test\UnitTest\Sample\SampleUnitTest\SampleUnitTest.c(516): ((BOOLEAN)(0==1))
[INFO]        [ASSERT PASS] c:\fw\ut\lj_unittest\3\edk2\UnitTestFrameworkPkg\Test\UnitTest\Sample\SampleUnitTest\SampleUnitTest.c:516: UT_EXPECT_ASSERT_FAILURE(ASSERT (FALSE)) detected expected assert
[INFO]        Detected expected ASSERT: c:\fw\ut\lj_unittest\3\edk2\MdePkg\Library\BaseLib\String.c(2243): Value < 100
[INFO]        [ASSERT PASS] c:\fw\ut\lj_unittest\3\edk2\UnitTestFrameworkPkg\Test\UnitTest\Sample\SampleUnitTest\SampleUnitTest.c:523: UT_EXPECT_ASSERT_FAILURE(DecimalToBcd8 (101)) detected expected assert
Log Output End
[       OK ] Test UT_EXPECT_ASSERT_FAILURE() macro
[ RUN      ] Test UT_LOG_ERROR() macro
UnitTest: MacroUtLogError - Test UT_LOG_ERROR() macro
Log Output Start
[ERROR]       UT_LOG_ERROR() message
Log Output End
[       OK ] Test UT_LOG_ERROR() macro
[ RUN      ] Test UT_LOG_WARNING() macro
UnitTest: MacroUtLogWarning - Test UT_LOG_WARNING() macro
Log Output Start
[WARNING]     UT_LOG_WARNING() message
Log Output End
[       OK ] Test UT_LOG_WARNING() macro
[ RUN      ] Test UT_LOG_INFO() macro
UnitTest: MacroUtLogInfo - Test UT_LOG_INFO() macro
Log Output Start
[INFO]        UT_LOG_INFO() message
Log Output End
[       OK ] Test UT_LOG_INFO() macro
[ RUN      ] Test UT_LOG_VERBOSE() macro
UnitTest: MacroUtLogVerbose - Test UT_LOG_VERBOSE() macro
Log Output Start
[VERBOSE]     UT_LOG_VERBOSE() message
Log Output End
[       OK ] Test UT_LOG_VERBOSE() macro
[==========] 13 test(s) run.
[  PASSED  ] 13 test(s).
---------------------------------------------------------
RUNNING TEST SUITE: Macro Tests with ASSERT() disabled
---------------------------------------------------------
[==========] Running 13 test(s).
[ RUN      ] Test UT_ASSERT_TRUE() macro
[       OK ] Test UT_ASSERT_TRUE() macro
[ RUN      ] Test UT_ASSERT_FALSE() macro
[       OK ] Test UT_ASSERT_FALSE() macro
[ RUN      ] Test UT_ASSERT_EQUAL() macro
[       OK ] Test UT_ASSERT_EQUAL() macro
[ RUN      ] Test UT_ASSERT_MEM_EQUAL() macro
[       OK ] Test UT_ASSERT_MEM_EQUAL() macro
[ RUN      ] Test UT_ASSERT_NOT_EQUAL() macro
[       OK ] Test UT_ASSERT_NOT_EQUAL() macro
[ RUN      ] Test UT_ASSERT_NOT_EFI_ERROR() macro
[       OK ] Test UT_ASSERT_NOT_EFI_ERROR() macro
[ RUN      ] Test UT_ASSERT_STATUS_EQUAL() macro
[       OK ] Test UT_ASSERT_STATUS_EQUAL() macro
[ RUN      ] Test UT_ASSERT_NOT_NULL() macro
[       OK ] Test UT_ASSERT_NOT_NULL() macro
[ RUN      ] Test UT_EXPECT_ASSERT_FAILURE() macro
UnitTest: MacroUtExpectAssertFailure - Test UT_EXPECT_ASSERT_FAILURE() macro
Log Output Start
[WARNING]     [ASSERT WARN] c:\fw\ut\lj_unittest\3\edk2\UnitTestFrameworkPkg\Test\UnitTest\Sample\SampleUnitTest\SampleUnitTest.c:516: UT_EXPECT_ASSERT_FAILURE(ASSERT (FALSE)) disabled
[WARNING]     [ASSERT WARN] c:\fw\ut\lj_unittest\3\edk2\UnitTestFrameworkPkg\Test\UnitTest\Sample\SampleUnitTest\SampleUnitTest.c:523: UT_EXPECT_ASSERT_FAILURE(DecimalToBcd8 (101)) disabled
Log Output End
[       OK ] Test UT_EXPECT_ASSERT_FAILURE() macro
[ RUN      ] Test UT_LOG_ERROR() macro
UnitTest: MacroUtLogError - Test UT_LOG_ERROR() macro
Log Output Start
[ERROR]       UT_LOG_ERROR() message
Log Output End
[       OK ] Test UT_LOG_ERROR() macro
[ RUN      ] Test UT_LOG_WARNING() macro
UnitTest: MacroUtLogWarning - Test UT_LOG_WARNING() macro
Log Output Start
[WARNING]     UT_LOG_WARNING() message
Log Output End
[       OK ] Test UT_LOG_WARNING() macro
[ RUN      ] Test UT_LOG_INFO() macro
UnitTest: MacroUtLogInfo - Test UT_LOG_INFO() macro
Log Output Start
[INFO]        UT_LOG_INFO() message
Log Output End
[       OK ] Test UT_LOG_INFO() macro
[ RUN      ] Test UT_LOG_VERBOSE() macro
UnitTest: MacroUtLogVerbose - Test UT_LOG_VERBOSE() macro
Log Output Start
[VERBOSE]     UT_LOG_VERBOSE() message
Log Output End
[       OK ] Test UT_LOG_VERBOSE() macro
[==========] 13 test(s) run.
[  PASSED  ] 13 test(s).

```
---
### **Optional** - Run test results with Automatic test results 
<i>Note the following is using Windows</i><br>
Use a Script file to get (Errorlevel==0) for a test pass or (Errorlevel==1) for a test Failure. 

To test automatically test pass/fail using a script file, check for string "\<failed\>" in XML file from other sample unit test with Host .EXE using a grep or on Windows `FindStr`

Create a script and include the script snippet below.  Notice that the script will set the error level to 1 on test failure or error level 0 on a test pass from the `SampleUnitTestHost.exe`

Note, the script will need to run or determine the directory where the `SampleUnitTestHost.exe`   file gets built. (Hint: the build directory will be based on your TOOL_CHAIN_TAG)

1. Notepad runSampleUnitTest.bat
2. add the following:

```shell
REM First CD to the correct build directory
REM Example: 
:: CD %WORKSPACE%\Build\UnitTestFrameworkPkg\HostTest\NOOPT_VS2015x86\X64

set CMOCKA_MESSAGE_OUTPUT=xml
set CMOCKA_XML_FILE=test.xml
if exist test.xml del test.xml
call  SampleUnitTestHost.exe >nul
findstr "<failure>" test.xml >nul
if %ERRORLEVEL% == 0 (
echo FAILURE in unit test occurred
EXIT /B 1
goto :EOF
) 
echo Unit Test Passed
EXIT /B 0
```
---

##  **5. Create and Add a unit test case to test a function**

The following will show the steps for creating a Unit Test case for testing a very simple function. For this lab the function to test is to pass an integer value to the function `PrimeNumber()`. This function will return a Boolean value "TRUE" if the number prime or a Boolean value "FALSE" if the number is NOT prime.

### **5.1. Create a new directory called CheckPrimeUnitTest**
from %WORKSPACE%/UnitTestFrameworkPkg/Test/UnitTest/Sample
```
$ cd UnitTestFrameworkPkg/Test/UnitTest/Sample
$ mkdir CheckPrimeUnitTest
$ cd CheckPrimeUnitTest
```
---
### **5.2. Create a file with the function** "`PrimeNumber()`" below code. 
Create a file, `PrimeNumber.c` and add the code below. This is a very simple function to pass an integer value a test if the value passed is prime.


```c++
/** @file
File Header 
**/
#include "CheckPrimeUnitTest.h"


/**
Function to test will return Boolean True if number is Prime or False if Number is Not Prime
@param[in] Number	Integer to check if prime number
@retval  TRUE		Number passed was prime
@retval  FALSE		Number passed was Not prime
*/
BOOLEAN PrimeNumber(
	IN INTN Number
)
{
	INTN i;
	BOOLEAN NumberisPrime = TRUE;  // True if the number passed is prime
	if (Number <= 1) NumberisPrime = FALSE; // not prime
	for (i = 2; (i * i) <= Number; i++) { // Square root of incrementor is <= Number passed
		if (Number % i == 0) // if the number MOD incrementor than the number is not prime
			NumberisPrime = FALSE; // not prime
	}

	return (NumberisPrime);  // Prime number
}

```
---

### **5.3. Create a file called**  `CheckPrimeUnitTest.c`
First add 2 test cases to test a number passed is prime and another for it the number is Not prime
  
#### 5.3.1. Use the following for test cass Is Prime:

```c++
/** @file
File Header 
**/
#include "CheckPrimeUnitTest.h"

#define UNIT_TEST_NAME     "Sample Check Number is Prime Unit Test"
#define UNIT_TEST_VERSION  "0.1"
#define A_PRIME_NUMBER  197
#define NOT_PRIME_NUMBER  64


/**
Sample unit test that verifies the expected result of an integer is Prime

@param[in]  Context    [Optional] An optional parameter that enables:
1) test-case reuse with varied parameters and
2) test-case re-entry for Target tests that need a
reboot.  This parameter is a VOID* and it is the
responsibility of the test author to ensure that the
contents are well understood by all test cases that may
consume it.

@retval  UNIT_TEST_PASSED             The Unit test has completed and the test
case was successful.
@retval  UNIT_TEST_ERROR_TEST_FAILED  A test case assertion has failed.
**/
UNIT_TEST_STATUS
EFIAPI
IsNumberPrimeTestTrue(
	IN UNIT_TEST_CONTEXT  Context
)
{
	UT_ASSERT_TRUE(PrimeNumber(A_PRIME_NUMBER));
	return UNIT_TEST_PASSED;
}
```
---
#### 5.3.2. Use the following for test case is NOT Prime

```c++
// Test case to test number is not prime

UNIT_TEST_STATUS
EFIAPI
IsNumberPrimeTestFalse(
	IN UNIT_TEST_CONTEXT  Context
)
{
	UT_ASSERT_FALSE(PrimeNumber(NOT_PRIME_NUMBER));
	return UNIT_TEST_PASSED;
}

```
#### 5.3.3. Add the Unit Test Framework Main called `UefiTestMain()` to perform the following steps:
* Register the Unit Test Framework use `InitUnitTestFramework()` 
* Register a Unit Test Suite use `CreateUnitTestSuite()` 
* Add the two test cases above use `AddTestCase()` twice
* Run the test cases in test suite use `RunAllTestSuites()`
* Free the test suite framework when done use `FreeUnitTestFramework()`

```c++

/**
  Initialize the unit test framework, suite, and unit tests for the
  sample unit tests and run the unit tests.

  @retval  EFI_SUCCESS           All test cases were dispatched.
  @retval  EFI_OUT_OF_RESOURCES  There are not enough resources available to
                                 initialize the unit tests.
**/
EFI_STATUS
EFIAPI
UefiTestMain (
  VOID
  )
{
  EFI_STATUS                  Status;
  UNIT_TEST_FRAMEWORK_HANDLE  Framework;
  UNIT_TEST_SUITE_HANDLE      PrimeNumberTests;

  Framework = NULL;

  DEBUG(( DEBUG_INFO, "%a v%a\n", UNIT_TEST_NAME, UNIT_TEST_VERSION ));

  //
  // Start setting up the test framework for running the tests.
  //
  Status = InitUnitTestFramework (&Framework, UNIT_TEST_NAME, gEfiCallerBaseName, UNIT_TEST_VERSION);
  if (EFI_ERROR (Status)) {
    DEBUG((DEBUG_ERROR, "Failed in InitUnitTestFramework. Status = %r\n", Status));
    goto EXIT;
  }

  //
  // Populate the SimpleMathTests Unit Test Suite.
  //
  Status = CreateUnitTestSuite (&PrimeNumberTests, Framework, "Simple Check Prime Number Tests", "Prime.Num", NULL, NULL);
  if (EFI_ERROR (Status)) {
    DEBUG ((DEBUG_ERROR, "Failed in CreateUnitTestSuite for PrimeNumberTests\n"));
    Status = EFI_OUT_OF_RESOURCES;
    goto EXIT;
  }


 
  AddTestCase(PrimeNumberTests, "Test Number is 197 IS Prime", "Test-1", IsNumberPrimeTestTrue, NULL, NULL, NULL);
  AddTestCase(PrimeNumberTests, "Test Number is 64 NOT Prime", "Test-2", IsNumberPrimeTestFalse, NULL, NULL, NULL);
  
  //
  // Execute the tests.
  //
  Status = RunAllTestSuites (Framework);

EXIT:
  if (Framework) {
    FreeUnitTestFramework (Framework);
  }

  return Status;
}

```
---
#### 5.3.4. Add the entry points for PEI, DXE and the Host Runtime 

```c++
/**
  Standard PEIM entry point for target based unit test execution from PEI.
**/
EFI_STATUS
EFIAPI
PeiEntryPoint (
  IN EFI_PEI_FILE_HANDLE     FileHandle,
  IN CONST EFI_PEI_SERVICES  **PeiServices
  )
{
  return UefiTestMain ();
}

/**
  Standard UEFI entry point for target based unit test execution from DXE, SMM,
  UEFI Shell.
**/
EFI_STATUS
EFIAPI
DxeEntryPoint (
  IN EFI_HANDLE        ImageHandle,
  IN EFI_SYSTEM_TABLE  *SystemTable
  )
{
  return UefiTestMain ();
}

/**
  Standard POSIX C entry point for host based unit test execution.
**/
int
main (
  int argc,
  char *argv[]
  )
{
  return UefiTestMain ();
}

```
#### 5.3.5. Save the file

---
### **5.4.  Create a file** `CheckPrimeUnitTest.h` 


This file has all the `#include` statements and function to test prototype. Then save the file.

#### 5.4.1. Add the following contents

```c++
/** @file
 File Header
**/
#ifndef _CHECK_PRIME_UNIT_TEST_H_
#define _CHECK_PRIME_UNIT_TEST_H_

#include <PiPei.h>
#include <Uefi.h>
#include <Library/UefiLib.h>
#include <Library/DebugLib.h>
#include <Library/UnitTestLib.h>
#include <Library/PrintLib.h>

/**
Super Simple FUNCTION TO TEST Prototype
@param[in]  Number  signed Integer value  
@retval  TRUE		Number passed was prime
@retval  FALSE		Number passed was Not prime
**/
BOOLEAN PrimeNumber(
	IN INTN Number
);

#endif
```
#### 5.4.2. Save the file

---
### **5.5.  Create INF files for Host, DXE, PEI, DXE_SMM, and Shell**
Just the Host INF file is shown here.  See the Directory: [SampleUnitTest](https://github.com/tianocore/edk2/tree/master/UnitTestFrameworkPkg/Test/UnitTest/Sample/SampleUnitTest) for the other differences between the INF files. The `Sources`, `Packages`, `LibraryClasses`, and `Pcd` sections will remain the same but the `MODULE_TYPE`, `ENTRY_POINT`, and `Depex` sections will be different. Also, note **always**   get a new `FILE_GUID` for each INF file. 

#### 5.5.1. Create a file called `CheckPrimeUnitTestHost.inf` with the following. Notice the `MODULE_TYPE` is `HOST_APPLICATION`:


```bash
## @file
# Sample UnitTest built for execution on a Host/Dev machine.
##
[Defines]
  INF_VERSION    = 0x00010005
  BASE_NAME      = CheckPrimeUnitTestHost
  FILE_GUID      = 66e96a37-e79f-4afa-bb30-e7bab44ecf94
  MODULE_TYPE    = HOST_APPLICATION
  VERSION_STRING = 1.0
#
# The following information is for reference only and not required by the build tools.
#
#  VALID_ARCHITECTURES           = IA32 X64
#
[Sources]
  CheckPrimeUnitTest.c
  CheckPrimeUnitTest.h
  PrimeNumber.c

[Packages]
  MdePkg/MdePkg.dec

[LibraryClasses]
  BaseLib
  DebugLib
  UnitTestLib

[Pcd]
  
```

#### 5.5.2. Create a file called `CheckPrimeUnitTestUefiShell.inf` with the following. Notice the `MODULE_TYPE` is `UEFI_APPLICATION`:

```bash
## @file
# Sample UnitTest built for execution in UEFI Shell.
#
# Copyright (c) Microsoft Corporation.<BR>
# SPDX-License-Identifier: BSD-2-Clause-Patent
##

[Defines]
  INF_VERSION    = 0x00010006
  BASE_NAME      = CheckPrimeUnitTestUefiShell
  FILE_GUID      = 9B6BA8D5-9590-4659-9DA6-73FA368892F7
  MODULE_TYPE    = UEFI_APPLICATION
  VERSION_STRING = 1.0
  ENTRY_POINT    = DxeEntryPoint

#
# The following information is for reference only and not required by the build tools.
#
#  VALID_ARCHITECTURES           = IA32 X64
#

[Sources]
  CheckPrimeUnitTest.c
  CheckPrimeUnitTest.h
  PrimeNumber.c

[Packages]
  MdePkg/MdePkg.dec

[LibraryClasses]
  UefiApplicationEntryPoint
  BaseLib
  DebugLib
  UnitTestLib
  PrintLib

[Pcd]
  gEfiMdePkgTokenSpaceGuid.PcdDebugPropertyMask

```
---
### **5.6. Add the** `CheckPrimeUnitTestHost.inf` **to the Test .dsc file**

#### 5.6.1. Edit the file, `edk2/UnitTestFrameworkPkg/Test/UnitTestFrameworkPkgHostTest.dsc` and add the reference to the new test case INF file to the `[Components]` section

Note, that the host [`UnitTestFrameWorkPkgHost.dsc.inc`](https://github.com/tianocore/edk2/blob/master/UnitTestFrameworkPkg/UnitTestFrameworkPkgHost.dsc.inc) file is included with this .dsc file.


#### 5.6.2. Add the `CheckPrimeUnitTestHost.inf` as shown below
```bash
...

!include UnitTestFrameworkPkg/UnitTestFrameworkPkgHost.dsc.inc

... 
[Components]
  #
  # Build HOST_APPLICATION that tests the SampleUnitTest
  #
  UnitTestFrameworkPkg/Test/UnitTest/Sample/SampleUnitTest/SampleUnitTestHost.inf
  UnitTestFrameworkPkg/Test/UnitTest/Sample/CheckPrimeUnitTest/CheckPrimeUnitTestHost.inf  # NEW INF for Check Prime Unit Test 
...

```
#### 5.6.3. Save the file `UnitTestFrameworkPkgHostTest.dsc`
---

### **5.7. Build and run as above for either CI or Non-CI Build and Run**
* CI Build and Run (where "Your TAG" is you Compiler Tag) from the `edk2` workspace directory
```
$  stuart_ci_build -c .pytool\CISettings.py TOOL_CHAIN_TAG=<Your TAG> -p UnitTestFrameworkPkg -t NOOPT -a X64
```
* Non-CI Build (where "Your TAG" is your VS Compiler Tag e.g. VS2015x86, VS2017, VS2019)
**Windows**
```shell
$ build -b NOOPT -t <Your TAG> -a X64 -p UnitTestFrameworkPkg\Test\UnitTestFrameworkPkgHostTest.dsc 
```
**Linux**
```shell
$ build -b NOOPT -t GCC5 -a X64 -p UnitTestFrameworkPkg/Test/UnitTestFrameworkPkgHostTest.dsc 
```


---
### **5.8. Run the Host Unit Test Locally**
```shell
:: FIRST CD to the BUILD Directory where unit test host .EXE was built.
$ CheckPrimeUnitTestHost.exe
```
---
##  **6. Add the Unit test case the UEFI Shell**
Use the above unit test case but add it to the shell INF file instead of the host. Note, for this example use the `EmulatorPkg` for adding the `CheckPrimeUnitTest` Test to be included with the UEFI Shell.

### **6.1 Check `UnitTestFrameworkPkgTarget.dsc.inc` file**





Check [`UnitTestFrameworkPkgTarget.dsc.inc`](https://github.com/tianocore/edk2/blob/master/UnitTestFrameworkPkg/UnitTestFrameworkPkgTarget.dsc.inc) for the required Libraries, PCDs and Build Options in the appropriate  sections. 

#### **6.1.1 Check the required Libraries**
Note that following Library Classes that will be required specific for the `UnitTestFrameworkPkg`:
```
[LibraryClasses]
. . .

  UnitTestLib|UnitTestFrameworkPkg/Library/UnitTestLib/UnitTestLib.inf
  UnitTestPersistenceLib|UnitTestFrameworkPkg/Library/UnitTestPersistenceLibNull/UnitTestPersistenceLibNull.inf
  UnitTestResultReportLib|UnitTestFrameworkPkg/Library/UnitTestResultReportLib/UnitTestResultReportLibDebugLib.inf
  NULL|UnitTestFrameworkPkg/Library/UnitTestDebugAssertLib/UnitTestDebugAssertLib.inf


```
Note for the LibraryClasses for `UEFI_APPLICATION` set up the log reporting to go to the console output.
```
 . . .
[LibraryClasses.common.UEFI_APPLICATION]
  UnitTestResultReportLib|UnitTestFrameworkPkg/Library/UnitTestResultReportLib/UnitTestResultReportLibConOut.inf
 . . .
```
---
#### **6.1.2 Check the required PCDs**
Note that the PCDs for `UnitTestFrameworkPkgTarget.dsc.inc` are:
```
[PcdsFixedAtBuild]
  gEfiMdePkgTokenSpaceGuid.PcdDebugPropertyMask|0x17
```
#### **6.1/3 Check the required Build Options**
Note that the Build Options for `UnitTestFrameworkPkgTarget.dsc.inc` are:
```
[BuildOptions]
  MSFT:*_*_*_CC_FLAGS  = -D DISABLE_NEW_DEPRECATED_INTERFACES -D EDKII_UNIT_TEST_FRAMEWORK_ENABLED
  GCC:*_*_*_CC_FLAGS   = -D DISABLE_NEW_DEPRECATED_INTERFACES -D EDKII_UNIT_TEST_FRAMEWORK_ENABLED
  XCODE:*_*_*_CC_FLAGS = -D DISABLE_NEW_DEPRECATED_INTERFACES -D EDKII_UNIT_TEST_FRAMEWORK_ENABLED
```
---
### **6.2 Add the Unit test for the UEFI Shell to the Simics OpenBoardPkg.dsc file**

#### 6.2.1. Edit the file:
##### **Windows**
```
C:\FW\edk2-ws\edk2-platforms\Platform\Intel\SimicsOpenBoardPkg\BoardX58Ich10\OpenBoardPkg.dsc
```
##### **Linux**
```
~fw/edk2-ws/edk2-platforms/Platform/Intel/SimicsOpenBoardPkg/BoardX58Ich10/OpenBoardPkg.dsc
```



#### 6.2.2. Add the following:
In the `[Components.X64]` Section of the `OpenBoardPkg.dsc` file. Hint, After the 
"`# UEFI / EDK II Training Class`" in the  Section.

```

  UnitTestFrameworkPkg/Test/UnitTest/Sample/CheckPrimeUnitTest/CheckPrimeUnitTestUefiShell.inf{

  <LibraryClasses>
  	UnitTestLib|UnitTestFrameworkPkg/Library/UnitTestLib/UnitTestLib.inf
  	UnitTestPersistenceLib|UnitTestFrameworkPkg/Library/UnitTestPersistenceLibNull/UnitTestPersistenceLibNull.inf
    UnitTestResultReportLib|UnitTestFrameworkPkg/Library/UnitTestResultReportLib/UnitTestResultReportLibConOut.inf

  <PcdsFixedAtBuild>
  gEfiMdePkgTokenSpaceGuid.PcdDebugPropertyMask|0x17
  
  <BuildOptions>
  MSFT:*_*_*_CC_FLAGS  = -D DISABLE_NEW_DEPRECATED_INTERFACES -D EDKII_UNIT_TEST_FRAMEWORK_ENABLED
  GCC:*_*_*_CC_FLAGS   = -D DISABLE_NEW_DEPRECATED_INTERFACES -D EDKII_UNIT_TEST_FRAMEWORK_ENABLED
  XCODE:*_*_*_CC_FLAGS = -D DISABLE_NEW_DEPRECATED_INTERFACES -D EDKII_UNIT_TEST_FRAMEWORK_ENABLED
}
```

#### 6.2.3. Save OpenBoardPkg.dsc
---
### **6.3  Build the Simics QSP Board**


#### **6.3.1 Windows Build**

 
1. Open another Visual studio Command Prompt

```bash
$> Cd C:\FW\edk2-ws\edk2-platforms\Platform\Intel\
$> python build_bios.py -p BoardX58Ich10  -t VS20XX
```
- Where XX is `15x86` or `17` or `19`

2. Copy the UefiAppLab.vhd

From:

`.../Lab_Material_FW/FW/LabSampleCode/AppLab/UefiAppLab.vhd` 

**TO**

`%USERPROFILE%\AppData\Local\Programs\Simics\simics-qsp-x86-6.0.57\targets\qsp-x86\images`


3. Mount the UefiAppLab.vhd using Disk Manager: 
[How to Mount VHD LINK](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_C_04_Writing_UEFI_App_Simics_Win_LabGuide.md#how-to-mount-vhd)


4. Copy & Paste CheckPrimeUnitTestUefiShell.efi

```bash
C:\FW\edk2-ws\Build\SimicsOpenBoardPkg\BoardX58Ich10\DEBUG_VS20xx\X64\CheckPrimeUnitTestUefiShell.efi
```
**To**

`X:\UEFIAPPLAB\` (where X is the VHD Drive)


#### **6.3.2 Linux Build**
1, Open another Terminal Prompt in $HOME/fw/edk2-ws

2. Then CD to edk2 to do edksetup.sh
```bash
$ cd ~/fw/edk2-ws/edk2
$ . edksetup.sh
```

3. Then CD to:
```bash
$ cd ~/fw/edk2-ws/edk2-platforms/Platform/Intel
```

4. Invoke the Python Build script for Simics OpenBoard QSP

```bash
$ python build_bios.py -p BoardX58Ich10 -t GCC5
```

5. Copy the UefiAppLab.vhd

From:

`.../Lab_Material_FW/FW/LabSampleCode/AppLab/UefiAppLab.vhd`

To:

`<SimicsInstallDir>/simics-qsp-x86-6.0.57/targets/qsp-x86/images`

Where *SimicsInstallDir* is the directory selected to install Simics, e.g., `Computer/usr/bin/simics`

6. Mount the UefiAppLab.vhd using GuestMount: [How to Link](https://github.com/tianocore-training/Presentation_FW/blob/main/FW/Presentations/Lab_Guides/_L_C_04_Writing_UEFI_App_Simics_Linux_LabGuide.md#slide-87-mounting-a-vhd-file-disk)

7. Copy & Paste CheckPrimeUnitTestUefiShell.efi

```bash
$cp ~/FW/edk2-ws/Build/SimicsOpenBoardPkg/BoardX58Ich10/DEBUG_GCC5/X64/CheckPrimeUnitTestUefiShell.efi ~/VHD
```

---
### **6.4  Update the Simics Script**

Update the Simics script to use the UefiAppLab.vhd image as a file system
Edit the file: `gsp-modern-core.simics` from
#### 6.4.1 Windows
```
%USERPROFILE%\ \AppData\Local\Programs\Simics\simics-qsp-cpu-6.0.4\targets\qsp-x86\qsp-modern-core.simics
```
#### 6.4.2 Linux
```
<SimicsInstallDir>/simics-qsp-cpu-6.0.4/targets/qsp-x86/qsp-modern-core.simics
```

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
#$disk1_image="%simics%/targets/qsp-x86/images/ShellLab.vhd“
$disk1_image="%simics%/targets/qsp-x86/images/UefiAppLab.vhd“

run-command-file "%simics%/targets/qsp-x86/qsp-clear-linux.simics"

```

---
### **6.5  Run the Simics QSP**
#### 6.5.1 Windows
1. Open a Windows Command prompt

```bash
$> cd simics-projects\my-simics-project-1
```

2. Run the qsp-modern-core script:

```bash
$> .\simics targets/qsp-x86/qsp-modern-core.simics 
simics> run
```


#### 6.5.2 Linux

1. Open another Terminal prompt

```bash
$> cd simics-projects/my-simics-project-1
```

2. Run the qsp-modern-core script:

```bash
$> ./simics targets/qsp-x86/qsp-modern-core.simics 
simics> run
```

#### 6.5.3 Both Linux and Windows
Press "F2" at the logo, then select "Boot Manager" followed by "EFI Internal Shell"



---
### **6.6  Run the App, `CheckPrimeUnitTestUefiShell` in the UEFI Shell** 

At the UEFI Shell prompt Run the Unit test Application.  Note that the log output goes to the UEFI Shell Console.
```
Shell> fs1:
FS1:\> CheckPrimeUnitTestUefiShell.efi
---------------------------------------------------------
------------- UNIT TEST FRAMEWORK RESULTS ---------------
---------------------------------------------------------
/////////////////////////////////////////////////////////
  SUITE: Simple Check Prime Number Tests
   PACKAGE: Prime.Num
/////////////////////////////////////////////////////////
*********************************************************
  CLASS NAME: Test-1
  TEST:    Test Number is 197 IS Prime
  STATUS:  PASSED
  FAILURE: NO FAILURE
  FAILURE MESSAGE:

**********************************************************
*********************************************************
  CLASS NAME: Test-2
  TEST:    Test Number is 64 NOT Prime
  STATUS:  PASSED
  FAILURE: NO FAILURE
  FAILURE MESSAGE:

**********************************************************
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Suite Stats
 Passed:  2  (100%)
 Failed:  0  (0%)
 Not Run: 0  (0%)
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++
=========================================================
Total Stats
 Passed:  2  (100%)
 Failed:  0  (0%)
 Not Run: 0  (0%)
=========================================================
```
To redirect the output to the Debug Serial Port, in the OpenBoardPkg.dsc file change the library for the library class `UnitTestResultReportLib` to the implementation for `UnitTestResultReportLibDebugLib.inf`

# Solutions:

See class files for the solution 

- . . .FW/LabSampleCode/LabSessions/LessonU_Unit_Test

Compare then copy the following:

- Copy the `OpenBoardPkg.dsc` to `.../edk2-ws/edk2-platforms/Platform/Intel/SimicsOpenBoardPkg/BoardX58Ich10/OpenBoardPkg.dsc`

- Copy the Dir `Test` to C:/FW/edk2-ws/edk2/UnitTestFrameworkPkg 


---
## Slide 7 Resouces


Unit Test Framework Package Overview:
https://github.com/tianocore/edk2/blob/master/UnitTestFrameworkPkg/ReadMe.md

Continuous Integration (CI) Configuring for Unit Tests:
https://github.com/tianocore/edk2/blob/master/.pytool/Readme.md


**Code Examples of Unit Test Cases**

- Sample Unit Test: https://github.com/tianocore/edk2/tree/master/UnitTestFrameworkPkg/Test/UnitTest/Sample/SampleUnitTest
- BaseSafeIntLib Unit Test: https://github.com/tianocore/edk2/tree/master/MdePkg/Test/UnitTest/Library/BaseSafeIntLib
- BaseLib Unit Test: https://github.com/tianocore/edk2/tree/master/MdePkg/Test/UnitTest/Library/BaseLib
- DxeResetSystemLib Unit Test: https://github.com/tianocore/edk2/tree/master/MdeModulePkg/Library/DxeResetSystemLib/UnitTest
- MtrrLibUnitTest: 
https://github.com/tianocore/edk2/tree/master/UefiCpuPkg/Library/MtrrLib/UnitTest


Cmocka Edk II Unit Test ChefCook example: https://github.com/Laurie0131/edk2/tree/LJ_UnitTest3/UnitTestFrameworkPkg/Test/UnitTest/Sample/ChefAppUnitTest



---
## Slide 8 Questions?


<br>

---
## Slide 9 Return to Main Training Page

Return to Training table of contents for next presentation [link](https://github.com/tianocore-training/Tianocore_Training_Contents/wiki)

---
## Slide 10 Logo Slide


<br>

---
## Slide 11 Acknowledgements

Redistribution and use in source (original document form) and 'compiled‘ forms (converted to PDF, epub, HTML and other formats) with or without modification, are permitted provided that the following conditions are met:


Redistributions of source code (original document form) must retain the above copyright notice, this list of conditions and the following disclaimer as the first lines of this file unmodified.


Redistributions in compiled form (transformed to other DTDs, converted to PDF, epub, HTML and other formats) must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.


THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL TIANOCORE PROJECT BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,  BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.


Copyright (c) 2021-2022, Intel Corporation. All rights reserved.








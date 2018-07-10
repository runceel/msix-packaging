# MSIX Packaging SDK 
--------------------
   Copyright (c) 2017 Microsoft Corp.  All rights reserved.

## Description
--------------
   The MSIX Packaging SDK project is an effort to enable developers on a variety of platforms to pack and unpack 
   packages for the purposes of distribution from either the Microsoft Store, or their own content distribution networks.  
    
   The MSIX Packaging APIs that a client app would use to interact with .msix/.appx packages are a subset of those
   documented [here](https://msdn.microsoft.com/en-us/library/windows/desktop/hh446766(v=vs.85).aspx).  See [sample/ExtractContentsSample/ExtractContentsSample.cpp](/sample/ExtractContentsSample/ExtractContentsSample.cpp) for additional details.


## Overview
-----------
The MSIX Packaging SDK project includes cross platform API support for unpacking of .msix/.appx packages

|                                      |                                 |
|--------------------------------------|---------------------------------|
| **msix**      | A shared library (DLL on Win32, dylib on MacOs, SO on Linux and Android) that exports a subset of the functionality contained within appxpackaging.dll on Windows. See [here](https://msdn.microsoft.com/en-us/library/windows/desktop/hh446766(v=vs.85).aspx) for additional details. On all platforms instead of CoCreating IAppxFactory, a c-style export: CoCreateAppxFactory is provided.  See sample folder at root of package for cross platform consumption examples.                <br /><br /> Finally, there is one export 'Unpack' that provides an simplified unpackage implementation.|
| **makemsix**  | A command line wrapper over the Unpack implementation.  This tool exists primarily as a means of validating the implementation of the MSIX Packaging SDK internal routines and is compiled for Win32, MacOS, and Linux platforms.|

Guidance on how to package your app contents and construct your app manifest such that it can take advantage of the cross platform support of this SDK is [here](tdf-guidance.md).
## Setup Instructions
---------------------
1. Clone the repository:
        ```git clone [URL]```
        
2. Initialize git submodules:
   ```
   git submodule init
   git submodule update
   ```

## Issues
---------
If you are using Visual Studio 2017 and you run into errors about not being able to find the v140 toolset:
1. Install the Microsoft Build Tools (https://chocolatey.org/packages/microsoft-build-tools)
2. Start -> visual studio installer -> Visual Studio Build Tools 2017 -> Modify the 2014 toolset -> individual components 
3. Make sure that VC++ 2015.3 v140 toolset for desktop is selected and then unselect VC++ 2017 141 toolset
4. Close, then re-open the solution.

## Dependencies
---------------
Depending on the platform for which the MSIX shared library (MSIX.DLL | libmsix.dylib | libmsix.so) is compiled, one or 
more of the following dependencies may be statically linked into the binary:

* [ZLib](https://github.com/madler/zlib)
* [Xerces-C](https://github.com/apache/xerces-c)
* [OpenSSL](https://github.com/openssl/openssl)
* [Android NDK](https://developer.android.com/ndk)

For convinience, Zlib and Xerces-C are git-subtrees that are mapped in under the lib folder of this project.  Edits on top
of these subtrees for build related optimizations are tracked within this repository.

OpenSSL is mapped in under the lib folder as a git-submodule.  For a variety of reasons, the MSIX project only uses OpenSSL on
non-Windows platforms, and only (at the time of creation) the latest version of the 1.0.2 release (specifically as tracked via tag "OpenSSL_1_0_2m").

The Android NDK is only required for targeting the Android platform.

## Prerequisites
----------------
Make sure that you have CMAKE installed on your machine 

   * https://cmake.org/download/

One or more of the following prerequisites may also be required on your machine:

##### Ninja-build:
https://github.com/ninja-build/ninja/releases

##### Android NDK:
https://developer.android.com/ndk/downloads/index.html

##### Clang/LLVM:
http://releases.llvm.org/download.html
    
##### VS 2017 clients:
Open Visual Studio 2017
File->Open Folder->navigate to project root and select "CMakeLists.txt"

See [cmake-support-vs](https://blogs.msdn.microsoft.com/vcblog/2016/10/05/cmake-support-in-visual-studio/) for details regarding how to configure your environment.

##### Xcode clients: 
   
open terminal, from project root:
mkdir build && cd build && cmake -DMACOS=on -G"Xcode" ..
open xcode
File->Open->navigate to project root/build and select "Project.xcodeproj"

See [cmake-Xcode-integration](https://www.johnlamp.net/cmake-tutorial-2-ide-integration.html#section-Xcode) for additional details

## Build
### On Windows using Visual Studio 2017 nmake:
```
   makewin32.cmd
```

### On Mac using make:
```
   ./makemac
   ./makeios
```

### On Linux using make:
```
        ./makelinux
        ./makeaosp
``` 

### How to compile for Android on Windows:

- Unpack the latest Android NDK to c:\android-ndk
- Unpack Ninja-build to c:\ninja
- Add c:\ninja to the path environment variable
- Create a folder under the root of the enlistment called "android", cd into that folder, then run the following command to create ninja build files:
```
    cmake -DCMAKE_ANDROID_NDK=c:/android-ndk ^
        -DCMAKE_ANDROID_NDK_TOOLCHAIN_VERSION=clang ^
        -DCMAKE_SYSTEM_NAME=Android ^
        -DCMAKE_SYSTEM_VERSION=19 ^
        -DCMAKE_ANDROID_ARCH_ABI=x86 ^
        -DCMAKE_ANDROID_STL_TYPE=c++_shared ^
        -DCMAKE_BUILD_TYPE=Release ^
        -DAOSP=on ^
        -G"Ninja" ..
```
To compile, run the following command from the android folder:
```
    ninja    
```
The following native platforms are in development now:

|Platform|Build|Docs|Status|
|---|---|---|---|
| Windows | [Source](https://github.com/Microsoft/msix-packaging/blob/master/sample/ExtractContentsSample/ExtractContentsSample.cpp)| [Docs](https://msdn.microsoft.com/en-us/library/windows/desktop/hh446766(v=vs.85).aspx) | ![Build Status](https://microsoft.visualstudio.com/_apis/public/build/definitions/65a7f9af-fe23-41b6-9aa7-71b0bb348bec/23627/badge) |
| iOS | [Source](https://github.com/Microsoft/msix-packaging/blob/master/sample/ExtractContentsSample/ExtractContentsSample.cpp)| [Docs](https://msdn.microsoft.com/en-us/library/windows/desktop/hh446766(v=vs.85).aspx) | ![Build Status](https://microsoft.visualstudio.com/_apis/public/build/definitions/65a7f9af-fe23-41b6-9aa7-71b0bb348bec/23627/badge) |
| Android | [Source](https://github.com/Microsoft/msix-packaging/blob/master/sample/ExtractContentsSample/ExtractContentsSample.cpp)| [Docs](https://msdn.microsoft.com/en-us/library/windows/desktop/hh446766(v=vs.85).aspx) | ![Build Status](https://microsoft.visualstudio.com/_apis/public/build/definitions/65a7f9af-fe23-41b6-9aa7-71b0bb348bec/23627/badge) |
| MacOS | [Source](https://github.com/Microsoft/msix-packaging/blob/master/sample/ExtractContentsSample/ExtractContentsSample.cpp)| [Docs](https://msdn.microsoft.com/en-us/library/windows/desktop/hh446766(v=vs.85).aspx) | ![Build Status](https://microsoft.visualstudio.com/_apis/public/build/definitions/65a7f9af-fe23-41b6-9aa7-71b0bb348bec/23627/badge) |
| Linux | [Source](https://github.com/Microsoft/msix-packaging/blob/master/sample/ExtractContentsSample/ExtractContentsSample.cpp)| [Docs](https://msdn.microsoft.com/en-us/library/windows/desktop/hh446766(v=vs.85).aspx) | ![Build Status](https://microsoft.visualstudio.com/_apis/public/build/definitions/65a7f9af-fe23-41b6-9aa7-71b0bb348bec/23627/badge) |

## Testing
----------
Unit tests should be run on builds that have the "Release" or "RelWithDebug" CMAKE switch. 

First build the project, then:

  On Windows:
  From within powershell, navigate to test\Win32, and run ".\Win32.ps1"

  On Mac & Linux:
  From within bash, navigate to test/MacOS-Linux, and run "./MacOS-Linux-Etc.sh

Testing on mobile platforms:

  On iOS (currently manual process):
  First build the project for iOS, then launch xCode and load test/mobile/iOSBVT.xcworkspace, compile the test app,
  and then launch the iPhone simulator.

  On Android:
  From within bash, navigate to test/MacOS-Linux, and run "./testaosponmac.sh". The test assumes there's an Android emulator named Nexus_5X_API_19_x86.

## Releasing
------------
If you are the current maintainer of this project:

  1. Pull latest payload to release in master
  2. Confirm that all platforms/architectures/flavors build and all BVTs pass
  3. From a windows cmd prompt: release_master.cmd
  4. Confirm that new branch called "release_v1.xxx" where "xxx" is the next incremental version is created
  
## Contributing
---------------
This project welcomes contributions and suggestions. Most contributions require you to
agree to a Contributor License Agreement (CLA) declaring that you have the right to,
and actually do, grant us the rights to use your contribution. For details, visit
https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need
to provide a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the
instructions provided by the bot. You will only need to do this once across all repositories 
using our CLA.

If you have any questions or comments, you can send them [our team](mailto:MSIXPackagingOSSCustomerQs@service.microsoft.com) directly! 

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/)
or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional 
questions or comments.

## Report a Computer Security Vulnerability
-------------------------------------------
If you are a security researcher and believe you have found a security vulnerability that meets
the [definition of a security vulnerability](https://technet.microsoft.com/library/cc751383.aspx) that is not resolved by the [10 Immutable Laws of Security](https://technet.microsoft.com/library/cc722487.aspx),
please send e-mail to us at secure@microsoft.com. To help us to better understand the nature and
scope of the possible issue, please include as much of the below information as possible.

  * Type of issue (buffer overflow, SQL injection, cross-site scripting, etc.)
  * Product and version that contains the bug, or URL if for an online service
  * Service packs, security updates, or other updates for the product you have installed
  * Any special configuration required to reproduce the issue
  * Step-by-step instructions to reproduce the issue on a fresh install
  * Proof-of-concept or exploit code
  * Impact of the issue, including how an attacker could exploit the issue

Microsoft follows [Coordinated Vulnerability Disclosure](https://technet.microsoft.com/security/dn467923.aspx) (CVD) and, to protect the ecosystem, we 
request that those reporting to us do the same.  To encrypt your message to our PGP key, please
download it from the [Microsoft Security Response Center PGP Key](https://aka.ms/msrcpgp). You should receive a response
within 24 hours. If for some reason you do not, please follow up with us to ensure we received
your original message. For further information, please visit the Microsoft Security Response 
Policy and Practices page and read the [Acknowledgment Policy for Microsoft Security Bulletins](https://www.microsoft.com/technet/security/bulletin/policy.mspx).

For additional details, see [Report a Computer Security Vulnerability](https://technet.microsoft.com/en-us/security/ff852094.aspx) on Technet

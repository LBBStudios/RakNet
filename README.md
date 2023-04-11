# RakNet

Copyright (c) 2014, Oculus VR, Inc.

Copyright (c) 2022-2023, Gie 'Max' Vanommeslaeghe

## Important Note ⚠️
This may be a fork of RakNet, but it doesn't make any promises on compatibility with the RakNet protocol, or other versions of the RakNet engine.
It is also important to remember that this is currently only being tested on Windows, and Linux support is offered as best effort.
Other platforms may work, but cannot be guaranteed. 

## Goals 
* Implement OpenSSL 3.x to replace OpenSSL 1.x being used in the TCPInterface class
* Remove or refactor old sections of code in order to shrink the total size of the library.
* Switch to git submodules as much as possible, rather than relying on a different folder. 
* Replace libcat with a different library for security. 

## Noteworthy changes
* Removed deprecated code
* Fixed cppcheck-reported errors
* Cleaned up old Visual Studio files
* Modernized CMakeLists.txt files
* Removed old samples and general console code. Most of it was empty, or generated warnings. (PS Vita, PS3, PS4, 360, Win8, WinPhone, GFx)

## Package notes
* The Help directory contains index.html, which is full help documentation in HTML format
* The Source directory contain all files required for the core of Raknet and is used if you want to use the source in your program or create your own dll
* The Samples directory contains code samples and one game using an older version of Raknet.  The code samples each demonstrate one feature of Raknet.  The game samples cover several features.
* The lib directory contains libs for debug and release versions of RakNet and RakVoice

## CMake 
CMake is a requirement for building this library; You can download it at https://cmake.org/download/

## Windows builds
* Generate RakNet.sln using CMake
* Open 
* Build required .dll or .lib files

# Linux builds
To build on Linux, simply clone the repository, and excute the following commands:
```
mkdir build
cd build
cmake ..
make
```
Optionally, you can use make -j<processor count -1> to enable multi-threaded compilation. 
This of course assumes that you have gcc, cmake and build-essentials installed. 

### ⚠️ Build instructions underneath this are not tested, and may be incorrect. ⚠️

Mac Users
-----------------------------------------
Open a Terminal window and type:

    cd ~/Desktop/RakNet/Source
    g++ -c -DNDEBUG -I -isysroot /Developer/SDKs/MacOSX10.5u.sdk/ -arch i386 *.cpp

Use whichever SDK you have. However, the 10.4 SDK is bugged and will not compile unless you use GCC 4.0 from inside XCODE

The sources should build cleanly. This gives you a bunch of PowerPC binaries, compiled against the 10.3.9 SDK which is a good thing.

Give the following command:

    libtool -static -o raknetppc.a *.o

This will stitch together a static library for the PowerPC architecture. There may be warnings that some .o files do not have any symbols. If you want to be prudent, remove the named files (the .o files, not the .cpp files!) and re-run the libtool command.

Now, we build the source files for Intel:

    gcc -c -I ../Include -isysroot /Developer/SDKs/MacOSX10.4u.sdk/ -arch i386 *.cpp

..and stitch it into a i386 library:

    libtool -static -o rakneti386.a *.o

Now, type:

    ls *.a

which should list the two .a files. Now, we make them into a universal binary:

    lipo -create *.a -o libraknet.a

You now have a file named libraknet.a. This is the RakNet library, built to run on both PowerPC and Intel Macs. Enjoy! ;-)

Apple Devices (iPhone, iPod)
-----------------------------------------
Depending on what version you target, you may have to change two defines to not use 64 bit integers and floats or doubles.

Android
-----------------------------------------

You will need the latest CYWGIN and the android SDK to build native code on the android. Under CYWGIN, you will need to run ndk-build on a directory for RakNet.

1. Under cygwin, create the RakNet directory somewhere, such as under samples.
For example, if you create the path `\cygwin\home\Kevin\android-ndk-r4b\samples\RakNet`

2. I copied the Android.Manifest.xml and other files from another sample

3. Under jni, you will need the following Android.mk

```
    LOCAL_PATH := $(call my-dir)
    include $(CLEAR_VARS)
    LOCAL_MODULE    := RakNet
    MY_PREFIX := $(LOCAL_PATH)/RakNetSources/
    MY_SOURCES := $(wildcard $(MY_PREFIX)*.cpp)
    LOCAL_SRC_FILES += $(MY_SOURCES:$(MY_PREFIX)%=RakNetSources/%)
    include $(BUILD_SHARED_LIBRARY)
```

This version of Android.mk assumes there is a directory called RakNetSources, for example
`cygwin/home/Kevin/android-ndk-r4b/samples/RakNet/jni/RakNetSources`

Under RakNetSources should be the /Source directory to RakNet. Rather than copy the files I used junction.exe
http://technet.microsoft.com/en-us/sysinternals/bb896768.aspx

The command I used to create the junction was:

    D:/cygwin/home/Kevin/android-ndk-r4b/samples/RakNet/jni/junction.exe -s D:/cygwin/home/Kevin/android-ndk-r4b/samples/RakNet/jni/RakNetSources D:/RakNet4/Source

To unjunction I used:

    D:/cygwin/home/Kevin/android-ndk-r4b/samples/RakNet/jni/junction.exe -d D:/cygwin/home/Kevin/android-ndk-r4b/samples/RakNet/jni/RakNetSources

From within the CYWGIN enviroment, navigate to home/Kevin/android-ndk-r4b/samples/RakNet. Then type

    ../../ndk-build

Everything should build and you should end up with a .so file.

You should then be able to create a project in eclipse, and import cygwin/home/Kevin/android-ndk-r4b/samples/RakNet

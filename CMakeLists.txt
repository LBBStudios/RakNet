cmake_minimum_required(VERSION 3.5)

project(RakNet)

set(CMAKE_CXX_STANDARD 11)

if (NOT APPLE)
    # check 64 bit
    if (CMAKE_SIZEOF_VOID_P MATCHES "4")
        set(HAVE_64_BIT 0)
    else (CMAKE_SIZEOF_VOID_P MATCHES "4")
        set(HAVE_64_BIT 1)
    endif (CMAKE_SIZEOF_VOID_P MATCHES "4")
endif (NOT APPLE)

IF (WIN32 AND NOT UNIX)
    set(PROGRAMFILESX86 $ENV{PROGRAMFILES})
    string(REPLACE "\\" "/" PROGRAMFILESX86 ${PROGRAMFILESX86})
ENDIF (WIN32 AND NOT UNIX)

set(CMAKE_DEBUG_POSTFIX "d")

IF (WIN32 AND NOT UNIX)
    set(RAKNET_LIBRARY_LIBS ws2_32.lib)
    set(RAKNET_LIBRARY_SO_NAME RakNetDLL)
ELSE (WIN32 AND NOT UNIX)
    set(RAKNET_LIBRARY_LIBS pthread)
    set(RAKNET_LIBRARY_SO_NAME raknet)
ENDIF (WIN32 AND NOT UNIX)


# Options
option(RAKNET_ENABLE_SAMPLES "Generate RakNet sample projects." FALSE)
option(RAKNET_ENABLE_DLL "Generate the DLL project." FALSE)
option(RAKNET_ENABLE_STATIC "Generate the static library project." TRUE)

set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/CmakeIncludes)

include(./CmakeIncludes/CmakeMacros.txt)

if (MSVC)
    FIXLINKOPTIONS()
    FIXCOMPILEOPTIONS()
endif (MSVC)


include_directories(SYSTEM include/raknet DependentExtensions Source)

add_subdirectory(Lib)

set(RAKNET_COMMON_LIBS RakNetLibStatic)

if (RAKNET_ENABLE_SAMPLES)
    add_subdirectory(Samples)
endif ()

cmake_minimum_required(VERSION 2.8.4)

project(atla)

if((NOT ${CMAKE_SYSTEM_NAME} MATCHES "Linux") AND (NOT DEFINED libusb_USE_STATIC_LIBS))
    set(libusb_USE_STATIC_LIBS YES)
endif((NOT ${CMAKE_SYSTEM_NAME} MATCHES "Linux") AND (NOT DEFINED libusb_USE_STATIC_LIBS))

find_package(libusb REQUIRED)

set(LIBPIT_INCLUDE_DIRS
    ../libpit/source)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11")

if(MINGW)
    set(CMAKE_EXE_LINKER_FLAGS "-static-libgcc -static-libstdc++ -static")
endif(MINGW)

if(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    add_definitions(-DOS_LINUX)
endif(${CMAKE_SYSTEM_NAME} MATCHES "Linux")

include_directories(SYSTEM ${LIBUSB_INCLUDE_DIRS})

include_directories(${LIBPIT_INCLUDE_DIRS})

set(HEIMDALL_SOURCE_FILES
    source/Arguments.cpp
    source/BridgeManager.cpp
    source/ClosePcScreenAction.cpp
    source/DetectAction.cpp
    source/DownloadPitAction.cpp
    source/FlashAction.cpp
    source/HelpAction.cpp
    source/InfoAction.cpp
    source/Interface.cpp
    source/main.cpp
    source/PrintPitAction.cpp
    source/Utility.cpp
    source/VersionAction.cpp)

include(LargeFiles)
use_large_files(atla YES)
add_executable(atla ${HEIMDALL_SOURCE_FILES})

target_link_libraries(atla PRIVATE pit)
target_link_libraries(atla PRIVATE ${LIBUSB_LIBRARY})
install (TARGETS atla
		RUNTIME	DESTINATION ${CMAKE_INSTALL_PREFIX}/bin
		LIBRARY	DESTINATION ${CMAKE_INSTALL_LIBDIR})


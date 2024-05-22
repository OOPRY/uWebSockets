cmake_minimum_required(VERSION 3.5)
set(CMAKE_BUILD_TYPE Release)
set(CMAKE_CXX_STANDARD 11)
project (µWebSockets)

option(BUILD_SHARED_LIBS "Build shared libraries." OFF)

find_package(OpenSSL REQUIRED)
#find_package(ZLIB REQUIRED)

add_subdirectory(uSockets)

if(NOT LIBUV_INCLUDE_DIR)
    find_path(LIBUV_INCLUDE_DIR uv.h)
endif()
if(NOT LIBUV_LIBRARY)
    find_library(LIBUV_LIBRARY NAMES uv uv1)
endif()

add_library(uWS INTERFACE)
if(CMAKE_VERSION VERSION_LESS "3.1")
    # CMake 3.0 does not implement "CMAKE_CXX_STANDARD":
    if(CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
        set(CMAKE_CXX_FLAGS "--std=gnu++11 ${CMAKE_CXX_FLAGS}")
    endif()
endif()
target_include_directories(uWS INTERFACE src)

#if (UNIX)
target_link_libraries (uWS INTERFACE pthread uSockets zlib)
#endif (UNIX)

#add_subdirectory(examples)
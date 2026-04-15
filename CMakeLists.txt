cmake_minimum_required(VERSION 3.5)

project(InfoNES)
# =============================
# Build Instructions
# =============================
#
# Requirements:
#   - SDL 1.2 development libraries (libsdl1.2-dev)
#
# On Debian/Ubuntu, install SDL 1.2 with:
#   sudo apt install libsdl1.2-dev
#
# Standard 64-bit build:
#   mkdir build && cd build
#   cmake ..
#   make
#
# 32-bit build (on a 64-bit system):
#   1. Install 32-bit libraries:
#        sudo dpkg --add-architecture i386
#        sudo apt update
#        sudo apt install gcc-multilib g++-multilib libsdl1.2-dev:i386
#   2. Build with 32-bit support:
#        mkdir build32 && cd build32
#        cmake -DBUILD_32BIT=ON ..
#        make
# =============================
option(BUILD_32BIT "Build for 32-bit" OFF)
if(BUILD_32BIT)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -m32")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -m32")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -m32")
    set(CMAKE_FIND_ROOT_PATH "/usr/lib/i386-linux-gnu")
    set(CMAKE_LIBRARY_ARCHITECTURE "i386-linux-gnu")
endif()

find_package(SDL REQUIRED SDL)
include_directories(${SDL_INCLUDE_DIRS})

file(GLOB SRCS
        src/*.cpp
        src/sdl/*.cpp
        )
add_executable(${PROJECT_NAME} ${SRCS})
target_link_libraries(${PROJECT_NAME} ${SDL_LIBRARIES})

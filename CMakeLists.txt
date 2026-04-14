cmake_minimum_required(VERSION 3.5)

project(InfoNES)
# On a 64 bit system thedefault build is 64-bit, but you can set BUILD_32BIT to ON to build for 32-bit
# note that you need to have 32-bit libraries installed (e.g. gcc-multilib g++-multilib libsdl1.2-dev:i386 
# on Debian/Ubuntu) to build for 32-bit:
#  sudo dpkg --add-architecture i386
#  sudo apt update
#  sudo apt install gcc-multilib g++-multilib libsdl1.2-dev:i386
# to build for 32-bit, run cmake with -DBUILD_32BIT=ON:
#  mkdir build32 && cd build32
#  cmake -DBUILD_32BIT=ON ..
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

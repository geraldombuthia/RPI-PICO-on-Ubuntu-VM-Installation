# Setting up Ubuntu on Oracle Virtual box

I had to do this since my windows machine contained other working compilers that for some reason interfered with the build process of the plico machine code and I ended up having a build where the Found assembler was mingw/gcc.exe instead of the arm-none-eabi

Below is a step by step guide on achieving that

## Introduction
The Raspberry Pi Pico SDK is a collection of libraries, headers, and tools that allows to develop programs for the RP2040 based devices such as the Raspberry Pi Pico using C, C++ or assembly language.

This tutorial explains how to set up Raspberry Pi Pico SDK on Ubuntu 22.04.

## Prepare environment

Have in your system the build essential package in your system

```
sudo apt update

sudo apt install -y build-essential 

```

You also need to have the following installed

* Git
* CMake
* Arm GNU ToolChain

## Install SDK

Clone the following repository
```
sudo git clone https://github.com/raspberrypi/pico-sdk.git /opt/pico-sdk

```

Initialize submodules
```
sudo git -C /opt/pico-sdk submodule update --init

```

Set the PICO_SDK_PATH environment variable which specifies where SDK is installed

```
echo 'export PICO_SDK_PATH=/opt/pico-sdk' | sudo tee -a /etc/profile.d/pico-sdk.sh
```

To make changes take effect immediately use:
```
source /etc/profile.d/pico-sdk.sh
```

## Build Program

Create a new directory to store project files and navigate to this directory:

```mkdir helloworld && cd helloworld```
Create a main.c file:

```nano main.c```
Once the file is opened, add the following lines of code:

```helloworld/main.c```

```#include <stdio.h>
#include <pico/stdlib.h>

int main()
{
    stdio_init_all();

    while (true) {
        printf("Hello world\n");
        sleep_ms(1000);
    }
}```
Create a CMakeLists.txt file:

```nano CMakeLists.txt```
Add the following content:

helloworld/CMakeLists.txt

```cmake_minimum_required(VERSION 3.13)

include($ENV{PICO_SDK_PATH}/external/pico_sdk_import.cmake)

project(myapp C CXX ASM)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

pico_sdk_init()

add_executable(${PROJECT_NAME} main.c)

pico_add_extra_outputs(${PROJECT_NAME})

target_link_libraries(${PROJECT_NAME} pico_stdlib)

pico_enable_stdio_usb(${PROJECT_NAME} 1)
pico_enable_stdio_uart(${PROJECT_NAME} 0)```
Create a build directory and navigate to it:

```mkdir build && cd build```
Prepare CMake build directory by running the following command:

```cmake ..```
Now run the make command to build program:

```make -j$(nproc)```
Using the ls command, you can check a list of generated files.

```CMakeCache.txt  cmake_install.cmake  generated  myapp.bin  myapp.elf      myapp.hex  pico-sdk
CMakeFiles      elf2uf2              Makefile   myapp.dis  myapp.elf.map  myapp.uf2```
The myapp.uf2 is a program which can be moved into storage of the Raspberry Pi Pico.
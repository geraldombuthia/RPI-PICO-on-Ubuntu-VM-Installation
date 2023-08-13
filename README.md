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

clone the following repository
```
sudo git clone https://github.com/raspberrypi/pico-sdk.git /opt/pico-sdk

```

initialize submodules
```
sudo git -C /opt/pico-sdk submodule update --init

```

set the PICO_SDK_PATH environment variable which specifies where SDK is installed

```
echo 'export PICO_SDK_PATH=/opt/pico-sdk' | sudo tee -a /etc/profile.d/pico-sdk.sh
```

to make changes take effect immediately use:
```
source /etc/profile.d/pico-sdk.sh
```

## Build Program
# Documentation


## Supported Boards

- [Globalscale MW302 AWS IoT Starter Kit](./starter-kit/)
- AzureWave EVB and EVBA
- [Makerville Knit](./makerville-knit/)


## Get the SDK

 [ Download .zip >][download] or use git

    git clone https://github.com/marvell-iot/ez-connect-lite


[download]: https://github.com/marvell-iot/ez-connect-lite/archive/master.zip

## Setup Development Host

Make sure you have followed the OS specific instructions listed below before proceeding further -

- [Windows](./windows-host-setup/)
- [Linux](./linux-host-setup/)
- [Mac](./mac-host-setup/)

### Install ARM Cross-compiler Toolchain

Apart from installing development host specific drivers and packages as mentioned above, an ARM cross-compiler toolchain should be installed on the development host. The EZ-Connect Lite SDK supports the GCC Compiler Toolchain.

The toolchain for GNU ARM is available from: https://launchpad.net/gcc-arm-embedded

Make sure that the toolchain is added to your environment path.

```
$ arm-none-eabi-gcc --version
arm-none-eabi-gcc (GNU Tools for ARM Embedded Processors) 4.9.3 20150529 (release) [ARM/embedded-4_9-branch revision 227977]
Copyright (C) 2014 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

```

### Eclipse IDE

You can find instructions for [setting up Eclipse here](./eclipse/). Eclipse C/C++ IDE is great tool for which we have support in the EZ-Connect IDE. Although it is not necessary to use it to build applications, it's a great resource if you are getting started.


## Building the SDK and Applications

The following command builds the entire SDK including all the libraries and sample applications:

```
$ cd ez-connect-lite
$ make
```

You could compile individual sample application by passing the ```APP=``` parameter to ```make```.

```
$ make APP=sample_apps/hello_world
```

To build applications for a particular development board, pass the ```BOARD_FILE=``` parameter to ```make```

```
$ make APP=sample_apps/hello_world BOARD_FILE=sdk/src/boards/knit-v1.c
```


## Uploading Your Code

There are 2 ways of executing your code on the micro-controller boards.

- RAM load
- Flash load

Before going into detail about them, let's check out the firmware files that are created when you compile an application using the SDK -

### Firmware Variants


When you compile an application, a `.axf`, `.map`, and `.bin` are created.

```
$ make APP=sample_apps/hello_world BOARD_FILE=sdk/src/boards/knit-v1.c  

make[1]: Entering directory '/home/user/ez-connect-lite'
 [cc] /home/user/ez-connect-lite/sdk/src/boards/knit-v1.c
 [axf] /home/user/ez-connect-lite/bin/knit-v1/hello_world.axf
 [map] /home/user/ez-connect-lite/bin/knit-v1/hello_world.map
 [bin] /home/user/ez-connect-lite/bin/knit-v1/hello_world.bin

make[1]: Leaving directory '/home/user/ez-connect-lite'

```

- `hello_world.axf`: This is a firmware image that can directly be loaded into the SRAM of the micro-controller. Since the firmware image is not written to flash, this operation is faster. This is commonly used for iterative development.
- `hello_world.map`: This is a file which helps us to understand where all the symbols are located in the memory after linking. This can be used for advanced debugging, and should be ignored otherwise.
- `hello_world.bin`: This is a firmware image that can be flashed on to the starter kit. This is commonly used as the default firmware that gets executed on boot up.


Let's take a closer look at how to use these files to run your application on the development boards -

### RAM Load

We will be using the `ramload.py` python script that can be found in the `sdk/tools/OpenOCD` directory. This is used to upload the `axf` firmware to the development board.

```
$ python sdk/tools/OpenOCD/ramload.py --help

Usage:
sdk/tools/OpenOCD/ramload.py <app.axf> [options]
Optional Usage:
 [<-i | --interface> <JTAG hardware interface name>]
          Supported ones are ftdi and stlink. Default is ftdi.
 [-s | --semihosting]
          Enable semihosting based console output
 [-h | --help]
          Display usage

```

For example, to upload a sample application to the RAM of the controller using the `stlink` interface, we will run

```
$ python sdk/tools/OpenOCD/ramload.py -i stlink bin/knit-v1/hello_world.axf
```

You should keep it in mind that reseting the board or shutting down the power will wipe the RAM. So code uploaded via `ramload.py` is not persistent across resets/power cycles.

### Flash Load

We will be using the `flash.py` python script that can be found in the `sdk/tools/OpenOCD` directory. This is used to upload the `bin` firmware to the development board.

```
$ python sdk/tools/OpenOCD/flash.py --help

Usage:
sdk/tools/OpenOCD/flash.py [options]
Optional Usage:
 [<-i | --interface> <JTAG hardware interface name>]
          Supported ones are ftdi and stlink. Default is ftdi.
 [--mcufw </path/to/mcufw>]
          Write MCU firmware binary <bin> to flash
 [<-f | --flash> </path/to/flash_blob>]
          Program entire flash
 [-r | --reset]
          Reset board
 [-h | --help]
          Display usage
```

For example, to upload a sample application to the flash of the development board using the stlink interface, we will run

```
$ python sdk/tools/OpenOCD/flash.py -i stlink --mcufw bin/knit-v1/hello_world.bin
```

Since the application gets stored on the flash memory of the development board/module, it is persistent across resets/power cycles.


## Tutorials

- [Hello World](./hello-world/)
- [Wi-Fi basics](./wifi-basics/)
- [PinMux](./pinmux/)
- [AWS IoT](./aws-iot/)

## Projects

Here's a list of projects that people have made using the Marvell SDK.

- <a href="https://www.hackster.io/cubot/cubled-79119f" target="_blank">CUBLED</a> - Control a 4x4x4 LED cube using AWS IoT
- <a href="https://www.hackster.io/anujdeshpande/state-of-the-city-b81d85" target="_blank">State of the City</a> - Create a map with sensor data from different cloud connected sensors
- More projects on <a href="https://www.hackster.io/marvell/projects" target="_blank">Hackster</a>

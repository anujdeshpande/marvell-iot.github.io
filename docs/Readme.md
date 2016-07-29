# Documentation


## Get the SDK

 [ Download .zip >][download] or use git

    git clone https://github.com/marvell-iot/ez-connect-lite


[download]: https://github.com/marvell-iot/ez-connect-lite/archive/master.zip

## Setup development host

The following operating systems are supported -

- [Windows](./windows-host-setup/)
- [Linux](./linux-host-setup/)
- [Mac](./mac-host-setup/)

## Install ARM cross-compiler toolchain

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

## Building the SDK and applications

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

## Firmware Variants

Whenever an application firmware is built, two kinds of artifacts are created

- **hello_world.axf:** This is a firmware image that can directly be loaded into the SRAM of the micro-controller using the ```ramload.py``` program. Since the firmware image is not written to flash, this operation is faster. This is commonly used for iterative development.
- **hello_world.bin:** This is a firmware image that can be flashed on to the starter kit. This is commonly used as the default firmware that gets executed on boot up.

## Uploading your code

There are 2 ways of executing your code on the micro-controller boards.

- RAM load
- Flash load

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

- `.axf` file 

Let's take a closer look at how to use these files to run your application on the development boards -

### RAM load

We will be using the `ramload.py` python script that can be found in the `sdk/tools/OpenOCD` directory.

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


### Flash load

We will be using the `flash.py` python script that can be found in the `sdk/tools/OpenOCD` directory.

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

## Tutorials

### [Develop applications using the SDK](./developing-with-the-sdk/)
### [Connect to AWS IoT](./aws-iot/)
### [Setting up Eclipse](./eclipse/)

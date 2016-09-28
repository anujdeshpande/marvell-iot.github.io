
# Eclipse

If you are using Eclipse, make sure that you have installed the IDE as well as the required plugins as mentioned in our guide [here](../eclipse/). Once you have completed the steps mentioned in the guide you can go through the sections below -

##  Building sample applications
Next we are going to generate the binaries.

* Project -> Clean...

* Project -> Build All

## Configure debug & external tools

The EZ Connect Lite SDK consists of a `.settings` folder which contains Debug & External Tools launchers. 


### Debug launchers :

1. Debug -> Organize Favourites
<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/orgfavs.png" width=450>
2. Select Add
3. Check Select All -> Ok -> Ok
<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/debuglaunchers.png" width=450>

### External tools launchers :

1. External tools -> Organize Favourites
2. Select Add
3. Select All -> Ok -> Ok
<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/externaltools.png" width=450>


## Use the launchers

### Debug launchers 

1. Debug.launch  
It loads the selected application `axf` file using `arm-none-eabi-gdb` and halts at application `main()`. It can be used to debug non-XIP applications from beginning

2. Live Debug.launch  
It connects to an already running application on hardware and halts at the current instruction. It only loads the debugging symbols from the selected application `axf` file.
This launcher can also be used to debug XIP applications already flashed using `Program MCU Firmware` launcher.


* From the binaries folder in the project view, select the .axf that you would like to debug.
* Press the drop down next to the Debug symbol and select either Debug or Live Debug.
* Select `Use configuration specific settings` and select `GDB Hardware Debugging Launcher`
<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/Debug.png" width=450>
* Press Ok and Select `Yes` on the dialog that pops up next asking permission to open a new perspective.
* A new perspective should open which should look something like the following screenshot.
<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/debugperspective.png" width=450>


### External tools launchers

- Load in Memory  
It loads the selected non-XIP application `axf` file on hardware. Internally it uses ramload.py script to load the selected axf file. The `.axf` files can be found under the `Binaries` section in Project Explorer in Eclipse. Loading the application in the memory/SRAM is faster and handy for development. Note that if your application requires files from the `ftfs` section as well as the Wi-Fi firmware, they have to be flashed first to the flash using `Program FTFS` and `Program Wi-Fi Firmware` launchers.

- Prepare Flash  
This erases the contents of the flash using the `flashprog.py` command. The layout that needs to be written can be found at  `sdk/tools/OpenOCD/mw300/layout.txt`. Make sure that you have selected this in the Project Explorer window in Eclipse.

- Program Boot2  
This flashes the bootloader to the flash on your development board. The bootloader always needs to be present. You can find it at `boot2/boot2.bin`.

- Program FTFS  
This flashes the filesystem to the flash on your development board. An example of this can be found in AWS Starter Demo in `sample_apps/aws_starter_demo`. A `www` folder is created which has the HTML, JS and other assets that you would like to server over the HTTP server. In the case of the `aws_starter_demo` these are required for provisioning the AWS credentials as well as the Wi-Fi SSID and passphrase. You can the build instructions for the `ftfs` partition  in the `build.mk` of the said sample application

```
#ifneq ($(wildcard $(d)/www),)
aws_starter_demo-ftfs-y := aws_starter_demo.ftfs
aws_starter_demo-ftfs-dir-y     := $(d)/www
aws_starter_demo-ftfs-api-y := 100
#endif
```
- Program MCU Firmware  
It flashes the selected application 'bin' file in the flash. Internally it uses flashprog.py script to burn selected bin file to the flash.

- Program Wi-Fi Firmware  
This flashes the Wi-Fi firmware to the flash on your development board. You can find the firmware at `wifi-firmware/mw30x/mw30x_uapsta_14.76.36.p103.bin.xz`

- Select Debug Interface  
You can use this launcher to connect to the micro-controller using different debug interfaces. The supported one's are found in `sdk/tools/OpenOCD/interface/` folder. Some development boards have a FTDI chip onboard which is used by default. If you are using Makerville Knit or your own custom board and would like to use  the ST Link debugger, you will have to change the default using this launcher.  

#### Note

**Linux** Users must once execute script sdk/tools/bin/perm_fix.sh to use OpenOCD and serial console in Eclipse.





# Command Line

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

## Factory
These are blobs of code that your application will depend on, but you won't be able to build them from source.

### boot2
This is the bootloader, which helps the uController to boot up. This is made available as a binary in the folder `boot2/boot2.bin`.
If you simply want to flash the bootloader, you can use

```
$ python sdk/tools/OpenOCD/flashprog.py --boot2 boot2/boot2.bin
```

### wififw
This is the standard Wi-Fi firmware that the controller requires. This is made available as a binary in the folder `wififw/mw30x`
If you simply want to flash the Wi-Fi firmware, you can use

```
$ python sdk/tools/OpenOCD/flashprog.py --wififw wifi-firmware/mw30x/mw30x_uapsta_14.76.36.p103.bin.xz
```

## User
When you compile an application, `.axf`, `.map`, and `.bin` files are created in the `bin/` folder.
You can optionally create a `.ftfs` by adding the following line to your `sample_apps/<app-name>/build.mk`. This will ensure that all the contents of your `<app-name>/www` directory make it to the generated `ftfs`

```
# Copyright (C) 2008-2016 Marvell International Ltd.
# All Rights Reserved.
#

exec-y                    += <app-name>
<app-name>-objs-y         := src/main.c
<app-name>-cflags-y       := -I$(d)/src -DAPPCONFIG_DEBUG_ENABLE=1

#ifneq ($(wildcard $(d)/www),)
<app-name>-ftfs-y         := <app-name>.ftfs
<app-name>-ftfs-dir-y     := $(d)/www
<app-name>-ftfs-api-y     := 100
#endif

```

### axf
This is a firmware image that can directly be loaded into the SRAM of the micro-controller. Since the firmware image is not written to flash, this operation is faster. This is commonly used for iterative development.

### map
This is a file which helps us to understand where all the symbols are located in the memory after linking. This can be used for advanced debugging, and should be ignored otherwise.

### bin
This is a firmware image that can be flashed on to the starter kit. This is commonly used as the default firmware that gets executed on boot up.

### ftfs
This is used to store files on the controller. This is usually used to save the web page that the http server serves. As the controller is connected to a flash which is usually 2/4MB, the files and hence the `ftfs` has to be extremely small.
## Building sample applications
```
$ make APP=sample_apps/hello_world BOARD_FILE=sdk/src/boards/knit-v1.c  

make[1]: Entering directory '/home/user/ez-connect-lite'
 [cc] /home/user/ez-connect-lite/sdk/src/boards/knit-v1.c
 [axf] /home/user/ez-connect-lite/bin/knit-v1/hello_world.axf
 [map] /home/user/ez-connect-lite/bin/knit-v1/hello_world.map
 [bin] /home/user/ez-connect-lite/bin/knit-v1/hello_world.bin

make[1]: Leaving directory '/home/user/ez-connect-lite'

```
## Flashing

There are 2 ways of executing your code on the micro-controller boards.

- RAM load
- Flash load

Before going into detail about them, let's check out the firmware files that are created when you compile an application using the SDK -



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

We will be using the `flashprog.py` python script that can be found in the `sdk/tools/OpenOCD` directory.

```
$ python sdk/tools/OpenOCD/flashprog.py --help

Usage:
sdk/tools/OpenOCD/flashprog.py [options]
Optional Usage:
 [<-i | --interface> <JTAG hardware interface name>]
          Supported ones are ftdi, and stlink. Default is ftdi.
 [<-l | --new-layout> </path/to/layout_file>]
          Flash partition using layout file <layout.txt>  
 [<--comp_name> </path/to/comp_file>]
          Write any component <comp_name> file <comp_file> to flash
 [-d | --debug]
          Debug mode, dump current flash contents
 [<-b | --batch> </path/to/config_file>]
          Batch processing mode config file <config.txt>
 [--read <flash_id>,<start_address>,<length>,<path/to/output_file>]
          Read flash <flash_id> (0: Internal QSPI, 1: External QSPI, 2: External SPI)
          starting from <start_address> of length <length> into output file <output_file>
 [--write <flash_id>,<start_address>,<path/to/input_file>]
          Write to flash <flash_id> (0: Internal QSPI, 1: External QSPI, 2: External SPI)
          starting from <start_address> from input file <input_file>
          Note: '-w | --write' option does not erase the flash before writing.
          If required, explicitly erase the flash before.
 [--erase <flash_id>,<start_address>,<length>]
          Erase flash <flash_id> (0: Internal QSPI, 1: External QSPI, 2: External SPI)
          starting from <start_address> and length <length>
 [--erase-all <flash_id>]
          Erase entire flash <flash_id> (0: Internal QSPI, 1: External QSPI, 2: External SPI)
 [<-p | --prod> </path/to/mfg_file>]
          Batch processing mode mfg file <mfg.txt>
 [-r | --reset]
          Reset board
 [--interactive]
          Run flashprog in interactive mode (Default: non-interactive)
 [-h | --help]
          Display usage
```

For example, to upload a sample application to the flash of the development board using the stlink interface, we will run

```
$ python sdk/tools/OpenOCD/flashprog.py -i stlink --mcufw bin/knit-v1/hello_world.bin
```

Since the application gets stored on the flash memory of the development board/module, it is persistent across resets/power cycles.

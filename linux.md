# Linux
Latest releases of most Linux distributions should work with the SDK.

## Installing Packages
The SDK needs 'binutils' package to run the SDK tools. Please run the following command to install the package:
```
$ sudo yum install binutils (for fedora)
$ sudo apt-get install binutils (for ubuntu)
```
Additionally, 64-bit Linux distributions require '32-bit libc' package to run 32-bit host tools. Please run the following command to install the package:
```
$ sudo yum install glibc.i686 (for fedora)
$ sudo apt-get install libc6:i386 (for ubuntu)
```

## Setting up Serial Console
* Once the Starter Kit is connected to your Linux host, two new ttyUSB devices will be detected. This can be verified with ```dmesg```. The second of these devices, in this case the ttyUSB1, is the device for the serial console.


      $ dmesg | grep ttyUSB
      [7337563.575701] usb 3-4: FTDI USB Serial Device converter now attached to ttyUSB0
      [7337563.576290] usb 3-4: FTDI USB Serial Device converter now attached to ttyUSB1

* We will use the _minicom_ program for accessing the serial console.
* For initial configuration of minicom, execute the program in setup mode as (minicom -s).
* Go to Serial Port Setup
* Perform the following settings:

```
    | A -    Serial Device      : /dev/ttyUSB1
    | B – Lockfile Location     : /var/lock
    | C -   Callin Program      :
    | D -  Callout Program      :
    | E -    Bps/Par/Bits       : 115200 8N1
    | F – Hardware Flow Control : No
    | G – Software Flow Control : No
```
where Serial Device is the second of the ttyUSB device that was detected on your system.

* Save the settings as default, and then relaunch minicom with the '-s' parameter.
* One pressing reset button on the starter kit, you should start seeing console messages that are generated by the firmware.



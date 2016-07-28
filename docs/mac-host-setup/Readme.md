OS X releases Yosemite and El Capitan can be used to develop with the SDK.

## Installing OpenOCD (using ports)
* Install macports from [Macports Site](http://www.macports.org/install.php). If you prefer brew, please refer to the end of this page for the instructions. Restart your terminal window so that it finds the port utility in its execution path. Then execute following commands in the terminal window

```
$ which port
$ sudo port selfupdate
```
* Install OpenOCD version 0.9.0 for Mac (Make sure the starter kit is still connected to the Mac)

```
$ sudo port install openocd @0.9.0
```

## Installing OpenOCD (using brew)
You may skip this section, if you have already installed OpenOCD using the ports tool above.

* Install brew

```
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
* Install OpenOCD version 0.9.0 for Mac (Make sure the starter kit is still connected to the Mac)

```
$ brew info openocd
$ brew install openocd
```

## Fix Driver Conflict

* Check the usbserial ports using the following command

```
$ ls -l  /dev/tty.usbserial*
crw-rw-rw-  1 root  wheel   18,   6 Nov 17 09:53 /dev/tty.usbserial-142A
crw-rw-rw-  1 root  wheel   18,   8 Nov 17 09:53 /dev/tty.usbserial-142B
```
* To use OpenOCD, we have to ensure that Apple's FTDI drivers are available for the USB-serial profile, while OpenOCD continues to use the JTAG interface. This can be done in the following way

```
$ sudo kextunload -p -b com.apple.driver.AppleUSBFTDI
$ sudo kextutil -b com.apple.driver.AppleUSBFTDI -p AppleUSBEFTDI-6010-1
```
* To verify that the unload has worked, rerun the ```ls``` command. You should now see only one ```usbserial``` device

```
$ ls -l  /dev/tty.usbserial*
crw-rw-rw-  1 root  wheel   18,  10 Nov 17 09:56 /dev/tty.usbserial-142B
```
* **NOTE** : You should not have to run this after disconnecting and connecting the board again. You will have to run this after a reboot.

## Setting up Serial Console
- Once the above settings are done, and the board is plugged in to your development host, a virtual USB device will be created for you. Please lookup the name of this device. The device will be of the form /dev/tty.usbserial-XXXX.
- Install any application that can read serial console (e.g. minicom) as:

```
$ sudo port install minicom
```
- Execute minicom in setup mode (minicom –s)
- Go to Serial Port Setup
- Perform the following settings:

```
    | A -    Serial Device      : /dev/tty.usbserial-XXXX
    | B – Lockfile Location     : /var/lock
    | C -   Callin Program      :
    | D -  Callout Program      :
    | E -    Bps/Par/Bits       : 115200 8N1
    | F – Hardware Flow Control : No
    | G – Software Flow Control : No
```
You can save these settings in minicom for future use. The minicom window will now show messages from the serial console.

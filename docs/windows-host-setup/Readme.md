## Installing Packages
* Install the following GnuWin32 packages from http://gnuwin32.sourceforge.net/packages.html:

 * coreutils
 * make
 * which

  Add GnuWin32 installation path to the system path.
* Install Python 2.7 for Windows from https://www.python.org/. Make sure python is added to the system path during installation.

## Getting OpenOCD

* Get OpenOCD 0.9.0 for Windows from [here](http://www.freddiechopin.info/en/download/category/4-openocd?download=118%3Aopenocd-0.9.0)

* Unzip the OpenOCD tarball and copy the contents of bin/ folder (for 32-bit Windows machines) or bin-x64/ folder (for 64-bit Windows machines) to __ez-connect-lite/sdk/tools/OpenOCD/Windows__ folder.

````
copy \path\to\OpenOCD-0.9.0\bin\* \path\to\ez-connect-lite\sdk\tools\OpenOCD\Windows
OR
copy \path\to\OpenOCD-0.9.0\bin-x64\* \path\to\ez-connect-lite\sdk\tools\OpenOCD\Windows
````
## Installing WinUSB Driver
* Download WinUSB driver installer from http://zadig.akeo.ie/.
* Connect the starter kit to the PC, let default Windows driver install first. After this,
   * Launch Zadig WinUSB driver installer
   * Click on options->List all Devices
   * Select Dual RS232 (Interface 0) in the dropdown list
   * Select the driver WinUSB for this device
   * Install the driver

## Getting Serial Console
Once the appropriate drivers are installed and the development board is connected to your Windows host, virtual COM port devices are added to your systems. Depending upon your Windows configuration, you may see one or two COM ports. These can be verified as follows:

- Right click on My Computer and go to Properties
- Hardware --> Device Manager
- Under "Ports (COM & LPT), you may find the entries of the form "USB Serial Port (COMx)"

This can be used to access the serial console of the development board.

The serial port can be accessed using any serial communication application like HyperTerminal or Putty. Please perform the following settings for your serial console application:
```
Device: "USB Serial Port (COMx)"
Bits per Second: 115200
Data Bits: 8
Parity: N
Stop Bits: 1
Flow Control: None
```
If two COM ports are listed on your system, please select the second of the COM ports for the serial console.

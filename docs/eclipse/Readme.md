This page documents how to setup Eclipse as a development IDE for the available development boards.


### Step 0 : Install prerequisites

Depending on your platform, follow the instructions given below to install the requisite tools :

1. [Windows](http://marvell-iot.github.io/docs/windows-host-setup/)
2. [Mac OS X](http://marvell-iot.github.io/docs/mac-host-setup/)
3. [Linux](http://marvell-iot.github.io/docs/linux-host-setup/)

### Step 1 : Install Eclipse Mars

* [Install Java](http://www.java.com/inc/BrowserRedirect1.jsp?locale=en)
* [Download](http://www.eclipse.org/downloads/packages/eclipse-ide-cc-developers/mars1) Eclipse Mars IDE for C/C++ developers.
Extract it and launch the executable.


### Step 2 : Install plugins
Install the following plugins from Help -> Install New Software

1. C/C++ GDB Hardware Debugging
2. TM Terminal

<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/AllPlugins.PNG" width=450>


### Step 3 : Import aws_starter_sdk project

We will now import the project into Eclipse using git.

* File -> Import
<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/import.png" width=450>

* Git -> Projects from Git
<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/localrepo.PNg" width=450>

* In the Directory dialog box, press Browse and set the path to where you cloned the repo in Step 0 : prerequisites.
<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/setpath.png" width=450>

* Press Next and then Finish.

The project along with the settings has been now imported in your Eclipse IDE.

### Step 4 : Build sample apps
Next we are going to generate the binaries.

* Project -> Clean...

* Project -> Build All

### Step 5 : Configure debug & external tools

The aws_starter_sdk consists of a .settings folder which contains Debug & External Tools launchers.


#### Steps to add the Debug launchers :

1. Debug -> Organize Favourites
<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/orgfavs.png" width=450>
2. Select Add
3. Check Select All -> Ok -> Ok
<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/debuglaunchers.png" width=450>

#### Steps to add External tools launchers :

1. External tools -> Organize Favourites
2. Select Add
<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/externaltools.png" width=450>
3. Select All -> Ok -> Ok



Following debug launchers are added:

1. Debug.launch  
It loads the selected application 'axf' file using arm-none-eabi-gdb and halts at application main(). It can be used to debug non-XIP applications from beginning
2. Live Debug.lauch  
It connects to an already running application on hardware and halts at the current instruction. It only loads the debugging symbols from the selected application 'axf' file.
This launcher can also be used to debug XIP applications already flashed using 'Program MCU Firmware' launcher.

Following External tools launchers are added:

1. Load in Memory.launch  
It loads the selected non-XIP application 'axf' file on hardware. Internally it uses ramload.py script to load the selected axf file.
2. Program MCU Firmware.launch  
It flashes the selected application 'bin' file in the flash. Internally it uses flash.py script to burn selected bin file in the flash.
3. Program Recovery Image.launch  
It flashes the selected recovery flash 'blob' in the flash. Internally it uses flash.py script to burn the selected recovery flash in the flash.


#### Note
**Linux** Users must once execute script sdk/tools/bin/perm_fix.sh to use OpenOCD and serial console in Eclipse.


### Step 7 : Use the launchers

* From the binaries folder in the project view, select the .axf that you would like to debug.
* Press the drop down next to the Debug symbol and select either Debug or Live Debug.
* Select `Use configuration specific settings` and select `GDB Hardware Debugging Launcher`
<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/Debug.png" width=450>
* Press Ok and Select `Yes` on the dialog that pops up next asking permission to open a new perspective.
* A new perspective should open which should look something like the following screenshot.
<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/debugperspective.png" width=450>

# Eclipse

This page documents how to setup Eclipse as a development IDE for the available development boards.


### Step 0 : Install prerequisites

Depending on your platform, follow the instructions given in this chapter.

#### Install ARM Cross-compiler Toolchain

Apart from installing development host specific drivers and packages as mentioned above, an ARM cross-compiler toolchain should be installed on the development host. The EZ-Connect Lite SDK supports the ARM GCC Compiler Toolchain.

The toolchain for GNU ARM is available from: https://launchpad.net/gcc-arm-embedded/+download

Make sure that the toolchain is added to your environment path.

```
$ arm-none-eabi-gcc --version
arm-none-eabi-gcc (GNU Tools for ARM Embedded Processors) 5.4.1 20160609 (release) [ARM/embedded-5-branch revision 237715]
Copyright (C) 2015 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.


```

### Step 1 : Install Eclipse IDE for C/C++

* Download and install Java for your OS. [Install Java](http://www.java.com/inc/BrowserRedirect1.jsp?locale=en)
* Eclipse Mars as well as Neon are the supported versions. **Note** that Eclipse Neon has known issues with debug launchers. We have instructions for a workaround in further sections.
 - Eclipse Mars [Download](http://www.eclipse.org/downloads/packages/eclipse-ide-cc-developers/mars2)
 - Eclipse Neon [Download](http://www.eclipse.org/downloads/packages/eclipse-ide-cc-developers/neonr)

Extract it and launch the executable.


### Step 2 : Install plugins
Install the following plugins from Help -> Install New Software

1. C/C++ GDB Hardware Debugging
2. TM Terminal

<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/AllPlugins.PNG" width=800>


### Step 3 : Import ez-connect-lite project

We will now import the project into Eclipse using git.

* File -> Import

<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/import.png" width=800>

* Git -> Projects from Git

<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/localrepo.png" width=800>

* In the Directory dialog box, press Browse and set the path to where you cloned the repo in Step 0 : prerequisites. Alternatively, you can give the https url to SDK hosted on GitHub

`https://github.com/marvell-iot/ez-connect-lite `

<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/setpath.png" width=800>

* Press Next and then Finish.

The project along with the settings has been now imported in your Eclipse IDE.

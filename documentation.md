
## Supported Boards
Find board specific information like pin map and board files on the following pages -

- [Globalscale MW302 AWS IoT Starter Kit](./starter-kit/)
- AzureWave EVB and EVBA
- [Makerville Knit](./makerville-knit/)


## Get the SDK

 [ Download .zip >][download] or use git

    git clone https://github.com/marvell-iot/ez-connect-lite

Getting the SDK using git makes it easier to stay in sync with all the updates made to EZ Connect Lite .

[download]: https://github.com/marvell-iot/ez-connect-lite/archive/master.zip

## Setup Development Host

Make sure you have followed the OS specific instructions listed below before proceeding further -

- [Windows](./windows-host-setup/)
- [Linux](./linux-host-setup/)
- [Mac](./mac-host-setup/)

### Install ARM Cross-compiler Toolchain

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

### Eclipse IDE

Eclipse C/C++ IDE is the official supported IDE for EZ Connect Lite. You can find instructions for [setting up Eclipse here](./eclipse/).

If you prefer to use the CLI from your OS and your own existing IDE, you can safely skip the above guide.

## Development
In this section, you will see how to build the SDK, the included sample applications as well as how to create your own applications.

[ Development >][dev]

[dev]: ./development/

## Tutorials

- [Hello World](./hello-world/)
- [Wi-Fi basics](./wifi-basics/)
- [PinMux](./pinmux/)
- [AWS IoT Starter Demo](./aws-iot/)
- [Alexa](./alexa/)

## Projects

Here are some of the projects that people have made using the 88MW30x and EZ Connect Lite (previously known as aws_starter_sdk) -

- <a href="https://www.hackster.io/cubot/cubled-79119f" target="_blank">CUBLED</a> - Control a 4x4x4 LED cube using AWS IoT
- <a href="https://www.hackster.io/anujdeshpande/state-of-the-city-b81d85" target="_blank">State of the City</a> - Create a map with sensor data from different cloud connected sensors
- More projects on <a href="https://www.hackster.io/marvell/projects" target="_blank">Hackster</a>

## FAQs

#### I am getting errors when running the `make` command !
Make sure you have followed the instructions given in our [Development Host Setup](http://marvell-iot.github.io/docs/#documentation-setup-development-host) guide. Please check -

- The `PATH` variable is set correctly, and you can access the toolchain from any location using the command prompt for your OS
- If you are on Windows, please make sure that you have `make` installed as per the instructions in the host setup guide. 

#### How do I erase everything on the board ?
One way to do that is to use the layout file that can be found in `sdk/tools/OpenOCD/mw300/` folder. Use the `flashprog.py` utility to erase all the contents from the flash

```
python sdk/tools/OpenOCD/flashprog.py -l sdk/tools/OpenOCD/mw300/layout.txt 
```

#### I want to change the configured Wi-Fi network. What should I do ?
Please check the steps mentioned in the above example. The Wi-Fi credentials are stored in the persistent memory/flash so that they can be accessed after a reboot. The `flashprog.py` utility presently does not have the provision to selectively erase the Wi-Fi credentials. 

#### How can I flash a `.blob` using `flashprog.py` ?
The older `flash.py` utility had the ability to flash a generated `.blob` file. The new `flashprog.py` allows you to flash only the partitions as mentioned in the layout file. You can read the flash from a dev board and write it back using the `--read` and the `--write` parameters, which should serve as a viable replacement for flashing `.blob` files using `flash.py`.

#### I have a question not listed above ! Help !
Head over to our Gitter.im channel [here](https://gitter.im/marvell-iot/aws_starter_sdk). You will need a GitHub account to sign in.
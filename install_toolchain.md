# Install Toolchain


Apart from installing development host specific drivers and packages, an ARM cross-compiler toolchain should be installed on the development host. The EZ-Connect Lite SDK supports the ARM GCC Compiler Toolchain.

The toolchain for GNU ARM is available from: https://launchpad.net/gcc-arm-embedded/+download

Make sure that the toolchain is added to your environment path.

```
$ arm-none-eabi-gcc --version
arm-none-eabi-gcc (GNU Tools for ARM Embedded Processors) 5.4.1 20160609 (release) [ARM/embedded-5-branch revision 237715]
Copyright (C) 2015 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.


```

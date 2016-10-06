# PinMux

# PinMux

Various functionalities are multiplexed on the same pin as the number of pins are limited. The PinMux driver can be used to select the right functionality as per the application.

To set the PinMux we need to include the `mdev_pinmux.h` file in our application code.
```
#include <mdev_pinmux.h>
```

After creating a new variable of the [`mdev_t`](http://marvell-iot.github.io/aws_starter_sdk/struct__mdev__t.html) type, we initialize the driver with the [`pinmux_drv_init`](http://marvell-iot.github.io/aws_starter_sdk/mdev__pinmux_8h.html#a2e2011653aaa3c2fd24d44b9c210a15e) function.


```
mdev_t *pinmux_dev;
pinmux_drv_init();
pinmux_dev = pinmux_drv_open("MDEV_PINMUX");

```

Once we have initialized the driver, we can set the functionality of the pin in question. For example, if we wish to set the pin as a general purpose input output (GPIO), we will use the [`pinmux_drv_setfunc`](http://marvell-iot.github.io/aws_starter_sdk/mdev__pinmux_8h.html#a1d9f975a84ef85bb1509a52b6ced7321) function as per the corresponding pin's list of functionalities from [`mdev_pinmux.h`](https://github.com/marvell-iot/ez-connect-lite/blob/master/sdk/src/incl/sdk/drivers/mw300/mw300_pinmux.h) file


```
pinmux_drv_setfunc(pinmux_dev, pin_number,PINMUX_FUNCTION_0);

```

Check out the `sample_apps/io_demo/gpio` for more.




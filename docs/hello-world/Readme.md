# Introduction
Let's quickly look at how you can develop a simple Hello World application using the SDK.

## Hello World!
Let's begin with the customary Hello World! Here's sample code for Hello World :

```
int main(void)
{
        int count = 0;

        /* Initialize console on uart0 */
        wmstdio_init(UART0_ID, 0);

        wmprintf("Hello World application Started\r\n");

        while (1) {
                count++;
                wmprintf("Hello World: iteration %d\r\n", count);

                /* Sleep  5 seconds */
                os_thread_sleep(os_msec_to_ticks(5000));
        }
        return 0;
}
```

The `main()` function is the entry point for any application. This function is expected to perform any application specific initializations and launch any application threads.

The `wmstdio` facility can be used to print log messages on the serial console over UART. Before using this though, we will have to initialize the UART. This is done using the [`wmstdio_init()`](http://marvell-iot.github.io/aws_starter_sdk/wmstdio_8h.html#a3e62da8e3f3e721eaa9b03f573b4cd62) call. This function identifies the UART that should be used for this communication and the baud rate that should be used.

The [`wmprintf()`](http://marvell-iot.github.io/aws_starter_sdk/wmstdio_8h.html#a4df7db7aad35d6411ad9b003e4890c16) function is then used to print messages to the console.

Check out this code in the `sample_apps/hello_world` directory.

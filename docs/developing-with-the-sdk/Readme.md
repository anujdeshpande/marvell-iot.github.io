# Introduction
Let's quickly look at how you can develop application firmware with the SDK.

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

The `wmstdio` facility can be used to print log messages on the serial console over UART. Before using this though, we will have to initialize the UART. This is done using the `wmstdio_init()` call. This function identifies the UART that should be used for this communication and the baud rate that should be used.

The `wmprintf()` function is then used to print messages to the console.

## Wi-Fi
The first thing that you typically do with an IoT Kit is to get it on a network. The easiest way to connect to a known Wi-Fi network is to use the function `wm_wlan_connect()`. This function takes a Wi-Fi network name and a passphrase as an argument. It then attempts to connect to this Wi-Fi network.

The `wm_wlan_connect()` is simple and useful, but what if you don't want to hard-code the network credentials in your application firmware's code? The SDK also offers another more powerful Wi-Fi API, `wm_wlan_start()`. This API does a number of things,

- Firstly, it starts a Wireless Network of its own, a Micro-AP/uAP
- On this uAP network, it hosts a web-app based wizard that guides the end-user through the provisioning of the home Wi-Fi network
- The end-user can launch this web-app by connecting with the uAP network, and then launching a browser that points to `http://192.168.10.1`
- The end goal of this web-app is to retrieve a Wi-Fi network name and passphrase from the end-user
- Once this configuration is received, then the kit makes connection attempts to the configured network. Once a network is configured, the firmware remembers this configuration across power resets. Subsequent boot-ups of the kit, will skip the setup of the initial configuration wizard, but instead directly start making connection attempts with this network.

### Connectivity Callbacks
The application firmware gets to know the status of the Wi-Fi connectivity using connectivity callbacks. The following connectivity callbacks have been defined:

- `wlan_event_normal_connected()`: This function is called whenever the Wi-Fi station successfully associates with the destined Wireless Access Point.
- `wlan_event_connect_failed()`: This function is called whenever the Wi-Fi station fails to associate with the destined
- `wlan_event_normal_link_lost()`: This function is called whenever an existing association with the Wireless Access Point is lost

An application firmware can define these functions to take the required action as is applicable to them.

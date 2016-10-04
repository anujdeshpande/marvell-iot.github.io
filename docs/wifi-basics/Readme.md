# Wi-Fi Basics

The first thing that you typically do with an IoT Kit is to get it on a network. The easiest way to connect to a known Wi-Fi network is to use the function [`wm_wlan_connect()`](http://marvell-iot.github.io/aws_starter_sdk/wmsdk_8h.html#abadf6262ff53dd041ebd0c3933bb2bdc). This function takes a Wi-Fi network name and a passphrase as an argument. It then attempts to connect to this Wi-Fi network.

```
#include <wm_os.h>
#include <wmstdio.h>
#include <wmtime.h>
#include <wmsdk.h>
#include <led_indicator.h>
#include <board.h>


#define MICRO_AP_SSID                "wifi-basics"
#define MICRO_AP_PASSPHRASE          "marvellwm"


void wlan_event_normal_link_lost(void *data)
{
	wmprintf("wlan_event_normal_link_lost() has been invoked \r\n");
}

void wlan_event_normal_connect_failed(void *data)
{
	wmprintf("wlan_event_normal_connect_failed() has been invoked \r\n");
}

void wlan_event_normal_connected(void *data)
{
	wmprintf("wlan_event_normal_connected() has been invoked \r\n");
}

int main()
{
	wm_wlan_start(MICRO_AP_SSID, MICRO_AP_PASSPHRASE);
	return 0;
}

```

The [`wm_wlan_connect()`](http://marvell-iot.github.io/aws_starter_sdk/wmsdk_8h.html#abadf6262ff53dd041ebd0c3933bb2bdc) is simple and useful, but what if you don't want to hard-code the network credentials in your application firmware? The SDK also offers another more powerful Wi-Fi API, [`wm_wlan_start()`](http://marvell-iot.github.io/aws_starter_sdk/wmsdk_8h.html#a487b6f0c6c72bd77453b110d9accb4f0). This API does a number of things,

- Firstly, it starts a Wireless Network of its own, a Micro-AP/uAP
- On this uAP network, it hosts a web-app based wizard that guides the end-user through the provisioning of the home Wi-Fi network
- The end-user can launch this web-app by connecting with the uAP network, and then launching a browser that points to `http://192.168.10.1`
- The end goal of this web-app is to retrieve a Wi-Fi network name and passphrase from the end-user
- Once this configuration is received, then the kit makes connection attempts to the configured network. Once a network is configured, the firmware remembers this configuration across power resets. Subsequent boot-ups of the kit, will skip the setup of the initial configuration wizard, but instead directly start making connection attempts with this network.

### Connectivity Callbacks
The application firmware gets to know the status of the Wi-Fi connectivity using connectivity callbacks. The following connectivity callbacks have been defined:

- [`wlan_event_normal_connected()`](http://marvell-iot.github.io/aws_starter_sdk/wmsdk_8h.html#a406020d6598caca9eb8074321ca7ccb1): This function is called whenever the Wi-Fi station successfully associates with the destined Wireless Access Point.
- [`wlan_event_connect_failed()`](http://marvell-iot.github.io/aws_starter_sdk/wmsdk_8h.html#a51ba3386516da0efca21ae06defe5636): This function is called whenever the Wi-Fi station fails to associate with the destined
- [`wlan_event_normal_link_lost()`](http://marvell-iot.github.io/aws_starter_sdk/wmsdk_8h.html#a41961638c0f6880ffdc32d1cc56caaa9): This function is called whenever an existing association with the Wireless Access Point is lost

An application firmware can define these functions to take the required action as is applicable to them. Check out `sample_apps/wifi-basics` to see the `wlan` APIs in action. 

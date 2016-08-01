# Marvell IoT

EZ-Connect Lite is the SDK for Marvell Semiconductor's 88MW30x series of Wi-Fi microcontrollers.


## SDK features

* Secure connections using TLS 1.2
* FreeRTOS as the underlying real time operating system
* Wi-Fi station and soft AP support
* lwIP open source TCP/IP stack
* MQTT client for real time communication with brokers
* Supported on Windows, Linux, OS X operating systems

### Get the SDK

 [ Download .zip >][download] or use git

    git clone https://github.com/marvell-iot/ez-connect-lite


[download]: https://github.com/marvell-iot/ez-connect-lite/archive/master.zip


## SoC specs

* 32-bit Cortex M4F
* Upto 200Mhz
* 512KB SRAM
* [XIP](https://en.wikipedia.org/wiki/Execute_in_place) support with external QSPI flash
* 802.11 b/g/n
* UART, SSP, I2C, PWM/GPT, ADC, DAC, and USB-OTG(only for 88MW302)
* [Product details](http://www.marvell.com/microcontrollers/88MW300/302/)


### Get the Development Boards

|Name|Link|
|:-:|:-:|
| IoT Starter Kit  | <a href="https://www.amazon.com/Globalscale-MW302-IoT-Starter-Powered/dp/B0168DLQHI/" target="_blank" class="button">Buy Now</a>|
| AzureWave EVB    |<a href="http://www.buyiot.net/" target="_blank" class="button">Buy Now</a>|
| Makerville Knit  |<a href="https://makerville.io/knit/" target="_blank" class="button">Buy Now</a>|


## Documentation

- [SDK Documentation](./docs) : You can find instructions on setting up your development host, flashing boards, etc
- [API Reference](http://marvell-iot.github.io/aws_starter_sdk) : Documentation about individual functions, classes, and so on.

## Support

- [![Join the chat at https://gitter.im/marvell-iot/aws_starter_sdk](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/marvell-iot/aws_starter_sdk?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
- [GitHub Issues](https://github.com/marvell-iot/ez-connect-lite/issues)

## Products

Some of the commercially available products that have been made using the 88MW300/2 Wi-Fi microcontroller-
<table style="width:100%">
  <tr>
    <td>Hello Barbie</td>
    <td><a href="http://www.somersetrecon.com/blog/2015/11/20/hello-barbie-security-part-1-teardown" target="_blank">Teardown from Somerset Recon</a></td>
    <td>See it in action <a href="https://www.youtube.com/watch?v=RJMvmVCwoNM" target="_blank">here</a></td>
  </tr>
  <tr>
    <td>Xiaomi Yeelight</td>
    <td><a href="http://www.miui.com/thread-4260673-1-1.html" target="_blank">Teardown from miui.com (in Chinese)</a></td>
    <td>See it in action <a href="https://www.youtube.com/watch?v=x0RCSBAH6gE" target="_blank">here</a></td>
  </tr>
</table>

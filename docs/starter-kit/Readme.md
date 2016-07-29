# AWS IoT Starter Kit

<a href="https://www.amazon.com/Globalscale-MW302-IoT-Starter-Powered/dp/B0168DLQHI/" target="_blank" class="button">Buy Now</a>

**NOTE** : The Starter Kit Board is 3.3v tolerant. If you are using a 5v sensor or module, please make sure that you shift down the voltage levels using a resistances or a level-shifter IC. **5v input can damage the module**.

<a href = "https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/PinMap.png"> <img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/PinMap.png" width=500> </a>

(click image to open larger version)

### Header 2

To the left of the SoC

| GPIO_#  | Function 0  | Function 1 | Function 2 |
| :------------ |:---------------:| :-----:| :-----:|
| 11      | UART1_CTS | SSP1_CLK|-
| 12      | UART1_RTS |SSP1_FRM |-
| 04      | I2C0_SDA | CAN_TX|GPT0_CH4
| 05      | I2C0_SCL | CAN_RX|GPT0_CH5
| 13      | UART1_TXD| SSP1_TXD|-
| 14      | UART1_RXD | SSP1_RXD| -
| 22      | WAKE0 | - | -
| 26      | PUSH_SW0 |I2C1_SCL|CAN_RX
| 25      | 32KHz_CLK_IN | I2C1_SDA|CAN_TX
| 39      | I2C0_RESET | - | -
| 23      | WAKE1 |- |-
| 02      | UART0_TXD | SSP0_TXD|GPT0_CH2
| 03      | UART0_RXD | SSP0_RXD|GPT0_CH3
| 16      | STRAP1 |- |-
| 17      | GPT3_CH0 |I2C1_SCL |-
| 18      | GPT3_CH1 | I2C1_SDA|-
| 24      | PUSH_SW1 | GPT1_CH5|-

### Header 4
To the right of the SoC

| GPIO_#  | Function 0  | Function 1 | Function 2 |
| :------------ |:---------------:| :-----:| :-----:|
| 0      | UART0_CTS | SSP0_CLK| GPT0_CH0
| 01      |  UART0_RTS|SSP0_FRM |GPT0_CH1
| 40      |  LED1|- |-
| 41      | LED2 | -|-
| 27      | STRAP0 |USB_DRVBUS|-
| 48      | SSP2_TXD | UART2_TXD |ADC0_6
| 49      | SSP2_RXD | UART2_RXD|ADC0_7
| 47      | SSP2_FRM | UART2_RTS|ADC0_5
| 42      | ADC0_0 | ACOMP0|-
| 43      |  ADC0_1| ACOMP1|-
| 46      |  SSP2_CLK| UART2_CTS|ADC0_4

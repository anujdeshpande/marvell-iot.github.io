## AWS IoT overview

The AWS IoT APIs is a collection of APIs that can be used to connect to the AWS IoT service. The detailed documentation of the AWS IoT API can be found here: http://aws-iot-device-sdk-embedded-c-docs.s3-website-us-east-1.amazonaws.com/index.html

Typically for using AWS IoT Shadows the following sequence of calls will be useful:

- `aws_iot_mqtt_init()` to initialize the MQTT module
- `aws_iot_shadow_init()` to initialize the AWS Shadows
- `aws_iot_shadow_connect()` to connect to an AWS Shadow
- Now, create a thread that keeps calling `aws_iot_shadow_yield()`. This keeps looking for incoming messages from the AWS IoT server
- Once such a setup is done, the application firmware can
   - subscribe to changes from the AWS IoT service using the `aws_iot_shadow_register_delta()` function. The callback registered will be called whenever the designated JSON objects are updated at the server
   - publish updates to states using the `aws_publish_property_state()` function


## Marvell's AWS IoT Starter Kit

Marvell's AWS IoT Starter Kit has the following contents:

* Marvell 88MW302 Development Board 
* USB to mini-USB Cable

<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/RD-88MW302-A0-V2%20wo%20shield.jpeg" width=800>

Marvell's AWS IoT Starter SDK can be used to build and flash firmware on the starter kit. Using Marvell's EZ Connect Lite SDK, it is extremely easy to create Internet-of-Things enabled devices communicating with AWS IoT service, in a very short amount of time.

## Getting Started

The Starter Kit has been pre-programmed with a simple demo that exposes some of the common functions of the AWS IoT service. The demo program showcases the following:

 - Uses the AWS ThingShadows feature of the AWS IoT service
   - The AWS thing registry tracks the number of push-button press events on the starter kit
   - The AWS thing registry can be used for controlling the LED on/off behavior on the starter kit
 - Uses the AWS IoT to AWS Lambda feature
   - A push-button press on the starter kit can trigger the an AWS Lambda function that takes an action

The Amazon IoT User Guide provides excellent overview of the functionality offered by AWS IoT. It is advisable to read that document to get a better idea of the AWS IoT Service. This Quick Start Guide will frequently refer to the steps described in the AWS IoT User Guide wherever applicable in the flow.

### Configuring your AWS IoT Service

Before we proceed, lets get your AWS IoT service setup. Please perform the following steps:

 - Create an [AWS Account](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AMS5.0CreatingAnAWSAccount.html).
 - Go to [AWS IoT](https://aws.amazon.com/iot/) and open up the AWS IoT Dashboard

<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/IoTDashboard.png" width=800>

 - Now let us create a _Thing_ on the AWS IoT dashboard by clicking on the create button

<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/CreateSomething.png" width=800>

 - Clicking on Create Thing will give you

<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/CreateThing.png" width=800>

 - Please enter a name for your thing, say starterkit1, and click on Create. Please make a note of this thing-name, we will have to configure the starter kit with this exact same name later on.

<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/ThingCreated.png" width=800>

 - The thing is now created on the AWS IoT service. Clicking on View Thing, will show you the contents of this thing.

<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/ViewThing.png" width=800>

 - The thing can now be associated with a device, in this case our starter kit. Please click on Connect a Device, to associate a device with this thing.

<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/ConnectDevice.png" width=800>

 - Please make selections as described in the above screenshots. We will let the AWS IoT service create a certificate and the corresponding policy for us. Then click on Generate Certificate and Policy.

<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/DownloadCertificates.png" width=800>

 - We will download and store the private key and the certificate that has been generated. These will then be configured in our starter kit. The starter kit will use this certificate to authenticate itself with the AWS IoT service.

### Configuring the starter kit

As a first step, lets get the basic configuration for your starter kit going. Basic configuration includes

 - Configuring AWS IoT certificates and details into the kit, so that it can talk to your AWS account
 - Configuring the Wi-Fi credentials in the starter kit, so that it can connect to your home Wi-Fi network

Please perform the following steps

* To begin, please supply power to your starter kit by connecting the USB-to-miniUSB cable between the starter kit and your PC/Mac.
* The starter kit will now have a slow blinking yellow LED. This indicates that the starter kit is in a provisioning mode. In this mode, the starter kit will launch its own network, of the form, aws_starter-WXYZ. Please connect your PC/Mac to this network. The passphrase of this network is `marvellwm`. You can connect to this network and configure the starter kit to connect with your home network and your AWS IoT service.
* Once the connection is established, launch a browser and enter the URL `http://192.168.10.1`. This will launch a WebApp based configuration wizard for configuring the starter kit.

<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/DevHomePage.png" width=800>

* The first step of the configuration is to program the AWS settings that we created in *Configuring your AWS IoT Service*. Please refer to the following screenshot. Fill-out the following entries on the web page:

  * **Thing Name**: Please enter the thing name of the thing that you created above.
  * **Region**: Please enter the applicable region settings for your AWS account, for example, `us-east-1` or `us-west-2`.
  * **Certificate**: This is the certificate file, `publickey.pem`, that was created above.
  * **Private Key**: This is the private key file, `privatekey.pem`, that was created above.

<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/DevAmazonConfig.png" width=800>

* The second step of the configuration is to program the home network credentials. Please refer to the following screenshot.
  * Select the target home network from the list of networks that is provided

<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/DevNetworkSelect.png" width=800>

  * Please enter the passphrase for your home network

<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/DevNetworkPass.png" width=800>

* The starter kit will now make connection attempts to the configured home network. This is indicated by a fast blink of the yellow LED.
* Once the starter kit successfully connects to your home network, it will have the yellow LED solid on. After connecting to the home network, the starter kit will connect with the AWS IoT service in a few seconds.

<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/DevConfigDone.png" width=800>

### Working with Thing Shadows (Simulating an app)

The starter kit synchronizes the states of one push-button and one LED with the AWS IoT service.

<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/RD-88MW302-A0-V2%20wo%20shield.jpeg" width=800>

In the steps below, we simulate an app controlling the starter kit, by using AWS IoT Console.

* Pressing the push-button marked GPIO_26, will increment a counter. Whenever the counter is incremented, the latest value of the counter is published to the AWS IoT Service. The AWS IoT service expects all state _reported_ by the device to be captured in the `reported` JSON composite object. For example in our scenario, the starter kit reports the following value to the AWS IoT service.

```json
{ "reported": { "pb": 4 } }
```

* You can see this value by searching for your thing name in the AWS IoT console. As shown below, the value reported by the device is seen in the `State` field. A remote app can also read this value using the REST API endpoint shown in the Details section of this thing.

<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/ReportedState.png" width=800>

* The AWS IoT console can also be used to change the state of the thing. The AWS IoT service expects that all the _desired_ state change requests should be captured in the `desired` JSON composite object. For example, in our case, the starter kit will look for the following LED target value. The starter kit will switch the LED, GPIO_40, on or off based on this value.

```json
{ "desired": { "led": 1 } }
```

* You can update the value of this LED, by searching for your thing name in the AWS IoT console. On the right hand side of the screen, click on the Update State tab. Please update the desired state of the LED, and then click on the Update State button at the bottom. The starter kit will read this value and then switch on or off the LED, GPIO_40 accordingly.

<img src="https://raw.githubusercontent.com/marvell-iot/aws_starter_sdk_wiki_images/master/UpdateState.png" width=800>

* A remote app can also update this value by writing to the REST API endpoint as shown in the Details section of the thing.

### What more?

We have also configured another push-button on the board, `pb_lambda`. This button also reports the button press counts to the AWS IoT service.

Why don't you go ahead and configure AWS IoT Rules to trigger an AWS Lambda function on this button-press. Its a quick way to build [Amazon Dash](http://www.amazon.com/oc/dash-button) like buttons right away! :v:

### Reset to Factory Defaults

You can reset the starter kit to factory default settings by press-and-hold the input push-button, GPIO_24 (pb_lambda) for more than 10 seconds. The two yellow LEDs will blink simultaneously once to indicate that the reset to factory action has been registered.

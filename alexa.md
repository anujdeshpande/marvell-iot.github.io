# Alexa

# Alexa With Marvell 88MW30X

Controlling an LED using Amazon Alexa's Smart Home Skills API

Amazon Alexa is a voice service which powers devices like Amazon Echo and Amazon Tap. You can also try it out using a compatible browser by visiting <a href="http://echosim.io/" target="_blank">Echosim.io</a> if you don't have a physical device with you.

Check out the final result of this tutorial in the video below -

&nbsp; <iframe width="560" height="315" src="https://www.youtube.com/embed/LP4hU_7EqSw" frameborder="1" allowfullscreen></iframe>



## AWS IoT

Please follow the instructions given in our [AWS IoT tutorial](../aws-iot/) to connect a Marvell development board with Amazon's AWS IoT service.

## Create a Lambda Function

Now we are going to create a [Lambda function](http://docs.aws.amazon.com/lambda/latest/dg/welcome.html), and configure some part of it. After creating the Alexa Skill we will come back to finish configuring.

- Hit getting started on the Lambda dashboard 

<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/Lambda.png" width=800></img>

- After skipping the template selection, we will be shown the configure function menu, where we will input our code and other details specific to the function. We will be using Node.js.

<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/ledSkillAdapter.png" width=800></img>

- Next copy and paste the following code into the Lambda function

```
var config = {};




config.IOT_BROKER_ENDPOINT      = "<your-end-point>.iot.us-west-2.amazonaws.com".toLowerCase();

config.IOT_BROKER_REGION        = "<your-region>";

config.IOT_THING_1              = "<thing-1>"; //lobby
config.IOT_THING_2              = "<thing-2>";//bedroom
config.IOT_THING_3              = "<thing-3>";//study

//Loading AWS SDK libraries

var AWS = require('aws-sdk');

AWS.config.region = config.IOT_BROKER_REGION;
// You can get your access key ID and success by vising your IAM profile. Make sure you give IoTFullAccess permission to the role in question
AWS.config.update({accessKeyId: '<your-access-key-id>', secretAccessKey: '<your-access-key-secret>'});

//Initializing client for IoT

var iotdata = new AWS.IotData({endpoint: config.IOT_BROKER_ENDPOINT});

// These values could be anything, but have to be unique
var bedroomLightApplianceId     = "7ec4c146c51";
var studyLightApplianceId       = "7ec4c146c52";
var lobbyLightApplianceId       = "7ec4c146c53";
// namespaces

const NAMESPACE_CONTROL = "Alexa.ConnectedHome.Control";

const NAMESPACE_DISCOVERY = "Alexa.ConnectedHome.Discovery";

// discovery

const REQUEST_DISCOVER = "DiscoverAppliancesRequest";

const RESPONSE_DISCOVER = "DiscoverAppliancesResponse";

// control

const REQUEST_TURN_ON = "TurnOnRequest";

const RESPONSE_TURN_ON = "TurnOnConfirmation";

const REQUEST_TURN_OFF = "TurnOffRequest";

const RESPONSE_TURN_OFF = "TurnOffConfirmation";

// errors

const ERROR_UNSUPPORTED_OPERATION = "UnsupportedOperationError";

const ERROR_UNEXPECTED_INFO = "UnexpectedInformationReceivedError";


// entry

exports.handler = function (event, context, callback) {

  log("Received Directive", event);

  var requestedNamespace = event.header.namespace;

  var response = null;

  try {

    switch (requestedNamespace) {

      case NAMESPACE_DISCOVERY:

        response = handleDiscovery(event);
        console.log("In Discovery Switch");
        break;

      case NAMESPACE_CONTROL:

        response = handleControl(event);

        break;

      default:

        log("Error", "Unsupported namespace: " + requestedNamespace);

        response = handleUnexpectedInfo(requestedNamespace);

        break;

    }// switch

  } catch (error) {

    log("Error", error);

  }// try-catch

  callback(null, response);

};// exports.handler


var handleDiscovery = function(event) {

  var header = createHeader(NAMESPACE_DISCOVERY, RESPONSE_DISCOVER);
   var appliances = [];
    var studyLight = {
        applianceId: studyLightApplianceId,
        manufacturerName: 'Marvell',
        modelName: '88MW30x',
        version: 'VER01',
        friendlyName: 'Study Light',
        friendlyDescription: 'Light up your study',
        isReachable: true,
        actions:[

            "turnOn",
            "turnOff"
        ],
        additionalApplianceDetails: {
            /**
             * OPTIONAL:
             * We can use this to persist any appliance specific metadata.
             * This information will be returned back to the driver when user requests
             * action on this appliance.
             */
            fullApplianceId: '2cd6b650-c0h0-4062-b31d-7ec2c146c5e1',
            deviceId: "39003d000447343232363231"
        }
    };
    var bedroomLight = {
        applianceId: bedroomLightApplianceId,
        manufacturerName: 'Marvell',
        modelName: '88MW300',
        version: 'VER01',
        friendlyName: 'Bedroom Light',
        friendlyDescription: 'So that you don't have to get up to switch off the light ;)',
        isReachable: true,
        actions:[

            "turnOn",
            "turnOff"
        ],
        additionalApplianceDetails: {
            /**
             * OPTIONAL:
             * We can use this to persist any appliance specific metadata.
             * This information will be returned back to the driver when user requests
             * action on this appliance.
             */
            fullApplianceId: '2cd6b650-c0h0-4062-b31d-7ec2c146c5e2',
            deviceId: "39003d000447343232363232"
        }
    };
    var lobbyLight = {
        applianceId: lobbyLightApplianceId,
        manufacturerName: 'Marvell',
        modelName: '88MW300',
        version: 'VER01',
        friendlyName: 'Lobby Light',
        friendlyDescription: 'Control your lobby light',
        isReachable: true,
        actions:[

            "turnOn",
            "turnOff"
        ],
        additionalApplianceDetails: {
            /**
             * OPTIONAL:
             * We can use this to persist any appliance specific metadata.
             * This information will be returned back to the driver when user requests
             * action on this appliance.
             */
            fullApplianceId: '2cd6b650-c0h0-4062-b31d-7ec2c146c5e3',
            deviceId: "39003d000447343232363233"
        }
    };
     appliances.push(bedroomLight);
     appliances.push(studyLight);
     appliances.push(lobbyLight);
    /**
     * Craft the final response back to Alexa Connected Home Skill. This will include all the 
     * discoverd appliances.
     */
    var payloads = {
        discoveredAppliances: appliances
    };

  console.log("Printing header and payload now");
  console.log(header,payloads);

  return createDirective(header,payloads);

};// handleDiscovery


var handleControl = function(event) {

  var response = null;

  var requestedName = event.header.name;

  switch (requestedName) {

    case REQUEST_TURN_ON :

      response = handleControlTurnOn(event);

      break;

    case REQUEST_TURN_OFF :

      response = handleControlTurnOff(event);

      break;

    default:

      log("Error", "Unsupported operation" + requestedName);

      response = handleUnsupportedOperation();

      break;     

  }// switch

  return response;

};//; handleControl


var handleControlTurnOn = function(event) {
console.log(event.payload.appliance.applianceId);
var thingPicker='';
console.log("Turning On the LED now");
var update = {
                "state": {
                   "desired" : {
                        "power" : 1
                    }
                }
            };
            
            switch(event.payload.appliance.applianceId){
            case lobbyLightApplianceId:
                thingPicker = config.IOT_THING_1; 
                break;
            case bedroomLightApplianceId:
                thingPicker = config.IOT_THING_2;
                break;
            case studyLightApplianceId:
                thingPicker = config.IOT_THING_3;
                break;
            }
            console.log(thingPicker);
            iotdata.updateThingShadow({
                payload: JSON.stringify(update),
                thingName: thingPicker
            }, function(err, data) {
                if (err) {
                    console.log(err);
                } else {
                    console.log(data);
                }
            });


  var header = createHeader(NAMESPACE_CONTROL,RESPONSE_TURN_ON);
  var payload = {};

  return createDirective(header,payload);

};// handleControlTurnOn


var handleControlTurnOff = function(event) {
    console.log(event.payload.appliance.applianceId);
    var thingPicker='';
    console.log("Turning Off the LED now");
var update = {
                "state": {
                   "desired" : {
                        "power" : 0
                    }
                }
            };
           switch(event.payload.appliance.applianceId){
            case lobbyLightApplianceId:
                thingPicker = config.IOT_THING_1; 
                break;
            case bedroomLightApplianceId:
                thingPicker = config.IOT_THING_2;
                break;
            case studyLightApplianceId:
                thingPicker = config.IOT_THING_3;
                break;
            }
            console.log(thingPicker);
            iotdata.updateThingShadow({
                payload: JSON.stringify(update),
                thingName: thingPicker
            }, function(err, data) {
                if (err) {
                    console.log(err);
                } else {
                    console.log(data);
                }
            });



  var header = createHeader(NAMESPACE_CONTROL,RESPONSE_TURN_OFF);

  var payload = {};

  return createDirective(header,payload);

};// handleControlTurnOff


var handleUnsupportedOperation = function() {

  var header = createHeader(NAMESPACE_CONTROL,ERROR_UNSUPPORTED_OPERATION);

  var payload = {};

  return createDirective(header,payload);

};// handleUnsupportedOperation


var handleUnexpectedInfo = function(fault) {

  var header = createHeader(NAMESPACE_CONTROL,ERROR_UNEXPECTED_INFO);

  var payload = {

    "faultingParameter" : fault

  };

  return createDirective(header,payload);

};// handleUnexpectedInfo


// support functions

var createMessageId = function() {

  var d = new Date().getTime();

  var uuid = 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {

    var r = (d + Math.random()*16)%16 | 0;

    d = Math.floor(d/16);

    return (c=='x' ? r : (r&0x3|0x8)).toString(16);

  });

  return uuid;

};// createMessageId


var createHeader = function(namespace, name) {

  return {

    "messageId": createMessageId(),

    "namespace": namespace,

    "name": name,

    "payloadVersion": "2"

  };

};// createHeader


var createDirective = function(header, payload) {

  return {

    "header" : header,

    "payload" : payload

  };

};// createDirective


var log = function(title, msg) {

  console.log('**** ' + title + ': ' + JSON.stringify(msg));

};
```

- If you don't already have a basic execution role for Lambda, you will have to create one now.
<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/CreateNewRole.png" width=800></img>

- Make sure you put in at least 512MB of RAM and a 10 sec timeout.
<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/lambdaBasicExec.png" width=800></img>


## Create a developer account

Head over to <a href="https://developer.amazon.com" target="_blank">developer.amazon.com</a> and register a new account, if you don't already have one. This services provided under this are differnt from the one's provided under Amazon AWS.

<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/login.png" width=800></img>
<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/Registration.png" width=800></img>
<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/Agreement.png" width=800></img>
<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/Screenshot from 2016-09-08 10-39-57.png" width=800></img>

## Create a LWA app (Login With Amazon App)

When creating a Smart Home Skill for Alexa, you need to have an OAuth login provider. This doesn't have to be Amazon, but for the sake of simplicity we will continue to use Amazon's services.

<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/LWA.png" width=800></img>
<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/LWAsignup.png" width=800></img>
<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/LWAsecurityProfile.png" width=800></img>
Make sure that the Consent Privacy Notice URL is the same as the one in the below screenshot.
<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/LWAdetails.png" width=800></img>

You will be shown a Client ID and a Client Secret. Use a notepad application to make a note of these 2 values, we will require them when creating an Alexa Skill.

<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/SecurityClientIDSecret.png" width=800></img>

## Create an Alexa Skill
Next up, we are going to create a new Alexa Skill. Click the Alexa tab on the navigation bar in the developer console.

- Select Alexa Skills Kit from the Alexa dashboard.

<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/AlexaDashboard.png" width=800></img>

- Select Add a New Skill.

<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/AddNewSkill.png" width=800></img>

- Make sure you select Smart Home Skill API in the Skill Type menu.


<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/SmartSkillInfo.png" width=800></img>

- The interaction model is taken care of by Amazon when it comes to the Smart Home Skills API. So here we will just hit Next.

<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/NextForInteractionModel.png" width=800></img>

- The Lambda ARN from the function that we have created earlier, will be needed now.
<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/Configuration.png" width=800></img>

- Copy the redirect URL, and go back to LWA (Login With Amazon), and make sure that you set it in the Web Settings of the App that we created earlier

<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/BackToLWA.png" width=800></img>

- Paste it into Allowed Return URLs

<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/AllowedReturnURLs.png" width=800></img>

## Continue Lambda

- We need to set the Alexa Skill Application ID after it has been created, in the triggers section of the Amazon AWS Lambda function that we created earlier.

<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/NextLambdaSkill.png" width=800></img>

## Alexa Mobile Application

- Now open your Alexa mobile app on your smartphone. Login using the same Amazon account that you used for Amazon AWS as well as developer.amazon.com.
- From the menu, select Skills and then go to Your Skills

<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/YourSkills.jpg" width=400></img>

You should see the skill that we created in the previous steps, along with any other Alexa skills that you might have created.

- After selecting the relevant Skill, you should select Enable Skill.

<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/EnableSkill.jpg" width=400></img>

- You will be required to signin using your Amazon account, as we are using the Login With Amazon service.

<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/OauthSignIn.jpg" width=400></img>

- After that, please accept the permissions window that will be shown. This was set from the profile field when we created the Alexa Skill.

<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/OauthAccept.jpg" width=400></img>

- You should now get a window saying that the skill has been successfully linked to Alexa.

<img src="https://github.com/marvell-iot/marvell-iot.github.io/raw/master/docs/alexa/imgs/AlexaLinked.jpg" width=400></img>

## Alexa device
Now ask Alexa (either the physical device or the browser simulation) to discover devices

    Alexa discover devices

After that you can ask it turn the light on or off. In our case the light is named "Kitchen Light"

    Alexa turn on kitchen light

or

    Alexa turn off kitchen light


## Advanced

- In the above setup we have done a few things from a development point of view that we will have to modify to make this Alexa Skill publishable. As we have hard coded the AWS IoT thing credentials of only three things, we won't be able to publish this skill. This will only be usable by the developer and hobbyists, from their Amazon account.

- As we have setup OAuth, with Amazon as a provider, we get a `accessToken` from the particular provider. Note that this does not have to be Amazon. It can be any of the other providers like Google, Facebook, Twitter or even your own custom OAuth service. This `accessToken` is passed to us along with the type of intent and should be used to identify the devices the particular user owns. The Lambda code shared above does not have this lookup, and the developer will have to write code to handle that. 

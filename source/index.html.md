---
title: Hut34 Wiki Page 



toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>


search: true
---

# Getting Started
## Overview

This page will walk through the most important features of the Hut34 Platform and show you how to connect your device or agent using our existing integrations. The methods for connection are generic webhook, [AllThingsTalk](https://www.allthingstalk.com/), [Dialogflow](https://dialogflow.com/) and [MS Bot framework](https://dev.botframework.com/).

## Create an account

This page describes how to create and log in to the Hut34 Platform.

### Sign up and Log in

Sign up to the alpha platform [here](https://platform.hut34.io/Account/Register). Access to the alpha release is limited by approval. Once approved you will need to verify your email address.

## Register your first device 

Once you are logged in, it will direct you to the platform dashboard where you can find information for all your devices and bots. 

To get started, click on the "register a device" option at the top of the page or click [here](https://platform.hut34.io/Registration/Create). You will have to fill out different details: 

  * Device Name - an identifier for the device (e.g 'Health Bot');
  * Invocation - a reserved keyword for direct routing to your device (e.g. health_bot);
  * Fee - this is the fee your device will collect each time it responds to a query;
  * Location - a label used when routing is specific to a region or location
  * Meta-tags - a set of labels, separated by commas that are used by our routing engine;
  * Connection Type - generic webhook, [AllThingsTalk](https://www.allthingstalk.com/), [Dialogflow](https://dialogflow.com/) and [MS Bot framework](https://dev.botframework.com/).

Further documentation about connection types can be found on the next section. 

<aside class="notice">
Device names and invocations cannot be changed once set.
</aside>

# Connection Types

## Dialogflow Integration

**Requirements**

Before proceeding, you will need: 

  * A [Hut34](https://platform.hut34.io/Account/Login) account;
  * A [Dialogflow](https://dialogflow.com/) account;
  * To be familiar with Dialogflow.

Get started with Dialogflow [here](https://dialogflow.com/docs/getting-started).

### 1. Obtain a Dialogflow agent client access token 

  * Login to your Dialogflow account and select the agent that you want to integrate with Hut34.
  * Go to your agent settings and copy "Client access token". This token will be used in the next step.

![alt text](/images/dialogFlow1.png)

### 2. Register a new device in Hut34 

  * Now login to your Hut34 account. Click ["Register a Device"](https://platform.hut34.io/Registration/Create) link at the top of the page. 
  * Complete device information. Select "Dialogflow" under "Connection Types".
  * Paste the "Client access token" copied from step 1 to the "API key" textbox. 


![alt text](/images/dialogFlow2.png)

  * Press the "Register" button. You will now be redirected to the "Device List" page and you should now see your device in this list. Copy your "Device key" from this list. This will be used in the next step. 

![alt text](/images/dialogFlow3.png)

### 3. Configure the Dialogflow webhook

  * In your Dialogflow account select the agent that you want to integrate with the hut34 platform, and click on the "Fulfillment" link.
  * Enable webhook and enter the webhook url by inserting your Device Key in the following url:

  `https://platform.hut34.io/botquery?platform=apiai&key=[YOUR_DEVICE_KEY]`



![alt text](/images/dialogFlow4.png)

### 4. Create Receiving Intent

  * Create a new Intent in your agent and set "@hut34" in the "User Says" field.
  * Under Fulfillment section enable the "Use Webhook" option. 

![alt text](/images/dialogFlow5.png)


## AllThingsTalk Integration

The Hut34 Platform supports Allthingstalk integration. You can integrate an Allthingstalk (ATT) device by following these steps.

### Requirements

  * A [Hut34](https://platform.hut34.io/Account/Login) account
  * A [AllThingsTalk](https://www.allthingstalk.com/) account


### 1. Obtain AllThingsTalk Device ID and Device Token

  * Login to your AllThingsTalk account and select the device you wish to connect to Hut34
  * Click on settings 

![alt text](/images/AttDoc1.png)

  * From the settings side bar, select authentification. Keep this tab open. 

![alt text](/images/AttDoc2.png)

### 2. Register a new device of Hut34 Platorm

  * From your Hut34 account, click on "Register a Device" link at the top of the page.
  * Complete device information and select "AllThingsTalk" under "Connection Type".
  * Copy and paste the Device ID and Device Token from ATT that you found during step 1.

![alt text](/images/AttDoc3.png)

### 3. How it works

  * Even though device data may be streamed continuously to ATT, Hut34 will only retrieve data upon request.
  * When your data is requested, Hut34 will make an API request to ATT retrieve your device’s state.
  * The response JSON from ATT will be parsed and returned to the requester.


## Microsoft Bot Framework Integration

You can integrate your Microsoft Bot framework bot with Hut34 by following these steps.

### 1. Register a new device in Hut34

  * Login to your Hut34 account. Click on "Register a Device" link at the top of the page.
  * Enter device information ("device name", "invocation" and "entropy fee" per transaction). Now select "MS Bot Framework" under "Connection Type".
  * Enter your bot's webhook url.

![alt text](/images/Ms_doc1.png)

  * Press the "Register" button. You will be redirected to the "Device List" page. Now copy your device key. This will be used in next step. 

![alt text](/images/Ms_doc2.png)

### 2. Send a question to Hut34 and get a response

Sending your question to Hut34 and getting responses is simple. To send your question you need to post a JSON to your Hut34. You can use following Payload class. 

<div class="center-column"></div>
```javascript
    public class Payload {
      public string message { get; set; }
    }
```

You will need to post your JSON to the following url by replacing [DEVICE_KEY] with the key that you found from the previous step.

`https://platform.hut34.io/botquery?platform=msft&key=[DEVICE_KEY]`

Here is the the complete code in C#:

<div class="center-column"></div>
```c#
var client = new WebClient();
        client.Headers[HttpRequestHeader.ContentType] = "application/json";
        var json = JsonConvert.SerializeObject(new Payload { message = "what is day today." });
        string response = client.UploadString("https://platform.hut34.io/botquery?platform=msft&key=[Device_Key]", json);
```

Your query will be processed by the platform. Hut34's response will be sent back to you in the following JSON format. 


<div class="center-column"></div>
```json
    {
        {
        "id": "b224cad-7337-448b-82dd-5ce6d77a755",
        "timestamp": "17-11-2017 11:35:42",
        "message": "response from hut34",
        "response": {
            "source": "source of response",
            "resolvedQuery": "original query",
            "isComplete": true
        },
        "input": {
            "source": "calling bot",
            "Key": "35686e7518v74dabnvbbd9224acd8d"
        },
        "status": {
            "code": 2,
            "type": "success"
        },
        "error": {
            "code": 1,
            "description": ""
        }
    }
```


### 3. Replying to questions sent by Hut34

To reply questions sent by Hut34, you will need to create a webhook that listens to questions posted by the platform and send back a reply. Hut34 will post the following JSON to your webhook:

<div class="center-column"></div>
```json
  {
    "message": "Insert your query here."
  }
```

You can get questions from message attribute and send response back by responding with the same JSON after populating your answer in the message attribute. You can use the Payload class (provided above) to process the JSON if you want. Webhooks can be created in different ways by extending the WebHookHandler class and overriding the ExecuteAsync method. You can also write a custom method code to handle post requests in Global.asax class.


## Generic Webhook Integration

The Hut34 Platfrom natively supports AllThingsTalk, Dialogflow and Microsoft Bot framework. It also supports custom integrations using generic webhooks. You can integrate your bot or device with the Hut34 Platform using generic webhooks.

### 1. Register new device on Hut34

  * Login to your Hut34 account. Click on"Register a Device" at the top of the page.
  * In device registration, enter the device name and other requested details. Now select "Generic Webhook" under "Connection Type".
  * Enter your device's webhook url.

![alt text](/images/gen1.png)

  * Press the "Register" button. You will be redirected to the device list page and you should now see your device listed. Now copy your "Device Key" from device list. This will be used in the next step. 

![alt text](/images/gen2.png)

### Post query to Hut34 and get response 

Sending your query to Hut34 and receiving a response is straightforward. To send your question you need to post the following JSON to Hut34. 

<div class="center-column"></div>
```json
{
  "message": "Insert your query here."
}
```

You will need to post your JSON to the following url by replacing [DEVICE_KEY] with the key that you got from the previous step.

`https://platform.hut34.io/botquery?platform=gen&key=[DEVICE_KEY]`

Make sure to set the 'Content-Type' header to 'application/json'. 

<div class="center-column"></div>
```json
{
  "message": "Hut34 response."
}
```

Your query will be processed by the platform. Hut34's response will be sent back to you in the following JSON format. 

<div class="center-column"></div>
```json
    {
        "id": "b224cad-7337-448b-82dd-5ce6d77a755",
        "timestamp": "17-11-2017 11:35:42",
        "message": "response from hut34",
        "response": {
            "source": "source of response",
            "resolvedQuery": "original query",
            "isComplete": true
        },
        "input": {
            "source": "calling bot",
            "Key": "35686e7518v74dabnvbbd9224acd8d"
        },
        "status": {
            "code": 2,
            "type": "success"
        },
        "error": {
            "code": 1,
            "description": ""
        }
    }
```

### 3. Replying to questions sent by Hut34

To reply to questions sent by Hut34 you will need to create a webhook that listens to questions posted by hut34 and sends back a reply. Hut34 will post the following JSON to your webhook:

<div class="center-column"></div>
```json
    {
    "message": ""
    }
```

You can get questions from message attribute and send a response back by responding with the same JSON format after populating your answer in the message attribute.


# Hut34 Wallet API #

<aside class="notice">
	<b>Wallet as a service API v.0.0.1</b>
	<ul>
		<li>API based security applies to all URLs /api/v1/</li>
		<li>Regular OAuth based security applies to all other URLs</li>
		<li>Use other providers such as etherscan to query balances/tx history, etc.</li>
	</ul>
</aside>


## Base URL

All endspoints are served at **https://walletbeta.hut34.io/api/v1**


## Authentification

<aside class="notice">
	<b>Note:</b> Addresses must be secured with a password in order to be used by the API. This is setup during address creation. The address will display a "Password Secured" tag if it is eligible. The API tag refers to addresses created via API.
</aside>

![alt text](/images/wallet1.png)

To enable API access, first log in to your wallet, and select ‘API access’ from the top right hand corner of the screen. Record your API key, and ensure all requests contain an ‘Authorization’ request header as follows:

`Authorization: Hut34 [APIKEY]`

For example:

`Authorization: Hut34 CKooV3KaDPg6K70H06aF9IgQi7zFCSkO`


## Endpoints

###/addresses (GET)

Method: GET

Returns: List of addresses for this user that can be managed via API i.e. password protected addresses. 

<div class ="center-column" ></div>
```json
[
  {
    "created": "2018-05-15T15:34:10.938+10:00",
    "updated": "2018-05-15T15:34:10.938+10:00",
    "address": "0x1a7ede45aac5ea993f5d3a5c1c966764d1313740"
  },
  {
    "created": "2018-05-14T11:35:04.142+10:00",
    "updated": "2018-05-14T11:35:04.142+10:00",
    "address": "0xf7b024a32cE0183616ee62bBA00786a71e987390"
  } 
]
```

###/addresses (POST)

Method: POST

Content: JSON object containing a new password

<div class= "center-column"></div>
```json
{
"password": "p455w0rd"
}
```

Returns: Newly created address details;
<aside class = "notice">
  <b>Note:</b> This creates a new address with the password you have chosen.
</aside>


<div class = "center-column"></div>
```json
{
  "created": "2018-05-15T15:34:10.938+10:00",
  "updated": "2018-05-15T15:34:10.938+10:00",
  "address": "0x1a7ede45aac5ea993f5d3a5c1c966764d1313740"
}
```


###/addresses/{source address}/send/eth

Method: POST
Content: JSON object as follows;

<div class = "center-column"></div>
```json
{
"password": "p455w0rd",
"to": "0xADDRESS",
"value": "100000000000000"
}
```

<aside class= "notice">
  <b>Note:</b> Value is in Wei, nonce is fetched automatically, gas limit is set to 21,000. "to" address is the destination address.
</aside>

Returns: Submitted tx hash

<div class="center-column"></div>
```json
{
"transactionHash": "0x6382c86eeb550cfb32eda5bb466ec99208b48a72532b01d8a53d3d2a1fe990f6"
}
```

###/addresses/{Source address}/send/tokens

Method: POST
Content: JSON object as follows

<div class = "center-column"></div>
```json
{
  "password": "p455w0rd",
  "to": "0xADDRESS",
  "tokenAddress": "0xTOKENADDRESS",
  "value": "100000000000000"
}
```

<aside class= "notice">
  <b>Note:</b> Nonce and gas price will be automatically fetched. Gas limit is fixed at 100,000 (as per user flow).
</aside>

Returns: Submitted tx hash

<div class = "center-column"></div>
```json
{
  "password": "p455w0rd",
  "to": "0xADDRESS",
  "tokenAddress": "0xTOKENADDRESS",
  "value": "100000000000000"
}
```


### Redirecting 

`https://walletbeta.hut34.io/?redirectAddressTo={redirect address}`

After sign in this will perform a redirect in the browser to the given address, with the parameter 'walletAddress' set to the first address in the wallet.

For example: 

`https://walletbeta.hut34.io/?redirectAddressTo=https://hut34.io`

will direct to 

`https://hut34.io/?walletAddress=0xf7b024a32cE0183616ee62bBA00786a71e987390`












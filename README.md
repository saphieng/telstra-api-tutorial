# Telstra Messaging API Tutorial
 This repo is for setting up and testing the telstra messaging API.

 ## Setup
 Before start we need to install some software and setup our account:
 1. Download and install [Postman](https://www.postman.com/downloads/)
 2. Create a [Telstra Dev Account](https://dev.telstra.com/)
 3. Create a [Particle Account](https://login.particle.io/signup?redirect=https%3A%2F%2Fwww.particle.io%2F) (Optional)

 ## Messaging API Docs
 To use the API we need to know how to format our calls. For this we will reference the [API Docs](https://dev.telstra.com/content/messaging-api#section/Introduction).

## Import Telstra API Examples
Testing of the API is made simple by using a pre-built Postman collection. You can import the collection by:
- Installing the collection from [Postman Online](https://app.getpostman.com/run-collection/44f2a97ae4c9689ce53e#?env%5BMessaging%20API%20Environment%5D=W3sidmFsdWUiOiJ0YXBpLnRlbHN0cmEuY29tIiwia2V5IjoiaG9zdCIsImVuYWJsZWQiOnRydWV9LHsidmFsdWUiOiIiLCJrZXkiOiJjbGllbnRfaWQiLCJlbmFibGVkIjp0cnVlfSx7InZhbHVlIjoiIiwia2V5IjoiY2xpZW50X3NlY3JldCIsImVuYWJsZWQiOnRydWV9LHsidmFsdWUiOm51bGwsImtleSI6ImFjY2Vzc190b2tlbiIsImVuYWJsZWQiOnRydWV9LHsidmFsdWUiOiIiLCJrZXkiOiJtZXNzYWdlX2lkIiwiZW5hYmxlZCI6dHJ1ZX1d)

or you can import the JSON collection stored in this Repo using the **Import** buton in Postman.
## Create an App
The first step to using the API is creating an App on your Telstra Dev Account:
1. Navigate to [My Apps](https://dev.telstra.com/user/me/apps)
2. [Create](https://dev.telstra.com/user/20431/add-app) and new App
3. Give the app a name and description and hit **Create**

Now you an app that you can use to play with the API. Take note of the Client ID and Secret as we will need it to get our Access Token.

## Setup Postman Variables
To make using the collection easier we can set up some variables in Postman that automatically get linked to the API call templates. To set these up follow these steps:
1. Click the gear icon next to the Environment Drop Down on Postman
2. In the pop-up window click **Import**
3. Select the `Telstra API.postman_environment.json` from this repo
4. Edit the variables by setting the inital value of `client_id` and `client_secret` to the ID and Secret provided in the app settings of your telstra account.

If you want to setup the environment variables manually follow these steps:
1. Click the gear icon next to the Environment Drop Down on Postman
2. Click **Add** on the pop-up window
3. In the **VARIABLE** field type **host** and set the **INITIAL VALUE** to `tapi.telstra.com`
4. Repeat this for **client_id** and add the client ID located under your app settings
5. Repeat this for **client_secret** and add the client secret located under your app settings
6. Repeat this for **access_token** but leave the **INITIAL VALUE** blank for now.

## Get Access Token
To make any calls using the Telstra API we need an access token. The access tokens only last one hour so when implementing in your own device keep in mind you will need to have the regular retrieval of an access token as part of your work flow. To get your access token follow these steps:
1. In the Postman collection go to the **Getting Started** folder
2. Open the **Generate OAuth Token** call
3. Make sure the **Telstra API** Environment is selected in the environment drop down
4. Hit **Send**

You will recieve a response containing your access token. It will look something like this.
```
{
    "access_token": "XZnecF4c0QfyYEGRo730pyEAbtro",
    "token_type": "Bearer",
    "expires_in": "3599"
}
```
The `access_token` environment variable will now be updated. Read more about the call in the [API docs for Generate OAuth2 token](https://dev.telstra.com/content/messaging-api#operation/authToken)

## Subscribe
All app have to have a subscription created for them before they can function. to do this:
1. In the Postman collection go to the **Getting Started** folder
2. Open the **Create Subscription** call
3. Make sure the **Telstra API** Environment is selected in the environment drop down
4. Hit **Send**

Assuming you have already run the **Get Access Token** call you should get a subscription response that looks like this:
```
{
    "destinationAddress": "+61411111111",
    "expiryDate": 1595383819180,
    "activeDays": "13"
}
```
You can now use your app for API call for the next 30 days. At that point you will have to re-subscribe. If you want to extend this period you have to sign up with a paid account. You can read more in the [API docs for Subscription](https://dev.telstra.com/content/messaging-api#operation/createSubscription).
## Single Message
To send a single message:
1. In the Postman collection go to the **Getting Started** folder
2. Open the **Send SMS Message** call
3. In the **Body** field copy the following data replacing the phone number with the number you want to send a message to:
    ```
    {
        "to":["+61411111111"],
        "validity":"60",
        "priority":false,
        "body":"Hello World!"
    }
    ```
4. Hit **Send**

You should recieve a response that looks something like this:
```
{
    "messages": [
        {
            "to": "+61411111111",
            "deliveryStatus": "MessageWaiting",
            "messageId": "CE3111A00FC1EA618F76010007008727",
            "messageStatusURL": "https://tapi.telstra.com/v2/messages/sms/CE3111A00FC1EA618F76010007008727/status"
        }
    ],
    "Country": [
        {
            "AUS": 1
        }
    ],
    "messageType": "SMS",
    "numberSegments": 1
}
```
You might have noticed that the `to` key is actually an array. If you want to send the same message to multiple phone number then you can simply add numbers to the array like so:
```
{
    "to":["+61411111111","+61422222222"],
    "validity":"60",
    "priority":false,
    "body":"Hello World!"
}

```
You can read more about sending messages in the [API docs for Send SMS](https://dev.telstra.com/content/messaging-api#operation/sendSms)

## Multi-Message
If you want to send personalised messages in a single API call you can do that as well!
1. In the Postman collection go to the **Getting Started** folder
2. Open the **Send SMS Message** call
3. Change the API url from `https://{{host}}/v2/messages/sms` to `https://{{host}}/v2/messages/sms/multi`
4. In the **Body** field copy the following data replacing the phone number with the number you want to send a message to:
    ```
    {
        "smsMulti": [
            {
            "to": "+61411111111",
            "body": "Hi Mum",
            "receiptOff": "true"
            },
            {
            "to": "+61422222222",
            "body": "Hi Dad",
            "receiptOff": "true"
            },
        ],
        "notifyURL": "http://www.example.com/"
    }
    ```
This allows you to send unique messages to each of your numbers. This is handy if you want to personalise a small section of the message like the receipients name.

Read more about the call in the [API docs for Send Multiple SMS](https://dev.telstra.com/content/messaging-api#operation/sendMultipleSms)
## Create a Particle Webhook
Its super straight forward to create a particle webhook that hit the Telstra API. For this we will link the video demonstration of this being setup after the event.

## Nanny Alert
A practical application for the Telstra Messaging api I used was in building an emergency alert system for my Nan. This used a Particle Electron and an RF key fob. It allows Nan to easily send for for by pressing the button on her fob. Once she does, SMS messages are sent to family to let them know she is in trouble. You can view the [Nanny Alert project](https://github.com/saphieng/nanny-alert) on my Github.


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
The `access_token` environment variable will now be updated.

## Subscribe

## Single Message
## Multi-Message
## Create a Particle Webhook
## Demonstartion using Nanny Alert


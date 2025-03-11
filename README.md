# Postman

## How to manage an access token for Entra ID app registrations

This guide sets out how to setup Postman to use an access token for Entra ID using an app registration that has a secret set. For example this could be used to test oauth authentication for odata based applications that utilise an Entra App Registration

### Prerequisites

- Installed Postman

- Created an App Registration

- Created a secret in the configuration of the App Registration

### Create a token request

- Within Postman click File > New

- Select Environment and give it a name

- Under the Variable header, add a new variable named access_token:

![image](https://github.com/user-attachments/assets/6c390260-8a63-44c6-9b29-bbccbcf0a66c)

- Create a new request with POST operation and name it something like Token Request - Auth Code

- In the Enter URL or paste text field enter: https://login.microsoftonline.com/<YOUR_TENANT_ID>/oauth2/v2.0/token

- Under the Headers tab enter the following key:

| Host | login.microsoftonline.com |
| ---- | ------------------------- |

- Under the Body tab enter the following keys:

| Key | Value |
| --- | ----- |
| client_Id | The client_id of your app registration |
| client_secret | The app registration secret you created |
| grant_type | This should be client_credentials |
| Scope | This very much depends on the API target you are querying. Typically this is your target API URL with /.default appended |


- Under the Scripts tab select Post-response and enter the following text:

```script
var resp = pm.response.json();
pm.environment.set("access_token", resp.access_token);
```

- Click Save and Send the `Token Request - Auth Code` request.

- You should receive a 200 OK response and if you check the Body of the response you will see an access_token

- You can check that the script has done its job by clicking on Environments on the lefthand side, selecting your environment and checking the Current value of the access_token

### Test the access token

- In order to test the access token, create a new request for your target API

- Under the Headers tab enter the following key:

| Key | Value |
| --- | ----- |
| Authorization | Bearer {{access_token}} |

- Ensure you have the Environment selected at the top right and click Send

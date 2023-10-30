# Guide-to-Immutable-Passport-Integration

## Table of Contents

* [Introduction to immutable passport](#Introduction-to-immutable-passport)
* [Creating a simple application](#Creating-a-simple-application)
* [Registering your application on Immutable Developer Hub](#Registering-your-application-on-Immutable-Developer-Hub)
* [Installing and initializing the Passport client](#Installing-and-initializing-the-Passport-client)
     * [Initializing the Passport module](#Initializing-the-Passport-module)
     * [Configuring your application to use the Passport module](#Configuring-your-application-to-use-the-Passport-module)
* [Logging in a user with Passport](#Logging-in-a-user-with-Passport)
* [Logout users](#Logging-out-a-user)
* [Initiate a transaction from Passport](#Initiate-a-transaction-from-Passport)

## Introduction
Immutable Passport is a new authentication protocol for web3 applications that uses a non-custodial wallet to store user credentials and a decentralized identity system to give users control over their own identity and data. It is more secure, user-friendly, and privacy-preserving than traditional authentication protocols. This guide will walk you through the step-by-step process of integrating Immutable Passport into your application. By the end of this guide, you will be able to create a simple application, register your application, installing and initializing the passport client, log in a user with passport, and more.

## Creating a simple application
Create a simple application that integrates Immutable Passport by following these steps:

1. **Clone the Template Repository**: Start by cloning the base template repository which is made with Vite and SvelteKit and includes the Immutable SDK for implementing passport functionality. You can clone it using the following command:
```bash
git clone https://github.com/Arturski/immutable-base-project-svelte.git
cd immutable-base-project-svelte
```
2. **Install Dependencies**: Next, install the necessary dependencies and test your environment with these commands:
   
```bash

npm i
npm run dev -- --open

```
3. **Create a Store for the Passport Provider Object**: Create a new file `src/store.ts` and add the following code, replacing placeholders with actual values:
   
```javascript
import { writable } from "svelte/store";
import {config, passport} from "@imtbl/SDK";

const passportConfig = {
  clientId: "APP_ID",
  redirectUri: "CALLBACK URL",
  logoutRedirectUri: "LOGOUT_URL",
  scope: "transact openid offline_access email",
  audience: "platform_api",
  baseConfig: new config.ImmutableConfiguration (
    {
      environment: config.Environment.SANDBOX, // Set the appropriate environment value
      apiKey: "", // Provide the apiKey if required
    }),
};

const passportInstance = new passport.Passport (passportConfig);

export const providerStore = writable<undefined | null> (null);
export const passportStore = writable<passport.Passport> (passportInstance);

```
4. **Create the Auth Helper Module**: Create a new file `src/auth.ts` and define the login and handleLoginCallback functions using the provided code to manage authentication.
5. **Create Pages and Routing**: Inside the `src/routes` directory, create Svelte components for different pages and layouts.

For more detailed instructions, please refer to the official [Immutable Documentation](https://devsupport.immutable.com/en/articles/8260567-passport-connect-to-your-authentication-client). Remember, integrating Immutable Passport into your application not only simplifies the user onboarding process but also provides access to a passionate audience of gamers.

## Registering your application on Immutable Developer Hub
To register your application on the Immutable Developer Hub, you need to follow these steps:

1. Go to the [Immutable Developer Hub](https://www.immutable.com/products/developer-hub).
2. Authenticate your account using Immutable Passport.
3. Register your application as an OAuth 2.0 client by following the **Add Client** button on the Passport page.
4. Enter the following information:

    * **Property:** A unique identifier for your application.
      
    * **Description:** A brief description of your application.
      
    * **Application Type:** Select the type of application you're developing. Currently, only **Web Application** is supported.
      
    * **Client Name:** The name that will be displayed to users when they login to your application using Passport.
      
    * **Logout URLs:** The URL that users will be redirected to when they log out of your application.
      
    * **Callback URLs:** The URL that users will be redirected to once the authentication process is complete. This is also where your application will process the authentication response.
      
    * **Web Origins URLs:** (Optional) A list of URLs that are allowed to request authorization from your application. This field is only available when you select the **Native Application** type.
      
5. Click the **Create Client** button.

Once you have created a client (Creating a client allows you to configure settings specific to your application, such as the permissions you require from users), you will be given a Client ID and Client Secret. You will need these to initialize the Passport module in your application. 

**Important:** Be sure to keep your Client ID and Client Secret secure. Do not share them with anyone else.

Here are some additional things to keep in mind when registering your application on the Immutable Developer Hub:

* You can only register one client per application. If you need to change any of the settings for your client, you can do so by clicking the **Edit** button next to your client name.
* You can delete your client by clicking the **Delete** button next to your client name. However, this will revoke all access tokens that have been issued to your client.
* If you are developing a native application, you will need to add the following code to your application's manifest file:

```
<oauth:android:client_id>YOUR_CLIENT_ID</oauth:android:client_id>
<oauth:android:client_secret>YOUR_CLIENT_SECRET</oauth:android:client_secret>
```

Replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with the Client ID and Client Secret that you received when you registered your application.

Once you have registered your application and initialized the Passport module, you will be able to use Passport to authenticate your users' access to their Immutable accounts and plug directly into a secure ecosystem of web3 tooling.


## Installing and initializing the Passport client
To install and initialize the Passport client, you will need to:

1. Install the Immutable SDK in your project.
2. Initialize the Passport module with your Client ID and Client Secret.
3. Configure your application to use the Passport module.

**Installing the Immutable SDK**

To install the Immutable SDK, follow these steps:

1. Open a terminal or command prompt.
2. Navigate to the root directory of your project.
3. Run the following command:

```
 npm install @immutable-x/sdk

 yarn add --dev @imtbl/sdk

```
**Dependencies**
```
npm install -D typescript ts-node

yarn add --dev typescript ts-node

```
This will install the Immutable SDK as a dependency in your project.

**Initializing the Passport module**

To initialize the Passport module, you will need to pass in your Client ID and Client Secret. You can do this by calling the `Passport.initialize()` method with the following parameters:

```
Passport.initialize({
  clientId: "YOUR_CLIENT_ID",
  clientSecret: "YOUR_CLIENT_SECRET"
});
```

You should call this method in your application's startup routine.

**Configuring your application to use the Passport module**

Once you have initialized the Passport module, you need to configure your application to use it. This will involve creating a new Passport strategy and registering it with the Passport module.

The specific steps involved in this process will vary depending on the programming language and framework you are using. However, here is a general overview:

1. Create a new Passport strategy. This strategy will be responsible for authenticating users and obtaining access tokens.
2. Register the new strategy with the Passport module. This will allow you to use the strategy in your application.
3. Configure your application to use the Passport module to authenticate users. This will typically involve adding a Passport middleware to your application's routing stack.

**Example**

Here is an example of how to install and initialize the Passport client in a Node.js application:

```
import { config, passport  } from '@imtbl/sdk';

const passportInstance = new passport.Passport({
  baseConfig: new config.ImmutableConfiguration({
    environment: config.Environment.PRODUCTION,
  }),
  clientId: '<YOUR_CLIENT_ID>',
  redirectUri: 'https://example.com',
  logoutRedirectUri: 'https://example.com/logout',
  audience: 'platform_api',
  scope: 'openid offline_access email transact'
});

// Register the new strategy with the Passport module
Passport.use(passportStrategy);

// Configure your application to use the Passport module to authenticate users
// ...

```

Once you have configured your application to use the Passport module, you can use it to authenticate users and access their Immutable accounts.

For more information on using the Passport module, please see the official documentation: [Immutable Passport Documentation](https://docs.immutable.com/docs/zkEVM/products/passport).

## Logging in a user with Passport

### To log in a user with Passport, you will need to:

1. Redirect the user to Passport's authentication page.
2. Once the user has authenticated, Passport will redirect the user back to your application with an access token.
3. Store the access token in your application and use it to access the Immutable API on behalf of the user.

**Redirecting the user to Passport's authentication page**

To redirect the user to Passport's authentication page, you can use the following code:

```
Passport.authenticate("immutable")(req, res);
```

This will redirect the user to Passport's authentication page with the following parameters:

* `response_type`: The type of response that your application expects. In this case, you should use `code`.
* `client_id`: Your Client ID.
* `redirect_uri`: The URL that Passport should redirect the user to once they have authenticated. This should be the same URL that you configured in the Immutable Developer Hub.

**Obtaining the access token**

Once the user has authenticated, Passport will redirect them back to your application with an access token in the `code` parameter. You can use the following code to obtain the access token:

```
const code = req.query.code;

Passport.tokenExchange(code, async (err, accessToken) => {
  if (err) {
    // Handle error
  }

  // Store the access token in your application

});
```

**Using the access token to access the Immutable API**

Once you have obtained the access token, you can use it to access the Immutable API on behalf of the user. To do this, you will need to include the access token in the `Authorization` header of your requests.

For example, to get the user's wallet address, you could use the following code:

```
const axios = require("axios");

const headers = {
  Authorization: `Bearer ${accessToken}`
};

const response = await axios.get("https://api.immutable.com/v1/users/me", { headers });

const walletAddress = response.data.walletAddress;
```

**Example**

Here is an example of how to log in as a user with Passport in a Node.js application:

```javascript
const Passport = require("@immutable-x/sdk").Passport;

app.get("/login", async (req, res) => {
  // Redirect the user to Passport's authentication page
  Passport.authenticate("immutable")(req, res);
});

app.get("/callback", async (req, res) => {
  // Obtain the access token
  const code = req.query.code;
  const accessToken = await Passport.tokenExchange(code);

  // Store the access token in the user's session
  req.session.accessToken = accessToken;

  // Redirect the user to the home page
  res.redirect("/");
});

app.get("/", async (req, res) => {
  // Get the user's wallet address
  const accessToken = req.session.accessToken;
  const walletAddress = await getWalletAddress(accessToken);

  // Display the user's wallet address
  res.send(`Your wallet address is: ${walletAddress}`);
});

async function getWalletAddress(accessToken) {
  const axios = require("axios");

  const headers = {
    Authorization: `Bearer ${accessToken}`
  };

  const response = await axios.get("https://api.immutable.com/v1/users/me", { headers });

  return response.data.walletAddress;
}
```

This is just an example, and you may need to modify it to fit your specific application. However, it should give you a good starting point for logging in users with Passport.

## Displaying on the app the user's information: ID token, access token obtained from authenticating with Passport after login, and the user's nickname

To display the ID token, access token, and user's nickname on the app after login, you can use the following steps:

1. Obtain the ID token and access token from the Passport authentication response.
2. Decode the ID token to get the user's nickname.
3. Display the ID token, access token, and nickname on the app.

**Obtaining the ID token and access token**

To obtain the ID token and access token from the Passport authentication response, you can use the following code:

```
const code = req.query.code;

const tokens = await Passport.tokenExchange(code);

const idToken = tokens.idToken;
const accessToken = tokens.accessToken;
```

**Decoding the ID token**

To decode the ID token, you can use the following code:

```
const jwt = require("jsonwebtoken");

const decodedToken = jwt.decode(idToken, { complete: true });

const nickname = decodedToken.payload.name;

```
In this example, 'jwt.decode' is a function provided by a library for decoding JWTs

**Displaying the ID token, access token, and nickname**

To display the ID token, access token, and nickname on the app, you can use the following code:

```
res.render("index", {
  idToken,
  accessToken,
  nickname
});

```

**Example**

Here is an example of how to display the ID token, access token, and nickname on the app in a Node.js application:

```javascript
const Passport = require("@immutable-x/sdk").Passport;
const jwt = require("jsonwebtoken");

app.get("/login", async (req, res) => {
  // Redirect the user to Passport's authentication page
  Passport.authenticate("immutable")(req, res);
});

app.get("/callback", async (req, res) => {
  // Obtain the ID token and access token
  const code = req.query.code;
  const tokens = await Passport.tokenExchange(code);

  const idToken = tokens.idToken;
  const accessToken = tokens.accessToken;

  // Decode the ID token to get the user's nickname
  const decodedToken = jwt.decode(idToken, { complete: true });
  const nickname = decodedToken.payload.name;

  // Display the ID token, access token, and nickname on the app
  res.render("index", {
    idToken,
    accessToken,
    nickname
  });
});

app.get("/", (req, res) => {
  // Render the index page
  res.render("index");
});

```

This is just a basic example, and you may need to modify it to fit your specific application. However, it should give you a good starting point for displaying the ID token, access token, and nickname on the app after login.

## Logging out a user

To log out a user who logged in with Passport, you need to:

1. Clear the user's session.
2. Revoke the access token.

**Clearing the user's session**

To clear the user's session, you can use the following code:

```
req.session.destroy();
```

This will clear all of the session data associated with the user.

**Revoking the access token**

To revoke the access token, you can use the following code:

```
await Passport.revokeToken(accessToken);
```

This will revoke the access token and prevent the user from using it to access the Immutable API.

**Example**

Here is an example of how to log out a user who logged in with Passport in a Node.js application:

```javascript
const Passport = require("@immutable-x/sdk").Passport;

app.get("/logout", async (req, res) => {
  // Clear the user's session
  req.session.destroy();

  // Revoke the access token
  const accessToken = req.session.accessToken;
  await Passport.revokeToken(accessToken);

  // Redirect the user to the home page
  res.redirect("/");
});
```

This is just a basic example, and you may need to modify it to fit your specific application. However, it should give you a good starting point for logging out users who logged in with Passport.

**Additional notes:**

* It is important to note that revoking the access token will only prevent the user from using it to access the Immutable API. It will not prevent the user from accessing other resources, such as your website or application.
* You may also want to consider clearing the user's cookies when you log them out. This will help to prevent the user from being automatically logged back in if they return to your website or application.
* If you are using Passport with other authentication providers, such as Google or Facebook, you will also need to log them out of those providers.

## How to Initiate a transaction from Passport, such as sending a placeholder string and obtaining the transaction hash

To initiate a transaction from Passport, such as sending a placeholder string and obtaining the transaction hash, you can use the following steps:

1. Initialize the Passport module with your Client ID and Client Secret.
2. Create a new Passport strategy. This strategy will be responsible for authenticating users and obtaining access tokens.
3. Register the new strategy with the Passport module.
4. Configure your application to use the Passport module to authenticate users.
5. Once the user has been authenticated, obtain the access token.
6. Use the access token to initiate the transaction.
7. Obtain the transaction hash.

**Example**

Here is an example of how to initiate a transaction from Passport in a Node.js application:

```javascript
const Passport = require("@immutable-x/sdk").Passport;

// Initialize the Passport module
Passport.initialize({
  clientId: "YOUR_CLIENT_ID",
  clientSecret: "YOUR_CLIENT_SECRET"
});

// Create a new Passport strategy
const PassportStrategy = require("passport-oauth2");
const passportStrategy = new PassportStrategy({
  authorizationURL: "https://api.immutable.com/auth/authorize",
  tokenURL: "https://api.immutable.com/auth/token",
  clientId: "YOUR_CLIENT_ID",
  clientSecret: "YOUR_CLIENT_SECRET"
});

// Register the new strategy with the Passport module
Passport.use(passportStrategy);

// Configure your application to use the Passport module to authenticate users
// ...

app.get("/initiate-transaction", async (req, res) => {
  // Obtain the access token
  const accessToken = req.session.accessToken;

  // Create a new transaction object
  const transaction = {
    action: "send_placeholder_string",
    placeholderString: "This is a placeholder string."
  };

  // Initiate the transaction
  const response = await Passport.initiateTransaction(accessToken, transaction);

  // Obtain the transaction hash
  const transactionHash = response.data.transactionHash;

  // Display the transaction hash
  res.send(`The transaction hash is: ${transactionHash}`);
});

```

This is just a basic example, and you may need to modify it to fit your specific application. However, it should give you a good starting point for initiating a transaction from Passport.

**Additional notes:**

* The `transaction` object can contain other parameters, depending on the type of transaction you are initiating. For more information, please refer to the Immutable API documentation.
* The `response` object will contain additional information about the transaction, such as the transaction status and the transaction fee.
* You can use the transaction hash to track the status of the transaction and to obtain the transaction receipt once it has been completed.

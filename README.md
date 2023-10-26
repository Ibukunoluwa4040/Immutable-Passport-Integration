# Guide-to-Immutable-Passport-Integration
## Creating a simple application
Create a simple application that integrates Immutable Passport by following these steps:

1. **Clone the Template Repository**: Start by cloning the base template repository which is made with Vite and SvelteKit and includes the Immutable SDK for implementing passport functionality¹. You can clone it using the following command:
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

For more detailed instructions, please refer to the official [Immutable Documentation]([https://devsupport.immutable.com/en/articles/8260567-passport-connect-to-your-authentication-client]). Remember, integrating Immutable Passport into your application not only simplifies the user onboarding process but also provides access to a passionate audience of gamers.

## Registering your application on Immutable Developer Hub
To register your application on the Immutable Developer Hub, you need to follow these steps:

1. Go to the [Immutable Developer Hub]((https://www.immutable.com/products/developer-hub)).
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

```javascript
// Install the Immutable SDK
const ImmutableSDK = require("@immutable-x/sdk");

// Initialize the Passport module
const Passport = ImmutableSDK.Passport;
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
```

Once you have configured your application to use the Passport module, you can use it to authenticate users and access their Immutable accounts.

For more information on using the Passport module, please see the official documentation: https://docs.immutable.com/docs/x/passport.

---
title: "Sign-in With Facebook"
description: "Let your users authenticate using their Facebook accounts."
headerTitle: "Auth"
layout: "guide"
weight: 4
---

### {$page.title}

###### {$page.description}

<aside>

If you are unfamiliar with our API, please visit the [API Clients](/docs/intro/api-clients/) page first to learn how to install and configure it.

</aside>

<article id="1">

## Authenticate

Your users can authenticate in a way that redirects them to Facebook's sign-in page, where they're going to type their credentials, and then redirect back to your application.

```javascript
var auth = WeDeploy.auth('https://<serviceID>-<projectID>.wedeploy.io');

var provider = new auth.provider.Facebook();
provider.setProviderScope("email");

auth.signInWithRedirect(provider);

auth.onSignIn(function(user) {
  // Fires when user is signed in after redirect.
});
```
```swift
let auth = WeDeploy.auth('https://<serviceID>-<projectID>.wedeploy.io');
let provider = AuthProvider(provider: .facebook, redirectUri: "my-app://")
provider.providerScope = "email"

auth.signInWithRedirect(provider: provider) { (user, error) in
  // Fires when user is signed in after redirect.
}

//Add WeDeploy.auth('https://<serviceID>-<projectID>.wedeploy.io').handle(url: url) in AppDelegate's open url method
```
```text/x-java
// In AndroidManifest.xml, declare an intent filter for the Activity you want to receive the token.
// Don't forget to replace [mypackagename] with you application package name.
<intent-filter>
  <action android:name="android.intent.action.VIEW" />
  <category android:name="android.intent.category.DEFAULT" />
  <category android:name="android.intent.category.BROWSABLE" />
  <data android:scheme="oauth-wedeploy" android:host="[mypackagename]" />
</intent-filter>

// On your Activity:
ProviderAuthorization.Builder provider = new ProviderAuthorization.Builder()
  .redirectUri("oauth-wedeploy://[mypackagename]")
  .provider(ProviderAuthorization.Provider.FACEBOOK)
  .providerScope("email")
  .build();

WeDeploy.auth(authUrl)
  .signIn(this, provider);

// To get the token inside the Activity that you have set the intent-filter:
Authorization authorization = TokenAuthorization.getAuthorizationFromIntent(getIntent());
```

<aside>

###### <span class="icon-16-alert"></span> Attention

Sign-in with Facebook provider is currently not available in Node.js environment. In React Native, Auth requires interacting with incoming and outgoing app links. Please read [here](https://help.wedeploy.com/user-authentication/how-to-setup-oauth-in-react-native) or check out our [example application](https://github.com/wedeploy-examples/auth-react-native-example).

</aside>

</article>

<article id="2">

## Setup app client id and secret

Create a client id and client secret by [registering your application](https://developers.facebook.com/docs/apps/register) on Facebook.

<aside>

Please be sure to add the callback url `https://<authID>-<projectID>.wedeploy.io/oauth/token` in your application.
For local development, use `wedeploy.me` instead. To use with custom domains you can change the suffix `<projectID>.wedeploy.io` for your domain.

</aside>

After retrieving the client id and client secret you can configure them as environment variables on `wedeploy.json`.

```application/json
{
  "id": "auth",
  "image": "wedeploy/auth",
  "env": {
    "WEDEPLOY_AUTH_FACEBOOK": "true",
    "WEDEPLOY_AUTH_FACEBOOK_CLIENT_ID": "<your-facebook-app-id>",
    "WEDEPLOY_AUTH_FACEBOOK_CLIENT_SECRET": "<your-facebook-app-secret>"
  }
}
```

Or you can add those environment variables [using the Console](/docs/intro/environment-variables/#2).

![Service Environment](/images/docs/auth/sign-in-with-facebook--project-container-environment.png)

</article>

## What's next?

Now we're ready to start enabling other [login providers with Google](/docs/auth/sign-in-with-google/).

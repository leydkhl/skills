# Unity - Getting Started

**Pages:** 48

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/relay/manual/introduction

---

## First steps

**URL:** https://docs.unity.com/ugs/en-us/manual/push-notifications/manual/get-started

**Contents:**
- First steps#
- Sign up#
- Link your project#
- Integration#
  - 1. Integrate the Push Notifications SDK#
    - Install the Push Notifications SDK#
      - 1. Import using the Unity Registry#
      - 2. Import by name#
      - 3. Import using the "manifest.json" file#
    - Register for Push Notifications#

To get started, you need to:

To use Push Notifications, you need to sign up for Unity Analytics, which is part of Unity Gaming Services (UGS). For more information about Analytics pricing, refer to UGS pricing.

If you don't have a Unity account, create one and create a new project to sign up for Unity Gaming Services.

Important: Only Organization Owners can sign up for Analytics.

To use the Unity Push Notifications service, you’re required to link your project to a cloud project in the Unity Editor through a project ID. Follow these steps to get your project ID.

You can find your project ID from the Settings tab in the Services window.

The Push Notifications SDK supports both iOS 10+ and Android SDK >= 26 (Oreo).

Summary of the integration steps:

The Push Notifications SDK might not be visible by default inside the Package Manager in your Unity Editor version.

You can install the Push Notifications SDK inside your Unity Project in one of three ways:

Copy the following code snippet and adjust the package version:

Go to the root of your project folder in the file explorer, then go to the package folder Packages/manifest.json.

Add the copied code at the end of the manifest.json file and append a , at the end if needed.

To receive Push Notifications, your app needs to register for Push Notifications. Follow Registering for Push Notifications for details.

To send notifications from the Unity Gaming Services dashboard, you need to upload service keys from Firebase for Android and from your Apple Developer account for iOS.

You need a Firebase Service Account Key to be added to your Unity project settings before sending notifications to Android devices. This needs to be done for every UGS environment of the game that you expect to test or use notifications with.

Return to the Dashboard and select the Next button to move to the next step in the Google Key configuration wizard. 9. Upload the private key created in the previous steps and select Finish.

For security reasons this file won’t be visible if you re-enter the edit settings page.

You need to add an Apple key, project, and account details to your Unity project settings before sending notifications to Apple devices. You can reuse the same Apple key between games, environments, and development and production builds in the Unity Dashboard (go to step 8 if you already have one). This needs to be done for every UGS environment of the game that you expect to test or use notifications with.

Log in to your Apple Developer console.

Go to the Certificates, Identifiers & Profiles page and select Keys.

Select + to create a new key.

Name your key and enable the Apple Push Notifications service (APNs) option to enable notifications, then select Continue. Note, you can only enable this capability on two keys per account.

Select Register on the next page to confirm.

Download the generated key and make note of the Key ID provided as you’ll need it later. You can only download the key file once; it needs to be revoked and regenerated if lost.

Go to Player Engagement > Notifications > Settings for your project in the Unity Dashboard, click the Set Up Keys link in the Setup banner at the top, then select Add Key or the Edit Icon in the "Apple Key" row.

There are five fields that need to be populated:

Select Finish. For security reasons this file won’t be visible if you re-enter the edit settings page.

The Mobile Notifications package has been imported into your project as a dependency of Push Notifications. In the Editor navigate to Project Settings > Mobile Notifications and select the iOS section. Make sure Request Authorization, Enable Push Notifications and Register for Push Notifications on App Launch options are checked. Refer to the Mobile Notifications documentation for more information on configuration and the additional features offered by this package.

When building the application within XCODE, ensure that the app is given the Remote Notification capabilities so it can receive notifications. You’ll receive the following error message at runtime if you fail to do this: Failed to register for remote notifications: no valid “aps-environment” entitlement string found for application

To test your Push Notification integration within your app, follow the Testing the SDK integration guide.

If you encounter any issues while testing, follow the Troubleshooting guide for debugging common issues.

You can now create your first notification campaign to start using Push Notifications.

**Examples:**

Example 1 (unknown):
```unknown
"com.unity.services.push-notifications": "4.0.1"
```

Example 2 (unknown):
```unknown
"com.unity.services.push-notifications": "4.0.1"
```

Example 3 (unknown):
```unknown
Packages/manifest.json
```

Example 4 (unknown):
```unknown
manifest.json
```

---

## Client SDK basics

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/client-sdk-basics/client-sdk-basics-toc

**Contents:**
- Client SDK basics#

The following pages cover the basic functionality of Vivox. If you haven't reviewed the Quick start guide it's recommended that you start there as an introduction to these topics.

---

## Generate a login access token

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/quickstart-guide/core-login-token

**Contents:**
- Generate a login access token#
- Login token - Header#
- Login token - Payload#
  - Example login token#
  - Some best practices for token payloads:#
- Login token - Signature#

The next step in the login process is to correctly form a login token on a game server. This step is important to understand as it contributes to a significant number of Vivox integration mistakes. Here are some key facts to understand Login Tokens in general:

Login tokens can only be used once.

Expiration timers are important; leave room for unforeseen circumstances such as slow internet connections or inactive instances.

Your game will provide its own account names and login tokens.

There are three key sections to the login tokens:

In the case of Vivox, the header is empty to satisfy the JSON Web Token (JWT) standards. The header can be formed either as code or as a constant.

Crafting a payload consists of a defined set of claims you make to the Vivox server. Your code can craft an object to store these claims. The claims apply to tokens other than login tokens, so they can be serialized and encoded when you submit your request to Vivox.

The signature combines the header and payload into a signed hash-based message authentication code (HMAC) which is then encoded. This is where the key provided by Vivox is used to protect your service and authenticate your communication.

A pseudo-code example of this can be found on the Access token signature page.

Your final result should resemble this example:

The next step is to submit your login request using the login token.

**Examples:**

Example 1 (unknown):
```unknown
Claims loginClaim = new Claims{
iss=issuer-ba63,
vxi = 93000,
vxa:login,
exp=1600349400,
f=sip:.issuer-ba63.030104_16.@tla.vivox.com };
```

Example 2 (unknown):
```unknown
Claims loginClaim = new Claims{
iss=issuer-ba63,
vxi = 93000,
vxa:login,
exp=1600349400,
f=sip:.issuer-ba63.030104_16.@tla.vivox.com };
```

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/overview/manual/unity-dashboard-introduction

---

## Sign out of a game

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/client-sdk-basics/sign-out-of-game

**Contents:**
- Sign out of a game#

When the game no longer wants the user signed in, it sends a vx_req_account_logout message to the Vivox SDK. After the user is signed out, no network traffic is sent to or is received by the Vivox SDK.

The following code is an example of the sign out process:

**Examples:**

Example 1 (unknown):
```unknown
vx_req_account_logout
```

Example 2 (unknown):
```unknown
. . .
vx_req_account_logout *req;
vx_req_account_logout_create(&req);
req->account_handle= vx_strdup(".issuer-w-dev.mytestaccountname.");
vx_issue_request3(&req->base, &request_count);
. . .
```

Example 3 (unknown):
```unknown
. . .
vx_req_account_logout *req;
vx_req_account_logout_create(&req);
req->account_handle= vx_strdup(".issuer-w-dev.mytestaccountname.");
vx_issue_request3(&req->base, &request_count);
. . .
```

---

## Sign in with the Authentication package

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/vivox-unity-sdk-basics/sign-in-with-authentication-package

**Contents:**
- Sign in with the Authentication package#

Prerequisite: Configure the Unity Package Manager Vivox package.

The easiest way to manage Vivox sign-ins is with the Unity Authentication package.

To use the Authentication package, complete the following steps.

Ensure that your project manifest.json file contains an entry for com.unity.services.authentication.

For more information, refer to the Unity documentation on the Project manifest.

Initialize UnityServices by implementing the following code snippet into a script that runs before using any Vivox functionality.

Log in by using VivoxService.Instance.LoginAsync(LoginOptions options = null).

Join a channel by using either of the following code examples depending on the type of channel.

When joining a non-positional channel use:

When joining a positional channel use:

When joining an echo channel use:

**Examples:**

Example 1 (unknown):
```unknown
manifest.json
```

Example 2 (unknown):
```unknown
com.unity.services.authentication
```

Example 3 (unknown):
```unknown
UnityServices
```

Example 4 (unknown):
```unknown
using System;
using UnityEngine;
using Unity.Services.Authentication;
using Unity.Services.Core;
using Unity.Services.Vivox;

async void Start()
{
    await UnityServices.InitializeAsync();
    await AuthenticationService.Instance.SignInAnonymouslyAsync();

    await VivoxService.Instance.InitializeAsync();
}
```

---

## Vivox Core quick start guide

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/quickstart-guide/core-quick-start

**Contents:**
- Vivox Core quick start guide#

Before you begin integrating Vivox, be sure to review the hardware permissions for capture device access for each platform you plan to develop for. If this is your first time using Vivox, familiarize yourself with the material in Before you begin.

These guides will cover the three central pieces of a Vivox voice integration. You can follow the steps in each section and confirm your setup by using the verification steps at the end of each topic.

---

## Initialize the Vivox SDK

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/client-sdk-basics/initialize-sdk

**Contents:**
- Initialize the Vivox SDK#

Before the game can issue requests to the Vivox SDK, it must first initialize the Vivox SDK by calling vx_initialize3(). This operation might cause additional shared libraries to be loaded, which can take anywhere from a few milliseconds to hundreds of milliseconds, depending on the machine.

Note: We recommend that you call vx_initialize3() on application startup.

The following code displays an example of how to initialize the Vivox SDK:

**Examples:**

Example 1 (unknown):
```unknown
vx_initialize3()
```

Example 2 (unknown):
```unknown
vx_initialize3()
```

Example 3 (cpp):
```cpp
#include"Vxc.h"
#include"VxcErrors.h"
. . .
vx_sdk_config_t defaultConfig;
int status = vx_get_default_config3(&defaultConfig, sizeof (defaultConfig));

if (status != VxErrorSuccess)
{
    printf("vx_sdk_get_default_config3() returned %d: %s\n", status,
    vx_get_error_string(status));
    return;
}

status = vx_initialize3(&defaultConfig, sizeof (defaultConfig));

if (status != VxErrorSuccess)
{
    printf ("vx_initialize3() returned %d : %s\n", status, vx_get_error_string(status));
    return;
}
// Vivox Client SDK is now initialized
```

Example 4 (cpp):
```cpp
#include"Vxc.h"
#include"VxcErrors.h"
. . .
vx_sdk_config_t defaultConfig;
int status = vx_get_default_config3(&defaultConfig, sizeof (defaultConfig));

if (status != VxErrorSuccess)
{
    printf("vx_sdk_get_default_config3() returned %d: %s\n", status,
    vx_get_error_string(status));
    return;
}

status = vx_initialize3(&defaultConfig, sizeof (defaultConfig));

if (status != VxErrorSuccess)
{
    printf ("vx_initialize3() returned %d : %s\n", status, vx_get_error_string(status));
    return;
}
// Vivox Client SDK is now initialized
```

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/mps-sdk/manual/build-your-first-session

---

## Create the connector object

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/client-sdk-basics/create-connector-object

**Contents:**
- Create the connector object#
- Disconnect the connector object#

Use a connector object after initialization to fetch server configuration information necessary for your game to login and use Vivox. The connector object is usually only called once per application startup before the first sign in.

To create the connector object, use the vx_req_connector_create request structure and the vx_req_connector_create_create() function. This is typically the first request issued after vx_initialize3().

Note: There can only be one active connector object at a time.

When the Vivox SDK processes the vx_req_connector_create request, it contacts the Vivox network and downloads configuration information to the client that's relevant to your game's communications infrastructure. This is a one-time HTTP request. The configuration information is loaded into memory before the sign in operation.

If the game's connection is lost, you don't need to issue vx_req_connector_create a second time because the information is already in memory. Simply proceed to re-sign in to the game.

The connector object doesn't hold open any ports, use bandwidth, network resources, or CPU for heartbeats, and only requires a small amount of memory to hold the configuration.

The following code displays an example creating the connector object:

Note: To use the connector object effectively, it's important to understand that the connector object doesn't establish a persistent connection to the Vivox back end that needs to be maintained while using the service.

After the game receives the vx_resp_connector_create message, the game can then sign in to Vivox.

Disconnecting the connector object unloads the configuration file from memory. You can disconnect the object by using the vx_req_connector_initiate_shutdown request.

Note: It's never necessary to call vx_req_connector_initiate_shutdown unless you intend to change which Vivox back end you connect to after the initial connection.

Use vx_uninitialize() instead of vx_req_connector_initiate_shutdown on application exit to clean up Vivox resources.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_connector_create
```

Example 2 (unknown):
```unknown
vx_req_connector_create_create()
```

Example 3 (unknown):
```unknown
vx_initialize3()
```

Example 4 (unknown):
```unknown
vx_req_connector_create
```

---

## Assign a channel name

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/quickstart-guide/core-assign-channel-name

**Contents:**
- Assign a channel name#

A channel name is important to a game to get multiple users into the same channel while also ensuring that there are no channel conflicts. Your channel name should mean something to your game to help identify how the channel is being used. See the documentation on Channel identifiers for large-scale games for more information.

Channel type identifiers:

Note: The identifiers outlined above should be unique to your game and game design.

Next step, generate a join token.

---

## Sign in to a game

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/vivox-unity-sdk-basics/sign-in-to-game

**Contents:**
- Sign in to a game#

After you initialize the Vivox SDK, you can sign a user in to the Vivox instance assigned to your game.

The name being used for players will either be the Unity AuthenticationId, or a new GUID, if Unity Authentication is not being used.

Note: Do not include your issuer or domain as a part of the username that you pass to the Account constructor. The Account constructor handles this for you.

After initialization, call the VivoxService.Instance.LoginAsync method to sign in to Vivox.

If a display name is set in LoginOptions, this name is visible to all users in channels they join; they receive it as DisplayName within the VivoxParticipant results from VivoxService.Instance.ParticipantAddedToChannel or VivoxService.Instance.ParticipantRemovedFromChannel.

Note: The Vivox SDK does not perform checks on the display name. It is the game developer's responsibility to ensure that the display name follows the rules of the game, that the font characters are supported by the in-game renderer, and that the name is checked for duplication, obscenity, and impersonation. We recommend that you check display names by using some out-of-band means (such as the game server), and not by using the game client.

The following code is an example of how to initiate the sign-in process, setting the DisplayName to "Bob", and enabling Text-to-Speech.

You can subscribe to VivoxService.Instance.LoggedIn and VivoxService.Instance.LoggedOut in order to get events when LoginAsync has been successfully called.

The following code is an example of how to subscribe to the VivoxService.Instance.LoggedIn and VivoxService.Instance.LoggedOut events, and how to handle the functions for the different LoginState values:

Note: It's recommended that a user is signed in when an application starts. In scenarios where users can sign in to different accounts within an application, it's recommended that they're signed in when they connect to the game server.

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.LoginAsync
```

Example 2 (unknown):
```unknown
VivoxService.Instance.LoginAsync
```

Example 3 (unknown):
```unknown
VivoxService.Instance.ParticipantAddedToChannel
```

Example 4 (unknown):
```unknown
VivoxService.Instance.ParticipantRemovedFromChannel
```

---

## Handle login errors

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/quickstart-guide/core-handle-login-errors

**Contents:**
- Handle login errors#
- Verify your login implementation#

When your game client receives an event from the Vivox SDK, evaluate if it was successful. For login, this is stored in the status_code field and should be 0. If it is non-zero your application should follow the below steps to attempt to reconnect.

For a more detailed login process flow, refer to Sign in to a game.

If login is unsuccessful, follow these steps:

Print an error to the debug log or the client event capture location.

Attempt to retry the login:

Once you confirm that you are logging users in successfully you can continue on to joining and leaving channels.

**Examples:**

Example 1 (unknown):
```unknown
status_code
```

Example 2 (unknown):
```unknown
void HandleLoginResponse(vx_resp_account_anonymous_login *resp)
{
    if (resp->base.return_code == 1)
    {
        printf(U, resp->base.status_code, vx_get_error_string(resp->base.status_code));
        return;
    }
    vx_req_account_anonymous_login *req = reinterpret_cast<vx_req_account_anonymous_login *>(req->base.request);  
	printf("login succeeded for account %s\n", req->acct_name);
}
```

Example 3 (unknown):
```unknown
void HandleLoginResponse(vx_resp_account_anonymous_login *resp)
{
    if (resp->base.return_code == 1)
    {
        printf(U, resp->base.status_code, vx_get_error_string(resp->base.status_code));
        return;
    }
    vx_req_account_anonymous_login *req = reinterpret_cast<vx_req_account_anonymous_login *>(req->base.request);  
	printf("login succeeded for account %s\n", req->acct_name);
}
```

Example 4 (unknown):
```unknown
status_code
```

---

## Process login responses

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/quickstart-guide/core-process-responses-events

**Contents:**
- Process login responses#

The application uses the vx_req_account_anonymous_login request to sign users into Vivox. After the game receives a successful vx_resp_account_anonymous_login message, vx_evt_account_login_state_change will change to login_state_logged_in. The event will trigger once the requested login has succeeded. It is important to wait for the state change event to trigger and confirm it was successful before attempting any other Vivox actions.

Note: To prevent racing issues where your application is attempting an action it is not yet authorized to do, configure your application to wait for Vivox events to return as successful.

Next step, check for errors and verify your implementation.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_account_anonymous_login
```

Example 2 (unknown):
```unknown
vx_resp_account_anonymous_login
```

Example 3 (unknown):
```unknown
vx_evt_account_login_state_change
```

Example 4 (unknown):
```unknown
login_state_logged_in
```

---

## Uninitialize the Vivox SDK

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/client-sdk-basics/uninitialize-sdk

**Contents:**
- Uninitialize the Vivox SDK#

The game must uninitialize the Vivox SDK by calling vx_uninitialize() before it exits the process or before it calls vx_initialize3() again. If this operation is called when a user is signed in, the method can block for up to two seconds while it cleans up network resources.

The following code is an example of the uninitialization process:

**Examples:**

Example 1 (unknown):
```unknown
vx_uninitialize()
```

Example 2 (unknown):
```unknown
vx_initialize3()
```

Example 3 (unknown):
```unknown
. . .
int status = vx_uninitialize();
if (status != 0)
{    
    printf("vx_uninitialize() returned %d: %s\n", status,vx_get_error_string(status));
}
. . .
```

Example 4 (unknown):
```unknown
. . .
int status = vx_uninitialize();
if (status != 0)
{    
    printf("vx_uninitialize() returned %d: %s\n", status,vx_get_error_string(status));
}
. . .
```

---

## Generate and submit a join token

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/quickstart-guide/core-join-token

**Contents:**
- Generate and submit a join token#
- Join token - Payload#
  - Example join token#
- Submit a join request#

To generate a join token, follow the same process that was introduced in Generate a login token.

The part you will change is the payload portion of the token.

You can re-use the same serializable class you crafted earlier for use in login tokens to craft the payload for a join token. The two key differences are a different Vivox Action: Join, and the use of the To (t) field.

Vivox has structures to store a common set of parameters to many different request types. Utilize this in your next step, which is to craft your request, and issue it to the Vivox SDK to be sent to Vivox services.

Create your join request in your game client and store any necessary parameters and the access_token received from your game server.

Issue the request to the Vivox SDK from the game client.

Next step, process join responses and events.

**Examples:**

Example 1 (unknown):
```unknown
Claims loginClaim = new Claims{
iss=testgames1234-aa12,
vxi = 93000,
vxa:join,
exp=1600349400,

f=sip:.testgames1234-aa12.030104_16.@tla.vivox.com,

t=sip:confctl-g-testgames1234-aa12.1M2n31IoU45b.t1@tla.vivox.com
};
```

Example 2 (unknown):
```unknown
Claims loginClaim = new Claims{
iss=testgames1234-aa12,
vxi = 93000,
vxa:join,
exp=1600349400,

f=sip:.testgames1234-aa12.030104_16.@tla.vivox.com,

t=sip:confctl-g-testgames1234-aa12.1M2n31IoU45b.t1@tla.vivox.com
};
```

Example 3 (unknown):
```unknown
access_token
```

Example 4 (unknown):
```unknown
vx_req_sessiongroup_add_session *req;
vx_req_sessiongroup_add_session_create(&req);
req->sessiongroup_handle = vx_strdup("sg1");
req->session_handle = vx_strdup("1M2n31IoU45b.t1");
req->uri = vx_strdup("sip:confctl-g-testgames1234-aa12.1M2n31IoU45b.t1@tla.vivox.com");
req->account_handle = vx_strdup("sip:.testgames1234-aa12.playerName@mt1s.vivox.com");
req->connect_audio = 1;
req->connect_text = 1;
req->access_token = vx_strdup(_the_access_token_generated_by_the_game_server);
```

---

## Uninitialize the Vivox SDK

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/vivox-unreal-sdk-basics/uninitialize-sdk

**Contents:**
- Uninitialize the Vivox SDK#

The game must uninitialize the Vivox SDK by calling IClient::Uninitialize() before exiting the process, or before calling IClient::Initialize() again. If this operation is called while a user is signed in, the method can block for up to two seconds while it cleans up network resources. This is a safe way to unwind all Vivox resources; it is not necessary to individually leave channels and sign out the user.

The following code displays an example of the uninitialization process:

**Examples:**

Example 1 (unknown):
```unknown
/* . . . */
// MyVoiceClient is the IClient reference that was initialized
MyVoiceClient->Uninitialize();
/* . . . */
```

Example 2 (unknown):
```unknown
/* . . . */
// MyVoiceClient is the IClient reference that was initialized
MyVoiceClient->Uninitialize();
/* . . . */
```

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/vivox-unity-sdk-basics/using-vivox-access-tokens-together-with-uas

---

## Sign out of a game

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/vivox-unity-sdk-basics/sign-out-of-game

**Contents:**
- Sign out of a game#

When the game no longer wants the user signed in, it calls the VivoxService.Instance.LogoutAsync method. After the user is signed out, there is no network traffic to or received by the Vivox SDK.

Note: This is typically called when quitting the application, or in the scenario where a user can sign in to different accounts within an app, when disconnecting from the game server.

The following code displays an example of the sign out process:

To handle sign outs caused by connection issues, the game must handle the event VivoxService.Instance.LoggedOut. The Vivox SDK sends this message when the connection to Vivox is lost unexpectedly, such as from network connectivity issues, or when a user manually initiates a sign-out.

The following code is an example of how to subscribe to the VivoxService.Instance.LoggedOut event:

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.LogoutAsync
```

Example 2 (unknown):
```unknown
using UnityEngine;
using Unity.Services.Vivox;

class LogoutExample : MonoBehaviour
{
    void LogOut()
    {
        VivoxService.Instance.LogoutAsync();
    }
}
```

Example 3 (unknown):
```unknown
using UnityEngine;
using Unity.Services.Vivox;

class LogoutExample : MonoBehaviour
{
    void LogOut()
    {
        VivoxService.Instance.LogoutAsync();
    }
}
```

Example 4 (unknown):
```unknown
VivoxService.Instance.LoggedOut
```

---

## Introduction to Remote Config

**URL:** https://docs.unity.com/ugs/en-us/manual/remote-config/manual/WhatsRemoteConfig

**Contents:**
- Introduction to Remote Config#
- Environments#
- Game Overrides#
- Remote Config Authoring#
- Samples#
- Remote Config interfaces#
- Support#

Remote Config is a cloud service that you can use to tune your game design without deploying new versions of your application. It consists of a set of namespaced Key-Value parameters, and you can optionally define a set of values that override or add to these parameters.

With Remote Config, you can:

Environments can also be structured to fit your application so that specific Game Overrides and Settings are retrieved and updated only when needed, so Game Overrides and Settings Keys can be reused.

Define Game Overrides that control which players receive what settings updates, and when. Unity manages the delivery and assignment of those settings with minimal impact to performance. No update to your application is necessary.

The service then returns customized settings for each player according to the Game Overrides that apply to them. This allows different players using the same version of your game to have slightly different experiences. It also allows you to understand the impact each experience has on your business.

Remote Config supports deployment workflows. For more information on how to write configurations with different authoring methods, please refer to Write Configuration.

Download the Unity Gaming Services Samples project to see how to implement Remote Config to solve common game development challenges:

Though you must implement Unity Remote Config in your game code, there are multiple ways to integrate and manage your application with Remote Config:

You can use REST APIs to update any Environment stored by the service.

Note: To enable this, you must implement Unity Remote Config Runtime in your game code.

The Remote Config package is under active development and subject to changes that may impact the service's stability. If you encounter any issues with Remote Config, or have any questions, please use the support ticket submission form or visit Remote Config Forum.

---

## Create your first trigger

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/triggers/tutorials/create-first-trigger

**Contents:**
- Create your first trigger#
- View trigger types#
- Create a trigger#
- List triggers#
- Edit triggers#
- Delete triggers#

This page shows a workflow for how to create and edit a trigger that validates your label names through the command line:

To associate a user script with a client or server event, you need to create a trigger. To show a list of all supported trigger types in your command line, you can use the showtypes command:

If your label names are created according to a given naming standard, you can create a trigger that validates label names. To create this trigger on Windows, you can use the following create command:

cm trigger create before-mklabel "check label name" "ruby c:\plastic\triggers\validate-label.rb"

This is a sample Ruby script (validate-label.rb) that checks that the label name starts with release:

if (ENV['PLASTIC_LABEL_NAME'] !~ /^release/) then exit(1) end

The script picks the name of the label from the PLASTIC_LABEL_NAME environment variable and checks the contents against the regular expression ^release, which matches a string that starts with release. If the label name doesn't match release (!~ operator), the trigger returns the exit code 1, which means the trigger fails and doesn't allow the mklabel operation to finish.

To see any triggers that you create, you can use the list command to show all the triggers of the same type you created:

To modify the script that this trigger is pointing to, you can use the trigger edit command. You need to indicate the trigger type and the trigger position, which is the first index that the list triggers command returns (1 in this case):

cm trigger edit before-mklabel 1 --script="c:\tmp\other-script.bat"

To remove a trigger, you can use the trigger delete. You need to indicate the trigger type and position:

cm trigger delete before-mklabel 1

**Examples:**

Example 1 (unknown):
```unknown
cm trigger showtypes
```

Example 2 (unknown):
```unknown
cm trigger create before-mklabel "check label name" "ruby c:\plastic\triggers\validate-label.rb"
```

Example 3 (unknown):
```unknown
validate-label.rb
```

Example 4 (unknown):
```unknown
if (ENV['PLASTIC_LABEL_NAME'] !~ /^release/) then exit(1) end
```

---

## Joining and leaving channels

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/quickstart-guide/core-joining-leaving-channels

**Contents:**
- Joining and leaving channels#
- Important information about channels#

After completion of this section you will be able to get your first user into a voice channel in your game.

Channels are the foundation of how Vivox connects players. While the game client has significant control over how to create channels and who can join which channels, there are some key details you need to know about channels:

A channel is identified by a URI.

A channel URI is assigned by the game. Refer to channel name criteria for more information.

Vivox has three channel types:

Players can only be in one positional audio channel at a time.

Channels are platform-agnostic, so game clients running on different platforms can connect with each other.

Inactive channels consume no Vivox resources.

The Vivox platform automatically load balances channels. Developers do not need to locate channels on a particular server.

For more information about channels, refer to the documentation on channels.

Next step, assign a channel name.

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/vivox-unity-first-steps

---

## Introduction to Vivox Safe Text

**URL:** https://docs.unity.com/ugs/en-us/manual/safe-text/manual/overview

**Contents:**
- Introduction to Vivox Safe Text#
- Safe Text features#
- Supported languages#

Safe Text is an AI-based suite of safety tools for tailoring in-game communications rules, detecting harmful messages, and collecting evidence on player behavior. If you are already using Vivox Text Chat, Safe Text can be activated without any more additional code. Once enabled, the chat filter can be immediately configured and activated and the Moderation Dashboard will begin to collect text evidence and analysis on toxic conversations in player reports.

To get started with Vivox Text Chat, refer to the Vivox documentation.

---

## Sign in to a game

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/vivox-unreal-sdk-basics/sign-in-to-game

**Contents:**
- Sign in to a game#

After you initialize the Vivox SDK, you can sign a user in to the Vivox instance assigned to your game by using an account name that is selected by the game.

Note: It’s recommended that you re-use player account names to facilitate the use of Vivox features such as cross-mute and blocking. It will also become a requirement for using future safety features designed for reducing toxicity and fostering more positive communities.

If you do not want to use a unique account name per player, or if you do not want to expose the username to the network, then use one of the following options:

The following criteria applies to account names:

0x30-0x39, 0x41-0x5A, 0x61-0x7A

Note: You can only use the percent sign for URL encoding. Follow the percent sign by two uppercase hex characters.

Each participant must have a unique username portion of the AccountId, or collisions can occur during gameplay. If a duplicate AccountId is used in a channel where that AccountId already exists, then the AccountId that is already in channel is kicked from the channel.

The game uses the method ILoginSession::BeginLogin to sign users in to Vivox. It takes as its last argument a delegate that lets you call a callback function after sign in is complete. Note that every sign in method call must have an access token that authorizes that game instance to sign in as the speciﬁed account.For more information, refer to the Access Token Developer Guide.

The following code displays an example of how to initiate the sign in process and how to handle the delegate callback:

In addition to registering an ILoginSession::FOnBeginLoginCompletedDelegate callback, the game must handle ILoginSession::EventStateChanged events that have the state value set to LoginState::LoggedOut. The Vivox SDK sends this message when whenever a sign out occurs.

When a sign out is initiated by a user, LoginState will transition through LoginState::LoggedIn to LoginState::LoggingOut to LoginState::LoggedOut. In the event of a sign out initiated by the server, or due to network loss, the LoggingOut state is skipped and the LoggedIn state changes directly to LoggedOut.

The following code displays an example of this message:

Note: We recommend that a user is signed in when an application starts. In scenarios where users can sign in to different accounts within an app, we recommend that they are signed in when they connect to the game server.

**Examples:**

Example 1 (unknown):
```unknown
/* . . . */
AccountId Account = AccountId(kDefaultIssuer, "example_user", kDefaultDomain);
ILoginSession &MyLoginSession(MyVoiceClient->GetLoginSession(Account));
bool IsLoggedIn = false;
// Setup the delegate to execute when login completes
ILoginSession::FOnBeginLoginCompletedDelegate OnBeginLoginCompleted;
OnBeginLoginCompleted.BindLambda([this, &IsLoggedIn, &MyLoginSession](VivoxCoreError Error)
{
    if (VxErrorSuccess == Error)
    {
        IsLoggedIn = true;
        // This bool is only illustrative. The user is now logged in.
    }
});
// Request the user to login to Vivox
MyLoginSession.BeginLogin(kDefaultServer, MyLoginSession.GetLoginToken(kDefaultKey, kDefaultExpiration), OnBeginLoginCompleted);
/* . . . */
```

Example 2 (unknown):
```unknown
/* . . . */
AccountId Account = AccountId(kDefaultIssuer, "example_user", kDefaultDomain);
ILoginSession &MyLoginSession(MyVoiceClient->GetLoginSession(Account));
bool IsLoggedIn = false;
// Setup the delegate to execute when login completes
ILoginSession::FOnBeginLoginCompletedDelegate OnBeginLoginCompleted;
OnBeginLoginCompleted.BindLambda([this, &IsLoggedIn, &MyLoginSession](VivoxCoreError Error)
{
    if (VxErrorSuccess == Error)
    {
        IsLoggedIn = true;
        // This bool is only illustrative. The user is now logged in.
    }
});
// Request the user to login to Vivox
MyLoginSession.BeginLogin(kDefaultServer, MyLoginSession.GetLoginToken(kDefaultKey, kDefaultExpiration), OnBeginLoginCompleted);
/* . . . */
```

Example 3 (unknown):
```unknown
void UMyGameClass::OnLoginSessionStateChanged(LoginState State)
{
    if (LoginState::LoggedOut == State)
    {
        UE_LOG(MyLog, Error, TEXT("LoginSession Logged Out Unexpectedly\n"));
        // Optionally handle other cases
    }
}
```

Example 4 (unknown):
```unknown
void UMyGameClass::OnLoginSessionStateChanged(LoginState State)
{
    if (LoginState::LoggedOut == State)
    {
        UE_LOG(MyLog, Error, TEXT("LoginSession Logged Out Unexpectedly\n"));
        // Optionally handle other cases
    }
}
```

---

## Initialize the voice client object

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/vivox-unreal-sdk-basics/initialize-voice-client-object

**Contents:**
- Initialize the voice client object#

You must create and initialize the voice client before other Vivox SDK actions can take place. During the voice client initialization process, the Vivox SDK sets up and starts all of its different subsystems. This can cause additional shared libraries to be loaded, and allows developers to customize a variety of Vivox SDK options before connecting to Vivox services. For example, a developer can select between available codecs, audio ducking settings, and logging level.

The following code displays an example of the process for obtaining and initializing the voice client:

Note: You do not need to wait for any message or callback to this method before the game can sign in to Vivox.

**Examples:**

Example 1 (unknown):
```unknown
/* . . . */
// IClient *MyVoiceClient in header.
MyVoiceClient = MyVoiceModule->VoiceClient();
MyVoiceClient->Initialize();
/* . . . */
```

Example 2 (unknown):
```unknown
/* . . . */
// IClient *MyVoiceClient in header.
MyVoiceClient = MyVoiceModule->VoiceClient();
MyVoiceClient->Initialize();
/* . . . */
```

---

## Assign a unique username

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/quickstart-guide/core-assign-username

**Contents:**
- Assign a unique username#
- Vivox account usernames#

The first step in logging in to Vivox services is to craft a unique account name for each user who intends to utilize voice or other services. You should use the same account name for the same player every time they play the game. The account name should be unique but also abstracted from any identifiable information. For example, don’t use a user's PlayStation or Xbox name, or real name. The usernames need to be generated by your game server, not assigned by Vivox. This account name will be utilized regularly in subsequent requests to the Vivox servers. For more details about account name syntax and login requests, refer to Sign in to a game.

The following are example methods for transmuting and forming usernames:

Output (MD5):98E1F108B5E78847DEC7FFF9CA486576

Identifying Code (Example: VIN)

Input:<game region>, <game shard>, <game channel>, <user number>

Output:030104_16<game region><game shard><game channel>_<user number>

.issuer-ba63.98E1F108B5E78847DEC7FFF9CA486576.

.issuer-ba63.030104_16.

Note: Vivox responds with the account name sent to it, your client or server will need to map the abstracted username back to the original user.

Next step is to generate a login access token.

---

## Introduction to resources in Economy

**URL:** https://docs.unity.com/ugs/en-us/manual/economy/manual/item-types

**Contents:**
- Introduction to resources in Economy#
- Currency#
- Inventory item#
- Virtual purchase#
- Real money purchase#
  - In-app purchase (IAP) integration#
  - Store settings configuration#
  - Unity IAP plug-in integration and receipt validation#
- Additional resources#

The Economy service works by allowing you to create different in-game units that define an economy system. Each type of unit you add to a game is called a resource. Economy comes with the following built-in configuration resource types:

Additionally, each resource type can have Custom data.

All fields that require numerical input (for example, amounts, balances) have a limit of 15 digits (integers only).

A currency in Economy defines virtual money that exists within your game. When defining currencies, there is the option to set an initial balance (the amount a player receives when you initialize the currency), and a maximum currency balance (the limit to how much of that currency a player can have). For each defined currency, Economy stores a balance against each player account, which the game manages.

Economy initializes currencies per player when the game client first requests a list of currency balances through the Economy SDK or directly from the API. This includes currencies that become available after the player has already interacted with the currency system.

For example, a player plays a game with a Coins virtual currency, then you add a Gems currency in a new version of the game with a configured initial balance set to 10. When the player first interacts with currencies after gems are available, Economy creates a balance of 10 for the player’s gems, while coins remain unaffected. Enter a value of zero to not have a currency initialized per player.

You can define a maximum balance for a currency. This is the maximum balance any player can hold for that currency in the system. This has the effect of returning an error in the game APIs when trying to increment a balance over the maximum value. If you don’t want the currency to have an upper limit, remove the maximum value or enter zero.

Inventory items are objects that a player can own in-game. You don’t need to set any property for your resources, but it is always possible to add Custom data to them.

You can associate an inventory item with a player by using the SDK or as part of a purchase (using virtual or real money). The instance of the inventory item associated with the player can have its own instance data. See Player data.

A virtual purchase is a transactional resource used to implement a shop or trade feature. Virtual purchases allow players to purchase previously defined in-game currencies or inventory items using their in-game currency balances or items already in their inventory, rather than real money.

Use cases for virtual purchases include:

Although virtual purchases are composed of a cost and a reward, it’s not mandatory to define either component. Depending on how you want to define a virtual purchase, it can be made up of either no cost or no reward.

A real money purchase is a transactional resource allowing players to purchase in-game currencies or items using real money through an app store. Use cases for real money purchases include:

To fully integrate real money transactions in your game, you must create the purchase in your game’s economy. Then, the IAP integration in Economy:

However, developers still need to support the connection to the store. The recommended best practice is to use the IAP plug-in to perform this integration.

The IAP plug-in and Economy perform two different roles in creating a purchase that requires players to buy resources with real money. Economy performs the validation of a receipt against a purchase and awards the resource(s) to the player, while the IAP plug-in integrates the game with the store for store initialization and collects the receipt.

Before you can configure the Economy service to redeem and verify in-app purchases, you must add some products to both the Apple App Store and the Google Play Store (depending on where you plan to make your game available).

For both stores, you must set up a Product ID in the configuration. The same Product ID is then used in the configuration of real money purchases. The two stores have different steps to follow, so refer to their documentation for the full setup instructions.

Google Play Store configuration requires additional IAP settings.

Once Economy receives the receipt data from the Unity IAP plug-in, it uses that receipt data to call the Redeem function for the appropriate store (for example, redeemAppleAppStore and redeemGooglePlayStore). The Economy call does all the validation, and rewards the player with the currency balances or the inventory item instance(s) as configured in the Real Money Purchase.

**Examples:**

Example 1 (unknown):
```unknown
redeemAppleAppStore
```

Example 2 (unknown):
```unknown
redeemGooglePlayStore
```

---

## Process join responses and events

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/quickstart-guide/core-process-join-responses-events

**Contents:**
- Process join responses and events#
- Audio and text event responses#
- Participant updated events#

When joining a channel there are two types of events your game should be configured to listen to. The first set of events will give you the status of the audio and text channels. The second set of events will provide information for UI events to manage a roster of “in-channel” users.

Use media to respond to audio events and use text to respond to text channel events. Craft responses to both if both text and audio are being used.

Participant updated events allow you to manage a list of in-channel users for use in your game UI or elsewhere in your application.

When tracking participant state, it is important to use the encoded_uri_with_tag field as the unique identifier of the participant. This allows the game to distinguish between users who have left a channel and users who have rejoined a channel.

Note: Use the is_current_user field to distinguish between events that pertain to the local user from events that pertain to other users.

Next step, check for errors and verify your implementation.

**Examples:**

Example 1 (unknown):
```unknown
void 
HandleMediaStreamUpdatedEvent(vx_evt_media_stream_updated *evt)
{
 if (evt->state == session_media_connected)
 {
   printf("Audio Connected to %s\n", evt->session_handle);
```

Example 2 (unknown):
```unknown
void 
HandleMediaStreamUpdatedEvent(vx_evt_media_stream_updated *evt)
{
 if (evt->state == session_media_connected)
 {
   printf("Audio Connected to %s\n", evt->session_handle);
```

Example 3 (unknown):
```unknown
void 
HandleTextStreamUpdatedEvent(vx_evt_text_stream_updated *evt)
{
 if (evt->state == session_text_connected)
 {
   printf("Text Connected to %s\n", evt->session_handle);
```

Example 4 (unknown):
```unknown
void 
HandleTextStreamUpdatedEvent(vx_evt_text_stream_updated *evt)
{
 if (evt->state == session_text_connected)
 {
   printf("Text Connected to %s\n", evt->session_handle);
```

---

## Submit a login request

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/quickstart-guide/core-submit-login-token

**Contents:**
- Submit a login request#

Vivox provides structures to store a common set of parameters for different request types. Use these structures to craft your request and issue it to the Vivox SDK to be sent to Vivox services.

Create your login request in your game client and store any necessary parameters including the access_token received from your game server.

Issue the request to the Vivox SDK from the game client.

Note: If your implementation is not using the Vivox Buddies system, set req -> enable_presence_persistence(0).

Next step, process login responses and events.

**Examples:**

Example 1 (unknown):
```unknown
access_token
```

Example 2 (unknown):
```unknown
vx_req_account_anonymous_login_t *req;
vx_req_account+anonymous_login_create(&req);
req -> account_name = vx_strdup(“.myissuer.myid.”);
req -> account_handle = vx_strdup(“.myid.”);
req -> connector_handle = vx_strdup(“c1”);
req->access_token = vx_strdup(<token from service>);
```

Example 3 (unknown):
```unknown
vx_req_account_anonymous_login_t *req;
vx_req_account+anonymous_login_create(&req);
req -> account_name = vx_strdup(“.myissuer.myid.”);
req -> account_handle = vx_strdup(“.myid.”);
req -> connector_handle = vx_strdup(“c1”);
req->access_token = vx_strdup(<token from service>);
```

Example 4 (unknown):
```unknown
vx_issue_request3(&req ->base, &request_count);
```

---

## Character encoding

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/client-sdk-basics/character-encoding

**Contents:**
- Character encoding#

The Vivox SDK supports UTF-8 encoding for group text communication services and for buddy/presence services.

Note: UTF-8 is backward compatible with printable standard ASCII. If your application uses only printable standard ASCII for text and presence, then no changes are needed.

Arrays that contain text with UTF-8 encoding must have their length specified by the number of bytes, and not by the number of display characters.

For example, the following 9 characters will be sent as a message. They are represented in UTF-8 by an array of 27 bytes. When sending the message, specifying 9 rather than 27 bytes can cause truncation of the message.

**Examples:**

Example 1 (unknown):
```unknown
私の車は青色です。
{
	0xE7,  0xA7,  0x81,
	0xE3,  0x81,  0xAE,
	0xE8,  0xBB,  0x8A,
	0xE3,  0x81,  0xAF,
	0xE9,  0x9D,  0x92,
	0xE8,  0x89,  0xB2,
	0xE3,  0x81,  0xA7,
	0xE3,  0x81,  0x99,
	0xE3,  0x80,  0x82
}
```

Example 2 (unknown):
```unknown
私の車は青色です。
{
	0xE7,  0xA7,  0x81,
	0xE3,  0x81,  0xAE,
	0xE8,  0xBB,  0x8A,
	0xE3,  0x81,  0xAF,
	0xE9,  0x9D,  0x92,
	0xE8,  0x89,  0xB2,
	0xE3,  0x81,  0xA7,
	0xE3,  0x81,  0x99,
	0xE3,  0x80,  0x82
}
```

---

## Request, response, and event mapping to Vivox functionality

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/client-sdk-basics/requests-responses-events

**Contents:**
- Request, response, and event mapping to Vivox functionality#
- Setup and life cycle management#
- Channel-based communications#
- Friends and presence#
- Settings panel#
- User block list management#
- Events#

The following tables map individual request and response pairs and events to high-level Vivox functionality.

req_connector_initiate_shutdown

req_account_anonymous_login

req_aux_notify_application_state_change

req_sessiongroup_terminate

req_sessiongroup_add_session

req_sessiongroup_remove_session

req_sessiongroup_set_focus

req_sessiongroup_unset_focus

req_sessiongroup_reset_focus

req_sessiongroup_set_tx_session

req_sessiongroup_set_tx_all_sessions

req_sessiongroup_set_tx_no_session

req_session_media_connect

req_session_media_disconnect

req_session_text_connect

req_session_text_disconnect

req_session_mute_local_speaker

req_session_set_local_speaker_volume

req_session_set_participant_volume_for_me

req_session_set_participant_mute_for_me

req_session_set_3d_position

req_channel_mute_user

req_channel_kick_user

req_channel_mute_all_users

req_connector_mute_local_mic

req_connector_mute_local_speaker

req_sessiongroup_control_audio_injection

req_sessiongroup_get_stats

req_account_send_message

req_account_buddy_set

req_account_buddy_delete

req_account_list_buddies_and_groups

req_session_send_message

req_account_set_presence

req_account_send_subscription_reply

req_session_send_notification

req_account_create_block_rule

req_account_delete_block_rule

req_account_list_block_rules

req_account_create_auto_accept_rule

req_account_delete_auto_accept_rule

req_account_list_auto_accept_rules

req_aux_get_render_devices

req_aux_get_capture_devices

req_aux_set_render_device

req_aux_set_capture_device

req_aux_get_mic_level

req_aux_get_speaker_level

req_aux_set_mic_level

req_aux_set_speaker_level

req_aux_render_audio_start

req_aux_render_audio_stop

req_aux_capture_audio_start

req_aux_capture_audio_stop

req_sessiongroup_set_session_3d_position

req_aux_start_buffer_capture

req_aux_play_audio_buffer

req_aux_set_vad_properties

req_aux_get_vad_properties

req_account_control_communications

evt_account_login_state_change

evt_session_notification

evt_aux_audio_properties

evt_media_stream_updated

evt_text_stream_updated

evt_sessiongroup_added

evt_sessiongroup_removed

evt_participant_added

evt_participant_removed

evt_participant_updated

evt_audio_device_hot_swap

---

## Sign out of a game

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/vivox-unreal-sdk-basics/sign-out-of-game

**Contents:**
- Sign out of a game#

When the game no longer wants a user signed in, it sends an ILoginSession::Logout method call to the Vivox SDK. After the user is signed out, there is no network trafﬁc sent to or received by the Vivox SDK.

Note: This is typically called when quitting an application, or in the scenario where the user can sign in to different accounts within an app, when disconnecting from the game server.

To determine if a sign out is user initiated you must listen for the EventStateChanged event with LoginState::LogginOut. The LoggingOut state is always raised on a successful Logout() call before reaching LoginState:LoggedOut. If a user moves directly between the sates of LoggedIn to LoggedOut without the LoggingOut state being raised it means the sign out was not user initiated.

Note: Trying to re-sign in a user too quickly after a sign out can fail if the LoggedOut event has not completed. It's best practice to wait for the LoggedOut event before calling BeginLogin() on a user.

The following code displays an example of the sign out process:

**Examples:**

Example 1 (unknown):
```unknown
/* . . . */
MyLoginSession.Logout();
/* . . . */
```

Example 2 (unknown):
```unknown
/* . . . */
MyLoginSession.Logout();
/* . . . */
```

---

## Introduction to the Moderation Platform

**URL:** https://docs.unity.com/ugs/en-us/manual/moderation/manual/overview

**Contents:**
- Introduction to the Moderation Platform#
- Moderation products#
  - Safe Text#

The Moderation Platform makes toxicity management accessible, impactful, and insightful by providing you with the tools you need to grow and maintain healthy communities within your games.

The Moderation Platform is the way you take action on incidents, review evidence that come from Safe Text, and monitor community health scores. You gain access to this dashboard when the product is included in your project.

The dashboard allows you to:

Gather and review incidents from your game through Safe Text with the Moderation SDK.

Review evidence that is automatically collected from Safe Text when user reports are submitted.

Search and filter incidents in the Moderation queue to prioritize the most toxic reports.

Action incidents manually or through automated processes with the Moderation API. Gain confidence in your review by referencing AI analysis and detections from Safe Text.

Assign moderation roles to project members to action player reports and monitor community health.

Important: Moderation requires the use of Unity's Authentication Service (UAS).

Safe Text is a suite of safety tools you can use to tailor in-game communications rules, filter harmful messages, and collect evidence on toxic players.

Learn more about Safe Text in the Safe Text documentation.

---

## Logging in

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/quickstart-guide/core-logging-in

**Contents:**
- Logging in#

One of the most important steps in utilizing Vivox is to log a user into the Vivox service. Before you begin, decide on when in the game flow the user will be logged in

The following image shows the login workflow:

Before you can log a user into the Vivox service you need to:

---

## Handle join errors

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/quickstart-guide/core-handle-join-errors

**Contents:**
- Handle join errors#
- Verify your join implementation#

To handle join channel errors, expand on the method created to check for a successful join from the Handling login errors.

Implement your channel name crafting code.

Request a token from your token vendor service.

Craft your issue request with the necessary parameters.

Confirm with vx_resp_sessiongroup_add_session that your request is valid.

Confirm that the Vivox service responds with a successful join event by listening to:

Process the event arguments to confirm success or failure and configure your game client to respond accordingly.

With your join process verified you can now have users log in to your application, join a voice channel and communicate with other players.

To continue to build out your communication system, refer to the documentation and guides in the Vivox Core Developer Guide.

**Examples:**

Example 1 (unknown):
```unknown
//success method above 
 else if(evt->state == session_[media|text]_disconnected)
 {
   if (evt->status_code == VxErrorSuccess)
   {
     printf("Audio/Text Disconnected from %s\n", evt->session_handle);
     //evaluate the error, if the game client can adjust/respond, adjust and retry
     //otherwise capture via log for later analysis
   }
   else
   {
     printf("Audio/Text Disconnected from %s, error %d:%s\n", evt->session_handle,evt->status_code, vx_get_error_string(evt->status_code));
     //evaluate the error, if the game client can adjust/respond, adjust and retry
     //otherwise capture via log for later analysis
   }
 }
```

Example 2 (unknown):
```unknown
//success method above 
 else if(evt->state == session_[media|text]_disconnected)
 {
   if (evt->status_code == VxErrorSuccess)
   {
     printf("Audio/Text Disconnected from %s\n", evt->session_handle);
     //evaluate the error, if the game client can adjust/respond, adjust and retry
     //otherwise capture via log for later analysis
   }
   else
   {
     printf("Audio/Text Disconnected from %s, error %d:%s\n", evt->session_handle,evt->status_code, vx_get_error_string(evt->status_code));
     //evaluate the error, if the game client can adjust/respond, adjust and retry
     //otherwise capture via log for later analysis
   }
 }
```

Example 3 (unknown):
```unknown
vx_resp_sessiongroup_add_session
```

Example 4 (unknown):
```unknown
vx_evt_media_stream_updated
```

---

## Introduction to dynamic workspaces (Windows)

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/dynamic-workspaces

**Contents:**
- Introduction to dynamic workspaces (Windows)#
- Change detection#
- Download prediction#
- Known issues#
  - Stop PlasticFS before you update UVCS#
  - Unity Accelerator#
  - Feature limitations#
  - Slower performance#
- Additonal resources#

Note: Dynamic workspaces are currently only available on Windows operating systems.

Dynamic workspaces rely on a virtual filesystem so that Unity Version Control (UVCS) can display your entire directory tree, but only download the files that you require. This means that if you have a large workspace, you can still access the workspace quickly because you don’t need to download every file:

Refer to more information on how to enable dynamic workspaces.

If you move a file in your dynamic workspace, UVCS is involved because it controls the underlying virtual filesystem. This means that UVCS can detect any moved files or directories precisely.

Dynamic workspaces use data from all active users in your repository to generate a predictive usage model. For example, if you read a file foo.c, and it’s likely that you’ll read 100 other files after, UVCS starts to download them in parallel so that one by one fetching of files doesn’t slow down the process.

The following issues might affect how you use dynamic workspaces:

PlasticFS is the executable that enables dynamic workspaces. The UVCS update process can't automatically stop plasticfs.exe, so before you update your UVCS installation, you need to manually stop PlasticFS. Right-click the PlasticFS icon in the system tray and select Exit.

Dynamic workspaces perform better with Unity projects that use Accelerator than with standard workspaces. If your Unity project doesn't use Accelerator, dynamic workspaces will only work on Unity versions 2021.2a and higher.

The performance of the dynamic workspaces is much slower when the Windows Real-Time Protection is enabled. For faster performance, you can add exclusion rules for the plasticfs.exe process and the for paths where your dynamic workspaces are located.

**Examples:**

Example 1 (unknown):
```unknown
plasticfs.exe
```

Example 2 (unknown):
```unknown
DefineDosDeviceW
```

Example 3 (unknown):
```unknown
GetFinalPathNameByHandleW
```

Example 4 (unknown):
```unknown
plasticfs.exe
```

---

## Introduction to Unity DevOps

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/unity-devops-home

**Contents:**
- Introduction to Unity DevOps#
  - Get Started#
- Overview of components#
  - Unity Version Control#
  - Unity Build Automation#
- Help and support#
    - Unity community forum#
    - DevOps Discord#
    - Build automation knowledge base#
    - Support tickets#

Unity DevOps is a group of tools and workflows that can help you develop games efficiently and keep your projects secure.

Unity’s DevOps components are designed to support your team as you scale, collaborate and create better together.

With Unity DevOps, you can:

DevOps consists of a pay-as-you-go consumption model and a free plan.

Unity Version Control (UVCS) is a version control and source code management tool for game and real-time 3D development that you can use to improve team collaboration and scalability with any engine. UVCS offers optimized workflows for artists and programmers, and efficiency for working with large files and binaries.

The UVCS web experience has new and deeply integrated role-based workflows, code reviews, and user management. These capabilities can connect teams and increase productivity for real-time content creators.

You can use UVCS directly in the Unity Editor, and you can use UVCS with plugins to connect with other applications.

Unity Build Automation is a continuous integration tool that automatically creates multiplatform builds in the Cloud. You can point Build Automation toward your version control system to do the following:

Build automation supports multiple version control systems, including Unity Version Control, and can build for multiple platforms simultaneously, including iOS.

There are multiple options available for you to access support for your DevOps services.

In the Unity community forum, you can post your questions and get help from other users. For common issues, you can search for threads to find if someone else has asked the same question.

You can talk with other Unity DevOps users in the DevOps Discord channel.

You can explore and search the Build Automation knowledge base to find relevant questions and answers.

To submit a ticket from the Unity Dashboard, open DevOps and select Help & Support > Ticket > File a ticket.

You can contact support at devops-vcs-support@unity3d.com for all version control questions.

The following resources are available to help you learn more about Unity DevOps and its components.

---

## Sign in with Custom IDs

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/vivox-unity-sdk-basics/sign-in-with-custom-id

**Contents:**
- Sign in with Custom IDs#
- Client-side setup#
- Server-side setup#

If you already have an existing player management system that creates player IDs, you can use the Custom ID provider to link the identities.

Important: Only use this flow if the Authentication package setup doesn't work for your use case and you want to link players to your custom player ID.

The overall flow of this setup:

Together with the server side sign in set up, an access token has to be retrieved from your back end and processed with the AuthenticationService:

ProcessAuthenticationTokens is used to process Unity Authentication Service Access Tokens and make them available to other UGS SDKs integrated into the game that require the player to be authenticated. Since the access tokens expire you must manually refresh the access token and call ProcessAuthenticationTokens with the new access token. You can call ProcessAuthenticationTokens with both the access token and the session token. The Unity Authentication SDK refreshes the access token before it expires, keeps the player's session active, and enables sign in a cached player.

Note that the sample code is a method only, not a class.

The access token needs to be retrieved from your backend. Refer to the Server-side setup below for more information on that.

During player sign-in, ensure that each player is registered with the UAS custom ID provider and retrieve a accessToken and sessionToken for the player. If you haven't done it yet, refer to Enable the UAS custom ID provider to set it up. Using the Service Account credentials you can send a request to register a player on your game server:

The response of this request will contain an idToken and a sessionToken that you have to return to the client for initializing the AuthenticationService. The idToken can be used as the accessToken in this context.

Optionally, you can use the Player Names API to set a name for the players. This is useful if you are using the Vivox Safety products as it will allow the player names to be displayed to moderators in the Moderation dashboard. Make sure you keep the player name up to date if players change it. You can also directly manage player names from the client side using Player Name Management.

**Examples:**

Example 1 (unknown):
```unknown
accessToken
```

Example 2 (unknown):
```unknown
sessionTokens
```

Example 3 (unknown):
```unknown
accessToken
```

Example 4 (unknown):
```unknown
sessionToken
```

---

## Initializing Vivox

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/quickstart-guide/initializing-vivox

**Contents:**
- Initializing Vivox#
- 1. Set up your message handler#
- 2. Call initialize#
- 3. Create a connector handle#
- Verify your implementation#
  - If connector create is unsuccessful#

Use this guide to initialize Vivox and test your integration. The game client must initialize with the Vivox SDK before interacting with it.

Goal: Create a function to process Vivox messages as they appear.

Set up Vivox to handle responses and events via a common callback. Set this callback function via the pf_sdk_message_callback member of the vx_sdk_config_t parameter passed into vx_initialize3(). The void *callback_handle, which is also registered with vx_sdk_config_t, will call the bound method. This can be a pointer to a singleton class that tracks the Vivox state.

When the callback occurs, call vx_get_message() (non-blocking) until no more messages are processed. It is unnecessary to handle messages on the thread that generated the notification callback, but you can if you want to.

The example below is an implementation of this process:

For more details, refer to Message callback.

Goal: Initialize the Vivox SDK.

Initialize the Vivox SDK by creating a config variable and passing it to vx_initialize3(). Both of these calls will return a response code defined in VxcErrors.h. A successful response returns VxErrorSuccess.

For more details, refer to Initialize the Vivox SDK.

Goal: Fetch the pre-login server-side configuration file.

Create a request object of type vx_req_connector_create, set a connector_handle value of your choosing and wait for the vx_resp_connector_create response before logging in to the Vivox network.

This request fetches important pre-login information from your assigned Vivox server. This information is required before logging into the Vivox network. If you have one or more dedicated servers, the acct_mgmt_server value might vary by title and region.

Note: It is best practice for the game client to craft the connector_handle and store it for use in subsequent requests.

For more information, refer to Create the connector object.

Follow these steps to verify your implementation:

When your game client receives an event from the Vivox SDK, your first step is to evaluate if it was successful. For the initialization process, check for success with the connector_create call. If successful, the status_code field will be 0. If it is not zero, your game client should follow the below steps to initiate a retry.

For more information on Vivox logs, refer to Vivox Client SDK logs.

Once you have verified your implementation you can move on to logging in users.

**Examples:**

Example 1 (unknown):
```unknown
pf_sdk_message_callback
```

Example 2 (unknown):
```unknown
vx_sdk_config_t
```

Example 3 (unknown):
```unknown
vx_initialize3()
```

Example 4 (unknown):
```unknown
vx_sdk_config_t
```

---

## Unity Relay

**URL:** https://docs.unity.com/ugs/en-us/manual/relay/manual/introduction

**Contents:**
- Unity Relay#

Unity Relay exposes a way for game developers to securely offer increased connectivity between players by using a join code style workflow without needing to invest in a third-party solution, maintain dedicated game servers (DGS), or worry about the network complexities of a peer-to-peer game. Instead of using DGS, the Relay service provides connectivity through a universal Relay server acting as a proxy.

The Relay service has two key components: the Relay servers and the Relay Allocations service.

Tip: Checkout How to setup Global Matchmaking for Unity by Tarodev.

---

## Load the VivoxCore module

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/vivox-unreal-sdk-basics/load-vivoxcore-module

**Contents:**
- Load the VivoxCore module#

In Unreal Engine 4, games are made up of one or more gameplay modules (DLLs) that serve as containers for a collection of related classes. Before the game can perform actions with the Vivox SDK, it must first load the VivoxCore module, which causes a number of shared libraries to be loaded. Because this load process can take up to hundreds of milliseconds (depending on the machine), we recommend that you call this function on application startup.

The following example displays the required code for loading the module that contains the Vivox Unreal SDK:

The VivoxCore module needs to be loaded only once after application start up. Maintain a handle to the loaded module object for later use. We recommend that you perform this step in a custom UGameInstance class, because these classes typically match the lifetime of the application.

For example, in your UGameInstance header, define a private member variable FVivoxCoreModule *MyVoiceModule;, and then to load the VivoxCore module, call this code snippet in the UGameInstance cpp in either the constructor or an overridden Init() method.

**Examples:**

Example 1 (cpp):
```cpp
// Typically included in header for type definitions in member variables
#include "VivoxCore.h"
/* . . . */
// FVivoxCoreModule *MyVoiceModule in header.
MyVoiceModule = static_cast<FVivoxCoreModule *>(&FModuleManager::Get().LoadModuleChecked(TEXT("VivoxCore")));
// The VivoxCore Module is now loaded.
/* . . . */
```

Example 2 (cpp):
```cpp
// Typically included in header for type definitions in member variables
#include "VivoxCore.h"
/* . . . */
// FVivoxCoreModule *MyVoiceModule in header.
MyVoiceModule = static_cast<FVivoxCoreModule *>(&FModuleManager::Get().LoadModuleChecked(TEXT("VivoxCore")));
// The VivoxCore Module is now loaded.
/* . . . */
```

---

## Vivox Core Unreal SDK basics

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/vivox-unreal-sdk-basics/vivox-unreal-sdk-basics-toc

**Contents:**
- Vivox Core Unreal SDK basics#

The following pages will walk you through getting started with the Vivox Unreal SDK.

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/overview/manual/ugs-cli-introduction

---

## Initialize the voice client object

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/vivox-unity-sdk-basics/initialize-voice-client-object

**Contents:**
- Initialize the voice client object#

You must create and initialize the voice client before other Vivox SDK actions can take place. During the voice client initialization process, the Vivox SDK sets up and starts all of its different subsystems. This can cause additional shared libraries to be loaded, and allows developers to customize a variety of Vivox SDK options before connecting to Vivox services. For example, a developer can select between audio ducking settings and logging level.

await VivoxService.Instance.InitializeAsync(VivoxConfigurationOptions config) has optional parameters that you can use to choose the configuration of the Vivox Core SDK.

The following code displays an example of how to create and initialize the voice client on Windows, macOS, Android, and iOS:

Important: If the client is already initialized and the application attempts to initialize the client again, it encounters a 5041:VxErrorAlreadyInitialized error. For information on this error refer to the Vivox SDK error codes.

For more details on the Unity Authentication Service, refer to Sign-in with the Authenticaiton package. For alternative authentication approaches, refer to Sign-in with Custom IDs, or if you are already using Vivox Access Tokens in your application, refer to Using VATs with UAS.

**Examples:**

Example 1 (unknown):
```unknown
await VivoxService.Instance.InitializeAsync(VivoxConfigurationOptions config)
```

Example 2 (unknown):
```unknown
using System;
using UnityEngine;
using Unity.Services.Authentication;
using Unity.Services.Core;
using Unity.Services.Vivox;

async void Start()
{
    await UnityServices.InitializeAsync();
    await AuthenticationService.Instance.SignInAnonymouslyAsync();

    await VivoxService.Instance.InitializeAsync();
}
```

Example 3 (unknown):
```unknown
using System;
using UnityEngine;
using Unity.Services.Authentication;
using Unity.Services.Core;
using Unity.Services.Vivox;

async void Start()
{
    await UnityServices.InitializeAsync();
    await AuthenticationService.Instance.SignInAnonymouslyAsync();

    await VivoxService.Instance.InitializeAsync();
}
```

Example 4 (unknown):
```unknown
5041:VxErrorAlreadyInitialized
```

---

## Sign in to a game

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/client-sdk-basics/sign-in-to-game

**Contents:**
- Sign in to a game#

After you initialize the Vivox SDK, you can sign a user in to the Vivox instance assigned to your game by using an account name that is selected by the game. For the account name:

Note: It’s recommended that you re-use player account names to facilitate the use of Vivox features such as cross-mute and blocking. It will also become a requirement for using future safety features designed for reducing toxicity and fostering more positive communities.

The following criteria applies to account names:

0x30-0x39, 0x41-0x5A, 0x61-0x7A

Note: You can only use the percent sign for URL encoding. Follow the percent sign by two uppercase hex characters.

Note: The account name and channel names must include a reference to the token issuer.

The game uses the vx_req_account_anonymous_login message to sign users in to Vivox. After the game receives a successful vx_resp_account_anonymous_login message, the user is signed in.

Note: Every sign in request must have an access token that authorizes that game instance to sign in as the specified account.

For more information, refer to the Access Token Developer Guide.

If you do not want to use a unique account name per player, or if you do not want to expose the username to the network, then use one of the following options:

The game sets a display name for the account by using UTF-8 encoding. This name is visible to all users in the channel; they receive it as the displayname field in the vx_evt_buddy_presence and vx_evt_participant_added events.

Note: The Vivox SDK does not perform checks on the display name. It is the game developer's responsibility to ensure that the display name follows the rules of the game, that the font characters are supported by the in-game renderer, and that the name is checked for duplication, obscenity, and impersonation. We recommend that you check display names by using some out-of-band means (such as the game server), and not by using the game client.

The following code is an example of how to initiate the sign in process:

The following code is an example of how to handle the sign in response:

In addition to handling the vx_resp_account_anonymous_login message, the game must also handle vx_evt_account_login_state_change events that have the state field set to login_state_logged_out. The Vivox SDK sends this message when the connection to Vivox is lost unexpectedly (such as from network connectivity issues).

The following code is an example of the connection state change messaging process:

After the game receives the vx_resp_connector_create message, the game can sign in to Vivox. For more information, refer to Create the connector object.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_account_anonymous_login
```

Example 2 (unknown):
```unknown
vx_resp_account_anonymous_login
```

Example 3 (unknown):
```unknown
vx_evt_buddy_presence
```

Example 4 (unknown):
```unknown
vx_evt_participant_added
```

---

## Requests and responses

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/client-sdk-basics/requests-responses

**Contents:**
- Requests and responses#

The Vivox SDK supports various requests that you can use to control its behavior. For each request issued to the Vivox SDK, there is a response that the game processes as a part of its response handler.

Requests are created by using functions that are closely named to the type of request. For example, the vx_req_account_anonymous_login request is created by calling the vx_req_account_anonymous_login_create() function. The request is then sent to the Vivox SDK by calling vx_issue_request3().

The following code displays an example of this process:

Note: Use vx_issue_request3() to get a count of the current request queue depth whenever a request is issued, and to monitor the queue depth in your application.

When vx_issue_request3() is called, the Vivox SDK posts a vx_resp_base_t message back to the game. Each vx_resp_base_t message that the game processes could be the response to one of multiple different requests. The game determines the true type of the vx_resp_base_t message by looking at the vx_resp_base_t.type field, and then casts that message to the corresponding specific response structure.

The convention for mapping vx_response_type values to response types is to use the following format: if the vx_response_type value is esp_xxx, then the corresponding structure type is vx_resp_xxx_t.

The following code displays an example of this process:

When the Vivox SDK returns a response, configure the application to check the return_code and the status_code fields.

The following code displays an example of this process:

For a mapping of all possible messages to common game functionality, refer to Request, response, and event mapping to Vivox functionality.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_account_anonymous_login
```

Example 2 (unknown):
```unknown
vx_req_account_anonymous_login_create()
```

Example 3 (unknown):
```unknown
vx_issue_request3()
```

Example 4 (unknown):
```unknown
. . .
vx_req_account_anonymous_login_t *req;
vx_req_account_anonymous_login_create(&req);
req->account_name = vx_strdup(".myissuer.myid.");
req->account_handle = vx_strdup(".myid.");
req->connector_handle = vx_strdup("c1");
vx_issue_request3(&req->base, &request_count);
. . .
```

---

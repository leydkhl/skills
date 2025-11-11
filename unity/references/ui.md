# Unity - Ui

**Pages:** 539

---

## Implement Vivox Unity

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/implement-vivox-unity/implement-vivox-unity

**Contents:**
- Implement Vivox Unity#

To implement the Vivox Unity SDK, you need the following items, which you can find in the Unity Developer Portal:

The URL for your Vivox API endpoint

The host for your Vivox domain

Your Vivox access token issuer and key pair.

You use this information to sign Vivox access tokens on your server and distribute those tokens to your game clients.

Note: You can initially sign access tokens with your game client, but this is not a recommended production practice.

For more information, refer to the Access Token Developer Guide.

A VivoxUnity package for each of the platforms that will be supported by your game.

After you familiarize yourself with the items in this list, install Vivox Unity.

---

## General use of the Audio Tap components

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/audio-taps/use-audio-taps

**Contents:**
- General use of the Audio Tap components#
- Audio Taps in channels#
- Vivox v16.2.0+#

The Audio Tap components are available in the Vivox Unity package. With the package installed, you can add the taps through both the GameObject and Component Addition menus under Audio.

Note: You cannot create a Participant Tap for yourself, it must be created for another user.

Adding a tap adds an Audio Source component to the associated GameObject. You can modify the audio provided by the taps directly through GameObject components, such as the components supplied by Unity under Audio, or by routing it through an Audio Mixer.

As shown above, in the Vivox Channel Audio Tap component screenshot, every Audio Tap has a visible loudness meter in its Inspector window to help monitor the audio stream activity during Play Mode.

Audio Mixers can also modify tap audio. After creating the Audio Mixer, in the Audio Source’s Output field, select the required Mixer Group to route the audio through the mixer. Note that all the components added to the Audio Source GameObject are applied before sending the audio to the Audio Mixer Group.

Audio Taps will only work when connected to a channel.

Taps will work in both positional (3D) and non-positional (2D) channels. In positional channels the participants must be within the audio range to hear each other.

Note: The Channel Audio Tap will only be applied to the first channel a user joins.

As of the 16.2.0 version of the Vivox package you can have more than one Participant Audio Tap applied to the same user across channels. However, if the Silence In Channel Audio Mix parameter is set to True at some point in the chain of taps, all following taps will be muted.

---

## Container builds

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/concepts/container-builds

**Contents:**
- Container builds#
- Container build requirements#
  - Create a container built with the base image#
  - Create a container build from scratch#
  - Create a container build using server builds uploaded to Steam#
- Pulling a container build#
- Server.json file#

A container is a way of packing software and all its dependencies together in a single unit. With Multiplay Hosting container builds, you can package your game servers and their dependencies, build a container, and push it to the Multiplay Hosting container registry.

Warning: Container builds require technical knowledge around Docker and Dockerfiles (Docker Docs).

You can create a container build through the Unity Dashboard by selecting Container image when you create a build. Refer to Create a build with a container image.

Multiplay Hosting uses Docker container tags in the format <ImageName>:<ImageTag>. Multiplay Hosting generates the correct name and tag for your build, and displays it to you in the Unity Dashboard when you create or update a container build.

Note: Multiplay Hosting only generates the ImageName and ImageTag for you when you create a build through the Unity Dashboard. If you use the CLI tool, you must create the ImageName and ImageTag.

When you create a Multiplay Hosting build, you have several options for uploading your build file. One option is to use Docker and the Multiplay Hosting container registry to upload a containerized version of your build. However, your build container must meet specific requirements (in addition to the general integration requirements) before you can use a containerized build.

The linux-build-image template container takes care of most of the additional requirements of using a container build. You can use the container by adding FROM unitymultiplay/linux-base-image:<tag> to the top of your Dockerfile, as the following code demonstrates. For more information, refer to Dockerfile reference (Docker documentation).

In the preceding example:

Warning: Launch parameters specified in the build configuration can overwrite CMD defined in the Dockerfile.

For a simple example, refer to this Simple Game Server Dockerfile (Unity Multiplay examples on GitHub).

If you prefer to create your Dockerfile from scratch, it’s your responsibility to ensure your container meets the following requirements.

Warning: Use the base image unless you have a specific or advanced uses in which you must create the container from scratch.

The following code block is a simple Dockerfile that meets these requirements:

To upload a container build for use with game server builds uploaded to Steam, it is possible to use SteamCMD to build your container image.

The following code block is an example Dockerfile for the SteamCMD process:

For security reasons, pulling content from the registry is not permitted. If you require an offline copy of your build, make sure to also push the image to a registry you control for later use.

If you use the base container image, the server.json file is already mounted into your container. You can find the server.json file location by checking the container's home environment variable.

If you create a container build from scratch, create your own server.json file and include it in the container image. Refer to Server.json for the format of this file, and Configuration variables for an explanation of server.json's variables.

**Examples:**

Example 1 (unknown):
```unknown
<ImageName>:<ImageTag>
```

Example 2 (unknown):
```unknown
docker tag <ImageName>:<ImageTag> registry.multiplay.com/4818982f-bb28-4c39-9d44-628204b6780e/1ae4de65-849d-4ff1-86fc-1de3bdfbd891/12788:v1
```

Example 3 (unknown):
```unknown
docker tag <ImageName>:<ImageTag> registry.multiplay.com/4818982f-bb28-4c39-9d44-628204b6780e/1ae4de65-849d-4ff1-86fc-1de3bdfbd891/12788:v1
```

Example 4 (unknown):
```unknown
FROM unitymultiplay/linux-base-image:<tag>
```

---

## Set Chat Markers

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/text-chat-guide/set-chat-markers

**Contents:**
- Set Chat Markers#

The Chat Markers API gives you control over the creation of Chat Markers. You can use Chat Markers to mark messages as read. It also provides data for the Conversation list API. This functionality works in both channels and direct messages.

To set a conversation's read marker, sign in to the application and use VivoxService.Instance.SetMessageAsReadAsync(). A VivoxMessage input is used by the method to determine which type of conversation it belongs to and the MessageId of the message to set as read. This method sets a read checkpoint for the relevant conversation at the MessageId of the VivoxMessage provided.

Note: The marker is created even if the URI or message doesn’t exist. In the future, this information will be used to augment the Conversations API.

The following code is an example of how to set a Chat Marker.

A VivoxMessage with its IsRead property set to true is returned upon completion. Note that DateTime is server side time and not client side time.

Since a VivoxMessage instance canont be created manually, the input for VivoxService.Instance.SetMessageAsReadAsync() needs to come from one of the events or methods in the SDK that provides a VivoxMessage to the client. These are some examples of events and methods that provide VivoxMessage instances that can be used as input for VivoxService.Instance.SetMessageAsReadAsync():

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.SetMessageAsReadAsync()
```

Example 2 (unknown):
```unknown
VivoxMessage
```

Example 3 (unknown):
```unknown
VivoxMessage
```

Example 4 (unknown):
```unknown
DateTime seenAt = DateTime.Now; // Optional timestamp for when the message was seen. Default is DateTime.Now converted to UTC.
var readMessage = await VivoxService.Instance.SetMessageAsReadAsync(message, seenAt);
```

---

## Unity Authentication Service in Unreal

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/uas-unreal

**Contents:**
- Unity Authentication Service in Unreal#
- Requirements#
- Integration steps#
- Authenticate with the Unity Authentication Service#

You can use the Unity Authentication Service (UAS) in your Unreal Vivox application.

Note: Find your Project ID and Environment on the Unity Dashboard, by selecting the Projects tab and then the project you’re working on.

Use the following instructions to set up the Unity Authentication Subsystem to sign-in to Vivox:

Add Authentication to the PublicDependencyModuleName in your project’s build.cs file

Get the UGameInstance from the GameWorld and assign the AuthenticationSubsystem to a variable from the GameInstance.

Set up an AuthenticationResponse UFunction, named OnAuthentication in the following example, to track when the Authentication has finished successfully. Then, pass it into SignInAnonymously, along with any FAuthenticationSignInOptions you need.

After Authentication has successfully authenticated to the UGS Service, replace the LoginToken in any LoginSession.BeginLogin call with the AuthenticationSubsystem->GetAccessToken(). Below is an example:

Note: When using Unity Authentication Access Tokens, the local AccountID’s Account Name should be the same as the Authentication PlayerID, as shown here:

You can also use Authentication Tokens in place of ChannelSession connect tokens, as shown in the following example:

**Examples:**

Example 1 (unknown):
```unknown
AuthenticationSubsystem
```

Example 2 (unknown):
```unknown
UWorld* GameWorld = GetWorld();
UGameInstance* GameInstance = GameWorld->GetGameInstance();
UAuthenticationSubsystem* AuthenticationSubsystem = GameInstance->GetSubsystem<UAuthenticationSubsystem>();
```

Example 3 (unknown):
```unknown
UWorld* GameWorld = GetWorld();
UGameInstance* GameInstance = GameWorld->GetGameInstance();
UAuthenticationSubsystem* AuthenticationSubsystem = GameInstance->GetSubsystem<UAuthenticationSubsystem>();
```

Example 4 (unknown):
```unknown
AuthenticationResponse
```

---

## Android app development

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/android/android-toc

**Contents:**
- Android app development#

---

## Muting

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/muting/muting-toc

**Contents:**
- Muting#

The Vivox SDK allows users to mute other users.

---

## Player evidence report HTTP request

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/chat-history/player-evidence-report-request

**Contents:**
- Player evidence report HTTP request#

The following code is an example of a player evidence request:

Note: Add the request_start_ts parameter to the request if using pagination to make subsequent requests.

**Examples:**

Example 1 (unknown):
```unknown
curl https://services.api.unity.com/vivox/text-evidence-management/v1/organizations/<organization_id>/projects/<project_id>/player-evidence-report?user_uri=sip:your-issuer.john@yourvivoxdomainname.vivox.com&start_ts=1234567&end_ts=1234568&num_conversations=1 -u service-acct-username:service-account-password
```

Example 2 (unknown):
```unknown
curl https://services.api.unity.com/vivox/text-evidence-management/v1/organizations/<organization_id>/projects/<project_id>/player-evidence-report?user_uri=sip:your-issuer.john@yourvivoxdomainname.vivox.com&start_ts=1234567&end_ts=1234568&num_conversations=1 -u service-acct-username:service-account-password
```

---

## Upload local builds

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/run-builds/upload-local-builds

**Contents:**
- Upload local builds#
- Upload a local build#

Use Unity Build Automation (UBA) to upload local builds to Unity Cloud storage. This feature enables you to archive your local builds or submit them to app stores through the Unity Dashboard.

There are several reasons you might want to upload your local builds to UBA:

To upload a local build to Unity Build Automation:

Note: If you upload a local build, the build counts toward your organization's storage quota in Unity Cloud. Regular cleanup of unnecessary builds can help manage your storage usage.

---

## Sessions tutorials with Unity Netcode

**URL:** https://docs.unity.com/ugs/en-us/manual/mps-sdk/manual/build-sessions-landing

**Contents:**
- Sessions tutorials with Unity Netcode#
- Additional resources#

Use Netcode to build a simple multiplayer game from scratch using sessions.

Multiplayer Services uses the following netcode solutions to manage real-time networking in sessions:

You can use only one of these networking libraries per project.

The following tutorials demonstrate how to use Multiplayer Services sessions and Unity Netcode to add simple multiplayer elements to a game. Completing these tutorials is the best way to learn the principles of the Multiplayer Services package.

**Examples:**

Example 1 (unknown):
```unknown
NetworkBehaviour
```

---

## ProGuard rules

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/android/proguard

**Contents:**
- ProGuard rules#

Starting in Vivox version 16.6 and above, ProGuard rules are included in the Android AAR library so you no longer need to provide your own rules. The rules will be used automatically by Android build systems.

To review the rules in use, open the Vivox Android AAR library as a ZIP file, then open proguard.txt.

**Examples:**

Example 1 (unknown):
```unknown
proguard.txt
```

---

## Configure Dedicated server Build Target

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/advanced-build-configuration/configure-dedicated-server-build-target

**Contents:**
- Configure Dedicated server Build Target#
- Prerequisites#
- Configure the Dedicated server Build Target#

The Dedicated server is a specialized desktop sub target that removes unnecessary assets and code during the build process to optimizes server build efficiency. This results in reduced build size and improved processing efficiency, which benefits server operators because it lowers operating costs when running Unity server applications.

The Dedicated server build target is available in Unity version 2021.3 and above. Ensure that you set your project to this version or later.

---

## Get started with Friends

**URL:** https://docs.unity.com/ugs/en-us/manual/friends/manual/guides/get-started

**Contents:**
- Get started with Friends#
- Prerequisites#
- Set up Friends#
- Link your project with the Unity Dashboard#
- Enable the Friends service#
- Install the Friends package#
- Initialize the Friends service#

Attention: The Digital Services Act (DSA) requires Unity to notify our customers’ end users if Unity takes an action that impacts those end users under the DSA. To comply with this requirement, if you use Unity Gaming Services (UGS) products that rely on the Unity Authentication Service, you must integrate the notification API.

For more information on DSA, refer to the Digital Services Act - compliance update.

To make your game compliant, refer to DSA notifications.

This guide walks you through how to set up and start using Friends.

To get started with Friends, you need to do the following:

You can set up and manage Friends through the Unity Dashboard:

When you launch Friends for the first time, this adds Friends to the Shortcuts section on the sidebar and opens the Overview page.

You must link your project with the Unity Dashboard to use the Friends service.

Your Unity Project ID now appears in Unity Editor, and the project is linked to Unity’s cloud services. You can also access your Unity Project ID in code using UnityEditor.CloudProjectSettings.projectId.

You can enable the Friends service for your project from the Unity Dashboard.

The Setup Guide walks you through linking your Unity project, installing the Friends package, and enabling the Friends service.

Once you've linked your project to the Unity Dashboard, you can install the latest version of the Friends package.

Use the Unity Package Manager to import the Friends package in the Unity Editor.

Once you’ve enabled the Friends service and imported the Friends SDK through the Package Manager in the Unity Editor, initialize the Core SDK using await UnityServices.InitializeAsync().

The following code sample shows how to initialize the Friends service and any other Unity services in your project.

**Examples:**

Example 1 (unknown):
```unknown
UnityEditor.CloudProjectSettings.projectId
```

Example 2 (unknown):
```unknown
await UnityServices.InitializeAsync()
```

Example 3 (unknown):
```unknown
// Import required packages
using Unity.Services.Core;
using Unity.Services.Authentication;
...
private async Task MyMethod()
{
    // Initialize all of the used services
    await UnityServices.InitializeAsync();

    // Sign in anonymously or using a provider
    await AuthenticationService.Instance.SignInAnonymouslyAsync();

    // Initialize friends service
    await FriendsService.Instance.InitializeAsync();

    // Start using the Friends SDK functionalities.
    var friends = FriendsService.Instance.Friends;
}
```

Example 4 (unknown):
```unknown
// Import required packages
using Unity.Services.Core;
using Unity.Services.Authentication;
...
private async Task MyMethod()
{
    // Initialize all of the used services
    await UnityServices.InitializeAsync();

    // Sign in anonymously or using a provider
    await AuthenticationService.Instance.SignInAnonymouslyAsync();

    // Initialize friends service
    await FriendsService.Instance.InitializeAsync();

    // Start using the Friends SDK functionalities.
    var friends = FriendsService.Instance.Friends;
}
```

---

## Positional channel parameters

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/positional-channel-parameters

**Contents:**
- Positional channel parameters#

Developers can optionally specify parameters for positional (d) channels by using an extended syntax in the channelid portion of the channel URI.

Note: Positional channel URIs with different 3D property parameters are considered different channels.

Immediately following the channelid, and before the "@", add "!p-" followed by hyphen-delimited parameters that indicate 3D channel property values in the following format:

Note: You can represent the max_range and clamping_distance in any units, but their units must match.

The channel parameters are:

{max_range}: The maximum distance from the listener at which you can hear a speaker. This must be an integer > 0.

{clamping_distance}: The distance from the listener at which a speaker’s voice is heard at its original volume. This must be an integer in the range 0 <= clamping_distance <= max_range. Consider this to be the expected close-by conversational distance.

{rolloff}: The strength of the audio fade effect as the speaker moves away from the listener. This must be a floating point value >= 0 that is rounded to three decimal places. The best starting point for each distance_model is a rolloff of 1.0.

{distance_model}: The model that determines voice volume at different distances.

1 = inverse-clamped (Default, most acoustically accurate.)

Note: You must use the same units as those used for the channel parameters when reporting your actor’s position by using the Set3DPosition method.

Depending on the amount of ambient game noise or music, you might want to adjust the parameter values to ensure that voice chat is heard only at the distances you want.

When using the exponent-clamped distance_model, we recommend keeping the clamping_distance value at 1 or greater. We also recommend that you adjust the value of the rolloff parameter based on the following formula: rolloff = 3 ÷ (1 + 1.75 × log10(clamping_distance)).

The following code displays an example string with parameter syntax. You can construct the string manually or by using the method vx_get_positional_channel_uri():

The following channel name is an example of what is passed to the Vivox servers when working with access tokens:

For more information, refer to the Access Token Developer Guide.

**Examples:**

Example 1 (unknown):
```unknown
!p-{max_range}-{clamping_distance}-{rolloff}-{distance_model}
```

Example 2 (unknown):
```unknown
!p-{max_range}-{clamping_distance}-{rolloff}-{distance_model}
```

Example 3 (unknown):
```unknown
clamping_distance
```

Example 4 (unknown):
```unknown
{max_range}
```

---

## Initiate a channel join

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/channels/initiate-channel-join

**Contents:**
- Initiate a channel join#

The following code displays an example of how to initiate a channel join:

Note: The IChannelSession::FOnBeginConnectCompletedDelegate callback does not indicate that the user has successfully joined the channel.

The IChannelSession::EventChannelStateChanged event runs when a player has completely joined the channel, has the intended audio and text states connected, and there is a fully formed list of channel participants.

The following code displays an example of how this event runs:

This event is generated in response to messages from the Vivox network (for example, when a user is forcibly removed from a channel by the game server).

**Examples:**

Example 1 (unknown):
```unknown
/* . . . */
ChannelId Channel(kDefaultIssuer, "example_channel", kDefaultDomain, ChannelType::NonPositional);
IChannelSession &MyChannelSession(MyLoginSession.GetChannelSession(Channel));
bool IsAsynchronousConnectCompleted = false;
IChannelSession::FOnBeginConnectCompletedDelegate OnBeginConnectCompleted;

OnBeginConnectCompleted.BindLambda([this, &IsAsynchronousConnectCompleted, &MyChannelSession](VivoxCoreError Error)
{
    if (VxErrorSuccess == Error)
    {
        IsAsynchronousConnectCompleted = true;
        // This bool is only illustrative. The connect call has completed.
    }
});

MyChannelSession.BeginConnect(true, true, true, MyChannelSession.GetConnectToken(kDefaultKey, kDefaultExpiration), OnBeginConnectCompleted);
/* . . . */
```

Example 2 (unknown):
```unknown
/* . . . */
ChannelId Channel(kDefaultIssuer, "example_channel", kDefaultDomain, ChannelType::NonPositional);
IChannelSession &MyChannelSession(MyLoginSession.GetChannelSession(Channel));
bool IsAsynchronousConnectCompleted = false;
IChannelSession::FOnBeginConnectCompletedDelegate OnBeginConnectCompleted;

OnBeginConnectCompleted.BindLambda([this, &IsAsynchronousConnectCompleted, &MyChannelSession](VivoxCoreError Error)
{
    if (VxErrorSuccess == Error)
    {
        IsAsynchronousConnectCompleted = true;
        // This bool is only illustrative. The connect call has completed.
    }
});

MyChannelSession.BeginConnect(true, true, true, MyChannelSession.GetConnectToken(kDefaultKey, kDefaultExpiration), OnBeginConnectCompleted);
/* . . . */
```

Example 3 (unknown):
```unknown
IChannelConnectionState
```

Example 4 (unknown):
```unknown
IChannelConnectionState
```

---

## Send data through text messages

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/messaging/data-through-text

**Contents:**
- Send data through text messages#
- Core application stanza body#
- Send hidden data in messages#
- Access hidden data from messages#
- SDKSampleApp#

Vivox text messages can send additional, non-messaging data through two string variables:

The application stanza namespace carries Uniform Resource Identifier (URI) reference information that identifies an XML namespace. This URI identifies the kind of information you send in the application stanza body. For more information about how to format the URI, refer to the W3C documentation on XML Namespaces.

The application stanza body carries the data associated with the given application stanza namespace. If the data is text, it must be UTF-8 encoded. If binary, use base64 to encode with a maximum of 8k per message.

Note: This information is hidden from the player and is only accessible through the code.

You can hide messages if you need to keep them hidden from users. For example, you are using them only to send data with no visible message. One method to accomplish this is to label your application stanza namespace accordingly. For example, you can have an application stanza namespace named "data:hidden" or "message:omit". You can then check for all messages with the same application stanza namespace.

Note: Messages must contain data. You cannot send an empty message.

You can access the hidden data after it is received. You can access its application stanza body by grabbing the message from the function that receives it.

The following examples detail how to send hidden data through text messages and access hidden data from messages in the Vivox Core SDK.

These examples mimic the SDKSampleApp::HandleMessage(vx_message_base_t *msg) function. For more information, refer to the SDKSampleApp.

**Examples:**

Example 1 (unknown):
```unknown
data:hidden
```

Example 2 (unknown):
```unknown
message:omit
```

Example 3 (unknown):
```unknown
string applicationStanzaNamespace = "team:color";
string applicationStanzaBody = teamColor; //"red" or "blue"

vx_req_account_send_message_t *req;
vx_req_account_send_message_create(&req);
req->account_handle = vx_strdup(accountHandle.c_str());
req->message_body = vx_strdup(message.c_str());
req->user_uri = vx_strdup(user.c_str());
req->application_stanza_namespace = vx_strdup(applicationStanzaNamespace);
req->application_stanza_body = vx_strdup(applicationStanzaBody);
IssueRequest(&req->base);
```

Example 4 (unknown):
```unknown
string applicationStanzaNamespace = "team:color";
string applicationStanzaBody = teamColor; //"red" or "blue"

vx_req_account_send_message_t *req;
vx_req_account_send_message_create(&req);
req->account_handle = vx_strdup(accountHandle.c_str());
req->message_body = vx_strdup(message.c_str());
req->user_uri = vx_strdup(user.c_str());
req->application_stanza_namespace = vx_strdup(applicationStanzaNamespace);
req->application_stanza_body = vx_strdup(applicationStanzaBody);
IssueRequest(&req->base);
```

---

## Crossmute functions

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/muting/crossmute-functions

**Contents:**
- Crossmute functions#

The following list details the actions that occur when you call various crossmuting functions.

ILoginSession::CrossMutedCommunications() - Returns a TSet of all AccountIds that are crossmuted for the current ILoginSession.

ILoginSession::BeginSetCrossMutedCommunications(AccountId &account, bool &muted, FOnBeginSetCrossMutedCommunicationsDelegate theDelegate) - Sets the crossmuting status of a single AccountId for the ILoginSession.

ILoginSession::BeginSetCrossMutedCommunications(TSet<AccountId> &accountIdSet, bool &muted, FOnBeginSetCrossMutedCommunicationsDelegate theDelegate) - Sets the crossmuting status of a TSet of AccountIds.

ILoginSession::ClearCrossMutedCommuncations(FOnBeginClearCrossMutedCommunicationsCompletedDelegate theDelegate) - Clears any crossmuted communications in the ILoginSession, which allows for the sending or receiving of text and voice communications that are not prevented by some other method.

**Examples:**

Example 1 (unknown):
```unknown
ILoginSession::CrossMutedCommunications()
```

Example 2 (unknown):
```unknown
ILoginSession::BeginSetCrossMutedCommunications(AccountId &account, bool &muted, FOnBeginSetCrossMutedCommunicationsDelegate theDelegate)
```

Example 3 (unknown):
```unknown
ILoginSession
```

Example 4 (unknown):
```unknown
ILoginSession::BeginSetCrossMutedCommunications(TSet<AccountId> &accountIdSet, bool &muted, FOnBeginSetCrossMutedCommunicationsDelegate theDelegate)
```

---

## Crossmute operations

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/muting/cross-mute

**Contents:**
- Crossmute operations#

To crossmute one or more users, use vx_req_account_control_communications. You can call this request before any session is established because it only requires the account handle of the muter.

Crossmuting is performed for all channels. Because vx_req_account_control_communications can support a list of users, you can also crossmute multiple users.

The following table details the possible crossmute operations:

vx_control_communications_operation_block = 0

Remove crossmute or cross-unmute

vx_control_communications_operation_unblock = 1

Get a list of crossmuted users

vx_control_communications_operation_block_list = 2

Remove crossmute from all users

vx_control_communications_operation_clear = 3

vx_control_communications_operation_clear_block_list = 3

**Examples:**

Example 1 (unknown):
```unknown
vx_req_account_control_communications
```

Example 2 (unknown):
```unknown
vx_req_account_control_communications
```

---

## Text-to-speech

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/text-to-speech/tts-overview

**Contents:**
- Text-to-speech#

Text-to-speech (TTS) is a feature in the Vivox SDK that is intended to support developers who are pursuing Communications and Video Accessibility Act (CVAA) compliance.

Note: TTS is only available in US English, and is only supported for one logged in user.

TTS provides a voice channel participant with the ability to convert text messages into synthesized speech that is then transmitted into the voice channel. This allows a user to participate in voice chat even when they might not be able to speak or speak clearly. TTS also provides a user with the ability to convert text chat or game-screen text into synthesized speech for local playback.

You can play a synthesized phrase locally through the user’s output audio device, transmit it and play it back for remote participants, or simultaneously play it locally and transmit it to remote participants.

---

## Project settings

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/implement-vivox-unity/project-settings

**Contents:**
- Project settings#

Depending on your operating system, confirm that the following settings to ensure that VivoxUnity can run in the background.

On Windows and macOS, select Edit > Project Settings, and then enable the Run In Background checkbox.

On iOS, select Player Settings > Other Settings > Configuration, and then in the Behavior in Background menu, select Exit.

Note: The Vivox SDK doesn't handle the automatic suspending and resuming of an application. In almost all cases, a suspended application loses connection to the Vivox servers. For an application to be suspended when running in the background, connections must be manually stored before moving to the suspended state. After resuming the application, the Vivox SDK must be reconnected to the Vivox servers and any channels need to be rejoined. For more information, see Reconnection logic: Handling unexpected disconnections, application suspension, and backgrounding in the Unity Support Knowledge Base.

---

## Vivox Unity package structure

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/implement-vivox-unity/vivox-unity-package-structure

**Contents:**
- Vivox Unity package structure#

The Vivox Unity SDK package uses the following structure.

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/integration-guide/integration-plan/phase-one/ui-integration/microphone-settings

---

## Unity Build Automation

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/unity-build-automation

**Contents:**
- Unity Build Automation#
- What is Unity Build Automation?#
- Key concepts#
  - Continuous integration#
  - How automated build generation works#
  - Source Control#
- Unity Versions#
- Supported Platforms#
- Additional resources#

Unity Build Automation is a continuous integration service that automatically creates multiplatform builds in the Cloud. You can point Build Automation toward your version control system to:

Build Automation supports most popular version control systems and builds for multiple platforms simultaneously, including iOS.

You can access Unity Build Automation through the Unity Dashboard.

Build Automation provides continuous integration by automatically building your project whenever changes are detected in the configured version control system.

By compiling your project whenever a change is committed, Build Automation gives you the most accurate idea of when and where errors occur and ensures you always have a build of your game based on the last working commit.

To enable automated build generation, set the Auto-build toggle to Yes in Build Automation > Configurations > Basic Info settings.

Build Automation connects to either Unity Version Control or your source control system and monitors that system for changes to your project. When it detects changes to your project, Build Automation downloads and builds your project for your target platforms. When the builds are complete, Build Automation notifies you of the results and links to download, share, and install the builds. If there are errors, Build automation informs you immediately so you can quickly fix them, commit the changes, and trigger new builds.

You can integrate different software for notifications such as Slack, Discord, and Jira. For more information, refer to integrations.

Source Control is a system for managing file changes. You can use Build Automation in conjunction with most version control tools, including UVCS, Perforce, and Git. Refer to Connect Your Version Control System.

In alignment with Unity’s platform-wide support policy, we only answer support tickets for LTS releases and the latest TECH release. For more information on Unity’s support policy for editor releases, refer to LTS releases.

Build Automation supports a wide range of platforms, enabling you to build projects for various targets efficiently. Below is the list of platforms currently supported by Build Automation:

Note: Refer to the available versions for Xcode and Android.

---

## Sign a macOS application

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/sign-build-artifacts/sign-a-macos-application

**Contents:**
- Sign a macOS application#
- Create a Developer ID Certificate from an Apple device#
- Create a Developer ID Certificate from a Windows or Linux device#
  - Convert a Developer ID certificate to a .p12 file on Windows and Linux#
- Generate an application password#
- Notarize with Unity Build Automation#
  - Environment variables#

Unity Build Automation can notarize and staple your macOS application during the build process. This ensures that your application meets the macOS notarization requirements.

Note: Notarization isn't required to distribute your application through the Mac App Store. The Mac App Store’s upload process includes similar content validation to notarization. Refer to Delivering to the Mac App Store for more information.

If you develop on an Apple device, you can set up Unity Build Automation to notarize and staple your application.

To meet the requirements, use the following steps:

Note: To generate the .p12 file with multiple certificates, you need to import the following into Keychain access: your Developer ID certificate, your Developer ID - G2 certificate, and the private key used for your Certificate Signing Request. In Keychain Access, export all three items as a single .p12 file and use that file.

After completing these steps, refer to Notarize with Unity Build Automation.

If you develop on Windows or Linux, you can set up Unity Build Automation to notarize and staple your application. To meet the requirements, use the following steps:

After completing these steps, refer to Notarize with Unity Build Automation.

A .p12 file bundles both your Developer ID certificate and a private key. To create one from your Developer ID certificate:

Open a command-line interface and go to the directory that has your Developer ID certificate file. If you didn't download your Developer ID certificate, refer to Signing identity.

Developer ID certificates use the .cer file format. Convert this file to the .pem file format. To do this, run the following command where:

Generate a new private key. To do this, run the following command where:

Generate the .p12 file. To do this, run the following command where:

To notarize an application, Apple requires an Apple ID and an app specific unique password in a particular format.

For information on how to generate an application password, see How to generate an app-specific password(Apple). The password you generate uses the following format: xxxx-xxxx-xxxx-xxxx.

To notarize your macOS application in Unity Build Automation, use the following steps:

When the build is complete, Unity Build Automation attempts to notarize and staple the result. Unity Build Automation runs the codesign command with the following flags: --deep --force --verify --verbose --timestamp --options runtime. You can't specify custom flags for your project.

After Unity Build Automation builds, notarizes, and staples your project, you can download a compressed file that has the build.

The following environment variables are available for use in your build configuration. To use them, go to Advanced Settings > Environment Variables and add a new variable.

**Examples:**

Example 1 (unknown):
```unknown
developer_identity.cer
```

Example 2 (unknown):
```unknown
developer_identity.pem
```

Example 3 (unknown):
```unknown
openssl x509 -in developer_identity.cer -inform DER -out developer_identity.pem -outform PEM
```

Example 4 (unknown):
```unknown
openssl x509 -in developer_identity.cer -inform DER -out developer_identity.pem -outform PEM
```

---

## Retrieve channel-based chat history

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/chat-history/retrieve-channel-chat-history

**Contents:**
- Retrieve channel-based chat history#

To retrieve the channel-based chat history, you need to sign in and connect to at least one channel. You can then use vx_req_session_chat_history_query to make a request.

vx_req_session_chat_history_query will return 10 messages by default. You can increase the number of messages that are returned by increasing the max value in the request. The maximum number of messages that we store per channel is 5000, with a maximum age of 7 days.

You will receive a vx_evt_account_archive_message event for every message.

The following code is an example of how to retrieve the last 20 messages for the specific channel:

After you get these messages, the game must then process two types of event messages:

**Examples:**

Example 1 (unknown):
```unknown
vx_req_session_chat_history_query *req;
vx_req_session_chat_history_query_create(&req);
req->session_handle = vx_strdup("mychannel");
req->max = 20;
vx_issue_request2(&req->base);
```

Example 2 (unknown):
```unknown
vx_req_session_chat_history_query *req;
vx_req_session_chat_history_query_create(&req);
req->session_handle = vx_strdup("mychannel");
req->max = 20;
vx_issue_request2(&req->base);
```

Example 3 (unknown):
```unknown
void HandleSessionChatHistoryMessage(vx_evt_session_archive_message
 *evt)
{
    // Use information in events to display messages
    …
}
 
void HandleSessionChatHistoryEnd(vx_evt_session_archive_query_end_t *resp)
{
    if (resp != nullptr && resp->return_code == 0) {
    // Ended abnormally
    } else {
        // Ended properly
    }
}
```

Example 4 (unknown):
```unknown
void HandleSessionChatHistoryMessage(vx_evt_session_archive_message
 *evt)
{
    // Use information in events to display messages
    …
}
 
void HandleSessionChatHistoryEnd(vx_evt_session_archive_query_end_t *resp)
{
    if (resp != nullptr && resp->return_code == 0) {
    // Ended abnormally
    } else {
        // Ended properly
    }
}
```

---

## Evidence report HTTP request

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/chat-history/evidence-report-http-request

**Contents:**
- Evidence report HTTP request#

The following is an example is a request with the additional parameters:

**Examples:**

Example 1 (unknown):
```unknown
curl https://services.api.unity.com/vivox/text-evidence-management/v1/organizations/<organization_id>/projects/<project_id>/text-evidence-report?target_uri=sip:confctl-g-your-issuer.moderation-testing2@yourdomainname.vivox.com&sender_uri=sip:your-issuer.username.1234.@yourdomainname.vivox.com&start_ts=1697117012&end_ts=1697117217&num_messages=2 -u service-acct-username:service-account-password
```

Example 2 (unknown):
```unknown
curl https://services.api.unity.com/vivox/text-evidence-management/v1/organizations/<organization_id>/projects/<project_id>/text-evidence-report?target_uri=sip:confctl-g-your-issuer.moderation-testing2@yourdomainname.vivox.com&sender_uri=sip:your-issuer.username.1234.@yourdomainname.vivox.com&start_ts=1697117012&end_ts=1697117217&num_messages=2 -u service-acct-username:service-account-password
```

---

## In-game audio control

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/audio/in-game-audio-control/in-game-audio-control-toc

**Contents:**
- In-game audio control#

---

## Sample app requirements

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/vivox-unreal-sample-app/sample-app-requirements

**Contents:**
- Sample app requirements#

The Unreal Shooter Game sample app follows Unreal Engine 4 hardware and software requirements.

For more information, see the Unreal Engine documentation.

---

## Manual workflow

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/manual-workflow

**Contents:**
- Manual workflow#
  - Create a .NET project#
    - JetBrains Rider#
    - Microsoft Visual Studio#
  - Import the Cloud Code Authoring package#
    - JetBrains Rider#
    - Microsoft Visual Studio#
  - Create a module endpoint#
  - Package code#
    - JetBrains Rider#

This section describes the steps you need to create a C# module in your project.

Important: The manual workflow is a low-level approach to create and deploy modules. This workflow is ideal for advanced cases where you need more control over the module deployment process. For most use cases, you can use the Unity Editor or the UGS CLI for a more streamlined experience. For a quick start, refer to the Get Started guide.

To get started with Cloud Code modules, you must follow these steps:

A Cloud Code module is a simple .NET project which exposes endpoints that a game client can call. To get started, create a new .NET project in your IDE of choice. To learn more about how a Cloud Code module is structured, refer to module structure.

The following sections show how to create a project in the JetBrains Rider and Microsoft Visual Studio IDEs.

Follow these steps to create a new project in JetBrains Rider:

Follow these steps to create a new project in Microsoft Visual Studio:

Cloud Code modules are .NET projects, so they use the NuGet package manager to manage dependencies.

Cloud Code modules require the Cloud Code Authoring package so you can create endpoints you can call out to. This is a NuGet package that you can install into your project.

The following sections show how to import the package in the following IDEs.

Follow these steps to install the Cloud Code Authoring package with JetBrains Rider:

Follow these steps to install the Cloud Code Authoring package with Microsoft Visual Studio:

To create an endpoint within the C# modules, you need to create an externally callable function, attribute it with the CloudCodeFunction attribute, and give it its public facing name.

Your function name can be different, and this allows you to refactor your code later without breaking the functions that the game client already calls.

You can use the following simple example of a module function for reference. This example creates a module endpoint called SayHello that takes a string parameter called name and returns a string:

Before you can deploy your module, you need to package your code into an archive.

This archive must contain the assemblies that the Cloud Code service needs to run your module.

To produce these assemblies, you can publish your project.

You need to build the assemblies for the linux-x64 target runtime. To improve performance, you can enable ReadyToRun compilation.

Follow the steps below to configure your IDE to produce assemblies.

To produce assemblies using the JetBrains Rider IDE, follow the steps below:

To produce assemblies using the Microsoft Visual Studio IDE, follow the steps below:

By default, the publish process produces a folder following the bin/Release/net6.0/linux-x64/publish path under your root project directory. You need to zip the contents of this folder into an archive.

It's recommended that you change the archive's extension to .ccm if you want to deploy the module using the UGS CLI.

Note: If you use File Explorer in Windows, you need to enable common file name extensions to change the archive's extension to .ccm.

On Unix systems, run the following command:

zip ExampleModule.ccm *

On Windows systems, run the following command:

zip -r ExampleModule.ccm path/to/your/project/bin/Release/net6.0/linux-x64/publish/*

To make your module endpoints accessible to the game client, you need to deploy the archive to the Cloud Code service.

Refer to write modules to learn more about module deployment.

To deploy using the UGS CLI, you have to change the archive file extension to .ccm.

Follow the steps below to get started with the UGS CLI:

To deploy a module, run the following command:

ugs deploy <path-to-ccm-file>

After you deploy your module, you can call your module function from your game client.

To use Cloud Code in the Unity Editor, you first need to install the Cloud Code SDK and link your Unity Gaming Services project to the Unity Editor.

You can find your UGS project ID in the Unity Dashboard. Follow the steps below to link your Unity Gaming Services project with the Unity Editor.

In Unity Editor, select Edit > Project Settings > Services.

Link your project.If your project doesn't have a Unity project ID:

If you have an existing Unity project ID:

Your Unity Project ID appears, and the project is now linked to Unity services. You can also access your project ID in a Unity Editor script using UnityEditor.CloudProjectSettings.projectId.

To install the latest Cloud Code package for Unity Editor:

Note: Refer to Package Manager window (Unity Manual) to familiarize yourself with the Unity Package Manager interface.

To get started with the Cloud Code SDK:

Note: Players need a valid player ID and access token to access the Cloud Code services. You need to authenticate players with the Authentication SDK before you use any of the Cloud Code APIs. You can use the following code snippet for anonymous authentication, or you can check the documentation for the Authentication SDK for more details and other sign-in methods.

To call a module function from your game client, you have to authenticate the call, initialize the Services Core SDK before you call out to any of the services’ functionality, and call out to the module function.

For more information on how to call modules using the game client, refer to running modules from Unity runtime.

Follow the steps below to call a module function:

Note: The CallEndpointModuleAsyncfunction is available with Cloud Code SDK version 2.3.0+.

Below is an example of how to call the module function SayHello from the module ExampleModule within your game client.

Important: When you call a module function for the first time it may take some time before it starts responding. If you receive a timeout, run the code again until it prints Hello, World! in the console.

**Examples:**

Example 1 (unknown):
```unknown
Com.Unity.Services.CloudCode.Core
```

Example 2 (unknown):
```unknown
Com.Unity.Services.CloudCode.Core
```

Example 3 (unknown):
```unknown
CloudCodeFunction
```

Example 4 (unknown):
```unknown
using Unity.Services.CloudCode.Core;

namespace ExampleModule;

public class MyModule
{
    [CloudCodeFunction("SayHello")]
    public string Hello(string name)
    {
        return $"Hello, {name}!";
    }
}
```

---

## In-game control of audio levels

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/in-game-audio-control/in-game-control-audio-levels

**Contents:**
- In-game control of audio levels#

You can use the Vivox SDK to allow in-game control for audio input and output levels.

The following list details examples of scenarios in which you would want to implement in-game control of audio levels:

Volume settings for both capture devices and render devices adjust the apparent loudness of the device. You can set these values to a range between 0 to 100 on a logarithmic scale, with +0db (no change) at 50. Because settings use a logarithmic scale, the generally usable range is between 40 and 75, with 75 tending towards over modulation on a capture device and possible user discomfort when using a headset. Note that any increase in the volume above the default value can introduce some amount of distortion, although this is less noticeable at lower levels.

Note: We do not recommend that you allow users to increase their capture device volume above 75.

In the Vivox SDK, use vx_req_aux_set_mic_level_t and vx_req_aux_set_speaker_level_t to allow users to change the gain (volume) of their respective audio device.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_aux_set_mic_level_t
```

Example 2 (unknown):
```unknown
vx_req_aux_set_speaker_level_t
```

---

## AVAudioSession configuration when using WWise

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/ios/avaudiosession-and-wwise

**Contents:**
- AVAudioSession configuration when using WWise#
- At game initialization#
- Before a channel join#
- After leaving a channel#

**Examples:**

Example 1 (unknown):
```unknown
AkPlatformInitSettings platformInitSettings;
AK::SoundEngine::GetDefaultPlatformInitSettings(platformInitSettings);
platformInitSettings.audioSession.eCategory = AkAudioSessionCategory::AkAudioSessionCategoryPlayback;
platformInitSettings.audioSession.eMode = AkAudioSessionMode::AkAudioSessionModeDefault;
platformInitSettings.audioSession.eCategoryOptions = (AkAudioSessionCategoryOptions)(AkAudioSessionCategoryOptions::AkAudioSessionCategoryOptionDefaultToSpeaker | AkAudioSessionCategoryOptions::AkAudioSessionCategoryOptionAllowBluetooth);
AK::SoundEngine::iOS::ChangeAudioSessionProperties(platformInitSettings.audioSession);
```

Example 2 (unknown):
```unknown
AkPlatformInitSettings platformInitSettings;
AK::SoundEngine::GetDefaultPlatformInitSettings(platformInitSettings);
platformInitSettings.audioSession.eCategory = AkAudioSessionCategory::AkAudioSessionCategoryPlayback;
platformInitSettings.audioSession.eMode = AkAudioSessionMode::AkAudioSessionModeDefault;
platformInitSettings.audioSession.eCategoryOptions = (AkAudioSessionCategoryOptions)(AkAudioSessionCategoryOptions::AkAudioSessionCategoryOptionDefaultToSpeaker | AkAudioSessionCategoryOptions::AkAudioSessionCategoryOptionAllowBluetooth);
AK::SoundEngine::iOS::ChangeAudioSessionProperties(platformInitSettings.audioSession);
```

Example 3 (unknown):
```unknown
AkPlatformInitSettings platformInitSettings;
AK::SoundEngine::GetDefaultPlatformInitSettings(platformInitSettings);
platformInitSettings.audioSession.eCategory = AkAudioSessionCategory::AkAudioSessionCategoryPlayAndRecord;
platformInitSettings.audioSession.eMode = AkAudioSessionMode::AkAudioSessionModeVoiceChat;
platformInitSettings.audioSession.eCategoryOptions = (AkAudioSessionCategoryOptions)(AkAudioSessionCategoryOptions::AkAudioSessionCategoryOptionDefaultToSpeaker | AkAudioSessionCategoryOptions::AkAudioSessionCategoryOptionAllowBluetooth);
AK::SoundEngine::iOS::ChangeAudioSessionProperties(platformInitSettings.audioSession);
```

Example 4 (unknown):
```unknown
AkPlatformInitSettings platformInitSettings;
AK::SoundEngine::GetDefaultPlatformInitSettings(platformInitSettings);
platformInitSettings.audioSession.eCategory = AkAudioSessionCategory::AkAudioSessionCategoryPlayAndRecord;
platformInitSettings.audioSession.eMode = AkAudioSessionMode::AkAudioSessionModeVoiceChat;
platformInitSettings.audioSession.eCategoryOptions = (AkAudioSessionCategoryOptions)(AkAudioSessionCategoryOptions::AkAudioSessionCategoryOptionDefaultToSpeaker | AkAudioSessionCategoryOptions::AkAudioSessionCategoryOptionAllowBluetooth);
AK::SoundEngine::iOS::ChangeAudioSessionProperties(platformInitSettings.audioSession);
```

---

## Channel types

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/channels/channel-types

**Contents:**
- Channel types#
- Echo channels#
- Non-positional channels#
- Positional channels#

Vivox uses the following types of channels:

The channel type is indicated by the character in the middle of a given channel's SIP address. For more information, refer to Channel URI structure.

Note: When working with both positional and non-positional channels, join the positional channel first, followed by the non-positional channels.

Echo channels include an -e- in their SIP address, which is displayed in the following example:

Developers can use these channels to give users a place to test their microphones, or as a general way to test connectivity to the Vivox voice servers.

Non-positional channels, also referred to as 2D channels, include a -g- in their SIP address, which is displayed in the following example:

Developers can use these channels to allow for level-wide audio and text channels that players can connect to.

Examples of scenarios where non-positional channels are often used include teams and squads in first-person shooters, and party chat in MMOs.

Non-positional channels are typically the most used channel types in a Vivox implementation.

Positional channels, also referred to as 3D channels, include a -d- in their SIP address, which is displayed in the following example:

Developers can use these channels to provide voice chat that is a part of a world, with player voices attenuating and panning to make it seem like they're speaking from where they are positioned in the game world.

Note: You can join only one positional channel at a time.

You can parametrize positional channels with certain values that change how the positioning of the player affects their voice. For more information, see Positional channel properties.

**Examples:**

Example 1 (unknown):
```unknown
sip:confctl-e-issuer.channel-name@domain.vivox.com
```

Example 2 (unknown):
```unknown
sip:confctl-e-issuer.channel-name@domain.vivox.com
```

Example 3 (unknown):
```unknown
sip:confctl-g-issuer.channel-name@domain.vivox.com
```

Example 4 (unknown):
```unknown
sip:confctl-g-issuer.channel-name@domain.vivox.com
```

---

## Chat history

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity//text-chat-guide/chat-history

**Contents:**
- Chat history#
- Channel text message history#

Vivox allows for users to access the history of a channel's text activity using VivoxService.Instance.GetChannelTextMessageHistoryAsync(string channelName, int requestSize = 10, ChatHistoryQueryOptions chatHistoryQueryOptions = null), with channelName being the name of the channel to request the history of, requestSize being the number of messages to return at a maximum (10 is the default), and chatHistoryQueryOptions being an optional parameter which can do things like specifying a word or phrase contained in the messages, a specific PlayerId sender of the messages, or times to query before or after. More specific information on ChatHistoryQueryOptions can be found in Chat history query options.

The messages returned in IReadOnlyCollection are listed in order from newest message to oldest message.

Note: Chat history is stored for 7 days. If Text Evidence Management is in use, the storage period for chat history is extended to 30 days.

The following code snippet is an example of pulling at most recent 25 messages from a set LobbyChannelName, then logging the sender's display name and the message in order from oldest to newest, without using the optional ChatHistoryQueryOptions.

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.GetChannelTextMessageHistoryAsync(string channelName, int requestSize = 10, ChatHistoryQueryOptions chatHistoryQueryOptions = null)
```

Example 2 (unknown):
```unknown
channelName
```

Example 3 (unknown):
```unknown
requestSize
```

Example 4 (unknown):
```unknown
chatHistoryQueryOptions
```

---

## In-game audio device selection

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/audio/in-game-audio-control/in-game-audio-device-selection

**Contents:**
- In-game audio device selection#

The Vivox SDK automatically uses the system default audio input and output devices. However, you can override this behavior and provide users with a choice of which audio device to use for group voice chat independent of the audio devices that are used for the game.

To perform this override, build a user interface for the game that displays a list of available devices to users that they can select a device from. Build this user interface with the following requests, where audio output devices are called render devices, and audio input devices are called capture devices:

IAudioDevices::AvailableDevices returns a TMap<FString, IAudioDevice \*> map that contains a list of every device name and its corresponding Vivox IAudioDevice object. Use this request to populate a drop-down menu or to find an appropriate IAudioDevice instance to pass in to IAudioDevices::SetActiveDevice.

The following code is an example of how to activate the devices that the user selects:

When a user selects a new device, the game passes the appropriate IAudioDevice in to IAudioDevices::SetActiveDevice.

Alternately, you can pass a virtual device, such as IAudioDevices::SystemDevice() or IAudioDevices::CommunicationDevice(), in to IAudioDevices::SetActiveDevice to set the ActiveDevice to the system's default device or the system's communication device, respectively.

Set IAudioDevices::NullDevice() explicitly to prevent input or output.

To alert the game to changes in the IAudioDevices setup, bind to the following events:

Binding to these events helps you to build a responsive system that can handle device hotswapping.

**Examples:**

Example 1 (javascript):
```javascript
VivoxCoreError IAudioDevices::SetActiveDevice(const FString \&device, const FOnSetActiveDeviceCompletedDelegate \&theDelegate=FOnSetActiveDeviceCompletedDelegate())
```

Example 2 (javascript):
```javascript
const TMap<FString, IAudioDevice *> \&IAudioDevices::AvailableDevices()
```

Example 3 (javascript):
```javascript
const IAudioDevice IAudioDevices::CommunicationDevice()
```

Example 4 (javascript):
```javascript
const IAudioDevice IAudioDevices::SystemDevice()
```

---

## Buddy behavior

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/presence/buddy-requests

**Contents:**
- Buddy behavior#

A user can add a buddy or delete a buddy.

The following list details scenarios for when User A adds User B as a buddy:

If User B is offline, then User A sees User B as unavailable.

When User B comes online, the following actions occur:

If User B has an accept rule for User A, then User A sees User B's online status.

If User B has a block rule for User A, then User A sees User B as unavailable.

If User B does not have a block rule or an accept rule, and vx_req_account_anonymous_login.buddy_managment_mode is set to mode_auto_accept, then User A sees User B as available.

If User B does not have a block rule or an accept rule, and vx_req_account_anonymous_login.buddy_managment_mode is set to mode_block, then User A sees User B as available.

If User B does not have a block rule or an accept rule, and vx_request_account_anonymous_login.buddy_managment_mode is set to mode_application, then User B receives a vx_evt_subscription message.

The game can then either ignore the subscription or issue a vx_req_account_send_subscription_reply_t message, and can either block or accept the subscription.

The following code displays an example of the process for handling an incoming buddy request:

**Examples:**

Example 1 (unknown):
```unknown
void HandleEvtSubscription(vx_evt_subscription *evt)
{
    // Only approve buddies that have "wizard" in the name.
    if (strstr(evt->buddy_uri, "wizard"))
    {
        vx_req_account_send_subscription_reply_t *req;
        vx_req_account_send_subscription_reply_create(&req);
        req->account_handle = vx_strdup(evt->account_handle);
        req->buddy_uri = vx_strdup(evt->buddy_uri);
        req->rule_type = rule_allow;
        vx_issue_request3(&req->base, &request_count);
    }
}
```

Example 2 (unknown):
```unknown
void HandleEvtSubscription(vx_evt_subscription *evt)
{
    // Only approve buddies that have "wizard" in the name.
    if (strstr(evt->buddy_uri, "wizard"))
    {
        vx_req_account_send_subscription_reply_t *req;
        vx_req_account_send_subscription_reply_create(&req);
        req->account_handle = vx_strdup(evt->account_handle);
        req->buddy_uri = vx_strdup(evt->buddy_uri);
        req->rule_type = rule_allow;
        vx_issue_request3(&req->base, &request_count);
    }
}
```

---

## Get started with Friends

**URL:** https://docs.unity.com/friends/en/manual/guides/get-started

**Contents:**
- Get started with Friends#
- Prerequisites#
- Set up Friends#
- Link your project with the Unity Dashboard#
- Enable the Friends service#
- Install the Friends package#
- Initialize the Friends service#

Attention: The Digital Services Act (DSA) requires Unity to notify our customers’ end users if Unity takes an action that impacts those end users under the DSA. To comply with this requirement, if you use Unity Gaming Services (UGS) products that rely on the Unity Authentication Service, you must integrate the notification API.

For more information on DSA, refer to the Digital Services Act - compliance update.

To make your game compliant, refer to DSA notifications.

This guide walks you through how to set up and start using Friends.

To get started with Friends, you need to do the following:

You can set up and manage Friends through the Unity Dashboard:

When you launch Friends for the first time, this adds Friends to the Shortcuts section on the sidebar and opens the Overview page.

You must link your project with the Unity Dashboard to use the Friends service.

Your Unity Project ID now appears in Unity Editor, and the project is linked to Unity’s cloud services. You can also access your Unity Project ID in code using UnityEditor.CloudProjectSettings.projectId.

You can enable the Friends service for your project from the Unity Dashboard.

The Setup Guide walks you through linking your Unity project, installing the Friends package, and enabling the Friends service.

Once you've linked your project to the Unity Dashboard, you can install the latest version of the Friends package.

Use the Unity Package Manager to import the Friends package in the Unity Editor.

Once you’ve enabled the Friends service and imported the Friends SDK through the Package Manager in the Unity Editor, initialize the Core SDK using await UnityServices.InitializeAsync().

The following code sample shows how to initialize the Friends service and any other Unity services in your project.

**Examples:**

Example 1 (unknown):
```unknown
UnityEditor.CloudProjectSettings.projectId
```

Example 2 (unknown):
```unknown
await UnityServices.InitializeAsync()
```

Example 3 (unknown):
```unknown
// Import required packages
using Unity.Services.Core;
using Unity.Services.Authentication;
...
private async Task MyMethod()
{
    // Initialize all of the used services
    await UnityServices.InitializeAsync();

    // Sign in anonymously or using a provider
    await AuthenticationService.Instance.SignInAnonymouslyAsync();

    // Initialize friends service
    await FriendsService.Instance.InitializeAsync();

    // Start using the Friends SDK functionalities.
    var friends = FriendsService.Instance.Friends;
}
```

Example 4 (unknown):
```unknown
// Import required packages
using Unity.Services.Core;
using Unity.Services.Authentication;
...
private async Task MyMethod()
{
    // Initialize all of the used services
    await UnityServices.InitializeAsync();

    // Sign in anonymously or using a provider
    await AuthenticationService.Instance.SignInAnonymouslyAsync();

    // Initialize friends service
    await FriendsService.Instance.InitializeAsync();

    // Start using the Friends SDK functionalities.
    var friends = FriendsService.Instance.Friends;
}
```

---

## Voice option dialog

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/integration-guide/integration-plan/phase-one/ui-integration/voice-option-dialog

**Contents:**
- Voice option dialog#

Users generally need some control over their voice experience.

The following image displays an example of a customizable voice option menu.

The following list details recommended customizable options:

Audio device selection

Capture device and render device level adjustment

Access to an echo channel to test voice settings

Voice activation methods:

---

## Audio management

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/in-game-audio-control/audio-management-toc

**Contents:**
- Audio management#

---

## Positional channels

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/positional-channels

**Contents:**
- Positional channels#

When working with positional channels (also referred to as 3D channels), a player’s position and facing direction matter. This is in contrast to non-positional channels, which act more like radio communication.

Note: When working with both positional and non-positional channels, join the positional channel first, followed by the non-positional channels.

You can create positional channels with different properties to allow for a range of audio spaces. For example, you might have a scenario where acoustically realistic sounding audio is needed for outdoor spaces, or a scenario where shorter-range, quickly dampening audio is more appropriate to account for tight inner corridors or for how voice carries in heavy rain. Positional channels can also add a "cocktail party" effect to the audio, which provides ambient crowd noise. Any style allows for voice chat to naturally fade in and out to near zero loudness as players approach or walk away from each other.

Vivox audio processing mixes all of the audio for every player, including volume attenuation over a distance and channel fading, which is based on the direction that the character or camera is facing. Channel participant added or removed events are based on players entering or exiting your max range, which ensures that you always know which players are close enough to hear.

---

## Example: Mute_All token

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/access-token-examples/example-mute-all-token

**Contents:**
- Example: Mute_All token#
- Signed in user muting all users in a channel#
- Admin user muting all users in a channel#
- Signed in user muting all but one user in a channel (Presentation mode)#
- Admin user muting all but one user in a channel (Presentation mode)#

Note: The token format for unmuting all users in a channel is the same as a mute_all token.

The token in this example allows the user "beef" to mute all users in the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

The token in this example allows the user "blindmelon-AppName-dev-Admin" to mute all users in the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

The token in this example allows the user "beef" to mute all users except for the user "jerky" in the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

In this scenario, when creating the mute token, a subject is specified. The subject in this instance ("jerky") is the only user in the channel that is not muted, which allows them to speak to the other, now muted, users in the channel without interruption.

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

The token in this example allows the user "blindmelon-AppName-dev-Admin" to mute all users except for the user "jerky" in the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

In this scenario, when creating the mute token, a subject is specified. The subject in this instance ("jerky") is the only user in the channel that is not muted, which allows them to speak to the other, now muted, users in the channel without interruption.

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

**Examples:**

Example 1 (unknown):
```unknown
{
    "vxi":19283,
    "f":"sip:.blindmelon-AppName-dev.beef.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"mute",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 2 (unknown):
```unknown
{
    "vxi":19283,
    "f":"sip:.blindmelon-AppName-dev.beef.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"mute",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 3 (unknown):
```unknown
e30.eyJ2eGkiOjE5MjgzLCJmIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2LmJlZWYuQHRsYS52aXZveC5jb20iLCJpc3MiOiJibGluZG1lbG9uLUFwcE5hbWUtZGV2IiwidnhhIjoibXV0ZSIsInQiOiJzaXA6Y29uZmN0bC1nLWJsaW5kbWVsb24tQXBwTmFtZS1kZXYudGVzdGNoYW5uZWxAdGxhLnZpdm94LmNvbSIsImV4cCI6MTYwMDM0OTQwMH0.fARLW2eX10ZbiIl_5WIg4bhPbYIhn2xfCcUNySfwBMs
```

Example 4 (unknown):
```unknown
e30.eyJ2eGkiOjE5MjgzLCJmIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2LmJlZWYuQHRsYS52aXZveC5jb20iLCJpc3MiOiJibGluZG1lbG9uLUFwcE5hbWUtZGV2IiwidnhhIjoibXV0ZSIsInQiOiJzaXA6Y29uZmN0bC1nLWJsaW5kbWVsb24tQXBwTmFtZS1kZXYudGVzdGNoYW5uZWxAdGxhLnZpdm94LmNvbSIsImV4cCI6MTYwMDM0OTQwMH0.fARLW2eX10ZbiIl_5WIg4bhPbYIhn2xfCcUNySfwBMs
```

---

## Example: Drop_All token

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/access-token-guide/access-token-examples/example-drop-all-token

**Contents:**
- Example: Drop_All token#

The token in this example allows the user "blindmelon-AppName-dev-Admin" to drop all users from the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Because this is a kick command, the vxa is "kick".

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

**Examples:**

Example 1 (unknown):
```unknown
{
    "vxi":729614,
    "f":"sip:blindmelon-AppName-dev-Admin@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"kick",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 2 (unknown):
```unknown
{
    "vxi":729614,
    "f":"sip:blindmelon-AppName-dev-Admin@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"kick",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 3 (unknown):
```unknown
e30.eyJ2eGkiOjcyOTYxNCwiZiI6InNpcDpibGluZG1lbG9uLUFwcE5hbWUtZGV2LUFkbWluQHRsYS52aXZveC5jb20iLCJpc3MiOiJibGluZG1lbG9uLUFwcE5hbWUtZGV2IiwidnhhIjoia2ljayIsInQiOiJzaXA6Y29uZmN0bC1nLWJsaW5kbWVsb24tQXBwTmFtZS1kZXYudGVzdGNoYW5uZWxAdGxhLnZpdm94LmNvbSIsImV4cCI6MTYwMDM0OTQwMH0.bvNDTcXIHRpckAiFzy3wPiwgYGks4E9_WuEA5HMGpGE
```

Example 4 (unknown):
```unknown
e30.eyJ2eGkiOjcyOTYxNCwiZiI6InNpcDpibGluZG1lbG9uLUFwcE5hbWUtZGV2LUFkbWluQHRsYS52aXZveC5jb20iLCJpc3MiOiJibGluZG1lbG9uLUFwcE5hbWUtZGV2IiwidnhhIjoia2ljayIsInQiOiJzaXA6Y29uZmN0bC1nLWJsaW5kbWVsb24tQXBwTmFtZS1kZXYudGVzdGNoYW5uZWxAdGxhLnZpdm94LmNvbSIsImV4cCI6MTYwMDM0OTQwMH0.bvNDTcXIHRpckAiFzy3wPiwgYGks4E9_WuEA5HMGpGE
```

---

## Access token issuers & signing keys

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/access-token-identifiers/access-token-issuer-unity

**Contents:**
- Access token issuers & signing keys#

When you access Vivox through the Unity Cloud Dashboard, find the issuer and signing key under Vivox Voice and Text Chat > Credentials in the Unity dashboard.

---

## Add a buddy

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/presence/add-buddy

**Contents:**
- Add a buddy#

After a user signs in, if they want to add a buddy, the game must send a vx_account_buddy_set request.

The following code displays an example of this process with the sample issuer key "MyIssuer":

**Examples:**

Example 1 (unknown):
```unknown
vx_req_account_buddy_set req;
vx_req_account_buddy_set_create(&req);
req->account_handle = vx_strdup(".issuer-w-dev.mytestaccountname.");
req->buddy_uri = vx_strdup(“sip:.issuer-w-dev.mybuddy.@mt1s.vivox.com”);
vx_issue_request3(&req->base, &request_count);
```

Example 2 (unknown):
```unknown
vx_req_account_buddy_set req;
vx_req_account_buddy_set_create(&req);
req->account_handle = vx_strdup(".issuer-w-dev.mytestaccountname.");
req->buddy_uri = vx_strdup(“sip:.issuer-w-dev.mybuddy.@mt1s.vivox.com”);
vx_issue_request3(&req->base, &request_count);
```

---

## Partial connection failure

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/partial-connection-failure

**Contents:**
- Partial connection failure#

In some cases, such as when an application is suspended in the background, the connection to Vivox services can be disrupted so that voice communication terminates without signing out the user. In that case, a vx_evt_media_stream_updated event will fire with a state of session_media_disconnected and a non-zero status_code.

Note: An event that indicates the media stream has disconnected is not guaranteed to occur before a reconnect failure and user sign out.

The following sample code is an example of how to handle media stream updated events.

**Examples:**

Example 1 (unknown):
```unknown
vx_evt_media_stream_updated
```

Example 2 (unknown):
```unknown
session_media_disconnected
```

Example 3 (unknown):
```unknown
status_code
```

Example 4 (unknown):
```unknown
void HandleMediaStreamUpdatedEvent(vx_evt_connection_state_changed &evt)
{
    vx_session_media_state media_state = evt.state;
    if (evt.state == session_media_connected)
    {
        printf("Connected to voice session %s\n", evt.session_handle);
    }
    else if (evt.state == session_media_disconnected)
    {
        if (evt.status_code != 0)
        {
            printf("Disconnected from voice session %s with status %d:%s\n", evt.session_handle, evt.status_code, vx_get_error_string(evt.status_code));
            // Expect a session_removed event to follow.
            // Wait to re-join the session until after the network connection is restored.
        }
        else
        {
            printf("Disconnected from voice session %s\n", evt.session_handle);
        }
    }
}
```

---

## Android app development

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/android/android-toc

**Contents:**
- Android app development#

---

## XMPP representation - Edit a message in a channel

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/chat-history/edit-delete/xmpp-edit-in-channel

**Contents:**
- XMPP representation - Edit a message in a channel#

Example edit request:

Example edit response:

Edited event notifications can be propagated back to every participant in the channel.

Example of propagating an edited message event:

**Examples:**

Example 1 (unknown):
```unknown
<iq type='set' id='...' from='...' to='...'>
 <query xmlns='urn:xmpp:mam:3'>
  <x xmlns='jabber:x:data' type='submit'>
   <field var='FORM_TYPE' type='hidden'>
    <value>urn:xmpp:mam:3</value>
   </field>
   <field var='message-id'>
    <value>123456789</value>
   </field>
   <field var='message'>
    <value>New Message</value>
   </field>
  </x>
 </query>
</iq>
```

Example 2 (unknown):
```unknown
<iq type='set' id='...' from='...' to='...'>
 <query xmlns='urn:xmpp:mam:3'>
  <x xmlns='jabber:x:data' type='submit'>
   <field var='FORM_TYPE' type='hidden'>
    <value>urn:xmpp:mam:3</value>
   </field>
   <field var='message-id'>
    <value>123456789</value>
   </field>
   <field var='message'>
    <value>New Message</value>
   </field>
  </x>
 </query>
</iq>
```

Example 3 (unknown):
```unknown
<iq from="someRoomSIP@cats.vivox.com"
    id="..."
    to="alice@cats.vivox.com"
    type="result">
    <message-edited xmlns="urn:vivox:message-edited" id="...">
        <new-message>New Message</new-message>
        <edit-time>1656521027</edit-time>
    </message-edited>
</iq>
```

Example 4 (unknown):
```unknown
<iq from="someRoomSIP@cats.vivox.com"
    id="..."
    to="alice@cats.vivox.com"
    type="result">
    <message-edited xmlns="urn:vivox:message-edited" id="...">
        <new-message>New Message</new-message>
        <edit-time>1656521027</edit-time>
    </message-edited>
</iq>
```

---

## Acoustic echo cancellation

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/android/acoustic-echo-cancellation

**Contents:**
- Acoustic echo cancellation#

When an Android device plays and records audio at the same time, there is the possibility that a capture device could pick up the audio that is playing. The Vivox SDK includes acoustic echo cancellation (AEC), which helps to prevent this type of audio capture from occurring.

Note: If there is a headset in use on an Android device, then the capture device will likely not pick up the output from the headset’s headphones, and no special configuration is required to avoid echo.

In some scenarios, Smart Platform Audio Management (SPAM) might not be the best option to use, such as when a particular Android device does not support hardware echo cancellation. To manually control platform AEC, disable SPAM, and then invoke the vx_set_platform_aec_enabled() function. This function turns on or turns off the recording preset and playback stream between SL_ANDROID_RECORDING_PRESET_VOICE_COMMUNICATION / SL_ANDROID_STREAM_VOICE and SL_ANDROID_RECORDING_PRESET_VOICE_RECOGNITION / SL_ANDROID_STREAM_MEDIA, respectively.

**Examples:**

Example 1 (unknown):
```unknown
vx_set_platform_aec_enabled()
```

Example 2 (unknown):
```unknown
SL_ANDROID_RECORDING_PRESET_VOICE_COMMUNICATION / SL_ANDROID_STREAM_VOICE
```

Example 3 (unknown):
```unknown
SL_ANDROID_RECORDING_PRESET_VOICE_RECOGNITION / SL_ANDROID_STREAM_MEDIA
```

---

## Group text messages

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/messaging/group-text-messages

**Contents:**
- Group text messages#

If a game has joined a channel with text enabled, then a user can send and receive group text messages. To send messages, the game uses the vx_request_session_send_message message. When this request is sent to the Vivox SDK, other users in that channel receive a vx_evt_message method.

The following code displays an example of the process for sending and receiving group text messages:

Note: vx_req_session_send_message.message_body must be UTF-8 encoded.

**Examples:**

Example 1 (unknown):
```unknown
vx_request_session_send_message
```

Example 2 (unknown):
```unknown
vx_evt_message
```

Example 3 (unknown):
```unknown
. . .
vx_req_session_send_message *req;
vx_req_session_send_message_create (&req);
req->session_handle = vx_strdup("mychannel");
req->message_body = "Here I am!";
vx_issue_request3(&req->base, &request_count);
. . .
void HandleEventMessage(vx_evt_message *evt)
{
    printf("Saw %s from user %s in %s\n", evt->message_body, evt->participant_uri, evt->session_handle);
}
```

Example 4 (unknown):
```unknown
. . .
vx_req_session_send_message *req;
vx_req_session_send_message_create (&req);
req->session_handle = vx_strdup("mychannel");
req->message_body = "Here I am!";
vx_issue_request3(&req->base, &request_count);
. . .
void HandleEventMessage(vx_evt_message *evt)
{
    printf("Saw %s from user %s in %s\n", evt->message_body, evt->participant_uri, evt->session_handle);
}
```

---

## User-to-user muting

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/muting/user-to-user-muting

**Contents:**
- User-to-user muting#

The muting scenarios in this section apply to both voice and text chat.

If User A mutes User B in a voice chat, then User A does not receive voice and text communications from User B.

---

## Builds

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/concepts/builds

**Contents:**
- Builds#
- Progression rollout#
- Forced rollout#

A build has the files to run your game or application on Multiplay Hosting’s servers. To link a build to a fleet and tell it how to run, you need to attach a build configuration to the build.

Note: Don't confuse Multiplay Hosting builds with Unity server builds that you export from the Unity editor.

Multiplay Hosting prompts you to upload your build files when you create a build. If you’re using a direct file upload, the files must be a loose set of unarchived (unzipped) files. The build won't work if it’s zipped into an archive.

Before you can use your build, you must create it into a release, which is a version of your build that’s ready to release to servers to run your game or application. The first version you create is Release 1.

The next time you update your build and create a new version, it will be Release 2. You can only use the latest version of a build.

When you update a build, it triggers build installs. The number of installs depends on the number of servers using the build.

There are two methods to update a build called rollout modes: progressive and forced.

A progressive rollout is a method of deploying a build update where you update servers only when they're empty. Multiplay Hosting waits for your matchmaker to deallocate the server if it has any connected players.

A forced rollout is a method of deploying a build update where you force servers to update even if the server has connected players. If players are connected when you start a forced rollout, Multiplay Hosting kicks the players from the server.

---

## Smoke test

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/integration-guide/integration-plan/phase-three/smoke-test

**Contents:**
- Smoke test#

When a user has voice enabled, UI elements indicate channel join state and what options (for example, mute, push-to-talk) are active and usable. Otherwise, UI elements aren't active or indicate that a user isn't in a channel.

---

## Channel identifiers for large scale games

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/channels/channel-identifiers-for-large-scale-games

**Contents:**
- Channel identifiers for large scale games#

If either of the following statements are true, then use caution when constructing your channel identifiers:

You can split large scale games across multiple physical audio servers ("shards") at the discretion of Vivox Operations. Construct your channel identifiers to use the following format:

For example, consider the application "Queen of the Death" (QotD), which allocates users according to geographical region, for example,"NA" for "North America". Assume that QotD joins a 3D channel for all participants in a match, and then also joins a 2D channel for members of a squad. In this case, the best practice for QotD is to apply the following criteria:

The match identifier assigned for this match is d0634bad1ca5a9cd, and the 3D space is tundra. Further assume that squads are assigned integer IDs starting with 1, and that the Channel3DProperties have already been set to the variable NewChannelProps.

Group Channel for Squad 1

Group Channel for Squad 2

**Examples:**

Example 1 (unknown):
```unknown
d0634bad1ca5a9cd
```

Example 2 (unknown):
```unknown
NewChannelProps
```

Example 3 (unknown):
```unknown
string shard = "NA";
string matchId = "d0634bad1ca5a9cd";
string positionalSpaceId = "tundra";
Channel3DProperties NewChannelProps = new Channel3DProperties(/* Add your own custom positional channel proprties here */);
await VivoxService.Instance.JoinPositionalChannelAsync($"{shard}.{matchId}-{positionalSpaceId}", ChatCapability.AudioOnly, NewChannelProps);
```

Example 4 (unknown):
```unknown
string shard = "NA";
string matchId = "d0634bad1ca5a9cd";
string positionalSpaceId = "tundra";
Channel3DProperties NewChannelProps = new Channel3DProperties(/* Add your own custom positional channel proprties here */);
await VivoxService.Instance.JoinPositionalChannelAsync($"{shard}.{matchId}-{positionalSpaceId}", ChatCapability.AudioOnly, NewChannelProps);
```

---

## Login management

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/integration-guide/integration-plan/phase-one/login-management

**Contents:**
- Login management#
- User identification#
- Additional resources#

Login management primarily focuses on selecting user IDs and identifying game client code paths to perform client-side login and authorization. The typical solution for signing in to the Vivox network is to associate your user's in-game account or character ID with a separately generated unique user ID or to use the account ID directly for identification.

Your game implementation must approve users who sign in to the Vivox network in advance. After a sign-in is approved, the game assigns a user ID to the client and instructs it to sign in with a provided access token. The client then signs in, is associated with the user ID that is provided, and can then perform voice operations within the Vivox network.

Several Vivox features rely upon the re-use of a player’s Vivox username or account ID. It’s important to remember to not send Vivox actual account names, or any identifiable information, such as a Vivox username or account ID. There must be a 1:1 relationship between your created ID for each session a player is logged in.

Because user IDs are selected on an ad-hoc basis and aren't preserved across sessions, it's important to select a unique ID that either persists with your game's user data or is newly generated during each successive sign-in.

For example, Player1 is hashed to 1a2b3c4d5e6f7g the first time they access Vivox through your game. The next time they access Vivox services through your game they will again be assigned 1a2b3c4d5e6f7g.

Special cases exist in which you can create permanent, persisted, password-authenticated users for your game server or for administrative purposes. You can create these accounts by using the Admin API. For more information, see the API Reference documentation.

Note: Signing in can take several seconds to complete. It's recommended that sign in is performed at a point in the game flow where this process will go unnoticed.

---

## Android build fails in Unity Build Automation

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/check-build-results/troubleshoot-build-failures/android-build-failures

**Contents:**
- Android build fails in Unity Build Automation#
- Symptoms#
- Cause#
- Resolution#
- Additional resource#

Unity Build Automation runs the Unity Editor in batchmode to execute your builds, and uses the default Android tools (JDK/SDK) to complete them. When your Android build fails in the service but works locally or other targets seem to work correctly, the root cause is often incompatible Android tools.

Failures due to Android tool often have the message Gradle Build failed in the logs. Gradle is the build system that Unity uses for Android builds.

The first step to resolve this issue is to ensure you can build your project locally in batch mode with the following tool versions:

You also need to validate the version of Gradle that you use.

Note: Check the versions of any project dependencies and ensure that they're pinned. Often builds begin to fail when dependency versions upgrade and require a newer Unity version or SDK version than you use for the build.

If your project does build locally, take note of the versions of the tools you used and ensure they match with the versions you use in the Unity Build Automation service. To check your local version of Gradle you can use the command gradle --version in the terminal.

If your build works locally in batchmode but fails in the Unity Build Automation service with the same tools and versions, please open a support ticket so the support team can investigate further. To submit a ticket from the Unity Dashboard, open DevOps and select Help & Support > Ticket > File a ticket.

**Examples:**

Example 1 (unknown):
```unknown
Gradle Build failed
```

Example 2 (unknown):
```unknown
gradle --version
```

---

## Authentication-based token generation

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/access-token-guide/authentication-package-tokens

**Contents:**
- Authentication-based token generation#

Authentication-based token generation is the simplest way to handle token generation.

Follow the steps on this page

---

## Example: Join token

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/access-token-guide/access-token-examples/example-join-token

**Contents:**
- Example: Join token#

The token in this example allows the user "jerky" to join the channel "sip:confctl-g-blindmelon.testchannel@tla.vivox.com".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

**Examples:**

Example 1 (unknown):
```unknown
"sip:confctl-g-blindmelon.testchannel@tla.vivox.com"
```

Example 2 (unknown):
```unknown
{
    "vxi":444000,
    "f":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"join",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 3 (unknown):
```unknown
{
    "vxi":444000,
    "f":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"join",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 4 (unknown):
```unknown
e30.eyJ2eGkiOjQ0NDAwMCwiZiI6InNpcDouYmxpbmRtZWxvbi1BcHBOYW1lLWRldi5qZXJreS5AdGxhLnZpdm94LmNvbSIsImlzcyI6ImJsaW5kbWVsb24tQXBwTmFtZS1kZXYiLCJ2eGEiOiJqb2luIiwidCI6InNpcDpjb25mY3RsLWctYmxpbmRtZWxvbi1BcHBOYW1lLWRldi50ZXN0Y2hhbm5lbEB0bGEudml2b3guY29tIiwiZXhwIjoxNjAwMzQ5NDAwfQ.u7us5eCxOBtuEZuDg1HapEEgxLedLaliIy7gOMfbeko
```

---

## Positional channels

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/channels/positional-channels

**Contents:**
- Positional channels#
- Related pages#

When working with positional channels (also referred to as 3D channels), a player’s position and facing direction matter. This is in contrast to non-positional channels, which act more like radio communication.

Note: When working with both positional and non-positional channels, join the positional channel first, followed by the non-positional channels.

You can create positional channels with different properties to allow for a range of audio spaces. For example, you might have a scenario where acoustically realistic sounding audio is needed for outdoor spaces, or a scenario where shorter-range, quickly dampening audio is more appropriate to account for tight inner corridors or for how voice carries in heavy rain. Positional channels can also add a "cocktail party" effect to the audio, which provides ambient crowd noise. Any style allows for voice chat to naturally fade in and out to near zero loudness as players approach or walk away from each other.

Vivox audio processing mixes all of the audio for every player, including volume attenuation over a distance and channel fading, which is based on the direction that the character or camera is facing. Channel participant added or removed events are based on players entering or exiting your max range, which ensures that you always know which players are close enough to hear.

---

## Generate a token on a secure server

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/access-token-guide/generate-token-secure-server/generate-token-secure-server-toc

**Contents:**
- Generate a token on a secure server#

Before you release your game to production, you need to generate your access tokens on a secure server and then send those access tokens to the game client. Allowing for token generation on the client during production is a security risk and can cause unexpected token expiration errors for your users.

Note: The actual implementation depends on your server programming environment and language.

---

## Retrieve account-wide chat history

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/chat-history/retrieve-account-chat-history

**Contents:**
- Retrieve account-wide chat history#

To retrieve account-wide chat history, you need to sign in and use vx_req_account_chat_history_query to make a request.

vx_req_account_chat_history_query will return 10 messages by default. You can increase the number of messages returned by increasing the max value in the request. The maximum number of messages stored per channel is 5000, with a maximum age of 7 days.

You will receive a vx_evt_account_archive_message event for every message.

The following code is an example of how to retrieve 20 messages for a particular user across all of their channels:

After you get these messages, the game must then process two types of event messages:

**Examples:**

Example 1 (unknown):
```unknown
vx_req_account_chat_history_query *req;
vx_req_account_chat_history_query_create(&req);
req->account_handle = vx_strdup("sip:.issuer.playerName@mt1s.vivox.com");
req->max = 20;
vx_issue_request2(&req->base);
```

Example 2 (unknown):
```unknown
vx_req_account_chat_history_query *req;
vx_req_account_chat_history_query_create(&req);
req->account_handle = vx_strdup("sip:.issuer.playerName@mt1s.vivox.com");
req->max = 20;
vx_issue_request2(&req->base);
```

Example 3 (unknown):
```unknown
void HandleAccountChatHistoryMessage(vx_evt_session_archive_message
 *evt)
{
    // Use information in events to display messages
    …
}
 
void HandleAccountChatHistoryEnd(vx_evt_session_archive_query_end_t *resp)
{
    if (resp != nullptr && resp->return_code == 0) {
    // Ended abnormally
    } else {
        // Ended properly
    }
}
```

Example 4 (unknown):
```unknown
void HandleAccountChatHistoryMessage(vx_evt_session_archive_message
 *evt)
{
    // Use information in events to display messages
    …
}
 
void HandleAccountChatHistoryEnd(vx_evt_session_archive_query_end_t *resp)
{
    if (resp != nullptr && resp->return_code == 0) {
    // Ended abnormally
    } else {
        // Ended properly
    }
}
```

---

## Text-to-speech destinations

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/text-to-speech/tts-destinations

**Contents:**
- Text-to-speech destinations#

Text-to-speech (TTS) messages are injected to destinations. Destinations determine two factors: output and mechanism.

Output is where the synthesized speech plays to and decides who hears the TTS message. Each destination uses one of the following outputs:

Mechanism is how new messages are handled when there is an ongoing message playing. Each destination uses one of the following injection mechanisms:

You can inject synthesized speech into the following destinations:

Note: Most destinations are independent and do not affect the behavior of any other destinations. However, the two queued destinations involving remote transmission share a queue.

Remote Transmission with Local Playback

Queued Remote Transmission

Note: Shares queue with Queued Remote Transmission with Local Playback.

Queued Local Playback

Queued Remote Transmission with Local Playback

Note: Shares queue with Queued Remote Transmission.

**Examples:**

Example 1 (unknown):
```unknown
TextToSpeechMessageType.RemoteTransmission
```

Example 2 (unknown):
```unknown
TextToSpeechMessageType.LocalPlayback
```

Example 3 (unknown):
```unknown
TextToSpeechMessageType.RemoteTransmissionWithLocalPlayback
```

Example 4 (unknown):
```unknown
TextToSpeechMessageType.QueuedRemoteTransmission
```

---

## Remote Config upgrade guide

**URL:** https://docs.unity.com/ugs/en-us/manual/remote-config/manual/upgrade-guide

**Contents:**
- Remote Config upgrade guide#

Do not upgrade this package if you're using Unity Editor 2018 LTS or Tech Stream 2019. Versions below 2020.2 (LTS) are not supported.

There's a new initialization for Unity Game Services which will require code updates for those who already implemented Remote Config in their Unity Project. Please review the Initialization section of CodeIntegration for the new code to be added to your implementation.

Upgrading a project from Unity 2018 or pre LTS 2019; developers no longer need to set the Scripting Runtime in the Editor. The option to change the Scripting Runtime version was removed after Unity 2019.2 https://docs.unity.cn/Manual/UpgradeGuide2019LTS.html#NET

The latest information on the Service features can be found in the REST API documentation

For a full list of changes and updates in this version, see the Remote Config Runtime package changelog.

---

## Transmission

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/transmission/transmission-overview

**Contents:**
- Transmission#
- Related pages#

Setting the TransmissionMode of an ILoginSession controls which channels any microphone audio and injected audio are broadcast into. Transmission settings only control where your audio goes and does not affect rendered audio spoken by other users.

By default, users do not transmit audio into any connected channel, and users can automatically transmit into a new channel when joined without calling specialized methods. Additional transmission-specific APIs exist in ILoginSession, which can set up transmission policy for current and future channels at any time. TransmissionMode is most important to consider for when a user connects to more than one channel at a time.

Many factors control whether other channel participants can hear a user speaking, including if they have locally muted the user or if the user has been muted by a moderator, if the user has globally muted their microphone, or if the user has joined the channel for text only. If everything else is set to allow voice, then the transmission policy determines if voice is heard.

Note: If you intend to join all channels as text only, or do not plan to join any channels at all (for example, if you plan to only use Vivox for online status and direct messaging), then no transmission APIs need to be called or considered.

**Examples:**

Example 1 (unknown):
```unknown
ILoginSession
```

Example 2 (unknown):
```unknown
ILoginSession
```

---

## SDKSampleApp overview

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/sdksampleapp/sdksampleapp-overview

**Contents:**
- SDKSampleApp overview#

The easiest way to get started with text chat is to use the SDKSampleApp that is packaged with the Vivox SDK. You need the issuer and key that is provided by Vivox Developer Support.

Note: For example purposes, the topics in this section use the issuer "xyzzy", the key "collosal", and the users "alice" and "bob".

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/concepts/build-configurations

---

## Access Token Developer Guide

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/access-token-guide-toc

**Contents:**
- Access Token Developer Guide#

Note: Vivox Access Tokens are an optional, and advanced, feature for managing player channel access. Unity Authentication tokens are the recommended way to manage players when channel access control is not required.

Player access to Vivox resources is controlled through Vivox access tokens (VATs). Vivox access tokens contain a payload that defines the privileged operation, are signed by the game server by using a token signing key, and are delivered by the client to the Vivox system when the player wants to perform a privileged operation. A Vivox access token is similar to a JSON Web Token, but instead has an empty access token header.

Access tokens have the following characteristics:

Game clients require access tokens to perform operations in the Vivox system.

When using Unity Authentication (UAS), Vivox access tokens can parse UASIDs from the token payload. It's mandatory to include embedded UAS IDs in your access tokens if you're using Vivox access tokens with Safe Text.

Refer to the sections titled Generate a token from the client and Generate a token on a secure sever to generate tokens.

Note that before either a client or a server can generate a token, you need a token issuer and a token signing key.

The following diagram displays a typical interaction between a game client, the game server, and the Vivox system:

---

## Automate local deployment

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/automate-local-deployment

**Contents:**
- Automate local deployment#
- Prerequisites#
- Automate with the UGS CLI#
- Automate using the project file#
  - Project file configuration#
    - Ready to Run compilation (optional)#
  - Running targets#
  - Expected output#

This guide shows you how to automate the deployment of your Cloud Code module with the Unity Gaming Services (UGS) CLI or the project file.

If you want to integrate the deployment into your CI/CD pipeline, refer to Integrating with CI/CD.

You need a Cloud Code module and the UGS CLI to automate local deployment:

The UGS CLI provides a deploy command that you can use to deploy a solution. The deploy command zips the module and deploys it to the Cloud Code service.

To deploy, run the following command from your terminal in your project directory:

Note: To support compilation, the solution needs to contain a publish profile for the main project. For more information, refer to Deploy C# solutions.

You can use the project file to automate the deployment of your Cloud Code module if you configure the project file to publish, zip and deploy your project on build.

Note: This is an advanced workflow. For most cases, the UGS CLI Deploy command is sufficient.

For more information, refer to Microsoft guide on Understanding the project file.

The script uses the UGS CLI Deploy command.

You can use the following project file configuration to automate the deployment of your Cloud Code module:

Copy and paste the above configuration into your project file.

Ready to Run compilation is an optional feature that improves the cold start time of your Cloud Code module. To check how this option affects your module, refer to Ready to Run_compilation).

To enable Ready to Run compilation, modify the PublishModule target to include the PublishReadyToRun property:

To run the targets, run the following command from your terminal in your project directory:

If the msbuild command is not found, you may need to add the msbuild path to your PATH environment variable.

Note: You can run any of the other targets individually if you replace DeployModule with another target name. The targets are dependent on each other, so any target that you run also runs the targets it depends on.

If the deployment is successful, the following output is displayed:

**Examples:**

Example 1 (unknown):
```unknown
ugs deploy <path_to_solution>
```

Example 2 (unknown):
```unknown
ugs deploy <path_to_solution>
```

Example 3 (unknown):
```unknown
<Target Name="RemoveDirectory">
    <RemoveDir Directories="$(OutDir)Module/" />
 </Target>
<Target Name="PublishModule" DependsOnTargets="RemoveDirectory">
    <Exec Command="dotnet publish -r linux-x64 --no-self-contained -o $(OutDir)Module/$(AssemblyName)" />
</Target>
<Target Name="ZipModule" DependsOnTargets="PublishModule">
    <ZipDirectory SourceDirectory="$(OutDir)Module/$(AssemblyName)" DestinationFile="$(OutDir)Module/$(AssemblyName).ccm" />
</Target>
<Target Name="DeployModule" DependsOnTargets="ZipModule">
    <Exec Command="ugs deploy $(OutDir)/Module/$(AssemblyName).ccm" />
</Target>
```

Example 4 (unknown):
```unknown
<Target Name="RemoveDirectory">
    <RemoveDir Directories="$(OutDir)Module/" />
 </Target>
<Target Name="PublishModule" DependsOnTargets="RemoveDirectory">
    <Exec Command="dotnet publish -r linux-x64 --no-self-contained -o $(OutDir)Module/$(AssemblyName)" />
</Target>
<Target Name="ZipModule" DependsOnTargets="PublishModule">
    <ZipDirectory SourceDirectory="$(OutDir)Module/$(AssemblyName)" DestinationFile="$(OutDir)Module/$(AssemblyName).ccm" />
</Target>
<Target Name="DeployModule" DependsOnTargets="ZipModule">
    <Exec Command="ugs deploy $(OutDir)/Module/$(AssemblyName).ccm" />
</Target>
```

---

## Voice-related logging

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/integration-guide/integration-plan/phase-one/voice-related-logging

**Contents:**
- Voice-related logging#

The following list details the recommended practices for client logging:

Debug/trace level logging, which includes all SDK messages.

The minimum level of logging includes the following data:

Mid-level logging includes the following data:

---

## Use a Google Cloud Storage bucket for a build

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/guides/use-a-gcs-bucket

**Contents:**
- Use a Google Cloud Storage bucket for a build#

You can directly reference cloud storage buckets as a source of game binaries from within the Multiplay Hosting platform. This feature allows you to adopt Multiplay Hosting without disrupting any existing infrastructure, pipelines, or CI/CD workflows that rely on an external cloud storage solution, such as Google Cloud Storage buckets. After linking an external bucket to a Multiplay Hosting project, you can view, manage, and sync the build files from the Unity Dashboard.

Multiplay Hosting supports using Google Cloud Storage buckets as a source for build files. You can upload your build files to a Google Cloud Storage bucket, then create a build version using a snapshot of the current files in that bucket.

Note: The keys you provide for the Google Cloud Storage bucket are encrypted and only used for syncing purposes.

You can upload your build files using a Google Cloud Storage bucket by selecting Google Cloud Storage as the Upload method when you create a build. The following instructions explain the process in more detail.

Note: After you select Finish, Multiplay Hosting begins syncing the build in the background. You can view the sync progress by going to the Builds view and selecting the build by name.

---

## Create a dynamic voice energy meter

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/audio/in-game-audio-control/create-dynamic-voice-energy-meter

**Contents:**
- Create a dynamic voice energy meter#
- Setting a "participant updated" event frequency for a speaking activity#
- Disabling a "participant updated" event frequency for a speaking activity#

Some application features might require a real-time visual representation of audio energy levels, sometimes called a Voice Unit (VU) meter. For example, this might be needed for a microphone check.

By default, the Vivox SDK is set up to support a simple speaking or not-speaking indicator for channel participants that is based on whether voice activity is detected; for that use case, you can iterate over IChannelSession::Participants() and check for SpeechDetected() or handle IChannelSession.EventAfterParticipantUpdated events to be notified when a participant’s speaking state changes. Real-time capability requires one extra step.

To create a dynamic voice activity indicator, call ILoginSession::SetParticipantSpeakingUpdateRate() with a ParticipantSpeakingUpdateRate value of Update1Hz, Update5Hz, or Update10Hz. When set to one of these values, IChannelSession.EventAfterParticipantUpdated events are raised for each participant in each channel up to 1, 5, or 10 times per second, respectively, if the audio energy or other state has changed since the last event.

Due to the sensitivity on most microphones, audio energy fluctuates near continuously unless the participant has fully muted their microphone (by SDK, OS, or hardware), resulting in no audio energy or speaking state changes. Similarly, SetParticipantSpeakingUpdateRate() does not increase the EventAfterParticipantUpdated rate when joined to a channel text-only because energy and speaking state changes do not occur.

Important: It is not possible to increase participant updated event frequency only for specific participants or channels, because the setting controls all of the channels that are connected to by the ILoginSession.

Calling ILoginSession::SetParticipantSpeakingUpdateRate() with a value of ParticipantSpeakingUpdateRate::Never prevents EventAfterParticipantUpdated events from getting raised due to changes in speech detection or changes in audio energy. If you are not displaying any UI related to speaking notifications or energy and do not need to track it for any other reason, this setting eliminates the minimal overhead that the StateChange setting involves. The separate events for a participant joining or leaving a channel continue to occur, and participants continue to get property change events when text or audio is enabled/disabled or when muted or unmuted, but events related to speaking activity do not occur, and the IParticipantProperties values for SpeechDetected() and AudioEnergy() are no longer updated and remain at their last known value.

**Examples:**

Example 1 (unknown):
```unknown
ParticipantSpeakingUpdateRate
```

Example 2 (unknown):
```unknown
SetParticipantSpeakingUpdateRate()
```

Example 3 (unknown):
```unknown
EventAfterParticipantUpdated
```

Example 4 (unknown):
```unknown
ILoginSession
```

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/get-started-with-build-automation/connect-your-version-control-system

---

## Build Automation glossary

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/build-automation-glossary

**Contents:**
- Build Automation glossary#
- General terms#
    - Artifacts#
    - Builder#
    - Builder OS#
    - Build cache#
    - Build configuration#
    - Build job#
    - Build logs#
    - Build manifest#

Find definitions for common terms used in Unity Build Automation (UBA):

The output files that a build process generates, such as executable files, packages, and associated metadata.

A virtual machine that performs build operations in the cloud.

The operating system of a build machine, which determines platform compatibility for builds.

A mechanism that stores build results and reuses unchanged components to speed up subsequent builds.

A collection of settings that define how a project builds, including target platform, build options, and other parameters.

A single build task with a specific configuration that runs on a builder.

A record of events, errors, and outputs that a build process generates.

A file that contains metadata about a build, including dependencies, versions, and settings.

A sequence of automated steps that process code from source to deployable artifacts.

A configuration that specifies the platform and settings for a build.

A limit on the duration of a build job before it automatically cancels.

A build that starts from scratch without the use of cached dependencies or previous build data.

A limit on the maximum number of builds that can run simultaneously. If you exceed the concurrency cap, UBA queues any additional builds.

The expenses associated with using build minutes and resources in Unity Build Automation.

Continuous Integration/Continuous Delivery includes any practices that automate the building, testing, and deployment of applications.

Unity's cloud-based build service that enables you to build projects remotely, previously known as Unity Cloud Build.

The ability to run multiple build jobs simultaneously to improve build efficiency.

The process when you publish a built project to a distribution platform or production environment.

A build with debugging features enabled, designed for testing rather than final distribution.

A process where you store and reuse project dependencies to improve build performance.

Variables available in the build environment that can influence the build process or provide configuration information.

A build process that recompiles only changed parts of the project to speed up build times.

A caching strategy that stores only the Unity Library folder to reduce build times for subsequent builds.

The hardware configuration of a builder, which affects build performance and expenses.

Custom scripts that run before or after the build process to automate additional tasks.

Custom code methods that execute before or after the Unity Editor exports a project.

A caching strategy that stores the entire project workspace to minimize build times for subsequent builds.

Intermediate Language to C++ Unity's scripting back end that converts IL code to C++ for better performance and platform compatibility.

The technology that runs scripts in the Unity Editor, such as Mono or IL2CPP.

Custom compiler directives that enable conditional compilation in Unity scripts.

A setting that determines how much unused code you want to remove from a build to reduce size.

A Unity system for managing and loading assets efficiently, which you can configure in Build Automation.

The final executable build of a Unity project that you can distribute to users.

A unique identifier for a Unity project within Build Automation services.

A file you use iOS development that contains certificates, device identifiers, and app identifiers.

A digital certificate you use to sign applications, which ensures they come from a trusted source.

The platform (such as iOS, Android, Windows) that you create the build for.

A collection of assets, scripts, and settings that make up a Unity application.

The specific version of the Unity Editor that a build uses.

A system that tracks changes to files over time, which allows multiple developers to collaborate.

A separate line of development in a version control system.

A saved set of changes in version control.

A storage location for a project's versioned files and history.

---

## Redirect log output for games using Unreal

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/guides/redirect-logs-for-unreal-games

**Contents:**
- Redirect log output for games using Unreal#

Multiplay Hosting expects your game to output logs to a log directory directly using the $$log_dir$$ configuration variable.

However, games using the Unreal Engine and the -Userdir launch parameter output logs to a log directory nested within $$log_dir$$/logs/Saved/Logs/ with Unreal creating the recursive directories.

Multiplay Hosting doesn't yet support traversing more than one nested log directory. To work around this, use the -LOG launch parameter to redirect to a specific file:

This causes the Unreal Engine to write to the correct directory and name the log after the specific server ID.

Note: Multiplay Hosting doesn't currently support file extensions other than .log.

**Examples:**

Example 1 (unknown):
```unknown
$$log_dir$$
```

Example 2 (unknown):
```unknown
$$log_dir$$/logs/Saved/Logs/
```

Example 3 (unknown):
```unknown
-LOG=../../../../$$log_dir$$/$$serverid$$.log
```

Example 4 (unknown):
```unknown
-LOG=../../../../$$log_dir$$/$$serverid$$.log
```

---

## Account chat history for blocked participants

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/chat-history/account-ch-blocked-participant

**Contents:**
- Account chat history for blocked participants#

When you submit a chat history request for an account, queries only show the user’s sent messages.

---

## In-game moderation

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/safety/in-game-moderation-toc

**Contents:**
- In-game moderation#
- Vivox Safe Voice#

The Unity Moderation Platform makes toxicity management accessible, impactful, and insightful by providing you with the tools you need to grow and maintain healthy communities within your games.

By using the Moderation SDK for Unreal Engine you can record voice and text conversation as evidence to support player reports. These reports are then made available within the Unity Dashboard for review and actioning.

Vivox Safe Voice is an AI-based voice analytics service that:

Recordings provide additional context to moderation teams around this currently undersupported aspect of communications. When a user identifies an incident, they can report a player; Safe Voice then records the event and returns an analysis of the recording. This analysis generates recorded evidence for the support team to review and use in decision-making. This evidence helps teams prioritize incidents and identify whether the user violated policies.

Using Safe Voice with the Vivox Unreal API requires the use of the Safe Voice Consent API.

---

## Vivox Client SDK logs

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/troubleshooting/vivox-client-sdk-logs

**Contents:**
- Vivox Client SDK logs#

The Vivox SDK produces log information that is useful for diagnosing issues both during development and in the field. We recommend that you include Vivox log information in the application logs that are sent to Vivox.

The game client can get Vivox log information through a callback that is set during vx_initialize3().

The vx_log_level enum includes the following values:

The following code displays an example of how to get Vivox SDK log information:

Note: The pf_logging_callback method is called directly from Vivox SDK threads. This callback must not block, otherwise, the user's audio experience could be negatively impacted. Applications that use pf_logging_called must call it immediately after initializing the Vivox SDK.

**Examples:**

Example 1 (unknown):
```unknown
vx_initialize3()
```

Example 2 (unknown):
```unknown
vx_log_level
```

Example 3 (unknown):
```unknown
log_none = -1
```

Example 4 (unknown):
```unknown
log_error = 0
```

---

## VAD adjustments

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/troubleshooting/vad-adjustments-core

**Contents:**
- VAD adjustments#
  - Setting the VAD Hangover, Sensitivity, and Noise Floor#

To make adjustments to the VAD settings, use:

In this example, the default values for the vad_hangover, vad_sensitivity, and vad_noise_floor are provided, and vad_auto has been disabled.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_aux_set_vad_properties
```

Example 2 (unknown):
```unknown
vx_req_aux_get_vad_properties
```

Example 3 (unknown):
```unknown
vad_hangover
```

Example 4 (unknown):
```unknown
vad_sensitivity
```

---

## User-to-user muting in a specified channel

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/muting/mute-specified-channel

**Contents:**
- User-to-user muting in a specified channel#

To mute a user in a specified channel, use the vx_req_session_set_participant_mute_for_me request.

Note: This is only valid for a currently connected channel.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_session_set_participant_mute_for_me
```

Example 2 (unknown):
```unknown
vx_req_session_set_participant_mute_for_me_t* reqStruct;
vx_req_session_set_participant_mute_for_me_create(&reqStruct);
reqStruct->session_handle = vx_strdup(“session handle”);
reqStruct->participant_uri = vx_strdup(“sip:.participant.@realm.vivox.com”); // Use SIP URI format
reqStruct->mute = 1; // 1 - mute, 0 - unmutereqStruct->
reqStruct->scope = mute_scope_all;

int ret = vx_issue_request3((vx_req_base_t*) reqStruct);
```

Example 3 (unknown):
```unknown
vx_req_session_set_participant_mute_for_me_t* reqStruct;
vx_req_session_set_participant_mute_for_me_create(&reqStruct);
reqStruct->session_handle = vx_strdup(“session handle”);
reqStruct->participant_uri = vx_strdup(“sip:.participant.@realm.vivox.com”); // Use SIP URI format
reqStruct->mute = 1; // 1 - mute, 0 - unmutereqStruct->
reqStruct->scope = mute_scope_all;

int ret = vx_issue_request3((vx_req_base_t*) reqStruct);
```

---

## Supported platforms and versions

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/supported-platforms

**Contents:**
- Supported platforms and versions#
- Terminology#
  - Supported versions#
  - Limited versions#
  - Experimental versions#
  - Deprecated versions#

Experimental is for the latest/edge releases that are available but not yet Supported or default. Use at your own risk. This encompasses all Alpha, Beta and Prerelease versions. Issues and improvement requests will be tracked and addressed based on frequency and severity.

Supported is known-good and actively supported by the organization. Supported versions are the default choice when creating a new configuration and represent the versions we have the highest confidence in. We recommend anyone experiencing issues upgrade/migrate to Supported versions if you are using an SDK version where a platform wasn’t fully supported. Issues and improvement requests will be tracked and addressed based on frequency and severity.

Limited indicates that the platform does not have full support from our team. While we will address security issues and critical bugs on the platform, the priority of implementing platform-specific fixes and improvements is low. We do still include the platform in our releases.

Deprecated means "do not use". This technology is scheduled to be removed as part of the deprecation schedule strategy. Deprecated also means that the Unity Editor team no longer supports the version as it has fallen out of Long Term Support (LTS). Contact us if that causes problems for your company, and we can negotiate to make that version available longer. Issues and improvement requests will not be tracked or addressed.

The Vivox SDK is available for the following platforms and versions:

Note: Access to some platform-specific Vivox SDKs and documentation requires approval. After your account is approved, you access the platform-specific SDKs and documentation from the Unity Cloud Dashboard. For more information, refer to How do I get approved for NDA protected platforms? in the Unity Support knowledge base.

---

## Mute a local speaker device

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/muting/mute-local-speaker

**Contents:**
- Mute a local speaker device#

To mute or unmute a local speaker device, use the vx_req_connector_mute_local_speaker request.

The account_handle in this request is an optional parameter. Generally, there is only one account handle. If a speaker device is shared between multiple users (for example, in couch co-op), then the account handle specifies the account handle of the user whose speaker device is to be muted or unmuted. This is the account handle that is passed in to a vx_req_account_anonymous_login. If this is left unset, then the default account_handle is used.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_connector_mute_local_speaker
```

Example 2 (unknown):
```unknown
vx_req_connector_mute_local_speaker_t* reqStruct;
vx_req_connector_mute_local_speaker_create(&reqStruct);
reqStruct->connector_handle = vx_strdup(“connector handle”);
reqStruct->mute_level = 1; // 1 - mute, 0 - unmute
reqStruct->account_handle = vx_strdup(“account handle”); // OPTIONAL

int ret = vx_issue_request3((vx_req_base_t*) reqStruct);
```

Example 3 (unknown):
```unknown
vx_req_connector_mute_local_speaker_t* reqStruct;
vx_req_connector_mute_local_speaker_create(&reqStruct);
reqStruct->connector_handle = vx_strdup(“connector handle”);
reqStruct->mute_level = 1; // 1 - mute, 0 - unmute
reqStruct->account_handle = vx_strdup(“account handle”); // OPTIONAL

int ret = vx_issue_request3((vx_req_base_t*) reqStruct);
```

Example 4 (unknown):
```unknown
account_handle
```

---

## Directed text

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/messaging/directed-text

**Contents:**
- Directed text#

Vivox provides directed text by using a similar process to what is used for group text. You can add players to existing text sessions, and you can send text messages directly to any user who is signed in, independent of being in a channel with them.

Use directed text for short user-to-user messages (sometimes called whispers or private/direct messages), or to implement invite to channel capabilities.

Note: To receive directed text messages, a user must be signed in with enable_buddies_and_presence = 1. For more information about Buddies and Presence, see Presence capabilities.

Use the vx_req_account_send_msg request to send a text message to another user.

Note: login_state_logged_in indicates that a user is signed in and can issue further requests, but it does not guarantee that the user can receive direct text messages. A best practice is using the vx_evt_presence_updated event to indicate when the local participant is available to receive direct text messages.

Using vx_evt_presence_updated is not mandatory. However, in testing scenarios where there could be more than one user signed into the local SDK messaging each other, vx_evt_presence_updated guarantees the availability of receiving a directed message.

The following code displays an example of this process:

When a user-to-user text message is available, the vx_evt_user_to_user_message event is sent to the game.

The following code displays an example of this action:

**Examples:**

Example 1 (unknown):
```unknown
enable_buddies_and_presence = 1
```

Example 2 (unknown):
```unknown
vx_req_account_send_msg
```

Example 3 (unknown):
```unknown
login_state_logged_in
```

Example 4 (unknown):
```unknown
vx_evt_presence_updated
```

---

## Example: Drop_All token

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/access-token-examples/example-drop-all-token

**Contents:**
- Example: Drop_All token#

The token in this example allows the user "blindmelon-AppName-dev-Admin" to drop all users from the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Because this is a kick command, the vxa is "kick".

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

**Examples:**

Example 1 (unknown):
```unknown
{
    "vxi":729614,
    "f":"sip:blindmelon-AppName-dev-Admin@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"kick",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 2 (unknown):
```unknown
{
    "vxi":729614,
    "f":"sip:blindmelon-AppName-dev-Admin@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"kick",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 3 (unknown):
```unknown
e30.eyJ2eGkiOjcyOTYxNCwiZiI6InNpcDpibGluZG1lbG9uLUFwcE5hbWUtZGV2LUFkbWluQHRsYS52aXZveC5jb20iLCJpc3MiOiJibGluZG1lbG9uLUFwcE5hbWUtZGV2IiwidnhhIjoia2ljayIsInQiOiJzaXA6Y29uZmN0bC1nLWJsaW5kbWVsb24tQXBwTmFtZS1kZXYudGVzdGNoYW5uZWxAdGxhLnZpdm94LmNvbSIsImV4cCI6MTYwMDM0OTQwMH0.bvNDTcXIHRpckAiFzy3wPiwgYGks4E9_WuEA5HMGpGE
```

Example 4 (unknown):
```unknown
e30.eyJ2eGkiOjcyOTYxNCwiZiI6InNpcDpibGluZG1lbG9uLUFwcE5hbWUtZGV2LUFkbWluQHRsYS52aXZveC5jb20iLCJpc3MiOiJibGluZG1lbG9uLUFwcE5hbWUtZGV2IiwidnhhIjoia2ljayIsInQiOiJzaXA6Y29uZmN0bC1nLWJsaW5kbWVsb24tQXBwTmFtZS1kZXYudGVzdGNoYW5uZWxAdGxhLnZpdm94LmNvbSIsImV4cCI6MTYwMDM0OTQwMH0.bvNDTcXIHRpckAiFzy3wPiwgYGks4E9_WuEA5HMGpGE
```

---

## Unrecognized project error

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/check-build-results/troubleshoot-build-failures/unrecognized-project-error

**Contents:**
- Unrecognized project error#
- Symptom#
- Cause#
- Resolution#
- Additional resource#

Your build fails with the following error:

Unity Build Automation only supports builds of Unity Projects. The most common cause for this error is when the ProjectSettings and Assets folders are contained within a subdirectory of the folder targeted by Unity Build Automation in your source code repository, or your repository.

If your project is a Unity Project but is in a subdirectory, you can create a new target and define the name of the Project subdirectory path. This tells Unity Build Automation to look in this subdirectory for the project it needs to build.

To change the target in your Unity Dashboard:

**Examples:**

Example 1 (unknown):
```unknown
[error] Error: unrecognized project! Please check your app configuration - if this is a Unity application, We expect your "Project Subdirectory" to be set to the path which directly contains the ProjectSettings and Assets directories. For a native app, this should be set to the path which directly contains the project file (.xcodeproj, project.properties, etc).
```

Example 2 (unknown):
```unknown
[error] Error: unrecognized project! Please check your app configuration - if this is a Unity application, We expect your "Project Subdirectory" to be set to the path which directly contains the ProjectSettings and Assets directories. For a native app, this should be set to the path which directly contains the project file (.xcodeproj, project.properties, etc).
```

Example 3 (unknown):
```unknown
ProjectSettings
```

---

## Directed messages

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/messaging/directed-messages

**Contents:**
- Directed messages#
- Send directed messages#
- Receive directed messages#
- Direct text message history#
- Editing direct messages#
- Deleting direct messages#

Vivox provides directed text by using a similar process to what is used for group text. You can add players to existing text sessions, and you can send text messages directly to any user who is signed in, independent of being in a channel with them.

Use directed text for short user-to-user messages, sometimes called whispers or private/direct messages, or to implement invite to channel capabilities.

You can send a message to a player by using VivoxService.Instance.SendDirectTextMessageAsync(string playerId, string message) with playerId being the playerId of the player that the message should be sent to, and message being the message that should be sent.

The following code snippet is an example implementation for sending a message to a player passed in from a managed channel roster, with the message being pulled from a dedicated message InputField.

In order to receive messages from other users, the VivoxService.Instance.DirectedMessageReceived event must be subscribed to.

Directed VivoxMessages are nearly identical to channel messages, with the caveat that the VivoxMessage.ChannelName will be set to null, and the VivoxMessage.FromSelf will be set to false.

The following code snippet is an example line of subscribing to the event, along with an example of pulling all available information out of the VivoxMessage on receipt.

Vivox also allows for users to access the history of a direct message conversation between two players using VivoxService.Instance.GetDirectTextMessageHistoryAsync(string playerId, int requestSize = 10, ChatHistoryQueryOptions chatHistoryQueryOptions = null). The playerId being the ID of the player to request the history of, requestSize being the number of messages to return at maximum (10 is the default), and chatHistoryQueryOptions being an optional parameter which can do things like specifying a word or phrase contained in the messages, or times to query before or after. More specific information on ChatHistoryQueryOptions can be found on the Chat History query options page.

The messages returned in IReadOnlyCollection are listed in order from newest message to oldest message.

The following code snippet is an example of pulling the most recent 25 messages from a set LobbyChannelName, then logging the sender's display name and the message in order from oldest to newest, without using the optional ChatHistoryQueryOptions.

Vivox allows for users to edit the text of messages they have sent into a channel using VivoxService.Instance.EditDirectTextMessageAsync(string messageId, string newMessage) with the messageId being the ID of the message to change, and newMessage being the updated text of the message.

When a message is successfully edited, logged in players associated with the direct message will receive a VivoxService.Instance.DirectedMessageEdited Action containing the VivoxMessage that was edited, with the updated MessageText.

Vivox allows for users to delete direct messages they have sent using VivoxService.Instance.DeleteDirectTextMessageAsync(string messageId) with messageId being the ID of the message to delete.

When a message is successfully deleted, logged in players associated with the directed message will receive a VivoxService.Instance.DirectedMessageDeleted Action containing the VivoxMessage that was deleted.

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.SendDirectTextMessageAsync(string playerId, string message)
```

Example 2 (unknown):
```unknown
void SendMessageAsync(string playerId)
{
    if (string.IsNullOrEmpty(MessageInputField.text))
    {
        return;
    }

    VivoxService.Instance.SendDirectTextMessageAsync(playerId, MessageInputField.text);
    MessageInputField.text = string.Empty;
}
```

Example 3 (unknown):
```unknown
void SendMessageAsync(string playerId)
{
    if (string.IsNullOrEmpty(MessageInputField.text))
    {
        return;
    }

    VivoxService.Instance.SendDirectTextMessageAsync(playerId, MessageInputField.text);
    MessageInputField.text = string.Empty;
}
```

Example 4 (unknown):
```unknown
VivoxService.Instance.DirectedMessageReceived
```

---

## Problems with iOS Build Automation during XCode export

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/check-build-results/troubleshoot-build-failures/xcode-export-issues

**Contents:**
- Problems with iOS Build Automation during XCode export#
- Symptom#
- Causes#
- Resolution#
- Additional resources#

Build errors appear in the Xcode project when you build your iOS project in Unity Build Automation (UBA).

Most Xcode build failures in UBA are related to third party plug-ins, specifically those that require a minimum version of Xcode or require that you disable bitcode.

Build errors can also occur if the post process can't find the Xcode project and can't add the appropriate libraries.

Ensure you can build your project locally in the desired version of Xcode with the same settings: don't modify the Xcode project unless you do the same modifications in the post-export script in UBA.

If the post process can't find the Xcode project, you need to pass that path to OnPostProcessBuild. Some third party plug-ins, or your own code, require a way to modify the XCode project to add libraries or files, which you can use post-process attributes and the Scripting API to do.

There are several Unity Discussions forum posts available, which can help to solve different issues related to iOS projects:

You can also refer to the following pages:

**Examples:**

Example 1 (unknown):
```unknown
OnPostProcessBuild
```

---

## Kick a user from a channel

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/kick-user

**Contents:**
- Kick a user from a channel#

You can kick a user from a channel by using the vx_req_channel_kick_user message.

Note: This request requires an access token.

For more information, refer to the Access Token Developer Guide.

If the game server is directly communicating with Vivox, you can instead have the game server kick the user through the Vivox Server to Server Web API. However, if the game server does not directly communicate with Vivox, then return an access token to the game client that allows the game client to perform this operation.

You can view example code for all of these requests and responses in the SDKSampleApp.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_channel_kick_user
```

---

## Access token header

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/access-token-guide/access-token-format/access-token-header

**Contents:**
- Access token header#

The header in a Vivox access token is an empty JSON object (for example, {}).

Unlike a true JSON Web Token (JWT), this header is empty and not {"'alg":"HS256","typ":"JWT"}.

**Examples:**

Example 1 (unknown):
```unknown
{"'alg":"HS256","typ":"JWT"}
```

---

## Create a build with a container image

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/guides/create-a-build-container

**Contents:**
- Create a build with a container image#

Refer to Container builds to learn more.

In the Unity Dashboard, open Multiplay Hosting.

Select Builds > Create build.

Fill in the build details:

Select Next to continue to the Service account step.

To add your container image to the Multiplay Hosting registry, you’ll need a service account. Follow the below instructions:

Select Next to continue to the Add to registry step.

Complete the following instructions to add your container image to the Multiplay Hosting registry. Note that the Create build window provides you with the required IDs.

Select Next to continue to the Select image step.

Choose the method by which you want to select your image.

You now have a new build. Before you can use the build, you need to create (and link) a build configuration and a fleet. Refer to Create a build configuration.

**Examples:**

Example 1 (unknown):
```unknown
docker login registry.multiplay.com -u <KeyID> -p <SecretKey>
```

Example 2 (unknown):
```unknown
docker tag <ImageName>:<ImageTag> registry.multiplay.com/<project_id>/<environment_id>/<build_id>:<version>
```

Example 3 (unknown):
```unknown
docker push registry.multiplay.com/<project_id>/<environment_id>/<build_id>:<version>
```

---

## Dependency injection

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/initialize-modules/dependency-injection

**Contents:**
- Dependency injection#
- Dependency injection setup#
- Dependency injection usage#

Cloud Code has support for dependency injection (DI), which allows you to decouple hard-coded dependencies in your code and make it easier for unit testing. The DI here is based on the similar system created by .Net.

Cloud Code offers support for the following dependency types:

In order to set up your dependencies, you must create a class that implements the ICloudCodeSetup interface.

The following example defines a singleton interface and its implementation, but you can also use a scoped or transient dependency by using either AddScoped or AddTransient.

To support dependency injection, attribute CloudCodeFunction to your methods along with their class constructors, as the following example demonstrates.

Warning: While the singleton is retained between multiple requests, it is still possible that it's different between requests because Cloud Code scales to multiple pods when needed in order to meet demand.

Refer to Microsoft's documentation on dependency injection usage.

**Examples:**

Example 1 (unknown):
```unknown
ICloudCodeSetup
```

Example 2 (unknown):
```unknown
AddTransient
```

Example 3 (unknown):
```unknown
using Unity.Services.CloudCode.Core;

namespace ExampleModule;

public interface IRandomNumber
{
    public int GetRandomNumber();
}

public class LockedRandomNumber : IRandomNumber
{
    private int randomNumber;

    public LockedRandomNumber()
    {
        randomNumber = Random.Shared.Next();
    }

    public int GetRandomNumber()
    {
        return randomNumber;
    }
}

public class ModuleConfig : ICloudCodeSetup
{
    public void Setup(ICloudCodeConfig config)
    {
        config.Dependencies.AddSingleton<IRandomNumber, LockedRandomNumber>();
    }
}
```

Example 4 (unknown):
```unknown
using Unity.Services.CloudCode.Core;

namespace ExampleModule;

public interface IRandomNumber
{
    public int GetRandomNumber();
}

public class LockedRandomNumber : IRandomNumber
{
    private int randomNumber;

    public LockedRandomNumber()
    {
        randomNumber = Random.Shared.Next();
    }

    public int GetRandomNumber()
    {
        return randomNumber;
    }
}

public class ModuleConfig : ICloudCodeSetup
{
    public void Setup(ICloudCodeConfig config)
    {
        config.Dependencies.AddSingleton<IRandomNumber, LockedRandomNumber>();
    }
}
```

---

## Presence capabilities

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/presence/presence-capabilities

**Contents:**
- Presence capabilities#

To use presence capabilities, set the following fields in the vx_req_account_anonymous_login message.

Note: Vivox does not store the list of buddies, blocked users, and allowed users. Every time a user signs in, pass the existing list of buddies, blocked users, and allowed users. You can generate the Vivox user URIs for these lists with vx_get_user_uri() by using your connection information and the account name that is used by the users when they sign in to Vivox.

The following code displays an example of presence-specific sign in behavior:

**Examples:**

Example 1 (javascript):
```javascript
. . .
char **vectorToList(const vector<string> &items)
{
    if (items.empty()) return nullptr;
    char **list = static_cast<char **>(vx_allocate(sizeof(char*) * (items.size() + 1)));
    for (size_t i = 0; i < items.size(); ++i)
    {
        list[i] = vx_strdup(items.at(i).c_str());
    }
    list[items.size()] = 0;
    return list;
}
void (MyLogin (const string &account, const vector<string> &buddies, const vector<string> &blocked, const vector<string> &blockedpresence, const vector<string> &allowed)
{
    vx_req_account_anonymous_login *req;
    vx_req_account_anonymous_login_create(&req);
    req->connector_handle = vx_strdup("c1");
    req->account_name = vx_strdup(".issuer-w-dev.mytestaccountname.");
    req->account_handle = vx_strdup(req->account_name);
    req->access_token = vx_strdup(_the_access_token_generated_by_game_server);
    req->enable_buddies_and_presence = 1;
    req->buddy_management_mode = mode_application;
    req->initial_buddy_uris = vectorToList(buddies);
    req->initial_blocked_uris = vectorToList(blocked);
    req->initial_blocked_uris_presence_only = vectorToList(blockedpresence);
    req->initial_allowed_uris = vectorToList(allowed);
    vx_issue_request3(&req->base, &request_count);
}
. . .
```

Example 2 (javascript):
```javascript
. . .
char **vectorToList(const vector<string> &items)
{
    if (items.empty()) return nullptr;
    char **list = static_cast<char **>(vx_allocate(sizeof(char*) * (items.size() + 1)));
    for (size_t i = 0; i < items.size(); ++i)
    {
        list[i] = vx_strdup(items.at(i).c_str());
    }
    list[items.size()] = 0;
    return list;
}
void (MyLogin (const string &account, const vector<string> &buddies, const vector<string> &blocked, const vector<string> &blockedpresence, const vector<string> &allowed)
{
    vx_req_account_anonymous_login *req;
    vx_req_account_anonymous_login_create(&req);
    req->connector_handle = vx_strdup("c1");
    req->account_name = vx_strdup(".issuer-w-dev.mytestaccountname.");
    req->account_handle = vx_strdup(req->account_name);
    req->access_token = vx_strdup(_the_access_token_generated_by_game_server);
    req->enable_buddies_and_presence = 1;
    req->buddy_management_mode = mode_application;
    req->initial_buddy_uris = vectorToList(buddies);
    req->initial_blocked_uris = vectorToList(blocked);
    req->initial_blocked_uris_presence_only = vectorToList(blockedpresence);
    req->initial_allowed_uris = vectorToList(allowed);
    vx_issue_request3(&req->base, &request_count);
}
. . .
```

---

## Merge multiple Unreal plug-in packages

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/implement-vivox-unreal/merge-multiple-unreal-plugin-packages

**Contents:**
- Merge multiple Unreal plug-in packages#
- Example: Targeting PlayStation 4 and Xbox One#

If you are working with multiple platforms, you might need to merge several Unreal plug-in packages before you can add it to your project or to all projects.

Note: The Vivox Core Unreal packages contain both the target platform and the editor platform. The editor platform can also serve as an additional target platform.

Download all required target platform packages from the Unity Dashboard.

Choose one package to be your primary plug-in package.

In one of the non-primary packages, navigate to vivox-unreal4-sdk-\<version>\VivoxCore\Source\ThirdParty\VivoxCoreLibrary. Copy the folder with the platform name that you want to use into your primary package at the same location.

In the vivox-unreal4-sdk-\<version>\VivoxCore directory of your primary package, locate the VivoxCore.uplugin file. Update the WhitelistPlatforms section of this file to include the names of all of the platforms that your product will support. Check the VivoxCore.uplugin of the non-primary package that you are migrating libraries from to know which platform names to use.

Note: The following example uses PlayStation 4 as the primary plug-in package.

Download the PlayStation 4 package and the Xbox One package.

In the Xbox One package, navigate to vivox-unreal4-sdk-\<version>\VivoxCore\Source\ThirdParty\VivoxCoreLibrary. Copy the XB1 folder to vivox-unreal4-sdk-\<version>\VivoxCore\Source\ThirdParty\VivoxCoreLibrary in the PlayStation 4 package.

This results in vivox-unreal4-sdk-\<version>\VivoxCore\Source\ThirdParty\VivoxCoreLibrary in the PlayStation 4 package containing the following folders: PlayStation4, Windows, and XB1.

In the vivox-unreal4-sdk-\<version>\VivoxCore directory of the PlayStation 4 package, edit the WhitelistPlatforms section of the VivoxCore.uplugin file to include "Win64", "PS4", "XboxOne", (including the trailing comma).

**Examples:**

Example 1 (unknown):
```unknown
vivox-unreal4-sdk-\<version>\VivoxCore\Source\ThirdParty\VivoxCoreLibrary
```

Example 2 (unknown):
```unknown
vivox-unreal4-sdk-\<version>\VivoxCore
```

Example 3 (unknown):
```unknown
VivoxCore.uplugin
```

Example 4 (unknown):
```unknown
WhitelistPlatforms
```

---

## Restore audio volume

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/ios/restore-audio-volume

**Contents:**
- Restore audio volume#

If the device is in a state in which the game audio is at the wrong volume when the Vivox SDK is using the microphone, you can restore the correct game audio volume by reconfiguring the AVAudioSession with:

Note: There is a brief interruption to the device audio during the restoration process.

---

## The iOS microphone recording indicator

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/ios/ios-recording-indicator

**Contents:**
- The iOS microphone recording indicator#

iOS displays the red bar/orange dot recording indicator while the microphone is in use. When an iOS device is muted by using IAudioDevices::SetMuted(bool value), the Vivox SDK accesses the capture device (microphone) but does not collect or transmit any audio. This results in the recording indicator displaying.

To hard mute and prevent the recording indicator from displaying, set the capture device to IAudioDevices::NullDevice():

To unmute, set the capture device to IAudioDevices::CommunicationDevice():

Note: Hard muting or unmuting causes a brief interruption to all audio that is playing from the device.

**Examples:**

Example 1 (javascript):
```javascript
// Assumes an IClient *MyClient;
const IAudioDevice &NewActive = MyClient->AudioInputDevices().NullDevice();
IAudioDevices::FOnSetActiveDeviceCompletedDelegate SetActiveDeviceCompletedCallback;
SetActiveDeviceCompletedCallback.BindLambda([](VivoxCoreError Status, const FString DeviceId)
{
	// DeviceId should match "No Device"
	UE_LOG(LogTemp, Log, TEXT("SetActiveDevice [Input] Completed (%d): %s"), Status, *DeviceId);
});
MyClient->AudioInputDevices().SetActiveDevice(NewActive, SetActiveDeviceCompletedCallback);
```

Example 2 (javascript):
```javascript
// Assumes an IClient *MyClient;
const IAudioDevice &NewActive = MyClient->AudioInputDevices().NullDevice();
IAudioDevices::FOnSetActiveDeviceCompletedDelegate SetActiveDeviceCompletedCallback;
SetActiveDeviceCompletedCallback.BindLambda([](VivoxCoreError Status, const FString DeviceId)
{
	// DeviceId should match "No Device"
	UE_LOG(LogTemp, Log, TEXT("SetActiveDevice [Input] Completed (%d): %s"), Status, *DeviceId);
});
MyClient->AudioInputDevices().SetActiveDevice(NewActive, SetActiveDeviceCompletedCallback);
```

Example 3 (javascript):
```javascript
// Assumes an IClient *MyClient;
const IAudioDevice &NewActive = MyClient->AudioInputDevices().CommunicationDevice();
IAudioDevices::FOnSetActiveDeviceCompletedDelegate SetActiveDeviceCompletedCallback;
SetActiveDeviceCompletedCallback.BindLambda([](VivoxCoreError Status, const FString DeviceId)
{
	// DeviceId should match "Default Communication Device"
	UE_LOG(LogTemp, Log, TEXT("SetActiveDevice [Input] Completed (%d): %s"), Status, *DeviceId);
});
MyClient->AudioInputDevices().SetActiveDevice(NewActive, SetActiveDeviceCompletedCallback);
```

Example 4 (javascript):
```javascript
// Assumes an IClient *MyClient;
const IAudioDevice &NewActive = MyClient->AudioInputDevices().CommunicationDevice();
IAudioDevices::FOnSetActiveDeviceCompletedDelegate SetActiveDeviceCompletedCallback;
SetActiveDeviceCompletedCallback.BindLambda([](VivoxCoreError Status, const FString DeviceId)
{
	// DeviceId should match "Default Communication Device"
	UE_LOG(LogTemp, Log, TEXT("SetActiveDevice [Input] Completed (%d): %s"), Status, *DeviceId);
});
MyClient->AudioInputDevices().SetActiveDevice(NewActive, SetActiveDeviceCompletedCallback);
```

---

## Bluetooth tradeoffs with Vivox

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/android/bt-tradeoffs-with-vivox

**Contents:**
- Bluetooth tradeoffs with Vivox#

Vivox provides the option to opt-out of Bluetooth SCO altogether, and use Bluetooth A2DP instead.

If you use vx_bluetooth_profile_hfp (the default), and the Bluetooth SCO connection fails, the device will route all audio (render and capture) to the speakerphone.

Note: If a Bluetooth device does not have SCO but does have A2DP, then A2DP should still work because it would not be possible to failover from an SCO-connect attempt into speakerphone mode. It is possible to individually disable Bluetooth SCO for devices in the Android Bluetooth settings.

If you use vx_bluetooth_profile_a2dp, then voice and game audio will almost always go through a Bluetooth headset when one is connected. One exception is for headsets without high-quality A2DP audio (like older-style single-ear handsfree headsets). Those older devices will then behave similarly to when an SCO connection fails, so audio will route to the handset speakerphone.

**Examples:**

Example 1 (unknown):
```unknown
vx_bluetooth_profile_hfp
```

Example 2 (unknown):
```unknown
vx_bluetooth_profile_a2dp
```

---

## Phase 1: Planning and organization

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/integration-guide/integration-plan/phase-one/planning-and-organization-overview

**Contents:**
- Phase 1: Planning and organization#

In phase one, you make determinations about the general structure and organization of in-game voice-related data and identify code paths for handling necessary actions such as signing in and joining or leaving channels.

During the planning phase, you must make decisions about the following topics:

The tasks that make up this phase of the integration are split between the game server, for authorization and server-enacted operations, and the game client, for user activity and UI integration.

---

## Build the Chat Channel Sample

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/chat-channel-sample/set-up-environment

**Contents:**
- Build the Chat Channel Sample#

Prerequisite: Import the Chat Channel Sample.

To set up an environment for building the Chat Channel Sample, complete the following steps:

Open your Unity project.

Select Edit > Project Settings. Select Vivox and update the Server, Domain, Token issuer, and Token key fields with the credentials for your application.

For information on generating credentials, refer to Unity Package Manager Vivox package.

Note: If a project is bound to the Services page, then you can change the Environment menu to Automatic to automatically pull the project’s credentials from the Unity Dashboard.

Add the MainScene from .../Samples/Vivox/{Version}/Chat Channel Sample/ to the Build Settings.

**Examples:**

Example 1 (unknown):
```unknown
.../Samples/Vivox/{Version}/Chat Channel Sample/
```

---

## Android SDK permission requirements

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/android/android-sdk-permission-requirements

**Contents:**
- Android SDK permission requirements#

The following permission requirements apply when working with the Vivox Android SDK:

android.permission.INTERNET - Allows communication with Vivox servers

android.permission.RECORD_AUDIO - Allows microphone access

For more information, refer to the Android developer documentation on requesting app permissions.

Note: Not giving these permissions might prevent some Vivox SDK features from working properly.

android.permission.MODIFY_AUDIO_SETTINGS

android.permission.ACCESS_NETWORK_STATE - Allows access to network information

android.permission.ACCESS_WIFI_STATE - Allows access to Wi-Fi information

android.permission.BLUETOOTH - Allows access to Bluetooth devices on Android versions earlier than 12

android.permission.BLUETOOTH_CONNECT - Allows access to Bluetooth devices on Android version 12 and later

Note: On Android 12 (Android SDK version 31) and later, you need to request the android.permission.BLUETOOTH_CONNECT permission at runtime. For more information, refer to the Android developer documentation on Bluetooth permissions.

The Vivox SDK plugin includes a UPL (Unreal Plugin Language) file, which complements the permissions that are defined by your project. Use this file to add or remove permissions.

The UPL file is available at the following location:

Your_Project_Path/Plugins/VivoxCore/Source/ThirdParty/VivoxCoreLibrary

The UPL file includes the following permissions:

Note: These permissions are automatically applied to your application after you have integrated the Vivox plugin.

When working with the UPL file, consider the following use cases:

You have a permission defined in your project and you want to keep it.

You need to define more permissions.

In this scenario, you can add permissions as necessary to the UPL file.

For example, if you need access to the network information, add the following permission to the UPL file:

<addPermission android:name="android.permission.ACCESS\_NETWORK\_STATE" />

You have a permission defined in your project and you want to remove it.

In this scenario, you can remove the permission from your manifest file.

If the manifest file is automatically generated, use the UPL file to remove the permission. For example, if you have the Bluetooth permission defined in your manifest file, remove the following permission with the UPL file:

<removePermission android:name="android.permission.BLUETOOTH "/>

**Examples:**

Example 1 (unknown):
```unknown
android.permission.INTERNET
```

Example 2 (unknown):
```unknown
android.permission.RECORD_AUDIO
```

Example 3 (unknown):
```unknown
android.permission.MODIFY_AUDIO_SETTINGS
```

Example 4 (unknown):
```unknown
android.permission.ACCESS_NETWORK_STATE
```

---

## Shut down text-to-speech

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/text-to-speech/shut-down-tts

**Contents:**
- Shut down text-to-speech#

When you are finished using text-to-speech (TTS) functionality, we recommend that you shut down the TTS manager. This removes all ongoing and enqueued messages, and safely releases all memory associated with TTS objects (other than vx_tts_utterance_t structs, which you must still explicitly destroy).

Note: After you shut down a TTS manager, its unique identifier becomes invalid, and a new identifier must be generated by using vx_tts_initialize before you can use TTS again.

**Examples:**

Example 1 (unknown):
```unknown
vx_tts_utterance_t
```

Example 2 (unknown):
```unknown
vx_tts_status status = vx_tts_shutdown(&managerId);
```

Example 3 (unknown):
```unknown
vx_tts_status status = vx_tts_shutdown(&managerId);
```

Example 4 (unknown):
```unknown
vx_tts_initialize
```

---

## Anti-flooding

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/text-chat-guide/anti-flooding

**Contents:**
- Anti-flooding#
- Control messaging rates#

Note: This feature is in Open Beta.

Vivox can help mitigate text message flooding through pre-login value settings. Anti-flooding sets a limit for the text message length as well as the number of text messages sent per second. Use this feature to limit texting rate to prevent channels from being flooded by a player.

If a player attempts to send a text message that exceeds the text message length, the requested message is not sent and an error code reading “The message is too large and cannot be sent” is returned.

Rate limiting is enforced by the Vivox SDK. If a user tries to send messages at a rate higher than allowed, a VxErrorMessageTextRateExceeded error is returned.

Message rates are managed through tokens. The tokens are stored in a bucket to limit how many tokens are available at any given moment. You can make changes to the individual token costs of a message as well as the size of the bucket to control the send rate of messages in your game.

You can use MaxTextMessageRate, MaxTextMessageBucketSize, TextMessageCreditCost together to set a non-integer value for the rate limit. In the following example you can send 2 messages before your bucket is empty, each message costs 3 tokens, and it will take 3 seconds for 3 tokens to be put back into the bucket, therefore it can send at a rate of about 1 message every 3 seconds.

To modify these settings from their default, reach out to a Vivox representative.

**Examples:**

Example 1 (unknown):
```unknown
VxErrorMessageTextRateExceeded
```

Example 2 (unknown):
```unknown
vx_req_account_send_msg *req;
vx_req_account_send_msg_create(&req);
req->connector_handle = vx_strdup("c1");
req->account_handle = vx_strdup(".issuer-w-dev.mytestaccountname.");
req->language = vx_strdup(“en-us”);
req->user_uri = vx_strdup("sip:theotheruser@vd1.vivox.com");
req->message_body = vx_strdup("Hey there buddy!");
int status = vx_issue_request2(&req->base); // If rate limiting is surpassed, status = VxErrorMessageTextRateExceeded
```

Example 3 (unknown):
```unknown
vx_req_account_send_msg *req;
vx_req_account_send_msg_create(&req);
req->connector_handle = vx_strdup("c1");
req->account_handle = vx_strdup(".issuer-w-dev.mytestaccountname.");
req->language = vx_strdup(“en-us”);
req->user_uri = vx_strdup("sip:theotheruser@vd1.vivox.com");
req->message_body = vx_strdup("Hey there buddy!");
int status = vx_issue_request2(&req->base); // If rate limiting is surpassed, status = VxErrorMessageTextRateExceeded
```

Example 4 (unknown):
```unknown
MaxTextMessageSize
```

---

## Single session groups

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/single-session-groups

**Contents:**
- Single session groups#

Vivox provides the capability for individual audio sessions to share the same capped bandwidth allocation.

Note: All sessions in a single session group use the same capped bandwidth allocation.

When sessions are all in a single session group, the application can use two important audio effects: session focus and transmit control.

You can view example code for these requests and responses in the SDKSampleApp.

---

## Optimize build speed

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/optimize-build-speed/overview

**Contents:**
- Optimize build speed#
- Choose a machine specification#
- Select a caching strategy#
- Choose a Git executable#

Efficient build processes are critical for you to maximize productivity and minimize wait times. Unity Build Automation offers several strategies to optimize your build speed based on the unique needs of your project. If you select the right machine specifications, implement caching strategies, and configure your Git executable correctly, you can significantly improve your build times.

It's crucial to select the appropriate machine specification for your build. A builder with more vCPU cores, RAM, or disk space can handle larger projects or more resource intensive builds. However, higher tier machines don't always guarantee faster build times for all projects. Learn how to choose the right machine for your needs.

Caching allows you to reuse previously processed assets and dependencies and can drastically reduce build times. Unity Build Automation offers various caching options, including library caching and workspace caching. Learn how to implement the best caching strategy for your project.

The Git executable you select can affect the speed of your source code checkout process, especially for large repositories. Unity Build Automation supports both Native Git and Cygwin Git for Windows-based projects. Discover which option suits your setup.

---

## In-game audio device selection

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/in-game-audio-control/in-game-audio-device-selection

**Contents:**
- In-game audio device selection#

The Vivox SDK automatically uses the default system audio input and output devices.

Changes to available audio devices are performed through system events. When a new device or a new user with a new device is detected, or the currently active device is removed, configure your application to perform the following actions:

To provide users with a choice of which audio device to use for group voice chat independent of the audio devices that are used for a game, build a user interface that displays a list of available devices to users that they can select a device from. Use the following requests for this user interface, where audio output devices are called render devices, and audio input devices are called capture devices:

Each of the vx_resp_aux_get_*_devices responses include OS default devices, a list of devices that are available for selection with a vx_req_aux_set_*_device request, the current device selection, and the effective device in use. The current_*_device is the vx_device_t entry as it appears in the list of available devices. In the scenario where the current device is a representative audio device that serves as a reference to some other device (for example, Default System Device), effective_*_device is set to the real device in use. The current and effective devices match if a specific, discrete audio device is set.

If either the capture or render device is set to Default System Device, when a compatible audio device is plugged in or the existing in-use audio device is unplugged, audio automatically switches to the other device, and the application is notified with an evt_audio_device_hot_swap message. Use this message to provide a visual cue to the user that they are now using a different audio device.

For example, if a user is using a speaker device for audio and a headset is plugged in, set the current Vivox capture and render devices to the headset by using vx_req_aux_set_capture_device and vx_req_aux_set_render_device. If there is no capture device to fall back to, but there is a render device (for example, HDMI output), set the capture device to No Device.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_aux_get_render_devices
```

Example 2 (unknown):
```unknown
vx_req_aux_get_capture_devices
```

Example 3 (unknown):
```unknown
vx_req_aux_set_render_device
```

Example 4 (unknown):
```unknown
vx_req_aux_set_capture_device
```

---

## Transmission modes

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/transmission/transmission-modes

**Contents:**
- Transmission modes#
- TransmissionMode::None#
- TransmissionMode::All#
- TransmissionMode::Single#

This is the default value of the TransmissionMode, and results in no audio being broadcast to any channel.

If you are transmitting into a specific channel by using TransmissionMode.Single, and that channel is disconnected entirely or has its audio capability disconnected, the TransmissionMode reverts to TransmissionMode.None, regardless of whether you are joined to other audio capable channels, rather than the plug-in switching transmission to another one automatically. If you want a switch to occur, you can use the VivoxService.Instance.ChannelLeft event and check it against the VivoxService.Instance.TransmittingChannels. Conversely, if set to TransmissionMode.All, you continue to transmit into all channels.

You can call VivoxService.Instance.SetChannelTransmissionModeAsync(TransmissionMode.All) at any point to enable transmission into all current and future channels. This enables users to broadcast audio into all channels that they are or will ever be connected to, until you change the policy to something else. There is no additional resource cost for transmitting to multiple channels compared to transmitting to a single channel.

Use this policy if a user will be in multiple audio-capable channels, but will only speak in one channel at a time. In cases where the user will only ever be in one audio-only or audio and text channel at a time, this performs identically to TransmissionMode.All.

When setting TransmissionMode.Single by using VivoxService.Instance.SetChannelTransmissionModeAsync(TransmissionMode transmissionMode, string channelName = null), you must also include a channelName as the second argument, specifying which single channel you want to transmit to. There is no benefit to preemptively setting this value for a channel that you are not already connected to, because the audio will have no where to go.

**Examples:**

Example 1 (unknown):
```unknown
TransmissionMode.Single
```

Example 2 (unknown):
```unknown
TransmissionMode.None
```

Example 3 (unknown):
```unknown
VivoxService.Instance.ChannelLeft
```

Example 4 (unknown):
```unknown
VivoxService.Instance.TransmittingChannels
```

---

## Volume shifts

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/ios/volume-shifts

**Contents:**
- Volume shifts#

iOS increases the volume of ducked game audio unexpectedly and without notification when certain device events occur. An example of this scenario is when you dismiss the AirPods battery dialog.

The following steps detail how to reproduce this behavior:

Configure the AVAudioSession with:

Open the capture device with VoiceProcessingIO enabled. To do this, use either:

Play audio from an AVAudioPlayer.

Open the AirPods case.

Dismiss the AirPods battery dialog.

Note that the audio player volume increases significantly.

**Examples:**

Example 1 (unknown):
```unknown
AVAudioSessionCategoryPlayAndRecord
```

Example 2 (unknown):
```unknown
AVAudioSessionModeVoiceChat
```

Example 3 (unknown):
```unknown
kAudioUnitSubType_VoiceProcessingIO
```

Example 4 (unknown):
```unknown
setVoiceProcessingEnabled(true)
```

---

## Moderator mute a user

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/muting/moderator-mute-users

**Contents:**
- Moderator mute a user#

A moderator is a user that has moderator privileges in a specified channel. A moderator can only mute users in a specified channel, and not across all channels at a time.

When a moderator mutes a specific user, no other users in the channel can hear that user.

To moderator mute a specific user, use the vx_req_channel_mute_user request.

A moderator can also mute all current users in a specified channel.

Note: Users who join the channel after the moderator mute are unmuted.

To moderator mute all users in a specific channel, use the vx_req_channel_mute_all_users request.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_channel_mute_user
```

Example 2 (unknown):
```unknown
vx_req_channel_mute_user_t* reqStruct;
vx_req_channel_mute_user_create(&reqStruct);
reqStruct->account_handle = vx_strdup(“account handle”);
reqStruct->channel_name = NULL; // deprecated and will be ignored
reqStruct->channel_uri = vx_strdup(“sip:confctl-g-channel_name@realm.vivox.com”); // Use SIP URI format
reqStruct->participant_uri = vx_strdup(“sip:.participant.@realm.vivox.com”); // Use SIP URI format
reqStruct->set_muted = 1; // 1 - mute, 0 - unmute
reqStruct->scope = mute_scope_audio;
reqStruct->access_token = vx_strdup(“access token”); // authorization of operation

int ret = vx_issue_request3((vx_req_base_t*) reqStruct);
```

Example 3 (unknown):
```unknown
vx_req_channel_mute_user_t* reqStruct;
vx_req_channel_mute_user_create(&reqStruct);
reqStruct->account_handle = vx_strdup(“account handle”);
reqStruct->channel_name = NULL; // deprecated and will be ignored
reqStruct->channel_uri = vx_strdup(“sip:confctl-g-channel_name@realm.vivox.com”); // Use SIP URI format
reqStruct->participant_uri = vx_strdup(“sip:.participant.@realm.vivox.com”); // Use SIP URI format
reqStruct->set_muted = 1; // 1 - mute, 0 - unmute
reqStruct->scope = mute_scope_audio;
reqStruct->access_token = vx_strdup(“access token”); // authorization of operation

int ret = vx_issue_request3((vx_req_base_t*) reqStruct);
```

Example 4 (unknown):
```unknown
vx_req_channel_mute_all_users
```

---

## Audio device management

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/in-game-audio-control/in-game-audio-device-selection

**Contents:**
- Audio device management#
- Identify effective device#

The Vivox SDK automatically uses the system’s default audio input and output devices. You can override this behavior to give users a choice of which audio device to use for group voice chat, independent of the audio devices used for the game.

To perform this override, build a user interface in the game that displays a list of available devices to users from which they can select a device. Build this user interface for InputDevices and OutputDevices, and utilize the VivoxService.Instance.AvailableInputDevices and VivoxService.Instance.AvailableOutputDevices to return a ReadOnlyCollection of either the VivoxInputDevices or VivoxOutputDevices, then populate the UI with the names of these devices.

This user interface should be subscribed to the VivoxService.Instance.AvailableInputDevicesChanged and VivoxService.Instance.AvailableOutputDevicesChanged actions, and refresh the UI with the new lists from VivoxService.Instance.AvailableInputDevices and VivoxService.Instance.AvailableOutputDevices to catch any additions or removals from the lists of AudioDevices.

After a user has selected a device from the list, use VivoxService.Instance.SetActiveInputDeviceAsync(VivoxInputDevice device) to set an input device or VivoxService.Instance.SetActiveOutputDeviceAsync(VivoxOutputDevice device) to set the output device.

The following example covers how an audio device should be set after it has been chosen from the list. This also includes a check to ensure that the audio device is still on the list of devices to ensure that a change in the audio devices didn't happen while the user interface was loaded.

The Chat Channel Sample has an example of a UI object controlling the AudioDevice list in AudioDeviceSettings.cs.

Participants can use a virtual device such as Default System Device or Default Communication Device as their active device when using voice chat. Sometimes, you might need information about the physical device represented by a virtual device, like when trying to troubleshoot a device-specific bug.

The physical device is known as the effective device within the Vivox SDK.

The Vivox SDK contains properties and events that can check the effective input and output devices, and subscribe to their changes.

As an example, when a player selects the Default System Device as their in-game input device, the underlying microphone set as the Default System Device in their system settings will be the real device used. In this situation, VivoxService.Instance.EffectiveInputDevice returns an object representing the real input device being used. The image is what this looks like in the Chat Channel Sample:

The effective device can change even if the available device list or the active device doesn't, such as when a user adjusts the Default System Device in the System Settings on Windows machines. In this case, neither the available device list or the active device changes but the effective device does because the real device that's used by Default System Device has changed.

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.AvailableInputDevices
```

Example 2 (unknown):
```unknown
VivoxService.Instance.AvailableOutputDevices
```

Example 3 (unknown):
```unknown
VivoxInputDevices
```

Example 4 (unknown):
```unknown
VivoxOutputDevices
```

---

## Troubleshoot Build Automation failures

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/check-build-results/troubleshoot-build-failures/overview

**Contents:**
- Troubleshoot Build Automation failures#
- Check the logs for errors#
- Run a clean build#
- Run locally in batch mode#
  - Windows CMD console#
  - macOS terminal#
  - Template build script#
- Common issues#

The first step to troubleshoot any Unity Build Automation (UBA) failure is to check the error logs.

The UBA logs highlight error messages in red and warnings in yellow to improve visibility. UBA also collects any warning and error messages in a compact log tab to make them easier to find, although not all failures are recorded in the compact log so it can be helpful to check the full log. If there are multiple error logs and you have a previous build that was successful, you can compare the logs to filter out any errors that might be present in the successful run.

UBA builds that cache either the library or the workspace folders can cause issues. To force the target to ignore the cache, use the Clean Build option when you trigger the build.

If the project builds successfully in the editor but fails to build through Unity Build Automation, it might be because you use batch mode to generate the build. To find out if the error reproduces, you can try to run in batch mode locally

First, you need to either delete the Library folder or check out the source code into a new fresh folder, which forces UBA a re-download of the packages. This simulates the UBA build server's behavior that checks out the source code on every build.

Here's a Windows and macOS version of the same command.

Note: Several of these flags are not required. If you don't want to run unit tests, you can skip the flags such as the -testPlatform, -testResults, and -runTests.

The directories provided are the default installation folders for the Unity Editor; adjust them according to the correct path on your local machine.

If you'd like to also run tests, you can add the following tags:

Place your build script containing the static method that performs the build inside the editor folder. If you don't have an editor folder yet, create one.

Here is a template build script for your reference:

Platform specific issues:

**Examples:**

Example 1 (unknown):
```unknown
-testPlatform
```

Example 2 (unknown):
```unknown
-testResults
```

Example 3 (unknown):
```unknown
"C:\Program Files\Unity\Hub\Editor\{UNITY_VERSION}\Editor\unity.exe" -batchMode -skipMissingProjectID -skipMissingUPID -buildTarget {BUILD_TARGET} -logFile C:\{SOME_PATH}\log.txt -projectPath C:\{PROJECT_PATH} -executeMethod {CLASS_AND_STATIC_METHOD_NAME_OF_BUILD_SCRIPT} -quit
```

Example 4 (unknown):
```unknown
"C:\Program Files\Unity\Hub\Editor\{UNITY_VERSION}\Editor\unity.exe" -batchMode -skipMissingProjectID -skipMissingUPID -buildTarget {BUILD_TARGET} -logFile C:\{SOME_PATH}\log.txt -projectPath C:\{PROJECT_PATH} -executeMethod {CLASS_AND_STATIC_METHOD_NAME_OF_BUILD_SCRIPT} -quit
```

---

## Android SDK permission requirements

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/android/android-sdk-permission-requirements

**Contents:**
- Android SDK permission requirements#

The following permission requirements apply when working with the Vivox Android SDK:

android.permission.INTERNET - Allows communication with Vivox servers

android.permission.RECORD_AUDIO - Allows microphone access

For more information, refer to the Android developer documentation on requesting app permissions.

Note: Not giving these permissions might prevent some Vivox SDK features from working properly.

android.permission.MODIFY_AUDIO_SETTINGS

android.permission.ACCESS_NETWORK_STATE - Allows access to network information

android.permission.ACCESS_WIFI_STATE - Allows access to Wi-Fi information

android.permission.BLUETOOTH - Allows access to Bluetooth devices on Android versions earlier than 12

android.permission.BLUETOOTH_CONNECT - Allows access to Bluetooth devices on Android version 12 and later

Note: On Android 12 (Android SDK version 31) and later, you need to request the android.permission.BLUETOOTH_CONNECT permission at runtime. For more information, refer to the Android developer documentation on Bluetooth permissions.

These permissions will be given by default to your app after it is compiled with the Vivox SDK.

To remove optional permissions that you don't want in your application, you need to have a custom AndroidManifest.xml file for your application. To create this file, complete the following steps:

Open your project in the Unity Editor.

Select Edit > Project Settings.

On the Player tab, open the Android section.

In the Publishing Settings section, enable the Build > Custom Main Manifest checkbox.

The file will generate in your assets, and its path will display after the checkbox.

When you have a custom AndroidManifest.xml file, you can add the permission tag to it and specify tools:node”remove” to ensure that, for example, android.permission.BLUETOOTH_CONNECT is not included:

**Examples:**

Example 1 (unknown):
```unknown
android.permission.INTERNET
```

Example 2 (unknown):
```unknown
android.permission.RECORD_AUDIO
```

Example 3 (unknown):
```unknown
android.permission.MODIFY_AUDIO_SETTINGS
```

Example 4 (unknown):
```unknown
android.permission.ACCESS_NETWORK_STATE
```

---

## Quick join

**URL:** https://docs.unity.com/ugs/en-us/manual/lobby/manual/quick-join

**Contents:**
- Quick join#
- Best practices#

The Quick Join API allows players to quickly find and join a lobby without needing to manually select a specific lobby from a query. Players set their query filters, call the Quick Join API, and the Lobby service then tries to place them in a lobby that matches their criteria and has capacity.

Quick Join is designed to solve the common problem where a player manually does a query, looks at the results, selects a lobby to try to join, and then repeatedly fails to join because the lobby has already filled up by the time they attempt to join. It can also be used as a form of basic matchmaking (although only in existing lobbies, it does not create new ones).

Quick Join doesn't guarantee that a request will be fulfilled. Failures can occur if there are no lobbies that match the query criteria, or if the join is attempted and then fails. In these scenarios, clients should expect to receive a 404 Not Found error. If a failure happens, clients can try to Quick Join again (see Rate limits), or can fall back to creating a new lobby and assuming the host role.

The following code sample shows how to set up Quick Join:

**Examples:**

Example 1 (unknown):
```unknown
try
{
    // Quick-join a random lobby with a maximum capacity of 10 or more players.
    QuickJoinLobbyOptions options = new QuickJoinLobbyOptions();

    options.Filter = new List<QueryFilter>()
    {
        new QueryFilter(
            field: QueryFilter.FieldOptions.MaxPlayers,
            op: QueryFilter.OpOptions.GE,
            value: "10")
    };

    var lobby = await LobbyService.Instance.QuickJoinLobbyAsync(options);

    // ...
}
catch (LobbyServiceException e)
{
      Debug.Log(e);
}
```

Example 2 (unknown):
```unknown
try
{
    // Quick-join a random lobby with a maximum capacity of 10 or more players.
    QuickJoinLobbyOptions options = new QuickJoinLobbyOptions();

    options.Filter = new List<QueryFilter>()
    {
        new QueryFilter(
            field: QueryFilter.FieldOptions.MaxPlayers,
            op: QueryFilter.OpOptions.GE,
            value: "10")
    };

    var lobby = await LobbyService.Instance.QuickJoinLobbyAsync(options);

    // ...
}
catch (LobbyServiceException e)
{
      Debug.Log(e);
}
```

---

## Custom serialization

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/initialize-modules/custom-serialization

**Contents:**
- Custom serialization#

Cloud Code modules use the Newtonsoft.Json library to serialize and deserialize data. You can use the Newtonsoft.Json attributes to customize the serialization and deserialization of your data.

For example, you can use the JsonProperty attribute to change the name of a property in the JSON representation of your data:

For more information, refer to the Newtonsoft.Json documentation.

**Examples:**

Example 1 (unknown):
```unknown
Newtonsoft.Json
```

Example 2 (unknown):
```unknown
JsonProperty
```

Example 3 (unknown):
```unknown
using Newtonsoft.Json;

public class MyData
{
    [JsonProperty("myProperty")]
    public string MyProperty { get; set; }
}
```

Example 4 (unknown):
```unknown
using Newtonsoft.Json;

public class MyData
{
    [JsonProperty("myProperty")]
    public string MyProperty { get; set; }
}
```

---

## Block functions

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/muting/block-functions

**Contents:**
- Block functions#

The Vivox SDK offers blocking functionality, allowing a user to prevent themselves from being heard by or hearing a specific user, in any channel that they are a part of.

Blocking is managed by the VivoxService.Instance.BlockPlayerAsync(string PlayerId) and VivoxService.Instance.UnblockPlayerAsync(string PlayerId) functions. PlayerId's are found in the VivoxParticipant construct representing each player in each channel.

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.BlockPlayerAsync(string PlayerId)
```

Example 2 (unknown):
```unknown
VivoxService.Instance.UnblockPlayerAsync(string PlayerId)
```

Example 3 (unknown):
```unknown
VivoxParticipant
```

---

## Access token error codes

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/access-token-error-codes

**Contents:**
- Access token error codes#

The following table details the access token-specific error codes that the Vivox SDK can return from responses to calls that require access tokens.

Access token already used

Access token has invalid signature

Access token payload claims do not match request parameters

Access token malformed

Unknown error using access tokens

Access token mode not enabled

Access token service unavailable

Access token issuer does not match claims

---

## Customize Fastlane configurations for iOS builds

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/advanced-build-configuration/customize-fastlane-configurations-for-ios-builds

**Contents:**
- Customize Fastlane configurations for iOS builds#
- Add required files#
  - File #1: A JSON options file that controls the paths to your custom Fastfile/Gymfile and which lanes in the Fastfile should be run.#
  - File #2: A custom Fastfile to install the provisioning profile and update the provisioning settings in the Xcode project.#
  - File #3: A custom Gymfile to define the application identifier to provisioning profile mapping as part of customizing the export options.#
- Update Advanced Settings#

Build Automation relies on Fastlane for building Xcode projects. This document describes how to modify the behavior of Fastlane using the Fastlane file format known as a Gymfile. All of the configuration options are very specific to Fastlane, so some basic familiarity with the toolset is recommended.

This section uses a basic example of how you might configure your project to build with multiple profiles currently. The following example assumes you are building an app with an application identifier of "com.unity3d.buildautomation.base" and a stickers extension with an app identifier of "com.unity3d.buildautomation.base.stickers".

This setup currently requires adding three extra files to your repository:

This file can be placed anywhere in the repository but is placed at Assets/ucb_xcode_fastlane.json in this example. Supports the following properties (all of which are optional, if you do not use a property remove it entirely): fastfile - a path to your custom Fastfile, relative to the root of your repository. gymfile - a path to your custom Gymfile, relative to the root of your repository. lanes - lanes within the Fastfile referenced above. pre_build - lane to run before the actual Xcode build steps. post_build - lane to run after the actual Xcode build steps.

These paths are all relative to the root of the project.

Example "Assets/ucb_xcode_fastlane.json":

This file can be placed anywhere in the repository, but it needs to match the path of the “fastfile” specified in ucb_xcode_fastlane.json above.

If you’ve configured pre_build/post_build lanes, an options hash is passed to those lanes when they run containing: project_dir - root of the repo this project is building from. build_target - build target identifier for this build. output_directory - final output directory where the Xcode project will build to.

In this example, we are installing the provisioning profile at “Assets/ucb/Stickers.mobileprovision” in the repo, and updating the “Unity-iPhone-Stickers” target in the Xcode project to use that profile.

Example Assets/ucb/Fastfile:

This file can be placed wherever you want in the repo, but it needs to match the path of the “gymfile” in ucb_xcode_fastlane.json above. For available options, see the fastlane docs.

In this example, the “com.unity3d.buildautomation.base.stickers” application identifier should map to a UUID of 1e33640e-9a55-4357-a632-ca6c48a53a96 (which is the UUID of the provisioning profile at Assets/ucb/Stickers.mobileprovision).

Example Assets/ucb/Gymfile:

All that’s left is to update the Advanced Settings for your build target in the dashboard.

**Examples:**

Example 1 (unknown):
```unknown
"com.unity3d.buildautomation.base"
```

Example 2 (unknown):
```unknown
"com.unity3d.buildautomation.base.stickers"
```

Example 3 (unknown):
```unknown
Assets/ucb_xcode_fastlane.json
```

Example 4 (unknown):
```unknown
"Assets/ucb_xcode_fastlane.json"
```

---

## Channels

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/channels/channels-toc

**Contents:**
- Channels#

Vivox provides group text and voice communications through channels. Users can communicate with each other by joining the same channel.

Channels support the following functionality:

Channels have the following characteristics:

---

## Connect your version control system

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/get-started-with-build-automation/connect-your-version-control-system

**Contents:**
- Connect your version control system#
- Connect Build Automation to Unity Version Control#
- Connect Build Automation to Git#
  - Personal Access Tokens#
    - GitHub#
    - GitLab#
    - Bitbucket#
  - SSH keys and private repositories#
- Connect Build Automation to Azure#
  - Configuring Azure on the Dashboard#

To use Build Automation with your projects, you must first host your project in a source control system (also known as a version control system). Unity Build Automation supports:

The following shows you how to set up source control for Build Automation.

Unity Build Automation supports projects stored in UVCS repositories.

Note: This product requires a subscription to Unity DevOps.

Unity Build Automation supports projects stored in Git repositories. Your repository can be hosted on GitHub, GitLab, Bitbucket, or private servers.

You can configure access to your repository using the following authorization protocols:

You can also choose which Git Executable your build will use if you are using a Windows operating system.

To configure Build Automation to use a Personal Access Token to access your repository:

Personal Access Tokens vary by provider. Relevant details for each provider are included below, but please refer to your provider's documentation for up to date details on how to generate and use your token.

Build Automation requires read access to the repository you wish to build.

If you are using a Personal Access Token (classic) then the specific permissions required are repo if the repository you intend to build is private, or public_repo if the repo you intend to build is public and you want to allow the minimum level of access.

If you plan to enable auto-build in Build Automation, then the write:repo_hook permission is also required.

GitHub automatically removes classic Personal Access Tokens that haven't been used in a year, even if you did not provide an expiration date when creating the token.

Fine-grained Personal Access Tokens can be created for a user or an organization. Access can be granted to all repositories owned by the user or organization, or only to specific repositories. If you choose to use Fine-grained tokens, and you are not configuring a token with only Public Repositories access, you will have to add specific permissions to the access token.

The following Repository Permissions are the minimum required to use a Fine-grained token:

Build Automation does not require any Account Permissions.

Note that Fine-grained Personal Access Tokens always have an expiration date, once a token has expired Build Automation will no longer be able to access your repository.

Fine-grained Personal Access Tokens are currently in beta, it's possible that the required permissions (or other aspects of the tokens) will change, which may require you to take action to keep using the tokens with Build Automation.

Personal Access Tokens must be granted the read_repository scope in order for Build Automation to build your repository. If you intend to enable auto-build, If you plan to enable auto-build in Build Automation, then the write_repository scope is also required.

GitLab tokens are always created with an expiration date, once a token has expired Build Automation will no longer be able to access your repository.

Repository Access Tokens grant access to a single repository, so you'll have to create a token for each repository you wish to build.

Tokens must be granted Read permissions on the Repositories scope in order for Build Automation to build your repository. If you plan to enable auto-build in Build Automation, then you will also need to grant Read and write on the Webhooks scope.

For all providers, Build Automation will no longer be able to access your repository if you revoke the access token, if your token expires, or if the user associated with the token has their access to the repository reduced or revoked.

For the next step, see Setting up a target build platform.

When Build Automation connects to the hosting site, it automatically detects whether your repository is public or private. If your repository is public, Build Automation automatically connects to it, and you can skip to Build Configuration.

If your repository is private an SSH key is generated and displayed in the settings screen below the repository URL. Add this SSH key to the settings of your source control provider to grant Build Automation access.

To configure Build Automation to use an SSH key to access your repository:

Unity Build Automation supports projects stored in Azure repositories.

Unity Build Automation supports projects hosted in Apache Subversion (SVN) repositories.

Build Automation does not support connecting to your SVN repository using a public SSH Key. Use a username and password instead.

Self-signed SSL certificates are not supported. Build Automation does not support an SSL handshake with a server with a self-signed certificate. The hostname in the certificate must match the hostname accessed by Build Automation.

Unity Build Automation supports Projects stored in Perforce repositories.

**Examples:**

Example 1 (unknown):
```unknown
public_repo
```

Example 2 (unknown):
```unknown
write:repo_hook
```

Example 3 (unknown):
```unknown
Read and write
```

Example 4 (unknown):
```unknown
read_repository
```

---

## Usernames

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/access-token-identifiers/usernames

**Contents:**
- Usernames#

When you create a username, the following criteria apply:

0x30-0x39, 0x41-0x5A, 0x61-0x7A

Note: You can only use the percent sign for URL encoding. Follow the percent sign by two uppercase hex characters.

The recommended best practice is to URL-encode the username by using only uppercase hex digit characters. Lowercase characters are allowed in the username, but not when used as the hex digit characters.

For example, if your issuer name is "crux", then a valid username is .crux.0verl0rd.

It’s recommended that you re-use player account names and create one-to-one mapping between and account and a player to facilitate the use of Vivox features such as cross-mute and blocking. It will also become a requirement for using future safety features designed for reducing toxicity and fostering more positive communities.

Note: For the Vivox Unity SDK and the Vivox Unreal SDK, the naming displayed in this example only applies during access token generation. When you construct an AccountId, you are expected to use only the "0verl0rd" part of the username and not ".crux.0verl0rd." for the name argument. The AccountId function ToString() returns a full user URI that is suitable for token generation, which includes the dots, issuer, and other URI components.

If you do not want to use a unique account name per player, or if you do not want to expose the username to the network, then use one of the following options:

**Examples:**

Example 1 (unknown):
```unknown
.crux.0verl0rd
```

Example 2 (unknown):
```unknown
".crux.0verl0rd."
```

---

## Chat history

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/chat-history/chat-history

**Contents:**
- Chat history#

Note: This feature is currently only available in the Core SDK and Unity SDK 16.

Chat history allows for the archiving and client-side retrieval of text conversations between users. Users can look up specific conversations or view previously sent messages by all users in a conversation.

Note: Messages from blocked participants are not returned in chat history search results. A history request can come back empty if all messages in the request were from a blocked participant. In this case, the response contains a cursor indicating that there are more messages in the channel. The max argument for chat history will show fewer messages than the number requested if there is a message from a blocked user in the channel. Use the cursor value in the subsequent request to retrieve the next page of messages.

Chat history has a storage period of 7 days. If Text Evidence Management is enabled in a project, the storage period is extended to 30 days.

---

## Example: Login token

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/access-token-guide/access-token-examples/example-login-token

**Contents:**
- Example: Login token#

The token in this example allows the user "jerky" to sign in to the server.

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

**Examples:**

Example 1 (unknown):
```unknown
{
   "iss":"blindmelon-AppName-dev",
   "vxi":933000,
   "vxa":"login",
   "exp":1600349400,
   "f":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com"
}
```

Example 2 (unknown):
```unknown
{
   "iss":"blindmelon-AppName-dev",
   "vxi":933000,
   "vxa":"login",
   "exp":1600349400,
   "f":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com"
}
```

Example 3 (unknown):
```unknown
e30.eyJpc3MiOiJibGluZG1lbG9uLUFwcE5hbWUtZGV2IiwidnhhIjoibG9naW4iLCJ2eGkiOjkzMzAwMCwiZXhwIjoxNjAwMzQ5NDAwLCJmIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2Lmplcmt5LkB0bGEudml2b3guY29tIn0.5FKsAQz7bRQGGlSZw-KIcCmft7Ic6nT3Ih-TbdYPWwI
```

Example 4 (unknown):
```unknown
e30.eyJpc3MiOiJibGluZG1lbG9uLUFwcE5hbWUtZGV2IiwidnhhIjoibG9naW4iLCJ2eGkiOjkzMzAwMCwiZXhwIjoxNjAwMzQ5NDAwLCJmIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2Lmplcmt5LkB0bGEudml2b3guY29tIn0.5FKsAQz7bRQGGlSZw-KIcCmft7Ic6nT3Ih-TbdYPWwI
```

---

## Cancel a text-to-speech message

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/text-to-speech/cancel-tts-message

**Contents:**
- Cancel a text-to-speech message#

To cancel a currently playing or enqueued text-to-speech (TTS) message, use vx_tts_cancel_utterance with the utterance ID of the message that you want to cancel. For example:

In destinations that contain queues, canceling an ongoing TTS message automatically triggers playback of the next message. Canceling an enqueued TTS message shifts all later messages up one place in the queue.

You can cancel all TTS messages in a destination (ongoing and enqueued), or all TTS messages in all destinations.

**Examples:**

Example 1 (unknown):
```unknown
vx_tts_cancel_utterance
```

Example 2 (unknown):
```unknown
vx_tts_utterance_id firstUtteranceId;
vx_tts_speak(managerId, voiceId, "Example message, this is great!", tts_dest_queued_local_playback, &utteranceId);

vx_tts_cancel_utterance(managerId, utteranceId);
```

Example 3 (unknown):
```unknown
vx_tts_utterance_id firstUtteranceId;
vx_tts_speak(managerId, voiceId, "Example message, this is great!", tts_dest_queued_local_playback, &utteranceId);

vx_tts_cancel_utterance(managerId, utteranceId);
```

Example 4 (unknown):
```unknown
vx_tts_cancel_all_in_dest(managerId
```

---

## Large text channel setting

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/chat-history/large-text-channels

**Contents:**
- Large text channel setting#
- Enable large text channels#

The large text channels setting is an optional feature that allows a Vivox backend to support over 200 participants in a text channel.

By default, the large text channel setting is disabled for all games, limiting those game channels to 200 users. With this feature enabled, text channels can support up to 2000 users.

Note: Changes to large text channel configurations won’t take effect for an existing channel until every occupant of that channel has left and the channel is destroyed.

Large text channels are an enterprise-only feature. To enable this feature, contact your Vivox representative.

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/run-modules

---

## Add the Vivox package to a Unity project

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/implement-vivox-unity/add-vivox-package-to-unity-project

**Contents:**
- Add the Vivox package to a Unity project#

To add the Unity Package Manager Vivox package to a Unity project, complete the following steps.

Download the Vivox package from Unity Package Manager.

Caution: Do not unzip the package.

Go to Window > Package Manager.

In the upper-left corner of the Package Manager window, select Add (+) > Add package from tarball and then select your zipped package.

After the package has finished the import process, you can choose to also import the Chat Channel Sample. To do this, go to your imported packages, and then next to the Chat Channel Sample, select Import.

To import additional platform packages, repeat the preceding steps as necessary.

After you import an additional platform package, you will see “Vivox for <platform>” in your project’s Packages directory.

**Examples:**

Example 1 (unknown):
```unknown
com.unity.services.vivox
```

---

## Unity Build Automation

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/overview

**Contents:**
- Unity Build Automation#
- What is Unity Build Automation?#
- Key concepts#
  - Continuous integration#
  - How automated build generation works#
  - Source Control#
- Unity Versions#
- Supported Platforms#
- Additional resources#

Unity Build Automation is a continuous integration service that automatically creates multiplatform builds in the Cloud. You can point Build Automation toward your version control system to:

Build Automation supports most popular version control systems and builds for multiple platforms simultaneously, including iOS.

You can access Unity Build Automation through the Unity Dashboard.

Build Automation provides continuous integration by automatically building your project whenever changes are detected in the configured version control system.

By compiling your project whenever a change is committed, Build Automation gives you the most accurate idea of when and where errors occur and ensures you always have a build of your game based on the last working commit.

To enable automated build generation, set the Auto-build toggle to Yes in Build Automation > Configurations > Basic Info settings.

Build Automation connects to either Unity Version Control or your source control system and monitors that system for changes to your project. When it detects changes to your project, Build Automation downloads and builds your project for your target platforms. When the builds are complete, Build Automation notifies you of the results and links to download, share, and install the builds. If there are errors, Build automation informs you immediately so you can quickly fix them, commit the changes, and trigger new builds.

You can integrate different software for notifications such as Slack, Discord, and Jira. For more information, refer to integrations.

Source Control is a system for managing file changes. You can use Build Automation in conjunction with most version control tools, including UVCS, Perforce, and Git. Refer to Connect Your Version Control System.

In alignment with Unity’s platform-wide support policy, we only answer support tickets for LTS releases and the latest TECH release. For more information on Unity’s support policy for editor releases, refer to LTS releases.

Build Automation supports a wide range of platforms, enabling you to build projects for various targets efficiently. Below is the list of platforms currently supported by Build Automation:

Note: Refer to the available versions for Xcode and Android.

---

## Add the Vivox Unreal plug-in to a speciﬁc project

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/implement-vivox-unreal/add-vivox-unreal-plugin-specific-project

**Contents:**
- Add the Vivox Unreal plug-in to a speciﬁc project#

Important: If you are using a prebuilt, binary version of Unreal Engine on a computer that is running macOS, you cannot use the plug-in installed for a specific project and must install it globally by using the steps in the Add the plug-in to all projects.

To add the Vivox Core Unreal 4 plug-in to a specific project, complete the following steps.

In File Explorer, navigate to the root of the directory where you extracted the Vivox package (for example, C:\vivox-unreal4-sdk-0.1.2.xxxx-win64). Fnd and copy the VivoxCore folder.

Navigate to your Unreal project folder (for example, C:\UE4Projects\ExProject).

Note: If you want to add the plug-in to a brand new project, you must first create the project in the Unreal Editor.

If your Unreal project directory does not already contain a Plugins folder, create a new folder named Plugins in the project directory.

In the Plugins folder, paste your copy of the VivoxCore folder.

**Examples:**

Example 1 (unknown):
```unknown
C:\vivox-unreal4-sdk-0.1.2.xxxx-win64)
```

---

## Initialize the AVAudioSession on app startup

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/ios/initialize-avaudiosession-on-app-startup

**Contents:**
- Initialize the AVAudioSession on app startup#

We strongly recommend that you set the AVAudioSession to AVAudioSessionCategoryPlayback, AVAudioSessionCategoryAmbient, or to another non-recording category on app startup.

Caution: Configuring the AVAudioSession near to or during voice chat can lead to unexpected volume changes. Limit this activity to the early stages of the app’s life cycle, unless you are restoring the device audio volume.

---

## Network connection state events

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/network-connection-state-events

**Contents:**
- Network connection state events#

The Vivox SDK provides details on the network connection status through network connection state events, which are informational and require no immediate action. These events are intended to supplement the game’s own notion of network reachability. They provide visibility into the Vivox SDK’s internal automatic connection recovery process under the following conditions:

The initial network connection to Vivox services has been established after vx_req_account_anonymous_login.

Note: This indicates normal operation and is not indicative of network connection recovery.

The network connection recovery process has begun.

Either the network connection has been restored, or reconnection attempts have failed.

These network connection state changes are reported through the following events for each user account that is signed in by using the standard Vivox event reporting mechanism:

Note: Some Vivox APIs that depend on network connectivity might result in undefined behavior when the Vivox SDK is disconnected from the network or is trying to recover its connection to Vivox services. Your application can avoid undefined behavior in the Vivox SDK by monitoring the Vivox connection state events that are detailed in the following list. Defer or temporarily disable Vivox requests that require an account_handle or session_handle in the request data structures when the connection state is recovering, because the success of those requests depends on a network connection to Vivox services.

vx_evt_connection_state_changed: Sent when a change has occurred in the connection to Vivox services.

connection_state_connected: Initial network connection has been established after vx_req_account_anonymous_login. Network-dependent API calls are safe.

Note: This occurs once for each time that a user signs in.

connection_state_recovering: Attempting to reestablish a connection to Vivox services. Avoid making network-dependent API calls until the connection has recovered or has failed to recover, otherwise undefined behavior can result.

connection_state_failed_to_recover: Failed to reestablish network connection to Vivox services. A logged_out event will follow. Wait to sign in again until after the game’s network connection has been restored.

connection_state_recovered: Network connection to Vivox services has been successfully reestablished. Network-dependent API calls are safe.

account_handle: The handle of a user who is signed in.

The following code is an example of how to handle connection state events. This, combined with the game’s own notion of network reachability, could be used to indicate voice and text communications availability to the user.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_account_anonymous_login
```

Example 2 (unknown):
```unknown
vx_evt_connection_state_changed
```

Example 3 (unknown):
```unknown
connection_state
```

Example 4 (unknown):
```unknown
connection_state_connected
```

---

## Request capture device access

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/request-capture-device-access

**Contents:**
- Request capture device access#

To ensure that voice functions as expected, developers who are using the Vivox SDK with applications on the following platforms must request capture device access before joining a channel.

Each platform has various ways to request capture device access. For example, you can use runtime permissions to ask a user if they want to give an application access to a capture device. For information about making runtime permission requests, refer to the platform-specific documentation that is maintained by each platform manufacturer.

Note: If an application does not request capture device access, then problems could occur, such as capture device or all channel audio not working until the channel has been left and rejoined.

---

## Get raw audio of synthesized speech

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/text-to-speech/get-raw-audio-synthesized-speech

**Contents:**
- Get raw audio of synthesized speech#

You can synthesize speech into an audio buffer for your direct use rather than having it maintained internally by the Vivox SDK. ITextToSpeech::SpeakToBuffer() can synthesize the speech signal and return it in the form of an ITTSAudioBuffer class object. This object includes a pointer to the raw audio data and metadata, such as the buffer length and audio format properties.

**Examples:**

Example 1 (unknown):
```unknown
ITextToSpeech::SpeakToBuffer()
```

Example 2 (unknown):
```unknown
ITTSAudioBuffer *Buffer;
VivoxCoreError Status = MyLoginSession->TTS().SpeakToBuffer("Some Text", &Buffer);
// If successful, Buffer will contain audio samples and metadata for the synthesized speech.
```

Example 3 (unknown):
```unknown
ITTSAudioBuffer *Buffer;
VivoxCoreError Status = MyLoginSession->TTS().SpeakToBuffer("Some Text", &Buffer);
// If successful, Buffer will contain audio samples and metadata for the synthesized speech.
```

---

## OSX notarization failure

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/check-build-results/troubleshoot-build-failures/osx-notarization-failure

**Contents:**
- OSX notarization failure#
- Symptoms#
- Environment#
- Potential cause#
- Resolution#

When you set your Unity Build Automation to notarize your macOS builds, your build fails.

You enter your correct credentials for macOS signing or notarization, but the macOS builds fail at the notarization step.

In your build logs, there is the following warning: [warning] Warning: unable to build chain to self-signed root for signer "Developer ID Application: Peter Davidson (6R6AR2S484)"

Followed by an error message:

"message": "The binary is not signed with a valid Developer ID certificate."

In Unity Build Automation (UBA), you want to build for macOS and you've used your uploaded credentials to sign and notarize your build artifact for distribution. Your credentilals are complete and work correctly when you build and notarize the app locally.

There are many potential causes for notarization failure. One of the most common causes is a problem with the exported credentials that you use to sign and notarize the app.

First, ensure the certificate is valid by validating it in Keychain Access.

If the certificate is valid and you can sign and notarize your app locally with the same credentials, you might need to export the .p12 with the intermediary certificate you use.

To export the .p12, select the private key, certificate, and intermediary certificate used in Keychain access and right-click to export the .p12 file. For more detailed instructions, refer to Creating a p12 file.

Update or create a new set of signing credentials in the target in the Basic settings tab of your build target with this new .p12, and try to build and sign or notarize your OSX app again.

If the issue persists, please contact the Service Support team. To submit a ticket from the Unity Dashboard, open DevOps and select Help & Support > Ticket > File a ticket.

**Examples:**

Example 1 (unknown):
```unknown
[warning] Warning: unable to build chain to self-signed root for signer "Developer ID Application: Peter Davidson (6R6AR2S484)"
```

Example 2 (unknown):
```unknown
"message": "The binary is not signed with a valid Developer ID certificate."
```

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/channel-names

---

## iOS app development

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/ios/ios-toc

**Contents:**
- iOS app development#

---

## Volume differences

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/audio/volume-differences

**Contents:**
- Volume differences#
- Equalizing channels#

Positional channels have lower default volume settings than 2D channels. 3D channels are a third quieter than 2D channels.

This difference is by design. Positional channels need more headroom for sound to be stereo panned, so they initialize with a lower level.

Positional channels are set to -6dB. This allows positional channels to adjust when panned hard left or right while maintaining volume dynamically.

For context on decibel levels, -10dB is what most listeners perceive as “half as loud”.

It’s possible to match channel loudness by lowering the loudness in 2D channels to the base level of 3D channels. It’s important to note that each 2D channel needs to be lowered individually every time they’re joined or re-joined. There isn’t a universal setting to manage all 2D channels at once.

Another approach is to raise the audio in the 3D channel by 6dB to match the volume of 2D channels.

If you choose to equalize the volumes between channel types, the main thing to keep in mind with these volume changes is that you need to manage individual volume boosting as the changes made are increasing the channel-wide volumes already. It’s best to not allow users to increase the volume to a level that could be harmful.

To set the volume level of an entire individual channel, use vx_req_session_set_local_render_volume. If you choose to raise the level of 3D channels set them to 56. If you choose to lower 2D channels to match the default 3D channel volume instead, set 2D channels to 44.

Important: If you boost the 3D channel default volume to match the 2D channel, test the max user volume levels. If users have control over local capture or render volume settings, test against the upper guidelines and consider lowering the maximum to which they can raise their volume. If users have control over other participants' volume levels, remember to test those as well.

For more information on managing local volume settings, refer to In-game control of audio levels.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_session_set_local_render_volume
```

---

## Large text channel setting

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/text-chat-guide/large-text-channels

**Contents:**
- Large text channel setting#
- Enable large text channels#

Note: This feature is in Open Beta.

The large text channels setting is an optional feature that allows a Vivox backend to support over 200 participants in a text channel.

By default, the large text channel setting is disabled for all games, limiting those game channels to 200 users. With this feature enabled, text channels can support up to 2000 users.

Note: Changes to large text channel configurations won’t take effect for an existing channel until every occupant of that channel has left and the channel is destroyed.

Large text channels are an enterprise-only feature. To enable this feature, contact a Vivox representative.

---

## Unity Package Manager Vivox package

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/implement-vivox-unity/unity-package-manager-vivox

**Contents:**
- Unity Package Manager Vivox package#

The Unity Package Manager is the optimal method for including Vivox functionality in a Unity project.

To use the Unity Package Manager Vivox package, complete the following steps.

Open your Unity project.

Select Edit > Project Settings. Select Services and perform either of the following tasks:

Sign in to cloud.unity3d.com.

In the left pane, select Explore Services, and then select the Vivox tile. Follow the Getting Started instructions to generate credentials for the project.

Add the Vivox package to the project.

Add com.unity.services.vivox to the project manifest.json file.

For more information, refer to the Unity documentation on the Project manifest.

In your Unity project, select Edit > Project Settings. Select the Vivox tab, and then update Environment to Automatic.

This automatically pulls Vivox credentials into your project.

**Examples:**

Example 1 (unknown):
```unknown
cloud.unity3d.com
```

Example 2 (unknown):
```unknown
com.unity.services.vivox
```

Example 3 (unknown):
```unknown
manifest.json
```

---

## Supported platforms and versions

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/supported-platforms

**Contents:**
- Supported platforms and versions#
- Terminology#
- Terminology#
  - Supported#
  - Limited versions#
  - Experimental versions#
  - Deprecated versions#

Experimental is for the latest/edge releases that are available but not yet Supported or default. Use at your own risk. This encompasses all Alpha, Beta and Prerelease versions. Issues and improvement requests will be tracked and addressed based on frequency and severity.

Supported is known-good and actively supported by the organization. Supported versions are the default choice when creating a new configuration and represent the versions we have the highest confidence in. We recommend anyone experiencing issues upgrade/migrate to Supported versions if you are using an SDK version where a platform wasn’t fully supported. Issues and improvement requests will be tracked and addressed based on frequency and severity.

Limited indicates that the platform does not have full support from our team. While we will address security issues and critical bugs on the platform, the priority of implementing platform-specific fixes and improvements is low. We do still include the platform in our releases.

Deprecated means "do not use". This technology is scheduled to be removed as part of the deprecation schedule strategy. Deprecated also means that the Unity Editor team no longer supports the version as it has fallen out of Long Term Support (LTS). Contact us if that causes problems for your company, and we can negotiate to make that version available longer. Issues and improvement requests will not be tracked or addressed.

The Vivox Unity SDK is available for the following platforms and versions:

Note: Access to some platform-specific Vivox SDKs and documentation requires approval. After approval, you access the platform-specific SDKs and documentation from the Unity Dashboard. For more information, refer to How do I get approved for NDA protected platforms? in the Unity Support Knowledge Base.

---

## Access token error codes

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/access-token-guide/access-token-error-codes

**Contents:**
- Access token error codes#

The following table details the access token-specific error codes that the Vivox SDK can return from responses to calls that require access tokens.

Access token already used

Access token has invalid signature

Access token payload claims do not match request parameters

Access token malformed

Unknown error using access tokens

Access token mode not enabled

Access token service unavailable

Access token issuer does not match claims

---

## Request runtime permissions

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/android/request-runtime-permissions

**Contents:**
- Request runtime permissions#

To ensure that your Unreal Android application will properly work with the Vivox SDK, you need to request the android.permission.RECORD_AUDIO and, on Android 12 and later, the android.permission.BLUETOOTH_CONNECT permissions at runtime. To do this, use UAndroidPermissionFunctionLibrary::CheckPermission to check if these permissions are granted, and UAndroidPermissionFunctionLibrary::AcquirePermissions to request the permission for the user.

The following code is an example of this process:

You also need to add the AndroidPermission module into the dependency in the [PROJECT].build.cs file. For example:

For more information on how to request permissions by using the Blueprint API, refer to the the Unreal documentation on Android Permission.

**Examples:**

Example 1 (unknown):
```unknown
android.permission.RECORD_AUDIO
```

Example 2 (unknown):
```unknown
android.permission.BLUETOOTH_CONNECT
```

Example 3 (unknown):
```unknown
UAndroidPermissionFunctionLibrary::CheckPermission
```

Example 4 (unknown):
```unknown
UAndroidPermissionFunctionLibrary::AcquirePermissions
```

---

## Sign in as Alice

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/sdksampleapp/log-in-as-alice

**Contents:**
- Sign in as Alice#

Start the SDKSample App with the arguments from Run the SDKSampleApp, and then sign in as the example user "Alice" (.xyzzy.alice) by using "login -u".

Note: The -help option displays the options that are available for each command, for example,"login -help".

**Examples:**

Example 1 (unknown):
```unknown
[SDKSampleApp]: conn
* Connecting to http://mt1s.www.vivox.com/api2 with connector handle http://mt1s.www.vivox.com/api2...
* Issuing req_connector_create with cookie=1
* Request req_connector_create with cookie=1 completed.
[SDKSampleApp]: login -u .xyzzy.alice.
* Logging .xyzzy.alice. in with connector handle http://mt1s.www.vivox.com/api2 and account handle .xyzzy.alice.
* Issuing req_account_anonymous_login with cookie=2
* evt_account_login_state_change: .xyzzy.alice. login_state_logging_in
* Request req_account_anonymous_login with cookie=2 completed.
* evt_account_login_state_change: .xyzzy.alice. login_state_logged_in
```

Example 2 (unknown):
```unknown
[SDKSampleApp]: conn
* Connecting to http://mt1s.www.vivox.com/api2 with connector handle http://mt1s.www.vivox.com/api2...
* Issuing req_connector_create with cookie=1
* Request req_connector_create with cookie=1 completed.
[SDKSampleApp]: login -u .xyzzy.alice.
* Logging .xyzzy.alice. in with connector handle http://mt1s.www.vivox.com/api2 and account handle .xyzzy.alice.
* Issuing req_account_anonymous_login with cookie=2
* evt_account_login_state_change: .xyzzy.alice. login_state_logging_in
* Request req_account_anonymous_login with cookie=2 completed.
* evt_account_login_state_change: .xyzzy.alice. login_state_logged_in
```

---

## Example: Drop_All token

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/access-token-guide/access-token-examples/example-drop-all-token

**Contents:**
- Example: Drop_All token#

The token in this example allows the user "blindmelon-AppName-dev-Admin" to drop all users from the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Because this is a kick command, the vxa is "kick".

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

**Examples:**

Example 1 (unknown):
```unknown
{
    "vxi":729614,
    "f":"sip:blindmelon-AppName-dev-Admin@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"kick",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 2 (unknown):
```unknown
{
    "vxi":729614,
    "f":"sip:blindmelon-AppName-dev-Admin@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"kick",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 3 (unknown):
```unknown
e30.eyJ2eGkiOjcyOTYxNCwiZiI6InNpcDpibGluZG1lbG9uLUFwcE5hbWUtZGV2LUFkbWluQHRsYS52aXZveC5jb20iLCJpc3MiOiJibGluZG1lbG9uLUFwcE5hbWUtZGV2IiwidnhhIjoia2ljayIsInQiOiJzaXA6Y29uZmN0bC1nLWJsaW5kbWVsb24tQXBwTmFtZS1kZXYudGVzdGNoYW5uZWxAdGxhLnZpdm94LmNvbSIsImV4cCI6MTYwMDM0OTQwMH0.bvNDTcXIHRpckAiFzy3wPiwgYGks4E9_WuEA5HMGpGE
```

Example 4 (unknown):
```unknown
e30.eyJ2eGkiOjcyOTYxNCwiZiI6InNpcDpibGluZG1lbG9uLUFwcE5hbWUtZGV2LUFkbWluQHRsYS52aXZveC5jb20iLCJpc3MiOiJibGluZG1lbG9uLUFwcE5hbWUtZGV2IiwidnhhIjoia2ljayIsInQiOiJzaXA6Y29uZmN0bC1nLWJsaW5kbWVsb24tQXBwTmFtZS1kZXYudGVzdGNoYW5uZWxAdGxhLnZpdm94LmNvbSIsImV4cCI6MTYwMDM0OTQwMH0.bvNDTcXIHRpckAiFzy3wPiwgYGks4E9_WuEA5HMGpGE
```

---

## UGS package version guide

**URL:** https://docs.unity.com/ugs/en-us/manual/overview/manual/sdk-upgrades

**Contents:**
- UGS package version guide#
- Package versions to use#
- Install recommended service packages in your project#
- Upgrade scenarios#
  - My Editor version supports UGS release packages#
  - My Editor version does not support UGS release packages#
  - I want to upgrade Analytics and Remote Config but can't see them in the Package Manager#

In order to take advantage of the newest UGS features and updates, Unity recommends upgrading all projects using beta versions of UGS services to the latest released version.

Compare your project’s installed package versions with the following minimum recommended versions for each service. Any packages with a lower version number than the minimum versions listed below are not recommended and should be updated.

The services listed above are compatible with any Unity Editor version 2020.3 or higher. However, depending on your specific Editor version, the service's packages may not be available to install from the Package Manager window. The following table shows which Editor releases to use in order to ensure that each service is fully compatible:

Note: If you do not see supported versions of Analytics and Remote Config packages in the Package Manager window for Editor versions 2021.3, 2022.1, or 2022.2, please see the below upgrade scenario.

In this scenario, you can download and install all packages from the Unity Editor’s Package Manager registry. To download and install packages using the Package Manager:

You can also type “services” in the search bar, which returns results for all the services except Remote Config.

In Editor versions 2022.1 or higher, you’ll find all services in the Package Manager’s Services tab.

For Editor versions 2020.3 or higher, the easiest way to install packages in the project is to include them directly in the project manifest with the following code:

Note: Analytics and Authentication are the only services that are compatible with Editor version 2019.4. Other UGS services have not been verified for this specific version.

You can add the following code to the package manifest:

Or, you can manually install the packages by name using the following versions:

**Examples:**

Example 1 (unknown):
```unknown
{
    "dependencies": {
        "com.unity.services.analytics": "4.0.1",
        "com.unity.services.authentication": "2.0.0",
        "com.unity.services.cloudcode": "2.0.0",
        "com.unity.services.cloudsave": "2.0.0",
        "com.unity.services.economy": "2.0.2",
        "com.unity.services.lobby": "1.0.1",
        "com.unity.services.relay": "1.0.2",
        "com.unity.remote-config": "3.0.0"
    }
}
```

Example 2 (unknown):
```unknown
{
    "dependencies": {
        "com.unity.services.analytics": "4.0.1",
        "com.unity.services.authentication": "2.0.0",
        "com.unity.services.cloudcode": "2.0.0",
        "com.unity.services.cloudsave": "2.0.0",
        "com.unity.services.economy": "2.0.2",
        "com.unity.services.lobby": "1.0.1",
        "com.unity.services.relay": "1.0.2",
        "com.unity.remote-config": "3.0.0"
    }
}
```

Example 3 (unknown):
```unknown
"com.unity.services.analytics": "4.0.1",
        "com.unity.remote-config": "3.0.0"
```

Example 4 (unknown):
```unknown
"com.unity.services.analytics": "4.0.1",
        "com.unity.remote-config": "3.0.0"
```

---

## Offline direct messages

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/text-chat-guide/offline-direct-messages

**Contents:**
- Offline direct messages#

Note: This feature is in Open Beta .

When you send a message to an offline user you will receive a evt_account_send_message_failed event. This can be used to relay the fact that the user receiving said message is offline. The message gets archived and when the user that received that message signs in, they can retrieve their account chat history to see the message that was sent to them while offline.

**Examples:**

Example 1 (unknown):
```unknown
evt_account_send_message_failed
```

---

## Access token payload

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/access-token-guide/access-token-format/access-token-payload

**Contents:**
- Access token payload#
- Unity Authentication IDs within payloads#

The payload is a base64url-encoded JSON object that contains the claims asserted by the token. You can list claims in the payload in any order.

The following table details the claims that you can use in a token. For more information on which parameters are required for each token, refer to Access token examples.

If your application is using Unity Authentication Service to authenticate players, you can use the UAS ID within a Vivox access token to ensure proper player identity. SIP URIs, which are used to identify players, must be in the proper format to ensure that the player ID is parsed from the full SIP URI. This format is mandatory for using any Unity Moderation service with VATs.

The following is an example of the expected SIP format:

It is the f and sub fields that will need this format to work with UAS.

Note: Ensure that you only include the parameters from the example. Claims that contain the wrong parameters return invalid signature or malformed payload errors on the API calls.

Guarantee token uniqueness.

If all other claims are identical, this must be different or the token could be rejected as already used.

Note: It's recommended that you use an unsigned integer that is increased by 1 for every token that is generated.

sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com

A user SIP URI that is used for mute and kick actions.

This is the user who is muted or unmuted, or the user who is kicked.

sip:.blindmelon-AppName-dev.beef.@tla.vivox.com

This is a user SIP URI that is used in all actions.

This is the user who performs actions such as signing in, joining the channel, or muting another user.

blindmelon-AppName-dev

An application-specific issuer.

The issuer is provided when you create an application on the Unity Dashboard.

For more information, see Supported values for the Vivox action claim.

sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com

This is a channel SIP URI that is used in join, mute, kick, and transcription actions.

This is the channel where the action takes place.

The expiration time as epoch seconds.

This value is usually the current time plus 90 seconds.

**Examples:**

Example 1 (unknown):
```unknown
sip:.issuer.unity_player_id.unity_environment_id.@domain.vivox.com
```

Example 2 (unknown):
```unknown
sip:.issuer.unity_player_id.unity_environment_id.@domain.vivox.com
```

Example 3 (unknown):
```unknown
invalid signature
```

Example 4 (unknown):
```unknown
malformed payload
```

---

## Run the SDKSampleApp

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/sdksampleapp/run-sdksampleapp

**Contents:**
- Run the SDKSampleApp#

Run the SDKSampleApp by using a command line similar to the following example. Replace "xyzzy" and "collosal" with your issuer and key.

Note: The "-m" command line option puts the SDKSampleApp into multitenant mode, which causes generated user names and channel names to be prefixed with the issuer.

In this example, the lines starting with " * " are requests, responses, and events.

The login command for this case generates the username .xyzzy.sa_c78d1595, which consists of the issuer and a random string that starts with "sa_" (for "sample app").

**Examples:**

Example 1 (unknown):
```unknown
SDKSampleApp --server=https://mt1s.www.vivox.com/api2 --realm=mt1s.vivox.com --issuer=xyzzy --key=collosal -m
[SDKSampleApp]: connect
* Connecting to http://mt1s.www.vivox.com/api2 with connector handle http://mt1s.www.vivox.com/api2...
* Issuing req_connector_create with cookie=1
* Request req_connector_create with cookie=1 completed.
[SDKSampleApp]: login
* Logging .xyzzy.sa_c78d1595. in with connector handle http://mt1s.www.vivox.com/api2 and account handle .xyzzy.sa_c78d1595.
* Issuing req_account_anonymous_login with cookie=2
* evt_account_login_state_change: .xyzzy.sa_c78d1595. login_state_logging_in
* Request req_account_anonymous_login with cookie=2 completed.
* evt_account_login_state_change: .xyzzy.sa_c78d1595. login_state_logged_in
```

Example 2 (unknown):
```unknown
SDKSampleApp --server=https://mt1s.www.vivox.com/api2 --realm=mt1s.vivox.com --issuer=xyzzy --key=collosal -m
[SDKSampleApp]: connect
* Connecting to http://mt1s.www.vivox.com/api2 with connector handle http://mt1s.www.vivox.com/api2...
* Issuing req_connector_create with cookie=1
* Request req_connector_create with cookie=1 completed.
[SDKSampleApp]: login
* Logging .xyzzy.sa_c78d1595. in with connector handle http://mt1s.www.vivox.com/api2 and account handle .xyzzy.sa_c78d1595.
* Issuing req_account_anonymous_login with cookie=2
* evt_account_login_state_change: .xyzzy.sa_c78d1595. login_state_logging_in
* Request req_account_anonymous_login with cookie=2 completed.
* evt_account_login_state_change: .xyzzy.sa_c78d1595. login_state_logged_in
```

---

## Voice activity detection

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/troubleshooting/voice-activity-detection-unreal

**Contents:**
- Voice activity detection#

Voice activity detection detects the presence or absence of speech in an application. In most cases, customers do not need to adjust the default voice activity detection (VAD) settings of the Vivox SDK.

The Vivox Unreal SDK does not provide an API to adjust the VAD settings. If you want to modify the code for the Unreal wrapper, you can add support for the vx_req_aux_set_vad_properties request from the Core SDK. You can manipulate the VAD mic gating by using these requests and handling their responses.

To use vx_req_aux_set_vad_properties and vx_req_aux_get_vad_properties, you need to add these requests to an existing endpoint or an exposed function of the Unity API, or create a new endpoint. To confirm your successful requests, ensure that you handle the responses.

Before manually tuning the VAD settings, test your setup using Automatically Adjusted VAD. Automatically Adjusted VAD is better at detecting a player speaking than the default VAD settings.

Note: Use vad_auto when a player uses the onboard microphone and speakers on PCs and mobile devices. Players using high-quality Bluetooth earbuds, headsets or USB headsets might not benefit from vad_auto as many of those devices have built-in noise reduction or directional microphones that already produce a clean voice signal.

Set vad_auto to 1 in the vx_req_aux_set_vad_properties call to enable automatic adjustment. See the example shown below to enable vad_auto:

Note: Although the VAD ignores the other settings when vad_auto is enabled, you must provide valid parameters or the call will return an error. This example provides the default values for the vad_hangover, vad_sensitivity, and vad_noise_floor.

If the Automatically Adjusted VAD does not help, disable vad_auto by setting it to 0 and provide values for vad_hangover, vad_sensitivity, and vad_noise_floor.

Note: Changes to the VAD noise floor settings do not affect currently joined channels. If the ability to change VAD settings is available to the end-user, indicate that noise floor changes only take effect in the next voice session or only allow changing the noise floor channel when the client is not in-channel.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_aux_set_vad_properties
```

Example 2 (unknown):
```unknown
vx_req_aux_set_vad_properties
```

Example 3 (unknown):
```unknown
vx_req_aux_get_vad_properties
```

Example 4 (unknown):
```unknown
vx_req_aux_set_vad_properties
```

---

## Initiate a channel join

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/initiate-channel-join

**Contents:**
- Initiate a channel join#

The following code displays an example of how to initiate a channel join:

Note: The vx_resp_sessiongroup_add_session message does not indicate that the user has successfully joined the channel.

The game must process two types of event messages: vx_evt_media_stream_updated and vx_evt_text_stream_updated. The Vivox SDK posts these messages when there are changes in the connectivity to the audio or text portions of the session.

The following code displays an example of how to handle these events:

Note: These events might be generated in response to messages from the Vivox network (for example, when a user is forcibly removed from a channel by the game server).

**Examples:**

Example 1 (unknown):
```unknown
. . .
vx_req_sessiongroup_add_session *req;
vx_req_sessiongroup_add_session_create(&req);
req->sessiongroup_handle = vx_strdup("sg1");
req->session_handle = vx_strdup("mychannel");
req->uri = vx_strdup("sip:confctl-g-issuer.mychannel@mt1s.vivox.com");
req->account_handle = vx_strdup("sip:.issuer.playerName@mt1s.vivox.com");
req->connect_audio = 1;
req->connect_text = 1;
req->access_token = vx_strdup(_the_access_token_generated_by_the_game_server);
vx_issue_request3(&req->base, &request_count);
. . .
```

Example 2 (unknown):
```unknown
. . .
vx_req_sessiongroup_add_session *req;
vx_req_sessiongroup_add_session_create(&req);
req->sessiongroup_handle = vx_strdup("sg1");
req->session_handle = vx_strdup("mychannel");
req->uri = vx_strdup("sip:confctl-g-issuer.mychannel@mt1s.vivox.com");
req->account_handle = vx_strdup("sip:.issuer.playerName@mt1s.vivox.com");
req->connect_audio = 1;
req->connect_text = 1;
req->access_token = vx_strdup(_the_access_token_generated_by_the_game_server);
vx_issue_request3(&req->base, &request_count);
. . .
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

## Write modules

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/write-modules

**Contents:**
- Write modules#

Write modules using different authoring methods.

---

## Custom XML Namespaces

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/chat-history/edit-delete/custom-xml

**Contents:**
- Custom XML Namespaces#

These are custom XML namespaces to route edit/delete XMPP requests to Vivox handlers.

---

## Unity Docs

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/includes/access-token-guide-general-link

For more information, refer to the Access Token Developer Guide.

---

## iOS build can't download or install from Unity Build Automation

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/check-build-results/troubleshoot-build-failures/ios-build-download-issue

**Contents:**
- iOS build can't download or install from Unity Build Automation#
- Symptoms#
  - Identify the issue#
- Potential causes#
- Resolutions#
  - No Install - Your device is not provisioned for this build.#
    - Register your UDID#
    - Reset your device session#
    - Use Safari#
  - Unable to Download App#

Show the issue in the console:

There are several possible causes:

Note: There might be additional possible causes. Resolutions for some issues might not work on every version of iOS.

The iOS build download displays the message: No Install - Your device is not provisioned for this build.

The most likely cause of this error message is that your UDID isn't registered in the development or distribution mobile provisioning profile.

To find and register your UDID do the following:

NOTE: Some third party apps that provide you with your UDID might report fake UDIDs (possibly starting with fffff). Always use the UDID reported by the system.

Sometimes, you might get this error message if the current session of the device is broken, for example, if you apply a backup of your phone or the session cookies get corrupted. To resolve the session issue, go to the device reset page to reset the device session information. The page then redirects you back to the Unity Build Automation page. If you are not redirected, visit the Unity Dashboard to log in and log out again to complete the reset process.

Use Safari as your browser and ensure that it's not in private mode. To clear your Safari cache and cookies, go to the Safari > Settings. Unity Build Automation is not guaranteed to work in other browsers on iOS.

The Unable to Download App dialog displays when you try to download the build.

This error can occur if your internet connection isn't stable enough to download the build. Ensure with your internet service provider that your connection is stable and make sure that you have enough free space on your device. To check if the issue is the internet connection, you can test a different connection such as 3G or a wired connection.

This error can occur if the certificate you use for the provisioning profile was revoked or deleted. To resolve this issue, update the certificate and regenerate the provisioning profile. After you update the files in the Unity Dashboard, rebuild the application to enact the change.

For more possible causes and troubleshooting steps, refer to this Stack Overflow post.

The Unable to Download Application - AppName could not be installed at this time error displays as an iOS dialog after a successful download.

Additionally to the above resolutions, ensure that the device that you want to use to install the build supports the architecture of the build.

For more possible causes and troubleshooting steps, refer to this Stack Overflow post.

For more information about the iOS build process, refer to sign an iOS application.

Discuss the issue in the forums on the Unity Discussions website.

If you need more information about distribution profiles, refer to the Apple documentation on Resolving the Provisioning Profile Invalid Status.

**Examples:**

Example 1 (unknown):
```unknown
No Install - Your device is not provisioned for this build.
```

Example 2 (unknown):
```unknown
Unable to Download App
```

Example 3 (unknown):
```unknown
Unable to Download Application - AppName could not be installed at this time.
```

Example 4 (unknown):
```unknown
No Install - Your device is not provisioned for this build.
```

---

## Set up an iOS build configuration

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/basic-build-configuration/set-up-an-ios-build-configuration

**Contents:**
- Set up an iOS build configuration#
- A note on the Apple Privacy Manifest#
- Xcode frameworks#

Build Automation helps you automate the process of building your Unity Project for iOS devices. There are a number of prequisites for iOS development. Note that Apple processes and requirements do change over time, so it's best to refer to the Apple developer website should you encounter issues.

Apple is introducing a privacy policy for including privacy manifest files in new and updated applications targeted for iOS, iPadOS, tvOS, and visionOS platforms on the App Store.

The privacy manifest file PrivacyInfo.xcprivacy lists the types of data your Unity applications collect (or any third-party SDKs, packages, and plug-ins), and the reasons for using certain Required Reason API categories. Apple also requires that certain domains be declared as "tracking" and they may be blocked unless a user provides consent.

Important: If the use of the Required Reasons APIs by you or third-party SDKs isn’t declared in the privacy manifest, your application might be rejected by the App Store. For more information, visit Apple’s documentation on Required Reasons APIs.

Unity Build Automation does not collect data or engage in any data practices requiring disclosure in a privacy manifest file.

If your iOS Project requires additional Xcode frameworks, use the PBXProject API to add those frameworks to the Xcode Project files created by Unity Build Automation.

For an example of a Unity Project that uses the API, see the UpdateXcodeProject example Project on GitHub. You can use the example to experiment and to learn from.

One of the plugins of the example Project is an external Xcode project manipulation DLL. The DLL is the build product of the source available in Unity’s Bitbucket repository. A preferred way to include Xcode project manipulation functionality is to copy the C# source code files to the Assets/Editor folder in your Project.

There are two ways to call this API to manipulate the Xcode project:

**Examples:**

Example 1 (unknown):
```unknown
PrivacyInfo.xcprivacy
```

---

## Manage build configurations

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/guides/manage-build-configurations

**Contents:**
- Manage build configurations#
- Create a build configuration#
- Update a build configuration#
- Remove a build configuration#
- Delete a build configuration#
- Change the active build configuration of a server#
- Add a configuration variables#
- Update usage settings#

A build configuration manages the build it’s linked to by dictating the query protocol, the application executable path, and the launch parameters. You can manage build configurations within your account from the Unity Dashboard. Refer to the sections below to learn how to perform basic build configuration tasks, such as creating a build configuration and updating a build configuration.

Refer to Create a build configuration.

To update an existing build configuration:

To remove a build configuration from a fleet:

To delete a build configuration:

Note: You must remove the build configuration from all fleets before deleting it.

The only way to change the active build configuration of a specific game server is to specify a different build configuration with an allocation call. Refer to Active build configuration.

To add configuration variables to a build configuration:

To update usage settings of a build configuration:

---

## Phase 2: Implementation

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/integration-guide/integration-plan/phase-two/implementation-overview

**Contents:**
- Phase 2: Implementation#

In this phase, the integrating developer uses and experiences Vivox voice features through a small application and begins integrating those features into their game.

A sample application is provided as a part of the Vivox SDK download. You can build and debug the sample application, and it can serve as either the basis for your official implementation or as a reference that you can use when configuring the appropriate behavior for your game.

Implementation generally uses the following process:

For more detailed implementation steps, refer to the Quickstart guide.

---

## Network reconnect failure

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/channels/automatic-connection-recovery/unity-reconnect-failure

**Contents:**
- Network reconnect failure#

If the connection recovery process is ultimately unable to reconnect to a session, VivoxService.Instance.ConnectionFailedToRecover action will fire. This only occurs when all attempts to reestablish communication with Vivox servers have failed. We recommend that you indicate a network reconnection failure to users because user action might be required to address any issues with their network connection health.

During connection issues VivoxService.Instance.ConnectionRecovering will fire.

When the connection is recovered VivoxService.Instance.ConnectionRecovered will fire.

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.ConnectionFailedToRecover
```

Example 2 (unknown):
```unknown
VivoxService.Instance.ConnectionRecovering
```

Example 3 (unknown):
```unknown
VivoxService.Instance.ConnectionRecovered
```

---

## Open UDP ports during Windows app installation

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/windows/open-udp-ports

**Contents:**
- Open UDP ports during Windows app installation#
- Setting UDP ports#

Note: In Vivox Core SDK version 5.21 and above, Vivox no longer requires binding Windows ports. You can ignore instructions on this page if you aren’t working in a restricted environment that requires specifying specific ports and are using a Vivox version of 5.21 or higher.

When writing a Windows application you can avoid opening ports for your application by setting the minimun_port and maximum_port in vx_config to 0. This is the default setting in Windows.

If you don’t use the default or set the ports to zero, you must ensure that your app installer runs with elevated privileges and that you open User Datagram Protocol (UDP) ports for your app. This prevents Windows Defender from prompting users to allow the app, which can occur when they join a channel for the first time if UDP ports are not open during the installation of a Windows app.

To open the UDP ports, run your app installer with elevated privileges and then perform either of the following actions:

The following code displays an example of how to open all UDP ports:

**Examples:**

Example 1 (unknown):
```unknown
minimun_port
```

Example 2 (unknown):
```unknown
maximum_port
```

Example 3 (unknown):
```unknown
minimum_port
```

Example 4 (unknown):
```unknown
maximum_port
```

---

## Required Info.plist settings for iOS apps

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/ios/ios-requirements

**Contents:**
- Required Info.plist settings for iOS apps#

The following Info.plist settings need to be used for the Vivox SDK to work on iOS devices.

UIBackgroundModes key

To stay connected to Vivox when an app is put in the background or if a device is locked, use the UIBackgroundModes Info.plist key and allow the audio setting. This setting allows users to continue to stream voice audio and use the microphone while the app is running in the background or the screen is off. The UIBackgroundModes key can be added to the Xcode project settings. If this setting is missing, the user will be disconnected from the channel and logged out after several seconds in the background, or the user will be logged out immediately when the device is locked.

NSMicrophoneUsageDescription property

The NSMicrophoneUsageDescription property must be included in your Info.plist in your Xcode project settings. Use it to describe what purpose your app will use the device’s microphone for; this will be displayed when requesting microphone permission.

You can view code samples for these settings and entitlements in the SDKSampleApp under /Source/MediaCaptureAuth.cpp in the iOS distribution files.

For more information, see How to: Request/check iOS & macOS microphone permission in Unity or Apple’s official documentation for requesting authorization of media capture on iOS.

---

## Voice activity detection

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/troubleshooting/voice-activity-detection-core

**Contents:**
- Voice activity detection#

Voice activity detection detects the presence or absence of speech in an application. In most cases, customers do not need to adjust the default voice activity detection (VAD) settings of the Vivox SDK. Before manually tuning the VAD settings, test your setup using Automatically Adjusted VAD. Automatically Adjusted VAD is better at detecting a player speaking than the default VAD settings.

Note: Use vad_auto when a player uses the onboard microphone and speakers on PCs and mobile devices. Players using high-quality Bluetooth earbuds, headsets or USB headsets might not benefit from vad_auto as many of those devices have built-in noise reduction or directional microphones that already produce a clean voice signal.

Set vad_auto to 1 in the vx_req_aux_set_vad_properties call to enable automatic adjustment. See the example shown below to enable vad_auto:

Note: Although the VAD ignores the other settings when vad_auto is enabled, you must provide valid parameters or the call will return an error. This example provides the default values for the vad_hangover, vad_sensitivity, and vad_noise_floor.

If the Automatically Adjusted VAD does not help, disable vad_auto by setting it to 0 and provide values for vad_hangover, vad_sensitivity, and vad_noise_floor.

Note: Changes to the VAD noise floor settings do not affect currently joined channels. If the ability to change VAD settings is available to the end-user, indicate that noise floor changes only take effect in the next voice session or only allow changing the noise floor channel when the client is not in-channel.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_aux_set_vad_properties
```

Example 2 (unknown):
```unknown
vx_req_aux_set_vad_properties_t *req;
vx_req_aux_set_vad_properties_create(&req);
req->vad_hangover = 2000;
req->vad_sensitivity = 43;
req->vad_noise_floor = 576;
req->vad_auto = 1;
vx_issue_request(&req->base);
```

Example 3 (unknown):
```unknown
vx_req_aux_set_vad_properties_t *req;
vx_req_aux_set_vad_properties_create(&req);
req->vad_hangover = 2000;
req->vad_sensitivity = 43;
req->vad_noise_floor = 576;
req->vad_auto = 1;
vx_issue_request(&req->base);
```

Example 4 (unknown):
```unknown
vad_hangover
```

---

## Unity Build Automation

**URL:** https://docs.unity.com/devops/en/manual/unity-build-automation

**Contents:**
- Unity Build Automation#
- What is Unity Build Automation?#
- Key concepts#
  - Continuous integration#
  - How automated build generation works#
  - Source Control#
- Unity Versions#
- Supported Platforms#
- Additional resources#

Unity Build Automation is a continuous integration service that automatically creates multiplatform builds in the Cloud. You can point Build Automation toward your version control system to:

Build Automation supports most popular version control systems and builds for multiple platforms simultaneously, including iOS.

You can access Unity Build Automation through the Unity Dashboard.

Build Automation provides continuous integration by automatically building your project whenever changes are detected in the configured version control system.

By compiling your project whenever a change is committed, Build Automation gives you the most accurate idea of when and where errors occur and ensures you always have a build of your game based on the last working commit.

To enable automated build generation, set the Auto-build toggle to Yes in Build Automation > Configurations > Basic Info settings.

Build Automation connects to either Unity Version Control or your source control system and monitors that system for changes to your project. When it detects changes to your project, Build Automation downloads and builds your project for your target platforms. When the builds are complete, Build Automation notifies you of the results and links to download, share, and install the builds. If there are errors, Build automation informs you immediately so you can quickly fix them, commit the changes, and trigger new builds.

You can integrate different software for notifications such as Slack, Discord, and Jira. For more information, refer to integrations.

Source Control is a system for managing file changes. You can use Build Automation in conjunction with most version control tools, including UVCS, Perforce, and Git. Refer to Connect Your Version Control System.

In alignment with Unity’s platform-wide support policy, we only answer support tickets for LTS releases and the latest TECH release. For more information on Unity’s support policy for editor releases, refer to LTS releases.

Build Automation supports a wide range of platforms, enabling you to build projects for various targets efficiently. Below is the list of platforms currently supported by Build Automation:

Note: Refer to the available versions for Xcode and Android.

---

## Player evidence report HTTP request

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/chat-history/player-evidence-report-request

**Contents:**
- Player evidence report HTTP request#

The following code is an example of a player evidence request:

Note: Add the request_start_ts parameter to the request if using pagination to make subsequent requests.

**Examples:**

Example 1 (unknown):
```unknown
curl https://services.api.unity.com/vivox/text-evidence-management/v1/organizations/<organization_id>/projects/<project_id>/player-evidence-report?user_uri=sip:your-issuer.john@yourvivoxdomainname.vivox.com&start_ts=1234567&end_ts=1234568&num_conversations=1 -u service-acct-username:service-account-password
```

Example 2 (unknown):
```unknown
curl https://services.api.unity.com/vivox/text-evidence-management/v1/organizations/<organization_id>/projects/<project_id>/player-evidence-report?user_uri=sip:your-issuer.john@yourvivoxdomainname.vivox.com&start_ts=1234567&end_ts=1234568&num_conversations=1 -u service-acct-username:service-account-password
```

Example 3 (unknown):
```unknown
request_start_ts
```

---

## Channel management

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/channels/channel-management

**Contents:**
- Channel management#

Channel management primarily focuses on selecting appropriate channel IDs. and identifying code paths for joining and leaving channels, in addition to authorizing those operations.

By default, channels are named and created on an ad-hoc basis. The process for joining channels is regulated by a game server authorization code prior to joining. The selection of appropriate unique channel IDs is important, as is associating the IDs with relevant in-game data, such as parties, teams, lobbies, and guilds. You can use this identifier in an authenticated channel join request to allow a user to perform an authorized join.

By default, channels have the following properties:

Special channel types also exist that you can use in certain specific or unusual circumstances. For example: the echo channel, static channels, and the 3D positional channel. These special channel types require additional setup and configuration, demand additional resources to provision, and are recommended only for use in cases where standard ephemeral channels are inadequate.

The following list details special channels and their properties:

Persistent/semi-persistent channels

3D positional audio channels

Note: Default channels are generally suitable for most use cases. Echo channels are useful for both internal development and testing, and for providing an audio test environment for the user.

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/mps-sdk/manual/build-with-netcode-for-entities

---

## Couch co-op example workflow

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/couch-co-op/couch-co-op-example-workflow

**Contents:**
- Couch co-op example workflow#

To initiate a multi-user play session with two users, a game might make the following series of API calls and requests.

In the following example, the names C, A1, A2, G1, G2, S1, S2, Render1, Render2, Capture1, and Capture2 are placeholders that identify a connector handle, account handles, session group handles, session handles, render device IDs, and capture device IDs, respectively.

User 1 is now in session S1 and is using devices Render1 and Capture1. User 2 is now in session S2 and is using devices Render2 and Capture2.

**Examples:**

Example 1 (unknown):
```unknown
vx_initialize()
vx_req_connector_create(connector_handle=C)
vx_req_account_anonymous_login(connector_handle=C, account_handle=A1) // First user logs in
vx_req_account_anonymous_login(connector_handle=C, account_handle=A2) // Second user logs in
vx_req_set_render_device(account_handle=A1, render_device_specifier=Render1)
vx_req_set_capture_device(account_handle=A1, capture_device_specifier=Capture1)
vx_req_set_render_device(account_handle=A2, render_device_specifier=Render2)
vx_req_set_capture_device(account_handle=A2, capture_device_specifier=Capture2)
vx_req_sessiongroup_create(account_handle=A1, sessiongroup_handle=G1)
vx_req_sessiongroup_create(account_handle=A2, sessiongroup_handle=G2)
vx_req_sessiongroup_addsession(sessiongroup_handle=G1, session_handle=S1)
vx_req_sessiongroup_addsession(sessiongroup_handle=G2, session_handle=S2)
```

Example 2 (unknown):
```unknown
vx_initialize()
vx_req_connector_create(connector_handle=C)
vx_req_account_anonymous_login(connector_handle=C, account_handle=A1) // First user logs in
vx_req_account_anonymous_login(connector_handle=C, account_handle=A2) // Second user logs in
vx_req_set_render_device(account_handle=A1, render_device_specifier=Render1)
vx_req_set_capture_device(account_handle=A1, capture_device_specifier=Capture1)
vx_req_set_render_device(account_handle=A2, render_device_specifier=Render2)
vx_req_set_capture_device(account_handle=A2, capture_device_specifier=Capture2)
vx_req_sessiongroup_create(account_handle=A1, sessiongroup_handle=G1)
vx_req_sessiongroup_create(account_handle=A2, sessiongroup_handle=G2)
vx_req_sessiongroup_addsession(sessiongroup_handle=G1, session_handle=S1)
vx_req_sessiongroup_addsession(sessiongroup_handle=G2, session_handle=S2)
```

Example 3 (unknown):
```unknown
vx_req_sessiongroup_removesession(sessiongroup_handle=G1, session_handle=S1)
vx_req_sessiongroup_removesession(sessiongroup_handle=G2, session_handle=S2)
vx_req_sessiongroup_terminate(sessiongroup_handle=G1)
vx_req_sessiongroup_terminate(sessiongroup_handle=G2)
vx_req_account_logout(account_handle=A1)
vx_req_account_logout(account_handle=A2)
vx_req_connector_initiate_shutdown(connector_handle=C)
vx_uninitialize()
```

Example 4 (unknown):
```unknown
vx_req_sessiongroup_removesession(sessiongroup_handle=G1, session_handle=S1)
vx_req_sessiongroup_removesession(sessiongroup_handle=G2, session_handle=S2)
vx_req_sessiongroup_terminate(sessiongroup_handle=G1)
vx_req_sessiongroup_terminate(sessiongroup_handle=G2)
vx_req_account_logout(account_handle=A1)
vx_req_account_logout(account_handle=A2)
vx_req_connector_initiate_shutdown(connector_handle=C)
vx_uninitialize()
```

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/guides/configure-core-dumps

---

## Participant management

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/channels/participant-events

**Contents:**
- Participant management#
- VivoxParticipant#

The Vivox SDK posts information about individual participants in a channel that is visible to all other participants. This includes the following information:

Handling participant events is optional. The game can ignore these events if there is no visualization of the user state (for example, displaying who has voice enabled).

To provide a visualization of user state information, a game must handle the following messages:

ParticipantAddedToChannel and ParticipantRemovedFromChannel both come with a VivoxParticipant. VivoxParticipant contains information about the participant that was just added, such as:

VivoxParticipant also contains the current state of that participant, including:

VivoxParticipants should be tightly coupled to the UI representation of the participant with VivoxParticipant.ParticipantMuteStateChanged and VivoxParticipant.ParticipantSpeechDetected to convey whether the local player has the participant muted, or if the participant is currently speaking in the channel.

You can use VivoxParticipant.ParticipantAudioEnergyChanged to create a more accurate volume unit (VU) meter then SpeechDetected allows for.

The following code, which is a simplified segment from the Vivox ChatChannelSample, is an example of these systems:

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.ParticipantAddedToChannel
```

Example 2 (unknown):
```unknown
VivoxService.Instance.ParticipantRemovedFromChannel
```

Example 3 (unknown):
```unknown
VivoxParticipant.ParticipantMuteStateChanged
```

Example 4 (unknown):
```unknown
VivoxParticipant.ParticipantSpeechDetected
```

---

## Evidence management

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/safety/moderation-evidence

**Contents:**
- Evidence management#

Important: When using the Vivox Channel Cache in the Moderation Unreal Module, all Vivox AccountIDs and ChannelIDs must be constructed with the Environment ID of the Unity Cloud Project passed into the unityEnvironmentID param. The following is an example of the format to follow:

When using the Vivox Unreal Module, the SDK automatically includes the list of channels the reporter and reported players are part of. As well as the list of extra players who were part of those channels.

This information is used to add details to the evidence in the report. The extra players' tracks will be included in the Safe Voice screenings and each channel in the channel list will be screened.

To ensure the voice sessions are captured and analyzed by Safe Voice, ensure your players have provided consent.

To begin the Vivox Channel Cache, pass the AccountId of the current LoginSession to UModerationSubsystem::BeginVivoxCache(AccountId accountIdToCache).

Note: If the AccountId is not included to identify the LoginSession, the Moderation Subsystem will search for an active LoginSession to cache, prioritizing those that are currently LoggedIn.

To better control the report context, you can alter the list of channels and players included in the report before it's sent.

Note: Channels and players are automatically cached for 45 minutes, this is the maximum time Safe Voice can go back in time to screen.

**Examples:**

Example 1 (unknown):
```unknown
AccountId(<Issuer>, <Name>, <Domain>,TOptional<FString>(), TOptional<TArray<FString>>(), <UnityEnvironmentId>)

ChannelId(<Issuer>, <Name>, <Domain>, ChannelType::NonPositional, Channel3DProperties(), <UnityEnvironmentId>);
```

Example 2 (unknown):
```unknown
AccountId(<Issuer>, <Name>, <Domain>,TOptional<FString>(), TOptional<TArray<FString>>(), <UnityEnvironmentId>)

ChannelId(<Issuer>, <Name>, <Domain>, ChannelType::NonPositional, Channel3DProperties(), <UnityEnvironmentId>);
```

Example 3 (unknown):
```unknown
UModerationSubsystem::BeginVivoxCache(AccountId accountIdToCache)
```

Example 4 (unknown):
```unknown
ModerationSubsystem->BeginVivoxCache(UserAccountId);
```

---

## Make a speaker history and TTL request

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/server-side-recording/ssr-make-speaker-ttl-and-history-request

**Contents:**
- Make a speaker history and TTL request#

The following code displays an example of how to pull 10 minutes of data for the target speaker. It gets 5 minutes of data in the past from the subscription time and continues to get data for 5 minutes into the future.

**Examples:**

Example 1 (unknown):
```unknown
curl -x POST
http://{{host}}/ssr/subscribe?auth_token={{auth_token}}&history=5&destination_credentials={"bucket":"bucket-name","access_key_id":"aws-access-id","secret_access_key":"aws-secret-key"}&target_uri=sip:.issuer.speaker.@domain.vivox.com&target_type=speaker&ttl=5
```

Example 2 (unknown):
```unknown
curl -x POST
http://{{host}}/ssr/subscribe?auth_token={{auth_token}}&history=5&destination_credentials={"bucket":"bucket-name","access_key_id":"aws-access-id","secret_access_key":"aws-secret-key"}&target_uri=sip:.issuer.speaker.@domain.vivox.com&target_type=speaker&ttl=5
```

---

## Text-to-speech destinations

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/text-to-speech/tts-destinations

**Contents:**
- Text-to-speech destinations#

Text-to-speech (TTS) messages are injected to destinations. Destinations determine two factors: output and mechanism.

Output is where the synthesized speech plays to and decides who hears the TTS message. Each destination uses one of the following outputs:

Mechanism is how new messages are handled when there is an ongoing message playing. Each destination uses one of the following injection mechanisms:

You can inject synthesized speech into the following destinations:

Note: Most destinations are independent and do not affect the behavior of any other destinations. However, the two queued destinations involving remote transmission share a queue.

Remote Transmission with Local Playback

Queued Remote Transmission

Queued Local Playback

Queued Remote Transmission with Local Playback

**Examples:**

Example 1 (unknown):
```unknown
tts_dest_remote_transmission
```

Example 2 (unknown):
```unknown
tts_dest_local_playback
```

Example 3 (unknown):
```unknown
tts_dest_remote_transmission_with_local_playback
```

Example 4 (unknown):
```unknown
tts_dest_queued_remote_transmission
```

---

## Retrieve the volume and mute status of a local device

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/muting/retrieve-volume-mute-status

**Contents:**
- Retrieve the volume and mute status of a local device#

To retrieve the current volume and mute status of the local speakers and microphone, use the vx_req_connector_get_local_audio_info request.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_connector_get_local_audio_info
```

Example 2 (unknown):
```unknown
vx_req_connector_mute_local_info_t* reqStruct;
vx_req_connector_mute_local_info_create(&reqStruct);
reqStruct->connector_handle = vx_strdup(“connector handle”);
reqStruct->account_handle = vx_strdup(“account handle”); // OPTIONAL

int ret = vx_issue_request3((vx_req_base_t*) reqStruct);
```

Example 3 (unknown):
```unknown
vx_req_connector_mute_local_info_t* reqStruct;
vx_req_connector_mute_local_info_create(&reqStruct);
reqStruct->connector_handle = vx_strdup(“connector handle”);
reqStruct->account_handle = vx_strdup(“account handle”); // OPTIONAL

int ret = vx_issue_request3((vx_req_base_t*) reqStruct);
```

---

## UI integration

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/integration-guide/integration-plan/phase-one/ui-integration/ui-integration

**Contents:**
- UI integration#
- Recommended in-game UI elements:#

Depending on the configuration and the number of voice communication features in your game, you must create various UI elements to display feedback for settings like volume and speaking indicators. Users also need access to controls for voice-related actions like configuring a microphone or loudspeaker, locally muting users, and selecting input and output devices.

The Vivox Client API operates by using a series of requests, responses, and generated events. After you create the UI elements, you can attach them to the signaling of requests or the processing of associated responses or events.

For more information, refer to the following UI examples.

---

## Manage regions

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/guides/manage-regions

**Contents:**
- Manage regions#
- Bring a region online#
- Take a region offline#
- Add a region#

A region is a collection of machines within a specific geographic region. You can manage regions within your account from the Unity Dashboard.

Refer to the following sections to learn how to:

The following instructions explain how to bring a region online from the Unity Dashboard.

Note: You can only bring a region online if it’s not already online.

The status text should update to Bringing online. The status updates again after the region is completely online.

The following instructions explain how to take a region offline from the Unity Dashboard.

Warning: Taking a region offline can impact players by limiting the available capacity in this region. If you allocate players in this region to a nearby region, they might experience higher latency than normal.

The status text should update to Draining. The status updates again after the region is completely offline.

The following instructions explain how to add a region to a fleet from the Unity Dashboard.

Note: Removing regions is not supported in Multiplay Hosting.

---

## Generate a token on the client

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/access-token-guide/generate-tokens-unity

**Contents:**
- Generate a token on the client#

There are two ways to handle token generation for the Vivox SDK:

---

## Access token examples

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/access-token-guide/access-token-examples/access-token-examples-toc

**Contents:**
- Access token examples#

The access token examples in this section include spacing in the payload section which has been added for demonstration purposes. Do not include any spacing in the payload for your token.

Note: In the payload section for each example, you can list the claims in any order.

---

## Channel-based chat history for blocked participants

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/chat-history/channel-based-ch-blocked-participant

**Contents:**
- Channel-based chat history for blocked participants#
- History sessions when blocked participants are involved#

You can filter messages from blocked users when formatting the history_session query responses.

When retrieving the chat history from a channel, the SDK omits messages from blocked users. If User A has User B blocked in the application, User A does not see messages sent by User B.

A history request can come back empty if all messages in the request are from a blocked participant. In this case, the response contains a cursor indicating that there are more messages in the channel on subsequent pages. The default amount of messages returned per history request is 10 per page. Use the cursor value in the following request to retrieve the next page of messages.

The maximum page size argument (Max) shows fewer messages than the number requested if there is a blocked user in the channel. When you request a specific page size for your chat history, be aware that history_session might display fewer messages than your chosen number if there are blocked users in the channel. This functionality is crucial to ensure privacy and prevent unwanted interactions.

The last read displays a count of unread messages. Vivox keeps track of your read messages and provides a count of unread messages. However, note that this count includes blocked messages, which are deliberately filtered from your response. The count of unread messages might be inaccurate if there are blocked users in the channel.

---

## Sign an Android application

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/sign-build-artifacts/sign-an-android-application

**Contents:**
- Sign an Android application#
- Create a keystore#
- Configure Unity Build Automation signing#

Unity Build Automation signs your Android application during the build process. This ensures your Android application meets requirements to be installed on devices or deploy to the Google Play Store.

Android requires that all Android Package Kits (APKs) and Android App Bundles are digitally signed with a certificate before you can install them on devices or publish them. This certificate serves to identify the app's author and establish trust relationships between applications.

A keystore is a binary file that contains one or more private keys. You need to create a keystore to sign your Android application.

For detailed instructions on how to create a keystore with the Unity Editor's keystore Manager, refer to Create a new keystore in the Unity manual.

Important: Store your keystore file in a secure location and remember the passwords. If you lose your keystore, you can't do the following actions:

Associate your uploaded signing credentials with your Android build target:

Note: After you configure the signing credentials, you can queue a build that signs your Android artifacts automatically.

Note: You can select Auto-generated debug keystore (for development only) as your credentials set for development and testing. This automatically generates a debug keystore for development builds. To publish to the Google Play Store, you need to use a release keystore that you create and maintain, because you can't use the debug keystore for app distribution.

---

## Builds cancel immediately upon start

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/check-build-results/troubleshoot-build-failures/builds-failing-immediately

**Contents:**
- Builds cancel immediately upon start#
- Symptoms#
- Environment#
- Causes#
- Resolution#
  - Change to a pay-as-you-go plan#
  - Change your build minute cap#
  - Contact the support team#

When you launch builds from either the Build Automation configuration page or the build history in the Unity Dashboard, builds cancel immediately when they trigger.

The most common causes for this issue are the following:

On the DevOps menu from the Unity Cloud Dashboard, if the dashboard displays Free Plan instead of a consumption-based plan, as displayed in the following screenshot, you can register for a consumption based plan. To sign up for the service, navigate to the Unity Dashboard > DevOps > and select Upgrade plan.

If you have an active subscription, the next thing to check is if you have a build minute cap and whether your build exceeds that cap. To check this in the Unity Dashboard, navigate to Devops > Build Automation > Settings, and select the Build Consumption tab.

If the Build minutes monthly cap is enabled and you're near the limit, you can either disable or increase the cap to continue to use the service.

If you have validated your plan and build minute cap, and there are no current reported issues with the service (visible on the Build Automation sidebar under the DevOps menu), please contact to the Service Support team. To submit a ticket from the Unity Cloud Dashboard, open DevOps and select Help & Support > Ticket > File a ticket.

---

## Set Chat Markers

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/set-chat-markers

**Contents:**
- Set Chat Markers#

The set Chat Markers API gives you control over the creation of Chat Markers. You can use Chat Markers to mark messages as read. It also provides data for the Conversation list API. This functionality works in both channels and DMs.

To set a conversation's read marker, sign in to the application and use vx_req_account_chat_history_set_marker to make a request. This request returns whether the marker was created or not.

Note: The marker is created even if the URI or message doesn’t exist.

The following code is an example of how to set a conversation marker.

After you retrieve this information, the game must process the vx_resp_account_chat_history_set_marker_t response:

**Examples:**

Example 1 (unknown):
```unknown
vx_req_account_chat_history_set_marker
```

Example 2 (unknown):
```unknown
vx_req_account_chat_history_set_marker *req;
vx_req_account_chat_history_set_marker_create(&req);
req->with_uri = vx_strdup(”sip:confctl-g-issuer.room_name.@domain”); // Optional parameter, if not set it will use the channel you’ve joined most recently
req->message_id = vx_strdup(“1234567890”); // The message id you’re setting the marker for
req->seen_at = 1234567890; // Optional, unix timestamp which defaults to the current time if not set
vx_issue_request2(&req->base);
```

Example 3 (unknown):
```unknown
vx_req_account_chat_history_set_marker *req;
vx_req_account_chat_history_set_marker_create(&req);
req->with_uri = vx_strdup(”sip:confctl-g-issuer.room_name.@domain”); // Optional parameter, if not set it will use the channel you’ve joined most recently
req->message_id = vx_strdup(“1234567890”); // The message id you’re setting the marker for
req->seen_at = 1234567890; // Optional, unix timestamp which defaults to the current time if not set
vx_issue_request2(&req->base);
```

Example 4 (unknown):
```unknown
vx_resp_account_chat_history_set_marker_t
```

---

## Access token format

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/access-token-format/access-token-format-overview

**Contents:**
- Access token format#

A Vivox access token is similar to a JSON Web Token (JWT), but with a limited set of claims (for example, vxi, f, iss, vxa, t, exp).

The Vivox access token is a string that uses the format header.payload.signature: three base64url-encoded parts that are separated by periods.

Important: Base64url encoding is not the same as Base64 encoding. For more information, see Access token terminology.

**Examples:**

Example 1 (unknown):
```unknown
vxi, f, iss, vxa, t, exp
```

Example 2 (unknown):
```unknown
header.payload.signature
```

---

## AVAudioSessionModeVoiceChat

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/ios/avaudiosessionmodevoicechat

**Contents:**
- AVAudioSessionModeVoiceChat#

To enable platform acoustic echo cancellation (AEC), the iOS device must be in AVAudioSessionModeVoiceChat. For more information, refer to the Apple developer documentation.

AVAudioSessionModeVoiceChat changes EQ to optimize for voice. Because of this change, any non-voice audio that the iOS app plays might seem quieter. The Voice-Processing I/O unit also mixes the audio to ensure that the voice is the loudest part of the overall audio mix.

Note: Although it is possible to change the AVAudioSession mode to default after activating the Voice-Processing I/O unit, Apple recommends using AVAudioSessionModeVoiceChat. Using the default mode when the Voice-Processing I/O unit is active might cause unpredictable behavior, and various device types (for example, iPad and iPhone) might behave in different ways.

---

## Parameter specifics

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/troubleshooting/vad-parameter-specifics

**Contents:**
- Parameter specifics#
- vad_hangover#
- vad_sensitivity#
- vad_noise_floor#

The hangover time is the time (in milliseconds) it takes for the VAD to switch back from speech mode to silence after the last speech frame is detected. The default value is 2000.

The sensitivity is a dimensionless value between 0 and 100 that indicates the sensitivity of the VAD. Increasing this value corresponds to decreasing the sensitivity of the VAD (0 is the most sensitive, and 100 is the least sensitive). Higher values of sensitivity require louder audio to trigger the VAD. The default value is 43.

Applications that use the default VAD and expose the vad_sensitivity as a slider should limit the possible settings between zero (transmit all mic activity) and 70 (very selective).

The noise floor is a dimensionless value between 0 and 20000 that controls how the VAD separates speech from background noise. Lower values assume the user is in a quieter environment where the audio is only speech. Higher values assume a noisy background environment. The default value is 576.

Note: Changes to the VAD noise floor settings do not affect currently joined channels. If the ability to change VAD settings is available to the end-user, indicate that noise floor changes only take effect in the next voice session or only allow changing the noise floor channel when the client is not in-channel.

---

## Evidence report HTTP response example

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/chat-history/evidence-report-response-examples

**Contents:**
- Evidence report HTTP response example#

The top level of the response contains a results object with individual message objects and a next field containing the Unix timestamp (integer) of the next message if there are more messages in the channel. The next field is returned as null if there are no more messages to return. Each message object contains a message_id, sender_id, and an array of message_versions.

The message_versions field contains a timestamp, the message body, and an optional unsafe_message field if filtering is applied.

The messages displays in ascending order with the oldest message in the results first. Message versions display in descending order with the most recent message version first and the oldest message version last.

The following is an example text evidence management response:

**Examples:**

Example 1 (unknown):
```unknown
{
   "results": [
       {
           "sender_id": "sip:your-issuer.username1.1234.@yourdomainname.vivox.com",
           "message_versions": [
               {
                   "unsafe_message": "Message 1: Edit 2: testing the fucking filter",
                   "timestamp": 1697117082123458,
                   "body": "Message 1: Edit 2: testing the **** filter"
               },
               {
                   "timestamp": 1697117040123457,
                   "body": "Message 1: Edit 1"
               },
               {
                   "timestamp": 1697117012123456,
                   "body": "Message 1"
               }
           ],
           "message_id": "434461955301228290"
       },
       {
           "sender_id": "sip:your-issuer.username2.1234.@yourdomainname.vivox.com",
           "message_versions": [
               {
                   "timestamp": 1697117091123459,
                   "body": "Message 2"
               }
           ],
           "message_id": "434461975538075906"
       },
       {
           "sender_id": "sip:your-issuer.username1.1234.@yourdomainname.vivox.com",
           "message_versions": [
               {
                   "timestamp": 1697117217124567,
                   "body": "Message 3"
               }
           ],
           "message_id": "434462007728276226"
       },
       {
           "sender_id": "sip:your-issuer.username2.1234.@yourdomainname.vivox.com",
           "message_versions": [
               {
                   "timestamp": 1697117224124568,
                   "body": "Message 4"
               }
           ],
           "message_id": "434462009596678402"
       }
   ],
   "next": null
}
```

Example 2 (unknown):
```unknown
{
   "results": [
       {
           "sender_id": "sip:your-issuer.username1.1234.@yourdomainname.vivox.com",
           "message_versions": [
               {
                   "unsafe_message": "Message 1: Edit 2: testing the fucking filter",
                   "timestamp": 1697117082123458,
                   "body": "Message 1: Edit 2: testing the **** filter"
               },
               {
                   "timestamp": 1697117040123457,
                   "body": "Message 1: Edit 1"
               },
               {
                   "timestamp": 1697117012123456,
                   "body": "Message 1"
               }
           ],
           "message_id": "434461955301228290"
       },
       {
           "sender_id": "sip:your-issuer.username2.1234.@yourdomainname.vivox.com",
           "message_versions": [
               {
                   "timestamp": 1697117091123459,
                   "body": "Message 2"
               }
           ],
           "message_id": "434461975538075906"
       },
       {
           "sender_id": "sip:your-issuer.username1.1234.@yourdomainname.vivox.com",
           "message_versions": [
               {
                   "timestamp": 1697117217124567,
                   "body": "Message 3"
               }
           ],
           "message_id": "434462007728276226"
       },
       {
           "sender_id": "sip:your-issuer.username2.1234.@yourdomainname.vivox.com",
           "message_versions": [
               {
                   "timestamp": 1697117224124568,
                   "body": "Message 4"
               }
           ],
           "message_id": "434462009596678402"
       }
   ],
   "next": null
}
```

---

## Troubleshooting Bluetooth SCO underrun issues

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/android/troubleshooting-bluetooth-sco-underun-issues

**Contents:**
- Troubleshooting Bluetooth SCO underrun issues#

If your users are experiencing Bluetooth SCO underrun issues, which can cause sound to be choppy on SCO, it is possible to force the Bluetooth device to use A2DP by going into the Android Bluetooth settings and disabling the “Phone Calls” option under the Bluetooth device options.

---

## Make a channel TTL-only request

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/server-side-recording/ssr-make-channel-ttl-only-request

**Contents:**
- Make a channel TTL-only request#

The following code displays an example of how to upload five minutes of data from the subscription time to five minutes into the future for all speakers in the target channel.

**Examples:**

Example 1 (unknown):
```unknown
curl -x POST
http://{{host}}/ssr/subscribe?auth_token={{auth_token}}&history=0&destination_credentials={"bucket":"bucket-name","access_key_id":"aws-access-id","secret_access_key":"aws-secret-key"}&target_uri=sip:confctl-g-.issuer.channel_name@domain.vivox.com&target_type=channel&ttl=5
```

Example 2 (unknown):
```unknown
curl -x POST
http://{{host}}/ssr/subscribe?auth_token={{auth_token}}&history=0&destination_credentials={"bucket":"bucket-name","access_key_id":"aws-access-id","secret_access_key":"aws-secret-key"}&target_uri=sip:confctl-g-.issuer.channel_name@domain.vivox.com&target_type=channel&ttl=5
```

---

## Hardware echo cancellation

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/android/hardware-echo-cancellation

**Contents:**
- Hardware echo cancellation#

Most modern Android devices have a hardware-provided acoustic echo cancellation (AEC) capability. If the target device provides hardware echo cancellation (HEC), then it can filter out all audio that is produced by the device from the capture device stream.

Note: We recommend that you test the HEC configuration on all target Android devices.

The resulting quality and behavior from using HEC can differ across various Android devices. On some devices, stereo playback is downgraded to mono, including Vivox positional channels. Using HEC could also reduce CPU and battery consumption.

---

## Initialize text-to-speech

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/text-to-speech/initialize-tts

**Contents:**
- Initialize text-to-speech#

To start using text-to-speech (TTS) functionality, you must first create a TTS manager. The TTS manager is an SDK-internal object that controls the synthesis, injection, and queueing of TTS messages.

Use the following code to create a TTS manager and return its unique identifier, which is required for accessing TTS functionality.

Note: All TTS-related functions return a status code. You can convert the status code to its corresponding error message string by using vx_tts_get_status_string.

**Examples:**

Example 1 (unknown):
```unknown
vx_tts_manager_id managerId;
vx_tts_status status = vx_tts_initialize(tts_engine_vivox_default, &managerId);
```

Example 2 (unknown):
```unknown
vx_tts_manager_id managerId;
vx_tts_status status = vx_tts_initialize(tts_engine_vivox_default, &managerId);
```

---

## Read and alter audio data for each participant

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/direct-voice-access/read-and-alter-audio

**Contents:**
- Read and alter audio data for each participant#

The pf_on_audio_unit_before_recv_audio_mixed callback provides a parameter with type vx_before_recv_audio_mixed_participant_data_t * called participants_data. An example of how to use this pointer to access an array of separate participants’ audio data is shown in the code sample below.

Note: If you are upgrading your Vivox integration from v5.10, 5.11, or 5.12 be aware that the unique identifier for participants has changed in v5.13 and later versions. If you were using participant_display_name previously you need to change it to participant_uri.

**Examples:**

Example 1 (unknown):
```unknown
pf_on_audio_unit_before_recv_audio_mixed
```

Example 2 (unknown):
```unknown
vx_before_recv_audio_mixed_participant_data_t *
```

Example 3 (unknown):
```unknown
participants_data
```

Example 4 (unknown):
```unknown
participant_display_name
```

---

## Channel-specific options

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/channel-specific-options

**Contents:**
- Channel-specific options#

You can control channel features on a channel-by-channel basis. You can do this by adding a flavor name within the channel name. Different flavor names apply different settings.

For example, a developer sets the options for their Vivox account to use chat filtering on all channels by default. If they want a channel where chat filtering is not applied, they can insert the Unfiltered flavor: f-unfiltered.

If the original channel name was confctl-g-.MYISSUER.spicyroom. changing it to confctl-g-.MYISSUER.spicyroom(f-unfiltered). results in a room with no text filtering.

At this time, Unfiltered is the only available flavor.

**Examples:**

Example 1 (unknown):
```unknown
f-unfiltered.
```

Example 2 (unknown):
```unknown
confctl-g-.MYISSUER.spicyroom.
```

Example 3 (unknown):
```unknown
confctl-g-.MYISSUER.spicyroom(f-unfiltered).
```

---

## Access token signature

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/access-token-guide/access-token-format/access-token-signature

**Contents:**
- Access token signature#

The signature is the base64url-encoded HMAC of the first two parts:

base64UrlEncode(HMACSHA256(base64UrlEncode(header) + '.' + base64UrlEncode(payload), key))

The encoded header and encoded payload are joined with a period, undergoes HMAC with the token signing key, and then the entirety is encoded.

**Examples:**

Example 1 (unknown):
```unknown
base64UrlEncode(HMACSHA256(base64UrlEncode(header) + '.' + base64UrlEncode(payload), key))
```

---

## Required entitlements and Info.plist settings for macOS apps

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/macos/macos-requirements

**Contents:**
- Required entitlements and Info.plist settings for macOS apps#
- NSMicrophoneUsageDescription property#
- Entitlements#

The following Info.plist property and entitlements need to be used for the Vivox SDK to work on macOS devices.

The NSMicrophoneUsageDescription property must be included in your Info.plist in your Xcode project settings. Use it to describe what purpose your app will use the device’s microphone for; this will be displayed when requesting microphone permission.

When shipping an application through the Mac App Store, you must include the com.apple.security.device.microphone entitlement. Add this entitlement for your app to use the device’s microphone if you are shipping a sandboxed app in the Mac App Store.

When shipping an app that’s notarized and shipped outside the Mac App Store, use the com.apple.security.device.audio-input entitlement. Add this entitlement for your app to use the device's microphone if you are shipping with Hardened Runtime enabled to ship outside Apple's Mac App Store.

You can view code samples for these settings and entitlements in the SDKSampleApp under /Source/MediaCaptureAuth.cpp in the macOS distribution files.

For more information, see How to: Request/check iOS & macOS microphone permission in Unity or Apple’s official documentation for requesting authorization of media capture on macOS.

**Examples:**

Example 1 (unknown):
```unknown
NSMicrophoneUsageDescription
```

Example 2 (unknown):
```unknown
com.apple.security.device.microphone
```

Example 3 (unknown):
```unknown
com.apple.security.device.audio-input
```

---

## Vivox SDK error codes

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/troubleshooting/error-codes

**Contents:**
- Vivox SDK error codes#

The following table lists the error codes that the Vivox SDK can return and recommendations for how to handle these errors.

VxErrorNoMessageAvailable

VxErrorTargetObjectDoesNotExist

Internal Vivox error.

Get Vivox logs and send to Vivox for analysis.

VxErrorInvalidArgument

A parameter in a request is either using the wrong type (for example, using a bool when it should be an int) or is missing.

VxErrorNotInitialized

VxErrorNotImplemented

Often a programming error, but sometimes occurs when a login or voice session terminates due to loss of network at the same time that a request is issued.

VxErrorFileOpenFailed

Programming error or packaging error.

User retry, or exponential backoff retry.

VxErrorAlreadyInitialized

VxErrorServerRtpTimeout

VxErrorAsyncOperationCanceled

VxErrorCaptureDeviceInUse

Indicates an attempt to open a second audio session on a second simultaneous session group.

Usually indicative of a client programming error.

VxErrorConnectionTerminated

Connection for Vivox lost, user retry, or exponential backoff.

VxErrorFileOpenFailed

Programming error or packaging error.

VxErrorHandleReserved

Internal Vivox error.

Get Vivox logs and send to Vivox for analysis.

VxErrorInvalidArgument

VxErrorInvalidOperation

Often a programming error, but sometimes occurs when a login or voice session terminates due to loss of network at the same time that a request is issued.

VxErrorInvalidValueTypeXmlQuery

Internal Vivox error.

Get Vivox logs and send to Vivox for analysis.

VxErrorNoMatchingXmlAttributeFound

Internal Vivox error.

Get Vivox logs and send to Vivox for analysis.

VxErrorNoMatchingXmlNodeFound

Internal Vivox error.

Get Vivox logs and send to Vivox for analysis.

Usually a corrupted heap.

VxErrorPortNotAvailable

Unable to find a port for audio.

Usually indicative of having too many calls active at once as a result of a programming error.

User retry or exponential backoff retry.

VxErrorUnableToOpenCaptureDevice

VxErrorXmppBackEndRequired

Client is configured to use the wrong Vivox backend, or the backend is set up incorrectly.

VxErrorPreloginDownloadFailed

Unable to reach a Vivox web server.

VxErrorPresenceMustBeEnabled

VxErrorConnectorLimitExceeded

VxErrorTargetObjectNotRelated

VxErrorTargetObjectDoesNotExist

VxErrorMaxLoginsPerUserExceeded

VxErrorRequestCanceled

VxErrorBuddyDoesNotExist

VxErrorChannelUriRequired

VxErrorTargetObjectAlreadyExists

Occurs when a developer tries to force a player to join a channel that the player is already connected to, while declaring a different session group.

This is because a player cannot connect to two channels with the same URI (same name, same audio/text status, same issuer, and same domain), even if they use a separate session group for each.

This error occurs regardless of whether a developer uses a different session group to try and join the player to the same channel again.

VxErrorInvalidCaptureDeviceForRequestedOperation

VxErrorInvalidCaptureDeviceSpecifier

VxErrorInvalidRenderDeviceSpecifier

VxErrorDeviceLimitReached

VxErrorInvalidEventType

VxErrorNotInitialized

VxErrorAlreadyInitialized

Attempted to initialize a client that was already initialized.

You might need to reopen your Unity project to fix this error.

VxErrorNotImplemented

VxNoAuthentificationStanzaReceived

VxFailedToConnectToXmppServer

VxSSLNegotiationToXmppServerFailed

If this error only occurs on one device, check that the device is up to date on all certificates.

If this error occurs on all devices, contact Vivox immediately.

VxErrorUserOffLineOrDoesNotExist

VxErrorCaptureDeviceInvalidated

VxErrorMaxEtherChannelLimitReached

Server value could not be resolved.

Check that the value is correct. Otherwise, retry with backoff.

VxErrorChannelUriTooLong

VxErrorUserUriTooLong

Occurs when a direct message is sent to a user who is cross-muted (blocked).

VxErrorMessageTextTooLong

Received when a text message exceeds the maximum length in bytes.

VxNetworkHttpInvalidUrl

VxNetworkNameResolutionFailed

Either a programming error or a networking issue.

Check the account management server URL. If pervasive, contact Vivox. Otherwise, retry with backoff.

VxNetworkUnableToConnectToServer

VxNetworkHttpInvalidServerResponse

VxNetworkHttpConnectionReset

VxNetworkHttpInvalidCertificate

VxNetworkHttpGeneralConnectionFailure

VxNetworkReconnectFailure

Received when the Vivox SDK fails to reconnect after several attempts.

VxAccessTokenAlreadyUsed

VxAccessTokenInvalidSignature

VxAccessTokenClaimsMismatch

VxAccessTokenMalformed

VxAccessTokenInternalError

VxAccessTokenServiceUnavailable

VxAccessTokenIssuerMismatch

Received when the Vivox SDK free-tier has been exceeded and payment information has not been provided within 30 days.

VxXmppErrorChannelAtCapacity

Received when trying to join a full channel.

---

## Access token error codes

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/access-token-guide/access-token-error-codes

**Contents:**
- Access token error codes#

The following table details the access token-specific error codes that the Vivox SDK can return from responses to calls that require access tokens.

Access token already used

Access token has invalid signature

Access token payload claims do not match request parameters

Access token malformed

Unknown error using access tokens

Access token mode not enabled

Access token service unavailable

Access token issuer does not match claims

---

## Batch requests

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/batch-requests

**Contents:**
- Batch requests#
- Call and return results#

You can batch your requests when you call UGS services so they are executed in parallel.

You can use the Task.WhenAll method to call UGS services in parallel. This method returns a Task that completes when all the tasks in the argument list complete.

Note: All the failed requests are logged. You can find them in the Cloud Code logs in the Unity Dashboard. To learn more about Logging, refer to Logging.

Note: If some requests fail, the successful requests are not rolled back.

You can use the sample below to call services and return the results of the requests.

The sample below makes calls to the Leaderboards and Economy services. These calls fail if you don't have a leaderboard or currency configured in your project. This allows you to test the error handling in your Cloud Code modules.

The method returns a dictionary with the results of the requests with the signature of the method that made the request as the key, and the result of the request as the value. If a request fails, it notes which request failed. These failures are also logged.

**Examples:**

Example 1 (unknown):
```unknown
Task.WhenAll
```

Example 2 (javascript):
```javascript
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;
using Unity.Services.CloudCode.Apis;
using Unity.Services.CloudCode.Core;
using Unity.Services.CloudSave.Model;
using Unity.Services.Economy.Model;

namespace BatchingRequests;

public class BatchingRequests
{
    private const string ExperienceKey = "experience";
    private const string ProgressXpKey = "progressXP";
    private const string FailedMessage = "Failed. Check logs.";

    private readonly ILogger<BatchingRequests> _logger;
    public BatchingRequests(ILogger<BatchingRequests> logger)
    {
        _logger = logger;
    }

    [CloudCodeFunction("HandleMultipleRequests")]
    public async Task<Dictionary<string, object>> HandleMultipleRequests(IGameApiClient gameApiClient, IExecutionContext ctx)
    {
        async Task<object>  GetCloudSaveData()
        {
           var res = await gameApiClient.CloudSaveData.GetItemsAsync(ctx, ctx.AccessToken, ctx.ProjectId, ctx.PlayerId, new List<string> { ExperienceKey, ProgressXpKey });
           return res.Data;
        }

        async Task<object>  SetCloudSaveData()
        {
            var res = await gameApiClient.CloudSaveData.SetItemAsync(ctx, ctx.AccessToken, ctx.ProjectId, ctx.PlayerId, new SetItemBody(ExperienceKey, 1));
            return res.Data;
        }
        async Task<object>  GetLeaderboardScore()
        {
            const string leaderboardId = "leaderboard";
            var res = await gameApiClient.Leaderboards.GetLeaderboardPlayerScoreAsync(ctx, ctx.AccessToken, Guid.Parse(ctx.ProjectId), leaderboardId, ctx.PlayerId);
            return res.Data;
        }

        async Task<object>  GetEconomyConfig()
        {
            var res = await gameApiClient.EconomyConfiguration.GetPlayerConfigurationAsync(ctx, ctx.AccessToken, ctx.ProjectId, ctx.PlayerId);
            return res.Data;
        }

        async Task<object> SetEconomyConfig()
        {
            const string currencyId = "NOT_REAL";
            var res = await gameApiClient.EconomyCurrencies.IncrementPlayerCurrencyBalanceAsync(ctx, ctx.AccessToken, ctx.ProjectId, ctx.PlayerId, currencyId, new CurrencyModifyBalanceRequest(currencyId, 1));
            return res.Data;
        }

        // Create a list of UGS tasks that return data
        var tasksWithObjects = new List<Func<Task<object>>>() { GetCloudSaveData, GetLeaderboardScore, SetCloudSaveData, GetEconomyConfig, SetEconomyConfig };
        var results = await AwaitBatch(tasksFns);

        foreach (var item in results)
        {
            if (item is Exception)
            {
                _logger.LogError($"AwaitBatch failed status {item}");
            }
            else if (item is CurrencyBalanceResponse)
            {
                _logger.LogInformation($"AwaitBatch CurrencyBalanceResponse status {item.GetType()} = {item}");
            }
            else
            {
                _logger.LogInformation($"AwaitBatch success status {item.GetType()} = {item}");
            }
        }
        // Alternatively to looping through all results, you may access function results directly
        // The order of function results follows the order of the function input e.g.
        // - GetCloudSaveData corresponds to results[0]: GetItemsResponse || Exception
        // - GetLeaderboardScore corresponds to results[1]: LeaderboardEntryWithUpdatedTime || Exception
        // ... etc
        // This can also help ascertain where the Exception originated from
    }

    private async Task<object[]> AwaitBatch(List<Func<Task<object>>> tasks)
    {
        var tasksToRun = tasks.Select(Run);
        var results = await Task.WhenAll(tasksToRun);
        return results;
    }

    async Task<object> Run(Func<Task<object>> fn)
    {
        try
        {
            return await fn();
        }
        catch (Exception e)
        {
            return e;
        }
    }
}

public class ModuleConfig : ICloudCodeSetup
{
    public void Setup(ICloudCodeConfig config)
    {
        config.Dependencies.AddSingleton(GameApiClient.Create());
    }
}
```

Example 3 (javascript):
```javascript
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;
using Unity.Services.CloudCode.Apis;
using Unity.Services.CloudCode.Core;
using Unity.Services.CloudSave.Model;
using Unity.Services.Economy.Model;

namespace BatchingRequests;

public class BatchingRequests
{
    private const string ExperienceKey = "experience";
    private const string ProgressXpKey = "progressXP";
    private const string FailedMessage = "Failed. Check logs.";

    private readonly ILogger<BatchingRequests> _logger;
    public BatchingRequests(ILogger<BatchingRequests> logger)
    {
        _logger = logger;
    }

    [CloudCodeFunction("HandleMultipleRequests")]
    public async Task<Dictionary<string, object>> HandleMultipleRequests(IGameApiClient gameApiClient, IExecutionContext ctx)
    {
        async Task<object>  GetCloudSaveData()
        {
           var res = await gameApiClient.CloudSaveData.GetItemsAsync(ctx, ctx.AccessToken, ctx.ProjectId, ctx.PlayerId, new List<string> { ExperienceKey, ProgressXpKey });
           return res.Data;
        }

        async Task<object>  SetCloudSaveData()
        {
            var res = await gameApiClient.CloudSaveData.SetItemAsync(ctx, ctx.AccessToken, ctx.ProjectId, ctx.PlayerId, new SetItemBody(ExperienceKey, 1));
            return res.Data;
        }
        async Task<object>  GetLeaderboardScore()
        {
            const string leaderboardId = "leaderboard";
            var res = await gameApiClient.Leaderboards.GetLeaderboardPlayerScoreAsync(ctx, ctx.AccessToken, Guid.Parse(ctx.ProjectId), leaderboardId, ctx.PlayerId);
            return res.Data;
        }

        async Task<object>  GetEconomyConfig()
        {
            var res = await gameApiClient.EconomyConfiguration.GetPlayerConfigurationAsync(ctx, ctx.AccessToken, ctx.ProjectId, ctx.PlayerId);
            return res.Data;
        }

        async Task<object> SetEconomyConfig()
        {
            const string currencyId = "NOT_REAL";
            var res = await gameApiClient.EconomyCurrencies.IncrementPlayerCurrencyBalanceAsync(ctx, ctx.AccessToken, ctx.ProjectId, ctx.PlayerId, currencyId, new CurrencyModifyBalanceRequest(currencyId, 1));
            return res.Data;
        }

        // Create a list of UGS tasks that return data
        var tasksWithObjects = new List<Func<Task<object>>>() { GetCloudSaveData, GetLeaderboardScore, SetCloudSaveData, GetEconomyConfig, SetEconomyConfig };
        var results = await AwaitBatch(tasksFns);

        foreach (var item in results)
        {
            if (item is Exception)
            {
                _logger.LogError($"AwaitBatch failed status {item}");
            }
            else if (item is CurrencyBalanceResponse)
            {
                _logger.LogInformation($"AwaitBatch CurrencyBalanceResponse status {item.GetType()} = {item}");
            }
            else
            {
                _logger.LogInformation($"AwaitBatch success status {item.GetType()} = {item}");
            }
        }
        // Alternatively to looping through all results, you may access function results directly
        // The order of function results follows the order of the function input e.g.
        // - GetCloudSaveData corresponds to results[0]: GetItemsResponse || Exception
        // - GetLeaderboardScore corresponds to results[1]: LeaderboardEntryWithUpdatedTime || Exception
        // ... etc
        // This can also help ascertain where the Exception originated from
    }

    private async Task<object[]> AwaitBatch(List<Func<Task<object>>> tasks)
    {
        var tasksToRun = tasks.Select(Run);
        var results = await Task.WhenAll(tasksToRun);
        return results;
    }

    async Task<object> Run(Func<Task<object>> fn)
    {
        try
        {
            return await fn();
        }
        catch (Exception e)
        {
            return e;
        }
    }
}

public class ModuleConfig : ICloudCodeSetup
{
    public void Setup(ICloudCodeConfig config)
    {
        config.Dependencies.AddSingleton(GameApiClient.Create());
    }
}
```

---

## Evidence report HTTP response example

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/chat-history/evidence-report-response-examples

**Contents:**
- Evidence report HTTP response example#

The top level of the response contains a results object with individual message objects and a next field containing the Unix timestamp (integer) of the next message if there are more messages in the channel. The next field is returned as null if there are no more messages to return. Each message object contains a message_id, sender_id, and an array of message_versions.

The message_versions field contains a timestamp, the message body, and an optional unsafe_message field if filtering is applied.

The messages displays in ascending order with the oldest message in the results first. Message versions display in descending order with the most recent message version first and the oldest message version last.

The following is an example text evidence management response:

**Examples:**

Example 1 (unknown):
```unknown
message_versions
```

Example 2 (unknown):
```unknown
message_versions
```

Example 3 (unknown):
```unknown
unsafe_message
```

Example 4 (unknown):
```unknown
{
   "results": [
       {
           "sender_id": "sip:your-issuer.username1.1234.@yourdomainname.vivox.com",
           "message_versions": [
               {
                   "unsafe_message": "Message 1: Edit 2: testing the fucking filter",
                   "timestamp": 1697117082123458,
                   "body": "Message 1: Edit 2: testing the **** filter"
               },
               {
                   "timestamp": 1697117040123457,
                   "body": "Message 1: Edit 1"
               },
               {
                   "timestamp": 1697117012123456,
                   "body": "Message 1"
               }
           ],
           "message_id": "434461955301228290"
       },
       {
           "sender_id": "sip:your-issuer.username2.1234.@yourdomainname.vivox.com",
           "message_versions": [
               {
                   "timestamp": 1697117091123459,
                   "body": "Message 2"
               }
           ],
           "message_id": "434461975538075906"
       },
       {
           "sender_id": "sip:your-issuer.username1.1234.@yourdomainname.vivox.com",
           "message_versions": [
               {
                   "timestamp": 1697117217124567,
                   "body": "Message 3"
               }
           ],
           "message_id": "434462007728276226"
       },
       {
           "sender_id": "sip:your-issuer.username2.1234.@yourdomainname.vivox.com",
           "message_versions": [
               {
                   "timestamp": 1697117224124568,
                   "body": "Message 4"
               }
           ],
           "message_id": "434462009596678402"
       }
   ],
   "next": null
}
```

---

## Security guide

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/security

**Contents:**
- Security guide#
- Permissions#
  - Permissions security#
- Users and groups#
- Read-access restriction#
  - Read-access functionality#
  - Incomplete changesets#
- Edit permissions#

The Unity Version Control (UVCS) security system can help you secure your version control processes. Unity Version Control focuses on the following:

In Unity Version Control, every object has an associated Access Control List (ACL) which makes it simple to customize access and security.

Note: This page focuses on permissions in the UVCS desktop application. To manage user security permissions through the Unity Dashboard, refer to Manage users.

For instructions on how to configure your security permissions for specific scenarios, refer to Security scenarios

If your version control environment requires security restrictions, to prevent unwanted access or enforce certain development policies, there are two main ways you can customize your permissions:

If users try to use an operation that they don’t have permissions for, UVCS displays an error message to notify them. The error message specifies the permission that they don’t have that means they can’t perform that action.

A secured path can prevent users from performing operations, such as read or write, on the path. To circumvent the security permissions, a user might edit the path name or the name of the parent directory. UVCS protects against this as it detects that the existing user permissions are different between the source and destination paths and doesn’t allow the user to rename. Instead, the user receives an error message.

Each project has the following three user groups:

Note: There is also the organization-admin group, which consists of the owners or managers of the Unity organization, who also are assigned a UVCS seat.

Any Unity groups also appear as UVCS groups.

You can restrict read access to certain paths so that some users can’t view parts of your repository. For example, you might restrict read access in centralized setups where sensitive areas of the repository need to be protected in such a way that some project members can't even view them.

For the desktop application, you need to configure the path-based security to protect read access before using the repository for the first time. Otherwise, the read permissions don't apply until users run update operations (without local changes) to propagate the new read permission rules to their workspaces.

Note: GitSync and GitServer ignore any path-based security.

If you restrict read access to certain paths, it can cause some incomplete changesets. The incomplete changesets are important if you work in distributed scenarios.

In distributed version control scenarios you can reject operations when the path permissions of a revision can’t be checked because a changeset wasn’t replicated. To reject these operations, enable the following setting inside the server.conf file: <SecuredPaths>true</SecuredPaths>.

UVCS permissions are defined by the user's Unity role. UVCS permissions can only deny permissions that are granted by the Unity role.

To configure the permissions on your UVCS server, you need an existing repository. This means you can right-click the repository to open one of the following configuration windows:

Note: To find the default permissions for each group, refer to users and groups.

For specific security configurations, refer to Security scenarios.

**Examples:**

Example 1 (unknown):
```unknown
organization-admin
```

Example 2 (unknown):
```unknown
<SecuredPaths>true</SecuredPaths>
```

---

## Acoustic echo cancellation

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/ios/acoustic-echo-cancellation

**Contents:**
- Acoustic echo cancellation#

When an iOS device plays and records audio at the same time, there is the possibility that a capture device could pick up the audio that is playing. The Vivox SDK includes acoustic echo cancellation (AEC), which helps to prevent this type of audio capture from occurring.

By default, the Vivox SDK leverages the platform AEC provided by Apple, which is enabled or disabled by using dynamic voice processing switching (DVPS).

DVPS is enabled by default. To disable DVPS, initialize the Vivox SDK with the dynamic_voice_processing_switching configuration parameter set to 0.

Note: Disabling DVPS is not recommended.

DVPS uses the following logic for enabling or disabling platform AEC:

DVPS causes Vivox to play and record audio by using a Voice-Processing I/O audio unit. It also makes changes to the shared AVAudioSession instance to ensure that the right category and mode are selected. When platform AEC is enabled, the AVAudioSessionCategory is set to AVAudioSessionCategoryPlayAndRecord, and the AVAudioSessionMode is set to AVAudioSessionModeVoiceChat. After all Vivox voice sessions are removed, DVPS automatically switches back to the original AVAudioSession configuration. For optimal results, before you initialize the Vivox SDK, we recommend that you set the AVAudioSessionCategory to AVAudioSessionCategoryPlayback.

DVPS enforces the use of the Bluetooth Headset Profile (HSP) when accessing the microphone on a connected Bluetooth device. This ensures that the headset microphone is used instead of the device microphone to provide a better voice chat experience.

---

## Vivox SDK error codes

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/troubleshooting/error-codes

**Contents:**
- Vivox SDK error codes#

The following table lists the error codes that the Vivox SDK can return and recommendations for how to handle these errors.

VxErrorNoMessageAvailable

VxErrorTargetObjectDoesNotExist

Internal Vivox error.

Get Vivox logs and send to Vivox for analysis.

VxErrorInvalidArgument

A parameter in a request is either using the wrong type (for example, using a bool when it should be an int) or is missing.

VxErrorNotInitialized

VxErrorNotImplemented

Often a programming error, but sometimes occurs when a login or voice session terminates due to loss of network at the same time that a request is issued.

VxErrorFileOpenFailed

Programming error or packaging error.

User retry, or exponential backoff retry.

VxErrorAlreadyInitialized

VxErrorServerRtpTimeout

VxErrorAsyncOperationCanceled

VxErrorCaptureDeviceInUse

Indicates an attempt to open a second audio session on a second simultaneous session group.

Usually indicative of a client programming error.

VxErrorConnectionTerminated

Connection for Vivox lost, user retry, or exponential backoff.

VxErrorFileOpenFailed

Programming error or packaging error.

VxErrorHandleReserved

Internal Vivox error.

Get Vivox logs and send to Vivox for analysis.

VxErrorInvalidArgument

VxErrorInvalidOperation

Often a programming error, but sometimes occurs when a login or voice session terminates due to loss of network at the same time that a request is issued.

VxErrorInvalidValueTypeXmlQuery

Internal Vivox error.

Get Vivox logs and send to Vivox for analysis.

VxErrorNoMatchingXmlAttributeFound

Internal Vivox error.

Get Vivox logs and send to Vivox for analysis.

VxErrorNoMatchingXmlNodeFound

Internal Vivox error.

Get Vivox logs and send to Vivox for analysis.

Usually a corrupted heap.

VxErrorPortNotAvailable

Unable to find a port for audio.

Usually indicative of having too many calls active at once as a result of a programming error.

User retry or exponential backoff retry.

VxErrorUnableToOpenCaptureDevice

VxErrorXmppBackEndRequired

Client is configured to use the wrong Vivox backend, or the backend is set up incorrectly.

VxErrorPreloginDownloadFailed

Unable to reach a Vivox web server.

VxErrorPresenceMustBeEnabled

VxErrorConnectorLimitExceeded

VxErrorTargetObjectNotRelated

VxErrorTargetObjectDoesNotExist

VxErrorMaxLoginsPerUserExceeded

VxErrorRequestCanceled

VxErrorBuddyDoesNotExist

VxErrorChannelUriRequired

VxErrorTargetObjectAlreadyExists

Occurs when a developer tries to force a player to join a channel that the player is already connected to, while declaring a different session group.

This is because a player cannot connect to two channels with the same URI (same name, same audio/text status, same issuer, and same domain), even if they use a separate session group for each.

This error occurs regardless of whether a developer uses a different session group to try and join the player to the same channel again.

VxErrorInvalidCaptureDeviceForRequestedOperation

VxErrorInvalidCaptureDeviceSpecifier

VxErrorInvalidRenderDeviceSpecifier

VxErrorDeviceLimitReached

VxErrorInvalidEventType

VxErrorNotInitialized

VxErrorAlreadyInitialized

Attempted to initialize a client that was already initialized.

You might need to reopen your Unity project to fix this error.

VxErrorNotImplemented

VxNoAuthentificationStanzaReceived

VxFailedToConnectToXmppServer

VxSSLNegotiationToXmppServerFailed

If this error only occurs on one device, check that the device is up to date on all certificates.

If this error occurs on all devices, contact Vivox immediately.

VxErrorUserOffLineOrDoesNotExist

VxErrorCaptureDeviceInvalidated

VxErrorMaxEtherChannelLimitReached

Server value could not be resolved.

Check that the value is correct. Otherwise, retry with backoff.

VxErrorChannelUriTooLong

VxErrorUserUriTooLong

Occurs when a direct message is sent to a user who is cross-muted (blocked).

VxErrorMessageTextTooLong

Received when a text message exceeds the maximum length in bytes.

VxNetworkHttpInvalidUrl

VxNetworkNameResolutionFailed

Either a programming error or a networking issue.

Check the account management server URL. If pervasive, contact Vivox. Otherwise, retry with backoff.

VxNetworkUnableToConnectToServer

VxNetworkHttpInvalidServerResponse

VxNetworkHttpConnectionReset

VxNetworkHttpInvalidCertificate

VxNetworkHttpGeneralConnectionFailure

VxNetworkReconnectFailure

Received when the Vivox SDK fails to reconnect after several attempts.

VxAccessTokenAlreadyUsed

VxAccessTokenInvalidSignature

VxAccessTokenClaimsMismatch

VxAccessTokenMalformed

VxAccessTokenInternalError

VxAccessTokenServiceUnavailable

VxAccessTokenIssuerMismatch

Received when the Vivox SDK free-tier has been exceeded and payment information has not been provided within 30 days.

VxXmppErrorChannelAtCapacity

Received when trying to join a full channel.

---

## Sign an iOS application

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/sign-build-artifacts/sign-an-ios-application

**Contents:**
- Sign an iOS application#
- Join the Apple Developer Program#
- Provisioning profiles#
  - Components of a provisioning profile#
- Create an iOS certificate and p12 file#
  - Create a certificate#
  - Creating a p12 file#
- Adding devices#
  - Finding your UDID#
  - Add the UDID in the Apple developer portal#

Unity Build Automation signs your iOS application during the build process. This ensures your iOS application will meet requirements to be installed in test devices or deployed to App Store.

To develop iOS apps you must be a member of the Apple Developer Program. This lets you build, test, and eventually release your apps to the Apple App Store.

A provisioning profile ties developers and devices to an authorized Development Team and enables you to use a device for testing. You must install a Development Provisioning Profile on each device you plan to use to run your app.

Each Development Provisioning Profile contains a set of Development Certificates, Unique Device Identifiers (UDID), and an App ID.

To use a device for testing, you must also include your Development Certificate in the profile. A single device can contain multiple provisioning profiles.

Certificates determine whether your app is development-only or a release candidate for the App Store.

Identifiers are the unique IDs that identify your project. For basic projects, or if this is your first iOS project, you should make an App ID. This is often the same as your Unity Project’s Bundle ID.

Tip: For more information on signing identities and certificates, see Maintain signing assets on the Apple developer website.

Devices are the hardware—such as an iPhone, iPad, or iPod—on which you plan to test your project. You must retrieve the UDID for each device on which you plan to test your game, even if you are using a virtual device or simulator.

You will then add the UDID to the Devices section in the Apple developer portal.

When you create a certificate, you must decide whether to create a Development Certificate (used only for testing), or a Distribution Certificate, which you can use for testing or to distribute your app via the App Store.

To create apps using Build Automation, you must convert your certificate file to a p12 file. A p12 file is a file that contains your private key and certificate and is used to sign your code.

For development purposes, Apple requires the UDID of each device on which you intend to install your app. Once your app is accepted into the App Store, anyone can download and install it; provided they have the correct version of iOS and meet any other requirements.

You can use iTunes to retrieve the UDID of your device. For a walkthrough of the retrieval process, see here.

You should now be able to configure an iOS target in Build Automation using the following items:

---

## Configure a build

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/basic-build-configuration/overview

**Contents:**
- Configure a build#
- Prerequisites#
- Choose a build configuration flow#
  - Quick target setup#
  - Target setup#
    - Basic Info#
    - Builder Configuration#
    - Credentials#
    - Scheduling#
    - Android configurations#

A build configuration defines the settings and requirements for creating a specific build of your project. A build configuration is a structured setup that determines how your project is compiled, packaged, and prepared for deployment across different platforms. Once a build configuration is created, you can run multiple build attempts against it. All the settings specified in the configuration, such as the platform, Unity version, and credentials automatically apply to every build attempt to ensure consistency and reproducibility across all builds.

To set up a build configuration:

Before you set up your first build configuration, ensure your project’s source control settings are configured. This step is required for your first configuration.

Select Get started to configure your source control.

You have two options to set up a new build configuration: quick target setup and target setup.

If you select Quick target setup, the dashboard displays the basic configurations required to configure a build target for each platform. If you don’t want to configure advanced settings for your build target, select Quick target setup.

When you select Target setup, you first set up the basic settings required for launching your build target. Then, you can either save your configuration or move to the advanced settings.

The Basic Info section includes fields to define core settings for your build:

Select the machine specifications for your build. The appropriate settings depend on your project’s complexity and platform requirements. For more details about builder configurations refer to Choose a machine specification.

The Credentials section allows you to provide the credentials Build Automation uses to sign your build to ensure the security and integrity of your build artifacts. Provide the required credentials, such as keystores for Android or signing certificates for macOS.

The Scheduling section enables you to automate build triggers and set recurring build schedules.

For more details, refer to Run builds automatically.

Additional build settings are available when you select Android as the target platform.

In the Basic Info section of the target configuration, the first setting is the Android SDK version. Use this setting to select the preferred Android SDK version, which depends on the selected Unity version. For more information, refer to the Android SDK to Unity versions compatibility guide.

The remaining Android specific settings are in Advanced Settings under Platform specific settings (Android).

Additional build settings are available when the selected target platform is Universal Windows Platform (UWP). These settings allow for fine-grained control over how you build the UWP application and how you configure target devices.

For comprehensive information about UWP settings, refer to the UWP Build Settings documentation.

The Basic Info section of the target configuration includes the following UWP-specific options:

The Advanced Settings of the target configuration include additional UWP-specific options:

**Examples:**

Example 1 (unknown):
```unknown
ProjectSettings
```

Example 2 (unknown):
```unknown
ProjectSettings/ProjectVersion.txt
```

Example 3 (unknown):
```unknown
AssetPackConfigSerializer.SaveConfig
```

Example 4 (unknown):
```unknown
PlayerSettings.Android.useAPKExpansionFiles
```

---

## Positional channel properties

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/channels/positional-channel-properties

**Contents:**
- Positional channel properties#

The audio experience of a user in a positional audio channel is determined by the Channel3DProperties component to VivoxService.Instance.JoinPositionalChannelAsync.

The following properties control the 3D experience in the channel. Each of these properties are detailed in the HTML reference for the Channel3DProperties class.

ConversationalDistance

AudioFadeIntensityByDistance

The default Channel3DProperties values were chosen to provide the most natural sounding, acoustically realistic 3D audio that is suitable for most situations. When using the default properties, any listener within 32 meters (about 105 feet) can hear a speaker, and the speaker is heard at full volume by anyone within 1 meter (about 3 feet).

Note: The default distance values use meters because this is the default unit of distance measurement in Unity, but you can specify these values in any scale.

When customizing distance values, consider the following guidelines:

You can represent the AudibleDistance and ConversationalDistance in any units, but the two units must always match. The values can be different.

Note: You must use the same units when reporting your actor’s position by using the Set3DPosition method.

ConversationalDistance is the distance from the listener at which a speaker’s voice is heard at its original volume. This must be an integer in the range 0 <= ConversationalDistance <= AudibleDistance. Consider this to be the expected close-by conversational distance.

The ConversationalDistance sounds the most realistic when it's half the height of a typical speaking entity in your game.

If your players are shorter or taller than typical adult humans, considering adjusting this value to match.

When using the default AudioFadeModel value, if you want voice chat to naturally fade to near zero loudness at the edge of the AudibleDistance so it doesn't sound like it's being abruptly cut off, then set the AudibleDistance to a minimum of:

32 × ConversationalDistance ÷ AudioFadeIntensityByDistance.

If you want a lower max AudibleDistance, for example, to limit the range of received texts, then you can increase AudioFadeIntensityByDistance.

Depending on the amount of ambient game noise or music, you might want to further adjust these values to ensure that voice chat is heard only at the distances you want.

When using the ExponentialByDistance AudioFadeModel, it's recommended that you keep the ConversationalDistance value at 1 or greater.

When using the ExponentialByDistance AudioFadeModel, it's recommended that you adjust the value of the AudioFadeIntensityByDistance property based on the following formula:

AudioFadeIntensityByDistance = 3 ÷ (1 + 1.75 × log10(ConversationalDistance))

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.JoinPositionalChannelAsync
```

Example 2 (unknown):
```unknown
AudibleDistance
```

Example 3 (unknown):
```unknown
ConversationalDistance
```

Example 4 (unknown):
```unknown
AudioFadeModel
```

---

## Interact with cross-player data

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/cross-player-data

**Contents:**
- Interact with cross-player data#
- Retrieve player IDs#
  - Use Leaderboards#
  - Use Lobby#
  - Use Friends#
- Interact with player data#
  - Authenticate as Cloud Code#
  - Update top 5 player data using Cloud Save#
  - Reward all players in the Lobby using Economy#

You can use Cloud Code modules to access and modify data for multiple players at once.

This unlocks a variety of use cases, such as to:

Note: To reduce risk of abuse, ensure that only specific users can call modules that interact with cross-player data instead of all players. To restrict access to particular modules or their endpoints, you can use Access Control.

To interact with multiple players, you need call to UGS services to retrieve the player IDs first.

To get the player IDs, you can use the Cloud Code C# SDKs in the Com.Unity.Services.CloudCode.Apis NuGet package.

You can also use a service's REST API directly. Services that you can use to get player IDs include Leaderboards, Lobby, and Friends.

First, you need to create a Leaderboard.

For example, you can retrieve the top five players from a leaderboard and save their IDs. The module below takes in a leaderboard ID and returns a list of top five players' scores:

Call the GetTopPlayers function to retrieve a list of top 5 players. The response looks similar to below:

First, you need to create a Lobby.

You can retrieve a list of players that are currently in the Lobby, and use their IDs to interact with their data:

Call the GetLobbyPlayers function to retrieve a list of players in the lobby.

The response looks similar to below:

To use Friends, you need to create a relationship. For more information, see the Friends documentation.

You can use the helper method SendFriendRequest to send a friend request to a player. The method uses the playerId parameter to send the request to the correct user.

You can then retrieve a list of friends for a player, and use their IDs to interact with their data. The sample below shows how you can retrieve a list of player IDs that a player has sent friend requests to:

Call the GetRelationships function to retrieve a list of players that the player has sent friend requests to.

The response looks similar to below:

Once you have a list of player IDs, you can use them to update player data through UGS services.

The samples below show how to use the Cloud Save and Economy services to interact with player data.

To interact with cross-player data, you need to authenticate as Cloud Code. To authenticate as Cloud Code, use the ServiceToken instead of the AccessToken in the module function:

Refer to Authentication for more information and check Service and access token support for a list of services that support the ServiceToken.

One way to interact with player data is to pass in a list of player IDs to Cloud Save.

The module function in the sample takes a leaderboard ID as a parameter.

The sample below shows how to record a value for reaching top 5 in Cloud Save for a list of top 5 players retrieved from a leaderboard. To ensure the sample works, follow the steps below:

Create a new module and add the following code:

The UpdateTopPlayerData function sets a key-value pair for all players in the leaderboard. To verify this, you can navigate to the Player Management service in the Unity Dashboard and inspect the Cloud Save data for the top players.

To interact with player balances, you can pass in a list of player IDs to the Economy API.

The sample below shows how to increment the balance of a currency for a list of players retrieved from a lobby. The module function takes in a lobby ID, currency ID, and an amount to increment the balance by as parameters.

To ensure the sample works, follow the steps below:

Create a new module and add the following code:

The RewardLobbyPlayers function increments the balance of the currency for all players in the lobby.

To verify, you can navigate to the Player Management service in the Unity Dashboard and inspect the balances for your players.

**Examples:**

Example 1 (unknown):
```unknown
Com.Unity.Services.CloudCode.Apis
```

Example 2 (unknown):
```unknown
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;
using Unity.Services.CloudCode.Apis;
using Unity.Services.CloudCode.Core;
using Unity.Services.CloudCode.Shared;
using Unity.Services.Leaderboards.Model;

namespace CrossPlayerData;

public class CrossPlayerData
{
    private readonly ILogger<CrossPlayerData> _logger;

    public CrossPlayerData(ILogger<CrossPlayerData> logger)
    {
        _logger = logger;
    }

    [CloudCodeFunction("GetTopPlayers")]
    public async Task<List<LeaderboardEntry>> GetTopPlayers(IExecutionContext ctx, IGameApiClient gameApiClient, string leaderboardId)
    {
        ApiResponse<LeaderboardScoresPage> response;
        try
        {
            response = await gameApiClient.Leaderboards.GetLeaderboardScoresAsync(ctx, ctx.ServiceToken, new Guid(ctx.ProjectId),
                    leaderboardId, null, 5);

            return response.Data.Results;

        }
        catch (ApiException e)
        {
            _logger.LogError("Failed to get top players for leaderboard: {LeaderboardId}. Error: {Error}", leaderboardId, e.Message);
            throw new Exception($"Failed to get top players for leaderboard: {leaderboardId}. Error: {e.Message}");
        }
    }

    public class ModuleConfig : ICloudCodeSetup
    {
        public void Setup(ICloudCodeConfig config)
        {
            config.Dependencies.AddSingleton(GameApiClient.Create());
        }
    }
}
```

Example 3 (unknown):
```unknown
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;
using Unity.Services.CloudCode.Apis;
using Unity.Services.CloudCode.Core;
using Unity.Services.CloudCode.Shared;
using Unity.Services.Leaderboards.Model;

namespace CrossPlayerData;

public class CrossPlayerData
{
    private readonly ILogger<CrossPlayerData> _logger;

    public CrossPlayerData(ILogger<CrossPlayerData> logger)
    {
        _logger = logger;
    }

    [CloudCodeFunction("GetTopPlayers")]
    public async Task<List<LeaderboardEntry>> GetTopPlayers(IExecutionContext ctx, IGameApiClient gameApiClient, string leaderboardId)
    {
        ApiResponse<LeaderboardScoresPage> response;
        try
        {
            response = await gameApiClient.Leaderboards.GetLeaderboardScoresAsync(ctx, ctx.ServiceToken, new Guid(ctx.ProjectId),
                    leaderboardId, null, 5);

            return response.Data.Results;

        }
        catch (ApiException e)
        {
            _logger.LogError("Failed to get top players for leaderboard: {LeaderboardId}. Error: {Error}", leaderboardId, e.Message);
            throw new Exception($"Failed to get top players for leaderboard: {leaderboardId}. Error: {e.Message}");
        }
    }

    public class ModuleConfig : ICloudCodeSetup
    {
        public void Setup(ICloudCodeConfig config)
        {
            config.Dependencies.AddSingleton(GameApiClient.Create());
        }
    }
}
```

Example 4 (unknown):
```unknown
GetTopPlayers
```

---

## Channel-specific options

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/channel-specific-options

**Contents:**
- Channel-specific options#

You can control channel features on a channel-by-channel basis. You can do this by adding a flavor name within the channel name. Different flavor names apply different settings.

For example, a developer sets the options for their Vivox account to use chat filtering on all channels by default. If they want a channel where chat filtering is not applied, they can insert the Unfiltered flavor: f-unfiltered.

If the original channel name was confctl-g-.MYISSUER.spicyroom. changing it to confctl-g-.MYISSUER.spicyroom(f-unfiltered). results in a room with no text filtering.

At this time, Unfiltered is the only available flavor.

---

## Make a channel history-only request

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/server-side-recording/ssr-make-channel-history-only-request

**Contents:**
- Make a channel history-only request#

The following code displays an example of how to upload data for every user who spoke in a channel for the five minutes previous to the subscription time.

**Examples:**

Example 1 (unknown):
```unknown
curl -x POST
http://{{host}}/ssr/subscribe?auth_token={{auth_token}}&history=5&destination_credentials={"bucket":"bucket-name","access_key_id":"aws-access-id","secret_access_key":"aws-secret-key"}&target_uri=sip:confctl-g-.issuer.channel_name@domain.vivox.com&target_type=channel&ttl=0
```

Example 2 (unknown):
```unknown
curl -x POST
http://{{host}}/ssr/subscribe?auth_token={{auth_token}}&history=5&destination_credentials={"bucket":"bucket-name","access_key_id":"aws-access-id","secret_access_key":"aws-secret-key"}&target_uri=sip:confctl-g-.issuer.channel_name@domain.vivox.com&target_type=channel&ttl=0
```

---

## Reduce Opus CPU load on low-powered Android devices

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/android/reduce-opus-cpu-load-on-low-powered-devices

**Contents:**
- Reduce Opus CPU load on low-powered Android devices#

Caution: Modifying Opus settings to reduce CPU load might negatively impact voice quality.

If you need to reduce the Vivox CPU load for lower-power Android devices, the Vivox SDK exposes the vx_opus_set_complexity method to set the algorithmic complexity of the codec to another value. The Vivox SDK uses a default complexity value of 9, but you can set this down to 0 to reduce the CPU usage. The lower the value, the more the audio quality is negatively affected. For example, in a speech signal test where the complexity value was decreased to 0, the encoding time decreased to 39% of the default settings (Complexity: 9, Bandwidth: Auto).

It is also possible to use the vx_opus_set_bandwidth method to change the audio bandwidth that Opus uses. If you pass the vx_opus_bandwidth::opus_bandwidth_nb value, this also reduces the CPU usage, but might have a significant negative impact on audio quality. Lowering the audio bandwidth makes the audio signal sound duller by discarding high frequency content.

Note that reducing complexity has more impact on CPU use reduction than changing the audio bandwidth. For example, a speech signal test with a complexity of 0 combined with vx_opus_bandwidth::opus_bandwidth_nb got the lowest CPU time of 27% of the CPU time of the default settings (Complexity: 9, Bandwidth: Auto).

If either of these methods do not reduce CPU use enough, you can use the Siren14 codec, which takes 14% of the CPU time of Opus at its default settings. To use the Siren14 codec, perform either of the following actions:

Note: These updates affect the audio quality for all session participants. This is because when you change the codec of a client, all participants also switch to that codec.

**Examples:**

Example 1 (unknown):
```unknown
vx_opus_set_complexity
```

Example 2 (unknown):
```unknown
vx_opus_set_bandwidth
```

Example 3 (unknown):
```unknown
vx_opus_bandwidth::opus_bandwidth_nb
```

Example 4 (unknown):
```unknown
vx_opus_bandwidth::opus_bandwidth_nb
```

---

## Make a speaker history-only request

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/server-side-recording/ssr-make-speaker-history-only-request

**Contents:**
- Make a speaker history-only request#

The following code displays an example of how to upload data for a target user who spoke for the five minutes previous to the subscription time.

**Examples:**

Example 1 (unknown):
```unknown
curl -x POST
http://{{host}}/ssr/subscribe?auth_token={{auth_token}}&history=5&destination_credentials={"bucket":"bucket-name","access_key_id":"aws-access-id","secret_access_key":"aws-secret-key"}&target_urisip:.issuer.speaker.@domain.vivox.com&target_type=speaker&ttl=0
```

Example 2 (unknown):
```unknown
curl -x POST
http://{{host}}/ssr/subscribe?auth_token={{auth_token}}&history=5&destination_credentials={"bucket":"bucket-name","access_key_id":"aws-access-id","secret_access_key":"aws-secret-key"}&target_urisip:.issuer.speaker.@domain.vivox.com&target_type=speaker&ttl=0
```

---

## Access token header

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/access-token-guide/access-token-format/access-token-header

**Contents:**
- Access token header#

The header in a Vivox access token is an empty JSON object (for example, {}).

Unlike a true JSON Web Token (JWT), this header is empty and not {"'alg":"HS256","typ":"JWT"}.

**Examples:**

Example 1 (unknown):
```unknown
{"'alg":"HS256","typ":"JWT"}
```

---

## Buddy and presence management

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/presence/presence-toc

**Contents:**
- Buddy and presence management#

---

## Message polling

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/messaging/message-polling

**Contents:**
- Message polling#

After you initialize the Vivox SDK, but before you sign in a user, the game must begin polling for messages. Typically, the game either polls directly in the game render loop or polls based on a timer mechanism. The recommended polling frequency is 10 times per second, where at each poll interval, vx_get_message() is called until there are no more messages to process.

The following code displays an example of how to poll for Vivox messages:

**Examples:**

Example 1 (unknown):
```unknown
vx_get_message()
```

Example 2 (cpp):
```cpp
#include "Vxc.h"
#include "VxcErrors.h"
. . .
// this is your timer handler or your render loop

vx_message_base_t *m;

for(;;)
{
    int status = vx_get_message(&m);

    if(status == VX_GET_MESSAGE_AVAILABLE)
    {
        MyGamesMessageHandler(m);
        // Sample implementation below
        vx_destroy_message(m);
        // destroy the message
    }
    else if(status == VX_GET_MESSAGE_FAILURE)
    {
        // handle errors here. The only error that you might see here
        // is because vx_initialize3() has not been called.
    }
    else if(status == VX_GET_MESSAGE_NO_MESSAGE)
    {
        break;
    }
}
```

Example 3 (cpp):
```cpp
#include "Vxc.h"
#include "VxcErrors.h"
. . .
// this is your timer handler or your render loop

vx_message_base_t *m;

for(;;)
{
    int status = vx_get_message(&m);

    if(status == VX_GET_MESSAGE_AVAILABLE)
    {
        MyGamesMessageHandler(m);
        // Sample implementation below
        vx_destroy_message(m);
        // destroy the message
    }
    else if(status == VX_GET_MESSAGE_FAILURE)
    {
        // handle errors here. The only error that you might see here
        // is because vx_initialize3() has not been called.
    }
    else if(status == VX_GET_MESSAGE_NO_MESSAGE)
    {
        break;
    }
}
```

---

## Authorization

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/integration-guide/integration-plan/phase-one/authorization

**Contents:**
- Authorization#

All secured actions require authorization through an access token before they can be successfully carried out. This includes the following actions:

The recommended approach for authorization actions in this category is to authorize the requested action (for example, joining an area or match chat, or joining a party chat) by generating an access token and then delivering it to the client.

During development, you can generate insecure access tokens on the client by using the appropriate Client API call (vx_debug_generate_token).

In production, authorize secured voice actions by using a separate, trusted party, such as a game server. Configure the game server to create access tokens and then pass them to the game client to submit to Vivox APIs that require those tokens.

For more information, refer to the Access Token Developer Guide.

**Examples:**

Example 1 (unknown):
```unknown
vx_debug_generate_token
```

---

## Required permissions for Amazon S3 buckets

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/server-side-recording/ssr-required-permissions-amazon-s3-buckets

**Contents:**
- Required permissions for Amazon S3 buckets#

The following list details the required permissions for Amazon S3 buckets when using server-side recording (SSR).

---

## Anti-flooding

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/chat-history/anti-flooding

**Contents:**
- Anti-flooding#

Set rate limiting by configuring the pre-login parameter MaxTextMessageRate. Setting it to 0 disables the feature.

Rate limiting is enforced by the Vivox SDK. If a user tries to send messages at a rate higher than allowed, a VxErrorMessageTextRateExceeded error is returned.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_account_send_msg *req;
vx_req_account_send_msg_create(&req);
req->connector_handle = vx_strdup("c1");
req->account_handle = vx_strdup(".issuer-w-dev.mytestaccountname.");
req->language = vx_strdup(“en-us”);
req->user_uri = vx_strdup("sip:theotheruser@vd1.vivox.com");
req->message_body = vx_strdup("Hey there buddy!");
int status = vx_issue_request2(&req->base); // If rate limiting is surpassed, status = VxErrorMessageTextRateExceeded
```

Example 2 (unknown):
```unknown
vx_req_account_send_msg *req;
vx_req_account_send_msg_create(&req);
req->connector_handle = vx_strdup("c1");
req->account_handle = vx_strdup(".issuer-w-dev.mytestaccountname.");
req->language = vx_strdup(“en-us”);
req->user_uri = vx_strdup("sip:theotheruser@vd1.vivox.com");
req->message_body = vx_strdup("Hey there buddy!");
int status = vx_issue_request2(&req->base); // If rate limiting is surpassed, status = VxErrorMessageTextRateExceeded
```

---

## Couch co-op configuration

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/couch-co-op/couch-co-op-configuration

**Contents:**
- Couch co-op configuration#

Many Vivox API request and event types include an additional account_handle field that identifies a particular user on a device. In those request types, this field is optional; if left unset, the first user who signs in is the default.

Note: No changes are required in the Vivox SDK initialization workflow for the vx_initialize3() or the vx_req_connector_create request.

To begin a multi-user voice play session, simultaneously sign in multiple users by using the vx_req_account_anonymous_login request, with different account_handle values specified for each of the different users.

Note: For couch co-op implementations, you receive a unique set of events for each user who is signed in. Use the account_handle attribute to differentiate the events for each user. For more information, refer to Network connection state events.

After multiple users are signed in, you must assign them separate render and capture devices. You can design your game to automatically select these devices, or the game can present a UI to allow users to select their specific devices.

To enumerate the list of valid devices, use the vx_req_aux_get_render_devices and vx_req_aux_get_capture_devices requests.

To set the capture and render devices for each user, you must call vx_req_aux_set_render_device and vx_req_aux_set_capture_device with each user's account handle to assign specific devices to specific users.

Note: This request is required because audio devices are not automatically associated with each user.

Set other device attributes on a per-user basis, such as the mute status, volume, and voice activity detector (VAD) properties, by supplying the corresponding user's account_handle in the request type.

You can also add users to their own independent sets of session groups and sessions, or add them to the same sessions. If you add multiple users on the same device to the same session, then use the vx_req_session_set_participant_mute_for_me request to locally mute those users to each other so they do not hear an in-channel echo of anyone who is speaking that is physically nearby to them.

**Examples:**

Example 1 (unknown):
```unknown
account_handle
```

Example 2 (unknown):
```unknown
vx_initialize3()
```

Example 3 (unknown):
```unknown
vx_req_connector_create
```

Example 4 (unknown):
```unknown
vx_req_account_anonymous_login
```

---

## Example: Kick tokens

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/access-token-guide/access-token-examples/example-kick-token

**Contents:**
- Example: Kick tokens#
- Signed in user kicking other user from a channel#
- Admin user kicking other user from a channel#
- Admin user kicking other user from a server#

Note: There is no example of a signed in user kicking another user from a server because this action is not possible with the current API.

The token in this example allows the user "beef" to kick the user "jerky" from the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

The token in this example allows the user "blindmelon-AppName-dev-Admin" to kick the user "jerky" from the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

The token in this example allows the user "blindmelon-AppName-dev-Admin" to kick the user "jerky" from the entire server.

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

**Examples:**

Example 1 (unknown):
```unknown
{
    "vxi":665000,
    "sub":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "f":"sip:.blindmelon-AppName-dev.beef.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"kick",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 2 (unknown):
```unknown
{
    "vxi":665000,
    "sub":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "f":"sip:.blindmelon-AppName-dev.beef.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"kick",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 3 (unknown):
```unknown
e30.eyJ2eGkiOjY2NTAwMCwic3ViIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2Lmplcmt5LkB0bGEudml2b3guY29tIiwiZiI6InNpcDouYmxpbmRtZWxvbi1BcHBOYW1lLWRldi5iZWVmLkB0bGEudml2b3guY29tIiwiaXNzIjoiYmxpbmRtZWxvbi1BcHBOYW1lLWRldiIsInZ4YSI6ImtpY2siLCJ0Ijoic2lwOmNvbmZjdGwtZy1ibGluZG1lbG9uLUFwcE5hbWUtZGV2LnRlc3RjaGFubmVsQHRsYS52aXZveC5jb20iLCJleHAiOjE2MDAzNDk0MDB9.kKnWD3smth6KUuRaY11O-yqAbXy2L2wDZeIoDK_098c
```

Example 4 (unknown):
```unknown
e30.eyJ2eGkiOjY2NTAwMCwic3ViIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2Lmplcmt5LkB0bGEudml2b3guY29tIiwiZiI6InNpcDouYmxpbmRtZWxvbi1BcHBOYW1lLWRldi5iZWVmLkB0bGEudml2b3guY29tIiwiaXNzIjoiYmxpbmRtZWxvbi1BcHBOYW1lLWRldiIsInZ4YSI6ImtpY2siLCJ0Ijoic2lwOmNvbmZjdGwtZy1ibGluZG1lbG9uLUFwcE5hbWUtZGV2LnRlc3RjaGFubmVsQHRsYS52aXZveC5jb20iLCJleHAiOjE2MDAzNDk0MDB9.kKnWD3smth6KUuRaY11O-yqAbXy2L2wDZeIoDK_098c
```

---

## Acoustic echo cancellation

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/acoustic-echo-cancellation

**Contents:**
- Acoustic echo cancellation#

When certain devices, particularly mobile devices, or devices with integrated input and output devices, are being used, there's a chance that the input device will pick up on the audio from the output device. Vivox includes Acoustic Echo Cancellation (AEC), which helps to prevent this situation from occurring.

While devices will often have their own hardware acoustic echo cancellation, Vivox's software Acoustic Echo Cancellation can be further enabled using the VivoxService.Instance.EnableAcousticEchoCancellation() method, and can be disabled using the VivoxService.Instance.DisableAcousticEchoCancellation(). This provides a second level of AEC on top of the platform’s AEC, if the device has AEC already, or it becomes the only layer of AEC if the device doesn’t have its own AEC. Often, platform AEC will only activate in certain scenarios while the Vivox AEC ensures that at least one form of AEC will activate in most circumstances.

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.EnableAcousticEchoCancellation()
```

Example 2 (unknown):
```unknown
VivoxService.Instance.DisableAcousticEchoCancellation()
```

---

## SDK migration guide

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-diagnostics/manual/UserReporting/SDKMigrationGuide

**Contents:**
- SDK migration guide#
- Significant changes#
- Use the new example prefab#
- Create a report#
- Send a report#
- General changes to adding data#
- Add custom attachments#
- Conclusion#

Important: Cloud Diagnostics is deprecated, and will be phased out in future versions of Unity. For more robust reports and device information, including information on Application Not Responding (ANR) errors for Android, use the diagnostics experience available in Unity 6.2 and later. For information about migrating from Cloud Diagnostics, refer to Migrate to Diagnostics in the Unity Dashboard.

The User Reporting SDK has been updated, affecting the entire API.

Most significantly, the public API scope has been greatly reduced for development focus and support scope.

Important:: Critically this means if you're using the previous version of the SDK you won’t receive updates unless you move over to the new version. The old version might eventually become incompatible with future changes to the Unity Dashboard or User Reporting backend as we further develop new features.

User Reporting 1.0 refers to the old SDK; a reference distribution is available here. User Reporting 2.0 refers to the updated SDK available in the Unity Package Manager.

Outdated elements from the old SDK are no longer considered supported; although you can continue to use them if you rely on the helpers such as the SimpleJSON parser, we strongly encourage you to replace them with alternatives. Unity will not maintain or support such elements from the previous version of the SDK.

If you consider relying on old SDK helpers for a new feature in your project, such as using the PngHelper, we encourage you to explore alternatives rather than depending on the outdated SDK.

The User Reporting API focuses on User Report related functionality and helps prevent future API churn. It also helps Unity deliver meaningful support and improvements to the User Reporting aspect of the SDK exclusively.

The example starting point included in the older version of the SDK has been updated and replaced. It provides identical end-user functionality and can be replaced in the scene.

User Report objects are no longer hand-created. Instead, one report at a time can be created and sent - the report in progress is referred to as the “ongoing” report. If you create a new User Report, it clears the previous ongoing report (if a previous one exists).

UserReportingService.Instance.CreateNewUserReport();

If you want to collect and arrange data for more than one report at a time, you need to manage the responsibility of the data and then finally export said data into a report when ready to send.

The SDK now handles only one report at a time. Rather than providing a UserReport object to the send method, the API instead has a single call for sending the ongoing report: UserReportingService.Instance.SendUserReport(...).

Additionally, instead of a single callback the ongoing report now offers two separate optional callbacks: one for submission progress updates, and another for when the report submission attempt ends.

Adding individual object values to a List on an individual report object, uses a specialized function with the same arguments as the constructors used in the past applies everywhere.

See the following example, in this case adding a new report dimension:

1.0 adding a new report dimension example:

2.0 adding a new report dimension example:

UserReportingService.Instance.AddDimensionValue("Category", “Value”);

The SDK now has a function to add attachments to the ongoing report, instead of directly adding attachments to the User Report object itself:

Add (device) metadata The moniker “Device Metadata” has been generalized to metadata. Again, there is now a function for this purpose.

UserReportingService.Instance.AddMetadata(“Example Name”, “Example Value”)

The User Reporting dashboard remains identical as of the writing of this document.

See the official documentation for User Reporting when upgrading to the latest version of the SDK.

**Examples:**

Example 1 (unknown):
```unknown
Unity.Services.UserReporting
```

Example 2 (unknown):
```unknown
Unity.Services.UserReporting.Instance
```

Example 3 (unknown):
```unknown
CyclicalList
```

Example 4 (unknown):
```unknown
AsyncUnityUserReportingPlatform
```

---

## Android speakerphone audio: Mono or stereo

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/android/core-android-speakerphone-audio

**Contents:**
- Android speakerphone audio: Mono or stereo#

On Android, Vivox downmixes stereo output into mono output when it detects that the speakerphone is in use. Vivox downmixes the audio output because many Android devices play mono in speakerphone mode. Some devices only play one channel when provided with stereo audio, which makes the panning of 3D channels detrimental.

You can control whether speakerphone downmixing occurs for all devices by providing a list of exceptions by listing which brands, models, and devices to exclude. To change the default speakerphone downmix behavior, add an Android meta-data name-value pair to your application's AndroidManifest.xml. You can learn more about Android meta-data tags in the official Android documentation (Android Developer Documentation).

The metadata name to use is:

"com.vivox.sdk.downmix_speakerphone_enabled"

Note: There is a default exception that disables speakerphone downmix for Meta/Oculus devices. Changing this value overwrites this default exception.

The value must be a string starting with either true or false. To add a list of exceptions, add a comma after true or false and start a comma-delimited list of substrings to search for in the string generated for the current device, formatted as "BRAND MODEL DEVICE". The value string is case-insensitive.

The following are examples of valid value strings:

Note: Replace the example brands, models, and devices with values retrieved by android.os.Build.* strings.

The following is an example of disabling downmixing on Samsung devices:

**Examples:**

Example 1 (unknown):
```unknown
"com.vivox.sdk.downmix_speakerphone_enabled"
```

Example 2 (unknown):
```unknown
“true,samsung"
```

---

## Transmission modes

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/transmission/transmission-modes

**Contents:**
- Transmission modes#
- TransmissionMode::None#
- TransmissionMode::All#
- TransmissionMode::Single#

This is the default value of the TransmissionMode and results in no audio being broadcast to any channel.

If transmitting into a specific channel with TransmissionMode::Single, and that channel is disconnected entirely or has its audio capability disconnected, TransmissionMode reverts to TransmissionMode::None even if you are joined to other audio capable channels. If you want the plug-in to switch transmission to another one automatically, you can use the delegate callbacks or state change events that are found in IChannelSession. However, if set to TransmissionMode::All, you continue to transmit into all channels.

You can call ILoginSession::SetTransmissionMode(TransmissionMode::All) at any point to enable transmission into all current and future channels. This enables users to broadcast audio into all channels they are or will ever be connected to until you change the policy. There is no additional resource cost for transmitting to multiple channels as compared to a single channel.

Use this policy for when a user is in multiple audio-capable channels but only wants to speak in one at a time. In scenarios where the user is in one audio-only or audio and text channel at a time, this performs identically to TransmissionMode::All.

When setting TransmissionMode::Single by using ILoginSession::SetTransmissionMode(), you must also include a ChannelId as the second argument, specifying which single channel you want to transmit to. There is no benefit to preemptively setting this value for a channel you are not already connected to because the audio does not have anywhere to go.

The IChannelSession methods BeginConnect() and BeginSetConnectedAudio(), which let you join a channel or add audio capability to a text-only channel, each have the argument bool switchTransmission. When supplied with true, they override and set the TransmissionMode of the ILoginSession to transmit only to that specific channel, effective as soon as the channel is joined or audio capability is available. Setting this method to false results in following whatever policy ILoginSession already has set. By using this parameter, applications that only join one audio channel at a time usually do not have to call any transmission-specific APIs.

**Examples:**

Example 1 (unknown):
```unknown
TransmissionMode
```

Example 2 (unknown):
```unknown
TransmissionMode::Single
```

Example 3 (unknown):
```unknown
TransmissionMode
```

Example 4 (unknown):
```unknown
TransmissionMode::None
```

---

## Override development key generation

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/override-development-key-generation

**Contents:**
- Override development key generation#

Prerequisite: This topic assumes you have already created a token generation service.

Don't include development and testing codes, such as those generated in VxTokenGen.cs, in any final shipped build. Instead, write a new class that inherits from VxTokenGen.cs and overrides the existing functions. This sends the relevant information for each key out to the new secure token generation server.

Configure this new class to override the GetToken() method within VxTokenGen.cs. The following code displays an example of this class:

Afterward, use this class to replace the value of the Client.tokenGen variable. All further calls through GetLoginToken, GetJoinToken, GetMuteForAllToken, and GetTranscriptionToken will route their UserUri and other assorted parameters through to the server generation code.

**Examples:**

Example 1 (unknown):
```unknown
VxTokenGen.cs
```

Example 2 (unknown):
```unknown
VxTokenGen.cs
```

Example 3 (unknown):
```unknown
VxTokenGen.cs
```

Example 4 (unknown):
```unknown
public class MyTokenGenOverride : VxTokenGen
{
    public override string GetToken(string issuer = null, TimeSpan? expiration = null, string userUri = null, string action = null, string tokenKey = null, string conferenceUri = null, string fromUserUri = null)
    {
        //Server Token Generation calls go here
    }
}

public void SetTokenOverride()
{
    Client.tokenGen = new MyTokenGenOverride();
}
```

---

## Get last read message information from a given channel

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/chat-history/get-last-read-message-info

**Contents:**
- Get last read message information from a given channel#

To get the last read message information, you need to sign in and connect to at least one channel. You can then use vx_req_account_chat_history_get_last_read to make a request. This will return:

Note: Last read information is updated when a user disconnects from a specified channel.

The following code is an example of how to get the last read information from a channel. This assumes that you have already successfully added a session.

After you receive this information, the game then must process the vx_resp_account_chat_history_get_last_read_t response:

**Examples:**

Example 1 (unknown):
```unknown
vx_req_account_chat_history_get_last_read
```

Example 2 (unknown):
```unknown
vx_req_account_chat_history_get_last_read *req;
vx_req_account_chat_history_get_last_read_create(&req);
req->account_handle = vx_strdup("sip:.issuer.playerName@mt1s.vivox.com");
req->session_handle = vx_strdup("mychannel");
vx_issue_request2(&req->base);
```

Example 3 (unknown):
```unknown
vx_req_account_chat_history_get_last_read *req;
vx_req_account_chat_history_get_last_read_create(&req);
req->account_handle = vx_strdup("sip:.issuer.playerName@mt1s.vivox.com");
req->session_handle = vx_strdup("mychannel");
vx_issue_request2(&req->base);
```

Example 4 (unknown):
```unknown
vx_resp_account_chat_history_get_last_read_t
```

---

## Restore audio volume

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/ios/restore-audio-volume

**Contents:**
- Restore audio volume#

If the device is in a state in which the game audio is at the wrong volume when the Vivox SDK is using the microphone, you can restore the correct game audio volume by reconfiguring the AVAudioSession with:

Note: There is a brief interruption to the device audio during the restoration process.

**Examples:**

Example 1 (unknown):
```unknown
AVAudioSessionCategoryPlayAndRecord
```

Example 2 (unknown):
```unknown
AVAudioSessionModeVoiceChat
```

---

## Access token payload

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/access-token-guide/access-token-format/access-token-payload

**Contents:**
- Access token payload#
- Unity Authentication IDs within payloads#

The payload is a base64url-encoded JSON object that contains the claims asserted by the token. You can list claims in the payload in any order.

The following table details the claims that you can use in a token. For more information on which parameters are required for each token, see Access token examples.

If your application is using Unity Authentication Service to authenticate players, you can use the UAS ID within a Vivox access token to ensure proper player identity. SIP URIs, which are used to identify players, must be in the proper format to ensure that the player ID is parsed from the full SIP URI. This format is mandatory for using any Unity Moderation service with VATs.

The following is an example of the expected SIP format:

Note: It's the f, t, and sub fields that will need to include the Unity environment ID, shown in the format below, to work with UAS.

Note: Ensure that you only include the parameters from the example. Claims that contain the wrong parameters return "invalid signature" or "malformed payload" errors on the API calls.

Guarantee token uniqueness.

If all other claims are identical, this must be different or the token could be rejected as already used.

Note: We recommend that you use an unsigned integer that is increased by 1 for every token that is generated.

sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com

A user SIP URI that is used for mute and kick actions.

This is the user who is muted or unmuted, or the user who is kicked.

sip:.blindmelon-AppName-dev.beef.@tla.vivox.com

This is a user SIP URI that is used in all actions.

This is the user who performs actions such as signing in, joining the channel, or muting another user.

blindmelon-AppName-dev

An application-specific issuer.

The issuer is provided when you create an application on the Unity Dashboard.

For more information, see Supported values for the Vivox action claim.

sip:confctl-g-blindmelon-AppName-dev.testchannel.unity_environment_id@tla.vivox.com

This is a channel SIP URI that is used in join, mute, kick, and transcription actions.

This is the channel where the action takes place.

The expiration time as epoch seconds.

This value is usually the current time plus 90 seconds.

**Examples:**

Example 1 (unknown):
```unknown
sip:.issuer.unity_player_id.unity_environment_id.@domain.vivox.com
```

Example 2 (unknown):
```unknown
sip:.issuer.unity_player_id.unity_environment_id.@domain.vivox.com
```

---

## Basic concepts

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/integration-guide/basic-concepts

**Contents:**
- Basic concepts#
- Core elements#
- Game server elements#
- Game client elements#

The following diagram details the various components of the Vivox integration:

The Vivox integration includes the following elements:

**Examples:**

Example 1 (unknown):
```unknown
sip:\<destination>@\<domainName>
```

---

## Example: Mute tokens

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/access-token-guide/access-token-examples/example-mute-token

**Contents:**
- Example: Mute tokens#
- Signed in user muting other user in a channel#
- Admin user muting other user in a channel#

The token in this example allows the user "beef" to mute the user "jerky" in the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

The token in this example allows the user "blindmelon-AppName-dev-Admin" to mute the user "jerky" in the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

**Examples:**

Example 1 (unknown):
```unknown
{
    "vxi":123456,
    "sub":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "f":"sip:.blindmelon-AppName-dev.beef.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"mute",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 2 (unknown):
```unknown
{
    "vxi":123456,
    "sub":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "f":"sip:.blindmelon-AppName-dev.beef.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"mute",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 3 (unknown):
```unknown
e30.eyJ2eGkiOjEyMzQ1Niwic3ViIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2Lmplcmt5LkB0bGEudml2b3guY29tIiwiZiI6InNpcDouYmxpbmRtZWxvbi1BcHBOYW1lLWRldi5iZWVmLkB0bGEudml2b3guY29tIiwiaXNzIjoiYmxpbmRtZWxvbi1BcHBOYW1lLWRldiIsInZ4YSI6Im11dGUiLCJ0Ijoic2lwOmNvbmZjdGwtZy1ibGluZG1lbG9uLUFwcE5hbWUtZGV2LnRlc3RjaGFubmVsQHRsYS52aXZveC5jb20iLCJleHAiOjE2MDAzNDk0MDB9.vM9zkCXTORjgv8w7eiMHHHkc4DumTwR_-I06y4SnpHA
```

Example 4 (unknown):
```unknown
e30.eyJ2eGkiOjEyMzQ1Niwic3ViIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2Lmplcmt5LkB0bGEudml2b3guY29tIiwiZiI6InNpcDouYmxpbmRtZWxvbi1BcHBOYW1lLWRldi5iZWVmLkB0bGEudml2b3guY29tIiwiaXNzIjoiYmxpbmRtZWxvbi1BcHBOYW1lLWRldiIsInZ4YSI6Im11dGUiLCJ0Ijoic2lwOmNvbmZjdGwtZy1ibGluZG1lbG9uLUFwcE5hbWUtZGV2LnRlc3RjaGFubmVsQHRsYS52aXZveC5jb20iLCJleHAiOjE2MDAzNDk0MDB9.vM9zkCXTORjgv8w7eiMHHHkc4DumTwR_-I06y4SnpHA
```

---

## Access token terminology

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/access-token-terminology

**Contents:**
- Access token terminology#

Base64 encoding with all trailing '=' removed, '+' changed to '-', and '/' changed to '_'.

For more information, see the IETF documentation on RFC 7515.

The following example is an implementation of base64url encoding in PHP:

The following example is an implementation of base64url encoding in Python:

For an example implementation of base64url encoding in C#, see the IETF documentation on RFC 7515.

URL and filename safe string

A string composed of characters found in the URL and filename safe Base64 alphabet.

For more information, see the IETF documentation on RFC 4648.

**Examples:**

Example 1 (unknown):
```unknown
function base64url_encoded($str)
{
    return rtrim(strtr(base64_encoded($str), '+/', '-_'), '=')
}
```

Example 2 (unknown):
```unknown
function base64url_encoded($str)
{
    return rtrim(strtr(base64_encoded($str), '+/', '-_'), '=')
}
```

Example 3 (python):
```python
import base64
def base64url_encode(s):
"""Return a base64url-encoded str"""
return base64.urlsafe_b64encode(s).rstrip('=')
```

Example 4 (python):
```python
import base64
def base64url_encode(s):
"""Return a base64url-encoded str"""
return base64.urlsafe_b64encode(s).rstrip('=')
```

---

## Speaking indicator

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/integration-guide/integration-plan/phase-one/ui-integration/speaking-indicator

**Contents:**
- Speaking indicator#

Associate speaking indications with a game-defined icon or other visual display to indicate when the user is speaking.

An example of this indicator is detailed in the following image:

The following list details common UI that is used for speaking indicators:

---

## Access token header

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/access-token-format/access-token-header

**Contents:**
- Access token header#

The header in a Vivox access token is an empty JSON object (for example, {}).

Unlike a true JSON Web Token (JWT), this header is empty and not {"'alg":"HS256","typ":"JWT"}.

**Examples:**

Example 1 (unknown):
```unknown
{"'alg":"HS256","typ":"JWT"}
```

---

## Access token examples

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/access-token-guide/access-token-examples/access-token-examples-toc

**Contents:**
- Access token examples#

The access token examples in this section include spacing in the payload section which has been added for demonstration purposes. Do not include any spacing in the payload for your token.

Note: In the payload section for each example, you can list the claims in any order.

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/android/request-runtime-permissions

---

## Chat History query options

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/text-chat-guide/chat-history-query-options

**Contents:**
- Chat History query options#
- SearchText#
- TimeStart - TimeEnd#
- PlayerId#
- Channel-based chat history for blocked participants#
  - History sessions when blocked participants are involved#
  - Account chat history for blocked participants#

ChatHistoryQueryOptions is an optional options model that can be included with a VivoxService.Instance.GetDirectTextMessageHistoryAsync or VivoxService.Instance.GetChannelTextMessageHistoryAsync call to specify what messages to return.

Use the SearchText field of ChatHistoryQueryOptions to specify a string of text that should be in every message returned by the chat history query. For example, the following code will ensure that the historyMessages returned only include messages that contain the text "Hello, lobby!".

The TimeStart and TimeEnd fields of ChatHistoryQueryOptions are both DateTimes that delineate times after and before, respectively, that messages should have been sent in. For example, the following code will return the last 10 messages from between 24 hours before the request was made and 12 hours before the request was made. DateTime must be server time and not client side time.

Note that chat history is only stored for seven days, unless you have enabled Text Evidence Management.

For ChannelTextMessageHistory requests, the PlayerId field of ChatHistoryQueryOptions delineates the PlayerId that will be the sender of all returned messages. For example, the following code will return the last 10 messages sent from the specific participant in the specified channel.

You can filter messages from blocked users when formatting the history_session query responses.

When retrieving the chat history from a channel, the SDK omits messages from blocked users. If User A has User B blocked in the application, User A does not see messages sent by User B.

A history request can come back empty if all messages in the request are from a blocked participant. In this case, the response contains a cursor indicating that there are more messages in the channel on subsequent pages. The default amount of messages returned per history request is 10 per page. Use the cursor value in the following request to retrieve the next page of messages.

The maximum page size argument (Max) shows fewer messages than the number requested if there is a blocked user in the channel. When you request a specific page size for your chat history, be aware that history_session might display fewer messages than your chosen number if there are blocked users in the channel. This functionality is crucial to ensure privacy and prevent unwanted interactions.

The last read displays a count of unread messages. Vivox keeps track of your read messages and provides a count of unread messages. However, note that this count includes blocked messages, which are deliberately filtered from your response. The count of unread messages might be inaccurate if there are blocked users in the channel.

When you submit a chat history request for an account, queries only show the user’s sent messages.

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.GetDirectTextMessageHistoryAsync
```

Example 2 (unknown):
```unknown
VivoxService.Instance.GetChannelTextMessageHistoryAsync
```

Example 3 (unknown):
```unknown
ChatHistoryQueryOptions
```

Example 4 (unknown):
```unknown
historyMessages
```

---

## Unity Gaming Services CLI

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/write-modules/cli

**Contents:**
- Unity Gaming Services CLI#
- Prerequisites#
- Using the CLI#
  - Create modules#
  - Deploy modules#
    - Deploy C# solutions#
    - Deploy pre-compiled modules#
  - Environment synchronization#
  - Retrieve modules#
  - Delete module#

You can use the Unity Gaming Services CLI to interact with Cloud Code modules. The CLI allows you to create, deploy, and manage Cloud Code modules from the command line.

For a deep dive into the CLI, follow the steps in the Unity Gaming Services CLI Get Started guide.

To follow this guide, you must first complete the following actions:

Configure your Project ID and Environment as such:ugs config set project-id <your-project-id>ugs config set environment-name <your-environment-name>

Configure a Service Account with the required roles for Cloud Code and environments management. Refer to Get Authenticated.

Refer to the Cloud Code Command Line documentation for a full reference of all commands and options.

Note: The ugs cloud-code modules command is also available as ugs cc m.

Note: The CLI does not currently support custom tags for modules, and they will be ignored. If you would like to attach custom key-value pairs to your modules, use the Cloud Code API.

To locally create a solution with a sample module, you can run the new-file command:

The command creates a new solution with a sample module.

You can either use the solution as it is or modify it to suit your needs.

Note: Storage limits apply. Check Cloud Code Limits.

The Deploy command allows you to deploy Cloud Code modules to the remote environment. The command supports both pre-compiled modules in .ccm format and C# solutions, which automatically compile and zip before deployment.

By default, the deployment is based on the Release build configuration of your solution. This configuration should avoid including extra content such as test projects to reduce the size. To learn more on build configurations, visit Rider or Visual Studio's documentation.

To deploy multiple modules at once, either provide multiple file paths or a directory containing multiple files:

You can deploy C# solution and have it automatically compile and zip before deployment.

To support compilation, the solution must contain a publish profile for the main project.

Refer to specific IDE documentation to learn how to create a publish profile:

You should also configure your solution to exclude unit test projects. For more information, refer to Create a unity test project.

When deploying solutions, they automatically compile and zip before deployment. The result saves your local temporary folder (for example, <temp-folder>/<solution-name>).

To deploy C# solutions, provide a path to a .sln file as an argument to the command:

Important: Currently, the solution deployment supports only one main project per solution. The main project is the project that contains a publish profile. You can't deploy solutions that contain multiple modules. Additionally, make sure all projects for this solution are under the solution file in the folder hierarchy. To understand how to structure your solution, refer to Module structure.

To deploy pre-compiled modules, provide a path to a .ccm file as an argument to the command:

Note: For most use cases, you can use the deploying solutions workflow. As a more advanced workflow, you can package and deploy pre-compiled modules. To learn how to compress your module into a .ccm file, refer to Package code.

You can move all your modules from one environment and deploy them to another environment.

Run the following command to produce an archive of all the modules in your current environment:

You can then import the modules and deploy them to another environment by running the following command:

Note: Import does not overwrite your environment by default. If you would like to delete existing modules before importing, use the --reconcile flag.

To get information about a single module that you deployed, run the following command:

The module name is the name of the .NET class library that is contained within the .ccm archive. You can also list all modules currently deployed to Cloud Code by running the following command:

To delete a module, run the following command:

**Examples:**

Example 1 (unknown):
```unknown
ugs config set project-id <your-project-id>
```

Example 2 (unknown):
```unknown
ugs config set environment-name <your-environment-name>
```

Example 3 (unknown):
```unknown
ugs cloud-code modules
```

Example 4 (unknown):
```unknown
ugs cloud-code modules new-file <module-name> <module-directory>
```

---

## Game server integration

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/integration-guide/integration-plan/phase-one/game-server-integration

**Contents:**
- Game server integration#

Game server integration primarily focuses on determining which game server component owns a user's login ID and which component owns a channel's ID. Additionally, consider how to determine relationships between channels, if any.

After you select appropriate login and channel ID schemes, you can associate users and channels with their respective game data. Because sign-ins and channels are created on an ad-hoc basis and identifiers are not established beforehand, your game's server code determines how to manage and use Vivox logins and channel joins.

Note: Generally, login identifiers are associated with a user's account, and channel identifiers are associated with social organizations such as guilds, parties, and matches.

---

## Unity Editor

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/write-modules/unity-editor

**Contents:**
- Unity Editor#
- Prerequisites#
  - Link project#
  - Install required packages#
  - Install .NET#
- Preview the Deployment window#
- Author within Unity Editor#
  - Create a C# Module Reference file#
  - Link a module#
    - Generate a new module#

You can use the Unity Editor to create and deploy Cloud Code modules. The Cloud Code package contains the Cloud Code Authoring module, which allows you to use the Deployment package to interact with modules.

Note: Only Unity 2021.3 and above support Unity Editor integration for modules.

To use Cloud Code in the Unity Editor, follow the steps below.

Link your Unity Gaming Services project with the Unity Editor. You can find your UGS project ID in the Unity Dashboard.

In Unity Editor, select Edit > Project Settings > Services.

If your project doesn't have a Unity project ID:

If you have an existing Unity project ID:

Your Unity Project ID appears, and the project is now linked to Unity services.

You can also use the UnityEditor.CloudProjectSettings.projectId parameter to access your project ID in a Unity Editor script.

To create Cloud Code modules within the Editor, you must install the following packages:

Note: Check Unity - Manual: Package Manager window to familiarize yourself with the Unity Package Manager interface.

Install these packages, and add them to your list of available packages:

To deploy Cloud Code modules in the editor, you first need to install .NET.

Follow the steps below to set your default .NET path in editor:

The Deployment window allows you to deploy Cloud Code modules to your remote environment. If you installed the Deployment package, you can access it from the Unity Editor.

Before you can use the Deployment window, you have to select the environment you want to deploy to.

Note: The Deployment window also supports other Unity Gaming services. If your Cloud Code modules rely on other services, you can deploy changes at the same time.

Refer to the Deployment window for more information.

The installed Cloud Code package contains the Cloud Code Authoring module, which allows you to create and deploy Cloud Code modules directly from the Unity Editor.

To do this, create a module reference file in the editor and link it to your module project. Once configured, the Deployment window allows you to deploy your module to the remote environment.

To get started, create a Cloud Code C# Module Reference file. This file is a reference to a solution containing the C# module project you want to deploy.

Note: The module deployed has the same name as the main project in the solution.

The new module reference is now visible in the Project window and in the Deployment window. To access the module reference, open the Deployment window.

The C# Module Reference needs to be linked to a module. You can either link an existing solution or generate a new one from the Unity Editor.

With a module reference file, you can generate a new module from the Deployment window. The generated module contains the required basic setup for you to start to develop your module.

You can also generate a new module directly from the Deployment window.

This places the generated module in the root of your project directory. Cloud Code uses the reference file name to generate the default solution path. The solution file name is the solution path Cloud Code uses to generate the module template.

For example, if you call your reference file test_module.ccmr, your default solution path might be ../test_module, which creates a module under a solution file of test_module.sln.

If you want to use a different authoring method to create a new module, refer to Write modules. The module reference comes with a default solution path. You should change this path to point to your existing module solution.

You can generate type-safe client code from the Unity Editor. Bindings generate from the module reference file, and you can use the bindings to call the module endpoints. A type-safe client guarantees the types used by the game-client and received by the module match, ensuring no serialization problems.

You can generate bindings from the following locations:

Generate from the Inspector window:

Generate from the Deployment Window:

Generate from the toolbar:

Generate from Project Settings:

Unity Editor generates the type-safe client code under the Assets/CloudCode/GeneratedModuleBindings directory.

Note: If you have an assembly definition, you might have to reference the generated assembly definition. Add "Unity.Services.CloudCode.GeneratedBindings" to your asmdef's references.

Refer to Run modules in Unity Runtime to learn how to use the generated bindings.

The bindings generation has the following limitations:

You can use the Deployment window to deploy your modules. You can also automatically deploy modules when you enter the Play mode in the Unity Editor.

To deploy the module:

Important: By default, the deployment is based on the Release build configuration of your solution. To reduce the size, this configuration should avoid including extra content such as test projects. To learn more on build configurations, visit Rider or Visual Studio's documentation and Create a unit test project.

Check the Deployment package manual for more information.

A successful deployment shows a green checkmark next to the module reference file in the Project window.

You can verify the deployment is successful by calling the module endpoints. The template module contains a default endpoint called SayHello.

Refer to Run modules in Unity Runtime to learn how to call the module endpoints.

**Examples:**

Example 1 (unknown):
```unknown
UnityEditor.CloudProjectSettings.projectId
```

Example 2 (unknown):
```unknown
com.unity.services.deployment
```

Example 3 (unknown):
```unknown
com.unity.services.cloudcode
```

Example 4 (unknown):
```unknown
test_module.ccmr
```

---

## Android SDK permission requirements

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/android/android-sdk-permission-requirements

**Contents:**
- Android SDK permission requirements#

The following permission requirements apply when working with the Vivox Android SDK:

android.permission.INTERNET: Allows communication with Vivox servers

android.permission.RECORD_AUDIO: Allows microphone access

For more information, refer to the Android developer documentation on requesting app permissions.

Note: Not giving these permissions might prevent some Vivox SDK features from working properly.

android.permission.MODIFY_AUDIO_SETTINGS

android.permission.ACCESS_NETWORK_STATE: Allows access to network information

android.permission.ACCESS_WIFI_STATE: Allows access to Wi-Fi information

android.permission.BLUETOOTH: Allows access to Bluetooth devices on Android versions earlier than 12

android.permission.BLUETOOTH_CONNECT: Allows access to Bluetooth devices on Android version 12 and later

Note: On Android 12 (Android SDK version 31) and later, you need to request the android.permission.BLUETOOTH_CONNECT permission at runtime. For more information, see the Android developer documentation on Bluetooth permissions.

These permissions will be given by default to your app after it is compiled with the Vivox SDK.

If you need to remove an optional permission, for example android.permission.BLUETOOTH_CONNECT, you can add the permission tag to your application AndroidManifest.xml and then specify tools:node”remove” to ensure that it is not included:

**Examples:**

Example 1 (unknown):
```unknown
android.permission.INTERNET
```

Example 2 (unknown):
```unknown
android.permission.RECORD_AUDIO
```

Example 3 (unknown):
```unknown
android.permission.MODIFY_AUDIO_SETTINGS
```

Example 4 (unknown):
```unknown
android.permission.ACCESS_NETWORK_STATE
```

---

## Retrieve account-wide chat history

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/chat-history/retrieve-account-chat-history

**Contents:**
- Retrieve account-wide chat history#

To retrieve account-wide chat history, you need to sign in and use vx_req_account_chat_history_query to make a request.

vx_req_account_chat_history_query will return 10 messages by default. You can increase the number of messages returned by increasing the max value in the request. The maximum number of messages stored per channel is 5000, with a maximum age of 7 days.

You will receive a vx_evt_account_archive_message event for every message.

The following code is an example of how to retrieve 20 messages for a particular user across all of their channels:

After you get these messages, the game must then process two types of event messages:

**Examples:**

Example 1 (unknown):
```unknown
vx_req_account_chat_history_query
```

Example 2 (unknown):
```unknown
vx_req_account_chat_history_query
```

Example 3 (unknown):
```unknown
vx_evt_account_archive_message
```

Example 4 (unknown):
```unknown
vx_req_account_chat_history_query *req;
vx_req_account_chat_history_query_create(&req);
req->account_handle = vx_strdup("sip:.issuer.playerName@mt1s.vivox.com");
req->max = 20;
vx_issue_request2(&req->base);
```

---

## User-to-user muting in all channels

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/muting/mute-all-channels

**Contents:**
- User-to-user muting in all channels#

To mute one or more users in all channels, use vx_req_account_control_communications. You can call this request before any session is established, because it requires only the account handle of the muter or the user who wants to mute other users.

vx_req_account_control_communications can support a list of users, which means that you can mute multiple users at a time. Separate the user URIs with "\n".

The response contains successfully muted users from the specified list. If a user is already muted or is cross-muted by a previous request, then the response does not contain their URI.

The following table details the possible operations for this request:

vx_control_communications_operation_mute = 4

Remove mute or unmute

vx_control_communications_operation_unmute = 5

Get a list of muted users

vx_control_communications_operation_mute_list = 6

Remove mute from all users

vx_control_communications_operation_clear_mute_list = 7

**Examples:**

Example 1 (unknown):
```unknown
vx_req_account_control_communications
```

Example 2 (unknown):
```unknown
vx_req_account_control_communications
```

Example 3 (unknown):
```unknown
vx_req_account_control_communications_t* reqStruct;
vx_req_account_control_communications_create(&reqStruct);
reqStruct->account_handle = vx_strdup(“session handle”);
reqStruct->operation = vx_control_communication_operation_block;
reqStruct->user_uris =
    vx_strdup(
        “sip:.user1.@realm.vivox.com\n”
        “sip.userN.@realm.vivox.com”);

int ret = vx_issue_request3((vx_req_base_t*) reqStruct);
```

Example 4 (unknown):
```unknown
vx_req_account_control_communications_t* reqStruct;
vx_req_account_control_communications_create(&reqStruct);
reqStruct->account_handle = vx_strdup(“session handle”);
reqStruct->operation = vx_control_communication_operation_block;
reqStruct->user_uris =
    vx_strdup(
        “sip:.user1.@realm.vivox.com\n”
        “sip.userN.@realm.vivox.com”);

int ret = vx_issue_request3((vx_req_base_t*) reqStruct);
```

---

## Request capture device access

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/channels/request-capture-device-access

**Contents:**
- Request capture device access#

To ensure that voice functions as expected, developers who are using the Vivox SDK with applications on the following platforms must request capture device access before joining a channel.

Each platform has various ways to request capture device access. For example, you can use runtime permissions to ask a user if they want to give an application access to a capture device. For information about making runtime permission requests, refer to the platform-specific documentation that is maintained by each platform manufacturer.

Note: If an application does not request capture device access, then problems could occur, such as capture device or all channel audio not working until the channel has been left and rejoined.

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/unity-services-integration

---

## Voice activity detection

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/troubleshooting/voice-activity-detection-unity

**Contents:**
- Voice activity detection#
- Parameter specifics#

Voice activity detection detects the presence or absence of speech in an application. In most cases, customers do not need to adjust the default voice activity detection (VAD) settings of the Vivox SDK.

Tip: Before manually tuning the VAD settings, test your setup using Automatically Adjusted VAD. Automatically Adjusted VAD is better at detecting a player speaking than the default VAD settings.

The Automatically Adjusted VAD can be enabled using VivoxService.Instance.EnableAutoVoiceActivityDetectionAsync(). This will allow the SDK to automatically configure the VoiceActivityDetection settings. This will override any manual settings from VivoxService.Instance.SetVoiceActivityDetectionPropertiesAsync().

If the Automatically Adjusted VAD has been disabled using VivoxService.Instance.DisableAutoVoiceActivityDetectionAsync(), then VivoxService.Instance.SetVoiceActivityDetectionPropertiesAsync(int hangover, int noiseFloor, int sensitivity) can be called to either set the properties specifically, or to reset them to the default levels.

The hangover parameter defines the amount of time (in milliseconds) it takes for the VAD to switch from speech mode back to silence after the last frame of speech is detected. The default setting is 2000.

The noiseFloor parameter is a dimensionless value between 0 and 20000 that controls how the VAD separates speech from background noise. Lower values assume the user is in a quieter environment where the audio is only speech. Higher values assume a noisy background environment. The default value is 576.

Note: Changes to the VAD noiseFloor settings do not affect currently joined channels. If the ability to change VAD settings is available to the end-user, indicate that noise floor changes only take effect in the next voice session or only allow changing the noise floor channel when the client is not in a channel.

The sensitivity parameter is a dimensionless value between 0 and 100 that indicates the sensitivity of the VAD. Increasing this value corresponds to decreasing the sensitivity of the VAD (0 is the most sensitive, and 100 is the least sensitive). Higher values of sensitivity require louder audio to trigger the VAD. The default value is 43.

Applications that use the default VAD and expose vad_sensitivity as a slider should limit the possible settings between zero (transmit all mic activity) and 70 (very selective).

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.EnableAutoVoiceActivityDetectionAsync()
```

Example 2 (unknown):
```unknown
VivoxService.Instance.SetVoiceActivityDetectionPropertiesAsync()
```

Example 3 (unknown):
```unknown
VivoxService.Instance.DisableAutoVoiceActivityDetectionAsync()
```

Example 4 (unknown):
```unknown
VivoxService.Instance.SetVoiceActivityDetectionPropertiesAsync(int hangover, int noiseFloor, int sensitivity)
```

---

## Configure server density

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/guides/configure-server-density

**Contents:**
- Configure server density#
- Configuring cloud server density#
- Configuring bare metal server density#

The number of servers that can run per machine is called the server density, set at the fleet level. This server density helps match the number of available game servers to the target buffer. There are separate workflows for configuring cloud and bare metal server density.

To configure server density on a cloud machine:

To configure server density on a bare metal machine:

Note: Configuring density for bare metal machines is a little different than for cloud, as bare metal density settings apply to all bare metal machines in a fleet. This means that you can’t input a value for Servers per machine for bare metal server density as you can for cloud, as the amount of servers on a machine differs depending on the hardware specification of that machine.

The Bare Metal Server Density tab shows the current server density configuration, as well as an inventory table displaying each bare metal machine specification within the selected fleet. The inventory table shows for each specification the following information:

To edit the current server density configuration:

---

## Example: Join token

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/access-token-guide/access-token-examples/example-join-token

**Contents:**
- Example: Join token#

The token in this example allows the user "jerky" to join the channel "sip:confctl-g-blindmelon.testchannel\@tla.vivox.com".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

**Examples:**

Example 1 (unknown):
```unknown
"sip:confctl-g-blindmelon.testchannel\@tla.vivox.com"
```

Example 2 (unknown):
```unknown
{
    "vxi":444000,
    "f":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"join",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 3 (unknown):
```unknown
{
    "vxi":444000,
    "f":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"join",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 4 (unknown):
```unknown
e30.eyJ2eGkiOjQ0NDAwMCwiZiI6InNpcDouYmxpbmRtZWxvbi1BcHBOYW1lLWRldi5qZXJreS5AdGxhLnZpdm94LmNvbSIsImlzcyI6ImJsaW5kbWVsb24tQXBwTmFtZS1kZXYiLCJ2eGEiOiJqb2luIiwidCI6InNpcDpjb25mY3RsLWctYmxpbmRtZWxvbi1BcHBOYW1lLWRldi50ZXN0Y2hhbm5lbEB0bGEudml2b3guY29tIiwiZXhwIjoxNjAwMzQ5NDAwfQ.u7us5eCxOBtuEZuDg1HapEEgxLedLaliIy7gOMfbeko
```

---

## Acoustic echo cancellation

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/ios/acoustic-echo-cancellation

**Contents:**
- Acoustic echo cancellation#

When an iOS device plays and records audio at the same time, there is the possibility that a capture device could pick up the audio that is playing. The Vivox SDK includes acoustic echo cancellation (AEC), which helps to prevent this type of audio capture from occurring.

By default, the Vivox SDK leverages the platform AEC provided by Apple, which is enabled or disabled by using dynamic voice processing switching (DVPS).

DVPS is enabled by default. To disable DVPS, perform either of the following actions:

Note: Disabling DVPS is not recommended.

DVPS uses the following logic for enabling or disabling platform AEC:

DVPS causes Vivox to play and record audio by using a Voice-Processing I/O audio unit. It also makes changes to the shared AVAudioSession instance to ensure that the right category and mode are selected. When platform AEC is enabled, the AVAudioSessionCategory is set to AVAudioSessionCategoryPlayAndRecord, and the AVAudioSessionMode is set to AVAudioSessionModeVoiceChat. After all Vivox voice sessions are removed, DVPS automatically switches back to the original AVAudioSession configuration. For optimal results, before you initialize the Vivox SDK, we recommend that you set the AVAudioSessionCategory to AVAudioSessionCategoryPlayback.

DVPS enforces the use of the Bluetooth Headset Profile (HSP) when accessing the microphone on a connected Bluetooth device. This ensures that the headset microphone is used instead of the device microphone to provide a better voice chat experience.

In some scenarios, it might be preferable to prevent the Vivox SDK from making changes to the shared AVAudioSession instance. To manually control platform AEC, disable dynamic voice processing switching (DVPS), and then use the vx_set_platform_aec_enabled() function to enable or disable platform AEC. This function reconfigures the audio unit in the Vivox SDK to Voice-Processing I/O or Remote I/O when enabled or disabled, respectively.

**Examples:**

Example 1 (unknown):
```unknown
vx_set_platform_aec_enabled()
```

---

## Access token terminology

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/access-token-guide/access-token-terminology

**Contents:**
- Access token terminology#

Base64 encoding with all trailing '=' removed, '+' changed to '-', and '/' changed to '_'.

For more information, see the IETF documentation on RFC 7515.

The following example is an implementation of base64url encoding in PHP:

The following example is an implementation of base64url encoding in Python:

For an example implementation of base64url encoding in C#, see the IETF documentation on RFC 7515.

URL and filename safe string

A string composed of characters found in the URL and filename safe Base64 alphabet.

For more information, see the IETF documentation on RFC 4648.

**Examples:**

Example 1 (unknown):
```unknown
function base64url_encoded($str)
{
    return rtrim(strtr(base64_encoded($str), '+/', '-_'), '=')
}
```

Example 2 (unknown):
```unknown
function base64url_encoded($str)
{
    return rtrim(strtr(base64_encoded($str), '+/', '-_'), '=')
}
```

Example 3 (python):
```python
import base64
def base64url_encode(s):
"""Return a base64url-encoded str"""
return base64.urlsafe_b64encode(s).rstrip('=')
```

Example 4 (python):
```python
import base64
def base64url_encode(s):
"""Return a base64url-encoded str"""
return base64.urlsafe_b64encode(s).rstrip('=')
```

---

## Call from Unity game server (Multiplay Hosting)

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/run-modules/game-server

**Contents:**
- Call from Unity game server (Multiplay Hosting)#
- Prerequisites#
- Call out to Cloud Code#
  - Authentication#
    - Authenticate using a Multiplay Hosting token#
    - Authenticate using a stateless token#
  - Call a module endpoint#

You can call a module endpoint from a dedicated game server using Multiplay Hosting.

Note: If a module has not received any traffic in the last 15 minutes, you may experience a cold start latency. Any subsequent calls to the module are faster.

Follow the steps below to call out to a Cloud Code module endpoint from a game server.

You can authenticate requests using a Multiplay Hosting token or a stateless token. Use the received token as a bearer token for HTTP authentication in the request header.

Authenticating with a Multiplay token is recommended. Authenticating with a stateless tokens requires you to create a service account and store the private key securely, whereas Multiplay tokens do not require this and are less complex to manage.

See the Game Server Documentation here.

To use a stateless token, you need to create a service account and call the Token Exchange API. Refer to authenticating trusted clients for the Cloud Code Client API.

You can call a module endpoint using any HTTP library that is native to the codebase. Use the retrieved authentication token as a bearer token for HTTP authentication in the request header. An example CURL request to call an endpoint using the Cloud Code API could look like this:

**Examples:**

Example 1 (unknown):
```unknown
curl -X POST -H "Authorization: Bearer <BEARER_TOKEN>" 'https://cloud-code.services.api.unity.com/v1/projects/<PROJECT_ID>/modules/<MODULE_NAME>/<FUNCTION_NAME>'
```

Example 2 (unknown):
```unknown
curl -X POST -H "Authorization: Bearer <BEARER_TOKEN>" 'https://cloud-code.services.api.unity.com/v1/projects/<PROJECT_ID>/modules/<MODULE_NAME>/<FUNCTION_NAME>'
```

---

## Unity Dashboard

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/write-modules/unity-dashboard

**Contents:**
- Unity Dashboard#
- Preview modules#
- Preview module details#
- Download your module API specification#
- Delete a module#

Note: The Unity Dashboard provides limited support for Cloud Code C# Modules.

The Unity Dashboard provides a simple way to preview the deployed modules.

You can access a list of all Cloud Code modules for an environment from the Unity Dashboard. To access it:

A list of all Cloud Code modules in the selected environment for the project appears. The table contains the name, tags, date created, and last uploaded date.

Note: You can't create or edit modules from the Unity Dashboard.

To preview the details of a module, select the module name in the list of modules.

The Details table for the module displays the module's name, date created and last uploaded date.

The Endpoints section displays the list of endpoints defined in the module, along with the parameters and return type for each endpoint.

You can download the API specification for your module in OpenAPI format. To download the API specification from the Modules page, select the Download API spec icon.

You can also download the API specification from the details page of the module if you select the Download API spec button.

To delete a module from the Unity Dashboard:

You can also select the Delete button on the details page of a module to delete the module.

Warning: Deletion of a module is irreversible. If a live game uses the module that you delete, you may cause unexpected behavior in your game.

---

## Example: Login token

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/access-token-guide/access-token-examples/example-login-token

**Contents:**
- Example: Login token#

The token in this example allows the user "jerky" to sign in to the server. If you're using Unity Authentication Service for user management, put the UASID in the place of "jerky".

Note: UASIDs, also known as the PlayerID within Authentication, is a random alphanumeric string of 28 characters used to identify users. Refer to How Authentication works for more information. UASIDs are required for using VATs with Unity Moderation services.

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

**Examples:**

Example 1 (unknown):
```unknown
{
   "iss":"blindmelon-AppName-dev",
   "vxi":933000,
   "vxa":"login",
   "exp":1600349400,
   "f":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com"
}
```

Example 2 (unknown):
```unknown
{
   "iss":"blindmelon-AppName-dev",
   "vxi":933000,
   "vxa":"login",
   "exp":1600349400,
   "f":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com"
}
```

Example 3 (unknown):
```unknown
e30.eyJpc3MiOiJibGluZG1lbG9uLUFwcE5hbWUtZGV2IiwidnhhIjoibG9naW4iLCJ2eGkiOjkzMzAwMCwiZXhwIjoxNjAwMzQ5NDAwLCJmIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2Lmplcmt5LkB0bGEudml2b3guY29tIn0.5FKsAQz7bRQGGlSZw-KIcCmft7Ic6nT3Ih-TbdYPWwI
```

Example 4 (unknown):
```unknown
e30.eyJpc3MiOiJibGluZG1lbG9uLUFwcE5hbWUtZGV2IiwidnhhIjoibG9naW4iLCJ2eGkiOjkzMzAwMCwiZXhwIjoxNjAwMzQ5NDAwLCJmIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2Lmplcmt5LkB0bGEudml2b3guY29tIn0.5FKsAQz7bRQGGlSZw-KIcCmft7Ic6nT3Ih-TbdYPWwI
```

---

## Get last read message information from a given channel

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/chat-history/get-last-read-message-info

**Contents:**
- Get last read message information from a given channel#

To get the last read message information, you need to sign in and connect to at least one channel. You can then use vx_req_account_chat_history_get_last_read to make a request. This will return:

Note: Last read information is updated when a user disconnects from a specified channel.

The following code is an example of how to get the last read information from a channel. This assumes that you have already successfully added a session.

After you receive this information, the game then must process the vx_resp_account_chat_history_get_last_read_t response:

**Examples:**

Example 1 (unknown):
```unknown
vx_req_account_chat_history_get_last_read *req;
vx_req_account_chat_history_get_last_read_create(&req);
req->account_handle = vx_strdup("sip:.issuer.playerName@mt1s.vivox.com");
req->session_handle = vx_strdup("mychannel");
vx_issue_request2(&req->base);
```

Example 2 (unknown):
```unknown
vx_req_account_chat_history_get_last_read *req;
vx_req_account_chat_history_get_last_read_create(&req);
req->account_handle = vx_strdup("sip:.issuer.playerName@mt1s.vivox.com");
req->session_handle = vx_strdup("mychannel");
vx_issue_request2(&req->base);
```

Example 3 (unknown):
```unknown
void HandleAccountGetLastReadResponse(vx_resp_account_chat_history_get_last_read_t *resp)
{
    // Handles the response, uses the number of unread messages to fetch history
}
```

Example 4 (unknown):
```unknown
void HandleAccountGetLastReadResponse(vx_resp_account_chat_history_get_last_read_t *resp)
{
    // Handles the response, uses the number of unread messages to fetch history
}
```

---

## Set presence information

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/presence/set-presence-information

**Contents:**
- Set presence information#

A user can set their presence information by using the vx_req_account_set_presence message.

The following code displays an example of this process:

**Examples:**

Example 1 (unknown):
```unknown
vx_req_account_set_presence_message   req;
vx_req_account_set_presence_message_create(&req);
req->account_handle = vx_strdup(".issuer-w-dev.mytestaccountname.");
req->presence = buddy_presence_busy;
req->custom_message = vx_strdup(“I am busy cleaning my keyboard.”);
vx_issue_request3(&req->base);
```

Example 2 (unknown):
```unknown
vx_req_account_set_presence_message   req;
vx_req_account_set_presence_message_create(&req);
req->account_handle = vx_strdup(".issuer-w-dev.mytestaccountname.");
req->presence = buddy_presence_busy;
req->custom_message = vx_strdup(“I am busy cleaning my keyboard.”);
vx_issue_request3(&req->base);
```

---

## Example: Join_Muted token

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/access-token-guide/access-token-examples/example-join-muted-token

**Contents:**
- Example: Join_Muted token#

The token in this example allows the user "jerky" to join the channel "sip:confctl-g-blindmelon.testchannel\@tla.vivox.com" in a muted state.

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

**Examples:**

Example 1 (unknown):
```unknown
"sip:confctl-g-blindmelon.testchannel\@tla.vivox.com"
```

Example 2 (unknown):
```unknown
{
    "vxi":542680,
    "f":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"join_muted",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 3 (unknown):
```unknown
{
    "vxi":542680,
    "f":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"join_muted",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 4 (unknown):
```unknown
e30.eyJ2eGkiOjU0MjY4MCwiZiI6InNpcDouYmxpbmRtZWxvbi1BcHBOYW1lLWRldi5qZXJreS5AdGxhLnZpdm94LmNvbSIsImlzcyI6ImJsaW5kbWVsb24tQXBwTmFtZS1kZXYiLCJ2eGEiOiJqb2luX211dGVkIiwidCI6InNpcDpjb25mY3RsLWctYmxpbmRtZWxvbi1BcHBOYW1lLWRldi50ZXN0Y2hhbm5lbEB0bGEudml2b3guY29tIiwiZXhwIjoxNjAwMzQ5NDAwfQ.N6sZL3F3e-p2KLQlMweXnbGNzE7Qc91rn_uqCEtRjsc
```

---

## Specify the scene to build

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/advanced-build-configuration/specify-the-scene-to-be-built

**Contents:**
- Specify the scene to build#

By default, Unity Build Automation builds the scenes you’ve added to your project in the Unity Editor to File > Build Settings > scenes in Build. You can set up Build Automation to ignore the Scenes in Build list in the Unity Editor and provide a Scene List in the Unity Dashboard to build a different list of scenes:

This is useful for development. For example, you might have one build target that includes all the scenes but takes a long time to build. If you’re only working on one scene, it might be helpful to use another build target that only includes that scene. Specified scenes build much faster, and allow for quicker iteration of development versions.

---

## Migrating to version 5.0.0

**URL:** https://docs.unity.com/analytics/en/manual/AnalyticsSDK5MigrationGuide

**Contents:**
- Migrating to version 5.0.0#
- Initialization flow#
- Opting out#

The 5.x versions of the Analytics SDK include numerous changes to the initialization and consent flow compared to 4.x versions. This guide will help you navigate these changes.

Note: The legacy initialization and consent logic is still present in version 5.0.0 for backwards compatibility during migration, but you should switch to the new logic as early as possible to ensure the SDK is working as intended. The legacy flow operations are marked with the Obsolete attribute and will be removed in a future version of the SDK.

The legacy initialization flow includs a number of automatic steps, including looking up the player's location (using GeoIP) to determine whether or not consent is required for data collection, and if data collection is allowed until explicitly revoked. This is changing.

In the new initialization flow, the SDK does not look up the player's location, and does not perform any activity automatically. The new initialization flow requires the SDK to be explicitly activated by calling the StartDataCollection() method. This method should only be called if one of the following is true:

Calling UnityServices.InitializeAsync() still initializes the Analytics SDK. However, this initialization only bring the SDK into memory and it remains dormant until you explicitly activate it with a call to StartDataCollection(). Similarly, if you deactivate the SDK with the StopDataCollecton() method, the SDK stays in memory, so you can reactivate it later if the player provides or reinstates consent some time after initialization.

Note: When you invoke a method from either the new or old flow, this locks the SDK into only accepting further calls associated with that flow. If you attempt to call methods from both flows together, these will throw exceptions.

The legacy initialization flow is similar to this:

In versions of the SDK prior to 5.0.0, services initialization makes a web request to look up the player's location to determine whether the player is in a region subject to certain privacy legislation. If this request fails, it is retried inside the CheckForRequiredConsents() call. If this second attempt fails, the SDK is unable to collect data at all. This effectively requires the application to be initialized with internet connectivity in order to collect data, regardless of whether or not consent has been given previously (which would allow events to be batched and stored for upload when connectivity is restored).

The new initialization flow does not make any web requests. In exchange, you must build your own logic to determine if a player is located in a region where consent is required to collect their personal data as required by certain data privacy legislation. Once you're certain you have the player's consent, call the StartDataCollection() method to enable the SDK and start personal data collection. If the player does not give consent, no action is required as the SDK remains dormant until explicitly given the StartDataCollection() signal.

Note: For more information about proper compliance with data privacy legislation, see our privacy overview. You should also consult your own legal team.

The new initialization flow should look like this:

You can call the StartDataCollection() method at any time after services initialization to begin data collection. You don't need to call it immediately at app start-up.

Warning: Events recorded while the SDK is dormant are ignored. They are not buffered for sending if StartDataCollection() is called later. Events are only recorded once you have called StartDataCollection().

Note: Start-up events such as gameStarted and clientDevice are now recorded when the StartDataCollection() method is called, not before. If data collection is started, then stopped and started again within a single session, they are recorded only once on the first StartDataCollection().

Under the legacy flow, the OptOut() method disables the SDK, removes it from memory and triggers a request for data deletion for the player. This makes it difficult for a player to change their mind and opt back in to data collection at a later date, because the SDK retains knowledge of its 'consent revoked' status and prevents further initialization. Requesting data deletion at this time is also unnecessary, as a player opting out of further personal data collection doesn't necessarily mean they also want their previously collected data to be purged.

When you call the new StopDataCollection() method, this only disables the SDK, leaving it in memory for later reactivation if needed. The SDK attempts to make a final upload of any events that have been recorded up to that point and then discards any further events.

If the player also wishes to request their data be purged from the back-end, you must call the RequestDataDeletion() method separately.

Note: The data deletion mechanism continues to attempt to send the request until it's successful, even across sessions if necessary. The Analytics SDK remembers this status using the PlayerPrefs API, so calling PlayerPrefs.DeleteAll() might disrupt the process.

If a player opts out without requesting data deletion, you can still use the RequestDataDeletion() method to request their data be purged later. You can call this at any time, regardless of whether the SDK is active or dormant.

**Examples:**

Example 1 (unknown):
```unknown
StartDataCollection()
```

Example 2 (unknown):
```unknown
UnityServices.InitializeAsync()
```

Example 3 (unknown):
```unknown
StartDataCollection()
```

Example 4 (unknown):
```unknown
StopDataCollecton()
```

---

## Messaging

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/messaging/messaging-toc

**Contents:**
- Messaging#
- Related pages#

Vivox provides channel-based and directed text chat.

Additional chat features can be enabled:

The server can customize a maximum byte count for text messages in its pre-login configuration. By default, the maximum is 320 bytes with UTF-8 encoding. A text message that exceeds the maximum length in bytes is rejected with a VxErrorMessageTextTooLong error.

The following table details text chat policy:

1000 bytes per second

The maximum rate at which a player can send data over text chat.

This is configured server-side by Vivox.

---

## Join a channel

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/channels/join-channel

**Contents:**
- Join a channel#

To join a channel, use the IChannelSession::BeginConnect() method on an IChannelSession that you create by using a game-selected identifier for the channel and a channel type.

Note: Before joining a Vivox voice channel on desktop or mobile, developers must check the permissions for the capture device at runtime.

Joining a channel requires an access token.For more information, refer to the Access Token Developer Guide.

When joining a channel, the game can choose whether to join with audio capability, which enables the player to participate in group audio, and whether to join text capability, which enables the player to participate in group text. This is set with the connectAudio and connectText arguments to BeginConnect(), respectively. You can add or drop these capabilities without having to leave or rejoin the channel by calling IChannelSession::BeginSetAudioConnected() or IChannelSession::BeginSetTextConnected().

You can also set the argument switchTransmission to true to automatically switch audio transmission into only the new channel when connected. This overrides and changes the TransmissionMode that is set in ILoginSession. You can manage channel transmission at any time by using ILoginSession::SetTransmissionMode().

Note: A user can join a maximum of 10 non-positional channels at a time. Attempts to join a channel beyond that limit fail with a VxXmppServerErrorServiceUnavailable (20502) error.

Note: Channels have a maximum occupancy of 200 participants. Attempts to join a channel that has reached 200 participants will fail with a VxXmppServerErrorServiceUnavailable (20502) error.

**Examples:**

Example 1 (unknown):
```unknown
connectAudio
```

Example 2 (unknown):
```unknown
connectText
```

Example 3 (unknown):
```unknown
BeginConnect()
```

Example 4 (unknown):
```unknown
switchTransmission
```

---

## iOS known limitations

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/ios/ios-known-limitations

**Contents:**
- iOS known limitations#

The following sections detail the known limitations of the iOS platform. These limitations are not specific to the Vivox SDK implementation and are documented here for reference purposes. Also, note that this list is not exhaustive.

For more information, refer to the Apple developer documentation.

---

## Advanced build configuration

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/advanced-build-configuration/overview

**Contents:**
- Advanced build configuration#

The Advanced settings tab displays the build target’s Advanced settings.

You set advanced options per build target. For example, if you select the Advanced settings link for an iOS target, those options are only for that iOS target. Or, when you select Advanced settings for an Android target, those options are only for that Android target. You can use different pre methods and post methods per platform, and per build target.

The following describes the Advanced settings:

---

## Automatic connection recovery

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/channels/connection-recovery

**Contents:**
- Automatic connection recovery#

Note: Automatic connection recovery is supported on Android, iOS, macOS, and Windows.

An application might temporarily lose internet connectivity when its user moves between internet connection points. For example, a disconnection could occur when a user is roaming between cellular networks, or when a device switches between an LTE mobile data connection and a home wireless network connection.

The Vivox SDK provides network connection recovery functionality for connection loss of up to 30 seconds. When connection recovery is enabled for an application, the application can automatically reconnect to a session after a network change in scenarios where the Vivox SDK retains CPU continuity.

If a session resume fails after the application attempts to reconnect to the network, a login_state_logged_out event fires.

**Examples:**

Example 1 (unknown):
```unknown
login_state_logged_out
```

---

## iOS known limitations

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/ios/ios-known-limitations

**Contents:**
- iOS known limitations#

The following sections detail the known limitations of the iOS platform. These limitations are not specific to the Vivox SDK implementation and are documented here for reference purposes. Also, note that this list is not exhaustive.

For more information, refer to the Apple developer documentation.

---

## Data transfer objects (DTOs)

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/initialize-modules/dto

**Contents:**
- Data transfer objects (DTOs)#
- Prerequisites#
- Manage DTOs#
  - Create and configure your DTO project#
  - Add the DTOs to your module#
  - Use DTOs in your module logic#
  - Extract the DLLs#
  - Import the DLLs to Unity project#
  - Reuse the DTOs in a Unity MonoBehaviour script#

You can define data transfer objects in your modules. You can use DTOs to transfer data between the client and the server.

For example, you can use DTOs to serialize your module data to JSON, and deserialize it into the same structure on the client side.

Important: You can generate bindings from the Unity Editor to call the module endpoints using type-safe client code. For most use cases, you don't need to manually create DTOs in your Unity project. For more information, refer to Using editor bindings.

Before you get started with DTOs, create a Cloud Code module.

You can define DTOs to capture data types for your module endpoint functions.

To get started, create a C# project to store your DTOs in, and add a reference to it from your main project. Refer to the Microsoft documentation on how to Manage references in a project.

For more information on modules, refer to module structure.

Next, you have to configure your DTO C# project to use a supported runtime .NET version in Unity Editor, which is netstandard.

Open the <project_name>.csproj file, and change the TargetFramework. If you use Unity 2021.2.0, this is netstandard.2.1.

For more information, refer to the Unity manual supported .NET versions.

You also need to disable implicit usings. Refer to the example C# project configuration below:

To add DTOs to your module, you need to define a new class in your project to store your DTOs. This can be similar to the following example:

In your main project with module functions, define your game logic, and use the defined DTO as the function return type.

The following is an example of a simple module endpoint:

Note: You can also use DTOs for input as function parameters.

To use the DTOs in your Unity project to match the response type, you need to extract the DLLs from the module C# project.

You should deploy your module for this step to be able to call the module function later.

For manual packaging, refer to package code for more information on how to generate the assemblies.

If you generate the assemblies when you deploy your module, by default, you can find the DLLs in the bin/Debug/Release/net6.0/linux-x64/publish folder of your module project.

Your assemblies be similar to the following example:

Copy the DTOs.dll file.

To use an external DLL in your game, place the DLL inside your Unity project in the Assets directory.

The next time the Unity Editor synchronizes the project, the Editor adds the necessary references to the DLLs.

For more information, refer to Managed plug-ins in the Unity manual.

You can call out to Cloud Code SDK and use the same DTOs to deserialize the response:

Use the following example of a successful response for reference:

For more information, refer to running modules from Unity Runtime.

**Examples:**

Example 1 (unknown):
```unknown
netstandard
```

Example 2 (unknown):
```unknown
<project_name>.csproj
```

Example 3 (unknown):
```unknown
TargetFramework
```

Example 4 (unknown):
```unknown
netstandard.2.1
```

---

## Unity Build Automation lightmap baking

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/check-build-results/troubleshoot-build-failures/lightmap-baking

**Contents:**
- Unity Build Automation lightmap baking#
- Symptoms#
- Causes#
- Resolution#

Unity Build Automation doesn't support building lightmaps, and if you try to build a lightmap the builds might be too dark and have poor build performance. There are two possible causes:

Turn off Auto Build in your scenes and manually build and commit your lightmaps. Depending on what your scene includes and your lighting settings, you need to commit the following files:

The files are in a subfolder named after the scene in the same folder as your scene. For more information about lightmaps please refer to this documentation on how to set up your scene and lights for baking.

**Examples:**

Example 1 (unknown):
```unknown
LightingData.asset
```

Example 2 (unknown):
```unknown
LightmapSnapshot.asset
```

Example 3 (unknown):
```unknown
Lightmap-xxx.exr
```

Example 4 (unknown):
```unknown
Lightprobe-xxx.exr
```

---

## User settings

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/integration-guide/integration-plan/phase-one/ui-integration/user-settings

**Contents:**
- User settings#

You will want to expose some settings to users so they can manage their in-game communication experience. The following are generally recommended user settings:

---

## Text-to-speech voice options

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/text-to-speech/tts-voice-options

**Contents:**
- Text-to-speech voice options#

Before you inject a text-to-speech (TTS) message, you can choose the voice to use for synthesizing speech. Two voices are available: male and female.

To list all available voices, use vx_tts_get_voices, which is displayed in the following example:

vx_tts_voice_t is a struct that contains the name of the voice and its ID.

vx_tts_get_voices returns a pointer to an array of vx_tts_voice_t structs of length numVoices.

You can choose a voice ID to save for global use, or you can allow a user to choose their own voice ID.

**Examples:**

Example 1 (unknown):
```unknown
vx_tts_get_voices
```

Example 2 (unknown):
```unknown
int numVoices;
vx_tts_voice_t *voices;
vx_tts_status status = vx_tts_get_voices(managerId, &numVoices, &voices);
```

Example 3 (unknown):
```unknown
int numVoices;
vx_tts_voice_t *voices;
vx_tts_status status = vx_tts_get_voices(managerId, &numVoices, &voices);
```

Example 4 (unknown):
```unknown
vx_tts_voice_t
```

---

## Package code

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/package-code

**Contents:**
- Package code#
- Ready to Run (R2R) compilation#
- Producing assemblies#
  - Using the .NET command-line interface#
  - Using Microsoft Visual Studio#
  - Using JetBrains Rider#
- Packaging the assemblies#

To deploy a module, you must package the code into an archive. The archive can be a .ccm file or a .zip file, depending on the authoring method you use to deploy your module. The .ccm file is simply a ZIP file with its extension changed to .ccm.

The archive can only contain the packaged assemblies to run on x64 Linux.

Refer to Cloud Code Limits to check the storage limits.

Important: This is a more advanced workflow. For most use cases, you can use the Unity Editor or the UGS CLI to deploy your modules with automatic packaging, and skip this step.

You can enable Ready to Run compilation when producing assembles for your module project. This can slightly improve the cold start time for your module, but it won't improve the response time after the first call. The option also increases the size of the assemblies. Check Microsoft documentation on ReadyToRun Compilation for further details.

To produce assemblies in this format, use the .NET command-line interface (CLI) or an IDE such as JetBrains Rider or Microsoft Visual Studio as described in the sections below. You must produce assemblies only for the main module project that contains your Cloud Code module endpoints.

Note: Using .NET 8.0 is recommended.

To produce assemblies with the .NET CLI, use the dotnet publish command. Refer to Microsoft documentation on dotnet publish for further details.

You can specify the target platform and R2R compilation in the project by adding the <PublishReadyToRun> setting.

Publish the module using the following command:

By default, this produces assembles in the bin/Release/net8.0/linux-x64/publish folder.

You can produce assemblies with Microsoft Visual Studio. To do so, follow these steps:

Check the tutorial Publish a .NET console application using Visual Studio from the Microsoft documentation for further detail.

You can produce assemblies with JetBrains Rider. To do so, follow these steps:

Check JetBrains Rider documentation Publish .NET applications to folder for further details.

After producing these assemblies, compress them in an archive. If you plan to deploy using the UGS CLI, you can set the archive extension to .ccm.

Note: If you are using file explorer in Windows, you must enable common file name extensions to change the extension of the archive to .ccm.

The structure of the archive should look like this:

By default, this will include the dependencies of Cloud Code NuGet packages.

Note: The produced assemblies also contain .pdb files. These files are necessary to produce a stack trace with line numbers. However, they are not required to run your module.

You can then deploy the archive using the UGS CLI or the Cloud Code Admin API.

**Examples:**

Example 1 (unknown):
```unknown
dotnet publish
```

Example 2 (unknown):
```unknown
dotnet publish -c Release -r linux-x64 -p:PublishReadyToRun=true
```

Example 3 (unknown):
```unknown
dotnet publish -c Release -r linux-x64 -p:PublishReadyToRun=true
```

Example 4 (unknown):
```unknown
<PublishReadyToRun>
```

---

## AVAudioSessionModeVoiceChat

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/ios/avaudiosessionmodevoicechat

**Contents:**
- AVAudioSessionModeVoiceChat#

To enable platform acoustic echo cancellation (AEC), the iOS device must be in AVAudioSessionModeVoiceChat. For more information, see the Apple developer documentation.

AVAudioSessionModeVoiceChat changes EQ to optimize for voice. Because of this change, any non-voice audio that the iOS app plays might seem quieter. The Voice-Processing I/O unit also mixes the audio to ensure that the voice is the loudest part of the overall audio mix.

Note: Although it is possible to change the AVAudioSession mode to default after activating the Voice-Processing I/O unit, Apple recommends using AVAudioSessionModeVoiceChat. Using the default mode when the Voice-Processing I/O unit is active might cause unpredictable behavior, and various device types (for example, iPad and iPhone) might behave in different ways.

**Examples:**

Example 1 (unknown):
```unknown
AVAudioSessionModeVoiceChat
```

Example 2 (unknown):
```unknown
AVAudioSessionModeVoiceChat
```

---

## Automatic connection recovery

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/channels/new-connection-recovery

**Contents:**
- Automatic connection recovery#

An application might temporarily lose internet connectivity when its user moves between internet connection points. For example, a disconnection could occur when a user is roaming between cellular networks, or when a device switches between an LTE mobile data connection and a home wireless network connection.

The Vivox SDK provides network connection recovery functionality for connection loss of up to 30 seconds.

VivoxService.Instance.ConnectionRecovering and VivoxService.Instance.ConnectionRecovered are events that can be subscribed to in order to track when the connection is recognized as lost and recovery begins and when the connection is recovered.

A VivoxService.Instance.ConnectionFailedToRecover event will be fired if a session resume fails after the application attempts to reconnect to the network.

Important: If an additional system exists to reconnect users, you must allow the default Vivox system to finish attempting to recover before another system tries to recover the connection. If the Vivox attempts aren't completed it can result in a connection recovery loop.

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.ConnectionRecovering
```

Example 2 (unknown):
```unknown
VivoxService.Instance.ConnectionRecovered
```

Example 3 (unknown):
```unknown
VivoxService.Instance.ConnectionFailedToRecover
```

---

## Offline direct messages

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/chat-history/offline-direct-messages

**Contents:**
- Offline direct messages#

Note: This feature is currently only available in the Core SDK and Unity SDK v16.

When you send a message to an offline user you will receive a evt_account_send_message_failed event. This can be used to relay the fact that the user receiving said message is offline. The message gets archived and when the user that received that message signs in, they can retrieve their account chat history to see the message that was sent to them while offline.

**Examples:**

Example 1 (unknown):
```unknown
evt_account_send_message_failed
```

---

## Text-to-speech

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/text-to-speech/tts-overview

**Contents:**
- Text-to-speech#

Text-to-speech (TTS) is a feature in the Vivox SDK that is intended to support developers who are pursuing Communications and Video Accessibility Act (CVAA) compliance.

Note: TTS is only available in US English, and is only supported for one logged in user.

TTS provides a voice channel participant with the ability to convert text messages into synthesized speech that is then transmitted into the voice channel. This allows a user to participate in voice chat even when they might not be able to speak or speak clearly. TTS also provides a user with the ability to convert text chat or game-screen text into synthesized speech for local playback.

You can play a synthesized phrase locally through the user’s output audio device, transmit it and play it back for remote participants, or simultaneously play it locally and transmit it to remote participants.

Note: Unlike with the Vivox Core SDK, the engine wrappers do not require manual initialization of text-to-speech to start using it. Also, clean up is handled automatically.

---

## Manage builds

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/guides/manage-builds

**Contents:**
- Manage builds#
- Create a build#
- Update a build with a direct upload#
- Update a build with a container image#
- Update a build using an Amazon S3 bucket for a build#
- Update a build using a Google Cloud Storage bucket for a build#
- Perform a progressive rollout#
- Perform a forced rollout#
- Delete a build#
- Manage installation failures#

A build is a collection of files required to run your game or application. You can manage builds from the Unity Dashboard. Refer to the sections below to learn how to perform basic build tasks such as creating and updating a build.

Note: You can link a build to a fleet through a build configuration that points to the build.

Refer to Create a build.

Warning: Builds created with the direct file upload process doesn't support symbolic links. If you want to use symbolic links, you must use a Container build.

The following instructions explain how to update an existing build with direct upload from the Unity Dashboard.

Refer to Create a build with a container image for instructions on how to update a build with a container image. In this case you would skip the Details step and in most cases the Service account step.

Refer to Use an Amazon S3 bucket for a build for instructions on how to update a build using an Amazon S3 bucket. In this case you would you skip the Details step.

Refer to Use a Google Cloud Storage bucket for a build for instructions on how to update a build using a Google Cloud Storage bucket. In this case you would you skip the Details step.

A progressive rollout is a method of deploying a build update where you update servers only when they're empty. Multiplay Hosting waits for your matchmaker to deallocate the server if any players are connected to a server.

To perform a progressive rollout:

A forced rollout is a method of deploying a build update where you force servers to update even if players are connected. If players are connected when you start a forced rollout, Multiplay Hosting kicks the players from the server.

To perform a forced rollout:

Note: You can't delete a build if it's assigned to a build configuration. Make sure to remove the build from any build configurations before continuing.

You can also delete multiple builds at a time by selecting more than one build, then selecting Delete builds.

Similar to deleting a single build, you must confirm that you want to delete the builds. You’ll receive an alert if Multiplay Hosting can’t delete any of the selected builds.

Build installation failures sometimes happen. You can view the failed installation jobs by selecting View failed installs in the Installs view.

Refer to Troubleshooting to learn how to fix common problems. Contact support if installs continue to fail.

---

## Use environment variables

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/advanced-build-configuration/environment-variables

**Contents:**
- Use environment variables#
- Common built-in environment variables#
- Set environment variables#
- Use environment variables#
  - Access variables in Unity C# scripts#
  - Access variables in shell scripts#
  - Missing environment variables#

Use built-in environment variables and define custom environment variables in Unity Build Automation (UBA):

UBA provides built-in environment variables that are automatically available during builds. These variables offer useful details about the build environment, target settings, and version control information. For example, the following are commonly used built-in environment variables:

For a complete list of available environment variables, refer to the environment variables reference documentation.

Configure environment variables at the Build Target level. This allows you to specify different variables for each build target so you can use tailored configurations for various platforms or environments, such as staging or production.

Define environment variables in UBA:

After you define your environment variables, you can reference them during the build process within Unity scripts or custom pre- and post-build shell scripts.

Note: Ensure the variable keys match exactly because they're case-sensitive.

Use the System.Environment class to retrieve environment variables:

If you use pre-build or post-build shell scripts, you can use the dollar sign prefix to access specific environment variables:

If you can't access your environment variables, check the following solutions:

**Examples:**

Example 1 (unknown):
```unknown
UNITY_VERSION
```

Example 2 (unknown):
```unknown
BUILD_TARGET
```

Example 3 (unknown):
```unknown
PROJECT_PATH
```

Example 4 (unknown):
```unknown
BUILD_NUMBER
```

---

## Generate a token on the client

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/access-token-guide/generate-tokens-unreal

**Contents:**
- Generate a token on the client#

Important: Use the following access token generation methods during initial development and prototyping. Do not include this method of generation in production builds of your game. Allowing token generation on the client during production is both a security risk and can cause unexpected token expiration errors for your users.

The following APIs are available to generate insecure access tokens from the client for use during early development:

Each API takes the following parameters:

The following examples display access tokens for login and join, respectively:

**Examples:**

Example 1 (javascript):
```javascript
const TCHAR *kKey = TEXT("demo-key");
const FTimespan kExpiration = FTimespan::FromSeconds(90);

// Login (This assumes an already initialized Client object)
ILoginSession &loginSession(client.GetLoginSession(/* Account to login */));
loginSession.GetLoginToken(kKey, kExpiration);

// Join (This assumes an already signed in ILoginSession object)
IChannelSession &channelSession(loginSession.GetChannelSession(/* ChannelId to join*/));
channelSession.GetConnectToken(kKey, kExpiration);

// Mute (This assumes an already signed in ILoginSession object joined to a Connected IChannelSession)
IChannelSession &channelSession(loginSession.GetChannelSession(/* ChannelId to mute in*/));
IParticipant &participant(channelSession.Participants()[ParticipantKey]);
participant.GetMuteForAllToken(kKey, kExpiration);

// Transcription (This assumes an already signed in ILoginSession object joined to a Connected IChannelSession)
IChannelSession &channelSession(loginSession.GetChannelSession(/* ChannelId to begin transcribing*/));
channelSession.GetTranscriptionToken(kKey, kExpiration);
```

Example 2 (javascript):
```javascript
const TCHAR *kKey = TEXT("demo-key");
const FTimespan kExpiration = FTimespan::FromSeconds(90);

// Login (This assumes an already initialized Client object)
ILoginSession &loginSession(client.GetLoginSession(/* Account to login */));
loginSession.GetLoginToken(kKey, kExpiration);

// Join (This assumes an already signed in ILoginSession object)
IChannelSession &channelSession(loginSession.GetChannelSession(/* ChannelId to join*/));
channelSession.GetConnectToken(kKey, kExpiration);

// Mute (This assumes an already signed in ILoginSession object joined to a Connected IChannelSession)
IChannelSession &channelSession(loginSession.GetChannelSession(/* ChannelId to mute in*/));
IParticipant &participant(channelSession.Participants()[ParticipantKey]);
participant.GetMuteForAllToken(kKey, kExpiration);

// Transcription (This assumes an already signed in ILoginSession object joined to a Connected IChannelSession)
IChannelSession &channelSession(loginSession.GetChannelSession(/* ChannelId to begin transcribing*/));
channelSession.GetTranscriptionToken(kKey, kExpiration);
```

---

## V1 to V2 Migration Guide

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/server-side-recording/v1-to-v2-guide

**Contents:**
- V1 to V2 Migration Guide#
- Key changes#
- Authentication changes#
- API Changes#
  - Request changes#
- SSR Decoder#
- Callback server#

Follow this guide if you're upgrading from version 1 of the server-side recording API.

The V2 API introduces the following changes:

SSR now requires the use of Unity Authentication for all requests.

Once you have your project and organization you need to share the details with your account manager. Your account manager will enable SSR for your organization.

Once SSR is enabled you need to create a service account to authenticate with the API. Refer to How to create a service account.

On the service account page, assign permissions to the service account. Select Manage organization permissions and assign the Server Side Recording Processor Role. This enables this service account to access the SSR API.

To use this service account refer to How to authenticate with a service account.

The full API documentation for SSR v2 is available in the Web Services API Docs.

This guide only covers what to change from the V1 API to the V2 API and does not include any new features. Refer to the full API documentation for an overview of new fields and options.

The V2 API removes query parameters for passing request fields. All fields are now passed in the request body as JSON. The following is a list of fields that no longer exist.

destination_credentials are now contained within storageOptions. To use AWS in the same way as the V1 API you need to replace a specific code segment.

{"destination_credentials": {"bucket": "my-bucket","access_key_id": "secret","secret_access_key": "secret"}}

{"storageOptions": {"provider": "aws","credentials": {"bucket": "my-bucket","accessKeyId": "secret","secretAccessKey": "secret"}}}

Remove the target_type field. The V2 API only supports channel subscriptions.

The ttl field has been removed. Use the history field to specify the duration of the recording.

Note: A new optional field callbackUri has been added. This field allows you to specify a callback server to receive a status of your job to a server you host. You can specify any URL in this field and Vivox make a request to it. Refer to the callback server documentation for more information.

The SSR decoder is no longer a separate request. The final product of the request will be one .WAV file per speaker in the channel and you can remove the tracking of job state from your infrastructure.

With the removal of the status_complete and status_decoded, a mechanism was added to notify you when a job is complete. This is done through a callback server. Refer to the callback server documentation for more information.

This feature is completely optional, but it is the only way to know when a job is complete or why your audio is missing.

**Examples:**

Example 1 (unknown):
```unknown
Server Side Recording Processor Role
```

Example 2 (unknown):
```unknown
destination_credentials
```

Example 3 (unknown):
```unknown
storageOptions
```

Example 4 (unknown):
```unknown
target_type
```

---

## Update the ProGuard rule file

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/android/build-android-app-proguard

**Contents:**
- Update the ProGuard rule file#

If you are building your Android application with ProGuard, then you must add the following code to the ProGuard rule file to prevent classes from being stripped out of the build:

For more information on how to shrink your app, refer to the Android developer documentation.

**Examples:**

Example 1 (unknown):
```unknown
public class com.vivox.sdk.jni.androidsdkJNI
```

Example 2 (unknown):
```unknown
public class com.vivox.sdk.jni.androidsdkJNI
```

---

## Transmission

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/transmission/transmission

**Contents:**
- Transmission#

Setting the TransmissionMode of the Vivox.Service.Instance controls the channels that microphone audio and injected audio are broadcast into. TransmissionMode is most important to consider for users who will connect to more than one channel at a time.

Transmission settings only control where audio for a specific user goes, and do not affect rendered audio spoken by other users. By default, users do transmit audio into any connected channel; however, you can configure user audio to not automatically transmit into a new channel when it is joined by setting the DisableAutomaticChannelTransmissionSwap field of LoginOptions to true when running VivoxService.Instance.LoginAsync(LoginOptions options = null).

The transmission policy determines whether voice is heard by other channel participants. However, note that many factors can affect whether other channel participants actually hear a user speaking, such as for example, if other users have locally muted the user, if the user has been moderator muted, if the user has globally muted their microphone, or if the user has joined the channel for text only.

Note: If you intend for a user to join all channels as text only, or do not plan for a user to join any channels at all, for example, if you plan to use Vivox only for online status and direct messaging, then no transmission APIs need to be called or considered.

**Examples:**

Example 1 (unknown):
```unknown
DisableAutomaticChannelTransmissionSwap
```

Example 2 (unknown):
```unknown
VivoxService.Instance.LoginAsync(LoginOptions options = null)
```

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/channels/positional-channel-configuration

---

## Volume-for-me control

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/integration-guide/integration-plan/phase-one/ui-integration/volume-for-me-control

**Contents:**
- Volume-for-me control#

Vivox allows users to adjust the volume of how other players personally sound to them. Users can also mute a specific player to never hear them again.

In the following image example, you can use the volume slider or the mute button to set the audio level for the selected user.

---

## Message callback

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/messaging/message-callback

**Contents:**
- Message callback#

When you initialize the Vivox SDK, you need to set up your implementation to handle SDK messages. Using a callback method is the suggested best practice. This simplifies your game logic by only responding to messages when they occur, rather than using a polling loop that might be idle most of the time.

The callback is a parameter for vx_sdk_config_t, which is shown in the following VxcTypes.h example:

The following example is built on code from the SDKSampleApp with extra details on using OnSdkMessageAvailable on a ListenerThread:

Note: The Vivox SDKSampleApp shows the use of a mutex while processing messages (the HandleMessage() function). This is a requirement of the SDKSampleApp, not the Vivox SDK. A mutex is only needed if your implementation requires it by design.

Note: Messages can be processed on the same thread as the callback or on a different thread.

**Examples:**

Example 1 (unknown):
```unknown
vx_sdk_config_t
```

Example 2 (unknown):
```unknown
// When the SDK message callback is called, call vx_get_message() until there are no more messages.
    void (*pf_sdk_message_callback)(void *callback_handle);
```

Example 3 (unknown):
```unknown
// When the SDK message callback is called, call vx_get_message() until there are no more messages.
    void (*pf_sdk_message_callback)(void *callback_handle);
```

Example 4 (javascript):
```javascript
static vx_sdk_config_t MakeConfig(const SDKSampleApp &app, unsigned int default_codecs_mask, vx_log_level log_level = log_error)
{
    vx_sdk_config_t config;
    vx_get_default_config3(&config, sizeof(config));

    config.pf_logging_callback = OnLog;
    config.pf_sdk_message_callback = OnSdkMessageAvailable;
}

void OnSdkMessageAvailable(void *callbackHandle)
{
    SDKSampleApp *app = reinterpret_cast<SDKSampleApp *>(callbackHandle);
    app->set_event(m_messageAvailableEvent);
}

void SDKSampleApp::ListenerThread()
{
    for (; m_started;) 
    
    // Pulls a message from the queue. If no message found, waits until notified, then tries again.
    {
        wait_event(m_messageAvailableEvent, -1);
        for (;;) 
        {
            vx_message_base_t *msg = NULL;
            vx_get_message(&msg);
           
    // Type of message, and then type of the Event or Response is discovered, and the
    // appropriate call is made to process that message type.
           
           if (msg == NULL) 
            {
                break;
            }
            Lock();
            HandleMessage(msg);
            Unlock();
            vx_destroy_message(msg);
        }
    }
    set_event(m_listenerThreadTerminatedEvent);
}
```

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/ios/cannot-set-ios-device-volume-to-zero

---

## Directed text

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/messaging/directed-text-voice

**Contents:**
- Directed text#

Vivox provides directed text by using a similar process to what is used for group text. You can add players to existing text sessions, and you can send text messages directly to any user who is signed in, independent of being in a channel with them.

Use directed text for short user-to-user messages (sometimes called whispers or private/direct messages), or to implement invite to channel capabilities.

To send a text message to another user, use the ILoginSession::BeginSendDirectedMessage method.

The following code displays an example of this process:

The ILoginSession::EventDirectedTextMessageReceived event is sent to the game when a user-to-user text message is available.

The following code is an example of this action:

**Examples:**

Example 1 (unknown):
```unknown
ILoginSession::BeginSendDirectedMessage
```

Example 2 (unknown):
```unknown
/* . . . */
AccountId Target = AccountId(kDefaultIssuer, "example_target", kDefaultDomain);
FString Message = TEXT("Example private message.");
ILoginSession::FOnBeginSendDirectedMessageCompletedDelegate SendDirectedMessageCallback;
SendDirectedMessageCallback.BindLambda([this, Target, Message](VivoxCoreError Error)
{
    if(VxErrorSuccess == Error)
    {
        UE_LOG(MyLog, Log, TEXT("Message sent to %s: %s\n"), *Target.Name(), *Message);
    }
});
MyLoginSession.BeginSendDirectedMessage(Target, Message, SendDirectedMessageCallback);
/* . . . */
```

Example 3 (unknown):
```unknown
/* . . . */
AccountId Target = AccountId(kDefaultIssuer, "example_target", kDefaultDomain);
FString Message = TEXT("Example private message.");
ILoginSession::FOnBeginSendDirectedMessageCompletedDelegate SendDirectedMessageCallback;
SendDirectedMessageCallback.BindLambda([this, Target, Message](VivoxCoreError Error)
{
    if(VxErrorSuccess == Error)
    {
        UE_LOG(MyLog, Log, TEXT("Message sent to %s: %s\n"), *Target.Name(), *Message);
    }
});
MyLoginSession.BeginSendDirectedMessage(Target, Message, SendDirectedMessageCallback);
/* . . . */
```

Example 4 (unknown):
```unknown
ILoginSession::EventDirectedTextMessageReceived
```

---

## Edit and Delete

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/chat-history/edit-delete/edit-delete-messages

**Contents:**
- Edit and Delete#
- Client Interface#

Note: This feature is currently only available in the Core SDK and Unity SDK v16.

With the Edit and Delete functionality you can provide users with the ability to edit or delete previously sent text messages.

The Edit and Delete features have interfaces exposed as listed below:

---

## Text-to-speech voice options

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/text-to-speech/tts-voice-options

**Contents:**
- Text-to-speech voice options#

Before you inject a text-to-speech (TTS) message or read text locally, you can choose the voice to use for synthesizing speech. You can set this option per user at any time.

Two voices are available: male and female. You can list all available voices by using the following script:

When you call a TTS method that calls for speech to be synthesized, the system uses the ITTSVoice that is set as the current voice for that user. If no voice is selected, the system uses the Vivox SDK default.

**Examples:**

Example 1 (unknown):
```unknown
foreach (string voiceName in VivoxService.Instance.TextToSpeechAvailableVoices)
{
    Console.WriteLine($"Available Voice: Name=[{voiceName}]");
}
// Available Voice: Name=[en_US male]
// Available Voice: Name=[en_US female]

Console.WriteLine($"Current Voice: {VivoxService.Instance.TextToSpeechCurrentVoice()}");
// Current Voice: en_US female
```

Example 2 (unknown):
```unknown
foreach (string voiceName in VivoxService.Instance.TextToSpeechAvailableVoices)
{
    Console.WriteLine($"Available Voice: Name=[{voiceName}]");
}
// Available Voice: Name=[en_US male]
// Available Voice: Name=[en_US female]

Console.WriteLine($"Current Voice: {VivoxService.Instance.TextToSpeechCurrentVoice()}");
// Current Voice: en_US female
```

---

## Queue a text-to-speech message

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/text-to-speech/queue-tts-message

**Contents:**
- Queue a text-to-speech message#

Some destinations offer a built-in queuing system. When a new message is injected and there is an ongoing message playing, then the new message is put into a queue. After the ongoing message finishes playing, the next message in the queue automatically begins playing.

A queue is shared between TTSDestination::QueuedRemoteTransmission and TTSDestination::QueuedRemoteTransmissionWithLocalPlayback. The destination TTSDestination::QueuedLocalPlayback has its own queue.

A destination queue can hold up to 10 messages. If a message is added when a destination queue is full, the Vivox SDK discards the message and the method returns the error VxErrorTTSDestinationQueueIsFull. In this case, no text-to-speech related events are raised.

**Examples:**

Example 1 (unknown):
```unknown
// Returns VxErrorSuccess and starts playing immediately.
MyLoginSession->TTS().Speak("First Message.", TTSDestination::QueuedLocalPlayback);

// Returns VxStatusTTSInputTextWasEnqueued and put in a queue.
// After the first message finishes playback, this message will start.
MyLoginSession->TTS().Speak("Second Message.", TTSDestination::QueuedLocalPlayback);
```

Example 2 (unknown):
```unknown
// Returns VxErrorSuccess and starts playing immediately.
MyLoginSession->TTS().Speak("First Message.", TTSDestination::QueuedLocalPlayback);

// Returns VxStatusTTSInputTextWasEnqueued and put in a queue.
// After the first message finishes playback, this message will start.
MyLoginSession->TTS().Speak("Second Message.", TTSDestination::QueuedLocalPlayback);
```

Example 3 (unknown):
```unknown
TTSDestination::QueuedRemoteTransmission
```

Example 4 (unknown):
```unknown
TTSDestination::QueuedRemoteTransmissionWithLocalPlayback
```

---

## Example: Mute_All token

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/access-token-guide/access-token-examples/example-mute-all-token

**Contents:**
- Example: Mute_All token#
- Signed in user muting all users in a channel#
- Admin user muting all users in a channel#
- Signed in user muting all but one user in a channel (Presentation mode)#
- Admin user muting all but one user in a channel (Presentation mode)#

Note: The token format for unmuting all users in a channel is the same as a mute_all token.

The token in this example allows the user "beef" to mute all users in the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

The token in this example allows the user "blindmelon-AppName-dev-Admin" to mute all users in the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

The token in this example allows the user "beef" to mute all users except for the user "jerky" in the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

In this scenario, when creating the mute token, a subject is specified. The subject in this instance ("jerky") is the only user in the channel that is not muted, which allows them to speak to the other, now muted, users in the channel without interruption.

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

The token in this example allows the user "blindmelon-AppName-dev-Admin" to mute all users except for the user "jerky" in the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

In this scenario, when creating the mute token, a subject is specified. The subject in this instance ("jerky") is the only user in the channel that is not muted, which allows them to speak to the other, now muted, users in the channel without interruption.

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

**Examples:**

Example 1 (unknown):
```unknown
{
    "vxi":19283,
    "f":"sip:.blindmelon-AppName-dev.beef.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"mute",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 2 (unknown):
```unknown
{
    "vxi":19283,
    "f":"sip:.blindmelon-AppName-dev.beef.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"mute",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 3 (unknown):
```unknown
e30.eyJ2eGkiOjE5MjgzLCJmIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2LmJlZWYuQHRsYS52aXZveC5jb20iLCJpc3MiOiJibGluZG1lbG9uLUFwcE5hbWUtZGV2IiwidnhhIjoibXV0ZSIsInQiOiJzaXA6Y29uZmN0bC1nLWJsaW5kbWVsb24tQXBwTmFtZS1kZXYudGVzdGNoYW5uZWxAdGxhLnZpdm94LmNvbSIsImV4cCI6MTYwMDM0OTQwMH0.fARLW2eX10ZbiIl_5WIg4bhPbYIhn2xfCcUNySfwBMs
```

Example 4 (unknown):
```unknown
e30.eyJ2eGkiOjE5MjgzLCJmIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2LmJlZWYuQHRsYS52aXZveC5jb20iLCJpc3MiOiJibGluZG1lbG9uLUFwcE5hbWUtZGV2IiwidnhhIjoibXV0ZSIsInQiOiJzaXA6Y29uZmN0bC1nLWJsaW5kbWVsb24tQXBwTmFtZS1kZXYudGVzdGNoYW5uZWxAdGxhLnZpdm94LmNvbSIsImV4cCI6MTYwMDM0OTQwMH0.fARLW2eX10ZbiIl_5WIg4bhPbYIhn2xfCcUNySfwBMs
```

---

## Implement Vivox Unreal

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/implement-vivox-unreal/implement-vivox-unreal

**Contents:**
- Implement Vivox Unreal#
- Related pages#

To implement the Vivox Unreal SDK, you need the following items, which you can find on the Unity Dashboard:

The URL for your Vivox API endpoint

The host for your Vivox domain

Your Vivox access token issuer and key pair.

You use these to sign Vivox access tokens on your server and distribute those tokens to your game clients.

Note: You can initially sign access tokens with your game client, but this is not a recommended production practice.

For more information, refer to the Access Token Developer Guide.

A Vivox Unreal SDK distribution for each of the platforms that will be supported by your game.

Each distribution contains the following resources:

These distributions come in the form of platform-specific archives (.zip or .tgz). Unzip the archive to a convenient location in your game source code working copy.

After you familiarize yourself with the items in this list, decide if you want to make the Vivox Core Unreal plug-in available to a specific project or to all of your Unreal projects.

**Examples:**

Example 1 (unknown):
```unknown
VivoxCore.h
```

---

## Methods for setting audio levels

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/audio/methods-for-setting-audio-levels

**Contents:**
- Methods for setting audio levels#

The Vivox SDK uses various methods for setting audio input levels. You can use these requests to set the render device and capture device level for a user, for specific users in a single session, for specific users in an entire session, or for every participant in every session.

vx_req_session_set_participant_volume_for_me

vx_req_aux_set_speaker_level

vx_req_aux_set_mic_level

vx_req_sessiongroup_set_focus

The following code displays an example implementation for setting both input and output audio levels:

**Examples:**

Example 1 (unknown):
```unknown
vx_req_session_set_participant_volume_for_me
```

Example 2 (unknown):
```unknown
vx_resp_session_set_participant_volume_for_me
```

Example 3 (unknown):
```unknown
vx_req_aux_set_speaker_level
```

Example 4 (unknown):
```unknown
vx_req_aux_set_mic_level
```

---

## iOS app development

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/ios/ios-toc

**Contents:**
- iOS app development#

---

## Authentication-based token generation

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/authentication-package-tokens

**Contents:**
- Authentication-based token generation#

Authentication-based token generation is the simplest way to handle token generation.

Token generation will be handled automatically when the Unity Authentication package is present in a project that the Vivox SDK is in and you sign into Authentication before calling VivoxService.Instance.Initialize.

The following code sample shows the setup:

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.Initialize
```

Example 2 (unknown):
```unknown
using Unity.Services.Authentication;

public class VoiceManager : MonoBehaviour
{
    async void Start()
    {
        await UnityServices.InitializeAsync();
        await AuthenticationService.Instance.SignInAnonymouslyAsync();
        await VivoxService.Instance.InitializeAsync();
    }
}
```

Example 3 (unknown):
```unknown
using Unity.Services.Authentication;

public class VoiceManager : MonoBehaviour
{
    async void Start()
    {
        await UnityServices.InitializeAsync();
        await AuthenticationService.Instance.SignInAnonymouslyAsync();
        await VivoxService.Instance.InitializeAsync();
    }
}
```

---

## Leave a channel

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/leave-channel

**Contents:**
- Leave a channel#

To remove a user from a channel, the game sends a vx_req_sessiongroup_remove_session message to the Vivox SDK.

The following code displays an example of how to perform this operation:

**Examples:**

Example 1 (unknown):
```unknown
vx_req_sessiongroup_remove_session
```

Example 2 (unknown):
```unknown
. . .
vx_req_sessiongroup_remove_session *req;
vx_req_sessiongroup_remove_session_create(&req);
req->sessiongroup_handle = vx_strdup("sg1");
req->session_handle = vx_strdup("mychannel");
vx_issue_request3(&req->base, &request_count);
. . .
```

Example 3 (unknown):
```unknown
. . .
vx_req_sessiongroup_remove_session *req;
vx_req_sessiongroup_remove_session_create(&req);
req->sessiongroup_handle = vx_strdup("sg1");
req->session_handle = vx_strdup("mychannel");
vx_issue_request3(&req->base, &request_count);
. . .
```

---

## Session groups

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/session-groups

**Contents:**
- Session groups#

Session groups are container objects that hold one or more sessions. A session group is local to an application and does not imply symmetrical association or "view" for other participants in individual channels.

If a client application needs to provide the ability for users to listen to multiple channels at the same time, then you can use a session group to add or remove channels (sessions).

Note: When you use the session create command, a session group is automatically created.

A session group enforces a common codec for all participants. There is only a single negotiated audio codec for a session group because it is a property of a SIP session.

Use session groups to support multi-channel mode. One example of multi-channel use is in a MMOG with a hierarchical organizational structure such as fleets, nested groups, or gangs. Another multi-channel example is using one channel to talk to others located "near" you in a virtual world, and using a second channel to bridge non-proximate participants from the different regions in that world.

You can also use a session group to manage the channels in a session group. For example, consider an example scenario where one channel might need to be the "focus" channel, and all other channels are in the background (which makes the focus channel louder and the other channels quieter). In this example, you would use a session group handle to change these aspects within the session group.

---

## Interface implementation-based token generation

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/access-token-guide/interface-implementation-tokens

**Contents:**
- Interface implementation-based token generation#
- To subscribe#
- Fetch the token#

If you want to control how Vivox Access Tokens (VATs) are generated, create a class that implements the Vivox SDK’s IVivoxTokenProvider interface and register it with the VivoxService.

When you register your implementation of IVivoxTokenProvider by calling VivoxService.Instance.SetTokenProvider, Vivox will call your implementation's IVivoxTokenProvider.GetTokenAsync method whenever a token is needed for actions such as logging in.

The information needed to create a payload is provided as input to the overridden IVivoxTokenProvider.GetTokenAsync method. This information is used when crafting a VAT.

You will still need to set up server-side token vending and fetch the token within the overridden IVivoxTokenProvider.GetTokenAsync method.

Before logging in your user, you must register your IVivoxTokenProvider implementation using:

Important: The subscribe flow changed in v16.5.0. For Unity Vivox SDK versions previous to 16.5.0, you must register your IVivoxTokenProvider implementation before initializing the Vivox SDK.

Create the payload with all of the parameters provided in the overridden method, even if some are empty, and send it to your secure server to generate your Vivox Access Token. Best practice is to send all the parameters; only what’s needed for the payload will be returned. Use the payload as input into the GetTokenAsync method.

For certain tokens, different parameters will be empty. For example, targetUserUri and channelUri will be empty for a login token. This is expected behaviour.

The following is an example of token generation:

For more details on this specific flow, refer to Sign in with Vivox Access Token

To learn more about generating server-side tokens, refer to Generate a token on a secure server.

**Examples:**

Example 1 (unknown):
```unknown
IVivoxTokenProvider
```

Example 2 (unknown):
```unknown
VivoxService.Instance.SetTokenProvider
```

Example 3 (unknown):
```unknown
IVivoxTokenProvider.GetTokenAsync
```

Example 4 (unknown):
```unknown
IVivoxTokenProvider.GetTokenAsync
```

---

## Channel types

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/channels/channel-types

**Contents:**
- Channel types#
- Echo channels#
- Group channels#
- Positional channels#

Vivox uses the following types of channels:

The channel type is indicated by the method used to initiate the channel join. JoinEchoChannelAsync joins an echo channel, JoinGroupChannelAsyc joins a group channel, and JoinPositionalChannelAsync joins a positional channel.

Note: When working with both positional and group channels, join the positional channel first, followed by the group channels.

Echo channels are joined by using the JoinEchoChannelAsync method, which is shown in the following example:

JoinEchoChannelAsync("TestChannel", ChatCapability.AudioOnly)

Developers can use these channels to give users a place to test their microphones or as a general way to test connectivity to the Vivox voice servers.

Note: Only use echo channels when disconnected from all other channels.

Group channels are joined by using the JoinGroupChannelAsync method, which is displayed in the following example:

JoinGroupChannelAsync("TestChannel", ChatCapability.TextAndAudio)

Developers can use these channels to allow for level-wide audio and text channels that players can connect to.

Examples of scenarios where non-positional channels are often used include teams and squads in first-person shooters and party chat in MMOs.

Non-positional channels are typically the most used channel types in a Vivox implementation.

Positional channels, also referred to as 3D channels, are joined by using the JoinPositionalChannelAsync method, which is displayed in the following example:

Developers can use these channels to provide voice chat that is part of a world, with player voices attenuating and panning to make it seem like they are speaking from where they are positioned in the game world. For this effect to work you must set a user's location. For instructions on how to set the location of players, refer to the Positional channel configuration page. You can parametrize positional channels with certain values that change how the player's positioning affects their voice.

Note: You can join only one positional channel at a time.

**Examples:**

Example 1 (unknown):
```unknown
JoinEchoChannelAsync
```

Example 2 (unknown):
```unknown
JoinGroupChannelAsyc
```

Example 3 (unknown):
```unknown
JoinPositionalChannelAsync
```

Example 4 (unknown):
```unknown
JoinEchoChannelAsync
```

---

## Echo channels

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/echo-channels

**Contents:**
- Echo channels#

Vivox provides an echo channel capability. This is often used to perform a quick check to ensure that voice is working properly in a particular environment.

To join an echo channel, use the channel that is prefixed with "confctl-e-".

Note: If multiple users are in the same echo channel, then they cannot hear themselves or other users.

To implement an echo channel, ensure that the game selects an echo channel URI that is unique. Do this by using the vx_get_echo_channel_uri() method with the user's ID as the channel name or by using vx_get_random_channel_uri().

Attention: You must initialize the Vivox SDK before you can use either of these methods.

**Examples:**

Example 1 (unknown):
```unknown
vx_get_echo_channel_uri()
```

Example 2 (unknown):
```unknown
vx_get_random_channel_uri()
```

---

## Text chat developer guide

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/text-chat-guide/text-chat-guide-overview

**Contents:**
- Text chat developer guide#

Vivox provides a text chat service:

The following additional features can be enabled:

The server can customize a maximum byte count for text messages in its pre-login configuration. By default, the maximum is 320 bytes with UTF-8 encoding. A text message that exceeds the maximum length in bytes is rejected with a VxErrorMessageTextTooLong error.

Text chat requires a Unicode-supported system to work properly. The Unity Editor has full Unicode support in versions 2017.1 and later.

**Examples:**

Example 1 (unknown):
```unknown
VxErrorMessageTextTooLong
```

---

## Integration requirements

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/concepts/integration-requirements

**Contents:**
- Integration requirements#
- General requirements#
  - Operating system#
  - Parameterized values#
  - Start-up conditions#
  - Network adapter binding#
  - Dynamic port number#
  - Logs directory#
  - Query protocol#
  - Post-allocation cleanup#

Your game must meet specific requirements to ensure that you can use the Multiplay Hosting service to host and scale your game. The following guidelines describe how to prepare your game for Multiplay Hosting.

Most of the requirements are about your build or, more specifically, your build executable file. Your build encapsulates all the files necessary to run your game. Your build executable file is the executable file within your build. You must specify the build executable file when you create a build.

Note: There are special requirements for using container builds. Refer to Container build requirements.

This section covers the general requirements your build must meet to use Multiplay Hosting. Refer to the following requirements to learn more:

Your build executable file must support running on Ubuntu Linux 22.04. If you want to run your build executable file on Ubuntu 24.04, contact your Unity representative or refer to Unity Support.

Note: Multiplay Hosting only supports 64-bit Linux applications.

Your build must support using information from parameterized values. Multiplay Hosting manages parameterized values through configuration variables, the server.json file, and launch parameters.

You can manage the information a build tracks through its build configuration.

Multiplay Hosting uses a server.json file to keep track of session-specific information, such as the allocation ID, port number, and any other variables you want to keep track of as configuration variables. Your build process must support keeping track of the information in the server.json file, so it can know when a server becomes allocated and deallocated. If you’re using Unity or the Unreal Engine, you can do so with the Game Server SDK.

Builds can also read information from launch parameters. Launch parameters are a good way to set values the build executable file needs, such as the game port, when it starts up.

Note: Although recommended, you don’t have to support launch parameters and build configuration variables. You can choose to support either option or a combination of both options.

Builds must support both starting with and without an allocation UUID populated in the server.json file. This is because Multiplay Hosting can create new servers to satisfy the scaling settings you provided. These servers are started in preparation for receiving allocations, which means the allocatedUUID field is an empty string ("") until it is allocated.

Builds must bind to 0.0.0.0 for both game server port and query protocol server port, if the build supports a query protocol.

Note: Trying to bind to any other address, including the externally visible IP address, might cause the server to be inaccessible or application bind failures.

Builds must support using a dynamic port number for game data transmission. Multiplay Hosting generates port numbers per server instance, with an offset to ensure there is no port conflict on server startup.

Note: You can use any ports in the range of 8100 to 65355.

Your build executable file must use the $$port$$ variable to leverage this functionality. For example, you can pass the $$port$$ variable to your build at start-up by using the following launch parameter:

Warning: Multiplay Hosting doesn't support using static or hard-coded port numbers. Static port numbers might work initially but can lead to problems with collisions later.

The logs directory requirement is optional. If you don’t set up a log directory, you won’t have any way of accessing logs and crash dumps through the Unity Dashboard. Multiplay Hosting displays server logs through the Unity Dashboard, where you can view, search, and download logs.

You can control where your build saves log files through a launch parameter and configuration variable. For example, you can use -logFile as the launch parameter and pass the $$log_dir$$ configuration variable.

The logs directory requirement is optional. If you don’t set up a log directory, you won’t have any way of accessing logs and crash dumps through the Unity Dashboard.

Tip: The recommended best practice is to set up a log rotation within the server code to ensure you have historical log data without continuously overwriting a single file. You use the $$timestamp$$ variable to set up automatic log rotation.

Note: If you’re using Unreal Engine, the log directory must be relative to the Saved directory. Games using the Unreal Engine also support -userdir as a launch parameter to re-route crashes and pattern files to a similar directory.

While it’s not a requirement, it’s best practice for builds to support responding to queries from a server query protocol, such as SQP.

A server query protocol is a protocol that facilitates querying information from a server instance, such as the server state and the number of allocated players. Typically, the response has static variables with dynamic (continuously updated) values.

Multiplay Hosting relies on query protocol responses to decide server instances' health. If your build doesn’t support a query protocol or if you set up your query protocol incorrectly, Multiplay Hosting might decide your server instances are unresponsive.

Additionally, the query protocol lets Multiplay Hosting report and watch real-time data about each server, such as concurrently connected users (CCU), active allocations, failed allocations, connected players per platform, and server events.

Tip: You can use the Game Server SDK to implement a server query protocol.

Builds must support some form of post-allocation (post-session) cleanup to prepare the game server for the next allocation.

You have a couple of options:

Returning to the lobby (or a ready state) is preferable to restarting the game server process. Ending the game server process at the end of a match is a less optimal use of resources because it’s typically more costly to restart than clean up a process.

Tip: Refer to the Game Server SDK to learn how to manage game server readiness.

When you create a Multiplay Hosting build, you have several options for uploading your build file.

One option is to use Docker and the Multiplay Hosting container registry to upload a containerized version of your build. Your build container must meet specific requirements (in addition to the General requirements) before you can use a container build.

The linux-build-image template container takes care of most of the additional requirements of using a container build. In most cases, use the base image. You can use it by adding the following line to the top of your Dockerfile:

Replace <tag> with the version of the base image to use. You can check the linux-base-image tags for a list of available tags.

If you prefer to create your Dockerfile from scratch, it’s your responsibility to ensure your container meets the following requirements.

Warning: Use the base image unless you have a specific or advanced uses in which you must create the container from scratch.

The following code block shows a simple Dockerfile that meets these requirements:

**Examples:**

Example 1 (unknown):
```unknown
server.json file
```

Example 2 (unknown):
```unknown
server.json
```

Example 3 (unknown):
```unknown
server.json
```

Example 4 (unknown):
```unknown
server.json
```

---

## Access token signature

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/access-token-format/access-token-signature

**Contents:**
- Access token signature#

The signature is the base64url-encoded HMAC of the first two parts:

base64UrlEncode(HMACSHA256(base64UrlEncode(header) + '.' + base64UrlEncode(payload), key))

The encoded header and encoded payload are joined with a period, undergoes HMAC with the token signing key, and then the entirety is encoded.

**Examples:**

Example 1 (unknown):
```unknown
base64UrlEncode(HMACSHA256(base64UrlEncode(header) + '.' + base64UrlEncode(payload), key))
```

---

## Parameter specifics

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/troubleshooting/vad-parameter-specifics

**Contents:**
- Parameter specifics#
- vad_hangover#
- vad_sensitivity#
- vad_noise_floor#

The hangover time is the time (in milliseconds) it takes for the VAD to switch back from speech mode to silence after the last speech frame is detected. The default value is 2000.

The sensitivity is a dimensionless value between 0 and 100 that indicates the sensitivity of the VAD. Increasing this value corresponds to decreasing the sensitivity of the VAD (0 is the most sensitive, and 100 is the least sensitive). Higher values of sensitivity require louder audio to trigger the VAD. The default value is 43.

Applications that use the default VAD and expose the vad_sensitivity as a slider should limit the possible settings between zero (transmit all mic activity) and 70 (very selective).

The noise floor is a dimensionless value between 0 and 20000 that controls how the VAD separates speech from background noise. Lower values assume the user is in a quieter environment where the audio is only speech. Higher values assume a noisy background environment. The default value is 576.

Note: Changes to the VAD noise floor settings do not affect currently joined channels. If the ability to change VAD settings is available to the end-user, indicate that noise floor changes only take effect in the next voice session or only allow changing the noise floor channel when the client is not in-channel.

**Examples:**

Example 1 (unknown):
```unknown
vad_hangover
```

Example 2 (unknown):
```unknown
vad_sensitivity
```

Example 3 (unknown):
```unknown
vad_sensitivity
```

Example 4 (unknown):
```unknown
vad_noise_floor
```

---

## Queue a text-to-speech message

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/text-to-speech/queue-tts-message

**Contents:**
- Queue a text-to-speech message#

Some destinations offer a built-in queuing system. When a new message is injected and there is an ongoing message playing, then the new message is put into a queue. After the ongoing message finishes playing, the next message in the queue automatically starts to play.

A queue is shared between TextToSpeechMessageType.QueuedRemoteTransmission and TextToSpeechMessageType.QueuedRemoteTransmissionWithLocalPlayback. The destination TextToSpeechMessageType.QueuedLocalPlayback has its own queue.

A destination queue can hold up to 10 enqueued messages in addition to the message that is actively playing. If a message is added when a destination queue is full, the Vivox SDK discards the message and raises an InvalidOperationException (VxErrorTTSDestinationQueueIsFull). In this case, no text-to-speech related events are raised.

**Examples:**

Example 1 (unknown):
```unknown
// Start playing speech as soon as it is synthesized.
VivoxService.Instance.TextToSpeechSendMessage("1st Message.", TextToSpeechMessageType.QueuedLocalPlayback);

// After the first message finishes playback, this message starts.
VivoxService.Instance.TextToSpeechSendMessage("2nd Message.", TextToSpeechMessageType.QueuedLocalPlayback);
```

Example 2 (unknown):
```unknown
// Start playing speech as soon as it is synthesized.
VivoxService.Instance.TextToSpeechSendMessage("1st Message.", TextToSpeechMessageType.QueuedLocalPlayback);

// After the first message finishes playback, this message starts.
VivoxService.Instance.TextToSpeechSendMessage("2nd Message.", TextToSpeechMessageType.QueuedLocalPlayback);
```

Example 3 (unknown):
```unknown
TextToSpeechMessageType.QueuedRemoteTransmission
```

Example 4 (unknown):
```unknown
TextToSpeechMessageType.QueuedRemoteTransmissionWithLocalPlayback
```

---

## Inject a text-to-speech message

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/text-to-speech/inject-tts-message

**Contents:**
- Inject a text-to-speech message#

To inject a text-to-speech (TTS) message into channel or local audio, use vx_tts_speak

vx_tts_speak accepts the following parameters:

The TTS manager identifier that is generated at TTS initialization.

The ID of the voice to be used when synthesizing the speech.

The message to be converted into speech.

An out parameter that contains the utterance identifier. You can use the utterance identifier later, for example, to cancel TTS messages.

Note: The word “utterance” is used to refer to the TTS message regardless of whether it is currently playing in the destination or is in a queue waiting to be synthesized.

If multiple TTS users are in one voice or text channel, then you can prefix the display name of the user who is sending the TTS message. For example, “[Display name] says…”

**Examples:**

Example 1 (unknown):
```unknown
vx_tts_speak
```

Example 2 (unknown):
```unknown
vx_tts_utterance_id utteranceId;
vx_tts_speak(managerId, voiceId, "Example message, this is great!", tts_dest_queued_local_playback, &utteranceId);
```

Example 3 (unknown):
```unknown
vx_tts_utterance_id utteranceId;
vx_tts_speak(managerId, voiceId, "Example message, this is great!", tts_dest_queued_local_playback, &utteranceId);
```

Example 4 (unknown):
```unknown
vx_tts_speak
```

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/analytics/manual/sdk5-migration-guide

---

## Make a channel history and TTL request

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/server-side-recording/ssr-make-channel-ttl-and-history-request

**Contents:**
- Make a channel history and TTL request#

The following code displays an example of how to pull 10 minutes of data per speaker in the channel. It gets 5 minutes of data in the past from the subscription time and continues to get data for 5 minutes into the future.

**Examples:**

Example 1 (unknown):
```unknown
curl -x POST
http://{{host}}/ssr/subscribe?auth_token={{auth_token}}&history=5&destination_credentials={"bucket":"bucket-name","access_key_id":"aws-access-id","secret_access_key":"aws-secret-key"}&target_uri=sip:confctl-g-.issuer.channel_name@domain.vivox.com&target_type=channel&ttl=5
```

Example 2 (unknown):
```unknown
curl -x POST
http://{{host}}/ssr/subscribe?auth_token={{auth_token}}&history=5&destination_credentials={"bucket":"bucket-name","access_key_id":"aws-access-id","secret_access_key":"aws-secret-key"}&target_uri=sip:confctl-g-.issuer.channel_name@domain.vivox.com&target_type=channel&ttl=5
```

---

## Network reconnect failure

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/network-reconnect-failure

**Contents:**
- Network reconnect failure#

If the connection recovery process is ultimately unable to reconnect to a session, a vx_evt_account_login_state_change event fires with a state of login_state_logged_out and a status_code of VxNetworkReconnectFailure. This only occurs when all attempts to reestablish communication with Vivox servers have failed. We recommend that you indicate a network reconnection failure to users because user action might be required to address any issues with their network connection health.

The following code is an example of a scenario in which automatic connection recovery has failed:

**Examples:**

Example 1 (unknown):
```unknown
vx_evt_account_login_state_change
```

Example 2 (unknown):
```unknown
login_state_logged_out
```

Example 3 (unknown):
```unknown
status_code
```

Example 4 (unknown):
```unknown
void HandleLoginStateChange(vx_evt_account_login_state_change &resp)
{
    if (evt.state == login_state_logged_in)
    {
        printf("%s is logged in\n", evt.account_handle);
    }
    else if (evt.state == login_state_logged_out)
    {
        if (evt.status_code != 0)
        {
            if (evt.status_code == VxNetworkReconnectFailure)
            {
                // Automatic connection recovery has failed; user notification is recommended
                printf("%s is logged out after automatic recovery attempts failed\n", evt.acct_handle);
            }
            else
            {
                printf("%s logged out with status %d:%s\n", evt.status_code, vx_get_error_string(evt.status_code));
            }
        }
        else
        {
            printf("%s is logged out\n", evt.acct_handle);
        }
    }
}
```

---

## Text evidence management

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/chat-history/text-evidence-management

**Contents:**
- Text evidence management#

Text evidence management is an optional feature that helps moderators evaluate player reports with conversational evidence. To enable this feature, contact Unity Support or your Vivox representative.

With this server-to-server API, you can request text messages from player chat history to review instances of reported behavior. All versions are stored for review if a message was edited, deleted, or redacted by the Chat filter. The text evidence management feature contains a text-evidence-report endpoint and a player-evidence-report endpoint. The text-evidence-report endpoint returns a report for a given channel or direct message. The player-evidence-report endpoint returns a list of conversations a player participated in with optional time filters.

When active, Text Evidence Management has a storage limit of 30 days, extending the storage limit of Chat history.

The API leverages Unity Dashboard service accounts for authentication. After enablement, follow the admin authentication instructions (Unity Services Web API Docs) to create a service account and add the Text Evidence Management role.

---

## AVAudioPlayer volume inconsistencies

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/ios/avaudioplayer

**Contents:**
- AVAudioPlayer volume inconsistencies#

If your app uses AVAudioPlayer, you must call prepareToPlay() before the Vivox SDK uses the microphone. If prepareToPlay() is called while the Vivox SDK is using the microphone, the audio for that instance of the AVAudioPlayer will be at a lower volume. For more information, refer to the Apple developer documentation on prepareToPlay().

This behavior can also occur when recovering from an audio interruption. For a workaround, see Restore audio volume.

---

## Initialize the AVAudioSession on app startup

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/ios/initialize-avaudiosession-on-app-startup

**Contents:**
- Initialize the AVAudioSession on app startup#

We strongly recommend that you set the AVAudioSession to AVAudioSessionCategoryPlayback, AVAudioSessionCategoryAmbient, or to another non-recording category on app startup.

Caution: Configuring the AVAudioSession near to or during voice chat can lead to unexpected volume changes. Limit this activity to the early stages of the app’s life cycle, unless you are restoring the device audio volume.

**Examples:**

Example 1 (unknown):
```unknown
AVAudioSessionCategoryPlayback
```

Example 2 (unknown):
```unknown
AVAudioSessionCategoryAmbient
```

---

## XMPP representation - Edit a message in a channel

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/chat-history/edit-delete/xmpp-edit-in-channel

**Contents:**
- XMPP representation - Edit a message in a channel#

Example edit request:

Example edit response:

Edited event notifications can be propagated back to every participant in the channel.

Example of propagating an edited message event:

**Examples:**

Example 1 (unknown):
```unknown
<iq type='set' id='...' from='...' to='...'>
 <query xmlns='urn:xmpp:mam:3'>
  <x xmlns='jabber:x:data' type='submit'>
   <field var='FORM_TYPE' type='hidden'>
    <value>urn:xmpp:mam:3</value>
   </field>
   <field var='message-id'>
    <value>123456789</value>
   </field>
   <field var='message'>
    <value>New Message</value>
   </field>
  </x>
 </query>
</iq>
```

Example 2 (unknown):
```unknown
<iq type='set' id='...' from='...' to='...'>
 <query xmlns='urn:xmpp:mam:3'>
  <x xmlns='jabber:x:data' type='submit'>
   <field var='FORM_TYPE' type='hidden'>
    <value>urn:xmpp:mam:3</value>
   </field>
   <field var='message-id'>
    <value>123456789</value>
   </field>
   <field var='message'>
    <value>New Message</value>
   </field>
  </x>
 </query>
</iq>
```

Example 3 (unknown):
```unknown
<iq from="someRoomSIP@cats.vivox.com"
    id="..."
    to="alice@cats.vivox.com"
    type="result">
    <message-edited xmlns="urn:vivox:message-edited" id="...">
        <new-message>New Message</new-message>
        <edit-time>1656521027</edit-time>
    </message-edited>
</iq>
```

Example 4 (unknown):
```unknown
<iq from="someRoomSIP@cats.vivox.com"
    id="..."
    to="alice@cats.vivox.com"
    type="result">
    <message-edited xmlns="urn:vivox:message-edited" id="...">
        <new-message>New Message</new-message>
        <edit-time>1656521027</edit-time>
    </message-edited>
</iq>
```

---

## Write unit tests

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/unit-testing

**Contents:**
- Write unit tests#

Unit testing your code helps to identify and fix issues early in the development process, ensuring that your code works as expected and reducing the risk of introducing bugs into your module.

Refer to Microsoft's documentation on Creating the first test for more information about unit testing with NUnit.

---

## Chat history query with blocked participants

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/chat-history/chat-history-query-blocked-participant

**Contents:**
- Chat history query with blocked participants#

Messages from blocked participants are not included in the results of a chat history query.

Parameters in chat history requests:

---

## Push-to-talk

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/transmission/push-to-talk

**Contents:**
- Push-to-talk#

Many games feature push-to-talk (PTT) functionality that allows users to control when they have a live microphone. For most users, it is sufficient to globally mute and unmute the microphone by using IAudioDevices.Muted. However, there are two notable situations where we recommend that you use transmission for muting purposes.

If a user has more than one audio source (for example, when they are both speaking into a microphone and injecting audio), then muting only the microphone does not stop the injected audio (if someone else mutes the user, then all of their audio is muted).

To ensure that PTT functionality can turn on or turn off muting for both microphone audio and injected audio, set transmission between None and Single/All.

If a user is in two audio channels (for example, 2D team chat and 3D area chat), and you want separate PTT keys for each channel so a user can speak into one or both channels at a time when PTT is turned on, and into no channels when PTT is turned off, then use SetTransmissionMode() between each of the different modes.

**Examples:**

Example 1 (unknown):
```unknown
IAudioDevices.Muted
```

Example 2 (unknown):
```unknown
SetTransmissionMode()
```

---

## Mute the local user

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/muting/mute-user-microphone

**Contents:**
- Mute the local user#
- VivoxInputDevice mute#
- TransmissionMode#

The Vivox Unity Package has two ways to mute a local user's audio input:

Use VivoxService.Instance.MuteInputDevice() and VivoxService.Instance.UnmuteInputDevice() to control whether or not the local input device is muted. This will mute the user for all channel sessions that they are in.

VivoxInputDevice mute is the recommended mute method for consistency unless you must manage which individual channels a user can speak into.

For more control over where a user's input is transmitted, use VivoxService.Instance.SetChannelTransmissionModeAsync(TransmissionMode transmissionMode, string channelName = null).

TransmissionMode allows a user's input to be transmitted into all channels they are in, a single channel or no channels at all.

Set TransmissionMode to TransmissionMode.All to transmit the user's input to all channels they are in. Refer to the following code as an example:

Set TransmissionMode to TransmissionMode.Single to transmit the user's input into a single channel. Set the specified channel name in the channelName field. Refer to the following code as an example:

Set TransmissionMode to TransmissionMode.None to mute the user's input in all channels. The best practice is to use VivoxInputDevice to handle this situation; however, TransmissionMode.None can have its use cases, mainly when used with the other two transmission mode settings to unify the input settings. Refer to the following code as an example of TransmissionMode.None:

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.MuteInputDevice()
```

Example 2 (unknown):
```unknown
VivoxService.Instance.UnmuteInputDevice()
```

Example 3 (unknown):
```unknown
VivoxService.Instance.SetChannelTransmissionModeAsync(TransmissionMode transmissionMode, string channelName = null)
```

Example 4 (unknown):
```unknown
TransmissionMode.All
```

---

## Bluetooth audio profile management

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/android/bluetooth-profile-management

**Contents:**
- Bluetooth audio profile management#
- Bluetooth profile#

The Vivox SDKs for Android support selecting a preference for a Bluetooth audio profile. By default, Bluetooth Synchronous Connection Oriented Link (Hands-Free Profile) is preferred so that microphone audio is captured from the Bluetooth headset. The only other available preference is for Bluetooth Advanced Audio Distribution Profile (A2DP), which allows for high-quality stereo audio output at the cost of having to get microphone audio from the Android handset microphones.

The Bluetooth profile preference may be set by developers (or users, if exposed in the application's settings) in order to achieve the desired Bluetooth behavior on Android. This documentation goes into the details of what the Bluetooth profiles are and what is to be expected from the default profile preference (SCO) and its alternative (A2DP).

The Bluetooth API includes support for working with Bluetooth profiles. A Bluetooth profile is a wireless interface specification for Bluetooth-based communication between devices, such as the Hands-Free profile. For example, for a mobile device to connect to a wireless headset, both devices must support the Hands-Free profile.

The Bluetooth API provides implementations for the following Bluetooth profiles that are relevant to Vivox:

---

## Channels

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/channels-toc

**Contents:**
- Channels#

In Vivox, communication happens within channels.

Channels are a resource that is used to provide audio or text to one or more participants. An individual channel is identified in the Vivox system by using a URI.

Channels support the following functionality:

Channels have the following characteristics:

---

## Volume shifts

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/ios/volume-shifts

**Contents:**
- Volume shifts#

iOS increases the volume of ducked game audio unexpectedly and without notification when certain device events occur. An example of this scenario is when you dismiss the AirPods battery dialog.

The following steps detail how to reproduce this behavior:

Configure the AVAudioSession with:

Open the capture device with VoiceProcessingIO enabled. To do this, use either:

Play audio from an AVAudioPlayer.

Open the AirPods case.

Dismiss the AirPods battery dialog.

Note that the audio player volume increases significantly.

**Examples:**

Example 1 (unknown):
```unknown
AVAudioSessionCategoryPlayAndRecord
```

Example 2 (unknown):
```unknown
AVAudioSessionModeVoiceChat
```

Example 3 (unknown):
```unknown
kAudioUnitSubType_VoiceProcessingIO
```

Example 4 (unknown):
```unknown
setVoiceProcessingEnabled(true)
```

---

## Access token payload

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/access-token-format/access-token-payload

**Contents:**
- Access token payload#
- Unity Authentication IDs within payloads#

The payload is a base64url-encoded JSON object that contains the claims asserted by the token. You can list claims in the payload in any order.

The following table details the claims that you can use in a token. For more information on which parameters are required for each token, see Access token examples.

If your application is using Unity Authentication Service to authenticate players, you can use the UAS ID within a Vivox access token to ensure proper player identity. SIP URIs, which are used to identify players, must be in the proper format to ensure that the player ID is parsed from the full SIP URI. This format is mandatory for using any Unity Moderation service with VATs.

The following is an example of the expected SIP format:

Note: It is the f and sub fields that will need this format to work with UAS.

Note: Ensure that you only include the parameters from the example. Claims that contain the wrong parameters return "invalid signature" or "malformed payload" errors on the API calls.

Guarantee token uniqueness.

If all other claims are identical, this must be different or the token could be rejected as already used.

Note: We recommend that you use an unsigned integer that is increased by 1 for every token that is generated.

sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com

A user SIP URI that is used for mute and kick actions.

This is the user who is muted or unmuted, or the user who is kicked.

sip:.blindmelon-AppName-dev.beef.@tla.vivox.com

This is a user SIP URI that is used in all actions.

This is the user who performs actions such as signing in, joining the channel, or muting another user.

blindmelon-AppName-dev

An application-specific issuer.

The issuer is provided when you create an application on the Unity Cloud Dashboard.

For more information, see Supported values for the Vivox action claim.

sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com

This is a channel SIP URI that is used in join, mute, kick, and transcription actions.

This is the channel where the action takes place.

The expiration time as epoch seconds.

This value is usually the current time plus 90 seconds.

**Examples:**

Example 1 (unknown):
```unknown
sip:.issuer.unity_player_id.unity_environment_id.@domain.vivox.com
```

Example 2 (unknown):
```unknown
sip:.issuer.unity_player_id.unity_environment_id.@domain.vivox.com
```

---

## Generate a token on a secure server

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/generate-token-secure-server/generate-token-secure-server-toc

**Contents:**
- Generate a token on a secure server#

Before you release your game to production, you need to generate your access tokens on a secure server and then send those access tokens to the game client. Allowing for token generation on the client during production is a security risk and can cause unexpected token expiration errors for your users.

Note: The actual implementation depends on your server programming environment and language.

---

## Filter a speaker by channel

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/server-side-recording/ssr-filter-speaker-by-channel

**Contents:**
- Filter a speaker by channel#

If a speaker is in multiple channels and you only want to record the one channel they are in, pass the channel to the filter_uris query.

For example, if a speaker is in channelA and channelB, and you only want to record what the user says in channelA, then if you pass channelA to the filter_uris query, it excludes anything the speaker says in channelB.

For an additional example, if you have speakerA, speakerB, and speakerC in a channel, and you only want to record data from speakerA and speakerB, then you can filter those two users by passing them in to the filter_uris parameter, which excludes speakerC from the recording. The following code displays an example of how to filter for this scenario.

**Examples:**

Example 1 (unknown):
```unknown
curl -x POST
http://{{host}}/ssr/subscribe?auth_token={{auth_token}}&history=5&destination_credentials={"bucket":"bucket-name","access_key_id":"aws-access-id","secret_access_key":"aws-secret-key"}&target_uri=sip:confctl-g-.issuer.channel_name@domain.vivox.com&target_type=channel&ttl=5&filter_uris=["sip:.issuer.speakerA.@domain.vivox.com", "sip:.issuer.speakerB.@domain.vivox.com"]
```

Example 2 (unknown):
```unknown
curl -x POST
http://{{host}}/ssr/subscribe?auth_token={{auth_token}}&history=5&destination_credentials={"bucket":"bucket-name","access_key_id":"aws-access-id","secret_access_key":"aws-secret-key"}&target_uri=sip:confctl-g-.issuer.channel_name@domain.vivox.com&target_type=channel&ttl=5&filter_uris=["sip:.issuer.speakerA.@domain.vivox.com", "sip:.issuer.speakerB.@domain.vivox.com"]
```

---

## Messaging

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/messaging/messaging-toc

**Contents:**
- Messaging#

---

## Create a build

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/guides/create-a-build

**Contents:**
- Create a build#
- Additional resources#

The embedded setup guide walks you through creating your first build. However, you can always add more builds by following these instructions.

Note: You can create up to 100 builds per organization.

Refer to the Dedicated Server Build Target documentation if you're using the Unity Engine.

---

## Positional channels

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/channels/positional-channels

**Contents:**
- Positional channels#
- Related pages#

When working with positional channels (also referred to as 3D channels), a player’s position and facing direction matter. This is in contrast to non-positional channels, which act more like radio communication.

Note: When working with both positional and non-positional channels, join the positional channel first, followed by the non-positional channels.

You can create positional channels with different properties to allow for a range of audio spaces. For example, you might have a scenario where acoustically realistic sounding audio is needed for outdoor spaces, or a scenario where shorter-range, quickly dampening audio is more appropriate to account for tight inner corridors or for how voice carries in heavy rain. Positional channels can also add a "cocktail party" effect to the audio, which provides ambient crowd noise. Any style allows for voice chat to naturally fade in and out to near zero loudness as players approach or walk away from each other.

Vivox audio processing mixes all of the audio for every player, including volume attenuation over a distance and channel fading, which is based on the direction that the character or camera is facing. Channel participant added or removed events are based on players entering or exiting your max range, which ensures that you always know which players are close enough to hear.

---

## Create a unit test project

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/unit-testing-create-project

**Contents:**
- Create a unit test project#
- JetBrains Rider#
- Microsoft Visual Studio#
- Exclude unit tests from publishing#
  - Remove from solution release configuration#
  - Set IsPublishable to false#

Before you create a unit test, first set up a unit test project. This page describes how to set up a project in the following IDEs:

Exclude your test projects from publishing to prevent size issues and inefficiencies with your modules. You can exclude test projects in the following ways:

In your IDE, open the solution configuration and remove the test project from the release configuration. They should not be enabled for build.

Examples for Visual Studio and Rider below:

In your unit test project properties, set IsPublishable to false:

**Examples:**

Example 1 (unknown):
```unknown
IsPublishable
```

Example 2 (unknown):
```unknown
<PropertyGroup>
  ...
    <IsPublishable>false</IsPublishable>
  ....
  </PropertyGroup>
```

Example 3 (unknown):
```unknown
<PropertyGroup>
  ...
    <IsPublishable>false</IsPublishable>
  ....
  </PropertyGroup>
```

---

## Mute other users for a specific user

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/muting/mute-other-users

**Contents:**
- Mute other users for a specific user#

You can mute other users for a specific user by using a local mute. The VivoxParticipant.MutePlayerLocally function prevents a user from hearing a muted user, but other users in the channel continue to hear the muted user. VivoxParticipant.UnmutePlayerLocally can be used to allow the user to hear the muted user again.

For example, Cynthia, Fernando, and Wang are in a channel. Fernando does not want to hear Wang’s audio. By using a local mute, you can allow Fernando to stop hearing Wang’s audio. However, Cynthia continues to hear Wang, and Wang continues to hear Cynthia and Fernando.

Because this is a VivoxParticipant function, it is best to have some way to handle individual UI representations for each VivoxParticipant, so that everyone has their own mute buttons (as shown in the Chat Channel Sample and the Participant Management page).

Alternatively, the following example showcases using the VivoxServices.Instance.ActiveChannels dictionary to find and mute/unmute a specific participant in a particular channel by using the PlayerId of the participant and the ChannelName of the channel:

Note: To update a UI element to indicate which users a user has muted, you need to wait until you receive an event from the Vivox SDK to confirm that the other users have been muted. You can find examples of handling this event and implementing it into a UI in the Chat Channel Sample and on the Participants management page.

**Examples:**

Example 1 (unknown):
```unknown
VivoxParticipant.MutePlayerLocally
```

Example 2 (unknown):
```unknown
VivoxParticipant.UnmutePlayerLocally
```

Example 3 (unknown):
```unknown
VivoxServices.Instance.ActiveChannels
```

Example 4 (javascript):
```javascript
void MutePlayerLocally(string PlayerId, string ChannelName)
{
    VivoxService.Instance.ActiveChannels[ChannelName].Where(participant => participant.PlayerId == PlayerId).First().MutePlayerLocally();
}

void UnmutePlayerLocally(string PlayerId, string ChannelName)
{
    VivoxService.Instance.ActiveChannels[ChannelName].Where(participant => participant.PlayerId == PlayerId).First().UnmutePlayerLocally();
}
```

---

## Make a speaker TTL-only request

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/server-side-recording/ssr-make-speaker-ttl-only-request

**Contents:**
- Make a speaker TTL-only request#

The following code displays an example of how to upload five minutes of data from the subscription time to five minutes into the future for only the target speaker.

**Examples:**

Example 1 (unknown):
```unknown
curl -x POST
http://{{host}}/ssr/subscribe?auth_token={{auth_token}}&history=0&destination_credentials={"bucket":"bucket-name","access_key_id":"aws-access-id","secret_access_key":"aws-secret-key"}&target_uri=sip:.issuer.speaker.@domain.vivox.com&target_type=speaker&ttl=5
```

Example 2 (unknown):
```unknown
curl -x POST
http://{{host}}/ssr/subscribe?auth_token={{auth_token}}&history=0&destination_credentials={"bucket":"bucket-name","access_key_id":"aws-access-id","secret_access_key":"aws-secret-key"}&target_uri=sip:.issuer.speaker.@domain.vivox.com&target_type=speaker&ttl=5
```

---

## Server-side recording

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/server-side-recording/ssr-overview

**Contents:**
- Server-side recording#

Note: Server-side recording is in limited early release and must be enabled by Vivox. For pricing information and to discuss enabling this service for your organization, contact your sales representative.

Server-side recording (SSR) is an optional paid Vivox service that can help you to manage the health and safety of the voice communications in your player community.

SSR provides customer service representatives with a workflow for capturing and reviewing conversations that occur on the Vivox platform where potentially unwanted behavior is either reported or detected.

---

## Transmission methods

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/transmission/transmission-methods

**Contents:**
- Transmission methods#

Two methods are available to determine which channels a user is transmitting into:

**Examples:**

Example 1 (unknown):
```unknown
ILoginSession::GetTransmittingChannels()
```

Example 2 (unknown):
```unknown
IChannelSession
```

Example 3 (unknown):
```unknown
IChannelSession::IsTransmitting()
```

---

## Access Token Developer Guide

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/access-token-guide/access-token-guide-toc

**Contents:**
- Access Token Developer Guide#

Player access to Vivox resources is controlled through Vivox access tokens. Vivox access tokens contain a payload that defines the privileged operation, are signed by the game server by using a token signing key, and are delivered by the client to the Vivox system when the player wants to perform a privileged operation. A Vivox access token is similar to a JSON Web Token, but instead has an empty access token header.

Note: Vivox Access Tokens are an optional, and advanced, feature for managing player channel access. Unity Authentication tokens are the recommended way to manage players when channel access control isn't required. The Authentication SDK for Unreal Engine is available as part of the Unity Gaming Services SDK plug-in for Unreal Engine.

Access tokens have the following characteristics:

You can only use atoken once. After you use a token for the privileged operation, it cannot be reused.

Tokens expire even if they are never used. You cannot use a token after the expiration time that is set by the token issuer.

Game clients require access tokens to perform operations in the Vivox system.

Note that before either a client or a server can generate a token, you need a token issuer and a token signing key.For more information, refer to Where do I find my custom application credentials? in the Unity Support knowledge base.

The following diagram displays a typical interaction between a game client, the game server, and the Vivox system:

---

## Check build results

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/check-build-results/overview

**Contents:**
- Check build results#
- View build results#
  - Build Status#
  - Detailed build information#
- Download build artifacts#
- Share build artifacts externally#
- Additional operations#
- View Logs#

Build Automation provides detailed information about each build's status, metrics, and progress. This helps you monitor build performance, troubleshoot issues, and track running builds.

To access detailed information on all build attempts in the Unity Dashboard, navigate to DevOps > Build Automation > Build history.

The build history table displays a list of all your build attempts along with their statuses, build targets, platforms, build times, and completion times.

Build Automation marks each with a status icon to indicate its result:

You can view more detailed information on specific builds, including those in progress.

Select the arrow next to the build entry in the Build History table to display the detailed information panel and expand metrics for the selected build, including:

To access and download the build artifacts for successful builds:

This downloaded file contains the build outputs and assets generated during the build, which can be used for further testing, distribution, or archival purposes.

You can share build artifacts with individuals who don't have an account associated with your project. Build Automation can automatically generate shareable links, allowing you to send builds via email or through integrations like Slack. Additionally, you can generate these links manually from the dropdown menu of a given build.

Note that share links expire after two weeks by default. If needed, you can adjust the expiration period within the share link options of the build you want to share.

To enable automatic sharing of build artifacts in the Unity Dashboard, go to Build Automation > Settings > General and toggle the Automatic build sharing option. This setting automatically creates a share link when your build successfully completes. It also populates notifications with the share link.

After reviewing your build results, you can perform several operations directly from the build history table to manage and interact with your builds. Access these options in the Unity Dashboard:

If a build encounters issues that result in build failures, you can view logs to diagnose the problem:

---

## Text-to-speech destinations

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/text-to-speech/tts-destinations

**Contents:**
- Text-to-speech destinations#

Text-to-speech (TTS) messages are injected to destinations. Destinations determine two factors: output and mechanism.

Output is where the synthesized speech plays to and decides who hears the TTS message. Each destination uses one of the following outputs:

Mechanism is how new messages are handled when there is an ongoing message playing. Each destination uses one of the following injection mechanisms:

You can inject synthesized speech into the following destinations:

Note: Most destinations are independent and do not affect the behavior of any other destinations. However, the two queued destinations involving remote transmission share a queue.

Remote Transmission with Local Playback

Queued Remote Transmission

Queued Local Playback

Queued Remote Transmission with Local Playback

You can obtain the state and content of all messages that are playing or are enqueued in any destination by using ITextToSpeech::GetMessages(TTSDestination Destination). Queued destinations return their TArray<ITTSMessage\*> in queue order, and others in the order that speech was injected.

**Examples:**

Example 1 (unknown):
```unknown
TTSDestination::RemoteTransmission
```

Example 2 (unknown):
```unknown
TTSDestination::LocalPlayback
```

Example 3 (unknown):
```unknown
TTSDestination::RemoteTransmissionWithLocalPlayback
```

Example 4 (unknown):
```unknown
TTSDestination::QueuedRemoteTransmission
```

---

## Channel-based message edit

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/chat-history/edit-delete/channel-message-edit

**Contents:**
- Channel-based message edit#

To edit a channel-based message, you need to sign in and join at least one channel. The message to be edited must have been sent to every participant in the channel.

Note: Only the sender of a message can edit it.

Use vx_req_session_edit_message to make an edit request. vx_req_session_edit_message will return:

You will receive a vx_evt_session_edit_message event for every message edited.

The following code is an example of how to edit a message for a specific channel:

After you edit this message, the game must then process the vx_evt_session_edit_message event.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_session_edit_message
```

Example 2 (unknown):
```unknown
vx_req_session_edit_message
```

Example 3 (unknown):
```unknown
vx_evt_session_edit_message
```

Example 4 (unknown):
```unknown
vx_req_session_edit_message *req;
vx_req_session_edit_message_create(&req);
req->session_handle = vx_strdup("channelName");
req->message_id = vx_strdup("oldMessageID");
req->new_message = vx_strdup("This is a test message");
vx_issue_request2(&req->base);
```

---

## Push-to-talk

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/transmission/push-to-talk

**Contents:**
- Push-to-talk#

Many games feature push-to-talk (PTT) functionality, which allows users to control when they have a live microphone. For most users, it is sufficient to globally mute or unmute the microphone by using IAudioDevices::SetMuted(). However, the following scenarios detail when we recommend that you instead use transmission to selectively silence a user.

If a user has more than one audio source (for example, if they are both speaking into a microphone and are injecting audio), then muting only the microphone does not stop the injected audio. For example, if someone else mutes the user, then all of the user's audio is muted.

To use PTT to turn on or turn off muting for both microphone audio and injected audio, we recommend that you set transmission between None and Single/All.

If a user is in two audio channels (for example, 2D team chat and 3D area chat), and you want separate PTT keys for each channel to allow the user to speak into only one channel (or both) at a time when enabled, and into no channels when disabled, then we recommend that you use SetTransmissionMode() between each of the different modes as appropriate.

If you want a single PTT key to turn on or turn off muting for all channels at a time rather than individually, then switching transmission between All/None has no real advantage over only muting the mic, except for when you are injecting audio.

**Examples:**

Example 1 (unknown):
```unknown
IAudioDevices::SetMuted()
```

---

## Text chat developer guide

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/text-chat-guide-overview

**Contents:**
- Text chat developer guide#

Vivox provides a text chat service:

Text chat - channel-based and directed text chat.

The following additional features can be enabled:

The server can customize a maximum byte count for text messages in its pre-login configuration. By default, the maximum is 320 bytes with UTF-8 encoding. A text message that exceeds the maximum length in bytes is rejected with a VxErrorMessageTextTooLong error. For more information about UTF-8, see Character encoding.

Text chat requires a Unicode-supported system to work properly. The Unity Editor has full Unicode support in versions 2017.1 and later.

The following table details text chat policy:

1000 bytes per second

The maximum rate at which a player can send data over text chat.

This is configured server-side by Vivox.

---

## Requests that require access tokens

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/requests-that-require-access-tokens

**Contents:**
- Requests that require access tokens#

Each privileged operation requires a token in a specific format. Examples of privileged operations include:

For more information, see Supported values for the Vivox action claim.

---

## Text evidence management

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/text-chat-guide/text-evidence/text-evidence-management

**Contents:**
- Text evidence management#

Text Evidence Management is an optional feature that helps moderators evaluate player reports with conversational evidence. To enable this feature, contact Vivox Support.

With this server-to-server API, you can request text messages from player chat history to review instances of reported behavior. All versions are stored for review if a message was edited, deleted, or redacted by the Chat filter. The text evidence management feature contains a text-evidence-report endpoint and a player-evidence-report endpoint. The text-evidence-report endpoint returns a report for a given channel or direct message. The player-evidence-report endpoint returns a list of conversations a player participated in with optional time filters.

When active, Text Evidence Management has a storage limit of 30 days, extending the storage limit of Chat History.

The API leverages Unity Dashboard service accounts for authentication. After enablement, follow the admin authentication instructions (Unity Services Web API Docs) to create a service account and add the Text Evidence Management role.

**Examples:**

Example 1 (unknown):
```unknown
text-evidence-report
```

Example 2 (unknown):
```unknown
player-evidence-report
```

Example 3 (unknown):
```unknown
text-evidence-report
```

Example 4 (unknown):
```unknown
player-evidence-report
```

---

## Channel name criteria

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/channels/channel-names

**Contents:**
- Channel name criteria#

When you create a channel name, the following criteria apply:

0x30-0x39, 0x41-0x5A, 0x61-0x7A

Note: You can only use the percent sign for URL encoding. Follow the percent sign by two uppercase hex characters.

Note: Channel names can have mixed case for clarity. However, do not use different case versions of the same channel. For example, do not use "MyChannel" and "mychannel" at the same time, or audio connection issues can occur.

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/troubleshooting/codec-comparison-core

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/access-control

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/safety/moderation-requirements

---

## Example: Mute tokens

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/access-token-guide/access-token-examples/example-mute-token

**Contents:**
- Example: Mute tokens#
- Signed in user muting other user in a channel#
- Admin user muting other user in a channel#

The token in this example allows the user "beef" to mute the user "jerky" in the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

The token in this example allows the user "blindmelon-AppName-dev-Admin" to mute the user "jerky" in the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

**Examples:**

Example 1 (unknown):
```unknown
{
    "vxi":123456,
    "sub":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "f":"sip:.blindmelon-AppName-dev.beef.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"mute",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 2 (unknown):
```unknown
{
    "vxi":123456,
    "sub":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "f":"sip:.blindmelon-AppName-dev.beef.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"mute",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 3 (unknown):
```unknown
e30.eyJ2eGkiOjEyMzQ1Niwic3ViIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2Lmplcmt5LkB0bGEudml2b3guY29tIiwiZiI6InNpcDouYmxpbmRtZWxvbi1BcHBOYW1lLWRldi5iZWVmLkB0bGEudml2b3guY29tIiwiaXNzIjoiYmxpbmRtZWxvbi1BcHBOYW1lLWRldiIsInZ4YSI6Im11dGUiLCJ0Ijoic2lwOmNvbmZjdGwtZy1ibGluZG1lbG9uLUFwcE5hbWUtZGV2LnRlc3RjaGFubmVsQHRsYS52aXZveC5jb20iLCJleHAiOjE2MDAzNDk0MDB9.vM9zkCXTORjgv8w7eiMHHHkc4DumTwR_-I06y4SnpHA
```

Example 4 (unknown):
```unknown
e30.eyJ2eGkiOjEyMzQ1Niwic3ViIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2Lmplcmt5LkB0bGEudml2b3guY29tIiwiZiI6InNpcDouYmxpbmRtZWxvbi1BcHBOYW1lLWRldi5iZWVmLkB0bGEudml2b3guY29tIiwiaXNzIjoiYmxpbmRtZWxvbi1BcHBOYW1lLWRldiIsInZ4YSI6Im11dGUiLCJ0Ijoic2lwOmNvbmZjdGwtZy1ibGluZG1lbG9uLUFwcE5hbWUtZGV2LnRlc3RjaGFubmVsQHRsYS52aXZveC5jb20iLCJleHAiOjE2MDAzNDk0MDB9.vM9zkCXTORjgv8w7eiMHHHkc4DumTwR_-I06y4SnpHA
```

---

## Direct voice audio access

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/direct-voice-access/direct-voice-audio-access-toc

**Contents:**
- Direct voice audio access#

Direct access to the audio buffers on the capture client or render client allows developers to:

Note: The Vivox SDK automatically handles the capture and render of voice data without requiring audio buffer access. Unless your implementation requires the situations detailed in the preceding list, you most likely do not need audio buffer access.

The Vivox Core SDK provides four hooks for optional callback functions that allow access to the audio buffers:

Note: Callbacks only occur when in a channel session and the audio data there is interleaved and in 16-bit integer format.

Vivox Core users have access to these hooks directly as displayed in the following sections.

Note: Accessing or modifying the audio buffers incorrectly can have a substantial negative impact on the user experience. The callbacks covered in these topics occur on Vivox’s real-time audio thread so take precautions when using them. Limit memory allocation on this thread to avoid negatively affecting the audio quality, especially on low-end devices.

---

## Channel identifiers for large scale games

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/channels/channel-identifiers-for-large-scale-games

**Contents:**
- Channel identifiers for large scale games#

If either of the following statements are true, then use caution when constructing your channel identifiers:

You can split large scale games across multiple physical audio servers ("shards") at the discretion of Vivox Operations. Construct your channel identifiers to use the following format:

For example, consider the application "Queen of the Death" (QotD), which allocates users according to geographical region, for example,"NA" for "North America". Assume that QotD joins a 3D channel for all participants in a match, and then also joins a 2D channel for members of a squad. In this case, the best practice for QotD is to apply the following criteria:

The match identifier assigned for this match is d0634bad1ca5a9cd, and the 3D space is tundra. Further assume that squads are assigned integer IDs starting with 1.

2D channel URI for squad 1

2D channel URI for squad 2

Caution: Failing to follow these guidelines might result in multichannel users not all being co-located on the same shard.

**Examples:**

Example 1 (unknown):
```unknown
d0634bad1ca5a9cd
```

Example 2 (unknown):
```unknown
sip:confctl-d-issuer.NA.d0634bad1ca5a9cd-tundra@example.vivox.com
```

Example 3 (unknown):
```unknown
sip:confctl-d-issuer.NA.d0634bad1ca5a9cd-tundra@example.vivox.com
```

Example 4 (unknown):
```unknown
sip:confctl-g-issuer.NA.d0634bad1ca5a9cd-1@example.vivox.com
```

---

## Troubleshooting Bluetooth SCO underrun issues

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/android/troubleshooting-bluetooth-sco-underun-issues

**Contents:**
- Troubleshooting Bluetooth SCO underrun issues#

If your users are experiencing Bluetooth SCO underrun issues, which can cause sound to be choppy on SCO, it is possible to force the Bluetooth device to use A2DP by going into the Android Bluetooth settings and disabling the “Phone Calls” option under the Bluetooth device options.

---

## Deploy a Multiplay build configuration

**URL:** https://docs.unity.com/ugs/en-us/manual/mps-sdk/manual/deployment/deploy-multiplay-configuration

**Contents:**
- Deploy a Multiplay build configuration#
- Prerequisites#
- Install the required packages#
- Set up the Deployment window#
- Create a Multiplay configuration#
- Edit a Multiplay configuration#
  - Editing within the Unity Editor#
  - Editing with a text editor#
- Deploy the Multiplay configuration#
- Additional resources#

Use the Deployment window in the Unity Editor to deploy a Multiplay configuration.

You can deploy a Multiplay configuration directly from the Unity editor.

The Multiplay config file is a YAML file that consists of the following collections:

Before you start, ensure you have done the following:

To enable the integration between the Multiplayer Services SDK and Deployment, install the following packages:

You can deploy Multiplay configurations to your cloud environment using the Deployment window.

Once you install the Deployment package, you can access it from the Unity Editor. Once you install the Deployment package, you can access the Deployment window from the Unity Editor by selecting Services > Deployment.

To select the deployment environment, follow these steps before you can use the Deployment window:

To create a Multiplay configuration:

There are two methods to edit a Multiplay configuration: within the Unity Editor, or through a text editor.

To edit the Multiplay environment configuration directly from the Unity Editor, do the following:

The following fields in the Multiplay configuration give you more control over the Multiplay Server deployment build:

To edit the Multiplay environment configuration from a text editor of your choice, do the following:

In the Project window, right-click (macOS: Ctrl+click) on the multiplay_config.gsh YAML file you created above.

Select Show in Explorer on Windows or Reveal in Finder on macOS.

Open the file with your text editor of choice.

Customize the file according to your project. The file has the following format:

To deploy the Multiplay configuration, do the following:

If your configuration assets don't appear in the Deployment window, make sure you activate domain reloading.

To activate domain reloading, do the following:

**Examples:**

Example 1 (unknown):
```unknown
multiplay_config.gsh
```

Example 2 (unknown):
```unknown
multiplay_config.gsh
```

Example 3 (unknown):
```unknown
version: 1.0
builds:
  my build: # replace with the name for your build
    executableName: Build.x86_64 # the name of your build executable
    buildPath: Builds/Multiplay # the location of the build files
    excludePaths: # paths to exclude from upload (supports only basic patterns)
      - Builds/Multiplay/DoNotShip/*.*
      - Builds/Multiplay/Server_Data/app.info
buildConfigurations:
  my build configuration: # replace with the name for your build configuration
    build: my build # replace with the name for your build
    queryType: sqp # sqp or a2s, delete if you do not have logs to query
    binaryPath: Build.x86_64 # the name of your build executable
    commandLine: -port $$port$$ -queryport $$query_port$$ -log $$log_dir$$/Engine.log # launch parameters for your server
    variables: {}
fleets:
  my fleet: # replace with the name for your fleet
    buildConfigurations:
      - my build configuration # replace with the names of your build configuration
    regions:
      North America: # North America, Europe, Asia, South America, Australia
        minAvailable: 0 # minimum number of servers running in the region
        maxServers: 1 # maximum number of servers running in the region
    usageSettings:
      - hardwareType: CLOUD #The hardware type of a machine. Can be CLOUD or METAL.
        machineType: GCP-N2 # Machine type to be associated with these setting.   * For CLOUD setting: In most cases, the only machine type available for your fleet is GCP-N2.   * For METAL setting: Please omit     this field. All metal machines will be using the same setting, regardless of its type.
        maxServersPerMachine: 2 # Maximum number of servers to be allocated per machine.   * For CLOUD setting: This is a required field.   * For METAL setting: This is an optional field.
```

Example 4 (unknown):
```unknown
version: 1.0
builds:
  my build: # replace with the name for your build
    executableName: Build.x86_64 # the name of your build executable
    buildPath: Builds/Multiplay # the location of the build files
    excludePaths: # paths to exclude from upload (supports only basic patterns)
      - Builds/Multiplay/DoNotShip/*.*
      - Builds/Multiplay/Server_Data/app.info
buildConfigurations:
  my build configuration: # replace with the name for your build configuration
    build: my build # replace with the name for your build
    queryType: sqp # sqp or a2s, delete if you do not have logs to query
    binaryPath: Build.x86_64 # the name of your build executable
    commandLine: -port $$port$$ -queryport $$query_port$$ -log $$log_dir$$/Engine.log # launch parameters for your server
    variables: {}
fleets:
  my fleet: # replace with the name for your fleet
    buildConfigurations:
      - my build configuration # replace with the names of your build configuration
    regions:
      North America: # North America, Europe, Asia, South America, Australia
        minAvailable: 0 # minimum number of servers running in the region
        maxServers: 1 # maximum number of servers running in the region
    usageSettings:
      - hardwareType: CLOUD #The hardware type of a machine. Can be CLOUD or METAL.
        machineType: GCP-N2 # Machine type to be associated with these setting.   * For CLOUD setting: In most cases, the only machine type available for your fleet is GCP-N2.   * For METAL setting: Please omit     this field. All metal machines will be using the same setting, regardless of its type.
        maxServersPerMachine: 2 # Maximum number of servers to be allocated per machine.   * For CLOUD setting: This is a required field.   * For METAL setting: This is an optional field.
```

---

## Channel management

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/channels/channel-management

**Contents:**
- Channel management#

Channel management primarily focuses on selecting appropriate channel IDs. and identifying code paths for joining and leaving channels, in addition to authorizing those operations.

By default, channels are named and created on an ad-hoc basis. The process for joining channels is regulated by a game server authorization code prior to joining. The selection of appropriate unique channel IDs is important, as is associating the IDs with relevant in-game data, such as parties, teams, lobbies, and guilds. You can use this identifier in an authenticated channel join request to allow a user to perform an authorized join.

By default, channels have the following properties:

Special channel types also exist that you can use in certain specific or unusual circumstances. For example: the echo channel, static channels, and the 3D positional channel. These special channel types require additional setup and configuration, demand additional resources to provision, and are recommended only for use in cases where standard ephemeral channels are inadequate.

The following list details special channels and their properties:

Persistent/semi-persistent channels

3D positional audio channels

Note: Default channels are generally suitable for most use cases. Echo channels are useful for both internal development and testing, and for providing an audio test environment for the user.

---

## Account chat history for blocked participants

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/chat-history/account-ch-blocked-participant

**Contents:**
- Account chat history for blocked participants#

When you submit a chat history request for an account, queries only show the user’s sent messages.

---

## Use groups to organize build configurations

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/basic-build-configuration/organize-build-configurations-using-groups

**Contents:**
- Use groups to organize build configurations#
- Create a Build target group#
- Related builds#
- Filter builds by group#

Build target groups allow you to associate and build related build target configurations together. The same build target can be part of multiple groups.

If you already have a build target, that means your Unity project is already set up. To access the Build target group option:

Once complete, you can then view your newly created Build target group in the Build target groups table.

When building from a group, all builds are linked together by a Group Build ID. To access all related builds:

When building from a group, the builds mention the Build target group that they are associated with in the Build History table. To see all builds for a specific group:

---

## Anti-flooding

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/chat-history/anti-flooding

**Contents:**
- Anti-flooding#

Set rate limiting by configuring the pre-login parameter MaxTextMessageRate. Setting it to 0 disables the feature.

Rate limiting is enforced by the Vivox SDK. If a user tries to send messages at a rate higher than allowed, a VxErrorMessageTextRateExceeded error is returned.

**Examples:**

Example 1 (unknown):
```unknown
MaxTextMessageRate
```

Example 2 (unknown):
```unknown
VxErrorMessageTextRateExceeded
```

Example 3 (unknown):
```unknown
vx_req_account_send_msg *req;
vx_req_account_send_msg_create(&req);
req->connector_handle = vx_strdup("c1");
req->account_handle = vx_strdup(".issuer-w-dev.mytestaccountname.");
req->language = vx_strdup(“en-us”);
req->user_uri = vx_strdup("sip:theotheruser@vd1.vivox.com");
req->message_body = vx_strdup("Hey there buddy!");
int status = vx_issue_request2(&req->base); // If rate limiting is surpassed, status = VxErrorMessageTextRateExceeded
```

Example 4 (unknown):
```unknown
vx_req_account_send_msg *req;
vx_req_account_send_msg_create(&req);
req->connector_handle = vx_strdup("c1");
req->account_handle = vx_strdup(".issuer-w-dev.mytestaccountname.");
req->language = vx_strdup(“en-us”);
req->user_uri = vx_strdup("sip:theotheruser@vd1.vivox.com");
req->message_body = vx_strdup("Hey there buddy!");
int status = vx_issue_request2(&req->base); // If rate limiting is surpassed, status = VxErrorMessageTextRateExceeded
```

---

## Participant events

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/participant-events

**Contents:**
- Participant events#

The Vivox SDK posts information about individual participants in a channel that is visible to all other participants. This includes the following information:

Handling participant events is optional. If there is no visualization of user state (for example, displaying who has voice enabled), then the game can ignore these events.

To provide a visualization of user state information, a game must handle the following messages:

The following code displays an example of how to handle these messages:

When keeping track of participant state, it is important to use the encoded_uri_with_tag field as the unique identifier of the participant. This allows the game to distinguish between users who have left a channel and users who have rejoined a channel.

Note: Use the is_current_user field to distinguish between events that pertain to you from events that pertain to other users.

**Examples:**

Example 1 (unknown):
```unknown
vx_evt_participant_added
```

Example 2 (unknown):
```unknown
vx_evt_participant_removed
```

Example 3 (unknown):
```unknown
vx_evt_participant_updated
```

Example 4 (unknown):
```unknown
void HandleParticipantAddedEvent(vx_evt_participant_added *evt)
{
    printf("User %s joined %s\n", evt->encoded_uri_with_tag, evt->session_handle);
}

void HandleParticipantRemovedEvent(vx_evt_participant_removed *evt)
{
    printf("User %s left %s\n", evt->encoded_uri_with_tag, evt->session_handle);
}

void HandleParticipantUpdatedEvent(vx_evt_participant_updated *evt)
{
    printf("User %s %s speaking to %s\n", evt->encoded_uri_with_tag,evt->is_speaking ? "is" : "is not", evt->session_handle);
}
```

---

## Unity Docs

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/channels/request-capture-device-access

[!include[](../../../../../vivox-unity/includes/capture-device-access.md]

Unreal Engine editor must also be granted capture device access for voice chat to work in the editor.

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/advanced-build-configuration/overview

---

## Cannot set iOS device volume to 0 when Voice-Processing I/O unit is active

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/ios/cannot-set-ios-device-volume-to-zero

**Contents:**
- Cannot set iOS device volume to 0 when Voice-Processing I/O unit is active#

When using an iOS device and the Voice-Processing I/O unit is active, you cannot set the volume of the iOS device to 0. For more information, see the Apple developer documentation on AVAudioSessionCategoryPlayAndRecord.

A possible workaround for this functionality when you are using Vivox on iOS is to observe changes in AVAudioSession.outputVolume that are being set by the user and then mute your game audio output based on the observed value. For example implementations of this workaround, refer to How to: Observe the iOS volume level to programmatically mute game audio in the Unity Support Knowledge Base.

---

## Active build configuration

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/concepts/active-build-configuration

**Contents:**
- Active build configuration#

Allocations drive Multiplay Hosting fleets by propagating configuration changes to servers as they’re necessary to fulfill player demand. One such configuration is the active build configuration.

A matchmaker controls servers' active build configuration in a typical production environment by requesting servers to meet player needs. But it’s also possible to manually allocate a server with a build configuration.

The simplest ways to change the active build configuration of a server are to:

---

## Change a Bluetooth profile

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/android/change-bluetooth-profile

**Contents:**
- Change a Bluetooth profile#

To change the Bluetooth profile in a Vivox application using the Core SDK, refer to the follow code sample:

To change the Bluetooth profile in a Vivox application using the Vivox Unity SDK, refer to the follow code sample:

**Examples:**

Example 1 (unknown):
```unknown
vx_sdk_config_t config;
	vx_get_default_config3(&config, sizeof(config));
config.bluetooth_profile = vx_bluetooth_profile.vx_bluetooth_profile_a2dp;
... // Any other config changes that are needed
vx_initialize3(&config, sizeof(config));
```

Example 2 (unknown):
```unknown
vx_sdk_config_t config;
	vx_get_default_config3(&config, sizeof(config));
config.bluetooth_profile = vx_bluetooth_profile.vx_bluetooth_profile_a2dp;
... // Any other config changes that are needed
vx_initialize3(&config, sizeof(config));
```

Example 3 (unknown):
```unknown
vx_sdk_config_t vivox_config = new vx_sdk_config_t();
vivox_config.bluetooth_profile = vx_bluetooth_profile.vx_bluetooth_profile_a2dp;
... // Any other config changes that are needed
VivoxService.Instance.Initialize(vivox_config);
```

Example 4 (unknown):
```unknown
vx_sdk_config_t vivox_config = new vx_sdk_config_t();
vivox_config.bluetooth_profile = vx_bluetooth_profile.vx_bluetooth_profile_a2dp;
... // Any other config changes that are needed
VivoxService.Instance.Initialize(vivox_config);
```

---

## Participant events

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/channels/participant-events

**Contents:**
- Participant events#

The Vivox SDK posts information about individual participants in a channel that is visible to all other participants. This includes the following information:

Handling participant events is optional. If there is no visualization of user state (for example, displaying who has voice enabled), then the game can ignore these events.

To provide a visualization of user state information, a game must handle the following messages:

The following code displays an example of how to handle these messages:

Tip: Use the Participant.IsSelf() query to distinguish events that pertain to you from those that pertain to other users.

**Examples:**

Example 1 (unknown):
```unknown
IChannelSession::EventAfterParticipantAdded
```

Example 2 (unknown):
```unknown
IChannelSession::EventBeforeBeforeParticipantRemoved
```

Example 3 (unknown):
```unknown
IChannelSession::EventAfterParticipantUpdated
```

Example 4 (javascript):
```javascript
void UMyGameClass::OnChannelParticipantAdded(const IParticipant &Participant)
{
    ChannelId Channel = Participant.ParentChannelSession().Channel();
    UE_LOG(MyLog, Log, TEXT("%s has been added to %s\n"), *Participant.Account().Name(), *Channel.Name());
}

void UMyGameClass::OnChannelParticipantRemoved(const IParticipant &Participant)
{
    ChannelId Channel = Participant.ParentChannelSession().Channel();
    UE_LOG(MyLog, Log, TEXT("%s has been removed from %s\n"), *Participant.Account().Name(), *Channel.Name());
}

void UMyGameClass::OnChannelParticipantUpdated(const IParticipant &Participant)
{
    ChannelId Channel = Participant.ParentChannelSession().Channel();
    UE_LOG(MyLog, Log, TEXT("%s has been updated in %s\n"), *Participant.Account().Name(), *Channel.Name());
}
```

---

## Manage consumption costs

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/manage-consumption-costs

**Contents:**
- Manage consumption costs#
- Build minutes monthly cap#
- Monthly cap reminder#
- Concurrency cap#

Use Unity Build Automation to help you monitor and control your consumption costs effectively. Set limits and configure notifications so that you can stay within your budget and optimize your build workflows.

The build minutes monthly cap feature allows you to control your spending on build minutes across macOS and Windows builders. You can specify a dollar amount to restrict additional build usage beyond the set cap.

Set your monthly cap in the Unity Dashboard:

Enable the monthly cap reminder to receive email notifications as you approach your build minutes cap. This ensures you are informed before you exceed your set limits.

Set the monthly cap reminder in your Unity Dashboard:

The concurrency cap helps you manage the maximum number of concurrent builds you can run per day. Any builds that exceed the concurrency cap queue, which ensures controlled resource utilization and predictable costs.

Set the concurrency cap in your Unity Dashboard:

---

## Derumbler

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/derumbler

**Contents:**
- Derumbler#

A high-pass filter, referred to as the derumbler, is enabled by default to reject frequencies of 60 Hz and lower. You can modify derumbler properties before or after logging in or joining a session.

This filter uses the following request and response types:

To get derumbler properties, use vx_req_aux_get_derumbler_properties_t. This struct uses the following methods:

To set derumbler properties, use vx_req_aux_set_derumbler_properties_t. This struct uses the following methods:

**Examples:**

Example 1 (unknown):
```unknown
req_aux_get_derumbler_properties
```

Example 2 (unknown):
```unknown
req_aux_set_derumbler_properties
```

Example 3 (unknown):
```unknown
resp_aux_get_derumbler_properties
```

Example 4 (unknown):
```unknown
resp_aux_set_derumbler_properties
```

---

## Edit and Delete

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/chat-history/edit-delete/edit-delete-messages

**Contents:**
- Edit and Delete#
- Client Interface#

Note: This feature is currently only available in the Core SDK and Unity SDK v16.

With the Edit and Delete functionality you can provide users with the ability to edit or delete previously sent text messages.

The Edit and Delete features have interfaces exposed as listed below:

---

## Example: Join_Muted token

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/access-token-examples/example-join-muted-token

**Contents:**
- Example: Join_Muted token#

The token in this example allows the user "jerky" to join the channel "sip:confctl-g-blindmelon.testchannel\@tla.vivox.com" in a muted state.

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

**Examples:**

Example 1 (unknown):
```unknown
"sip:confctl-g-blindmelon.testchannel\@tla.vivox.com"
```

Example 2 (unknown):
```unknown
{
    "vxi":542680,
    "f":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"join_muted",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 3 (unknown):
```unknown
{
    "vxi":542680,
    "f":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"join_muted",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 4 (unknown):
```unknown
e30.eyJ2eGkiOjU0MjY4MCwiZiI6InNpcDouYmxpbmRtZWxvbi1BcHBOYW1lLWRldi5qZXJreS5AdGxhLnZpdm94LmNvbSIsImlzcyI6ImJsaW5kbWVsb24tQXBwTmFtZS1kZXYiLCJ2eGEiOiJqb2luX211dGVkIiwidCI6InNpcDpjb25mY3RsLWctYmxpbmRtZWxvbi1BcHBOYW1lLWRldi50ZXN0Y2hhbm5lbEB0bGEudml2b3guY29tIiwiZXhwIjoxNjAwMzQ5NDAwfQ.N6sZL3F3e-p2KLQlMweXnbGNzE7Qc91rn_uqCEtRjsc
```

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/vivox-unity-developer-guide-toc

---

## Message metadata

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/messaging/message-metadata

**Contents:**
- Message metadata#

Send and receive additional information between players by attaching metadata to Vivox messages. This metadata can enhance player communication and support gameplay features..

Send information such as display names, a player’s local mute state, game data and more by adding metadata to messages. The metadata doesn't alter the original message content but provides context or instructions to the recipient or the game logic. Attach the metadata by using MessageOptions.Metadata; once received, retrieve the metadata with VivoxMessage.Metadata.

Note: Vivox is unable to send empty messages. To attach metadata, you must send a non-empty text message.

The following code sample is an example of sending a message with JSON metadata that communicates the user’s local mute state:

The following code sample is an example of parsing a message with JSON metadata:

Note: The size limit for attached metadata is 65,536 bytes (64 KiB).

**Examples:**

Example 1 (unknown):
```unknown
MessageOptions.Metadata
```

Example 2 (unknown):
```unknown
VivoxMessage.Metadata
```

Example 3 (unknown):
```unknown
using UnityEngine;
using Unity.Services.Vivox;

[System.Serializable]
public class MetadataInfo
{
    public string Type;
}

[System.Serializable]
public class PlayerMuteInfoMetadata
{
    public string Type = "PlayerMuteInfo";
    public string PlayerId;
    public bool LocallyMuted;
}

public async void MutePlayerLocally(VivoxParticipant participant)
{
    // ... (player mute logic)


    // If it was a self-mute, the local user's input device would be muted.
    if (participant.IsSelf)
    {
        // Send a message with metadata to the channel communicating the local player's updated input device mute state to other participants.
        var messageOptions = new MessageOptions
        {
            Metadata = JsonUtility.ToJson(new PlayerMuteInfoMetadata
            {
                PlayerId = VivoxService.Instance.SignedInPlayerId,
                LocallyMuted = VivoxService.Instance.IsInputDeviceMuted
            })
        };
        await VivoxService.Instance.SendChannelTextMessageAsync(participant.ChannelName, "Metadata", messageOptions);
    }
}
```

Example 4 (unknown):
```unknown
using UnityEngine;
using Unity.Services.Vivox;

[System.Serializable]
public class MetadataInfo
{
    public string Type;
}

[System.Serializable]
public class PlayerMuteInfoMetadata
{
    public string Type = "PlayerMuteInfo";
    public string PlayerId;
    public bool LocallyMuted;
}

public async void MutePlayerLocally(VivoxParticipant participant)
{
    // ... (player mute logic)


    // If it was a self-mute, the local user's input device would be muted.
    if (participant.IsSelf)
    {
        // Send a message with metadata to the channel communicating the local player's updated input device mute state to other participants.
        var messageOptions = new MessageOptions
        {
            Metadata = JsonUtility.ToJson(new PlayerMuteInfoMetadata
            {
                PlayerId = VivoxService.Instance.SignedInPlayerId,
                LocallyMuted = VivoxService.Instance.IsInputDeviceMuted
            })
        };
        await VivoxService.Instance.SendChannelTextMessageAsync(participant.ChannelName, "Metadata", messageOptions);
    }
}
```

---

## Example: Python

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/access-token-guide/generate-token-secure-server/example-python

**Contents:**
- Example: Python#

The following code is an example of how to generate access token in Python.

**Examples:**

Example 1 (python):
```python
#!/usr/env/python

#############################################
# Vivox Token Generation Example - Python
#############################################

import base64
import hashlib
import hmac
import json


def b64url(value):
    """Return a base64url-encoded str without trailing ='s"""
    return base64.urlsafe_b64encode(value).rstrip('=')


def vx_generate_token(key, issuer, exp, vxa, vxi, f, t):
    # Create dictionary of claims
    claims = {
        'iss': issuer,
        'exp': exp,
        'vxa': vxa,
        'vxi': vxi,
        'f': f,
        't': t
    }

    # Header is static - base64url encoded {}
    header = b64url('{}')  # Can also be defined as a constant "e30"

    # Encode claims payload
    json_payload = b64url(json.dumps(claims))

    # Join segments to prepare for signing
    segments = [header, json_payload]
    to_sign = b'.'.join(segments)

    # Sign token with key and HMACSHA256
    sig = hmac.new(key, to_sign, hashlib.sha256).digest()
    segments.append(b64url(sig))

    # Join all 3 parts of token with . and return
    return b'.'.join(segments)


if __name__ == '__main__':
    # Example usage
    token = vx_generate_token('secret', 'issuer', 1559359105, 'join', 123456, 'sip:.username.@domain.vivox.com', 'sip:confctl-g-channelname@domain.vivox.com')
    print(token)
```

Example 2 (python):
```python
#!/usr/env/python

#############################################
# Vivox Token Generation Example - Python
#############################################

import base64
import hashlib
import hmac
import json


def b64url(value):
    """Return a base64url-encoded str without trailing ='s"""
    return base64.urlsafe_b64encode(value).rstrip('=')


def vx_generate_token(key, issuer, exp, vxa, vxi, f, t):
    # Create dictionary of claims
    claims = {
        'iss': issuer,
        'exp': exp,
        'vxa': vxa,
        'vxi': vxi,
        'f': f,
        't': t
    }

    # Header is static - base64url encoded {}
    header = b64url('{}')  # Can also be defined as a constant "e30"

    # Encode claims payload
    json_payload = b64url(json.dumps(claims))

    # Join segments to prepare for signing
    segments = [header, json_payload]
    to_sign = b'.'.join(segments)

    # Sign token with key and HMACSHA256
    sig = hmac.new(key, to_sign, hashlib.sha256).digest()
    segments.append(b64url(sig))

    # Join all 3 parts of token with . and return
    return b'.'.join(segments)


if __name__ == '__main__':
    # Example usage
    token = vx_generate_token('secret', 'issuer', 1559359105, 'join', 123456, 'sip:.username.@domain.vivox.com', 'sip:confctl-g-channelname@domain.vivox.com')
    print(token)
```

---

## Text chat developer guide

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/text-chat-guide-overview

**Contents:**
- Text chat developer guide#

Vivox provides a text chat service:

Text chat - channel-based and directed text chat.

The following additional features can be enabled:

The server can customize a maximum byte count for text messages in its pre-login configuration. By default, the maximum is 320 bytes with UTF-8 encoding. A text message that exceeds the maximum length in bytes is rejected with a VxErrorMessageTextTooLong error.

Text chat requires a Unicode-supported system to work properly. The Unity Editor has full Unicode support in versions 2017.1 and later.

The following table details text chat policy:

1000 bytes per second

The maximum rate at which a player can send data over text chat.

This is configured server-side by Vivox.

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/authentication

---

## Retrieve Conversations list

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/text-chat-guide/retrieve-conversations-list

**Contents:**
- Retrieve Conversations list#

You can retrieve a user’s list of conversations. A conversation is a channel where a user has sent a message or a direct message (DM). You can use this API to populate a list of channels or DMs for a user to find their active chats.

Note: The returned Conversation list is ranked by most recent activity. Conversations with newer messages show up first in the results.

To retrieve the conversation list, sign in to the application and then use await VivoxService.Instance.GetConversationsAsync(); to make a request. This method returns a collection of VivoxConversation objects, each representing either a DM or a channel conversation.

The following code is an example of how to retrieve a user’s conversation list.

After you retrieve the user’s conversation list, you can parse the resulting collection of VivoxConversation to determine which are DMs and which are channel conversations:

Note: Depending on the VivoxConversation.ConversationType of a returned VivoxConversation, the VivoxConversation.Name will either be the PlayerId of another user, or the unique channel name of a channel. In both cases, the VivoxConversation.Name is a unique identifier for one of the two types of conversations.

**Examples:**

Example 1 (unknown):
```unknown
await VivoxService.Instance.GetConversationsAsync();
```

Example 2 (unknown):
```unknown
VivoxConversation
```

Example 3 (unknown):
```unknown
// The use of `ConversationQueryOptions` is optional and the method can be called without providing it.
var options = new ConversationQueryOptions()
{
    CutoffTime = DateTime.Now, // Point in time to fetch conversations from. Conversations joined after this timestamp will not be present in the query results. This timestamp is server side and gets converted to UTC internally.
    PageCursor = 1, // Parameter if you’re fetching anything but the first page.
    PageSize = 10 // The number of results returned per page. The default is 10.
};
var conversations = await VivoxService.Instance.GetConversationsAsync(options);
```

Example 4 (unknown):
```unknown
// The use of `ConversationQueryOptions` is optional and the method can be called without providing it.
var options = new ConversationQueryOptions()
{
    CutoffTime = DateTime.Now, // Point in time to fetch conversations from. Conversations joined after this timestamp will not be present in the query results. This timestamp is server side and gets converted to UTC internally.
    PageCursor = 1, // Parameter if you’re fetching anything but the first page.
    PageSize = 10 // The number of results returned per page. The default is 10.
};
var conversations = await VivoxService.Instance.GetConversationsAsync(options);
```

---

## Chat history

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/text-chat-guide/chat-history

**Contents:**
- Chat history#
- Channel text message history#

Vivox allows for users to access the history of a channel's text activity using VivoxService.Instance.GetChannelTextMessageHistoryAsync(string channelName, int requestSize = 10, ChatHistoryQueryOptions chatHistoryQueryOptions = null), with channelName being the name of the channel to request the history of, requestSize being the number of messages to return at a maximum (10 is the default), and chatHistoryQueryOptions being an optional parameter which can do things like specifying a word or phrase contained in the messages, a specific PlayerId sender of the messages, or times to query before or after. More specific information on ChatHistoryQueryOptions can be found in Chat history query options.

The messages returned in IReadOnlyCollection are listed in order from newest message to oldest message.

Note: Chat history is stored for 7 days. If Text Evidence Management is in use, the storage period for chat history is extended to 30 days.

The following code snippet is an example of pulling at most recent 25 messages from a set LobbyChannelName, then logging the sender's display name and the message in order from oldest to newest, without using the optional ChatHistoryQueryOptions.

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.GetChannelTextMessageHistoryAsync(string channelName, int requestSize = 10, ChatHistoryQueryOptions chatHistoryQueryOptions = null)
```

Example 2 (unknown):
```unknown
channelName
```

Example 3 (unknown):
```unknown
requestSize
```

Example 4 (unknown):
```unknown
chatHistoryQueryOptions
```

---

## Feature checkpoints

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/integration-guide/integration-plan/phase-three/feature-checkpoints

**Contents:**
- Feature checkpoints#

Verify that the following features work for each use case.

When you enter a channel map, the location of the user is correct

The default location matches the virtual location

If a location is specified, it matches the virtual location

When users are co-located, you can hear and be heard by others

Moving out of audio range stops you from hearing and being heard by others

Moving back into audio range causes you to hear and be heard by others

The audio space mode matches the virtual world space. For example:

The UI correctly identifies the following:

All channels currently joined by the user

When channels are added or removed

Any advanced features that are used:

For both capture (microphone) and render (loudspeaker) devices:

When choosing a device:

---

## Run builds manually

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/run-builds/run-builds-manually

**Contents:**
- Run builds manually#
- Run individual builds#
  - Run builds from the Configurations page#
  - Run builds from the Build history page#
- Run builds for a build target group#
  - Run builds for build target groups from the Configurations page#
  - Run builds for build target groups from the Build history page#

Build Automation allows you to manually trigger builds for individual configurations or groups of build targets.

There are two ways to manually trigger a build for an individual build configuration:

If you have set up build target groups, you can trigger builds for these groups in a similar way to individual builds. Running builds for a group allows you to build multiple configurations simultaneously, streamlining the process.

---

## Android speakerphone audio: Mono or stereo

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/android/unreal-android-speakerphone-audio

**Contents:**
- Android speakerphone audio: Mono or stereo#

On Android, Vivox downmixes stereo output into mono output when it detects that the speakerphone is in use. Vivox downmixes the audio output because many Android devices play mono in speakerphone mode. Some devices only play one channel when provided with stereo audio, which makes the panning of 3D channels detrimental.

You can control whether speakerphone downmixing occurs for all devices by providing a list of exceptions by listing which brands, models, and devices to exclude. To change the default speakerphone downmix behavior, add an Android meta-data name-value pair to your application's AndroidManifest.xml. You can learn more about Android meta-data tags in the official Android documentation (Android Developer Documentation).

The metadata name to use is:

"com.vivox.sdk.downmix_speakerphone_enabled"

Note: There is a default exception that disables speakerphone downmix for Meta/Oculus devices. Changing this value overwrites this default exception.

The value must be a string starting with either true or false. To add a list of exceptions, add a comma after true or false and start a comma-delimited list of substrings to search for in the string generated for the current device, formatted as "BRAND MODEL DEVICE". The value string is case-insensitive.

The following are examples of valid value strings:

Note: Replace the example brands, models, and devices with values retrieved by android.os.Build.* strings.

The following is an example of disabling downmixing on Samsung devices:

To modify your application's AndroidManifest.xml, refer to the Unreal Engine documentation on Android Manifest control or Unreal Plugin Language.

**Examples:**

Example 1 (unknown):
```unknown
"com.vivox.sdk.downmix_speakerphone_enabled"
```

Example 2 (unknown):
```unknown
"true,brand,brand_two model,model_two"
```

Example 3 (unknown):
```unknown
"false,model_three device,device_two"
```

Example 4 (unknown):
```unknown
“true,samsung"
```

---

## Channel-based message delete

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/chat-history/edit-delete/channel-message-delete

**Contents:**
- Channel-based message delete#

To delete a channel-based message, you need to sign in and join at least one channel. An existing message must have been sent to every participant in the channel.

Note: Only the sender of a message can delete it. Use vx_req_session_delete_message to make a delete request.

vx_req_session_delete_message will return:

You will receive a vx_evt_session_delete_message event for every message deleted.

The following code is an example of how to delete a message for a specific channel:

After you delete this message, the game must then process the vx_evt_session_delete_message event.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_session_delete_message
```

Example 2 (unknown):
```unknown
vx_req_session_delete_message
```

Example 3 (unknown):
```unknown
vx_evt_session_delete_message
```

Example 4 (unknown):
```unknown
vx_req_session_delete_message *req;
vx_req_session_delete_message_create(&req);
req->session_handle = vx_strdup("channelName");
req->message_id = vx_strdup("oldMessageID");
vx_issue_request2(&req->base);
```

---

## XMPP representation - Delete a message in a channel

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/chat-history/edit-delete/xmpp-delete-in-channel

**Contents:**
- XMPP representation - Delete a message in a channel#

Example delete request:

Example delete response:

Deleted event notification is then propagated back to every participant in the channel.

Example of propagating a deleted message event:

**Examples:**

Example 1 (unknown):
```unknown
<iq type='set' id='...' from='...' to='...'>
   <query xmlns='urn:xmpp:mam:4'>
       <x xmlns='jabber:x:data' type='submit'>
           <field var='FORM_TYPE' type='hidden'>
               <value>urn:xmpp:mam:4</value>
           </field>
           <field var='message-id'>
               <value>...</value>
           </field>
       </x>
   </query>
</iq>
```

Example 2 (unknown):
```unknown
<iq type='set' id='...' from='...' to='...'>
   <query xmlns='urn:xmpp:mam:4'>
       <x xmlns='jabber:x:data' type='submit'>
           <field var='FORM_TYPE' type='hidden'>
               <value>urn:xmpp:mam:4</value>
           </field>
           <field var='message-id'>
               <value>...</value>
           </field>
       </x>
   </query>
</iq>
```

Example 3 (unknown):
```unknown
<iq from="someRoomSIP@cats.vivox.com"
    id="..."
    to="alice@cats.vivox.com"
    type="result">
    <message-deleted xmlns="urn:vivox:message-deleted" id="..." />
</iq>
```

Example 4 (unknown):
```unknown
<iq from="someRoomSIP@cats.vivox.com"
    id="..."
    to="alice@cats.vivox.com"
    type="result">
    <message-deleted xmlns="urn:vivox:message-deleted" id="..." />
</iq>
```

---

## Disable the Vivox SDK internal AEC

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/android/disable-internal-aec

**Contents:**
- Disable the Vivox SDK internal AEC#

It is possible to disable the Vivox SDK internal acoustic echo cancellation (AEC), which can save processing power and battery.

Caution: Only disable the Vivox SDK internal AEC in circumstances where you are certain AEC is not needed, such as when headphones are plugged in or when platform AEC is enabled.

To disable the Vivox SDK internal AEC, invoke the vx_set_vivox_aec_enabled(0) call.

**Examples:**

Example 1 (unknown):
```unknown
vx_set_vivox_aec_enabled(0)
```

---

## Filter a channel by speaker

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/server-side-recording/ssr-filter-channel-by-speaker

**Contents:**
- Filter a channel by speaker#

To subscribe to a channel and only record certain users, filter a channel subscription by passing a list of speakers in to the channel you want to subscribe to.

For example, if you have speakerA, speakerB, and speakerC in a channel, and you only want data from speakerA and speakerB, then you can filter those two users by passing them in to the filter_uris parameter, which excludes speakerC from the recording. The following code displays an example of how to filter for this scenario.

**Examples:**

Example 1 (unknown):
```unknown
curl -x POST
http://{{host}}/ssr/subscribe?auth_token={{auth_token}}&history=5&destination_credentials={"bucket":"bucket-name","access_key_id":"aws-access-id","secret_access_key":"aws-secret-key"}&target_uri=sip:confctl-g-.issuer.channel_name@domain.vivox.com&target_type=channel&ttl=5&filter_uris=["sip:.issuer.speakerA.@domain.vivox.com", "sip:.issuer.speakerB.@domain.vivox.com"]
```

Example 2 (unknown):
```unknown
curl -x POST
http://{{host}}/ssr/subscribe?auth_token={{auth_token}}&history=5&destination_credentials={"bucket":"bucket-name","access_key_id":"aws-access-id","secret_access_key":"aws-secret-key"}&target_uri=sip:confctl-g-.issuer.channel_name@domain.vivox.com&target_type=channel&ttl=5&filter_uris=["sip:.issuer.speakerA.@domain.vivox.com", "sip:.issuer.speakerB.@domain.vivox.com"]
```

---

## Unity Editor Audio Tap components

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/audio-taps/audio-tap-components

**Contents:**
- Unity Editor Audio Tap components#

The Vivox Unity SDK allows finer Vivox Voice Chat audio integration within your game through Audio Tap components. Audio Taps use Unity Audio Sources to make the Voice Chat audio accessible within the Unity Engine’s audio system. Different Audio Tap components are available through the Vivox Unity package to achieve your use cases at different steps of the Vivox audio chain:

---

## Game engine best practices

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/guides/game-engine-best-practices

**Contents:**
- Game engine best practices#
- Set target frame rate in Unity#

Use the following guidance to get the most out of Multiplay Hosting when working with specific game engines.

To avoid causing your server to use 100% of your CPU, the recommended best practice is to set the target frame rate for a Unity server.

To set the target frame rate, add the following code into your server build:

**Examples:**

Example 1 (unknown):
```unknown
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class TargetFPS : MonoBehaviour
{
    public int target = 60;

    void Awake()
    {
        QualitySettings.vSyncCount = 0;
        Application.targetFrameRate = target;
    }
}
```

Example 2 (unknown):
```unknown
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class TargetFPS : MonoBehaviour
{
    public int target = 60;

    void Awake()
    {
        QualitySettings.vSyncCount = 0;
        Application.targetFrameRate = target;
    }
}
```

---

## Mute a local microphone device

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/muting/mute-local-microphone

**Contents:**
- Mute a local microphone device#

To mute or unmute a local microphone device, use the vx_req_connector_mute_local_mic request.

The account_handle in this request is an optional parameter, and is similar to what is used when muting a local speaker device. This parameter specifies the account handle of the user whose microphone is to be muted or unmuted, which is the account handle that is passed in to a vx_req_account_anonymous_login. If this is left unset, then the default account handle is used. Generally, there is only one account handle, except for couch co-op.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_connector_mute_local_mic
```

Example 2 (unknown):
```unknown
vx_req_connector_mute_local_mic_t* reqStruct;
vx_req_connector_mute_local_mic_create(&reqStruct);
reqStruct->connector_handle = vx_strdup(“connector handle”);
reqStruct->mute_level = 1; // 1 - mute, 0 - unmute
reqStruct->account_handle = vx_strdup(“account handle”); // OPTIONAL

int ret = vx_issue_request3((vx_req_base_t*) reqStruct);
```

Example 3 (unknown):
```unknown
vx_req_connector_mute_local_mic_t* reqStruct;
vx_req_connector_mute_local_mic_create(&reqStruct);
reqStruct->connector_handle = vx_strdup(“connector handle”);
reqStruct->mute_level = 1; // 1 - mute, 0 - unmute
reqStruct->account_handle = vx_strdup(“account handle”); // OPTIONAL

int ret = vx_issue_request3((vx_req_base_t*) reqStruct);
```

Example 4 (unknown):
```unknown
account_handle
```

---

## AVAudioPlayer volume inconsistencies

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/ios/avaudioplayer

**Contents:**
- AVAudioPlayer volume inconsistencies#

If your app uses AVAudioPlayer, you must call prepareToPlay() before the Vivox SDK uses the microphone. If prepareToPlay() is called while the Vivox SDK is using the microphone, the audio for that instance of the AVAudioPlayer will be at a lower volume. For more information, refer to the Apple developer documentation on prepareToPlay().

This behavior can also occur when recovering from an audio interruption. For a workaround, see Restore audio volume.

**Examples:**

Example 1 (unknown):
```unknown
prepareToPlay()
```

---

## Muting

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/muting/muting-overview

**Contents:**
- Muting#

There are two types of muting in the Vivox SDK: muting a user’s microphone, and muting other users for a user.

You can also allow a user to prevent themselves from being heard by or hearing a specific user by implementing block functionality.

---

## Error handling

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/error-handling

**Contents:**
- Error handling#
- Error codes#
- Error wrapping#
- Error handling with batch requests#
- Status codes#
- Unhandled errors#
- Logging#

Learn how to handle errors in your modules.

Cloud Code returns a 422 status error if your module throws an exception.

Use the error code or error type for your error-handling logic, instead of the error message returned from the Cloud Code API error response. Error messages can change in the future, but error codes and type remain the same.

Wrap your code in try/catch blocks to handle errors. If an error occurs, you can log it and return an error to the client.

For example, you can have a non-wrapped function:

The non-wrapped function returns a generic error:

If you add the try/catch blocks to handle errors, you can return a more specific error message. You can also log the error:

The wrapped function returns an error message with more detail:

You can check the logs in the Unity Dashboard. To get more details about the error, select Products > Cloud Code > Logs.

For more information about batch requests, refer to Batch requests.

Every time a Cloud Code C# SDK returns an unsuccessful status code, the SDK throws an ApiException exception. You can use this exception to get the status code and error message.

You can catch the ApiException exception and handle it, or log and rethrow the error. This means you don't have to check for the status code in the ApiResponse object.

Cloud Code can't handle exceptions that result from memory leaks, such as infinite loops. If your module exceeds the memory limit, the worker eventually crashes. Cloud Code can't handle exceptions for async void patterns. Always return a Task or Task<T>. For best practices in asynchronous programming, refer to the official Microsoft documentation.

To help you debug your modules, you can log errors and warnings with the ILogger interface.

Cloud Code automatically adds attributes like the playerId, environmentId and projectId to the logs, so you don't need to add them to your log messages.

You can log messages at different levels. These levels can help you query logs in the Unity Dashboard.

To view and filter your logs, navigate to the Unity Dashboard and select Products > Cloud Code > Logs.

For more information, refer to Emit logs.

**Examples:**

Example 1 (unknown):
```unknown
[CloudCodeFunction("IncrementBalance")]
    public async  Task<ApiResponse<CurrencyBalanceResponse>> IncrementPlayerBalance(IGameApiClient gameApiClient, IExecutionContext ctx, string currencyId)
    {
        return await gameApiClient.EconomyCurrencies.IncrementPlayerCurrencyBalanceAsync(ctx, ctx.ServiceToken,  ctx.ProjectId, ctx.PlayerId, currencyId, new CurrencyModifyBalanceRequest(currencyId, 10));
    }
```

Example 2 (unknown):
```unknown
[CloudCodeFunction("IncrementBalance")]
    public async  Task<ApiResponse<CurrencyBalanceResponse>> IncrementPlayerBalance(IGameApiClient gameApiClient, IExecutionContext ctx, string currencyId)
    {
        return await gameApiClient.EconomyCurrencies.IncrementPlayerCurrencyBalanceAsync(ctx, ctx.ServiceToken,  ctx.ProjectId, ctx.PlayerId, currencyId, new CurrencyModifyBalanceRequest(currencyId, 10));
    }
```

Example 3 (unknown):
```unknown
{
  "type": "problems/invocation",
  "title": "Unprocessable Entity",
  "status": 422,
  "detail": "Invocation Error",
  "instance": null,
  "code": 9009,
  "details": [
    {
      "message": "Error executing Cloud Code function. Exception type: ApiException. Message: Bad Request",
      "name": "ScriptRunner.Exceptions.CloudCodeRuntimeException",
      "stackTrace": [
        "at Unity.Services.CloudCode.Shared.HttpApiClient.ToApiResponse[T](HttpResponseMessage response)",
        "at Unity.Services.CloudCode.Shared.HttpApiClient.SendAsync[T](String path, HttpMethod method, ApiRequestOptions options, IApiConfiguration configuration, CancellationToken cancellationToken)",
        "at Unity.Services.Economy.Api.EconomyCurrenciesApi.IncrementPlayerCurrencyBalanceAsync(IExecutionContext executionContext, String accessToken, String projectId, String playerId, String currencyId, CurrencyModifyBalanceRequest currencyModifyBalanceRequest, String configAssignmentHash, String unityInstallationId, String analyticsUserId, CancellationToken cancellationToken)",
        "at Sample.HelloWorld.IncrementBalanceUnhandled(IGameApiClient gameApiClient, IExecutionContext ctx, String currencyId) in Sample/Main/HelloWorld.cs:line 148"
      ]
    }
  ]
}
```

Example 4 (unknown):
```unknown
{
  "type": "problems/invocation",
  "title": "Unprocessable Entity",
  "status": 422,
  "detail": "Invocation Error",
  "instance": null,
  "code": 9009,
  "details": [
    {
      "message": "Error executing Cloud Code function. Exception type: ApiException. Message: Bad Request",
      "name": "ScriptRunner.Exceptions.CloudCodeRuntimeException",
      "stackTrace": [
        "at Unity.Services.CloudCode.Shared.HttpApiClient.ToApiResponse[T](HttpResponseMessage response)",
        "at Unity.Services.CloudCode.Shared.HttpApiClient.SendAsync[T](String path, HttpMethod method, ApiRequestOptions options, IApiConfiguration configuration, CancellationToken cancellationToken)",
        "at Unity.Services.Economy.Api.EconomyCurrenciesApi.IncrementPlayerCurrencyBalanceAsync(IExecutionContext executionContext, String accessToken, String projectId, String playerId, String currencyId, CurrencyModifyBalanceRequest currencyModifyBalanceRequest, String configAssignmentHash, String unityInstallationId, String analyticsUserId, CancellationToken cancellationToken)",
        "at Sample.HelloWorld.IncrementBalanceUnhandled(IGameApiClient gameApiClient, IExecutionContext ctx, String currencyId) in Sample/Main/HelloWorld.cs:line 148"
      ]
    }
  ]
}
```

---

## Example: Login token

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/access-token-examples/example-login-token

**Contents:**
- Example: Login token#

The token in this example allows the user "jerky" to sign in to the server.

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

**Examples:**

Example 1 (unknown):
```unknown
{
   "iss":"blindmelon-AppName-dev",
   "vxi":933000,
   "vxa":"login",
   "exp":1600349400,
   "f":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com"
}
```

Example 2 (unknown):
```unknown
{
   "iss":"blindmelon-AppName-dev",
   "vxi":933000,
   "vxa":"login",
   "exp":1600349400,
   "f":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com"
}
```

Example 3 (unknown):
```unknown
e30.eyJpc3MiOiJibGluZG1lbG9uLUFwcE5hbWUtZGV2IiwidnhhIjoibG9naW4iLCJ2eGkiOjkzMzAwMCwiZXhwIjoxNjAwMzQ5NDAwLCJmIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2Lmplcmt5LkB0bGEudml2b3guY29tIn0.5FKsAQz7bRQGGlSZw-KIcCmft7Ic6nT3Ih-TbdYPWwI
```

Example 4 (unknown):
```unknown
e30.eyJpc3MiOiJibGluZG1lbG9uLUFwcE5hbWUtZGV2IiwidnhhIjoibG9naW4iLCJ2eGkiOjkzMzAwMCwiZXhwIjoxNjAwMzQ5NDAwLCJmIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2Lmplcmt5LkB0bGEudml2b3guY29tIn0.5FKsAQz7bRQGGlSZw-KIcCmft7Ic6nT3Ih-TbdYPWwI
```

---

## Use text-to-speech for incoming messages

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/text-to-speech/tts-incoming-messages

**Contents:**
- Use text-to-speech for incoming messages#

To use text-to-speech (TTS) to read incoming messages, use the Speak() function in the message event handler. We recommend that you use the TTSDestination::QueuedLocalPlayback destination because it ensures that messages are queued and that they only play locally.

The following code is an example of how to use TTS for channel messages:

Tip: To only read text messages from other users in a channel, compare the current user’s AccountId with the IChannelTextMessage::Sender() value.

**Examples:**

Example 1 (unknown):
```unknown
TTSDestination::QueuedLocalPlayback
```

Example 2 (unknown):
```unknown
IChannelSession::EventTextMessageReceived
```

Example 3 (unknown):
```unknown
ILoginSession::EventDirectedTextMessageReceived
```

Example 4 (javascript):
```javascript
MyChannelSession->EventTextMessageReceived.AddLambda([MyLoginSession](const IChannelTextMessage &Message)
{
    // If the text message received is not from me...
    if (Message.Sender() != MyLoginSession->LoginSessionId())
    {
        // Check if optional display name is set.
        FString DisplayName = Message.Sender().DisplayName();
        if (DisplayName.IsEmpty())
            { DisplayName = "Anonymous"; }

        // This queues the message and lets the user know who sent it.
        // NB: the colon (:) inserts a slight pause before subsequent text.
        FString Text = DisplayName + " says: " + Message.Message();
        MyLoginSession->TTS().Speak(Text, TTSDestination::QueuedLocalPlayback);
    }
});
```

---

## Monitor for configuration variable changes

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/guides/monitor-configuration-variables

**Contents:**
- Monitor for configuration variable changes#
- Monitor allocation ID changes#
- Container builds#
- Best practices#
  - Unity Engine#
  - Unreal Engine#
  - Custom engine#

Each game server build must monitor the server.json file for configuration variable changes. For example, when you allocate a server instance, the IP address, port number, and game mode might contain different values from the previous allocation.

Each game server instance must detect its allocation ID by monitoring the value of the allocatedID configuration variable in the server.json file. The allocatedID variable value changes each time you allocate or deallocate the server instance. Game server builds also monitor the server.json file to detect its allocation status and its allocation ID.

Note: The server start and server stop actions don't impact the allocation status of the server instance or the allocation ID.

If you're using a container build, the server.json file is already mounted into your container. You can find the server.json file location by checking the container's home environment variable.

The recommended way monitor changes to the server.json file depends on the game engine your game is built on.

If your game is built using Unity, the recommended way to monitor for configuration variable changes is to use the Game Server SDK.

If your game is built using Unreal Engine, the recommended way to monitor for configuration variable changes is to use the Game Server SDK.

If you’re using a custom game engine, the recommended way is to use inotify to detect CLOSE_WRITE events. For an example of what a CLOSE_WRITE event looks like, refer to the following inotify log sample.

**Examples:**

Example 1 (unknown):
```unknown
server.json
```

Example 2 (unknown):
```unknown
allocatedID
```

Example 3 (unknown):
```unknown
server.json
```

Example 4 (unknown):
```unknown
allocatedID
```

---

## The iOS microphone recording indicator

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/ios/ios-recording-indicator

**Contents:**
- The iOS microphone recording indicator#

iOS displays the red bar/orange dot recording indicator while the microphone is in use. When an iOS device is muted by using vx_req_connector_mute_local_mic, the Vivox SDK accesses the capture device (microphone) but does not collect or transmit any audio. This results in the recording indicator displaying.

To hard mute and prevent the recording indicator from displaying, set the capture device to No Device:

To unmute, set the capture device to Default Communication Device:

Note: Hard muting or unmuting causes a brief interruption to all audio that is playing from the device.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_connector_mute_local_mic
```

Example 2 (unknown):
```unknown
int request_count;
vx_req_aux_set_capture_device_t *req;
vx_req_aux_set_capture_device_create(&req);
req->capture_device_specifier = vx_strdup("No Device");
vx_issue_request3(&req->base, &request_count);
```

Example 3 (unknown):
```unknown
int request_count;
vx_req_aux_set_capture_device_t *req;
vx_req_aux_set_capture_device_create(&req);
req->capture_device_specifier = vx_strdup("No Device");
vx_issue_request3(&req->base, &request_count);
```

Example 4 (unknown):
```unknown
int request_count;
vx_req_aux_set_capture_device_t *req;
vx_req_aux_set_capture_device_create(&req);
req->capture_device_specifier = vx_strdup("Default Communication Device");
vx_issue_request3(&req->base, &request_count);
```

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/analytics/manual/acquisition-source-event

---

## Sign a UWP application

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/sign-build-artifacts/sign-a-uwp-application

**Contents:**
- Sign a UWP application#
- Prerequisites#
- Configure Unity Build Automation signing#
- Troubleshoot UWP signing#

Use Unity Build Automation to sign your Universal Windows Platform (UWP) application during the build process. This ensures your UWP application meets the requirements for installation on Windows devices or deployment to the Microsoft Store.

UWP applications require digital signing with a certificate before they can be installed on devices or published to the Microsoft Store. This certificate verifies the app's publisher and establishes trust for users installing your application.

Note: If you plan to publish your application to the Microsoft Store, you need to register for a Microsoft Partner Center developer account.

To associate your signing credentials with your UWP build target:

If you encounter issues with UWP signing, check the following solutions:

For additional help with UWP application signing, refer to the Microsoft documentation on app signing.

---

## Run builds automatically

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/run-builds/run-builds-automatically

**Contents:**
- Run builds automatically#
- Set up Automatic Builds#
- Set Scheduled Builds#
- Run Builds Using Build Automation API#

In Build Automation, you can configure your project to build automatically when you push changes to your repository, or you can set up recurring builds on a defined schedule. These features work independently and suit different workflows.

Auto-build automatically triggers a new build when you commit changes to your repository. This ensures your project is consistently built with the latest updates:

Auto-build is perfect for workflows that require continuous integration, because it ensures a build triggers for the latest changes without manual intervention.

Scheduled builds allow you to automatically start builds at defined intervals, regardless of repository activity. Schedules builds help to ensure that you have regular builds, such as nightly builds or scheduled testing.

When you configure a scheduled build, you can specify the following details:

Scheduled builds are ideal to maintain regular build cycles and ensure you have a reliable build at specific times even if no new changes have been pushed.

There's no limit to the number of schedules you can create for a project, but you can only set up one schedule for each build target. To create multiple schedules for a project, you need to create additional build targets.

In addition to Auto-build and scheduled builds, you can programmatically trigger builds using the Build Automation API. This approach is ideal for workflows that require custom build triggers, such as to integrate build processes with external systems, or when you need more granular control over how and when builds trigger.

The Build Automation API allows you to do the following tasks:

For detailed API documentation and additional functionality, you can refer to the Build Automation API Documentation.

---

## Cancel a text-to-speech message

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/text-to-speech/cancel-tts-message

**Contents:**
- Cancel a text-to-speech message#

To cancel a currently playing or enqueued text-to-speech (TTS) message, use VivoxService.Instance.TextToSpeechCancelMessages() or VivoxService.Instance.TextToSpeechCancelAllMessages() with the TTS Message that you want to cancel. You can also use the Dequeue() method on any ITTS Message collection to cancel the message at the head of the queue.

You can directly cancel any TTS Message object that was previously spoken by using VivoxService.Instance.TextToSpeechCancelMessages(). This is detailed in the following example:

In destinations that contain queues, canceling an ongoing TTS message automatically triggers playback of the next message. Canceling an enqueued TTS message shifts all later messages up one place in the queue.

You can cancel all TTS messages in a destination (ongoing and enqueued), or all TTS messages in all destinations.

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.TextToSpeechCancelMessages()
```

Example 2 (unknown):
```unknown
VivoxService.Instance.TextToSpeechCancelAllMessages()
```

Example 3 (unknown):
```unknown
VivoxService.Instance.TextToSpeechCancelMessages()
```

Example 4 (unknown):
```unknown
VivoxService.Instance.TextToSpeechSendMessage()
```

---

## Handle messages

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/messaging/handle-messages

**Contents:**
- Handle messages#

There are two ways to manage SDK messages in the Vivox SDK. The best practice is to use a callback method. Message polling can be used however it is not recommended.

---

## Retrieve channel-based chat history

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/chat-history/retrieve-channel-chat-history

**Contents:**
- Retrieve channel-based chat history#

To retrieve the channel-based chat history, you need to sign in and connect to at least one channel. You can then use vx_req_session_chat_history_query to make a request.

vx_req_session_chat_history_query will return 10 messages by default. You can increase the number of messages that are returned by increasing the max value in the request. The maximum number of messages that we store per channel is 5000, with a maximum age of 7 days.

You will receive a vx_evt_account_archive_message event for every message.

The following code is an example of how to retrieve the last 20 messages for the specific channel:

After you get these messages, the game must then process two types of event messages:

**Examples:**

Example 1 (unknown):
```unknown
vx_req_session_chat_history_query
```

Example 2 (unknown):
```unknown
vx_req_session_chat_history_query
```

Example 3 (unknown):
```unknown
vx_evt_account_archive_message
```

Example 4 (unknown):
```unknown
vx_req_session_chat_history_query *req;
vx_req_session_chat_history_query_create(&req);
req->session_handle = vx_strdup("mychannel");
req->max = 20;
vx_issue_request2(&req->base);
```

---

## Select a caching strategy

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/optimize-build-speed/select-caching-strategy

**Contents:**
- Select a caching strategy#
- Types of caching#
  - Library caching#
  - Workspace caching#
- Configure caching options#
  - Configure your caching settings for a project#
  - Configure your caching settings for a Build Target Configuration:#

If you build a Unity project, it can be a complex process, with many tasks and dependencies that you need to manage. Part of this complexity is because you need to perform the build process on remote servers which might be less powerful than your local machines.

Note: Unity Build Automation has a feature for you to choose the machine specification of your builder, which changes the performance characteristics of the hardware.

Unity Build Automation needs to download the project files from your source code repository. This download can take time, depending on both the size of the project and the network connectivity between the Unity platform and your version control system. This download process is the checkout phase, and can be a significant bottleneck in your build process.

Another factor that can impact the build time is the number of dependencies and assets that you need to process. When Unity builds a project, it needs to process all the scripts, shaders, textures, and other assets used in the project. If your project has a large number of assets or if the dependencies are complex, it can take more time. To improve the build time, you can use caching, which stores some of your data in Unity Build Automation servers to it doesn't need to reprocess every time you run a build.

Unity Build Automation currently supports the following caching options:

Note: When you use either library or workspace caching, the first import of a project takes slightly longer as it builds the Library first. After the first successful build, caching reduces build time. Apple related builds will generate a separate cache for each architecture utilized.

Unity Build Automation offers a library caching feature which can significantly reduce the time it takes to process dependencies and assets. Library caching stores a cache of the dependencies on the UBA servers to save significant time and bandwidth, especially for larger projects.

The workspace caching feature means that you store all of your workspace data in Unity Build Automation servers. In most cases, after the first successful build, if you cache your entire workspace it takes less time to build than library caching, but it also uses a lot more storage.

You can configure your caching settings at two levels: for a specific project, or for a build target configuration.

You can edit the Cache compression level when you use caching. The default is Low, which compromises between speed and cost. If you want to maximize performance, select None. High compression can be useful to reduce storage usage, but it comes with a very high penalty to performance.

Note: This option overrides the project level caching settings.

---

## Example: Mute tokens

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/access-token-examples/example-mute-token

**Contents:**
- Example: Mute tokens#
- Signed in user muting other user in a channel#
- Admin user muting other user in a channel#

The token in this example allows the user "beef" to mute the user "jerky" in the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

The token in this example allows the user "blindmelon-AppName-dev-Admin" to mute the user "jerky" in the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

**Examples:**

Example 1 (unknown):
```unknown
{
    "vxi":123456,
    "sub":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "f":"sip:.blindmelon-AppName-dev.beef.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"mute",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 2 (unknown):
```unknown
{
    "vxi":123456,
    "sub":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "f":"sip:.blindmelon-AppName-dev.beef.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"mute",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 3 (unknown):
```unknown
e30.eyJ2eGkiOjEyMzQ1Niwic3ViIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2Lmplcmt5LkB0bGEudml2b3guY29tIiwiZiI6InNpcDouYmxpbmRtZWxvbi1BcHBOYW1lLWRldi5iZWVmLkB0bGEudml2b3guY29tIiwiaXNzIjoiYmxpbmRtZWxvbi1BcHBOYW1lLWRldiIsInZ4YSI6Im11dGUiLCJ0Ijoic2lwOmNvbmZjdGwtZy1ibGluZG1lbG9uLUFwcE5hbWUtZGV2LnRlc3RjaGFubmVsQHRsYS52aXZveC5jb20iLCJleHAiOjE2MDAzNDk0MDB9.vM9zkCXTORjgv8w7eiMHHHkc4DumTwR_-I06y4SnpHA
```

Example 4 (unknown):
```unknown
e30.eyJ2eGkiOjEyMzQ1Niwic3ViIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2Lmplcmt5LkB0bGEudml2b3guY29tIiwiZiI6InNpcDouYmxpbmRtZWxvbi1BcHBOYW1lLWRldi5iZWVmLkB0bGEudml2b3guY29tIiwiaXNzIjoiYmxpbmRtZWxvbi1BcHBOYW1lLWRldiIsInZ4YSI6Im11dGUiLCJ0Ijoic2lwOmNvbmZjdGwtZy1ibGluZG1lbG9uLUFwcE5hbWUtZGV2LnRlc3RjaGFubmVsQHRsYS52aXZveC5jb20iLCJleHAiOjE2MDAzNDk0MDB9.vM9zkCXTORjgv8w7eiMHHHkc4DumTwR_-I06y4SnpHA
```

---

## Integrate with CI/CD

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/automation

**Contents:**
- Integrate with CI/CD#
- Additional resources#

You can use the Unity Gaming Services command-line tool and the Cloud Code CLI commands to automate your CI/CD pipeline.

For examples of how you can integrate the Unity CLI into your CI/CD pipeline, refer to the samples below:

For more information on how to use the Unity CLI, refer to the documentation on how to write modules using the CLI.

---

## Mute other users

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/muting/mute-other-users

**Contents:**
- Mute other users#

Vivox Unreal provides the following functions to mute another user:

Use BeginSetLocalMute to mute or unmute a participant for the currently connected user.

Use BeginSetIsMutedForAll to send a request to the Vivox server to mute the participant for all participants in the channel. This requires an access token that you can produce with IParticipantProperties::GetMuteForAllToken().For more information, refer to the Access Token Developer Guide.

**Examples:**

Example 1 (unknown):
```unknown
IParticipantProperties::BeginSetLocalMute(bool setMuted, FOnBeginSetLocalMuteCompletedDelegate theDelegate = FOnBeginSetLocalMuteCompletedDelegate())
```

Example 2 (javascript):
```javascript
IParticipantProperties::BeginSetIsMutedForAll(bool setMuted, const FString& accessToken, FOnBeginSetIsMutedForAllCompletedDelegate theDelegate = FOnBeginSetIsMutedForAllCompletedDelegate())
```

Example 3 (unknown):
```unknown
BeginSetLocalMute
```

Example 4 (unknown):
```unknown
void MuteParticipantForMe(IParticipant &participantToMute)
{
    IParticipant::FOnBeginSetLocalMuteCompletedDelegate BeginSetLocalMuteCompletedCallback;
    bool IsMuted = false;
    BeginSetLocalMuteCompletedCallback.BindLambda([this, &IsMuted](VivoxCoreError Status)
    {
        if(VxErrorSuccess == Status)
        {
            IsMuted = true;
            // This bool is only illustrative. The participant is now muted.
        }
    });
    participantToMute.BeginSetLocalMute(true, BeginSetLocalMuteCompletedCallback);
}
```

---

## Audio

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/audio/audio-toc

**Contents:**
- Audio#

---

## Manage fleets

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/guides/manage-fleets

**Contents:**
- Manage fleets#
  - Create a fleet#
  - Update a fleet#
  - Take a fleet offline#
  - Bring a fleet online#
  - Delete a fleet#

You can manage your fleets from the Unity Dashboard. Refer to the sections below to learn how to perform basic fleet tasks, such as creating a fleet, updating a fleet, taking a fleet offline, and bringing a fleet online.

Refer to Create a fleet.

Taking a fleet offline initializes the process of scaling down all regions within the fleet. The fleet won’t be offline until each region within it finishes scaling down.

To take a fleet offline:

Warning: Taking a fleet offline impacts the availability of your game or application.

Note: Taking a region offline can take up to several hours. The process of taking a region offline drains all the servers within the region and prevents new players from joining servers in the region.

To bring a fleet online:

Warning: You can't delete a fleet if it's currently online and taking server allocations. You must take a fleet offline before you can delete it.

---

## Choose a machine specification

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/optimize-build-speed/choose-machine-specification

**Contents:**
- Choose a machine specification#
- Machine type specifications for build machines that run Windows OS#
- Machine type specifications for build machines that run macOS#
- Disk size#

Build Automation provides several options for the build machine that you use to run your build, in Build Automation we call this a machine type.

A machine type with a larger disk allows you to build projects with a larger source code repository.

A higher tier machine type is expected to have faster build times, but this is not guaranteed and is not the case for all Unity projects.

Premium machines are recommended for projects with heavy shader compilation, lightmap baking, or projects over 50GB in size.

A machine type includes all the machine resources needed for the duration of the build.

Note: The costs below are in USD, and are examples based on the official public pricing. Your specific pricing may vary based on any custom negotiated agreements.

Note: Free tier eligible means this machine type has an allotment of free build minutes. Find more details on the free tier on the Unity Cloud pricing page.

For Standard Machine Types, we guarantee a minimum 150GB of free space that is usable for User Content. User Content includes the following types of data:

Note: The specific details and complexity of each project affects the project's use of resources at build time. Base your choice of builder machine on your specific project and its size and requirements.

---

## Windows app development

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/windows/windows-toc

**Contents:**
- Windows app development#

Use the following pages to help with developing Windows applications.

---

## Chat history query with blocked participants

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/chat-history/chat-history-query-blocked-participant

**Contents:**
- Chat history query with blocked participants#

Messages from blocked participants are not included in the results of a chat history query.

Parameters in chat history requests:

---

## Clone hangs when you use Git submodules

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/check-build-results/troubleshoot-build-failures/git-clone-hangs

**Contents:**
- Clone hangs when you use Git submodules#
- Symptom#
- Resolution#
- Additional resource#

You configured your project to use Git submodules, but the clone step hangs.

To stop the clone step from hanging, verify that you configured your submodules to use either the relative path or the SSH path to the submodule.

For an example of how to use relative paths, refer to this Stack overflow post.

---

## Buddy presence changes

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/presence/buddy-presence-changes

**Contents:**
- Buddy presence changes#

After User B has approved User A as a buddy, User A can see changes to User B's online status.

Online status consists of two pieces of information: an online state and a custom message.

The following list details valid online states:

You can use any text for the custom message, but it must be UTF-8 encoded. Whenever User B changes this information, User A receives a vx_evt_buddy_presence message.

The following code displays an example of the process for handling this message:

Note: The vx_evt_buddy_presence.encoded_uri_with_tag field distinguishes the presence information of a single user that is signed in at multiple locations. The application can choose to display this information separately or aggregate the information together.

**Examples:**

Example 1 (unknown):
```unknown
void  HandleEvtBuddyPresence(vx_evt_buddy_presence *evt)
{
    printf("%s is now %s (%s)\n", evt->encoded_uri_with_tag,vx_get_presence_state_string(evt->presence), evt->custom_message);
}
```

Example 2 (unknown):
```unknown
void  HandleEvtBuddyPresence(vx_evt_buddy_presence *evt)
{
    printf("%s is now %s (%s)\n", evt->encoded_uri_with_tag,vx_get_presence_state_string(evt->presence), evt->custom_message);
}
```

---

## Access token examples

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/access-token-examples/access-token-examples-toc

**Contents:**
- Access token examples#

The access token examples in this section include spacing in the payload section which has been added for demonstration purposes. Do not include any spacing in the payload for your token.

Note: In the payload section for each example, you can list the claims in any order.

---

## Inject a text-to-speech message

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/text-to-speech/inject-tts-message

**Contents:**
- Inject a text-to-speech message#

To inject a text-to-speech (TTS) message into channel or local audio, use VivoxService.Instance.TextToSpeechSendMessage(string message, TextToSpeechMessageType messageType). Each method takes a string and a TextToSpeechMessageType argument that specifies the text to synthesize and the destination for where to inject the synthesized text. The Voice property and the State property of the TTSMessage are updated by either function when it successfully completes.

After being injected into the text-to-speech subsystem, the TTS Message reference can later be used to cancel that message. If you do not want to hold on to the message, you can obtain any previously injected messages at a later time while they remain in the VivoxService.Instance.TextToSpeechCancelMessages() collection until the message is canceled or playback ends.

If multiple TTS users are in one voice or text channel, then you can prefix the display name of the user who is sending the TTS message. For example, “[Display name] says…”

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.TextToSpeechSendMessage(string message, TextToSpeechMessageType messageType)
```

Example 2 (unknown):
```unknown
VivoxService.Instance.TextToSpeechCancelMessages()
```

Example 3 (unknown):
```unknown
VivoxService.Instance.TextToSpeechSendMessage("Hello All", TextToSpeechMessageType.QueuedLocalPlayback);
```

Example 4 (unknown):
```unknown
VivoxService.Instance.TextToSpeechSendMessage("Hello All", TextToSpeechMessageType.QueuedLocalPlayback);
```

---

## Create a build configuration

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/guides/create-a-build-configuration

**Contents:**
- Create a build configuration#
- Set the build configuration details#
- Define any configuration variables#

The embedded Multiplay Hosting setup guide walks you through creating your first build configuration. However, you can always add more build configurations by following these instructions.

Note: You can create up to 200 build configurations.

The first step is to name the build configuration, link it to a specific build, and define any additional details about the build configuration.

The next step is to add configuration variables. Add each configuration variable you want to track in this build configuration, then select Next.

If you’re unsure or don’t want to track any additional parameters here, you can continue to the next step without making any changes.

---

## Partial connection failure

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/channels/automatic-connection-recovery/unity-partial-failure

**Contents:**
- Partial connection failure#

In some cases, such as when an application is suspended in the background, the connection to Vivox services can be disrupted so that voice communication terminates without signing out the user.

We recommend that when a user is signed out, voluntarily or involuntarily, that they are disconnected from all channels successfully and then start them anew when VivoxService.Instance.LoginAsync is run again.

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.LoginAsync
```

---

## Companion applications

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/presence/companion-applications

**Contents:**
- Companion applications#

Desktop companion applications often want to detect keyboard input while the game has focus but the keyboard does not. Companion applications that want to automatically set presence to "away" also want to know whether a computer has gone idle. You can configure this by using many techniques on both Windows and macOS, and published source code for these scenarios is widely available on the internet.

For assistance with implementing companion applications, contact Vivox Support.

---

## Leave a channel

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/channels/leave-channel

**Contents:**
- Leave a channel#

To remove a user from a channel, call VivoxService.Instance.LeaveChannelAsync(string channelName) with the name of the channel to leave. Alternatively, VivoxService.Instance.LeaveAllChannelsAsync() can be used to leave all channels the user is connected to. The disconnected channel object remains in the VivoxService.Instance.ActiveChannels list until after the disconnection is successful.

The object is immediately removed from the list if the session is completely disconnected.

Note: Disconnection is not an immediate operation and can take several seconds to complete. A ChannelLeft event will trigger when a disconnection has been completed successfully.

The following code is an example of how to perform this operation:

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.LeaveChannelAsync(string channelName)
```

Example 2 (unknown):
```unknown
VivoxService.Instance.LeaveAllChannelsAsync()
```

Example 3 (unknown):
```unknown
VivoxService.Instance.ActiveChannels
```

Example 4 (unknown):
```unknown
using UnityEngine;
using Unity.Services.Vivox;

class VivoxBasicsExample : MonoBehaviour
{
    . . .
    void LeaveChannel(string channelNameToLeave)
    {
        . . .
        VivoxService.Instance.LeaveChannelAsync(channelNameToLeave);
        . . .
    }
    . . .
}
```

---

## Absolute paths for native libraries

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/android/absolute-paths-for-native-libraries

**Contents:**
- Absolute paths for native libraries#

If you plan to split up your Android application to reduce the initial download size, then you are probably looking to use Android App Bundles with Play Feature Delivery or APK Expansion Files (OBB). For more information, see the Android developer documentation on Play Feature Delivery and APK Expansion Files.

When using Play Feature Delivery or APK Expansion Files, you can choose to store some native libraries outside of the APK. To do this, you need to provide absolute paths to those native libraries.

For example, if libvivox-sdk.so is stored outside of the APK, specify this by calling a JNIHelpers.init(...) function that accepts the vivoxLibraryPath parameter:

After vivox-sdk is loaded, any additional libraries that you specify to load through JNIHelpers.init(..., String[] libs) work based on whether they are an absolute path:

**Examples:**

Example 1 (unknown):
```unknown
libvivox-sdk.so
```

Example 2 (unknown):
```unknown
JNIHelpers.init(...)
```

Example 3 (unknown):
```unknown
vivoxLibraryPath
```

Example 4 (unknown):
```unknown
JNIHelpers.init("/the/absolute/path/to/libvivox-sdk.so", context)
```

---

## Inject a text-to-speech message

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/text-to-speech/inject-tts-message

**Contents:**
- Inject a text-to-speech message#

To inject a text-to-speech (TTS) message into channel or local audio, use ITextToSpeech::Speak():

Speak() accepts the following parameters:

The text to be converted into speech.

Optionally as an out parameter, a TTS message reference set to the newly created ITTSMessage, regardless of whether it is currently playing in the destination or is in a queue waiting to be synthesized.

Note: You can use this reference at a later time, for example, to cancel TTS messages. It is possible to obtain these references at a later time by using ITextToSpeech::GetMessages().

If multiple TTS users are in one voice or text channel, then you can prefix the display name of the user who is sending the TTS message. For example, “[Display name] says…”

**Examples:**

Example 1 (unknown):
```unknown
ITextToSpeech::Speak()
```

Example 2 (unknown):
```unknown
ITTSMessage *Message;
TTSDestination Destination = TTSDestination::QueuedRemoteTransmissionWithLocalPlayback;
MyLoginSession->TTS().Speak("Hello World!", Destination, &Message);
```

Example 3 (unknown):
```unknown
ITTSMessage *Message;
TTSDestination Destination = TTSDestination::QueuedRemoteTransmissionWithLocalPlayback;
MyLoginSession->TTS().Speak("Hello World!", Destination, &Message);
```

Example 4 (unknown):
```unknown
ITextToSpeech::GetMessages()
```

---

## Text-to-speech voice options

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/text-to-speech/tts-voice-options

**Contents:**
- Text-to-speech voice options#

Before you inject a text-to-speech (TTS) message or read text locally, you can choose the voice to use for synthesizing speech. You can set this option per user at any time.

Two voices are available: male and female. You can list all available voices by using the following script:

When a TTS method is called which calls for speech to be synthesized, the system uses the ITTSVoice that is set as the current voice for that user. If no voice is selected, the system uses the Vivox SDK default.

**Examples:**

Example 1 (unknown):
```unknown
for (auto& Elem : MyLoginSession->TTS().GetAvailableVoices())
    { UE_LOG(LogTemp, Log, TEXT("Available Voice: Name=[%s]"), *Elem.Key); }
// Available Voice: Name=[en_US male]
// Available Voice: Name=[en_US female]

ITTSVoice *NewVoice = MyLoginSession->TTS().GetAvailableVoices()["en_US female"];
if (NewVoice != nullptr)
    { MyLoginSession->TTS().SetCurrentVoice(*NewVoice); }
UE_LOG(LogTemp, Log, TEXT("Current Voice: %s"), *MyLoginSession->TTS().GetCurrentVoice().Name());
// Current Voice: en_US female
```

Example 2 (unknown):
```unknown
for (auto& Elem : MyLoginSession->TTS().GetAvailableVoices())
    { UE_LOG(LogTemp, Log, TEXT("Available Voice: Name=[%s]"), *Elem.Key); }
// Available Voice: Name=[en_US male]
// Available Voice: Name=[en_US female]

ITTSVoice *NewVoice = MyLoginSession->TTS().GetAvailableVoices()["en_US female"];
if (NewVoice != nullptr)
    { MyLoginSession->TTS().SetCurrentVoice(*NewVoice); }
UE_LOG(LogTemp, Log, TEXT("Current Voice: %s"), *MyLoginSession->TTS().GetCurrentVoice().Name());
// Current Voice: en_US female
```

---

## Troubleshooting

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/troubleshooting/troubleshooting-toc

**Contents:**
- Troubleshooting#

Use the topics in this section to help troubleshoot the Vivox SDK.

---

## SCO handshake failures

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/android/sco-handshake-failures

**Contents:**
- SCO handshake failures#

There are a lot of Android devices, Android versions, and Bluetooth devices from different manufacturers. This leads to a lot of combinations. Because of the variation in handset/device combinations, it’s very difficult to say why it happens with any particular set. It could be limited resources, an incompatibility in Bluetooth versions, one of the devices in the wrong mode, or one of many other reasons. In the Vivox SPAM logic, we retry up to three times with a 300 ms delay between each attempt in order to maximize the opportunity for a Bluetooth SCO device to connect correctly.

---

## Channel management

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/integration-guide/integration-plan/phase-one/channel-management

**Contents:**
- Channel management#
- Types of channels#
- Additional resources#

Channel management primarily focuses on selecting appropriate channel IDs and identifying code paths for joining and leaving channels, in addition to authorizing those operations.

By default, channels are named and created on an ad-hoc basis. The process for joining channels is regulated by a game server authorization code before joining. The selection of appropriate unique channel IDs is important, as is associating the IDs with relevant in-game data, such as parties, teams, lobbies, and guilds. You can use this identifier in an authenticated channel join request to allow a user to perform an authorized join.

By default, channels have the following properties:

There are special channel types that you can use in certain specific or unusual circumstances. For example, the echo channel, static channels, and the 3D positional channel. These special channel types require additional setup and configuration, demand additional resources to provision, and are recommended only for use in cases where standard ephemeral channels are inadequate.

The following list details special channels and their properties:

Persistent/semi-persistent channels

3D positional audio channels

Note: Default channels are generally enough for most use cases. Echo channels are useful for both internal development and testing, and for providing an audio test environment for the user.

---

## Player evidence report HTTP response

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/chat-history/player-evidence-report-responses

**Contents:**
- Player evidence report HTTP response#

The following code is an example of a player evidence response:

The next field is used for pagination and represents the player’s most recent activity in the next conversation returned. This value will be null if there are no more pages of results to return. Use the next value as the end_ts in a subsequent request to retrieve the next page of results. The request_start_ts is also used for pagination and represents the timestamp of the initial request. This value maintains consistency in paging results if the player is active during review. The request_start_ts is required when using the next value to request subsequent pages of results.

The conversation objects in the response contain the following fields:

Returned conversation results are in order of the player’s most recent activity.

**Examples:**

Example 1 (unknown):
```unknown
{
   "request_start_ts": 1706203970429807,
   "next": 1704400836045162,
   "conversations": [
       {
           "message_count": 2,
           "last_activity": 1704400939799491,
           "conversation_uri": "sip:confctl-g-your-issuer.test-channel-1@youvivoxdomainname.vivox.com"
       },
       {
           "message_count": 1,
           "last_activity": 1704400878101904,
           "conversation_uri": "sip:your-issuer.doe@yourvivoxdomainname.vivox.com"
       }
   ]
}
```

Example 2 (unknown):
```unknown
{
   "request_start_ts": 1706203970429807,
   "next": 1704400836045162,
   "conversations": [
       {
           "message_count": 2,
           "last_activity": 1704400939799491,
           "conversation_uri": "sip:confctl-g-your-issuer.test-channel-1@youvivoxdomainname.vivox.com"
       },
       {
           "message_count": 1,
           "last_activity": 1704400878101904,
           "conversation_uri": "sip:your-issuer.doe@yourvivoxdomainname.vivox.com"
       }
   ]
}
```

Example 3 (unknown):
```unknown
request_start_ts
```

Example 4 (unknown):
```unknown
request_start_ts
```

---

## Example: Mute_All token

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/access-token-guide/access-token-examples/example-mute-all-token

**Contents:**
- Example: Mute_All token#
- Signed in user muting all users in a channel#
- Admin user muting all users in a channel#
- Signed in user muting all but one user in a channel (Presentation mode)#
- Admin user muting all but one user in a channel (Presentation mode)#

Note: The token format for unmuting all users in a channel is the same as a mute_all token.

The token in this example allows the user "beef" to mute all users in the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

The token in this example allows the user "blindmelon-AppName-dev-Admin" to mute all users in the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

The token in this example allows the user "beef" to mute all users except for the user "jerky" in the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

In this scenario, when creating the mute token, a subject is specified. The subject in this instance ("jerky") is the only user in the channel that is not muted, which allows them to speak to the other, now muted, users in the channel without interruption.

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

The token in this example allows the user "blindmelon-AppName-dev-Admin" to mute all users except for the user "jerky" in the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

In this scenario, when creating the mute token, a subject is specified. The subject in this instance ("jerky") is the only user in the channel that is not muted, which allows them to speak to the other, now muted, users in the channel without interruption.

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

**Examples:**

Example 1 (unknown):
```unknown
{
    "vxi":19283,
    "f":"sip:.blindmelon-AppName-dev.beef.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"mute",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 2 (unknown):
```unknown
{
    "vxi":19283,
    "f":"sip:.blindmelon-AppName-dev.beef.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"mute",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 3 (unknown):
```unknown
e30.eyJ2eGkiOjE5MjgzLCJmIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2LmJlZWYuQHRsYS52aXZveC5jb20iLCJpc3MiOiJibGluZG1lbG9uLUFwcE5hbWUtZGV2IiwidnhhIjoibXV0ZSIsInQiOiJzaXA6Y29uZmN0bC1nLWJsaW5kbWVsb24tQXBwTmFtZS1kZXYudGVzdGNoYW5uZWxAdGxhLnZpdm94LmNvbSIsImV4cCI6MTYwMDM0OTQwMH0.fARLW2eX10ZbiIl_5WIg4bhPbYIhn2xfCcUNySfwBMs
```

Example 4 (unknown):
```unknown
e30.eyJ2eGkiOjE5MjgzLCJmIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2LmJlZWYuQHRsYS52aXZveC5jb20iLCJpc3MiOiJibGluZG1lbG9uLUFwcE5hbWUtZGV2IiwidnhhIjoibXV0ZSIsInQiOiJzaXA6Y29uZmN0bC1nLWJsaW5kbWVsb24tQXBwTmFtZS1kZXYudGVzdGNoYW5uZWxAdGxhLnZpdm94LmNvbSIsImV4cCI6MTYwMDM0OTQwMH0.fARLW2eX10ZbiIl_5WIg4bhPbYIhn2xfCcUNySfwBMs
```

---

## Chat history

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/chat-history/chat-history

**Contents:**
- Chat history#

Note: This feature is currently only available in the Core SDK and Unity SDK 16.

Chat history allows for the archiving and client-side retrieval of text conversations between users. Users can look up specific conversations or view previously sent messages by all users in a conversation.

Note: Messages from blocked participants are not returned in chat history search results. A history request can come back empty if all messages in the request were from a blocked participant. In this case, the response contains a cursor indicating that there are more messages in the channel. The max argument for chat history will show fewer messages than the number requested if there is a message from a blocked user in the channel. Use the cursor value in the subsequent request to retrieve the next page of messages.

Chat history has a storage period of 7 days. If Text Evidence Management is enabled in a project, the storage period is extended to 30 days.

---

## Channel names

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/access-token-identifiers/channel-names

**Contents:**
- Channel names#

When you create a channel name, the following criteria apply:

0x30-0x39, 0x41-0x5A, 0x61-0x7A

Note: You can only use the percent sign for URL encoding. Follow the percent sign by two uppercase hex characters.

The recommended best practice is to URL-encode the channel name by using only uppercase hex digit characters. Lowercase characters are allowed in the channel name, but not when used as the hex digit characters.

Channel names must start with a confctl prefix and channel type, followed by your issuer, followed by a period '.'.

For example, if your issuer name is "crux", then a valid channel name is confctl-g-crux.0verl0rd-channel

Note: For the Vivox Unity SDK and the Vivox Unreal SDK, the naming displayed in this example only applies during access token generation. When you construct a ChannelId, you are expected to use only the "0verl0rd-channel" part of the channel name and not "confctl-g-crux.0verl0rd-channel" for the name argument. The ChannelId function ToString() returns a full channel URI that is suitable for token generation, which includes the channel type, issuer, dot, and other URI components.

**Examples:**

Example 1 (unknown):
```unknown
confctl-g-crux.0verl0rd-channel
```

Example 2 (unknown):
```unknown
"0verl0rd-channel"
```

Example 3 (unknown):
```unknown
"confctl-g-crux.0verl0rd-channel"
```

---

## Example: C++

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/access-token-guide/generate-token-secure-server/example-c-plus-plus

**Contents:**
- Example: C++#

The following code is an example of how to generate access tokens in C++.

**Examples:**

Example 1 (javascript):
```javascript
/*
* Vivox Token Generation Example - C++
* Note: This example uses C++11, and is written to not use external libraries to demonstrate the token generation process.
* However, because HMACSHA256 is required, this example uses OpenSSL 1.1.0+.
*
*/
#include <openssl/sha.h>
#include <openssl/evp.h>
#include <openssl/hmac.h>
#include <iostream>
#include <iomanip>
#include <sstream>
#include <string>

using namespace std;

typedef unsigned char uchar;
static const string b = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789-_+/";
/*
* You can use Base64URL encoding or Base64 and URL encoding libraries in place of this implementation
* Note: Any trailing ='s must be removed from the encoded string.
*/
static string base64_encode(const string &in)
{
    string out;

    int val = 0, valb = -6;
    for (const char c : in) {
        val = (val << 8) + static_cast<unsigned char>(c);
        valb += 8;
        while (valb >= 0) {
            out.push_back(b[(val >> valb) & 0x3F]);
            valb -= 6;
        }
    }

    if (valb >- 6) out.push_back(b[((val << 8) >> (valb + 8))& 0x3F]);

    return out;
}

#if (OPENSSL_VERSION_NUMBER < 0x10100000L) || defined(LIBRESSL_VERSION_NUMBER)
static HMAC_CTX *HMAC_CTX_new(void)
{
    HMAC_CTX *ctx = static_cast<HMAC_CTX *>(OPENSSL_malloc(sizeof(*ctx)));
    if (ctx != nullptr) {
        HMAC_CTX_init(ctx);
    }

    return ctx;
}

static void HMAC_CTX_free(HMAC_CTX *ctx)
{
    if (ctx != nullptr) {
        HMAC_CTX_cleanup(ctx);
        OPENSSL_free(ctx);
    }
}
#endif

/*
* Uses OpenSSL 1.1.0+ to encode data with a secret using HMAC SHA-256
*/
string hmac(string key, string msg)
{
    unsigned char hash[32];

    HMAC_CTX *hmac = HMAC_CTX_new();
    HMAC_Init_ex(hmac, &key[0], static_cast<int>(key.length()), EVP_sha256(), nullptr);
    HMAC_Update(hmac, reinterpret_cast<const unsigned char *>(&msg[0]), msg.length());
    unsigned int len = 32;
    HMAC_Final(hmac, hash, &len);
    HMAC_CTX_free(hmac);

    stringstream ss;
    ss << setfill('0');
    for (unsigned int i = 0; i < len; i++) {
        ss << hash[i];
    }

    return ss.str();
}

static string vx_generate_token(const string &key, const string &issuer, int exp, const string &vxa, int vxi, const string &f, const string &t)
{
    // Header is static - base64url encoded {}
    string header = base64_encode("{}");  // Can also be defined as a constant "e30"

    // Create payload and base64 encode
    stringstream ss;
    ss << "{ \"iss\": \"" << issuer << "\",";
    ss << "\"exp\": " << exp << ",";
    ss << "\"vxa\": \"" << vxa << "\",";
    ss << "\"vxi\": " << vxi << ",";
    ss << "\"t\": \"" << t << "\",";
    ss << "\"f\": \"" << f << "\" }";
    string payload = base64_encode(ss.str());

    // Join segments to prepare for signing
    string to_sign = header + "." + payload;

    // Sign token with key and HMACSHA256, then base64 encode
    string signed_payload = base64_encode(hmac(key, to_sign));

    // Combine header and payload with signature
    return to_sign + "." + signed_payload;
}

int main()
{
    string token = vx_generate_token("secret", "issuer", 1559359105, "join", 123456, "sip:.username.@domain.vivox.com", "sip:confctl-g-channelname@domain.vivox.com");
    cout << token << endl;

    return 0;
}
```

Example 2 (javascript):
```javascript
/*
* Vivox Token Generation Example - C++
* Note: This example uses C++11, and is written to not use external libraries to demonstrate the token generation process.
* However, because HMACSHA256 is required, this example uses OpenSSL 1.1.0+.
*
*/
#include <openssl/sha.h>
#include <openssl/evp.h>
#include <openssl/hmac.h>
#include <iostream>
#include <iomanip>
#include <sstream>
#include <string>

using namespace std;

typedef unsigned char uchar;
static const string b = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789-_+/";
/*
* You can use Base64URL encoding or Base64 and URL encoding libraries in place of this implementation
* Note: Any trailing ='s must be removed from the encoded string.
*/
static string base64_encode(const string &in)
{
    string out;

    int val = 0, valb = -6;
    for (const char c : in) {
        val = (val << 8) + static_cast<unsigned char>(c);
        valb += 8;
        while (valb >= 0) {
            out.push_back(b[(val >> valb) & 0x3F]);
            valb -= 6;
        }
    }

    if (valb >- 6) out.push_back(b[((val << 8) >> (valb + 8))& 0x3F]);

    return out;
}

#if (OPENSSL_VERSION_NUMBER < 0x10100000L) || defined(LIBRESSL_VERSION_NUMBER)
static HMAC_CTX *HMAC_CTX_new(void)
{
    HMAC_CTX *ctx = static_cast<HMAC_CTX *>(OPENSSL_malloc(sizeof(*ctx)));
    if (ctx != nullptr) {
        HMAC_CTX_init(ctx);
    }

    return ctx;
}

static void HMAC_CTX_free(HMAC_CTX *ctx)
{
    if (ctx != nullptr) {
        HMAC_CTX_cleanup(ctx);
        OPENSSL_free(ctx);
    }
}
#endif

/*
* Uses OpenSSL 1.1.0+ to encode data with a secret using HMAC SHA-256
*/
string hmac(string key, string msg)
{
    unsigned char hash[32];

    HMAC_CTX *hmac = HMAC_CTX_new();
    HMAC_Init_ex(hmac, &key[0], static_cast<int>(key.length()), EVP_sha256(), nullptr);
    HMAC_Update(hmac, reinterpret_cast<const unsigned char *>(&msg[0]), msg.length());
    unsigned int len = 32;
    HMAC_Final(hmac, hash, &len);
    HMAC_CTX_free(hmac);

    stringstream ss;
    ss << setfill('0');
    for (unsigned int i = 0; i < len; i++) {
        ss << hash[i];
    }

    return ss.str();
}

static string vx_generate_token(const string &key, const string &issuer, int exp, const string &vxa, int vxi, const string &f, const string &t)
{
    // Header is static - base64url encoded {}
    string header = base64_encode("{}");  // Can also be defined as a constant "e30"

    // Create payload and base64 encode
    stringstream ss;
    ss << "{ \"iss\": \"" << issuer << "\",";
    ss << "\"exp\": " << exp << ",";
    ss << "\"vxa\": \"" << vxa << "\",";
    ss << "\"vxi\": " << vxi << ",";
    ss << "\"t\": \"" << t << "\",";
    ss << "\"f\": \"" << f << "\" }";
    string payload = base64_encode(ss.str());

    // Join segments to prepare for signing
    string to_sign = header + "." + payload;

    // Sign token with key and HMACSHA256, then base64 encode
    string signed_payload = base64_encode(hmac(key, to_sign));

    // Combine header and payload with signature
    return to_sign + "." + signed_payload;
}

int main()
{
    string token = vx_generate_token("secret", "issuer", 1559359105, "join", 123456, "sip:.username.@domain.vivox.com", "sip:confctl-g-channelname@domain.vivox.com");
    cout << token << endl;

    return 0;
}
```

---

## Build Addressables with Build Automation

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/advanced-build-configuration/build-addressables-using-build-automation

**Contents:**
- Build Addressables with Build Automation#
- Prerequisites#
- Build configuration#
  - Run a new Addressables build#
  - Enable Cloud Content Delivery service#
  - Run a content-update build#
- Start a build#
- Copy Addressables content from Build Automation to your hosting provider#
  - Copy Addressables content manually#
  - Copy Addressables content with a post-build script#

Addressable Assets are assets that have a unique address which you can use to load them from local or remote AssetBundles.

Before you can build Addressable Assets in Build Automation, you must:

To start from a new Build Automation project, you need to create a new build configuration first.

Use the Addressables section to configure properties that determine how the Addressables build process behaves. Tooltips describe each property in more detail.

To run a new Addressables build,

For more information, refer to Run builds manually.

To upload Addressables to the Cloud Content Delivery (CCD) service from Build Automation:

For more information about using Addressables with CCD, refer to CCD + Addressables walkthrough.

Content update builds update a previously built player with new addressable content.

To update the existing player, update builds require a Content State file. This links the content from an Update build to an existing player. The Content State file generates when you do a new Addressables build.

To update a previously built player with new Addressable assets:

Build Automation can automatically use the most recent Content State file produced by the selected Build Target.

When the build target is configured, to start a new Addressables build:

When the new build has successfully completed, the More menu (⋮) for the build displays the Download Addressable Assets option.

Once you complete an Addressables build, you can copy the Addressables content from Build Automation to your hosting provider. You can copy the content manually, or use a post-build script.

Once you complete an Addressables build, select Download Addressables Assets from the More menu (⋮). When you download the content, you can upload it to your hosting provider when you build locally.

Build Automation supports running a custom shell script before or after a build. You can create a post-build script to automatically upload your Addressables content when a build successfully completes.

To enable a post-build script:

Consider the following information when you write your script:

**Examples:**

Example 1 (unknown):
```unknown
$OUTPUT_DIRECTORY/extra_data/addrs
```

---

## Manage audio

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/audio/audio-toc

**Contents:**
- Manage audio#

---

## Transmitting channels

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/transmission/transmitting-channels

**Contents:**
- Transmitting channels#

VivoxService.Instance.TransmittingChannels is a read-only collection that contains the name of each channel that a user is set to transmit to.

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.TransmittingChannels
```

---

## Manage test allocations

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/guides/manage-test-allocations

**Contents:**
- Manage test allocations#
- Create a test allocation#
- Delete a test allocation#

You can manage test allocations from the Unity Dashboard.

Test allocations give you a way to quickly make an allocation request. This is useful for testing your game server while you are developing it and don't have a matchmaker configured yet.

Both a test allocation and normal allocations behave the same way. A test allocation additionally has tracking information allowing it to be shown on the Unity Dashboard.

Note: Test allocations currently dont support allocations payloads.

Test allocations allow you to ensure your Multiplay Hosting configuration is working properly. You can create test allocations through the Unity Dashboard.

Note: You must have at least one online fleet before creating a test allocation.

You can view your test allocation listed in the Test allocations view. You can also use your game client and the provided connection details to test your game live.

Sign in to the Unity Dashboard.

---

## Adaptive chat filter

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/chat-history/adaptive-chat-filter

**Contents:**
- Adaptive chat filter#
- Chat filter language handling per message#
- Speech-to-text#

The chat filter censors words in a message based on a blocklist. This feature is currently server-enabled and needs to be enabled on your account before you can use it. For more information on how to enable this feature, contact your sales representative.

The chat filter handles messages based on the user’s set language. The chat filter expects the language to be set in each request on a per-message basis. This means that if a preferred language is set, the Vivox SDK must specify which language to use on the respective channel class or interface that sends messages to ensure that the proper chat filter is used.

The following is a code example for setting the language of a direct message. The same approach can be used on channel-based messages.

Messages produced through Speech-to-text can be filtered. Words are filtered out when the speech is converted to text. To enable this feature, contact your Vivox representative.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_account_send_msg *req;
vx_req_account_send_msg_create(&req);
req->connector_handle = vx_strdup("c1");
req->account_handle = vx_strdup(".issuer-w-dev.mytestaccountname.");
req->language = vx_strdup(“en-us”);
req->user_uri = vx_strdup("sip:theotheruser@vd1.vivox.com");
req->message_body = vx_strdup("Hey there buddy!");
vx_issue_request2(&req->base);
```

Example 2 (unknown):
```unknown
vx_req_account_send_msg *req;
vx_req_account_send_msg_create(&req);
req->connector_handle = vx_strdup("c1");
req->account_handle = vx_strdup(".issuer-w-dev.mytestaccountname.");
req->language = vx_strdup(“en-us”);
req->user_uri = vx_strdup("sip:theotheruser@vd1.vivox.com");
req->message_body = vx_strdup("Hey there buddy!");
vx_issue_request2(&req->base);
```

---

## Delete a buddy

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/presence/delete-buddy

**Contents:**
- Delete a buddy#

After a user signs in, if they want to delete a buddy, the game must send a vx_account_buddy_delete request.

The following code displays an example of this process with the sample issuer key ''MyIssuer":

**Examples:**

Example 1 (unknown):
```unknown
vx_req_account_buddy_delete req;
vx_req_account_buddy_delete_create(&req);
req->account_handle = vx_strdup(".MyIssuer.mytestaccountname.");
req->buddy_uri = vx_strdup(“sip:MyIssuer.mybuddy.@mt1s.vivox.com”);
vx_issue_request3(&req->base, &request_count);
```

Example 2 (unknown):
```unknown
vx_req_account_buddy_delete req;
vx_req_account_buddy_delete_create(&req);
req->account_handle = vx_strdup(".MyIssuer.mytestaccountname.");
req->buddy_uri = vx_strdup(“sip:MyIssuer.mybuddy.@mt1s.vivox.com”);
vx_issue_request3(&req->base, &request_count);
```

---

## Retrieve conversations list

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/retrieve-conversation-list

**Contents:**
- Retrieve conversations list#

You can retrieve a user’s list of conversations. A conversation is a channel where a user has sent a message or a direct message (DM). You can use this API to populate a list of channels or DMs for a user to find their active chats.

Note: The returned Conversation list is ranked by most recent activity. Conversations with newer messages show up first in the results.

To retrieve the conversation list, sign in to the application and then use vx_req_account_get_conversations to make a request. This request returns:

The following code is an example of how to retrieve a user’s conversation list.

After you retrieve the user’s conversation list, the game must process the vx_resp_account_get_conversations_t response:

**Examples:**

Example 1 (unknown):
```unknown
vx_req_account_get_conversations *req;
vx_req_account_get_conversations_create(&req);
req->cursor = ”1”; // Optional parameter if you’re fetching anything but the first page
req->request_time = 1697135622123456; // Optional, unix time in microseconds, needs to be set to the same time to keep results consistent between pages
req->page_size = 10; // Optional, otherwise server default is used. Default is 10
vx_issue_request2(&req->base);
```

Example 2 (unknown):
```unknown
vx_req_account_get_conversations *req;
vx_req_account_get_conversations_create(&req);
req->cursor = ”1”; // Optional parameter if you’re fetching anything but the first page
req->request_time = 1697135622123456; // Optional, unix time in microseconds, needs to be set to the same time to keep results consistent between pages
req->page_size = 10; // Optional, otherwise server default is used. Default is 10
vx_issue_request2(&req->base);
```

Example 3 (unknown):
```unknown
void HandleAccountGetConversationsResponse(vx_resp_account_get_conversations_t *resp)
{
	// Handles the response, if a next-cursor exists in the response this can be used
	// to fetch the next page
}
```

Example 4 (unknown):
```unknown
void HandleAccountGetConversationsResponse(vx_resp_account_get_conversations_t *resp)
{
	// Handles the response, if a next-cursor exists in the response this can be used
	// to fetch the next page
}
```

---

## Troubleshooting

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/guides/troubleshooting

**Contents:**
- Troubleshooting#
- Troubleshooting servers#
  - Missing server instances#
  - Servers crashing#
  - Servers fail to start#
  - Too many server instances#
- Troubleshooting installations#
  - Slow build installations#
- Troubleshooting usage settings#
  - Usage settings not optimized#

When you first create a fleet, it might take several minutes for the new servers to display in the Unity Dashboard.

If the servers are still not there after 15 minutes, check the scaling settings for each region within your fleet. The servers might not appear in the Unity Dashboard if you have a Minimum servers setting of less than one.

Servers sometimes crash, ending any active game allocations on that server. The following list details of some common reasons a server might crash.

If a server is crashing and you are sure there isn't a bug in your build, reach out to the customer success team.

Note: Before Multiplay Hosting restarts a build executable, it tries to dump the process to allow you to check the stacktrace. This appears in logs as a SIGSEGV.

Servers can sometimes fail to start up, which prevents you from using the server in an allocation. The following list details some common reasons a server might fail to start.

If more server instances than you expect appear in the Unity Dashboard, it might be due to how Multiplay Hosting manages server density according to your usage settings.

Multiplay Hosting creates these two additional servers to make the most of the available capacity. By default, these additional servers are in a stopped state and offer a quick way of increasing server availability without needing to start a new machine.

Usually, a build installation shouldn’t take longer than five to 15 minutes. The following list details some common reasons why installing a game image might take longer than expected.

You might have to adjust the server density values several times before you find the ideal value. For example, if you notice that servers running this build configuration lag or exhibit performance issues, adjust the server density settings.

One of the best ways to find the ideal usage settings for your game server is to make CPU value changes in increments and decrements of about 100 MHz and RAM value changes in increments and decrements of about 100 MB. Each time, you make an change, test the build to view how your servers perform with the build.

There are many reasons why a server might use more CPU than you expect. However, one common reason is that the main game loop runs as fast as the CPU allows, consuming all resources. Multiplay Hosting detects this and after a grace period, stop the server.

You can prevent this by limiting the application target frame rate and the number of vertical syncs that pass between each frame.

The application target frame is controlled by the Application.targetFrameRate setting. In most cases, setting it to a value around 60 prevents the game loop from using too much CPU.

Refer to the Application.targetFrameRate script reference documentation.

The QualitySettings.vSyncCount setting controls the vertical syncs your game uses even without a display. In most cases, setting it to 0 prevents the game loop from using too much CPU. Refer to the QualitySettings.vSyncCount script reference documentation.

You can use Multiplay Hosting to surface server files through the Unity Dashboard. Before you can access your server files:

Specify the location of your logs and files in the build configuration.

Specify the correct log and file locations in the build configuration through the launch parameters, a configuration variable, or both. Like the query protocol, the log and file locations implemented in a build must match the log and file location in all build configurations pointing to that build. The following code blocks show how to use the $$log_dir$$ and $$file_dir$$ configuration variable in the launch parameters.

Note: If you built your game using Unreal Engine, you’ll need to redirect your logs before you can access them from the Unity Dashboard.

Note: If your server executable is a Unity build, use -logFile $$log_dir$$ in the launch parameters. If you're using a Unity build and not seeing any log files in the Unity Dashboard, try using -logFile $$log_dir$$/logfile.log.

Known issue with Unity 2021.3.10f1. and up.

This occurs when attempting a Multiplay Hosting build while having a current build target that is different than Dedicated server, and the selected scripting backend is not available.

Note: If your server executable is a Unity build, use -logFile $$log_dir$$ in the launch parameters. If you're using a Unity build and not seeing any log files in the dashboard, try using -logFile $$log_dir$$/logfile.log. For this use case the $$files_dir$$ parameter can also be used.

**Examples:**

Example 1 (unknown):
```unknown
Application.targetFrameRate
```

Example 2 (unknown):
```unknown
Application.targetFrameRate = 60
```

Example 3 (unknown):
```unknown
Application.targetFrameRate = 60
```

Example 4 (unknown):
```unknown
QualitySettings.vSyncCount
```

---

## Moderation SDK

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/safety/moderation-sdk

**Contents:**
- Moderation SDK#
- Report a player#
- Report reason#

Once the setup is complete, you can report players by using their UAS ID and providing a reason for the report.

Once the player is reported, you can navigate to the Unity Dashboard to review incidents with the attached report information.

If you’re using the Vivox Unreal Module packaged with the Moderation Unreal Module, the last 45 minutes of the conversation from the channel that the reporter and the reported were in will be attached to the report.

Below is an example of a report being submitted:

Reasons are defined by Unity and are available using the EReportReason enum.

Find the constants for each reason in Moderation > Public > ReportReason.h.

**Examples:**

Example 1 (cpp):
```cpp
#include "ModerationSubsystem.h"

void UGameInstance::IssueModerationReport(FString ReportedPlayerUASId, EReportReason reportReason)
{
    UModerationSubsystem* moderationSubsystem;
    moderationSubsystem = GetSubsystem<UModerationSubsystem>();

    FReport report = ModerationClient->GetReport(ReportedPlayerUASId, reportReason);

    Moderation::THandler<FReportResponse> reportHandler;
    reportHandler.BindLambda([](FReportResponse response) {
        if (response.bWasSuccessful) {
            UE_LOG(LogTemp, Log, TEXT("call was successfull"));
        }
        else {
            UE_LOG(LogTemp, Log, TEXT("call was not successfull"));
        }
        });
    ModerationClient->ReportPost(report, authenticationToken, handler);
}
```

Example 2 (cpp):
```cpp
#include "ModerationSubsystem.h"

void UGameInstance::IssueModerationReport(FString ReportedPlayerUASId, EReportReason reportReason)
{
    UModerationSubsystem* moderationSubsystem;
    moderationSubsystem = GetSubsystem<UModerationSubsystem>();

    FReport report = ModerationClient->GetReport(ReportedPlayerUASId, reportReason);

    Moderation::THandler<FReportResponse> reportHandler;
    reportHandler.BindLambda([](FReportResponse response) {
        if (response.bWasSuccessful) {
            UE_LOG(LogTemp, Log, TEXT("call was successfull"));
        }
        else {
            UE_LOG(LogTemp, Log, TEXT("call was not successfull"));
        }
        });
    ModerationClient->ReportPost(report, authenticationToken, handler);
}
```

---

## Moderation actions

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/safety/moderation-actions

**Contents:**
- Moderation actions#

Actions are applied through the Unity Dashboard when reviewing incidents.

When an action is applied, and the actioned user logs in, you will receive various error messages from the different UGS packages integrated with the Moderation services. Errors will follow the access control convention described in Error responses. This is the expected behavior.

Some SDKs will provide a mechanism to wrap those errors into exceptions of their own, for example, the Vivox SDK emits a MintException when a policy is preventing a user from accessing communications.

Below is the supported sanctions error list:

**Examples:**

Example 1 (unknown):
```unknown
MintException
```

Example 2 (unknown):
```unknown
com.unity.services.authentication
```

Example 3 (unknown):
```unknown
com.unity.services.vivox
```

---

## Access Token Developer Guide

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/access-token-guide/access-token-guide-toc

**Contents:**
- Access Token Developer Guide#

Note: Vivox Access Tokens are an optional, and advanced, feature for managing player channel access. Unity Authentication tokens are the recommended way to manage players when channel access control is not required.

Player access to Vivox resources is controlled through Vivox access tokens (VATs). Vivox access tokens contain a payload that defines the privileged operation, are signed by the game server by using a token signing key, and are delivered by the client to the Vivox system when the player wants to perform a privileged operation. A Vivox access token is similar to a JSON Web Token, but instead has an empty access token header.

Access tokens have the following characteristics:

Game clients require access tokens to perform operations in the Vivox system.

When using Unity Authentication (UAS), Vivox access tokens can parse UASIDs from the token payload. It's mandatory to include embedded UAS IDs in your access tokens if you're using Vivox access tokens with Safe Text.

Refer to the sections titled Generate a token from the client and Generate a token on a secure sever to generate tokens.

Note that before either a client or a server can generate a token, you need a token issuer and a token signing key. For more information, refer to Where do I find my custom application credentials? in the Unity Support Knowledge Base.

The following diagram displays a typical interaction between a game client, the game server, and the Vivox system:

---

## Supported values for the Vivox action claim

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/access-token-format/supported-values-vxa-claim

**Contents:**
- Supported values for the Vivox action claim#

The following table details the Vivox action (vxa) claims in the Vivox SDK.

vx_req_sessiongroup_add_session

vx_req_session_create

https://.../api2/viv_multi_chan_cmd.php?mode=add

Channel join listen-only and moderator muted.

vx_req_sessiongroup_add_session

vx_req_session_create

https://.../api2/viv_multi_chan_cmd.php?mode=add

Kick user from a channel.

vx_req_channel_kick_user

https://.../api2/viv_chan_cmd.php?mode=kick

https://.../api2/viv_chan_cmd.php?mode=drop_all

vx_req_account_anonymous_login

https://.../api2/viv_signin.php

Mute or unmute a user in a channel.

vx_req_channel_mute_user

https://.../api2/viv_chan_cmd.php?mode=mute

https://.../api2/viv_chan_cmd.php?mode=unmute

vx_req_channel_mute_all_users

https://.../api2/viv_chan_cmd.php?mode=mute_all

https://.../api2/viv_chan_cmd.php?mode=unmute_all

Enable or disable speech-to-text transcription.

If the Vivox domain that you are using is not enabled for speech-to-text transcription, attempting to enable transcription results in an error.

vx_req_session_transcription_control

---

## Choose a Git Executable

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/optimize-build-speed/choose-git-executable

**Contents:**
- Choose a Git Executable#
- Git Executable options#
  - Native#
  - Cygwin#
- Configure Git Executable#
  - Configure the Git Executable for a project#
  - Configure your Git Executable for a Build Target Configuration#

Unity Build Automation supports selecting the Git Executable for your build to use if you use a Windows operating system.

Unity Build Automation currently supports the following Git executable options for Windows operating systems:

If you choose this option, builds running on Windows operating system will use the Windows native Git client to clone your source code. This is the default and recommended option for new projects and using this option, the clone performance tend to improve the bigger your repository size is.

If you choose this option, builds running on Windows operating system will use a Cygwin Git client, and it's recommended for better compatibility with Linux and macOS Git setups. It's also recommended if you are using Linux-based symlinks in your repository.

You can configure your Windows Git Executable at two levels: for a specific project, or for a build target configuration.

Note: This option overrides the project level Git Executable setting.

---

## Supported values for the Vivox action claim

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/access-token-guide/access-token-format/supported-values-vxa-claim

**Contents:**
- Supported values for the Vivox action claim#

The following table details the Vivox action (vxa) claims in the Vivox SDK.

vx_req_sessiongroup_add_session

vx_req_session_create

https://.../api2/viv_multi_chan_cmd.php?mode=add

Channel join listen-only and moderator muted.

vx_req_sessiongroup_add_session

vx_req_session_create

https://.../api2/viv_multi_chan_cmd.php?mode=add

Kick user from a channel.

vx_req_channel_kick_user

https://.../api2/viv_chan_cmd.php?mode=kick

https://.../api2/viv_chan_cmd.php?mode=drop_all

vx_req_account_anonymous_login

https://.../api2/viv_signin.php

Mute or unmute a user in a channel.

vx_req_channel_mute_user

https://.../api2/viv_chan_cmd.php?mode=mute

https://.../api2/viv_chan_cmd.php?mode=unmute

vx_req_channel_mute_all_users

https://.../api2/viv_chan_cmd.php?mode=mute_all

https://.../api2/viv_chan_cmd.php?mode=unmute_all

Enable or disable speech-to-text transcription.

If the Vivox domain that you are using is not enabled for speech-to-text transcription, attempting to enable transcription results in an error.

vx_req_session_transcription_control

---

## Requests that require access tokens

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/access-token-guide/requests-that-require-access-tokens

**Contents:**
- Requests that require access tokens#

Each privileged operation requires a token in a specific format. Examples of privileged operations include:

For more information, see Supported values for the Vivox action claim.

---

## Send and receive group text messages

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/messaging/group-text-messages

**Contents:**
- Send and receive group text messages#

If the game has joined the channel with text enabled, the user can send and receive group text messages.

To send messages, the game uses the IChannelSession::BeginSendText method. When this SDK call is made, other users in that channel receive a IChannelSession::EventTextMessageReceived event.

The following code displays an example of the process for sending and receiving group text messages:

**Examples:**

Example 1 (unknown):
```unknown
IChannelSession::BeginSendText
```

Example 2 (unknown):
```unknown
IChannelSession::EventTextMessageReceived
```

Example 3 (javascript):
```javascript
/* . . . */
ChannelId Channel = ChannelId(kDefaultIssuer, "example_channel", kDefaultDomain);
FString Message = TEXT("Example channel message."); 
IChannelSession::FOnBeginSendTextCompletedDelegate SendChannelMessageCallback;

SendChannelMessageCallback.BindLambda([this, Channel, Message](VivoxCoreError Error)
{
    if(VxErrorSuccess == Error)
    {
        UE_LOG(MyLog, Log, TEXT("Message sent to %s: %s\n"), *Channel.Name(), *Message);
    }
});
MyChannelSession.BeginSendText(Message, SendChannelMessageCallback);
/* . . . */

void OnChannelTextMessageReceived(const IChannelTextMessage &Message)
{
    UE_LOG(MyLog, Log, TEXT("%s: %s\n"), *Message.Sender().Name(), *Message.Message());
}
```

Example 4 (javascript):
```javascript
/* . . . */
ChannelId Channel = ChannelId(kDefaultIssuer, "example_channel", kDefaultDomain);
FString Message = TEXT("Example channel message."); 
IChannelSession::FOnBeginSendTextCompletedDelegate SendChannelMessageCallback;

SendChannelMessageCallback.BindLambda([this, Channel, Message](VivoxCoreError Error)
{
    if(VxErrorSuccess == Error)
    {
        UE_LOG(MyLog, Log, TEXT("Message sent to %s: %s\n"), *Channel.Name(), *Message);
    }
});
MyChannelSession.BeginSendText(Message, SendChannelMessageCallback);
/* . . . */

void OnChannelTextMessageReceived(const IChannelTextMessage &Message)
{
    UE_LOG(MyLog, Log, TEXT("%s: %s\n"), *Message.Sender().Name(), *Message.Message());
}
```

---

## Message types

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/messaging/message-types

**Contents:**
- Message types#

Games need a message handler to process Vivox event and response messages.

To determine the message type, check the vx_message_base_t.type field.

The following code displays an example of these message types:

If the message is of the type vx_resp_base, then the game can look at the vx_resp_base.type field to determine the actual response type.

The following code displays an example of this message type:

If the message is of the type vx_evt_base, then the game can look at the vx_evt_base.type field to determine the actual response type.

The following code displays an example of this message type:

**Examples:**

Example 1 (unknown):
```unknown
vx_message_base_t.type
```

Example 2 (unknown):
```unknown
msg_response
```

Example 3 (unknown):
```unknown
x_resp_base_t
```

Example 4 (unknown):
```unknown
vx_resp_base_t
```

---

## Text-to-speech events

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/text-to-speech/tts-events

**Contents:**
- Text-to-speech events#

Text-to-speech (TTS) reports the following events through the standard Vivox events reporting mechanism:

vx_evt_tts_injection_started - Raised when a TTS message has finished preparing for playback and is starting to play.

vx_evt_tts_injection_ended - Raised when a TTS message finishes playback.

vx_evt_tts_injection_failed - Raised when playback of a TTS message has failed.

**Examples:**

Example 1 (unknown):
```unknown
vx_evt_tts_injection_started
```

Example 2 (unknown):
```unknown
num_consumers
```

Example 3 (unknown):
```unknown
tts_dest_remote_transmission
```

Example 4 (unknown):
```unknown
tts_dest_local_playback
```

---

## Channel types

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/channel-types

**Contents:**
- Channel types#
- Echo channels#
- Non-positional channels#
- Positional channels#

Vivox uses the following types of channels:

The channel type is indicated by the character in the middle of a given channel's SIP address. For more information, see Channel URI structure.

For convenience purposes, Vxc.h includes functions that you can use to construct channel URIs of each type. You can use any method of string creation for these channel URIs. vx_uri_to_string() returns a textual representation of the compositional elements of a Vivox URI, which is suitable for logging.

Note: When working with both positional and non-positional channels, join the positional channel first, followed by the non-positional channels.

Echo channels include an -e- in their SIP address, which is displayed in the following example:

Developers can use these channels to give users a place to test their microphones, or as a general way to test connectivity to the Vivox voice servers.

Non-positional channels, also referred to as 2D channels, include a -g- in their SIP address, which is displayed in the following example:

Developers can use these channels to allow for level-wide audio and text channels that players can connect to.

Examples of scenarios where non-positional channels are often used include teams and squads in first-person shooters, and party chat in MMOs.

Non-positional channels are typically the most used channel types in a Vivox implementation.

Positional channels, also referred to as 3D channels, include a -d- in their SIP address, which is displayed in the following example:

Developers can use these channels to provide voice chat that is a part of a world, with player voices attenuating and panning to make it seem like they are speaking from where they are positioned in the game world.

Note: You can join only one positional channel at a time.

You can parametrize positional channels with certain values that change how the positioning of the player affects their voice. For more information, refer to Positional channel parameters.

**Examples:**

Example 1 (unknown):
```unknown
// The Vivox SDK must be initialized
char *uri = vx_get_echo_channel_uri("channel-name", "domain.vivox.com", "issuer");
// returns "sip:confctl-e-issuer.channel-name@domain.vivox.com"
. . .
vx_free(uri);
```

Example 2 (unknown):
```unknown
// The Vivox SDK must be initialized
char *uri = vx_get_echo_channel_uri("channel-name", "domain.vivox.com", "issuer");
// returns "sip:confctl-e-issuer.channel-name@domain.vivox.com"
. . .
vx_free(uri);
```

Example 3 (unknown):
```unknown
// The Vivox SDK must be initialized
char *uri = vx_get_general_channel_uri("channel-name", "domain.vivox.com", "issuer");
// returns "sip:confctl-g-issuer.channel-name@domain.vivox.com"
. . .
vx_free(uri);
```

Example 4 (unknown):
```unknown
// The Vivox SDK must be initialized
char *uri = vx_get_general_channel_uri("channel-name", "domain.vivox.com", "issuer");
// returns "sip:confctl-g-issuer.channel-name@domain.vivox.com"
. . .
vx_free(uri);
```

---

## Troubleshooting Bluetooth SCO underrun issues

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/android/troubleshooting-bluetooth-sco-underun-issues

**Contents:**
- Troubleshooting Bluetooth SCO underrun issues#

If your users are experiencing Bluetooth SCO underrun issues, which can cause sound to be choppy on SCO, it is possible to force the Bluetooth device to use A2DP by going into the Android Bluetooth settings and disabling the “Phone Calls” option under the Bluetooth device options.

---

## Vivox Unity SDK 16.0.0 upgrade guide

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/v16-upgrade-guide

**Contents:**
- Vivox Unity SDK 16.0.0 upgrade guide#
- SDK setup and initialization#
  - Using Authentication#
  - Using Vivox Developer Portal credentials#
- Login lifecycle changes#
- Blocking players#
- Channel lifecycle changes#
  - Joining and leaving channels#
- Channel transmission#
- Channel participant management#

Note: Follow this guide if you are upgrading from Vivox Unity version 15.1.x or earlier.

This guide provides details on how to upgrade to Vivox Unity SDK version 16.0.0 and highlights the differences between the two SDK versions.

Version 16.0.0 introduces simplified functionalities and requires changes in areas related too:

Replace using VivoxUnity; with using Unity.Services.Vivox; to access Vivox APIs.

The VivoxUnity namespace no longer exists.

Additionally, the Client, which was the original way to initialize the SDK, no longer exists.

If you are using the Unity Authentication package, await UnityServices.InitializeAsync() first, followed by an await AuthenticationService.Instance.SignInAnonymously(), before calling await VivoxService.Instance.InitializeAsync(), as shown in the following example:

You can also use any of the other sign in options provided by the AuthenticationService.Instance as well.

If the project uses Unity Dashboard credentials but does not use the AuthenticationService package, you can enable Test Mode and omit the AuthenticationService sign-in call. To turn on Test Mode, in the Unity Editor, you can go to Project Settings > Services > Vivox and check the box labeled Test Mode.

Note: Enabling Test Mode caches the project’s token signing key on the client and should only be used for testing purposes. Test Mode should be toggled off once a secure server that can vend Vivox Access Tokens to the game client has been provisioned. Disabling Test Mode will remove the token signing key from the project.

The following example shows the initialization setup without the AuthenticationService package and with Test Mode enabled:

If you have legacy Vivox Developer Portal credentials, provide these credentials to VivoxService.Instance.InitializeAsync through InitializationOptions. These credentials can include the signing key for testing purposes.

The following example shows the initialization setup with Vivox Developer Portal credentials:

Note: Before you release your application, remove the signing key, ‘_key’ in the above example, from your options.SetVivoxCredentials(...) call. This also assumes that a server capable of vending Vivox Access Tokens to the game client has been provisioned.

ILoginSession.BeginLogin has been replaced by VivoxService.Instance.LoginAsync(LoginOptions options = null) to log in to the Vivox service.

LoginOptions contains properties for:

ILoginSession.Logout() has been replaced by VivoxService.Instance.LogoutAsync for logging out of the Vivox Service.

Move any logic tied to a LoginSession.PropertyChanged event with the PropertyName "State" and either the LoginState.LoggedIn or LoginState.LoggedOut to a different function. Split out the logic for login and logout to trigger on VivoxService.Instance.LoggedIn and VivoxService.Instance.LoggedOut separately, as shown in the following example.

ILoginSession.SetCrossMutedCommunications has been replaced by VivoxServince.Instance.BlockPlayerAsync(string playerId) and VivoxService.Instance.UnblockPlayerAsync(string playerId) which now require a playerId instead of an AccountId as input.

All logic related to ChannelSession.BeginConnect has been replaced by the following methods:

Note: Set ChatCapability to Text, Audio, or TextandAudio according to channel communication capabilities.

ChannelSession.Disconnect() has been replaced by:

ILoginSession.ChannelSessions functionality is replaced by VivoxService.Instance.ActiveChannels.

VivoxService.Instance.ActiveChannels is a dictionary with all currently active channels as keys, and the list of all channels VivoxParticipants as values.

Implementations previously depended on AudioState and TextState to determine if the user was connected to the channel. Replace events tracking state changes of those two properties with functions tied to VivoxService.Instance.ChannelJoined, and VivoxService.Instance.ChannelLeft.

VivoxService.Instance.ChannelJoined and VivoxService.Instance.ChannelLeft will provide the channel name of the channel joined or left to any callbacks bound to the events.

The follow code sample is an example of these changes:

By default, Channel transmission (the channel being spoken into) switches to the most recently joined channel. You can disable this by setting LoginOptions.DisableAutomaticChannelTransmissionSwap to true before calling VivoxService.Instance.LoginAsync with that LoginOptions object as input.

You can change the channel transmission mode manually by using the VivoxService.Instance.SetChannelTransmissionModeAsync(TransmissionMode transmissionMode, string channelName = null) method.

This API can set a specific channel, all channels, or no channels as the transmitting channel(s) based on the method’s input.

VivoxParticipant has replaced ChannelParticipant.

You can access a collection of each active channel’s VivoxParticipant collection by retrieving the value from a VivoxService.Instance.ActiveChannels index provided a correct key (channel name) is used.

Use VivoxService.Instance.ParticipantAddedToChannel and VivoxServince.Instance.ParticipantRemovedFromChannel to know when a VivoxParticipant was added or removed from a channel.

The events will provide the relevant VivoxParticipant as input to any callbacks bound to them. The VivoxParticipant object contains information about the PlayerId of the participant, the name of the channel the participant joined or left, and other valuable information, including whether the participant is the local player or their display name. These types of events are shown in the following examples:

VivoxParticipant contains information about the current state of the participant, such as:

VivoxParticipant also contains the events:

You can use these events to create a functional volume unit (VU) meter or speech indicator. Connect these events at the individual UI level for each participant.

Note: The object that they are a part of should have a reference to the Participant being represented to easily update the UI.

The following sample class is a simplified version of the RosterItem in the ChatChannelSample:

VivoxParticipant.MutePlayerLocally() and VivoxParticipant.UnmutePlayerLocally() can be used to mute/unmute a specific participant in a channel.

Note: These VivoxParticipant mute/unmute APIs will not affect the local player.

Device management is no longer achieved by leveraging the Client object. You can now make any device adjustments such as muting, changing the volume, or switching the active device using the VivoxService.Instance.

Muting yourself locally has changed to utilize the functions VivoxService.Instance.MuteInputDevice() and VivoxService.Instance.UnmuteInputDevice() instead of having to do this through APIs on an IAudioDevice object, or switching the channel transmission to none. These APIs will mute or unmute the local user’s input device.

You can now adjust device volume by using VivoxService.Instance.SetInputDeviceVolume(int value) and VivoxService.Instance.SetOutputDeviceVolume(int value).

Adjusting the active input or output device can be achieved by calling either VivoxService.Instance.SetActiveOutputDeviceAsync(VivoxOutputDevice device) or VivoxService.Instance.SetActiveInputDeviceAsync(VivoxInputDevice device).

A collection of VivoxInputDevices and VivoxOutputDevices can be retrieved by accessing the properties VivoxService.Instance.AvailableInputDevices and VivoxService.Instance.AvailableOututDevices.

The following example demonstrates how to use the Vivox SDK audio device APIs to populate a dropdown list of input devices that provides functionality for changing the active input device.

Note: This is a simplified version of the AudioDeviceSettings class used in our ChatChannelSample sample that demonstrates how to change the active input/output device, adjust input/output device volume, and react to device list changes.

**Examples:**

Example 1 (unknown):
```unknown
using VivoxUnity;
```

Example 2 (unknown):
```unknown
using Unity.Services.Vivox;
```

Example 3 (unknown):
```unknown
await UnityServices.InitializeAsync()
```

Example 4 (unknown):
```unknown
await AuthenticationService.Instance.SignInAnonymously()
```

---

## Generate a token on a secure server

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/access-token-guide/generate-token-secure-server/generate-token-secure-server-toc

**Contents:**
- Generate a token on a secure server#

Before you release your game to production, you need to generate your access tokens on a secure server and then send those access tokens to the game client. Allowing for token generation on the client during production is a security risk and can cause unexpected token expiration errors for your users.

Note: The actual implementation depends on your server programming environment and language.

---

## User-to-channel muting

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/muting/user-to-channel-muting

**Contents:**
- User-to-channel muting#

A user can mute a whole channel to not hear any participants in that specified channel.

To perform this action, use the vx_req_session_mute_local_speaker request.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_session_mute_local_speaker
```

Example 2 (unknown):
```unknown
vx_req_session_mute_local_speaker_t* reqStruct;
vx_req_session_mute_local_speaker_create(&reqStruct);
reqStruct->session_handle = vx_strdup(“session handle”);
reqStruct->mute_level = 1; // 1 - mute, 0 - unmute
reqStruct->scope = mute_scope_all;

int ret = vx_issue_request3((vx_req_base_t*) reqStruct);
```

Example 3 (unknown):
```unknown
vx_req_session_mute_local_speaker_t* reqStruct;
vx_req_session_mute_local_speaker_create(&reqStruct);
reqStruct->session_handle = vx_strdup(“session handle”);
reqStruct->mute_level = 1; // 1 - mute, 0 - unmute
reqStruct->scope = mute_scope_all;

int ret = vx_issue_request3((vx_req_base_t*) reqStruct);
```

---

## Queue a text-to-speech message

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/text-to-speech/queue-tts-message

**Contents:**
- Queue a text-to-speech message#

Some destinations offer a built-in queuing system. When a new message is injected and there is an ongoing message playing, then the new message is put into a queue. After the ongoing message finishes playing, the next message in the queue automatically starts to play.

Note: The following code example uses the Queued Local Playback destination, but you can inject all text-to-speech (TTS) messages into any of the TTS destinations.

A queue is shared between tts_dest_queued_remote_transmission and tts_dest_queued_remote_transmission_with_local_playback. tts_dest_queued_local_playback has its own queue.

A destination queue can hold up to 10 messages. If a message is added when a destination queue is full, the Vivox SDK discards the message and returns the error tts_error_destination_queue_is_full.

**Examples:**

Example 1 (unknown):
```unknown
vx_tts_utterance_id firstUtteranceId;
vx_tts_speak(managerId, voiceId, "Example message, this is great!", tts_dest_queued_local_playback, &firstUtteranceId);
// Immediately injected and returns tts_status_success

vx_tts_utterance_id secondUtteranceId;
vx_tts_speak(managerId, voiceId, "Really amazing.", tts_dest_queued_local_playback, &secondUtteranceId);
// Put in a queue and returns tts_status_input_text_was_enqueued. After the first message finishes playback, this message will start.
```

Example 2 (unknown):
```unknown
vx_tts_utterance_id firstUtteranceId;
vx_tts_speak(managerId, voiceId, "Example message, this is great!", tts_dest_queued_local_playback, &firstUtteranceId);
// Immediately injected and returns tts_status_success

vx_tts_utterance_id secondUtteranceId;
vx_tts_speak(managerId, voiceId, "Really amazing.", tts_dest_queued_local_playback, &secondUtteranceId);
// Put in a queue and returns tts_status_input_text_was_enqueued. After the first message finishes playback, this message will start.
```

Example 3 (unknown):
```unknown
tts_dest_queued_remote_transmission
```

Example 4 (unknown):
```unknown
tts_dest_queued_remote_transmission_with_local_playback
```

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/v16-upgrade-guide

---

## Initiate a channel join

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/channels/initiate-channel-join

**Contents:**
- Initiate a channel join#

The following code is an example of how to initiate a channel join.

To determine when the channel is joined, the VivoxService.Instance.ChannelJoined event must be bound to. The event itself will fire with the name of the channel, which should be used to trigger the specific events required for that channel. Using a structure for channel names that can help differentiate how different channels will be processed is suggested.

ChannelLeft is an action that will fire upon leaving a channel, with the name of the channel that was left as a parameter.

VivoxService.Instance.JoinGroupChannelAsync, VivoxService.Instance.JoinEchoChannelAsync, and VivoxService.Instance.JoinPositionalChannelAsync have an optional parameter called ChannelOptions. Use ChannelOptions to set behaviour, such as making the channel the active channel being transmitted into when the channel join finishes.

**Examples:**

Example 1 (unknown):
```unknown
using System;
using System.ComponentModel;
using UnityEngine;
using Unity.Services.Vivox;

class JoinChannelExample : MonoBehaviour
{
    // For this example, _loginSession is a signed in ILoginSession.
    . . .
    void OnLoggedIn()
    {
        //These events can be bound anywhere, but keeping them within the lifecycle of an active LoginSession is typically best
        VivoxService.Instance.ChannelJoined += OnChannelJoined
        VivoxService.Instance.ChannelLeft += OnChannelLeft
    }

    void OnChannelJoined(string channelName)
    {
        //Perform actions to react to joining the specific channel with name channelName
        //UI switches, participant UI setup, etc
    }

    void OnChannelLeft(string channelName)
    {
        //Perform cleanup to react to leaving a specific channel with name channelName
    }

    async void JoinChannelAsync(string channelName)
    {
        //Join channel with name channelName and capability for text and audio transmission
        VivoxService.Instance.JoinGroupChannelAsync(channelName, ChatCapability.TextAndAudio);
    }
    . . .
}
```

Example 2 (unknown):
```unknown
using System;
using System.ComponentModel;
using UnityEngine;
using Unity.Services.Vivox;

class JoinChannelExample : MonoBehaviour
{
    // For this example, _loginSession is a signed in ILoginSession.
    . . .
    void OnLoggedIn()
    {
        //These events can be bound anywhere, but keeping them within the lifecycle of an active LoginSession is typically best
        VivoxService.Instance.ChannelJoined += OnChannelJoined
        VivoxService.Instance.ChannelLeft += OnChannelLeft
    }

    void OnChannelJoined(string channelName)
    {
        //Perform actions to react to joining the specific channel with name channelName
        //UI switches, participant UI setup, etc
    }

    void OnChannelLeft(string channelName)
    {
        //Perform cleanup to react to leaving a specific channel with name channelName
    }

    async void JoinChannelAsync(string channelName)
    {
        //Join channel with name channelName and capability for text and audio transmission
        VivoxService.Instance.JoinGroupChannelAsync(channelName, ChatCapability.TextAndAudio);
    }
    . . .
}
```

Example 3 (unknown):
```unknown
VivoxService.Instance.ChannelJoined
```

Example 4 (unknown):
```unknown
ChannelLeft
```

---

## Adaptive chat filter

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/chat-history/adaptive-chat-filter

**Contents:**
- Adaptive chat filter#
- Chat filter language handling per message#
- Speech-to-text#

The chat filter censors words in a message based on a blocklist. This feature is currently server-enabled and needs to be enabled on your account before you can use it. For more information on how to enable this feature, contact your sales representative.

The chat filter handles messages based on the user’s set language. The chat filter expects the language to be set in each request on a per-message basis. This means that if a preferred language is set, the Vivox SDK must specify which language to use on the respective channel class or interface that sends messages to ensure that the proper chat filter is used.

The following is a code example for setting the language of a direct message. The same approach can be used on channel-based messages.

Messages produced through Speech-to-text can be filtered. Words are filtered out when the speech is converted to text. To enable this feature, contact your Vivox representative.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_account_send_msg *req;
vx_req_account_send_msg_create(&req);
req->connector_handle = vx_strdup("c1");
req->account_handle = vx_strdup(".issuer-w-dev.mytestaccountname.");
req->language = vx_strdup(“en-us”);
req->user_uri = vx_strdup("sip:theotheruser@vd1.vivox.com");
req->message_body = vx_strdup("Hey there buddy!");
vx_issue_request2(&req->base);
```

Example 2 (unknown):
```unknown
vx_req_account_send_msg *req;
vx_req_account_send_msg_create(&req);
req->connector_handle = vx_strdup("c1");
req->account_handle = vx_strdup(".issuer-w-dev.mytestaccountname.");
req->language = vx_strdup(“en-us”);
req->user_uri = vx_strdup("sip:theotheruser@vd1.vivox.com");
req->message_body = vx_strdup("Hey there buddy!");
vx_issue_request2(&req->base);
```

---

## NDA platform SDKs

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/nda-platform-access

**Contents:**
- NDA platform SDKs#

Console SDKs are accessible after you request access through the Unity Dashboard. After downloading the packages, follow these steps to add them to your Unity project:

Important: You must be using NDA package versions that match the standard Vivox package that's in your project. For example, if you are using Vivox Unity v16.4.0, use NDA packages that are also v16.4.0. Otherwise you will get compiler errors in your project.

---

## Economy SDK guide

**URL:** https://docs.unity.com/ugs/en-us/manual/economy/manual/SDK-main

**Contents:**
- Economy SDK guide#

This package helps you integrate the Economy service into your game.

The SDK is divided into these features:

---

## In-game control of audio levels

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/in-game-audio-control/in-game-control-audio-levels

**Contents:**
- In-game control of audio levels#
- Adjust local user input and output device volume#
- Adjust a channel participant's volume#
- Adjust the volume of a channel#

You can use the Vivox SDK to allow in-game control for audio input and output levels.

The following list details examples of scenarios in which you would want to implement in-game control of audio levels:

Volume settings adjust the apparent loudness of a device or participant. Volume settings have a settable range between -50 to 50 on a logarithmic scale, with +0db (no change) being at 0. Because it is a logarithmic scale, the usable range is generally between -10 and 25, with 25 tending to result in over modulation on a capture device and possible user discomfort on a headset. Note that any increase in the volume above the default value can introduce some amount of distortion, although this is less noticeable at lower levels.

Note: We do not recommend that you allow users to increase their capture device volume above 25.

The Vivox Unity SDK uses the following methods for setting input and output device audio levels:

VivoxService.Instance.SetInputDeviceVolume(int value)

VivoxService.Instance.SetOutputDeviceVolume(int value)

The Vivox Unity SDK uses the following method for setting the volume of individual players that a user is in a channel with:

The Vivox Unity SDK uses the following method for setting the volume of a channel:

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.SetInputDeviceVolume(int value)
```

Example 2 (unknown):
```unknown
VivoxService.Instance.SetOutputDeviceVolume(int value)
```

Example 3 (unknown):
```unknown
VivoxParticipant.SetLocalVolume(int volume)
```

Example 4 (unknown):
```unknown
VivoxParticipant
```

---

## Muting

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/muting/mute-overview

**Contents:**
- Muting#
- Related pages#

The Vivox SDK allows users and moderators to mute other users.

Mute functionality is divided into the following categories:

Local device control: A user's ability to control their own local audio device. Does not involve other users.

User-to-user muting in one or more channels

User-to-channel muting

Moderator muting one or more users: A moderator's ability to mute one or more participants.

The scenarios for user-to-user muting in one or more channels work as a hierarchy, which is detailed in the following table.

Note: Muting is different from blocking (see vx_req_account_create_block_rule). Muting allows a user to not receive voice communication from one or more other users. Blocking allows a user to not see another user on the channel participant list. For example, when User A blocks User B, User A cannot see User B on User A’s channel participant list, and cannot subscribe to User B’s presence.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_account_create_block_rule
```

---

## Android speakerphone audio: Mono or stereo

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/android/android-speakerphone

**Contents:**
- Android speakerphone audio: Mono or stereo#

On Android, Vivox downmixes stereo output into mono output when it detects that the speakerphone is in use. Vivox downmixes the audio output because many Android devices play mono in speakerphone mode. Some devices only play one channel when provided with stereo audio, which makes the panning of 3D channels detrimental.

You can control whether speakerphone downmixing occurs for all devices by providing a list of exceptions by listing which brands, models, and devices to exclude. To change the default speakerphone downmix behavior, add an Android meta-data name-value pair to your application's AndroidManifest.xml. You can learn more about Android meta-data tags in the official Android documentation (Android Developer Documentation).

The metadata name to use is:

Note: There's a default exception that disables speakerphone downmix for Meta/Oculus devices. Changing this value overwrites this default exception. The value must be a string starting with either true or false. To add a list of exceptions, add a comma after true or false and start a comma-delimited list of substrings to search for in the string generated for the current device, formatted as "BRAND MODEL DEVICE". The value string is case-insensitive.

The following are examples of valid value strings:

Note: Replace the example brands, models, and devices with values retrieved by android.os.Build.* strings.

The following is an example of disabling downmixing on Samsung devices:

To modify your application's AndroidManifest.xml, refer to the Unity documentation on Overriding the Android app manifest.

**Examples:**

Example 1 (unknown):
```unknown
"com.vivox.sdk.downmix_speakerphone_enabled"
```

Example 2 (unknown):
```unknown
"com.vivox.sdk.downmix_speakerphone_enabled"
```

Example 3 (unknown):
```unknown
"true,brand,brand_two model,model_two"
```

Example 4 (unknown):
```unknown
"false,model_three device,device_two"
```

---

## Migrate to Unity 6.2 with Analytics SDK 6.1.0

**URL:** https://docs.unity.com/ugs/en-us/manual/analytics/manual/sdk61-migration-guide

**Contents:**
- Migrate to Unity 6.2 with Analytics SDK 6.1.0#
- Request data deletion#

Follow this guide if you're upgrading to Unity 6.2 with Analytics SDK 6.1.0.

Unity 6.2 introduces the Developer Data framework to manage player consent. This framework uses the Unity Engine's EndUserConsent API, replacing the SDK's StartDataCollection() and StopDataCollection() methods. This guide details these changes and how to adapt your project.

Note: If you are using SDK version 6.1.0 or later in an earlier version of Unity where the Developer Data framework is not present, the old methods are still present. This guide only applies to using the latest SDK versions in the latest Unity Editor versions.

You no longer need to manually activate the Analytics SDK after calling UnityServices.InitializeAsync(). To use the new Developer Data framework, you must remove all usages of the StartDataCollection() and StopDataCollection() methods.

Instead, use the EndUserConsent.SetUserConsent(...) method to grant consent for AnalyticsIntent. You only need to do this once. The Developer Data framework stores this consent status locally, and the Analytics SDK activates automatically based on this stored value.

The following code demonstrates how to record player consent for data collection:

To learn more about managing consent with the EndUserConsent API, refer to the Developer Data framework documentation.

If UnityServices.InitializeAsync() has already been called, setting AnalyticsIntent to Granted immediately starts data collection. If you set AnalyticsIntent to Granted first, data collection starts during UnityServices.InitializeAsync() instead.

If the AnalyticsIntent status is set to any value other than Granted, at any time, data collection stops immediately or does not start during initialization.

The Analytics SDK still controls data deletion requests, using the existing RequestDataDeletion() method. However, the SDK no longer automatically stops collecting data when doing so. In order to make a data deletion request, you must deny consent using the EndUserConsent API first.

**Examples:**

Example 1 (unknown):
```unknown
EndUserConsent
```

Example 2 (unknown):
```unknown
StartDataCollection()
```

Example 3 (unknown):
```unknown
StopDataCollection()
```

Example 4 (unknown):
```unknown
UnityServices.InitializeAsync()
```

---

## Channel messages

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/messaging/channel-messages

**Contents:**
- Channel messages#
- Send channel messages#
- Receive channel messages#
- Channel text message history#
- Edit channel messages#
- Delete channel messages#

VivoxMessages can be sent into specific Vivox channels that have Text enabled using a ChatCapability of either TextOnly or TextAndAudio.

You can send a message to a channel by using VivoxService.Instance.SendChannelTextMessageAsync(string channelName, string message) with channelName being the destination channel of the message, and message being the message to be sent.

The following code snippet is an example implementation for sending a message to a set Lobby channel and pulling the text from a MessageInputField InputField object.

In order to receive messages from channels with text enabled, the VivoxService.Instance.ChannelMessageReceived Action must be subscribed to.

VivoxMessages sent to a channel are nearly identical to directed messages, with the addition of the ChannelName in VivoxMessage.ChannelName, and with the VivoxMessage.FromSelf field set based on whether the message is from the currently logged in user or not.

The following code snippet is an example line of subscribing to the Action, along with an example of pulling all available information out of the VivoxMessage on receipt.

Vivox allows for users to access the history of a channel's text activity using VivoxService.Instance.GetChannelTextMessageHistoryAsync(string channelName, int requestSize = 10, ChatHistoryQueryOptions chatHistoryQueryOptions = null), with channelName being the name of the channel to request the history of, requestSize being the number of messages to return at a maximum (10 is the default), and chatHistoryQueryOptions being an optional parameter which can do things like specifying a word or phrase contained in the messages, a specific PlayerId sender of the messages, or times to query before or after. More specific information on ChatHistoryQueryOptions can be found in Chat history query options.

The messages returned in IReadOnlyCollection are listed in order from newest message to oldest message.

Note: Chat history is stored for 7 days. If Text Evidence Management is in use, the storage period for chat history is extended to 30 days.

The following code snippet is an example of pulling at most recent 25 messages from a set LobbyChannelName, then logging the sender's display name and the message in order from oldest to newest, without using the optional ChatHistoryQueryOptions.

Vivox allows users to edit the text of messages they have sent into a channel using VivoxService.Instance.EditChannelTextMessageAsync(string channelName, string messageId, string newMessage). channelName being the name of the channel that the message was sent to, the messageId being the id of the message to change, and newMessage being the updated text of the message.

When a message has been successfully edited by anyone in a channel, everyone in the channel will receive a VivoxService.Instance.ChannelMessageEdited Action containing the VivoxMessage that was edited, with the updated MessageText.

Vivox allows for users to delete messages they have sent into a channel using VivoxService.Instance.DeleteChannelTextMessageAsync(string channelName, string messageId). channelName being the name of the channel that the message was sent to, and the messageId being the id of the message to delete.

When a message has been successfully deleted by anyone in a channel, everyone in the channel will receive a VivoxService.Instance.ChannelMessageDeleted Action containing the VivoxMessage that was deleted.

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.SendChannelTextMessageAsync(string channelName, string message)
```

Example 2 (unknown):
```unknown
channelName
```

Example 3 (unknown):
```unknown
public async void SendMessageAsync()
{
    if (string.IsNullOrEmpty(MessageInputField.text))
    {
        return;
    }

    VivoxService.Instance.SendChannelTextMessageAsync(LobbyChannelName, MessageInputField.text);
    MessageInputField.text = string.Empty;
}
```

Example 4 (unknown):
```unknown
public async void SendMessageAsync()
{
    if (string.IsNullOrEmpty(MessageInputField.text))
    {
        return;
    }

    VivoxService.Instance.SendChannelTextMessageAsync(LobbyChannelName, MessageInputField.text);
    MessageInputField.text = string.Empty;
}
```

---

## Access token terminology

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/access-token-guide/access-token-terminology

**Contents:**
- Access token terminology#

Base64 encoding with all trailing '=' removed, '+' changed to '-', and '/' changed to '_'.

For more information, see the IETF documentation on RFC 7515.

The following example is an implementation of base64url encoding in PHP:

The following example is an implementation of base64url encoding in Python:

For an example implementation of base64url encoding in C#, see the IETF documentation on RFC 7515.

URL and filename safe string

A string composed of characters found in the URL and filename safe Base64 alphabet.

For more information, see the IETF documentation on RFC 4648.

**Examples:**

Example 1 (unknown):
```unknown
function base64url_encoded($str)
{
    return rtrim(strtr(base64_encoded($str), '+/', '-_'), '=')
}
```

Example 2 (unknown):
```unknown
function base64url_encoded($str)
{
    return rtrim(strtr(base64_encoded($str), '+/', '-_'), '=')
}
```

Example 3 (python):
```python
import base64
def base64url_encode(s):
"""Return a base64url-encoded str"""
return base64.urlsafe_b64encode(s).rstrip('=')
```

Example 4 (python):
```python
import base64
def base64url_encode(s):
"""Return a base64url-encoded str"""
return base64.urlsafe_b64encode(s).rstrip('=')
```

---

## Offline direct messages

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/chat-history/offline-direct-messages

**Contents:**
- Offline direct messages#

Note: This feature is currently only available in the Core SDK and Unity SDK v16.

When you send a message to an offline user you will receive a evt_account_send_message_failed event. This can be used to relay the fact that the user receiving said message is offline. The message gets archived and when the user that received that message signs in, they can retrieve their account chat history to see the message that was sent to them while offline.

---

## In-game control of audio levels

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/audio/in-game-audio-control/in-game-control-audio-levels

**Contents:**
- In-game control of audio levels#

You can use the Vivox SDK to allow in-game control for audio input and output levels.

The following list details examples of scenarios in which you would want to implement in-game control of audio levels:

Volume settings adjust the apparent loudness of a device or participant. Volume settings have a settable range between -50 to 50 on a logarithmic scale, with +0db (no change) being at 0. Because it is a logarithmic scale, the usable range is generally between -10 and 25, with 25 tending to result in over modulation on a capture device and possible user discomfort on a headset. Note that any increase in the volume above the default value can introduce some amount of distortion, although this is less noticeable at lower levels.

Note: We do not recommend that you allow users to increase their capture device volume above 25.

In the Vivox Unreal SDK, use IAudioDevices::SetVolumeAdjustment(int value) to allow users to change the gain (volume) of their audio device.

The following code displays an example of an implementation for setting both input and output audio levels:

In the Vivox Unreal SDK, use IParticipantProperties::BeginSetLocalVolumeAdjustment to allow users to change the volume of a participant within a channel.

The following code displays an example of an implementation for setting another participant's volume:

**Examples:**

Example 1 (unknown):
```unknown
IAudioDevices::SetVolumeAdjustment(int value)
```

Example 2 (unknown):
```unknown
void AdjustOutputVolume(int value)
{
    VivoxVoiceClient.AudioOutputDevices().SetVolumeAdjustment(value);
}

void AdjustInputVolume(int value)
{
    VivoxVoiceClient.AudioInputDevices().SetVolumeAdjustment(value);
}
```

Example 3 (unknown):
```unknown
void AdjustOutputVolume(int value)
{
    VivoxVoiceClient.AudioOutputDevices().SetVolumeAdjustment(value);
}

void AdjustInputVolume(int value)
{
    VivoxVoiceClient.AudioInputDevices().SetVolumeAdjustment(value);
}
```

Example 4 (unknown):
```unknown
IParticipantProperties::BeginSetLocalVolumeAdjustment
```

---

## Player management

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/player-management/player-management-toc

**Contents:**
- Player management#

---

## Supported platforms and versions

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/supported-platforms

**Contents:**
- Supported platforms and versions#
- Terminology#
  - Supported#
  - Limited versions#
  - Experimental versions#
  - Deprecated versions#

Experimental is for the latest/edge releases that are available but not yet Supported or default. Use at your own risk. This encompasses all Alpha, Beta and Prerelease versions. Issues and improvement requests will be tracked and addressed based on frequency and severity.

Supported is known-good and actively supported by the organization. Supported versions are the default choice when creating a new configuration and represent the versions we have the highest confidence in. We recommend anyone experiencing issues upgrade/migrate to Supported versions if you are using an SDK version where a platform wasn’t fully supported. Issues and improvement requests will be tracked and addressed based on frequency and severity.

Limited indicates that the platform does not have full support from our team. While we will address security issues and critical bugs on the platform, the priority of implementing platform-specific fixes and improvements is low. We do still include the platform in our releases.

Deprecated means "do not use". This technology is scheduled to be removed as part of the deprecation schedule strategy. Deprecated also means that the Unity Editor team no longer supports the version as it has fallen out of Long Term Support (LTS). Contact us if that causes problems for your company, and we can negotiate to make that version available longer. Issues and improvement requests will not be tracked or addressed.

The Vivox Unreal SDK is available for the following platforms and versions:

Note: The Unreal SDK does not currently support using Vivox with Unreal's Live Coding feature in Unreal 5.2.

Note: Access to some platform-specific Vivox SDKs and documentation requires approval. After approval, you access the platform-specific SDKs and documentation from the Unity Dashboard. For more information, refer to How do I get approved for NDA protected platforms? in the Unity Support Knowledge Base.

---

## Unity Build Automation Editor package

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/build-automation-editor-package

**Contents:**
- Unity Build Automation Editor package#
- Get Started#
  - Configure the UBA package#
- Shelve and Build#
- Shelving#
  - Missing files during build#

Starting from Unity 6, the Unity Build Automation package (version 2.0.0 and later) works seamlessly with the new build profiles that Unity 6 introduced. This package provides a set of services you can use to build and distribute your applications efficiently. This enhanced workflow helps you manage multiple deployment targets and offloads resource-intensive build tasks to the cloud to accelerate your development cycle.

In addition to standard local builds, when you create new build profiles within the Unity Editor, you can opt to begin a cloud build.

If you select the Cloud Build option, the Unity Build Automation (UBA) system automatically creates a new build target based on the currently active build profile and remote builder configuration. This integration streamlines the build process and allows you to build your local changes in the cloud without having to manually commit your changes to the repository by using the Shelve and Build feature.

Get started in the Unity Editor:

This action prompts you to install the latest version of the UBA package.

After you install the package, configure your settings:

The Shelve and Build feature allows you to temporarily shelve pending changes when you initiate a cloud build. This feature is exclusive to projects that use Unity Version Control (UVCS).

If you have pending changes in your project when you start a cloud build, UBA prompts you to shelve and build.

Note: If UBA doesn't detect your pending changes, try the following fixes:

Shelving is a process in Unity Version Control that you can use to temporarily store pending changes without committing them to the repository. For example, you can shelve pending changes for the following reasons:

When you trigger a cloud build with uncommitted changes and select the Shelve and Build feature, Unity automates this shelving process.

When you use Shelve and Build, be aware that UBA doesn't include files marked as private (unversioned) in the shelveset. To ensure all necessary files are included in your build:

**Examples:**

Example 1 (unknown):
```unknown
cm add <file_path>
```

Example 2 (unknown):
```unknown
.ignore.conf
```

---

## Positional channel configuration

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/positional-channel-configuration

**Contents:**
- Positional channel configuration#

If a game has joined a positional channel, it must frequently update the Vivox SDK with the position and orientation of the user in the 3D space that is associated with that channel. The client does this by sending a vx_req_session_set_3d_position message to the Vivox SDK.

Positional channels use a right-hand coordinate system. When the player avatar is standing facing directly forward, the following criteria applies:

When a player joins a positional channel, the player avatar is placed at a null position until they move to a position.

You can set the position of the avatar's mouth independently of the avatar's ears. These are respectively referred to as the speaker position and the listener position, and they share coordinates in most scenarios. Because the positions are independent, this allows for effects like an "audio zoom," where a character who is using something like a shotgun microphone could be configured to hear voice as though they were closer to the target, but they are still only speaking to those immediately around them. In addition to position, the listener can also have its orientation set to accurately render audio panning. However, the speaker cannot have its orientation set; spoken voices are treated as omni-directional audio “point sources” and are unaffected by the speaker’s orientation.

After you set up and join a positional channel with any configured 3D properties, you then report your actor's position and orientation to the Vivox SDK.

The following code displays an example of how to set a user's position in a positional channel:

The most frequently issued request is vx_req_session_set_3d_position. Successful responses from this request are ignored.

To save processing time in your game, and because responses with errors are still returned, to prevent the Vivox SDK from returning successful responses to this request, set the vx_req_session_set_3d_position.req_disposition_type to req_disposition_no_reply_required.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_session_set_3d_position
```

Example 2 (unknown):
```unknown
. . .
vx_req_session_set_3d_position *req;
vx_req_session_set_3d_position_create(&req);
req->session_handle = vx_strdup("mychannel");
req->req_disposition_type = req_disposition_no_reply_required;
req->speaker_position[0] = 10;
req->speaker_position[1] = 0;
req->speaker_position[2] = 0;
req->listener_position[0] = 10;
req->listener_position[1] = 0;
req->listener_position[2] = 0;
req->listener_at_orientation[0] = 0;
req->listener_at_orientation[1] = 0;
req->listener_at_orientation[2] = -1;
req->listener_up_orientation[0] = 0;
req->listener_up_orientation[1] = 1;
req->listener_up_orientation[2] = 0;
vx_issue_request3(&req->base, &request_count);
. . .
```

Example 3 (unknown):
```unknown
. . .
vx_req_session_set_3d_position *req;
vx_req_session_set_3d_position_create(&req);
req->session_handle = vx_strdup("mychannel");
req->req_disposition_type = req_disposition_no_reply_required;
req->speaker_position[0] = 10;
req->speaker_position[1] = 0;
req->speaker_position[2] = 0;
req->listener_position[0] = 10;
req->listener_position[1] = 0;
req->listener_position[2] = 0;
req->listener_at_orientation[0] = 0;
req->listener_at_orientation[1] = 0;
req->listener_at_orientation[2] = -1;
req->listener_up_orientation[0] = 0;
req->listener_up_orientation[1] = 1;
req->listener_up_orientation[2] = 0;
vx_issue_request3(&req->base, &request_count);
. . .
```

Example 4 (unknown):
```unknown
vx_req_session_set_3d_position
```

---

## In-game audio control

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/in-game-audio-control/in-game-audio-control-toc

**Contents:**
- In-game audio control#

---

## Set Chat Markers

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/set-chat-markers

**Contents:**
- Set Chat Markers#

The set Chat Markers API gives you control over the creation of Chat Markers. You can use Chat Markers to mark messages as read. It also provides data for the Conversation list API. This functionality works in both channels and DMs.

To set a conversation's read marker, sign in to the application and use vx_req_account_chat_history_set_marker to make a request. This request returns whether the marker was created or not.

Note: The marker is created even if the URI or message doesn’t exist.

The following code is an example of how to set a conversation marker.

After you retrieve this information, the game must process the vx_resp_account_chat_history_set_marker_t response:

**Examples:**

Example 1 (unknown):
```unknown
vx_req_account_chat_history_set_marker *req;
vx_req_account_chat_history_set_marker_create(&req);
req->with_uri = vx_strdup(”sip:confctl-g-issuer.room_name.@domain”); // Optional parameter, if not set it will use the channel you’ve joined most recently
req->message_id = vx_strdup(“1234567890”); // The message id you’re setting the marker for
req->seen_at = 1234567890; // Optional, unix timestamp which defaults to the current time if not set
vx_issue_request2(&req->base);
```

Example 2 (unknown):
```unknown
vx_req_account_chat_history_set_marker *req;
vx_req_account_chat_history_set_marker_create(&req);
req->with_uri = vx_strdup(”sip:confctl-g-issuer.room_name.@domain”); // Optional parameter, if not set it will use the channel you’ve joined most recently
req->message_id = vx_strdup(“1234567890”); // The message id you’re setting the marker for
req->seen_at = 1234567890; // Optional, unix timestamp which defaults to the current time if not set
vx_issue_request2(&req->base);
```

Example 3 (unknown):
```unknown
void HandleAccountChatHistorySetMarkerResponse(vx_resp_account_chat_history_set_marker_t *resp)
{
	// Handles the response, nothing to really be done here unless the request fails
}
```

Example 4 (unknown):
```unknown
void HandleAccountChatHistorySetMarkerResponse(vx_resp_account_chat_history_set_marker_t *resp)
{
	// Handles the response, nothing to really be done here unless the request fails
}
```

---

## Example: Join_Muted token

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/access-token-guide/access-token-examples/example-join-muted-token

**Contents:**
- Example: Join_Muted token#

The token in this example allows the user "jerky" to join the channel "sip:confctl-g-blindmelon.testchannel\@tla.vivox.com" in a muted state.

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

**Examples:**

Example 1 (unknown):
```unknown
"sip:confctl-g-blindmelon.testchannel\@tla.vivox.com"
```

Example 2 (unknown):
```unknown
{
    "vxi":542680,
    "f":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"join_muted",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 3 (unknown):
```unknown
{
    "vxi":542680,
    "f":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"join_muted",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 4 (unknown):
```unknown
e30.eyJ2eGkiOjU0MjY4MCwiZiI6InNpcDouYmxpbmRtZWxvbi1BcHBOYW1lLWRldi5qZXJreS5AdGxhLnZpdm94LmNvbSIsImlzcyI6ImJsaW5kbWVsb24tQXBwTmFtZS1kZXYiLCJ2eGEiOiJqb2luX211dGVkIiwidCI6InNpcDpjb25mY3RsLWctYmxpbmRtZWxvbi1BcHBOYW1lLWRldi50ZXN0Y2hhbm5lbEB0bGEudml2b3guY29tIiwiZXhwIjoxNjAwMzQ5NDAwfQ.N6sZL3F3e-p2KLQlMweXnbGNzE7Qc91rn_uqCEtRjsc
```

---

## Channel name criteria

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/channels/channel-names

**Contents:**
- Channel name criteria#

When you create a channel name, the following criteria apply:

0x30-0x39, 0x41-0x5A, 0x61-0x7A

Note: You can only use the percent sign for URL encoding. Follow the percent sign by two uppercase hex characters.

Note: Channel names can have mixed case for clarity. However, do not use different case versions of the same channel. For example, do not use "MyChannel" and "mychannel" at the same time, or audio connection issues can occur.

---

## Add the Vivox Unreal plug-in to all projects

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/implement-vivox-unreal/add-vivox-unreal-plugin-all-projects

**Contents:**
- Add the Vivox Unreal plug-in to all projects#

To add the Vivox Core Unreal 4 plug-in to all projects, complete the following steps.

Important: If you installed Unreal in the default location, this location is usually at the following path: /Users/Shared/Epic Games/UE_4.2X/Engine/Plugins/VivoxCore.

**Examples:**

Example 1 (unknown):
```unknown
C:\vivox-unreal4-sdk-0.1.2.xxxx-win64)
```

Example 2 (unknown):
```unknown
C:\Program Files\Epic Games\UE_4.19\Engine\Plugins)
```

Example 3 (unknown):
```unknown
/Users/Shared/Epic Games/UE_4.2X/Engine/Plugins/VivoxCore
```

---

## Example: Python

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/access-token-guide/generate-token-secure-server/example-python

**Contents:**
- Example: Python#

The following code is an example of how to generate access token in Python.

**Examples:**

Example 1 (python):
```python
#!/usr/env/python

#############################################
# Vivox Token Generation Example - Python
#############################################

import base64
import hashlib
import hmac
import json


def b64url(value):
    """Return a base64url-encoded str without trailing ='s"""
    return base64.urlsafe_b64encode(value).rstrip('=')


def vx_generate_token(key, issuer, exp, vxa, vxi, f, t):
    # Create dictionary of claims
    claims = {
        'iss': issuer,
        'exp': exp,
        'vxa': vxa,
        'vxi': vxi,
        'f': f,
        't': t
    }

    # Header is static - base64url encoded {}
    header = b64url('{}')  # Can also be defined as a constant "e30"

    # Encode claims payload
    json_payload = b64url(json.dumps(claims))

    # Join segments to prepare for signing
    segments = [header, json_payload]
    to_sign = b'.'.join(segments)

    # Sign token with key and HMACSHA256
    sig = hmac.new(key, to_sign, hashlib.sha256).digest()
    segments.append(b64url(sig))

    # Join all 3 parts of token with . and return
    return b'.'.join(segments)


if __name__ == '__main__':
    # Example usage
    token = vx_generate_token('secret', 'issuer', 1559359105, 'join', 123456, 'sip:.username.@domain.vivox.com', 'sip:confctl-g-channelname@domain.vivox.com')
    print(token)
```

Example 2 (python):
```python
#!/usr/env/python

#############################################
# Vivox Token Generation Example - Python
#############################################

import base64
import hashlib
import hmac
import json


def b64url(value):
    """Return a base64url-encoded str without trailing ='s"""
    return base64.urlsafe_b64encode(value).rstrip('=')


def vx_generate_token(key, issuer, exp, vxa, vxi, f, t):
    # Create dictionary of claims
    claims = {
        'iss': issuer,
        'exp': exp,
        'vxa': vxa,
        'vxi': vxi,
        'f': f,
        't': t
    }

    # Header is static - base64url encoded {}
    header = b64url('{}')  # Can also be defined as a constant "e30"

    # Encode claims payload
    json_payload = b64url(json.dumps(claims))

    # Join segments to prepare for signing
    segments = [header, json_payload]
    to_sign = b'.'.join(segments)

    # Sign token with key and HMACSHA256
    sig = hmac.new(key, to_sign, hashlib.sha256).digest()
    segments.append(b64url(sig))

    # Join all 3 parts of token with . and return
    return b'.'.join(segments)


if __name__ == '__main__':
    # Example usage
    token = vx_generate_token('secret', 'issuer', 1559359105, 'join', 123456, 'sip:.username.@domain.vivox.com', 'sip:confctl-g-channelname@domain.vivox.com')
    print(token)
```

---

## Initial implementation check list

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/integration-guide/initial-implementation-task-list

**Contents:**
- Initial implementation check list#

The following list details a condensed version of the tasks that are required for completing a basic Vivox voice integration. For a more in-depth explanation follow the Integration guide.

Add Vivox DLLs and header files to the client build process.

Select a client code path for logging the user in to the Vivox network (for example, a character sign-in, a lobby, or a match). See Planning and organization for more details on in-game flows.

Use anonymous logins and the echo channel (sip:confctl-2@\<your vivox domain>) until basic integration testing is complete.

Add console commands to perform the following actions:

Switch to enforced access-token-authenticated logins after basic settings have been tested and are verified to be working.

If you are using 3D channels, send positional updates for the channel by using the Vivox APIs as necessary (not for every frame).

Create a code handler for the vx_evt_account_login_state_change event.

Create a code handler for the vx_evt_media_stream_updated event, check status_code and status_string.

This handles a disconnected state, such as a voice server dropping an end user.

Create a code handler for the vx_evt_participant_updated event for speaking indicators.

Update the existing event handlers to update UI elements to reflect the correct state and expose the User settings you created.

**Examples:**

Example 1 (unknown):
```unknown
sip:confctl-2@\<your vivox domain>
```

Example 2 (unknown):
```unknown
vx_evt_account_login_state_change
```

Example 3 (unknown):
```unknown
vx_evt_media_stream_updated
```

Example 4 (unknown):
```unknown
status_code
```

---

## Positional channel configuration

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/channels/positional-channel-configuration

**Contents:**
- Positional channel configuration#

If a game has joined a positional channel, it must frequently update the Vivox SDK with the position and orientation of the user in the 3D space that is associated with that channel. The client does this by calling the ChannelSession::Set3DPosition method on a ChannelSession of ChannelType::Positional that has its AudioState connected. You can additionally have your TextState connected, but to set your position, you must connect to audio.

A participant's location in a positional channel affects who they can hear, and also the perceived loudness and direction of other players’ voices. With text enabled, location limits the delivery of text messages to only users within the audible vicinity of the sender.

Positional channels use a right-hand coordinate system. When the player avatar is standing facing directly forward, the following criteria applies:

When a player joins a positional channel, the player avatar is placed at a null position until they move to a position.

Note: Unreal Engine 4 uses a left-hand, Z-up world coordinate system. Do not perform this conversion manually when using Set3DPosition because the conversion between coordinate systems is handled internally by the Vivox SDK. However, you can provide an actor’s position and orientation by using standard AActor methods.

You can set the position of the avatar's mouth independently of the avatar's ears. These are respectively referred to as the speaker position and the listener position, and they share coordinates in most scenarios. Because the positions are independent, this allows for effects like an "audio zoom," where a character who is using something like a shotgun microphone could be configured to hear voice as though they were closer to the target, but they are still only speaking to those immediately around them. In addition to position, the listener can also have its orientation set to accurately render audio panning. However, the speaker cannot have its orientation set; spoken voices are treated as omni-directional audio “point sources” and are unaffected by the speaker’s orientation.

After you set up and join a positional channel with any configured 3D properties, you then report your actor's position and orientation to the Vivox SDK.

The following code displays an example of how to set a user's position in a positional channel:

This code displays methods for limiting the number of 3D positional updates that are sent. For example, a time tracking technique is used in the Tick() function to rate limit update attempts to a reasonable number of times per second. Also, in the sample method Update3DPosition(), a CachedProperty template class caches the player’s position and orientation. If the player has moved since last calling IChannelSession::Set3DPosition, then utility functions check and reset their Boolean triggers.

**Examples:**

Example 1 (unknown):
```unknown
ChannelSession::Set3DPosition
```

Example 2 (unknown):
```unknown
ChannelSession
```

Example 3 (unknown):
```unknown
ChannelType::Positional
```

Example 4 (unknown):
```unknown
Set3DPosition
```

---

## Get raw audio of synthesized speech

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/text-to-speech/get-raw-audio-synthesized-speech

**Contents:**
- Get raw audio of synthesized speech#

You can synthesize speech into an audio buffer for your direct use rather than having it maintained internally by the Vivox SDK. vx_tts_speak_to_buffer synthesizes the speech signal and returns it in the form of a vx_tts_utterance_t struct. This struct includes a pointer to the raw audio data, and metadata, such as buffer length and audio format properties.

Note: After you are finished using the text-to-speech (TTS) utterance, you must destroy it to avoid memory leaks.

**Examples:**

Example 1 (unknown):
```unknown
vx_tts_speak_to_buffer
```

Example 2 (unknown):
```unknown
vx_tts_utterance_t
```

Example 3 (unknown):
```unknown
vx_tts_utterance_t *utterance = NULL;
vx_tts_status status = vx_tts_speak_to_buffer(managerId, voiceId, "Gimme the audio samples", &utterance);
// If succeeds, utterance will contain the audio samples and the metadata for the synthesized speech.
```

Example 4 (unknown):
```unknown
vx_tts_utterance_t *utterance = NULL;
vx_tts_status status = vx_tts_speak_to_buffer(managerId, voiceId, "Gimme the audio samples", &utterance);
// If succeeds, utterance will contain the audio samples and the metadata for the synthesized speech.
```

---

## Manage servers

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/guides/manage-servers

**Contents:**
- Manage servers#
- Start a server#
- Stop a server#
- Restart a server#
- View server events#
- View server files#

You can manage your server instances from the Unity Dashboard.

Refer to the following sections to learn how to:

To start an offline server:

Warning: Any number of available servers incurs costs, even without traffic. If you're in development or trying to limit costs in a low traffic environment, set the Minimum available scaling value to 0.

To stop a running server:

To restart a running server:

To view the events from a specific server:

To view the available files, including logs from a specific server:

The files shown in this list include everything under the configured $$log_dir$$ and $$files_dir$$. Refer to Launch parameters.

**Examples:**

Example 1 (unknown):
```unknown
$$log_dir$$
```

Example 2 (unknown):
```unknown
$$files_dir$$
```

---

## Access token format

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/access-token-guide/access-token-format/access-token-format-overview

**Contents:**
- Access token format#

A Vivox access token is similar to a JSON Web Token (JWT), but with a limited set of claims (for example, vxi, f, iss, vxa, t, exp).

The Vivox access token is a string that uses the format header.payload.signature: three base64url-encoded parts that are separated by periods.

Important: Base64url encoding is not the same as Base64 encoding. For more information, see Access token terminology.

**Examples:**

Example 1 (unknown):
```unknown
vxi, f, iss, vxa, t, exp
```

Example 2 (unknown):
```unknown
header.payload.signature
```

---

## Cancel a text-to-speech message

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/text-to-speech/cancel-tts-message

**Contents:**
- Cancel a text-to-speech message#

To cancel a currently playing or enqueued text-to-speech (TTS) message, use ITextToSpeech::CancelMessage() with the ITTSMessage that you want to cancel. For example:

In destinations that contain queues, canceling an ongoing TTS message automatically triggers playback of the next message. Canceling an enqueued TTS message shifts all later messages up one place in the queue.

You can cancel all TTS messages in a destination (ongoing and enqueued), or all TTS messages in all destinations.

**Examples:**

Example 1 (unknown):
```unknown
ITextToSpeech::CancelMessage()
```

Example 2 (unknown):
```unknown
ITTSMessage
```

Example 3 (unknown):
```unknown
ITTSMessage *Message;
MyLoginSession->TTS().Speak("Some Text", TTSDestination::RemoteTransmission, &Message);

MyLoginSession->TTS().CancelMessage(*Message);
```

Example 4 (unknown):
```unknown
ITTSMessage *Message;
MyLoginSession->TTS().Speak("Some Text", TTSDestination::RemoteTransmission, &Message);

MyLoginSession->TTS().CancelMessage(*Message);
```

---

## Initialize Vivox Android services

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/android/initialize-vivox-android-services

**Contents:**
- Initialize Vivox Android services#

To address Android platform requirements, you must initialize the Vivox Android services that are included in the Vivox Java library by invoking the com.vivox.sdk.JniHelpers.init(...) static method. Failing to do so will report SDK_UNINITIALIZED even though the underlying native Vivox SDK library is initialized.

There are six variants of com.vivox.sdk.JniHelpers.init(...). The Android app context is the only non-optional argument:

For an example, see SDKSampleApp/Source/src/main/java/com/vivox/sampleapp/MainActivity.java

**Examples:**

Example 1 (unknown):
```unknown
com.vivox.sdk.JniHelpers.init(...)
```

Example 2 (unknown):
```unknown
SDK_UNINITIALIZED
```

Example 3 (unknown):
```unknown
JniHelpers.init(getApplicationContext(), null, new String[]{"app-so-lib-name"});
```

Example 4 (unknown):
```unknown
libapp-so-lib-name.so
```

---

## Use text-to-speech for incoming messages

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/text-to-speech/tts-incoming-messages

**Contents:**
- Use text-to-speech for incoming messages#

After you set up message polling for the Vivox SDK, you receive events whenever the Vivox SDK receives a message.

If you want to use text-to-speech (TTS) to read incoming messages, use the vx_tts_speak function in the message event handler. The tts_dest_queued_local_playback destination is an ideal choice because it ensures that the messages are queued and that they are only played locally.

The following code displays an example of how to use TTS to read incoming messages:

Note: You can use the evt_message->is_current_user flag to speak only to other users' messages.

**Examples:**

Example 1 (unknown):
```unknown
vx_evt_message
```

Example 2 (unknown):
```unknown
vx_evt_user_to_user_message
```

Example 3 (unknown):
```unknown
vx_tts_speak
```

Example 4 (unknown):
```unknown
tts_dest_queued_local_playback
```

---

## Example: Join token

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/access-token-examples/example-join-token

**Contents:**
- Example: Join token#

The token in this example allows the user "jerky" to join the channel "sip:confctl-g-blindmelon.testchannel\@tla.vivox.com".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

**Examples:**

Example 1 (unknown):
```unknown
"sip:confctl-g-blindmelon.testchannel\@tla.vivox.com"
```

Example 2 (unknown):
```unknown
{
    "vxi":444000,
    "f":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"join",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 3 (unknown):
```unknown
{
    "vxi":444000,
    "f":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"join",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 4 (unknown):
```unknown
e30.eyJ2eGkiOjQ0NDAwMCwiZiI6InNpcDouYmxpbmRtZWxvbi1BcHBOYW1lLWRldi5qZXJreS5AdGxhLnZpdm94LmNvbSIsImlzcyI6ImJsaW5kbWVsb24tQXBwTmFtZS1kZXYiLCJ2eGEiOiJqb2luIiwidCI6InNpcDpjb25mY3RsLWctYmxpbmRtZWxvbi1BcHBOYW1lLWRldi50ZXN0Y2hhbm5lbEB0bGEudml2b3guY29tIiwiZXhwIjoxNjAwMzQ5NDAwfQ.u7us5eCxOBtuEZuDg1HapEEgxLedLaliIy7gOMfbeko
```

---

## Example: Kick tokens

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/access-token-guide/access-token-examples/example-kick-token

**Contents:**
- Example: Kick tokens#
- Signed in user kicking other user from a channel#
- Admin user kicking other user from a channel#
- Admin user kicking other user from a server#

Note: There is no example of a signed in user kicking another user from a server because this action is not possible with the current API.

The token in this example allows the user "beef" to kick the user "jerky" from the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

The token in this example allows the user "blindmelon-AppName-dev-Admin" to kick the user "jerky" from the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

The token in this example allows the user "blindmelon-AppName-dev-Admin" to kick the user "jerky" from the entire server.

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

**Examples:**

Example 1 (unknown):
```unknown
{
    "vxi":665000,
    "sub":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "f":"sip:.blindmelon-AppName-dev.beef.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"kick",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 2 (unknown):
```unknown
{
    "vxi":665000,
    "sub":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "f":"sip:.blindmelon-AppName-dev.beef.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"kick",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 3 (unknown):
```unknown
e30.eyJ2eGkiOjY2NTAwMCwic3ViIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2Lmplcmt5LkB0bGEudml2b3guY29tIiwiZiI6InNpcDouYmxpbmRtZWxvbi1BcHBOYW1lLWRldi5iZWVmLkB0bGEudml2b3guY29tIiwiaXNzIjoiYmxpbmRtZWxvbi1BcHBOYW1lLWRldiIsInZ4YSI6ImtpY2siLCJ0Ijoic2lwOmNvbmZjdGwtZy1ibGluZG1lbG9uLUFwcE5hbWUtZGV2LnRlc3RjaGFubmVsQHRsYS52aXZveC5jb20iLCJleHAiOjE2MDAzNDk0MDB9.kKnWD3smth6KUuRaY11O-yqAbXy2L2wDZeIoDK_098c
```

Example 4 (unknown):
```unknown
e30.eyJ2eGkiOjY2NTAwMCwic3ViIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2Lmplcmt5LkB0bGEudml2b3guY29tIiwiZiI6InNpcDouYmxpbmRtZWxvbi1BcHBOYW1lLWRldi5iZWVmLkB0bGEudml2b3guY29tIiwiaXNzIjoiYmxpbmRtZWxvbi1BcHBOYW1lLWRldiIsInZ4YSI6ImtpY2siLCJ0Ijoic2lwOmNvbmZjdGwtZy1ibGluZG1lbG9uLUFwcE5hbWUtZGV2LnRlc3RjaGFubmVsQHRsYS52aXZveC5jb20iLCJleHAiOjE2MDAzNDk0MDB9.kKnWD3smth6KUuRaY11O-yqAbXy2L2wDZeIoDK_098c
```

---

## Bluetooth profile comparison

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/android/bluetooth-profile-comparison

**Contents:**
- Bluetooth profile comparison#
  - Bluetooth A2DP#
  - Bluetooth SCO(HFP)#
- Tradeoffs of using SCO or A2DP#
  - Latency#
  - Implications of using the handset microphone#

The Bluetooth API provides implementations for the following Bluetooth profiles that are relevant to Vivox:

The Advanced Audio Distribution Profile (A2DP) profile defines how high-quality audio can be streamed from one device to another over a Bluetooth connection. It defines how multimedia audio can be streamed from one device to another over a Bluetooth connection (it is also called Bluetooth Audio Streaming). For example, music can be streamed from a mobile phone to a wireless headset, hearing aid/cochlear implant streamer, or car audio. A2DP is uni-directional. There is no support for capturing audio on a Bluetooth device at the same time as rendering audio.

The Bluetooth Hands-Free Profile (HFP) allows the Bluetooth device to make and receive voice calls via a connected handset. Synchronous Connection-Oriented (SCO) is the type of radio link used for voice data.

A2DP audio is rendered on the Bluetooth headset. Because there’s no support for capture and render at the same time using this profile, most handsets will capture audio from the microphone on the phone. The rendered audio is high quality and stereo. Game audio will remain high quality and will play through the headset along with voice.

SCO voice audio is both rendered and captured from the Bluetooth device. The rendered audio is of lower quality (similar to that of a standard phone call) and mono. Depending on the Android device, game audio will either:

A2DP tends to have more latency than SCO, but not too much more (100 - 200 ms). The extra audio quality of A2DP is notable.

Because of Bluetooth’s inherent low-power requirements, low latency, two-way communication is provided through SCO by reducing the sample rate and the number of channels rendered which leaves bandwidth for a microphone channel.

With A2DP, having the microphone on the handset means that hand noises and screen taps may be audible, so extra rustling and thumping is expected. The hands holding the device can cause muffling of captured audio because they occlude the microphone or completely cover it. With SCO the audio is captured from the Bluetooth device, so no hand noise is typically captured.

Another concern is that the handset can be put down and end up far from the user. With A2DP, as the user moves away from the handset, they will get further from the microphone, and be more difficult to capture. With SCO, they can still be heard clearly no matter how far the user is from their phone (within Bluetooth range).

---

## Callback usage

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/direct-voice-access/direct-voice-callback-usage

**Contents:**
- Callback usage#
- pf_on_audio_unit_after_capture_audio_read#
- pf_on_audio_unit_before_capture_audio_sent#
- pf_on_audio_unit_before_recv_audio_mixed#
- pf_on_audio_unit_before_recv_audio_rendered#

This callback is the most appropriate approach to inject audio to replace the captured audio.

pf_on_audio_unit_after_capture_audio_read returns the data as close to the audio capture device as possible based on the native device. For example, if the device performs hardware echo cancellation, this data is obtained after that step. If a user wants to inject audio to replace the captured audio, have this function overwrite the PCM frames with the data to inject. The data is then run through Vivox audio processing, such as Voice Activity Detection (VAD), Acoustic Echo Cancellation (AEC), and Automatic Gain Control (AGC).

The data must be in the following format to be written to the buffer that is pointed to by pcm_frames.

pcm_frame_count is the total number of frames for the period, where a frame consists of one sample for each channel. For 32 kHz, the number of frames in a 20 ms period would be 640, regardless of whether the audio is stereo or mono.

Note: All resampling or other audio conditioning of injected data is the developer's responsibility. The buffer must always be filled. If there is no audio data, then represent this by entering 0s in that portion of the buffer.

This callback is the most appropriate way for recording applications that are designed to capture a player’s speech.

pf_on_audio_unit_before_capture_audio_sent is called after Vivox audio processing (such as Voice Activity Detection, Acoustic Echo Cancellation, and Automatic Gain Control) occurs, and before being transmitted to the Vivox server. It is not recommended that developers modify the media payload at this point because the metadata (for example, is_speaking) would no longer match the originally analyzed data.

This callback is the most appropriate for adding Digital Signal Processing (DSP) effects to individual participants on the render side.

This callback is called after receiving the audio from the Vivox server, and prior to mixing the audio down to a single stream. Use this callback to gain access to the per-participant audio data.

If the audio frames are zeroed out in this callback, no events for remote participants in the session are generated because a zeroed-out frame plays silence for that participant.

This callback is called to record applications that are designed to capture what a player hears. It is also suitable for applying effects to all voice audio, for example, when the listener is underwater or dazed.

pf_on_audio_unit_before_recv_audio_rendered is called before rendering the audio to the render device. This action occurs after the per-participant mixdown and the application of any 3D audio effects.

This callback needs to be defined if a third-party audio package will be used to render all voice chat audio. Setting the audio frames to all zeros prevents the Vivox SDK from rendering audio to any device, but writes to the audio device will still occur. The proper way to disable Vivox’s access to a render audio device while continuing to process audio is to set the Vivox render device to 'No Device' with the vx_req_aux_set_render_device request.

**Examples:**

Example 1 (unknown):
```unknown
pf_on_audio_unit_after_capture_audio_read
```

Example 2 (unknown):
```unknown
pf_on_audio_unit_after_capture_audio_read
```

Example 3 (unknown):
```unknown
pcm_frame_count
```

Example 4 (unknown):
```unknown
channels_per_frame
```

---

## Update scaling settings

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/guides/update-scaling-settings

**Contents:**
- Update scaling settings#

Fleets implement scaling settings per region. You can manage scaling settings for each region within each of your fleets from the Unity Dashboard.

Warning: Any number of available servers incurs costs, even without traffic. If you're in development or trying to limit costs in a low traffic environment, set the Minimum available scaling value to 0.

To update the scaling settings for a region:

---

## Edit and delete messages

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/text-chat-guide/edit-delete

**Contents:**
- Edit and delete messages#
- Edit channel messages#
- Delete channel messages#

Vivox provides ways to edit and delete messages already sent to channels.

Vivox allows users to edit the text of messages they have sent into a channel using VivoxService.Instance.EditChannelTextMessageAsync(string channelName, string messageId, string newMessage). channelName being the name of the channel that the message was sent to, the messageId being the id of the message to change, and newMessage being the updated text of the message.

When a message has been successfully edited by anyone in a channel, everyone in the channel will receive a VivoxService.Instance.ChannelMessageEdited Action containing the VivoxMessage that was edited, with the updated MessageText.

Vivox allows for users to delete messages they have sent into a channel using VivoxService.Instance.DeleteChannelTextMessageAsync(string channelName, string messageId). channelName being the name of the channel that the message was sent to, and the messageId being the id of the message to delete.

When a message has been successfully deleted by anyone in a channel, everyone in the channel will receive a VivoxService.Instance.ChannelMessageDeleted Action containing the VivoxMessage that was deleted.

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.EditChannelTextMessageAsync(string channelName, string messageId, string newMessage)
```

Example 2 (unknown):
```unknown
VivoxService.Instance.ChannelMessageEdited
```

Example 3 (unknown):
```unknown
public async void UpdateChannelMessageAsync(VivoxMessage messageToUpdate, string updatedMessageText)
{
    await VivoxService.Instance.EditChannelTextMessageAsync(messageToUpdate.ChannelName, messageToUpdate.MessageId, updatedMessageText);
}
```

Example 4 (unknown):
```unknown
public async void UpdateChannelMessageAsync(VivoxMessage messageToUpdate, string updatedMessageText)
{
    await VivoxService.Instance.EditChannelTextMessageAsync(messageToUpdate.ChannelName, messageToUpdate.MessageId, updatedMessageText);
}
```

---

## Smart Platform Audio Management

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/android/smart-platform-audio-management

**Contents:**
- Smart Platform Audio Management#

The Vivox SDK uses Smart Platform Audio Management (SPAM) to automatically select the optimal Android audio configuration based on the context.

Note: Smart Platform Audio Management was known as dynamic voice processing switching (DVPS) in earlier Vivox SDK releases.

SPAM will optimize the audio configuration for voice if both of the following scenarios occur:

In all other scenarios, SPAM will optimize the audio configuration for general-purpose audio.

SPAM configuration includes:

The Android AudioManager mode

The OpenSL ES render stream and recording preset

The Bluetooth profile

SPAM also ensures that audio interruptions, such as phone calls, are handled correctly and that Vivox recovers gracefully when the interruption ends. This includes routing to the active audio device and setting up the audio configuration as described earlier in this section.

SPAM is enabled by default. To disable SPAM, perform either of the following actions:

Warning: Disabling SPAM is not recommended.

**Examples:**

Example 1 (unknown):
```unknown
MODE_IN_COMMUNICATION
```

Example 2 (unknown):
```unknown
MODE_NORMAL
```

Example 3 (unknown):
```unknown
SL_ANDROID_STREAM_VOICE
```

Example 4 (unknown):
```unknown
SL_ANDROID_RECORDING_PRESET_VOICE_COMMUNICATION
```

---

## Send data through text messages

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/messaging/data-through-text-unreal

**Contents:**
- Send data through text messages#
- Unreal application stanza body#
- Send hidden data in group messages#
- Send hidden data in directed messages#
- Access hidden data from messages#
- Related classes#

Vivox text messages can send additional, non-messaging data through two string variables:

The application stanza namespace carries Uniform Resource Identifier (URI) reference information that identifies an XML namespace. This URI identifies the kind of information you send in the application stanza body. For more information about how to format the URI, refer to the W3C documentation on XML Namespaces.

The application stanza body carries the data associated with the given application stanza namespace. If the data is text, it must be UTF-8 encoded. If binary, use base64 to encode with a maximum of 8k per message.

Note: This information is hidden from the player and is only accessible through the code.

You can hide messages if you need to keep them hidden from users. For example, you are using them only to send data with no visible message. One method to accomplish this is to label your application stanza namespace accordingly. For example, you can have an application stanza namespace named "data:hidden" or "message:omit". You can then check for all messages with the same application stanza namespace.

Note: Messages must contain data. You cannot send an empty message.

You can access the hidden data after it is received. You can access its application stanza body by grabbing the message from the function that receives it.

The following examples detail how to send hidden data through text messages and access hidden data from messages in the Vivox Unreal SDK.

**Examples:**

Example 1 (unknown):
```unknown
FString Message = playerInput; //Hello World!
FString ApplicationStanzaNamespace =  TEXT("team:color");
FString ApplicationStanzaBody =  TEXT("red");
IChannelSession::FOnBeginSendTextCompletedDelegate SendChannelMessageCallback;

SendChannelMessageCallback.BindLambda([this, channelId, Message](VivoxCoreError Error) {

   if (VxErrorSuccess == Error)
   {
       UE_LOG(LogVivoxGameInstance, Log, TEXT("Message sent to %s: %s \n"),*channelId.Name(), *Message);
   }

   return;
});

currentChannelSession.BeginSendText("", *Message, ApplicationStanzaNamespace, ApplicationStanzaBody, SendChannelMessageCallback);
```

Example 2 (unknown):
```unknown
FString Message = playerInput; //Hello World!
FString ApplicationStanzaNamespace =  TEXT("team:color");
FString ApplicationStanzaBody =  TEXT("red");
IChannelSession::FOnBeginSendTextCompletedDelegate SendChannelMessageCallback;

SendChannelMessageCallback.BindLambda([this, channelId, Message](VivoxCoreError Error) {

   if (VxErrorSuccess == Error)
   {
       UE_LOG(LogVivoxGameInstance, Log, TEXT("Message sent to %s: %s \n"),*channelId.Name(), *Message);
   }

   return;
});

currentChannelSession.BeginSendText("", *Message, ApplicationStanzaNamespace, ApplicationStanzaBody, SendChannelMessageCallback);
```

Example 3 (javascript):
```javascript
FString Message = playerInput; //Hello World!
FString ApplicationStanzaNamespace =  TEXT("team:color");
FString ApplicationStanzaBody =  TEXT("red");
ILoginSession::FOnBeginSendDirectedMessageCompletedDelegate SendDirectedMessageCallback;

SendDirectedMessageCallback.BindLambda([this, accountName, message](VivoxCoreError error, const FString &request_id)
{
   if (VxErrorSuccess != error)
   {
       UE_LOG(LogVivoxGameInstance, Error, TEXT("BeginSendDirectedMessage() returned %d:%s"), error, ANSI_TO_TCHAR(FVivoxCoreModule::ErrorToString(error)));
   }

   return;
});

currentLoginSession.BeginSendDirectedMessage(AccountId(VIVOX_VOICE_ISSUER, accountName, VIVOX_VOICE_DOMAIN), "", Message, ApplicationStanzaNamespace, ApplicationStanzaBody, SendDirectedMessageCallback);
```

Example 4 (javascript):
```javascript
FString Message = playerInput; //Hello World!
FString ApplicationStanzaNamespace =  TEXT("team:color");
FString ApplicationStanzaBody =  TEXT("red");
ILoginSession::FOnBeginSendDirectedMessageCompletedDelegate SendDirectedMessageCallback;

SendDirectedMessageCallback.BindLambda([this, accountName, message](VivoxCoreError error, const FString &request_id)
{
   if (VxErrorSuccess != error)
   {
       UE_LOG(LogVivoxGameInstance, Error, TEXT("BeginSendDirectedMessage() returned %d:%s"), error, ANSI_TO_TCHAR(FVivoxCoreModule::ErrorToString(error)));
   }

   return;
});

currentLoginSession.BeginSendDirectedMessage(AccountId(VIVOX_VOICE_ISSUER, accountName, VIVOX_VOICE_DOMAIN), "", Message, ApplicationStanzaNamespace, ApplicationStanzaBody, SendDirectedMessageCallback);
```

---

## Integration with the Vivox Unity SDK

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/integration-with-vivox-unity

**Contents:**
- Integration with the Vivox Unity SDK#

Vivox provides group text and voice communications through channels. Users can communicate with each other by joining the same channel. The following pages will walk you through getting started with the Vivox Unity SDK.

---

## Join a channel

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/join-channel

**Contents:**
- Join a channel#

To join a channel, the game uses the vx_req_sessiongroup_add_session message and the channel URI that is generated by the game that maps to an audio element in the game.

Note: Developers must ensure that their game checks the permissions for the capture device at runtime before it join a Vivox voice channel on desktop or mobile.

Joining a channel requires an access token. For more information, refer to the Access Token Developer Guide.

The game can choose to join a channel with audio capability, which enables the player to participate in group audio. The game can also choose to join a channel with text capability, which enables the player to participate in group text.

Note: A user can join a maximum of 10 non-positional channels at a time. Attempts to join a channel beyond that limit fail with a VxXmppServerErrorServiceUnavailable (20502) error.

Note: Channels have a maximum occupancy of 200 participants. Attempts to join a channel that has reached 200 participants will fail with a VxXmppServerErrorServiceUnavailable (20502) error.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_sessiongroup_add_session
```

Example 2 (unknown):
```unknown
VxXmppServerErrorServiceUnavailable (20502)
```

Example 3 (unknown):
```unknown
VxXmppServerErrorServiceUnavailable (20502)
```

---

## Vivox Unity Developer Guide

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/vivox-unity-developer-guide-toc

**Contents:**
- Vivox Unity Developer Guide#

The following guides cover the main functionalities and features of voice and text chat.

---

## Vivox Core Developer Guide

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/core-developer-guide-toc

**Contents:**
- Vivox Core Developer Guide#

The following guides cover the main functionalities and features of voice and text chat.

---

## Evidence report HTTP request

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/chat-history/evidence-report-http-request

**Contents:**
- Evidence report HTTP request#

The following is an example is a request with the additional parameters:

**Examples:**

Example 1 (unknown):
```unknown
curl https://services.api.unity.com/vivox/text-evidence-management/v1/organizations/<organization_id>/projects/<project_id>/text-evidence-report?target_uri=sip:confctl-g-your-issuer.moderation-testing2@yourdomainname.vivox.com&sender_uri=sip:your-issuer.username.1234.@yourdomainname.vivox.com&start_ts=1697117012&end_ts=1697117217&num_messages=2 -u service-acct-username:service-account-password
```

Example 2 (unknown):
```unknown
curl https://services.api.unity.com/vivox/text-evidence-management/v1/organizations/<organization_id>/projects/<project_id>/text-evidence-report?target_uri=sip:confctl-g-your-issuer.moderation-testing2@yourdomainname.vivox.com&sender_uri=sip:your-issuer.username.1234.@yourdomainname.vivox.com&start_ts=1697117012&end_ts=1697117217&num_messages=2 -u service-acct-username:service-account-password
```

---

## Access token signature

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/access-token-guide/access-token-format/access-token-signature

**Contents:**
- Access token signature#

The signature is the base64url-encoded HMAC of the first two parts:

base64UrlEncode(HMACSHA256(base64UrlEncode(header) + '.' + base64UrlEncode(payload), key))

The encoded header and encoded payload are joined with a period, undergoes HMAC with the token signing key, and then the entirety is encoded.

**Examples:**

Example 1 (unknown):
```unknown
base64UrlEncode(HMACSHA256(base64UrlEncode(header) + '.' + base64UrlEncode(payload), key))
```

---

## Vivox SDK error codes

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/troubleshooting/error-codes

**Contents:**
- Vivox SDK error codes#

The following table lists the error codes that the Vivox SDK can return and recommendations for how to handle these errors.

VxErrorNoMessageAvailable

VxErrorTargetObjectDoesNotExist

Internal Vivox error.

Get Vivox logs and send to Vivox for analysis.

VxErrorInvalidArgument

A parameter in a request is either using the wrong type (for example, using a bool when it should be an int) or is missing.

VxErrorNotInitialized

VxErrorNotImplemented

Often a programming error, but sometimes occurs when a login or voice session terminates due to loss of network at the same time that a request is issued.

VxErrorFileOpenFailed

Programming error or packaging error.

User retry, or exponential backoff retry.

VxErrorAlreadyInitialized

VxErrorServerRtpTimeout

VxErrorAsyncOperationCanceled

VxErrorCaptureDeviceInUse

Indicates an attempt to open a second audio session on a second simultaneous session group.

Usually indicative of a client programming error.

VxErrorConnectionTerminated

Connection for Vivox lost, user retry, or exponential backoff.

VxErrorFileOpenFailed

Programming error or packaging error.

VxErrorHandleReserved

Internal Vivox error.

Get Vivox logs and send to Vivox for analysis.

VxErrorInvalidArgument

VxErrorInvalidOperation

Often a programming error, but sometimes occurs when a login or voice session terminates due to loss of network at the same time that a request is issued.

VxErrorInvalidValueTypeXmlQuery

Internal Vivox error.

Get Vivox logs and send to Vivox for analysis.

VxErrorNoMatchingXmlAttributeFound

Internal Vivox error.

Get Vivox logs and send to Vivox for analysis.

VxErrorNoMatchingXmlNodeFound

Internal Vivox error.

Get Vivox logs and send to Vivox for analysis.

Usually a corrupted heap.

VxErrorPortNotAvailable

Unable to find a port for audio.

Usually indicative of having too many calls active at once as a result of a programming error.

User retry or exponential backoff retry.

VxErrorUnableToOpenCaptureDevice

VxErrorXmppBackEndRequired

Client is configured to use the wrong Vivox backend, or the backend is set up incorrectly.

VxErrorPreloginDownloadFailed

Unable to reach a Vivox web server.

VxErrorPresenceMustBeEnabled

VxErrorConnectorLimitExceeded

VxErrorTargetObjectNotRelated

VxErrorTargetObjectDoesNotExist

VxErrorMaxLoginsPerUserExceeded

VxErrorRequestCanceled

VxErrorBuddyDoesNotExist

VxErrorChannelUriRequired

VxErrorTargetObjectAlreadyExists

Occurs when a developer tries to force a player to join a channel that the player is already connected to, while declaring a different session group.

This is because a player cannot connect to two channels with the same URI (same name, same audio/text status, same issuer, and same domain), even if they use a separate session group for each.

This error occurs regardless of whether a developer uses a different session group to try and join the player to the same channel again.

VxErrorInvalidCaptureDeviceForRequestedOperation

VxErrorInvalidCaptureDeviceSpecifier

VxErrorInvalidRenderDeviceSpecifier

VxErrorDeviceLimitReached

VxErrorInvalidEventType

VxErrorNotInitialized

VxErrorAlreadyInitialized

Attempted to initialize a client that was already initialized.

You might need to reopen your Unity project to fix this error.

VxErrorNotImplemented

VxNoAuthentificationStanzaReceived

VxFailedToConnectToXmppServer

VxSSLNegotiationToXmppServerFailed

If this error only occurs on one device, check that the device is up to date on all certificates.

If this error occurs on all devices, contact Vivox immediately.

VxErrorUserOffLineOrDoesNotExist

VxErrorCaptureDeviceInvalidated

VxErrorMaxEtherChannelLimitReached

Server value could not be resolved.

Check that the value is correct. Otherwise, retry with backoff.

VxErrorChannelUriTooLong

VxErrorUserUriTooLong

Occurs when a direct message is sent to a user who is cross-muted (blocked).

VxErrorMessageTextTooLong

Received when a text message exceeds the maximum length in bytes.

VxNetworkHttpInvalidUrl

VxNetworkNameResolutionFailed

Either a programming error or a networking issue.

Check the account management server URL. If pervasive, contact Vivox. Otherwise, retry with backoff.

VxNetworkUnableToConnectToServer

VxNetworkHttpInvalidServerResponse

VxNetworkHttpConnectionReset

VxNetworkHttpInvalidCertificate

VxNetworkHttpGeneralConnectionFailure

VxNetworkReconnectFailure

Received when the Vivox SDK fails to reconnect after several attempts.

VxAccessTokenAlreadyUsed

VxAccessTokenInvalidSignature

VxAccessTokenClaimsMismatch

VxAccessTokenMalformed

VxAccessTokenInternalError

VxAccessTokenServiceUnavailable

VxAccessTokenIssuerMismatch

Received when the Vivox SDK free-tier has been exceeded and payment information has not been provided within 30 days.

VxXmppErrorChannelAtCapacity

Received when trying to join a full channel.

---

## Custom XML Namespaces

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/chat-history/edit-delete/custom-xml

**Contents:**
- Custom XML Namespaces#

These are custom XML namespaces to route edit/delete XMPP requests to Vivox handlers.

---

## Create a unit test

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/unit-testing-create

**Contents:**
- Create a unit test#

Consult the mock code below to add a unit test to your project. This example uses dependency injections.

Run the unit test according to your IDE and confirm that the test passes.

Refer to Microsoft's documentation on unit testing with NUnit for more information on this topic.

To avoid publishing excess binaries when creating unit tests, refer to Create a unit test project.

**Examples:**

Example 1 (unknown):
```unknown
using ExampleModule;
using NUnit.Framework;

namespace TestExampleModule;

public class MockedRandomNumber : IRandomNumber
{
    public int Number;

    public MockedRandomNumber(int number)
    {
        Number = number;
    }

    public int GetRandomNumber()
    {
        return Number;
    }
}

public class Tests
{
    private MockedRandomNumber mockedRandomNumberA;
    private MockedRandomNumber mockedRandomNumberB;

    [SetUp]
    public void Setup()
    {
        mockedRandomNumberA = new MockedRandomNumber(1);
        mockedRandomNumberB = new MockedRandomNumber(1);
    }

    [Test]
    public void TestDependencyInjection()
    {
        TestDependencyInjection dependencyInjection = new TestDependencyInjection(mockedRandomNumberA);
        DependencyInjectionResult result = dependencyInjection.TestInjection(mockedRandomNumberB);

        Assert.AreEqual(new DependencyInjectionResult
        {
            ConstructorNumber = mockedRandomNumberA.Number,
            MethodNumber = mockedRandomNumberB.Number,
        }, result, "Values are not the same");
    }
}
```

Example 2 (unknown):
```unknown
using ExampleModule;
using NUnit.Framework;

namespace TestExampleModule;

public class MockedRandomNumber : IRandomNumber
{
    public int Number;

    public MockedRandomNumber(int number)
    {
        Number = number;
    }

    public int GetRandomNumber()
    {
        return Number;
    }
}

public class Tests
{
    private MockedRandomNumber mockedRandomNumberA;
    private MockedRandomNumber mockedRandomNumberB;

    [SetUp]
    public void Setup()
    {
        mockedRandomNumberA = new MockedRandomNumber(1);
        mockedRandomNumberB = new MockedRandomNumber(1);
    }

    [Test]
    public void TestDependencyInjection()
    {
        TestDependencyInjection dependencyInjection = new TestDependencyInjection(mockedRandomNumberA);
        DependencyInjectionResult result = dependencyInjection.TestInjection(mockedRandomNumberB);

        Assert.AreEqual(new DependencyInjectionResult
        {
            ConstructorNumber = mockedRandomNumberA.Number,
            MethodNumber = mockedRandomNumberB.Number,
        }, result, "Values are not the same");
    }
}
```

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/guides/create-a-build-direct

---

## Get started

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/guides/get-started

**Contents:**
- Get started#
- Prerequisites#
- Set up Multiplay Hosting#
- Integrate your game server#
  - Select your game engine#
  - Link a Unity project#
  - Install the Multiplay Hosting package#
- Create a build#
- Create a build configuration#
- Create a fleet#

This guide walks you through setting up Multiplay Hosting as your project's game server orchestration platform. You’ll learn how to:

Tip: Check out our YouTube video How to set up Game Server Hosting.

Before you start, make sure you meet the following prerequisites:

You can set up and manage Multiplay Hosting through the Unity Dashboard:

Note: When you launch Multiplay Hosting for the first time, this adds Multiplay Hosting to the Shortcuts section on the sidebar and opens the Overview page.

The first step in the process of setting up Multiplay Hosting is to integrate your game server executable. Multiplay Hosting also refers to this executable as the build executable.

Before you continue, you must link your project with the Unity Dashboard using the Unity Editor.

Note: You can skip this step if you’ve already linked your project in the Unity Editor, or you are using Unreal or a custom engine.

Tip: Refer to the Game Server SDK for Unity documentation to learn how to integrate your game with the Game Server SDK.

After you’ve linked your project to the Unity Dashboard, you can install the latest version of the Multiplay Hosting package.

Use the Unity Package Manager to import the Multiplay Hosting package in the Unity Editor:

Note: For most users, the unified Multiplayer Services package replaces the Multiplay standalone package, which is deprecated in Unity 6. Consider migrating to the unified package to facilitate a smooth transition. Visit the migration guide for a step-by-step transition process.

The following sections guide you through creating your first build, and supplement the embedded guide on the Unity Dashboard.

You can create a build in the following ways:

Create a build configuration for the build you created in the earlier step, and set the server density. Refer also to Build configurations.

After you've created a build configuration, create your first fleet and define the following settings:

Finally, create a test allocation to make sure everything is working correctly. Refer to Test allocation documentation.

Congratulations! You’ve successfully set up Multiplay Hosting. You can continue to Multiplay Hosting in the Unity Dashboard to:

You can also configure other Unity services, such as Matchmaker, Analytics, or Cloud Code.

**Examples:**

Example 1 (unknown):
```unknown
com.unity.services.multiplayer
```

Example 2 (unknown):
```unknown
com.unity.services.multiplay
```

---

## Example: C++

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/access-token-guide/generate-token-secure-server/example-c-plus-plus

**Contents:**
- Example: C++#

The following code is an example of how to generate access tokens in C++.

**Examples:**

Example 1 (javascript):
```javascript
/*
* Vivox Token Generation Example - C++
* Note: This example uses C++11, and is written to not use external libraries to demonstrate the token generation process.
* However, because HMACSHA256 is required, this example uses OpenSSL 1.1.0+.
*
*/
#include <openssl/sha.h>
#include <openssl/evp.h>
#include <openssl/hmac.h>
#include <iostream>
#include <iomanip>
#include <sstream>
#include <string>

using namespace std;

typedef unsigned char uchar;
static const string b = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789-_+/";
/*
* You can use Base64URL encoding or Base64 and URL encoding libraries in place of this implementation
* Note: Any trailing ='s must be removed from the encoded string.
*/
static string base64_encode(const string &in)
{
    string out;

    int val = 0, valb = -6;
    for (const char c : in) {
        val = (val << 8) + static_cast<unsigned char>(c);
        valb += 8;
        while (valb >= 0) {
            out.push_back(b[(val >> valb) & 0x3F]);
            valb -= 6;
        }
    }

    if (valb >- 6) out.push_back(b[((val << 8) >> (valb + 8))& 0x3F]);

    return out;
}

#if (OPENSSL_VERSION_NUMBER < 0x10100000L) || defined(LIBRESSL_VERSION_NUMBER)
static HMAC_CTX *HMAC_CTX_new(void)
{
    HMAC_CTX *ctx = static_cast<HMAC_CTX *>(OPENSSL_malloc(sizeof(*ctx)));
    if (ctx != nullptr) {
        HMAC_CTX_init(ctx);
    }

    return ctx;
}

static void HMAC_CTX_free(HMAC_CTX *ctx)
{
    if (ctx != nullptr) {
        HMAC_CTX_cleanup(ctx);
        OPENSSL_free(ctx);
    }
}
#endif

/*
* Uses OpenSSL 1.1.0+ to encode data with a secret using HMAC SHA-256
*/
string hmac(string key, string msg)
{
    unsigned char hash[32];

    HMAC_CTX *hmac = HMAC_CTX_new();
    HMAC_Init_ex(hmac, &key[0], static_cast<int>(key.length()), EVP_sha256(), nullptr);
    HMAC_Update(hmac, reinterpret_cast<const unsigned char *>(&msg[0]), msg.length());
    unsigned int len = 32;
    HMAC_Final(hmac, hash, &len);
    HMAC_CTX_free(hmac);

    stringstream ss;
    ss << setfill('0');
    for (unsigned int i = 0; i < len; i++) {
        ss << hash[i];
    }

    return ss.str();
}

static string vx_generate_token(const string &key, const string &issuer, int exp, const string &vxa, int vxi, const string &f, const string &t)
{
    // Header is static - base64url encoded {}
    string header = base64_encode("{}");  // Can also be defined as a constant "e30"

    // Create payload and base64 encode
    stringstream ss;
    ss << "{ \"iss\": \"" << issuer << "\",";
    ss << "\"exp\": " << exp << ",";
    ss << "\"vxa\": \"" << vxa << "\",";
    ss << "\"vxi\": " << vxi << ",";
    ss << "\"t\": \"" << t << "\",";
    ss << "\"f\": \"" << f << "\" }";
    string payload = base64_encode(ss.str());

    // Join segments to prepare for signing
    string to_sign = header + "." + payload;

    // Sign token with key and HMACSHA256, then base64 encode
    string signed_payload = base64_encode(hmac(key, to_sign));

    // Combine header and payload with signature
    return to_sign + "." + signed_payload;
}

int main()
{
    string token = vx_generate_token("secret", "issuer", 1559359105, "join", 123456, "sip:.username.@domain.vivox.com", "sip:confctl-g-channelname@domain.vivox.com");
    cout << token << endl;

    return 0;
}
```

Example 2 (javascript):
```javascript
/*
* Vivox Token Generation Example - C++
* Note: This example uses C++11, and is written to not use external libraries to demonstrate the token generation process.
* However, because HMACSHA256 is required, this example uses OpenSSL 1.1.0+.
*
*/
#include <openssl/sha.h>
#include <openssl/evp.h>
#include <openssl/hmac.h>
#include <iostream>
#include <iomanip>
#include <sstream>
#include <string>

using namespace std;

typedef unsigned char uchar;
static const string b = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789-_+/";
/*
* You can use Base64URL encoding or Base64 and URL encoding libraries in place of this implementation
* Note: Any trailing ='s must be removed from the encoded string.
*/
static string base64_encode(const string &in)
{
    string out;

    int val = 0, valb = -6;
    for (const char c : in) {
        val = (val << 8) + static_cast<unsigned char>(c);
        valb += 8;
        while (valb >= 0) {
            out.push_back(b[(val >> valb) & 0x3F]);
            valb -= 6;
        }
    }

    if (valb >- 6) out.push_back(b[((val << 8) >> (valb + 8))& 0x3F]);

    return out;
}

#if (OPENSSL_VERSION_NUMBER < 0x10100000L) || defined(LIBRESSL_VERSION_NUMBER)
static HMAC_CTX *HMAC_CTX_new(void)
{
    HMAC_CTX *ctx = static_cast<HMAC_CTX *>(OPENSSL_malloc(sizeof(*ctx)));
    if (ctx != nullptr) {
        HMAC_CTX_init(ctx);
    }

    return ctx;
}

static void HMAC_CTX_free(HMAC_CTX *ctx)
{
    if (ctx != nullptr) {
        HMAC_CTX_cleanup(ctx);
        OPENSSL_free(ctx);
    }
}
#endif

/*
* Uses OpenSSL 1.1.0+ to encode data with a secret using HMAC SHA-256
*/
string hmac(string key, string msg)
{
    unsigned char hash[32];

    HMAC_CTX *hmac = HMAC_CTX_new();
    HMAC_Init_ex(hmac, &key[0], static_cast<int>(key.length()), EVP_sha256(), nullptr);
    HMAC_Update(hmac, reinterpret_cast<const unsigned char *>(&msg[0]), msg.length());
    unsigned int len = 32;
    HMAC_Final(hmac, hash, &len);
    HMAC_CTX_free(hmac);

    stringstream ss;
    ss << setfill('0');
    for (unsigned int i = 0; i < len; i++) {
        ss << hash[i];
    }

    return ss.str();
}

static string vx_generate_token(const string &key, const string &issuer, int exp, const string &vxa, int vxi, const string &f, const string &t)
{
    // Header is static - base64url encoded {}
    string header = base64_encode("{}");  // Can also be defined as a constant "e30"

    // Create payload and base64 encode
    stringstream ss;
    ss << "{ \"iss\": \"" << issuer << "\",";
    ss << "\"exp\": " << exp << ",";
    ss << "\"vxa\": \"" << vxa << "\",";
    ss << "\"vxi\": " << vxi << ",";
    ss << "\"t\": \"" << t << "\",";
    ss << "\"f\": \"" << f << "\" }";
    string payload = base64_encode(ss.str());

    // Join segments to prepare for signing
    string to_sign = header + "." + payload;

    // Sign token with key and HMACSHA256, then base64 encode
    string signed_payload = base64_encode(hmac(key, to_sign));

    // Combine header and payload with signature
    return to_sign + "." + signed_payload;
}

int main()
{
    string token = vx_generate_token("secret", "issuer", 1559359105, "join", 123456, "sip:.username.@domain.vivox.com", "sip:confctl-g-channelname@domain.vivox.com");
    cout << token << endl;

    return 0;
}
```

---

## Large text channel setting

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/chat-history/large-text-channels

**Contents:**
- Large text channel setting#
- Enable large text channels#

The large text channels setting is an optional feature that allows a Vivox backend to support over 200 participants in a text channel.

By default, the large text channel setting is disabled for all games, limiting those game channels to 200 users. With this feature enabled, text channels can support up to 2000 users.

Note: Changes to large text channel configurations won’t take effect for an existing channel until every occupant of that channel has left and the channel is destroyed.

Large text channels are an enterprise-only feature. To enable this feature, contact your Vivox representative.

---

## Join a channel

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/channels/join-channel

**Contents:**
- Join a channel#

When joining a channel, you need to:

For information on the differences between channels types, refer to the Channel types page.

Note: A user can join a maximum of 10 non-positional channels at a time. Attempts to join a channel beyond that limit fail with a VxXmppServerErrorServiceUnavailable (20502) error.

Note: Channels have a maximum occupancy of 200 participants. Attempts to join a channel that has reached 200 participants will fail with a VxXmppServerErrorServiceUnavailable (20502) error. To have channels support more than 200 participants, you can use the Large 3D channels setting, available as an enterprise feature.

**Examples:**

Example 1 (unknown):
```unknown
VxXmppServerErrorServiceUnavailable (20502)
```

---

## Channel-based message delete

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/chat-history/edit-delete/channel-message-delete

**Contents:**
- Channel-based message delete#

To delete a channel-based message, you need to sign in and join at least one channel. An existing message must have been sent to every participant in the channel.

Note: Only the sender of a message can delete it. Use vx_req_session_delete_message to make a delete request.

vx_req_session_delete_message will return:

You will receive a vx_evt_session_delete_message event for every message deleted.

The following code is an example of how to delete a message for a specific channel:

After you delete this message, the game must then process the vx_evt_session_delete_message event.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_session_delete_message *req;
vx_req_session_delete_message_create(&req);
req->session_handle = vx_strdup("channelName");
req->message_id = vx_strdup("oldMessageID");
vx_issue_request2(&req->base);
```

Example 2 (unknown):
```unknown
vx_req_session_delete_message *req;
vx_req_session_delete_message_create(&req);
req->session_handle = vx_strdup("channelName");
req->message_id = vx_strdup("oldMessageID");
vx_issue_request2(&req->base);
```

Example 3 (unknown):
```unknown
void HandleSessionDeleteMessage(vx_evt_session_delete_message *evt)
{
    // Use information in events to display messages
    …
}

void HandleSessionDeleteMessage(vx_resp_session_delete_message_t *resp)
{
    if (resp != nullptr) {
        // Ended properly
    } else {
        // Ended abnormally
    }
}
```

Example 4 (unknown):
```unknown
void HandleSessionDeleteMessage(vx_evt_session_delete_message *evt)
{
    // Use information in events to display messages
    …
}

void HandleSessionDeleteMessage(vx_resp_session_delete_message_t *resp)
{
    if (resp != nullptr) {
        // Ended properly
    } else {
        // Ended abnormally
    }
}
```

---

## Sign in as Bob

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/sdksampleapp/log-in-as-bob

**Contents:**
- Sign in as Bob#

Start the SDKSampleApp in a second command prompt with the arguments from Run the SDKSampleApp, and then sign in as the example user "Bob".

**Examples:**

Example 1 (unknown):
```unknown
[SDKSampleApp]: conn
* Connecting to http://mt1s.www.vivox.com/api2 with connector handle http://mt1s.www.vivox.com/api2...
* Issuing req_connector_create with cookie=1
* Request req_connector_create with cookie=1 completed.
[SDKSampleApp]: login -u .xyzzy.bob.
* Logging .xyzzy.bob. in with connector handle http://mt1s.www.vivox.com/api2 and account handle .xyzzy.bob.
* Issuing req_account_anonymous_login with cookie=2
* evt_account_login_state_change: .xyzzy.bob. login_state_logging_in
* Request req_account_anonymous_login with cookie=2 completed.
* evt_account_login_state_change: .xyzzy.bob. login_state_logged_in
```

Example 2 (unknown):
```unknown
[SDKSampleApp]: conn
* Connecting to http://mt1s.www.vivox.com/api2 with connector handle http://mt1s.www.vivox.com/api2...
* Issuing req_connector_create with cookie=1
* Request req_connector_create with cookie=1 completed.
[SDKSampleApp]: login -u .xyzzy.bob.
* Logging .xyzzy.bob. in with connector handle http://mt1s.www.vivox.com/api2 and account handle .xyzzy.bob.
* Issuing req_account_anonymous_login with cookie=2
* evt_account_login_state_change: .xyzzy.bob. login_state_logging_in
* Request req_account_anonymous_login with cookie=2 completed.
* evt_account_login_state_change: .xyzzy.bob. login_state_logged_in
```

---

## Choose Build output compression level

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/optimize-build-speed/choose-build-output-compression-level

**Contents:**
- Choose Build output compression level#
- Compressed build output files#
- Configure Build output compression level#
  - Configure Build output compression level for a project#
  - Configure Build output compression level for a Build Target Configuration#

Unity Build Automation (UBA) compresses some build outputs after the Unity build completes and before UBA uploads the build to storage buckets. UBA uses compression level 1 by default.

Usually, builds Unity produces are already compressed. Compression level 1 is the compression level with the most efficient in regards to compression time and final compressed file size.

However, depending on factors such as your project, your platform, and your Unity build output compression technique (None, LZ4, or LZ5 HC), you might want to change the final build output compression levels. In general, lower compression levels reduce build outputs compression time, but result in larger final compressed files.

You can edit your compression level to test which level is best for your project. For example, you can evaluate compression time and final compressed file size to find the optimal build output compression level for your project and performance thresholds.

The following build output files are compressed and will be affected by the build output compression level setting:

You can configure the build output compression level at two levels: for a specific project, or for a build target configuration.

Note: This option overrides the project level Build output compression level setting for the specific build target.

---

## Positional channel properties

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/channels/positional-channel-properties

**Contents:**
- Positional channel properties#

The audio experience of a user in a positional audio channel is determined by the Channel3DProperties component of the ChannelId that is passed in to the IChannelSession during its construction.

The following properties control the 3D experience in the channel. Each of these properties are detailed in the HTML reference for the Channel3DProperties class.

ConversationalDistance

AudioFadeIntensityByDistance

The default Channel3DProperties values were chosen to provide the most natural sounding, acoustically realistic 3D audio that is suitable for most situations. When using the default properties, any listener within 27 meters (about 88.5 feet) can hear a speaker, and the speaker is heard at full volume by anyone within 0.9 meters (about 3 feet).

Note: The default distance values use centimeters because this is the default unit of distance measurement in Unreal, but you can specify these values in any scale.

When customizing distance values, consider the following guidelines:

You can represent the AudibleDistance and ConversationalDistance in any units, but the two units must always match. The values can be different.

Note: You must use the same units when reporting your actor’s position by using the Set3DPosition method.

ConversationalDistance is the distance from the listener at which a speaker’s voice is heard at its original volume. This must be an integer in the range 0 <= ConversationalDistance <= AudibleDistance. Consider this to be the expected close-by conversational distance.

The ConversationalDistance sounds the most realistic when it's half the height of a typical speaking entity in your game.

If your players are shorter or taller than typical adult humans, considering adjusting this value to match.

When using the default AudioFadeModel value, if you want voice chat to naturally fade to near zero loudness at the edge of the AudibleDistance so it doesn't sound like it's being abruptly cut off, then set the AudibleDistance to a minimum of:

32 × ConversationalDistance ÷ AudioFadeIntensityByDistance.

If you want a lower max AudibleDistance, for example, to limit the range of received texts, then you can increase AudioFadeIntensityByDistance.

Depending on the amount of ambient game noise or music, you might want to further adjust these values to ensure that voice chat is heard only at the distances you want.

When using the ExponentialByDistance AudioFadeModel, it's recommended that you keep the ConversationalDistance value at 1 or greater.

When using the ExponentialByDistance AudioFadeModel, it's recommended that you adjust the value of the AudioFadeIntensityByDistance property based on the following formula:

AudioFadeIntensityByDistance = 3 ÷ (1 + 1.75 × log10(ConversationalDistance))

**Examples:**

Example 1 (unknown):
```unknown
Channel3DProperties
```

Example 2 (unknown):
```unknown
IChannelSession
```

Example 3 (unknown):
```unknown
Channel3DProperties
```

Example 4 (unknown):
```unknown
AudibleDistance
```

---

## Channels

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/channels/channels-toc

**Contents:**
- Channels#

Vivox provides group text and voice communications through channels. Users can communicate with each other by joining the same channel.

Channels support the following functionality:

Channels have the following characteristics:

---

## Access token format

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/access-token-guide/access-token-format/access-token-format-overview

**Contents:**
- Access token format#

A Vivox access token is similar to a JSON Web Token (JWT), but with a limited set of claims (for example, vxi, f, iss, vxa, t, exp).

The Vivox access token is a string that uses the format header.payload.signature: three base64url-encoded parts that are separated by periods.

Important: Base64url encoding is not the same as Base64 encoding. For more information, see Access token terminology.

**Examples:**

Example 1 (unknown):
```unknown
vxi, f, iss, vxa, t, exp
```

Example 2 (unknown):
```unknown
header.payload.signature
```

---

## Audio injection

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/audio/audio-injection

**Contents:**
- Audio injection#

Note: There is a known issue with the Vivox SDK in which the local playback of the injected audio does not occur unless the file also has the same sample rate as the negotiated audio codec. Other players always hear the file correctly, regardless of sample rate. For Windows, this is normally 48kHz.

Audio injection allows you to broadcast audio from a file to all connected ChannelSessions. Injected audio is treated like a second capture device that you are speaking into. This means that muting or disconnecting your input device does not stop others from hearing the file audio. However, muting yourself in a channel, stopping transmission to a channel, or disconnecting channel AudioState does stop others from hearing the file audio. Because the played file is treated as speech, it is heard by anyone that would normally hear the user, triggers speaking indicators and VU meters like speech does, and is heard by participants in accordance with the player's position and orientation in a 3D channel, or when in a 2D channel, from anywhere on the map.

Use cases for this feature include a player selecting pre-scripted audio clips in their character's voice saying things like "Enemy spotted!" or "Need healing!" to signal their situation, which is especially useful when the player does not have a microphone. Depending on your application, it might also be appropriate to let the user play music into the channel for everyone to listen to. The EventAudioInjectionCompleted event is raised when the file that you are injecting has reached its end, so you can use it to loop an audio track when complete, or move to the next track in a playlist. You can also use the method IsAudioInjecting() to check if you are currently injecting file audio.

You can broadcast only one file at a time. Calls of BeginStartAudioInjection() for which the completion delegate has a successful response code always begins playing the specified file from the beginning and replaces any file that was previously playing. You can stop file audio from playing at any time with StopAudioInjection(). Note that injecting file audio does not replace or otherwise prevent the user from speaking into their capture device and being heard as normal; it is considered to be an additional audio source.

To simulate the experience of file injection as a second spoken audio source, the file also plays locally for the user at the same time it is being transmitted to others. This means that a user playing a file into a non-positional or a positional channel hears the file slightly earlier than other participants, just like their own voice when speaking. In an echo channel, the file plays with an echo, just like your own voice.

Important: Audio files broadcast with this feature must have a .wav extension and contain single channel, 16-bit PCM audio. Otherwise, the delegate response returns VxErrorInvalidFormat and the file does not play.

To review all methods and events that are related to audio injection, see the ILoginSession Class Reference in the Unreal API Reference Manual.

**Examples:**

Example 1 (unknown):
```unknown
ChannelSessions
```

Example 2 (unknown):
```unknown
EventAudioInjectionCompleted
```

Example 3 (unknown):
```unknown
IsAudioInjecting()
```

Example 4 (unknown):
```unknown
BeginStartAudioInjection()
```

---

## Supported values for the Vivox action claim

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/access-token-guide/access-token-format/supported-values-vxa-claim

**Contents:**
- Supported values for the Vivox action claim#

The following table details the Vivox action (vxa) claims in the Vivox SDK.

vx_req_sessiongroup_add_session

vx_req_session_create

https://.../api2/viv_multi_chan_cmd.php?mode=add

Channel join listen-only and moderator muted.

vx_req_sessiongroup_add_session

vx_req_session_create

https://.../api2/viv_multi_chan_cmd.php?mode=add

Kick user from a channel.

vx_req_channel_kick_user

https://.../api2/viv_chan_cmd.php?mode=kick

https://.../api2/viv_chan_cmd.php?mode=drop_all

vx_req_account_anonymous_login

https://.../api2/viv_signin.php

Mute or unmute a user in a channel.

vx_req_channel_mute_user

https://.../api2/viv_chan_cmd.php?mode=mute

https://.../api2/viv_chan_cmd.php?mode=unmute

vx_req_channel_mute_all_users

https://.../api2/viv_chan_cmd.php?mode=mute_all

https://.../api2/viv_chan_cmd.php?mode=unmute_all

Enable or disable speech-to-text transcription.

If the Vivox domain that you are using is not enabled for speech-to-text transcription, attempting to enable transcription results in an error.

vx_req_session_transcription_control

---

## Integration guide

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/integration-guide/integration-plan/integration-guide-overview

**Contents:**
- Integration guide#
- Additional resources#

The Vivox Core integration guide contains a basic project plan that can assist you in getting Vivox voice services integrated and QA-ready in three short development phases.

Note: This integration guide focuses on basic voice integration.

You can integrate Vivox into your game by following integration plan:

---

## Mute voice and text messages

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/muting/mute-voice-text

**Contents:**
- Mute voice and text messages#

In user-to-user, user-to-channel, and moderator muting, a user can choose to mute voice, mute text messages, or mute both types of messages.

Note: The default setting is mute_scope_all.

**Examples:**

Example 1 (unknown):
```unknown
mute_scope_audio
```

Example 2 (unknown):
```unknown
mute_scope_text
```

Example 3 (unknown):
```unknown
mute_scope_all
```

Example 4 (unknown):
```unknown
mute_scope_all
```

---

## Roster list

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/integration-guide/integration-plan/phase-one/ui-integration/roster-list

**Contents:**
- Roster list#

A roster list allows players to see who is online or in-game. If you require a roster list, create a code handler for the vx_participant_added event updates to the roster list you create.

When creating a roster list, determine how to display users:

Can an existing lobby/match/region roster list be used?

Rendered in game, usually over a character's head or by flashing a game piece representing that user.

Will users be allowed to set the local volume for individual users in their roster list?

Will users be allowed to mute or unmute an individual user in their roster list?

Will a text ignore setting also apply to voice? (Recommended)

**Examples:**

Example 1 (unknown):
```unknown
vx_participant_added
```

---

## Mute a user's microphone

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/muting/mute-user-microphone

**Contents:**
- Mute a user's microphone#
- AudioInputDevices#
- SetTransmissionMode()#
  - Transmit to all channels#
  - Transmit to one channel#
  - Transmit to no channels#

There are two methods for muting a user’s microphone: AudioInputDevices or SetTransmissionMode(). Each method is beneficial for different scenarios, which are detailed in the following sections.

Use AudioInputDevices if you want a user to transmit to one channel at a time, or if you want to mute the user's microphone in all channels. This method mutes a user’s microphone across the client.

Note: Unless you need to manage which channels a user can speak into, it's recommended that you use this method for consistency purposes.

The following code displays an example of how to use AudioInputDevices.

Use SetTransmissionMode() when you are managing multiple channels.

You can transmit to channels by using one of the following methods:

Note: If you are transmitting to a channel, then you must transmit to either all channels or to only one channel. For example, if you are joined to channels A, B, and C, you cannot transmit to A and B but not to C.

Use TransmissionMode::All when you want to create a realistic environment.

For example, if a player is talking into a two-way radio, a developer might want other players to also hear the player in the 3D space. By using TransmissionMode::All, when the player speaks, both the players in the two-way radio channel and the players nearby in the world chat can hear the player.

Use Transmission::Single to determine which channel the player is talking into.

For example, a player might be in two squad chats, and they can listen to both channels simultaneously. By using Transmission::Single, the player can speak into one channel at a time.

Use Transmission::None to prevent a player from being heard in any channels.

**Examples:**

Example 1 (unknown):
```unknown
AudioInputDevices
```

Example 2 (unknown):
```unknown
SetTransmissionMode()
```

Example 3 (unknown):
```unknown
AudioInputDevices
```

Example 4 (unknown):
```unknown
AudioInputDevices
```

---

## Player evidence report HTTP response

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/chat-history/player-evidence-report-responses

**Contents:**
- Player evidence report HTTP response#

The following code is an example of a player evidence response:

The next field is used for pagination and represents the player’s most recent activity in the next conversation returned. This value will be null if there are no more pages of results to return. Use the next value as the end_ts in a subsequent request to retrieve the next page of results. The request_start_ts is also used for pagination and represents the timestamp of the initial request. This value maintains consistency in paging results if the player is active during review. The request_start_ts is required when using the next value to request subsequent pages of results.

The conversation objects in the response contain the following fields:

Returned conversation results are in order of the player’s most recent activity.

**Examples:**

Example 1 (unknown):
```unknown
{
   "request_start_ts": 1706203970429807,
   "next": 1704400836045162,
   "conversations": [
       {
           "message_count": 2,
           "last_activity": 1704400939799491,
           "conversation_uri": "sip:confctl-g-your-issuer.test-channel-1@youvivoxdomainname.vivox.com"
       },
       {
           "message_count": 1,
           "last_activity": 1704400878101904,
           "conversation_uri": "sip:your-issuer.doe@yourvivoxdomainname.vivox.com"
       }
   ]
}
```

Example 2 (unknown):
```unknown
{
   "request_start_ts": 1706203970429807,
   "next": 1704400836045162,
   "conversations": [
       {
           "message_count": 2,
           "last_activity": 1704400939799491,
           "conversation_uri": "sip:confctl-g-your-issuer.test-channel-1@youvivoxdomainname.vivox.com"
       },
       {
           "message_count": 1,
           "last_activity": 1704400878101904,
           "conversation_uri": "sip:your-issuer.doe@yourvivoxdomainname.vivox.com"
       }
   ]
}
```

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/guides/use-an-s3-bucket

---

## XMPP representation - Delete a message in a channel

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/chat-history/edit-delete/xmpp-delete-in-channel

**Contents:**
- XMPP representation - Delete a message in a channel#

Example delete request:

Example delete response:

Deleted event notification is then propagated back to every participant in the channel.

Example of propagating a deleted message event:

**Examples:**

Example 1 (unknown):
```unknown
<iq type='set' id='...' from='...' to='...'>
   <query xmlns='urn:xmpp:mam:4'>
       <x xmlns='jabber:x:data' type='submit'>
           <field var='FORM_TYPE' type='hidden'>
               <value>urn:xmpp:mam:4</value>
           </field>
           <field var='message-id'>
               <value>...</value>
           </field>
       </x>
   </query>
</iq>
```

Example 2 (unknown):
```unknown
<iq type='set' id='...' from='...' to='...'>
   <query xmlns='urn:xmpp:mam:4'>
       <x xmlns='jabber:x:data' type='submit'>
           <field var='FORM_TYPE' type='hidden'>
               <value>urn:xmpp:mam:4</value>
           </field>
           <field var='message-id'>
               <value>...</value>
           </field>
       </x>
   </query>
</iq>
```

Example 3 (unknown):
```unknown
<iq from="someRoomSIP@cats.vivox.com"
    id="..."
    to="alice@cats.vivox.com"
    type="result">
    <message-deleted xmlns="urn:vivox:message-deleted" id="..." />
</iq>
```

Example 4 (unknown):
```unknown
<iq from="someRoomSIP@cats.vivox.com"
    id="..."
    to="alice@cats.vivox.com"
    type="result">
    <message-deleted xmlns="urn:vivox:message-deleted" id="..." />
</iq>
```

---

## Requests that require access tokens

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/access-token-guide/requests-that-require-access-tokens

**Contents:**
- Requests that require access tokens#

Each privileged operation requires a token in a specific format. Examples of privileged operations include:

For more information, see Supported values for the Vivox action claim.

---

## macOS app development

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/macos/macos-toc

**Contents:**
- macOS app development#

Use the following pages to help with developing macOS applications.

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/channels/leave-channel

---

## Implementation requirements

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/integration-guide/implement-vivox-core

**Contents:**
- Implementation requirements#

To implement the Vivox SDK, you need the following items, which you can find in the Unity Cloud Dashboard:

The URL for your Vivox API endpoint.

The host for your Vivox domain.

Your Vivox access token issuer and key pair.

Use these items to sign Vivox access tokens on your server and to distribute those tokens to your game clients.

Note: You can initially sign access tokens with your game client, but this is not a recommended production practice.

For more information, refer to the Access Token Developer Guide.

A Vivox SDK distribution for each of the platforms that will be supported by your game.

Each distribution contains the following files:

One or more libraries that are linked with your game client.

C/C++ header files that are included in your sources.

Note: These header files are identical for all platforms; if you are targeting multiple platforms, add only the header files from one distribution into your source code control system.

One or more sample applications that demonstrate how to use the Vivox SDK.

---

## Make a decode request

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/server-side-recording/ssr-make-decode-request

**Contents:**
- Make a decode request#

The following code shows an example of how to decode .tar archives from a finished subscription.

**Examples:**

Example 1 (unknown):
```unknown
curl -x POST
http://{{host}}/ssr/decode?auth_token={{auth_token}}&destination_credentials={"access_key_id":"aws-access-id","secret_access_key":"aws-secret-key"}&s3_uri=”s3://bucket-name/uuid/”&decode_options=”[“concat”]“
```

Example 2 (unknown):
```unknown
curl -x POST
http://{{host}}/ssr/decode?auth_token={{auth_token}}&destination_credentials={"access_key_id":"aws-access-id","secret_access_key":"aws-secret-key"}&s3_uri=”s3://bucket-name/uuid/”&decode_options=”[“concat”]“
```

---

## Automatic connection recovery

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/channels/automatic-connection-recovery/connection-recovery

**Contents:**
- Automatic connection recovery#
- Related pages#

Note: Automatic connection recovery is supported on Android, iOS, macOS, and Windows.

The Vivox SDK provides automatic connection recovery to accommodate brief periods of network interruption. This connection recovery occurs without intervention from the application.

An application might temporarily lose internet connectivity when its user moves between internet connection points. For example, a disconnection could occur when a user is roaming between cellular networks or when a device switches between an LTE mobile data connection and a home wireless network connection.

If network connectivity is lost, the Vivox SDK attempts for up to 30 seconds to restore connectivity.

VivoxService.Instance.ConnectionRecovering and VivoxService.Instance.ConnectionRecovered are events that can be subscribed to track when the connection is recognized as lost and recovery begins and when the connection is recovered. You can monitor these events to provide users with a display of their Vivox connection health.

A VivoxService.Instance.ConnectionFailedToRecover event will be fired if a session resume fails after the application attempts to reconnect to the network.

In the scenario where attempts at recovery have ultimately failed, you can use a minimal integration to alert the user of the reconnect failure, because user action is likely required to address any issues with their network connection health. A more in-depth integration might help with recovery from a partial disconnection from Vivox.

Important: If an additional system exists to reconnect users, you must allow the default Vivox system to finish attempting to recover before another system tries to recover the connection. If the Vivox attempts aren't completed it can result in a connection recovery loop.

**Examples:**

Example 1 (unknown):
```unknown
VivoxService.Instance.ConnectionRecovering
```

Example 2 (unknown):
```unknown
VivoxService.Instance.ConnectionRecovered
```

Example 3 (unknown):
```unknown
VivoxService.Instance.ConnectionFailedToRecover
```

---

## Automatic connection recovery

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/connection-recovery

**Contents:**
- Automatic connection recovery#

The Vivox SDK provides automatic connection recovery to accommodate brief periods of network interruption. This connection recovery occurs without intervention from the application. If network connectivity is lost, the Vivox SDK attempts for up to 30 seconds to restore connectivity.

For example, a disconnection could occur when a user is roaming between cellular networks, when a device switches between a mobile data connection and a home wireless network, or when a user turns on airplane mode.

The Vivox SDK fires events to indicate the connection recovery status. You can monitor these events to provide users with a display of their Vivox connection health.

In the scenario where attempts at recovery have ultimately failed, you can use a minimal integration to alert the user of the reconnect failure, because user action is likely required to address any issues with their network connection health. A more in-depth integration might help with recovery from a partial disconnection from Vivox.

Important: If an additional system exists to reconnect users, you must allow the default Vivox system to finish attempting to recover before another system tries to recover the connection. If the Vivox attempts aren't completed it can result in a connection recovery loop.

---

## Code reviews in the desktop application

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/gui-code-reviews

**Contents:**
- Code reviews in the desktop application#
- Code review window#
- Conversation tab#
  - Request reviewers#
  - Change the status of a code review#
- Review changes tab#
  - Branch review options#
  - Diff options#
  - View existing comments#
    - Comment types#

Create or interact with code reviews in your Unity Version Control desktop application so that you and your team can collaborate and review each other's work to approve changes before you check them in.

The code review window opens automatically when you create a code review. To start a new code review, right-click on either a branch or a changeset, and select the corresponding option:

To open an existing code review from the UVCS desktop application, select the Code reviews tab, and double-click on a code review.

The code review window has two tabs:

Use the Conversation tab of the code review window to set up, edit, monitor, and manage your code review.

Note: You can edit the title, description, and reviewers of your code review at any time from the Conversation tab of the code review window.

A reviewer can change the status of a code review on the Conversation tab:

Use the review changes tab to view the diffs of files and add or reply to any comments.

For a code review of a branch, there are two modes that you can change between when you review:

Any comments only display in the mode that they were added.

In the Review changes tab, you have the following diff options:

The Review comments summary panel at the bottom of the Review changes tab displays all the comments part of the code review. The panel is divided into three sections for each type of comment, and shows the comment, the status, and the owner of each comment.

There are the following types of comment:

Note: The Comments section doesn’t show replies to regular comments. The other two sections do display replies.

To navigate to the related comment, double-click on the comment.

You can’t add a comment to the following types of file:

When someone requests a change in a code review, the request comment has an associated GUID.

When you complete the requested change, you can automatically resolve the requested change comment:

You can also edit the comment of an existing changeset and add the [apply-change:{commentID}] text to resolve a code review change request.

Note: To apply the requested change in the command line, run the checkin command with the comment GUID: cm checkin FileName.cs -c="[apply-change:b3ab21d4] Apply requested changes"

When a comment is resolved, the Review comments summary panel changes the status of the comment to Done and displays a link to the linked changeset:

**Examples:**

Example 1 (unknown):
```unknown
[apply-change:{commentID}]
```

Example 2 (unknown):
```unknown
[apply-change:{commentID}]
```

Example 3 (unknown):
```unknown
cm checkin FileName.cs -c="[apply-change:b3ab21d4] Apply requested changes"
```

---

## Troubleshooting

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/troubleshooting/troubleshooting-toc

**Contents:**
- Troubleshooting#

Use the topics in this section to help troubleshoot the Vivox SDK.

---

## Vivox SDK design

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/integration-guide/vivox-client-sdk-design

**Contents:**
- Vivox SDK design#

The Vivox SDK interface is based on request, response, and event messages. The game client creates request messages, fills in the fields of those messages, and sends those messages to the Vivox Client SDK by using the vx_issue_request3() method.

Note: vx_issue_request3() is a non-blocking method, and you can safely call it from the game client user interface thread.

When the request completes, or when there is data available for the game client to use, the Vivox SDK makes a response message or an event message available to the game client. The game client gets access to this message by calling the vx_get_message() method.

Note: vx_get_message() is a non-blocking method, and can safely call it from the game client user interface or in the game message processing loop.

After the game client has an even or response, the game client uses the information in those fields to update the game client state.

The following diagram displays how messages are communicated between the game client and the Vivox SDK.

Important: There are dozens of request/response pairs and events that are exposed by the Vivox SDK, although it is rare that the game needs to make use of all of them. For a detailed mapping of individual request and response pairs and events to functional areas, see Request, response, and event mapping to Vivox functionality.

**Examples:**

Example 1 (unknown):
```unknown
vx_issue_request3()
```

Example 2 (unknown):
```unknown
vx_issue_request3()
```

Example 3 (unknown):
```unknown
vx_get_message()
```

Example 4 (unknown):
```unknown
vx_get_message()
```

---

## Server-side recording decoder

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/server-side-recording/ssr-decoder

**Contents:**
- Server-side recording decoder#

Use the server-side recording (SSR) decoder service to generate .wav files from the audio data that is captured by SSR and then uploaded to the cloud as .tar files.

After the .wav files are decoded, they are compressed into .zip files and uploaded back to the cloud in a subfolder of where the .tar files were found. By default, decoded subscriptions result in a single .wav file for each participant in the recorded conversation, and all files are the same duration. Other options are detailed in the SSR API documentation.

---

## Errors after migration to a new Xcode version

**URL:** https://docs.unity.com/ugs/en-us/manual/devops/manual/build-automation/check-build-results/troubleshoot-build-failures/error-migrating-xcode

**Contents:**
- Errors after migration to a new Xcode version#
- Symptoms#
- Causes#
- Resolution#
- Additional resources#

Errors after you migrate to a new version of Xcode from an earlier version. The following frequent error messages and warnings from the UBA/Xcode logs are symptomatic of this issue:

These errors often appear when you change Xcode versions if some components, packages, or plug-ins aren't compatible with the new version of Xcode.

The first step is to ensure you can build locally in batch mode with a clean clone of the repository (that has no locally cached library) and build the Xcode project without any additional changes beyond what you are doing in your Build Automation targets.

After you successfully build locally in batch mode, ensure the version of cocoapods you use locally matches the one you use in Build Automation. If the version doesn't match, use the following pre-build script to install the correct version. The following example is for cocoapods version 1.12.0:

Note: You can configure pre-build scripts in the Unity Dashboard, in the Advanced Configuration tab for your build target:

To check your local version of cocoapods, use the following command in the terminal:

Note: Ensure that if you apply any changes to the local Xcode project before the build, you also apply those changes to the project in Build Automation as well.

You can pin cocoapods to a specific version to ensure that the version is consistent across all builds. You can do this with an external tool such as the External Dependency Manager for Unity or you can directly edit Dependencies.xml files. With this tool, if you use an exact version such as 7.0.0 rather than an expression, such as ~> 7.0 which uses any version greater than 7.0, you guarantee that the dependent package version doesn't change and introduce any new bugs.

If you haven't resolved the issue, please open a support ticket so the support team can investigate further. To submit a ticket from the Unity Dashboard, open DevOps and select Help & Support > Ticket > File a ticket.

**Examples:**

Example 1 (unknown):
```unknown
ld: symbol(s) not found for architecture arm64
clang: error: linker command failed with exit code 1 (use -v to see invocation)
```

Example 2 (unknown):
```unknown
ld: symbol(s) not found for architecture arm64
clang: error: linker command failed with exit code 1 (use -v to see invocation)
```

Example 3 (unknown):
```unknown
warning: The iOS deployment target 'IPHONEOSDEPLOYMENTTARGET' is set to 10.0, but the range of supported deployment target versions is 11.0 to 16.1.99
```

Example 4 (unknown):
```unknown
warning: The iOS deployment target 'IPHONEOSDEPLOYMENTTARGET' is set to 10.0, but the range of supported deployment target versions is 11.0 to 16.1.99
```

---

## Retrieve conversations list

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/retrieve-conversation-list

**Contents:**
- Retrieve conversations list#

You can retrieve a user’s list of conversations. A conversation is a channel where a user has sent a message or a direct message (DM). You can use this API to populate a list of channels or DMs for a user to find their active chats.

Note: The returned Conversation list is ranked by most recent activity. Conversations with newer messages show up first in the results.

To retrieve the conversation list, sign in to the application and then use vx_req_account_get_conversations to make a request. This request returns:

The following code is an example of how to retrieve a user’s conversation list.

After you retrieve the user’s conversation list, the game must process the vx_resp_account_get_conversations_t response:

**Examples:**

Example 1 (unknown):
```unknown
vx_req_account_get_conversations
```

Example 2 (unknown):
```unknown
vx_req_account_get_conversations *req;
vx_req_account_get_conversations_create(&req);
req->cursor = ”1”; // Optional parameter if you’re fetching anything but the first page
req->request_time = 1697135622123456; // Optional, unix time in microseconds, needs to be set to the same time to keep results consistent between pages
req->page_size = 10; // Optional, otherwise server default is used. Default is 10
vx_issue_request2(&req->base);
```

Example 3 (unknown):
```unknown
vx_req_account_get_conversations *req;
vx_req_account_get_conversations_create(&req);
req->cursor = ”1”; // Optional parameter if you’re fetching anything but the first page
req->request_time = 1697135622123456; // Optional, unix time in microseconds, needs to be set to the same time to keep results consistent between pages
req->page_size = 10; // Optional, otherwise server default is used. Default is 10
vx_issue_request2(&req->base);
```

Example 4 (unknown):
```unknown
vx_resp_account_get_conversations_t
```

---

## Example: C++

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/generate-token-secure-server/example-c-plus-plus

**Contents:**
- Example: C++#

The following code is an example of how to generate access tokens in C++.

**Examples:**

Example 1 (javascript):
```javascript
/*
* Vivox Token Generation Example - C++
* Note: This example uses C++11, and is written to not use external libraries to demonstrate the token generation process.
* However, because HMACSHA256 is required, this example uses OpenSSL 1.1.0+.
*
*/
#include <openssl/sha.h>
#include <openssl/evp.h>
#include <openssl/hmac.h>
#include <iostream>
#include <iomanip>
#include <sstream>
#include <string>

using namespace std;

typedef unsigned char uchar;
static const string b = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789-_+/";
/*
* You can use Base64URL encoding or Base64 and URL encoding libraries in place of this implementation
* Note: Any trailing ='s must be removed from the encoded string.
*/
static string base64_encode(const string &in)
{
    string out;

    int val = 0, valb = -6;
    for (const char c : in) {
        val = (val << 8) + static_cast<unsigned char>(c);
        valb += 8;
        while (valb >= 0) {
            out.push_back(b[(val >> valb) & 0x3F]);
            valb -= 6;
        }
    }

    if (valb >- 6) out.push_back(b[((val << 8) >> (valb + 8))& 0x3F]);

    return out;
}

#if (OPENSSL_VERSION_NUMBER < 0x10100000L) || defined(LIBRESSL_VERSION_NUMBER)
static HMAC_CTX *HMAC_CTX_new(void)
{
    HMAC_CTX *ctx = static_cast<HMAC_CTX *>(OPENSSL_malloc(sizeof(*ctx)));
    if (ctx != nullptr) {
        HMAC_CTX_init(ctx);
    }

    return ctx;
}

static void HMAC_CTX_free(HMAC_CTX *ctx)
{
    if (ctx != nullptr) {
        HMAC_CTX_cleanup(ctx);
        OPENSSL_free(ctx);
    }
}
#endif

/*
* Uses OpenSSL 1.1.0+ to encode data with a secret using HMAC SHA-256
*/
string hmac(string key, string msg)
{
    unsigned char hash[32];

    HMAC_CTX *hmac = HMAC_CTX_new();
    HMAC_Init_ex(hmac, &key[0], static_cast<int>(key.length()), EVP_sha256(), nullptr);
    HMAC_Update(hmac, reinterpret_cast<const unsigned char *>(&msg[0]), msg.length());
    unsigned int len = 32;
    HMAC_Final(hmac, hash, &len);
    HMAC_CTX_free(hmac);

    stringstream ss;
    ss << setfill('0');
    for (unsigned int i = 0; i < len; i++) {
        ss << hash[i];
    }

    return ss.str();
}

static string vx_generate_token(const string &key, const string &issuer, int exp, const string &vxa, int vxi, const string &f, const string &t)
{
    // Header is static - base64url encoded {}
    string header = base64_encode("{}");  // Can also be defined as a constant "e30"

    // Create payload and base64 encode
    stringstream ss;
    ss << "{ \"iss\": \"" << issuer << "\",";
    ss << "\"exp\": " << exp << ",";
    ss << "\"vxa\": \"" << vxa << "\",";
    ss << "\"vxi\": " << vxi << ",";
    ss << "\"t\": \"" << t << "\",";
    ss << "\"f\": \"" << f << "\" }";
    string payload = base64_encode(ss.str());

    // Join segments to prepare for signing
    string to_sign = header + "." + payload;

    // Sign token with key and HMACSHA256, then base64 encode
    string signed_payload = base64_encode(hmac(key, to_sign));

    // Combine header and payload with signature
    return to_sign + "." + signed_payload;
}

int main()
{
    string token = vx_generate_token("secret", "issuer", 1559359105, "join", 123456, "sip:.username.@domain.vivox.com", "sip:confctl-g-channelname@domain.vivox.com");
    cout << token << endl;

    return 0;
}
```

Example 2 (javascript):
```javascript
/*
* Vivox Token Generation Example - C++
* Note: This example uses C++11, and is written to not use external libraries to demonstrate the token generation process.
* However, because HMACSHA256 is required, this example uses OpenSSL 1.1.0+.
*
*/
#include <openssl/sha.h>
#include <openssl/evp.h>
#include <openssl/hmac.h>
#include <iostream>
#include <iomanip>
#include <sstream>
#include <string>

using namespace std;

typedef unsigned char uchar;
static const string b = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789-_+/";
/*
* You can use Base64URL encoding or Base64 and URL encoding libraries in place of this implementation
* Note: Any trailing ='s must be removed from the encoded string.
*/
static string base64_encode(const string &in)
{
    string out;

    int val = 0, valb = -6;
    for (const char c : in) {
        val = (val << 8) + static_cast<unsigned char>(c);
        valb += 8;
        while (valb >= 0) {
            out.push_back(b[(val >> valb) & 0x3F]);
            valb -= 6;
        }
    }

    if (valb >- 6) out.push_back(b[((val << 8) >> (valb + 8))& 0x3F]);

    return out;
}

#if (OPENSSL_VERSION_NUMBER < 0x10100000L) || defined(LIBRESSL_VERSION_NUMBER)
static HMAC_CTX *HMAC_CTX_new(void)
{
    HMAC_CTX *ctx = static_cast<HMAC_CTX *>(OPENSSL_malloc(sizeof(*ctx)));
    if (ctx != nullptr) {
        HMAC_CTX_init(ctx);
    }

    return ctx;
}

static void HMAC_CTX_free(HMAC_CTX *ctx)
{
    if (ctx != nullptr) {
        HMAC_CTX_cleanup(ctx);
        OPENSSL_free(ctx);
    }
}
#endif

/*
* Uses OpenSSL 1.1.0+ to encode data with a secret using HMAC SHA-256
*/
string hmac(string key, string msg)
{
    unsigned char hash[32];

    HMAC_CTX *hmac = HMAC_CTX_new();
    HMAC_Init_ex(hmac, &key[0], static_cast<int>(key.length()), EVP_sha256(), nullptr);
    HMAC_Update(hmac, reinterpret_cast<const unsigned char *>(&msg[0]), msg.length());
    unsigned int len = 32;
    HMAC_Final(hmac, hash, &len);
    HMAC_CTX_free(hmac);

    stringstream ss;
    ss << setfill('0');
    for (unsigned int i = 0; i < len; i++) {
        ss << hash[i];
    }

    return ss.str();
}

static string vx_generate_token(const string &key, const string &issuer, int exp, const string &vxa, int vxi, const string &f, const string &t)
{
    // Header is static - base64url encoded {}
    string header = base64_encode("{}");  // Can also be defined as a constant "e30"

    // Create payload and base64 encode
    stringstream ss;
    ss << "{ \"iss\": \"" << issuer << "\",";
    ss << "\"exp\": " << exp << ",";
    ss << "\"vxa\": \"" << vxa << "\",";
    ss << "\"vxi\": " << vxi << ",";
    ss << "\"t\": \"" << t << "\",";
    ss << "\"f\": \"" << f << "\" }";
    string payload = base64_encode(ss.str());

    // Join segments to prepare for signing
    string to_sign = header + "." + payload;

    // Sign token with key and HMACSHA256, then base64 encode
    string signed_payload = base64_encode(hmac(key, to_sign));

    // Combine header and payload with signature
    return to_sign + "." + signed_payload;
}

int main()
{
    string token = vx_generate_token("secret", "issuer", 1559359105, "join", 123456, "sip:.username.@domain.vivox.com", "sip:confctl-g-channelname@domain.vivox.com");
    cout << token << endl;

    return 0;
}
```

---

## Unity SDK WebGL support

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/vivox-webgl

**Contents:**
- Unity SDK WebGL support#
- Known Vivox WebGL limitations#
- Build a Unity project for WebGL#
- Best practices#

The Vivox Unity WebGL SDK is available starting at version 16.5.0.

This SDK is available at a Limited level of support. For more information on this support level, refer to the Supported platforms and versions page.

Review the prerequisites for building a Unity project for WebGL in the Unity Editor documentation on building for WebGL.

To build your application, set the build target to WebGL: File > Build Settings > WebGL > Build (or Build and Run).

**Examples:**

Example 1 (unknown):
```unknown
~NotImplementedException
```

---

## Couch co-op

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/couch-co-op/couch-co-op-overview

**Contents:**
- Couch co-op#

Note: Couch co-op is supported on PlayStation 4, Xbox, and Windows.

The couch co-op feature enables multiple users to be signed in and using voice chat simultaneously, each with their own independent capture and render devices. A common example of this is participating in a split-screen gameplay experience.

---

## Phase 3: Testing

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/integration-guide/integration-plan/phase-three/testing-overview

**Contents:**
- Phase 3: Testing#

In this phase, use the following test plan to help verify your integration. Common trouble areas are typically experienced during error handling.

---

## Troubleshooting

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/troubleshooting/troubleshooting-toc

**Contents:**
- Troubleshooting#

Use the topics in this section to help troubleshoot the Vivox SDK.

---

## Send a channel message

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/sdksampleapp/send-channel-message

**Contents:**
- Send a channel message#

To send a channel message, you must first join the channel. Channel names are URIs that are formatted by using the issuer string.

In this example scenario, join "sip:confctl-g-xyzzy.test@mt1s.vivox.com", which is a g-type (general) channel that is controlled by issuer "xyzzy".

The user Alice joins a channel, as detailed in the following code:

Alice then sends a channel message, as detailed in the following code:

The other user Bob joins the same channel, as detailed in the following code:

Although Bob joined after Alice, because the channel message history policy has been enabled on this server, Bob sees the message (message event) "bob are you there?".

**Examples:**

Example 1 (unknown):
```unknown
[SDKSampleApp]: addsession -c sip:confctl-g-xyzzy.test@mt1s.vivox.com
* Adding channel sip:confctl-g-xyzzy.test@mt1s.vivox.com (muted=no) (audio=yes) (text=yes) to session group sg_sip:confctl-g-xyzzy.test@mt1s.vivox.com with handle sip:confctl-g-xyzzy.test@mt1s.vivox.com
* Issuing req_sessiongroup_add_session with cookie=4
* Request req_sessiongroup_add_session with cookie=4 completed.
* evt_sessiongroup_added: sg_sip:confctl-g-xyzzy.test@mt1s.vivox.com [.xyzzy.alice.]
* evt_session_added: sip:confctl-g-xyzzy.test@mt1s.vivox.com [sg_sip:confctl-g-xyzzy.test@mt1s.vivox.com]
* evt_text_stream_updated: sip:confctl-g-xyzzy.test@mt1s.vivox.com session_text_connecting
* evt_media_stream_updated: sip:confctl-g-xyzzy.test@mt1s.vivox.com session_media_connecting 'Success' (0)
* evt_text_stream_updated: sip:confctl-g-xyzzy.test@mt1s.vivox.com session_text_connected
* evt_participant_added: sip:confctl-g-xyzzy.test@mt1s.vivox.com sip:.xyzzy.alice.@mt1s.vivox.com
* evt_media_stream_updated: sip:confctl-g-xyzzy.test@mt1s.vivox.com session_media_connected 'Success' (0)
```

Example 2 (unknown):
```unknown
[SDKSampleApp]: addsession -c sip:confctl-g-xyzzy.test@mt1s.vivox.com
* Adding channel sip:confctl-g-xyzzy.test@mt1s.vivox.com (muted=no) (audio=yes) (text=yes) to session group sg_sip:confctl-g-xyzzy.test@mt1s.vivox.com with handle sip:confctl-g-xyzzy.test@mt1s.vivox.com
* Issuing req_sessiongroup_add_session with cookie=4
* Request req_sessiongroup_add_session with cookie=4 completed.
* evt_sessiongroup_added: sg_sip:confctl-g-xyzzy.test@mt1s.vivox.com [.xyzzy.alice.]
* evt_session_added: sip:confctl-g-xyzzy.test@mt1s.vivox.com [sg_sip:confctl-g-xyzzy.test@mt1s.vivox.com]
* evt_text_stream_updated: sip:confctl-g-xyzzy.test@mt1s.vivox.com session_text_connecting
* evt_media_stream_updated: sip:confctl-g-xyzzy.test@mt1s.vivox.com session_media_connecting 'Success' (0)
* evt_text_stream_updated: sip:confctl-g-xyzzy.test@mt1s.vivox.com session_text_connected
* evt_participant_added: sip:confctl-g-xyzzy.test@mt1s.vivox.com sip:.xyzzy.alice.@mt1s.vivox.com
* evt_media_stream_updated: sip:confctl-g-xyzzy.test@mt1s.vivox.com session_media_connected 'Success' (0)
```

Example 3 (unknown):
```unknown
[SDKSampleApp]: message -m "bob are you in channel?"
* Sending message 'bob are you in channel?' to session handle sip:confctl-g-xyzzy.test@mt1s.vivox.com
* Issuing req_session_send_message with cookie=5
* Request req_session_send_message with cookie=5 completed.
```

Example 4 (unknown):
```unknown
[SDKSampleApp]: message -m "bob are you in channel?"
* Sending message 'bob are you in channel?' to session handle sip:confctl-g-xyzzy.test@mt1s.vivox.com
* Issuing req_session_send_message with cookie=5
* Request req_session_send_message with cookie=5 completed.
```

---

## Example: Kick tokens

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/access-token-examples/example-kick-token

**Contents:**
- Example: Kick tokens#
- Signed in user kicking other user from a channel#
- Admin user kicking other user from a channel#
- Admin user kicking other user from a server#

Note: There is no example of a signed in user kicking another user from a server because this action is not possible with the current API.

The token in this example allows the user "beef" to kick the user "jerky" from the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

The token in this example allows the user "blindmelon-AppName-dev-Admin" to kick the user "jerky" from the channel "testchannel".

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

The token in this example allows the user "blindmelon-AppName-dev-Admin" to kick the user "jerky" from the entire server.

The token expiration time is September 17, 2020 at 1:30 PM UTC.

The token signing key is "secret!" (not including quotes).

Note: Spacing has been added to this code for demonstration purposes. You can also list claims in any order.

**Examples:**

Example 1 (unknown):
```unknown
{
    "vxi":665000,
    "sub":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "f":"sip:.blindmelon-AppName-dev.beef.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"kick",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 2 (unknown):
```unknown
{
    "vxi":665000,
    "sub":"sip:.blindmelon-AppName-dev.jerky.@tla.vivox.com",
    "f":"sip:.blindmelon-AppName-dev.beef.@tla.vivox.com",
    "iss":"blindmelon-AppName-dev",
    "vxa":"kick",
    "t":"sip:confctl-g-blindmelon-AppName-dev.testchannel@tla.vivox.com",
    "exp":1600349400
}
```

Example 3 (unknown):
```unknown
e30.eyJ2eGkiOjY2NTAwMCwic3ViIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2Lmplcmt5LkB0bGEudml2b3guY29tIiwiZiI6InNpcDouYmxpbmRtZWxvbi1BcHBOYW1lLWRldi5iZWVmLkB0bGEudml2b3guY29tIiwiaXNzIjoiYmxpbmRtZWxvbi1BcHBOYW1lLWRldiIsInZ4YSI6ImtpY2siLCJ0Ijoic2lwOmNvbmZjdGwtZy1ibGluZG1lbG9uLUFwcE5hbWUtZGV2LnRlc3RjaGFubmVsQHRsYS52aXZveC5jb20iLCJleHAiOjE2MDAzNDk0MDB9.kKnWD3smth6KUuRaY11O-yqAbXy2L2wDZeIoDK_098c
```

Example 4 (unknown):
```unknown
e30.eyJ2eGkiOjY2NTAwMCwic3ViIjoic2lwOi5ibGluZG1lbG9uLUFwcE5hbWUtZGV2Lmplcmt5LkB0bGEudml2b3guY29tIiwiZiI6InNpcDouYmxpbmRtZWxvbi1BcHBOYW1lLWRldi5iZWVmLkB0bGEudml2b3guY29tIiwiaXNzIjoiYmxpbmRtZWxvbi1BcHBOYW1lLWRldiIsInZ4YSI6ImtpY2siLCJ0Ijoic2lwOmNvbmZjdGwtZy1ibGluZG1lbG9uLUFwcE5hbWUtZGV2LnRlc3RjaGFubmVsQHRsYS52aXZveC5jb20iLCJleHAiOjE2MDAzNDk0MDB9.kKnWD3smth6KUuRaY11O-yqAbXy2L2wDZeIoDK_098c
```

---

## Call from Unity Runtime

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/run-modules/unity-runtime

**Contents:**
- Call from Unity Runtime#
- Prerequisites#
- Link project#
- SDK installation#
- SDK setup#
- Typical workflow#
- Authentication#
- Calling a Cloud Code module endpoint#
  - Use editor bindings#
  - Use CallModuleEndpointAsync#

Run a module endpoint by calling it from an authenticated game client in the Unity Editor.

Note: If a module has not received any traffic in the last 15 minutes, you may experience a cold start latency. Any subsequent calls to the module are faster.

To use Cloud Code in the Unity Editor, you must first install the Cloud Code SDK and link your Unity Gaming Services project to the Unity Editor.

Link your Unity Gaming Services project with the Unity Editor. You can find your UGS project ID in the Unity Dashboard.

In Unity Editor, select Edit > Project Settings > Services.

If your project doesn't have a Unity project ID:

If you have an existing Unity project ID:

Your Unity Project ID appears, and the project is now linked to Unity services. You can also access your project ID in a Unity Editor script using UnityEditor.CloudProjectSettings.projectId.

To install the latest Cloud Code package for Unity Editor:

Check Unity - Manual: Package Manager window to familiarize yourself with the Unity Package Manager interface.

To get started with the Cloud Code SDK:

You can call the Cloud Code SDK from a regular MonoBehaviour C# script within the Unity Editor.

Players must have a valid player ID and access token to access the Cloud Code services. You must authenticate players with the Authentication SDK before using any of the Cloud Code APIs. You can do this by initializing the Authentication SDK inside a C# Monobehaviour script. Refer to Authenticate players.

To initialize the Authentication SDK, include the following in your C# Monobehaviour script:

To get started, we recommend using anonymous authentication. The following example demonstrates how to start anonymous authentication using the authentication package and log the player ID in a script within Unity Editor.

After including authentication in your newly created script, you are ready to call a Cloud Code module endpoint. Include the following namespace to integrate Cloud Code into your Unity Editor project:

The samples below assume you have deployed a module called HelloWorld with an endpoint called RollDice with the following code:

You can generate editor bindings for your Cloud Code modules, so that you can use them in the Unity Editor interface. Refer to Generate bindings for more information.

The following examples are a type-safe way to call your Cloud Code modules from the Unity Editor. Create a C# MonoBehaviour script RollDiceExample in the Unity Editor, and include the following code:

You can also use the CallModuleEndpointAsync API client method, which you can find under CloudCodeService.Instance.

Create a C# Monobehaviour script RollDiceExample in the Unity Editor. The following is a full example of how to integrate both Authentication and Cloud Code into your script:

To test the script, attach it to a game object in the Unity Editor, then select Play.

You can include the following code in the Start() method to call the CallMethod() method:

**Examples:**

Example 1 (unknown):
```unknown
UnityEditor.CloudProjectSettings.projectId
```

Example 2 (unknown):
```unknown
com.unity.services.cloudcode
```

Example 3 (unknown):
```unknown
UnityServices.InitializeAsync()
```

Example 4 (unknown):
```unknown
MonoBehaviour
```

---

## Vivox Unreal Developer Guide

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/vivox-unreal-developer-guide-toc

**Contents:**
- Vivox Unreal Developer Guide#

The following guides cover the main functionalities and features of voice and text chat.

---

## Send push messages

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/push-messages

**Contents:**
- Send push messages#
- Use cases and benefits#
- Create a module#
- Set up the Unity Editor#
  - Link project#
  - SDK installation#
  - SDK setup#
- Subscribe to messages#
- Send messages to players#
  - Run from Unity runtime#

Cloud Code C# modules allow you to push messages to clients over a WebSocket. This page provides an overview of this feature and instructions on how to use it in Cloud Code modules and the Cloud Code SDK.

Note: Push messages and push notifications serve different purposes in your game. Push messages target players who are actively engaged in game play, and push notifications reach devices when the game is either inactive or operating in the background.

To send push messages, you need to call a module endpoint. You can subscribe to push messages from the Unity Editor.

You can use push messages for a variety of purposes, such as the following:

The following code example demonstrates the basic structure for a module that includes two methods:

Important: The SendProjectMessage method sends a message to all connected players in the project. This can create an opening for abuse, such as spamming players with messages. To prevent this, you can create an Access Control policy to limit player access to the endpoint.

Note: You can broadcast a message to the whole project by using Triggers. Refer to Use case sample: Wish players a happy new year.

The Unity.Services.CloudCode.Apis package provides the PushClient instance, which is the interface that you use to send push messages.

To learn how to deploy a module, refer to the Deploying Hello World page.

The Cloud Code SDK provides an interface for subscribing to messages. You can subscribe to messages for a specific player or for the entire project.

To use Cloud Code in the Unity Editor, you must first install the Cloud Code SDK and link your Unity Gaming Services project to the Unity Editor.

Link your Unity Gaming Services project with the Unity Editor. You can find your UGS project ID in the Unity Dashboard.

In Unity Editor, select Edit > Project Settings > Services.

Link your project.If your project doesn't have a Unity project ID:

If you have an existing Unity project ID:

This shows your Unity Project ID and links the project to Unity services. You can also access your project ID in a Unity Editor script with the UnityEditor.CloudProjectSettings.projectId parameter.

Install the latest Cloud Code package for Unity Editor:

Note: You can subscribe to messages with Cloud Code SDK versions 2.4.0+.

Note: To familiarize yourself with the Unity Package Manager interface, refer to Package Manager window documentation.

Get started with the Cloud Code SDK:

Note: Players must have a valid player ID and access token to access the Cloud Code services. You must authenticate players with the Authentication SDK before using any of the Cloud Code APIs. You can do this with the following code snippet for anonymous authentication or check the documentation for Authentication for more details and other sign-in methods.

You can subscribe to messages for a specific player or for the entire project. You can use the following code snippet as a guide for a MonoBehaviour script for your Unity project.

The script authenticates as an anonymous player, and subscribes to both player-specific and project-specific messages and handles various types of events:

To send messages to players, call either the SendPlayerMessage or the SendProjectMessage function from your module.

To learn how to run modules, refer to Run modules.

You can send messages to other players via the CallModuleEndpointAsync method of the CloudCodeService instance. You need to use the module name and function name as its first two arguments, and a dictionary with the necessary parameters as the third argument.

For example, to send a player message, you can call:

Note: You can create an Access Control policy to deny players from calling the SendProjectMessage module function that sends a project message to all other players in your game.

Both player-specific and project-specific subscriptions have four event handlers:

The IMessageReceivedEvent is an interface that represents a message the player receives from the push service. The event triggers when Cloud Code sends a message via a WebSocket connection.

Refer to the table below for the payload of theIMessageReceivedEvent interface:

**Examples:**

Example 1 (unknown):
```unknown
SendPlayerMessage
```

Example 2 (unknown):
```unknown
SendProjectMessage
```

Example 3 (unknown):
```unknown
SendProjectMessage
```

Example 4 (unknown):
```unknown
Unity.Services.CloudCode.Apis
```

---

## Service and access token support

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/token-support

**Contents:**
- Service and access token support#
- Overview#
- Access token support#
  - Supported services#
  - Use tokens with Cloud Code C# Client SDKs#
  - Use with UGS Client APIs#
- Service token support#
  - Supported services#
  - Use tokens with Cloud Code C# Client SDKs#
  - Use tokens with UGS Client APIs#

In modules, the IExecutionContext interface provides an AccessToken and a ServiceToken.

You can authenticate as a player or as Cloud Code with these tokens when you call other UGS services.

Refer to Authentication page for more information.

Evaluate whether you want to authenticate as a player or as a trusted client.

The module IExecutionContext interface provides you with a AccessToken and a ServiceToken.

If the purpose of the module is to only access the data of the authenticated player, you should use the AccessToken to authenticate as the player calling the module endpoint. This ensures that the player can only access and interact with their own data.

If the purpose of the module is to access cross-player data, you should use the ServiceToken to authenticate as Cloud Code. This will ensure that the module can access and interact with cross-player data. However, this creates an opening for malicious players to call the module endpoint and access cross-player data where not intended. To prevent this, you should ensure that the module is protected with Access Control rules.

To learn more about how you can interact with cross-player data, refer to the Cross-player data documentation.

An access token is a JWT that the Authentication service generates. You can use the access token to authenticate players.

When an authenticated player calls a Cloud Code module, they pass their access token to the module as the AccessToken property of the IExecutionContext interface.

You can use access tokens with the Cloud Code C# Game SDKs. To use access tokens with other UGS APIs, you need to pass the token as a bearer token in the Authentication header.

To check which services support access tokens, refer to the table below:

Note: Most UGS services are onboarded with the Authentication service and support access tokens.

To use the AccessToken with the Cloud Code C# Client SDKs, you need to pass the token from the IExecutionContext interface.

The example below increments the currency balance for the player that calls the module:

To use the AccessToken with the UGS Client APIs, pass the token as a bearer token in the Authentication header.

Note: If the service you want to call provides a Cloud Code C# SDK, you can use the SDK instead of calling the service API directly. For a list of available SDKs, refer to Available libraries page.

A service token is a JWT that Cloud Code generates. You can use service tokens to authenticate services as Cloud Code. This is a service account token that's gone through the Token Exchange) process.

You can use the service token with the Cloud Code C# Client SDKs. To use it with other APIs, you need to pass it as a bearer token in the Authentication header.

For a list of services that support service tokens, refer to the table below:

Note: We are still working on the documentation for Cloud Code C# Client SDKs. For now you can refer to the JavaScript documentation as it has a similar structure. Please also make use of your IDE's auto complete and intellisense features.

To use a ServiceToken with the Cloud Code C# Client SDKs, pass the token from the IExecutionContext interface.

The example below takes in a player ID and increments the balance of a currency for that player:

To use a ServiceToken with the UGS Client APIs, pass the token as a bearer token in the Authentication header.

Note: If the service you call provides a Cloud Code C# SDK, you can use the SDK instead of calling the service API directly. For a list of available SDKs, refer to the Available libraries page.

For example, if you define an interface for Cloud Save API, you can use the ServiceToken to authenticate and interact with cross-player data. The example below takes in a player ID and returns all the data stored for that player:

Note: Cloud Code provides a Cloud Save C# SDK that you can use to interact with Cloud Save data. This sample above is demonstrates how you can use the ServiceToken to authenticate with other UGS services.

**Examples:**

Example 1 (unknown):
```unknown
IExecutionContext
```

Example 2 (unknown):
```unknown
AccessToken
```

Example 3 (unknown):
```unknown
ServiceToken
```

Example 4 (unknown):
```unknown
accessToken
```

---

## VAD adjustments

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/troubleshooting/vad-adjustments-unreal

**Contents:**
- VAD adjustments#
  - Setting the VAD Hangover, Sensitivity, and Noise Floor#

To make adjustments to the VAD settings, use:

In this example, the default values for the vad_hangover, vad_sensitivity, and vad_noise_floor are provided, and vad_auto has been disabled.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_aux_set_vad_properties
```

Example 2 (unknown):
```unknown
vx_req_aux_get_vad_properties
```

Example 3 (unknown):
```unknown
vad_hangover
```

Example 4 (unknown):
```unknown
vad_sensitivity
```

---

## Text-to-speech events

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/Unreal/developer-guide/text-to-speech/tts-events

**Contents:**
- Text-to-speech events#

The SDK can sometimes raise the following ITextToSpeech text-to-speech (TTS) events.

Note: You can bind to these events by using ILoginSession::TTS().

EventPlaybackStarted - Raised when a TTS message has finished preparing for playback and is starting to play.

EventPlaybackCompleted - Raised when a TTS message has finished playback.

EventPlaybackFailed - Raised when playback of a TTS message has failed.

**Examples:**

Example 1 (unknown):
```unknown
ITextToSpeech
```

Example 2 (unknown):
```unknown
ILoginSession::TTS()
```

Example 3 (unknown):
```unknown
EventPlaybackStarted
```

Example 4 (unknown):
```unknown
ITTSMessage
```

---

## Text evidence management

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/chat-history/text-evidence-management

**Contents:**
- Text evidence management#

Text evidence management is an optional feature that helps moderators evaluate player reports with conversational evidence. To enable this feature, contact Unity Support or your Vivox representative.

With this server-to-server API, you can request text messages from player chat history to review instances of reported behavior. All versions are stored for review if a message was edited, deleted, or redacted by the Chat filter. The text evidence management feature contains a text-evidence-report endpoint and a player-evidence-report endpoint. The text-evidence-report endpoint returns a report for a given channel or direct message. The player-evidence-report endpoint returns a list of conversations a player participated in with optional time filters.

When active, Text Evidence Management has a storage limit of 30 days, extending the storage limit of Chat history.

The API leverages Unity Dashboard service accounts for authentication. After enablement, follow the admin authentication instructions (Unity Services Web API Docs) to create a service account and add the Text Evidence Management role.

---

## Data safety requirements for Unity Gaming Services products

**URL:** https://docs.unity.com/ugs/en-us/manual/overview/manual/data-safety-requirements

**Contents:**
- Data safety requirements for Unity Gaming Services products#
- Build your foundations#
  - Accounts#
  - Content management#
  - Multiplayer services#
- Engage your players#
  - Analytics and Player Engagement#
  - Community#
  - Crash Reporting#
- Grow your game#

Developers publishing on the App Store, for Apple platforms, or the Google Play Store, for Android, must define what data their apps collect, including the data that integrated third-party SDKs collect. For your convenience, Unity Gaming Services provides information on its data collection practices for each product.

---

## Example: Python

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/generate-token-secure-server/example-python

**Contents:**
- Example: Python#

The following code is an example of how to generate access token in Python.

**Examples:**

Example 1 (python):
```python
#!/usr/env/python

#############################################
# Vivox Token Generation Example - Python
#############################################

import base64
import hashlib
import hmac
import json


def b64url(value):
    """Return a base64url-encoded str without trailing ='s"""
    return base64.urlsafe_b64encode(value).rstrip('=')


def vx_generate_token(key, issuer, exp, vxa, vxi, f, t):
    # Create dictionary of claims
    claims = {
        'iss': issuer,
        'exp': exp,
        'vxa': vxa,
        'vxi': vxi,
        'f': f,
        't': t
    }

    # Header is static - base64url encoded {}
    header = b64url('{}')  # Can also be defined as a constant "e30"

    # Encode claims payload
    json_payload = b64url(json.dumps(claims))

    # Join segments to prepare for signing
    segments = [header, json_payload]
    to_sign = b'.'.join(segments)

    # Sign token with key and HMACSHA256
    sig = hmac.new(key, to_sign, hashlib.sha256).digest()
    segments.append(b64url(sig))

    # Join all 3 parts of token with . and return
    return b'.'.join(segments)


if __name__ == '__main__':
    # Example usage
    token = vx_generate_token('secret', 'issuer', 1559359105, 'join', 123456, 'sip:.username.@domain.vivox.com', 'sip:confctl-g-channelname@domain.vivox.com')
    print(token)
```

Example 2 (python):
```python
#!/usr/env/python

#############################################
# Vivox Token Generation Example - Python
#############################################

import base64
import hashlib
import hmac
import json


def b64url(value):
    """Return a base64url-encoded str without trailing ='s"""
    return base64.urlsafe_b64encode(value).rstrip('=')


def vx_generate_token(key, issuer, exp, vxa, vxi, f, t):
    # Create dictionary of claims
    claims = {
        'iss': issuer,
        'exp': exp,
        'vxa': vxa,
        'vxi': vxi,
        'f': f,
        't': t
    }

    # Header is static - base64url encoded {}
    header = b64url('{}')  # Can also be defined as a constant "e30"

    # Encode claims payload
    json_payload = b64url(json.dumps(claims))

    # Join segments to prepare for signing
    segments = [header, json_payload]
    to_sign = b'.'.join(segments)

    # Sign token with key and HMACSHA256
    sig = hmac.new(key, to_sign, hashlib.sha256).digest()
    segments.append(b64url(sig))

    # Join all 3 parts of token with . and return
    return b'.'.join(segments)


if __name__ == '__main__':
    # Example usage
    token = vx_generate_token('secret', 'issuer', 1559359105, 'join', 123456, 'sip:.username.@domain.vivox.com', 'sip:confctl-g-channelname@domain.vivox.com')
    print(token)
```

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/cloud-code/manual/modules/how-to-guides/write-modules/unity-editor

---

## Requirements

**URL:** https://docs.unity.com/ugs/en-us/manual/moderation/manual/requirements

**Contents:**
- Requirements#
- Requirements for users#
- Dependencies#

Unity Moderation has the following requirements:

To access the Moderation Platform, project members must have one of two roles: Safety Moderator or Safety Admin. Each role comes with distinct permissions:

To assign a role to a user:

Organization members that have the base role “Owner” or “Manager” have all the safety roles applied by default.

To use the Moderation SDK you will need additional Unity packages. Some are mandatory requirements while others are optional requirements:

For information on the Authentication package, refer to the Authentication documentation.

---

## Send a directed message

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/sdksampleapp/send-directed-message

**Contents:**
- Send a directed message#

When you send a directed message (account_send_message in the Vivox SDK), you need to know the URI of the participant, which starts with "sip:" and ends with "@mt1s.vivox.com".

The following code displays an example of the user Alice sending a directed message to the user Bob:

Bob sees the message as a user_to_user_message event:

**Examples:**

Example 1 (unknown):
```unknown
[SDKSampleApp]: message -u sip:.xyzzy.bob.@mt1s.vivox.com -m "hey bob"
* Sending directed message 'hey bob' to user sip:.xyzzy.bob.@mt1s.vivox.com
* Issuing req_account_send_message with cookie=3
```

Example 2 (unknown):
```unknown
[SDKSampleApp]: message -u sip:.xyzzy.bob.@mt1s.vivox.com -m "hey bob"
* Sending directed message 'hey bob' to user sip:.xyzzy.bob.@mt1s.vivox.com
* Issuing req_account_send_message with cookie=3
```

Example 3 (unknown):
```unknown
* evt_user_to_user_message: .xyzzy.bob. sip:.xyzzy.sa_c78d1595.@mt1s.vivox.com - hey bob
```

Example 4 (unknown):
```unknown
* evt_user_to_user_message: .xyzzy.bob. sip:.xyzzy.sa_c78d1595.@mt1s.vivox.com - hey bob
```

---

## Disable the Vivox SDK internal AEC

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/ios/disable-internal-aec

**Contents:**
- Disable the Vivox SDK internal AEC#

It is possible to disable the Vivox SDK internal acoustic echo cancellation (AEC), which can save processing power and battery.

Caution: Only disable the Vivox SDK internal AEC in circumstances where you are certain AEC is not needed, such as when headphones are plugged in or when the Voice-Processing I/O audio unit is activated.

To disable the Vivox SDK internal AEC, invoke the vx_set_vivox_aec_enabled(0) call.

**Examples:**

Example 1 (unknown):
```unknown
vx_set_vivox_aec_enabled(0)
```

---

## Audio injection

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/audio/core-audio-injection

**Contents:**
- Audio injection#

Note: There is a known issue with the Vivox SDK in which the local playback of the injected audio only occurs if the file also has the same sample rate as the negotiated audio codec. Other players always hear the file correctly, regardless of the sample rate. For Windows, this usually is 48kHz.

Audio injection allows you to broadcast audio from a file to all connected sessions in a single session group. Injected audio is treated like a second capture device you are speaking into. This means that muting or disconnecting your input device does not stop others from hearing the file audio. However, muting yourself in a channel, stopping transmission to a channel, or disconnecting a channel's media stream does stop others from hearing the file audio. Because the played file is treated as speech, it will beheard by anyone that would typically hear the user. It also triggers speaking indicators and VU meters like speech does, and is heard by participants in accordance with the player's position and orientation in a 3D channel, or when in a 2D channel, from anywhere on the map.

Use cases for this feature include a player selecting pre-scripted audio clips in their character's voice saying things like "Enemy spotted!" or "Need healing!" to signal their situation, which is especially useful when the player does not have a microphone. Depending on your application, it might also be appropriate to let the user play music into the channel for everyone to listen to. When the file being injected ends, the vx_evt_media_completion event will trigger, you can use it to loop an audio track when complete or move to the next track in a playlist.

You can broadcast only one file at a time. Successfully completed vx_req_sessiongroup_control_audio_injection requests with a "start" control type will play the specified file from the beginning only if no file is already playing; a "restart" control type always begins playing the specified file from the beginning and replaces any file that was previously playing. You can stop file audio from playing at any time by making the request with a "stop" control type. Note that injecting file audio does not replace or otherwise prevent the user from speaking into their capture device and being heard as normal; it is considered to be an additional audio source.

The file also plays locally for the user while it is being transmitted to others to simulate the experience of file injection as a second spoken audio source. This means that a user playing a file into a non-positional or a positional channel hears the file slightly earlier than other participants, just like their own voice when speaking. The file plays with an echo in an echo channel, just like your own voice.

Important: Audio files broadcast with this feature must have a .wav extension and contain single channel, 16-bit PCM audio. Otherwise, the vx_resp_sessiongroup_control_audio_injection response returns VxErrorInvalidFormat, and the file does not play.

**Examples:**

Example 1 (unknown):
```unknown
vx_evt_media_completion
```

Example 2 (unknown):
```unknown
vx_req_sessiongroup_control_audio_injection
```

Example 3 (unknown):
```unknown
vx_resp_sessiongroup_control_audio_injection
```

---

## Channel-based message edit

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unreal/manual/text-chat-guide/chat-history/edit-delete/channel-message-edit

**Contents:**
- Channel-based message edit#

To edit a channel-based message, you need to sign in and join at least one channel. The message to be edited must have been sent to every participant in the channel.

Note: Only the sender of a message can edit it.

Use vx_req_session_edit_message to make an edit request. vx_req_session_edit_message will return:

You will receive a vx_evt_session_edit_message event for every message edited.

The following code is an example of how to edit a message for a specific channel:

After you edit this message, the game must then process the vx_evt_session_edit_message event.

**Examples:**

Example 1 (unknown):
```unknown
vx_req_session_edit_message *req;
vx_req_session_edit_message_create(&req);
req->session_handle = vx_strdup("channelName");
req->message_id = vx_strdup("oldMessageID");
req->new_message = vx_strdup("This is a test message");
vx_issue_request2(&req->base);
```

Example 2 (unknown):
```unknown
vx_req_session_edit_message *req;
vx_req_session_edit_message_create(&req);
req->session_handle = vx_strdup("channelName");
req->message_id = vx_strdup("oldMessageID");
req->new_message = vx_strdup("This is a test message");
vx_issue_request2(&req->base);
```

Example 3 (unknown):
```unknown
void HandleSessionEditMessage(vx_evt_session_edit_message *evt)
{
    // Use information in events to display messages
    …
}
void HandleSessionEditMessage(vx_resp_session_edit_message_t *resp)
{
    if (resp != nullptr) {
       // Ended properly
    } else {
       // Ended abnormally
    }
}
```

Example 4 (unknown):
```unknown
void HandleSessionEditMessage(vx_evt_session_edit_message *evt)
{
    // Use information in events to display messages
    …
}
void HandleSessionEditMessage(vx_resp_session_edit_message_t *resp)
{
    if (resp != nullptr) {
       // Ended properly
    } else {
       // Ended abnormally
    }
}
```

---

## Generate a token on the client

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/access-token-guide/generate-tokens-core

**Contents:**
- Generate a token on the client#

Important: Use the following access token generation methods during initial development and prototyping. Do not include this method of generation in production builds of your game. Allowing token generation on the client during production is both a security risk and can cause unexpected token expiration errors for your users.

To generate your access token, call vx_debug_generate_token() with the following parameters:

The following examples display access tokens for login, join, join_muted, kick, and mute, respectively:

**Examples:**

Example 1 (unknown):
```unknown
vx_debug_generate_token()
```

Example 2 (javascript):
```javascript
static long long serialNumber = 0;
// Login
request->access_token = vx_debug_generate_token("issuer", time(NULL) + 120, "login", serialNumber++, NULL, "sip:.issuer.me.@mt1s.vivox.com", NULL, (const unsigned char *)key, (int)strlen(key));

// Join
request->access_token = vx_debug_generate_token("issuer", time(NULL) + 120, "join", serialNumber++, NULL, "sip:.issuer.me.@mt1s.vivox.com", "sip:confctl-g-issuer.lobby@mt1s.vivox.com", (const unsigned char *)key, (int)strlen(key));

// Join Muted
request->access_token = vx_debug_generate_token("issuer", time(NULL) + 120, "join_muted", serialNumber++, NULL, "sip:.issuer.me.@mt1s.vivox.com", "sip:confctl-g-issuer.lobby@mt1s.vivox.com", (const unsigned char *)key, (int)strlen(key));

// Kick
request->access_token = vx_debug_generate_token("issuer", time(NULL) + 120, "kick", serialNumber++, "sip:.issuer.them.@mt1s.vivox.com", "sip:.issuer.me.@mt1s.vivox.com", "sip:confctl-g-issuer.lobby@mt1s.vivox.com", (const unsigned char *)key, (int)strlen(key));

// Mute
request->access_token = vx_debug_generate_token("issuer", time(NULL) + 120, "mute", serialNumber++, "sip:.issuer.them.@mt1s.vivox.com", "sip:.issuer.me.@mt1s.vivox.com", "sip:confctl-g-issuer.lobby@mt1s.vivox.com", (const unsigned char *)key, (int)strlen(key));

// Transcription
request->access_token = vx_debug_generate_token("issuer", time(NULL) + 120, "trxn", serialNumber++, NULL, "sip:.issuer.me.@mt1s.vivox.com", "sip:confctl-g-issuer.lobby@mt1s.vivox.com", (const unsigned char *)key, (int)strlen(key));
```

Example 3 (javascript):
```javascript
static long long serialNumber = 0;
// Login
request->access_token = vx_debug_generate_token("issuer", time(NULL) + 120, "login", serialNumber++, NULL, "sip:.issuer.me.@mt1s.vivox.com", NULL, (const unsigned char *)key, (int)strlen(key));

// Join
request->access_token = vx_debug_generate_token("issuer", time(NULL) + 120, "join", serialNumber++, NULL, "sip:.issuer.me.@mt1s.vivox.com", "sip:confctl-g-issuer.lobby@mt1s.vivox.com", (const unsigned char *)key, (int)strlen(key));

// Join Muted
request->access_token = vx_debug_generate_token("issuer", time(NULL) + 120, "join_muted", serialNumber++, NULL, "sip:.issuer.me.@mt1s.vivox.com", "sip:confctl-g-issuer.lobby@mt1s.vivox.com", (const unsigned char *)key, (int)strlen(key));

// Kick
request->access_token = vx_debug_generate_token("issuer", time(NULL) + 120, "kick", serialNumber++, "sip:.issuer.them.@mt1s.vivox.com", "sip:.issuer.me.@mt1s.vivox.com", "sip:confctl-g-issuer.lobby@mt1s.vivox.com", (const unsigned char *)key, (int)strlen(key));

// Mute
request->access_token = vx_debug_generate_token("issuer", time(NULL) + 120, "mute", serialNumber++, "sip:.issuer.them.@mt1s.vivox.com", "sip:.issuer.me.@mt1s.vivox.com", "sip:confctl-g-issuer.lobby@mt1s.vivox.com", (const unsigned char *)key, (int)strlen(key));

// Transcription
request->access_token = vx_debug_generate_token("issuer", time(NULL) + 120, "trxn", serialNumber++, NULL, "sip:.issuer.me.@mt1s.vivox.com", "sip:confctl-g-issuer.lobby@mt1s.vivox.com", (const unsigned char *)key, (int)strlen(key));
```

---

## 

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/android/android-library-requirements

---

## Channel-based chat history for blocked participants

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/text-chat-guide/chat-history/channel-based-ch-blocked-participant

**Contents:**
- Channel-based chat history for blocked participants#
- History sessions when blocked participants are involved#

You can filter messages from blocked users when formatting the history_session query responses.

When retrieving the chat history from a channel, the SDK omits messages from blocked users. If User A has User B blocked in the application, User A does not see messages sent by User B.

A history request can come back empty if all messages in the request are from a blocked participant. In this case, the response contains a cursor indicating that there are more messages in the channel on subsequent pages. The default amount of messages returned per history request is 10 per page. Use the cursor value in the following request to retrieve the next page of messages.

The maximum page size argument (Max) shows fewer messages than the number requested if there is a blocked user in the channel. When you request a specific page size for your chat history, be aware that history_session might display fewer messages than your chosen number if there are blocked users in the channel. This functionality is crucial to ensure privacy and prevent unwanted interactions.

The last read displays a count of unread messages. Vivox keeps track of your read messages and provides a count of unread messages. However, note that this count includes blocked messages, which are deliberately filtered from your response. The count of unread messages might be inaccurate if there are blocked users in the channel.

**Examples:**

Example 1 (unknown):
```unknown
history_session
```

Example 2 (unknown):
```unknown
history_session
```

---

## Channel identifiers for large scale games

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-core/manual/Core/developer-guide/channels/channel-identifiers-for-large-scale-games

**Contents:**
- Channel identifiers for large scale games#

If either of the following statements are true, then use caution when constructing your channel identifiers:

You can split large scale games across multiple physical audio servers ("shards") at the discretion of Vivox Operations. Construct your channel identifiers to use the following format:

For example, consider the application "Queen of the Death" (QotD), which allocates users according to geographical region, for example,"NA" for "North America". Assume that QotD joins a 3D channel for all participants in a match, and then also joins a 2D channel for members of a squad. In this case, the best practice for QotD is to apply the following criteria:

The match identifier assigned for this match is d0634bad1ca5a9cd, and the 3D space is tundra. Further assume that squads are assigned integer IDs starting with 1.

2D channel URI for squad 1

2D channel URI for squad 2

Caution: Failing to follow these guidelines might result in multichannel users not all being co-located on the same shard.

**Examples:**

Example 1 (unknown):
```unknown
sip:confctl-d-issuer.NA.d0634bad1ca5a9cd-tundra@example.vivox.com
```

Example 2 (unknown):
```unknown
sip:confctl-d-issuer.NA.d0634bad1ca5a9cd-tundra@example.vivox.com
```

Example 3 (unknown):
```unknown
sip:confctl-g-issuer.NA.d0634bad1ca5a9cd-1@example.vivox.com
```

Example 4 (unknown):
```unknown
sip:confctl-g-issuer.NA.d0634bad1ca5a9cd-1@example.vivox.com
```

---

## Create a fleet

**URL:** https://docs.unity.com/ugs/en-us/manual/game-server-hosting/manual/guides/create-a-fleet

**Contents:**
- Create a fleet#
- Define the fleet details#
- Define the fleet scaling settings#
- Define the server density#

The embedded Multiplay Hosting setup guide walks you through creating your first fleet. However, you can always add more fleets by following these instructions.

Note: You can create up to 25 fleets per organization.

The first step in the process of creating a fleet is defining the details of the fleet, such as the name, the operating system, and the build configurations.

One of the most important considerations of a fleet is the scaling settings. The scaling settings of a fleet are composed of a Min available servers value and a Max servers value for each region within the fleet.

Warning: Minimum available servers will incur costs, even with no traffic. If you are trying to limit costs, we suggest you set a minimum of 0.

At this stage, it is time to define the server density for this fleet, telling Multiplay Hosting how many resources each server has access to when running this build using this build configuration. There are separate workflows for configuring cloud and bare metal server density.

You now have a fleet, a build configuration, and a build configured and ready. To view the servers for this fleet, go to Multiplay Hosting > Servers.

---

## Integrate an existing project using VATs with Unity Authentication

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/developer-guide/implement-vivox-unity/integrate-existing-vat-project-with-uas

**Contents:**
- Integrate an existing project using VATs with Unity Authentication#
- Sign In#
- Creating VATs#
- Steps#
  - 1. Update sign-In flow#
  - 2. Update VAT creation flow#

If you already have a Vivox integration that's using VATs for players to join channels and interact with Vivox, you can use this guide to integrate with UAS to access the full suite of services Unity provides.

You will have to make some changes to the player sign-in flow and the VAT creation flow.

Overall, the two flows will work like this:

Important: You might not need to use VATs at all. VATs are only needed if you have custom logic in your backend to determine if a player is allowed to access a specific channel. If this is not the case, you can simply remove the VAT generation flow and let the Unity Authentication SDK handle it. Simply use the default initialization for Vivox: await VivoxService.Instance.InitializeAsync(); and skip the Creating VATs step.

Follow these tutorials to update your sign-in and VAT creation flows.

To set up the custom ID sign-in, follow the steps in Sign-in with Custom IDs.

To adapt your VAT creation flow to integrate with UAS, follow the steps in Using Vivox Access Tokens together with UAS.

With all of this done, you are now ready to use any of the Unity Gaming Services, including the Vivox Moderation Platform.

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

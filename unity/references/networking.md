# Unity - Networking

**Pages:** 12

---

## Use Relay with UTP

**URL:** https://docs.unity.com/ugs/en-us/manual/relay/manual/relay-and-utp

**Contents:**
- Use Relay with UTP#
- Configure the Relay SDK#
- Simple Relay sample (using UTP)#
  - Import the Simple Relay sample (using UTP) project#
  - Initialize Unity Services#
  - Authenticate the player#
- Host player#
  - The host player update loop#
  - Create an allocation#
  - Bind to the Relay server and listen for connections#

The Relay SDK works well with the Unity Transport Package (UTP), a modern networking library built for the Unity game engine to abstract networking. UTP lets developers focus on the game rather than low-level protocols and networking frameworks. UTP is netcode-agnostic, which means it can work with various high-level networking code abstractions, supports all Unity’s netcode solutions, and works with other netcode libraries.

Note: The recommended best practice is to use Relay with NGO. If you want to use something besides NGO (such as custom netcode) to synchronize GameObjects, you can use Relay and UTP for transport and Relay proxy purposes.

You must configure your Unity project for Unity before using Relay. To do so, visit Get started with Relay. After you’ve enabled the Relay service, link your Unity Project ID through the Unity Editor.

Check out the Simple Relay sample (using UTP) to learn the basics of using Relay with UTP.

This sample expands on the Simple Relay sample. It demonstrates the basics of building a multiplayer game using Relay with UTP. Import the sample from the Relay package in the Package Manager, and follow along with the included script.

This sample works with at least two separate clients. One host player and one or more joining players.

You might want to run multiple clients on the same machine when you test the sample project code. You can do so by building the project into an executable binary or using a solution, such as ParrelSync or Multiplayer Play Mode, that allows you to run multiple Unity Editor instances of the same project.

If you build the project into a binary, you can use the binary in conjunction with the open Unity Editor or with clones of the binary.

Upon playing the scene, you can start the client as a host player (who initiates the game session) or as a joining player.

You must have at least one host player, and the host player is necessary to run the game for the joining players.

The flow for the host player is as follows:

Get regions (optional)

Select region (optional)

Allocate game session

Bind to the selected Relay server

The host player shares the join code with joining players. After they join and connect to the host player, the host player can:

Send messages to all connected players

Disconnect all connected players

The flow for a player is as follows:

Join the game session using the host player's join code

Bind to the selected Relay server

Request a connection to the host player

Once connected to the host player, a player can:

Send a message to the host player

Disconnect from the host player

Open a Relay project with the Unity Editor (version 2020.3). If you don’t have a Relay project, visit Set up a Relay project.

Open the Package Manager and navigate to the Relay package.

Expand the Samples section.

Select Import to import the Simple Relay sample (using UTP) project.

Now that you have imported the Simple Relay sample project, you can open it as a scene. It's located within the current project under Assets/Samples/Relay/1.0.4/Simple Relay sample (using UTP).

Select File > Open Scene.

Navigate to the Simple Relay sample (using UTP) scene.

Before using any Unity services, such as Relay, you must initialize the UnityServices singleton. Check out the Services Core API.

You must authenticate both the host player and the connecting players. The simplest way to authenticate players is with the Authentication service's SignInAnonymouslyAsync() method. Visit How to use Anonymous Sign-in and How to use External Sign-in.

For more information, visit About Unity Authentication and Unity Authentication use cases.

When your game client functions as a host player, it must be able to create an allocation, request a join code, configure the connection type, and create an instance of the NetworkDriver to bind the Relay server and listen for connection requests from joining players.

Before starting the first steps of the allocation flow, you need to set the host player’s update loop, which handles incoming connections and messages.

Note: For simplicity, the sample code calls this loop on every Update() frame. Handling the host player update loop in a separate coroutine makes more sense in practice.

The following code snippet has a function, OnAllocate, that shows how to use the Relay SDK to create an allocation.

Warning: You won't be able to connect to the Relay server if there's a mismatch between the information provided through SetRelayServerData / SetRelayClientData and the information obtained from the allocation. For example, you'll receive a "Failed to connect to server" error message if the isSecure parameter doesn't match.

One of the easiest ways to prevent this from happening is to build the RelayServerData directly from the allocation. However, you must use Netcode for GameObjects (NGO) version 1.1.0 or later.

The following code snippet has a function, OnBindHost, that shows how to use the Relay SDK to bind to a host player to the Relay server and listen for connection requests from joining players.

The following code snippet has a function, OnJoinCode, that shows how to request a join code after successfully requesting an Allocation. The host player shares this join code with joining players so they can join the host player in their game session.

When your game client functions as a joining player, it must be able to join an allocation, configure the connection type, and create an instance of its NetworkDriver to bind to the host player’s Relay server and send a connection request to the host player.

Note: The default behavior of the DefaultDriverBuilder.RegisterClientDriver method uses to create the NetworkDriver is to avoid using a socket connection if possible when a player joins the Relay server. However, the client uses a socket to connect to the server in the following scenarios:

The player update loop

As with the host player, before joining the host player’s game session, you need to set the joining player’s update loop, which handles incoming messages.

Note: For simplicity, the sample code calls this loop on every Update() frame. However, handling the player update loop in a separate coroutine makes more sense in practice.

The following code snippet has a function, OnJoin, that shows how to use the Relay SDK to join an allocation with a join code and configure the connection type as UDP.

Warning: You won't be able to connect to the Relay server if there's a mismatch between the information provided through SetRelayServerData / SetRelayClientData and the information obtained from the allocation. For example, you'll receive a "Failed to connect to server" error message if the isSecure parameter doesn't match.

One of the easiest ways to prevent this from happening is to build the RelayServerData directly from the allocation. However, you must use Netcode for GameObjects (NGO) version 1.1.0 or later.

The following code snippet has a function, OnBindPlayer, that shows how to bind to the Relay server.

Note: This binds the player to the Relay server. The next step connects the player to the host player so they can begin exchanging messages.

The following code snippet has a function, OnConnectPlayer, that shows how to bind to send a connection request to the host player. This appears as an incoming connection in the host player’s update loop, as it calls hostDriver.Accept().

Now that the host player and player are connected, they can communicate by sending messages.

The following code snippet has a function, OnHostSendMessage, that shows how a host player can send messages to its connected players. In this sample, the host broadcasts messages to all connected players.

The following code snippet has a function, OnPlayerSendMessage, that shows how a player can send a message to the host player.

After a player finishes, they can disconnect from the host. A host player can also forcefully disconnect a connected player.

The following code snippet has a function, OnDisconnectPlayers, that shows how a host player can forcefully disconnect a connected player. In this sample, the host player disconnects all connected players.

The following code snippet has a function, OnDisconnect, that shows how a player can disconnect from the host player.

**Examples:**

Example 1 (unknown):
```unknown
await UnityServices.InitializeAsync();
```

Example 2 (unknown):
```unknown
await UnityServices.InitializeAsync();
```

Example 3 (unknown):
```unknown
SignInAnonymouslyAsync()
```

Example 4 (unknown):
```unknown
public async void OnSignIn()
{
    	await  AuthenticationService.Instance.SignInAnonymouslyAsync();
    	playerId = AuthenticationService.Instance.PlayerId;

    	Debug.Log($"Signed in. Player ID: {playerId}");
}
```

---

## Netcode for GameObjects (NGO) vs Mirror

**URL:** https://docs.unity.com/ugs/en-us/manual/relay/manual/ngo-vs-mirror-for-relay

**Contents:**
- Netcode for GameObjects (NGO) vs Mirror#

Two of the most popular solutions for the underlying netcode of a Relay game include Netcode for GameObjects (NGO) and the Mirror Networking API.

The recommended best practice is to use NGO (in most cases) because it offers a stable breadth of mid-level features, such as network variables, scene management, remote procedure calls (RPCs), and messaging. However, the Mirror Networking API, with its simplicity and ease of use, is also an excellent choice for games that don't need the full set of features NGO provides.

---

## Deploying Multiplayer configurations

**URL:** https://docs.unity.com/ugs/en-us/manual/mps-sdk/manual/deployment/deployment-window-integration

**Contents:**
- Deploying Multiplayer configurations#
- Additional resources#

Use the Deployment window to deploy Multiplayer Services assets.

The Multiplayer Services package exposes an interface to configure the Multiplay Hosting and Matchmaker services in the Unity Editor from the Deployment window. You can use the Deployment window, instead of the Unity Dashboard, to deploy configuration assets to these services.

---

## Multiplayer

**URL:** https://docs.unity.com/ugs/en-us/manual/mps-sdk/multiplayer-landing

**Contents:**
- Multiplayer#

Create and manage multiplayer experiences using Unity's integrated suite of services.

Unity Multiplayer Services provides a comprehensive framework for building multiplayer games, offering solutions for networking, server hosting, and player connections.

The Multiplayer Services (MPS) SDK unifies Lobby, Matchmaker, Multiplay Hosting, and Relay under a single, cohesive API, replacing their individual SDKs. This consolidated approach reduces development complexity while maintaining the full capabilities of Unity's multiplayer infrastructure.

The MPS SDK introduces sessions, an abstraction layer that manages how players connect and interact in multiplayer games. Sessions handle the complexities of player grouping, connection management, and game state coordination, streamlining the development of both peer-to-peer games and dedicated server experiences.

---

## Networking

**URL:** https://docs.unity.com/ugs/en-us/manual/relay/manual/networking

**Contents:**
- Networking#

Note: WebSocket is only supported on UTP 2.0.0-pre.3+.

The Relay service allows players to communicate over several different protocols, including UDP, datagram TLS (DTLS), and secure WebSocket (WSS). After allocated to a Relay server, clients talk directly to the Relay server using one of the aforementioned protocols. Relay BIND messages are further authenticated through HMAC signatures. The Allocation service relies on UAS authentication and HTTPS encryption.

WebSocket connections allow for multiplayer connectivity on browsers using WebGL.

For connections to a Relay allocation, for any of the supported protocols, the system uses a port range of 37000 to 37100.

Check out Authentication, DTLS encryption, Scaling, and Locations.

---

## Use Relay with Netcode for GameObjects

**URL:** https://docs.unity.com/ugs/en-us/manual/relay/manual/relay-and-ngo

**Contents:**
- Use Relay with Netcode for GameObjects#
- Installation and configuration#
- Set up the NetworkManager#
- Host player#
- Joining player#
- Note about Unity Web platform support#

Important: If you're using the standalone Relay package, refer to Use Relay with Netcode for GameObjects (Relay SDK).

Relay works seamlessly with Netcode for GameObjects, which is a Unity package that provides networking capabilities to GameObject and MonoBehaviour workflows.

This tutorial demonstrates how to use Relay with Netcode for GameObjects.

Note: For most users, the unified Multiplayer Services package replaces the Relay standalone package, which is deprecated in Unity 6. Consider migrating to the unified package to facilitate a smooth transition. Visit the migration guide for a step-by-step transition process.

You must first configure your Unity project for Unity before using Relay with Netcode for GameObjects. Refer to Get started with Relay to learn how to link your project in the project settings.

After installing the packages, you can now set up the NetworkManager:

The StartHostWithRelay function shows how to create a Relay allocation, and request a join code. This function requires the maximum number of connections the allocation is expecting and the host player connection type.

The connection type must be one of the following options:

Refer to DTLS to learn more about DTLS encryption, and to Web Platform Support to learn about using wss.

This code should be adapted to your needs to use a different authentication mechanism, a different error handling, or to use a different connection type. Similarly, you can call StartServer instead of StartHost to start a server instead of a host.

Note: The Multiplayer Services Package introduces the concept of sessions. Check out Build a session with Netcode for GameObjects to learn how to use it.

Note: If this code succeeds in calling the NetworkManager.Singleton.StartHost(), it returns the join code retrieved from the Relay allocation.

When your game client functions as a joining player, the relay join code, retrieved in the previous step when the host created the allocation, must be passed to find the allocation. The following code samples show how to join an allocation with a join code and configure the connection type.

The connection type must be one of the following options:

Refer to DTLS to learn more about DTLS encryption, and to Web Platform Support to learn about using wss.

Note: The Multiplayer Services Package introduces the concept of session. Check out Build a session with Netcode for GameObjects to learn how to use it.

To use Relay with Netcode for GameObjects in Unity Web platform supported game, upgrade the Unity Transport Package to 2.0.0 or later and configure the Unity transport component to use Web Sockets.

Using the above code snippets, pass wss as connectionType and use the following to SetRelayServerData:

**Examples:**

Example 1 (unknown):
```unknown
MonoBehaviour
```

Example 2 (unknown):
```unknown
com.unity.services.multiplayer
```

Example 3 (unknown):
```unknown
com.unity.netcode.gameobjects
```

Example 4 (unknown):
```unknown
NetworkManager
```

---

## Relay integrations

**URL:** https://docs.unity.com/ugs/en-us/manual/relay/manual/integration

**Contents:**
- Relay integrations#
- Lobby#
- Authentication#
- Unity Transport Package#
- Netcode for GameObjects (NGO)#
- Mirror Networking API#

As part of the Unity ecosystem, Relay integrates well with other Unity products, including Lobby, Unity Authentication, Unity Transport Package, Netcode for GameObjects, and the Mirror Networking API.

Note: For the time being, the Unity Relay SDK has a hard dependency on the Unity Transport Package (UTP). Check out Requirements and limitations.

The Lobby service allows you to connect players before or during a game session with public or private lobbies. You can use the Lobby service to group players together in a lobby before starting a game session or prevent connection loss if a host player becomes unavailable. Check out the Lobby documentation.

Note: Check out Lobby vs Relay to understand where Lobby and Relay overlap in their capabilities.

Unity Authentication provides anonymous and platform-specific authentication solutions for supported platforms, including mobile and PC (personal computers). You can use Unity Authentication to authenticate players with Unity services, including Relay. Check out the Unity Authentication documentation.

Relay leverages the Unity Transport Package (UTP) to offer a connection-based abstraction layer over UDP sockets with optional functionality like reliability, ordering, and fragmentation. You can use Relay with UTP and NGO, or only UTP if you prefer an alternative netcode library.

Check out the UTP documentation.

Relay works seamlessly with the Netcode for GameObjects (NGO) package to offer Relay networking capabilities to GameObject and MonoBehaviour workflows.

Check out the NGO documentation.

Relay supports the Mirror Networking API. Check out the Unity Mirror sample project documentation to learn more.

---

## Get started

**URL:** https://docs.unity.com/ugs/en-us/manual/mps-sdk/manual/get-started

**Contents:**
- Get started#

After you install the Multiplayer Services package, you're now ready to begin adding multiplayer elements to your game with sessions. A session represents a group of players that are connected together for a set period of time during a multiplayer game. Players can join sessions through matchmaking, a Join Code, or by browsing a list of active sessions.

An effective way to learn how to include sessions in your game is through tutorials using either the Netcode for GameObjects or the Netcode for Entities networking libraries. For a simple example, begin with Build a session with Netcode for GameObjects. Next, complete Build a session with Netcode for Entities for a more complex lesson for projects using Data-Oriented Technology Stack (DOTS). Completing these tutorials results in a simple multiplayer game that gives you the foundational knowledge you need to add multiplayer functionality to your games using the Multiplayer Services package.

To learn how to interact programmatically with your multiplayer game, refer to Integrating Multiplayer Sessions in your game.

Familiarize yourself with the following key terms as they relate to sessions before you continue:

---

## 

**URL:** https://docs.unity.com/ugs/en-us/solutions/manual/ServerlessMultiplayerGame

---

## Multiplayer sessions

**URL:** https://docs.unity.com/ugs/en-us/manual/mps-sdk/manual

**Contents:**
- Multiplayer sessions#
- Additional resources#

Note: The Multiplayer Services SDK uses sessions to manage groups of players. Sessions relies internally on different combinations of Unity Gaming Services such as Relay, Distributed Authority, Lobby, Matchmaker and Multiplay Hosting, and thus contributes to the billing of those services.

Implement multiplayer functionality in your games using sessions, a unified system that manages player connections and interactions across multiple Unity Gaming Services.

A session represents a group of connected players and manages their interactions through various connection types, including client-hosted solutions like Relay and Distributed Authority, or dedicated game servers through Multiplay Hosting. Sessions work with either the Netcode for GameObjects or the Netcode for Entities networking libraries. Sessions provide an abstraction layer for managing multiplayer game states, handling everything from initial player connections to ongoing gameplay interactions, and automatically handle complex multiplayer operations such as host election, player joining and leaving, and network connection establishment.

Beginning with Unity 2022 LTS, developers can use session operations to create their multiplayer experiences. Instead of having to manually coordinate between the SDKs for Lobby, Matchmaker, Multiplay Hosting, and Relay, the Multiplayer Services (MPS) SDK merges the functionality previously spread across these individual services' SDKs into a unified API centered around sessions. For example, when creating a session, the MPS SDK automatically handles lobby creation, relay allocation, and manages the Netcode connection. Similarly, while joining a session, the MPS SDK manages lobby joining, and Netcode connection establishment. This combined approach maintains all the capabilities of the underlying Unity Gaming Services (UGS) Multiplayer services while reducing implementation complexity.

---

## Integrating Multiplayer Sessions in your game

**URL:** https://docs.unity.com/ugs/en-us/manual/mps-sdk/manual/working-with-mps

**Contents:**
- Integrating Multiplayer Sessions in your game#

Manage multiplayer sessions with the Multiplayer Services SDK.

The Multiplayer Services SDK provides a unified, session-based API for implementing multiplayer functionality. This section describes how to programmatically interact with your multiplayer game.

---

## Use Relay with Netcode for GameObjects (Relay SDK)

**URL:** https://docs.unity.com/ugs/en-us/manual/relay/manual/relay-and-ngo-standalone

**Contents:**
- Use Relay with Netcode for GameObjects (Relay SDK)#
- Installation and configuration#
- Set up the NetworkManager#
- Host player#
- Joining player#
- Note about Unity Web platform support#

Important: If you're using the unified Multiplayer Services package, refer to Use Relay with Netcode for GameObjects.

The following instructions demonstrate how to use Relay with Netcode for GameObjects using the standalone Relay SDK.

Note: For most users, the unified Multiplayer Services package replaces the Relay standalone package, which is deprecated in Unity 6. Consider migrating to the unified package to facilitate a smooth transition. Visit the migration guide for a step-by-step transition process.

These steps describe a deprecated process. Refer to Use Relay with Netcode for GameObjects for the latest instructions.

You must first configure your Unity project for Unity before using Relay with Netcode for GameObjects. Refer to Get started with Relay to learn how to link your project in the project settings.

These steps describe a deprecated process. Refer to Use Relay with Netcode for GameObjects for the latest instructions.

After installing the packages, you can now set up the NetworkManager:

These steps describe a deprecated process. Refer to Use Relay with Netcode for GameObjects for the latest instructions.

The StartHostWithRelay function shows how to create a Relay allocation, and request a join code. This function requires the maximum number of connections the allocation is expecting and the host player connection type.

The connection type must be one of the following options:

Refer to DTLS to learn more about DTLS encryption, and to Web Platform Support to learn about using wss.

This code should be adapted to your needs to use a different authentication mechanism, a different error handling, or to use a different connection type. Similarly, you can call StartServer instead of StartHost to start a server instead of a host.

Note: If this code succeeds in calling the NetworkManager.Singleton.StartHost(), it returns the join code retrieved from the Relay allocation.

These steps describe a deprecated process. Refer to Use Relay with Netcode for GameObjects for the latest instructions.

When your game client functions as a joining player, the relay join code, retrieved in the previous step when the host created the allocation, must be passed to find the allocation. The following code samples show how to join an allocation with a join code and configure the connection type.

The connection type must be one of the following options:

Refer to DTLS to learn more about DTLS encryption, and to Web Platform Support to learn about using wss.

These steps describe a deprecated process. Refer to Use Relay with Netcode for GameObjects for the latest instructions.

To use Relay with Netcode for GameObjects in Unity Web platform supported game, upgrade the Unity Transport Package to 2.0.0 or later and configure the Unity transport component to use Web Sockets.

Using the above code snippets, pass wss as connectionType and use the following to SetRelayServerData:

**Examples:**

Example 1 (unknown):
```unknown
com.unity.services.relay
```

Example 2 (unknown):
```unknown
com.unity.netcode.gameobjects
```

Example 3 (unknown):
```unknown
NetworkManager
```

Example 4 (unknown):
```unknown
MonoBehaviour
```

---

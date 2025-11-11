---
name: unity
description: Unity game engine development. Use for Unity projects, C# scripting, scene setup, GameObject systems, 2D/3D development, physics, animation, UI, shaders, asset management, or any Unity-specific questions.
---

# Unity Skill

Comprehensive assistance with unity development, generated from official documentation.

## When to Use This Skill

This skill should be triggered when:
- Working with unity
- Asking about unity features or APIs
- Implementing unity solutions
- Debugging unity code
- Learning unity best practices

## Quick Reference

### Common Patterns

**Pattern 1:** MultiplayerMultiplay HostingConceptsBuildsContainer buildsEnglish日本語 (日本)한국어(대한민국)中文（中国）English日本語 (日本)한국어(대한민국)中文（中国）Container builds#A container is a way of packing software and all its dependencies together in a single unit. With Multiplay Hosting container builds, you can package your game servers and their dependencies, build a container, and push it to the Multiplay Hosting container registry.Warning: Container builds require technical knowledge around Docker and Dockerfiles (Docker Docs).You can create a container build through the Unity Dashboard by selecting Container image when you create a build. Refer to Create a build with a container image.Multiplay Hosting uses Docker container tags in the format <ImageName>:<ImageTag>. Multiplay Hosting generates the correct name and tag for your build, and displays it to you in the Unity Dashboard when you create or update a container build.docker tag <ImageName>:<ImageTag> registry.multiplay.com/4818982f-bb28-4c39-9d44-628204b6780e/1ae4de65-849d-4ff1-86fc-1de3bdfbd891/12788:v1Note: Multiplay Hosting only generates the ImageName and ImageTag for you when you create a build through the Unity Dashboard. If you use the CLI tool, you must create the ImageName and ImageTag.Container build requirements#When you create a Multiplay Hosting build, you have several options for uploading your build file. One option is to use Docker and the Multiplay Hosting container registry to upload a containerized version of your build. However, your build container must meet specific requirements (in addition to the general integration requirements) before you can use a containerized build.Create a container built with the base image#The linux-build-image template container takes care of most of the additional requirements of using a container build. You can use the container by adding FROM unitymultiplay/linux-base-image:<tag> to the top of your Dockerfile, as the following code demonstrates. For more information, refer to Dockerfile reference (Docker documentation).FROM unitymultiplay/linux-base-image:<tag> # copy game files here # for example: WORKDIR /game COPY --chown=mpukgame . . # set your game binary as the entrypoint ENTRYPOINT [ "./gamebinary" ]In the preceding example:Replace <tag> with the version of the base image to use. You can check the linux-base-image tags for a list of available tags.Set WORKDIR to the current working directory of the container process. Any actions to run processes or copy files are relative to the working directory.Set ENTRYPOINT to the executable that runs when the container starts. This entrypoint should be your game server binary.Warning: Launch parameters specified in the build configuration can overwrite CMD defined in the Dockerfile.For a simple example, refer to this Simple Game Server Dockerfile (Unity Multiplay examples on GitHub).Create a container build from scratch#If you prefer to create your Dockerfile from scratch, it’s your responsibility to ensure your container meets the following requirements.Warning: Use the base image unless you have a specific or advanced uses in which you must create the container from scratch.RequirementDescriptionUSER mpukgameYour container must use mpukgame as the USER. This user must have a group identifier (GID) of 2000 and a user identifier (UID) of 2000.ENTRYPOINTYour container must have an ENTRYPOINT.The following code block is a simple Dockerfile that meets these requirements:# ======================================================== # # Unity base image stuff # # ======================================================== # FROM ubuntu:20.04 AS mpuk RUN addgroup --gid 2000 mpukgame && \ useradd -g 2000 -u 2000 -ms /bin/sh mpukgame && \ mkdir /game && \ chown mpukgame:mpukgame /game && \ apt update && \ apt upgrade && \ apt install -y ca-certificates USER mpukgame # ======================================================== # # Custom game stuff # # ======================================================== # FROM mpuk AS game # copy game files here # for example: WORKDIR /game COPY --chown=mpukgame . . # set your game binary as the entrypoint ENTRYPOINT [ "./gamebinary" ]Create a container build using server builds uploaded to Steam#To upload a container build for use with game server builds uploaded to Steam, it is possible to use SteamCMD to build your container image.The following code block is an example Dockerfile for the SteamCMD process:# ======================================================== # # SteamCMD stuff # # ======================================================== # FROM steamcmd/steamcmd:ubuntu-20 AS steamcmd WORKDIR /game COPY update.txt /data/update.txt RUN steamcmd +runscript /data/update.txt # if you want to avoid having the update.txt file, use: # RUN steamcmd +force_install_dir /game/<game_name> \ # +login anonymous +app_update <steam_appid> validate +quit # ======================================================== # # Unity base image stuff # # ======================================================== # FROM ubuntu:20.04 AS mpuk RUN addgroup --gid 2000 mpukgame && \ useradd -g 2000 -u 2000 -ms /bin/sh mpukgame && \ mkdir /game && \ chown mpukgame:mpukgame /game && \ apt update && \ apt upgrade && \ apt install -y ca-certificates lib32z1 USER mpukgame # ======================================================== # # Custom game stuff # # ======================================================== # WORKDIR /game COPY --from=steamcmd --chown=mpukgame:mpukgame /game/<game_name> /game/<game_name> # set your game binary as the entrypoint ENTRYPOINT [ "./game_binary" ]Pulling a container build#For security reasons, pulling content from the registry is not permitted. If you require an offline copy of your build, make sure to also push the image to a registry you control for later use.Server.json file#If you use the base container image, the server.json file is already mounted into your container. You can find the server.json file location by checking the container's home environment variable.For Linux containers, it's in the $HOME environment variable.For Windows containers, it's in the $HOMEPATH environment variable.If you create a container build from scratch, create your own server.json file and include it in the container image. Refer to Server.json for the format of this file, and Configuration variables for an explanation of server.json's variables.Container buildsContainer build requirementsCreate a container built with the base imageCreate a container build from scratchCreate a container build using server builds uploaded to SteamPulling a container buildServer.json file

```
<ImageName>:<ImageTag>
```

**Pattern 2:** FROM unitymultiplay/linux-base-image:<tag> # copy game files here # for example: WORKDIR /game COPY --chown=mpukgame . . # set your game binary as the entrypoint ENTRYPOINT [ "./gamebinary" ]

```
FROM unitymultiplay/linux-base-image:<tag>

# copy game files here
# for example:
WORKDIR /game
COPY --chown=mpukgame . .

# set your game binary as the entrypoint
ENTRYPOINT [ "./gamebinary" ]
```

**Pattern 3:** In the preceding example:

```
<tag>
```

**Pattern 4:** Vivox Voice & TextVivox Unreal SDKDeveloper guideUnity Authentication Service in UnrealEnglish日本語 (日本)한국어(대한민국)中文（中国）English日本語 (日本)한국어(대한민국)中文（中国）Unity Authentication Service in Unreal#You can use the Unity Authentication Service (UAS) in your Unreal Vivox application.Requirements#Unreal Engine version 5.1.1 or laterA Unity accountThe Unity Gaming Services SDK for Unreal EngineIntegration steps#Ensure you have a compatible version of the Unreal Engine for the Unity Gaming Services for Unreal Plugin. Refer to the SDK page in the Unreal Marketplace for more information.Go to the Unity Gaming Services SDK for Unreal Engine Unreal Engine Marketplace page in your Epic Games Launcher, or select Open in Launcher from the linked Marketplace page.Load the plug-in into your project on the Epic Games Launcher Marketplace page.Go into your Project Settings and locate the Unity Authentication plug-in settings.Place the Project ID from your Project page on the Unity Dashboard into the Project ID field. The setting will likely autoformat. Place the target environment name in the Environment Name field.Note: Find your Project ID and Environment on the Unity Dashboard, by selecting the Projects tab and then the project you’re working on.Authenticate with the Unity Authentication Service#Use the following instructions to set up the Unity Authentication Subsystem to sign-in to Vivox:Add Authentication to the PublicDependencyModuleName in your project’s build.cs fileOnce you install the plug-in and assign settings in your project, you can set up and une the AuthenticationSubsystem in C++.Get the UGameInstance from the GameWorld and assign the AuthenticationSubsystem to a variable from the GameInstance.UWorld* GameWorld = GetWorld(); UGameInstance* GameInstance = GameWorld->GetGameInstance(); UAuthenticationSubsystem* AuthenticationSubsystem = GameInstance->GetSubsystem<UAuthenticationSubsystem>();Set up an AuthenticationResponse UFunction, named OnAuthentication in the following example, to track when the Authentication has finished successfully. Then, pass it into SignInAnonymously, along with any FAuthenticationSignInOptions you need.Unity::Services::Core::THandler<FAuthenticationResponse> authenticationResponse; FName OnAuthenticationFName("OnAuthentication"); authenticationResponse.BindUFunction(this, OnAuthenticationFName); FAuthenticationSignInOptions signInOptions; AuthenticationSubsystem->SignInAnonymously(signInOptions, authenticationResponse);After Authentication has successfully authenticated to the UGS Service, replace the LoginToken in any LoginSession.BeginLogin call with the AuthenticationSubsystem->GetAccessToken(). Below is an example:LoginSession.BeginLogin(WebService, AuthenticationSubsystem->GetAccessToken(), SubscriptionMode::Accept, TSet<AccountId>(), TSet<AccountId>(), TSet<AccountId>(), OnBeginLoginCompleteCallback);Note: When using Unity Authentication Access Tokens, the local AccountID’s Account Name should be the same as the Authentication PlayerID, as shown here://This is being done in OnAuthentication, using the returned FAuthenticationResponse as response AccountId m_Account = AccountId(m_tokenIssuer, response.UserId, m_domain)You can also use Authentication Tokens in place of ChannelSession connect tokens, as shown in the following example:ChannelSession.BeginConnect(true, false, true, AuthenticationSubsystem->GetAccessToken(), OnBeginConnectCompleteCallback);Unity Authentication Service in UnrealRequirementsIntegration stepsAuthenticate with the Unity Authentication Service

```
AuthenticationSubsystem
```

**Pattern 5:** After Authentication has successfully authenticated to the UGS Service, replace the LoginToken in any LoginSession.BeginLogin call with the AuthenticationSubsystem->GetAccessToken(). Below is an example:

```
LoginSession.BeginLogin
```

**Pattern 6:** You can also use Authentication Tokens in place of ChannelSession connect tokens, as shown in the following example:

```
ChannelSession
```

**Pattern 7:** MultiplayerUGS for the Unreal EngineMatchmaker SDK for the Unreal EngineIntegrate using C++English日本語 (日本)한국어(대한민국)中文（中国）English日本語 (日本)한국어(대한민국)中文（中国）Integrate using C++#The following section shows how to integrate with the Matchmaker SDK using the Unreal Engine Subsystems.The Unity Gaming Services SDK offers two matchmaker interfaces:Matchmaker Client SubsystemMatchmaker Server SubsystemAdd the Matchmaker SDK as a dependency#Before continuing, add the MatchmakerSDK as a public dependency of your module, then include the plugin header files in your classes as shown below.Add MatchmakerServer and MatchmakerClient to your module's dependencies to your Unreal project build file (YourProjectName.Build.cs):PublicDependencyModuleNames.AddRange(new string[] { "Core", "CoreUObject", "Engine", "InputCore" }); PublicDependencyModuleNames.AddRange(new string[] { "MatchmakerClient", "MatchmakerServer" }); PublicDependencyModuleNames.AddRange(new string[] { "Json", "JsonUtilities" });Include the plugin header files you want to access in your classes:#include "MatchmakerClientSubsystem.h" #include "MatchmakerServerSubsystem.h"Matchmaker Client Subsystem#Note: Before accessing the Matchmaking Client Subsystem, ensure that you set up Authentication.The Matchmaker Client Subsystem controls the client portion of matchmaking and finding matches. This includes creating, deleting, and polling matchmaking tickets.You can access the Matchmaker Client Subsystem by getting a reference to the UMatchmakerClientSubsystem subsystem. UMatchmakerClientSubsystem is a UGameInstanceSubsystem you can retrieve from UGameInstance.UWorld* GameWorld = GetWorld(); UGameInstance* GameInstance = GameWorld->GetGameInstance(); UMatchmakerClientSubsystem* MatchmakerClientSubsystem = GameInstance->GetSubsystem<UMatchmakerClientSubsystem>();CreateTicket#Use the CreateTicket() method to start the matchmaking process by creating a matchmaking ticket for the clients.Calling this method adds all valid players in the Players array to the matchmaking queue as a group and joins a match as part of the same team.There are multiple parameters you can pass in, but the only required parameter is the Players list (with a minimum of one player using a valid ID). Other optional parameters include Queue Name, Qos Results, and custom data for players.// Create Players array TArray<FMatchmakerPlayer> Players; // Create player definition FMatchmakerPlayer SamplePlayer; SamplePlayer.Id = TEXT("SamplePlayer"); // Adding custom player data TSharedPtr<FJsonObject> CustomData = MakeShared<FJsonObject>(); CustomData->SetNumberField("Skill", 100); CustomData->SetStringField("PreferredMap", "Dune"); SamplePlayer.CustomData = CustomData; Players.Add(SamplePlayer); // Setup Ticket Options FCreateTicketOptions Options; Options.QueueName = TEXT("default-queue"); MatchmakerClientSubsystem->CreateTicket(Players, options, Unity::Services::Core::THandler<FCreateTicketResponse>::CreateLambda([this](FCreateTicketResponse Response) { // Your response logic here (response includes Ticket Id) }));You can handle the response from the SDK using THandler which accepts a FCreateTicketResponse.GetTicketStatus#Poll for a client's ticket status against the matchmaker service by using the GetTicketStatus() method.You should poll continuously in a loop until you retrieve a response indicating either a success or failure of the matchmaking process.To achieve this, use FTimerDelegate to start a timer as in the following example:// Create the Timer Delegate and set the function to call FTimerDelegate PollTicketDelegate = FTimerDelegate::CreateUObject(this, &UMyCustomClass::PollTicket, Response.TicketId); // Start the Timer that runs every 5 seconds, cache PollTicketTimerHandler to use for stopping the timer later GEngine->GameViewport->GetWorld()->GetTimerManager().SetTimer(PollTicketTimerHandle, PollTicketDelegate, 5, true);Then your poll match function would look similar to the following example:void UMyCustomClass::PollTicket(FGuid TicketId) { MatchmakerClientSubsystem->GetTicketStatus(TicketId, THandler<FGetTicketStatusResponse>::CreateLambda([this, TicketId](FGetTicketStatusResponse Response) { if (Response.bWasSuccessful) { FString TicketIdStr = *TicketId.ToString(EGuidFormats::DigitsWithHyphens).ToLower(); switch (Response.Status) { case StatusEnum::Failed: UE_LOG(LogTemp, Log, TEXT("polling for ticket has failed, response was: %s"), *Response.ErrorMessage); break; case StatusEnum::Timeout: UE_LOG(LogTemp, Log, TEXT("polling for ticket has timed out, response was: %s"), *Response.ErrorMessage); break; case StatusEnum::InProgress: UE_LOG(LogTemp, Log, TEXT("polling for ticket %s is still in progress."), *TicketId.ToString(EGuidFormats::DigitsWithHyphens).ToLower()); return; case StatusEnum::Found: UE_LOG(LogTemp, Log, TEXT("polling for ticket has completed, match found with ip: %s"), *Response.Ip); break; default: UE_LOG(LogTemp, Log, TEXT("unknown status received for ticket: %s), *TicketId.ToString(EGuidFormats::DigitsWithHyphens).ToLower()); break; } // If we made it this far, we got a status other than InProgress so we can end the polling timer. GEngine->GameViewport->GetWorld()->GetTimerManager().ClearTimer(PollTicketTimerHandle); // Delete ticket here } else { UE_LOG(LogTemp, Log, TEXT("Failed to poll Ticket status with ticket ID: %s"), *TicketId.ToString(EGuidFormats::DigitsWithHyphens).ToLower()); } })); }You can handle the response from the SDK using a THandler which accepts a FGetTicketStatusResponse.DeleteTicket#Use the DeleteTicket() method to delete a matchmaking ticket and cancel matchmaking. This is typically done after a match is found (successfully or not) or when a client no longer wants to be considered for the matchmaking process.You can handle the response from the SDK in a response handler which accepts a FDeleteTicketResponse.MatchmakerClientSubsystem->DeleteTicket(TicketId, Unity::Services::Core::THandler<FDeleteTicketResponse>::CreateLambda([](FDeleteTicketResponse Response) { // Your response logic here }));You can handle the response from the SDK using a THandler which accepts a FDeleteTicketResponse.Matchmaker Server Subsystem#The Matchmaker Server Subsystem controls the server portion of matchmaking. This includes creating, approving, deleting, and updating backfill tickets.Before you can use the UMatchmakerServerSubsystem, you must retrieve it as shown in the following code snippet:UWorld* GameWorld = GetWorld(); UGameInstance* GameInstance = GameWorld->GetGameInstance(); UMatchmakerServerSubsystem* MatchmakerServerSubsystem = GameInstance->GetSubsystem<UMatchmakerServerSubsystem>();Note: Matchmaker is at the time of writing expected to be used solely with Multiplay Servers and as such it is important to note that the following server functions are expected to be used with some of the Multiplay SDK functionality.See Allocation Payload for more details (access with GetPayloadAllocation() with Multiplay’s Subsystem). This is used to initially fill MatchProperties.CreateBackfillTicket#You need to create a new backfill ticket when a player or players leave a full match, and the server needs to fill in the empty slots. To create a new backfill ticket for the server, use the CreateBackfillTicket() method.The following code snippet shows how to create a new backfill ticket:// Setup Backfill Ticket Options FCreateBackfillTicketOptions CreateBackfillTicketOptions; CreateBackfillTicketOptions.Connection = TEXT("35.245.90.171:9000"); CreateBackfillTicketOptions.QueueName = TEXT("default-queue"); // Create Match Properties FMatchProperties MatchProperties; FGuid BackfillTicketId = FGuid::NewGuid(); MatchProperties.BackfillTicketId = BackfillTicket.Id; MatchProperties.Region = TEXT("aaaaaaaa-1111-bbbb-2222-cccccccccccc"); // Create Player FMatchmakerPlayer Player; Player.Id = FGuid::NewGuid().ToString(); FQosResult QosResult; QosResult.Latency = 20; QosResult.PacketLoss = 5; QosResult.Region = TEXT("aaaaaaaa-1111-bbbb-2222-cccccccccccc"); Player.QoSResults.Add(QosResult); MatchProperties.Players.Add(Player); // Create Team FMatchmakerTeam Team; Team.TeamName = TEXT("Red"); FGuid TeamId = FGuid::NewGuid(); Team.TeamId = TeamId.ToString(EGuidFormats::DigitsWithHyphens); Team.PlayerIds.Add(Player.Id); MatchProperties.Teams.Add(Team); CreateBackfillTicketOptions.Properties.MatchProperties = MatchProperties; MatchmakerServerSubsystem->CreateBackfillTicket(CreateBackfillTicketOptions, Unity::Services::Core::THandler<FCreateBackfillTicketResponse>::CreateLambda([this, Player, Team](FCreateBackfillTicketResponse CreateResponse) { // Your response logic here }));You can handle the response from the SDK using a THandler which accepts a FCreateBackfillTicketResponse.ApproveBackfillTicket#To approve a backfill ticket after creation, use the ApproveBackfillTicket() method. Approving a backfill ticket allows new players into the server.It's recommended to approve backfill tickets no faster than once a second. The Unity Matchmaker deletes tickets if they haven’t been approved in 20 seconds.The following code snippet shows how to approve a backfill ticket:MatchmakerServerSubsystem->ApproveBackfillTicket(BackfillTicketId, Unity::Services::Core::THandler<FApproveBackfillTicketResponse>::CreateLambda([this](FApproveBackfillTicketResponse ApproveResponse) { // Your response logic here }));You can handle the response from the SDK using a THandler which accepts a FApproveBackfillTicketResponse.DeleteBackfillTicket#Delete a backfill ticket when a match becomes full, and you no longer need the server to accept new players. You should also do this after a match concludes and you no longer want the server to receive new players. To stop backfilling on a server, use the DeleteBackfillTicket() method.The following code snippet shows how to delete a backfill ticket:MatchmakerServerSubsystem->DeleteBackfillTicket(BackfillTicketId, Unity::Services::Core::THandler<FDeleteBackfillTicketResponse>::CreateLambda([this](FDeleteBackfillTicketResponse DeleteResponse) { // Handle response here }));You can handle the response from the SDK using a THandler which accepts a FDeleteBackfillTicketResponse.UpdateBackfillTicket#To update a server's current backfill ticket, use the UpdateBackfillTicket() method.Update a backfill ticket anytime a player leaves the server or anytime a player joins the server from outside of matchmaking logic. This can include (but isn't limited to) party invites, direct connections, and friend invites.You should update backfill tickets no more than once every three seconds or after an approved backfill ticket sees a change in the backfill ticket to ensure that a matchmaking cycle has passed. Updating a backfill ticket too often can cause players to never backfill into the match. See Matchmaking Logic Sample to learn more.// From Approve FBackfillTicket BackfillTicket; FGuid BackfillTicketId = FGuid::Parse(ApproveResponse.BackfillTicket.Id, BackfillTicketId); FBackfillTicket BackfillTicket = ApproveResponse.BackfillTicket; TSharedPtr<FJsonObject> CustomData = MakeShared<FJsonObject>(); CustomData->SetNumberField("Skill", 100); CustomData->SetStringField("PreferredMap", "Dune"); NewPlayer.CustomData = CustomData; BackfillTicket.Properties.Players.Add(NewPlayer); BackfillTicket.Properties.[0].PlayerIds.Add(NewPlayer.Id); // Remove a Player from the backfill ticket FString PlayerIdToRemove = TEXT("SamplePlayer"); for (FMatchmakerPlayer Player : BackfillTicket.Properties.Players) { if (Player.Id == PlayerIdToRemove) { BackfillTicket.Properties.Players.Remove(Player); break; } } if (BackfillTicket.Properties.Teams[0].PlayerIds.Contains(PlayerIdToRemove)) { BackfillTicket.Properties.Teams[0].PlayerIds.Remove(PlayerIdToRemove); } // Call to update backfill ticket MatchmakerServerSubsystem->UpdateBackfillTicket(BackfillTicketId, BackfillTicket, Unity::Services::Core::THandler<FUpdateBackfillTicketResponse>::CreateLambda([this, BackfillTicketId](FUpdateBackfillTicketResponse UpdateResponse) { // Handle Response here }));Integrate using C++Add the Matchmaker SDK as a dependencyMatchmaker Client SubsystemMatchmaker Server Subsystem

```
MatchmakerSDK
```

**Pattern 8:** To achieve this, use FTimerDelegate to start a timer as in the following example:

```
FTimerDelegate
```

### Example Code Patterns

**Example 1** (cpp):
```cpp
#include <ITTSMessage.h>
```

**Example 2** (javascript):
```javascript
virtual const ITTSVoice & Voice() const =0
```

## Reference Files

This skill includes comprehensive documentation in `references/`:

- **3d.md** - 3D documentation
- **assets.md** - Assets documentation
- **audio.md** - Audio documentation
- **getting_started.md** - Getting Started documentation
- **networking.md** - Networking documentation
- **other.md** - Other documentation
- **scripting.md** - Scripting documentation
- **ui.md** - Ui documentation

Use `view` to read specific reference files when detailed information is needed.

## Working with This Skill

### For Beginners
Start with the getting_started or tutorials reference files for foundational concepts.

### For Specific Features
Use the appropriate category reference file (api, guides, etc.) for detailed information.

### For Code Examples
The quick reference section above contains common patterns extracted from the official docs.

## Resources

### references/
Organized documentation extracted from official sources. These files contain:
- Detailed explanations
- Code examples with language annotations
- Links to original documentation
- Table of contents for quick navigation

### scripts/
Add helper scripts here for common automation tasks.

### assets/
Add templates, boilerplate, or example projects here.

## Notes

- This skill was automatically generated from official documentation
- Reference files preserve the structure and examples from source docs
- Code examples include language detection for better syntax highlighting
- Quick reference patterns are extracted from common usage examples in the docs

## Updating

To refresh this skill with updated documentation:
1. Re-run the scraper with the same configuration
2. The skill will be rebuilt with the latest information

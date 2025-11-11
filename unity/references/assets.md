# Unity - Assets

**Pages:** 5

---

## Asset management

**URL:** https://docs.unity.com/ugs/en-us/manual/overview/manual/asset-management

**Contents:**
- Asset management#
- Asset Store credits#
  - Creating credit codes#
- Order requests#

The Asset Management page lets you control who has access to the Assets that are tied to the Organization’s account. To view and manage your Organization’s Assets from the Unity ID portal:

This page displays a list of Unity Assets that the Organization's Owner has purchased, or Assets that were purchased through the Order Requests feature. To assign access to an Asset, click the value in the Assigned to/Shared with column, locate the member you want to provide with access, then click Assign. You can follow the same process to revoke access.

Asset Store credits allow Organizations to add credits to their account to be used as a payment method on the Asset Store. This feature also allows the Organization to create codes that can be distributed and used as a payment method. To view your Organization’s Asset Store credits from the Unity ID portal:

This page displays your Organization's credit balance and purchase history. Contact your Unity Business Development or Sales representative to purchase credits for your Organization.

When your Organization has a positive credit balance, you can convert your credit into credit codes for distribution and use as a payment method, by clicking Convert it Now.

A module will prompt you to enter the value and quantity of the codes you want to create. Click Generate codes to create the codes.

Select the Credit Codes tab to view a list of existing codes and information about their use within the Organization.

Note: Credit codes are uniquely generated, and appear obfuscated by default for security reasons. Click the eye icon () to reveal the full serial.

Click Order Requests from the left navigation menu, then click Enable to turn on order requests for your Organization. Order requests allow Organization members to submit purchase requests for Assets they want to download.

Note: Contact your Unity Business Development or Sales representative to enable Order Requests.

---

## Create UGS Asset Store packages

**URL:** https://docs.unity.com/ugs/en-us/manual/overview/manual/create-asset-store-packages

**Contents:**
- Create UGS Asset Store packages#
- Create UGS assets#
- Install Asset Store Tools#
- Upload package to the Asset Store#
  - Set your dependencies#
  - Choose your export type#
- Service-specific intructions#
  - Cloud Code modules#
- Deployment window#

This guide explains how to create assets and packages using Asset Store Tools and publish them to the Asset Store. Other users can then download the asset and deploy it to their own project using the Deployment package.

Create the UGS assets you wish to include in the Asset Store package. Refer to the corresponding documentation for the assets you can create. The following services are supported:

Install Asset Store Tools to upload your package from the Unity Editor. Refer to Publishing to the Asset Store to learn more.

Refer to Creating a new package draft to learn how to upload packages to the Asset Store.

You can upload your package directly from the assets folder, or from a pre-exported .unitypackage file.

To upload from the Assets folder:

To upload from a .unitypackage:

The recommended best practice is for Cloud Code modules to live outside the Assets folder, or in a hidden folder (a folder name ending with ~, such as Module~). To successfully export to the Asset Store, you must choose a hidden path within the Assets folder. By default, hidden paths are not included in because .meta files are not generated for the files inside a hidden path.

To generate the .meta files and include the contents of the hidden folder to your Asset Store package:

Now your package is ready to be exported with an included Cloud Code module.

The Deployment window is part of the Deployment package and it offers a uniform interface to deploy assets for UGS. Refer to Deployment package to learn how to use it.

The UGS use cases samples project contains UGS assets that you can deploy using the Deployment window. For an example, see A/B Testing.

**Examples:**

Example 1 (unknown):
```unknown
Include Package Manifest
```

Example 2 (unknown):
```unknown
.unitypackage
```

Example 3 (unknown):
```unknown
Upload type
```

Example 4 (unknown):
```unknown
From Assets Folder
```

---

## Leaderboard assets

**URL:** https://docs.unity.com/ugs/en-us/manual/leaderboards/manual/leaderboards-assets

**Contents:**
- Leaderboard assets#
- Prerequisites#
- Create leaderboard assets#
- File format#
  - Example#
- Deploy files#

Use leaderboard configuration assets to manage leaderboard configurations in the Unity Editor. You can deploy these configurations to the Unity Dashboard using the Deployment package.

To use leaderboard assets, you must install the Deployment package (com.unity.services.deployment).

To create a leaderboard asset:

Leaderboard configuration files must respect this JSON schema. The default leaderboard configuration file contains the $schema field to enable IntelliSense in IDEs.

The following example contains these keys:

When you've created your leaderboard configuration files, you can deploy them to your chosen environment.

You can now edit your leaderboard configuration in the Unity Dashboard.

For more information on working with the Deployment window, refer to Deployment package.

**Examples:**

Example 1 (unknown):
```unknown
com.unity.services.deployment
```

Example 2 (unknown):
```unknown
ResetConfig
```

Example 3 (unknown):
```unknown
TieringConfig
```

Example 4 (unknown):
```unknown
{
  "$schema": "https://ugs-config-schemas.unity3d.com/v1/leaderboards.schema.json",
  "Name": "leaderboard_config",
  "SortOrder": "asc",
  "UpdateType": "keepBest",
  "ResetConfig": {
    "Start": "2023-10-21T00:00:00-04:00",
    "Schedule": "0 12 1 * *"
  },
  "TieringConfig": {
    "Strategy": "score",
    "Tiers": [
      {
        "Id": "Gold",
        "Cutoff": 200.0
      },
      {
        "Id": "Silver",
        "Cutoff": 100.0
      },
      {
        "Id": "Bronze"
      }
    ]
  }
}
```

---

## Resource types

**URL:** https://docs.unity.com/ugs/en-us/manual/economy/manual/resources-landing

**Contents:**
- Resource types#
- Additional resources#

Resources are the in-game units that define an economy system. This section describes the types of resources in Economy, and how to add them to your project.

---

## Deploying resources with Unity Editor

**URL:** https://docs.unity.com/ugs/en-us/manual/economy/manual/write-configuration/unity-editor

**Contents:**
- Deploying resources with Unity Editor#
- Prerequisites#
  - Link project#
  - Install required packages#
- Authoring within Unity Editor#
  - Create a resource#
  - Edit a resource#
  - Resource file content#
  - Deploy a resource#
- Deployment window#

The Economy Authoring module (installed with the Economy package) allows you to optionally author and modify resources directly within the Unity Editor. You can then upload resources from the Unity Editor to the Dashboard by using the Deployment package.

Economy resources existing in the Unity Editor allow users to treat their source control as the single source of truth (instead of the version in the cloud), simplifying actions such as rollbacks, bisection, and other common operations.

To use Economy in the Unity Editor, you must first install the com.unity.services.economy SDK and link your Unity Gaming Services project to the Unity Editor.

Link your Unity Gaming Services project with the Unity Editor. You can find your UGS project ID in the Unity Dashboard.

In Unity Editor, select Edit > Project Settings > Services.

If your project doesn't have a Unity project ID:

If you have an existing Unity project ID:

Your Unity Project ID appears, and the project is now linked to Unity services. You can also access your project ID in a Unity Editor script using UnityEditor.CloudProjectSettings.projectId.

To create Economy resources within the Editor, you must install the following packages:

See Unity - Manual: Package Manager window to familiarize yourself with the Unity Package Manager interface.

To install these packages and add them to your list of available packages:

The Economy Authoring module allows you to create, edit, and deploy Economy resources directly within the Unity Editor.

Follow these steps to create an Economy resource using the Economy Authoring module:

The new resource is now visible in the Project window, and in the Deployment window, accessible by selecting Services > Deployment.

There is currently one method to edit an existing Economy resource:

The deployment window finds resources and assigns their type according to their file extension.

Valid file extensions are .ecc (Currency), .eci (Inventory Item), .ecv (Virtual Purchase) and .ecr (Real Money Purchase).

An Economy resource file contains json describing the resource to be deployed.

The json content must match with the extension (type of the resource).

Example for my_currency.ecc:

Example for my_inventory_item.eci:

Example for my_virtual_purchase.ecv:

Example for my_real_money_purchase.ecr:

You can find out which fields should be in your json by looking at the schema for each resource type here.

Some fields may be omitted, such as id and customData. In the event where id is omitted, the resource will use name as id, therefore name will have to follow the naming rules of id as defined in the schema.

You can deploy an Economy resource through the Deployment window. See the Deployment package manual for more information.

Some Economy resources have dependencies on other resources, such as Virtual Purchase resources having dependencies on Currency or Inventory Item resources.

When deploying, the Currency and Inventory Item resources are always deployed first.

When deploying Virtual Purchase or Real Money Purchase resources, make sure that the resources used for rewards and costs are also getting deployed at the same time, or already exist.

The Deployment window is a core feature of the Deployment package. It allows all services to have a single cohesive interface for deployment needs, and allows you to upload cloud assets to their respective cloud services.

See the Deployment package manual for more information.

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
com.unity.services.economy
```

Example 4 (unknown):
```unknown
my_currency.ecc
```

---

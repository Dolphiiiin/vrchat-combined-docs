# sdk 統合ドキュメント

以下は自動収集された全ドキュメントのコンテンツです。各セクションの始めにメタデータが記載されています。



---

## ドキュメント: build-pipeline-callbacks-and-interfaces.md

```metadata
階層: /sdk/build-pipeline-callbacks-and-interfaces.md
ディレクトリ: sdk
ファイル名: build-pipeline-callbacks-and-interfaces.md
拡張子: .md
サイズ: 2.24 KB
最終更新: 2025-06-05T03:07:52.757Z
```

---
title: "Build Pipeline Callbacks and Interfaces"
slug: "build-pipeline-callbacks-and-interfaces"
hidden: false
createdAt: "2023-04-11T21:01:07.855Z"
updatedAt: "2023-04-11T21:04:48.137Z"
---
VRChat SDK contains multiple interfaces that can be used via Editor Scripts to enhance the World and Avatar build process.

## For Scene Components

The interfaces outlined below can be used in combination with `MonoBehaviours` and as such - be placed on scene objects directly, which can be useful in a situation where you need to hold some specific scene references to perform your modifications.

### IEditorOnly

`VRC.SDKBase.IEditorOnly`

The interface has no members to implement.

You can use `IEditorOnly` to mark a script Editor-only for the SDK Validation. This will make it so the SDK ignores it when scanning your World or Avatar for incompatible scripts.

### IPreprocessCallbackBehaviour

`VRC.SDKBase.IPreprocessCallbackBehaviour`

Members to implement

```csharp
public void OnPreprocess()
{
}

public int PreprocessOrder { get; }
```

This interface allows you to execute custom code when the build process is about to begin. This can be useful if you need to perform modifications before content gets built and uploaded to VRChat.

> 🚧 Note that this does not automatically bypass the SDK validation. You should also use `IEditorOnly` if your scripts exist directly on the avatar you're uploading

## For Project-Wide Scripts

These interfaces are suited for anything that does not rely on particular scene objects and performs bulk modifications to the scene/avatar before it gets uploaded to VRChat.

### IVRCSDKBuildRequestedCallback

`VRC.SDKBase.Editor.BuildPipeline.IVRCSDKBuildRequestedCallback`

Members to implement

```csharp
    public int callbackOrder => 0;

    public bool OnBuildRequested(VRCSDKRequestedBuildType requestedBuildType)
    {
        return true;
    }
```



Where `VRCSDKRequestedBuildType` is an enum of the following shape

```csharp
public enum VRCSDKRequestedBuildType
{
    Avatar,
    Scene,
}
```



This interface allows you to perform some logic before the VRChat SDK starts building the content. 

`OnBuildRequested` can also abort the build by returning `false`.

---

## ドキュメント: detecting-vrcsdk.md

```metadata
階層: /sdk/detecting-vrcsdk.md
ディレクトリ: sdk
ファイル名: detecting-vrcsdk.md
拡張子: .md
サイズ: 2.39 KB
最終更新: 2025-06-05T03:07:52.758Z
```

import CurrentUnityVersion from '@site/src/components/UnityVersionedText.js';
import UnityVersionedLink  from '@site/src/components/UnityVersionedLink.js';

# Detecting the VRChat SDK

There are several ways to detect the VRChat SDK in a Unity project. This can be helpful when developing Unity tools or libraries that do not depend on the VRChat SDK, but may still want to utilize [VRChat's SDK API](/sdk/public-sdk-api).

## Using Version Defines

The best way to detect the VRChat SDK is with <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/ScriptCompilationAssemblyDefinitionFiles.html#define-symbols">Version Defines</UnityVersionedLink> in your assembly definition file.

You can define symbols in your assembly when `com.vrchat.base`, `com.vrchat.avatars`, or `com.vrchat.worlds` are installed.
It is recommended to only use the "Expression" property to define your symbol when the installed VRChat SDK version is compatible with your tool.
For versioning of VRChat SDK, please refer to the [Creation Companion documentation](https://vcc.docs.vrchat.com/vpm/packages/#brandingbreakingbumps).

Since Version Defines is a feature for UPM packages, this method only works for VPM-based SDKs, which are treated as UPM packages by Unity.
If you also want to detect legacy `.unitypackage`-based SDKs, use legacy method below by defining the same symbol as the VRCSDK defines, or adding the following code to every file:

```csharp
#if !YOUR_VRCSDK3_AVATARS && !YOUR_VRCSDK3_WORLDS && VRC_SDK_VRCSDK3
    #if UDON
        #define YOUR_VRCSDK3_WORLDS
    #else
        #define YOUR_VRCSDK3_AVATARS
    #endif
#endif
```

## Using Legacy VRCSDK-defined scripting symbols (Deprecated) {#using-scripting-symbols}

The other method to detect VRChat SDK installation is with VRCSDK-defined scripting symbols.
For all VRCSDK projects, `VRC_SDK_VRCSDK3` will be defined, and for world projects, `UDON` will be defined.

This method is deprecated and will be removed in the future. Do not solely depend on this method.

In the old VRChat SDK, `VRC_SDK_VRCSDK3` and `UDON` symbols were used internally. But because those symbols are active in the whole project, many tools depend on those symbols for detecting the VRChat SDK.

Currently, in the VRChat SDK, all those symbol usages are migrated to Version Defines. Please migrate to Version Defines as soon as possible!



---

## ドキュメント: example-central.md

```metadata
階層: /sdk/example-central.md
ディレクトリ: sdk
ファイル名: example-central.md
拡張子: .md
サイズ: 3.13 KB
最終更新: 2025-06-05T03:07:52.758Z
```

# Example Central

Example Central provides examples for the VRChat SDK, which you can browse, download and modify to learn from for your own projects.

![Example Central Window Layout](/img/sdk/example-central/ec-layout.png)

:::info

Example Central has only recently been released.

- We're working on improving the features, design and examples.
- Currently, only world examples are available, Avatar examples will be added in the future.
- You can send us your feedback on the [Example Central Canny Board](https://feedback.vrchat.com/example-central).
- Your VRChat account must have a trust rank of "New User" or higher.

:::

## Basic Usage
Open the window from the Unity Editor Menu under "VRChat SDK > 🏠 Example Central"

![Opening Example Central From Menu](/img/sdk/example-central/ec-menu.png)

Example Central has two main sections - a listing of examples on the left, and information about the selected example on the right.

![Example Central Window Layout](/img/sdk/example-central/ec-overlay.png)

* Browse the list and select an example to see more information about it.
* Once selected, the example's details are shown on the right.
* Press the "Docs" button to open a page with detailed information and a walkthrough of using the example.
* Press the "Import" button to import the example's `.unitypackage` into your project.

## Example Details

Each example has a thumbnail, title, version, tags, description, guide, and example scene. World examples also have a published VRChat world. Lots of information to help you get started!

### Version

Each example has a version indicator. When an example is updated, you'll see the newest version in Example Central.

Examples use [Semantic Versioning](https://semver.org/).

:::warning

Currently, you cannot see the version of an example after importing it. This makes it difficult to see whether you are using the latest version. 

In the future, versions will have release notes and a mechanism to check whether you're using the latest version.

:::


### Tags

Look at the tags of examples to quickly understand the systems and concepts involved. You can search for any tag to filter the list of examples.

### Guide

Each example has a detailed guide in the Creator Docs, which you can access by pressing the "Docs" button in the window.

### Example World

Visit the example world in VRChat to get a better understanding of how the example works on each platform!


### Settings

The gear icon (⚙) beside the search box opens up the settings of Example Central. 

![Example Central Window Layout](/img/sdk/example-central/ec-settings.png)

There is one setting:
- Show Creator Economy Examples

:::warning

The setting is disabled by default because it is currently in Beta testing. You can enable it, but the examples will not function properly if you are not a Beta tester.

:::


## Feedback

What do you think of Example Central?
- Use the [Example Central Canny Board](https://feedback.vrchat.com/example-central) to tell us your feature requests and report bugs.
- Your VRChat account must have a trust rank of "New User" or higher.


---

## ドキュメント: index.md

```metadata
階層: /sdk/index.md
ディレクトリ: sdk
ファイル名: index.md
拡張子: .md
サイズ: 4.79 KB
最終更新: 2025-06-05T03:07:52.759Z
```

---
sidebar_position: -1
---

# Getting Started

Before you can create [avatars](/avatars) and [worlds](/worlds), you need to perform the following steps.

:::tip

If you're new to VRChat, read the [VRChat's "Getting Started"](https://vrch.at/getting-started) page.

:::

The **[VRChat Creator Companion](https://vcc.docs.vrchat.com/)** is the easiest and quickest way to get started. It installs [Unity](https://unity.com/) and the VRChat software development kit (SDK) for you.

- VRChat uses the Unity game engine. Avatars and worlds are also created in Unity.
- The VRChat SDK allows you to use Unity to create avatars and worlds. 

## Your first project
You should use Windows to create your first project.

1. [Download the VRChat Creator Companion (VCC)](https://vrchat.com/download/vcc).
    - Read the [documentation](https://vcc.docs.vrchat.com/). 
2. Install the VCC.
	- The default install location is `%LocalAppData%/Programs/VRChat Creator Companion`, but you can change this as you'd like.
4. The VCC should open automatically. If not, search for "Creator Companion" in Windows.
5. Click "Create New Project".
6. Choose "Avatar" or "World project".
7. Name your project.
8. Choose a location.
9. Click "Create Project".

:::note
Do you want to upload to Android, Quest, or iOS? Read the [platforms](/platforms) page.
:::

## Opening your project

You can now open your new project! After creating a new project, the next page in the Creator Companion will show an **Open Project** button. You can also access it from the **Projects** tab on the left sidebar.

If your project isn't listed, click the dropdown menu next to **Create New Project** and then **Add Existing Project** via the project screen and select it. After the project is open:

1. Check the title bar to ensure it ends with `PC, Mac & Linux Standalone <DX11>`. 
    - If it does not, then go to `File > Build Settings...`, select `PC, Mac & Linux Standalone`, then click `Switch Platform` in the bottom left.

2. Navigate to `VRChat SDK > Show Control Panel > Authentication`. 

3. Sign into your VRChat account. You'll need to do this to upload any content you create.
    - You must have a VRChat account of at least "New User" [Trust Rank](https://docs.vrchat.com/docs/vrchat-safety-and-trust-system) to upload content. You cannot use a Steam, Meta, or Viveport account to upload content.

## Using Unity Hub instead
Though we don't recommend this, if you'd like to install Unity yourself without the VCC, check the [Current Supported Unity Version](/sdk/upgrade/current-unity-version) page and install the version of Unity that VRChat currently supports using the Unity Hub.

If you didn't use the VCC to set up your project, you'll also need to install the SDK. Do so via the [VRChat Creator Companion](https://vcc.docs.vrchat.com/guides/getting-started).

To create projects using just the Unity Hub:
* Open Unity Hub (or just the editor, if you chose to go that route).
* Create a new project, **set it to 3D, and save it**.
* Don't use HDRP or URP. VRChat doesn't use it.

To open projects using just the Unity Hub:
* Click **Open** in the top right, then select the directory where your project lives.

## Tips 

* If you're building content for VRChat for Meta Quest, you should also be building for Android. Check our [Android documentation](/platforms/android/index.md) for more details.
* Save your projects in a mass-storage drive with a lot of space. Unity projects can get quite large, especially if you use versioning software
* Do not use a single project for tons of different avatars or worlds. This is a quick way to make future migrations a huge pain in the butt!
* If you know how to use version control software like [Git](https://git-scm.com/) or [Unity Version Control](https://unity.com/solutions/version-control), use it! It makes it very easy to roll back changes that break your project.

### Understanding the Terms "Player" vs. "User"

In VRChat, the terms "player" and "user" can refer to different things depending on the context:

- User: This is the standard term for anyone using VRChat, whether they're in the same world as you or not. It’s the general term used for all participants on the platform.
- Player: In technical terms, a "player" specifically refers to the individual instantiation of a user within a world instance. This distinction is more relevant for the underlying system and certain technical features (like multiplayer interactions). For example, when locally testing worlds there can be multiple players of the same user in a world instance.

### What's Next?
Your project is ready! You can move on to [World Creation](/worlds) or [Avatar Creation](/avatars).

If there are any errors, even with a brand new empty project, [please contact our Support team](https://vrch.at/support).


---

## ドキュメント: public-sdk-api.md

```metadata
階層: /sdk/public-sdk-api.md
ディレクトリ: sdk
ファイル名: public-sdk-api.md
拡張子: .md
サイズ: 4.05 KB
最終更新: 2025-06-05T03:07:52.759Z
```

---
title: "Public SDK API"
slug: "public-sdk-api"
hidden: false
createdAt: "2023-08-18T21:01:07.855Z"
updatedAt: "2023-08-18T21:04:48.137Z"
---

The VRChat SDK provides a set of interfaces and methods you can use to enhance your world and avatar build process. 

You can find the **Public SDK API** folder in both SDKs:

- For worlds: Packages/VRChat SDK - Worlds/Editor/VRCSDK/SDK3/Public SDK API
- For avatars: Packages/VRChat SDK - Avatars/Editor/VRCSDK/SDK3A/Public SDK API

However, most of the events and methods are shared between both the world and avatar SDKs and are defined in the **Base SDK Package**: `Packages/VRChat SDK - Base > Editor/VRCSDK/Dependencies/VRChat/Public SDK API`.

Other methods marked as `[PublicAPI]` are also be a part of Public SDK API. E.g., "Packages/VRChat SDK - Base/Editor/VRCSDK/Dependencies/VRChat/API/VRCApi.cs" for updating description of contents.

:::note

These types are in assembly definitions that are auto referenced. If you are writing code in your own project, the types will usually be available by default. If you're writing a redistributable package and have your own assembly definition, you will need to add the appropriate assembly definition references:

For avatars: `VRC.SDK3A.Editor`  
For worlds: `VRC.SDK3.Editor`  
For both: `VRC.SDKBase.Editor`

:::

# What's available?

For the most up-to-date list of events and methods, we recommend looking at the files directly mentioned above.

But here is a short list of what is available:

- OnEnable/OnDisable events of the main SDK Panel
- Build Start/End events
- Upload Success/Error events
- Build, Build and Test, and Build and Upload methods

:::note
If you run into exceptions during the build process, you can view the list of expected exceptions in the Interface definitions.
:::
## Examples

### Getting an instance of a builder

Connecting to `OnSdkPanelEnable` will ensure that the SDK window was opened and the builders were registered. You can then use `TryGetBuilder` to get an instance of the builder you need.

> You can call `VRCSdkControlPanel.TryGetBuilder` at any point in time, but it will return false if the SDK window is not open or the builder you're trying to access is not available.

```cs
[InitializeOnLoadMethod]
public static void RegisterSDKCallback()
{
    VRCSdkControlPanel.OnSdkPanelEnable += AddBuildHook;
}

private IVRCSdkAvatarBuilderApi _builder;

private static void AddBuildHook(object sender, EventArgs e)
{
    VRCSdkControlPanel.TryGetBuilder<IVRCSdkAvatarBuilderApi>(out _builder);
}
```

### Running code before the build

`OnSdkBuildStart` runs right before the SDK kicks off the build process, but after validations and Build Request Callbacks have passed.

```cs
[InitializeOnLoadMethod]
public static void RegisterSDKCallback()
{
    VRCSdkControlPanel.OnSdkPanelEnable += AddBuildHook;
}

private static void AddBuildHook(object sender, EventArgs e)
{
    if (VRCSdkControlPanel.TryGetBuilder<IVRCSdkAvatarBuilderApi>(out var builder))
    {
        builder.OnSdkBuildStart += OnBuildStarted;
    }
}

private static void OnBuildStarted(object sender, object target)
{
    Debug.Log("Building " + ((GameObject) target).name);
}
```

### Building from script

```cs
[MenuItem("My Tools/Build Selected Avatar")]
public static async void BuildSelectedAvatar()
{
    var avatar = Selection.activeGameObject;
    if (!VRCSdkControlPanel.TryGetBuilder<IVRCSdkAvatarBuilderApi>(out var builder)) return;
    try {
      await builder.BuildAndTest(avatar);
    } catch (Exception e) {
      Debug.LogError(e.Message);
    }
}
```
## Heads up
:::caution
If you're currently using reflection to access the SDK internals, we recommend switching to the public API as soon as possible.
:::
We're going to make our best effort to provide a stable API, but it's still subject to change in the future. We recommend leveraging semver to define which version of the SDK your tools are compatible with. [Learn more here](https://vcc.docs.vrchat.com/vpm/packages/#versions-and-ranges).



---

## ドキュメント: sdk-troubleshooting.md

```metadata
階層: /sdk/sdk-troubleshooting.md
ディレクトリ: sdk
ファイル名: sdk-troubleshooting.md
拡張子: .md
サイズ: 3.12 KB
最終更新: 2025-06-05T03:07:52.759Z
```

---
title: "SDK Troubleshooting"
slug: "sdk-troubleshooting"
excerpt: "Common SDK issues and how to solve them"
hidden: false
createdAt: "2017-09-29T02:44:14.279Z"
updatedAt: "2020-04-23T03:39:19.161Z"
---
Here are common issues you may come across when using the SDK and how to solve them. You can find some additional assistance at our [Help Knowledgebase](http://help.vrchat.com).

## The build control panel isn't appearing in the VRChat SDK menu dropdown!
There are two main reasons this might be happening: 

Make sure you're using the recommended version for VRChat. (See [Setting up the SDK](/sdk))

Check your console tab to ensure there aren't any compilation errors from third-party scripts or components. If there are, you may need to remove those components/scripts.

## I uploaded my content but I can't see it in-game!
There are a few reasons this might be the case. Here are some things to check:
- Make sure you are using the [correct version of Unity](/sdk/upgrade/current-unity-version).
- Make sure you are using the correct account in-game and are not logged into a platform account.
- Check the `Content Manger` tab found in the control panel window to see if your content is there.
- Check the editor console to see if there were any errors when uploading.

## I can't see one of the windows but there aren't any errors!
In rare cases, Unity tabs can go off screen and become inaccessible. You'll need to delete some registry keys to get these windows back on your screen. 

1. Close Unity.
2. Press your Windows key and type `regedit`. Press Enter. You'll be prompted by UAC to permit Administrator access.
3. Be very careful in `regedit`! This application contains all of the settings for your PC.
4. Find the following key: `Computer\HKEY_CURRENT_USER\Software\Unity Technologies\Unity Editor 5.x`. You can do by pasting it into the bar at the top of `regedit`, or if you don't have a bar at the top of the window, navigating through the directory listing on the left.
5. Remove ALL keys in that directory that start with `VRC`, including `VRCSDK2` or any other related keys.
6. Close `regedit`.
7. Reopen Unity. You should be good to go!

## I'm getting errors related to SDK3 or Udon in a project using SDK2 or VRC_SpatialAudioSource in a project using SDK3!

1. Follow [the correct steps](/sdk/updating-the-sdk) on removing your SDK by closing Unity and removing all the SDK related folders and their related `.meta` files.
2. Go to your `Project Settings` and find `Scripting Define Symbols` under `Player > Other Settings`.
3. Remove the symbols that are not associated with the SDK your project was made on. For projects made with SDK2 remove `UDON` and `VRC_SDK_VRCSDK3`. For projects made with SDK3 remove `VRC_SDK_VRCSDK2`. The symbols are separated by `;`. Afterwards save changes by pressing `Enter`.
4. Import the correct SDK into the project.

## I have a problem that the Knowledgebase doesn't solve and isn't listed here!

Sorry about that! Please browse our [official knowledgebase](http://help.vrchat.com/) or [submit a support request](https://help.vrchat.com/hc/en-us/requests/new). We're happy to help you.

---

## ドキュメント: updating-the-sdk.md

```metadata
階層: /sdk/updating-the-sdk.md
ディレクトリ: sdk
ファイル名: updating-the-sdk.md
拡張子: .md
サイズ: 5.30 KB
最終更新: 2025-06-05T03:07:52.759Z
```

---
title: "Updating the SDK"
slug: "updating-the-sdk"
hidden: false
createdAt: "2019-07-24T03:33:36.520Z"
---
If you need to update your SDK, it is important that you follow these steps to ensure the update proceeds properly and you don't have any old/conflicting files.

## Version Control
If you know how to use it, you may find it beneficial to use version control software like [Git](https://git-scm.com/) to manage your project. You don't need to upload your repository to Github or similar service to gain the benefits of version control. Create a commit before you upgrade SDKs just to be sure.

## VRChat Creator Companion
Keeping your SDK up to date is extremely easy with the VCC. See the [VRChat Creator Companion docs](https://vcc.docs.vrchat.com/guides/getting-started) to learn how to use it!

## Migrating to the VCC
If you want to learn how to migrate your project to use the VCC, check out the guide [here](https://vcc.docs.vrchat.com/vpm/migrating).

## Manual Updates
If you're using the VCC, you don't need to worry about manual updates.
However, if you wish to, you can download the SDK on the [VRChat website](https://vrchat.com/home/download) after signing into your VRChat account.

## Legacy SDK3
:::caution 

These instructions only apply to users of our Legacy SDK (`Assets\VRCSDK`).
If your SDK is in your `Packages` folder (`Packages\com.vrchat.base`), do not follow the instructions below.
:::

**For SDK3, you should be able to update by simply importing the new SDK over the old one, also known as an in-place upgrade.** This is especially important for SDK3-Avatars, as you may lose State Behaviors on your animators if you incorrectly update!

If you want to be super careful, always back up your projects before updating the SDK.

### SDK3 - World
1. Close Unity.
2. Back up your Unity project! You don't have to backup your Library folder, these files are auto-generated by Unity.

:::caution Pre-2020.3.2 SDK3 Upgrade

**This is an uncommon step and you probably don't need to do it.** 

If you are upgrading from an SDK older than 2020.3.2, go into your project's `Assets` folder and delete the `VRCSDK` and `Udon` folders as well as the `VRCSDK.meta` and `Udon.meta` files.
:::
3. Open your Unity project.
4. Import the new SDK3 - World over the old one.

### SDK3 - Avatars
:::danger Do not perform "Deletion Reinstalls" For SDK3 - Avatars!

***If you delete the SDK folders with Unity closed and open Unity without the SDK installed, you will lose State Behaviors.*** They are fragile and do not persist through full deletion upgrades. Make sure you back up your projects often, and save/document your state behavior setups.

If you **must** perform a full deletion reinstall of your SDK3 - Avatars package, back up your project first. You will have to set up your State Behaviors again, so ensure you have documented them well.
:::
1. Close Unity.
2. Back up your Unity project! You don't have to backup your Library folder, these files are auto-generated by Unity.
3. Open your Unity project.
4. Import the new SDK3 - Avatars over the old one. If you get errors after import is complete, try restarting Unity afterward. 

:::note SDK3 Expected Errors on Avatar Dynamics Upgrade

It is expected to have a handful of errors when you first upgrade SDK3 for Avatars to the Avatar Dynamics SDK. This is due to the SDK installing the Burst and Mathematics packages during the install process, and Unity getting a bit ahead of itself in importing them early.

If you clear the errors or restart Unity, they should go away.
:::

### SDK3 - Avatars - Separate Project Process
If you run into issues with upgrading via the above process, try this instead:
1. Close Unity.
2. Make a new, blank project.
3. Import the new SDK3 - Avatars package into that project.
4. Close that Unity project.
5. Using Explorer (Do not open Unity yet!), delete the VRCSDK3 folders from the project you're upgrading. Until this guide says otherwise, ***do not open Unity.***
6. Edit the references of the VRChat SDK3 from `Packages/vpm-manifest.json` to the new SDK version from the project you're upgrading.
7. From your new blank project that you imported the SDK into, copy the VRCSDK3 folders into the project that you're upgrading.
8. After you copy the files, open Unity and open your upgraded project. You may delete the blank project.

### Advanced Update Process

If you're reinstalling the SDK in a project that contains a world using complex trigger setups, here's a safer way to update your SDK.

1. Close Unity
2. Back up project to another folder (do not back up the Library folder, these files are auto-generated by Unity)
3. Delete SDK and Plugins folder, as well as the the associated .META files
4. Edit the references of the VRChat SDK3 from `Packages/vpm-manifest.json` to the new SDK version from the project you're upgrading.
5. Create a new "dummy" Unity Project by creating a blank Unity project with Unity Hub.
6. Install the latest VRChat SDK on the dummy project.
7. Copy the newly added `SDK/Plugin` folder and associated `.meta` files from the dummy project into your original project.
8. Done. You can now open your upgraded project.

## Updating Unity

If you are updating from a previous version of Unity, we [have a guide for updating](/sdk/upgrade/index.md) to our latest version we support!


---

## ドキュメント: vrcpipelinemanager.md

```metadata
階層: /sdk/vrcpipelinemanager.md
ディレクトリ: sdk
ファイル名: vrcpipelinemanager.md
拡張子: .md
サイズ: 1.60 KB
最終更新: 2025-06-05T03:07:52.763Z
```

---
title: "VRCPipelineManager"
slug: "vrcpipelinemanager"
hidden: false
createdAt: "2017-11-22T18:54:45.512Z"
updatedAt: "2019-10-28T19:23:09.669Z"
---
Used to store the ID of a world or avatar.
:::note Added Automatically

This component is added automatically when the component it depends on is added to an object. You should not need to add this component manually.
:::

| Parameter    | Description                           |
|--------------|---------------------------------------|
| Blueprint ID | The unique id for the avatar or world |

If you want to upload the world or avatar to a different blueprint you can press the `Detach (Optional)` button
:::danger Required Blueprint Format

Blueprint IDs can only be of the following format where 0 is replaced with [0-9] [a-f]:

`avtr_00000000-0000-0000-0000-000000000000`

`wrld_00000000-0000-0000-0000-000000000000`

Any other ID format will not be accepted. This is normally done automatically, so you shouldn't ever have to create your own Blueprint ID-- just click "Attach" and one will be generated for you.
:::

![vrcpipelinemanager-7d57e76-Unity_2017-12-10_01-35-44.png](/img/sdk/vrcpipelinemanager-7d57e76-Unity_2017-12-10_01-35-44.png)

If you have a blueprint id that you want to upload to you can attach a new one with the `Attach (Optional)` button

![vrcpipelinemanager-db63e77-Unity_2017-12-10_01-37-47.png](/img/sdk/vrcpipelinemanager-db63e77-Unity_2017-12-10_01-37-47.png)

:::caution TIP

Don't have more than one PipelineManager in the scene when building a world! You may end up uploading to the wrong blueprint ID.

:::

---

# ドキュメント終了

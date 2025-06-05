# upgrade 統合ドキュメント

以下は自動収集された全ドキュメントのコンテンツです。各セクションの始めにメタデータが記載されています。



---

## ドキュメント: current-unity-version.md

```metadata
階層: /sdk/upgrade/current-unity-version.md
ディレクトリ: sdk\upgrade
ファイル名: current-unity-version.md
拡張子: .md
サイズ: 2.31 KB
最終更新: 2025-06-05T03:07:52.759Z
```

---
sidebar_position: 1
---
import CurrentUnityVersion from '@site/src/components/UnityVersionedText.js';
import UnityVersionedLink from '@site/src/components/UnityVersionedLink.js';

# Current Unity Version

The current Unity version used by VRChat is <UnityVersionedLink versionKey="patch" url="https://unity.com/releases/editor/whats-new/<VERSION>"><CurrentUnityVersion/></UnityVersionedLink>

The simplest way to install the latest Unity version is with the [Creator Companion](https://vcc.docs.vrchat.com/#download-it). After installing the Creator Companion, simply follow the instructions. 

Alternatively, if you have Unity Hub installed, you can <UnityVersionedLink url="unityhub://<VERSION>">click this link</UnityVersionedLink> to install the correct version of Unity. <CurrentUnityVersion/> is also available in the [Unity editor release archive](https://unity.com/releases/editor/archive).

In the installation screen, choose "Android Build Support" if you'd like to be able to create worlds or avatars for Android or Quest.

For instructions on how to upgrade from Unity 2019, [visit our Unity 2019 to 2022 documentation](/sdk/upgrade/unity-2022).

## Differences from previous versions

Unity 2022 includes many improvements such as faster iteration times, improved asset import times, *much* faster platform switching times, better editor stability, fully featured C# 8 support, a quick search feature, [and much more!](https://unity.com/releases/lts)


## Known Issues

* The first time you open a Scene and select a GameObject inside a prefab with a U# Behaviour, the GUI for the component directly below that U# Behaviour will not show its GUI. Deselecting and re-selecting the prefab fixes this.
* Buffer Particles don't work as they did in Unity 2019, [there is a workaround to fix them from community member hfcRed here](https://x.com/hfcRedddd/status/1696915379090604179).
* Unity 2022 sometimes causes Rider's debugger to stop for unhandled exceptions in Unity's IMGUI.
    * Please refer to [this workaround](https://forum.unity.com/threads/rider-debugger-breaks-on-unhandled-exception.1135879/#post-7305256) and [Jetbrains's bug tracker](https://youtrack.jetbrains.com/issue/RIDER-64944) for more information.
* Spatialized Audio Sources can create warnings when entering playmode or adjusting their settings.

---

## ドキュメント: index.md

```metadata
階層: /sdk/upgrade/index.md
ディレクトリ: sdk\upgrade
ファイル名: index.md
拡張子: .md
サイズ: 846 B
最終更新: 2025-06-05T03:07:52.760Z
```

---
excerpt: Learn how to update to a newer version of Unity
sidebar_position: -1
---
# Managing Unity

If you've worked with the VRChat SDK in the past, your project may be using an old version of Unity and the VRChat SDK. To use the latest version of the VRChat SDK, you'll first need to update Unity.

- [Check which versions of Unity you already have installed](unity-2022.md#managing-unity-versions) on your PC that are VRChat compatible.
- Read our [Unity 2022 migration guide](/sdk/upgrade/unity-2022) to learn how to upgrade your Unity 2019 project to Unity 2022.
- Learn how to perform a [minor Unity version upgrade](/sdk/upgrade/migrating-to-a-newer-minor-unity-version).

:::info Starting a new project?

Read [Setting up the SDK](/sdk/index.md) to learn how to create new projects with VRChat's latest Unity SDK.

:::

---

## ドキュメント: migrating-to-a-newer-minor-unity-version.md

```metadata
階層: /sdk/upgrade/migrating-to-a-newer-minor-unity-version.md
ディレクトリ: sdk\upgrade
ファイル名: migrating-to-a-newer-minor-unity-version.md
拡張子: .md
サイズ: 3.00 KB
最終更新: 2025-06-05T03:07:52.762Z
```

---
title: "Minor Unity Upgrades"
---
Occasionally, VRChat will update within minor Unity versions. For example, VRChat might update from Unity 2022.1.**1** to Unity 2022.1.**2**.

## Install the new Unity version

1. Close all of your open Unity projects.

2. Check the [Currently Supported Unity Version](/sdk/upgrade/current-unity-version) and install the new version of Unity via Unity Hub. 
    - Although we list the standalone installer on that page, we strongly recommend using the Hub. For this doc, we're assuming you're using it.

## Copy your project

1. Copy or back up your project.
	- If you're using the [VRChat Creator Companion](https://creators.vrchat.com/), it will automatically suggest copying your project before migrating it. You can create a backup of your project with the "Make Backup" button.
	- Otherwise, duplicate the whole project folder and give it a new name.
	- Export your entire project as a Unity Package. This takes a long time and may cause errors.

:::danger Don't skip this step!
Upgrades can fail. If you keep your original project files safe, you can restore them, try again, and find out what went wrong.

Without a backup, you don't get a second try. If you make a mistake or the upgrade fails, fixing it may be difficult or even impossible.
::: 

If you're an advanced user and know how to use version control like [Git](https://git-scm.com/), you should use that.

## Open your project

1. Open the copy of your project in the new version. 
    - You'll get some upgrade warnings. This is fine! Click the affirmative button.

2. After some time, your migration will be complete. That's it!

## Optional info and tips

SDK updates aren't always needed in minor version upgrades. If they are, this is when you'd do it.

1. Close your project after upgrading.
2. Use the [VCC](https://vcc.docs.vrchat.com/) to upgrade your SDK.

#### Unity warnings

There are a few Unity warnings that may pop up during migration that you can safely click past. Here are a few you may see:

![migrating-to-a-newer-minor-unity-version-f3995eb-image_10.png](/img/sdk/migrating-to-a-newer-minor-unity-version-f3995eb-image_10.png)

![migrating-to-a-newer-minor-unity-version-b20553b-image_11.png](/img/sdk/migrating-to-a-newer-minor-unity-version-b20553b-image_11.png)

#### Clean up the copy

If your project is large, migration might take a long time. There are a few folders that you don't need to migrate over if the project is especially huge. You can delete any of these folders safely from the copy.

```text
/Library/
/Temp/
/Obj/
/Build/
/Builds/
/Logs/
/UserSettings/
```
#### Version warnings

The SDK may warn you that you're on the wrong version, even though you _know_ you're on the correct one.

![migrating-to-a-newer-minor-unity-version-1b8194d-2022-11-30_10-35-54_chrome.png](/img/sdk/migrating-to-a-newer-minor-unity-version-1b8194d-2022-11-30_10-35-54_chrome.png)

This is fine! If you know for a fact you're on the correct version, you can ignore this message.

---

## ドキュメント: unity-2022.md

```metadata
階層: /sdk/upgrade/unity-2022.md
ディレクトリ: sdk\upgrade
ファイル名: unity-2022.md
拡張子: .md
サイズ: 6.97 KB
最終更新: 2025-06-05T03:07:52.762Z
```

---
title: Upgrading Projects to 2022
sidebar_position: 2
---
import CurrentUnityVersion from '@site/src/components/UnityVersionedText.js';

This page explains how to upgrade your VRChat project from version 2019.4.31f1 to Unity <CurrentUnityVersion/>.
Unity <CurrentUnityVersion versionKey="major"/> is required to use the latest version of the VRChat SDK. You can check out the [benefits to upgrading here](/sdk/upgrade/current-unity-version).

If you're using the Creator Companion, upgrading your project is easy! But first, make sure you create backups of your projects.

## Backing up your Unity project

If you're using the [VRChat Creator Companion](https://creators.vrchat.com/), it will automatically suggest backing up your project before migrating it.

1. To back up your project, click the three dot menu next to the Manage Project button, then choose the "Create Backup" option. This is the recommended method of backup, especially for newer creators! 
	- Note that "Create Backup" will not back up the `Udon`,  `UdonSharp`, or `VRChat Examples` folder. If you made changes to those folders, choose a different backup option.
	- You can also back up the project by duplicating the whole project folder with a new name, such as `MyProject-Backup`.  
	- You *can* export your entire project as a Unity Package, but this takes a long time and may cause errors. We don't recommend it.

![Creating a backup.](/img/sdk/migrate-2019-2022/creating_backup.png)

:::danger Don't skip this step!
Keeping a backup when making major changes to your precious project is always a good idea.

Upgrades can fail. If they do, your backup can be used as a checkpoint. If you keep your original project files safe, you can restore them, try again, and find out what went wrong.

**Without a backup, you don't get a second try.** If you make a mistake or the upgrade fails, fixing it may be difficult or even **impossible.**
:::

1. Open your project in Unity 2019 to see if there are any errors or warnings in [Unity's console](https://docs.unity3d.com/Manual/Console.html) or the [VRChat SDK Build panel](https://creators.vrchat.com/worlds/creating-your-first-world#step-4---configure-your-world-in-the-sdk-build-panel).
	- If you don't fix issues in your Unity 2019 project, they may cause issues in Unity 2022.
	- *Some* warnings can be safely ignored, but you should try to understand why they're present.

Now you're ready to upgrade!

:::danger Test your content before uploading it

After successfully uploading a world or avatar with Unity 2022, you won't be able to upload it again with Unity 2019.

:::

## Using the Creator Companion 

There are two ways you can upgrade your project from the VCC: Directly from your Projects page or from each Manage Project page. **Make sure the project you are trying to upgrade is closed before proceeding.**

1. Go to Settings, then click Updates to see if your Creator Companion needs to be updated. Without updating, prompts to upgrade to Unity 2022 will not show.

![Check VCC updates.](/img/sdk/migrate-2019-2022/updating_vcc.png)

2. On the Projects page, you'll see a new **Unity** column with a version switcher for each of your projects. Click this, then click Migrate to Unity 2022.

![Click the correct project.](/img/sdk/migrate-2019-2022/updating_vcc_via_projects.png)

Otherwise, click **Manage Project** on any project and you'll see an Upgrade to 2022 banner.

![Upgrade via Manage Project.](/img/sdk/migrate-2019-2022/manage_project_upgrade.png)

## Using the Unity Hub

Unity Hub is a separate application that allows you to seamlessly install and work with multiple Unity versions at one time. You can also use it to install Unity 2022 if you do not wish to use the Creator Companion.

1. Install the [Unity Hub](https://unity.com/download).
	- You can follow Unity's official [installation guide](https://learn.unity.com/tutorial/install-the-unity-hub-and-editor).
	- You'll need to create a [Unity account](https://id.unity.com/account/new) after installing Unity Hub.
2. Visit [Unity's download archive](https://unity.com/releases/editor/archive).
3. Click **Unity 2022.x**.
4. Scroll down until you see Unity <CurrentUnityVersion versionKey="patch"/> and click the blue **Unity Hub** button.
	- Do not choose the first Unity version in the list!
	- You can also go to the [Current Unity Version page](/sdk/upgrade/current-unity-version) to find a link to download the correct version of Unity.

![Select the right version to install in the Unity Archive.](/img/sdk/migrate-2019-2022/unity_webpage_search.png)

![Accept the browser prompt to open Unity Hub.](/img/sdk/migrate-2019-2022/browser-prompt-unity-hub.png)

5. Click **Open Unity Hub** in your web browser.

![Enable Android Build support if you'd like to be able to develop content for Android devices.](/img/sdk/migrate-2019-2022/unity_version_hub_upgrade_android.png)

6. Enable **Android Build Support** in the Unity installation screen.
	- You can skip this if you're not planning on uploading content to Android or Quest yet.
	- You can complete this step later by choosing [Add modules](https://docs.unity3d.com/hub/manual/AddModules.html) in the Unity Hub.
7. Click **Continue** to install Unity <CurrentUnityVersion versionKey="patch"/>.

## Managing Unity versions

It's easy to check what versions of Unity are available on your PC for use with the Creator Companion. You can also change what version of Unity is used to open all *new* projects.

1. In the CC, go to **Settings**.
2. Under **Unity Editors**, use the dropdown menu to change the default Unity editor. This will **not** apply retroactively to any projects already created.
3. If you have not already installed Unity <CurrentUnityVersion versionKey="patch"/>, [follow the instructions above](#using-the-creator-companion). 
4. If you don't see a version of Unity here that you've installed, try using the refresh button or finding the path directly using the file button.

## Packages

If you are trying to add packages to your 2022 project, please keep in mind:

- Every Curated Package has a version which works in 2022.
- Other packages *may* work, but some may need a new version from the package author!
- Many errors will result from using out of date packages, so make sure anything you import is compatible with Unity 2022.

# Troubleshooting

- Test your world before uploading it. Check Unity's console for errors, and test your world in VRChat to see if it work.
	- After successfully uploading a world with Unity 2022, you won't be able to upload that same world with Unity 2019.
- [Make sure to activate your Unity Personal license](https://support.unity.com/hc/en-us/articles/211438683-How-do-I-activate-my-license-) before using VRChat's SDK. Unity is free for personal use.
- To learn more about the Unity Hub, visit [Unity's documentation](https://docs.unity3d.com/hub/manual/index.html).
- If you're experiencing issues related to the SDK itself, please read our [SDK Troubleshooting](/sdk/sdk-troubleshooting) page.


---

## ドキュメント: worlds-and-avatars-in-2022.md

```metadata
階層: /sdk/upgrade/worlds-and-avatars-in-2022.md
ディレクトリ: sdk\upgrade
ファイル名: worlds-and-avatars-in-2022.md
拡張子: .md
サイズ: 953 B
最終更新: 2025-06-05T03:07:52.762Z
```

---
title: New Worlds and Avatars in 2022
sidebar_position: 2
---

import CurrentUnityVersion from '@site/src/components/UnityVersionedText.js';

Looking to make a new project in Unity 2022? Luckily it's pretty easy: The Creator Companion will handle most of the work for you!

### Download the Creator Companion
**If you don't already have the VCC**, you'll need to download it! 

Follow the steps on our [Getting Started](/sdk/) page.

This is the easiest way to create a new project with the correct Unity version. The Creator Companion will automatically install Unity <CurrentUnityVersion/>, the world SDK, and the avatar SDK for you.

**If you already have the VCC**, [make sure it's updated and upgraded to Unity <CurrentUnityVersion versionKey="major"/>](unity-2022.md#using-the-creator-companion), and you're [launching all new projects in Unity <CurrentUnityVersion versionKey="major"/>](unity-2022.md#managing-unity-versions).

---

# ドキュメント終了

# iOS 統合ドキュメント

以下は自動収集された全ドキュメントのコンテンツです。各セクションの始めにメタデータが記載されています。



---

## ドキュメント: build-test-mobile.md

```metadata
階層: /platforms/iOS/build-test-mobile.md
ディレクトリ: platforms\iOS
ファイル名: build-test-mobile.md
拡張子: .md
サイズ: 5.59 KB
最終更新: 2025-06-05T03:07:52.753Z
```

---
sidebar_position: 0
---

import CurrentUnityVersion from '@site/src/components/UnityVersionedText.js';

# Build and Test for iOS

This guide shows you how to test your VRChat world on iOS by connecting an iOS device to Unity. Build and Test allows you to quickly test your world in VRChat without publishing it.

## Set Up For Build and Test

To use Build and Test for iOS, you must set up your Unity project and connect your iOS device to the SDK.

### Set Up iOS Build Support Unity Module

You must install Unity's iOS Build Support module to build or publish VRChat worlds on IOS. Follow the steps below to install it:

1. Open Unity Hub and navigate to "Installs" on the left.
2. Find the install for the Unity version that your project uses (the current recommended version is [<CurrentUnityVersion/>](/sdk/upgrade/current-unity-version/)).
  - If your install lists "iOS" below the Editor path, then the module is already installed, and you can [set up your project](#set-up-your-project-to-target-ios).

![Unity Install with iOS Module](/img/platforms/unity-hub-modules.png)

3. Click on the gear icon next to that install and click "Add modules".
4. Find "iOS Build Support". Check the box next to "iOS Build Support" to include it.
5. Click continue to install the iOS build support module.

You should now be able to build worlds for iOS.

### Set Up Your Project to Target iOS

1. Open the SDK panel by selecting "VRChat SDK"->"Show Control Panel" and then selecting the Builder tab.

2. Find the "Select Platform" section and change it to "iOS".

3. After switching your platform to iOS, you should see `[VTP] Listening on port 9002...` in the console.
See [debugging steps](#debugging-connection-issues) if you do not see this.

### VRChat Test Protocol

Build and Test for iOS makes use of the VRChat Test Protocol (VTP) to facilitate communication between the SDK and iOS devices.

The SDK uses VTP to send World and Avatar files to your iOS device over your local network, so you can instantly see the results of a build in the iOS client.

### Connecting the SDK to your iOS Device

You can connect the SDK to your iOS device by entering its local IP address in the iOS app settings.

1. Find the local IP address of the machine running the SDK, which can be found in the SDK settings tab by pressing the "Show Local IP Address" button.
2. Enter the local IP address found in the SDK settings panel into the iOS app settings under "Settings"->"Apps"->"VRChat"->"SDK IP Address"
3. Launch VRChat and press the "Play" button to enter a world.
4. Once loaded into the world, the app may ask for permission to communicate over your local network. You must allow this for the VRChat Test Protocol to be able to communicate with your device.
5. After accepting the local network permission, check the SDK console to see if the connection was successful. You should see `[VTP] Client connected.` along with details about what device was connected. See [debugging steps](#debugging-connection-issues) if you do not see this.
6. You can now click Build and Test to test content directly on your iOS device.

## Debugging Connection Issues

If you find that you are unable to establish connection between your iOS device and the SDK, follow these steps to debug the issue.

- If you do not see `[VTP] Listening on port 9002...` after switching to the iOS platform in the SDK, ensure that no other editors are open with iOS selected. Additionally, ensure that no other running applications are using port 9002, since only one application can open the port at a time.

- If you do see `[VTP] Listening on port 9002...` in the console, but you're unable to establish a connection:

- Make sure that the IP address shown in the SDK settings tab matches the one in the VRChat iOS App settings page under "Settings"->"Apps"->"VRChat"->"SDK IP Address".

- Make sure that both the SDK machine and the iOS device are on the same Wi-fi network.

- Make sure you have allowed local network access on your iOS device. This can be found at "Settings"->"Privacy & Security"->"Local Network." Locate the VRChat app and enable the permission.

- Make sure you have allowed local network access on your SDK machine. On Windows, local network access requests appear as a prompt titled "Windows Security," on which you must click "Allow access" with the Public networks checkbox checked.

![Windows local network access permission prompt](/img/platforms/windows-local-network.png)

- It's possible you've disallowed access previously and the prompt does not reappear. In this case you must locate the rule in Windows Defender Firewall and allow it.
    - To quickly test if this is the issue, you can temporarily disable the firewall, although it's much safer to follow the steps below to allow Unity access to your local network.

- To enable local access for the Unity editor on Windows:
    1. Search "Firewall," find "Windows Defender firewall," and look for "Inbound Rules."
    2. In the list of inbound rules, look for any titled "Unity Editor"
    3. Delete any rules titled "Unity Editor" that have a red circle with a cross through it
    
![Disallowed Windows firewall rule for Unity Editor](/img/platforms/windows-firewall-rule.png)

    4. Relaunch the Unity editor. Once launched (and with iOS selected as the platform) you should see the "Windows Security" asking for local network permission.

# VPN usage
- Since VTP communicates over the local network, usage of a VPN can prevent the SDK and iOS device from establishing connection depending on the VPN configuration. It is recommended to disable your VPN when using VTP.


---

## ドキュメント: index.md

```metadata
階層: /platforms/iOS/index.md
ディレクトリ: platforms\iOS
ファイル名: index.md
拡張子: .md
サイズ: 1.25 KB
最終更新: 2025-06-05T03:07:52.753Z
```

# iOS

:::warning
VRChat on iOS is currently in Beta. Please report issues on [Canny](https://feedback.vrchat.com/ios-mobile-beta). (Your VRChat account must have a trust rank of "New User" or higher.)
:::

This section contains helpful pages on creating VRChat Worlds and Avatars for iOS devices.

:::info
We're still working on our iOS documentation. Please check back often for changes or new pages.
:::

Guidelines that refer to "Android" or "Quest" apply to iOS, too. Use these guides to learn more about optimization and best practices.

## Guides
- [Setting up Unity for Creating iOS Content](/platforms/iOS/setting-up-unity-for-creating-ios-content) - How to enable iOS support in Unity.
- [Content Optimization](/platforms/android/quest-content-optimization)- How to optimize Worlds and Avatars for mobile devices.
- [Cross-Platform Setup](/platforms/android/cross-platform-setup) - How to develop Worlds and Avatars for multiple platforms.
- [Content Limitations](/platforms/android/quest-content-limitations) - Read about some of the limitations you'll need to consider when creating VRChat content for mobile platforms.
- [Mobile Best Practices](/platforms/android/android-best-practices) - Read helpful advice about World creation on mobile platforms.

---

## ドキュメント: setting-up-unity-for-creating-ios-content.md

```metadata
階層: /platforms/iOS/setting-up-unity-for-creating-ios-content.md
ディレクトリ: platforms\iOS
ファイル名: setting-up-unity-for-creating-ios-content.md
拡張子: .md
サイズ: 1.63 KB
最終更新: 2025-06-05T03:07:52.753Z
```

# Setting up Unity for Creating iOS Content
:::warning
VRChat on iOS is currently in Beta. Please report issues on [Canny](https://feedback.vrchat.com/ios-mobile-beta).
:::
This page explains how to publish your World or Avatar on iOS.

![Building for iOS instructions](/img/setting-up-unity-for-creating-ios-content-iOSContent-QuickStart.png)

If you're starting a brand new project, this won't take long at all. However, if you're converting a Windows or Android platform project to an iOS platform project, the SDK has to reimport your assets appropriately. This can take quite a while for larger projects.

For more details on best practices when working with cross-platform projects, read our documentation on [Cross-Platform Setup](/platforms/android/cross-platform-setup).


## 1. Open Your Build Settings

You can access your [Build Settings](https://docs.unity3d.com/Manual/BuildSettings.html) from Unity's main menu by going to "File" -> "Build Settings". Or you can use the keyboard shortcut `Ctrl` + `Shift` + `B`.

## 2. Switch Platform to iOS

:::caution Requires Additional Setup

If the iOS option isn't appearing, [you need to install Unity's iOS module.](https://docs.unity3d.com/hub/manual/AddModules.html)
You do not need a macOS device or XCode installed to create iOS content for VRChat! Adding the iOS module to your Unity editor is sufficient.

:::

Click "iOS" -> "Switch Platform". Unity will reimport your assets for iOS. This will take a while for larger projects with many assets.

## 3. Publish -> New Build

That's it! Your content is now available on iOS! Note that you must **upload** your content to test it.


---

# ドキュメント終了

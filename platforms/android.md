# android 統合ドキュメント

以下は自動収集された全ドキュメントのコンテンツです。各セクションの始めにメタデータが記載されています。



---

## ドキュメント: android-best-practices.md

```metadata
階層: /platforms/android/android-best-practices.md
ディレクトリ: platforms\android
ファイル名: android-best-practices.md
拡張子: .md
サイズ: 6.34 KB
最終更新: 2025-06-05T03:07:52.748Z
```

---
sidebar_position: 1
---
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Mobile Best Practices

Making your VRChat worlds cross-platform is a great way of allowing more players to enjoy it. Most VRChat players are on Android, so it’s worth creating an Android of your VRChat world.

However, mobile players and VR players will experience your world quite differently! In this guide, we’ll explain a few ways to make your mobile VRChat world more comfortable and enjoyable.
:::info

VRChat is available on Android as an [open beta](https://play.google.com/store/apps/details?id=com.vrchat.mobile.playstore).

:::
## 1. Publish Your VRChat World to Android

To make your world accessible to more users, publish it on all platforms supported by VRChat.

Any worlds uploaded to Android are available on Oculus Quest and Android mobile devices. If you’ve previously published a Quest version of your world, it’ll be available on phones and tablets as well!

To learn how to upload your world to Android, please refer to our [cross-platform setup documentation](https://creators.vrchat.com/platforms/android/cross-platform-setup).

:::note

If you see the term ‘Quest’ in reference to the VRChat SDK, it generally applies to Android, too.

For example: Existing tools like [EasyQuestSwitch](https://vcc.docs.vrchat.com/vpm/curated-community-packages#easyquestswitch) are great for cross-platform development. A Meta Quest isn’t required!

:::

## 2. Detect Mobile Players in Your World Automatically

When an Android player joins your world, you may want to tweak certain aspects of it. Players on an Android mobile device won’t have access to VR controllers, just like VR players won’t have access to a touchscreen.

Use [OnInputMethodChanged](/worlds/udon/input-events/#oninputmethodchanged) to detect whenever the player's input method has changed. For example:

<Tabs groupId="udon-compiler-language">
<TabItem value="graph" label="Udon Graph">

![A screenshot of an Udon Graph. The OnInputMethodChanged event is used to branch the execution based on whether the inputMethod parameter is Touch.](/img/worlds/OnInputMethodChanged.png)

</TabItem>
<TabItem value="cs" label="UdonSharp">

```cs
public override void OnInputMethodChanged(VRCInputMethod inputMethod)  
{  
    if (inputMethod == VRCInputMethod.Touch)  
    {  
        // Run code for touch input  
    }  
    else  
    {  
        // Run code for non-touch input  
    }  
}
```

</TabItem>
</Tabs>

You can also use GetLastUsedInputMethod to detect the input method at any time. For example:

<Tabs groupId="udon-compiler-language">
<TabItem value="graph" label="Udon Graph">

![A screenshot of an Udon Graph. GetLastUsedInputMethod is used to branch the execution based on whether the inputMethod parameter is Touch.](/img/worlds/GetLastUsedInputMethod.png)

</TabItem>
<TabItem value="cs" label="UdonSharp">

```cs
if (VRC.SDKBase.InputManager.GetLastUsedInputMethod() == VRCInputMethod.Touch)  
{  
    // Run code for touch input
}  
else  
{  
    // Run code for non-touch input  
}
```

</TabItem>
</Tabs>

## 3. Optimize Your World for Android

Android devices are usually less powerful than PCs. Read our [Quest Content Optimization guide](https://creators.vrchat.com/platforms/android/quest-content-optimization/) to optimize your world’s performance.

Good news: If your world runs OK on the Quest, it will probably run well on Android mobile devices. Phones and tablets usually have a lower resolution than VR headsets, and performance issues will cause less nausea than in VR.

You should still ensure that Quest players have an acceptable frame rate.

## 4. Test Your World on Android

Testing your world on an Android phone will help you improve the experience of mobile players. Even if your world is still in the early stages of development, consider how it will play on a mobile device.

When rapidly iterating on VRChat worlds, it may be convenient to test them in the Unity Editor or the Steam version of VRChat. A joystick gamepad can be an appropriate substitute for VRChat’s mobile control screen.

- On a touchscreen, _virtual_ joysticks control the player’s movement and camera.
- Touchscreens make screen-space interfaces easier to interact with other input devices.
- Playtest your world with mobile players

## 5. Design Your User Interfaces for Touchscreen

Phone screens are different from PC and VR devices. You may want to adjust your user interfaces to make them easier for mobile users to read and interact with.

- Make text easy to read. Use legible fonts, large text sizes, and big buttons.
- Remove unnecessary text. Reading large amounts of text is difficult on a mobile device.
- Localize text. Mobile users user a much wider spectrum of languages than on other platforms.
- Avoid relying on interactions that require a VR device or complex camera movements.
- Try using [screen-space user interfaces](https://docs.unity3d.com/Packages/com.unity.ugui@2.0/manual/UICanvas.html). On a touchscreen, they are easier to interact with than world space canvases. Consider adding a user interface that users can open from anywhere without needing to walk over to an in-world menu panel.
- Use the `OnScreenUpdateEvent` to get the orientation and resolution of the player's screen. This will trigger once when they first load into the world, and whenever the orientation of their device changes.


## In Conclusion: Give Players a Smooth User Experience

When a player joins your world, try to make the experience as smooth as possible. Try to apply the other tips mentioned in this article, and think of how you could use them in your world.

- **Don't** reupload your PC world without optimizing your materials.
- **Do** optimize your world for users on Android and enhance their experience.

- **Don't** force players to follow complex steps before enjoying your world.
- **Do** make your world enjoyable automatically, without any user input.

- **Don't** upload your world without testing it.
- **Do** listen to user feedback.

---

Following these steps will help make your world an excellent experience for mobile players.

Do you have any tips or tricks you’d like us to include in this article? Submit your feedback below, and we’ll do our best to help share your knowledge with the VRChat Community.


---

## ドキュメント: build-test-mobile.md

```metadata
階層: /platforms/android/build-test-mobile.md
ディレクトリ: platforms\android
ファイル名: build-test-mobile.md
拡張子: .md
サイズ: 10.33 KB
最終更新: 2025-06-05T03:07:52.749Z
```

---
sidebar_position: 0
---

import CurrentUnityVersion from '@site/src/components/UnityVersionedText.js';

# Build and Test for Android Mobile

You can test a VRChat world on your Android device without publishing it using the Build and Test feature. This document describes how you can set up and use this functionality to build worlds more quickly!

## Set Up For Build and Test

To use Build and Test for Android Mobile you need to set up your Unity Android Tools, your Project, and your Device.

### Set Up Unity Android Tools

You need the Android Build Support module installed for your VRChat-compatible Unity Editor to work on Android worlds in the first place. You may already have this, as the VRChat Creator Companion installs it by default. Follow the steps below to check this.

1. Open Unity Hub and navigate to "Installs" on the left.
2. Find the install for the Unity version that your project uses (the current recommended version is [<CurrentUnityVersion/>](/sdk/upgrade/current-unity-version/)). If your install lists "Android" below the Editor path, then you already have the module installed, and you can move on to Setting up your Project. Otherwise, continue to step 3.

![Unity Install with Android Module](/img/platforms/unity-hub-modules.png)

3. Click on the gear icon next to that install and click "Add modules".
4. Find "Android Build Support". If the box and check the box next to it if 
5. Click continue to install the Android build support module.

You should now be able to build worlds for Android.

### Set Up Your Project to Target Android

Open the SDK panel by selecting "VRChat SDK"->"Show Control Panel" and then select the Builder tab.

Find the "Select Platform" section and change it to "Android".

### Set Up Your Android Device to Test Worlds

Build and Test for Mobile makes use of the [Android Debug Bridge](https://developer.android.com/tools/adb) (ADB) to transfer built world files to your android device.

ADB uses USB debugging on an android device to communicate with the device. This allows ADB to send files and launch apps, among many other debug related features.

Enabling USB debugging also requires you to enter developer mode on your device.

:::caution Warning

USB debugging allows your computer to perform many potentially dangerous operations without any notification. You should only use it with applications that you trust. 

The VRChat SDK uses USB debugging to send files to your device, open the VRChat app, and launch your test world.

:::

#### 📱 Enabling Developer Mode on Android Phones and Tablets

To enable developer mode on your Android phone or tablet you must do the following:

1. Ensure that your phone is plugged in to your computer with a USB cable capable of data transfer.
2. Enable developer mode by going to "Settings"->"About Phone" and scrolling down to the "Build number" at the bottom. Tap the build number 7 times to enable Developer mode.
3. Now that developer mode is enable you can see more developer options. Go to "Settings"->"System"->"Developer Options" and locate the "USB Debugging" checkbox. Check "USB Debugging".

#### 🥽 Enabling Developer Mode on a Meta Quest Headset

To build and test on your Meta Quest device, you also need to enable developer mode and allow USB debugging.

You must use the [Meta Quest Developer Hub](https://developers.meta.com/horizon/documentation/unity/ts-odh/) to switch your Meta Quest headset into developer mode.

To learn how to set up developer mode on your headset, read [Meta's instructions](https://developers.meta.com/horizon/documentation/unity/unity-quickstart-mqdh/#connect-headset-to-meta-quest-developer-hub).

## Use Build and Test

With developer mode enabled, USB debugging enabled, and your Android device plugged in, you should now be able to build and test.

### First Launch

* Ensure that you have launched the app at least once to allow app directories to be generated before trying to test a world, and then close VRChat. 
* The VRChat appplication must be closed before launching into a test world for the first time.
* Your Android device must remain unlocked for ADB to be able to launch the VRChat app into a test world.

With everything above set up, and your Project in Android mode, press the "Build and Test New Build" button to test the world on your device.

After the world is built you should see the VRChat app automatically open. If you are not already logged into the app you will be sent directly to the login screen. After logging in you will be sent directly to the test world.

If you were already logged in, you should be sent directly to the test world after the client is done loading. The play button at the bottom indicates the loading status of the client, and once it's done loading you should be sent to the world after a short delay.

When making future changes to your world, press "Build and Test New Build" again. Your world will be rebuilt, sent to your device, and reloaded without any further action.

## Tools & Troubleshooting

This section describes how to fix some common issues as well as some tools that can help in your mobile development journey.

### Troubleshooting

If you're a developer who uses debugging mode for other reasons, please make sure to unplug or disable USB debugging on all other devices except the Android device that you would like to build and test on.

For Android phones and tablets, if USB debugging is enabled but your computer is unable to see your phone/tablet, the best way to troubleshoot this is to completely restart the USB debugging process.
   1. Unplug the Android device.
   2. Disable developer mode and USB debugging.
   3. Plug in the Android device.
   4. Enable developer mode by tapping "About Phone"->"Build Number" 7 times.
   5. Enable USB debugging in the "System"->"Developer Options" section.
   6. Attempt to Build and Test again.

If for some reason your VRChat app does not have the package name `com.vrchat.mobile.playstore` you can modify which package name the SDK looks for in the project settings. This can be found at "VRChat"->"SDK"->"Android App Package Name"

This may be useful if you are trying to build and test on a beta build of the VRChat app, or for some reason you are not using the live version from the Google Play store. Keep this as `com.vrchat.mobile.playstore` unless you have good reason to change it.

### Useful Device Mirroring Tool

You may want to be able to view your phone from a window on your desktop instead of having to look down at your phone every time. There is a useful open source tool called [scrcpy](https://github.com/Genymobile/scrcpy) which can facilitate this. 

[scrcpy](https://github.com/Genymobile/scrcpy) makes use of ADB to display a screen capture of your device in a small window on your desktop. You can use this to interact with your phone using a mouse instead of having to use your phone.

To use this tool, download the [latest release](https://github.com/Genymobile/scrcpy/releases) for your platform (For example, download the `scrcpy-win64` zip file on Windows). This will give you a zipped folder with all the required files to run `scrcpy.exe`. Unzip this somewhere convenient.

You can run `scrcpy.exe` either by double clicking it or running `scrcpy.exe` in a command prompt in that folder. If you've successfully set up USB debugging, this will display a window mirroring what you see on your device.

Please note that ADB is unable to record the lock screen so the preview window will be completely black while trying to unlock the phone. You can unlock your phone physically and continue interacting through the mirror window.

Also it is recommended to launch scrcpy *after* launching Unity. This is because scrcpy will launch it's own ADB server if there isn't one running already, but Unity will kill any existing ADB servers when it launches, which will close your device mirror window.

You may also find it useful to enable the "Stay Awake" in "System"->"Developer Options" to avoid having to tap the device to keep it awake and prevent locking. This keeps the device awake forever as long as it is plugged in and charging.

### Build and Test Over Wi-Fi

You can also build and test wirelessly. This may be more convenient for some setups, but it also requires some command line interaction in order to connect, so this is not recommended over USB debugging in general.

In order to enable wireless debugging you will need to interact with ADB directly on the command line.

1. First make sure Unity is open so ADB will use Unity's ADB server process
2. Navigate to the Unity install folder for the version of Unity you are using for your project.
    * This can be found by opening Unity Hub, click "Installs" on the left, click the gear next to your install, click "Show in Explorer".
    * `adb.exe` can be found in Data/PlaybackEngines/AndroidPlayer/SDK/platform-tools
    * Open a command prompt in this folder by launching command prompt and navigating to the folder using `cd [your unity install path]/Data/PlaybackEngines/AndroidPlayer/SDK/platform-tools`.

Now you can use ADB commands. If you still have USB debugging enabled, a good command to test if things are working is to use `adb devices -l` which should list all debugging-enabled devices. You can also try `adb shell ls` to print out some of the top level folders on the device, again requiring debugging to be enabled.

Now that we've located ADB, what about that fancy Wi-Fi debugging?
1. Disable USB debugging and just to be sure unplug the device
2. Enable System->Developer Options->Wireless Debugging.
3. Tap Wireless Debugging to find more options.
4. Tap "Pair device with pairing code" and you should get a 6 digit code to pair with, along with an IP address and port to use.
5. Run `adb pair [ip]:[port]` 
    * For example `adb pair 127.0.0.1:1234` if the pairing screen had IP address `127.0.0.1` and port `1234`
    * Note that the port will be randomized every time, so you will have to come back to this step every time you want to debug over Wi-Fi.
6. Enter the pairing code when asked.
7. Now type `adb connect [ip]:[port]` to connect to the device

You should be able to use ADB over Wi-Fi now.

Try running `adb devices -l` to see if it's working. You should see your device in the list of devices now.

You can now go back to the SDK and click "Build & Test New Build" to test a world over Wi-Fi.


---

## ドキュメント: cross-platform-setup.md

```metadata
階層: /platforms/android/cross-platform-setup.md
ディレクトリ: platforms\android
ファイル名: cross-platform-setup.md
拡張子: .md
サイズ: 6.78 KB
最終更新: 2025-06-05T03:07:52.750Z
```

---
sidebar_position: 0
---

# Cross-Platform Setup

Setting up a cross-platform world or avatar is actually quite straightforward! In short, all you have to do is switch Unity to the desired target platform and build the content.

If you need a bit more detail on how to do this properly, here's a short guide.

## Setting up for Android

First, you'll want to switch your project to target Android to perform first-time setup and ensure your content can be built for the platform.

1. Open the SDK Control Panel and switch to the Builder tab
2. In the "Build" section select the platforms dropdown. Uncheck "Windows" and check "Android"
3. In the popup that appears, click "Switch" to confirm the change
4. Inspect the validation messages that appear in the SDK control panel to ensure your content is compatible with Android

## Fine-tuning and Optimization

Now that you've got your project ready, you'll need to start optimizing. **You cannot skip this.** Modern android devices are pretty powerful, but not nearly as powerful as a typical VR-ready PC. You'll need to check out our [Quest Content Optimization](/platforms/android/quest-content-optimization) page to see what you need to do. For worlds this means baking lighting, lowering geometry complexity, avoiding transparency, and lowering texture resolution. For avatars, this means removal of excess components, excess bones, lowering geometry complexity, avoiding transparency, and reducing texture size.

This will take a while, and is expected to be challenging. Optimizing for mobile hardware is difficult! Thankfully, there's a ton of resources out there, and even a cursory YouTube search for "optimizing Unity for mobile" reveals a ton of good content.

You can also check out some of our documentation on optimizing content for Oculus Quest.

- [Quest Content Optimization](/platforms/android/quest-content-optimization)
- [Quest Content Limitations](/platforms/android/quest-content-limitations)
- [Avatar Performance Ranking System](/avatars/avatar-performance-ranking-system)

We also highly recommend using tools like EasyQuestSwitch (available as a package in the VRChat Creator Companion) to adjust the content per-platform.

:::info

If you have multiple version of your avatar available for different platforms. E.g. a dedicated PC version and a Quest version - you can use the [Per-Platform Override](/avatars/per-platform-avatar-overrides) functionality in the SDK to use them.

:::

## Uploading Content

Once your world or avatar is ready, you can upload! This upload process is identical to the VRChat PC upload process, although the SDK will be a lot more aggressive with warning you about performance issues.

If you want to upload your content for all the platforms at once - you can select multiple targets in the "Platforms" dropdown of the Build section in the SDK Control Panel.

**You need to upload your world or avatar to the same blueprint ID as you have for the VRChat PC version of the content.** Blueprint ID is defined by a [Pipeline Manager component](/sdk/vrcpipelinemanager) on a Game Object, which usually accompanies a VRC Scene Descriptor, typically on your VRCWorld prefab. Press the "Detach" button to edit the blueprint ID, and paste in the ID from the first version that you uploaded (you can also find this in the "Content Manager" tab of the VRChat SDK control panel).

The version you're uploading depends on the originating project's build target. If you're on a project set up for Android, it'll upload for Quest. If the build target is Windows, then you're uploading a PC version. That's basically it-- once you've uploaded, any client that views your content will talk to our servers a bit like this:

>"Hey, I'm a Meta Quest and I want this content."
>"Ok, here's a VRChat Android version."

>"Hey I'm a VRChat PC user and I want this content."
>"Ok, here's the VRChat PC version."

>"Hey, I'm a VRChat iOS user and I want this content."
>"OK, here's the VRChat iOS version."

:::info

VRChat is experimenting with content fallback systems for iOS. If your world is available on Android but not iOS, VRChat attempts to load the Android version on iOS.

This tends to work well for content that primarily uses the Standard Lite shader as outlined by [Quest Content Limitations](/platforms/android/quest-content-limitations). Uploading an iOS version of your content using the SDK is recommended for best results.

:::

If an avatar isn't available for the platform you're on, you'll see an [impostor avatar](https://creators.vrchat.com/avatars/avatar-impostors/) instead.

If a world isn't available for the platform you're on, you'll be unable to enter portals to that world or join it through the UI.

**However**, if you join a world that has both Android and PC versions, and the people in the instance have both Android and PC versions of their avatars, you'll view the world appropriately for your platform and be able to hang out with everyone, with no issues!

:::danger Armatures MUST be identical!

For avatars to work properly cross-platform, the armature path MUST be identical between the PC and Quest versions to essential bones like the head, hands, and feet. Additionally, the scale and rotation of the "root bone" (the first bone in the hierarchy) MUST be identical between versions.

:::

The rigging (armatures) between Android and PC avatars must be mostly identical. If you want to remove non-essential bones like skirt/hair/etc bones for the Android version, that's fine-- but do not change the base structure of the armature layout. Doing so will result in strange behavior when viewed across platforms.

Most importantly, the "root bone" (as in, Hips) should be the first bone in the hierarchy after the Armature GameObject. To be specific, as long as the setup (scale, rotation) of the "root bones" is identical, you should have no problems.

## Tips

- Your VRChat PC avatar can have all kinds of bells and whistles that the VRChat Android avatar can't have. Depending on the platform, users will see whatever version is appropriate for their client.
- You can be a bit creative with this as well-- you could have a high-poly world or avatar for PC users, and then a low-fi (but still stylish) version for Quest users.
- Define a set of box colliders or a low-poly mesh collider for both the PC and Quest versions of the world and use that instead of a mesh collider. Parent the colliders to an empty GameObject at a specific coordinate, and if you update one project, you can copy/paste that object to the other project easily. That way you'll never see users on different platforms "float", and you won't have issues with expensive and complex high vertex count mesh colliders.
- Remember, avoid transparency at all costs! It is quite expensive.
  - As an aside, yes, "alpha cutout" counts as transparency.


---

## ドキュメント: index.md

```metadata
階層: /platforms/android/index.md
ディレクトリ: platforms\android
ファイル名: index.md
拡張子: .md
サイズ: 860 B
最終更新: 2025-06-05T03:07:52.750Z
```

# Android

This page contains helpful pages on creating VRChat worlds and avatars for Quest or Android mobile devices.

## Guides
- [Setting up Unity for Creating Quest Content](/platforms/android/setting-up-unity-for-creating-quest-content) - How to enable Android support in Unity.
- [Quest Content Optimization](/platforms/android/quest-content-optimization)- How to optimize worlds and avatars for mobile devices.
- [Cross-Platform Setup](/platforms/android/cross-platform-setup) - How to develop worlds and avatars for multiple platforms.
- [Quest Content Limitations](/platforms/android/quest-content-limitations) - Read about some of the limitations you'll need to keep in mind while creating VRChat content for Android.
- [Mobile Best Practices](/platforms/android/android-best-practices) - Read helpful advice about world creation on Android.

---

## ドキュメント: quest-content-limitations.md

```metadata
階層: /platforms/android/quest-content-limitations.md
ディレクトリ: platforms\android
ファイル名: quest-content-limitations.md
拡張子: .md
サイズ: 7.88 KB
最終更新: 2025-06-05T03:07:52.750Z
```

---
title: "Quest Content Limitations"
slug: "quest-content-limitations"
hidden: false
createdAt: "2019-05-15T01:40:38.749Z"
updatedAt: "2022-07-04T09:34:33.253Z"
---
This page will describe various limits in place for the Oculus Quest version of VRChat. These limitations are in place in the interest of performance, user safety, and discouraging malicious behavior.

Find more information about limited components on our [Quest Content Optimization](/platforms/android/quest-content-optimization) page.
## Avatar-Specific Limitations
Although the current version of VRChat does not implement a hard limit, **we may implement a hard limit for avatars based on triangle count, material counts, mesh counts, and other qualities in the future.** Please keep our recommendations in mind as described in [Quest Content Optimization](/platforms/android/quest-content-optimization).

Currently, if you upload an avatar or avatar world that features avatars exceeding our recommendations, that world or avatar may be removed from public access.
## Shaders
VRChat on Quest only permits the shaders provided with the latest SDK on avatars. The shaders are listed below with a short description and their inputs. This list may change, and we'll announce in our patch notes when new shaders are available.

All of the shaders listed below are under `VRChat/Mobile` in the shader selection dialog.

**For performance reasons, make sure you always enable "Enable GPU Instancing" on your materials.**

| Shader Name                | Shader Description |
| :-- | :-- |
| Toon Standard              | The most powerful and configurable Toon-style shader available for VRChat on mobile platforms. It is recommended for most cases where Standard Lite is not the right choice. It supports features such as: <ul> <li>Detail and Emission maps from UV0 or UV1.</li> <li>Custom shadow ramps, configurable simplified specular lighting, rim lighting and Matcaps.</li> <li>Maskable hue-shift for albedo, emission and detail.</li> <li>Normal maps including tilable and maskable detail normals.</li> <li>Mask textures with configurable color channels for combining multiple maps into different packed formats.</li> </ul> Note: The "Outline" version is only supported on PC, mobile devices will fall back to the non-outline variant automatically. You can use this shader on PC avatars as well, although it does not support realtime shadows at this time. |
| Standard Lite              | A "Lite" version of Unity Standard that requires less VRAM. <ul> <li> Supports the channel mappings of Unity's Standard Metallic setup, except Alpha and Parallax. </li> <li> The diffuse texture is tinted by the mesh's vertex colors. </li> <li> You can optimize different channels by packing them into the same texture: <ul> <li> Texture 1: Albedo (RGB) and Detail Mask(A) </li> <li> Texture 2: Metallic (R), Occlusion (G), and Smoothness (A) </li> </ul> </li> <li> Not recommended on PC because it does not support real-time lighting. </li> </ul> |
| Bumped Diffuse             | Diffuse but with a normal map. The diffuse texture is tinted by the vertex colours.                                                                                                                                                                                 |
| Bumped Mapped Specular     | Diffuse, but with a specular map (shininess) on the alpha channel. The diffuse texture is tinted by the vertex colours. Normal map also supported.                                                                                                                |
| Diffuse                    | Just diffuse! The diffuse texture is tinted by the vertex colours.                                                                                                                                                                                                 |
| Matcap Lit                 | Diffuse, but with a matcap input. Can be used to simulate a shiny metal surface. The diffuse texture is tinted by the vertex colours.                                                                                                                               |
| Toon Lit                   | Provides toon-like shading and shadows. Should be used on cartoon-like characters with flat colors. The diffuse texture is tinted by the vertex colours.                                                                                                          |
| Particles/Additive         | Should be used on particles. Blends using Additive mode.                                                                                                                                                                                                             |
| Particles/Multiply         | Should be used on particles. Blends using Multiply mode.                                                                                                                                                                                                             |
| Lightmapped (Only for worlds) | A basic diffuse shader that supports lightmapping. This shader is only meant for use on worlds. It cannot be used on avatars. It does not support real-time lighting.                                                                                          |
| Skybox (Only for worlds)      | This shader is an optimized skybox shader, meant for use in worlds.                                                                                                                                                                                                    |
| Supersampled UI (Only for worlds) | An unlit shader which is is supersampled at the texture sample phase. Use with mipmapping to make in-world UI text crisp without being grainy or distracting.

## Components

The following components are not supported on Android or Quest and will not work. This list may change. We'll note in the Patch Notes and updated documentation when these change.

| Shader Name                | Shader Description |
| :-- | :-- |
| Dynamic Bones              | Completely disabled on Android and Quest. Use [PhysBones](/avatars/avatar-dynamics/physbones) instead!! |
| Cloth                      | Completely disabled on Android and Quest. |
| Cameras                    | Completely disabled for avatars on Android and Quest. Permitted for use in Worlds. Be careful with overuse. |
| Lights                     | Completely disabled for avatars on Android and Quest. |
| Video Players | Works with some limitations. Read more in [Video Players](/worlds/udon/video-players). |
| Post-Processing | Post processing systems are disabled completely on Android and Quest. The GPU is not designed to handle these effects very well. |
| Audio Sources | Audio sources are disabled completely for avatars on Android and Quest. Audio sources consume a significant amount of CPU resources and voices have priority. |
| Physics Objects | Rigidbodies, colliders, and joints are disabled completely for avatars on Android and Quest. <br /> They are permitted in worlds, but you should be careful not to go overboard with them. |
| Particle Systems | Particles are limited heavily on avatars for Android and Quest, with settings mirroring the [Avatar Particle System Limits](https://docs.vrchat.com/docs/avatar-particle-system-limits) on PC. |
| Constraints | Unity constraints are disabled completely for avatars on Android and Quest due to complex performance issues. Use [VRChat Constraints](/avatars/avatar-dynamics/constraints) instead.<br /><br />Permitted for use in Worlds. Be careful with overuse, as they impact performance more than previously thought, especially with the limited resources of Quest and mobile devices. |
| FinalIK | Custom FinalIK components are completely disabled for avatars on Android and Quest.<br />FinalIK components are an unbounded source of resource usage. We do not currently plan to enable them on these platforms. |


---

## ドキュメント: quest-content-optimization.md

```metadata
階層: /platforms/android/quest-content-optimization.md
ディレクトリ: platforms\android
ファイル名: quest-content-optimization.md
拡張子: .md
サイズ: 11.76 KB
最終更新: 2025-06-05T03:07:52.752Z
```

---
title: "Quest Content Optimization"
slug: "quest-content-optimization"
hidden: false
createdAt: "2019-04-08T23:52:28.397Z"
updatedAt: "2023-02-03T01:02:49.409Z"
---
Creating content for VRChat Quest is a challenge-- you have to create attractive, compelling content all the while keeping the content optimized for a mobile device. These are the same challenges that game developers must deal with while building for mobile. 

Here, we'll give you some general guidelines of what you need to keep in mind while building content for VRChat Quest.

The items below will apply to both avatars and worlds unless otherwise noted.

Unity has a guide for [Optimizing your VR/AR Experiences ](https://learn.unity.com/tutorial/optimizing-your-vr-ar-experiences) which has quite a lot of good points.

There's also this [excellent video on optimizing your VR content by Lucas Rizzotto](https://www.youtube.com/watch?v=w0n4fuC4fNU)! It is very well done and covers a lot of what we cover here. **This video was not created by VRChat or for VRChat specifically, and as a fair warning, contains some harsh language.** A lot of the items in this post are covered in this video.

As a final note, all items on this list are subject to change. In other words, we're not quite done nailing down restrictions and recommendations, so keep that in mind.
## Limit Enforcement
The Oculus Quest has several hard (and soft) limits for content on avatars. Check out [Quest Content Limitations](/platforms/android/quest-content-limitations) to find out more, as well as our page [Avatar Performance Ranking System](/avatars/avatar-performance-ranking-system) for some more details on how blocking works.

If you upload an avatar or avatar world that features avatars greatly exceeding our recommendations, that world or avatar may be removed from public access.
## Unity Profiler
We strongly, strongly recommend that you check out the [Unity Profiler](https://docs.unity3d.com/Manual/Profiler.html). Using the profiler, you can quantify precise values for  various performance metrics for your world or avatar. Of particular interest is probably the number of draw calls in a scene, or the proportional amount of frame time a component uses. 

Of course, the profiler on your powerful PC won't represent how a profiler on the Quest might look, but you can still see that X component is using a ton of frame time versus rendering, or etc. Its all relative!

There's lots of tutorials on how to use the Unity Profiler out there, including two from Unity: [Profiler Overview for Beginners](https://unity3d.com/learn/tutorials/topics/interface-essentials/profiler-overview-beginners) and the [intermediate Introduction to the Profiler](https://unity3d.com/learn/tutorials/topics/interface-essentials/introduction-profiler). These tutorials were made for older versions of Unity, but still cover the basic concepts quite well.
## File Size
You've only got a limited amount of memory on mobile platforms, and keeping that in mind is extremely important. You can see the size of your assets once you've built the content (press "Build & Publish" in the SDK) and search your Editor log for "statistics". The pre-compressed size is what you're looking for.

As a rule of thumb, avoid large (>1k) textures. They are the primary culprit of high memory usage. Utilization of vertex colors and flat colors can help greatly with reducing texture size.

Please note that Crunch compression does _not_ help with in-memory size! Crunch compression only helps with download size. Your content package should be within the limits without Crunch.

**Worlds**

You cannot upload or access worlds that exceed 100MB in size after build-time compression for VRChat Quest.

**Avatars**

You should be aiming for a maximum of 5-8 MB. You cannot upload or wear/view avatars that exceed 10MB in size after build-time compression for VRChat Quest.
## Triangle Count
Keeping triangle count low is very important on mobile platforms. Although the Quest is quite powerful for a mobile headset, its hardware does have limits. Keeping an eye on your polygon count is very important to keep performance high.

These recommendations are technically enforced via our [Avatar Performance Ranking System](/avatars/avatar-performance-ranking-system).

**Worlds**

While building worlds, you should try to keep triangle count low. You want to leave room for the user's avatars as well. **We recommend that you budget approximately 50,000 triangles for your world in total.**

**Avatars**

The same general rules apply for avatars that apply for worlds. Keep in mind that you may have 10 or more users in the same room, so you'll want to budget your triangle usage pretty heavily. **We recommend that you aim for under 10,000 triangles for your avatar.**

This is a challenge for avatar authors that prefer to import characters from various sources rather than create an avatar themselves. Decimation down to this level can be destructive, and you may need to look into techniques like retopologizing geometry to keep your polygon count low.
## Mesh Count
This applies to both worlds and avatars.

No matter what tool you use to do it, you should limit the number of meshes you use in your content. For static objects in worlds, this isn't so important (due to the need for occlusion culling) but for avatars, it is exceedingly important.

You should only ever have one Skinned Mesh Renderer on your avatar. Any accessories or additions to your avatar should be done in 3D editing software like Blender, and merged into the original mesh. Any animations or movement should be handled via shape keys or bones.

A hard Mesh Count limit will be established in the near future for VRChat Quest avatars.

For worlds, you should think in terms of "objects" in the world. A set of pots on the ground could be a single object, but you probably wouldn't want to merge the set of pots into the ground mesh. Otherwise, you might run into various optimization issues as well as difficulty with editing the world later on.
## Materials
Reducing material count is important for both avatars and worlds. Additional materials creates additional submeshes, which costs draw calls. Reducing the number of draw calls necessary to render your viewport is very important.

**Worlds**

You should aim to have the minimal possible material count for your world. That being said, you can be a little less careful with the world than you are with avatars. Its best to think of your world as a collection of objects, and combine materials accordingly.

For example, if you have a beach scene, a chair/umbrella/blanket set should probably be a single material on a single texture atlas. A set of rocks a little bit farther down the beach would be another material and texture. That way, you can separate out the object for occlusion culling purposes.

Getting too aggressive with combining materials and atlasing in worlds results in some non-optimal behavior when Unity does batching and its own runtime optimization.

**Avatars**

You should be aiming for 1 material on your avatar, although having 2 in cases where you need a different shader variant may be permissible. Atlasing textures is essential.

A hard Material limit will be established in the near future for VRChat Quest avatars.

**Avatars and Worlds**

You should be enabling GPU Instancing on all of your materials. Although the actual use case of this is more complex and technical, it is best just to turn it on.
## Textures
Concerns for texture size apply evenly across both avatars and worlds, but keep in mind that avatar texture size should be reduced, as you'll have multiple avatars in a single instance (but only one world).

Keeping texture size low is important. You should aim for using 1k (1024x1024) resolution textures at maximum. You should also create efficiently packed atlases, allowing for more texture resolution in the same size.

Avatars cannot exceed 10MB in size after compression, and worlds cannot exceed 100MB after compression. Since most of this is usually texture data, you should keep your textures small and compress them. 

Consider using Crunch compression, but keep in mind that this may break your avatars later on if a new Unity version employs an incompatible version of Crunch.
## Lighting
This section only applies for worlds.

Baking lighting for your world is **essential**. It is not unreasonable to write off real-time lights completely, as they are very expensive. Make extensive use of baked lighting and light probes. Keep your lightmap resolution low. Even with low lightmap resolution, lighting can look very good.
## Occlusion Culling
This section only applies for worlds.

Baking occlusion culling is exceedingly important. Doing so will allow the hardware to only render what it needs to, and ignore what you can't see. Setting up occlusion culling doesn't take long at all.

This is also the reason you don't want to be too aggressive in merging meshes in worlds-- if you've got some objects like a building set on some ground, you probably don't want to merge the building mesh into the ground mesh so you can cull out the building.
## Bone Count
Keeping bone count low is important to keep the cost of skinning calls low. If a bone isn't animated by an animation or by the rig, you should merge its weights into its parent and delete the original bone. Tools like [Cat's Blender Plugin](https://github.com/michaeldegroot/cats-blender-plugin) make this exceedingly easy.

Since Dynamic Bones is disabled on VRChat Quest, this means that there's no need for extra bones for dynamic bits. You'll want to merge the weights for those bones into their parents.

A hard Bone limit will be established in the near future for VRChat Quest avatars.
:::caution Matching Rigs

Ensure that the basic bone layout and hierarchy of your rigs are identical, including scale, rotation, and position. This especially applies to your root (usually hip) bone. Having a mismatch can result in strange behavior when viewing content cross-platform.
:::

## Shaders
**Avatars**

Shaders are restricted for avatars in VRChat Quest, and you can only use the VRChat Mobile shaders included in the VRChat SDK.

You can read about these variants on our [Quest Content Limitations](/platforms/android/quest-content-limitations) page.

If you don't have a normal map for your avatar, don't use Bumped variants. It won't do anything for you, and you'll incur a little bit of a performance cost. Same for Specular.

**Worlds**

Shaders are not restricted for worlds in VRChat Quest. However, you should be *extremely careful* when writing and using custom shaders. Aim for performance above all else. If you're looking for a highly optimized basic world shader, use `Mobile/VRChat/Lightmapped` and bake your lighting.

You should avoid needing transparency completely. Alpha fill rate is a significant performance sink for mobile GPUs, so design around not having transparency whenever possible.
## Other Components

### Cloth

Cloth components are disabled completely in VRChat Quest.

### Cameras

Camera components are disabled on avatars in VRChat Quest.

They are permitted in worlds, but you should be careful not to go overboard with them.

### Lights

Lights are disabled completely on avatars in VRChat Quest.

### Post Processing (v1 and v2)

Post processing systems are disabled completely in VRChat Quest.

### Audio Sources

Audio sources are disabled completely on avatars in VRChat Quest.

Audio sources are restricted in worlds in VRChat Quest.

### Rigidbodies, Colliders, and Joints

Rigidbodies, colliders, and joints are disabled completely on avatars in VRChat Quest.

They are permitted in worlds, but you should be careful not to go overboard with them.

### Particles

Particles are limited heavily on avatars in VRChat Quest.

Numbers for limits pending.

---

## ドキュメント: setting-up-unity-for-creating-quest-content.md

```metadata
階層: /platforms/android/setting-up-unity-for-creating-quest-content.md
ディレクトリ: platforms\android
ファイル名: setting-up-unity-for-creating-quest-content.md
拡張子: .md
サイズ: 1.64 KB
最終更新: 2025-06-05T03:07:52.752Z
```

---
title: "Setting up Unity for Creating Quest Content"
slug: "setting-up-unity-for-creating-quest-content"
hidden: false
createdAt: "2019-04-10T00:53:35.417Z"
updatedAt: "2019-10-28T19:23:09.636Z"
sidebar_position: -1
---
![Building for Mobile instructions](/img/setting-up-unity-for-creating-quest-content-1ac8b19-VRChat_QuestContent_QuickStart.png)

If you're starting a brand new project, this won't take long at all. However, if you're converting a Windows platform project to an Android platform project, you will have to convert your assets appropriately. This can take quite a while for larger projects.

For more details on best practices when working with dual-platform projects, read our documentation on [Cross-Platform Setup](/platforms/android/cross-platform-setup).

## 3 Steps to get your world on VRChat for Quest

### 1. Open Your Build Settings

You can access your [Build Settings](https://docs.unity3d.com/Manual/BuildSettings.html) from Unity's main menu by going to "File" -> "Build Settings". Or you can use the keyboard shortcut `Ctrl` + `Shift` + `B`.

### 2. Switch Platform to Android

:::caution Requires Additional Setup

If the Android option isn't appearing, [you need to install Unity's Android SDK.](https://docs.unity3d.com/Manual/android-sdksetup.html)

:::

Click "Android" -> "Switch Platform". Unity will reimport your assets for Android. This will take a while for larger projects with many assets.

### 3. Publish -> New Build

That's it! Your content is now available on Quest! Note that you must **upload** your content to test it. You can't use the SDK's "Build and Test" feature for Android content.

---

# ドキュメント終了

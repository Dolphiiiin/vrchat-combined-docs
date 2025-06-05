# udon 統合ドキュメント

以下は自動収集された全ドキュメントのコンテンツです。各セクションの始めにメタデータが記載されています。



---

## ドキュメント: ai-navigation.md

```metadata
階層: /worlds/udon/ai-navigation.md
ディレクトリ: worlds\udon
ファイル名: ai-navigation.md
拡張子: .md
サイズ: 2.91 KB
最終更新: 2025-06-05T03:07:52.803Z
```

# AI Navigation

AI Navigation allows you to create non-player characters (NPCs) that can intelligently move around the game world, using navigation meshes that are created automatically from your Scene geometry. Dynamic obstacles allow you to alter the navigation of the characters at runtime, while offmesh links let you build specific actions like opening doors or jumping over gaps or down from a ledge.

:::tip

AI Navigation is a built-in Unity feature. Read [Unity's official documentation](https://docs.unity3d.com/Packages/com.unity.ai.navigation@1.1/manual/index.html) to understand important concepts on how to use it.

:::

## Differences from Unity 2019 Navigation Features

Compared to previous versions, Unity 2022 has changed how AI Navigation works in Unity:

* The system is now a separate package maintained by Unity rather than integrated into the core.
* Runtime generation! You can now create and update your navigation meshes at runtime in the VRChat client. This was only possible in the Unity Editor before.
* [NavMesh Links](https://docs.unity3d.com/Packages/com.unity.ai.navigation@1.1/manual/NavMeshLink.html) and [Off-Mesh Links](https://docs.unity3d.com/Packages/com.unity.ai.navigation@1.1/manual/CreateOffMeshLink.html) are simpler to use.
* NavMesh generation is generally improved and more robust.
* Obstacles can be dynamic now.
* You can use multiple [NavMesh Surfaces](https://docs.unity3d.com/Packages/com.unity.ai.navigation@1.1/manual/NavMeshSurface.html) within your world.
* NavMesh Surfaces, [Modifiers](https://docs.unity3d.com/Packages/com.unity.ai.navigation@1.1/manual/NavMeshModifier.html) and [Modifier Volumes](https://docs.unity3d.com/Packages/com.unity.ai.navigation@1.1/manual/NavMeshModifierVolume.html) provide more granular control over the system.
* Area costs are now properly exported into the world data and restored when joining a world.

## Limitations in VRChat

Some of the features of AI Navigation cannot be used in the VRChat client or are not currently useful:

### Custom Agent Types

[Unity lets you declare different types of agents](https://docs.unity3d.com/Packages/com.unity.ai.navigation@1.1/manual/NavMeshAgent.html), which can have different values for Radius, Height, Step Height, Max Slope, and more. This info is bundled into the world data, but VRChat does not currently have a method for retrieving and applying custom agent types. This feature is being considered, but you need to use the default agent type for now.

### Various Methods and Properties

* `NavMeshLink.agentTypeID` - runtime baking does not work for custom agents, so this is not very useful.
* `NavMeshSurface.CollectObjects` - Enum arrays are not available in Udon.
* `NavMeshSurface.agentTypeID` - runtime baking will not work with custom agents, so this is not useful.
* `NavMeshSurface.UpdateNavMesh()` - returns an AsyncOperation, which is not usable. The method itself should still work.


---

## ドキュメント: animation-events.md

```metadata
階層: /worlds/udon/animation-events.md
ディレクトリ: worlds\udon
ファイル名: animation-events.md
拡張子: .md
サイズ: 783 B
最終更新: 2025-06-05T03:07:52.804Z
```

import UnityVersionedLink from '@site/src/components/UnityVersionedLink.js';

# Animation Events

You can <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/script-AnimationWindowEvent.html">call events from your Animations</UnityVersionedLink>. This page lists the events that are allowlisted for use in this way. If an event is not in this allow list, it will not be called when your world runs in VRChat.

![animation-events-af04d2a-AnimationEventInspector.png](/img/worlds/animation-events-af04d2a-AnimationEventInspector.png)

## Allowed Animation Events
* RunProgram
* SendCustomEvent
* Play
* Pause
* Stop
* PlayInFixedTime
* Rebind
* SetBool
* SetFloat
* SetInteger
* SetTrigger
* ResetTrigger
* SetActive

---

## ドキュメント: avatar-events.md

```metadata
階層: /worlds/udon/avatar-events.md
ディレクトリ: worlds\udon
ファイル名: avatar-events.md
拡張子: .md
サイズ: 2.03 KB
最終更新: 2025-06-05T03:07:52.805Z
```

# Avatar Events

These events allow Udon to react to changes regarding player avatars.

## OnAvatarChanged

Returns `VRCPlayerApi` object for the instigating player. Called when a player's avatar has finished loading.

### Persisted eye heights
Players will sync any variance from their prefab eye height after their avatar loads, triggering an `OnAvatarEyeHeightChanged` event.

If the `VRCPlayerApi` object is for the local player, retrieving its eye height will return the prefab height during this event.

If the `VRCPlayerApi` object is for a remote player, be aware that the remote player may not have synced their new eye height yet, and you should not rely on the returned value in this case.

## OnAvatarEyeHeightChanged

Returns a `VRCPlayerApi` object for the instigating player and a `float` describing their previous or previously synced eye height in meters. Called when a player has their eye height change via switching to another avatar or via the [avatar scaling system](/worlds/udon/players/player-avatar-scaling).

### First avatar load
When a local or remote user joins a world, the first previous eye height value received for that user may be `0`.

### Avatar changes, remote players, and event ordering
When a **local** user changes their avatar and applies a persisted eye height (if they have one saved that differs from their prefab height), this event should only execute for their persisted height.

When a **remote** user changes their avatar and applies a persisted eye height (if they have one saved that differs from their prefab eye height), this event may execute more than once. 

For remote players, you will receive this event every time a new eye height is synced to you by the remote player. This means that you could receive an `OnAvatarEyeHeightChanged` event prior to an `OnAvatarChanged` event, but you should not receive `OnAvatarEyeHeightChanged` events out of order.


:::note More Info

See [Player Avatar Scaling](/worlds/udon/players/player-avatar-scaling) for more info about this feature.

:::

---

## ドキュメント: debugging-udon-projects.md

```metadata
階層: /worlds/udon/debugging-udon-projects.md
ディレクトリ: worlds\udon
ファイル名: debugging-udon-projects.md
拡張子: .md
サイズ: 9.59 KB
最終更新: 2025-06-05T03:07:52.810Z
```

import UnityVersionedLink from '@site/src/components/UnityVersionedLink.js';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Debugging Udon Projects

Debugging is how you learn about what's going on under the hood in VRChat, your world, and your Udon code. It's a key skill to develop for programming in general, and for building your worlds.

It's important to test your worlds in VRChat before publishing them. Most errors can be fixed in the Unity editor, but you will sometimes find yourself with worlds or Udon programs that can only be debugged in VRChat.

## VRChat's Debug Log

To help you find errors and make them easier to understand, VRChat saves information about worlds you visit, errors you encounter, and other behind-the-scenes info. This is called the "output log" or the "debug log."

You can access VRChat's log directly in VRChat or as a text file on your PC.

### 1. View Logs in VRChat

When you launch VRChat with the Debug GUI enabled (see below), you can turn on special Debug overlays in both Desktop and VR modes. To view your log messages as they occur, press RShift + Backtick[^1] + 3. You can find all the shortcuts available for different debug overlays on the [Keyboard and Mouse](https://docs.vrchat.com/docs/keyboard-and-mouse) page.

[^1]: On a standard English US QWERTY layout keyboard, "Backtick" is the key in the top left, next to the `1` key. It shares a key with the tilde (`~`) character.

### 2. View Logs in a Text Editor
You can view these files during or after a VRChat session by finding them on your disk and opening them up. They are typically saved to the following folder, with your computer username instead of 'YourName':

`C:\Users\YourName\AppData\LocalLow\VRChat\VRChat`

In this folder, you'll find some more folders, and a handful of files with names like:
` output_log_08-55-48.txt`

These are your log files - a new one is made each time you launch VRChat, with a timestamp to keep the names unique. You can open them in any text browser to find detailed information of what happened during your session.

## How to enable Udon Debugging
By default, VRChat's log does not contain detailed information about Udon.

When you "Build and Test" your world using the button in the VRChat Control Panel, Unity launches VRChat with additional debugging features enabled. This gives you all the information possible.

In order to copy the way that *Build and Test* launches VRChat, you need to launch VRChat with additional launch parameters.

There are three ways to configure VRChat's launch parameters, described below.

### 1. VRC Quick Launcher

The [VRC Quick Launcher](https://vcc.docs.vrchat.com/tools/vrc-quick-launcher/) allows you to choose which debugging features to enable. You can find the VRC Quick Launcher in the "Tools" tab of the [Creator Companion](https://vcc.docs.vrchat.com/).

### 2. Batch Files
You can use a batch file to launch VRChat with special options. A batch file is a plain text file that contains some special commands. This gives you a convenient shortcut to immediately start VRChat with debugging enabled.
1. Make a new text file called `debug.bat` right next to the VRChat.exe on your machine.
2. Add this line to the file: `VRChat.exe --no-vr --enable-debug-gui --enable-sdk-log-levels --enable-udon-debug-logging`
	- This command turns on three flags for extra logging, and also forces VRChat to bypass VR for desktop testing.
	- There are more options you can pass along - you can include any of the flags from the [VRChat Launch Options](https://docs.vrchat.com/docs/launch-options) page as well as the [Unity Standalone Player command line arguments](https://docs.unity3d.com/2022.3/Documentation/Manual/CommandLineArguments.html).
	- For example, the following command launches VRChat on your secondary VRChat profile and forces a screen height of 720 pixels:
`VRChat.exe --profile=1 --no-vr --enable-debug-gui --enable-sdk-log-levels --enable-udon-debug-logging -screen-width 1280 -screen-height 720`
3. Save the batch file and run it.
4. (Optional) Right-click the batch file, create a shortcut, and move the shortcut to your desktop.
5. Run the batch file by double-clicking it.

### 3. Steam Launch Options
If you use the Steam version of VRChat, you enable debugging by changing VRChat's launch options. 

1. In your Steam Library, right-click on the VRChat entry and choose 'Properties'.
2. In the 'General' tab, press the 'Set Launch Options' button.
3. In the field that appears, you can enter the VRChat-specific flags you want always enabled, like `--enable-debug-gui --enable-udon-debug-logging` to always have the Debug GUI and Udon Debugging enabled.

:::note Debugging reduces performance

It is **not** recommend to always keep debugging enabled. It decreases VRChat's performance and increases the size of your log files. Disable debugging if you don't need it.

:::

## How to add Logs to your Udon Program 

If you're ever in a situation where Udon is not doing something that you want it to do, a good way to diagnose it is to add `Debug Log` nodes with unique text. You can do this anywhere in your script, at any time. For example:

<Tabs groupId="udon-compiler-language">
<TabItem value="graph" label="Udon Graph">

![An Udon Graph showing the "InputJump" event, "Debug Log" node, and a string constant.](/img/worlds/udon/examples/debug-log.png)

</TabItem>
<TabItem value="cs" label="UdonSharp">

```cs
using UdonSharp;  
using UnityEngine;  
using VRC.Udon.Common;  
  
public class JumpDetector : UdonSharpBehaviour  
{  
    public override void InputJump(bool value, UdonInputEventArgs args)  
    {  
        Debug.Log("Local player jumped!");  
    }  
}
```

</TabItem>
</Tabs>

Add logging messages before or after your script does something important. When your UdonBehaviour runs, look at Unity's or VRChat's log to see your log messages. This helps you identify whether your script behaves as expected, or whether your script stops running before your `Debug.Log` could be executed.

## Understanding Udon Errors
When an UdonBehaviour runs into a major issue while running in the client, it will disable itself. If you're looking at the VRChat's logs, you'll see an entry like this:
`[UdonBehaviour] An exception occurred during Udon execution, this UdonBehaviour will be halted.`
To find out more about what happened, open up your log files using the instructions above under *Finding Your Logs*, and search for the world 'halted'. There, you will find some more information about what happened.

### Example
Here's an error you might encounter in Udon. Let's take it apart:


```
2020.08.28 17:40:51 Error      -  [UdonBehaviour] An exception occurred during Udon execution, this UdonBehaviour will be halted.
VRC.Udon.VM.UdonVMException: An exception occurred in an UdonVM, execution will be halted. ---> VRC.Udon.VM.UdonVMException: An exception occurred during EXTERN to 'VRCSDK3VideoComponentsBaseBaseVRCVideoPlayer.__GetTime__SystemSingle'. ---> System.NullReferenceException: Object reference not set to an instance of an object.
  at VRC.SDK3.Internal.Video.Components.AVPro.AVProVideoPlayerInternal.GetTime () [0x00000] in <00000000000000000000000000000000>:0 
  at VRC.Udon.Wrapper.Modules.ExternVRCSDK3VideoComponentsBaseBaseVRCVideoPlayer.__GetTime__SystemSingle (VRC.Udon.Common.Interfaces.IUdonHeap heap, System.UInt32[] parameterAddresses) [0x00000] in <00000000000000000000000000000000>:0 
```

- The important information is in the second line.
	- `An exception occurred during EXTERN to 'VRCSDK3VideoComponentsBaseBaseVRCVideoPlayer.__GetTime__SystemSingle'. ---> System.NullReferenceException: Object reference not set to an instance of an object.`
- This error tells you that our world is trying to access something that does not exist.
	- Specifically, the script was trying to access a `VRCVideoPlayer` when it doesn't have one assigned. That's what ` Object reference not set to an instance of an object` means, and `VRCSDK3VideoComponentsBaseBaseVRCVideoPlayer.__GetTime__SystemSingle` tells you that it occurred when it tried to call `GetTime` on a `VRCVideoPlayer`.
	- Once you get comfortable reading logs, this kind of information is invaluable.
- You can now go to the graph that tries to call `VRCVideoPlayer.GetTime` and make sure it has a `VRCVideoPlayer` connected to it.

## Use Debug Log to Diagnose Udon
If you're ever in a situation where Udon is not doing something that you want it to do, a good way to diagnose it is to add logging.

Add the "Debug Log" node to your Udon Graph or execute `Debug.Log` in UdonSharp. Add unique message to help you understand your log, such as "The Rotating Cube script has finished running its Start event."

Put Debug Logs right before or after your script tries to do something important. Then when you run your UdonBehaviour, you can observe the log to see how far it's getting and whether or not it is doing what you expect.

## View UdonSharp Logs in Unity

UdonSharp includes a runtime exception watcher that looks for Udon exceptions from VRChat's output log. It allows you to see exactly which line in your UdonSharp script that threw an  exception. This feature is enabled by default but can be disabled in your Project Settings:

![Project Settings](/img/worlds/udon/udonsharp/red-1.png)
![Listen for Client Exceptions](/img/worlds/udon/udonsharp/red-2.png)

In VRChat, any errors that are thrown in your world will be shown to your editor's console. For example, in the following example, the script `FullBodyPlayerTracker.cs` encountered an error in line 92 at the 86th character:

![Error in Console](/img/worlds/udon/udonsharp/red-6.png)


---

## ドキュメント: event-execution-order.md

```metadata
階層: /worlds/udon/event-execution-order.md
ディレクトリ: worlds\udon
ファイル名: event-execution-order.md
拡張子: .md
サイズ: 1.07 KB
最終更新: 2025-06-05T03:07:52.810Z
```

import UnityVersionedLink from '@site/src/components/UnityVersionedLink.js';

# Event Execution Order

Udon and Unity have built-in events that are automatically called if you include them in your scripts. For example, the `Start()` event runs once for every script, and the `Update()` event runs once per frame. When you're writing Udon scripts, it's helpful to know which of these events happen first.
:::note

Unity provides an (incomplete) <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/ExecutionOrder.html">list of built-in events</UnityVersionedLink>, many of which are also available in VRChat.
:::
The following diagram shows the execution order of the most important events available in Udon and Unity.

![Example banner](/img/worlds/event-execution-order.svg)

:::caution

Unity and VRChat updates may change the event execution order depicted above.
Not all events are listed, and some events may be executed in a different order depending on circumstances (being an object's owner, joining a world late, etc.)

:::

---

## ドキュメント: external-urls.md

```metadata
階層: /worlds/udon/external-urls.md
ディレクトリ: worlds\udon
ファイル名: external-urls.md
拡張子: .md
サイズ: 1.76 KB
最終更新: 2025-06-05T03:07:52.810Z
```

# External URLs
Udon can use external URLs to load remote content. URLs must be wrapped in a [VRCUrl](#vrcurl) object. Users can enter URLs into `VRCUrlInputField` components at runtime, and world creators can provide pre-defined VRCUrls with their uploaded worlds.

## Allowlist
For security reasons, VRChat restricts how external URLs can be used. By default, a [VRCUrl](#vrcurl) can only be accessed if it is on VRChat's domain allowlist.
If a URL is **not** on the required allowlist for its type, it cannot be used unless the user chooses to "Allow Untrusted URLs" in VRChat's settings. This allows Udon to use load untrusted URL from `VRCUrlInputField` components and any `VRCUrl` that was uploaded alongside the world.

|                                           | On allowlist | Not on allowlist                   |
| ----                                      | ----         | ----                               |
| User enters `VRCUrl` into input field     | ✔Allowed     | ⚠ Requires "Allow Untrusted URLs" |
| Udon declares the `VRCUrl` before runtime | ✔Allowed     | ⚠ Requires "Allow Untrusted URLs" |

## VRCUrl
`class VRC.SDKBase.VRCUrl`

### Constructor
| Name | Summary |
| --- | --- |
| VRCUrl(string url) | Constructor that takes a URL as input.  Note that this can only be called at *editor time*. |

### Properties
| Static | Type | Name | Summary |
| :---: | --- | --- | --- |
| ✔️ | [VRCUrl](#vrcurl) | Empty | An empty URL. |

### Methods
| Static | Returns | Name | Summary |
| :---: | --- | --- | --- |
|| string | Get() | Retrieves the current string value of the URL. |
| ✔️ | bool | IsNullOrEmpty([VRCUrl](#vrcurl) vrcUrl) | Indicates whether the specified [VRCUrl](#vrcurl) is null or referencing an empty string (""). |


---

## ドキュメント: image-loading.md

```metadata
階層: /worlds/udon/image-loading.md
ディレクトリ: worlds\udon
ファイル名: image-loading.md
拡張子: .md
サイズ: 8.46 KB
最終更新: 2025-06-05T03:07:52.812Z
```

import UnityVersionedLink from '@site/src/components/UnityVersionedLink.js';

# Image Loading

Image Loading allows you to display images from the internet in your VRChat world. When a user visits your world, the image can be downloaded from the internet and used as a texture in your materials. Here are a few examples on how Image Loading can be used:

- Updating textures in your world without a re-upload.
- Creating a poster in your world and updating it for seasonal events or parties.
- Reusing the same texture in multiple worlds and updating them all at once.

The SDK includes an easy-to-use `ImageDownload` script, or you can make your own script with the new `VRCImageDownloader` object.

:::tip
You can [view our Image Loader example](/worlds/examples/image-loading) to get started quickly.
:::
## Before You Begin

There are a few Image Loader limits and parameters you should know:

- The maximum resolution is 2048 × 2048 pixels. Attempting to download larger images will result in an error.
- One image can be downloaded every five seconds.
  - If this limit is exceeded, images downloads are queued and downloaded in a random order.
  - This limit applies to your entire scene, regardless of the amount of VRCImageDownload components used.
- The URL must point directly at an image file. URL redirection is not allowed and will result in an error.
- Downloaded images are automatically interpreted as RGBA, RGB, or RG images.
  - For example, a grayscale image with an alpha channel is interpreted as an RG image.
- There is a limit of 1000 elements in the queue.
- Both the Input and Output buffers are limited to a maximum of 32MB, images exceeding these will result in an error.

And only certain domains are allowed. If a domain is not on the list, images will not download unless **Allow Untrusted URLs** has been enabled in the user's settings.


- DisBridge (`*.disbridge.com`)
- Dropbox (`dl.dropbox.com`,`dl.dropboxusercontent.com`)
- GitHub (`*.github.io`)
- ImageBam (`images4.imagebam.com`)
- ImgBB (`i.ibb.co`)
- imgbox (`images2.imgbox.com`)
- Imgur (`i.imgur.com`)
- Postimages (`i.postimg.cc`)
- Reddit (`i.redd.it`)
- Twitter (`pbs.twimg.com`)
- VRCDN (`*.vrcdn.cloud`)
- VRChat (`assets.vrchat.com`)
- Ytimg (`i.ytimg.com`)

## Memory Management

Downloaded images can take up a lot of memory. Once you're done using an image, you should dispose of it via Udon, which frees up memory for something else.

For example, if you download a new image and want to use it to replace another image you downloaded earlier, you should use the `Dispose` method documented below to remove the old image from memory. If you don't do this and keep downloading new images, visitors to your world may run out of memory and crash after spending enough time in your world!

## UdonGraph Nodes

### VRCImageDownloader

Use `VRCImageDownloader`'s constructor to create an image downloader, which can download image from the Internet during runtime.

#### DownloadImage

Downloads an image, and calls an event indicating success or failure (see 'Events' below).  
Returns an `IVRCImageDownload`, which can be used to track the progress of the download.

- **Instance**: The `ImageDownloader` component to download the image with.  
- **Url** : The `VRCURL` of the texture to download.  
- **Material** (optional): The Material to automatically apply the downloaded image to, as a main texture.
- **UdonBehavior** (optional): The `Udonbehavior` to send `VRCImageDownloader` events to. If `udonBehavior` is empty, the current UdonBehaviour will receive all events.
  - Note that UdonSharp will not receive any events unless `udonBehavior` is specified.
- **TextureInfo** (optional):  The `TextureInfo` object containing settings for the newly created texture.

#### Dispose

Cleans up the `VRCImageDownloader`. Frees up downloaded textures from memory.

Calling `Dispose` on a `VRCImageDownloader` invalidates the object, meaning it can't be used to download any new images.

**Notes on disposal and garbage collection:**

- Calling `Dispose` will invalidate the `VRCImageDownloader`, all of its associated `IVRCImageDownload` objects, and the textures associated with those downloads.
  - If you only want to dispose of a single download, call `Dispose` on the `IVRCImageDownload` object instead.
- Make sure to save the reference to your `VRCImageDownloader` as a variable to prevent it (and any downloaded texture) from randomly being garbage collected.

### TextureInfo

Contains settings to apply to a downloaded texture. 

- **GenerateMipmaps**: Enables Mipmap generation. (Default: `false`)
- **FilterMode**: Sets the `FilterMode` of the texture. (Default: `Trilinear`)
- **WrapModeU**: The `TextureWrapMode` along the U (horizontal) axis (Default: `Repeat`)
- **WrapModeV**: The `TextureWrapMode` along the V (vertical) axis  (Default: `Repeat`)
- **WrapModeW**: The `TextureWrapMode` along the W (depth, only relevant for Texture3D) axis. (Default: `Repeat`)
- **AnisoLevel**: The `anisoLevel` of the texture. A value of 0 disables filtering, 16 equals full filtering. (Default: `9`)
  - VRChat uses forced anisotropic filtering. When the anisoLevel value is between 1 and 9, Unity sets the anisoLevel to 9. If the value is higher than 9, Unity clamps it between 9 and 16.
- **MaterialProperty**: Overrides which `MaterialProperty` to apply the downloaded texture to, if a `material` was specified in `DownloadImage`. (Default: `_MainTex`)

### IVRCImageDownload

Contains information about the downloaded image. Returned by `VRCImageDownloader`'s `DownloadImage` function, by `OnImageLoadSuccess`, and by `OnImageLoadError`.  
Note that many of these fields will be invalid until the download has completed or failed.

- **Get Error**: Gets the `VRCImageDownloadError` associated with the event. 
- **Get Errormessage**: Gets the error message as a `string`.  
- **Get Material**: Gets the Material sent into the `DownloadImage` function.  
- **Get Progress**:`Gets the progress of the image download as a`float\` between 0 and 1. Use this to track the progress of the download, i. e. for custom loading bars.
- **Get Result**: The `Texture2d` of the downloaded image.  
- **Get SizeInMemoryBytes**: Gets the size of the texture in bytes as an `int`. 
- **Get State**: Gets the `VRCImageDownloadState` indicating the state of the image download.  
- **Get TextureInfo**: The texture info given to the DownloadImage function (TextureInfo).  
- **Get Udonbehavior**: Gets the given udonbehavior the events of the download image are being sent to (UdonBehavior).
- **Get URL**: Gets the `VRCURL` of the image download.

VRChat automatically selects the <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/ScriptReference/TextureFormat.html">texture format</UnityVersionedLink> of the downloaded image.
- Images with an alpha channel as loaded as RGBA32, RGB64, etc.
- Images without an alpha channel are loaded as RGB24, RGB48, etc.
- Greyscale images are loaded as R8, R16, etc.

#### Dispose

Cleans up the `IVRCImageDownload`. Unloads the downloaded texture and frees up the memory it was using.

Unlike the dispose method of `VRCImageDownloader`, this will only dispose this individual download and its associated texture, leaving other downloads and their textures intact.

Disposing an `IVRCImageDownload` will change its `State` to `Unloaded`.

### VRCImageDownloadState

Indicates the state of the image download in `IVRCImageDownload`:

- **Pending**: Not been started or still in progress.
- **Error**: Download failed an error (see `VRCImageDownloadError`).
- **Complete**: Download complete, texture is ready to use.
- **Unloaded**: Pending garbage collection after `Dispose` has been called on `IVRCImageDownload`.
- **Unknown**: Unknown state.

### VRCImageDownloadError

When an image download fails, `OnImageLoadError` is called. `IVRCImageDownload`'s `Error` field will contain one of the following error states:

- **InvalidURL**: The download URL used in `DownloadImage` is invalid.
- **AccessDenied**: Access to the URL was denied.
- **InvalidImage**: The downloaded image is invalid.
- **DownloadError**: A web request error occured.
- **Unknown**: Unknown error state.

## Events

* **OnImageLoadSuccess**: Returns `IVRCImageDownload`. Called when a `VRCImageDownloader` has successfully download an image.
* **OnImageLoadError**: Returns `IVRCImageDownload`. Called when a `VRCImageDownloader` has failed to download an image.


---

## ドキュメント: index.md

```metadata
階層: /worlds/udon/index.md
ディレクトリ: worlds\udon
ファイル名: index.md
拡張子: .md
サイズ: 2.41 KB
最終更新: 2025-06-05T03:07:52.814Z
```

---
sidebar_position: 1
---
# Udon

Udon allows players to interact with your world in interesting ways! Use scripts written by other creators, or create your own games, prefabs, and other experiences.

## What is Udon?

Udon is a programming language[^1] for VRChat worlds. Scripts can interact with scene objects, [players](./players), [synced networked variables](./networking), and more. Udon makes your world come to life!

Udon runs in both VRChat *and* the Unity Editor. You can test and debug your scripts without needing to build and upload your VRChat world. You can also use [Udon's debugging features](debugging-udon-projects).

After you [create your VRChat world](/sdk/), there are two main ways to create Udon scripts:
- The [Udon Node Graph](./graph) is a visual programming interface that uses nodes and wires to connect flow, inputs, and outputs.
	- The Graph is similar to Unity animators, Blender shaders, geometry nodes, or Unreal blueprints.
	- The Graph is unique to the VRChat SDK does not require any third-party tools.
	- Use the Graph if you're very new to programming or only want to create very simple scripts.
- [UdonSharp](./udonsharp) allows you to use C# to create scripts.
	- UdonSharp is similar to Unity's built-in C# scripting system.
	- Most UdonSharp users use an [IDE](https://en.wikipedia.org/wiki/Integrated_development_environment). [Visual Studio](https://visualstudio.microsoft.com/vs/unity-tools/) is free, and [Rider](https://www.jetbrains.com/rider/) is free for non-commercial use.
	- Use UdonSharp if you're already familiar with programming or want to create powerful scripts.


And if you're an expert user:

- You can write your own compiler to generate Udon Assembly code.
	- VRChat Udon is technically a [virtual machine](https://en.wikipedia.org/wiki/Virtual_machine) running bytecode compiled from Udon Assembly.
	- You *can* write Udon Assembly code manually, though this is extremely uncommon.

## Bug Reports and Feature Requests
To submit bug reports or feature requests, use VRChat's [Canny feedback board](https://vrchat.canny.io/udon).

[^1]: For the more technically inclined: **VRChat Udon** is a VM running bytecode compiled from **Udon Assembly**. You can generate **Udon Assembly** using the built-in **VRChat Udon Node Graph** UI, writing your own **Udon Assembly**, or even by writing your own compiler to generate **Udon Assembly** or bytecode programs directly.



---

## ドキュメント: input-events.md

```metadata
階層: /worlds/udon/input-events.md
ディレクトリ: worlds\udon
ファイル名: input-events.md
拡張子: .md
サイズ: 5.51 KB
最終更新: 2025-06-05T03:07:52.814Z
```

# Input Events

You can read the input of a player's controller in a unified way across all platforms by using Udon Input Events. These events will work correctly even when the player has remapped their controls. 

There are currently two types of events - [Button](#button-events) and [Axis](#axis-events), which include boolean and float values. Each event also holds a special [UdonInputEventArgs](#udoninputeventargs) object. You can also use [Unity input methods and properties](/worlds/udon/input-events#unity-input-methods-and-properties) to directly read input data. 

## Button Events
Button events include a *bool* value which is **true** when the button is pressed and **false** when it is released. 

### InputJump
Spacebar on Desktop, typically a face button on controllers.

### InputUse
Left-Click on Desktop, typically a trigger button on controllers.

### InputGrab
Left-Click on Desktop, typically a grip button on VR controllers.

### InputDrop
Right-Click on Desktop, press grip button on Vive Wands and some Windows Mixed Reality Controllers, release grip button on others.

## Axis Events
Axis events have a **float** value which typically ranges between -1 and 1. When using a controller with analog sticks, a new event will be triggered for each change in value, from 0 to 0.1, then to 0.2, etc. Desktop users will output whole numbers: -1, 0, 1, etc.

### InputMoveHorizontal
A and D on Desktop, typically the left stick/pad moving left and right on controllers.

### InputMoveVertical
W and S on Desktop, typically the left stick/pad moving up and down on controllers.

### InputLookVertical
Moving the mouse up and down on Desktop, typically the right stick up and down on gamepad and VR controllers.

### InputLookHorizontal
Moving the mouse left and right on Desktop, turning left and right using the right stick/pad without Comfort Turning on VR controllers, typically the right stick left and right on gamepad controllers.

## UdonInputEventArgs
This object is included in every input event, and holds additional data for the event which may be useful. We may add more data into this object in the future, let us know if you think of something handy you'd like to reference here. For now, it includes:

- **UdonInputEventType**: BUTTON or AXIS
- **boolValue**: True/False if this is a button event, false if it's an axis event (default value)
- **floatValue**: Number between -1 and 1 for an axis event, 0 if it's a button event (default value)
- **handType**: LEFT or RIGHT. Included for keyboard and mouse users as well (mouse is RIGHT, keyboard is LEFT).

## OnInputMethodChanged
This event fires whenever a user switches input methods, like from Keyboard to Mouse, Controller, or Touchscreen. It includes a [VRCInputMethod](/worlds/udon/graph/type-nodes/#vrcsdkbasevrcinputmethod) enum as its parameter.

:::note Ambiguous Vive input names 

- `VRCInputMethod.Vive` is a Vive controller running through SteamVR.
- `VRCInputMethod.ViveXr` is a Vive XR Elite Controller running via OpenXR.

:::

## Unity Input Methods and Properties

Udon can access some methods and properties from the [`UnityEngine.Input`](https://docs.unity3d.com/ScriptReference/Input.html) namespace. They provide detailed information about user input.

The following methods and properties are available in Udon: 
- [`Input.anyKey`](https://docs.unity3d.com/ScriptReference/Input-anyKey.html), [`Input.anyKeyDown`](https://docs.unity3d.com/ScriptReference/Input-anyKeyDown.html)
- [`Input.inputString`](https://docs.unity3d.com/ScriptReference/Input-inputString.html)
- [`Input.imeIsSelected`](https://docs.unity3d.com/ScriptReference/Input-imeIsSelected.html)
- [`Input.GetAxis()`](https://docs.unity3d.com/ScriptReference/Input.GetAxis.html), [`Input.GetAxisRaw()`](https://docs.unity3d.com/ScriptReference/Input.GetAxisRaw.html)
- [`Input.GetButton()`](https://docs.unity3d.com/ScriptReference/Input.GetButton.html), [`Input.GetButtonDown()`](https://docs.unity3d.com/ScriptReference/Input.GetButtonDown.html), [`Input.GetButtonUp()`](https://docs.unity3d.com/ScriptReference/Input.GetButtonUp.html)
- [`Input.GetMouseButton()`](https://docs.unity3d.com/ScriptReference/Input.GetMouseButton.html), [`Input.GetMouseButtonDown()`](https://docs.unity3d.com/ScriptReference/Input.GetMouseButtonDown.html), [`Input.GetMouseButtonUp()`](https://docs.unity3d.com/ScriptReference/Input.GetMouseButtonUp.html)
- [`Input.GetJoystickNames()`](https://docs.unity3d.com/ScriptReference/Input.GetJoystickNames.html)
- [`Input.GetKey()`](https://docs.unity3d.com/ScriptReference/Input.GetKey.html), [`Input.GetKeyUp()`](https://docs.unity3d.com/ScriptReference/Input.GetKeyUp.html), [`Input.GetKeyDown()`](https://docs.unity3d.com/ScriptReference/Input.GetKeyDown.html)

## Input Detection in VRChat Menus

Udon can't detect input while any of the following VRChat menus are open:
- Main menu
- Quick menu (Desktop or mobile only)
- Text input popup

When you open a VRChat menu, Udon releases all held inputs, even if you continue holding them. For example:
- If you hold the arrow key and open the VRChat menu, `Input.GetButtonUp()` returns `true` for that key.
- If you hold the jump button and open the VRChat menu, Udon executes `InputJump(false)`.

When you close a VRChat menu, Udon presses all held **Unity** inputs. For example:
- If you hold the right arrow key while closing the VRChat menu, `Input.GetButtonDown()` returns `true` for that key.
- If you hold the jump button and close the VRChat menu, Udon does **not** execute `InputJump(true)`.




---

## ドキュメント: string-loading.md

```metadata
階層: /worlds/udon/string-loading.md
ディレクトリ: worlds\udon
ファイル名: string-loading.md
拡張子: .md
サイズ: 4.13 KB
最終更新: 2025-06-05T03:07:52.825Z
```

# String Loading

String Loading allows you to download text files from the internet and use them in your VRChat world. You can either use the `DownloadString` script included in the SDK, or you can make your own script using the new `VRCStringDownloader.LoadUrl` function.

- Your text files can be in any format, such as `.txt` or `.json`.
* One string can be downloaded every five seconds.
If this limit is exceeded, string downloads are queued and downloaded in a random order.
* One string can only be of a maximum of 100MB
* You can only have 1000 elements in the queue

## Trusted URLs
If a site is not on the list, it will not download unless ‘Allow Untrusted URLs’ has been enabled in the user’s settings.

The following URLs are available:

* Disbridge (`*.disbridge.com`)
* GitHub (`*.github.io`)
* Github Gist (`gist.githubusercontent.com`)
* Pastebin (`pastebin.com`)
* VRCDN (`*.vrcdn.cloud`)

## Guides

There are multiple ways to use string loading in your world.

### Using the `DownloadString` script to download a string
The SDK includes a script to download strings easily:

1. Create a new GameObject in your scene.
2. Add an `UdonBehaviour` component.
3. Select `DownloadString` as the program source.
4. Enter the URL and select the text component where you'd like to display the downloaded text.

### Create your own script for LoadUrl
You can use the function `VRCStringDownloader.LoadUrl` to download strings in your own scripts by following these steps:

1. Execute `VRCStringDownloader.LoadUrl` with a URL and specify an UdonBehaviour.
	- You can find `VRCStringDownloader` in the `VRC.SDK3.StringLoading` namespace.
2. Wait for the `OnStringLoadSuccess` or `OnStringLoadError` event to be called on the specified UdonBehaviour.
3. Use the event's `IVRCStringDownload` to get the `Result` of the string download.

## New events

### OnStringLoadSuccess
Returns `IVRCStringDownload`. Called when the function `LoadUrl` has successfully downloaded the string from the internet.

### OnStringLoadError
Returns `IVRCStringDownload`. Called when the function `LoadUrl` has failed to download the string.

## New types
### VRCStringDownloader

Use this static class to download strings from the web.

#### VRCStringDownloader.LoadUrl
* **Url**: the URL to load from the internet.
* **UdonBehaviour**: the UdonBehaviour to send the events to. 
    * In Udon Graph, this defaults to the current UdonBehaviour
    * In Udon Sharp, you can use `(IUdonEventReceiver)this`


### IVRCStringDownload
Result from the string load events.

* **Get Error (`string`)**: The error message for `OnStringLoadError`.
* **Get ErrorCode (`int`)**: The HTTP Error code for `OnStringLoadError`.
* **Get ResultBytes (`byte[]`)**: The raw data that was downloaded as a byte array. You can use `System.Text.Encoding` on this to decode a string in a custom format. Accessing this property will return a copy of the data.
* **Get Result (`string`)**: The string that was downloaded, decoded via the UTF8 standard.
* **Get UdonBehaviour (`UdonBehaviour`)**: The UdonBehaviour to which events are sent.
* **Get Url (`VRCUrl`)**: Gets the URL from which the download was attempted.

## Example Code

```csharp title="String Download Example, Custom Text Encoding"
using System.Text;
using UdonSharp;
using UnityEngine;
using VRC.SDK3.StringLoading;
using VRC.SDKBase;
using VRC.Udon.Common.Interfaces;

public class ResultBytesExample : UdonSharpBehaviour
{
    [SerializeField]
    private VRCUrl url;

    void Start()
    {
        VRCStringDownloader.LoadUrl(url, (IUdonEventReceiver)this);
    }

    public override void OnStringLoadSuccess(IVRCStringDownload result)
    {
        string resultAsUTF8 = result.Result;
        byte[] resultAsBytes = result.ResultBytes;
        string resultAsASCII = Encoding.ASCII.GetString(resultAsBytes);
        Debug.Log($"UTF8: {resultAsUTF8}");
        Debug.Log($"ASCII: {resultAsASCII}");
    }

    public override void OnStringLoadError(IVRCStringDownload result)
    {
        Debug.LogError($"Error loading string: {result.ErrorCode} - {result.Error}");
    }
}
```


---

## ドキュメント: udon-moderation-tool-guidelines.md

```metadata
階層: /worlds/udon/udon-moderation-tool-guidelines.md
ディレクトリ: worlds\udon
ファイル名: udon-moderation-tool-guidelines.md
拡張子: .md
サイズ: 228 B
最終更新: 2025-06-05T03:07:52.826Z
```

# Udon Moderation Tool Guidelines

For information on Udon-powered moderation tools in worlds, please consult the [VRChat Creator Guidelines](https://hello.vrchat.com/creator-guidelines), which contains a section on Worlds. 


---

## ドキュメント: ui-events.md

```metadata
階層: /worlds/udon/ui-events.md
ディレクトリ: worlds\udon
ファイル名: ui-events.md
拡張子: .md
サイズ: 4.91 KB
最終更新: 2025-06-05T03:07:52.829Z
```

# UI Events

You can use Unity UI events to directly call methods for simple interactions, rather than building an UdonBehaviour. 
![ui-events-3c37d22-UIEventTarget.png](/img/worlds/ui-events-3c37d22-UIEventTarget.png)

However, we've limited what can be called to this list:
# Allowed UI Event Targets
### Animator
* Play
* PlayInFixedTime
* Rebind
* SetBool
* SetFloat
* SetInteger
* SetTrigger
* ResetTrigger
* speed

### AudioSource
* Pause
* Play
* PlayDelayed
* PlayOneShot
* Stop
* UnPause
* bypassEffects
* bypassListenerEffects
* bypassReverbZones
* dopplerLevel
* enabled
* loop
* maxDistance
* rolloffMode
* minDistance
* mute
* pitch
* playOnAwake
* priority
* spatialize
* spread
* time
* volume

### AudioDistortionFilter
* decayRatio
* delay
* dryMix
* enabled
* wetMix

### AudioEchoFilter
* decayRatio
* delay
* dryMix
* enabled
* wetMix

### AudioHighPassFilter
* cutoffFrequency
* enabled
* highpassResonanceQ

### AudioLowPassFilter
* cutoffFrequency
* enabled
* lowpassResonanceQ

### AudioReverbFilter
* decayHFRatio
* decayTime
* density
* diffusion
* dryLevel
* enabled
* hfReference
* reflectionsDelay
* reflectionsLevel
* reverbDelay
* reverbLevel
* room
* roomHF
* roomLF

### AudioReverbZone
* decayHFRatio
* decayTime
* density
* diffusion
* enabled
* HFReference
* LFReference
* maxDistance
* minDistance
* reflections
* reflectionsDelay
* room
* roomHF
* roomLF

### Button
* enabled
* interactable
* targetGraphic

### Collider
* enabled
* isTrigger

### Dropdown
* captionText
* enabled
* interactable
* itemText
* targetGraphic
* template
* value

### Image
* alphaHitTestMinimumThreshold
* enabled
* fillAmount
* fillCenter
* fillClockwise
* fillOrigin
* maskable
* preserveAspect
* raycastTarget
* useSpriteMesh

### GameObject
* SetActive

### InputField
:::caution Character limit

Please note that input fields are limited to 16.000 characters, which is the maximum amount of characters a text component can render.
:::
* ForceLabelUpdate
* caretBlinkRate
* caretPosition
* caretWidth
* characterLimit
* customCaretColor
* enabled
* interactable
* readOnly
* selectionAnchorPosition
* text
* textComponent
* selectionFocusPosition


### Light
* Reset
* bounceIntensity
* colorTemperature
* cookie
* enabled
* intensity
* range
* shadowBias
* shadowNearPlane
* shadowNormalBias
* shadowStrength
* spotAngle

### LineRenderer
* allowOcclusionWhenDynamic
* shadowCastingMode
* enabled
* endWidth
* loop
* motionVectorGenerationMode
* numCapVertices
* numCornerVertices
* probeAnchor
* receiveShadows
* shadowBias
* startWidth
* lightProbeUsage
* useWorldSpace
* widthMultiplier

### Mask
* enabled
* showMaskGraphic

### MeshRenderer
* shadowCastingMode
* enabled
* probeAnchor
* probeAnchor
* receiveShadows
* lightProbeUsage

### ParticleSystem
* Clear
* Emit
* Pause
* Pause
* Play
* Simulate
* Stop
* Stop
* TriggerSubEmitter
* time
* useAutoRandomSeed

### ParticleSystemForceField
* endRange
* gravityFocus
* length
* multiplyDragByParticleSize
* multiplyDragByParticleVelocity
* startRange

### Projector
* aspectRatio
* enabled
* nearClipPlane
* farClipPlane
* fieldOfView
* orthographic
* orthographicSize

### RawImage
* enabled
* maskable
* raycastTarget

### RectMask2D
* enabled

### Scrollbar
* enabled
* handleRect
* interactable
* numberOfSteps
* size
* targetGraphic
* value

### ScrollRect
* content
* decelerationRate
* elasticity
* enabled
* horizontal
* horizontalNormalizedPosition
* horizontalScrollbar
* horizontalScrollbarSpacing
* inertia
* scrollSensitivity
* vertical
* verticalNormalizedPosition
* verticalScrollbar
* verticalScrollbarSpacing
* viewport

### Selectable
* enabled
* interactable
* targetGraphic

### SkinnedMeshRenderer
* allowOcclusionWhenDynamic
* shadowCastingMode
* enabled
* lightProbeProxyVolumeOverride
* motionVectorGenerationMode
* probeAnchor
* receiveShadows
* rootBone
* skinnedMotionVectors
* updateWhenOffscreen
* lightProbeUsage

### Slider
* enabled
* fillRect
* handleRect
* interactable
* maxValue
* minValue
* normalizedValue
* targetGraphic
* value
* wholeNumbers

### Text
* alignByGeometry
* enabled
* fontSize
* lineSpacing
* maskable
* raycastTarget
* resizeTextForBestFit
* resizeTextMaxSize
* resizeTextMinSize
* supportRichText
* text

### Toggle
* enabled
* group
* interactable
* isOn
* targetGraphic

### ToggleGroup
* allowSwitchOff
* enabled

### TrailRenderer
* Clear
* allowOcclusionWhenDynamic
* autodestruct
* shadowCastingMode
* enabled
* emitting
* endWidth
* motionVectorGenerationMode
* numCapVertices
* numCornerVertices
* probeAnchor
* receiveShadows
* shadowBias
* startWidth
* lightProbeUsage
* widthMultiplier


### UdonBehaviour
* RunProgram
* SendCustomEvent
* Interact

---

## ドキュメント: using-build-test.md

```metadata
階層: /worlds/udon/using-build-test.md
ディレクトリ: worlds\udon
ファイル名: using-build-test.md
拡張子: .md
サイズ: 5.49 KB
最終更新: 2025-06-05T03:07:52.829Z
```

# Using Build & Test

:::note

Before you read this page, you should read [What is Udon?](/worlds/udon/#what-is-udon)

:::

<iframe class="embedly-embed" src="//cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fwww.youtube.com%2Fembed%2Fvideoseries%3Flist%3DPLe9XHNvXcouQjg5GULWGLj1tMzeythnQi%26start%3D0&display_name=YouTube&url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3D8yaQY0arCnc&image=https%3A%2F%2Fi.ytimg.com%2Fvi%2F8yaQY0arCnc%2Fhqdefault.jpg&key=f2aa6fc3595946d0afc3d76cbbd25dc3&type=text%2Fhtml&schema=youtube" width="854" height="480" scrolling="no" title="YouTube embed" frameborder="0" allow="autoplay; fullscreen" allowfullscreen="true"></iframe>
Some simple things in your world will work just by pressing 'Play' in the Editor - mouse events, timers, things that don't need interaction from avatars or networking. For a lot of the interesting functionality, you're going to need to make a build of your world that runs in the actual VRChat Client.

## Open the UdonExampleScene

Open the UdonExampleScene from the VRChat Examples folder - this scene has lots of great reusable Graphs from which you can learn. It's all set up to work as-is, so we can use it to make sure everything's working.

## Setting Up Your Settings

1. Start by creating a new project, making sure the Worlds SDK has been imported correctly, and opening up the VRChat Control panel through the Menu Bar under "VRChat SDK > Show Control Panel".

![](/img/worlds/using-build-test-e47cc0f-show-control-panel.png)

2. Enter your Login information on the 'Authentication' tab and press 'Sign In'.
![](/img/worlds/using-build-test-8c5c7ff-sign-in.png)

3. Click on the Settings tab and look for the 'VRChat Client' entry at the bottom. This is the VRChat Client that Unity will use to test your worlds. If you don't set this, your worlds may not launch correctly. 
![](/img/worlds/using-build-test-69f8274-installed-client-path.png)

Press 'Edit' to bring up a File Chooser, then navigate to the place you installed VRChat and choose the VRChat.exe program. Here are some default places it might be:
* Steam: `C:\Program Files (x86)\Steam\steamapps\common\VRChat\VRChat.exe`
* Oculus: `C:\Program Files\Oculus\Software\Software\vrchat-vrchat\VRChat.exe`
* Viveport: `C:\Viveport\ViveApps\469fbcbb-bfde-40b5-a7d4-381249d387cd\1597468388\VRChat.exe`

4. Switch to the 'Builder' tab. We need to set up our Layers and Collision Matrix to the way that VRChat expects.  Just press the 'Setup Layers for VRChat' button, then 'Do it!' on the popup that appears.
![](/img/worlds/using-build-test-5f05f9b-setup-layers.png)

Next, do the same thing for the 'Set Collision Matrix' button.
![](/img/worlds/using-build-test-7ccc247-set-collision-matrix.png)

## Running Your First Test

With all of our settings correct, we're ready to make a build of the scene. Once you've cleared the issues from the Builder tab by following the instructions above, you have access to the 'Build & Test' button. For this first test, turn on 'Force Non-VR', then press Build & Test.

![](/img/worlds/using-build-test-8712faf-build-and-test.png)

Your VRChat client should launch into a local copy of this world where you can run around and try everything out!

![](/img/worlds/using-build-test-2acac91-UdonExampleScene.jpg)

## Launching Multiple Clients
In order to test Synced Variables and Custom Network Events, you need multiple people in the same world. The easiest way to accomplish this is to use the Builder tab to launch multiple clients. Close the VRChat client window you just launched if it's still open, and change the 'Number of Clients' to 2, then press Build and Test again. This time, Unity will open up two VRChat clients, with your same avatar in both of them. You can swap between the windows to control your two avatars, and even see yourself talking in both of them. Try playing with the Synced Variables area. The first avatar that loads in will be the Master of the instance and therefore Owner of those GameObjects, so they will be able to update the UI Elements, whereas the second avatar can only see the updates. The one exception in this scene is the 'SyncButtonAnyone' which transfers ownership to whoever clicks on it.

## Build & Reload
When testing many clients, it can be a hassle to arrange your windows and wait for VRChat to login every time you make a change to your world. You can change the 'Number of Clients' to launch to 0 to change "Build & Test" into "Build & Reload"

![Build & Reload!](/img/worlds/using-build-test-07685ac-build-and-reload.png)

This will build a new version of your world and move all open clients into that new local instance, skipping the VRChat startup sequence altogether.

You can also do this for clients you launch yourself, if you want to test with multiple profiles. You can use the new command-line flag `--watch-worlds` to turn this functionality on. For example, this command launches VRChat with my main profile in desktop mode, with full debugging, at 1920x1080, with reloading worlds turned on.
```shell
VRChat.exe --watch-worlds --profile=0 --no-vr --enable-debug-gui --enable-sdk-log-levels --enable-udon-debug-logging -screen-width 1920 -screen-height 1080
```

:::danger New 'Build & Test' Clients Don't Join Reloaded Worlds

If you Build & Reload some clients, then choose 'Build & Test' to add more clients, they may not be joined into the right room. However, you can simply reduce the number of clients back to 0 and then 'Build & Reload' or 'Reload Last Build' in order to join them all together again.
:::

---

## ドキュメント: world-debug-views.md

```metadata
階層: /worlds/udon/world-debug-views.md
ディレクトリ: worlds\udon
ファイル名: world-debug-views.md
拡張子: .md
サイズ: 5.37 KB
最終更新: 2025-06-05T03:07:52.835Z
```

# World Debug Views

These are the tools you can use to debug your worlds in-game.

In order to access debug views you need two things:
* Launch VRChat with the launch parameter `--enable-debug-gui`
* Press Rshift + Tilde + 1-9.

If you have a keyboard layout where tilde is not directly to the left of the 1 key, try using whichever key is directly to the left of the 1 key, as it will prioritize the position of the key over the character of the key itself.
![world-debug-views-2103941-szhLVX11II.png](/img/worlds/world-debug-views-2103941-szhLVX11II.png)

## Debug Menu 1

![world-debug-views-9bfe518-VRChat_Q9pMRjUJHI.png](/img/worlds/world-debug-views-9bfe518-VRChat_Q9pMRjUJHI.png)

Debug menu 1 displays some information about your connection to the VRChat API. It is mostly irrelevant to world development.
## Debug Menu 2

![world-debug-views-ae12b10-VRChat_kGTo5o998Y.png](/img/worlds/world-debug-views-ae12b10-VRChat_kGTo5o998Y.png)

Debug menu 2 displays the current build of VRChat that you are using, along with your FPS.
## Debug Menu 3

![world-debug-views-e85c111-VRChat_gk2BOiYkIr.png](/img/worlds/world-debug-views-e85c111-VRChat_gk2BOiYkIr.png)

Debug menu 3 displays your output log. The output log is very useful for world creators because it can show you information such as Udon Behaviour crashes, any information logged via the Debug.Log nodes, and any other potential errors in your world generated by Unity or VRChat.

You can hold the Tab key to activate the mouse cursor and use the buttons on the top to toggle different options on and off, as well as scroll the log output.
## Debug Menu 4

![world-debug-views-0db830e-VRChat_pZdYm9u4ox.png](/img/worlds/world-debug-views-0db830e-VRChat_pZdYm9u4ox.png)

Debug menu 4 displays various stats about other players.
* M: Whether or not the player is the [master of the instance](/worlds/udon/networking#the-instance-master)
* L: Whether or not the player is the local player
* VR: Whether or not the player is in VR
* Ping: The player's ping
* Desrd D: The desired delay that the internal system is targeting. The actual delay is frequently adjusted to find a good balance between latency and smoothness.
* Intrvl: The amount of time between the player sending synced data about themselves.
* G: The current group that the player is in. Grouping in this context is an internal networking system used to combine multiple objects together by distance so that their data can be sent together.
* D: The current delay, or how far back in time you are viewing this player.
## Debug Menu 5

![world-debug-views-1232152-VRChat_SVxaMmgiLC.png](/img/worlds/world-debug-views-1232152-VRChat_SVxaMmgiLC.png)

Debug menu 5 displays some graphs related to networking. They are unlabeled and mostly not useful.
:::caution World debugging views above 5 are restricted

By default, only the world creator can access debug menus above 5. However, you can allow others to see it as well by enabling World Debugging in your world's settings on the website. Don't forget to click "Save Changes"! Other users will need to rejoin the world to be able to access this debug view after World Debugging gets enabled.
:::

![world-debug-views-da70084-unknown_7.png](/img/worlds/world-debug-views-da70084-unknown_7.png)

## Debug Menu 6

![world-debug-views-b0257d6-VRChat_ZRiWB4bTU7.png](/img/worlds/world-debug-views-b0257d6-VRChat_ZRiWB4bTU7.png)

Debug menu 6 displays all the networked objects in your world, along with various stats.
* Owner: The playerid of the owner of the object.
* Group: The current group that this object is in. Grouping in this context is an internal networking system used to combine multiple objects together by distance so that their data can be sent together.
* Sleeping: Whether or not the object is sleeping. Only objects with VRCObjectSync can sleep. Sleeping causes the object to stop transmitting data.
* Delay: The current delay of this object between the owner and the viewer
* Size: The current number of bytes per serialization of this object. Every time it needs to sync, it will send this many bytes.
* Bps: A rough approximation of how many bytes per second this object is using up.
* Since Last: A running counter of how long it has been since the last time this object has sent data.
* Interval: A rough approximation of how many times this object tries to sync per second.
## Debug Menu 7

![world-debug-views-6ea6192-VRChat_pWGDiXUnlh.png](/img/worlds/world-debug-views-6ea6192-VRChat_pWGDiXUnlh.png)

Debug menu 7 displays all the same information as 6, but filtered and sorted to bring objects that have the highest networking impact to the top.
## Debug Menu 8

![world-debug-views-88ea4e6-VRChat_6e9Vhf8jnq.png](/img/worlds/world-debug-views-88ea4e6-VRChat_6e9Vhf8jnq.png)

Debug menu 8 overlays a panel on top of every synced object in the world. Each panel displays various stats about that specific object
* P: Ping of the owner
* Q: Quality of the data (100% is no dropped packets)
* O: PlayerID of the owner of the object
* G: The current group that this object is in. Grouping in this context is an internal networking system used to combine multiple objects together by distance so that their data can be sent together.
* Held: Whether or not this object is held, if it is a pickup
* Status: Displays various things about what this object is doing, such as `Should Sleep`, `Player`, `Held`, or `Discontinuity`


---

# ドキュメント終了

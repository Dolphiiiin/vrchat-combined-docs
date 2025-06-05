# worlds 統合ドキュメント

以下は自動収集された全ドキュメントのコンテンツです。各セクションの始めにメタデータが記載されています。



---

## ドキュメント: creating-your-first-world.md

```metadata
階層: /worlds/creating-your-first-world.md
ディレクトリ: worlds
ファイル名: creating-your-first-world.md
拡張子: .md
サイズ: 8.40 KB
最終更新: 2025-06-05T03:07:52.784Z
```

---
sidebar_position: -2
---
# Creating Your First World

This guide explains how to create and upload a very simple VRChat world. You'll learn the basics of setting up a Unity scene, using VRChat's Control Panel, and publishing your world.

## Requirements

Before you get started, ensure that you meet the following requirements:

- A VRChat account
- Trust rank of "New User" or higher
- A [Unity project with the VRChat SDK](/sdk) set up

:::info

Need help? Visit our Discord at [discord.gg/vrchat](https://discord.gg/vrchat) or our official forum at [ask.vrchat.com](https://ask.vrchat.com).

:::

## Step 1 - Setting up a scene

The first thing you need is a Unity scene. You can either create a new scene, or open an existing Unity scene with pre-existing content. If you created a new project from the Creator Companion, skip to [step 2](/worlds/creating-your-first-world/#step-2---creating-spawn-points).

With the scene open, you need to add a **VRC Scene Descriptor** to your scene. You can easily add it with the VRChat SDK:

![Adding a scene descriptor automatically via the VRChat SDK build panel.](/img/worlds/build-panel-add-vrc-scene-descriptor.png)

1. Click **VRChat SDK** > **Show Control Panel**.
	- If you do not see this menu at the top of your Unity window, your SDK may not be installed correctly. Try clicking **Assets** > **Reimport All**, and check our [SDK troubleshooting guide](/sdk/sdk-troubleshooting).
2. In the **Authentication**, log into your VRChat account.
3. Switch to the **Builder** tab and click **Add a VRCSceneDescriptor**.

![Adding a scene descriptor automatically via the VRChat SDK build panel.](/img/worlds/vrcworld-prefab-in-scene.png)

A Game Object called **VRCWorld** will automatically be added to your scene. It contains a **VRC Scene Descriptor** and other helpful components. Click on VRCWorld in your hierarchy to inspect its settings.

## Step 2 - Creating spawn points

Your world needs at least one spawn point. When players join your world, that's where they'll appear. By default, players will spawn at the location of your VRCWorld prefab. Simply move the VRCWorld prefab to wherever you'd like users to spawn.

![Move your scene descriptor to change your spawn.](/img/worlds/vrc-scene-descriptor-gizmo.png)

If you'd like to create additional spawn points, create an empty Game Object and place it where you want users to appear. Add the Game Object to the **Spawns** list in the [VRC_SceneDescriptor](/worlds/components/vrc_scenedescriptor) component. Do this for as many spawn points as you want.

If you have more than one spawn point, you can choose the order in which people will spawn into them by changing the **Spawn Order** property.

## Step 3 - Descriptor Settings

There are various settings in the [VRC_SceneDescriptor](/worlds/components/vrc_scenedescriptor) that change the behavior of your world. Here are some of the more important ones.

_Spawns_ - An array of transforms where players will spawn when they join your world. By default, players spawn at your Scene Descriptor.

_Respawn Height_ - The vertical height (on the y axis) at which players respawn and pickups are respawned (or destroyed, depending on the "Object Behaviour At Respawn" setting).

_Reference Camera_ - Settings you apply to this camera will be applied to the player when they join the room. It is most often used for adjusting the clipping planes and adding post-processing effects.

More settings can be found on the [VRC_SceneDescriptor](/worlds/components/vrc_scenedescriptor) page.


## Step 4 - Configure your World in the SDK build panel

Click  `VRChat SDK > Show Control Panel`. Before you can upload your world, you need to give VRChat some basic information about it, such as the world's name, capacity, or content warnings.

![VRChat's SDK World build panel.](/img/worlds/build-panel-worlds-2023.png)

- World name - The name of your world, as shown to everyone.
- Description - This will be displayed on the 'World Details' page in VRChat and on the website.
- Content warnings - Warnings that work in conjunction with VRChat's [Content Gating system](https://hello.vrchat.com/blog/content-gating).
- Maximum capacity - The maximum amount of players allowed in your world.
  - If an instance has reached its player capacity, new players cannot join.
  - The instance creator, world creator, or group owner can always join, even if it would exceed the player capacity. (Unless they do not have permission to enter/see that instance)
- Recommended capacity - The recommended maximum amount of players for your world.
  - If a public instance has reached its recommended capacity, VRChat will discourage more players from joining. The instance will stop appearing in VRChat's list of public instances.
  - Players can still try to join the instance under some circumstances if they have a direct invite URL on vrchat.com.
- Tags - Keywords that help users find your world in VRChat.
- World debugging - Allows other users to debug your Udon code.
- Thumbnail - A preview image of your world.

:::note What if my world doesn't have a recommended capacity?

If you uploaded your VRChat world with an old VRChast SDK, without 'recommended capacity', player capacity works differently:

 - 'Recommended capacity' will be the same as your player capacity value
 - 'Player capacity' will be **twice** your player capacity value
 
 For example: If you set 'Player capacity' to 10 and did not set 'Recommended capacity', your _actual_ 'Player capacity' will be 20. 'Player capacity' was sometimes referred to as the 'soft cap' for this reason.

:::

## Step 5 - Check for warnings or validation messages

![Validations in the SDK build panel.](/img/worlds/build-panel-validations-everything-looks-good.png)

At the bottom of the VRChat SDK build panel, you'll find a section called **Validations**. It contains suggestions on how to set up your scene and build your world. For example:

- Is your scene missing a VRC Scene Descriptor?
- Is your scene missing VRChat's layers and collision layer matrix?
- Are there any issues with Audio Sources, textures, or Udon scripts?

The SDK will often give you the option to fix these issues automatically. If not, please read the validation messages carefully to learn how to improve your world. Some of the messages are optional and are not required for uploading your world.

## Step 6 - Building and publishing your World

Next, you need to build the world! You'll need to choose what you will be doing first: You can either make a **test build** to test your world without uploading it or **publish** your world directly to VRChat. Under both "Offline Testing" and "Online Publishing" headings, you will find buttons to publish a new build or your last build. Last Build takes the last successful build of the world to either test or upload. New Build puts a new world together to either test or upload.

![Validations in the SDK build panel.](/img/worlds/build-panel-upload-or-test.png)

_(Optional)_  
If you wish to test your world, go to **Offline Testing** and click **Build & Test New Build**. This will build a new version of your world and launch into the world in VRChat. The **Number of Clients** option is used when you want to open multiple VRChat windows for testing your world with multiple players.

Once you're ready to publish your world, go to **Online Publishing** and click **Build and Upload**! This will build you world and get it ready for upload. Keep in mind that you're not permitted to upload content to VRChat that violates our [Community Guidelines](https://vrchat.com/community-guidelines) or [Terms of Service](https://vrchat.com/legal). Doing so will result in moderation action.

After uploading your world, it will become available in VRChat! You should able to see it in-game or via the content manager in the SDK via `VRChat SDK > Show Control Panel > Content Manager`.

:::caution World Upload Failures

If your world fails to upload, [check Unity's console](https://docs.unity3d.com/Manual/Console.html) to see if there are any errors. If so, then solve them before trying to build your world again. Make sure to read the entirety of Unity's log, and click on errors to see additional information.

Check our other documentation, the [Ask Forum](https://ask.vrchat.com/),  or ask on [Discord](https://discord.com/invite/vrchat) if you need help. Make sure to provide as much information as possible, such as Unity console errors.

:::


---

## ドキュメント: index.md

```metadata
階層: /worlds/index.md
ディレクトリ: worlds
ファイル名: index.md
拡張子: .md
サイズ: 645 B
最終更新: 2025-06-05T03:07:52.800Z
```

---
sidebar_position: 1
---

# Worlds
In VRChat, you can create and visit custom worlds, powered by Unity!

- To get started, check out [Creating Your First World](/worlds/creating-your-first-world).
- Learn about [Udon](/worlds/udon), VRChat's programming language.
- Use [ClientSim](/worlds/clientsim/) to test your worlds in Unity.
- [Allowed World Components](/worlds/whitelisted-world-components) 
- [Supported Scripted Assets](/worlds/supported-assets) 
- [World Creation, Optimization, and Community Labs Tips](/worlds/submitting-a-world-to-be-made-public)

import DocCardList from '@theme/DocCardList';

<DocCardList />


---

## ドキュメント: layers.md

```metadata
階層: /worlds/layers.md
ディレクトリ: worlds
ファイル名: layers.md
拡張子: .md
サイズ: 11.08 KB
最終更新: 2025-06-05T03:07:52.801Z
```

import UnityVersionedLink from '@site/src/components/UnityVersionedLink.js';

# Unity Layers in VRChat 

<UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/Layers.html">Layers</UnityVersionedLink> are used in Unity to organize your Game Objects, determine <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/LayerBasedCollision.html">collisions</UnityVersionedLink> and <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/ScriptReference/Physics.Raycast.html">Raycasts</UnityVersionedLink> between Game Objects, selectively <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/ScriptReference/Camera-cullingMask.html">render</UnityVersionedLink> parts of the scene, and more.

**You can freely use most Layers in your VRChat world.** Some Layers are shared and used by Unity and VRChat.

When you create a Unity project with VRChat's Worlds SDK, your project will automatically be configured to use VRChat's built-in layers. If you change the collision matrix, rename, or remove built-in layers, your changes will be overridden when you upload your world to VRChat.

Layers 22-31 are unused 'User' Unity layers. You can edit them freely, and changes to these layers will not be discarded when you build & upload your world.

## Unity's built-in layers

The table below lists Unity's built-in layers, how they are used in VRChat, and whether users can place [stickers](#stickers) on them.

| Layer number | Layer Name          | Description                                                                                                                                                                                                                                            | Can have stickers |
| :----------- | :------------------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------- |
| 0            | Default             | Used for Unity's Game Objects by default. Used by VRChat's Avatar Pedestals.                                                                                                                                                                           | ✔                 |
| 1            | TransparentFX       | Used for Unity's Flare Assets.                                                                                                                                                                                                                         | ❌                 |
| 2            | IgnoreRaycast       | Ignored by Unity's Physics Raycasts if no layer mask is provided. Not ignored by VRChat's Physics Raycasts.                                                                                                                                            | ❌                 |
| 3            | reserved3           | ⚠ Avoid using this layer. Reserved by VRChat. When you upload your world, any Game Object on a reserved Layer will be moved to Layer 0 (Default).                                                                                                      | ❌                 |
| 4            | Water               | Used by Unity's Standard Assets. Used by VRChat's Portals. Used by VRChat's Mirrors. Often used for Unity's Post Processing.                                                                                                                           | ✔                 |
| 5            | UI                  | ⚠ You may not want to use this layer. Used by Unity's UI by default. Ignored by VRChat's UI pointer unless the player has the VRChat menu open. Ignored by the VRChat's camera unless 'UI' is enabled in the camera.                                   | ❌                 |
| 6            | reserved6           | ⚠ Avoid using this layer. Reserved by VRChat. When you upload your world, any Game Object on a reserved Layer will be moved to Layer 0 (Default).                                                                                                      | ❌                 |
| 7            | reserved7           | ⚠ Avoid using this layer. Reserved by VRChat. When you upload your world, any Game Object on a reserved Layer will be moved to Layer 0 (Default).                                                                                                      | ❌                 |
| 8            | Interactive         | Unused by Unity and VRChat.<br />Use this layer for colliders where you don't want users to place [stickers](#stickers).                                                                                                                               | ❌                 |
| 9            | Player              | Used for VRChat's players, except the local player.                                                                                                                                                                                                    | ❌                 |
| 10           | PlayerLocal         | Used by VRChat to render the local player. Humanoid avatars are rendered without their head bone.                                                                                                                                                      | ❌                 |
| 11           | Environment         | Unused by Unity and VRChat.                                                                                                                                                                                                                            | ✔                 |
| 12           | UiMenu              | ⚠ Avoid using this layer. Used by VRChat's nameplates. Ignored by VRChat's UI pointer unless the player has the VRChat menu open.                                                                                                                      | ❌                 |
| 13           | Pickup              | Used by VRChat's Pickups by default when you add a pickup component. Does not collide with players.                                                                                                                                                    | ❌                 |
| 14           | PickupNoEnvironment | Colliders on this Layer only collide with 'Pickup.'                                                                                                                                                                                                    | ❌                 |
| 15           | StereoLeft          | Unused by Unity and VRChat.                                                                                                                                                                                                                            | ❌                 |
| 16           | StereoRight         | Unused by Unity and VRChat.                                                                                                                                                                                                                            | ❌                 |
| 17           | Walkthrough         | Colliders on this layer do not collide with players.                                                                                                                                                                                                   | ✔                 |
| 18           | MirrorReflection    | Used by VRChat to render the local player in Mirrors. <br />Renderers on this Layer will only appear in Mirrors. <br />Renderers on this Layer are not rendered in VRChat's main camera.<br /> Colliders on this Layer do not block VRChat's Raycasts. | ❌                 |
| 19           | InternalUI          | ⚠ Avoid using this layer. Used by VRChat for internal UI elements such as debug consoles.                                                                                                                                                              | ❌                 |
| 20           | HardwareObjects     | ⚠ Avoid using this layer. Used by VRChat to render virtual representations of physical hardware in-game i.e. controllers and trackers                                                                                                                  | ❌                 |
| 21           | reserved4           | ⚠ Avoid using this layer. Reserved by VRChat. When you upload your world, any Game Object on a reserved Layer will be moved to Layer 0 (Default).                                                                                                      | ❌                 |
| 22-30        |                     | Unused by Unity and VRChat. VRChat will not override the name and collision matrix of these Layers in uploaded worlds.                                                                                                                                 | (✔)[^1]           |
| 31           |                     | ⚠ Avoid using this layer. Used by Unity's Editor’s Preview window mechanics.                                                                                                                                                                           | (✔)[^1]           |

[^1]: Layers 22-31 are enabled for Stickers by default. You can disable them by adding them to the [Interaction Passthrough](/worlds/layers#interaction-passthrough-for-user-layers) list.

## Stickers

Stickers allow [VRChat+](https://hello.vrchat.com/vrchatplus) users to place images on `Collider` components in your world.

Stickers can't be placed on [all layers](#unitys-built-in-layers). If you don't want users to place stickers on a `Collider` component, change its layer. We recommend using layer 8 ("Interactive") because it is unused by VRChat and Unity.

If you don't want to allow stickers anywhere, edit your world on the [VRChat Website](https://vrchat.com/home/content/worlds) and disable stickers.

## Interaction Block and Passthrough on VRChat Layers

Interaction (grabbing an item from a distance, toggling a UI element with the laser) is blocked by most VRChat layers. The following layers are transparent to interaction and allow you to interact through them:
 - UiMenu
 - UI
 - PlayerLocal
 - MirrorReflection

## Interaction Passthrough for User Layers

Interaction through User layers is blocked by default. Use the "Interact Passthrough" mask to define layers that will be transparent to interaction (allow interactions to pass through). Note that collision test rays originate differently from Desktop/Mobile players (inside the player capsule) versus VR players (from the user's tracked hand). This means that VR players can penetrate colliders with their hand even when the player collider is blocked. Those same colliders will therefore not block interaction from the VR player, since the hand has penetrated.


---

## ドキュメント: sdk-prefabs.md

```metadata
階層: /worlds/sdk-prefabs.md
ディレクトリ: worlds
ファイル名: sdk-prefabs.md
拡張子: .md
サイズ: 2.29 KB
最終更新: 2025-06-05T03:07:52.801Z
```

---
title: "SDK Prefabs"
slug: "sdk-prefabs"
hidden: false
createdAt: "2022-03-03T00:33:18.460Z"
updatedAt: "2022-03-03T00:38:28.712Z"
---
This is a list of prefabs available in our SDKs!

## VRCAvatarPedestal
An example of how to create an avatar pedestal that you click on to wear a new avatar using Udon.

Found in `Packages > VRChat SDK - Worlds > Samples > UdonExampleScene > Prefabs > AvatarPedestal`.

## VRCChair
An example of how to create a chair that you click on to sit in using Udon.

Found in `Packages > VRChat SDK - Worlds > Samples > UdonExampleScene > Prefabs > VRCChair > VRCChair3`.

## VRCMirror
An example of how to create the ever-popular Mirror using Udon.

Found in `Packages > VRChat SDK - Worlds > Samples > UdonExampleScene > Prefabs > VRCMirror`.

## VRCPortalMarker
An example of how to use [VRC_PortalMarker](/worlds/components/vrc_portalmarker).

Found in `Packages > VRChat SDK - Worlds > Samples > UdonExampleScene > Prefabs > VRCPortalMarker`.

This prefab **must** be at the root of your scene hierarchy for the portal's destination instance to be synced with other players.

## VRCWorld
An example of how to use [VRC_SceneDescriptor](/worlds/components/vrc_scenedescriptor) to define a VRChat World.

Found in  `Packages > VRChat SDK - Worlds > Samples > UdonExampleScene > Prefabs > World`.

## VRCVideoSync
An example of how to create an Udon-powered [Video Player](/worlds/udon/video-players/). 

Found in `Packages > VRChat SDK - Worlds > Samples > UdonExampleScene > Prefabs > VideoPlayers`.

Two examples exist:
- `UdonSyncPlayer (AVPro).prefab` - Uses AVPro, which permits watching livestreams.
- `UdonSyncPlayer (Unity).prefab` - Uses Unity's built-in video player, which may be more stable in some situations.

## Simple Pen System
An example of how to create an Udon-powered pen for drawing in 3D space!

Documented here: [Simple Pen System](/worlds/examples/udon-example-scene/simple-pen-system)

Found in `Packages > VRChat SDK - Worlds > Samples > UdonExampleScene > Prefabs > SimplePenSystem`.

## Udon Variable Sync
A self-documenting example on how Udon Variable Sync works! Drop it into your world and test it out to see how it works.

Found in `Packages > VRChat SDK - Worlds > Samples > UdonExampleScene > Prefabs > Udon Variable Sync`.

---

## ドキュメント: submitting-a-world-to-be-made-public.md

```metadata
階層: /worlds/submitting-a-world-to-be-made-public.md
ディレクトリ: worlds
ファイル名: submitting-a-world-to-be-made-public.md
拡張子: .md
サイズ: 8.57 KB
最終更新: 2025-06-05T03:07:52.802Z
```

---
title: "World Creation, Optimization, and Community Labs Tips"
slug: "submitting-a-world-to-be-made-public"
sidebar_position: -1
hidden: false
createdAt: "2018-12-29T00:05:06.003Z"
updatedAt: "2023-04-21T14:54:14.165Z"
---
Want to make your world public? You've come to the right place! You need to submit your world to **Community Labs**.

There are a few things you should take into consideration before submitting your world to Community Labs. **Make sure you read all of this!** Failure to do so may result in your world being taken down, or never leaving Community Labs.

You can submit your world to Community Labs on VRChat.com (Edit World Info -> Danger Zone -> World Visibility -> Publish) or whenever you upload a new version in Unity.

Publishing your world will make it immediately available to all users that opt-in for Community Labs. Eventually, your world will go public and become accessible to all users outside Community Labs! [Read more about Labs here.](https://docs.vrchat.com/docs/vrchat-community-labs)

## Important Info

- **You can only submit one world per user per seven days to Community Labs.** 
- **You can update your world as often as you like.** Just push an update! It won't change the status of your world.
- **If your world is already Public, you don't need to re-submit the world if you update it.** It should update automatically and you will not lose your Public status.
- If your world or any content in the world (videos, avatars, images) violates the VRChat Terms of Service or the Community Guidelines, your ability to submit worlds to Community Labs will be suspended for a period of time. Repeated suspensions may result in in-app moderation action.
- **Content Warnings are deprecated and not used at this time.** You cannot upload content to VRChat that violates our [Community Guidelines](https://vrchat.com/community-guidelines) or [Terms of Service](https://vrchat.com/legal). Doing so (even if you have checked off a content warning) will result in moderation action.
- We do not approve worlds via Discord DMs, emails to VRChat, or any other channel.
- If your world is _very large in filesize_, we may ask you to reduce the size of the world and remove it from Public in the meantime. **Try to keep your worlds under 200MB.**

## Avatar Worlds / All Avatar Pedestals in any World

- **Avatars on pedestals are expected to be "reasonably optimized."** Check out our [Avatar Optimization Docs](/avatars/avatar-optimizing-tips) for more details.  
  Avoid sharing very poor avatars. This applies to all worlds, not just avatar worlds. If avatars in your world have severe performance issues, your world may be removed from Public or Community Labs.
- **If you upload a world with placeholder avatars and replace them with TOS-violating avatars after being made public, you will be suspended from submitting worlds for a month. You may be moderated in-app, depending on the offense severity.**
- **If you have an avatar world, none of your avatars may violate TOS/Community Guidelines.**
- Look into using [Cat's Blender Plugin](https://github.com/absolute-quantum/cats-blender-plugin) and Shotariya's Texture Combiner addons for Blender to optimize your models. 

## Performance Tips

- **Aim for at least 45 FPS with a single VR user at the spawn.** If you do not have VR, have a friend test the world for you. Having a badly performing world will mean people don't spend time in your world, and you probably won't make it out of Labs very easily.
- **Don't use shaders that are not VR-compatible.** Shaders must support single-pass stereo rendering. If you are looking for a good water shader, [check out Silent's Water Shader](https://gitlab.com/s-ilent/clear-water).
- **Use mobile shaders on Android.** Most shaders will _work_ on Android but usually take more processing power to render. Stick to mobile shaders if you can.
- **Use a super-sampled shader for in-world UI.** This is a performance friendly way of achieving crisp-looking text in VR. The SDK will suggest switching over if it finds any UI materials using Unity's built-in UI shader.
- **Be very careful with post-processing effects.** Some screen-space post-processing effects cause major issues for VR users. In particular, be careful with chromatic aberration, screen-space reflection, and screen-space ambient occlusion. 
- **Bad things happen when you put more than 2 video players in a room.** It usually impacts performance negatively.
- **Bad things also happen when you put more than 1 mirror in your room.** Mirrors severely affect a world's performance. If you have 1 mirror in the room, make sure to set it to toggle.
- **We** **_STRONGLY SUGGEST_** **not enabling mirrors by default.** Add a toggle that can be activated by players or activated automatically when players enter a certain area.
- **Do NOT overuse real-time lights.** They are **very** expensive and can kill your world's performance if used incorrectly.
- **Baking your lighting is exceedingly important and can give you huge performance gains.** You can learn more about light baking in [Unity's documentation](https://docs.unity3d.com/Manual/progressive-lightmapper.html) or  [Android's development guide](https://developer.android.com/games/optimize/lighting-for-mobile-games-with-unity#lightmap-baking).

## General Tips

- Test your world! It isn't uncommon for us to see worlds where you immediately drop out of the portal forever.
- Test your world in VR, as well. Check to ensure your shaders are working properly and display properly in VR. If you don't have a VR headset, ask a friend to take a look around.
- **_TEST YOUR LIGHTING!_** Lighting a world is very important and doing it properly is wildly important. Don't just test using Toon shaders as they do not represent lighting properly, use Standard or a PBR shader to see how lighting affects it. If you look blown-out, you probably have too many lights, your intensity is too high, or you need to look into using Tonemapping.
- Want to make your world private again?  Edit your world on the website and you can set it to Private.
- Avoid directly using `.blend` files. Exporting FBX files from Blender for use in VRChat usually causes fewer issues.

If you have any questions about the process, [visit our forum](https://ask.vrchat.com/c/worlds/27) or email hello@vrchat.com with your question.  If you run an event or have a highly trafficked world in the app and need a world made public at a different time, please reach out to us via email at least 48 hours in advance.

## Submitting to Community Labs

Once you've read everything above, submit your new world to Community Labs! If you're curious about how Community Labs works, check out our [VRChat Community Labs](https://docs.vrchat.com/docs/vrchat-community-labs) documentation.

## Becoming a Game or Avatar World

If you want your world to be categorized as an Avatar World or Game World, just add the appropriate tag during upload.


:::caution Don't abuse the world rows

These rules are in place to give **all** worlds a chance to be discovered. Utilizing "SEO-like" techniques is not permitted and will result in actions such as tag removal, or in repeated/worse cases, moderation of the author.

VRChat reserves the right to action users who abuse our systems to unfairly or misleadingly promote their own content.

:::

### Avatar Worlds

An Avatar World is a world where sharing a variety of avatars is the primary focus. In an Avatar World, avatars are quick and easy to find. Avatars shouldn't be an afterthought or a late addition.

To categorize your world as an Avatar World, its title must include one of the terms "avatar", "avatars", "avi", or "avis". 

By including one of these terms in your world's title, it will show up in the "Avatar World" category in VRChat's menu, making it easier for users to discover.

:::caution Don't miscategorize your world

Does your world contain avatars, but it's not an Avatar World?

Add the tag "avatar" to your world instead. It won't be categorized as an Avatar World, but users can still find it by searching for "avatar".

If avatars are not the primary focus of your world, don't include terms like "avatar" in the world's title. Your world may be hidden from certain categories, making it more difficult for users to discover.

:::

### Game Worlds

**A Game World is a world where playing a game or set of games is the** **_primary focus._** Playing games in worlds tagged as Game Worlds should be quick and easy, and should not be an afterthought or "addition" to another clearly primary functionality of the world.

To categorize your world as a game world, add the tag `game`.


---

## ドキュメント: supported-assets.md

```metadata
階層: /worlds/supported-assets.md
ディレクトリ: worlds
ファイル名: supported-assets.md
拡張子: .md
サイズ: 1.79 KB
最終更新: 2025-06-05T03:07:52.802Z
```

---
title: "Supported Scripted Assets"
slug: "supported-assets"
hidden: false
createdAt: "2017-08-06T01:53:56.686Z"
updatedAt: "2023-02-24T00:48:47.712Z"
---
You can use certain external assets that use scripts in VRChat worlds.

To see a list of precise components permitted in VRChat worlds, please see [Allowlisted World Components](/worlds/whitelisted-world-components). Most of these components (except TextMeshPro) are unavailable on Quest.

## TextMeshPro
"TextMesh Pro is the ultimate text solution for Unity. It's the perfect replacement for Unity's UI Text & Text Mesh."

As of Unity 2018, TextMesh Pro is a built-in component of Unity. We strongly recommend TextMeshPro over Unity's built-in text components, as it delivers high-quality text in any font size or screen size.

## Post Processing Stack v2
Import from Package Manager.

We strongly suggest checking out [Silent's Post Processing guide](https://gitlab.com/s-ilent/SCSS/wikis/Other/Post-Processing) for more info and best practices.
:::caution 

Do not import the `Test` folder when importing post-processing. It will cause script errors which will prevent you from uploading the world.
:::
"Post-processing is the process of applying full-screen filters and effects to a camera’s image buffer before it is displayed to screen. It can drastically improve the visuals of your product with little setup time."

## [Final IK](https://assetstore.unity.com/packages/tools/animation/final-ik-14290)
"The final Inverse Kinematics solution for the game developer."

## [Dynamic Bone](https://assetstore.unity.com/packages/tools/animation/dynamic-bone-16743)
"Dynamic Bone applies physics to objects. With simple setup, your object will move realistically."

*Works up to v1.3.0, currently supplied version on asset store may not work.


---

## ドキュメント: whitelisted-world-components.md

```metadata
階層: /worlds/whitelisted-world-components.md
ディレクトリ: worlds
ファイル名: whitelisted-world-components.md
拡張子: .md
サイズ: 6.72 KB
最終更新: 2025-06-05T03:07:52.835Z
```

import UnityVersionedLink from '@site/src/components/UnityVersionedLink.js';

# Allowlisted World Components

The following is the complete list of scripts usable within worlds. Components that are not in this list will not work.
:::caution Oculus Quest

The Android version of VRChat has some exceptions to this list. Check [here](/platforms/android/quest-content-limitations#components) for more info.
:::
## Unity Components
- Animator
- AudioChorusFilter
- AudioDistortionFilter
- AudioEchoFilter
- AudioHighPassFilter
- AudioLowPassFilter
- AudioReverbFilter
- AudioReverbZone
- AudioSource
- BillboardRenderer
- BoxCollider
- Camera
- Canvas
- CanvasGroup
- CanvasRenderer
- CapsuleCollider
- CharacterJoint
- Cloth
- Collider
- ConfigurableJoint
- ConstantForce
- EllipsoidParticleEmitter
- FixedJoint
- FlareLayer
- GUIElement
- GUILayer
- Grid
- GridLayout
- Halo
- HingeJoint
- Joint
- LODGroup
- LensFlare
- Light
- LightProbeGroup
- LightProbeProxyVolume
- LineRenderer
- MeshCollider
- MeshFilter
- MeshParticleEmitter
- MeshRenderer
- NavMeshAgent
- NavMeshObstacle
- OcclusionArea
- OcclusionPortal
- OffMeshLink
- ParticleAnimator
- ParticleEmitter
- ParticleRenderer
- ParticleSystem
- ParticleSystemRenderer
- PlayableDirector
- Projector
- RectTransform
- ReflectionProbe
- Rendering.SortingGroup
- Rigidbody
- SkinnedMeshRenderer
- Skybox
- SphereCollider
- SpringJoint
- SpriteMask
- SpriteRenderer
- Terrain
- TerrainCollider
- TextMesh
- Tilemap
- TilemapRenderer
- TrailRenderer
- Transform
- Tree
- VideoPlayer
- WheelCollider
- WorldParticleCollider
- WindZone

### Unity Components

:::info

VRChat is currently working on [VRCConstraint](https://feedback.vrchat.com/avatar-30/p/vrcconstraint-optimized-replacement-for-unity-constraints), an optimized replacement for Unity's Constraints. 

:::

- <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/class-AimConstraint.html">AimConstraint</UnityVersionedLink>
- <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/class-LookAtConstraint.html">LookAtConstraint</UnityVersionedLink>
- <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/class-ParentConstraint.html">ParentConstraint</UnityVersionedLink>
- <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/class-ParticleSystemForceField.html">ParticleSystemForceField</UnityVersionedLink>
- <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/class-PositionConstraint.html">PositionConstraint</UnityVersionedLink>
- <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/class-RotationConstraint.html">RotationConstraint</UnityVersionedLink>
- <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/class-ScaleConstraint.html">ScaleConstraint</UnityVersionedLink>

## VRChat Components
- [VRC_AvatarPedestal](/worlds/components/vrc_avatarpedestal)
- [VRC_IKFollower](https://docs.vrchat.com/docs/vrc_ikfollower) - Deprecated. Use 
<UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/Constraints.html">Constraints</UnityVersionedLink> instead!
- [VRC_MidiListener](/worlds/udon/midi/realtime-midi/)
- [VRC_MirrorReflection](/worlds/components/vrc_mirrorreflection)
- [VRCPipelineManager](/sdk/vrcpipelinemanager)
- [VRC_PortalMarker](/worlds/components/vrc_portalmarker)
- [VRC_SceneDescriptor](/worlds/components/vrc_scenedescriptor)
- [VRC_SpatialAudioSource](/worlds/components/vrc_spatialaudiosource)
- [VRC_Station](/worlds/components/vrc_station)
- [VRC_UiShape](/worlds/components/vrc_uishape)

## Dynamic Bone
- DynamicBone
- DynamicBoneCollider

## Text Mesh Pro
- TMP_Dropdown
- TMP_InputField
- TMP_ScrollbarEventHandler
- TMP_SelectionCaret
- TMP_SpriteAnimator
- TMP_SubMesh
- TMP_SubMeshUI
- TMP_Text
- TextContainer
- TextMeshPro
- TextMeshProUGUI

## Unity Event System
- BaseInput
- BaseInputModule
- BaseRaycaster
- EventSystem
- EventTrigger
- PhysicsRaycaster
- PointerInputModule
- StandaloneInputModule
- TouchInputModule
- UIBehaviour

## Unity UI
- AspectRatioFitter
- BaseMeshEffect
- Button
- CanvasScaler
- ContentSizeFitter
- Dropdown
- Dropdown
- Graphic
- GraphicRaycaster
- GridLayoutGroup
- HorizontalLayoutGroup
- HorizontalOrVerticalLayoutGroup
- Image
- InputField
- LayoutElement
- LayoutGroup
- Mask
- MaskableGraphic
- Outline
- PositionAsUV1
- RawImage
- RectMask2D
- ScrollRect
- Scrollbar
- Selectable
- Shadow
- Slider
- Text
- Toggle
- ToggleGroup
- VerticalLayoutGroup

## Post Processing Stack V2
:::caution Post Processing Stack v1

PPSv1 is not supported in either VRCSDK2 or VRCSDK3. It has been deprecated by Unity.
:::
- PostProcessDebug
- PostProcessLayer
- PostProcessVolume

## AVPro
- ApplyToMaterial
- ApplyToMesh
- AudioOutput
- DisplayIMGUI
- DisplayUGUI
- MediaPlayer
- SubtitlesUGUI

## Oculus Spatializer Unity
- ONSPAmbisonicsNative
- ONSPAudioSource
- ONSPReflectionZone
- OculusSpatializerUnity

## Final IK

:::caution FinalIK Components Modified

VRChat has highly modified its implementation of FinalIK. As such, these components may not work as documented.

We do not directly support or test custom FinalIK implementations in worlds. However, they *should* work fine, and if we must intentionally break one or more of these, we will try our best to inform creators.

If you discover a bug, please [let us know](https://feedback.vrchat.com).
:::

- AimIK
- AimPoser
- Amplifier
- AnimationBlocker
- BehaviourBase
- BehaviourFall
- BehaviourPuppet
- BipedIK
- BipedRagdollCreator
- BodyTilt
- CCDIK
- FABRIK
- FABRIKRoot
- FBBIKArmBending
- FBBIKHeadEffector
- FingerRig
- FullBodyBipedIK
- GenericPoser
- Grounder
- GrounderBipedIK
- GrounderFBBIK
- GrounderIK
- GrounderQuadruped
- GrounderVRIK
- HandPoser
- HitReaction
- HitReactionVRIK
- IK
- IKExecutionOrder
- Inertia
- InteractionObject
- InteractionSystem
- InteractionTarget
- InteractionTrigger
- JointBreakBroadcaster
- LegIK
- LimbIK
- LookAtIK
- MuscleCollisionBroadcaster
- OffsetModifier
- OffsetModifierVRIK
- OffsetPose
- Poser
- PressureSensor
- Prop
- PropRoot
- PuppetMaster
- PuppetMasterSettings
- RagdollCreator
- RagdollEditor
- RagdollUtility
- Recoil
- RotationLimit
- RotationLimitAngle
- RotationLimitHinge
- RotationLimitPolygonal
- RotationLimitSpline
- ShoulderRotator
- SolverManager
- TriggerEventBroadcaster
- TrigonometricIK
- TwistRelaxer
- VRIK

---

# ドキュメント終了

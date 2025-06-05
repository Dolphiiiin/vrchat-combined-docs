# components 統合ドキュメント

以下は自動収集された全ドキュメントのコンテンツです。各セクションの始めにメタデータが記載されています。



---

## ドキュメント: index.md

```metadata
階層: /worlds/components/index.md
ディレクトリ: worlds\components
ファイル名: index.md
拡張子: .md
サイズ: 1.76 KB
最終更新: 2025-06-05T03:07:52.777Z
```

---
title: "Scene Components"
hidden: false
createdAt: "2017-07-11T20:33:31.318Z"
updatedAt: "2021-10-20T20:03:08.484Z"
---

Every Unity scene you'd like to bring into VRChat requires a [VRC_SceneDescriptor](/worlds/components/vrc_scenedescriptor) component. The VRChat Worlds SDK contains various other components to allow your users to interact with your world, pick up objects, see themselves in a mirror, and more.

Please consult [Allowlisted World Components](/worlds/whitelisted-world-components) for a full list of components available in the VRChat SDK.

| Parameter | Description |
| --- | --- |
| [TextMesh Pro](/worlds/components/textmeshpro) | Used to display high-quality text. |
| [VRC_AvatarPedestal](/worlds/components/vrc_avatarpedestal) | Used to display and / or switch to an avatar. |
| [VRC_MirrorReflection](/worlds/components/vrc_mirrorreflection) | Used to create a mirror in VRChat. |
| [VRC_ObjectSync](/worlds/components/vrc_objectsync) | Syncs the transform of a GameObject with all players in the instance. |
| [VRC_Pickup](/worlds/components/vrc_pickup) | Allows objects to be picked up, held and used by players. |
| [VRC_PortalMarker](/worlds/components/vrc_portalmarker) | Creates portals to other VRChat worlds. |
| [VRC_SceneDescriptor](/worlds/components/vrc_scenedescriptor) | Describes your VRChat world. Required for every Unity scene you'd like to use as a VRChat world. |
| [VRC_SpatialAudioSource](/worlds/components/vrc_spatialaudiosource) | Adds 3D spatialization to a Unity AudioSource. Usually automatically added with AudioSource components in editor. |
| [VRC_Station](/worlds/components/vrc_station) | Allow users to sit down. |
| [VRC_UIShape](/worlds/components/vrc_uishape) | Allow users to interact with Unity's UI system. |


---

## ドキュメント: vrcenablepersistence.md

```metadata
階層: /worlds/components/vrcenablepersistence.md
ディレクトリ: worlds\components
ファイル名: vrcenablepersistence.md
拡張子: .md
サイズ: 344 B
最終更新: 2025-06-05T03:07:52.783Z
```

---
title: "VRC Enable Persistence"
slug: "vrc_enablepersistence"
---
Add this component to a [PlayerObject](/worlds/udon/persistence/player-object) to persist all of its synced variables, as well as the synced variables of any UdonBehaviours on children of the PlayerObject.

It requires a PlayerObject component on the same game object.

---

## ドキュメント: vrc_avatarpedestal.md

```metadata
階層: /worlds/components/vrc_avatarpedestal.md
ディレクトリ: worlds\components
ファイル名: vrc_avatarpedestal.md
拡張子: .md
サイズ: 866 B
最終更新: 2025-06-05T03:07:52.779Z
```

# VRC Avatar Pedestal

Displays an avatar and allows users interact with the pedestal to switch into the avatar.

You can find an example in the SDK named [AvatarPedestal](/worlds/examples/udon-example-scene#avatarpedestal).

| Parameter            | Description                                                                   |
|----------------------|-------------------------------------------------------------------------------|
| Blueprint Id         | Blueprint Id of the avatar to be shown                                        |
| Placement(optional)  | Transform to display the avatar on                                            |
| Change Avatar On Use | If set to true,switches the user to the avatar set on the pedestal when used. |
| Scale                | How big or small the avatar should be,only affects the pedestal avatar        |

---

## ドキュメント: vrc_mirrorreflection.md

```metadata
階層: /worlds/components/vrc_mirrorreflection.md
ディレクトリ: worlds\components
ファイル名: vrc_mirrorreflection.md
拡張子: .md
サイズ: 2.73 KB
最終更新: 2025-06-05T03:07:52.779Z
```

---
title: "VRC Mirror Reflection"
slug: "vrc_mirrorreflection"
hidden: false
createdAt: "2017-07-06T06:10:45.478Z"
updatedAt: "2022-08-17T20:23:01.162Z"
---
This component can be used to create a mirror in your VRChat world.

It requires a [mesh renderer component](https://docs.unity3d.com/Manual/class-MeshRenderer.html) on the same game object. It writes to the `_MainTex` value of the mesh renderer's first material. You can find an example in the [SDK prefabs](/worlds/sdk-prefabs#vrcmirror) called `VRCMirror.prefab`.

| Parameter | Description |
| --- | --- |
| Disable Pixel Lights | Disables real-time pixel shaded point and spot lighting. Pixel shaded lights will fall-back to vertex lighting when this is enabled. |
| Turn Off Mirror Occlusion | Disables occlusion culling on the mirror. Enable this if you see objects flickering in the mirror. |
| Reflect Layers | Only objects on the selected layers will be rendered in the mirror. Objects on the Water layer are never rendered in mirrors. |
| Mirror Resolution | Rendering resolution of the mirror (per eye in VR). Auto renders at the same resolution as the user's HMD or monitor up to the maximum of 2048x2048[^1]. |
| Maximum Antialiasing | The maximum level of MSAA applied to the image rendered in the mirror. Can be overruled by client graphics settings. |
| Custom Shader | The mirror will use this shader instead of the default shader if one is provided. |
| Camera Clear Flags | Specifies the CameraClearFlags that the mirror will use to clear the background before rendering. The default "From Reference Camera" will use the same flags as the camera rendering the mirror plane. |
| Custom Skybox | If "Camera Clear Flags" is set to "Custom Skybox," this skybox will be shown in the mirror. If "Custom Skybox" mode is selected but nothing is provided, the background will be black. |
| Custom Clear Color | If "Camera Clear Flags" is set to "Solid Color," this color will be used as the background. Note that the alpha channel will be respected, so you can use this to clear alpha and use it in a custom shader (e.g., for cutout-style mirrors). |

Mirrors can drastically reduce the framerate of your VRChat world. To avoid this, try the following:
- Keep mirrors off by default. Enable them automatically when users get near, or allow users to enable them manually.
- Don't reflect every layer, or allow users to choose which layers to reflect. ("High quality" and "low quality" mirrors.)
- If your users still experience performance issue, reduce the mirror resolution.

[^1]: Displays with a resolution higher than 2048px suffer from reduced mirror quality. Users can switch their mirror resolution to 'Unlimited' in VRChat's settings. This improves quality, but reduces VRChat's performance.

---

## ドキュメント: vrc_objectsync.md

```metadata
階層: /worlds/components/vrc_objectsync.md
ディレクトリ: worlds\components
ファイル名: vrc_objectsync.md
拡張子: .md
サイズ: 1.50 KB
最終更新: 2025-06-05T03:07:52.780Z
```

---
title: "VRC Object Sync"
slug: "vrc_objectsync"
hidden: false
createdAt: "2017-07-07T19:28:16.383Z"
updatedAt: "2021-10-20T20:08:39.807Z"
---
The VRC Object Sync component synchronizes the transform of a GameObject with all players in the instance. It synchronizes the object's:

- position
- rotation
- kinematic state (see `SetKinematic`)
- and gravity state (see `SetGravity`).

## Properties

| Parameter | Description |
| --- | --- |
| Allow Collision Ownership Transfer | If checked, ownership of the object will transfer if it collides with another object owned by another player. |
| Force Kinematic On Remote | If checked, the attached rigidbody will be forced into kinematic mode for all non-owning players. |

## Methods

| Name | Summary |
| --- | --- |
| SetKinematic(bool value) | Changes the kinematic state, usually handled by the Rigidbody of the object but controlled here for sync purposes. When the kinematic state is on, this Rigidbody ignores forces, collisions and joints. |
| SetGravity(bool value) | Changes the gravity state, usually handled by the Rigidbody of the object but controlled here for sync purposes. |
| FlagDiscontinuity() | Trigger this when you want to teleport the object - the changes you make this frame will be applied without smoothing. |
| TeleportTo([Transform](https://docs.unity3d.com/ScriptReference/Transform.html) targetLocation) | Moves the object to the specified location. |
| Respawn() | Moves the object back to its original spawn location. |

---

## ドキュメント: vrc_pickup.md

```metadata
階層: /worlds/components/vrc_pickup.md
ディレクトリ: worlds\components
ファイル名: vrc_pickup.md
拡張子: .md
サイズ: 4.73 KB
最終更新: 2025-06-05T03:07:52.780Z
```

---
title: "VRC Pickup"
slug: "vrc_pickup"
hidden: false
createdAt: "2017-07-06T00:55:36.747Z"
updatedAt: "2023-05-04T21:43:45.162Z"
---
Allows objects to be picked up, held and used by players.

## Proximity Rules

:::note

All of the rules described in this section also apply to "Interactables", i.e. UdonBehaviours that have an `Interact` event (they will also show a "Proximity" slider in the Inspector).
:::

The "Proximity" value defines from how far away your pickup will be grabbable. It is given in meters, aka. "Unity units", where the side-length of one default Cube equals 1 unit.

There are 2 mechanisms of grabbing where the Proximity value will be in play:

- **Raycast:** If you are far away from a pickup, or you are running in desktop mode, pickups will be highlighted if the "laser" coming out of your hands (in VR) or your head (desktop) intersects the collider on a pickup object. The distance calculation to compare against Proximity is different in VR and Desktop mode:
    - VR: The distance between the origin of the laser (i.e. your hand controllers) and the impact point on the collider, in meters
    - Desktop: The distance between the origin of the laser (i.e. your head or main camera position) minus an "extra reach" value to compensate for being unable to move your hands forward. This is an approximate value that also takes your avatar scale into account ("longer arms"). Since it is subtracted from the distance, it will allow you to grab objects that are technically outside of the "Proximity" range, but could be grabbed by moving your arms while standing in this position in VR.
- **Hover (VR only):** If your hands are close enough to an object, pickups will be highlighted even if a ray in the direction of the UI laser would not intersect the object. This allows more natural "grabbing" of objects. The distance of reach is a sphere centered on your hands with a radius of 0.4 meters times your avatar scale (note that this value is not directly comparable to the "avatar scaling" system available to users, although changing the avatar scale that way can affect the reach). 
    - The "Proximity" value is still compared against the raw distance between your hand and the collider on the pickup, meaning the "reach distance" described is only an upper bound for the closeness at which "Hover" mode will engage.
    - For example, setting the Proximity to 0 will require the hand to be _inside_ the collider for the pickup to be highlighted (it will still be grabbable in Desktop mode however, because of the "extra reach" desktop users get to compensate).
    - As an advanced technique, you may want to adjust the Proximity value based on the data provided via the [avatar scaling system](/worlds/udon/players/player-avatar-scaling).

## Requires:

- Rigidbody
- Collider

| Parameter | Description |
| - | - |
| Momentum Transfer Method         | This defines how the collision force will be added to the other object which was hit, using Rigidbody.AddForceAtPosition.<br />Note: the force will only be added if 'AllowCollisionTransfer' is on. |
| Disallow Theft                   | If other users are allowed to take the pickup out of someone else's grip |
| Exact Gun                        | The position object will be held if set to Exact Gun |
| Exact Grip                       | The position object will be held if set to Exact Grip. |
| Allow Manipulation When Equipped | Should the user be able to manipulate the pickup while the pickup is held if using a controller? |
| Orientation                      | What way the object will be held. |
| Auto Hold                        | Should the pickup remain in the user's hand after they let go of the grab button.<br />- Auto Detect: Automatically detects what to do<br />- Yes: After the grab button is released the pickup remains in the hand until the drop button is pressed and released<br />- No: After the grab button is released the pickup is let go<br />Note: This only applies to control schemes which lack an independent "Use" input from "Grab", such as Desktop, Vive, and Vive-like input systems. This does not apply to Quest, Quest-like, and other input systems with a defined trigger. |
| Use Text                         | Text that appears when the user has an object equipped, prompting them to "fire" the object.<br />Requires "Auto Hold" to be set to "Yes". |
| Throw Velocity Boost Min Speed   | How fast the object needs to move to be thrown. |
| Throw Velocity Boost Scale       | How much throwing should scale. Higher = faster thrown while lower means slower throw speed. |
| Pickupable                       | Can you pick up the pickup? |
| Proximity                        | The maximum distance this pickup is reachable from. See section above for more details. |

---

## ドキュメント: vrc_portalmarker.md

```metadata
階層: /worlds/components/vrc_portalmarker.md
ディレクトリ: worlds\components
ファイル名: vrc_portalmarker.md
拡張子: .md
サイズ: 1.06 KB
最終更新: 2025-06-05T03:07:52.780Z
```

# VRC Portal Marker

Creates a portal to another VRChat world. Users can walk into the portal to travel to other worlds. While the user's VRChat menu is open, they can also select the portal to learn more about the portal's world. 

## Usage

Set the World ID property to a specific world, and the portal will lead to an existing public instance of that world. If no public instance exists, the portal leads to a new instance.

To create a portal to the user's home world or the VRChat Hub world, use the "World" option instead of specifying a world ID.

## Properties

VRC Portal Marker offers the following properties:

| Parameter | Description |
|-----|-----|
| World ID | The world ID to create a portal to, like `wrld_f995a2eb-7ddc-4558-aef1-815c3b23df6c`. |
| Custom Portal Name | Overrides the world name users see above the portal. |
| World | <ul><li>**None** - Creates a portal for the world ID you specified above.</li><li>**Home** - Creates a portal to the user's current home world.</li><li>**Hub** - Creates a portal to the VRChat Hub world.</li></ul> |


---

## ドキュメント: vrc_scenedescriptor.md

```metadata
階層: /worlds/components/vrc_scenedescriptor.md
ディレクトリ: worlds\components
ファイル名: vrc_scenedescriptor.md
拡張子: .md
サイズ: 3.96 KB
最終更新: 2025-06-05T03:07:52.781Z
```

---
title: "VRC Scene Descriptor"
slug: "vrc_scenedescriptor"
hidden: false
createdAt: "2017-07-06T00:43:48.565Z"
updatedAt: "2019-11-23T01:41:31.381Z"
---
Describes your VRChat world. Required for every Unity scene you'd like to use as a VRChat world.

| Parameter                    | Description                                                                                                     |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------- |
| Spawns                       | An array of transforms used as reference for spawn points of the world.                                        |
| Spawn Order                  | Order in which spawn locations should be used, options include:                                                |
|                              | - First: Always use the first spawn                                                                             |
|                              | - Sequential: In the order that spawns are listed                                                               |
|                              | - Random: Spawns users randomly                                                                                  |
|                              | - Demo: The spawn point represents the center of your room scale meaning, for example, if you're a meter away from the center of your room scale, you spawn a meter away from the spawn. |
| Spawn Orientation            | Orientation the user will be spawned in at, options include:                                                   |
|                              | - Default: The VRChat Default spawn setting (Currently Align Player With Spawn Point)                         |
|                              | - Align Player With Spawn Point: Aligns player with the rotation of the spawn transform                         |
|                              | - Align Room With Spawn Point: Aligns players' room scale to be centered on the spawn point                    |
| Reference Camera             | Settings from this camera are applied to users in the room. Can be an object in the scene or prefab.            |
| Respawn Height -Y            | Height at which players respawn and pickups are respawned or destroyed.                                        |
| Object Behaviour At Respawn  | What should pickups do when they fall out of the world, options include:                                        |
|                              | - Destroy: Delete the pickup                                                                                    |
|                              | - Respawn: Respawn the pickup to the location it started at                                                     |
| Forbid Free Modification     | If true, non-sync'd objects can't be manipulated by non-master.                                                 |
| Forbid User Portals          | Prevent users from opening portals from the world menu.                                                         |
| User Custom Voice Falloff Range | Enable the next couple options which control the voice falloff range.                                           |
| Voice Falloff Near           | The distance where users' voices start reducing in volume.                                                      |
| Voice Falloff Far            | The distance where users' voices become inaudible.                                                              |
| Unity Version                | Unity version being used; you should never need to touch this.                                                  |
| Dynamic Prefabs              | *Deprecated, unused in the VRChat SDK3.* |
| Dynamic Materials            | *Deprecated, unused in the VRChat SDK3.* |
| Interact Passthrough         | A mask defining which User Layers should allow interactions to pass through. See the [Layers](/worlds/layers) page for more info. |


---

## ドキュメント: vrc_spatialaudiosource.md

```metadata
階層: /worlds/components/vrc_spatialaudiosource.md
ディレクトリ: worlds\components
ファイル名: vrc_spatialaudiosource.md
拡張子: .md
サイズ: 7.58 KB
最終更新: 2025-06-05T03:07:52.781Z
```

import UnityVersionedLink from '@site/src/components/UnityVersionedLink.js';

# VRC Spatial Audio Source

Use the `VRC_SpatialAudioSource` to add 3D spatialization to a Unity `Audio Source` component.

When added, `VRC_SpatialAudioSource` will automatically add a Unity `Audio Source` component.

This component can be used on both avatars and worlds.

![image](/img/worlds/vrc_spatialaudiosource-1.png)
## Unity Editor Interface

The component generates two <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/ScriptReference/Gizmos.html">Unity Gizmos</UnityVersionedLink> that show:

- Far
- Near

In addition, the `Audio Source` component generates a "Volumetric Radius" gizmo.

Here's what the gizmos look like:
![Component in this image is a bit out of date, but the gizmos are the same.](/img/worlds/vrc_spatialaudiosource-e975780-Unity_2019-07-09_11-51-13.png)
The component contains tooltips for all properties. Hover over the name of the property to see a short description.

## Falloff Mechanics

All units are in *meters*. Falloff of audio intensity is roughly inverse-square by default, as illustrated below:
![](/img/worlds/vrc_spatialaudiosource-c969d41-crowhurst_basic_audio_vol1-39.gif)

You can override the curve using the graph on the `Audio Source`. This curve determines the intensity of the audio at a given distance.

At ranges approaching the `Far` value, audio may fade out more quickly depending on your settings.

### Using 2D Audio

2D audio is where the audio's volume is constant no matter where you are in a world. This might be used for background noise that is already recorded as a spacialized source (a field recording) or background music.

**You can use 2D avatar audio if you like** by disabling the `Use Spatialized Audio` option in the component. Unless you choose to use a different audio falloff curve, the intensity will still drop off over distance as before, it just won't be spatialized.

All that being said, **we do not recommend using 2D audio.** All real-world sources of sound have distinct point or volumetric sources. If you wish to use 2D audio regardless, ensure that you:
- Uncheck `Use Spatialized Audio` on the `VRC_SpatialAudioSource`
- Adjust Spatial Blend on the `Audio Source` to be 100% 2D

## Spatial Audio on Avatars

VRChat supports using `VRC_SpatialAudioSource` on avatars, albeit with some [limitations](/worlds/components/vrc_spatialaudiosource#avatar-limitations). These limitations are in place to prevent abuse and malicious sounds.

Other than these limitations, `VRC_SpatialAudioSource` works precisely the same on avatars as it does in worlds.

:::warning 

Don't forget to add a `VRC_SpatialAudioSource` to your avatar's audio sources! Otherwise, the VRChat SDK will create the component and choose the default settings, and you may experience unexpected, undocumented, and undesired behavior.

If you use a pre-existing avatar's `Audio Source`, ensure that it has a `VRC_SpatialAudioSource` component!

:::

## Component Properties

The `VRC_SpatialAudioSource` component has the following properties:

:::info

Adjusting these properties via animations during runtime is not supported. These values are set at initialization.

Animating properties of the `Audio Source` should still work for properties that are not related to spatialization settings, like pitch.

:::

| Property                         | Description     |
| :-- | :-- |
| Gain                             | An additional boost to volume (0-24 dB). By default, world audio sources get a 10dB boost. Avatar audio sources are limited to a maximum gain of 10dB. |
| Far                              | The far radius, in meters, where volume falls off to silence. By default, it is set to 40m. Avatar audio is limited to a maximum of 40m. <br /> Far only overrides an Audio Source curve if you turn on the "Use Spatializer Falloff" checkbox on VRC_SpatialAudioSource. |
| Advanced: Near                   | The near radius, in meters, where volume begins to fall off. We recommend keeping this at zero for realism and effective spatialization. Defaults to 0m. <br /> Near only overrides an Audio Source curve if you turn on the "Use Spatializer Falloff" checkbox on VRC_SpatialAudioSource.  |                                                                                                                                                 |
| Advanced: Volumetric Radius      | An audio source is normally simulated to be a point source, however changing this value allows the source to appear to come from a larger area. This should be used carefully and is mainly for distant audio sources that need to sound "large" as you move past them. <br /> The listener should not ever get close to the radius for best results. Keep this at zero unless you know what you're doing. Defaults to 0m. <br />  The value for Volumetric Radius should always be lower than Far. |
| Advanced: Use AudioSource Volume Curve | Use the AudioSource's '3D Sound Settings' volume curve. If unchecked, use Inverse Square falloff. It is recommended to keep this disabled to ensure realistic and authentic spatialization. <br /> <br /> Defaults to False. |
| Advanced: Enable Spatialization  | Uncheck this to disable the default inverse-square falloff curve and instead use the Audio Source's spatialization settings. <br /><br /> Defaults to True.|

## Avatar Limitations
You are permitted to adjust the fall-off curve on avatar-based `Audio Sources`. Simply set `Use AudioSource Volume Curve` to True, adjust the curve in the `Audio Source`, and VRChat will use that fall-off curve instead of the default inverse-square.

However, as noted above, there are some limitations on `VRC_SpatialAudioSource` components on avatars. These limits are enforced at run-time.

- `Gain` cannot exceed 10db
- `Far` cannot exceed 40m

[Player Audio](/worlds/udon/players/player-audio) can override these settings.

:::note

On avatars, it is best to disable and enable the Audio Source components rather than the entire GameObject.
:::

### Curve Squashing

If you attempt to play avatar audio with a custom rolloff curve in a world with a shorter `Far` distance than normal, VRChat will "squash" the curve by changing the "Max Distance" setting of your avatar's audio source. You can see what happens by adjusting the "Max Distance" range on the Audio Source in Unity.

### Avatar Audio Compressor
VRChat uses a compressor on the Avatar audio channel that prevents sounds from being maliciously loud. This should not affect normal use of avatar audio sources that have reasonable volume levels.

### Tips for Avoiding the Compressor

If you've got audio on your avatar, there's a few things you can do with your audio beforehand to ensure you're not going to hit the compressor.

1. Try to get "dry" audio files-- that is, audio files with no effects. Reverb and delay are the most egregious in causing compressor "pumping".
2. Leave a bit of headroom in the audio file. In other words, don't fit the waveform to the very top and bottom of the range. In Audacity (or other wave editor), normalize your audio between -6 and -12db.
3. Try to avoid extremely high peaks in the waveform with very short attack-- in other words, don't suddenly "pump" the audio to very high levels. If you normalize, this should drop out of the file regardless.

## Replacing ONSP in Old Scenes

Using the "Enable 3D Spatialization on all AudioSources" button in the Build Control Panel now converts any `ONSPAudioSource` to `VRC_SpatialAudioSource` components. Some of these sources may require tweaking after the conversion.

---

## ドキュメント: vrc_station.md

```metadata
階層: /worlds/components/vrc_station.md
ディレクトリ: worlds\components
ファイル名: vrc_station.md
拡張子: .md
サイズ: 8.92 KB
最終更新: 2025-06-05T03:07:52.782Z
```

# VRC Station

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

Allow users to sit down by interacting with an object. An example can be found in the SDK as [VRCChair3](/worlds/examples/udon-example-scene#vrcchair3).

This component can also be used on avatars to create seats on avatars!

## Parameters and Options

| Parameter                    | Description                                                                                             |
| ---------------------------- | ------------------------------------------------------------------------------------------------------- |
| Player Mobility              | Should the player be able to move, options include:                                                     |
|                              | - Mobile: Allow users to move when seated in the station.                                               |
|                              | - Immobilize: Prevents the user from moving.                                                            |
|                              | - Immobilize For Vehicle: Same as Immobilized but optimized for moving stations.                        |
| Can Use Station From Station | If the user can switch stations when sitting in a station.                                              |
| Animation Controller (optional) | Used to override normal seating animations with a custom one.                                        |
| Disable Station Exit         | If the user cannot exit the station by usual means, use triggers to unseat the user.                    |
| Seated                       | Is this a station that the user should be sitting in? See the details above to see what this indicates. |
| Station Enter Player Location | Transform that defines where the user should be transported to when seated.                            |
| Station Exit Player Location  | Transform that defines where the user should be transported to when they are unseated.                  |
| Controls Object              | This is used for stations where you can control an object, such as a vehicle.                           |
## Creating a station with Udon

You can use stations in your own custom Udon scripts. Making a chair to sit in is fairly straightforward. The [VRCChair3](/worlds/examples/udon-example-scene#vrcchair3) prefab included with the VRCSDK shows how to setup one. 

All you need for a chair is a GameObject with the following components:
1. `VRC_Station` with an entry and exit point set to itself or another transform that designates where the player is rooted to the station.
2. A collider, usually with "Is Trigger" enabled.
3. An UdonBehaviour with an Udon program that handles sitting in the station.
4. (Optional) A mesh that looks like a chair.

The VRChat SDK comes with a program that handles step 3, which is called **StationGraph**.

<Tabs groupId="udon-compiler-language">
<TabItem value="graph" label="Udon Graph">

![The station graph, part of the VRCChair3 prefab.](/img/worlds/station-graph.png)

</TabItem>
<TabItem value="cs" label="UdonSharp">

```cs
using UdonSharp;  
using UnityEngine;  
using VRC.SDKBase;  
  
public class StationGraph : UdonSharpBehaviour  
{  
    public override void Interact()  
    {  
        Networking.LocalPlayer.UseAttachedStation();  
    }  
  
    public override void OnStationEntered(VRCPlayerApi player)  
    {  
        Debug.Log($"{player.displayName} Entered");  
    }  
  
    public override void OnStationExited(VRCPlayerApi player)  
    {  
        Debug.Log($"{player.displayName} Exited");  
    }  
}
```

</TabItem>
</Tabs>

Udon's "OnStationEntered" and "OnStationExited" events can be very useful for advanced use cases, such as detecting which player has entered or exited a moving vehicle.

## Stations used in Worlds

Your Animator can access avatar parameters to react to avatars entering the station. 

- Use the `InStation` parameter to detect whether an avatar has entered a station. but might not have Seated-IK enabled.
- Use the `Seated` parameter to detect whether Seated-IK is enabled for the avatar in the station.
	- The `Seated` parameter only used if the `Seated` property on the station is enabled.

### Detecting SDK2 Avatars

Station behave slightly differently for avatars that were uploaded with very old versions of the VRChat SDK (SDK2).

Check the `AvatarVersion` parameter to detect SDK2 avatars. For example:
- If `AvatarVersion` is `3`, then the avatar uses the modern SDK3.
- If `AvatarVersion` is less than `3`, then the avatar uses the outdated SDK2.

Use `AvatarVersion` and `InStation` to transition to different custom seated animations. The differences are described below.

### SDK2 avatars

If an SDK2 avatar uses your station, your Animator should apply a fixed seated pose (if `Seated` is enabled), or a full-body animation if `Seated` is not enabled.

### SDK3 avatars

If an SDK3 avatar uses your station, your Animator can choose to do any combination of animations, Tracking Control changes, and Pose Space changes that are available for SDK3 avatar. However, there will be no behind-the-scenes State Behaviors applied (which does occur in SDK2 stations).

Note that since the creator decides what type of Tracking Control to apply, the Seated property on a station does not necessarily indicate the tracking type on SDK3 avatars. 

Our example Seated controllers show this branching behavior and the proper transitions and State Behavior setup for applying a seated animation.

:::caution

Stations can only affect parameters that already existed on an avatar. You can't use [State Behaviors](/avatars/state-behaviors) to create new parameters.

:::
## Stations used on Avatars
The default `VRCChair` prefab included in the SDK can be used on avatars to let other players "sit" on you. You can use this to make your avatar into a car, a dinner table that moves around, and more! An avatar can have up to 6 stations.

When using stations on an avatar that you want to animate on or off, you need to toggle specific objects and components.

![image](/img/worlds/vrc_station-0adc923-av-station-fix.png)

**The red box (in the screenshot above) needs to be enabled upon upload. If it is disabled upon upload, the station will not work!**

Toggling the red box will remove any player currently sitting in the station. Toggling the green boxes will only stop new players from sitting in the station. Since this involves disabling/enabling components and objects, this **must** be done in the FX layer.

Keep in mind that users can disable animations with the [Safety system](https://docs.vrchat.com/docs/vrchat-safety-and-trust-system). If an avatar station is on by default (as in, having the toggles on by default), the stations remain active, even if the wearer has used an animation to turn them off.

There are two other restrictions upon upload:
- The Entry and Exit points must not be more than 2 meters apart.
- There can be up to 6 stations on the avatar. Any more will be disabled.

## Parameters and Options

| Parameter | Description |  
| - | - |  
| Player Mobility                 | Whether players are allowed to walk around while using the station. |  
|                                 | - Mobile: Allows players to move when seated in the station. |  
|                                 | - Immobilize: Prevents user from moving and moves them to "Station Enter Location." |  
|                                 | - Immobilize For Vehicle: Same as "Immobilize" but optimized for moving stations.  |  
| Can Use Station From Station    | Whether the players can switch stations when sitting in a station.  |  
| Animation Controller (optional) | Overrides the "Sitting" playable layer in the avatar controller. |  
| Disable Station Exit            | Prevents the player from exiting the station by moving or jumping.<br />Use the `ExitStation` node to remove the player from the station. |  
| Seated                          | Whether the player's avatar will sit or stand when using the station.<br />This affects the "Seated" parameter of the avatar’s playable layers. |  
| Station Enter Player Location   | The player is moved to the position of this Transform when they enter the station. |  
| Station Exit Player Location    | The player is moved to the position of this Transform when they exit the station. |

## Limitations

Avatars must meet the following requirements to sit in stations:

- The avatar must have an has an [Animator component](https://docs.unity3d.com/Manual/class-AnimatorController.html) with an assigned [Unity Avatar](https://docs.unity3d.com/Manual/AvatarCreationandSetup.html). This is usually assigned automatically for humanoid avatars.
- The Animator component must be on the same game object as the avatar descriptor component.

---

## ドキュメント: vrc_uishape.md

```metadata
階層: /worlds/components/vrc_uishape.md
ディレクトリ: worlds\components
ファイル名: vrc_uishape.md
拡張子: .md
サイズ: 6.92 KB
最終更新: 2025-06-05T03:07:52.782Z
```

# VRC Ui Shape

The VRC Ui Shape component allows you to create [Canvas](https://docs.unity3d.com/Manual/UICanvas.html) components that players can interact with.

- Players can point, click, or scroll your UIs to interact with them, similar to the VRChat menu.
- Players can interface from a distance. This often makes them easier to use than [Interact](/worlds/examples/udon/#interact) events.

![Two examples UIs: A "Toggle Mirror" setting with a checkbox, and a page indicator with a "Previous" and "Next" button.](/img/worlds/components/VRC_UiShape.png)

VRC Ui Shape only has one configurable parameter:

| Parameter        | Description                                                                                         |
| ---------------- | --------------------------------------------------------------------------------------------------- |
| Allow Focus View | Whether this canvas should allow users to enter [Focus View](#focus-view) if they're playing on a phone or tablet. |

## How to set up your Canvas

When you right-click your hierarchy window and click "UI" -> "TextMeshPro (VRC)", the SDK automatically sets up your Canvas. The SDK automatically configures your Canvas and adds other required components.

![Two examples UIs: A "Toggle Mirror" setting with a checkbox, and a page indicator with a "Previous" and "Next" button.](/img/worlds/components/vrc-ui-components.png)

If you want to configure a Canvas manually, follow the steps below. Otherwise, your Canvas may not work in VRChat.

1. Add a [Canvas](https://docs.unity3d.com/Packages/com.unity.ugui@1.0/manual/class-Canvas.html) component to your GameObject, or create a new GameObject.
    - Unity automatically adds other helpful components to your Canvas.
2. Add a "VRC_UIShape" component to the same GameObject as your Canvas.
3. Change the GameObject's layer from "UI" to "Default" or another layer.
	- The UI layer would prevent users from interacting with your UI unless the VRChat menu is open.
4. Reduce the GameObject's `x`, `y`, and `z` scale.
	- By default, the scale is 1, which is too large. (1 pixel = 1 meter)
	- Reduce the scale to something like `0.01`. (1 pixel = 0.01 meters) 
5. Change the Canvas's "Render Mode" property from "Screen Space" to "World Space".
	- World Space Canvases are shown inside your world instead of directly on your screen. 
6. Add UI elements to the Canvas.
	- For example, you can add text, buttons, toggles, scroll views, input fields, and more.
	- Set the "Navigation" property to of your UI elements to "None". This prevents players from accidently using the UI while moving in your world.
	- Use TextMeshPro instead of Unity's built-in components for higher text quality.

If you follow these steps, players can interact with your UI by clicking on it.

## Common problems

If your Canvas is not configured correctly, players may be unable to interact with it. Here's how you can fix common issues:

### If you have a canvas that does not make the VRChat pointer show up:

* **The canvas must have a** `VRC_UIShape` **component on it.** Make sure that you didn't place it on some other child object.
* **The layer of the Canvas cannot be UI.** Setting the canvas and all it's children to default will work.
* **The object with the** `VRC_UIShape` **must have a box collider.** If there is none, one will be added automatically after the world is uploaded. However if you have added a collider yourself, you must make sure that it is the correct size.
* **Make sure you do not have some other collider blocking the canvas.** 

### If the pointer shows up but the UI is not responsive:
* **The scene must have an EventSystem.** This is added automatically when you make the canvas, so don't delete it.
* **Make sure that interact-able elements are not covered by invisible elements.** This often happens when a text box overlaps and covers a button. There's a few solutions: You can rearrange the button so it is on top (lower in the hierarchy), you can resize the text so that it does not cover the button, or you can set the text's `raycast target` to `false`.
* **Make sure that the UI you are trying to interact with has an image with `Raycast Target` enabled.** This is auto-generated if you create UI elements with the right-click menu in the hierarchy.
* **Make sure that the canvas has the `Graphic Raycaster` component.** This component is automatically added to your GameObject when you add a Canvas component.
* **Make sure that you are looking at the canvas from the correct side. The Z-forward axis of the canvas should be facing _away_ from you.

### If the UI is responsive but does not do what you expect it to do:

* Some UI events get removed in VRChat for security reasons. Make sure that the events you are trying to use are on [this list](/worlds/udon/ui-events)
* If you are using `SendCustomEvent`, make sure to type the event exactly the same both in the UI button and in the UdonBehaviour's `event custom` node
* If you are using `SendCustomEvent` to an UdonSharp behaviour, the event must be set to public. If it is set to private, it will not work.
* If something is wrong with an UdonBehaviour, it might halt which will stop it from doing anything. Learn how to [debug your Udon projects](/worlds/udon/debugging-udon-projects) for more details.

### If the UI is moving when you move, press a key, or move a joystick:

* Set `Navigation` to `None` on all UI elements.
- Set `Scroll Sensitivity` to `0` if your UI has a scroll bar.
### If you'd like a TextField not to show VRChat's keyboard:

* Add the `VRCInputFieldKeyboardOverride` component to prevent VRChat's keyboard from appearing.

## Focus view

VRChat's "Focus View" feature allows users to expand, pan, and zoom your world's UI on their phone or tablet. This makes it easier for them to read and interact with small text on their screen.

<div class="video-container">
    <video src="https://assets.vrchat.com/videos/docs/focusViewDemo.mp4" title="Focus View demo" muted controls></video>
</div>

Your canvas must meet the following conditions for Focus View to be available: 

- The canvas is configured correctly and has a [`VRC_UIShape`](https://creators.vrchat.com/worlds/components/vrc_uishape) component. 
- The `VRC_UIShape` component's "Allow Focus View" parameter is enabled.
- You have not disabled Focus View in your world's settings on VRChat.com.

In addition, users in your world must meet the following conditions:

- The user is playing VRChat on a phone or tablet.
- The user is using the touchscreen as their only input device.
- The user has not disabled Focus View in their VRChat settings.
- The user is less than 3-6 meters away from the canvas (depending on the canvas's size)
- The user is more than 0.6-2 meters away from the canvas (depending on the canvas's size)

While a user is in focus view:
- The user's nameplate displays a focus view icon that's visible to other players.
- Users stay in focus view until they manually close it.


---

# ドキュメント終了

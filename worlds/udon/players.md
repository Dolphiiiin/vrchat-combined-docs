# players 統合ドキュメント

以下は自動収集された全ドキュメントのコンテンツです。各セクションの始めにメタデータが記載されています。



---

## ドキュメント: getting-players.md

```metadata
階層: /worlds/udon/players/getting-players.md
ディレクトリ: worlds\udon\players
ファイル名: getting-players.md
拡張子: .md
サイズ: 2.69 KB
最終更新: 2025-06-05T03:07:52.820Z
```

---
title: "Getting Players"
slug: "getting-players"
hidden: false
createdAt: "2021-01-22T01:48:13.564Z"
updatedAt: "2021-11-12T01:23:15.713Z"
---
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

These nodes are useful for getting an individual Player, a group of them, or all of them.

### [Networking.get LocalPlayer](/worlds/udon/networking/network-components)
*VRCPlayerApi*

:::note

Please note that this function is a member of the Networking class, but it is being included here.

:::
The local player is the Player that this Udon script is currently running on-- alternately, the local player is *you*. It's very important to know yourself!

### GetPlayerCount
*int*

Gets the actual number of Players in the instance when it is called.

### GetPlayers
*VRCPlayerApi[]*

This is how you get all the Players in your world so you can go through them in a For Loop and apply settings, make changes, look for a particular name, etc. To use it, you *first need to create a VRCPlayerApi Array*.


<Tabs groupId="udon-compiler-language">
<TabItem value="graph" label="Udon Graph">

![The bare minimum for a working call to GetPlayers. A better approach would be to construct VRCPlayerApi[] as a variable so you can reuse it.](/img/worlds/graphgetplayers.png)

</TabItem>
<TabItem value="cs" label="UdonSharp">

```cs
var players = new VRCPlayerApi[VRCPlayerApi.GetPlayerCount()];  
VRCPlayerApi.GetPlayers(players);
```

</TabItem>
</Tabs>



Above, you can see the bare minimum for a working call to GetPlayers. A better approach would be to construct VRCPlayerApi\[\] as a variable so you can reuse it.

### GetPlayerById
*int*

Get a VRCPlayerApi object for the given player Id if it exists.

### get playerId
*int*

Get the cached PlayerId, calls GetPlayerId if it hasn't been cached yet.

### GetPlayerId
*int*

Gets the Player's Network Id from the source.

## Player Tag System
This system is a quick-and-dirty way of assigning strings to players without creating your own variables and collections.

### SetPlayerTag / GetPlayerTag
Set: *string, string*

Get: *string*

Sets a string variable that you can look up later. For example, you could set the "role" of a player in a cooking game to "chef" or "customer". Then you could GetPlayerTag for "role" and get back either "chef" or "customer".

### ClearPlayerTags
*VRCPlayerApi*

Remove all tags that you've set on a Player.

### GetPlayersWithTag

:::caution

Not currently working. Returns a List, which is unavailable in Udon.

You will be able to pass in an array of VRCPlayerApi objects and a tag and the method will fill the array with Players who have that tag set.

:::

---

## ドキュメント: index.md

```metadata
階層: /worlds/udon/players/index.md
ディレクトリ: worlds\udon\players
ファイル名: index.md
拡張子: .md
サイズ: 5.01 KB
最終更新: 2025-06-05T03:07:52.822Z
```

---
sidebar_position: 1
---
# Player API

You can use Udon to retrieve information about players in your world instance.

Udon interacts with Players through the VRCPlayerApi. Each Player has a VRCPlayerApi Object, and your world fires the OnPlayerJoined / OnPlayerLeft events on any UdonBehaviours that listen for them when a player joins or leaves.

This page includes info on using some general player properties and methods. Since there are so many things you can do with the VRCPlayerApi object, we grouped some information together on the following pages:

* [Getting Players](/worlds/udon/players/getting-players)
* [Player Positions](/worlds/udon/players/player-positions)
* [Player Forces](/worlds/udon/players/player-forces)
* [Player Collisions](/worlds/udon/players/player-collisions)
* [Player Audio](/worlds/udon/players/player-audio)
* [Player Avatar Scaling](/worlds/udon/players/player-avatar-scaling)
* [Player Events](/worlds/udon/graph/event-nodes#player-events)

## Generally Useful Properties and Methods

### IsValid
*VRCPlayerApi, Boolean*

Before you try to get or set anything on a Player, check whether IsValid returns true. If a player has left since you saved a reference to them, this will return False. For easier use on the graph, search for the generic "IsValid" method which works for any Object, since it gives you separate flows for True and False.

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs groupId="udon-compiler-language">
<TabItem value="graph" label="Udon Graph">

![An Udon Graph screenshot showing the OnPlayerLeft event connected to an IsValid node.](/img/worlds/player-isvalid.png)

</TabItem>
<TabItem value="cs" label="UdonSharp">

```cs
   public override void OnPlayerLeft(VRCPlayerApi player)  
   {  
	if (Utilities.IsValid(player))
	{
		// Player is valid
	}
	else
	{
		// Player is not valid
	}
}  
```

</TabItem>
</Tabs>



### EnablePickups
*VRCPlayerApi, Boolean*

Turn off the Player's ability to pickup and use VRCPickup objects in the world. This property is *on* by default, so you only need to use the method if you want to turn it off.

### get displayName
*VRCPlayerApi*

Get the name displayed for the Player (can be different than Username, which is used to log into VRChat and not exposed publicly)

### Get isLocal
in: *VRCPlayerApi*

out: *Boolean*

Tells you whether the given Player is the local one.

### Get isMaster
in: *VRCPlayerApi*

out: *Boolean*

Tells you whether the given Player is the [instance master](/worlds/udon/networking#the-instance-master).

### Get isInstanceOwner
in: *VRCPlayerApi*

out: *Boolean*

Tells you whether the given player is the current instance owner.

### Get isSuspended
in: *VRCPlayerApi*

out: *Boolean*

Tells you if the player's device is suspended. A device is suspended if it enters sleep mode or switches to a different app. While suspended, devices don't run Udon code or respond to network events until the player reopens VRChat.

Your code should account for any device becoming suspended at any time, regardless of platform. PC players currently never become suspended, but this should not be assumed.

Note that `isSuspended` is always `false` for the local player - Udon cannot run while the local player is suspended.

### GetPickupInHand
in: *VRCPlayerApi, Hand (none, left, right)*

out: *VRCPickup*

Gets the pickup a Player is holding in the specified hand. Only works for the Local Player. Returns null if no VRCPickup is found.

### IsOwner
in: *VRCPlayerApi, GameObject*

out: *Boolean*

Tells you whether a Player is the Owner of a given GameObject, important for Sync.

### IsUserInVR
in: *VRCPlayerApi*

out: *Boolean*

Tells you whether a Player is using a VR headset.

### PlayHapticEventInHand
*VRCPlayerApi, Hand, float, float, float*

Vibrate the Player's Haptic controllers if they have them. The float inputs should be in the range 0-1. *duration* is the amount of time that they vibrate, *amplitude* is the strength with which they vibrate, and *frequency* is the speed at which they vibrate. The feeling can vary quite a bit between controllers.

### UseAttachedStation
*VRCPlayerApi*

Makes the player get into the Station which is on the same GameObject as this UdonBehaviour.

### SimulationTime
*float*

The [simulation time](/worlds/udon/networking/network-components) of a player.

### UseLegacyLocomotion
*VRCPlayerApi*

**NOT RECOMMENDED** - Turns on legacy locomotion for this player, mimicking how players moved in VRChat's deprecated SDK2. After enabling legacy locomotion, it stays enabled until the user leaves your world.

## Language

### GetCurrentLanguage
*string*

Gets the selected language of the local user. The value is formatted in the RFC 5646 syntax. (`en`, `ja`, `es`, `zh-CN`, etc.)

### GetAvailableLanguages
*string[]*

Gets all available languages a player can select in VRChat's settings. The value is formatted in the RFC 5646 syntax. (`en`, `ja`, `es`, `zh-CN`, etc.)

---

## ドキュメント: player-audio.md

```metadata
階層: /worlds/udon/players/player-audio.md
ディレクトリ: worlds\udon\players
ファイル名: player-audio.md
拡張子: .md
サイズ: 3.95 KB
最終更新: 2025-06-05T03:07:52.823Z
```

# Player Audio

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

Players have two sources of audio: the voice coming through their microphone, and sounds attached to their Avatar. With Udon, you can change how a Player hears *other* players' voices and avatar sounds. For example, this code makes a player quieter by setting their gain to 5 dB (which is lower than the default of 15 dB):

<Tabs groupId="udon-compiler-language">
<TabItem value="graph" label="Udon Graph">

![Setting the player's voice gain in Udon Graph.](/img/worlds/player-audio-8e50220-setvoicegain.png)

</TabItem>
<TabItem value="cs" label="UdonSharp">

```cs
somePlayer.SetVoiceGain(5);
```

</TabItem>
</Tabs>

Here are all the properties you can access:

## Voice

### Set Voice Gain
*in Decibels, Range 0-24*
Add boost to the Player's voice in decibels. Default is 15.

### Set Voice Distance Near
*in Meters, Range 0 - 1,000,000*
The near radius, in meters, where volume begins to fall off. It is strongly recommended to leave the Near value at zero for realism and effective spatialization for user voices.

### Set Voice Distance Far
*in Meters, Range is 0 - 1,000,000*
This sets the end of the range for hearing the user's voice. Default is 25 meters. You can lower this to make another player's voice not travel as far, all the way to 0 to effectively 'mute' the player.

### Set Voice Volumetric Radius
*in Meters, Range is 0 -1,000*
Default is 0.
A player's voice is normally simulated to be a point source, however changing this value allows the source to appear to come from a larger area. This should be used carefully, and is mainly for distant audio sources that need to sound "large" as you move past them. **Keep this at zero unless you know what you're doing.** The value for Volumetric Radius should always be lower than Voice Distance Far.

If you want a user's voice to sound like it is close no matter how far it is, increase the Voice Distance Near range to a large value.

### Set Voice Lowpass
*On/Off*
When a voice is some distance off, it is passed through a low-pass filter to help with understanding noisy worlds. You can disable this if you want to skip this filter. For example, if you intend for a player to use their voice channel to play a high-quality DJ mix, turning this filter off is advisable.

## Avatar

### SetAvatarAudioGain
*in Decibels, Range 0-10*
Set the Maximum Gain allowed on Avatar Audio. Default is 10.

### SetAvatarAudioFarRadius
*in Meters, Range is not limited*
This sets the maximum end of the range for hearing the avatar's audio. Default is 40 meters. You can lower this to make another player's avatar audio not travel as far, all the way to 0 to effectively 'mute' the player. Note that this is compared to the audio source's maxDistance, and the smaller value is used.

### SetAvatarAudioNearRadius
*in Meters, Range is not limited*
This sets the maximum start of the range for hearing the avatar's audio. Default is 0 meters. Increasing this value makes the avatar audio sources reach their maximum volume from a further distance. Note that this is compared to the audio source's maxDistance, and the smaller value is used.

### SetAvatarAudioVolumetricRadius
*in Meters, Range is not limited*
An avatar's audio source is normally simulated to be a point source, however changing this value allows the source to appear to come from a larger area. This should be used carefully, and is mainly for distant audio sources that need to sound "large" as you move past them. Keep this at zero unless you know what you're doing. The value for Volumetric Radius should always be lower than Avatar AudioFarRadius. Default is 0 meters.

### SetAvatarAudioForceSpatial
*On/Off*
If this is on, then Spatialization is enabled for the source, and the spatialBlend is set to 1. Default is Off.

### SetAvatarAudioCustomCurve
*On/Off*
This sets whether the audio source should use a pre-configured custom curve. Default is Off.


---

## ドキュメント: player-avatar-scaling.md

```metadata
階層: /worlds/udon/players/player-avatar-scaling.md
ディレクトリ: worlds\udon\players
ファイル名: player-avatar-scaling.md
拡張子: .md
サイズ: 5.46 KB
最終更新: 2025-06-05T03:07:52.823Z
```

---
title: "Player Avatar Scaling"
slug: "player-avatar-scaling"
hidden: false
createdAt: "2023-06-22T01:23:45.678Z"
updatedAt: "2023-07-19T01:23:45.678Z"
---
Udon provides functions allowing world creators to
- Permit or enforce avatar scaling features and parameters.
- React to changes in player height.

To see relevant events to use in concert with these functions, including "OnAvatarChanged" and "OnAvatarEyeHeightChanged", see [Avatar Events](/worlds/udon/avatar-events)

:::note Scaling and collision
Please note that scaling an avatar to another size does **not** affect its collision with the environment -- just as uploading an avatar that you've scaled up in Unity would not do so.
:::

Avatar Scaling operates in two modes and can be adjusted individually on a player-by-player basis:

1. A player-controlled mode, where the player is allowed to utilize a radial puppet in their action menu or a "Match Eye Height" button in their quick menu to adjust their own height.
2. A world-authoritative mode where these features are disabled, and player heights can only be set by an Udon program running in the world.

In either case, avatar changes and eye height changes will fire "OnAvatarChanged" and "OnAvatarEyeHeightChanged" events so that an Udon program can react to these occurrences.

:::note Player-controlled by default

Currently, avatar scaling is operating in the player-controlled mode by default. If you wish to operate in the world-authoritative mode, you must disable it on the website or with the Udon functions below.

:::

## Disabling player-controlled scaling via the website
If you simply wish to disable the avatar scaling system in your world, you can log into the [My Worlds section of the VRChat Website](https://vrchat.com/home/content/worlds), select your world, toggle it off, and save your changes.

![It's really easy!](/img/worlds/udon/website_avatar_scaling_enabled.png)

When you disable avatar scaling via the website, the system is disabled for players in your world entirely. They will only be able to use your world at their default scale, and the menu components for avatar scaling will be disabled. 

Because of this, the following `VRCPlayerApi` functions are not available to Udon when avatar scaling is disabled via the website, and using them will have no effect: `SetManualAvatarScalingAllowed`, `SetAvatarEyeHeightMinimumByMeters`, `SetAvatarEyeHeightMaximumByMeters`, `SetAvatarEyeHeightByMeters`, `SetAvatarEyeHeightByMultiplier`.

If you wish to retain access to these Udon functions but don't want users to be able to scale themselves via the radial puppet in their Action Menu, please leave avatar scaling enabled via the website and instead use Udon to disable the player-controlled avatar scaling mode via `SetManualAvatarScalingAllowed`.

## Functions for player-controlled scaling
:::note Local player only
Unless otherwise stated, all of the functions below can only be used by an Udon program affecting the local player and cannot be called on a `VRCPlayerApi` object belonging to another player successfully.
:::

### GetManualAvatarScalingAllowed
Checks if the local player is allowed to control their avatar scale.

**Output**
- `bool`: `true` if the local player is in the player-controlled avatar scaling mode, or `false` if they're in the world-authoritative avatar scaling mode.

### SetManualAvatarScalingAllowed
Toggles between player-controlled and world-authoritative scaling modes.

**Input**
- `bool`: `true` sets the player-controlled mode, and `false` sets the world-authoritative mode.

### GetAvatarEyeHeightMinimumAsMeters

Returns the minimum eye height in meters that the local player is permitted to scale themselves to in the player-controlled avatar scaling mode. (Greater than or equal to 0.2 meters.)

**Output**
- `float`: The minimum eye height in meters.

### SetAvatarEyeHeightMinimumByMeters
Sets the minimum height in meters that the local player is permitted to scale themselves to in the player-controlled avatar scaling mode. (Must be greater than or equal to 0.2 meters.)

**Input**
- `float`: Sets the minimum avatar eye height in meters.

### GetAvatarEyeHeightMaximumAsMeters

Returns the maximum eye height that the local player is permitted to scale themselves to in the player-controlled avatar scaling mode. (Less or equal to 5 meters )

**Output**
- `float`: The maximum avatar eye height in meters.

### SetAvatarEyeHeightMaximumByMeters
Sets the maximum eye height in meters that the local player is permitted to scale themselves to in the player-controlled avatar scaling mode. (Must be less or equal to 5 meters.)

**Input**
- `float`: Sets the maximum eye height in meters.

## Functions for world-authoritative scaling

:::note Local player only
Unless otherwise stated, all of the functions below can only be used by an Udon program affecting the local player and cannot be called on a `VRCPlayerApi` object belonging to another player successfully.
:::

### GetAvatarEyeHeightAsMeters

Returns the configured eye height for the target player's avatar. This function works for the local player **and** remote players.

**Output**
- `float`: The configured eye height for the target player's avatar.

### SetAvatarEyeHeightByMeters

**Input**
- `float`: Sets the eye height in meters for the current player avatar.

### SetAvatarEyeHeightByMultiplier

**Input**
- `float`: Sets the eye height as a multiple of the target player's avatar eye height at its prefab scale.


---

## ドキュメント: player-collisions.md

```metadata
階層: /worlds/udon/players/player-collisions.md
ディレクトリ: worlds\udon\players
ファイル名: player-collisions.md
拡張子: .md
サイズ: 2.99 KB
最終更新: 2025-06-05T03:07:52.824Z
```

import UnityVersionedLink from '@site/src/components/UnityVersionedLink.js';

# Player Collisions

Udon has three ways to detect when a Player and an Object Collide - **Triggers**, **Physics**, and **Particles**.

## Triggers

If you want to detect when a player has entered or exited an area, your best bet will be to use the **OnPlayerTrigger **events. There are three of these:

- **OnPlayerTriggerEnter** is called when a player's capsule enters a Trigger Collider 
- **OnPlayerTriggerStay** is called on frames while a player's capsule is inside a Trigger Collider
- **OnPlayerTriggerExit** is called when a player's capsule exits a Trigger Collider.

![A simple Box Collider with 'Is Trigger' checked.](/img/worlds/player-collisions-6d9aaf6-trigger-collider.png)

To use these events, add an object with a [collider](https://docs.unity3d.com/Manual/collider-shapes-introduction.html) component and enable the 'Is Trigger' box on the collider. A trigger collider allows objects and players pass through it and calls events when those objects have colliders. You can learn more about <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/CollidersOverview.html">Collision in Unity's documentation</UnityVersionedLink>.


:::danger

There are some edge cases where one of these events could be skipped, like when a player teleports in/out of a collider, or is moving VERY fast.

:::
## Physics
There is another set of events you can use when you've got objects like bouncing balls or bullets that you're moving around with physics. These objects have Colliders with IsTrigger turned off so that they'll interact with the environment and each other. 

To detect events on these Colliders, you can use:
- **OnPlayerCollisionEnter** is called when a player's capsule enters a Collider.
- **OnPlayerCollisionStay** is called on frames while a player's capsule is inside a Collider.
- **OnPlayerCollisionExit** is called when a player's capsule exits a Collider.
- **OnControllerColliderHitPlayer** is called when a CharacterController hits a Player.

:::caution OnPlayerCollision Events are for Moving Objects

These events WILL NOT be called when a player 'walks into' a stationary object. If you want to handle that, use a Trigger Collider.

:::
## Particles
Finally, you can use **OnPlayerParticleCollision** to detect when a Particle colliders with a player, assuming that Particle System has Collision and Send Collision Messages turned on.

![This Particle System has the Collision module turned on, is set to 'World' and '3D' modes, with 'Send Collision Messages' turned on.](/img/worlds/player-collisions-40d1f44-particle-collisions.png)
## Examples

The [Udon example scene](https://creators.vrchat.com/worlds/examples/udon-example-scene/) demonstrates how to use all three methods of detecting player collisions. 

![Check out the Udon Example Scene to see how these events can be used.](/img/worlds/player-collisions-f98c33a-udonexamplescene-collisions.png)

---

## ドキュメント: player-forces.md

```metadata
階層: /worlds/udon/players/player-forces.md
ディレクトリ: worlds\udon\players
ファイル名: player-forces.md
拡張子: .md
サイズ: 1.75 KB
最終更新: 2025-06-05T03:07:52.824Z
```

---
title: "Player Forces"
slug: "player-forces"
hidden: false
createdAt: "2021-01-22T00:32:15.132Z"
updatedAt: "2021-01-22T01:07:11.156Z"
---
Here are the nodes relating to forces that act on Players. For nodes that deal with Player positions, see [Player Positions](/worlds/udon/players/player-positions) 

:::note Local player only

Only use these nodes for the [local player](/worlds/udon/players/getting-players/#networkingget-localplayer). They can't be used for remote players.

:::



### GetWalkSpeed / SetWalkSpeed
*float, working range around 0-5*

The speed at which a Player can walk around your world. Set this below your Run Speed. Default is 2.

### GetRunSpeed / SetRunSpeed
*float, working range around 0-10*

The speed at which a Player can run around your world. Set this above your Walk Speed. Default is 4.

### GetStrafeSpeed / SetStrafeSpeed
*float, working range around 0-5*

The speed at which a Player can move sideways through your world. Recommended to be set to the same as Walk Speed. Not affected by running. Default is 2.

### GetJumpImpulse / SetJumpImpulse
*float, working range around 0-10*

How much force applied when a player jumps. Default is 0, so set this if you want to enable jump in your world. The default VRCWorld prefab sets this to 3.

### GetGravityStrength / SetGravityStrength
*float, working range around 0-10*

Multiplier for the Gravity force of the world (set to Earth's default). Don't change Unity's Physics.Gravity in your project, get and set it here instead. Default is 1.

### Immobilize
*Boolean*

Set to true to keep a Player stuck in place, turning off their Locomotion. Note that VR Players may be able to move their viewpoint around a bit anyways, but their Avatar will stay in place.

---

## ドキュメント: player-positions.md

```metadata
階層: /worlds/udon/players/player-positions.md
ディレクトリ: worlds\udon\players
ファイル名: player-positions.md
拡張子: .md
サイズ: 4.56 KB
最終更新: 2025-06-05T03:07:52.825Z
```

---
title: "Player Positions"
slug: "player-positions"
hidden: false
createdAt: "2021-01-22T00:32:00.432Z"
updatedAt: "2023-03-28T19:07:26.827Z"
---
The player's position, rotation, and velocity can be accessed and changed with Udon. All of the following nodes require `VRCPlayerAPI` as an input.

For nodes that deal with forces relating to Players, see [Player Forces](/worlds/udon/players/player-forces). 
### GetPosition

Gets the position of the Player.

**Output**
- `Vector3`: The player's position in world space.  


### GetRotation

Gets the rotation of the Player.

**Input**

**Output**
- `Quaternion`: The player's rotation in world space.  

### GetBonePosition

Gets the position of the specified Bone in the Player's Avatar, or Vector3.Zero (0,0,0) if the bone does not exist. Note that Avatars may not have all the same bones in the places you expect, so be careful making assumptions about attributes like a player's height, pose etc based on the position of bones.

**Input**
- `HumanBodyBones`: The bone to check.

**Output**
- `Vector3`: The bone position in world space.
### GetBoneRotation

Gets the rotation of the specified Bone in the Player's Avatar, or Quaternion.Identity (0,0,0,1) if the bone does not exist. Note that Avatars may not have all the same bones in the places you expect, so be careful making assumptions about attributes like a player's height, pose etc based on the rotation of bones.

**Output**
- `Quaternion`: The bone rotation in world space.

### GetTrackingData

Gets a struct called TrackingData, which contains separate Position and Rotation data. This is the suggested way to get position and rotation data for a Player's head and hands. This returns data from the TrackingManager for a Local Player (ie the data coming from their headset / trackers) and from the RightHand, LeftHand and Head bones for a Remote Player. Origin returns the center of the local VR user's playspace, while returning the player's position for local Desktop users and all remote users. AvatarRoot returns data for the root transform of the avatar (the same transform that the player capsule is attached to). For users in Fully-Body Tracking, AvatarRoot will not rotate with the head facing direction. If you need data reflecting the general forward facing direction of a Player, consider using GetRotation instead.

**Input**
- `TrackingDataType`: The tracking data to check.

**Output**
- `TrackingData`: The player's tracking data of the specified type.  

### GetVelocity

Get the speed and direction of the player's movement.

**Output**
- `Vector3 velocity`: The player's velocity in world space.  

### SetVelocity

Set the speed and direction of the player's movement. If SetVelocity is called on the local player, their 'IsGrounded' property is set to false since they are not in direct control of their movements while this is happening.

**Input**
- `Vector3`: The player's velocity in world space.  

### IsPlayerGrounded

Get whether the player is touching the ground, which enables Jump.

**Ouput**
- `bool IsPlayerGrounded`: Whether the player is grounded.  

### TeleportTo

Send the [local player](/worlds/udon/players/getting-players/#networkingget-localplayer) to a new position and specified rotation, unless a Station does not allow it.

- Udon can only teleport the local player. Use [networking](/worlds/udon/networking/) to cause other players to teleport themselves. 
- Stations can prevent Udon from teleporting players.
- When you teleport very often or across very short distances, consider setting `lerpOnRemote` to `false`.

:::warning

Do not teleport players during network updates (e.g. [OnDeserialization](/worlds/udon/networking/network-components/#ondeserialization)). Otherwise, their avatar may unexpectedly collide with geometry during the teleport. Instead, use [SendCustomEventDelayedFrames](/worlds/udon/graph/special-nodes/#sendcustomeventdelayedframes) and delay the teleport by one frame.

:::

**Inputs**
- `Vector3 teleportPos`: The target position in world space.
- `Quaternion teleportRot`: The target rotation in world space.
- `SceneDescriptorSpawnOrientation TeleportOrientation` (optional): How to align players with the destination position and rotation. 
- `bool lerpOnRemote` (optional): Whether the interpolate the player's movement for remote players.
    - (Default) If `false`, the teleportation looks instant to remote players. The network bandwidth cost increases.
    - If `true,` the teleportation is treated like normal player movement, and movement looks smooth to remote players.


---

# ドキュメント終了

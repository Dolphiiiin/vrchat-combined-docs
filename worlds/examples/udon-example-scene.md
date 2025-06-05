# udon-example-scene Documentation

This document contains automatically collected documentation with metadata headers for each section.



---

## Document: avatar-scaling-settings.md

Path: /worlds/examples/udon-example-scene/avatar-scaling-settings.md
---
title: "Avatar Scaling Settings"
slug: "avatar-scaling-settings"
excerpt: "Configure avatar scaling in your world"
hidden: true
createdAt: "2023-06-24T00:00:00.076Z"
updatedAt: "2023-06-24T00:00:00.070Z"
---

This UdonBehaviour example script allows you easily to override your world's avatar scaling settings.

To learn how to write your own Udon script for avatar scaling, read our [avatar scaling documentation](/worlds/udon/players/player-avatar-scaling).

[IMAGE: avatar-scaling-example-component.png]

## Variables

| Name | Type | Default | Description |
| - | - | - | - |
| **disableAvatarScaling** | bool | false | | If enabled, players in your world will not be able to choose their own avatar scale, even if you enabled it on VRChat.com. |
| **minimumHeight** | float | 0.2 | The minimum avatar eye height in meters which players can choose. |
| **maximumHeight** | float | 5 | The maximum avatar eye height in meters which players can choose. |
| **alwaysEnforceHeight** | bool | false | If enabled, players who switch to use a very tall or very short avatar will automatically be set to "minimumHeight" or "maximumHeight". 

## Examples

- I want players to use avatar scaling freely.
	- You don't need to change any settings.
- I don't want players to use avatar scaling.
	- Turn on "disableAvatarScaling."
- I want players to use a specific avatar height.
	- Set "minimumHeight" and "maximumHeight" how you prefer.
	- If you want to prevent very tall or very short avatars, enable "alwaysEnforceHeight".

Here's what the Udon Graph for the example script looks like:

[IMAGE: avatar-scaling-example-component.png]


---

## Document: index.md

Path: /worlds/examples/udon-example-scene/index.md
---
sidebar_custom_props:
    customIcon: 🛝
---

# Udon Example Scene

This scene contains examples on how worlds can use Udon. You can [build & test](/worlds/udon/using-build-test) or publish this world to try it in VRChat.

## Prefabs
The Prefabs in this scene demonstrate common interactions with the VRChat components for buttons, avatar pedestals, stations, mirrors, and more.

All of the following Prefabs are included in the VRChat Worlds SDK. You can add them to any of your own worlds. For example, the [VRCWorld](#vrcworld) is included in most VRChat scenes by default.

 [IMAGE: UdonExampleScene]

### VRCWorld
This prefab makes it easy to upload your Unity scene to VRChat. It has four components:
- The [VRC Scene Descriptor](/worlds/components/vrc_scenedescriptor/) script, which defines basic properties of your world. It is required for every VRChat world.
- The [VRC Pipeline Manager](/sdk/vrcpipelinemanager/) script, which contains the world ID. It is added automatically with the VRC Scene Descriptor.
- The [VRCWorldSettings](/worlds/examples/udon-example-scene/player-mod-setter) Udon Graph program, which allows you to change the movement speed of players in your world.
- The [Avatar Scaling Settings](/worlds/examples/udon-example-scene/avatar-scaling-settings) Udon Graph program, which allows you to limit the avatar scale of players in your world.

If you'd like to use the VRCWorld prefab in your own Unity scene, you can find it in `\Packages\com.vrchat.worlds\Samples\VRCPrefabs\VRCWorld.prefab`.

### AvatarPedestal
This is available as a prefab, and has a program to switch a user into an avatar when they Interact with the pedestal, which is the default behavior of the prefab.

To change the interaction distance of the prefab, unpack the prefab and change the "Proximity" setting of the Udon Behaviour component.

### VRCChair3
This is available as a prefab, and has a program called `StationGraph`. It seats a player in the station when they Interact with it, and logs their display name when they enter or exit.

### MirrorSystem
This object has a **ToggleGameObject** program on it which uses the Interact event to flip a target object between Active/Inactive. In this case, it controls a VRCMirror object which is a child of this one.

### Cubes
These objects demonstrate some simple things you can do with Cubes, mostly reusing a **ChangeMaterialOnEvent** program to show how you can trigger custom events from other objects.

[IMAGE: Cube Examples]

### InteractCube
This object has a **SendEventOnInteract** program on it - which listens for the Interact event, triggered when a player points at an object and presses their 'use' button. In this case, it sends a custom event called "changeMaterial" to a **ChangeMaterialOnEvent** program on a child object, which changes out materials from an array whenever it receives this event. Both of these programs are reusable - you can change the eventName sent from the **SendEventOnInteract** program in the inspector on the UdonBehaviour component.

### TimerCube
This object has a **SendEventOnTimer** program on it - which runs a timer for a given *duration* and then sends the specified event. In this case, it sends a custom event called "changeMaterial" to a **ChangeMaterialOnEvent** program on a child object, which changes out materials from an array whenever it receives this event. Both of these programs are reusable - you can change the eventName sent from the **SendEventOnTimer** program in the inspector on the UdonBehaviour component, and change its *duration* in seconds to change how often it triggers.

### UseCube
The `Model` child of this object has a **SendEventOnUse** program on it - which can send out an event when the object is picked up and "Used". In this case, it sends a custom event called "changeMaterial" to a **ChangeMaterialOnEvent** program on a the `MaterialChanger` object, which changes out materials from an array whenever it receives this event. Both of these programs are reusable - you can change the eventName sent from the **SendEventOnUse** program in the inspector on the UdonBehaviour component. The `Model` object also has a `VRCObjectSync` component, which syncs the movement of this object to other users as it is moved around.

## Udon Variable Sync
These objects demonstrate different ways to sync variable values from the owner of an object to everyone else in the instance. 
[IMAGE: Udon Variable Sync Examples]

The "Canvas" item has many UI items with synced variables:

### ButtonSyncOwner
This is the first program described here which uses the [Manual Sync](/worlds/udon/networking#2-manual-variable) method. In the image below, you can see it has an OnClick() handler which calls UdonBehaviour.SendCustomEvent with a value of "OnClick". It's targeting the UdonBehaviour just below it, where it will run the custom event "OnClick". This is how UI Elements can run events on UdonBehaviours.

[IMAGE: Triggering Custom Events from Unity UI controls]

In the Graph Program, the OnClick event checks whether the player who clicked is the Owner of the object. If they are, it increases the "clickCount" variable by 1 and then calls **RequestSerialization**, which signals Udon to update the data on this Manual-synced UdonBehaviour.

[IMAGE: OnClick ▸ If Owner ▸ Set clickCount to clickCount + 1 ▸ Serialize.]

Notice that the 'Set clickCount' node has the 'sendChange' toggle turned on. this will trigger an event for everyone when they receive the new clickCount value.

[IMAGE: Variable Change events are very powerful! You can use them to run the same logic on the owner and others whenever a synced variable is changed.]

When clickCount is updated, this Change event triggers, which will then set the text of the button to the new value no matter who is the 'owner' of this program.

### ButtonSyncAnyone
This object uses a program very similar to **ButtonSyncOwner** above, but adds logic for users who click the button and are *not* the Owner of the object. In that case, they send the Custom Event "OnClick" to the Owner of the object. That's it! The owner will receive this event and process it as if they'd just clicked the button themselves.

[IMAGE: Non-Owners will send an event to the Owner to easily update the number.]

### ButtonSyncBecomeOwner
This object builds on the now-familiar ButtonSync program to demonstrate how to easily change ownership of an Object. When a non-owner clicks on the button, it will assign them ownership, and then update the variable. This is useful when you want to change multiple variables, or do logic more complicated than simply incrementing a value.
[IMAGE: ButtonSyncBecomeOwner]

When changing ownership of an object, some logic is run to decide whether or not the transfer is allowed. You can learn more about that here: [Networking](/worlds/udon/networking#object-ownership). If you don't add any custom logic, all Requests for Ownership will be approved. The nodes below show a simple setup checks a boolean variable called 'someSpecialLogic' to decide whether the Transfer will be approved. You could build your own logic based on the 'requester', the 'newOwner', or both.

[IMAGE: Does someone want to be the new owner? Check 'someSpecialLogic' that you've updated elsewhere.]

### SliderSync
[IMAGE: SliderSync]

This object has a program called **SliderSync** that works similar to **ButtonSyncBecomeOwner**. When someone moves the slider, they become the owner, and send the new value to everyone else. One difference is that when the "OnValueChanged" event is triggered from the UI, it will check whether this new value is different from the current value of the slider. This is because updating the value of the slider from Udon will also trigger this event, which would cause an infinite loop. So instead, we have some logic that makes sure the 'sliderValue' variable is different than the Slider's value before we run the rest of our logic. 

It also uses the Variable Change event to update a text field with the value of the slider whenever anyone becomes the owner and updates it.

### Toggle

This object has a program called **ToggleSync** which works the same as the Slider above. When someone changes the value to something Inequal to the current value, they become the owner, and send the new value to everyone else, 
[IMAGE: Toggle]

### Dropdown
 
[IMAGE: Dropdown]

This object has a program called **DropdownSync** which works the same as the Toggle and Slider above. When someone changes the value, they make sure it's different than the current value, then become the owner and send the new value to everyone else.

### InputField
 
[IMAGE: InputField]

This object has a program called **InputFieldSync** which works similar to the Dropdown above. When someone changes the value, they check that it's different, become the owner, and send the new value to everyone else.

### PickupCube
 
[IMAGE: PickupCube]

This object has VRC Pickup and VRC Object Sync components which enable it to be picked up and moved around, automatically syncing it to other users. The UdonBehaviour is set to "Continuous" instead of "Manual" like the sync programs above, since it needs to update more often to keep its Transform up-to-date. This UdonBehaviour has a **SyncPickupColor** program on it which smoothly changes the color while it's being held. It does this by checking during the Update event to see if the local player is the Owner of the object AND VRCPickup.get isHeld is true. Notice that it doesn't use RequestSerialization since it's set to Continuous - it will simply update the values as often as it can.

[IMAGE: This is run every frame, and when it's true the program will use some fun math to slowly change between two colors specified in the inspector.]

### PickupSphere
This object doesn't actually have any Udon on it! It simply uses VRC Pickup and VRC Object Sync components to let users pick it up and move it around in a synced manner.

[IMAGE: PickupSphere]

## PlayerDetection
It can be very useful to respond to a player moving into a certain area, or colliding with a physics object. These example programs illustrate a few ways that can be done.

### PlayerTrigger

[IMAGE: PlayerTrigger]

This is the most commonly used way to detect a player entering or leaving an area. The "TriggerArea" object has a see-through blue material on it, a Box Collider with *IsTrigger* checked, and an UdonBehaviour with a **PlayerTrigger** program source. 

In this program, the **OnPlayerTriggerEnter** and **OnPlayerTriggerExit** events are triggered whenever any player enters or exists the collider. The program then gets the **displayName** of that player and updates the text on the target canvas.
[IMAGE: PlayerTrigger Program]

### PlayerCollision
This setup demonstrates how to trigger and respond to the **OnPlayerCollisionEnter/Exit** events, which are triggered when a Physics object moves into a player's collider.
[IMAGE: PlayerCollision]

The *TriggerArea* object has a **FireOnTrigger** Udon Program which detects a player entering its trigger area, just like in the **PlayerTrigger** program above. In this case, this event is used to send the custom event "Fire" to the *Projectile* object. This will cause the Projecticle cube to add a force to itself which will move it towards the player. When it collides with the player, it will write that player's displayName into a target Text field.

Note that the PlayerCollision events here will only fire locally for the player that experienced them. If you want to inform other players of these events, you will need to add that functionality yourself through Synced Variables or Custom Network Events.

### PlayerParticleCollision

[IMAGE: PlayerParticleCollision Scene]

 This demo has a setup similar to PlayerCollision above, where it uses a Trigger Area to start other events. In this case, when a player enters the Trigger Area, the **SetActiveFromPlayerTrigger** program will turn on the *CollisionParticles* object. This object has a ParticleSystem which fires at the player with World Collision and Send Collision Messages turned on. The Udon Program **PlayerCollisionParticles** attached to this object will fire the **OnPlayerParticleCollision* events in the graph, which write the displayName of the affected player into the target text field.

[IMAGE: PlayerParticleCollision Program]

## [Udon Sync Player](/worlds/examples/udon-example-scene/udon-video-sync-player)

[IMAGE: Udon Sync Player]

This setup demonstrates one way to use the Unity / AVPro video players to load and sync video playback. It's a big program, so we've separated it out to its own page.

## CubeArraySync

[IMAGE: CubeArraySync]

This simple program makes a grid of cubes that randomly flip on and off when anyone clicks on them while using very little data, demonstrating the power of syncing arrays. The **CubeArraySync** program has a variable called *data* which is a Boolean Array with 25 values. This means it has 25 simple yes/no values which are synced to every user. It also has a GameObject array called *cubes* with 25 cubes.

When anyone clicks on the object containing all the cubes, a Custom Network Event called "Randomize" is sent to the owner of the CubeArraySync object. The owner then uses a **For** loop to randomly set each value to on or off, by generating a number between 0 and 1 and checking if that value is greater than 0.5. Once it updates the array variable, it calls **RequestSerialization** and then calls the custom event **UpdateCubes**. 

[IMAGE: Each user will turn on/off each cube based on the random value from Boolean array.]

The **UpdateCubes** event uses another **For** loop to step through each yes/no variable in the array and set its corresponding Cube to on or off. This event is triggered by the owner after updating the array, or by **OnDeserialization** after VRChat has updated the array variable for all the other users. We use OnDeserialization here instead of OnVariableChange because Array Variables don't currently fire Variable Change events, so we wait until we have new data in OnDeserialization and update our scene then.

## ObjectPool

[IMAGE: These cubes will never be this neat again once they start dropping and bouncing around.]

The [Object Pool](/worlds/udon/networking/network-components#vrc-object-pool) is a component that helps you manage a collection of objects. It will automatically sync its objects' Active state. This example program will drop boxes from the sky one at a time into a stacked grid, and when you click on a box, it is removed and respawned from the sky.

To do this, the **ObjectPool** program runs a simple timer and tries to **Spawn** an object at a regular interval. Each *Pooled Box* in its Pool has a simple **Pooled Box** program which saves its initial position on Start, restores that position whenever it is Enabled (which happens when it is spawned by the pool), and returns each object when you click on it.

## [Simple Pen System](/worlds/examples/udon-example-scene/simple-pen-system)
Even a basic pen takes quite a bit of work, so this example gets its own page.

---

## Document: player-mod-setter.md

Path: /worlds/examples/udon-example-scene/player-mod-setter.md
---
excerpt: Sets the player's default movement speed.
---
# Player Mod Setter

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

This UdonBehaviour example script allows you configure the movement of players in your world.

## Variables

| Name                  | Type  | Default | Description                                                                                                                                                                                     |
| --------------------- | ----- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Jump Height           | float | 3       | The jump strength of players. Affected by gravity.                                                                                                                                              |
| Run Speed             | float | 4       | - Keyboard input: The movement speed when moving forward or backward without holding the 'Shift' key.<br />- Analog stick input: The movement speed when tilting the stick forward or backward. |
| Walk Speed            | float | 2       | - Keyboard input: The movement speed when moving forward or backward without holding the 'Shift' key.<br />- Analog stick input: Always uses "Run Speed" instead of "Walk Speed."               |
| Strafe Speed          | float | 2       | The movement speed when moving left or right.                                                                                                                                                   |
| Gravity               | float | 1       | The amount that [gravity](https://docs.unity3d.com/ScriptReference/Physics-gravity.html) affects players.                                                                                       |
| Use Legacy Locomotion | bool  | false   | Enables VRChat's deprecated legacy locomotion system. Cannot be disabled by Udon later.                                                                                                         |

## Example

The UdonSharp program is called `PlayerModSetter.cs` and the Udon Graph program is called `VRCWorldSettings.asset`. They work very similarly, and either one can be used in your VRChat world.

<Tabs groupId="udon-compiler-language">
<TabItem value="graph" label="Udon Graph">

This Graph program can be found in `Packages/com.vrchat.worlds/Samples/UdonExampleScene/UdonProgramSources/VRCWorldSettings.asset/`.

[IMAGE: An Udon Graph program that sets various attributes for the player when they join the world. It contains several text comments explaining basic Graph syntax.]

</TabItem>
<TabItem value="cs" label="UdonSharp">

This is a simplified example of the UdonSharp script `\Assets\UdonSharp\UtilityScripts\PlayerModSetter.cs`.

```cs showlinenumbers
using UnityEngine;
using VRC.SDKBase;

public class PlayerModSettings : UdonSharpBehaviour
{
    VRCPlayerApi playerApi;

    [Header("Player Settings")]
    [SerializeField] float jumpImpulse = 3;
    [SerializeField] float walkSpeed = 2;
    [SerializeField] float runSpeed = 4;
    [SerializeField] float gravityStrength = 1;

    void Start()
    {
        playerApi = Networking.LocalPlayer;
        playerApi.SetJumpImpulse(jumpImpulse);
        playerApi.SetWalkSpeed(walkSpeed);
        playerApi.SetRunSpeed(runSpeed);
        playerApi.SetGravityStrength(gravityStrengh);
    }
}
```

</TabItem>
</Tabs>

---

## Document: simple-pen-system.md

Path: /worlds/examples/udon-example-scene/simple-pen-system.md
---
title: "Simple Pen System"
slug: "simple-pen-system"
excerpt: "Simple is a relative term."
hidden: false
createdAt: "2021-05-14T00:44:52.076Z"
updatedAt: "2021-05-20T23:00:49.470Z"
---
The Simple Pen system consists of two programs - one for the Pen, and one for each Line that will be drawn

# How the Pen and Lines Work Together

### Pen
The pen has VRCPickup and VRCObjectSync components, which provide the basic pickup and sync functionality. The program itself is uses Continuous sync since that works well with these components.

### Lines
Each line object has its own LineRenderer and a program with Manual Sync, since it doesn't need to update as often as the pen. The line has a **points** variable which is Vector3 array. This variable is the main way that data for the lines are synced for everyone in the instance.

## Drawing Starts
When someone uses the Pen, this calls **OnPickupUseDown** on the pen. This will cause a few things to happen in the program:
* A new Line is retrieved from the pool and saved in a variable
*The player with the pen is made the Owner of the line
* *isDrawing* is set to true
* The line is reset to have two points with their positions at the tip of the pen
* a variable is incremented to track which line will be used next.

## Drawing Continues
Every frame, the **Update** event is called on the pen, and the following logic is run:
* If *isDrawing* is true, we continue:
* If the pen has moved more than *minMoveDistance*, we continue:
* We add a new point to the LineRenderer at the position of the *penTip*
* We check if we've queued up enough points to send by comparing currentIndex against pointsPerUpdate
* If we're ready to update the data, we call **OnUpdate** on the UdonBehaviour on the target line.

When this **OnUpdate** method is called on the line, the program retrieves the current positions of the points in the line, updates the synced *points* variable, and calls **RequestSerialization**, which is how Manual-synced UdonBehaviours tell VRChat to send out the queued data. This method is only called on the owner of the line. Everyone else receives the data, and then has their **OnDeserialization** method called. When this method triggers on a line, the line program reads the positions from the *points* array, and uses them to update the positions in their own line.

## Drawing Finishes
When the user lets go of the Use button, the **OnPickupUseUp** event is called on the pen. This event simply sets *isDrawing* to false, and calls **OnFinish** on the target line's UdonBehaviour. This will send the **OnUpdate** method one more time to make sure the **points** data is up-to-date for everyone.

---

## Document: udon-video-sync-player.md

Path: /worlds/examples/udon-example-scene/udon-video-sync-player.md
---
title: "Udon Video Sync Player"
slug: "udon-video-sync-player"
excerpt: "Basic example of synced video"
hidden: false
createdAt: "2021-05-14T00:58:34.059Z"
updatedAt: "2021-08-05T19:51:13.507Z"
---
[IMAGE: Image]

## Overview
There's two main things we need to sync for people to watch videos together - the URL of the video to watch, and the playback time so people are watching things simultaneously. In order to understand how we sync these two items for everyone, including late joiners - let's walk through a scenario that uses this program.

**The flow for someone entering a URL is: **
Become Owner of UdonSyncPlayer Object▸send new **_url_** ▸Try to Load & Play Url ▸When Video Starts, send Sync info out ▸Send new Sync info every **_syncFrequency_** seconds

**The flow for everyone else is:**
Receive new **_url_** value ▸Try to Load & Play Url ▸Receive Sync Info▸Jump to synced time

## Someone Loads a URL
When our hypothetical scene loads, let's say there is no video playing yet, and there are two people in the room. Someone pastes a new URL into the Input Field, which triggers the **OnURLChanged** event which is wired up in the UI.
[IMAGE: When someone enters a new URL, this logic runs to send the new URL to everyone else.]
There's a few 'IsValid' calls in here that we use just to make sure we're not trying to call methods on objects that have been destroyed or improperly set up. We'll skip describing these for the rest of this example these to keep the explanations simpler.

The Local Player has just put in a new URL, so we make them the Owner of the program so they can control its variables. We get the URL from the InputField, then call **SetProgramVariable** on the **_url_** symbol with this new value. This works the same as calling **set url** with "sendChange" enabled, it's just another way to do it, handy to know about if you want to change the variable on another UdonBehaviour. Once we've updated this variable, we call **RequestSerialization** to ask Udon to update the value of **_url_** for everyone else in the world.


## Users Get New URL
[IMAGE: Whenever the synced **_url_** variable changes, try to Play it!]
Since we have a **Variable Change** event for **_url_** in our graph, this event will be triggered whenever the URL is updated, and it will simply try to play the URL.


## The Video Starts
[IMAGE: Image]

This event is triggered locally when the video actually beings playing. We call the same event for the Owner and everyone else - the different logic is handled in **UpdateTimeAndOffset**.

## Update Time and Offset
[IMAGE: Image]

First, this logic checks whether it's running on the Owner of the object. If it's not, it runs the **Resync** event instead. If it is on the owner, the we want to sync both _where_ in the video we are, and _when_ we were there. We should be at the very beginning of the video since this is the first time the logic is running, but by saving both of these values, we can use this for future sync updates as well. 

We want to sync two numbers to everyone else, and these two numbers are closely related, so we combine them into a single Vector2 variable in order to keep them together and simplify some of the sync logic. We construct a Vector2 where 'x' is the current time of the video and 'y' is the Server Time observed by the owner when they were at that video time. With this info, everyone else can set themselves to a matching time - see [Resync](/worlds/examples/udon-example-scene/udon-video-sync-player#resync)  below.

After **Requesting Serialization** of this synced variable, the owner calls **SendCustomEventDelayedSeconds** to update this value again. They use the variable **_syncFrequency_** to determine how long until they update the value. For a _very_ simple approach, this variable can be left at 0 if the owner never pauses, rewinds or fast-forwards the video, and everyone can sync from the start time of the video instead of updating **_timeAndOffset_** every so often.

## Resync
[IMAGE: Image]

When non-owners start playing the video or receive an update to the **_timeAndOffset_** variable, they can use the data to figure out where to jump to in the video.

For a simple example, let's say the owner was at video-time **0** at server-time **1000**.
  * Owners sets **_timeAndOffset_** to (0,1000).
  * You join 45 seconds later and get this value. Your own server-time is **1045**, so you jump to **00:45** in the video by finding the difference in the server time (45 seconds) and adding the video-time (0 seconds).

## Improvements and Augmentations
[IMAGE: Image]

We kept this example pretty simple so it would be understandable and upgradeable. There's lots you could do to improve it and share your changes! Here are some ideas:

* Have non-owners wait to play the video until they receive info from the Owner
* Detect stream urls vs videos and turn off syncing
* Handle Video Error events with helpful notes for users
* Only allow certain players to change videos
* Create video playlists
* Create a video Queueing system

---

## Document: world-audio-settings.md

Path: /worlds/examples/udon-example-scene/world-audio-settings.md
---
title: World Audio Settings
createdAt: 2023-01-02
updatedAt: 2023-01-02
excerpt: Configure voice and avatar settings in your world.
---
:::note

This script is currently not present in the Udon example scene.

:::

This UdonBehaviour example script allows you configure player voices and avatar audio in your VRChat world.

This script sets these values once when a player joins your world. You may use other scripts to change these values later.

For more information, please read the [Player Audio](/worlds/udon/players/player-audio/) documentation.

---

# End of Documentation

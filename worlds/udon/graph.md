# graph Documentation

This document contains automatically collected documentation with metadata headers for each section.



---

## Document: event-nodes.md

Path: /worlds/udon/graph/event-nodes.md
# Event Nodes

This is a list of Udon Nodes that are considered "Events".

Your scripts can use events to detect actions and set off chains of action or logic. [Input Events](/worlds/udon/input-events)  have their own special page. To jump to an Event in the graph, click it in the Graph Sidebar.

All below nodes have flow nodes where logic requires it.

:::note

More events specifically related to networking are listed on the [Network Components](/worlds/udon/networking/network-components/#networking-events) page.

:::

### Interact
Fired when the local player interacts with this GameObject.

- Players can only interact with GameObjects that have a Collider component and an UdonBehaviour component.
- If you want players to interact with a 2D UI, use [VRC Ui Shape](/worlds/components/vrc_uishape/) and a Button component instead.

### OnDrop
Fired when the local player drops this object after being held.

### OnPickup
Fired when this object is picked up by the local player.

### OnPickupUseDown
Fired when the local player is holding this object presses the "Use" button. Fires when the button is pressed. Requires 'Auto Hold' on Desktop.
 
### OnPickupUseUp
Fired when the local player is holding this object presses the "Use" button. Fires when the button is released. Requires 'Auto Hold' on Desktop.

### OnPlayerJoined
Outputs: `player` - `VRC.SDKBase.VRCPlayerApi`

Fired when any player joins the instance. Outputs the `player` that joined.

When you join an instance, you execute OnPlayerJoined for every player in the instance, including yourself. When another player joins your instance, you only execute OnPlayerJoined for the player who joined.
 
### OnPlayerLeft
`Event_OnPlayerLeft`

Outputs: `player` - `VRC.SDKBase.VRCPlayerApi`

Fired when any player in the instance leaves. Outputs the `player` that left.

### OnPlayerRestored
`Event_OnPlayerRestored`

Outputs: `player` - `VRC.SDKBase.VRCPlayerApi`

Triggered after all persistent data of a player in the instance has finished loading, including all of their [PlayerData](/worlds/udon/persistence/player-data) and [PlayerObjects](/worlds/udon/persistence/player-object). Outputs the `player` whose data was loaded.

When you join an instance, you execute OnPlayerRestored for every player in the instance, including yourself. When another player joins your instance, you only execute OnPlayerRestored for the player who joined.

### OnStationEntered
`Event_OnStationEntered`

Fired when the local player enters the station on this object.
 
### OnStationExited
`Event_OnStationExited`

Fired when the local player exits the station on this object.
 
### OnVideoEnd
`Event_OnVideoEnd`

Fired when the video player on this object is finished playing, either via the end of the video or via player interaction.

### OnVideoError
`Event_OnVideoError`

Outputs: `videoError` - `VRC.SDK3.Components.Video.VideoError`

Fired when the video player encounters an error loading the video.

### OnVideoLoop
`Event_OnVideoLoop`

If looping is enabled, fired when the video player finishes a loop.
 
### OnVideoPause
`Event_OnVideoPause`

Fired when the video player on this object is paused.
 
### OnVideoPlay
`Event_OnVideoPlay`

Fired when the video player on this object starts playback, either via the start of a new video in a queue, unpausing, or via player interaction.

### OnVideoStart
`Event_OnVideoStart`

Fired when the video player on this object starts playback from a stopped state.

### OnVideoReady
`Event_OnVideoReady`

Fired when the video player loads a new video.
## Player Events
### OnPlayerTriggerEnter
`Event_OnPlayerTriggerEnter`

Outputs: `player` - `VRC.SDKBase.VRCPlayerApi`

Fired when the capsule collider of any player in the instance enters a trigger collider.

### OnPlayerTriggerStay
`Event_OnPlayerTriggerStay`

Outputs: `player` - `VRC.SDKBase.VRCPlayerApi`

Fired every frame while the capsule collider of any player in the instance is inside a trigger collider.

### OnPlayerTriggerExit
`Event_OnPlayerTriggerExit`

Outputs: `player` - `VRC.SDKBase.VRCPlayerApi`

Fired when the capsule collider of any player in the instance exits a trigger collider.

### OnPlayerCollisionEnter
`Event_OnPlayerCollisionEnter`

Outputs: `player` - `VRC.SDKBase.VRCPlayerApi`

Fired when the capsule collider of any player in the instance enters a collider.

### OnPlayerCollisionStay
`Event_OnPlayerCollisionStay`

Outputs: `player` - `VRC.SDKBase.VRCPlayerApi`

Fired every frame while the capsule collider of any player in the instance is inside a collider.

### OnPlayerCollisionExit
`Event_OnPlayerCollisionExit`

Outputs: `player` - `VRC.SDKBase.VRCPlayerApi`

Fired when the capsule collider of any player in the instance exits a collider.

### OnPlayerParticleCollision
`Event_OnPlayerParticleCollision`

Outputs: `player` - `VRC.SDKBase.VRCPlayerApi`

Fired when a particle collides with the capsule collider of any player in the instance, assuming that particle system has "Collision" and "Send Collision Messages" turned on.

### OnPlayerRespawn
`Event_OnPlayerRespawn`

Outputs: `player` - `VRC.SDKBase.VRCPlayerApi`

Fired when the local player respawns by clicking "Respawn" in their menu.

### OnScreenUpdate
`Event_OnScreenUpdate`

Outputs: `data` - `VRC.SDK3.Platform.ScreenUpdateData`

Triggered when the local player first enters the world on a Mobile Device, and whenever the orientation of their device changes. Outputs a `ScreenUpdateData` struct with the following values:
* `type` - `ScreenUpdateType` - only `OrientationChanged` for now, can be expanded in the future.
* `orientation` - `VRCOrientation` - the orientation of the player's device, as a `VRC.SDKBase.Platform.VRCOrientation` enum, which is `Landscape` or `Portrait`.
* `resolution` - `Vector2` - the resolution of the player's device, as a `Vector2` struct. 

### OnInputMethodChanged
`Event_OnInputMethodChanged`
Outputs: `inputMethod` - `VRC.SDKBase.VRCInputMethod`
Fired when the local player uses a different input method - Keyboard, Mouse, Controller, etc.

### OnLanguageChanged
`Event_OnLanguageChanged`
Outputs: `language` - `string`
Fired when the local player updates their display language.

### OnPlayerSuspendChanged
`Event_OnPlayerSuspendChanged`

Outputs: `player` - `VRC.SDKBase.VRCPlayerApi`

Fired when the device of any player in the instance becomes suspended. A device is suspended if it enters sleep mode or switches to a different app. For the player that is being suspended, this event will fire on wakeup. Check `VRCPlayerApi.isSuspended` to know if this is a wakeup or suspend event.

While suspended, devices don't run Udon code or respond to network events until the player reopens VRChat.

When you create multiplayer interactions in VRChat, you should react to suspended players to ensure that your Udon code continues running as intended. For example, you may want to transfer the ownership of important objects to [a player who is not suspended](/worlds/udon/players/#get-issuspended).

Your code should account for any device becoming suspended at any time, regardless of platform. PC players currently never become suspended, but this should not be assumed.

### OnVRCPlusMassGift
`Event_OnVRCPlusMassGift`

Outputs: 
* `gifter` - `VRC.SDKBase.VRCPlayerApi`
* `numGifts` - `int`

Fired when any player in the instance drops a gift bomb.

### OnVRCCameraSettingsChanged
`Event_OnVRCCameraSettingsChanged`

Outputs: `camera` - `VRC.SDK3.Rendering.VRCCameraSettings`

Fired when the user changes certain options in the VRChat graphics settings, e.g. "Near Clip Override" or "Field of View."

Changing values yourself via `VRCCameraSettings` will _not_ trigger this event! Similarly, camera `Position` and `Rotation` will not trigger it, since those change almost every frame anyway.

The `camera` object passed in refers to either the `ScreenCamera` or the `PhotoCamera` (see [VRCCameraSettings](/worlds/udon/vrc-graphics/vrc-camera-settings)). Note that this event will trigger every frame while the user is using the zoom slider on the `PhotoCamera` or a Dolly path is adjusting the zoom value, since they affect the `FieldOfView` value.

Changing the screen resolution, including by resizing the VRChat window, will also call this event every frame. It is recommended to do minimal processing to avoid affecting performance.

This event may trigger multiple times per frame.

### OnVRCQualitySettingsChanged
`Event_OnVRCQualitySettingsChanged`

Fired when the user adjust Graphics settings affecting one or more values in `VRC.SDK3.Rendering.VRCQualitySettings`.

Similarly to `OnVRCCameraSettingsChanged`, this event may trigger often, and it is recommended to keep processing light.

### Advanced Notes
All nodes in this list have the type `System.Void`.


---

## Document: graph-elements.md

Path: /worlds/udon/graph/graph-elements.md
---
title: "Graph Elements"
slug: "graph-elements"
hidden: false
createdAt: "2020-06-24T22:08:14.066Z"
updatedAt: "2022-10-18T23:46:16.884Z"
---
## Overview
When you build programs in the graph, you mostly use Nodes. There are a few other items available, described here.

## Groups
Groups are helpful for organizing and describing your graph. They don't *change* the way your graphs function or compile.
[IMAGE: You can select elements and right-click to get the *Create Group* function.]
To create Groups, you can:
* Right-Click in the graph and choose 'Create Group', then drag and drop elements into the group.
* Select elements with a box-drag or by holding Ctrl while you click on them, then right-click on the graph and choose 'Create Group', or press 'Ctrl+G' for quick grouping.

To remove items from Groups, you can:
* Select the items, hold shift, then drag the items out of the group.
* Select the items, right-click and choose 'Remove From Group'.

To jump to a Group in the graph, click it in the Graph Sidebar.
:::note Nested Groups

Currently, nested groups are not supported. The current grouping behavior is to delete the existing group if you try to select and enclose it with a new group.
:::
## Comments

Comments are simple blocks of text that you can place near items that need more information.
[IMAGE: A comment next to a node.]
You can create comments by right-clicking on the Graph and choosing *Create Comment*. They can be added to groups, as well. You can use comments and groups together for ultimate readability!
[IMAGE: The 'SendEventOnTimer' graph included in the VRChat Examples folder uses groups and comments to explain what's happening in the graph.]
## Noodles
The noodles of a graph are what connect everything together, and how Udon gets its name! They connect nodes together through their ports. They are colored according to the ports to which they connect, so if you connect two ports with different types, you'll see the color change halfway through. In Unity documentation and elsewhere, they are called 'edges' - so if you see a reference to edges, they're talking about our tasty noodles.

---

## Document: index.md

Path: /worlds/udon/graph/index.md
---
sidebar_position: -1
---
# Udon Node Graph

The Udon Node Graph is the default interface for creation of Udon programs. This section goes over how to use it. If you want to dive right into examples, take a look at the [Udon Example Scene](/worlds/examples/udon-example-scene).

## Interface

You can open up the Udon Graph window using the Menu Item under **VRChatSDK > Udon Graph**, or by clicking the **Open Udon Graph** button on an UdonBehaviour Component.

[IMAGE: The Udon Window]

If you open the window through the Menu command, you'll see the welcome screen, which has a changelog and some settings.

Multiple Graphs can be opened simultaneously, and you can switch between them using the tabs at the top of the Graph Window. 

You can close tabs, by clicking the X in the corner of the tab you want to close. Graph Tabs are not "real" tabs, and simply reopen each tab as you select them. This means switching tabs takes as long as opening Graphs.

## Flow
The Flow of your graph defines which nodes will run, and the order in which they'll do it.
[IMAGE: Image]

The triangles in the picture above are the _Flow_ ports, and they trigger in order from left to right, following the noodles that connect them. To understand what is happening in Udon graphs and to make your own, _follow the flow_. 

There is a "Highlight flow" toggle on the topbar, which, when enabled, will highlight the nodes connected via the flow edges, allowing you to quickly see how does the program arrive to the particular node. 
[IMAGE: Image]

If the node doesn't have any flow connections, then nothing will happen.

In the graph above:
1. The _Start_ event triggers when the world loads
2. The Branch triggers next. It checks the value of its checkbox and then triggers either the *True* or *False* path.
3. If the value is True, then we trigger the top node, which sends a Custom Event called "Hello".
4. Otherwise, we will send a Custom Event called "Goodbye"

It's ok if you don't know what **Sending a Custom Event** means yet. Learning to read the flow of a graph is the first step to understanding what they do.

## Creating Nodes
Nodes are the boxes that represent the methods you can trigger. Building a graph consists of creating and connecting nodes together to create a program.

There are several ways to create nodes:
  * [Hotkeys](#hotkeys)
  * [Drag-and-Drop actions](#drag-and-drop-for-gameobjects-and-components)
  * [Search menus](#searching-for-nodes)

### Hotkeys
Press and hold one of the following keys, then click anywhere on the graph to create the corresponding node:
* `1` : float
* `2` : Vector2
* `3` : Vector3
* `4` : Vector4
* `+` : float addition
* `-` : float subtraction
* `=` - float equality comparison
* `b`: Branch
* `shift+b` : Block 

### Other Hotkeys:
* Press and hold "C", then click on a constant to convert it into a variable.
* Shift+A aligns selected nodes 
* Press and hold Ctrl+G for quick grouping
* L+Click logs the value of the selected node 
* Press and hold "Shift+F", then click on a node that outputs an array type, to generate a foreach loop automatically
Many of these features are also available in the right-click menus for their respective nodes.
[IMAGE: Making for loops the easy way!]
### Drag and Drop for GameObjects and Components

If you want to add interactivity to a GameObject or Component, you can drag and drop them from your hierarchy to the graph. For example, you can drag and drop a Light component by grabbing dragging from the 'Light' title onto the graph.
[IMAGE: Easy way to get a reference to a Light component so you can play with it.]
Creating nodes via Drag and Drop this way creates Variables that are tied to your GameObject or Component, so you'll see a new variable appear in the Variables window, and a node which is actually a "Get Variable" node which is automatically set up to get your new Component.

### Drag and Drop for Variables

You can create variables of any type by pressing the **+** button in the Variables pane of the Graph Sidebar. Then you can drag and drop the variable name onto the graph to create a "Get Variable" node, hold the **Ctrl** key while dragging to make a "Set Variable" node, or hold the **Alt** key to make an "On Variable Changed" node.

### Searching for Nodes

Press the **Spacebar** to open up Quick Search, then type in the first few letters of the class you want to interact with.
[IMAGE: Image]

This method of searching works best if you know Unity's basic classes and object types. There are other ways of searching, see: [Searching for nodes](/worlds/udon/graph/searching-for-nodes)

## Compiling the Graph
The graph automatically compiles in the background at regular intervals. When this happens, you'll see a flash in the upper-right corner of your graph, and the Status box will turn green if things went well, or red if there's an issue. In either case, you can click on the Status box to see the Assembly Code that was generated, or the errors if there was a problem.

[IMAGE: The status box shows 'OK' and we can see the Variables declared at the top of this Assembly.]
## Running the Graph
There are two ways to run the graphs in your scene before you upload them to VRChat.

### Running In-Editor
You can use Unity's Play Button to run your scene directly in the editor to test out some graphs. This will work for some simple methods and logic, but the following items won't work as expected:
* Synced Variables & Networked Events

### Running Build & Test
Use the VRChat SDK Window to do Local Testing. This takes slightly longer as it bundles your content into an offline world and launches the actual VRChat client to give you an Avatar that can Interact with objects and VRCPlayerAPI requests.
[IMAGE: The simplest way to test Sync features is to launch 2 local clients.]

To test Synced variables and NetworkEvents, you'll need two clients - you can use the 'Number of Clients' field to launch up to 8 local clients that will launch in a private test world. They will all have the same DisplayName, but they'll otherwise be recognized as separate players so you can test out your interactions.

If 'Force Non-VR' doesn't work for you, then switch to the 'Settings' tab of the VRChat SDK Window and set your VRChat Client Path to point at your actual VRChat installation.

[IMAGE: Installed Client Path setting in the VRChat SDK]

## Uploading Your World
You will be able to Build & Test as soon as you [make a VRChat Account](https://vrchat.com/home/register). In order to publish a world so others can visit, you need to spend some time in VRChat - visit worlds, make some friends and get inspired!


---

## Document: searching-for-nodes.md

Path: /worlds/udon/graph/searching-for-nodes.md
---
title: "Searching for Nodes"
slug: "searching-for-nodes"
hidden: false
createdAt: "2020-06-24T04:27:16.858Z"
updatedAt: "2022-10-18T23:45:45.454Z"
---
## Quick Search

Press the **Spacebar** to open up Quick Search, then type in the first few letters of the class you want to interact with.

[IMAGE: Image]

## Full Search

Press **Tab** to open up Full Search, then you can search for any method on any object. For GameObject.GetName, you could just search for 'getname' and see all the objects that have a way to get the name of the object. You could also search directly for 'gameobject.getname' and you will be directed to exactly the right node. This search method is slower than Quick Search, so it should be used only when you're not sure which class to look for in Quick Search.

[IMAGE: image]
## Search Bar

There is a searchbar at the top of the Udon Graph Sidebar which allows you to search through your graph. 
You can use "Ctrl+F" to to focus it from the Graph Window.
The search will begin to return results after you enter more than 3 characters.
Pressing "Enter" when there are search results, will jump between the results, in the order of "best match first".

[IMAGE: Image]

## Focused Search

This mode is turned off by default, so first you need to go to your Welcome Screen by clicking on 'Welcome' in the upper-left corner of a graph. Then check the box next to 'Focus Search On Selected Node'. This gives you a shortcut to skip to Quick Search part two - if you have a GameObject.GetName node, you can select it and press **Spacebar** to open up a search for more **GameObject** methods.

[IMAGE: Search on particular classes by pressing **Spacebar** with a Node selected]
## Search on Noodle Drop

This mode is also off by default - you'll need to open your Welcome Screen and check the box next to 'Search on Noodle Drop'. Once it's on, you can drag a noodle from any port and drop it into empty space to open up a search for **any** node that could connect to that port. This works forwards and backwards, and will also search for Variables that could be connected.

[IMAGE: image]

---

## Document: special-nodes.md

Path: /worlds/udon/graph/special-nodes.md
# Special Nodes

The "Special" category contains nodes for custom variables, custom events, flow control, and communicating with other UdonBehaviours.

### Block
Splits flow into multiple sections. One flow input, multiple flow output. Executes all right-side flow slots from top to bottom.

### Branch
Inputs: `Bool` - `System.Boolean`

Branches execution based on a conditional evaluation. If `Bool` is True, `True` flow path is executed. If `Bool` is False, `False` flow path is executed.

### Comment
Provides a space for the user to type a comment string. This string is not included during compilation.

### Const Null
Provides a "null" value for nullchecking purposes.

### Const This
Provides a reference to the GameObject that the UdonBehavior is a component of.

### Event Custom
Inputs: `name` - `System.String`

A a custom event that can be executed by [UdonBehaviour nodes](#udonbehaviour-nodes). 

You must name the custom event by typing its name in the Graph. The event name cannot be changed while your program is running.

### For
Inputs: `start`, `end`, `step` - `System.Int32`

Outputs: `index` - `System.Int32`

Executes flow by using a counter. A counter is initalized with the value of `start`. The `Body` flow is executed, and then the counter is incremented by the `step` value. This continues until the counter's value is equal or greater than `end`. Once that has occured, flow continues along on the `Exit` flow.

### Get Variable
Inputs: `name` - `System.String`

Outputs: `System.Object`

Gets the Udon variable named `name` and provides it as output.

### Set Variable
Inputs: `name` - `System.String` `value` - `System.Object` `sendChange` - `Boolean`

Sets the Udon variable named `name` to `value` when flow is executed. If `sendChange` is checked, it will also trigger the OnVariableChanged event for that variable.

### Get Program Variable
Inputs: `instance` - `UdonBehaviour` `symbolName` - `string`

Get the value of an Udon variable `symbolName` from another UdonBehaviour `instance`. Works best if the target UdonBehaviour is a public variable and it's wired up in the Inspector, which allows you to choose the target variable name from a dropdown. If there's no UdonBehaviour connected to instance, it will use the current UdonBehaviour's variable names. If you instead know the name of the variable and want to set it directly, use a `String const` node to write it in by hand.

### Set Program Variable
Inputs: `instance` - `UdonBehaviour` `symbolName` - `string` `value` - `Object`

Sets the value of an Udon variable `symbolName` on another UdonBehaviour `instance` to `value`. Works best if the target UdonBehaviour is a public variable and it's wired up in the Inspector, which allows you to choose the target variable name from a dropdown. If there's no UdonBehaviour connected to instance, it will use the current UdonBehaviour's variable names. If you instead know the name of the variable and want to set it directly, use a `String const` node to write it in by hand. This node will also trigger the OnVariableChanged event for the target variable.

### On Variable Changed
Outputs: `newValue` `oldValue`

Triggers whenever SetProgramVariable is called on the target variable, or when Set Variable is called with `sendChange` checked. Works for synced variables, too!

### While
Inputs: `Bool` - `System.Boolean`

Executes the flow of `Body` while `Bool` is true. If `Bool` is false, executes `Exit` flow.

## UdonBehaviour Nodes
Udonbehaviour nodes allow your programs to interact with other UdonBehaviours - either locally, with a delay, or over the network.

:::note UdonSharp

The nodes below only work for `public` events. If you use the Udon Graph, your custom events are always `public`. If you use UdonSharp, make sure to use `public` instead of `private` events here!

:::

### SendCustomEvent
Inputs: `instance` - `UdonBehaviour`, `eventName` - String

Runs the event 'eventName' on the target UdonBehaviour. If `instance` is left blank, it points to one of its own events.

### SendCustomEventDelayedFrames
Inputs: `instance` - `UdonBehaviour`, `eventName` - String, `delayFrames` - int, `eventTiming` - EventTiming

Runs the event `eventName` on the target UdonBehaviour, after waiting for `delayFrames`. It will run the event during Update or LateUpdate, depending on which `eventTiming` is selected. Minimum of 1 frame delay.

:::note Timing issues

Note that [Unity's frame count](https://docs.unity3d.com/ScriptReference/Time-frameCount.html) is based on the Update event. If you call SendCustomEventDelayedFrames [before the Update event](/worlds/udon/event-execution-order), such as [Start](https://docs.unity3d.com/ScriptReference/MonoBehaviour.Start.html) or an [Input event](/worlds/udon/input-events), the delay may be 1 frame shorter than expected.

:::

### SendCustomEventDelayedSeconds
Inputs: `instance` - `UdonBehaviour`, `eventName` - String, `delaySeconds` - float, `eventTiming` - EventTiming

Runs the event 'eventName' on the target UdonBehaviour, after waiting for `delaySeconds`. It will run the event during Update or LateUpdate, depending on which `eventTiming` is selected.

If `delaySeconds` is zero, the event will be executed in the same frame *or* the next frame (see [SendCustomEventDelayedFrames](/worlds/udon/graph/special-nodes#sendcustomeventdelayedframes) above).

### SendCustomNetworkEvent
Inputs: `instance` - `UdonBehaviour`, `target` - NetworkEventTarget, `eventName` - String

Runs the event `eventName` on the target UdonBehaviour for remote players according to `target`. Has overloads for including parameters.

Check the page on [Network Events](/worlds/udon/networking/events) for more details.

---

## Document: type-nodes.md

Path: /worlds/udon/graph/type-nodes.md
---
title: "Type Nodes"
slug: "type-nodes"
hidden: false
createdAt: "2020-03-20T20:08:24.139Z"
updatedAt: "2020-03-21T01:06:17.230Z"
---
These nodes are used to get references to certain types. All of them have one output of the type they're referring to.

We are only including documentation for VRChat types here. For information on the Unity types Udon refers to, please see their documentation.

### VRCSDK3ComponentsVRCAvatarPedestal
Type definition for `VRC.SDK3.Components.VRCAvatarPedestal`.

### VRCSDK3ComponentsVRCAvatarPedestal[]
Type definition for `VRC.SDK3.Components.VRCAvatarPedestal[]`.

### VRCSDK3ComponentsVRCAvatarPedestalRef
Type definition for `VRC.SDK3.Components.VRCAvatarPedestal`.

### VRCSDK3ComponentsVRCMirrorReflection
Type definition for `VRC.SDK3.Components.VRCMirrorReflection`.

### VRCSDK3ComponentsVRCMirrorReflection[]
Type definition for `VRC.SDK3.Components.VRCMirrorReflection[]`.

### VRCSDK3ComponentsVRCMirrorReflectionRef
Type definition for `VRC.SDK3.Components.VRCMirrorReflection`.

### VRCSDK3ComponentsVRCPickup
Type definition for `VRC.SDK3.Components.VRCPickup`.

### VRCSDK3ComponentsVRCPickup[]
Type definition for `VRC.SDK3.Components.VRCPickup[]`.

### VRCSDK3ComponentsVRCPickupRef
Type definition for `VRC.SDK3.Components.VRCPickup`.

### VRCSDK3ComponentsVRCPortalMarker
Type definition for `VRC.SDK3.Components.VRCPortalMarker`.

### VRCSDK3ComponentsVRCPortalMarker[]
Type definition for `VRC.SDK3.Components.VRCPortalMarker[]`.

### VRCSDK3ComponentsVRCPortalMarkerRef
Type definition for `VRC.SDK3.Components.VRCPortalMarker`.

### VRCSDK3ComponentsVRCStation
Type definition for `VRC.SDK3.Components.VRCStation`.

### VRCSDK3ComponentsVRCStation[]
Type definition for `VRC.SDK3.Components.VRCStation[]`.

### VRCSDK3ComponentsVRCStationRef
Type definition for `VRC.SDK3.Components.VRCStation`.

### VRCSDKBaseInputManager
Type definition for `VRC.SDKBase.InputManager`.

### VRCSDKBaseInputManager[]
Type definition for `VRC.SDKBase.InputManager[]`.

### VRCSDKBaseInputManagerRef
Type definition for `VRC.SDKBase.InputManager`.

### VRCSDKBaseIVRC Destructible
Type definition for `VRC.SDKBase.IVRC_Destructible`.

### VRCSDKBaseRPCDestination
Type definition for `VRC.SDKBase.RPC+Destination`.

### VRCSDKBaseVRC EventDispatcher
Type definition for `VRC.SDKBase.VRC_EventDispatcher`.

### VRCSDKBaseVRC EventHandler
Type definition for `VRC.SDKBase.VRC_EventHandler`.

### VRCSDKBaseVRC EventHandlerVrcBroadcastType
Type definition for `VRC.SDKBase.VRC_EventHandler+VrcBroadcastType`.

### VRCSDKBaseVRC Pickup
Type definition for `VRC.SDKBase.VRC_Pickup`.

### VRCSDKBaseVRC PickupPickupHand
Type definition for `VRC.SDKBase.VRC_Pickup+PickupHand`.

### VRCSDKBaseVRC SceneDescriptorSpawnOrientation
Type definition for `VRC.SDKBase.VRC_SceneDescriptor+SpawnOrientation`.

### VRCSDKBaseVRCInputMethod
Type definition for `VRC.SDKBase.VRCInputMethod`.

| Index | VRCInputMethod     |
| ----- | ------------------ |
| 0     | Keyboard           |
| 1     | Mouse              |
| 2     | Controller         |
| 3     | Gaze               |
| 5     | Vive               |
| 6     | Oculus             |
| 7     | ViveXR             |
| 10    | Index              |
| 11    | HPMotionController |
| 12    | Osc                |
| 13    | QuestHands         |
| 14    | Generic            |
| 15    | Touch              |
| 16    | OpenXRGeneric      |
| 17    | Pico               |
| 18    | SteamVR2           |

:::note Ambiguous Vive input names 

- `VRCInputMethod.Vive` is a Vive controller running through SteamVR.
- `VRCInputMethod.ViveXr` is a Vive XR Elite Controller running via OpenXR.

:::
 
### VRCSDKBaseVRCInputSetting
Type definition for `VRC.SDKBase.VRCInputSetting`.

### VRCSDKBaseVRCPlayerApi
Type definition for `VRC.SDKBase.VRCPlayerApi`.

### VRCSDKBaseVRCPlayerApi[]
Type definition for `VRC.SDKBase.VRCPlayerApi[]`.

### VRCSDKBaseVRCPlayerApiRef
Type definition for `VRC.SDKBase.VRCPlayerApi`.

### VRCSDKBaseVRCPlayerApiTrackingData
Type definition for `VRC.SDKBase.VRCPlayerApi+TrackingData`.

### VRCSDKBaseVRCPlayerApiTrackingData[]
Type definition for `VRC.SDKBase.VRCPlayerApi+TrackingData[]`.

### VRCSDKBaseVRCPlayerApiTrackingDataRef
Type definition for `VRC.SDKBase.VRCPlayerApi+TrackingData`.

### VRCSDKBaseVRCPlayerApiTrackingDataType
Type definition for `VRC.SDKBase.VRCPlayerApi+TrackingDataType`.

---

# End of Documentation

# runtime 統合ドキュメント

以下は自動収集された全ドキュメントのコンテンツです。各セクションの始めにメタデータが記載されています。



---

## ドキュメント: blacklist-manager.md

```metadata
階層: /worlds/clientsim/systems/runtime/blacklist-manager.md
ディレクトリ: worlds\clientsim\systems\runtime
ファイル名: blacklist-manager.md
拡張子: .md
サイズ: 289 B
最終更新: 2025-06-05T03:07:52.768Z
```

# Blacklist Manager
The BlacklistManager is an interface into Udon’s blacklist system. Its only job is to add an object’s hierarchy into the blacklist. This is used when spawning players through the [PlayerManager](player-manager.md) as well as spawning the ClientSim system itself.


---

## ドキュメント: event-dispatcher.md

```metadata
階層: /worlds/clientsim/systems/runtime/event-dispatcher.md
ディレクトリ: worlds\clientsim\systems\runtime
ファイル名: event-dispatcher.md
拡張子: .md
サイズ: 570 B
最終更新: 2025-06-05T03:07:52.769Z
```

# Event Dispatcher and Events

The EventDispatcher is responsible for notifying other systems when specific events happen within ClientSim. A behaviour can subscribe to an event type or send an event of specific type to all of ClientSim without knowing what will handle it. The EventDispatcher is just a collection of event types paired with event handlers. This method decouples the different systems in ClientSim without needing to create a direct dependency. All events sent must extend the IClientSimEvent interface, but can contain any data needed with the event.

---

## ドキュメント: helpers.md

```metadata
階層: /worlds/clientsim/systems/runtime/helpers.md
ディレクトリ: worlds\clientsim\systems\runtime
ファイル名: helpers.md
拡張子: .md
サイズ: 2.85 KB
最終更新: 2025-06-05T03:07:52.769Z
```

# VRCSDK Helpers

The Helper components are added to an object to help with handling the behavior of VRC SDK components. The role of these components remains the same compared to CyanEmu and Phase 2, although some logic not specific to the function of the object itself has been stripped out. As an example, in CyanEmu the CyanEmuPickupHelper script handled the logic for holding pickups. Now this behavior has been moved outside the pickup helper class, and into the pickup management system. The PickupHelper code now only provides data for how the PlayerHand should handle the pickup.

Helper classes may also extend interfaces that are used in ClientSim. There are two categories of interfaces: [Usables](#usable-interfaces) and [Handlers](#handler-interfaces). 

## Usable Interfaces

Usable interfaces normally end in “able”, and represent items that can be used somehow within ClientSim. They provide information on how they can be used, but do not include the methods to use them.

| Name                       | Description                                                      |
|----------------------------|------------------------------------------------------------------|
| IClientSimInteractable     | Represents an object that can be interacted with                 |
| IClientSimPickupable       | Represents an object that can be picked up, Extends Interactable |
| IClientSimStation          | Represents an object that the player can use to sit              |
| IClientSimSyncable         | Represents an object that can have an owner                      |
| IClientSimPositionSyncable | Represents an object that syncs its position, Extends Syncable   |

## Handler Interfaces

Using these two interface types, the Helper classes are ways of wrapping VRChat SDK component information to provide it to ClientSim.

| Name                       | Description                                                      |
|----------------------------|------------------------------------------------------------------|
| PositionSyncedHelperBase   | Helper for VRCObjectSync,  Extends PositionSyncedHelperBase. Syncable, PositionSyncable, RespawnHandler |
| ObjectSyncHelper| Helper for VRCObjectSync, Extends PositionSyncedHelperBase. Syncable, PositionSyncable, RespawnHandler |
| UdonHelper          | Helper for UdonBehaviour, Extends PositionSyncedHelperBase. Syncable, PositionSyncable, RespawnHandler, Interactable, PickupHandler, StationHandler, SyncableHandler |
| PickupHelper     | Helper for VRCPickup. Pickupable |
| StationHelper | Helper for VRCStation. Implements IClientSimStation |
| ObjectPoolHelper| Helper for VRCObjectPool. Syncable |
| CombatSystemHelper | Helper for Udon CombatSetup. Implements IVRC_Destructible. Helper component is added to the player object directly when initialized. |
| SpatialAudioHelper | Helper for VRCSpatialAudioSource |

---

## ドキュメント: highlight-manager.md

```metadata
階層: /worlds/clientsim/systems/runtime/highlight-manager.md
ディレクトリ: worlds\clientsim\systems\runtime
ファイル名: highlight-manager.md
拡張子: .md
サイズ: 1.39 KB
最終更新: 2025-06-05T03:07:52.770Z
```

# HighlightManager

The HighlightManager will take an object and display an outline highlight effect for that object. This system wraps VRChat’s HighlightFX class. HighlightFX only takes a single renderer, whereas the HighlightManager takes a GameObject. Matching how VRChat handles highlighting objects, all renderers on the object and on its children are used for highlighting. Renderers that are disabled, have a null mesh, or are part of a static batch are ignored. If an object has no valid renderers, then a Highlight Proxy is used based on the first collider on the object. The Highlight Proxy will copy the transform values of the original mesh and also apply the collider size and scale to make it appear that the collider is being highlighted. The HighlightManager is used to visualize the results from the [PlayerRaycaster](player.md#playerraycaster) system. There is no set limit to the number of objects that can be highlighted, but only 2 objects are expected to be highlighted at once through ClientSim, one object per player hand. The HighlightManager links to the VRCSDK API for InputManager.EnableObjectHighlight. This hook only takes in renderers though and does not go through the full steps of finding children objects and creating proxies. 

:::note
The HighlightManager currently shows only a preview for how the object will look on Quest. A style matching Windows systems is forthcoming.
:::

---

## ドキュメント: index.md

```metadata
階層: /worlds/clientsim/systems/runtime/index.md
ディレクトリ: worlds\clientsim\systems\runtime
ファイル名: index.md
拡張子: .md
サイズ: 96 B
最終更新: 2025-06-05T03:07:52.771Z
```

# Runtime Systems

These systems help simulate things that happen at Runtime in VRChat worlds.

---

## ドキュメント: input.md

```metadata
階層: /worlds/clientsim/systems/runtime/input.md
ディレクトリ: worlds\clientsim\systems\runtime
ファイル名: input.md
拡張子: .md
サイズ: 1.82 KB
最終更新: 2025-06-05T03:07:52.771Z
```

# Input

In ClientSim, all input calls are in one class to handle input and send events. The ClientSimInputManager uses the new Input System, allowing for event-driven input. It uses the PlayerInput component to gain access to the specific input events based on the Input Bindings displayed below. Since the new Unity Input System package is not included by default, and Unity requires a special setting to enable, all references to the Input System are wrapped in define conditions, which prevents errors when importing into new projects.

## Input Events

Similar to the [EventDispatcher](event-dispatcher.md), the InputManager also has its own Events that different systems can listen to directly. These events are separated from the EventDispatcher itself because all input events have similar parameters and also has input values that are not broadcasted through events but require the listening system to poll for updated axis values.

## Input Bindings

The Input System also allows for different bindings for various control schemes. See below for the included bindings: KeyboardMouse, Gamepad, and Experimental XR Controller bindings. Note that XR input bindings within the InputSystem are very limited in Unity 2019. The InputManager will need to be expanded to properly support various VR Controllers


## UdonInput

The UdonInput system is part of the InputManager Prefab, which subscribes to the proper events in the InputManager and also polls for updates on movement and look-based inputs. Due to the timing of when Unity sends input events, and when Udon should receive input events, all button-based input is queued and processed later in the frame at the same time as movement and look-based input. This queuing and processing allows input events to happen after Udon’s update method is called, similar to how it is in VRChat.

---

## ドキュメント: interactive-layer-provider.md

```metadata
階層: /worlds/clientsim/systems/runtime/interactive-layer-provider.md
ディレクトリ: worlds\clientsim\systems\runtime
ファイル名: interactive-layer-provider.md
拡張子: .md
サイズ: 441 B
最終更新: 2025-06-05T03:07:52.772Z
```

# InteractiveLayerProvider

The InteractiveLayerProvider simply listens to menu open state events and provides a layer mask for which layers are currently interactive. When the menu is open, only the UI and UIMenu layers are interactive. When the menu is closed, all other layers, excluding MirrorReflection, are interactive. InteractiveLayerProvider is used by [Raycasters](player.md#raycaster) and the [ClientSimInputModule](input.md).


---

## ドキュメント: main.md

```metadata
階層: /worlds/clientsim/systems/runtime/main.md
ディレクトリ: worlds\clientsim\systems\runtime
ファイル名: main.md
拡張子: .md
サイズ: 686 B
最終更新: 2025-06-05T03:07:52.772Z
```

# Client Sim Main

ClientSimMain is the central point of ClientSim that handles initialization and destruction of ClientSim. It is contained in the ClientSimSystem prefab. On creation, all core systems will be initialized with their dependencies. This system also maintains all the implementations of the VRCSDK hooks to link VRC components to the ClientSim Helpers. ClientSimMain is a singleton to ensure only one instance is running at a time and to help easily pass information from Editor Windows and Tests. None of the runtime systems within ClientSim depend on ClientSimMain being a singleton.

![ClientSimSystem Hierarchy](/img/worlds/clientsim/client-sim-main-hierarchy.png)

---

## ドキュメント: menu.md

```metadata
階層: /worlds/clientsim/systems/runtime/menu.md
ディレクトリ: worlds\clientsim\systems\runtime
ファイル名: menu.md
拡張子: .md
サイズ: 3.48 KB
最終更新: 2025-06-05T03:07:52.772Z
```

# Client Sim Menu

The ClientSimMenu has four pages that can be displayed depending on the situation. The menu is now positioned in world space instead of being an overlay to the camera. It will render on top of everything to ensure players can use the menu.

## Warning Page

![Warning](/img/worlds/clientsim/warning.png)

The Warning page will be displayed every time ClientSim starts. This page informs the user that ClientSim is not the same as VRChat, and there will be differences. It only has one button, which the user must press to dismiss the page and close the menu.

## Pause Page

![Warning](/img/worlds/clientsim/pause.png)

The Pause page is displayed whenever the user opens the menu after accepting the Warning page. This page has three sections: Actions, Player Info, and Settings.

## Actions

The actions section contains four buttons:
1. Close Menu - Close the menu and allow player movement
2. Respawn - Close the menu and teleport the player to the spawn point
3. Settings Window - Open the ClientSim [Settings Window](../editor/settings-window.md)
4. Exit Playmode - Exit Play mode and go back to Edit mode.

## Player Info

The Player Info section contains info regarding the local player.
1. Player Name
2. Player ID
3. Is the local player the master
4. Is the local player the instance owner
5. A button to spawn remote players for testing

## Settings

The Settings section provides options to change the current ClientSim Runtime Settings. Changing these will save the values even after Playmode ends.
* **Show Tooltips** - Toggle the display of interaction tooltips.
* **Desktop Reticle** - Toggle the desktop reticle in the center of the screen. This does not disable the UI pointer if hovering over a UI interactive object.
* **Invert Mouse Look** - Toggle whether the mouse Y should be inverted.
* **Console Logging** - Toggle whether debug information should be logged to the console.
* **Player Height** - A slider to set the player’s height. Default value is 1.9 Unity units tall. The slider is limited between 0.2 and 4 units. If the value in the ClientSim Unity Settings Window is set, then the height can be overridden up to 80 units. Toggling the menu will clamp the max value to keep the slider usable without exiting playmode. Note that PlayerHeight is different from TrackingScale, although the values are related.

## Delay Start Page

![Delay](/img/worlds/clientsim/delay.png)

The Delay Start page is displayed whenever the [ClientSim Settings](settings.md) has a non zero value for how long ClientSim should be delayed before starting. This page will dismiss automatically when ClientSim starts. If the player controller was spawned, the Warning page will then be displayed. The user cannot close out of this page and must wait the duration or exit playmode.

## Invalid Settings Page

![Invalid Settings](/img/worlds/clientsim/invalid-settings.png)

The Invalid Settings page is displayed when the user has Unity Project Settings that are not set up to use ClientSim. This can happen when ClientSim has just imported, or if the user modifies a setting that ClientSim relies on. When this is displayed, the [Settings Window](../editor/settings-window.md) is forced to be displayed so that users can click the buttons needed to modify project settings. In the case the player does not have the correct Unity Input settings, the buttons on the menu will do nothing. There is no way to get past this page except to exit playmode and correct the invalid settings.

---

## ドキュメント: player-manager.md

```metadata
階層: /worlds/clientsim/systems/runtime/player-manager.md
ディレクトリ: worlds\clientsim\systems\runtime
ファイル名: player-manager.md
拡張子: .md
サイズ: 501 B
最終更新: 2025-06-05T03:07:52.772Z
```

# Player Manager
The PlayerManager system is responsible for VRCPlayerApi data during ClientSim’s runtime. This system handles creating and destroying players, managing who is the current master, and sending the related [Events](event-dispatcher.md) such as OnPlayerJoin, OnPlayerLeft, and OnNewMaster. The PlayerManager is similar to how it was in CyanEmu, but this time is an instanced class rather than static. Most of the VRCPlayerApi SDK hooks are linked to static functions within this class.

---

## ドキュメント: player-spawner.md

```metadata
階層: /worlds/clientsim/systems/runtime/player-spawner.md
ディレクトリ: worlds\clientsim\systems\runtime
ファイル名: player-spawner.md
拡張子: .md
サイズ: 304 B
最終更新: 2025-06-05T03:07:52.772Z
```

# Player Spawner
The PlayerSpawner system is responsible for spawning and initializing local and remote players. Players are spawned from a prefab, set to the location of a spawn point provided by the [SceneManager](scene-manager.md), and then initialized through the [PlayerManager](player-manager.md).

---

## ドキュメント: player.md

```metadata
階層: /worlds/clientsim/systems/runtime/player.md
ディレクトリ: worlds\clientsim\systems\runtime
ファイル名: player.md
拡張子: .md
サイズ: 8.15 KB
最終更新: 2025-06-05T03:07:52.772Z
```

# Player

The ClientSim representation of a player has been split into many components compared to CyanEmu. Each component handles a different aspect of the player. Below you can see the hierarchy of both the Local and Remote player prefabs.
![Local Player Hierarchy](/img/worlds/clientsim/player-local-hierarchy.png)![Remote Player Hierarchy](/img/worlds/clientsim/player-remote-hierarchy.png)

## ClientSimPlayer

The ClientSimPlayer class is the container that holds all the systems for the player. While both local and remote players have a ClientSimPlayer, it is only initialized for the local player. All systems for the remote player are left blank, other than the [Avatar}(#avatar) and VRCPlayerApi changeable data. The ClientSimPlayer is the first component on the top level object on the Player prefabs. In the inspector, you can view and modify VRCPlayerApi data such as Locomotion Settings and Audio Settings at runtime.

## TrackingProvider

The TrackingProvider interface is a generic way to define how tracking data for the player should be controlled. The abstract TrackingProviderBase class provides data on the head, hands, and playspace position, and also determines the player’s current stance based on the head height. The tracking provider can be scaled to move the player camera up or down, helping test various player avatar scenarios. It will listen to PlayerHeightUpdate events and calculate a new tracking scale based on the requested player height. The default avatar height is 1.9, which represents TrackingScale 1. Since this class is abstract, it must be extended to be used in ClientSim. Currently only the DesktopTrackingProvider is included in ClientSim. Implementing VR in ClientSim would need a new VRTrackingProvider. The tracking provider is expected to be a child of the [PlayerController](#playercontroller), but will need to be reconsidered when implementing VR as playspace x/z offset is not applied to the player controller.

### DesktopTrackingProvider

Currently the only implemented TrackingProvider for ClientSim. This tracking provider locks the hand positions relative to the camera. Camera height is modified based on [Crouch and Prone Input Events](input.md). On every frame, this tracking provider checks for look input changes and updates the Camera’s x rotation (up/down). If the player is sitting in a station, then the head y rotation is unlocked to allow the player to look left and right, up to 90 degrees off the playspace rotation.

## PlayerController

The PlayerController now only controls the movement of the player. In CyanEmu, the PlayerController class contained everything related to the player. Now these systems have been split off, making the PlayerController focused on simply handling the player’s movement. Every frame, the controller will check for movement input as well as [TrackingProvider](#trackingprovider) stance to know how much the player should move that frame.

## PlayerStationManager

The PlayerStationManager manages how players interact with stations. It stores the current station the player is in, as well if the player is locked to the station. While locked to a station, at the end of the frame, for all Update, LateUpdate, and FixedUpdate methods, the [PlayerController’s](#playercontroller) position is updated to the station’s. This happens at the end of the frame to ensure that any other script modifying the station’s position happens first. 

## InteractManager

The InteractManager is responsible for determining if a given GameObject can be interacted with and performing the interaction. It calculates the current distance the player can interact with using the [TrackingProvider’s](#trackingprovider) current TrackingScale and each Interact’s proximity value.

## Raycaster

ClientSim interact detection is handled through the Raycasters. This system will search for Interactables based on a provided ray used in Physics.Raycast. The [InteractiveLayerProvider](interactive-layer-provider.md) is used to know what layers to consider when raycasting. All the hit objects are then filtered based on the components found. Objects with UIShape are always prioritized first. The InteractManager is used to determine the components on the object which can be interacted with. For each raycast, a RaycastResult is returned. This contains information about the ray, the object hit, and the type of interactable, if there is one.

### RayProvider
The Raycaster uses a RayProvider to know what direction and origin to raycast. RayProviders are a generic way to supply the ray without knowing exact detail. ClientSim implements two RayProviders:

#### CameraRayProvider
Given a camera and the current mouse position, create a ray that goes through the mouse from the camera. This is the RayProvider used when TrackingProvider is set to not VR.

#### TransformRayProvider
Given a transform, create a Ray based on the position of the transform and the forward direction. This is used to raycast from the hands when the TrackingProvider is set to VR.

## PlayerRaycaster

The PlayerRaycaster is responsible for searching the world for interacts and sending events based on what it finds. It will also update the [PlayerHand](#playerhand) system for both the left and right hands. When initialized, the PlayerRaycaster will create two [Raycasters](#raycaster), one for the left and another for the right hand. If the TrackingProvider is not VR (is desktop), then the right hand uses a Raycaster with a mouse based [RayProvider](#rayprovider). The left hand is left uninitialized as it will never be used when not in VR. If the [TrackingProvider](#trackingprovider) is in VR, then the left and right Raycasters are initialized with a transform-based ray provider using the TrackingProvider’s hand transforms. Every frame, both hand Raycaster systems are updated to search for Interactables and the results are sent through the [EventDispatcher](event-dispatcher.md). The last found interact is saved for each hand as the hovered interact. If a hovered interact is a Pickupable, that Pickupable is set as the hovered Pickupable for the given PlayerHand. If the Use Input action occurs for a given hand while hovering over an interact, then the hovered object will be interacted with.

## PlayerHand

The PlayerHand system is responsible for managing Pickupable objects. The [PlayerRaycaster](#playerraycaster) will set the current hovered pickup. Then the PlayerHand will listen for the Grab Input Events on when to pickup the hovered Pickupable. PlayerHand will also listen to Use and Drop events to perform actions on the currently held pickup. If the pickup is kinematic, then the position of the pickup will be directly set every frame to match the PlayerHand’s rigidbody transform. If the pickup is non kinematic, it will be attached to the PlayerHand’s rigidbody using a fixed joint. When holding a pickup in the right hand, the pickup can be manipulated using the different Manipulate bindings. 

## PlayerAvatarManager

The PlayerAvatarManager manages items related to the player’s avatar. Both local and remote players have an Avatar Manager. The avatar for all players is the VRChat Default Robot. The manager is responsible for providing avatar information to the VRCPlayerApi. GetBonePosition and GetBoneRotation are implemented here as wrappers over Animator.GetBoneTransform. The local player’s avatar manager is the only one that is properly initialized, which allows it to listen to [TrackingScale](#trackingprovider) change [Events](event-dispatcher.md), which then update the scale of the avatar to match the new tracking scale.

## Reticle

The ClientSimReticle system is responsible for displaying a reticle icon in the center of Unity Game Window. This system should only be available for [DesktopTrackingProvider](#desktoptrackingprovider). The reticle can be disabled through the settings menu. In addition to the center reticle, whenever the [PlayerRaycaster](#playerraycaster) hovers over an object with a UIShape, it will display a pointer over the mouse position. This pointer is not limited to the center of the screen and will still be displayed when the mouse is released. The location of the mouse is provided by [ClientSimBaseInput](input.md).


---

## ドキュメント: runtime-loader.md

```metadata
階層: /worlds/clientsim/systems/runtime/runtime-loader.md
ディレクトリ: worlds\clientsim\systems\runtime
ファイル名: runtime-loader.md
拡張子: .md
サイズ: 451 B
最終更新: 2025-06-05T03:07:52.772Z
```

# RuntimeLoader
The RuntimeLoader is a static class responsible for starting ClientSim on entering playmode. It uses the InitializeOnLoad Unity hook to check the settings instance to see if ClientSim should start, and creates an instance of ClientSimMain. This class also handles deleting editor-only objects in the scene. In order to allow for testability of ClientSim, a few methods are provided to set test settings and event dispatcher overrides.

---

## ドキュメント: scene-manager.md

```metadata
階層: /worlds/clientsim/systems/runtime/scene-manager.md
ディレクトリ: worlds\clientsim\systems\runtime
ファイル名: scene-manager.md
拡張子: .md
サイズ: 273 B
最終更新: 2025-06-05T03:07:52.773Z
```

# Scene Manager
The SceneManager system is mainly a wrapper for the VRC_SceneDescriptor. It provides an interface into VRC scene details such as getting a spawn point and respawn height. This system also handles copying over reference camera settings to the player camera.

---

## ドキュメント: settings.md

```metadata
階層: /worlds/clientsim/systems/runtime/settings.md
ディレクトリ: worlds\clientsim\systems\runtime
ファイル名: settings.md
拡張子: .md
サイズ: 1.51 KB
最終更新: 2025-06-05T03:07:52.774Z
```

# Settings

The ClientSim Settings are not a system, but data on how to run ClientSim.
### Enable ClientSim
Should ClientSim be enabled when entering playmode? ClientSim is forced disabled when uploading worlds.

### Enable Console Logging
Should Debug information be logged to the console?

### Remove “EditorOnly”
On enter playmode, should all objects tagged with “EditorOnly” be deleted?

### Set Target FrameRate
Should ClientSim set the Application target framerate?

### Target FrameRate
The expected framerate for Unity while in playmode. This will set both Application.TargetFramerate and the FixedTimeDelta.

### Startup Delay
How long should ClientSim wait before starting and initializing Udon? Use this as a way to simulate long world loading times and verify Unity component behavior.

### Spawn Player Controller
Spawn a controllable player when starting ClientSim. If disabled, a local player is still created to prevent Udon Programs crashing.

### Show Desktop Reticle
Should the desktop reticle be displayed or not?

### Show Tooltips
Show tooltips above interactable objects

### Invert Mouse Look
Should the mouse Y be inverted

### Player Height
The height of the player in unity units. This is clamped between 0.2 and 80.

### Local Player Name
What is the name of the local player, used for VRCPlayerApi.displayName

### Local Player Is Master
When set to false, a remote player is spawned and set as master.

### Is Instance Owner
Is the local player the instance owner?


---

## ドキュメント: synced-object-manager.md

```metadata
階層: /worlds/clientsim/systems/runtime/synced-object-manager.md
ディレクトリ: worlds\clientsim\systems\runtime
ファイル名: synced-object-manager.md
拡張子: .md
サイズ: 1008 B
最終更新: 2025-06-05T03:07:52.775Z
```

# SyncedObjectManager

The SyncedObjectManager keeps track of all initialized synced objects (IClientSimSyncable) in the scene. These synced objects are put into two lists: one list for all synced objects, and another for all position-synced objects. The SyncedObjectManager currently has only two main functions. The first is to check all position-synced objects to verify they are above the respawn height. If they fall below the respawn height, they are respawned to their start position or destroyed, depending on the settings in the [SceneManager](scene-manager.md). The second function is to ensure objects have the correct owners when a player leaves. The manager listens for the OnPlayerLeft [Event](event-dispatcher.md), goes through all objects to check if that player was the  owner, and then sets those objects to be owned by the master player instead. This ownership transfer happens before Udon Programs are notified of the player leaving.


VRC.SDK3.ClientSim.ClientSimSyncedObjectManager

---

## ドキュメント: tooltip-manager.md

```metadata
階層: /worlds/clientsim/systems/runtime/tooltip-manager.md
ディレクトリ: worlds\clientsim\systems\runtime
ファイル名: tooltip-manager.md
拡張子: .md
サイズ: 647 B
最終更新: 2025-06-05T03:07:52.775Z
```

# TooltipManager

The TooltipManager will display text in the world above a given interactable object. Tooltips in ClientSim only display text, unlike VRChat which also displays an icon of the respective button needed to use the interact. In SDK3, it appears that the option to set a tooltip location for an interactable is ignored. Tooltips always display at the top center of the first collider on the interactable object. There is no set limit to the number of tooltips that can be displayed, but only 2 tooltips are expected through ClientSim, one per player hand. Displaying tooltips can be disabled in the [ClientSimSettings](settings.md).

---

## ドキュメント: udon-manager.md

```metadata
階層: /worlds/clientsim/systems/runtime/udon-manager.md
ディレクトリ: worlds\clientsim\systems\runtime
ファイル名: udon-manager.md
拡張子: .md
サイズ: 693 B
最終更新: 2025-06-05T03:07:52.775Z
```

# UdonManager

The UdonManager keeps track of all initialized UdonBehaviours in the scene. Note that with the VRCSDK, an UdonBehaviour will not initialize if it does not have a program. This means that legacy position-synced UdonBehaviours without programs are not tracked, even with the SyncedObjectManager. The UdonManager has two main roles. The first is to notify all Udon Helpers when ClientSim has finished initializing, which allows UdonBehaviours to start. The second is to listen for certain ClientSim [Events](event-dispatcher.md) to forward to all UdonBehaviours. Currently the UdonManager only forwards the following events:
* OnPlayerJoined
* OnPlayerLeft
* OnPlayerRespawn


---

## ドキュメント: unity-event-system.md

```metadata
階層: /worlds/clientsim/systems/runtime/unity-event-system.md
ディレクトリ: worlds\clientsim\systems\runtime
ファイル名: unity-event-system.md
拡張子: .md
サイズ: 2.27 KB
最終更新: 2025-06-05T03:07:52.775Z
```

# Unity Event System

ClientSim uses two classes to translate actions into Unity’s EventSystem. These classes decouple Unity’s old input system into values based on ClientSim’s current bindings and match VRChat’s interactive UI object filtering. 

## BaseInput

The ClientSimBaseInput system extends Unity’s BaseInput class. Unity’s BaseInput is responsible for passing mouse position and button input into the EventSystem. The ClientSim BaseInput system overrides these methods to instead pass values based on the current ClientSim input bindings and last [PlayerRaycaster](player.md#raycaster) results. Mouse input is replaced with the current binding’s [Use Input](input.md). Since Use input is a handed action, only the value of the last activated hand is passed as mouse input. The mouse position sent to the Event System ignores the actual mouse position, and instead calculates the screen position of the last interact raycast. Using the raycast position abstracts out the real mouse’s position, allowing Desktop and VR to use Unity UI through the same system.
The BaseInput system is also responsible for providing the current mouse position to the rest of ClientSim. It controls if the mouse pointer is hidden and locked to the center of the screen, or visible and free to move. This mouse position is used for displaying the [Desktop Reticle](player.md#reticle) as well as using the mouse to create the ray direction for [DesktopRayProvider](player.md#rayprovider).

## InputModule

The ClientSimInputModule extends Unity’s StandaloneInputModule. This system processes Unity mouse events and filters out any UI objects that are not currently interactable. UI objects are interactable when all of the following conditions have been met:

1. The [PlayerRaycaster](player.md#playerraycaster) last hit an object with a VRC_UIShape component. This data is provided through ClientSimBaseInput.
2. The UI object has a UIShape component in its parent
3. The layer of the parent UIShape object is on a currently interactive layer. Interactive layers are determined by the [InteractiveLayerProvider](interactive-layer-provider.md).
4. The hit point of the UI Object raycast is contained within the collider of the UIShape. If any of those conditions fail, then the UI cannot be interacted with.

---

# ドキュメント終了

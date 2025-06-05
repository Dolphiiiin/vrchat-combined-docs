# persistence Documentation

This document contains automatically collected documentation with metadata headers for each section.



---

## Document: index.md

Path: /worlds/udon/persistence/index.md
# Persistence

## What is Persistence?

Persistence allows worlds to save high scores, inventories, the player's last position, currency, unlocks, preferences, and much more. When a player leaves a world and returns later, Udon can access their saved data.

This data is stored on VRChat's servers and is accessible on all platforms and in all instances of the same world. It is connected to the user's account, so if a player visits a world on a completely different device, Udon will still have access to that world's data.

One of the primary ways to use persistence is with [PlayerData](/worlds/udon/persistence/player-data). PlayerData is a key-value storage system associated with each player. Any script can access the data, and it's capable of storing all integer types, floats, doubles, vectors, colors, strings, and byte arrays.

Persistence also includes [PlayerObjects](/worlds/udon/persistence/player-object). Any game object with this component is automatically instantiated for each player. Udon scripts can be attached to PlayerObjects, and if a `VRCEnablePersistence` component is added to them then all synced data on those objects will persist. PlayerObjects also have many uses outside of persistence, serving a similar use-case as per-player object pools. They can be used for anything that should be tied to each player in the instance, such as health bars, colliders for combat systems, and robust networked systems without ownership transfers.

## Examples of using Persistence

You can find all our [Persistence Examples here](/worlds/examples/persistence).

## Persistent User Data

Data saved by Persistence is called "User Data." Persistence has two types of User Data that Udon can save and load from VRChat's servers:

- [PlayerData](/worlds/udon/persistence/player-data) is a key-value database that allows Udon scripts to save and load User Data.
- [PlayerObject](/worlds/udon/persistence/player-object) are GameObjects that automatically instantiate themselves for each player. All their synced Udon variables can be persistent, if marked with a `VRCEnablePersistence` component.

## Data storage in different environments

User Data is stored in different locations if you're testing your world during development: 

- **Uploaded World**:
	- Persistent data is stored on VRChat's servers connected to your account. You can visit your world on any device and the data will be available.
	- If you open VRChat multiple times on the same account in the same world at the same time, you may cause conflicts and accidently overwrite your own data.
- **[Local Test World](/worlds/udon/graph/#running-build--test)**
	- User Data is stored locally if you "Build & Test."
		- When you launch a test client, it starts with no User Data.
		- When you rejoin or "Build & Reload," User Data is **not** reset.
		- When you close a test client, it deletes its User Data.
	- If you set "Number of Clients" to two or higher, User Data is stored separately for each test client.
- **ClientSim**
	- Persistent data is stored in JSON files within your project
	- See [ClientSim Persistence documentation](/worlds/clientsim/) for more details.

## Limitations

Please consider the following limitations when using Persistence in your world:

- Each world may store 100 kilobytes(KB) of PlayerData and 100 KB of PlayerObject data per player on VRChat's servers.
	- VRChat stores User Data in a compressed format. If your world's data is easy to compress, you may be able to store more than 300 KB (compressed into 100 KB by VRChat).
	- You cannot save data that would cause you to exceed this limit. Instead, VRChat logs an error and your data won't be saved. Reduce the size of your data to avoid this.
- The local player's User Data must be saved before they leave the world. Persistent data cannot be saved in the local player's [OnPlayerLeft](https://creators.vrchat.com/worlds/udon/graph/event-nodes/#onplayerleft) event. 
- Persistent data cannot be shared between different worlds.
- Persistence does not have a feature for "save slots" built-in. However, world creators can use Udon to build a save slot system inside their worlds.

## Making Persistent Prefabs

When building prefabs that have persistent behavior, it's important to consider how they will interact with other prefabs in creators' projects.

First, consider using PlayerObjects rather than PlayerData, which are a better fit for _most_ prefabs. PlayerObjects are nicely contained within a Prefab's hierarchy and are a better fit for persist large amounts of data and/or data that changes frequently. Remember that 
when you change **any** PlayerData for the local player, **all** of their PlayerData is sent, including data that hasn't changed. Every prefab in a world which uses PlayerData will add to this collection and require sending each time, which could quickly expand the networking usage in a world and could lead to bandwidth issues.

If PlayerData is _still_ a better place for some of your prefab's data, consider adding a prefix to all the keys your prefab uses. For example, Momo's Persistent Post-Processing settings could pick the prefix "Momo-PPP-" and then use the keys:
- Momo-PPP-BloomAmount
- Momo-PPP-Vignette
- Momo-PPP-Weight

This approach will greatly reduce the likelihood of the name colliding with another Prefab. The "Weight" key above, for example, could easily run into an issue if you had two different prefabs with a "Weight" parameter, but it's not very likely that another prefab will use "Momo-PPP" as their prefix.

## Reset User Data

:::danger

This is irreversible! You cannot restore your user data after deleting it.

:::

You can delete your own persistent user data. This allows you (or your players) to "start over" or to resolve technical issues.

There are multiple ways to reset user data, both in VRChat and on our website.
### In VRChat

Resetting your user data in VRChat is the easiest option.

#### A specific world

You can delete your User Data for any world by following the steps below:

1. Select a world in VRChat's main menu.
2. Scroll down to the "Actions" section.
3. Select "Reset User Data."
    - This button only appears if you have User Data for this world.
5. Select "YES, RESET."

If you're currently in a persistent world, you may need to leave or rejoin the world before you can reset user data.

#### All worlds

You can also delete your User Data for all worlds that you have visited by following the steps below:

1. Open your settings in VRChat's main menu.
2. Select the "Debug" section.
3. Scroll down to the "User Data" section.
4. Select "Reset All User Data"
5. Select "YES, RESET."

### On the VRChat Website

You can reset your User Data on the VRChat website. Please ensure that you have a VRChat account. If you only have a Meta, Pico, or Viveport account, you must [link it to a VRChat account](https://help.vrchat.com/hc/en-us/articles/360062659053-I-want-to-turn-my-platform-account-through-Steam-Meta-Pico-or-Viveport-into-a-VRChat-account) first.

#### A specific world

You can reset your own User Data for a specific world by following the steps below:

1. Open [a world on VRChat.com.](https://vrchat.com/home/world/wrld_4432ea9b-729c-46e3-8eaf-846aa0a37fdd)
2. Scroll down to the "User Data" section
    - This section will not exist if you do not have User Data for that world.
4. Click **Reset User Data**.
5. After reading the warning, click **Yes** to confirm.

#### All worlds

You can reset your own User Data for all worlds you visited by going to the VRChat website:

1. Open the [settings page on VRChat.com.](https://vrchat.com/home/profile).
2. Scroll down to the **Persistent Data** section.
3. Click **Reset All User Data**.
4. After reading the warning, click **Yes** to confirm.


---

## Document: player-data.md

Path: /worlds/udon/persistence/player-data.md
# PlayerData


import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

PlayerData is a [key-value database](https://en.wikipedia.org/wiki/Key%E2%80%93value_database) for storing [persistent data](/worlds/udon/persistence/) about players, such as their score in a game or their preferences in a world.

## Setup

In order for an UdonBehaviour to use PlayerData, you simply need to use the various PlayerData functions described below. Any Udonbehaviour can access those functions anywhere, at almost any time.

## Best Practices

* When using `OnPlayerDataUpdated`, consider whether your script can be limited to trigger on changes for the local player, or if it needs to trigger for every remote player, as well. 
    * For example - a user setting for video player volume can just update for the local player, but a co-op dog-petting game that tallies a "total dogs pet" score for the instance can react to the increase of any player's score to increment the global score.
* Wait for the `OnPlayerRestored` event before using Player Data. `OnPlayerRestored` indicates that the player's saved data has been loaded and is safe to access with Udon. This event will run even if you have no save data.
* Iterating through all Player Keys can be slow if you have many of them. As a general guideline, use the `TryGet` methods to directly check the values of specific keys if you're only checking a few keys or you have more than ten keys to check through.

## Networking

PlayerData does not rely on an UdonBehaviour's [synchronization](/worlds/udon/networking/) settings because it is not tied to any one specific UdonBehaviour. Setting a script to "None" instead of "Manual" or "Continuous" will not stop that script from being able to access and modify PlayerData, nor will it negatively affect the operation of PlayerData as a whole.

When an UdonBehaviour sets a value in PlayerData, PlayerData automatically synchronizes that value. Internally, PlayerData sends the value in a similar manner as manual synchronization with the `RequestSerialization` event. This means that an UdonBehaviour can set multiple PlayerData keys simultaneously and send them together. The UdonBehaviour won't need to send each key separately. Similarly, if an UdonBehaviour sets a key to one value and then immediately changes to something else, remote users don't receive the intermediate value. If data is changed too quickly, only the final state is received by remote users.

PlayerData has a similar [bandwidth cost](/worlds/udon/networking/network-details) as one UdonBehaviour with [synchronization](/worlds/udon/networking/) set to "Manual."

All UdonBehaviours in your world share access to your world's PlayerData. When you set a PlayerData key, any UdonBehaviour in your world can access it. PlayerData is not 'separated' between different UdonBehaviours.

When you change **any** data for the local player, **all** of their PlayerData is sent, including data that hasn't changed. You are unlikely to run into [Udon's bandwidth limits](/worlds/udon/networking/network-details#bandwidth-limits) if you have a small amount of data that updates very frequently or a large amount of data that updates slowly. But if you use PlayerData to synchronize **large amounts** of data and also **high-frequency** data, you should consider moving one of those to a [player object](/worlds/udon/persistence/player-object). You can use player objects to sync persistent variables, but each object is synced separately. This allows you to separate your fast data from your big data, reducing your world's networking bandwidth.

## Events

| Event | Output | Notes |
| -------- | -------- | -------- |
| OnPlayerDataUpdated | VRCPlayerApi player, PlayerData.Info[] infos | Triggered at the end of the frame if the PlayerData of any player has changed or been received.<br/>Provides the VRCPlayerApi of the player associated with that data, along with an array of information on all the keys in that data. The information in the array includes both the keys used for the data and the state of that data, such as whether it was changed, added, or unchanged. |
| [OnPlayerRestored](/worlds/udon/graph/event-nodes#onplayerrestored) | VRCPlayerApi player | Triggered after a VRChat player's persistent data has been loaded.

:::info Watch out for timing issues

Wait for the `OnPlayerRestored` event before you read or write PlayerData.

When a player joins, the [OnPlayerJoined](/worlds/udon/graph/event-nodes#onplayerjoined) event is called. However, it may take some additional time before their PlayerData is received. If you set PlayerData too early, it may be overwritten when the persistent data is received. 

:::

## PlayerData Info

The `OnPlayerDataUpdated` event provides a `PlayerData.Info` array. This array contains all the current PlayerData keys associated with that player. Each element contains the following information:

| Property | Type | Notes |
| -------- | -------- | -------- |
| Key | String | The string associated with the PlayerData key, which can be used in queries and mutators. |
| State | Enum | The most recent state of the PlayerData Key at the point in which this OnPlayerDataUpdated has happened. |

The `State` enum describes these possible states:

| State     | Index | Notes                                                                                                                                                                                                                                       |
| --------- | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Unchanged | 0     | Indicates that the data in this key has not changed since the last update.                                                                                                                                                                  |
| Added     | 1     | Indicates that this key has been added since the last update.                                                                                                                                                                               |
| Removed   | 2     | Indicates that this key has been removed since the last update. Keys which have been removed will only appear in this array once with this state, and the next time they will be gone.*<br/>Note: Removing keys is not currently possible.* |
| Changed   | 3     | Indicates that the data in this key has been changed since the last update.                                                                                                                                                                 |
| Restored  | 4     | Indicates that this key has been restored from persistent records. This only happens when you join the instance after having been there before.                                                                                             |

## Best Practices

* When using `OnPlayerDataUpdated`, consider whether your script can be limited to trigger on changes for the local player, or if it needs to trigger for every remote player, as well. 
    * For example - a user setting for video player volume can just update for the local player, but a co-op dog-petting game that tallies a "total dogs pet" score for the instance can react to the increase of any player's score to increment the global score.
* Wait for the `OnPlayerDataRestored` event before using PlayerData. `OnPlayerRestored` indicates that the player's saved data has been loaded and is safe to access with Udon.
* Iterating through all Player Keys can be slow if you have many of them. As a general guideline, use the `TryGet` methods to directly check the values of specific keys if you're only checking a few keys or you have more than ten keys to check through.

## Methods

### Queries

Use these methods to retrieve more information about the value associated with a key. They are helpful to gather more information about what exists at a key before attempting to do something with it.

| Function | Input | Output | Notes |
| -------- | -------- | -------- | ------- |
| HasKey | VRCPlayerApi player, string key     | bool value     | Returns true if a value exists in PlayerData at that key |
| GetType | VRCPlayerApi player, string key | Type | Gets the type of the value contained in PlayerData at that key |
| TryGetType | VRCPlayerApi player, string key | out Type t, bool success | Gets the type of the value contained in PlayerData at that key. Returns false if that key does not exist |

### Mutators

Use these methods to save PlayerData for the local player. It is not possible to set a remote player's data.

Values can be overwritten, even if they previously had a different type. **Keys cannot be deleted after being written.**

| Function      | Input                        |
| ------------- | ---------------------------- |
| SetString     | string key, string value     |
| SetBool       | string key, bool value       |
| SetSByte      | string key, sbyte value      |
| SetByte       | string key, byte value       |
| SetBytes      | string key, byte[] value     |
| SetShort      | string key, short value      |
| SetUShort     | string key, ushort value     |
| SetInt        | string key, int value        |
| SetUInt       | string key, uint value       |
| SetLong       | string key, long value       |
| SetULong      | string key, ulong value      |
| SetFloat      | string key, float value      |
| SetDouble     | string key, double value     |
| SetQuaternion | string key, Quaternion value |
| SetVector4    | string key, Vector4 value    |
| SetVector3    | string key, Vector3 value    |
| SetVector2    | string key, Vector2 value    |
| SetColor      | string key, Color32 value    |
| SetColor32    | string key, Color32 value    |

### Accessors

Use these methods to get PlayerData for any player in the instance.

If a key does not exist, the default value for that type is returned. For example, calling `PlayerData.GetInt()` would return `0`.

Default values are also returned when using the wrong accessor type, such as using `GetInt` on a key which contains a `string`.

If default values are undesirable, use `TryGet` or Queries to distinguish default values from missing keys. 

| Function | Input | Output |
| -------- | -------- | -------- |
| GetString | VRCPlayerApi player, string key | string value |  
| TryGetString | VRCPlayerApi player, string key | string value, bool success | 
| GetBool | VRCPlayerApi player, string key | bool value | 
| TryGetBool | VRCPlayerApi player, string key | bool value, bool success | 
| GetSByte | VRCPlayerApi player, string key | sbyte value | 
| TryGetSByte | VRCPlayerApi player, string key | sbyte value, bool success | 
| GetByte | VRCPlayerApi player, string key | byte value | 
| TryGetByte | VRCPlayerApi player, string key | byte value, bool success | 
| GetBytes | VRCPlayerApi player, string key | byte[] value | 
| TryGetBytes | VRCPlayerApi player, string key | byte[] value, bool value | 
| GetShort | VRCPlayerApi player, string key | short value | 
| TryGetShort | VRCPlayerApi player, string key | bool value | 
| GetUShort | VRCPlayerApi player, string key | ushort value | 
| TryGetUShort | VRCPlayerApi player, string key | ushort value, bool success | 
| GetInt | VRCPlayerApi player, string key | int value | 
| TryGetInt | VRCPlayerApi player, string key | int value, bool success | 
| GetUInt | VRCPlayerApi player, string key | uint value | 
| TryGetUInt | VRCPlayerApi player, string key | uint value, bool success | 
| GetLong | VRCPlayerApi player, string key | long | 
| TryGetLong | VRCPlayerApi player, string key | long value, bool success | 
| GetULong | VRCPlayerApi player, string key | ulong | 
| TryGetULong | VRCPlayerApi player, string key | ulong value, bool success | 
| GetFloat | VRCPlayerApi player, string key | float | 
| TryGetFloat | VRCPlayerApi player, string key | float value, bool success | 
| GetDouble | VRCPlayerApi player, string key | double | 
| TryGetDouble | VRCPlayerApi player, string key |  double value, bool success | 
| GetQuaternion | VRCPlayerApi player, string key | Quaternion | 
| TryGetQuaternion | VRCPlayerApi player, string key |  Quaternion value, bool success | 
| GetVector4 | VRCPlayerApi player, string key | Vector4 | 
| TryGetVector4 | VRCPlayerApi player, string key | Vector4 value, bool success | 
| GetVector3 | VRCPlayerApi player, string key | Vector3 | 
| TryGetVector3 | VRCPlayerApi player, string key | Vector3 value, bool success | 
| GetVector2 | VRCPlayerApi player, string key | Vector2 | 
| TryGetVector2 | VRCPlayerApi player, string key | Vector2 value, bool success | 
| GetColor | VRCPlayerApi player, string key | Color | 
| TryGetColor | VRCPlayerApi player, string key | Color value, bool success | 
| GetColor32 | VRCPlayerApi player, string key | Color32 | 
| TryGetColor32 | VRCPlayerApi player, string key | Color32 value, bool success | 

## Examples

### Persistent Jumps Counter

<Tabs>
<TabItem value="graph" label="Udon Graph">


[IMAGE: The persistent jump counter example script in the Udon Graph.]


</TabItem>
<TabItem value="cs" label="UdonSharp">

```cs
using TMPro;
using UdonSharp;
using VRC.SDK3.Persistence;
using VRC.SDKBase;
using VRC.Udon.Common;

public class JumpCounter : UdonSharpBehaviour
{
    public TextMeshProUGUI jumpText;
    private const string JumpsKey = "jumps";
    public override void InputJump(bool value, UdonInputEventArgs args)
    {
        if (value)
        {
            AddJump();
        }
    }

    public override void OnPlayerDataUpdated(VRCPlayerApi player, PlayerData.Info[] infos)
    {
        if (player.isLocal)
        {
            UpdateTextComponent();
        }
    }

    private void AddJump()
    {
        var currentJumps = PlayerData.GetInt(Networking.LocalPlayer, JumpsKey);
        PlayerData.SetInt(JumpsKey, currentJumps + 1);
    }
    
    private void UpdateTextComponent()  
    {  
        jumpText.text = $"Jumps: {PlayerData.GetInt(Networking.LocalPlayer, JumpsKey)}";    
    }
}
```

</TabItem>
</Tabs>


---

## Document: player-object.md

Path: /worlds/udon/persistence/player-object.md
# PlayerObject

PlayerObjects allow you to automatically give each player who joins your world a copy of a GameObject, such as a flashlight, a health bar, or a sword.

- VRChat automatically spawns a copy of the GameObject for each player when they join your world.
- The copy includes all components and child GameObjects.
- The copy spawns with the same position, rotation, scale and parent as the original [PlayerObject Template](#playerobject-templates).


## Setup

Follow the instructions below to create a simple PlayerObject. When a player joins an instance, VRChat will spawn the PlayerObject for them.

1. Create a GameObject in your scene's hierarchy.
2. Add the `VRCPlayerObject` component 
	- This turns your GameObject into a PlayerObject [template](#playerobject-templates).
3. (Optional) Add an `UdonBehaviour` component to the GameObject (or one of its children).
4. (Optional) Add the `VRCEnablePersistence` to the same GameObject as the `UdonBehaviour` if you want synced variables to persist.

## Loading Persistent User Data

VRChat automatically loads Persistent User Data on PlayerObject that fulfill these conditions:

- The PlayerObject (or a child GameObject) has an `UdonBehaviour` component.
- The `UdonBehaviour` has synced variables.
- The `UdonBehaviour`'s GameObject also has a `VRCEnablePersistence` component.

Use the [`OnPlayerRestored`](/worlds/udon/graph/event-nodes#onplayerrestored) to detect that a PlayerObject has finished loading User Data. This event is executed once for every player in the instance, including the owner of the PlayerObject.

OnPlayerRestored includes a reference to the player whose data was just restored. You can use this to make the Owner of the GameObject initialize variables or execute events.

In the `Start` and `OnDeserialization` events, ownership is guaranteed to be correct. However, you should avoid reading or writing a PlayerObject's User Data before the `OnPlayerRestored` event. You might accidently read or write outdated User Data, and your changes may be overwritten when User Data is received from VRChat's server.

## Ownership

When players own a PlayerObject, they also own all GameObjects with synced `UdonBehaviours` within their PlayerObject. They cannot transfer them to anyone else.

This is convenient for giving players tools or pickups that can never be "stolen" by other players. You can guarantee that they will always own their own PlayerObjects. 

## PlayerObject Templates

PlayerObject templates are the original GameObjects that you create in the editor by adding a `VRCPlayerObject`. VRChat copies your Template to spawn PlayerObjects.

Components on Templates and PlayerObjects can contain references to other components: 
- Templates can reference their own components or child components.
- Templates can reference other components in your scene.
- Components in your scene can **not** reference Templates.
	- Instead, your scene objects must use direct references to spawned PlayerObjects.
	- Templates are automatically disabled by VRChat, but still exist in the scene.
	- Do not modify, destroy, or edit your Template after it has been disabled! This may cause errors or unexpected behaviour.
  
Use the following methods in your scripts to find references to PlayerObjects and their components:
- Use `Networking.GetPlayerObjects` to get all the PlayerObjects associated with a player, then sort through that array and find the specific thing you are looking for.
- Use `Networking.FindComponentInPlayerObjects` to translate a reference from a PlayerObject template into a specific player's PlayerObject.
- See the [examples](#examples) below.

## Events

There is only one event related to PlayerObjects:

| Event | Output | Notes |
| -------- | -------- | -------- |
| [OnPlayerRestored](/worlds/udon/graph/event-nodes#onplayerrestored) | VRCPlayerApi player | Triggered after all of a VRChat player's persistent data has been loaded. |

## Methods

PlayerObject methods are in the `VRC.SDKBase.Networking` namespace.

| Function | Input | Output | Notes |
| -------- | -------- | -------- | ------- |
| GetPlayerObjects | VRCPlayerApi target | GameObject[] objects | Returns an array of all the PlayerObjects associated with the player provided |
| FindComponentInPlayerObjects | VRCPlayerApi player, Component referenceComponent | Component | Using the `referenceComponent` which must be a child of a PlayerObject template, this function returns the corresponding component associated with the provided player. |

Utility wrappers to these methods are found on the `VRCPlayerApi` class.

| Function | Input | Output | Notes |
| -------- | -------- | -------- | ------- |
| GetPlayerObjects | | GameObject[] objects | Returns an array of all the PlayerObjects associated with the provided player. |

## Examples

### Finding a custom script on a PlayerObject

This example automatically looks through all PlayerObjects and finds a specific component that you're looking for. Replace `CustomPlayerObjectScript` with the type of component that you want to find.

```cs
public CustomPlayerObjectScript Find(VRCPlayerApi player)
{
    var objects = Networking.GetPlayerObjects(player);
    for (int i = 0; i < objects.Length; i++)
    {
        if (!Utilities.IsValid(objects[i])) continue;
        CustomPlayerObjectScript foundScript = objects[i].GetComponentInChildren<CustomPlayerObjectScript>();
        if (Utilities.IsValid(foundScript)) return foundScript;
    }
    return null;
}
```

### Finding a component on a PlayerObject

This example will use the `FindComponentInPlayerObjects` function to translate a reference from the PlayerObject template into a reference from a specific player's PlayerObject. 

```cs
public Transform referenceChildTransform; // A reference to a Transform on a PlayerObject or a child of a PlayerObject

public void Find(VRCPlayerApi targetPlayer)
{
    Component foundComponent = Networking.FindComponentInPlayerObjects(targetPlayer, referenceChildTransform);
    // foundComponent will be of type Transform and will correspond to the targetPlayer's PlayerObject that matches the reference.
}
```


---

# End of Documentation

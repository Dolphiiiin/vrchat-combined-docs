# clientsim Documentation

This document contains automatically collected documentation with metadata headers for each section.



---

## Document: index.md

Path: /worlds/clientsim/index.md
---
sidebar_position: 0
---

# ClientSim

[IMAGE: ClientSim screenshot. The Scene view shows the local player and a remote player. The Game view shows ClientSim's "Pause" menu, with buttons to respawn the player, spawn remote players, close the menu, and various other options. The ClientSim Settings window and PlayerData window are also visible.]

ClientSim, short for client simulator, is a part of the Worlds SDK that replicates VRChat client behavior in the Unity editor and can be used to speed up development.

## Features

- Test your World in Unity without needing to open VRChat.
- Control the player with your Mouse and Keyboard or a Gamepad.
- Inspect Udon variables in Play Mode.
- Grab Pickups, use Interacts, UI, and Stations.
- Delete EditorOnly objects when entering Play Mode.

Always test your world in VRChat before making it public! ClientSim can't simulate all of VRChat's features. 

## Windows

ClientSim adds several Unity Editor windows to help you debug and test your world: 

- The ClientSim Settings allow you to change the local player's name and the player's height, disable ClientSim, and more.
- While in Play Mode, open an in-game settings window by pressing the "Escape" key.
- The [ClientSim PlayerData](playerdata-editor-window) window allows you to debug PlayerData.
- The [ClientSim PlayerObject](playerObject-editor) window allows you to debug PlayerObjects.

## Getting started

1. Open your VRChat world scene.
2. Press play in Unity.
3. Test your world in Unity's "Game" window.

Learn more about how all the systems work in the [Systems](systems) section

## Networking Differences in ClientSim

ClientSim only simulates the local VRChat player, not remote players. [Deserialization events](/worlds/udon/networking/network-components/#ondeserialization) are simulated for the local player, but not for spawned remote players.

Some parts of the VRChat server are simulated. For example, the local player receives a `OnPlayerRestored` events when you spawn a remote player.

ClientSim has a different implementation of VRChat's networking serializer. Due to this, there are differences in amount of data stored & serialized in event properties. Data in events like `OnPostSerialization` and `OnDeserialization` differs between ClientSim and the VRChat (see below).

### [OnPostSerialization(SerializationResult)](/worlds/udon/networking/network-components/#onpostserialization)

ClientSim only executes this event if it's part of a synced Player Object.

|                               | In ClientSim                                  | In VRChat                            |     |
| ----------------------------- | --------------------------------------------- | ------------------------------------ | --- |
| SerializationResult.success   | Always true                                   | True if serialization was successful |     |
| SerializationResult.byteCount | Number of properties serialized on the object | Number of bytes sent                 |     |


### [OnDeserialization(DeserializationResult)](/worlds/udon/networking/network-components/#ondeserializationdeserializationresult)

ClientSim only executes this event if it's part of a synced Player Object.

|                                     | In ClientSim    | In VRChat                                              |
| ----------------------------------- | --------------- | ------------------------------------------------------ |
| DeserializationResult.sendTime      | Always 0        | The time in seconds at which this message was sent.    |
| DeserializationResult.receiveTime   | Always 0        | The time in seconds at which this message was received |
| DeserializationResult.isFromStorage | Works as normal | True if sent from persistent storage                   |

---

## Document: playerdata-editor-window.md

Path: /worlds/clientsim/playerdata-editor-window.md
# PlayerData Editor

ClientSim can simulate the VRChat SDK's "Persistence" feature. The PlayerData editor window allows you to debug persistent data in your world. You can open it by going to "VRChat SDK" > "ClientSim PlayerData".

## Usage

[IMAGE: ClientSim PlayerData Window]

1. A dropdown for selecting which player's PlayerData to display.
	* By default, the local player is selected.
	 * If a remote player has been spawned in Play Mode, they can be selected as well.
2. The list of selected player's PlayerData keys, values, and data types.
	* The display updates in real-time as PlayerData is updated.
3. "Clear All PlayerData" deletes all PlayerData for the current scene.
	* For example, this allows you to "start over" if you change how your world uses PlayerData.
4. "Refresh" allows you to refresh the PlayerData list.
	* You can manually view or edit the scene's JSON file. The changes apply immediately while in Play Mode, but you must click the "Refresh" button to see the changes.
5. "Open Local Data Folder" opens the folder where ClientSim stores JSON files for this scene's persistent data.
	* If your scripts use PlayerData, ClientSim automatically creates the JSON files. They're saved in your project and persist between Play Mode sessions.
	* PlayerData files are unique to scenes. Each scene using PlayerData will have a JSON file associated with it. The scene name is used for this association: If you rename a scene and want to keep your data, you must rename the corresponding JSON file to match the new scene name. 
7. "Add Test Data For Remote Player" allows you to create PlayerData for remote players.
	* This button is only displayed while a remote player is selected in Play Mode and the local player has data.
	* Since PlayerData can only be set for the local player, this button duplicates the local player's test data JSON file and randomizes each value. The assumption here is that the structure of PlayerData should be consistent between players, but values can be different.
	* Creating test data like so posts the corresponding OnPlayerDataUpdated events and allows creators to verify that their world handles data updates between multiple players correctly.
	* The remote player's data is randomized every time you click the button.

The data for PlayerData can be found in the root of your project (next to the Assets folder, not inside of it) in `<root>/ClientSimStorage/PlayerData`.

---

## Document: playerObject-editor.md

Path: /worlds/clientsim/playerObject-editor.md
# PlayerObject Editor

ClientSim can simulate how PlayerObjects work. When entering play mode, PlayerObjects will be spawned in, and their synced properties restored.

## Usage

[IMAGE: ClientSim PlayerObject Inspector]

When you enter play mode, ClientSim spawns PlayerObjects and restores their persistent properties. Select any PlayerObject in your [Hierarchy window](https://docs.unity3d.com/Manual/Hierarchy.html) to see its `ClientSimNetworkingView` and `ClientSimNetworkIdHolder` components. 

The `ClientSimNetworkIdHolder` component shows the names and values of a component's synced properties.

1. **Network Id**: The network ID of the game object.
    - This property is filled in just before Unity loads the scene. The scene will not run if it has network ID issues.
2. **Network Components**: A list of saved components on this game object.
    - This property contains all the persisted data of this object.

The data used by ClientSim for PlayerObjects is stored in your project at `projectName/ClientSimStorage/PlayerObjects/PlayerObjects_#_SceneName.json`. You can find this file using your standard file browser.


---

# End of Documentation

# persistence 統合ドキュメント

以下は自動収集された全ドキュメントのコンテンツです。各セクションの始めにメタデータが記載されています。



---

## ドキュメント: health-bar.md

```metadata
階層: /worlds/examples/persistence/health-bar.md
ディレクトリ: worlds\examples\persistence
ファイル名: health-bar.md
拡張子: .md
サイズ: 1.85 KB
最終更新: 2025-06-05T03:07:52.792Z
```

---
description: "Save player health amounts in a PlayerObject."
sidebar_custom_props:
    customIcon: 💗
---
import HowToImportExample from '/docs/worlds/examples/persistence/_how-to-import.mdx';

# Health Bar

![Health Bar World Preview](/img/worlds/examples/persistence/health-bar.jpg)

A simple health bar which uses a PlayerObject to sync and persist players' health amounts.

Visit the [HealthBar Persistence Example World](https://vrchat.com/home/world/wrld_012ab69b-74d8-439a-ae13-db42478d8680) to try it for yourself!

## Using the Example

1. Open the HealthBar_ExampleScene.
2. Run it in the editor or Build & Test.
3. Walk over to the red 'lava' and stand in it.
4. Look directly up to see your health bar, which will be shrinking as you continue to stand in the lava.
5. Exit the lava, take note of your health bar, and rejoin the world.
6. Your health will be restored to its previous level. Note that if you let your health bar run out entirely, you will respawn and your health will be restored to its full amount.

<HowToImportExample/>

## Technical Breakdown
The lava stores a reference to the local player's health bar. Upon entering the trigger, it will start damaging the player through the health bar's `TakeDamage` function (which should only be called on the local client).

### Inspector Parameters

You can customize the prefabs by changing their parameters:

#### HealthBar

* `float` **MaxHealth** - The player's maximum health.
* `Vector3` **Offset Above Head** - The distance of the health bar above the player's head.
* `Slider` **Health Bar Slider** - The UI slider that shows the player's health.

#### Lava

* `float` **Damage per second** - The damage per second that the lava deals to players touching it.

---
## Changelog
- 0.0.1 - Initial Version
- 0.0.2 - Added in-world UI, thumbnail, published world

---

## ドキュメント: index.md

```metadata
階層: /worlds/examples/persistence/index.md
ディレクトリ: worlds\examples\persistence
ファイル名: index.md
拡張子: .md
サイズ: 320 B
最終更新: 2025-06-05T03:07:52.792Z
```

---
sidebar_custom_props:
    description: Learn about Persistence by adding prefabs to your world.
    customIcon: 📚
---

# Persistence Examples

Learn about the possibilities of Persistence and User Data by exploring these example prefabs.

import DocCardList from '@theme/DocCardList';

<DocCardList />

---

## ドキュメント: leaderboard.md

```metadata
階層: /worlds/examples/persistence/leaderboard.md
ディレクトリ: worlds\examples\persistence
ファイル名: leaderboard.md
拡張子: .md
サイズ: 1.61 KB
最終更新: 2025-06-05T03:07:52.792Z
```

---
description: "Save and display high scores with PlayerData."
sidebar_custom_props:
    customIcon: 🏅
---
import HowToImportExample from '/docs/worlds/examples/persistence/_how-to-import.mdx';

# Leaderboard

![Leaderboard World Preview](/img/worlds/examples/persistence/leaderboard.jpg)

A leaderboard which uses PlayerData to persist and display high scores.

Visit the [Persistent Leaderboard Example World](https://vrchat.com/home/world/wrld_107de08b-0dfa-4ed1-a3d4-398c29616b4c) to try it for yourself!

## Using the Example

1. Open the Leaderboard scene.
2. Then enter Play Mode
3. Your name is on the leaderboard with a score of 0 score.
4. Jump and your score increases.
5. Exit and re-enter Play Mode. 
6. You are once again on the leaderboard, and your score was remembered from the previous session.

<HowToImportExample/>

## Technical Breakdown
There's a `Leaderboard` script where you can add any key you want players to be ordered by. Works with Float and Int. 

Then there's also a `LeaderboardSlot` script which is a RectTransform. This is a PlayerObject which means one will be created for each player joining. When the object is created it's automatically added to Scroll View defined by the Leaderboard. 

The Scroll View's content transform has a Vertical Layout Group which makes all children of it automatically sort by the order they are in the hierarchy. The Leaderboard script updates the position of the `LeaderboardSlot` in the hierarchy each time their persistent score changes.

---
## Changelog
- 0.0.2: Script tweaks: Use VRCEnablePersistence components on player objects

---

## ドキュメント: persistent-idle-game.md

```metadata
階層: /worlds/examples/persistence/persistent-idle-game.md
ディレクトリ: worlds\examples\persistence
ファイル名: persistent-idle-game.md
拡張子: .md
サイズ: 1.88 KB
最終更新: 2025-06-05T03:07:52.792Z
```

---
description: "Save points & upgrades in a simple Idle Game."
sidebar_custom_props:
    customIcon: 👆
---
import HowToImportExample from '/docs/worlds/examples/persistence/_how-to-import.mdx';

# Persistent Idle Game

![Persistent Idle Game World Preview](/img/worlds/examples/persistence/persistent-idle-game.png)

This scene implements a simple [idle game](https://en.wikipedia.org/wiki/Incremental_game) that uses PlayerData to save the points and auto-clicker count for each player.

Visit the [Persistent Idle Game Example World](https://vrchat.com/home/world/wrld_42ba5f54-7fab-4f3a-8fc3-8d2f2ccbb33e) to try it for yourself!

## Using the Example
Load the scene and tweak the Udon values on the "IdleGame" object to your liking.
Play the scene, click the button to collect cheese, buy auto-clickers, leave play mode, and observe that the values are saved under their respective keys in the ClientSim PlayerData window.

<HowToImportExample/>

## Technical Breakdown
The main script is `IdleGame.cs`. It handles the gameplay, saving, and loading.
The only other script, `IdleGameButton.cs` communicates the player's button click to the main script.

Whenever the player clicks the button, they gain one point. The script saves the points in PlayerData using the `POINTS_KEY` string value.

Similarly, when the player buys an auto-clicker, they gain one auto-clicker and lose points. The script saves the amount of autoclickers using the `AUTOCLICKERS_KEY` string value.

The script gives the player one point per second for every auto-clicker. The price of the auto-clickers is multiplied by a fixed amount for every purchased auto-clicker.

Reloading the scene loads the points and auto-clicker value using `POINTS_KEY` and `AUTOCLICKERS_KEY` and calculates the auto-clicker price.

---
## Changelog
- 0.0.2: Script tweaks: The code uses OnPlayerRestored to load player data

---

## ドキュメント: persistent-pen.md

```metadata
階層: /worlds/examples/persistence/persistent-pen.md
ディレクトリ: worlds\examples\persistence
ファイル名: persistent-pen.md
拡張子: .md
サイズ: 2.50 KB
最終更新: 2025-06-05T03:07:52.793Z
```

---
description: "Save Pen Lines with PlayerObjects."
sidebar_custom_props:
    customIcon: 🖊️
---
import HowToImportExample from '/docs/worlds/examples/persistence/_how-to-import.mdx';

# Persistent Pen

![Persistent Idle Game World Preview](/img/worlds/examples/persistence/persistent-pen.jpg)

This example allows players to use a pen to draw up to 20 colored lines. Lines are synchronized for all players. The eraser can highlight and delete lines drawn by the local player.

Visit the [Persistent Pen Example World](https://vrchat.com/home/world/wrld_e24ce788-1fda-42dc-912b-69cd9e51140c) to try it for yourself!

## Using the Example

Play the scene, pick up the pen, and tap "Use" without moving the pen much to cycle through the available colors. Hold "Use" and move the pen to make lines in 3D space, release "Use" to finish the line.

Drop the pen and pick up the eraser, then stick the eraser directly into any of your lines. Known Issue: In ClientSim, this does not highlight the line - it works in the VRChat Client. 

Press "Use" to delete the selected line - this adds it back to your collection of 20 lines.

Stop the scene and then restart it to see your lines restored!

<HowToImportExample/>

## Technical Breakdown
The `SimplePenSystem` has a `VRCPlayerObject` component on it, which instructs VRChat to automatically spawn it for each player in the world, and remove it when they leave. [VRCEnablePersistence](/worlds/components/vrc_enablepersistence) ensures that all synced properties of the pen system, such as the positions, points, and colors of each line, are automatically saved and loaded.

### Udon Program Details

This experience is powered by two programs - one for the [Pen](#udon-pen), and another for each [Line](#lines).

#### Udon Pen

Check out the `Udon Pen` script in the scene under `SimplePenSystem/Pen`. 
- Change the gradient used as the `Palette Color` to swap the available colors.

#### Lines
You can find the Lines the pen uses under `SimplePenSystem/Lines`.
- If you want more lines available per pen, just duplicate some of the existing lines! No other changes are needed.
- You can make changes per line, like setting different widths or materials.

---

## Roadmap
We plan to make the following improvements to this example in the future:
- Respawn the pen whenever it's more than X units away from you.
- Add a color picker where you hold a button to show a palette, then move to the color you want to use.
- Refactor the prefab to allow infinite lines.

---

## ドキュメント: playerdata-types.md

```metadata
階層: /worlds/examples/persistence/playerdata-types.md
ディレクトリ: worlds\examples\persistence
ファイル名: playerdata-types.md
拡張子: .md
サイズ: 2.21 KB
最終更新: 2025-06-05T03:07:52.793Z
```

---
description: "Demonstrates data types supported by the PlayerData interface."
sidebar_custom_props:
    customIcon: 📃
---
import HowToImportExample from '/docs/worlds/examples/persistence/_how-to-import.mdx';

# Player Data Types

![Player Data Types World Preview](/img/worlds/examples/persistence/playerdata_types.jpg)

A barebones examples that demonstrates data types supported by the PlayerData interface. 

Visit the [Player Data Types Example World](https://vrchat.com/home/world/wrld_f9a960d5-c282-431a-bfd0-f9bd6f1158d6) to try it for yourself!

Supported data types include:

- bool
- byte
- byte[]
- int
- float
- double
- long
- short
- string
- sbyte
- uint
- ulong
- ushort
- Vector2
- Vector3
- Vector4
- Quaternion
- Color
- Color32

## Using the Example

In Client:

1. Enter the world
2. Click button “Set Test Data”
3. Observe the display updating with the test data

In Editor:

1. Open the `VRChat SDK > ClientSim Player Data` editor window
2. Observe the test data update here, as well, as you press the button in Play Mode
3. You can clear and re-set the test data
4. You can open the JSON file containing containing the test data, update it manually, and hit Refresh in the editor window to display the updated test data

<HowToImportExample/>

## Technical Breakdown

The `PlayerDataController` has a `SetTestData` method that sets some example player data, which is triggered from the button in the scene. 

It listens for updates to player data using the `OnPlayerDataUpdated` method. It checks if the local player’s data has changed, and if so, it grabs the new data and formats it as a readable string. This string includes details like the type of data (bool, int, string) and the value associated with the player’s unique ID.

The formatted data is then displayed on screen using a `TextMeshProUGUI component`.

The script includes a helper function, `PlayerDataToString`, that checks the type of the data stored under a specific key and converts it to a string so it can be displayed. This function supports all the Persistable data types.

If the player data type is not recognized, it outputs an error message indicating that the type is unknown.

---

## ドキュメント: position-sync.md

```metadata
階層: /worlds/examples/persistence/position-sync.md
ディレクトリ: worlds\examples\persistence
ファイル名: position-sync.md
拡張子: .md
サイズ: 1.44 KB
最終更新: 2025-06-05T03:07:52.794Z
```

---
description: "Save and restore player positions using PlayerObjects."
sidebar_custom_props:
    customIcon: 📌
---
import HowToImportExample from '/docs/worlds/examples/persistence/_how-to-import.mdx';

# Position Sync

![Position Sync World Preview](/img/worlds/examples/persistence/position-sync.png)

A simple PlayerObject prefab which saves each player's position and rotation, then restores them when they rejoin the world.

Visit the [PositionSync Example World](https://vrchat.com/home/world/wrld_7be7b189-7ac8-4cf1-8d0a-1b7743edcacc) to try it for yourself!

## Using the Example

In the VRChat Client:
1. Join the world, move to a spot away from the Spawn point.
2. Rejoin  the world, you should be around the same position and rotation.

In the Unity Editor:
Add the prefab: `PositionSync` to the scene, and done.

<HowToImportExample/>

## Technical Breakdown

If you are standing on the ground, the script will save your position and rotation every 0.5 seconds. Upon receiving the persistent data from the server, it will return you to the position you originally found yourself in.


### Inspector Parameters

* `string` **Synced Position key** - The key name for the position you want to use from PlayerData.
* `string` **Synced Rotation key** - The key name for the rotation you want to use from PlayerData.

---
## Changelog

- 0.0.1: Initial Publish
- 0.0.2: Added in-world instructions, swapped to OnPlayerRestored.

---

## ドキュメント: post-processing-settings.md

```metadata
階層: /worlds/examples/persistence/post-processing-settings.md
ディレクトリ: worlds\examples\persistence
ファイル名: post-processing-settings.md
拡張子: .md
サイズ: 1.60 KB
最終更新: 2025-06-05T03:07:52.794Z
```

---
description: "Save and load bloom settings with PlayerData."
sidebar_custom_props:
    customIcon: 🎚️
---
import HowToImportExample from '/docs/worlds/examples/persistence/_how-to-import.mdx';

# Post-Processing Settings

![Persistent Post-Processing Settings World Preview](/img/worlds/examples/persistence/post-processing-weight-slider.png)

This scene saves and loads its bloom settings by saving the weight of a PostProcessing Volume into PlayerData.

Visit the [Persistence Post-Processing Settings Example World](https://vrchat.com/home/world/wrld_ccc0b71a-63ea-4476-bcc2-25dd1b24745d) to try it for yourself!

## Using the Example

Play the scene, and use the "Post-Processing Weight" slider to set the Bloom level. It should appear in your ClientSim PlayerData window as a float named "settings_pp_weight".

<HowToImportExample/>

## Technical Breakdown

There is an UdonSharp script called "UdonPostProcessing" on a GameObject with the same name.

When the Slider found at "UI for Post-Processing > Content > Slider" is moved, it calls `UdonPostProcessing.SliderUpdated` via a Unity UI Event. This method sets the target PlayerData value whenever the slider is updated.

Whenever PlayerData is updated, the `OnPlayerDataUpdated` event will trigger on this script, and do two things:

- Set the weight on the target PostProcessingVolume, which will change the local bloom strength.

- Update the slider's value and position to match the stored weight. This is done so that the slider will match the value when a player first loads into the instance and the values are restored from the server.


---

## ドキュメント: simple-rpg.md

```metadata
階層: /worlds/examples/persistence/simple-rpg.md
ディレクトリ: worlds\examples\persistence
ファイル名: simple-rpg.md
拡張子: .md
サイズ: 2.24 KB
最終更新: 2025-06-05T03:07:52.795Z
```

---
description: "Save RPG-like player classes and levels."
sidebar_custom_props:
    customIcon: ⚔️
---
import HowToImportExample from '/docs/worlds/examples/persistence/_how-to-import.mdx';

# Simple RPG

![RPG World Preview](/img/worlds/examples/persistence/simple-rpg.jpg)

This scene contains an example of a simple RPG experience where you can level up, change class, and defeat enemies.

## Using the Example

Open the scene or [visit the example world in VRChat](https://vrchat.com/home/world/wrld_32b82323-1d05-4ed4-9ac1-89b437223359) using the `persistence-beta` branch.

1. Open the scene RPGExample in the Assets/Examples/RPG folder.
2. Play the scene.
3. Choose a class by walking over the corresponding pedestal.
4. Once you've gained enough experience by killing enemies, you will level up. You can tell your level by the amount of diamonds above your head.
5. Rejoin the world.
6. When you load into the world, you should be teleported to your previous position and have your last selected class with the same level and experience.

<HowToImportExample/>

## Technical Breakdown

RPGPlayer is a script attached to a GameObject, which has the PlayerObject script on it, this means that the RPGPlayer script is instantiated once for each player.
RPGPlayer is what controls so you can see the diamonds above a players head indicating their class and level, this also is what makes it so you can see others weapon.

Then there are 3 different pedestals you can walk over, which, when triggered, will tell your own RPGPlayer instance to change its class.

The class, last position, experience, and level are all persistent by being UdonSynced and being on a VRCPlayerObject. Any UdonSynced variables that are on a script that is on a GameObject that is part of a PlayerObject are automatically persistent.

Combat is done using particle collisions, so each class has a different type of particle effect as an attack, which has a collider on it.
The enemies give 1 exp when killed, and you need 4 exp for leveling up.
Enemies respawn 10–15 seconds after being killed.

---
## Changelog
- 0.0.3: Added Instructions Canvas
- 0.0.2: Script tweaks: RPG Example uses `VRCEnablePersistence` components on player objects so that they persist again

---

## ドキュメント: unlock-items.md

```metadata
階層: /worlds/examples/persistence/unlock-items.md
ディレクトリ: worlds\examples\persistence
ファイル名: unlock-items.md
拡張子: .md
サイズ: 1.70 KB
最終更新: 2025-06-05T03:07:52.795Z
```

---
description: "Unlock items forever, using PlayerData."
sidebar_custom_props:
    customIcon: 🔒
---
import HowToImportExample from '/docs/worlds/examples/persistence/_how-to-import.mdx';

# Unlock Items

![Unlock Items World Preview](/img/worlds/examples/persistence/unlock_items.jpg)

How to persistently unlock items using PlayerData, using simple in-world achievements as a demo.

Visit the [Unlock Items Example World](https://vrchat.com/home/world/wrld_abe04fe4-583e-44d3-ae89-29bc9f60d166) to try it for yourself!

## Using the Example

1. Enter the world
2. Perform actions to unlock achievements
   - Spend time in the world (10sec,2min, 5min)
   - Move around (10 units, 100 units, 300 units)
   - Respawn
   - Find the secret
   - All of the above
3. Observe the UI showing your unlocked achievements
4. Leave and return - your achievements and stats (time spent in world, distance travelled) should persist

<HowToImportExample/>

## Technical Breakdown

The main `UnlockItems` script checks for the local player's achievements when their data first loads in during the `OnPlayerRestored` method, and restores all the unlocked items that are found.

It then triggers the `UpdateStats` method to run, which runs the following methods:

* CheckTimeInWorld();
* CheckDistanceMoved();
* CheckHasPlayerRespawned();
* CheckHasFoundSecret();
* CheckAllAchievementsUnlocked();

After all methods are run and achievements updated, this method calls itself to run again after a delay of `UPDATE_STATS_TIME`.

For each achievement that is unlocked during the `UpdateStats` check, the `UnlockAchievement` method is called, which updates the sprite, color and text of the target achievement.

---

## ドキュメント: _how-to-import.mdx

```metadata
階層: /worlds/examples/persistence/_how-to-import.mdx
ディレクトリ: worlds\examples\persistence
ファイル名: _how-to-import.mdx
拡張子: .mdx
サイズ: 432 B
最終更新: 2025-06-05T03:07:52.792Z
```

## Importing the Example
Follow the steps below to add this example to your Unity project:
1. Open [the Example Central Window](https://creators.vrchat.com/sdk/example-central) from the window from the Unity Editor Menu under "VRChat SDK > 🏠 Example Central"
3. Find this prefab in the list or search for it by title (same as the title of this page).
4. Press the "Import" button to import the Unitypackage into your project.

---

# ドキュメント終了

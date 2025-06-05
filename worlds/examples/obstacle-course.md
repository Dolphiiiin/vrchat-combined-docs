# obstacle-course 統合ドキュメント

以下は自動収集された全ドキュメントのコンテンツです。各セクションの始めにメタデータが記載されています。



---

## ドキュメント: build-from-custom-parts.md

```metadata
階層: /worlds/examples/obstacle-course/build-from-custom-parts.md
ディレクトリ: worlds\examples\obstacle-course
ファイル名: build-from-custom-parts.md
拡張子: .md
サイズ: 11.07 KB
最終更新: 2025-06-05T03:07:52.789Z
```

---
title: "Obstacle Course: Build From Custom Parts"
slug: "build-from-custom-parts"
hidden: false
createdAt: "2021-08-10T19:34:59.759Z"
updatedAt: "2021-08-18T20:57:33.538Z"
---
:::note Make a Simple Course First

It's highly recommended to work with the Demo prefabs and Starter Scene first. Read through the instructions to [Build From Demo Parts](/worlds/examples/obstacle-course/build-from-demo-parts).
:::
# Customizations
Ok, you've made a simple remix of our demo parts, and you're now ready to add your own custom PowerUps, Hazards and more. Read on to learn how to make new prefabs that interact with the existing systems.

# Making Custom Checkpoints

Here is the hierarchy of the Checkpoint prefab we include:

![build-from-custom-parts-692d375-checkpoint-hierarchy.png](/img/worlds/build-from-custom-parts-692d375-checkpoint-hierarchy.png)

## Trigger Requirements
![build-from-custom-parts-f72c567-checkpoint-inspector.png](/img/worlds/build-from-custom-parts-f72c567-checkpoint-inspector.png)

:::note It's a Pattern!

The "Trigger" object on this prefab uses a pattern that you'll see repeated on nearly every object that detects the player - Checkpoints, PowerUps, Hazards, Respawners, etc. We'll go into detail on this first prefab, and we can skip past it for the rest.
:::
Your checkpoint needs a **Collider** on the **CourseTrigger** layer with _isTrigger_ turned on and an UdonBehaviour with the **OnPlayerDataEnter** program.

![build-from-custom-parts-f896bef-checkpoint-inspector.png](/img/worlds/build-from-custom-parts-f896bef-checkpoint-inspector.png)

This UdonBehaviour needs the following variables set:
1. _fxPrefab_ should reference a Prefab which has some fun effects on it, and either the Udon Program **DestroyAfterXSeconds** or some other way that it destroys itself after some time.
2. _program_ should reference an UdonBehaviour on another object under this prefab, which has a **Checkpoint** program on it.
3. _eventName_ should be set to an Event that exists on the UdonBehaviour referenced above.
4. _deactivateOnTrigger_ should be checked for Checkpoints so that it can only be activated once per run until a player finishes the course.
5. _sendPlayerData_ is only required for a Checkpoint being used as a StartGate.

:::note OnPlayerDataEnter details

If you want to learn more about what's going on in this program, you can find the full documentation here: [OnPlayerDataEnter](/worlds/examples/obstacle-course/uoc-how-stuff-works#onplayerdataenter)
:::
## Checkpoint Program Requirements
All the above setup just detects the player, creates an FX object, and then triggers an event on the _real_ program. In this case, that's a program called **Checkpoint** on an object called "UdonProgram". The program on the collider will try to run a program called "Trigger" on this UdonBehaviour. 

All you need to do is have an object with an UdonBehaviour, with the **Checkpoint** program on it. all the variables of this program will be set automatically when you place it, or realtime when someone Triggers it in your world.

## Start / Finish Program Requirements
If you're making a Checkpoint to be used as a Start or Finish Gate, you'll set up your program slightly differently. The Trigger Collider will be the same, and you'll use the same **Checkpoint** program, but you'll need to make the following changes:

### For a **StartGate**:
* Set the _eventName_ to "StartRace"
* Make sure _sendPlayerData_ is on.

### For a **FinishGate**:
* Set the _eventName_ to "FinishRace"

## Customize It!
As long as you have the **Trigger Collider** and **Checkpoint Program** set up as described above, you have a working checkpoint prefab you can use in your world. Note that when someone Triggers your checkpoint, the GameObject on which the Trigger Collider sits will be set to **inactive**. So if you want to hide the whole checkpoint, put the collider on the topmost object. If you want to just hide some parts like we do in the demo prefabs, make those parts children of the Trigger Collider object.

When the someone finishes running the course or Respawns themselves through their menu, the Course is **Reset**. When this method is called, the Course program will search every checkpoint for Trigger Colliders. It will set all the GameObjects with Trigger Colliders to inactive, except for the first one, which it will set to active (your **Start Gate**). Keep this in mind if you have additional colliders on your prefabs - their GameObjects will be set to inactive if the colliders have _isTrigger_ turned on.

## Add it to your Checkpoint Prefabs list

The Utility Window lists your "Checkpoint Prefabs" for easily adding them to your scene. You can drag and drop your new custom prefabs into this list to swap them out, or change the "Size" of the list first to add new empty slots to which you can add your new prefabs.

# Making Custom PowerUps
Read through "Making Custom Checkpoints" above first to understand how the Trigger Collider system works, since it's the same for PowerUps. Once you've got a your Trigger Collider set up, you can work on the **PowerUp** program.

## PowerUp Program Requirements
![build-from-custom-parts-c3ecfa0-speed-up-program.png](/img/worlds/build-from-custom-parts-c3ecfa0-speed-up-program.png)

* First, make sure you have a Trigger Collider set up which calls "Trigger" on this UdonBehaviour.
* _playerModsManager_ can be left alone, this will be injected when you create the PowerUp through the Utility window, or when you press "Refresh".
* Either _speedChange_ or _jumpChange_ should be set to something other than 0. Positive values will be added to the default **MoveSpeed* or **Jump Impulse** you set as defaults in the **Power Ups** section of your Utility Window. Negative values will be subtracted. You can combine them - a PowerUp that makes you jump high but move really slow is totally valid.
* The _effectDuration_ should be something higher than 0 (no negative numbers). This is how long the  PowerUp will last. The player will get a message on their HUD that shows the change, this will fade away at the speed you've set here.

## Add it to your PowerUps Prefabs list

The Utility Window lists your "PowerUp Prefabs" for easily adding them to your scene. You can drag and drop your new custom prefabs into this list to swap them out, or change the "Size" of the list first to add new empty slots to which you can add your new prefabs.

# Making Custom Hazards

Hazards use the Trigger Collider setup that we documented under **Checkpoints** above, so make sure you've read through that first.

We've created two types of Hazard that you can work from - **Respawn** hazards and **Spawned** Hazards.

## Respawn Hazards
This is the most common Hazard in our demo course. It uses a Trigger Collider to Respawn the player to the last Checkpoint. You need a trigger collider set up to run the "Trigger" event on another object which has the **RespawnOnCourse** program on it.
![build-from-custom-parts-752dc13-moving-wall-hazard.png](/img/worlds/build-from-custom-parts-752dc13-moving-wall-hazard.png)

The RespawnOnCourse program will automatically have the _course_ variable set when you Refresh your UtilityWindow.

## Spawned Hazards
This hazard is made up of two parts, it's the most customized single-purpose Hazard we provide, as an example of extending our basic system to add functionality.

### HazardSpawner Program
![build-from-custom-parts-3ab9259-hazardspawner.png](/img/worlds/build-from-custom-parts-3ab9259-hazardspawner.png)

This program will spawn its _prefab_ every _delay_ seconds. It has a reference to _playerModsManager_ which will be injected when the Utility Window is Refreshed.
When it spawns a Hazard, it will look for an UdonBehaviour on the new object, and set its _playerModsManager_ variable to the reference it has. This is needed so the spawned Hazard can reduce the player's speed.

### SpawnedHazard Program
![build-from-custom-parts-a85b1af-barrel-hazard.png](/img/worlds/build-from-custom-parts-a85b1af-barrel-hazard.png)

This prefab has a TriggerCollider on the "Trigger" child object which runs the event "HitPlayer" on the SpawnedHazard program. 

* _lifeDuration_ controls how long this prefab will exist after spawning, to ensure it doesn't stick around forever if there are no players around.
* _playerModsManager_ is set by the **HazardSpawner** program
* _speedChange_ works like a typical **PowerUp**. The 'x' value is the change to apply to the player's default speed, and the 'y' value is the duration of the effect. The default setting of (-3,3) on this prefab will subtract 3 from the player's speed for 3 seconds when they touch it.

:::danger UNPACK YOUR HAZARD PREFABS

Since these prefabs are not created and managed through the Utility Window, it's important to unpack them after you place them in your scene. Otherwise the auto-injection might not stick.
:::
# Other Customizations
Here are some other things you can play with:

## Score Fields
![build-from-custom-parts-60cfc05-ScoreManager.png](/img/worlds/build-from-custom-parts-60cfc05-ScoreManager.png)

If you want to change the look of the Score Fields or the number of scores that are shown, you can duplicated the "ScoreField" prefab, drop your new version into the "Score Object Prefab" slot in the "Score Manager" section of the Utility Window, and set the "Number of Scores to Show" to regenerate the UI that displays the scores.

## HUD
You can change the look and layout of HUD items - just look through the hierarchy of the "HUD" object.

## Minimap
If you want to change the camera angle, smoothing amount, etc of the Minimap, you can find the **CinemachineVirtualCamera** which it uses under "MinimapCameraSystem/VCam-Follow". Its _Follow_ and _LookAt_ variables are set at runtime, so if you want to test it in the Editor, you can drop in a Capsule, set that as the target for those two variables, and drag it around your course to see how it looks in the Minimap.

## Advanced Stuff
You can click on the new Course Asset you made in your Project pane to see its raw data, and access all kinds of stuff that you can change **_at your own risk_**, like the default Udon Programs. You can also modify the "Variable to Scene Object Lookup" section:

![build-from-custom-parts-489afea-lookup.png](/img/worlds/build-from-custom-parts-489afea-lookup.png)

This is where the UtilityWindow looks when it runs Refresh() to inject the right objects into UdonBehaviours. On the left are variable names, and on the right are the names of objects in the scene on which the correct Component can be found. Right now, the system supports finding and injecting these Component Types:
* GameObject
* UdonBehaviour
* CinemachineVirtualCamera

So you can make a new Udon Program with a public variable called "course", and it will automatically be injected with the CourseManager UdonBehaviour. You can add your own items here, and even add additional types if you like - just modify the **InjectVariableReferences** method on the **ObstacleCourseEditorWindow** class, adding extra types in the if-chain. If you write a good generic Component handler, make a Pull Request to the git repo!

---

## ドキュメント: build-from-demo-parts.md

```metadata
階層: /worlds/examples/obstacle-course/build-from-demo-parts.md
ディレクトリ: worlds\examples\obstacle-course
ファイル名: build-from-demo-parts.md
拡張子: .md
サイズ: 9.66 KB
最終更新: 2025-06-05T03:07:52.789Z
```

import UnityVersionedLink from '@site/src/components/UnityVersionedLink.js';

# Obstacle Course: Build From Demo Parts

## Open Starter Scene
The easiest way to make a new course is to use the models and prefabs we provide.
Start by opening the scene "_WorldJam2/Scenes/Starter.unity"

## Make your own Folder
It's important to save anything specific to your project in a folder outside of the "_WorldJam2" folder so you can import updates without overwriting your work. We recommend you create a new folder under Assets, we'll call it "_MyProject" for this demo. We use underscores at the beginning of important folders so they show up at the top of the alphabetically-sorted file listing.

## Make New Course Asset
The **ObstacleCourseAsset** holds all the special information about your Checkpoints, Player Prefabs,  Score Display, PowerUps and more. 
1. In the **Project** window, find the "StarterCourse.asset" under "_WorldJam2/Courses" and Duplicate it.
2. Rename the new course to something custom like "MyCourse.asset" and move it to your "_MyProject" folder.
3. In your hierarchy, select the **CourseManager** under "Udon/CourseManager".
4. Drag and drop the new Course Asset your just created to the "Asset" field on the **Obstacle Course Data** script on the CourseManager object.

![build-from-demo-parts-8a69d28-drop-course-asset.png](/img/worlds/build-from-demo-parts-8a69d28-drop-course-asset.png)

Now all your changes will be saved to your custom course instead of the Starter course.


## Add Course Pieces
You can find all the available Course Pieces in the project under "Assets/_WorldJam2/Prefabs/Course Pieces".
![build-from-demo-parts-ebf489c-all-course-pieces.png](/img/worlds/build-from-demo-parts-ebf489c-all-course-pieces.png)

1. To add new pieces to your course, just drag and drop them into the scene view. You can hold CTRL while dragging to snap the items to a grid.
2. If you're using Udon on any of your pieces, make sure to Unpack them from being prefabs to just regular GameObjects. It's also a good idea in case you want to load in an updated package for this project with overwriting your existing pieces.

:::note Grid Snapping

Unity has many settings for aligning items to a grid - check out <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/GridSnapping.html">Unity's Manual</UnityVersionedLink>
:::

## Add Checkpoints
Your Start Gate, Checkpoints and Finish Gate are best added through the special Utilities window we made for this jam.

1. Open the **Obstacle Jam Utilities Window** from your menu bar under "⏵Obstacle Jam Utilities / Open Window"
![build-from-demo-parts-df0b76f-open-utilities-window.png](/img/worlds/build-from-demo-parts-df0b76f-open-utilities-window.png)

2. Check out the prefabs:

![build-from-demo-parts-22a6f4e-checkpoint-prefabs.png](/img/worlds/build-from-demo-parts-22a6f4e-checkpoint-prefabs.png)

These are the only three prefabs we need to make a working course - a start Gate, a Checkpoint and a FinishGate.

3. The utilities window makes it very easy to add new Checkpoints.Select a prefab from the "Checkpoint Prefabs" list and move your cursor over to your Scene View. You'll see a preview of the selected prefab, it will try to place itself intelligently on the surface you're pointing at - notice how the gate snaps to the side of the block in the GIF below until I point at the top. 
4. Once you're happy with the placement, press the Spacebar to actually add the prefab and wire it up to your scene. 
![build-from-demo-parts-4200ff4-place-gates.gif](/img/worlds/build-from-demo-parts-4200ff4-place-gates.gif)

5. When you add a Checkpoint prefab this way, it is automatically added to your "Checkpoints In Scene" list. Open that list to see the new checkpoint included:

![build-from-demo-parts-84c73fe-checkpoints-in-scene.png](/img/worlds/build-from-demo-parts-84c73fe-checkpoints-in-scene.png)

Select one of these checkpoints and the Scene View will move to focus on it. You can also see it selected in the hierarchy. If you press the triangle next to this new "Checkpoint 1" object in your hierarchy, you can see an UdonProgram on it. Select that UdonProgram and you can see that its 'index' has been automatically set to 1.

![build-from-demo-parts-feaf465-checkpoint-program.png](/img/worlds/build-from-demo-parts-feaf465-checkpoint-program.png)

6. Continue to add checkpoints around your course until you've got enough to get started. If you need to rearrange the order, you can use the up and down arrows in the "Checkpoints in Scene" list to change the order in which players should go through your gates. When you change the order this way, the Checkpoint's index is changed to match its actual order, and its name is changed to match its index.

## Test Checkpoints
At this point, you've extended or changed the course, and added some checkpoints - time for a test! Open the VRChat SDK Control Panel, sign into your account, and choose "Build & Test" to test out your course!
:::note Build and Test

If you have any problems launching your world or just want to learn more about local testing, check out the [Build and Test](/worlds/udon/using-build-test) docs.
:::
## Add PowerUps
Let's add some PowerUps to make things more interesting.

1. In the Utility Window, open the "Power Ups" section. It's very similar to the Checkpoints section, with different parameters for the ones you've already placed.

![build-from-demo-parts-9c8911e-power-ups-section.png](/img/worlds/build-from-demo-parts-9c8911e-power-ups-section.png)

2. Select one of the PowerUp Prefabs, then place it in your scene by pointing your mouse at a surface and pressing the spacebar, just like you did for Checkpoints.
3. After placing a PowerUp, the **Transform** tool is selected so you can fine-tune the placement of your item.
4. Your new PowerUp appears in the "PowerUps In Scene" section. You can use the fields to the right of the PowerUp's name to change the effect it has on your players. You can use negative numbers to _reduce_ a player's Speed and Jump, but a negative duration will have no effect.
5. Select a PowerUp to automatically focus on it in the Scene View and select it in the hierarchy. If you want to remove one, you can delete it from your scene and press 'Refresh' at the bottom of the Utility Window to update the listing.

You can also change the default **Move Speed** and **Jump Impulse** in this section. Make sure you **DON'T** put a **VRCWorldSettings** program in your world or else it will fight with these settings.

Move Speed sets **Walk**, **Run** and **Strafe** speeds to be all the same.

## Test PowerUps

**Build & Test** your scene again to try out your new PowerUps!

## Add Hazards

1. Find the Hazard prefabs in "Assets/_WorldJam2/Prefabs/Hazards"
![build-from-demo-parts-5c1a72c-hazards.png](/img/worlds/build-from-demo-parts-5c1a72c-hazards.png)

2. These don't use our special tools since they don't need any fancy setup. Just drag and drop them into your scene and fine-tune their placement.

3. After you've placed a hazard, right-click the GameObject in your hierarchy and choose "Unpack Prefab Completely". This will ensure that any updates to your project won't overwrite Hazards you've already created, and avoid some known issues with Udon and Prefabs.
![build-from-demo-parts-9d80caf-unpack-prefab.png](/img/worlds/build-from-demo-parts-9d80caf-unpack-prefab.png)

4. Once you've placed your Hazards, press the "Refresh" button at bottom of the Utility window. This will inject some references into your Hazards so they can properly Respawn your users when they touch the Hazard.

:::danger NO SERIOUSLY - UNPACK THAT PREFAB!


:::
### Modular Hazards
The hazards we've included are modular so you can easily modify their look and difficulty. Each moving hazard consists of a collider set to "Trigger" attached to an animated game object. You can add different meshes and trigger placement for a whole new hazard concept using different course pieces we've included or kit-bashing other asset packs.
![build-from-demo-parts-26e93e5-uoc_hazard_pic1.png](/img/worlds/build-from-demo-parts-26e93e5-uoc_hazard_pic1.png)

The four types of moving hazards included give you a starting point for horizontal, vertical, spinning and swinging hazards. You can change the speed of each type by using the Animator window.
![build-from-demo-parts-9b460a2-uoc_hazard_pic2.png](/img/worlds/build-from-demo-parts-9b460a2-uoc_hazard_pic2.png)

## Check Hazards

Time for another **Build & Test!**

## Set Number of Players
It's a good idea to set the **Number of Players** in your Obstacle Course world to twice as high as the **Player Capacity** in your world. Just change the number in this field in your Utility Window, and the **Object Pool** which manages **Player Objects** will automatically fill with that number of players, and it will set all those players up with the variables they need.
![build-from-demo-parts-574bf2e-number-of-players.png](/img/worlds/build-from-demo-parts-574bf2e-number-of-players.png)

## Build & Publish

Once you're ready to invite your friends to help test your world, you can **Build & Publish** it to make it available in VRChat. You'll set your world name, take a picture to share, and you can Submit to Community Labs once you're ready for more people to see it!
:::note Publishing & Trust Levels

You need to be spend some time in VRChat before you can publish worlds. Learn more here: [FAQ](https://docs.vrchat.com/docs/frequently-asked-questions#why-cant-i-upload-content-yet)
:::
If you want to explore more options for Customizing your Obstacle Course, you can read on to [Build From Custom Parts](/worlds/examples/obstacle-course/build-from-custom-parts)

---

## ドキュメント: index.md

```metadata
階層: /worlds/examples/obstacle-course/index.md
ディレクトリ: worlds\examples\obstacle-course
ファイル名: index.md
拡張子: .md
サイズ: 2.77 KB
最終更新: 2025-06-05T03:07:52.789Z
```

---
title: "Obstacle Course"
hidden: false
createdAt: "2021-08-10T18:17:12.204Z"
updatedAt: "2021-09-21T22:51:28.690Z"
sidebar_custom_props:
    customIcon: 🏃‍➡️
---
# Overview
<iframe class="embedly-embed" src="//cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fwww.youtube.com%2Fembed%2Fid3VqPUy_rY%3Ffeature%3Doembed&display_name=YouTube&url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3Did3VqPUy_rY&image=https%3A%2F%2Fi.ytimg.com%2Fvi%2Fid3VqPUy_rY%2Fhqdefault.jpg&key=f2aa6fc3595946d0afc3d76cbbd25dc3&type=text%2Fhtml&schema=youtube" width="854" height="480" scrolling="no" title="YouTube embed" frameborder="0" allow="autoplay; fullscreen" allowfullscreen="true"></iframe>

This project was created for our second World Jam, and serves as a fun starter kit for a Time Trial game world!

Visit the example world here: [VRChat Obstacle Course Jam World](https://vrchat.com/home/world/wrld_39c422c4-ab87-4cc1-a4d1-390af2e45c74).
Download a ready-to-use project here: [VRChat Obstacle Course Project](https://github.com/vrchat/VRChat-Obstacle-Jam/releases/download/1.0.3/obstacle-jam-public_v1.0.3.zip).
For advanced users, you can [fork the GitHub project](https://github.com/vrchat/VRChat-Obstacle-Jam) to easily stay up-to-date with any updates or fixes.

It's built to be usable by creators with little-to-no Udon experience, you can just snap together a new course using the existing models, or creating / importing your own.

To explore the scene, simply download the latest version, and Build & Test the 'Assets/_WorldJam2/Scenes/DemoScene' scene.

# Quick Start
Here are some easy things you can modify to get started:

## Move & Jump
Open the Toolkit window from your menu bar by finding the "▶ Obstacle Course Toolkit" item and selecting "Open Window" from its dropdown. Open the "Power Ups" section and you can change the default Move and Jump powers for the world.
![index-40a0a08-utility-change-defaults.png](/img/worlds/index-40a0a08-utility-change-defaults.png)

## Number of Players
If you want to increase the number of players that can run through the course, just open the "Player Manager" section of the Utility Window and increase the number here. Make sure you change the number of players your world can handle when you publish your world to be half of this number!
![index-985e270-number-of-players.png](/img/worlds/index-985e270-number-of-players.png)

## PowerUp Properties
Back in the "Power Ups" section of the Utility Window, open the "PowerUps in Scene" section. Click on the name of a PowerUp and the Scene View will focus on it so you can see which one it is. Then you can change the _speed_, _jump_ and/or _duration_ fields to update the variables on that PowerUp.
![index-f5481d9-powerups-in-scene.png](/img/worlds/index-f5481d9-powerups-in-scene.png)


---

## ドキュメント: uoc-flythrough.md

```metadata
階層: /worlds/examples/obstacle-course/uoc-flythrough.md
ディレクトリ: worlds\examples\obstacle-course
ファイル名: uoc-flythrough.md
拡張子: .md
サイズ: 2.93 KB
最終更新: 2025-06-05T03:07:52.790Z
```

---
title: "Obstacle Course: Flythrough"
slug: "uoc-flythrough"
hidden: false
createdAt: "2021-08-12T22:56:42.710Z"
updatedAt: "2021-08-18T20:58:14.295Z"
---
We included a Cinemachine system to easily generate a fly-through video of your course!

# Basic Setup
* Drag and Drop the **Flythrough** prefab from **Assets/_WorldJam2/_SubSystems/Flythrough** into your scene.
* Press "Refresh" in the Obstacle Utility Window. This will generate a path based on all the Checkpoints in your scene!
* Select the **FlythroughPrefab/RecordCamPath** object in your hierarchy to see the path that was created.
![uoc-flythrough-2114971-flythrough-path.png](/img/worlds/uoc-flythrough-2114971-flythrough-path.png)

* Press 'Play' in the Game View to see your flythrough! Exit Play Mode once you have an idea of how it looks.

# Modifying the Path

By default, the path will be generated 0.5 units above each checkpoint's origin. If this is too high or low for your particular scene, you can modify the "Record Path Y Offset" in the **Recording** section of the VRC Obstacle Window to change the height of every checkpoint at the same time. You can click and drag left and right on the label to easily dial in the right spot.

If you want to add / remove / reposition the waypoints in the path, turn off 'Auto Update Checkpoints' in the Recording section, then select the **RecordCamPath** in the hierarchy. Here, you can change the numbers directly, or press the number of the waypoint on the left side of the inspector to select the waypoint in the scene view and move it using the standard transform tool.

You can change the speed of the flythrough by selecting the *RecordCamTarget* object in the hierarchy and changing the _Speed_ parameter on the **Cinemachine Dolly Cart* component. The default setting uses Normalized Position Units with a Speed of .03 to make it through the course in about 30 seconds.
![uoc-flythrough-5f34a79-waypoints.png](/img/worlds/uoc-flythrough-5f34a79-waypoints.png)

# Recording the Output
You can use a screen recorder to directly record your Game View in the Editor, or use the Unity Recorder Package to generate a higher-quality output. You can add this from the Package Manager. It has been tested and works with this setup. If you want full instructions on using the Recorder, you can find the official Unity Manual here: [Unity Recorder 1.0 User Manual.](https://unitytech.github.io/unity-recorder/manual/index.html) and a tutorial here: [Working with Unity Recorder](https://learn.unity.com/tutorial/working-with-unity-recorder).

# Cleaning Up
Make sure to remove the recorder from your scene before you publish! If you customized a path, you can make a copy of the prefab with your new path to use it again later.

# Extra Credit
You could integrate this flythrough camera with the existing Cinemachine system in the scene (which renders to the Minimap and in-game jumbotrons), or create a separate system for high-quality in-game recording!

---

## ドキュメント: uoc-how-stuff-works.md

```metadata
階層: /worlds/examples/obstacle-course/uoc-how-stuff-works.md
ディレクトリ: worlds\examples\obstacle-course
ファイル名: uoc-how-stuff-works.md
拡張子: .md
サイズ: 17.66 KB
最終更新: 2025-06-05T03:07:52.790Z
```

---
title: "Obstacle Course: How Stuff Works"
slug: "uoc-how-stuff-works"
excerpt: "How all the different programs and custom editor scripts work together in the Udon Obstacle Course"
hidden: false
createdAt: "2021-08-10T19:42:13.400Z"
updatedAt: "2021-08-18T20:57:54.974Z"
---
Each system was designed to have a specific set of responsibilities, and to know about other systems as little as necessary. 

# Overview
* The **PlayerDataManager** assigns **PlayerData** objects to each player who enters the world.
* When a **PlayerData** object enters the Start Gate **Checkpoint**, the **Course** they just entered starts tracking their time, and activates the next **Checkpoint**.
* When the **PlayerData** object passes through the last **Checkpoint**, their time is added to the Scoreboard.
* If the **PlayerData** object enters a **PowerUp** trigger, the **PlayerModsManager** temporarily changes their speed and/or jump abilities, resetting them to default after a set duration
* If the **PlayerData** object enters a **Respawn** trigger, the **Course** will respawn them at the last **Checkpoint** through which they passed.

The following sections describe the programs and scripts that combine to make the whole experience.

# Players
Each player who joins the world gets a 'PlayerData' object to manage their state and progress through a course. The **PlayerDataManager** assigns **PlayerData** objects, which can trigger **OnPlayerDataEnter** programs.

## PlayerDataManager
You can find this program on the "PlayerDataManager" GameObject under the "Udon" object in the scene. It has two important public variables:
**dataPool**: Reference to the VRC Object Pool component on the same object as this Manager. When a Player Joins the world, this manager will TryToSpawn a PlayerData object for them, and give them ownership.
**followCam**: Reference to the camera that will follow above a Player as they run through the course. Set here so the PlayerDataManager can assign the reference to each PlayerData object when they are refreshed.

When you change the 'Number of Players' option in the Toolkit Window, all the existing PlayerData objects will be removed from the scene, then new copies of them will be added as children of the PlayerDataManager. Each one will have its public variables set up properly, and the Object Pool will be updated to hold all the new PlayerData objects.

## PlayerObject
The PlayerObject prefab has a Rigidbody and Capsule Collider component, which are needed to trigger PowerUps, Hazards, etc. It's on a custom layer **CoursePlayer** which only collides with **CourseTrigger** to interact with Hazards and PowerUps. It also has an UdonBehaviour with an important program on it:

## PlayerData
This program is the main connector between the player running the course and all the other systems. Its variables are:
**timeElapsed**: Synced Float which is updated by the **Course** program when they cross the Finish Gate. When it changes, the owner of the PlayerData object will show this time on the scoreboard so they can see their latest time locally. The owner of the ScoreManager object will see this change and add the new time and displayName of the Player to the scoreboard.

**isRacing**: Boolean which is set true by the **Course** when the player has entered a start gate. It's set to false when the player enters a finish gate, manually respawns using their menu, or **Reset** is called on the **Course**. Used by the **Course**, see that program for more info.

**rigidbody**: Cached on Start by the program, it doesn't need to be set in the inspector. It's moved to the position and rotation of the player during every Update.

**player**: Reference to the actual VRCPlayerApi object of the local player. Cached when the synced _playerId_ on this program is changed. Used to retrieve the _displayName_ of the player.

**timeDisplay**: Reference to the UdonBehaviour which displays the latest time for the local player.

**scoreManager**: Reference to the **ScoreManager** UdonBehaviour. When the owner of that object receives a _timeElapsed_ change from a **PlayerData** object which just finished the course, it sets the public variable _scoreToProcess_ on the **ScoreManager** object to a string which combines the _displayName_ and _elapsedTime_ into a single string to be processed.

**scoreManagerObject**: Reference to the GameObject which holds the UdonBehaviour with the **ScoreManager** program. Needed to ensure we only run the Score Processing logic on the owner of the **ScoreManager** object. We can't get this GameObject from an UdonBehaviour reference, so we include it here.

**followCam**: Reference to the CinemachineVirtualCamera which follows the player around the course. The program sets its own Transform as both the _follow_ and _lookAt_ targets for the camera, and changes the priority on this camera when _isRacing_ changes.

## OnPlayerDataEnter
This program is used on objects which should detect the **PlayerData** object entering its Trigger Collider. We use the custom layers **CoursePlayer** and **CourseTrigger** ensure that only certain objects will trigger this collider. When they do, it fires the internal event **OnPlayerDataEnter** to do a multitude of things. This program has the following variables:

**fxPrefab**: A GameObject to spawn when on **Trigger**, meant to play a sound, show some particles, etc so the Player knows that something has happened.

**program**: A target UdonBehaviour with an event we want to run on **Trigger**. This _program_ contains the specific logic for a **Checkpoint**, **PowerUp**, **Hazard**, etc.

**eventName**: The event name to run on the target _program_.

**deactivateOnTrigger**: Whether this object should deactivate itself after a single **Trigger**. This is useful for **Checkpoints** and other items that should only activate once per run.

**lastCollider**: Collider which started the Trigger logic, which is temporarily cached before **Trigger** is called and used to find the **PlayerData** UdonBehaviour if needed.

**fxSpawn**: A Transform we use to set the position of the FX we will spawn. Defaults to the Transform of the object with the collider if not set, useful if you want to trigger Fireworks in another location when running through a collider, like we do for the Finish Gate.

**sendPlayerData**: A Boolean that decides whether or not to try to pass along the **PlayerData** program that Triggered the logic. Used when entering a Start Gate, could be useful for other things as well.

When a **PlayerData** collider trigger entry is detected, this program does the following:
* If we have set an _eventName_ variable, then we will check whether _sendPlayerData_ is true. If it is, we will try to set the _playerData_ variable on the target UdonBehaviour program to the UdonBehaviour with which we just collided.
* We will then run the event _eventName_ on the target program.
* If the _fxPrefab_ GameObject on this program was set (not left at default of 'self'), then we will **Instantiate** a copy of the prefab and set its position and rotation from the _fxSpawn_ variable.
* If _deactivateOnTrigger_ is true, then we will set **this** GameObject to inactive.

# Course & Checkpoints
This is the heart of the project, the gates and checkpoints that you need to move through to complete the time trial.

# Course
This program lives on the CourseManager object and manages the state of the time trial for the local player. It doesn't have any synced variables - it only knows about the Local Player who is running through it.

On **Start**, it calls **Reset** to set itself up properly.
If the player Respawns themselves, the Course will **Reset**.

On **Reset**, we turn off all of the **Checkpoint** triggers except for the Start Gate, which we turn on. We do this by looping through each GameObject in the _checkpoints_ array, finding every Trigger Collider, and calling **SetActive** on that collider's GameObject to true for index 0 and false for all the others.
We also set _nextIndex_ to -1 and set _isRacing_ to false.

On **StartRace**, we:
* set _startTime_ from the current time
* set _isRacing_ to true
* set _nextIndex_ to 1 (since the race is started by passing through Checkpoint 0)

When a **Checkpoint** is triggered, it sets the _nextIndex_ on the Course to its own index + 1. This triggers the **nextIndexChange** event on the Course program, which will then activate the GameObject for the next Checkpoint.

During **Update**, we check whether a player _isRacing_, and if so we get the elapsed time of the run and set it on the _timeDisplay_ Text object.

On **FinishRace**, we:
* set _isRacing_ to false
* set _timeElapsed_ on the target **PlayerData** program to the current time minus _startTime_.
* set _playerData_ to null since we no longer have a player running the course.
* wait for _resetDelay_ seconds and then **Reset** the course.

On **Respawn**, we check whether the player _isRacing_. If so, we send them back to the transform position of the last checkpoint. If not, we teleport them down low enough that they will be respawned by the world, back at one of the original spawn points.

## ObstacleCourseData
This custom script just holds a reference to the **ObstacleCourseAsset** with all the info about your course like which prefabs to use, the number of players, the default speeds, etc. It's loaded by the Utility Window so you should have one in your scene. You should create your own so it's not overwritten if you update your project with a newer version of this package. Do this by duplicating an existing asset, which will ensure the default values are correct.

## Checkpoint
The Checkpoint objects each have an index which represents their order in the time trial, this is automatically set when placing Checkpoints through the Utility Window or modifying their order. 
They have a Trigger Collider with an **OnPlayerDataEnter** program which call into a **Checkpoint** program that we have on an object called "UdonProgram" in our example prefabs. The program is simple, with three possible events that can be triggered on it:

**StartRace** will set the _playerData_ variable on the **Course** program to the UdonBehaviour that just entered this checkpoint. The Course will start the race when that happens.

**Trigger** will set the _nextIndex_ variable on the **Course** program to the _index_ + 1.

**FinishRace** will simply call **FinishRace** on the **Course** program.

# Score
What's a time trial without some friendly competition? The Score system syncs the names and times of the latest runs, as well as the best run so far in the instance.

## ScoreManager
This program sits on an Object called "ScoreManager" under the "Udon" GameObject. It uses a queue system to process incoming scores and sync them. It doesn't actually have _any synced variables_ itself, relying on the *ScoreFields* to sync the values instead. These fields are automatically populated by the Utility Window when you change the Number of Scores to Show.

On **Start**, this program calls its own **Render** event once.

On **Render**, the program calls **Render** on the _scoreCam_, which will render its current view to a RenderTexture used all over the course to show the current score.

When _scoretoProcess_ is changed on the Owner of this Object, we call **MakeRoom** and then **ProcessNextScore**. This works because every player in your instance will receive an update to _timeElapsed_ when someone finishes a run, and that program will update _scoreToProcess_ on this object if they are the owner.

On **MakeRoom**, we check if our scoreFields are all full already, and if so we'll copy the values down iteratively to make room at the top.

On **ProcessNextScore**:
* Pull apart the score into displayName and time again in order to format them nicely, and then set the _targetVarName_ value on the corresponding **ScoreField** to this. This target variable is synced, so we set it this way to update it for everyone. 
* Compare the time of this score against the time of our High Score and update the **HighScoreField** if necessary. 
* Set the value of _scoreToProcess_ to an empty string so its ready to process the next score that comes in. 
* Send the **Render** event to everyone to update their score texture.

## ScoreField
This program uses a simple and effective pattern - it has a public synced variable called _log_. When log changes, it updates the text in the field to the new value. In this way, the values are synced and updated for everyone when the owner of the object updates it, which can be done easily from another program. In our case, we update this value from the **ScoreManager**.

## HighScoreField
This program uses the same pattern as the score field above, but also has a synced _score_ float that can be used to compare scores and update only if the new score is better. It also has a "prefix" which is a string injected before any changes. In this case, the string "High Score:" is prepended to the incoming string.

# PowerUps
It's fun to offer speed and jump boosts for players looking to maximize their scores, You can also use speed and jump penalties as part of obstacles and hazards to give your players some choice in strategy. PowerUps are all placed as children of the "PlayerModsManager" object when you create them with the Utility Window. They also have the *PlayerModsManager* UdonBehaviour set on them automatically so they can apply their effects.

They have a very simple program. It's called from an **OnPlayerDataEnter** program of course, and has a single **Trigger** event. Its variables are:

**playerModsManager**: Automatically set when creating PowerUps through the Utility window. Used to actually apply the effects.

**speedChange**: Effect to apply to the Player's speed when triggered. 0 will skip, positive will increase speed, negative will decrease it.

**jumpChange**: Same as speedChange, but for Jump Impulse.

**effectDuration**: How long until the effect wears off.

On **Trigger**, the program will set _speedToProcess_ on the **PlayerModsManager** if it's not 0, and it will set _jumpToProcess_ if it's not 0. In order to simplify the logic, we bundle the _amount_ and _duration_ values into a single Vector2, where the _x_ is amount and _y_ is duration.

## PlayerModsManager
It's useful to have a central place to manage changes to a Player's abilities, especially when you consider that someone could run through a "Speed + 3" with a 2 second duration, and then a "Speed - 1" with a 3 second duration. In our program, speed mods cancel each other out, and jump mods cancel each other out. So in the example above, as soon as the Player triggered the "Speed- 1" PowerUp, they would reset to their default speed - 1, with a new 3 second timer running.

The program works with a queue, like the **ScoreManager**. When _speedToProcess_ is changed, it will figure out the new speed to use, apply that to the VRCPlayerApi of the Local Player, and start a countdown based on the _effectDuration_ of the PowerUp. The program displays the mod on the user's HUD and fades it out along with the timing so they Player can intuitively understand how much time is left. When the timer runs out, it resets the target property on the VRCPlayerApi to the default value, which is why we store and set those here instead of in the "VRCWorldSettings" program.

## DestroyAfterXSeconds
This simple program is useful for locally-instantiated objects, like the FX Prefabs created by **OnPlayerDataEnter** programs. It will ensure that the object destroys itself so you don't wind up with hundreds of old sound effects and particle systems sitting around.

## PlayClipFromArray
This program is useful for introducing some variety in your sounds, for use on FX Prefabs for example. Instead of a single AudioClip, you can set a group of them on this program and it will randomly choose one when it is created and play that one. Could also be useful for a Footsteps program.

# Hazards
If you want to challenge your players, you can add a variety of hazards. We included a couple example programs, feel free to make your own!

## Autorotate
This program simply rotates the Transform on which it lives. You can adjust the _amount_ for each axis, which will be multiplied by Time.deltaTime to ensure it rotates smoothly. An animator would have better performance, but this works when you're experimenting.

## SpawnedHazard
This hazard will reduce the speed of the Player who comes into contact with it. You can set _speedChange_ like you would on a PowerUp - the x is the amount to add to the Players' speed, and the y is the duration of the effect. To reduce a player's speed by 3 for 1 second, you would set _speedChange_ to (-3,1). They find the "PlayerModsManager" GameObject and UdonBehaviour by name when they are created - not very performant but it works.

## HazardSpawner
This program uses **SendCustomEventDelayedSeconds** to spawn hazards every _delay_ seconds. In our example project, we use slightly different delays to make a tricky hill of barrels for our players to dodge.

## FallingBlock
This program is the only one we include that interacts with a player and doesn't use **OnPlayerDataEnter**. This is because we want to know when a Player Enters _and_ when they Exit, which isn't accounted for in that program. When a player enters, we use **SendCustomEventDelayedSeconds** to run **CheckForDrop** after _triggerTime_ seconds.

On **CheckForDrop**, if the player has not yet exited the collider, it will set its Rigidbody to non-kinematic, causing it to fall (and the player along with it). It will then call **Reset** after _resetTime_ seconds.

# Misc

## Injection
This project has a system to inject references to certain components. It is described [here](/worlds/examples/obstacle-course/build-from-custom-parts#advanced-stuff).

---

## ドキュメント: uoc-window.md

```metadata
階層: /worlds/examples/obstacle-course/uoc-window.md
ディレクトリ: worlds\examples\obstacle-course
ファイル名: uoc-window.md
拡張子: .md
サイズ: 3.06 KB
最終更新: 2025-06-05T03:07:52.791Z
```

---
title: "Obstacle Course Toolkit"
slug: "uoc-window"
hidden: false
createdAt: "2021-08-10T19:19:18.669Z"
updatedAt: "2021-08-18T20:56:44.155Z"
---
![Obstacle Course Toolkit](/img/worlds/uoc-window-0a203a2-obstacle-course-toolkit.png)

We created a special window to help manage all the special prefabs and programs in the project. Here's what you can do in each section:

## Checkpoints

### Checkpoint Prefabs
![image](/img/worlds/uoc-window-a05aa7a-checkpoint-prefabs.png)
You can set all the prefabs you would like to use for your start gate, checkpoints and finish gate here. They can then be easily placed into your scene: [Add Checkpoints](/worlds/examples/obstacle-course/build-from-demo-parts#add-checkpoints).

### Checkpoints In Scene
![uoc-window-f588547-checkpoints-in-scene.png](/img/worlds/uoc-window-f588547-checkpoints-in-scene.png)

Select one of these to zoom to the selected checkpoint in Scene View. You can re-order the checkpoints (which will rename them as well) and delete them. Changes made here will properly update the variables on the CourseManager.

## Player Manager

### Player Object Prefab
![uoc-window-b781ee6-playerobjectprefab.png](/img/worlds/uoc-window-b781ee6-playerobjectprefab.png)

Change this object if you make your own Player Object, which holds the PlayerData program and follows each local player around. Most creators won't need to do this.

### Number of Players
![uoc-window-0aa8cd9-number-of-players.png](/img/worlds/uoc-window-0aa8cd9-number-of-players.png)

Change this number to generate the right number of Player Objects and automatically populate the ObjectPool.

## Score Manager

### Score Object Prefab
![uoc-window-c808c71-score-object-prefab.png](/img/worlds/uoc-window-c808c71-score-object-prefab.png)

Change this object if you make your own ScoreField, which will display the player scores as they finish a run.

### Number of Scores to Show
![uoc-window-778d7ed-number-of-scores.png](/img/worlds/uoc-window-778d7ed-number-of-scores.png)

Change this number to generate the right number of Score Fields and populate the ScoreManager references to them.

## Power Ups

### Power Up Prefabs
![uoc-window-d656caf-power-up-prefabs.png](/img/worlds/uoc-window-d656caf-power-up-prefabs.png)

You can set all the prefabs you would like to use for your Power Ups here. They can then be easily placed into your scene (link to powerUp placement) and automatically wired up.

## Power Ups In Scene
![uoc-window-4bf4a4c-power-ups-in-scene.png](/img/worlds/uoc-window-4bf4a4c-power-ups-in-scene.png)

Select one of these to zoom to the selected Power Up in Scene View. You can change the effects they will have on Speed and Jump for the player, as well as how long those effects will last after they touch the pickup.

## Defaults
![uoc-window-f1374e0-defaults.png](/img/worlds/uoc-window-f1374e0-defaults.png)

Here is where you set the Move and Jump speeds instead of on the VRCWorld object. Walk, Run and Strafe are all set to the same speed by default, and they're modified together when the player touches  Power Ups.

---

# ドキュメント終了

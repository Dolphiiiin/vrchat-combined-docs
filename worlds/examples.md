# examples Documentation

This document contains automatically collected documentation with metadata headers for each section.



---

## Document: ai-navigation.md

Path: /worlds/examples/ai-navigation.md
---
description: "NPCs that follow you."
sidebar_custom_props:
    customIcon: 🗺️
---
# AI Navigation Example

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import HowToImportExample from '/docs/worlds/examples/_how-to-import.mdx';

## Example World

[IMAGE: example world]

Udon can use [AI Navigation](/worlds/udon/ai-navigation). This example shows you how it can be used for pathfinding and creating NPC characters that walk around in your world.

Visit the [AI Navigation Example World](https://vrchat.com/home/world/wrld_b7b99484-d92f-403d-ac10-4da7e5a9ce14) to try it for yourself!

## Using the Example

This scene has a collection of NPCs which are trying to get from the red block to the green one. You can move the blue blocks to form a pathway for them. Unity's AI Navigation automatically recalculates the pathing of the NPCs whenever you drop the blocks.

Open the scene in the Unity Editor to test the basic functionality. It should work roughly the same as in VRChat. When you enter play mode, the NPCs will attempt to reach the green block. You can move the blue blocks to help the AI to reach their goal. You can press the "Reset" button to try again.

<HowToImportExample/>

## Technical Breakdown

Let's explore the Udon logic, scene structure and networking that makes this scene work.

### Logic

When the scene initializes, each NPC has the "NPC Finish" block set as their `destination`. They will try to reach it, but fail because there is no direct connection to get there. When each NPCStep is dropped, it will call `NavMeshSurface.BuildNavMesh` to rebuild the main surface. If this results in a better path to the target, each Agent will hop aboard to continue their journey. Every 0.5 seconds, the progress is recalculated by comparing the distance between the first NPC and the target block with the initial distance, which was saved at the start of the scene. If the distance is less than 0.15 units during a check, then particles are set off, and the progress checks are stopped. You can then press the "Reset" button to start it all over.

### Scene Overview

The scene's hierarchy consists of the following:

- "World" contains the scene descriptor, floor, lighting, etc.
- "Course" defines where NPCs can move.
  - The [NavMeshSurface](https://docs.unity3d.com/Packages/com.unity.ai.navigation@1.1/manual/NavMeshSurface.html) component allows Unity to rebuild the navigation logic at runtime.
  - The CourseManager UdonBehaviour component handles the progress bar and resetting it.
  - The "NPC Start" and "NPC Finish" child GameObjects define the NPC's starting and target positions.
  - The three "NPC Step" child GameObjects act as surfaces for the NPCs.
    - Each NPC Step has a NavMeshModifier component, which attempts to add its surface to the main NavMeshSurface.
    - It also has a VRCPickup component and an UdonBehaviour, which locks its rotation to stay flat when carried and applies the logic to rebuild the main navigation surface.
- "NPCs" has four child "NPC" GameObjects, which are Capsules with a [NavMeshAgent](https://docs.unity3d.com/ScriptReference/AI.NavMeshAgent.html) and UdonBehaviour.
  - The NavMeshAgent provides the basic navigation and movement logic, while the UdonBehaviour has the logic we want for the scene.
- "Canvas" contains the process indicator and reset button.

### Networking
Everything that happens should be synced for all users in the instance, through careful choices between which variables and logic to sync and which to run locally.

The Course has a single synced variable - a `progress` float which is checked on the owner of the Course, and synced to all other users whenever it is updated. The other users listen for changes of this variable and update their progress bar when it changes. The Course also uses a Networked Event to call the `Win` event for everyone when `progress` is greater than `successThreshold`, which is set to `0.8` to account for other NPCs getting in the way of the one whose progress we measure.

Each NPC is moved by the `NavMeshAgent` on its Owner's device, and the `VRCObjectSync` component updates its position automatically for everyone else. Only the Owner of the NavMesh continuously rebuilds the actual NavMesh, since the synced NPCs don't need it.

---

## Making Your Own Changes

After making a copy of the scene, try some of these ideas to evolve the scene further, and explore the power of the AI Navigation package:

* Change the course, moving the start and end points further apart.
* Reduce the number of available blocks from 3 to 2.
* Make the target point move over time.
* Give each person in the world their own NPC and NavMesh to turn it into a race!

---

## Document: detect-controller-collide.md

Path: /worlds/examples/detect-controller-collide.md
---
title: Detect Controller Collide
description: "Detect a Character Controller colliding with something."
sidebar_custom_props:
    customIcon: 🧱
---
import HowToImportExample from '/docs/worlds/examples/_how-to-import.mdx';
import UnityVersionedLink from '@site/src/components/UnityVersionedLink.js';

[IMAGE: Detect Controller Collide]

The Unity event <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/ScriptReference/MonoBehaviour.OnControllerColliderHit.html">OnControllerColliderHit</UnityVersionedLink> is useful for knowing when a <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/ScriptReference/CharacterController.html">CharacterController</UnityVersionedLink> has collided with another object. VRChat adds the event [OnControllerColliderHitPlayer](/worlds/udon/players/player-collisions#physics) to detect when a CharacterController collides with another Player. This event includes a `ControllerColliderPlayerHit` struct with a reference to the [VRCPlayerApi](https://udonsharp.docs.vrchat.com/vrchat-api/#vrcplayerapi) object for the player that was collided with.

Visit the [Detect Controller Collide Example World](https://vrchat.com/home/world/wrld_7da557ad-3584-4b0a-bf61-6cbba33701d4) to try it for yourself!

## Using the Example

Start the scene in the Unity Editor or visit the world in VRChat. Look at the canvas in the world, and observe that the empty space below the "Last hit:" label changes to "Wall" when the capsule "character" collides with it. 

Stand in-between the spawn point for the capsule and the wall so that it runs into you on the way to the wall, and you should see the label change to your display name when it collides with you! It will reset to its spawn point and continue running into you until you move aside. At that point, it will collide with the wall again and change the label back to "Wall".

<HowToImportExample/>

## Technical Breakdown

The scene has a `DetectControllerCollide` GameObject which contains the main prefabs and logic for the example.

The first child object, `CharacterController`, has a CharacterController component on it as well as an UdonBehaviour with a Graph Program with the logic for detecting collisions.

### OnCharacterControllerHitExampleGraph

#### Variables

This graph has the following public variables:

| **Name**                           | **Description**                                                                                   |
|------------------------------------|---------------------------------------------------------------------------------------------------|
| `CharacterController`              | Reference to the CharacterController, included to run the `Move()` method during `Update()`.      |
| `float` moveSpeed                  | The speed at which the characterController will move, multiplied internally by `Time.deltaTime`.  |
| `TextMeshProUGUI` hitNameText      | A reference to the target textField to update when a collision is detected.                       |
| `Transform` characterControllerStartPos | The position from which the characterController will start its journey each time.            |

#### Events

The graph has four events:

* `Update()` runs continuously, calling `Move()` on the characterController to move it forward.
* `OnControllerColliderHit()` runs whenever the characterController collides with a non-player object. The name of the object is extracted from the `ControllerColliderHit` struct passed through by the event, and set on the `hitNameText` field.
* `OnControllerColliderHitPlayer()` runs whenever the characterController collides with a Player. The name of the Player is extracted from the `ControllerColliderPlayerHit` struct passed through by the event, and set on the `hitNameText` field.
* `__ResetCharacterController()` is a custom event which is called from the two collision events to reset the characterController back to its starting position.

---

## Document: image-loading.md

Path: /worlds/examples/image-loading.md
---
description: "Loading a remote image."
sidebar_custom_props:
    customIcon: 🖼️
---

# Image Loading Example

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<video loop="loop" autoplay="autoplay" muted>
  <source src="/img/worlds/image-loader.mp4" type="video/mp4" />
    Your browser does not support the video tag.
</video>

Udon can load images from the internet and display them as UI elements or as textures on world objects. Our example world below demonstrates how the system works and how to use GitHub to host the images and captions.

## Example World

[Download the example project](https://github.com/vrchat-community/examples-image-loading/archive/refs/heads/main.zip) or [visit the GitHub repo](https://github.com/vrchat-community/examples-image-loading) to clone it for yourself.

This scene has a picture frame that automatically changes to show different images with matching captions. The images and captions are both hosted for free on GitHub Pages and are included in the GitHub repository above.

Scene File: `Assets/_Project/Gallery`

## Using the Prefab in Your World

To use the prefab, you'll need to add it to your project and set up the image caption URLs.

This repo publishes to [GitHub Pages](https://pages.github.com/) for free hosting. You can host the images and captions anywhere you want, but we recommend using GitHub Pages because it's free, easy to set up, and you can keep the images and captions in the same repository as your world. If you're hosting them elsewhere, skip to step 4.

1. [Fork the example repo](https://github.com/vrchat-community/examples-image-loading/fork) to your own GitHub account.

2. Edit the images and captions in the "Web" directory. You can ignore or delete the `index.html` page, it's just there as an example to test the images and captions in a browser. You can keep the images named 1.jpg, etc to make it easier to use the prefab, or rename them and update the prefab URLs. 

:::tip
When the files in the "Web" directory are edited, the website is re-published. As long as the filenames stay the same (images are 1.jpg, 2.jpg, etc.), the URLs in the world will point to the newly published files. Republishing happens automatically through [an included GitHub Action](https://github.com/vrchat-community/examples-image-loading/actions/workflows/static.yml).
:::
3. Clone the repo to your computer.

4. Make sure your project has SDK 3.2.3 or newer as well as ClientSim and UdonSharp, which you can easily add through the [Creator Companion](https://vcc.docs.vrchat.com/).

5. Import the [Prefab Unitypackage](https://github.com/vrchat-community/examples-image-loading/releases/download/0.2.0/SlideshowFrame.unitypackage) into your project.

6. Drag the **SlideshowFrame** prefab into your scene.

7. Select the **SlideshowFrame** in your scene's Inspector.

8. In the **SlideshowFrame** component, set the **Image Urls** array size to match the number of images you want to load, then update the URLs to match your image URLs. If you're using GitHub Pages, the URLs will be in the format `[IMAGE_URL]`.

9. Update the **String Url** to match your caption URL. If you're using GitHub Pages, the URL will be in the format `https://<your-github-username>.github.io/<your-repo-name>/captions.csv`.

#### Testing it Out

If you're using GitHub to host the images and captions, make sure you've committed and pushed your changes to GitHub, which will trigger the GitHub Action to publish the files to GitHub Pages.

1. Enter Play Mode in Unity.
2. The images and captions should load automatically. If they don't, check the Console for errors.
3. Run a Build and Test to make sure it works in VRChat as well. You may need to turn on "Untrusted URLs" in your settings.

## Important GameObjects

The most important objects to inspect in the scene are [TheFrame](#theframe) and [SlideshowFrame](#slideshowframe). 


[IMAGE: image]

### TheFrame

TheFrame is a GameObject with a couple of important pieces:
* **SlideshowFrame**: an `UdonBehaviour` which loads the images and captions from the web server.
* **Mesh**: Is the black frame around the picture.
* **Picture**: Is a `Mesh` which renders the downloaded textures.
* **UI**: Is a World-Space `Canvas` which renders the captions.

### SlideshowFrame

The **SlideshowFrame** `UdonBehaviour` has all of the logic to download the images and captions from the web server.

[IMAGE: image]

It has these public variables:
* **Image Urls**: An `Array` of all the `VRCUrls` for the images to download.
* **String Url**: Is a single `VRCUrl` where the caption text can be downloaded.
* **Renderer**: This target `Renderer's` **sharedMaterial** will have its texture set from the downloaded textures.
* **Field**: This `UI Element's` **text** property will be set from the downloaded caption for the matching texture.
* **Slide Duration Seconds**: How long to show each image.

The basic logic flow of the script is this:

## Creating an Image Downloader

### Using the `ImageDownload` script to download an image

The SDK includes a script to easily download images:

1. Create a new GameObject in your scene.
2. Add an UdonBehaviour component.
3. Select `ImageDownload` as the program source.
4. Select a Material to apply the downloaded texture to
5. (Optional) Customize `TextureInfo` to change the downloaded texture's settings. 

### Create your own script for `VRCImageDownloader`

You can use `VRCImageDownloader` in your own Udon Graph scripts.

1. Create a new `VRCImageDownloader` object with its Constructor node.
2. Save the newly created `VRCImageDownloader` as a variable. (This **required**.)
3. Execute the `DownloadImage` function on the `VRCImageDownloader` instance.
4. (Optional) Wait for the `OnImageLoadSuccess` or `OnImageLoadError` event to execute.

#### The basic logic flow of the script is:

1. On Start, construct a `VRCImageDownloader` to reuse for downloading all the images. It's important to keep this around so the textures will persist.

```csharp
// It's important to store the VRCImageDownloader as a variable, to stop it from being garbage collected!
_imageDownloader = new VRCImageDownloader();
```

2. Download the captions/strings from the `String Url`.

If the String downloads successfully, split it up line-by-line into separate strings, and save those to a `_captions` array. If it doesn't download, log the error message.


<Tabs groupId="udon-compiler-language">
<TabItem value="graph" label="Udon Graph">

[IMAGE: An example of how to use image loading in the Udon Graph. The Udon Graph can't use newline characters directly, so an integer conversion to a character is used instead.]

</TabItem>
<TabItem value="cs" label="UdonSharp">

```cs
private void Start()
{
    // To receive Image and String loading events, 'this' is casted to the type needed
    _udonEventReceiver = (IUdonEventReceiver)this;
        
    // Captions are downloaded once. On success, OnImageLoadSuccess() will be called.
    VRCStringDownloader.LoadUrl(stringUrl, _udonEventReceiver);
}

public override void OnStringLoadSuccess(IVRCStringDownload result)
{
    _captions = result.Result.Split('\n');
    UpdateCaptionText();
}

public override void OnStringLoadError(IVRCStringDownload result)
{
    Debug.LogError($"Could not load string {result.Error}");
}
```

</TabItem>
</Tabs>

3. Try to Load the next Image. Increment the `_loadedIndex` to keep track of our place, then call `DownloadImage()` on the downloader we saved earlier.

If the Image downloads successfully, save a reference to it and then load it up on the `Renderer`. If it fails, log the error message.

4. Call the function to Load the next Image again, delayed by `SlideDurationSeconds`. The `_loadedIndex` is incremented during each Load call, and starts over after reaching the last url in the array.

When each image is visited for the second+ time, it will be displayed from its saved Texture2D reference instead of loaded fresh, unless it failed to download the first time.

:::tip Source Code
View the full source code for [SlideshowFrame.cs on GitHub](https://github.com/vrchat-community/examples-image-loading/blob/main/Assets/_Project/Frame/SlideshowFrame.cs).
:::

## Known Issues

* The first image doesn't have its caption loaded quickly enough, so it doesn't show until the first loop around.

:::tip Udon

View the [main Image Loading docs page](/worlds/udon/image-loading) for full details on the Image Loading system, including domain and file limits.

:::


---

## Document: index.md

Path: /worlds/examples/index.md
---
title: "Examples"
sidebar_position: 1
hidden: false
createdAt: "2017-07-06T00:13:32.007Z"
updatedAt: "2021-08-10T19:15:56.090Z"
---
# Udon / SDK3

import DocCardList from '@theme/DocCardList';

<DocCardList />

---

## Document: midi-playback.md

Path: /worlds/examples/midi-playback.md
---
title: MIDI Playback
description: "Play synchronized MIDI and audio files."
sidebar_custom_props:
    customIcon: 🎵
---
import HowToImportExample from '/docs/worlds/examples/_how-to-import.mdx';
import UnityVersionedLink from '@site/src/components/UnityVersionedLink.js';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<video controls>
  <source src="[IMAGE_URL]
</video>

This example plays back a [MIDI file](https://midi.org/standard-midi-files) synchronized to an audio file made from the MIDI. You can use this to synchronize events in your world from a MIDI track, to tie music and visuals together for an immersive experience.

Visit the [Midi Playback Example World](https://vrchat.com/home/world/wrld_57799f09-406a-4c8c-9c42-e593cae6305a) to try it for yourself!

## Using the Example

Play the scene in the Unity Editor or visit the world in VRChat to see and hear the MIDI playback. As the short loop plays, differently colored images will flash in time with the music - these are visualizations of the MIDI Note events happening on four different channels in the MIDI file.

<HowToImportExample/>

## Technical Breakdown

The [VRCMidiPlayer](/worlds/udon/midi/midi-playback/#component-vrcmidiplayer) is similar to an <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/ScriptReference/AudioSource.html">AudioSource</UnityVersionedLink> but it uses a Midi Asset instead, and sends [Note On](/worlds/udon/midi#midinoteon) and [Note Off](/worlds/udon/midi#midinoteoff) events to the [MidiGrid UdonBehaviour](#midigrid).

### MidiGrid

This program visualizes Midi Note On and Off events using colored blocks. 

Whenever a Note On event is sent from the VRCMidiPlayer, the program will check if its channel matches one of the grids (see the `channels` field below). If there is a match, then the program chooses the corresponding block to enable by calculating the [remainder](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/language-specification/expressions#12104-remainder-operator) of the note number with 12 to find the corresponding image. For example, note numbers 11 and 12 will trigger the 11th and 12th images in the grid, while numbers 13 and 14 will trigger images 0 and 1, rolling over to fit the note numbers to the grid. Once the target image is calculated, it is enabled.

When a Note Off event is sent from the VRCMidiPlayer, the program makes the same calculations to find the target grid and image, but disables it to make it invisible.

### Inspector Fields

| Name | Description |
|---|---|
| grids | References to <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/ScriptReference/RectTransform.html">RectTransform</UnityVersionedLink> which have a [GridLayoutGroup](https://docs.unity3d.com/Packages/com.unity.ugui@2.0/manual/script-GridLayoutGroup.html) on them, with 12 child [Images](https://docs.unity3d.com/Packages/com.unity.ugui@2.0/manual/script-Image.html) corresponding to the notes in a full octave. |
| channels | Array of integers used to remap MIDI channels to the four image grids in the scene. The default value of `[3, 4, 1, 2]` will show note events from channel 3 on grid 0, events from channel 4 on grid 1, etc. |
| player | Reference to the VRCMidiPlayer sending events to the MidiGrid. |

#### Swapping the Data

If you have MIDI and Audio files, you can import them and replace the existing assets with your own to see how they look in the scene.

#### Changing the Channels

The MidiGrid program has a variable called `channels`. This array matches channels from the MIDI data to grids on-screen. By default, the order is "3 4 1 2". This means that the first grid will show data from channel 3, the second grid will show channel 4, etc. You can switch the order here to change the visuals a little.

If you load your own MIDI data file, you can check Unity's console to see the channels and notes being played. Each Note On event will log a message like "3:75". This shows you that channel 3 played note 75.

#### Adding Grids

If you want to use a song with more than 4 channels, you can duplicate the grids and add them to the `grids` variable on the program. Make sure to add more `channels` as well!

#### The Whole Program, Explained

Here's a breakdown of what happens in the MidiGrid Program.

<Tabs groupId="udon-compiler-language">
<TabItem value="graph" label="Udon Graph">

**Start Event:**

[IMAGE: Start event for the midi playback example in the Udon Graph]

On Start, it goes through each object in the `grids` array, finds the 'Image' component on its child, and sets its `enabled` value to `false`, effectively hiding all the Images to start.

It also waits 1 second after loading and then calls `Play()` on the VRCMidiPlayer to start the music and data flow.

**Note Events:**

[IMAGE: Midi note on event for the midi playback example in the Udon Graph]


When it receives a `Midi Note On` event, it will loop through each entry in the `channels` array and check if the incoming note's channel matches one of the entries. If a match is found, that number is used as the `index` for the `grids` array to find the matching grid. The incoming note is run through `int.Remainder()` to find its index in the octave - a C will be 0, a C# will be 1, etc. This index is used to find the right child of the grid, and then set `enabled` on the 'Image' to `true`. Finally, the note's channel and note number are logged to the console. 

[IMAGE: Midi note off event for the midi playback example in the Udon Graph]

When the script receives a `Midi Note Off` event, it goes through a similar process as above. To hide the 'Image' component again, it sets `enabled` to `false`.

</TabItem>
<TabItem value="cs" label="UdonSharp">

```cs
using UdonSharp;  
using UnityEngine;  
using UnityEngine.UI;  
using VRC.SDK3.Midi;  
  
[UdonBehaviourSyncMode(BehaviourSyncMode.None)]  
public class LogoButton : UdonSharpBehaviour  
{  
    [SerializeField] private Transform[] grids;  
    [SerializeField] private VRCMidiPlayer player;  
    [SerializeField] private int[] channels;  
      
    private void Start()  
    {  
        // Disable Image components of all grid children  
        foreach (var grid in grids)  
        {  
            for (var i = 0; i < grid.childCount; i++)  
            {  
                var child = grid.GetChild(i);  
                var image = child.GetComponent<Image>();  
                image.enabled = false;  
            }  
        }  
  
        SendCustomEventDelayedSeconds(nameof(_PlayAudio), 1);  
    }  
  
    public void _PlayAudio()  
    {  
        player.Play();  
    }  
  
    public override void MidiNoteOn(int channel, int number, int velocity)  
    {  
        UpdateGridState(channel, number, true);  
        Debug.Log($"{channel} : {number}");  
    }  
  
    public override void MidiNoteOff(int channel, int number, int velocity)  
    {  
        UpdateGridState(channel, number, false);  
    }  
  
    private void UpdateGridState(int midiEventChannel, int midiEventNoteNumber, bool isEnabled)  
    {  
        // Find all grids that are mapped to the midi event's channel.  
        for (var gridIndex = 0; gridIndex < grids.Length; gridIndex++)  
        {  
            var gridChannel = channels[gridIndex];  
            if (midiEventChannel != gridChannel) continue;  
              
            // Enable/Disable image, quantized by chromatic 12 note scale.  
            var child = grids[gridIndex].GetChild(midiEventNoteNumber % 12);  
            var image = child.GetComponent<Image>();  
            image.enabled = isEnabled;  
        }  
    }  
}
```

</TabItem>
</Tabs>

---

## Document: minimap.md

Path: /worlds/examples/minimap.md
---
title: Minimap
description: "Displays a live minimap using Graphics.Blit"
sidebar_custom_props:
    customIcon: 📌
---
import HowToImportExample from '/docs/worlds/examples/_how-to-import.mdx';

[IMAGE: Minimap Example World]

This example includes a minimap that you can pickup and take around the world with you. Your position is shown as a blue dot on the map, and other players are shown as green dots.

Visit the [Minimap Example World](https://vrchat.com/home/world/wrld_12492ad5-ff17-445d-9f90-7b14376b1f32) to try it for yourself!

## Using the Example

Grab the minimap from the instruction area and walk around the world. Observe how the blue dot (you) stays centered as the map updates around you. 

Return to the instructions near spawn and press the button labeled "Toggle Visibility of Other Players", to turn it on. If you're testing in ClientSim, use the pause menu to Spawn a Remote Player. If you're testing in VRChat, join the world with an alt account or invite a friend. The remote player should show up on your map in green. You can press the button again to hide the green dot(s).

<HowToImportExample/>

## Technical Breakdown

### Udon Program

The Udon program works in the following way:
  - On Start: A camera, which is a part of a prefab, captures your scene from above. As a world creator - you can place it anywhere in your world.
  - On Update: The position of the local player is taken and passed through to a special shader (`MiniMap Blit`).
  - The shader then overlays a dot at the player's position onto the map capture.
  - If you allow showing other players - you can then use the `_ShowOthersToggle` to toggle dots of other players on and off. Try it out with friends!

### Pickup

The pickup, in turn, is incredibly simple. It simply uses a material with the final output texture `MiniMap RT` assigned as its Main Texture and Emission.

### Other Notes

One of the benefits of `Graphics.Blit` is that you get a regular texture out of it, which you can in turn use for anything you want!

You can also control the update rate by simply calling `Graphics.Blit` more or less often.

Almost all of the parameters you can think of are adjustable via `MiniMap` program, so feel free to experiment!

If you're going to use this Example Prefab in your world, don't forget to adjust `MaxPlayers` variable to match your `World Capacity`.

---

## Document: mute-others.md

Path: /worlds/examples/mute-others.md
---
description: "Mute and unmute other players."
sidebar_custom_props:
    customIcon: 🔇
---
import HowToImportExample from '/docs/worlds/examples/_how-to-import.mdx';

# Mute Others

[IMAGE: Mute Others World Preview]

This example shows how to mute and unmute other players.

Visit the [Mute Others Example World](https://vrchat.com/home/world/wrld_6b2ed4f1-09be-4dee-91b2-fc49e12ecb88) to try it for yourself!

## Using the Example

Open the `mute-others` scene to first test it out in the Unity Editor, or visit [this world](https://vrchat.com/home/world/wrld_6b2ed4f1-09be-4dee-91b2-fc49e12ecb88) in the VRChat Client.
This example requires TextMeshPro, a window will show offering to "Import TMP Essentials" if you don't already have TextMeshPro in your project. Accept this offer and re-open the scene after it's done importing.

Press the button labeled "Mute Players" to mute all the other players who are in the instance with you, and change the button label to "Unmute Players". Press the button again to restore the other players' voices and the original "Mute Players" label.

<HowToImportExample/>

## Technical Breakdown

[IMAGE: Mute Others Inspector]

There is a single U# program on the "PlayerMuteLogic" GameObject which contains all the logic for the demo.

### MuteOthers.cs

This program has a boolean `_areOtherPlayersMuted` variable which is toggled whenever the `_Trigger()` function is called from the MuteButton in the scene.

After flipping the value of this variable, `OnMuteUpdated()` is called, which fetches a list of all the Players in the instance, and then changes their [VoiceDistanceFar](/worlds/udon/players/player-audio#set-voice-distance-far) to effectively mute and unmute them. When this value is set to 0, it means that other player voices will travel 0 meters before they are silent. 

When unmuted, the value is restored to `_defaultVoiceDistance` which is 25 unless you change it in the inspector, so their voices can be heard for the default distance of 25 meters.
This function also changes the text on the button, which is referenced as `_buttonLabel`.

[IMAGE: Inspectors for changing button text]

The messages to display for each state are easily changeable in the inspector, as `Message Muted` and `Message Unmuted`.

#### Variables
* Button Label - a reference to the label of the button to change its message, should be already set properly.
* Message Muted - the text shown on the button while other players are muted, default is "Unmute Players"
* Message Unmuted - the text shown on the button while other players are unmuted, default is "Mute Players"
* Default Voice Distance - the value that will be set for all player voice distances when they are unmuted, default is 25, which is the platform-wide default.

---

## Challenge

Can you update this prefab to only mute some players? One approach would be to do this based on where players are, for a jump-start on that you can check out the [PlayerJoinZones](/worlds/examples/player-join-zones) example, which creates a collection of players based on whether they are in a Trigger area.

---

## Document: player-join-zones.md

Path: /worlds/examples/player-join-zones.md
---
description: "Basic lobby functionality."
sidebar_custom_props:
    customIcon: 🚪
---
import HowToImportExample from '/docs/worlds/examples/_how-to-import.mdx';

# Player Join Zones

[IMAGE: Player Join Zones World Preview]

This example shows how to collect players based on their position, and handle basic lobby functionality, enabling users to join a game, play it, see results, then start a new game. It can be used to build opt-in experiences, like a game played by only some of the players in the instance. It also shows how to randomly choose a player to make larger, and extend the prefab’s logic with additional modes.

Visit the [Player Join Zones Example World](https://vrchat.com/home/world/wrld_12492ad5-ff17-445d-9f90-7b14376b1f32) to try it for yourself!

## Using the Example

These examples work with one player, but benefit from testing with two or more.
Open the `player-join-zones` scene to first test it out in the Unity Editor, or visit the example world linked above in the VRChat Client.
This example requires TextMeshPro, a window will offer to "Import TMP Essentials" if you don't already have TextMeshPro in your project. Accept this offer and re-open the scene after it's done importing.

### Player Join Zone:
1. First, notice that the button at the bottom of the canvas reads "Players Needed", and is not interactable. 
2. Walk into the highlighted area on the floor and see your displayName appear on the board in front of it. 
3. Your name should appear and disappear as you walk in and out of the zone. 
4. Press the "Start Game" button to lock-in the list of players, it will no longer change as players enter and exit.
5. Press the button that now reads "Reset", and the list of players clears to make a new one.

### Boss Picker:
1. Walk into the highlighted area on the floor and see your displayName appear on the board in front of it under the label "Possible Bosses:". 
2. Your name should appear and disappear as you walk in and out of the zone. 
3. Press the "Start Game" button to randomly choose one Player to make large, they are now the "Boss". Notice that the label reads "Boss:" instead of "Possible Bosses:".
4. Press the button that now reads "Reset", the Boss should shrink to their original size and the list of players clears to make a new one.

<HowToImportExample/>

## Technical Breakdown

This section explains the base program and its two extensions - `JoinZoneWithDisplay` and `BossPicker`.

The `PlayerJoinZone` program is a base class for managing Players within a specific zone. It handles the following:
* **Modes**: It defines three modes - `MODE_JOIN` accepts changes to the player list, `MODE_GAME` freezes the player list for gameplay, and `MODE_END` offers a place to show end-of-game information. This mode is synced to all players, and uses a [FieldChangeCallback](https://udonsharp.docs.vrchat.com/udonsharp/#fieldchangecallback) to call additional functionality when the mode is changed.
* **Player Tracking**: It uses three events to track players, adding them to a [DataList](/worlds/udon/data-containers/data-lists) called `Players`.
    * `OnPlayerTriggerStay` detects players entering the zone, as well as players who were in the zone when the mode was changed.
    * `OnPlayerTriggerExit` detects players leaving the zone.
    * `OnPlayerLeft` detects players leaving the instance, who may otherwise get 'stuck' in the list.
* **Interaction**: It has a method `_ToggleMode` designed to be triggered from a UI Button. This method will toggle between the three modes when anyone presses it.

Example Interaction:
1. The zone sets the mode to `MODE_JOIN`, which triggers the Owner of the GameObject with this program to run `ResetPlayers()`.
    1. `ResetPlayers()` creates a new `Players` Datalist on the Owner.
2. A player "Dingbat" enters the zone's collider, triggering the `OnPlayerTriggerStay` event for everyone in the instance.
    1. The Owner checks whether Dingbat is already in the list, everyone else ignores the trigger. Dingbat is not found in the list, so the Owner adds them.
    2. Dingbat will continue to trigger this event as they stay in the zone, but no further action will be taken since they are already in the Datalist.
3. A player "SquirrelFam" enters the zone's collider, triggering the above action again, so there are now two players in the `Players` Datalist.
4. Dingbat decides to play a different game and leaves the instance.
5. The zone receives the `OnPlayerLeft` event for Dingbat, and removes them from the `Players` Datalist, now only SquirrelFam remains.
6. SquirrelFam exits the zone, triggering the `OnPlayerTriggerExit` event, which the Owner will respond to by removing them from the Datalist. There are no more players in the Datalist.

### Extending the Class

In the interaction described above, the `Players` list was created and updated several times, but no information was shown to any users in the instance. This is because the base class contains logic which is useful for many scenarios, but is incomplete on its own. We include two extensions of this base class to demonstrate how you can add functionality.

#### JoinZoneWithDisplay

This extension connects some UI fields and a button to the core logic to make it all usable. The base class doesn't contain any references to UI items so that it can be more easily reused and extended in your own projects.

It includes these UI objects:
* _playerNamesField: A Textfield to display the names of all players currently in the Zone.
* _buttonLabelField: The Textfield within the Button is used to toggle the current `Mode`, which is updated whenever the Mode changes.
* _toggleButton: A UI Button to trigger the `_ToggleMode` method.
* _ownerField: A Textfield which shows the owner of this GameObject for debugging.

This class adds a new string `PlayerNamesString`, which is a line of text containing all the current player names with commas between them. This is constructed on the Owner and then Synced to all other players and uses a FieldChangeCallback to update the `_playerNamesField`. It also adds a new mode called `MODE_WAIT` which freezes the UI Button for 3 seconds to keep the 'game' from ending too quickly. The duration can be set in the Inspector field for `_waitDuration`.

Revisiting the Example Interaction for the `PlayerJoinZone` program above, here is what would happen differently when using the `JoinZoneWithDisplay` program:
1. After creating the `Players` Datalist, the method `OnPlayersChanged()` is called. This method is empty in the base class, but in our extension it will set the synced string `PlayerNamesString` by running the method `GetPlayersAsStringList()` from the base class. 
    1. All the players in the instance receive this updated string, and set the text in their `_playerNamesField` from the value.
    1. Each player will also call `SetupButtonFromPlayers()`, which will update the text of their `_toggleButton` to show "Players Needed" if no one is in the zone.
2. After the Owner adds Dingbat to the `Players` Datalist, the method `OnPlayersChanged()` is triggered again, which will propagate the same changes above, updating the synced `PlayerNamesString` value and triggering updates the text field and button label if needed.

Every change the Owner makes to the `Players` list is followed by a call to `OnPlayersChanged()` to update the data and display it for everyone else in the instance.

This example also has a Button to toggle the mode like this:
1. Any player presses the Button, which has been linked to `UdonBehavior.SendCustomEvent(_ToggleMode)`. If they are not the owner, then the event `ToggleModeRPC()` will be sent to the Owner. If they are the owner, then they will call `ToggleModeRPC()` themselves. Either way, that method runs and flips switches to the next `Mode`.
2. `Mode` is a synced variable with a FieldChangeCallback, so its value will be updated for everyone in the instance, and the function `OnModeChanged()` will be run locally for each player.
3. This method is mostly empty in the base class (it only contains logic to propagate its logic to external listeners), but in this example, it will set the label of the `_toggleButton` to "Reset" if the mode is now `MODE_CHOSEN`.

All the text set by this class is defined in a few strings near the top of the class for easy updating.

#### BossPicker

This extension has some of the same fields as [JoinZoneWithDisplay](#joinzonewithdisplay), adds logic to pick one player from the chosen group and apply some scaling to them, which is useful if you're making a game where one character should be bigger than the others. It also adds another Mode called `MODE_GAMEOVER` for reviewing scores.

It includes a synced `PlayerNamesString` field and uses the same logic to create the list, sync it to all players in the instance, and update the text field. 

When the `Mode` is changed to `MODE_CHOSEN`, the Owner chooses one Player from the Datalist at random, and saves their PlayerId as `_bossPlayerId`, which is a new synced field with a FieldChangeCallback that triggers `_OnBossChanged()`.
In `OnBossChanged()`, each player will check if they are the Boss. If they are, then they will set their AvatarEyeHeight to the maximum possible size (currently 5 units). All other players will only have their height changed if it is bigger than `_maxPlayerHeight`, which can be adjusted in the program's Inspector.
The `ToggleModeRPC()` method has been overridden in this class in order to handle the new Mode. Now, `MODE_CHOSEN` will start a timer to automatically transition to `MODE_END` after 5 seconds, or whatever value you've set for `gameDuration` in the Inspector. This method also disables the button while in the `MODE_CHOSEN` state so the game can't be ended early. Finally, it has logic to transition from `MODE_END` to `MODE_JOIN` when the UI button is pressed.
In `OnModeChanged()`, each player resets to their original height when the mode changes back to `MODE_JOIN`, which would typically happen after a game finishes and a new round is opened up.

### Integration with Udon Graph & Existing Programs

Each of these programs has a `targets` field, which is an array of UdonBehaviours. You can write / modify Udon Programs to include some special variables which will be kept up-to-date. They are:
* `int` Mode - updated during `OnModeChanged()` in all programs
* `Datalist` Players - updated during `OnPlayersChanged()` in all programs
* `int` BossPlayerId - updated during `OnBossChanged()` in the BossPicker program

The "Graph Listener" canvas demonstrates this, updating a display with the latest events and updates from both programs. Its Udon Graph program simply implements all three public variables, and used `OnVariableChanged` nodes to react to their changes, writing the results to a text field.

---

## Document: screen-canvas.md

Path: /worlds/examples/screen-canvas.md
---
description: "2D UI canvas for Mobile and Desktop users."
sidebar_custom_props:
    customIcon: 📲
---
import HowToImportExample from '/docs/worlds/examples/_how-to-import.mdx';

# Screen Canvas

[IMAGE: Screen Canvas World Preview]

This example shows how to create a Screen-Space UI Canvas for users on 2D screens like Mobile and non-VR Desktop. It also demonstrates how to teleport the user to a destination position and rotation.

Visit the [Screen Canvas Example World](https://vrchat.com/home/world/wrld_1448021c-b126-4cb9-ac92-1ab660884b02) to try it for yourself!

## Using the Example

Open the `screen-canvas` scene to first test it out in the Unity Editor, or visit [this world](https://vrchat.com/home/world/wrld_1448021c-b126-4cb9-ac92-1ab660884b02) in the VRChat Client.
This example requires TextMeshPro, a window will show offering to "Import TMP Essentials" if you don't already have TextMeshPro in your project. Accept this offer and re-open the scene after it's done importing.

When entering the world with a non-VR display like a Mobile Device or a Windows PC with a monitor, a thick white outline will be visible around the edges of the display, as well as a button that reads "Teleport". Press this button - on mobile, a simple screen-tap will activate it. On Desktop, hold the Tab key to free your cursor from the center of the screen and press it. Either way, you are teleported to a second spot in the world, in front of another canvas which reads "Second Location / Here you are!".

[IMAGE: Second Location Screenshot]

When you visit the world using a VR display, the canvas is hidden, rather than being stuck to your face with no way to use the button.

### Extras

[IMAGE: VRCButtonLayout in Game, Hierarchy and Inspector]

The `VRCButtonLayout` object is a helpful tool for showing where different buttons in the VRChat Mobile UI will appear so you can try to work around them. It is tagged `EditorOnly` and will not be uploaded with your world.

<HowToImportExample/>

## Technical Breakdown

This section explains the two Graph Programs which provide the above functionality. You can find both programs on the "ScreenCanvas" object in the scene.

### HideInVR

[IMAGE: Graph Program to Hide Object in VR]

This program runs on `Start`, and deactivates the ScreenCanvas and its UdonPrograms by checking the value of `IsUserInVR` for the local player, and calling `GameObject.SetActive(false)` if they are. If they are not in VR, no further action is needed.
This program is provided separately from the teleportation functionality below to make it easy to add to a variety of objects.

### TeleportToTarget

[IMAGE: Graph Program to Teleport Local Player]

This program is triggered by the Button, which targets its `_Trigger` custom event. It has a public `target` Transform variable. When triggered, it calls `Transform.GetPositionAndRotation()` on the `target`, then passes the position and rotation to `VRCPlayerApi.TeleportTo(position,rotation)` to move the player.
You can change the value of the target to update the location, or move the `TeleportTarget` transform. Note that the transform has a child Sprite to show the spot on the ground where the player will be teleported, as a convenience. This Sprite is a child of the main transform because it requires a rotation that we do not want to apply to the player when they teleport.

## Known Issue

[IMAGE: Scene view of large canvas]

In ClientSim, the collider generated for the ScreenCanvas is always placed at (0,0,0) and covers the whole size of the canvas. In the VRChat Client, the existing collider is used and appears in the proper position. In order to make the scene usable in ClientSim, the initial spawn location is offset to place the user in front of the generated Collider.

---

## Document: udon.md

Path: /worlds/examples/udon.md
---
sidebar_position: -1
description: "Very simple Udon programs in Udon Graph and UdonSharp."
sidebar_custom_props:
    customIcon: 🐣
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Udon Basics

This page contains examples of how to use [Udon](/worlds/udon/). All examples can be viewed as an [Udon Graph](/worlds/udon/graph) or in UdonSharp.

## Rotating cube

This behaviour rotates a game object (such as a cube) by 90 degrees every second on its local Y-axis.

<Tabs groupId="udon-compiler-language">
<TabItem value="graph" label="Udon Graph">

[IMAGE: An Udon Graph for a spinning cube.]

</TabItem>
<TabItem value="cs" label="UdonSharp">

```cs showLineNumbers
using UnityEngine;
using VRC.SDKBase;

public class RotatingCubeBehaviour : UdonSharpBehaviour
{
    private void Update()
    {
        transform.Rotate(Vector3.up, 90f * Time.deltaTime);
    }
}
```

</TabItem>
</Tabs>

## Interact

This behaviour uses [Interact](/worlds/udon/graph/event-nodes/#interact) to allow players interact with a object to disable it. This can be used for a message or a door that disappears when players click on it, for example. The game object must have a collider component to allow players to interact with it.

<Tabs groupId="udon-compiler-language">
<TabItem value="graph" label="Udon Graph">

[IMAGE: An Udon Graph that disables an object when interacting with it. It has several comments. For example, it demonstrates that leaving "instance" empty in the "Get gameObject" node causes it to retrieve to game object that this script is attached to.]

</TabItem>
<TabItem value="cs" label="UdonSharp">

```cs showLineNumbers
using UnityEngine;
using VRC.SDKBase;

public class ClickMe: UdonSharpBehaviour
{
    public override void Interact()
    {
        gameObject.SetActive(false);
    }
}
```

</TabItem>
</Tabs>

## Teleport player

This behaviour uses [Interact](/worlds/udon/graph/event-nodes/#interact) and [TeleportTo](/worlds/udon/players/player-positions/#teleportto) to teleport the player. The `targetPositon` transform determines the player's destination position and rotation after teleporting. Don't forget to add a collider component to the targetPosition GameObject.


<Tabs groupId="udon-compiler-language">
<TabItem value="graph" label="Udon Graph">

[IMAGE: An Udon Graph program that teleports the player to a target position. Comment points out that "targetPosition" must be defined as "public" to allow setting it in the inspector for instances of this script.]

</TabItem>
<TabItem value="cs" label="UdonSharp">
```cs showLineNumbers
using UnityEngine;
using VRC.SDKBase;

public class TeleportPlayer : UdonSharpBehaviour
{
    public Transform targetPosition;
    
    public override void Interact()
    {
        Networking.LocalPlayer.TeleportTo(
            targetPosition.position,   
            targetPosition.rotation);
    }
}
```
</TabItem>
</Tabs>

## Sending events

This behaviour shows how to interact with other behaviours. UdonBehaviours can communicate with each other through variables and custom events.

<Tabs groupId="udon-compiler-language">
<TabItem value="graph" label="Udon Graph">

[IMAGE: An Udon Graph that communicates with other graph programs. Public variables and custom event names from other scripts are specified as constant strings and fed into the "symbolName" parameter of various nodes.]

</TabItem>
<TabItem value="cs" label="UdonSharp">
```cs showLineNumbers
using UdonSharp;  
using UnityEngine;  
using VRC.Udon.Common.Interfaces;  
  
public class SomeExample : UdonSharpBehaviour  
{  
    [SerializeField] private SomeOtherExample otherBehaviour;  
  
    void Start()  
    {  
        if(otherBehaviour.somePublicBoolean)  
        {  
            otherBehaviour.SomeCustomEvent();  
        }  
    }  
      
    public override void Interact()  
    {  
        DoStuff();  
    }  
  
    private void DoStuff()  
    {  
        SendCustomNetworkEvent(NetworkEventTarget.All, nameof(DoNetworkEventStuff));  
    }  
  
    public void DoNetworkEventStuff()  
    {  
        otherBehaviour.somePublicBoolean = false;  
        otherBehaviour.SomeCustomEvent();  
        otherBehaviour.SendCustomNetworkEvent(NetworkEventTarget.Owner, nameof(DoOwnerStuff));  
    }  
}
```
</TabItem>
</Tabs>


---

## Document: _how-to-import.mdx

Path: /worlds/examples/_how-to-import.mdx
## Importing the Example
Follow the steps below to add this example to your Unity project:
1. Open [the Example Central Window](https://creators.vrchat.com/sdk/example-central) from the window from the Unity Editor Menu under "VRChat SDK > 🏠 Example Central"
3. Find this prefab in the list or search for it by title (same as the title of this page).
4. Press the "Import" button to import the Unitypackage into your project.

---

# End of Documentation

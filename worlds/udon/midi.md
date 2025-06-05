# midi 統合ドキュメント

以下は自動収集された全ドキュメントのコンテンツです。各セクションの始めにメタデータが記載されています。



---

## ドキュメント: index.md

```metadata
階層: /worlds/udon/midi/index.md
ディレクトリ: worlds\udon\midi
ファイル名: index.md
拡張子: .md
サイズ: 2.01 KB
最終更新: 2025-06-05T03:07:52.814Z
```

# Midi in Udon

Since the 1980s, MIDI has connected musical instruments in imaginative ways. We've included it in VRChat so you can build worlds that respond to real-time instruments and prerecorded performances. 

>Read more about [MIDI on Wikipedia](https://en.wikipedia.org/wiki/MIDI).

![image](/img/worlds/index-215557268-2d85f551-8fff-4990-a95a-c8a2d412d6a2.png)

There are two ways to work with MIDI in your Udon worlds:
- ## [Realtime](realtime-midi) data from an instrument plugged into your computer.
- ## [Playback](midi-playback) of MIDI data along with audio files.

Either way, you'll be working with Udon's MIDI Events, detailed below.

## Midi Events

### MidiNoteOn
Triggered when a 'Note On' message is received. Either triggered by MIDI playback, or by pressing a key/button on your device.
Outputs:
* `int channel` MIDI Channel that received the event, 0-15.
* `int number` Note number from 0-127 (your MIDI Device may not output the full range)
* `int velocity` Number from 0-127 representing the speed at which the note was triggered, if supported by your MIDI device.

### MidiNoteOff
Triggered when a 'Note Off' message is received, typically by releasing a key/button on your device.
Outputs:
* `int channel` Midi Channel that received the event, 0-15.
* `int number` Note number from 0-127 (your midi Device may not output the full range)
* `int velocity` This value is typically 0 for Note Off events, but may vary depending on your device.

### MidiControlChange
Triggered when a control change is received. These are typically sent by knobs and sliders on your Midi device.
Outputs:
* `int channel` Midi Channel that received the event, 0-15.
* `int number` Control number from 0-127.
* `int value` Number from 0-127 representing the value sent by your controller. For some knobs that can spin endlessly rather than being limited by physical start/end positions, this value might be simply 0 and 1 or some other range indicating "positive" and "negative" increments that you must manage on your own.

---

## ドキュメント: midi-playback.md

```metadata
階層: /worlds/udon/midi/midi-playback.md
ディレクトリ: worlds\udon\midi
ファイル名: midi-playback.md
拡張子: .md
サイズ: 6.02 KB
最終更新: 2025-06-05T03:07:52.815Z
```

import UnityVersionedLink from '@site/src/components/UnityVersionedLink.js';

# Midi Playback

You can play back MIDI data along with an audio track to control anything you want in your Udon world. You can jump to the [Example Scene](#example-midiplaybackscene) to get started right away.

## Assets: MidiFile and AudioClip

Files with the extension .mid are processed as MIDI assets. To get started with your own MIDI and Audio files:
1. Drag and drop them somewhere into your Assets folder. The MIDI file must have the extension .mid, the audio file can be of any <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/class-AudioClip.html">type supported by Unity</UnityVersionedLink> (.aif, .wav, .mp3, .ogg).
2. Select the MIDI file and set its AudioClip to the matching audio file.

![image](/img/worlds/midi-playback-214464414-32af9c18-c003-49ed-bd12-dd431367db56.png)

3. It's imperative that the BPM for your MIDI file is set correctly. If the data seems like it doesn't match the audio, this is likely the issue. You can override the BPM here by toggling on "Override Bpm" and supplying the right value. Even better would be to edit your MIDI file and add the correct BPM.


## Component: VRCMidiPlayer

![VRCMidiPlayer](/img/worlds/midi-playback-215556799-a546e119-afdb-441f-8019-70ee50b6c008.png)

This is the brains of the operation. It works similarly to an Audio Source but uses a Midi Asset instead. It sends MIDI [Note On](/worlds/udon/midi#midinoteon) and [Note Off](/worlds/udon/midi#midinoteoff) events to all target UdonBehaviours.

### Inspector Fields

Midi File

| Name                | Description                                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------|
| Midi File           | The MIDI file in SMF format whose data you want to trigger.                                                           |
| Audio Source        | The AudioSource component with the audio clip corresponding to your MIDI data.                                        |
| Target Behaviours   | An array of UdonBehaviours which will have MIDI [Note On](/worlds/udon/midi#midinoteon) and [Note Off](/worlds/udon/midi#midinoteoff) events sent to them. |
| Display Debug Blocks| When enabled, you can see a display of all the notes in your current MIDI file in the Scene View of the Unity Editor while the VRCMidiPlayer is selected. Helpful for a quick view into your data. |

### Methods

### Methods

| Name   | Description                                                                 |
|--------|-----------------------------------------------------------------------------|
| Play() | Starts the playback of MIDI events and the Audio Source.                    |
| Stop() | Stops the playback of MIDI events and the Audio Source.                     |

### Properties

| Name       | Description                                                                 |
|------------|-----------------------------------------------------------------------------|
| `float` time | Set and Get the current time of both the MIDI and Audio sources.           |
| `MidiData` midiData | Get a MidiData object containing all data about the current MIDI track. Can be used before playback to set things up. |

### Example: MidiPlaybackScene

<video controls>
  <source src="https://user-images.githubusercontent.com/737888/214626843-53a4c069-ea69-423a-926d-e2ce024c9819.mp4"/>
</video>

Example Central includes [a simple MIDI playback example](/worlds/examples/midi-playback). You can load it from the menu bar under VRChat SDK > Samples > MidiPlayback.

## Class: MidiData

When requesting MIDI data from a VRCMidiPlayer, this is what you get. It holds an array of all tracks as well as the BPM.

| Type         | Name         | Description                                             |
|--------------|--------------|---------------------------------------------------------|
| `MidiTrack[]`| Tracks       | Array of MidiTracks in the file.                        |
| `byte`       | Bpm          | Represents the BPM of the track.                        |

## Class: MidiTrack

This class simply wraps an array of MidiBlocks, and provides some handy references for note and velocity ranges discovered in the track.

| Type         | Name         | Description                                             |
|--------------|--------------|---------------------------------------------------------|
| `MidiBlock[]`| Blocks       | Array of MidiBlocks in the track.                       |
| `byte`       | minNote      | The lowest note played in the track.                    |
| `byte`       | maxNote      | The highest note played in the track.                   |
| `byte`       | minVelocity  | The lowest velocity played in the track (besides 0).    |
| `byte`       | maxVelocity  | The highest velocity played in the track.               |

## Class: MidiBlock

A MidiBlock represents a whole Midi Note event from On to Off, and some helpful metadata.

| Type   | Name        | Description                                              |
|--------|-------------|----------------------------------------------------------|
| `byte` | Note        | 0-127 range number for the note played.                  |
| `byte` | Velocity    | 0-127 range number for the velocity of the note played.  |
| `byte` | Channel     | 1-16 range number for the channel on which the note is played. |
| `float`| StartTimeMs | The start time in Milliseconds at which the Note On event starts. |
| `float`| EndTimeMs   | The end time in Milliseconds at which the Note Off event triggers. |
| `float`| StartTimeSec| The start time in Seconds at which the Note On event starts. |
| `float`| EndTimeSec  | The end time in Seconds at which the Note Off event triggers. |
| `float`| LengthSec   | The length in Seconds of the whole event from Note on to Note Off. |


---

## ドキュメント: realtime-midi.md

```metadata
階層: /worlds/udon/midi/realtime-midi.md
ディレクトリ: worlds\udon\midi
ファイル名: realtime-midi.md
拡張子: .md
サイズ: 2.42 KB
最終更新: 2025-06-05T03:07:52.815Z
```

# Realtime Midi

You can use MIDI devices to control your Udon world in realtime using MIDI Notes and controller changes.

## Components

To get started with Midi in your scene, add a **VRC Midi Listener** component to one of your GameObjects.

![VRCMidiListener](/img/worlds/realtime-midi-215557542-bf65a6ef-47d0-4e2f-8d39-337847db461c.png)

This component informs VRChat that you want to receive MIDI events and starts up the MIDI system if needed. **You need to select the events you want to receive** by pressing the 'Active Events' toggles to select them - no events are selected by default, so turn them on before you start testing. **You also need to choose an UdonBehaviour** that will receive these events by selecting it as the 'Behaviour' on the VRC Midi Listener. This UdonBehaviour can be on the same GameObject as the MIDI Listener, or any other object.

When you start your scene, you may notice a **VRCMidiHandler** GameObject that is added automatically. This handles the MIDI device driver and sends events to all the Listeners. DO NOT add this component anywhere yourself - it is meant to be automatically added and removed so that MIDI Devices are only connected to once, and disconnected when someone leaves your world.

## Device Selection - Editor

You can test your MIDI events in the Unity Editor by selecting your device through the VRChat SDK. It is saved in your Editor preferences, so Unity will remember your device for every project.

![Midi Utility Window](/img/worlds/realtime-midi-215557576-5414eb63-a857-4334-8a8c-05f3b6436773.png)

![Midi Utility Selector](/img/worlds/realtime-midi-215557616-8cc3fd99-0fe4-4564-9413-cc805708cf89.png)

## Device Selection - Runtime

When you visit a world with MIDI events, VRChat will try to open the first MIDI device it can find on your machine. If you have multiple devices and want to specify which one to use, you can pass part of its name as a command-line argument. For example, if you have a device that appears in Windows as "SchneebleCo MidiKeySmasher 89", you can add this to your launch options/script:
``--midi=midikeysmasher`
VRChat will match partial names and ignore capitalization. Learn about all other Launch Options: [Launch Options](https://docs.vrchat.com/docs/launch-options)

## Example Scene

Visit the  [Udon Midi Test](https://vrchat.com/home/world/wrld_f8bc6485-dcdf-4646-89d8-14e4772561ee) example world that reads and displays all three message types.

---

# ドキュメント終了

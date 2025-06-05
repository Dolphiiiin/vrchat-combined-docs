# udonsharp 統合ドキュメント

以下は自動収集された全ドキュメントのコンテンツです。各セクションの始めにメタデータが記載されています。



---

## ドキュメント: attributes.md

```metadata
階層: /worlds/udon/udonsharp/attributes.md
ディレクトリ: worlds\udon\udonsharp
ファイル名: attributes.md
拡張子: .md
サイズ: 3.85 KB
最終更新: 2025-06-05T03:07:52.826Z
```

# Attributes

Attributes extend the functionality of your UdonSharp classes, fields, and methods. UdonSharp adds new attributes, but also supports many existing C# and Unity attributes.

:::tip

For a helpful overview of UdonSharp attributes, read [Varneon's UdonSharp Development Practices](https://vrclibrary.com/wiki/books/varneons-udonsharp-development-practices/page/attributes). 
:::

## UdonSharp Attributes

The following attributes are to unique to UdonSharp and the VRChat SDK.

### UdonSynced
`[UdonSynced]` / `[UdonSynced(UdonSyncMode)]`

Fields with this attribute are synchronized over the network for all players in the instance. Read VRChat's [networking documentation](/worlds/udon/networking/) to learn more.

**Example**
```cs
public class Example : UdonSharpBehaviour 
{
    [UdonSynced]
    public bool synchronizedBoolean;

    [UdonSynced(UdonSyncMode.Linear)]
    // This float will be linearly interpolated
    public float synchronizedFloat;
}
```
### UdonSyncMode
`UdonSharp.UdonSyncMode`

By default, `[UdonSynced]` fields are not interpolated. When the field changes, it changes instantly. You can change the sync mode to smoothly interpolate between the old and new value.

| Name        | Summary                                   |
| ----------- | ----------------------------------------- |
| None        | No interpolation (Default)                |
| Linear      | Linear interpolation                      |
| Smooth      | Smooth interpolation                      |
| *NotSynced* | *(Default when not using `[UdonSynced]`)* |
### UdonBehaviourSyncMode
`[UdonBehaviourSyncMode]` / `[UdonBehaviourSyncMode(BehaviourSyncMode)]`

This attribute allows you to choose a [sync mode](#behavioursyncmode) that is applied to all instances of this UdonBehaviour. If you add this attribute, UdonSharp will hide the dropdown for choosing a sync mode in Unity. It also enabled additional validations to ensure that your synced variables are suitable for the chosen sync mode.

**Example**
```cs
[UdonBehaviourSyncMode(BehaviourSyncMode.Manual)]
public class Example : UdonSharpBehaviour 
{
  // This class's sync mode is manual.
}
```

### BehaviourSyncMode
`UdonSharp.BehaviourSyncMode`

Use this in conjunction with [UdonBehaviourSyncMode](#udonbehavioursyncmode) to choose a sync mode for your behaviour.

| Name           | Summary                                                                                                                                                                                              |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Any            | Nothing is enforced. You can set any sync type in Unity. (Default)                                                                                                                                   |
| Continuous     | Synced variables will be updated automatically at a very frequent rate, but may not always reliably update to save bandwidth.                                                                        |
| Manual         | Synced variables are updated manually by the user less frequently, but ensures that updates are reliable when requested.                                                                             |
| None           | Enforces no synced variables on the behaviour and hides the selection dropdown in the UI for the sync mode. Nothing is synced and SendCustomNetworkEvent/NetworkCalling will not work on the behaviour.             |
| NoVariableSync | Enforces that there are no synced variables on the behaviour, hides the sync mode selection dropdown, and allows you to use the behaviours on GameObjects that use either Manual or Continuous sync. |


---

## ドキュメント: class-exposure-tree.md

```metadata
階層: /worlds/udon/udonsharp/class-exposure-tree.md
ディレクトリ: worlds\udon\udonsharp
ファイル名: class-exposure-tree.md
拡張子: .md
サイズ: 583 B
最終更新: 2025-06-05T03:07:52.827Z
```

# Class Exposure Tree

The Class Exposure Tree shows you which classes and methods are available in UdonSharp.

You can open the window by going to "VRChat SDK" > "Udon Sharp" > "Class Exposure Tree."

- Red = Not exposed to Udon
- Green = Exposed to Udon

![Udon Type Exposure Tree](/img/worlds/udon/udonsharp/type-exposure-tree.png)

The "Show base members" toggle shows methods inherited from base classes that are exposed. For example, any class inheriting from `UnityEngine.Component` will show `GetComponent<T>()` functions because they're defined in the base class.

---

## ドキュメント: configuration.md

```metadata
階層: /worlds/udon/udonsharp/configuration.md
ディレクトリ: worlds\udon\udonsharp
ファイル名: configuration.md
拡張子: .md
サイズ: 1.51 KB
最終更新: 2025-06-05T03:07:52.827Z
```

# Configuration

UdonSharp's settings can be found at `Edit > Project Settings > Udon Sharp`.

![UdonSharp Settings window](/img/worlds/udon/udonsharp/udon-sharp-settings.png)

## Udon Sharp

### Auto compile on modify
Having this enabled will automatically compile scripts when they are modified and saved.

### Compile all script
Compiles all scripts whenever changes are detected to a U# script.

### Compile on focus
Will only compile when the editor gets focused and changes have been made to a script.

### Script template override
You can define your own custom template to be used when creating U# scripts.
This can be done by dragging a script into the `Script template override` field and that will now be used when you create a new U# script.

`<TemplateClassName>` can be used to set the class name based on the file name you give.

**Default Template**
```cs
using UdonSharp;
using UnityEngine;
using VRC.SDKBase;
using VRC.Udon;

public class <TemplateClassName> : UdonSharpBehaviour
{
    void Start()
    {
        
    }
}
```

## Debugging

### Debug build
Enables or disabled `Inline Code` and `Listen for client exceptions`

### Inline Code
Includes the C# inline code in the generated assembly code.

### Listen for client exceptions
This will listen for exceptions from the output log the VRChat client makes, then try to match it up against scripts in the project.

[Read more here on how to set it up](https://github.com/vrchat-community/UdonSharp/wiki/class-exposure-tree)

---

## ドキュメント: editorscripting.md

```metadata
階層: /worlds/udon/udonsharp/editorscripting.md
ディレクトリ: worlds\udon\udonsharp
ファイル名: editorscripting.md
拡張子: .md
サイズ: 12.90 KB
最終更新: 2025-06-05T03:07:52.827Z
```

# Editor Scripting

UdonSharp includes an editor scripting API that allows you to make custom editors and editor scripts that interact with UdonSharpBehaviours the same as you would interact with the normal C# version of a behaviour.

For the most part the editor scripting API allows you to write C# code that just works the same as it would for the C# version of the script, but there are some things to keep in mind while working with the scripts. 

The basis for U# editor scripting is that UdonSharp will create instances of the C# version of your behaviour script and copy the fields from the UdonBehaviour version of the script to the C# 'proxy' behind the scenes. This proxy is what you'll be interacting with most of the time from editor scripts.

## Proxies
UdonSharp scripts are valid C#, which allows you to add them to GameObjects as Proxies alongside typical standalone components. Proxies are similar to Unity's `SerializedObject` type, where any changes you apply to the SerializedObject need to be applied to the original object, and any changes to the original object need to be updated on the serialized object.

Proxies are created on the same GameObject as their backing UdonBehaviour and are disabled. They should remain disabled at all times so Unity does not execute events on them. They are marked as hidden in the GameObject inspector so you cannot see them on the GameObject directly, but they are there. Proxies are also flagged to not be saved in the scene and not be saved in builds so you don't have to worry about them bloating package or download size since they only exist in the editor. 

You can execute methods on the proxies and have them work the same as running events on UdonBehaviours. The main thing you need to keep in mind is that in C# UdonSharpBehaviours are not UdonBehaviours, this is explained in further detail below.

## Differences from regular C# behaviours and from U#
### UdonSharpBehaviours are not UdonBehaviours
In UdonSharp code, it is possible to treat **UdonBehaviours** as **UdonSharpBehaviours**, since they are represented as the same object internally. Technically, UdonSharpBehaviours do not actually inherit from UdonBehaviours, so it's best to use `UdonSharpBehaviour` as variable types instead of `UdonBehaviour` when working in UdonSharp.

Because of this, unless you want to allow people to plug in Udon behaviours built from the graph, you should always prefer to use `UdonSharpBehaviour` as variable types instead of `UdonBehaviour`.

### Proxy references are automatically handled only for UdonSharpBehaviour variables
Only variable types for UdonSharpBehaviours will have their proxies handled automatically. When you reference another proxy behaviour in a proxy behaviour, its reference will automatically be converted to the UdonBehaviour reference by the proxy system. Because of this, variables that reference other **UdonSharpBehaviours** should be stored as the specific **UdonSharpBehaviour** type, or as the base **UdonSharpBehaviour** type if any **UdonSharpBehaviour** type can be stored in the variable. 

If you use variables of the type `UdonBehaviour`, store plain `Component` references, or anything ambiguous at all, the proxy system will just populate the reference to the underlying UdonBehaviour. This is good if you want to allow references to graph assets since they do not fall under the proxy API.

An important thing to keep in mind is that if you store references to the proxy behaviours directly in a variable that is not an UdonSharpBehaviour variable or some subclass of UdonSharpBehaviour, the reference will get cleared to null on build. For instance, if you want to have an `Component` reference, make sure it's referencing the UdonBehaviour and not the proxy UdonSharpBehaviour.

### Proxies are always disabled and should remain disabled
Proxies get disabled to prevent Unity from calling events and running the same logic twice during gameplay on them. You should never re-enable the proxy behaviours. Because proxy behaviours are disabled, if you run methods on the proxy behaviour that call things like GetComponentInChildren without telling it to get disabled behaviours as well, it will not return the proxies.

## Making a custom inspector for your UdonSharpBehaviour
When making custom editors for UdonSharpBehaviours, most things are abstracted to the point that it's exactly like making a normal custom inspector. You just need to make a class that inherits from `Editor` and add the `CustomEditor` attribute with the type of your UdonSharpBehaviour.

When making custom editors for any U# or C# scripts, you must ensure that your editor code is not included in the game build since it uses Editor libraries that are not allowed in Unity games (your world will fail to build if it includes them).

There are two recommended ways to exclude Editor code from your world.
- Place your inspector scripts inside a folder named **Editor**.
- Wrap your code in a check for the preprocessor definition `UNITY_EDITOR`, for example:
```cs
#if UNITY_EDITOR
[CustomEditor(typeof(CustomInspectorBehaviour))]
public class CustomInspectorEditor : Editor
{
    ...
}
#endif
```

Using statements for editor only namespaces like `UnityEditor` will also need to be wrapped in the same `UNITY_EDITOR` check

If you want to write your inspector in the same script file as your UdonSharpBehaviour script, you will need to use the `COMPILER_UDONSHARP` preprocessor definition to prevent UdonSharp from parsing the editor only code. Using the above example with this it would look like the following:
```cs
public class CustomInspectorBehaviour : UdonSharpBehaviour 
{
    ...
}

#if !COMPILER_UDONSHARP && UNITY_EDITOR
[CustomEditor(typeof(CustomInspectorBehaviour))]
public class CustomInspectorEditor : Editor
{
    ...
}
#endif
```

:::warning

The `COMPILER_UDONSHARP` preprocessor definition will only ever be `true` inside the same script as the UdonSharpBehaviour. External scripts that do not contain an UdonSharpBehaviour and are not connected to an UdonSharpProgramAsset will never set `COMPILER_UDONSHARP` to `true`.

:::

:::warning

Do not use `COMPILER_UDONSHARP` or `UNITY_EDITOR` to conditionally remove or add fields to UdonSharpBehaviours. This will result in unexpected behavior

:::

When you make a custom inspector, you should always start the OnInspectorGUI with 
```cs
if (UdonSharpGUI.DrawDefaultUdonSharpBehaviourHeader(target)) return;
```
This handles drawing the default UdonSharp header that contains the convert to behaviour button for C# scripts, the syncing settings, the interact settings, and the utilities. You can draw each individual section as well, look at the implementation of `DrawDefaultUdonSharpBehaviourHeader()` for what you can draw.

### Example inspector
This example is included with UdonSharp

```cs
using UnityEngine;
using VRC.SDK3.Components;
using VRC.SDKBase;
using VRC.Udon;

#if !COMPILER_UDONSHARP && UNITY_EDITOR // These using statements must be wrapped in this check to prevent issues on builds
using UnityEditor;
using UdonSharpEditor;
#endif

namespace UdonSharp.Examples.Inspectors
{
    /// <summary>
    /// Example behaviour that has a custom inspector
    /// </summary>
    public class CustomInspectorBehaviour : UdonSharpBehaviour 
    {
        public string stringVal;

        private void Update()
        {
            Debug.Log($"CustomInspectorBehaviour: {stringVal}");
        }
    }

    // Editor scripts must be wrapped in a UNITY_EDITOR check to prevent issues while uploading worlds. The !COMPILER_UDONSHARP check prevents UdonSharp from throwing errors about unsupported code here.
#if !COMPILER_UDONSHARP && UNITY_EDITOR 
    [CustomEditor(typeof(CustomInspectorBehaviour))]
    public class CustomInspectorEditor : Editor
    {
        public override void OnInspectorGUI()
        {
            // Draws the default convert to UdonBehaviour button, program asset field, sync settings, etc.
            if (UdonSharpGUI.DrawDefaultUdonSharpBehaviourHeader(target)) return;

            CustomInspectorBehaviour inspectorBehaviour = (CustomInspectorBehaviour)target;

            EditorGUI.BeginChangeCheck();

            // A simple string field modification with Undo handling
            string newStrVal = EditorGUILayout.TextField("String Val", inspectorBehaviour.stringVal);

            if (EditorGUI.EndChangeCheck())
            {
                Undo.RecordObject(inspectorBehaviour, "Modify string val");

                inspectorBehaviour.stringVal = newStrVal;
            }
        }
    }
#endif
}
```

## Using [Handles](https://docs.unity3d.com/ScriptReference/Handles.html)
This works the same as making a custom inspector GUI, everything is handled automatically for the most part. You just use the OnSceneGUI event on the Editor and it should work as expected.

## Using [Gizmos](https://docs.unity3d.com/ScriptReference/Gizmos.html)
Gizmos need a little special handling to work as you'd expect. One of the [example scripts](https://github.com/vrchat-community/UdonSharp/blob/master/Packages/com.vrchat.UdonSharp/Samples~/CustomInspectors/CustomInspectorChildBehaviour.cs#L33) makes use of Gizmos. For Gizmos you should wrap the OnDrawGizmos events themselves in the same `#if !COMPILER_UDONSHARP && UNITY_EDITOR` check that we wrapped the editor in, and the **OnDrawGizmos** and **OnDrawGizmosSelected** events should go on the behaviour itself.

Gizmos draw using the proxy behaviour that gets attached to all UdonBehaviours with UdonSharpProgramAssets. They do not get executed through Udon since Udon does not run UdonBehaviours when not in play mode. The Gizmos events are not managed by UdonSharp so you need to do a little to make sure your proxy behaviour is up to date. To do this, call either of these two methods, both do the same thing, `UpdateProxy` is just made to emulate the Unity API's for serialized objects.
```cs
// Call this
UdonSharpEditorUtility.CopyUdonToProxy(this);
// Or this
this.UpdateProxy();
// Do not call both since you'd be doing redundant work
```
You can see an example of this in the example script linked above.

## Non-inspector Editor Scripts
When you are creating editor scripts that create/delete/modify UdonSharpBehaviours, you need to manage updating the proxy and applying its modifications yourself. 

### Adding an UdonSharpBehaviour
Adding a new UdonSharpBehaviour is as simple as getting the GameObject you want to attach it to and calling .`AddUdonSharpComponent<T>()` on it.
```cs
GameObject targetGameObject = ... // Get some game object from somewhere here
MyComponentType newComponent = targetGameObject.AddUdonSharpComponent<MyComponentType>();
```
Now newComponent is a valid UdonSharpBehaviour proxy of your component type MyComponentType. You can interact with this like any other C# component from the editor script. If you want the component creation to be undoable, instead use:
```cs
GameObject targetGameObject = ... // Get some game object from somewhere here
MyComponentType newComponent = UdonSharpUndo.AddComponent<MyComponentType>(targetGameObject);
```

### Getting an existing UdonSharpBehaviour
UdonSharp has equivalents for GetComponent(s) defined as extension methods to GameObject. When you are working with editor scripts, instead of calling `GetComponent<T>()`, you should call its equivalent `GetUdonSharpComponent<T>()`.

In order to get all UdonSharpBehaviours of the type `MyComponentType` on children of a GameObject, you would do the following:
```cs
GameObject sourceGameObject = ... // Get some game object from somewhere here
MyComponentType[] myComponents = sourceGameObject.GetUdonSharpComponentsInChildren<MyComponentType>();
```

### Working with an UdonSharpBehaviour and modifying it
Once you have an UdonSharpBehaviour to work with, you will need to handle making sure any modifications to the proxy get propagated to Udon.

If Udon is making any changes to the behaviour, the behaviour should get updated if you are using GetUdonSharpComponent(s) since those will automatically update the behaviour. If you store a reference to the behaviour, you will need to update it yourself. This can be done by calling `UpdateProxy()` on the behaviour.

Once you have modified your behaviour, you must apply the modifications on the proxy behaviour to Udon. This can be done by calling `ApplyProxyModifications()` on the behaviour.

You can think of this as being similar to working with Unity SerializedObjects.

```cs
MyComponentType myComponent = ...
// We only need to update the proxy if we storing some persistent reference to it
myComponent.UpdateProxy();
// Add 5 to a `float` on our behaviour
myComponent.myFloatField += 5f;
// Apply the changes to myComponent to the Udon copy of it
myComponent.ApplyProxyModifications();
```

## Destroying UdonSharpBehaviours
You should use the UdonSharpEditorUtility.DestroyImmediate() method to destroy UdonSharpBehaviours and delete their underlying UdonBehaviour.


---

## ドキュメント: index.md

```metadata
階層: /worlds/udon/udonsharp/index.md
ディレクトリ: worlds\udon\udonsharp
ファイル名: index.md
拡張子: .md
サイズ: 6.64 KB
最終更新: 2025-06-05T03:07:52.828Z
```

---
sidebar_position: -1
---
# UdonSharp

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

UdonSharp allows you to create Udon scripts in C#. UdonSharp is included in the VRChat worlds SDK and compiles your C# to Udon Assembly code.

- Add helpful [attributes](./attributes) to extend the functionality of your UdonSharp scripts. 
- Open the [class exposure tree](./class-exposure-tree) to see which C# classes and methods are available in UdonSharp.
- Change your [configuration](./configuration) to disable auto-compilation or change other settings.
- Learn how to write [custom editor scripts](./editorscripting).
- Learn how to [migrate](./migration) projects that use very old versions of UdonSharp.

## How to create an UdonSharp script

You can create an UdonSharp script the project window or the hierarchy window by following the steps below.

### In the Project window
1. Right-click in your project's asset explorer.
2. Navigate to "Create" > "U# script".
3. Click "U# script". This will open a file creation dialog.
4. Choose a name for your script and click "Save".
5. This will create a `.cs` script file and an UdonSharp program asset that's set up for the script in the same directory.
### In the Hierarchy window
1. Create a new game object in your scene.
2. Add an `Udon Behaviour` component to the object.
3. Below the "New Program" button click the dropdown and select "Udon C# Program Asset".
4. Now click the "New Program" button. This will create a new UdonSharp program asset for you.
5. Click the "Create Script" button and choose a save destination and name for the script.

When you create a new UdonSharp script, you end up with two files:
- An "UdonSharp Program Asset" file. (You don't need to edit this file after creating it.)
- A `.cs` file. It has the class `UdonSharpBehaviour` and looks like this:

<Tabs>
<TabItem value="cs" label="UdonSharp">

```cs
using UdonSharp;
using UnityEngine;
using VRC.SDKBase;
using VRC.Udon;

public class YourScriptName : UdonSharpBehaviour
{
    void Start()
    {
        
    }
}
```

</TabItem>
</Tabs>

Edit the UdonSharpBehaviour in your editor of choice and start programming. You can add the UdonSharpBehaviour to any GameObject in your scene.



## Supported C# Features

UdonSharp support most of C#'s basic syntax:

- Flow control: `if`, `else`, `while`, `for`, `do`, `foreach`, `switch`, `return`, `break`, `continue`, `ternary operator (condition ? true : false)`, `??`
- Implicit and explicit type conversions
- Arrays and array indexers
- All built-in arithmetic operators
- Conditional short circuiting `(true || CheckIfTrue())` will not execute `CheckIfTrue()`
- `typeof()`
- Extern methods with `out` or `ref` parameters, such as many variants of `Physics.Raycast()`
- User defined methods with parameters and return values, supports out/ref, extension methods, and `params`
- User defined properties
- Static user methods
- UdonSharpBehaviour inheritence, virtual methods, etc
- Unity/Udon event callbacks with arguments. For instance, registering an `OnPlayerJoined` event with a `VRCPlayerApi` argument is valid.
- String interpolation
- Field initializers
- Jagged arrays
- Referencing other custom UdonSharpBehaviour classes, accessing fields, and calling methods on them
- Recursive method calls are supported via the `[RecursiveMethod]` attribute

## Differences from Unity/C# Features

UdonSharp is not conformant to any version of the C# language specification. Some C# features are not implemented or will not work.

- For the best experience when creating UdonSharp scripts, make your scripts inherit from `UdonSharpBehaviour` instead of `MonoBehaviour`.
- If you need to call `GetComponent<UdonBehaviour>()`, you will need to use `(UdonBehaviour)GetComponent(typeof(UdonBehaviour))` since the generic get component is not exposed for UdonBehaviours yet. `GetComponent<T>()` works for other Unity component types though.
- Udon currently only supports array `[]` collections. By extension, UdonSharp only supports arrays at the moment. `List<T>` is not supported yet.
- Field initializers are evaluated at compile time. If you have any initialization logic that depends on other objects in the scene you should use `Start`.
- Use the `[UdonSynced]` attribute on fields that you want to sync over the network for all players.
- Numeric casts are checked for overflow due to UdonVM limitations.
- The internal type of variables returned by `.GetType()` will not always match what you may expect since U# abstracts some types in order to make them work in Udon. For instance, any jagged array type will return a type of `object[]` instead of something like `int[][]` for a 2D `int` jagged array.


## Example scripts

### The rotating cube demo

This rotates the object that it's attached to by 90 degrees every second.

```cs
using UnityEngine;
using UdonSharp;

public class RotatingCubeBehaviour : UdonSharpBehaviour
{
    private void Update()
    {
        transform.Rotate(Vector3.up, 90f * Time.deltaTime);
    }
}
```

### Other examples

- The VRChat Worlds SDK contains UdonSharp examples in `Assets/UdonSharp/UtilityScripts`. 
	- These scripts can be safely deleted if they are not used.
- For more example scripts, take a look at the [GitHub wiki examples](https://github.com/vrchat-community/UdonSharp/wiki/examples) or the Examples folder included with U#.
- Visit the [VRChat Creator Hub](https://ask.vrchat.com/c/creator-hub/63/none) to speak to other creators and see their examples.

## Frequently asked questions

### Does UdonSharp support all Udon features?
Yes. If Udon supports it, then so does UdonSharp. You can check the [class exposure tree](https://github.com/Merlin-san/UdonSharp/wiki/class-exposure-tree) to see everything UdonSharp has access to.

### Can I access the player camera?
No, Udon can not access the player's camera. You can, however, get the head position and rotation. For example, you can use [VRCPlayerApi.GetTrackingData](https://github.com/Merlin-san/UdonSharp/wiki/vrchat-api#vrchatplayerapi) like this:
 
`var headPosition = localPlayer.GetTrackingData(TrackingDataType.Head).position`

### Can I have more than one UdonSharp UdonBehavior on a GameObject?
Yes.

### I'm starting from scratch and need to use C# tutorials. What common aspects of C# don't work in UdonSharp?
If you are learning UdonSharp and not familiar with C# already, you may run across some commonly used techniques that don't work in Udon and UdonSharp yet. These include, but are not limited to, the following:
- Generic classes (`Class<T>`) and generic non-static methods,
- Interfaces
- Method overloads
- Properties

---

## ドキュメント: migration.md

```metadata
階層: /worlds/udon/udonsharp/migration.md
ディレクトリ: worlds\udon\udonsharp
ファイル名: migration.md
拡張子: .md
サイズ: 4.31 KB
最終更新: 2025-06-05T03:07:52.828Z
```

---
unlisted: true
---
# Migration

UdonSharp 0.x (the .unitypackage version) is deprecated and no longer supported.

Newer versions of UdonSharp are included in the VRChat Worlds SDK, which is easy to install through the [Creator Companion](https://vcc.docs.vrchat.com). We recommend you [Migrate your Projects using the Creator Companion](https://vcc.docs.vrchat.com/vpm/migrating). If you want to do the migration manually, read [Manual Migration](#manual-migration).

## New Features in UdonSharp 1.0
* **More C# features** in your UdonSharp programs:
	* `static` methods
	* Generic `static` methods
	* `params`, `out`, `ref`, and default parameters
	* Extension methods
	* Inheritance, virtual methods, and abstract classes
	* Partial classes
	* Enums
- **Multi-edit** multiple UdonSharp scripts in the Unity inspector
- **Prefab variants**, **instances**, and **nesting** are now fully supported
- **Editor scripting** has been overhauled and simplified
- **Compiler fixes** and **optimizations**
- **Fixed various bugs**, edge cases, and other rough edges

## Known Issues

When migrating your project from a very old version of UdonSharp, you may encounter some known issues. Here's how you can address them.

:::tip Try this first!

Is UdonSharp giving you trouble? Try reimporting all assets with "Assets" > "Reimport All."

In many cases, this fixes import issues related to UdonSharp, especially when migrating or upgrading your project from an older version.

:::

### Nested Prefabs

**Issue**: UdonSharp always warned against using nested prefabs. When migrating from an old version, nested prefabs may break.

**Symptoms**: Errors in Unity's console like `Cannot upgrade scene behaviour 'SomethingOrOther' since its prefab must be upgraded`.

**How to Fix**: Unpack the prefab in your 0.x UdonSharp project first. You can also open the "Udon Sharp" menu item and choose "Force Upgrade".

### Does Not Belong to U# Assembly

**Issue**: Libraries with their own Assembly Definitions need to have an U# assembly definition, too.

**Symptoms**: Errors in Unity's console like `[UdonSharp] Script 'Assets/MyScript.cs' does not belong to a U# assembly, have you made a U# assembly definition for the assembly the script is a part of?`

**How to Fix**:
1. Use the Project window to find the file ending in `.asmdef` in the same or a parent directory of the script in question. 
2. Right-click in the folder which has this Assembly Definition and choose `Create > U# Assembly Definition`. 
3. Select this new U# asmdef, and use the inspector to set its "Source Assembly" to the other Assembly Definition File. 
4. You may need to restart Unity after doing this.

### Newtonsoft.Json.Dll

**Issue**: Some packages include their own copy of this JSON library, which the VRCSDK pulls in itself. This results in two copies of the library.

**Symptoms**: Errors in Unity's console which mention the above library. It might not be at the front of the sentence, but something like `System.TypeInitializationException: the type initializer for blah blah blah...Assets/SketchfabForUnity/Dependencies/Libraries/Newtonsoft.Json.dll`

**How to Fix**: Remove any copies of Newtonsoft.Json.dll from your Assets folder. The VRCSDK will provide it for any package that needs it through the Package Manager.

### Other breaking changes
- Your U# behaviour name must match the .cs file name.
- Duplicate program assets may not reference the same `.cs` file.
- Program assets must point to a script and may not be empty.
- Editor scripting is now different: Data is owned by a C# proxy of the UdonSharpBehaviour, and the corresponding UdonBehaviour is empty until runtime.
- Obsoleted overloads for station and player join events may no longer be used.

## Manual Migration

Follow these steps to upgrade a project that uses a version of UdonSharp below 1.0 without using the Creator Companion:

1. Back up your project.
2. Delete the VRCSDK folder, Udon folder, UdonSharp folder, and Gizmos/UdonSharp folders from your project's "Assets" folder.
3. Download and install the [Unity Package versions of the World SDK](https://vrchat.com/download/sdk3-worlds).
4. Download and install the [Unity Package version of the UdonSharp SDK](https://github.com/vrchat-community/UdonSharp/releases/download/1.1.7/com.vrchat.udonsharp-1.1.7.unitypackage).


---

## ドキュメント: performance-tips.md

```metadata
階層: /worlds/udon/udonsharp/performance-tips.md
ディレクトリ: worlds\udon\udonsharp
ファイル名: performance-tips.md
拡張子: .md
サイズ: 2.10 KB
最終更新: 2025-06-05T03:07:52.829Z
```


# Performance Tips

This page contains a few tips on how to improve the performance of your UdonSharp scripts.

## UdonSharp is slower than C\#
Udon can take about 200x to 1000x longer to run a piece of code than the equivalent in normal C#, depending on what you're doing. 
- Avoid massive complex algorithms that iterate over thousands of elements.
- Avoid iterating over very long arrays every frame.
	- Consider "time-slicing" your array by only processing only a limited amount of array elements per frame.
- Use native Unity components or VRChat SDK components whenever possible.
	- For example: Instead of rotating 40 GameObjects in Udon, use an animation.
- Cache method results to improve performance.
	- For example: Instead of getting [`GameObject.transform`](https://docs.unity3d.com/ScriptReference/GameObject-transform.html) or [`Networking.LocalPlayer`](/worlds/udon/players/getting-players/#networkingget-localplayer) every frame, use the `Start()` event to save them as variables and reuse them later.

## Use `private` methods
You should mark methods as `private` if they are not being called from other scripts. Due to how Udon searches for methods to call, the fewer public methods you have, the better the performance of your custom events.

Calls across behaviours are slower than local method calls. If you can, keep similar behavior in one UdonSharpBehaviour rather than separating it into different behaviours.

Don't forget that in order to send a [CustomNetworkEvent](/worlds/udon/graph/special-nodes#udonbehaviour-nodes), the target method must be `public`. If it is not public, it will not fire. Additionally, any methods that start with an underscore such as `_MyLocalMethod` will not receive network events.

## Don't use `GetComponent<T>` frequently
You should only use `GetComponent<T>` in the `Start()` event or event that happen that don't happen frequently.

`GetComponent<T>()` is slow, especially when retrieving an UdonSharpBehaviour type in Udon. That's because UdonSharp needs to insert a loop that checks all UdonBehaviours to make sure they're the correct UdonSharpBehaviour type. 


---

# ドキュメント終了

# editor 統合ドキュメント

以下は自動収集された全ドキュメントのコンテンツです。各セクションの始めにメタデータが記載されています。



---

## ドキュメント: editor-runtime-linker.md

```metadata
階層: /worlds/clientsim/systems/editor/editor-runtime-linker.md
ディレクトリ: worlds\clientsim\systems\editor
ファイル名: editor-runtime-linker.md
拡張子: .md
サイズ: 262 B
最終更新: 2025-06-05T03:07:52.766Z
```

# Editor Runtime Linker

This system links and unlinks, on enter and exit playmode, the Editor only hooks in the [ClientSim Menu](../runtime/menu.md) for checking if settings are invalid and a method to open the [ClientSim Settings Window](settings-window.md).

---

## ドキュメント: helper-editors.md

```metadata
階層: /worlds/clientsim/systems/editor/helper-editors.md
ディレクトリ: worlds\clientsim\systems\editor
ファイル名: helper-editors.md
拡張子: .md
サイズ: 855 B
最終更新: 2025-06-05T03:07:52.766Z
```

# Helper Editors

Some of the ClientSim SDK Helper components have specialized editors that offer insight into the VRC component.

### SyncableEditorHelper
* Provides helper method to display the current owner of an IClientSimSyncable. This is a dropdown selector allowing you to change the current owner of the object.

### ObjectSyncHelperEditor
* Displayed on ObjectSyncHelper
* Shows the owner selector dropdown

### ObjectPoolHelperEditor
* Displayed on ObjectPoolHelper
* Shows the owner selector dropdown

### UdonHelperEditor
* Displayed on UdonHelper
* Shows the owner selector dropdown
* Displays an editor for all public variables in the Udon Program
* Displays buttons to SendCustomEvent for each exported custom event in the Udon Program

### SpatialAudioHelperEditor
* Display nothing instead of ONSP audio’s editor


---

## ドキュメント: index.md

```metadata
階層: /worlds/clientsim/systems/editor/index.md
ディレクトリ: worlds\clientsim\systems\editor
ファイル名: index.md
拡張子: .md
サイズ: 65 B
最終更新: 2025-06-05T03:07:52.767Z
```

# Editor

These systems help set things up in the Unity Editor.

---

## ドキュメント: settings-window.md

```metadata
階層: /worlds/clientsim/systems/editor/settings-window.md
ディレクトリ: worlds\clientsim\systems\editor
ファイル名: settings-window.md
拡張子: .md
サイズ: 1.72 KB
最終更新: 2025-06-05T03:07:52.767Z
```

# Settings Window

The Settings window displays all the [ClientSim Settings](../runtime/settings.md) that can be edited. Some of these values cannot be changed at runtime. There is also a button to spawn remote players and a text field to give those remote players a custom name.
The Settings window can be opened through the “VRChat SDK/Utilities/ClientSim” option.

## Project Setting Warnings

If the Unity Project Settings do not match what is needed to run ClientSim, warnings will display for each project setting that is invalid. 

## Project Settings Setup

ClientSim requires specific Unity Project settings to run properly. These settings are not default for a project and require action from the user to modify. The ClientSim Settings Window will display the current incorrect project settings with buttons to update the project to the correct setting. This behaviour differs from CyanEmu, which on every assembly reload would automatically force update project settings.

### Input Manager settings are incorrect
ClientSim requires the new Unity Input System. Upon importing this package, it will ask the user to enable it and restart Unity. Neither option in this dialog is the correct setting for ClientSim and the user must enable both old input and new input. Clicking the “Do it!” button in the settings window will set the correct input and force a Unity restart.

### Input Axes differ from VRChat’s
While not used within ClientSim, this is helpful for users to test getting input within Udon Programs.

### Audio Spatializer not set
ClientSim depends on the Oculus Audio Spatializer.

### Project Layers and Collision Matrix not set
The user must open the VRChat Build Control Panel to properly set this.

---

# ドキュメント終了

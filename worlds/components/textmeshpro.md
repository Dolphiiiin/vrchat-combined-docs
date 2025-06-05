# textmeshpro 統合ドキュメント

以下は自動収集された全ドキュメントのコンテンツです。各セクションの始めにメタデータが記載されています。



---

## ドキュメント: index.md

```metadata
階層: /worlds/components/textmeshpro/index.md
ディレクトリ: worlds\components\textmeshpro
ファイル名: index.md
拡張子: .md
サイズ: 3.46 KB
最終更新: 2025-06-05T03:07:52.777Z
```

# TextMesh Pro

[TextMesh Pro](https://docs.unity3d.com/Packages/com.unity.textmeshpro@4.0/manual/index.html) is a tool for displaying high-quality 2D and 3D text in Unity and VRChat worlds. Currently, the following components are exposed to Udon:

- [TMP Text](./tmp_text) (TextMeshProUGUI and TextMeshPro)
- [TMP Dropdown](./tmp_dropdown)
- [TMP InputField](./tmp_inputfield)

You can also user other TextMeshPro components from VRChat's [allowlisted world components](/worlds/whitelisted-world-components#text-mesh-pro), but they aren't available in Udon. 

:::caution

Avoid using Unity's built-in [text components](https://docs.unity3d.com/2018.1/Documentation/ScriptReference/UI.Text.html).

- Unity's built-in text causes aliasing and is limited to 16250 characters.
- TextMesh Pro has better text rendering, [rich text](http://digitalnativestudios.com/textmeshpro/docs/rich-text/), and doesn't have a character limit.

:::

## Installation

The TextMesh Pro package is included in the Unity editor. When you use it for the first time in a project, Unity will prompt you to add essential files to your "Assets" folder. 
For example, creating any TMP component will show the following prompt:

![TextMesh Pro prompt for importing essential assets.](/img/worlds/components/textmeshpro-essentials.png)

TextMesh Pro's assets can also be imported by selecting "Window" > "TextMeshPro" > "Import TMP Essential Resources".

## Importing fonts

TextMesh Pro includes a default font called "LiberationSans SDF." This allows you to get started with TextMesh Pro right away.

You can import your own font into TextMesh Pro with the [font asset creator](https://docs.unity3d.com/Packages/com.unity.textmeshpro@4.0/manual/FontAssetsCreator.html). This generated a font asset that you can use in your TextMesh Pro components.

<div class="video-container">
    <iframe src="https://www.youtube.com/embed/qzJNIGCFFtY?si=JsAJMtzdp3KC-8tJ" title="TextMesh Pro - Font Asset Creation" frameborder="0" allow="encrypted-media; gyroscope; web-share" allowfullscreen></iframe>
</div>

:::tip Optimize your font assets!

Font assets increase the download size and RAM requirements of your world. When importing fonts, reduce your atlas resolution and don't import unused characters.

:::

## Using VRChat's built-in fallback font

When you [import a font](https://docs.unity3d.com/Packages/com.unity.textmeshpro@4.0/manual/FontAssetsCreator.html) into TextMeshPro, it usually won't include every Unicode character. If your world contains strings or user names that aren't in your font asset, TextMeshPro won't be able to render them. For example, instead of "何？！", you may see "□□□".

To use VRChat's TextMeshPro Fallback fonts, go to "Project Settings" > "TextMeshPro settings" remove all "Fallback Font Assets."

This allows your TextMeshPro components to use VRChat's fallback fonts. When you visit your world in VRChat, almost all Unicode characters are rendered correctly.

![When configured correctly, missing Unicode characters will appear as boxes in the Unity editor, but appear correctly after uploading your world to VRChat.](/img/worlds/components/textmeshpro-editor-vs-fallback-vrchat.png)

## See also

- [TextMeshPro documentation](https://docs.unity3d.com/Packages/com.unity.textmeshpro@4.0/api/TMPro.html) (Unity scripting API)
- [Text in Unity - Working with TextMesh Pro](https://learn.microsoft.com/en-us/windows/mixed-reality/develop/unity/text-in-unity) (Microsoft Learn)

---

## ドキュメント: tmp_dropdown.md

```metadata
階層: /worlds/components/textmeshpro/tmp_dropdown.md
ディレクトリ: worlds\components\textmeshpro
ファイル名: tmp_dropdown.md
拡張子: .md
サイズ: 5.43 KB
最終更新: 2025-06-05T03:07:52.777Z
```

# TMP Dropdown

[`TMP_Dropdown`](https://docs.unity3d.com/Packages/com.unity.textmeshpro@4.0/api/TMPro.TMP_Dropdown.html) is a standard dropdown that presents a list of options when clicked, of which one can be chosen. It should always be used instead of Unity's legacy `UI.Dropdown` component.

## Properties 

| Property | Type | Description |
|----------|------|-------------|
| value    | int  | Gets or sets the index of the selected element in the dropdown. |
| IsExpanded| bool | Gets a value indicating whether the dropdown is expanded. |
| enable| bool | Enables/disables the attached component |

## Methods

| Function              | Input        | Output | Description                                                                                                                                                                                                                                                           |
| --------------------- | ------------ | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SetValueWithoutNotify | int          | void   | Sets the value of the input field without invoking `onValueChanged`.                                                                                                                                                                                                  |
| RefreshShownValue     |              |        | Refreshes the text and image (if available) of the currently selected option.                                                                                                                                                                                         |
| ClearOptions          |              |        | Removes all options from the dropdown.                                                                                                                                                                                                                                |
| Show                  |              |        | Shows the dropdown. Plan for dropdown scrolling to ensure dropdown is contained within screen. [TMP assumes the Canvas is the screen that the dropdown must be kept inside.](https://docs.unity3d.com/Packages/com.unity.textmeshpro@2.0/api/TMPro.TMP_Dropdown.html) |
| Hide                  |              |        | Hides the dropdown list.                                                                                                                                                                                                                                              |

## Related classes

Udon has access to two additional classes related to TMP_Dropdown. 

### VRCTMPDropdownExtension
This class is part of the VRChat SDK. It allows you to add options to your dropdown.

| Function   | Input                           | Output | Description                                                                              |
| ---------- | ------------------------------- | ------ | ---------------------------------------------------------------------------------------- |
| AddOptions | TMP_Dropdown,<br/> string[]     |        | Adds the array of strings to the dropdown.                                               |
| AddOptions | TMP_Dropdown,<br/> sprite[]     |        | Adds the array of sprites to the dropdown.                                               |
| AddOptions | TMP_Dropdown,<br/> OptionData[] |        | Adds the array of [TMP_Dropdown.OptionData](#tmp_dropdownoptiondata) to the dropdown. |

:::note Extension method
In UdonSharp, AddOptions is an [extension method](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/extension-methods).

Instead of `AddOptions(dropdown, options)`, you can type `dropdown.AddOptions(options)`. Make sure you have the namespace `VRC.SDK3.Components` included in the file.
:::
### TMP_Dropdown.OptionData
This is built into TextMesh Pro. It allows you to add a combination of strings and sprites to a dropdown. 

| Functions  | Input | Output | Description|
|------------|-------|--------|------------|
| Constructor| string |OptionData| Creates an OptionData object with text as the option. |
| Constructor| sprite|OptionData| Creates an OptionData object with a sprite as the option |
| Constructor| string, sprite |OptionData| Creates an OptionData object with text and a sprite as the option |

## How to add sprites to a dropdown

Dropdowns can display a different sprite for each option. You need to set up your dropdown correctly:

1. Add an image to the dropdown.

![Add an image to the dropdown hierarchy, moving the label out of the way - part 1](/img/worlds/components/VRC_DropdownAddLabelScene.png)
![part 2](/img/worlds/components/VRC_DropdownAddLabelHierarchie.png)

2. Add an image to the template.

![Add an image to the template hierarchy, moving the label out of the way - part 1](/img/worlds/components/VRC_DropdownAddLabelTemplateScene.png)
![part 2](/img/worlds/components/VRC_DropdownAddLabelTemplateHierarchie.png)

3. Add the correct references to `Caption Image` and `Item Image` fields in the dropdown.

![part 2](/img/worlds/components/VRC_DropdownAddLabelInspector.png)

4. Success! The dropdown can now display sprites.


---

## ドキュメント: tmp_inputfield.md

```metadata
階層: /worlds/components/textmeshpro/tmp_inputfield.md
ディレクトリ: worlds\components\textmeshpro
ファイル名: tmp_inputfield.md
拡張子: .md
サイズ: 1019 B
最終更新: 2025-06-05T03:07:52.777Z
```

# TMP InputField
[`TMP_InputField`](https://docs.unity3d.com/Packages/com.unity.textmeshpro@4.0/api/TMPro.TMP_InputField.html) is an editable text input field. It should always be used instead of Unity's legacy `UI.InputField` component.
## Properties

| Property | Type | Description |
|----------|------|-------------|
| text | string | Gets or sets the value of the input field. |
| isFocused | bool | Gets a value indicating whether the input field currently has focus and is able to process UI events. This field is read-only. |
| readOnly | bool | Gets or sets a value indicating whether the value of the input field is read-only. |
| richText | bool | Gets or sets a value indicating whether rich text editing is allowed. |
| enable| bool | Enables/disables the attached component. |

## Methods

| Function | Input | Output | Description|
|----------|-------|--------|------------|
| SetTextWithoutNotify | string | void | Sets the value of the input field without invoking `onValueChanged`. |


---

## ドキュメント: tmp_text.md

```metadata
階層: /worlds/components/textmeshpro/tmp_text.md
ディレクトリ: worlds\components\textmeshpro
ファイル名: tmp_text.md
拡張子: .md
サイズ: 3.71 KB
最終更新: 2025-06-05T03:07:52.778Z
```

# TMP Text
`TMP_Text` is the base class of two TextMesh Pro components:

- [`TextMeshProUGUI`](https://docs.unity3d.com/Packages/com.unity.textmeshpro@1.1/api/TMPro.TextMeshProUGUI.html) allows you to display text with a [canvas renderer](https://docs.unity3d.com/Packages/com.unity.ugui@2.0/manual/UICanvas.html).
	- This component is an ideal replacement for the UI.Text component.
	- Change the "Render Mode" of your canvas depending on how you want to display your text.
		- "World Space" allows you to place text anywhere in your world.
		- "Screen Space" allows you to display text directly on the user's screen.
- [`TextMeshPro`](https://docs.unity3d.com/Packages/com.unity.textmeshpro@1.1/api/TMPro.TextMeshPro.html) allows you to display text with a [mesh renderer](https://docs.unity3d.com/Manual/class-MeshRenderer.html).
	- This component does not require a canvas. It's an ideal replacement for the legacy TextMesh components.

# Properties
Use the `TMP_Text` base class to access these properties. Both `TextMeshProUGUI` and `TextMeshPro` inherit from `TMP_Text`.

| Property           | Type | Result |
|--------------------|------|--------|
| text               | String | Accesses the text.|
| isRightToLeftText  | Bool | Gets/sets if the text needs to be displayed right to left. Useful when implementing a variety of languages.|
| fontMaterial       | Material | Gets/sets the font material. Take note that accessing this field will clone the material.|
| fontSharedMaterial | Material | Gets/sets the shared font material. This returns null if this component's FontAsset is null.|
| color              | Color | Gets/sets the color of the text.|
| alpha              | Float | Gets/sets the opacity of the text.|
| fontSize           | Float | Gets/sets the size of the text.|
| enableAutoSizing   | Bool | Gets/sets if the text should try to size itself as big as possible and still be visible within the range of FontSizeMin and FontSizeMax.| 
| fontSizeMin        | Float | The minimum font size if AutoSizing is enabled.|
| fontSizeMax        | Float | The maximum font size if AutoSizing is enabled.|
| horizontalAlignment| HorizontalAlignmentOptions | How the text will be aligned horizontally.|
| verticalAlignment  | VerticalAlignmentOptions | How the text will be aligned vertically.|
| alignment          | TextAlignmentOptions | How the text will be aligned overall.|
| characterSpacing   | Float | The amount of space between characters.|
| wordSpacing        | Float | The amount of space between words.|
| lineSpacing        | Float | The amount of vertical space between text lines.|
| paragraphSpacing   | Float | The amount of vertical space between paragraphs.|
| characterWidthAdjustment| Float | The amount of overlap characters have.|
| enableWordWrapping | Bool | Controls whether words that exceed the available space will be put on a new line.|
| overflowMode       | TextOverflowModes | Controls how the text will be treated if it overflows its available space.|
| richText           | Bool | Gets/sets if [Rich Text](https://docs.unity3d.com/Packages/com.unity.textmeshpro@3.2/manual/RichText.html) is enabled.|
| parseCtrlCharacters| Bool | Enables or Disables parsing of escape characters typed into input text fields, such as `\n`, `\r`, `\t`, or `\\`. |
| firstVisibleCharacter | Int | The first character that should be made visible in conjunction with the Text Overflow Linked mode.|
| maxVisibleCharacters | Int | Controls how many characters are visible from the input.|
| maxVisibleWords    | Int | Controls how many words are visible from the input.|
| maxVisibleLines    | Int | Controls how many lines of text are displayed.|
| enable             | bool | Enables/disables the attached component. |



---

# ドキュメント終了

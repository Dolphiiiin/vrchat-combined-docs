# vrc-graphics 統合ドキュメント

以下は自動収集された全ドキュメントのコンテンツです。各セクションの始めにメタデータが記載されています。



---

## ドキュメント: asyncgpureadback.md

```metadata
階層: /worlds/udon/vrc-graphics/asyncgpureadback.md
ディレクトリ: worlds\udon\vrc-graphics
ファイル名: asyncgpureadback.md
拡張子: .md
サイズ: 3.15 KB
最終更新: 2025-06-05T03:07:52.833Z
```

import UnityVersionedLink from '@site/src/components/UnityVersionedLink.js';

# AsyncGPUReadback

AsyncGPUReadback in Unity is a feature that allows developers to copy data, such as a specific pixel's color, from textures on the GPU to code on the CPU. This function is similar to `Texture2D.GetPixels`except it does not block tasks on the main thread as it runs asynchronously.

By performing the data transfer asynchronously, Unity ensures that the process does not negatively affect the application's frame rate and overall performance. AsyncGPUReadback helps developers avoid stalling the rendering pipeline, as the data transfer occurs in the background, parallel to the main thread.

Common use cases for AsyncGPUReadback include generating CPU-side data based on the rendered output, such as creating a texture mipmap chain, implementing custom post-processing effects, or analyzing rendered frames for AI and computer vision purposes.

## Differences between Unity AsyncGpuReadback and VRCAsyncGpuReadback

VRChat's implementation of AsyncGpuReadback employs a wrapper that calls the Unity functions. This wrapper uses a different interface. The differences are as follows:

- Rather than calling AsyncGpuReadback, you need to use VRCAsyncGpuReadback.
- Rather than providing `Action<AsyncGPUReadbackRequest> callback`, you provide an UdonBehaviour, and then that UdonBehaviour will receive `OnAsyncGpuReadbackComplete` when the readback is complete.
- Rather than using `GetData` on the completed readback, you need to use `TryGetData`.

See Unity's documentation on this feature for other shared details:  
- <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/ScriptReference/Rendering.AsyncGPUReadback.Request.html">Making a readback request</UnityVersionedLink>
- <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/ScriptReference/Rendering.AsyncGPUReadbackRequest.html">Getting data from a readback</UnityVersionedLink>

## Using VRCAsyncGpuReadback

When using VRCAsyncGpuReadback, there are 4 main steps that you need to follow:

1. Set up an array of data to be used.
2. Make the AsyncGpuReadback request
3. Receive the message when the request is complete
4. Get the data out of the request

## Udon Node Graph Example

![asyncgpureadback-mu8QGGS.png](/img/worlds/asyncgpureadback-mu8QGGS.png)

## UdonSharp Example

```csharp

using UdonSharp;
using UnityEngine;
using VRC.SDK3.Rendering;
using VRC.Udon.Common.Interfaces;
​
public class AGPURB : UdonSharpBehaviour
{
    public Texture texture;
​
    void Start()
    {
        VRCAsyncGPUReadback.Request(texture, 0, (IUdonEventReceiver)this);
    }
​
    public void OnAsyncGpuReadbackComplete(VRCAsyncGPUReadbackRequest request)
    {
        if (request.hasError)
        {
            Debug.LogError("GPU readback error!");
            return;
        }
        else
        {
            var px = new Color32[texture.width * texture.height];
            Debug.Log("GPU readback success: " + request.TryGetData(px));
            Debug.Log("GPU readback data: " + px[0]);
        }
    }
}
```

---

## ドキュメント: index.md

```metadata
階層: /worlds/udon/vrc-graphics/index.md
ディレクトリ: worlds\udon\vrc-graphics
ファイル名: index.md
拡張子: .md
サイズ: 3.16 KB
最終更新: 2025-06-05T03:07:52.833Z
```

import UnityVersionedLink from '@site/src/components/UnityVersionedLink.js';

# VRCGraphics

Udon has access to a variety of Unity's graphics functions.

## Types

### VRCShader

Overarching class for global shader setters. See [functions documented below](#vrcshaderpropertytoid) for more.

### VRCGraphics

Exposes a subset of Unity’s built-in \`Graphics\` class. See documented functions for more.

## VRCCameraSettings & VRCQualitySettings

Exposes information and limited control over the user's screen camera, the handheld camera, and Unity's global quality settings. See [VRCCameraSettings](/worlds/udon/vrc-graphics/vrc-camera-settings) for more.

## Exposed Functions

### VRCGraphics.Blit()

Copies source texture into destination RenderTexture with a shader. Note that we do not allow you to supply a null destination.

See <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/ScriptReference/Graphics.Blit.html">Graphics.Blit</UnityVersionedLink>.

Check out the [Minimap Example](/worlds/examples/minimap) to learn more about how to use VRCGraphics.Blit().

![Minimap Example World](/img/worlds/examples/minimap-example-world.png)

#### Meta Quest Exceptions

VRCGraphics.Blit will not work on the Quest GPU unless you:

Add ZTest Always to the shader
OR
Turn off the depth on the target RenderTexture.

Failing to do so will cause the operation to fail.

### VRCGraphics.DrawMeshInstanced()

Draw the same mesh multiple times using GPU instancing.

See <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/ScriptReference/Graphics.DrawMeshInstanced.html">Graphics.DrawMeshInstanced</UnityVersionedLink>.

### VRCShader.PropertyToID()

Use PropertyToID to get an ID based on a shader property name. Call this function only once during initialization, the ID can be reused and will not change, even between different materials and shaders.

Note that the property name must be prefixed with “\_Udon”, or be the literal string “\_AudioTexture” in order to be used with VRCShader.SetGlobal, however, will still return the ID regardless of this.

See <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/ScriptReference/Shader.PropertyToID.html">Shader.PropertyToID</UnityVersionedLink>.

### VRCShader.SetGlobal()

Use the ID acquired with PropertyToID as a key and specify a value of the correct type. The value will be available in _all_ shaders in the world (including ones on avatars!) under the name passed into PropertyToID.

Available variants:

  * VRCShader.SetGlobalColor()
  * VRCShader.SetGlobalFloat()
  * VRCShader.SetGlobalFloatArray()
  * VRCShader.SetGlobalInteger() still sets the value as `float` for now, due to a Unity bug
  * VRCShader.SetGlobalMatrix()
  * VRCShader.SetGlobalMatrixArray()
  * VRCShader.SetGlobalTexture()
  * VRCShader.SetGlobalVector()
  * VRCShader.SetGlobalVectorArray()

# Additional shader globals

VRChat exposes a few special global shader variables you can use in any custom avatar or world shader. They are listed [here](/worlds/udon/vrc-graphics/vrchat-shader-globals).

---

## ドキュメント: vrc-camera-settings.md

```metadata
階層: /worlds/udon/vrc-graphics/vrc-camera-settings.md
ディレクトリ: worlds\udon\vrc-graphics
ファイル名: vrc-camera-settings.md
拡張子: .md
サイズ: 9.98 KB
最終更新: 2025-06-05T03:07:52.834Z
```

---
title: "VRC Camera Settings"
slug: "vrc-camera-settings"
hidden: false
createdAt: "2025-02-21T18:46:09.500Z"
updatedAt: "2025-02-21T18:46:09.500Z"
---

import UnityVersionedLink from '@site/src/components/UnityVersionedLink.js';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# VRCCameraSettings

Exposes information and limited control over the user's screen camera, the handheld camera, and Unity's global quality settings via APIs listed here.

You can access 2 instances of this class via the following static accessors:

- `VRCCameraSettings.ScreenCamera`: The user's screen camera, i.e. what is rendering their current view (will be a stereo camera if the user is in VR)
- `VRCCameraSettings.PhotoCamera`: The user's handheld photo camera. Note that properties on this instance will only update if the user has the camera enabled, you can use the `Active` property to check for that. Further note that this property will always point to the camera rendering the preview image, not the one used for taking pictures itself or rendering the view for the stream camera (although most properties will by synced from and to those whenever the Shutter is pressed).
  - ⚠️ In ClientSim, this property will be `null` as there is no photo camera available. In a real VRChat client, it will never be `null`.

:::note

These two properties may not always refer to a single real `Camera` component under the hood. VRChat abstracts away some of the complexity for you, and you should never need to worry about this. But for completeness, note the following:

- The [Focus View](/worlds/components/vrc_uishape/#focus-view) camera will be affected when changing properties on `VRCCameraSettings.ScreenCamera`.
- Similarly, the component used while the handheld camera is opened in "Stream Camera" mode will be affected when accessing `VRCCameraSettings.PhotoCamera`.
- The `VRCCameraSettings.PhotoCamera` is also linked to all cameras used in a Dolly Multi Cam setup, although some properties like `FieldOfView` may differ between them. In this case, the first camera's settings will be returned.

For advanced use-cases this state can be read via the [`CameraMode`](#camera-mode) property.

:::

### Exposed properties

These instances will expose certain properties of Unity's <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/ScriptReference/Camera.html">Camera</UnityVersionedLink> class (you can reference the Unity docs for detailed information on these properties). Note that for safety reasons, you will not be able to access the raw `Camera` components. The `Camera` class is exposed for components that you have placed into your world manually however.

Currently exposed as **read-only**:

- `Vector3 Position`: World-space position
- `Quaternion Rotation`: World-space rotation
- `Vector3 Forward`: World-space forward vector, convenience
- `Vector3 Up`: World-space up vector, convenience
- `Vector3 Right`: World-space right vector, convenience
- `int PixelWidth` and `int PixelHeight`: The current render target's size in pixels (may be affected by VR super-sampling settings)
- `float Aspect`: The current aspect ratio of the render target
- `float FieldOfView`: The camera's current _vertical_ field of view (this may be inaccurate for VR users)
- `bool Active`: If the camera is currently rendering. Will always be true for `ScreenCamera`, useful to detect the handheld camera via `PhotoCamera`. Also `true` for `PhotoCamera` while Spout streaming is enabled.
- `bool StereoEnabled`: `true` for `ScreenCamera` if the user is in VR. It is recommended to check the [Player API](/worlds/udon/players) to detect VR users instead.


:::note

Transform data like `Position` and `Rotation` will be all 0 if `Active` is `false`. `Forward/Up/Right` will return `Vector3.forward/up/right` respectively in that case.

:::

Currently exposed as **read-write**:

- `float NearClipPlane` and `float FarClipPlane`: You can adjust the camera's clipping planes at runtime. This follows similar restrictions to setting them via the Reference Camera in your scene descriptor:
  - `NearClipPlane` may be adjusted by the user's "Forced Camera Near Distance" settings. You can detect this happening by reading the value back after setting it.
  - `NearClipPlane` will be clamped between `0.001` and `0.05`
  - `FarClipPlane` can only be set to `0.1` higher than `NearClipPlane` at the lowest, this will be clamped
  - Adjusting the `FarClipPlane` may also adjust the `NearClipPlane` if the user has "Forced Camera Near Distance" set to "Dynamic"
- `bool AllowHDR`: If the camera will submit HDR values to the render target
- `DepthTextureMode DepthTextureMode`: Can be used to enable camera depth texture rendering, which is useful for certain shader effects. This can be used instead of a realtime-light with shadows enabled to force a camera depth texture. Note however, that having such a light in your scene will not change the `DepthTextureMode` property, meaning that reading a value of `None` from this property does _not_ mean there is no depth-light in the scene forcing a depth-pass to render regardless.
- `bool UseOcclusionCulling`: Whether or not the Camera will use occlusion culling during rendering. Defaults to `true`, but will only have an effect if your world has baked occlusion data.
- `bool AllowMSAA`: Disable all MSAA (anti-aliasing) on this camera if set to `false`, regardless of user settings. Defaults to `true`, where it will use the user's graphics settings.
- `CameraClearFlags ClearFlags`: Sets the background clear mode used when rendering this camera.
  - `Color BackgroundColor`: The color to use when `ClearFlags` is set to `SolidColor`.
- `bool LayerCullSpherical`: See <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/ScriptReference/Camera-layerCullSpherical.html">Unity Docs</UnityVersionedLink>.
- `float[] LayerCullDistances`: See <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/ScriptReference/Camera-layerCullDistances.html">Unity Docs</UnityVersionedLink>. Array must have 32 entries, corresponding to GameObject layers. A value of `0` for a layer means it will use the value of `FarClipPlane`. Setting this property to `null` is the same as passing in `0` for every layer.
  - Just like `FarClipPlane`, this will be clamped to a minimum of `NearClipPlane + 0.1`. Any `reserved` [layer](/worlds/layers) and `InternalUI` cannot be changed and will always read `0`.

### Camera Mode

The `CameraMode` property is available on the `ScreenCamera` and `PhotoCamera`.

`ScreenCamera` has the following camera modes:

| Mode        | Description                                                                                            |
|-------------|--------------------------------------------------------------------------------------------------------|
| Screen      | The default mode for rendering the user's current view.                                                |
| FocusView   | Active when the user is in [Focus View](/worlds/components/vrc_uishape/#focus-view) on mobile devices. |

`PhotoCamera` has the following camera modes:

| Mode            | Description                                                                   |
|-----------------|-------------------------------------------------------------------------------|
| PhotoOrVideo    | Camera is in Photo or Stream mode. Includes modes such as Emoji and Stickers. |
| Print           | Camera has the "Prints" skin active.                                          |
| DroneHandheld   | Camera is in Drone mode.                                                      |
| DroneFPV        | The user is flying the Drone in FPV mode.                                     |
| Unknown         | Set when `Active` is `false`.                                                 |

### Static functions

Mostly useful for VR users, 2 static functions are exposed on `VRCCameraSettings`:

* `Vector3 GetEyePosition(Camera.StereoscopicEye eye)`: Returns the specified eye's world space position. Equivalent to `ScreenCamera.Position` for non-VR users.
* `Quaternion GetEyeRotation(Camera.StereoscopicEye eye)`: Returns the specified eye's world space rotation. Equivalent to `ScreenCamera.Rotation` for non-VR users.

## OnChanged Event

When a user changes certain graphics settings, such as "Near Clip Override", the [OnVRCCameraSettingsChanged](/worlds/udon/graph/event-nodes/#onvrccamerasettingschanged) event is triggered.

This event may trigger every frame, or even multiple times per frame. It is recommended to do minimal processing to avoid affecting performance.

## Example

<Tabs groupId="udon-compiler-language">
<TabItem value="graph" label="Udon Graph">

![Udon graph that prints screen size and FOV on Start and whenever it changes.](/img/worlds/udon/vrc-graphics/basic-camera-settings-example.png)

</TabItem>
<TabItem value="cs" label="UdonSharp">

```cs
using TMPro;
using UdonSharp;
using UnityEngine;
using UnityEngine.UI;
using VRC.SDK3.Rendering;
​
public class CameraInfoDisplay : UdonSharpBehaviour
{
    [SerializeField] private TextMeshProUGUI info;

    void Start()
    {
        // call it once to initialize
        OnVRCCameraSettingsChanged(VRCCameraSettings.ScreenCamera);

        Debug.Log($"Started CameraInfoDisplay at resolution of {VRCCameraSettings.ScreenCamera.PixelWidth}x{VRCCameraSettings.ScreenCamera.PixelHeight}");
        Debug.Log($"The handheld photo camera is {(VRCCameraSettings.PhotoCamera.Active ? "enabled" : "disabled")}");
    }
​
    // will be called if resolution or a couple other properties change
    public override void OnVRCCameraSettingsChanged(VRCCameraSettings camera)
    {
        // ignore handheld photo camera
        if (camera != VRCCameraSettings.ScreenCamera)
            return;

        info.text = $"{camera.PixelWidth}x{camera.PixelHeight} fov={camera.FieldOfView} frame={Time.frameCount}°";
    }
}
```

</TabItem>
</Tabs>

---

## ドキュメント: vrc-quality-settings.md

```metadata
階層: /worlds/udon/vrc-graphics/vrc-quality-settings.md
ディレクトリ: worlds\udon\vrc-graphics
ファイル名: vrc-quality-settings.md
拡張子: .md
サイズ: 2.28 KB
最終更新: 2025-06-05T03:07:52.834Z
```

---
title: "VRC Quality Settings"
slug: "vrc-quality-settings"
hidden: false
createdAt: "2025-04-22T18:46:09.500Z"
updatedAt: "2025-04-22T18:46:09.500Z"
---

import UnityVersionedLink from '@site/src/components/UnityVersionedLink.js';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# VRCQualitySettings

A thin read-only layer over properties of <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/ScriptReference/QualitySettings.html">UnityEngine.QualitySettings</UnityVersionedLink>.

Currently exposed:

- `int AntiAliasing`
- `int PixelLightCount`
- `float LODBias`
- `int MaximumLODLevel`
- `ShadowResolution ShadowResolution`
- `float ShadowDistance`
- `int ShadowCascades`
- `int vSyncCount`

Some of these may be affected by the user's graphics settings.

### Shadow Distance

You can override the <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/shadow-distance.html">shadow distance</UnityVersionedLink> in your world using `VRCQualitySettings.SetShadowDistance()`. In VRChat, the value is clamped between `0.1f` and `10000.0f`.

`SetShadowDistance()` takes four float parameters: `low`, `medium`, `high`, and `mobile`. These correspond to the "Low", "Medium" and "High" shadow quality settings in the graphics settings menu. If you specify different values, the parameter that matches the local user's current setting is used. `mobile` applies to all non-PC platforms, regardless of settings. If you want to use the same value for all four parameters, use the overload method of `SetShadowDistance()` that accepts a single parameter.

Once you set a shadow distance using this property, the user's configured "Shadow Quality" setting will be overridden. The user will see a warning in their graphics settings. Be careful what you set as your shadow distance, since larger values can have heavy performance implications.

To disable the override and go back to the user configured shadow distance value, you can call `VRCQualitySettings.ResetShadowDistance()`.

## OnChanged Event

When a user changes graphics settings that affect an exposed property, the [OnVRCQualitySettingsChanged](/worlds/udon/graph/event-nodes/#onvrcqualitysettingschanged) event is triggered.

---

## ドキュメント: vrchat-shader-globals.md

```metadata
階層: /worlds/udon/vrc-graphics/vrchat-shader-globals.md
ディレクトリ: worlds\udon\vrc-graphics
ファイル名: vrchat-shader-globals.md
拡張子: .md
サイズ: 1.34 KB
最終更新: 2025-06-05T03:07:52.835Z
```

---
title: "VRChat Shader Globals"
slug: "vrchat-shader-globals"
hidden: false
createdAt: "2023-04-11T20:25:09.753Z"
updatedAt: "2023-04-11T20:28:55.746Z"
---
VRChat provides multiple global shader parameters Shader creators can use to implement VRChat-specific features.

The following shader globals are currently available:

- `float _VRChatCameraMode`:
  - `0` - Rendering normally
  - `1` - Rendering in VR handheld camera
  - `2` - Rendering in Desktop handheld camera
  - `3` - Rendering for a screenshot
- `uint _VRChatCameraMask` - The `cullingMask` property of the active camera, available if `_VRChatCameraMode != 0`
- `float _VRChatMirrorMode`:
  - `0` - Rendering normally, not in a mirror
  - `1` - Rendering in a mirror viewed in VR
  - `2` - Rendering in a mirror viewed in desktop mode
- `float3 _VRChatMirrorCameraPos` - World space position of mirror camera (eye independent, "centered" in VR), `(0,0,0)` when not rendering in a mirror
- `float3 _VRChatScreenCameraPos` - World space position of main screen camera
- `float4 _VRChatScreenCameraRot` - World space rotation (quaternion) of main screen camera
- `float3 _VRChatPhotoCameraPos` - World space position of handheld photo camera (first instance when using Dolly Multicam), `(0,0,0)` when camera is not active
- `float4 _VRChatPhotoCameraRot` - Look, you get the idea

---

# ドキュメント終了

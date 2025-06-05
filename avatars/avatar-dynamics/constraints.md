# constraints 統合ドキュメント

以下は自動収集された全ドキュメントのコンテンツです。各セクションの始めにメタデータが記載されています。



---

## ドキュメント: index.md

```metadata
階層: /avatars/avatar-dynamics/constraints/index.md
ディレクトリ: avatars\avatar-dynamics\constraints
ファイル名: index.md
拡張子: .md
サイズ: 13.68 KB
最終更新: 2025-06-05T03:07:52.730Z
```

# Constraints

VRChat provides its own custom constraints system, which allows bones on your avatar to move, rotate, and scale relative to other bones.

This is intended to be a like-for-like replacement of Unity's constraints system, with a few additional features based on how VRChat avatars typically use constraints. If you've never used constraints before, you may find it useful to refer to [Unity's documentation for constraints](https://docs.unity3d.com/Manual/Constraints.html).

You should use VRChat's constraints instead of Unity's when creating avatars. If your avatar contains any Unity constraints, they will be automatically converted into VRChat constraints when your avatar is loaded in-game, so using VRChat constraints directly will give you a more accurate representation of how your avatar will behave as well as a more accurate performance rank.

## Constraint Types

VRChat currently provides the following constraint types, which are designed to work in the same way as Unity's six built-in constraints:
- [**VRCAimConstraint**](/avatars/avatar-dynamics/constraints/vrc-aim-constraint) - Rotates the target transform so it aims towards the sources. You can customize which direction is treated as forwards.
- [**VRCLookAtConstraint**](/avatars/avatar-dynamics/constraints/vrc-look-at-constraint) - A simplified Aim Constraint that rotates the target transform to keep its positive Z axis facing towards the sources.
- [**VRCParentConstraint**](/avatars/avatar-dynamics/constraints/vrc-parent-constraint) - Moves and rotates the target transform as if it were a child of its sources.
- [**VRCPositionConstraint**](/avatars/avatar-dynamics/constraints/vrc-position-constraint) - Changes the position of the target transform to match the positions of its sources.
- [**VRCRotationConstraint**](/avatars/avatar-dynamics/constraints/vrc-rotation-constraint) - Changes the rotation of the target transform to match the rotations of its sources.
- [**VRCScaleConstraint**](/avatars/avatar-dynamics/constraints/vrc-scale-constraint) - Changes the scale of the target transform to match the scales of its sources.

Visit the links above for more information about the settings available for each constraint type.

## Advanced Constraint Settings

The Advanced Settings foldout contains some additional functions provided by VRChat constraints.

### Target Transform

The `Target Transform` setting allows you to change the transform targeted by this constraint. By default, this setting is empty, and the constraint is applied to the transform that the constraint component is attached to. Note that changing this transform with an animation is not possible.

This may be useful if you'd like to keep all of the constraint components on your avatar in one place, or if you're setting up a system that uses constraints and you want it to be transferrable between different avatars.

### Solve In Local Space

Normally, a constraint is solved in world space, which means it will match the **world** position/rotation/scale of its sources. If the `Solve In Local Space` option is enabled, the constraint will match the **local** position/rotation/scale of its sources instead.

This can be useful in situations such as setting up additional fake limbs for avatars. You might, for example, have a chain of locally solved rotation constraints that refer to each bone of the avatar's real arm, which would then cause that chain to rotate around as the real arm does. This isn't limited to rotation constraints, however - all types can use local solving.

The video below illustrates the difference between globally and locally solved rotation constraints as an example. In this clip, the middle and right arrows each use rotation constraints to match the rotation of the left arrow, where the middle arrow uses world space solving and the right arrow uses local space solving. Notice how the world solved constraint always matches the rotation of the target in world space. In contrast, the locally solved constraint always matches the direction the target is facing relative to its parent bone.

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/6iBJ5QntrMU?si=YxAkg17x3LvinnL_" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### Freeze To World

When `Freeze To World` is enabled, the constraint ignores all of its sources and locks its target transform in world space. The transform's position/rotation/scale will stay the same until `Freeze To World` is disabled.

This setting works best when animated on and off. For example:

 1. Set up an [expressions toggle](/avatars/expression-menu-and-controls/#types-of-controls) for `Freeze To World` and disable it by default.
 2. When `Freeze To World` is enabled in your Animation Clip, the transform is locked in world space.
 3. When `Freeze To World` is disabled in your Animation Clip, the transform follows the constraint's sources again.

This allows avatars to drop an object at a fixed position in the world. When you walk away, the object won't follow your avatar. Parent constraints are most suitable for this because they can freeze both the position _and_ rotation of the target transform. However, `Freeze To World` is also available for all the other constraint types.

The `Freeze To World` property only affects axes that are selected as frozen in the Constraint Settings section. You must freeze all axes if you want to stop the object in place completely. Otherwise, those axes will not be updated and the transform will not remain locked in world space.

:::note

Enabling `Freeze To World` is not the same as disabling the constraint component!

- When the constraint is disabled, the affected transform stops moving in **local** space. It still follows your avatar movement.
- When you enable `Freeze To World`, the constraint actively moves the transform in local space to prevent it from moving in **world** space.

Additionally, having `Freeze To World` turned on by default means the constraint will freeze itself at its starting position/rotation/scale relative to your avatar the instant it loads in. This is not a guaranteed way to place it at a certain position/rotation/scale in world space.

:::

#### Rebake Offsets When Unfrozen

When `Rebake Offsets When Unfrozen` is enabled, the constraint will recalculate its offset relative to its sources when it is unfrozen by having `Freeze To World` disabled, instead of the usual behavior of keeping its original offset.

Toggling this value itself has no effect - it just determines what should happen when `Freeze To World` is disabled.

## Performance

There are two performance categories related to constraints. Refer to the [Performance Ranks](/avatars/avatar-performance-ranking-system#avatar-performance-ranks---value-maximums-per-rank) page for the limits applied to each platform.

### Constraint Count

The constraint count is fairly straightforward - it's the total number of constraints attached to your avatar, including disabled constraints. This includes both VRChat constraints and Unity constraints. Unity constraints are automatically converted into VRChat constraints when your avatar is loaded in-game.

Decreasing the total number of constraints can improve your avatar's performance.

### Constraint Depth

When you set up a chain of constraints on your avatar (for example, a chain of rotation constraints along an extra limb), those constraints need to be evaluated one at a time in a specific order, running from the constraint at the base of the chain up to the constraint at the tip. If the chain has 3 constraints along it, then that means the chain has a constraint depth of 3. The avatar's overall constraint depth rating is the longest chain of dependencies across the entire avatar.

Constraint depth can be lowered by reducing the length of the longest chain of constraints. Keep in mind that this category searches for the *longest* chain rather than the sum of all chains - if your avatar has several arms that all have a depth of 3, the overall score for the avatar would still be 3 (assuming there are no longer chains anywhere else on the avatar).

Constraint depth is important because it gives a better estimate of how the constraints on an avatar will perform than just counting how many constraints there are in total. By organizing constraints to minimize the dependencies between them, many of them will be able to run at the same time, which results in better performance compared to situations where the constraints must run one after another.

:::info[Constraint Depth with Unity Constraints]

The constraint depth of an avatar can only be accurately calculated when it uses VRChat constraints exclusively.

If your avatar contains any Unity constraints, the constraint depth is likely to be over-estimated, as the category assumes that Unity constraints can only run one at a time. You can convert all of the Unity constraints on your avatar into equivalent VRChat constraints by using the relevant Auto Fix option in the control panel in the SDK.

:::

## Usage Tips

- If your constraint doesn't seem to work, make sure it's actually running.
	- The `Is Active` option should be enabled.
	- The component itself should be enabled (the tick-box next to its name) and should be attached to an active game object on your avatar.
	- Make sure the `Lock` option is enabled. Otherwise, the constraint will re-evaluate its `At Rest` and `Offset` values instead of affecting the transform.
- The `Target Transform` reference cannot be changed by animations. This is because changing the transform targeted by the constraint would require recalculating the avatar's performance rank. If you want to change the targeted transform of a constraint, you could instead try toggling between several different constraint components each with a different target transform.
- If you can avoid it, don't use animations to change which transforms are referenced by your constraints. Animating a transform reference can cause performance issues for your avatar, as the constraints may need to be re-evaluated each time the references change.
    - This specifically refers to animating a constraint's _reference_ to a transform - animating the transform's position, rotation and scale is okay!
	- It isn't possible to animate transform references for individual sources due to technical limitations. As a simpler alternative to this, you can set up several sources with different transforms and animate their weights instead.

## Editor Tooling Info

If you're an advanced Unity user, you can write your own custom editor tooling in C# that interacts with the constraints converter.

These utilities are only briefly summarized here. Please see the inline documentation for them for full descriptions of how they work.

### Conversion Methods

The SDK class `AvatarDynamicsSetup` contains the conversion methods that the SDK uses to translate Unity constraints into equivalent VRChat constraints. The following constraint conversion methods are exposed for user tooling:
| Method | Description |
|---|---|
| `ConvertUnityConstraintsAcrossGameObjects(List<GameObject> targetGameObjects)` | Converts Unity constraints on a list of GameObjects into VRChat constraints. |
| `ConvertUnityConstraintsAcrossAnimationClips(List<AnimationClip> targetAnimationClips)` | Modifies a list of AnimationClips so any tracks in them targeting Unity constraints are updated to target VRChat constraints instead. |
| `DoConvertUnityConstraints(IConstraint[] unityConstraints, VRCAvatarDescriptor avatarDescriptor, bool convertReferencedAnimationClips)` | Converts an array of Unity constraints into VRChat constraints, optionally including any referenced animation clips. This runs immediately with no confirmation dialog. |
| `RebindConstraintAnimationClip(AnimationClip clip, IConstraint oldConstraint)` | Attempts to modify a single animation clip to retarget tracks from Unity constraints to VRChat constraints, optionally limiting conversions to the given Unity constraint. |
| `TryGetSubstituteAnimationBinding(Type unityConstraintType, string unityConstraintPropertyName, out Type vrcConstraintType, out string vrcConstraintPropertyName, out bool isArrayProperty)` | Attempts to translate a Unity constraint property name and type into an equivalent VRChat constraint property name and type. |

### Delegate Functions

To complement the above methods, the class `AvatarDynamicsSetup` also provides delegates that allow your tooling to control how the converter behaves. The following delegates are available:
| Delegate | Description |
|---|---|
| `bool IsUnityConstraintAutoConverted(IConstraint constraint)` | Given a Unity constraint, return `true` if this constraint will be converted into a VRChat constraint at build time by user tooling. You can use this to suppress the validation warning normally generated by the SDK prompting the user to convert their Unity constraints to VRChat constraints. |
| `bool ConvertUnityConstraintsAcrossGameObjects(List<GameObject> gameObjects, bool isAutoFix)` | Given a list of GameObjects, convert all of the constraints and underlying animation clips on them into VRChat constraints. The `isAutoFix` parameter is set to `true` if this was triggered by the user clicking auto-fix in the validations list, or `false` if it was triggered by a menu entry or a custom user script. Return `true` to prevent the native SDK converter from running. |
| `bool ConvertUnityConstraintsAcrossAnimationClips(List<AnimationClip> animationClips)` | Given a list of animation clips, update all tracks referencing Unity constraints to reference VRChat constraints instead. Return `true` to prevent the native SDK converter from running. |


---

## ドキュメント: vrc-aim-constraint.mdx

```metadata
階層: /avatars/avatar-dynamics/constraints/vrc-aim-constraint.mdx
ディレクトリ: avatars\avatar-dynamics\constraints
ファイル名: vrc-aim-constraint.mdx
拡張子: .mdx
サイズ: 1.64 KB
最終更新: 2025-06-05T03:07:52.730Z
```

import ConstraintMainSettings from './_partial-constraint-mainsettings.mdx'
import ConstraintActivation from './_partial-constraint-activation.mdx'
import ConstraintSubSettings from './_partial-constraint-subsettings.mdx'
import ConstraintSources from './_partial-constraint-sources.mdx'
import ConstraintAdvanced from './_partial-constraint-advanced.mdx'

# Aim Constraint

The `VRCAimConstraint` component rotates the target transform so it aims towards the sources. You can customize which direction is treated as forwards.

![](/img/avatars/constraints/aim.png)

<ConstraintMainSettings />
- **Aim Vector** - Controls the axes that should face towards the sources.
- **Up Vector** - Controls the upwards axis for this constraint. This is the axis of the affected transform that the constraint will try to align with the world up axis defined below.
- **World Up Type** - Controls the world upwards direction used by this aim constraint.
  - **Scene Up** - Treat upwards as the positive Y axis of the scene.
  - **Object Up** - Treat upwards as the vector pointing from the target transform of the constraint to the transform specified in the `World Up Object` property.
  - **Object Up Rotation** - Treat upwards as the `World Up Vector` axis in the local space of the transform specified in the `World Up Object` property.
  - **Vector** - Treat upwards as the `World Up Vector` in world space.
  - **None** - Do not define an upwards direction.

<ConstraintActivation />

## Constraint Settings

<ConstraintSubSettings affectedTrs="Rotation" />

## Sources

<ConstraintSources />

## Advanced

<ConstraintAdvanced affectedTrs="Rotation" />

---

## ドキュメント: vrc-look-at-constraint.mdx

```metadata
階層: /avatars/avatar-dynamics/constraints/vrc-look-at-constraint.mdx
ディレクトリ: avatars\avatar-dynamics\constraints
ファイル名: vrc-look-at-constraint.mdx
拡張子: .mdx
サイズ: 1.78 KB
最終更新: 2025-06-05T03:07:52.731Z
```

import ConstraintMainSettings from './_partial-constraint-mainsettings.mdx'
import ConstraintActivation from './_partial-constraint-activation.mdx'
import ConstraintSources from './_partial-constraint-sources.mdx'
import ConstraintAdvanced from './_partial-constraint-advanced.mdx'

# Look At Constraint

The `VRCLookAtConstraint` component is a simplified [Aim Constraint](/avatars/avatar-dynamics/constraints/vrc-aim-constraint). It rotates the target transform so its positive Z axis faces towards the sources.

![](/img/avatars/constraints/look-at.png)

<ConstraintMainSettings />
- **Use Up Object** - Controls whether or not to use a separate transform to determine roll.
- **Roll** - Controls the angle (in degrees) around the target transform's Z axis that should be used to determine its upwards direction. Only available when `Use Up Object` is disabled.
- **World Up Object** - When set, the constraint will roll to try and point its positive Y axis towards this transform. Only available when `Use Up Object` is enabled.

<ConstraintActivation />

## Constraint Settings <!--Not partial because look-at does not expose freeze axes.-->

- **Rotation At Rest** - Defines the transform's rotation when the constraint's overall weight is 0.
- **Rotation Offset** - Defines the offset applied to the result of this constraint.
- **Lock** - When enabled, locks the `At Rest` and `Offset` values in place, preventing them from being edited.
  - If you activate the constraint and leave these values unlocked, they'll be recalculated as the target transform's rotation changes.
  - If you lock these values and activate the constraint, the constraint will start taking control of the transform.

## Sources

<ConstraintSources />

## Advanced

<ConstraintAdvanced affectedTrs="Rotation" />

---

## ドキュメント: vrc-parent-constraint.mdx

```metadata
階層: /avatars/avatar-dynamics/constraints/vrc-parent-constraint.mdx
ディレクトリ: avatars\avatar-dynamics\constraints
ファイル名: vrc-parent-constraint.mdx
拡張子: .mdx
サイズ: 894 B
最終更新: 2025-06-05T03:07:52.731Z
```

import ConstraintMainSettings from './_partial-constraint-mainsettings.mdx'
import ConstraintActivation from './_partial-constraint-activation.mdx'
import ConstraintSubSettings from './_partial-constraint-subsettings.mdx'
import ConstraintSources from './_partial-constraint-sources.mdx'
import ConstraintAdvanced from './_partial-constraint-advanced.mdx'

# Parent Constraint

The `VRCParentConstraint` component moves and rotates the target transform as if it were a child of its sources.

![](/img/avatars/constraints/parent.png)

<ConstraintMainSettings />

<ConstraintActivation />

## Constraint Settings

<ConstraintSubSettings affectedTrs="Position/Rotation" />
:::info

Offsets are set individually for each source when using Parent Constraints.

:::

## Sources

<ConstraintSources />

## Advanced

<ConstraintAdvanced affectedTrs="Position/Rotation" />

---

## ドキュメント: vrc-position-constraint.mdx

```metadata
階層: /avatars/avatar-dynamics/constraints/vrc-position-constraint.mdx
ディレクトリ: avatars\avatar-dynamics\constraints
ファイル名: vrc-position-constraint.mdx
拡張子: .mdx
サイズ: 794 B
最終更新: 2025-06-05T03:07:52.731Z
```

import ConstraintMainSettings from './_partial-constraint-mainsettings.mdx'
import ConstraintActivation from './_partial-constraint-activation.mdx'
import ConstraintSubSettings from './_partial-constraint-subsettings.mdx'
import ConstraintSources from './_partial-constraint-sources.mdx'
import ConstraintAdvanced from './_partial-constraint-advanced.mdx'

# Position Constraint

The `VRCPositionConstraint` component changes the position of the target transform to match the positions of its sources.

![](/img/avatars/constraints/position.png)

<ConstraintMainSettings />

<ConstraintActivation />

## Constraint Settings

<ConstraintSubSettings affectedTrs="Position" />

## Sources

<ConstraintSources />

## Advanced

<ConstraintAdvanced affectedTrs="Position" />

---

## ドキュメント: vrc-rotation-constraint.mdx

```metadata
階層: /avatars/avatar-dynamics/constraints/vrc-rotation-constraint.mdx
ディレクトリ: avatars\avatar-dynamics\constraints
ファイル名: vrc-rotation-constraint.mdx
拡張子: .mdx
サイズ: 794 B
最終更新: 2025-06-05T03:07:52.731Z
```

import ConstraintMainSettings from './_partial-constraint-mainsettings.mdx'
import ConstraintActivation from './_partial-constraint-activation.mdx'
import ConstraintSubSettings from './_partial-constraint-subsettings.mdx'
import ConstraintSources from './_partial-constraint-sources.mdx'
import ConstraintAdvanced from './_partial-constraint-advanced.mdx'

# Rotation Constraint

The `VRCRotationConstraint` component changes the rotation of the target transform to match the rotations of its sources.

![](/img/avatars/constraints/rotation.png)

<ConstraintMainSettings />

<ConstraintActivation />

## Constraint Settings

<ConstraintSubSettings affectedTrs="Rotation" />

## Sources

<ConstraintSources />

## Advanced

<ConstraintAdvanced affectedTrs="Rotation" />

---

## ドキュメント: vrc-scale-constraint.mdx

```metadata
階層: /avatars/avatar-dynamics/constraints/vrc-scale-constraint.mdx
ディレクトリ: avatars\avatar-dynamics\constraints
ファイル名: vrc-scale-constraint.mdx
拡張子: .mdx
サイズ: 773 B
最終更新: 2025-06-05T03:07:52.731Z
```

import ConstraintMainSettings from './_partial-constraint-mainsettings.mdx'
import ConstraintActivation from './_partial-constraint-activation.mdx'
import ConstraintSubSettings from './_partial-constraint-subsettings.mdx'
import ConstraintSources from './_partial-constraint-sources.mdx'
import ConstraintAdvanced from './_partial-constraint-advanced.mdx'

# Scale Constraint

The `VRCScaleConstraint` component changes the scale of the target transform to match the scales of its sources.

![](/img/avatars/constraints/scale.png)

<ConstraintMainSettings />

<ConstraintActivation />

## Constraint Settings

<ConstraintSubSettings affectedTrs="Scale" />

## Sources

<ConstraintSources />

## Advanced

<ConstraintAdvanced affectedTrs="Scale" />

---

## ドキュメント: _partial-constraint-activation.mdx

```metadata
階層: /avatars/avatar-dynamics/constraints/_partial-constraint-activation.mdx
ディレクトリ: avatars\avatar-dynamics\constraints
ファイル名: _partial-constraint-activation.mdx
拡張子: .mdx
サイズ: 305 B
最終更新: 2025-06-05T03:07:52.728Z
```

The two buttons at the top the inspector are utilities to let you:
- **Activate** the constraint. This saves the current offset from the sources, then activates and locks the constraint.
- **Zero out** the constraint. This resets the offset to its default value, then activates and locks the constraint.

---

## ドキュメント: _partial-constraint-advanced.mdx

```metadata
階層: /avatars/avatar-dynamics/constraints/_partial-constraint-advanced.mdx
ディレクトリ: avatars\avatar-dynamics\constraints
ファイル名: _partial-constraint-advanced.mdx
拡張子: .mdx
サイズ: 792 B
最終更新: 2025-06-05T03:07:52.729Z
```

- **Target Transform** - Defines the transform affected by this constraint component. If left empty, the transform this constraint is attached to will be affected instead.
- **Solve In Local Space** - If ticked, this constraint will be solved as if the sources are in local space rather than world space.
- **Freeze To World** - If ticked, this constraint will ignore its sources and keep a fixed {props.affectedTrs.toLowerCase()} instead.
- **Rebake Offsets When Unfrozen** - If ticked, this constraint will rebake its offsets relative to its sources when `Freeze To World` is disabled.

See the parent section covering [Advanced Constraint Settings](/avatars/avatar-dynamics/constraints#advanced-constraint-settings) for a more in-depth description of how these advanced settings work.

---

## ドキュメント: _partial-constraint-mainsettings.mdx

```metadata
階層: /avatars/avatar-dynamics/constraints/_partial-constraint-mainsettings.mdx
ディレクトリ: avatars\avatar-dynamics\constraints
ファイル名: _partial-constraint-mainsettings.mdx
拡張子: .mdx
サイズ: 416 B
最終更新: 2025-06-05T03:07:52.729Z
```

- **Is Active** - Controls whether the constraint will be evaluated or not. Disabling the entire component and deactivating the game object with the component on it will also stop the constraint from running.
- **Weight** - Controls the overall weight applied to this constraint.
  - This should normally be in the range 0 to 1, but you can tick the `Free edit` box if you want to set values outside of that range.

---

## ドキュメント: _partial-constraint-sources.mdx

```metadata
階層: /avatars/avatar-dynamics/constraints/_partial-constraint-sources.mdx
ディレクトリ: avatars\avatar-dynamics\constraints
ファイル名: _partial-constraint-sources.mdx
拡張子: .mdx
サイズ: 505 B
最終更新: 2025-06-05T03:07:52.729Z
```

The "Sources" list determines which transforms affect this constraint. Click the `+` button in the bottom right to add a new source. Click the `-` button to remove the currently selected source. Each source has the following options:
- **Source Transform** - The transform to use as a source.
- **Weight** - Controls how much this source should affect the constraint.
  - This should normally be in the range 0 to 1, but you can tick the `Free edit` box if you want to set values outside of that range.

---

## ドキュメント: _partial-constraint-subsettings.mdx

```metadata
階層: /avatars/avatar-dynamics/constraints/_partial-constraint-subsettings.mdx
ディレクトリ: avatars\avatar-dynamics\constraints
ファイル名: _partial-constraint-subsettings.mdx
拡張子: .mdx
サイズ: 804 B
最終更新: 2025-06-05T03:07:52.730Z
```

- **{props.affectedTrs} At Rest** - Defines the transform's {props.affectedTrs.toLowerCase()} when the constraint's overall weight is 0.
- **{props.affectedTrs} Offset** - Defines the offset applied to the result of this constraint.
- **Lock** - When enabled, locks the `At Rest` and `Offset` values in place, preventing them from being edited.
  - If you activate the constraint and leave these values unlocked, they'll be recalculated as the target transform's {props.affectedTrs.toLowerCase()} changes.
  - If you lock these values and activate the constraint, the constraint will start taking control of the transform.
- **Freeze {props.affectedTrs} Axes** - Allows you to exclude specific axes from evaluation as the constraint is solved. By default, all three axes are selected for evaluation.

---

# ドキュメント終了

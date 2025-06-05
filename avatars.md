# avatars 統合ドキュメント

以下は自動収集された全ドキュメントのコンテンツです。各セクションの始めにメタデータが記載されています。



---

## ドキュメント: avatar-impostors.md

```metadata
階層: /avatars/avatar-impostors.md
ディレクトリ: avatars
ファイル名: avatar-impostors.md
拡張子: .md
サイズ: 5.71 KB
最終更新: 2025-06-05T03:07:52.733Z
```

---
title: "Impostors"
slug: "avatar-impostors"
hidden: false
---

# Impostors
## What are Impostors?
Impostors are avatar body doubles, allowing you to see avatars in situations where you would typically see a fallback avatar or robot. They are intended to bridge gaps between different systems/user types in VRChat. For example, if a user’s avatar was only uploaded for Windows, VRChat makes an acceptable impostor for Quest automatically. You can only generate impostors for avatars you own and have uploaded.

Even if you've never uploaded a cross-platform version of your avatar, or if your avatar is "performance blocked" due to its performance rank, other users will still be able to see an impostor of your avatar. Eventually, impostors will be auto-generated, but for now, you create them with a few easy steps. Once you've made an impostor, you can toggle it on and off - when it's off, your fallback avatar will be shown instead.

## How do I create an Impostor?
The first step to creating an impostor is to [create and upload an avatar](/avatars/creating-your-first-avatar).

Once you've uploaded an avatar, creating an impostor for it is easy:

- Log in to the VRChat website.

- Navigate to the info page for the avatar you'd like to impostorize. You can do this by pressing "Avatars", then "My Avatars", then the name and icon of the one you want.

- Click "Generate Impostors", or, if the avatar already has an impostor that you'd like to be updated, "Regenerate Impostors"

- Wait.

- Refresh the page, after some time you should now see that your avatar has impostors for Quest and PC.

![A screnshot of an avatar's page on vrchat.com. It allows avatar creators to (re)generate impostors and see which impostors have already been generated. You can see if impostors have been generated for PC and/or Android. You can also see if the impostor has been customized by the avatar creator.](/img/avatars/impostors/generation.png)

:::note

Impostors currently only support [humanoid](https://docs.unity3d.com/Manual/AvatarCreationandSetup.html) avatars. [Generic](https://docs.unity3d.com/Manual/GenericAnimations.html) avatars will be supported in the future.

:::

## Previewing an Impostor
Once you've got your impostor generated, you're probably going to be pretty excited to see how it looks! Well, no worries, we've got you covered!

Once you've logged into VRChat, open the Avatar menu, and click the avatar that you generated an impostor for.

You should notice that the "Features" of the avatar now includes "Impostor". 

![A screenshot of the "Features" section of an avatar in VRChat. It shows an "Imposter" icon, among other features.](/img/avatars/impostors/features-row.png)


You should also see a new button underneath the avatar model preview, which will allow you to switch between viewing the impostor and the normal avatar for a quick preview.

![A screenshot of an avatar being previewed in the VRChat menu.](/img/avatars/impostors/preview-avatar.png)
![A screenshot of an avatar's imposter being previewed in the VRChat menu. A toggle near the bottom has been enabled.](/img/avatars/impostors/preview-impostor.png)

:::note
Impostors that are previewed in the menu may exhibit more artifacts than they would when viewed on another player.
:::

## VRCImpostorSettings

Impostors come out pretty good by default. However, complex avatars may benefit from some customization.

To customize your impostor, add the "VRCImpostorSettings" component to your avatar before uploading it. Changing the settings of this component allows you to change the impostor's appearance. You can add multiple "VRCImpostorSettings" to customize different body parts.

### Resolution Scale

Changes the amount of space on the impostors texture atlas that is dedicated to this body part's texture.
For instance, you can place this script on the head bone and change this value to make the head take up more or less of the texture atlas, increasing or decreasing the overall texture quality.
Note that this may shrink other parts of the body on the atlas if needed. 


_This is relative to the bone that VRCImpostorSettings is placed on._


### Transforms To Ignore
Ignores these transforms when capturing data for the impostor. This will hide them from the final result.

_This is independent of the bone that VRCImpostorSettings is placed on._

### Extra Child Transforms
This is good for things like wings and tails, it will tell the Impostorizer to make a separate sprite for the bone this script is on.

As an example of what not to do - you _could_ put one of these on each finger to turn them into independent sprites. However, since all sprites share a single texture sheet, filling it with things like fingers will cause quality to decrease elsewhere - it's a balancing act.

_This is independent of the bone that VRCImpostorSettings is placed on._

### Re-parent Here
Re-parents another bone to this impostor sprite. This means that it will be impostorized with this body part, and be a part of that sprite.

For instance, if you'd like your wings to be a part of the upper body, you can re-parent the root wing bone to the chest bone during impostorization with this.

_This is relative to the bone that VRCImpostorSettings is placed on._

## When is an impostor visible?
Currently, there are only three ways to see an impostor.

- Avatar Preview (e.g. viewing the impostor on the avatar's details page)
- Performance Blocking (e.g. the avatar's performance rank is "Very Poor" but your [minimum displayed performance rank](https://docs.vrchat.com/docs/vrchat-configuration-window#minimum-displayed-performance-rank) is set to "Medium").
- Platform Mismatch (e.g. the avatar is uploaded for PC, but you're on Android or vice versa)


---

## ドキュメント: avatar-optimizing-tips.md

```metadata
階層: /avatars/avatar-optimizing-tips.md
ディレクトリ: avatars
ファイル名: avatar-optimizing-tips.md
拡張子: .md
サイズ: 14.50 KB
最終更新: 2025-06-05T03:07:52.733Z
```

# Avatar Optimization Tips

:::caution

**This guide is not meant to be the end-all, be-all of avatar optimization!** Optimizing your avatar properly requires pretty wide knowledge of a ton of things. We don't expect everyone to know everything.

However, we try our best to keep this document updated with the most common things people miss, and the most important targets to hit.

If you have input on optimization tips, please use the **Suggest Edits** button in the top right and add your own!
:::
Do you want your avatar to be efficient and be loved by everyone because of all the frames you're saving them? Follow these tips and you should be good!

Any recommended numbers or limits in this document are subject to change at any time. Although some of the descriptions provided below are not precise in a technical manner, this document is intended to assist novice users in learning how to optimize their avatars.

Community-created blender plugins like [Cats](https://catsblenderplugin.xyz/) or [Tuxedo](https://github.com/feilen/tuxedo-blender-plugin) allow users to very easily optimize their models and assist with common VRChat avatar problems. We strongly recommend using tools like this! It makes your job easier, and improves performance for all.

As a sidenote, the SDK's Build Control panel provides numbers of components on avatars to help with optimization.

## Optimize your content for Android/Quest

When developing content for Android, please keep the [Content Limitations](/platforms/android/quest-content-limitations) in mind! For example, avatars don't have access to all shaders and avatar components.

In addition, you should [optimize your content for Android](/platforms/android/quest-content-optimization). This improves your avatar's [performance rank](/avatars/avatar-performance-ranking-system) and allows more players to see your avatar.

## Do not use Dynamic Bones!
Dynamic Bones is a Unity Asset that you can purchase that allows you to define bones on your avatar's rig to move around as if they were hanging. You can also define static forces like gravity which can make hair fall more realistically. 

Dynamic Bones is deprecated and will be removed eventually. Use [PhysBones](/avatars/avatar-dynamics/physbones) instead.

VRChat will automatically convert Dynamic Bones to PhysBones at runtime.

## Do not use Unity Constraints!
[Unity Constraints](https://docs.unity3d.com/Manual/Constraints.html) are components provided by the engine that allow you to change the position, rotation and scale of transforms on your avatar based on one or more other transforms.

The engine's constraints are sorted based on the dependencies between them every frame, which means they can cause significant performance problems when enough of them exist at once. Use [VRChat Constraints](/avatars/avatar-dynamics/constraints) instead, because they're designed to provide better performance in the context of VRChat avatars.

VRChat will automatically convert Unity constraints to VRChat constraints at runtime.

## Limit usage of Cloth
Cloth is a default Unity component that has a similar cost to Dynamic Bones and is more difficult to set up. Limit your use of Cloth heavily, and do not apply it to meshes that have greater than 200 or so vertices.

## Reduce the amount of meshes on your avatar
There's two types of Mesh Renderers that your avatar could have on it-- Static Mesh Renderers and Skinned Mesh Renderers. Static Meshes do not deform. Skinned Meshes, however, usually have rigs (bones) that tell the engine how to move and deform the mesh based on the position of the bones. These Skinned Meshes are significantly more expensive, and you should only have one skinned Mesh Renderer on your avatar. There is very little reason to have more than one-- most of the time, additional items can be built into the original model.

On top of that, each additional mesh on your avatar incurs one or more additional "Draw Calls"-- essentially, time spent by your processor telling your graphics card to draw something on the screen.

Therefore, **VRChat recommends that you have one Skinned Mesh Renderer at maximum, and 3 static mesh renderers at maximum.** Merging meshes together is very simple in Blender, and is shown in the Meshes video below.

Finally, ensure that you're not using an excessive amount of triangles. The SDK will warn you if you're trying to upload a model that exceeds 70,000 triangles for PC or 20,000 on Quest. It is very rare that you need even this many polygons for details-- look into baking a normal map and simplifying your mesh via decimation or retopology.

Creating avatars for the Quest can be more challenging due to the reduced limits. The most effective optimization tends to occur during initial design and avatar creation. In other words, you're going to have problems if you try to take a 120,000 made-for-rendering model and squeeze it into 20,000 polygons. Don't make things harder than they have to be-- find a model that starts low! 20k is quite a large amount of leeway.

Notably if you are using Cats Blender Plugin, it merges meshes automatically when you "Fix Model". **If you seperate meshes by Material or by Loose Parts using Cats to assist with decimation or editing, do not forget to merge the meshes again.** 
<iframe class="embedly-embed" src="//cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fwww.youtube.com%2Fembed%2F1fco-G2j0Jg%3Ffeature%3Doembed&url=http%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3D1fco-G2j0Jg&image=https%3A%2F%2Fi.ytimg.com%2Fvi%2F1fco-G2j0Jg%2Fhqdefault.jpg&key=f2aa6fc3595946d0afc3d76cbbd25dc3&type=text%2Fhtml&schema=youtube" width="854" height="480" scrolling="no" frameborder="0" allow="autoplay; fullscreen" allowfullscreen="true"></iframe>

## Reduce the amount of material slots you use
Each additional material slot is also a draw call, which eats more processor time! If you have a lot of materials (more than 10), look into Texture Atlasing. With Community-created tools, atlasing is exceedingly easy. Check out the Materials video for more details.

As an aside, what is important is the number of **material slots on the Renderer components** in your avatar. If you have the same material in 20 slots, you still technically have 20 "materials". 

This is due to the way that Unity splits meshes into submeshes. What really matters for performance is the number of submeshes created, which Unity creates based on Material slots.

<iframe width="854" height="480" src="https://www.youtube-nocookie.com/embed/5LwRi26RxSQ?si=_TuNCYuWLrsWrVIm" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share; fullscreen" allowfullscreen="true"></iframe>

## Watch your VRAM usage!

Even if you use texture atlases, you might end end up using more VRAM than you did before!

Textures eat up VRAM. The higher the resolution of each texture, the more VRAM it consumes. Avoid using several high-resolution textures, or reduce their size by reducing the "Max Size" parameter in [Unity's import settings](https://docs.unity3d.com/Manual/class-TextureImporter.html). 

For example: A 30 MB avatar *can* use 3 GB of VRAM if it uses inefficient high-resolution textures. Don't be fooled by an avatar's download size.

Check out [Poiyomi's Texture Optimization guide](https://www.poiyomi.com/blog/2022-10-17-texture-optimization). It's an excellent and comprehensive guide on how to optimize your avatar's textures.

## Use Optimized Shaders
Some shaders can cause excessive time spent rendering on the GPU. Try to stick with the Unity Standard shaders, or shaders that you know perform well. If you don't know how to tell if a shader is well-optimized, that's fine! Here are some examples-- these certainly aren't all the shaders available, but are all well-made and well-optimized with a variety of features.
* VRChat's SDK includes various shaders optimized for mobile devices, such as 'Standard Lite.'
* [Xiexe's "XSToon" Unity Shaders](https://github.com/Xiexe/Xiexes-Unity-Shaders) (MIT) - A collection of PBR 'Toon' shaders for Unity.
* [Silent's Shaders](https://gitlab.com/s-ilent/SCSS) (MIT) - Shaders for Unity for cel shading, originally based off the discontinued CubedParadox's Flat Lit Toon, featuring a number of handy features.
* [Poiyomi's Toon Shader](https://github.com/poiyomi/PoiyomiToonShader/releases) (MIT) - A very robust, powerful shader with a lot of options. 

### Minimize Excess Shader Passes
Speaking more technically, you want to avoid shaders that have excess shader passes. This incurs additional draw calls. This might be a bit too much for most users to worry about, so if you stick with commonly used and proven community shaders, that should suffice.

### Avoid Tessellation
**You should always avoid using shaders on avatars that use Tessellation.** This is very common in "fur" shaders. [Tessellation](https://en.wikipedia.org/wiki/Tessellation_%28computer_graphics%29) is a method by which your graphics card can take meshes and subdivide them for various effects. However, this effect is **extremely expensive** and will slow down even the most powerful of graphics cards. **Do not use shaders with tessellation effects.** If you want a fur effect, consider looking into shaders that reproduce the effect without tessellation, such as [XSFur](https://xiexe.booth.pm/items/1084711) and [Warren's Fast Fur Shader](https://warrenwolfy.gumroad.com/l/atntv).

### Keywords
You should also avoid shaders that use excessive amounts of shader keywords. This can cause serious and unpredictable issues with rendering your view in VRChat, and will fill your output log with a lot of redundant error messages. There is no need to include excessive shader keywords in your shader, so please only use the ones that are required for the features you are targeting.

#### Clearing Keywords
When you change or upgrade your shader, ensure that you remove old, unused keywords from your materials. Having excessive keywords in use is very bad for performance and optimization. Not only will it cause issues with your own avatar, but it may prevent others from seeing **all shaders** properly.

The VRChat SDK contains a tool to remove keywords from materials on your avatar. This tool can also remove keywords you need, so be careful!

Usually, it is best to check the keywords with this tool-- **if you've got too many keywords, you probably need to find another shader.** Swap to Standard, clear keywords, then swap to your new shader. 

#### Note for Shader Authors
You may want to consider using the keywords reserved by the Standard shader as your own keywords. These are essentially guaranteed to already be reserved, so if you must use keywords, use the ones already defined by Standard and Post Processing v2. Here's a [list of recommended keywords to use](https://pastebin.com/83fQvZ3n).

### Alpha Transparency
Alpha transparency is also another expensive part of shaders-- typically you want to be using Cutout or Opaque modes on shaders. Transparency can be quite expensive, so only use it if you know what you're doing! 

### Measuring Draw Calls
The Unity Profiler can be very useful when judging for how many draw calls you're incurring-- just make sure you turn off shadows on your Directional Light for a level playing field.

## Reduce the amount of bones
Even if you have a bunch of bones sitting in a scarf, skirt, or your hair that you're not using for anything, they can incur additional costs during skinning calls that your GPU has to worry about. If you're not going to use a bone, consider deleting the bone and merging it into the parent bone. If you want to know how to merge the weights of a bone into its parent, check out the video on Dynamic Bones above, which includes a part on bone merging.

## Reduce the emission amount/amount of particle systems
Although particle systems can result in a lot of cool effects, having excessively large amounts of them can cause issues for some PCs. Limit the number of particle systems that you're using, and limit the maximum amount of particles emitting at any one given moment.

There are ways to have particle systems with large numbers of particles and retain performance. If you are interested in this, look into dynamic batching for sprite particles, don't use collision, and ensure the movement of your particles is simplistic.

If you're more technically inclined, you can try looking into Unity's Profiler view to judge how much CPU time your particle simulation is taking. Generally speaking, large transparent particles are worse than a lot of smaller, opaque ones. Unity's Particle System is actually quite optimized and runs quickly *if used well.*

## Limit the number of Lights your avatar uses
Lights on avatars are real-time, and as such, are exceedingly expensive. Adding a light to your avatar means that everything that your Light touches will render with *double* the draw calls. Additional lights multiply the effect. This is obviously *very bad for performance.* Do not use Lights that are always on. Try using an Animation Override to turn a flashlight on and off, or alternately, do not use Lights at all.

If you do use a Light, turn off Shadows for the Light. Shadows on Realtime lights are VERY expensive and often don't look that great on something that moves around.

Particle Systems can be configured to have a light on for a number of particles. *Never do this!* Each particle with a light counts as a real-time light, which is (once again) extremely expensive.

**In total, VRChat recommends that you do not use Lights of any type on avatars at all.** Not only do they adversely affect your own avatar's performance, they multiply performance cost of avatars the light is hitting as well.

## Recommended Software/Plugins
There's a large amount of software available to help you optimize your avatar and make it easier to build avatars. 

For example, check out [Pumkin's Avatar Tools](https://github.com/rurre/PumkinsAvatarTools) (MIT) for the Unity Editor. Among other things, this Editor script allows you to quickly see stats on your avatars. This tool is in beta, and may have bugs-- please report any issues on Pumkin's GitHub.

The following software has not been authored by VRChat. Please read and respect the licensing provided with each individual product.

* [Unity](/sdk/upgrade/current-unity-version) 
* [Blender](https://blender.org)
* [Cats Blender Plugin
](https://github.com/absolute-quantum/cats-blender-plugin)
* [Shotariya's Material Combiner](https://github.com/Grim-es/material-combiner-addon)
* [Pumkin's Avatar Tools](https://github.com/rurre/PumkinsAvatarTools)

---

## ドキュメント: avatar-performance-ranking-system.md

```metadata
階層: /avatars/avatar-performance-ranking-system.md
ディレクトリ: avatars
ファイル名: avatar-performance-ranking-system.md
拡張子: .md
サイズ: 33.98 KB
最終更新: 2025-06-05T03:07:52.733Z
```

# Performance Ranks

The Avatar Performance Ranking System allows you to see how much a user's avatar is affecting performance via analysis of the components on that user's avatar. You can also use it on yourself to see how performant your avatar is.

This system is provided to inform users what is likely the most performance-heavy components on their avatars, and offer basic advice on what to look into when optimizing their avatar.

It is also used to drive the [Minimum Displayed Performance Rank](/avatars/avatar-performance-ranking-system#minimum-displayed-performance-rank-on-pc) system, which is a way for users to decide what avatars they wish to show based on their Performance Rank.

**This system is not meant to be an end-all-be-all authority on avatar performance**, but is a good general guide to indicate if an avatar needs a bit more work to be performant.
:::danger Perf Ranks are not the final word!

Although the Performance Rank system does as best as it can to judge the "worst case" scenario of an avatar's performance, there are many ways to have a well-optimized avatar appear as Very Poor, and have a FPS hog rank as Excellent.

For the technically inclined: the Performance Rank system is based on a static analysis of the avatar's properties without any consideration paid to things like animators, shaders, texture resolution, pixel lights, and many more factors. *However*, it tends to provide an excellent litmus test for detecting problematic avatars 95% of the time!
:::

## Short Version
**Aim for Good ranking.** If you can't hit that, **Medium is perfectly fine.** 

Creating avatars is already hard, and creating optimized avatars is even harder. It is a skill that takes a long time to build!

Keep in mind that many events, groups, and locations in VRChat may ask you to change your avatar if you show up in a Very Poor avatar. As such, even if you choose to use a Very Poor avatar in small instances with your friends, make sure you also have one meant for usage in instances with more people.

Your avatar affects everyone else's framerate, so be mindful of how your choices affect other people's experiences. Otherwise, they might see you as your Fallback!
## Performance Ranking Icons
When you open your Quick Menu, you'll see icons appear on top of the nameplates of users. 

The ranks are as follows:

| Icon                                                  | Performance Rank | Description                                                                                                                                                                       |
|-------------------------------------------------------|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ![image](/img/avatars/performance-rank/excellent.png) | Excellent        | This is as good as you can get! The "Gold Star on the Fridge" rank.                                                                                                               |
| ![image](/img/avatars/performance-rank/good.png)      | Good             | Good enough! A great target to aim for.                                                                                                                                           |
| ![image](/img/avatars/performance-rank/medium.png)    | Medium           | Don't let the name fool you, Medium is plenty good. If you're here and don't want to work any farther up, you're fine.                                                            |
| ![image](/img/avatars/performance-rank/poor.png)      | Poor             | Could use some work.                                                                                                                                                              |
| ![image](/img/avatars/performance-rank/very-poor.png) | Very Poor        | This avatar has some serious performance problems. Since this rank is unbounded, it is very possible that your performance is suffering as a result of this avatar being visible. |

## Viewing Detailed Avatar Stats
If you click on a user with your Quick Menu open, you'll notice a new **"Show Avatar Stats"** button on the left side, displaying the icon of that user's Performance Rank.


![avatar-perormance-breakdown.gif](/img/avatars/view-avatar-details-qm.png)

If you click this icon, you can view the detailed **Avatar Stats** of that avatar. You can get to this for your own avatar by going to your Avatar Menu, clicking one of your avatars, and clicking the **Avatar Stats** button in the bottom left of the screen.


![avatar-perormance-breakdown.gif](/img/avatars/view-avatar-details-preview.png)


When you click the **Avatar Stats** button, you'll get a screen pop up with the details of avatar you're looking at, or your own avatar (if you clicked the button in the Avatar tab).

![avatar-perormance-breakdown.gif](/img/avatars/avatar-perormance-breakdown.gif)


The color of the text matches the rank that the particular stat "drags" the rank down to.

You'll also see a "before and after" in the form of the "Original" and "Perf Filtered" lines. If you're using the [Minimum Displayed Performance Rank](/avatars/avatar-performance-ranking-system#minimum-displayed-performance-rank-on-pc) system, you can see what the stats were before and after the system removed components. In the case of the Minimum Displayed Performance Rank system blocking an avatar for performance reasons, you'll only see the original stats.

In the example given above, Lights and Particle Systems are disabled due to exceeding the limit defined. Because Particle Systems employ at least one material each, the count of materials from Particle Systems is also subtracted from the pre-filtered avatar. 

You can also see that we link to our **Documentation**, in particular our [Avatar Optimization Tips](/avatars/avatar-optimizing-tips).
## Avatar Performance Ranking Stats
Here is a listing of all of the statistics that the system looks at and their description.

Bolded stats will cause the avatar to be fully blocked if they exceed the Minimum Displayed Performance Rank. If other stats (except for bounds) exceed the Minimum Displayed Performance Rank the avatar will only be partially blocked. The avatar will be shown with any components related to the exceeded stats will be removed. 

For example with the Minimum Displayed Performance Rank set to Poor an avatar with 9 Trail Renderers (Very Poor) will be displayed with all of its Trail Renderers removed. Refer to [Minimum Displayed Performance Rank](/avatars/avatar-performance-ranking-system#minimum-displayed-performance-rank-on-pc) for more information.

| Avatar Quality                     | Quality Description                                                                                                                                                                                                                                                                                                                          |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Triangles                          | The triangle count of the model in question. (In the past, this was incorrectly referred to as "Polygons.")                                                                                                                                                                                                                                  |
| Bounds Size                        | The total size of the avatar. If this is really huge, that user probably has a large animation on the avatar that isn't showing all the time.<br/>Important note: Bounds Size will not cause the avatar to be blocked, even if it is below the "Minimum Displayed Performance Rank" setting.                                                 |
| Texture Memory                     | The amount of memory estimated to be in use by the avatar's textures. These textures occupy space in both system RAM and in the video card's memory.                                                                                                                                                                                         |
| Skinned Meshes                     | The number of Skinned Mesh Renderer components on the avatar.                                                                                                                                                                                                                                                                                |
| Basic Meshes                       | The number of (non-skinned) Mesh Renderer components on the avatar.                                                                                                                                                                                                                                                                          |
| Material Slots                     | The number of material slots on the avatar. Material slots are the slots on the mesh where you fit materials in. This is what counts toward Submesh creation, which incurs further draw calls. Keep in mind that Particle Systems will use one material slot, Particle System with trails use two, and Line Renderers use one material slot. |
| PhysBones Components               | The number of PhysBone components on the avatar.                                                                                                                                                                                                                                                                                             |
| PhysBones Affected Transforms      | The total number of transforms affected by PhysBones components on the avatar.                                                                                                                                                                                                                                                               |
| PhysBones Colliders                | The number of PhysBone collider scripts on the avatar.                                                                                                                                                                                                                                                                                       |
| PhysBones Collision Check Count    | The sum of how many PhysBone transforms each collider can affect. This can count transforms twice or more, because a single transform can be affected by multiple colliders.                                                                                                                                                                 |
| Avatar Dynamics Contacts           | The number of Avatar Dynamics Contacts on the avatar. Does not count [receivers](/avatars/avatar-dynamics/contacts/#vrccontactreceiver) with [filtering](/avatars/avatar-dynamics/contacts/#filtering-1) set to "Local Only."                                                                                                                                                                                                              |
| Constraint Count                   | The total number of VRChat constraints and Unity constraints on the avatar. [Click here for more detailed info.](/avatars/avatar-dynamics/constraints#performance) |
| Constraint Depth                   | The deepest chain of dependencies across all constraints on the avatar. [Click here for more detailed info.](/avatars/avatar-dynamics/constraints#performance) |
| Animators                          | The number of Animators on the avatar. Important note: This will always be at least 1 due to the root animator being counted. This means that for the Excellent ranking, you can have no additional animators.                                                                                                                               |
| Bones                              | The number of Bones in the avatar's rig.                                                                                                                                                                                                                                                                                                     |
| Lights                             | The number of Light components on the avatar.                                                                                                                                                                                                                                                                                                |
| Particle Systems                   | The number of Particle System components on the avatar.                                                                                                                                                                                                                                                                                      |
| Total Particles Active             | The sum of maxParticles across all particle systems on the avatar.                                                                                                                                                                                                                                                                           |
| Mesh Particle Active Polys         | The total number of triangles of Mesh Particles emitted by Particle Systems that are active. In other words, maxEmission * meshParticleVerts.                                                                                                                                                                                                |
| Particle Trails Enabled            | If any Particle Systems on the avatar have Particle Trails enabled, this will be True.                                                                                                                                                                                                                                                       |
| Particle Collision Enabled         | If any Particle Systems on the avatar have Particle Collision enabled, this will be True.                                                                                                                                                                                                                                                    |
| Trail Renderers                    | The number of Trail Renderers on the avatar.                                                                                                                                                                                                                                                                                                 |
| Line Renderers                     | The number of Line Renderers on the avatar.                                                                                                                                                                                                                                                                                                  |
| Cloths                             | The total number of Cloth components on the avatar.                                                                                                                                                                                                                                                                                          |

## Avatar Performance Ranks - Value Maximums per Rank
Below, you'll find the limits for each of the Performance Ranks. If you go above these numbers for any category, you'll be bumped into the next rank.

For example (on PC), if your avatar has 2 Skinned Meshes, your avatar will be ranked as Good, as that exceeds the rating for Excellent, but does not exceed the rating for Good. 
:::caution All GameObjects and Components are counted!

All GameObjects and Components, **including those that are currently disabled**, count towards the Avatar Performance Rank.
:::

:::caution Mesh Read/Write Disabled

If you disable Mesh Read/Write on **any** mesh on the avatar (including particle systems), the "Triangles" count will read "Mesh Read/Write Disabled" and the avatar's Performance Rank will be immediately downgraded to "Very Poor" regardless of the actual triangle count on the avatar.

The SDK warns you of this and will require that you fix it before you upload.
:::

## PC Limits
| Avatar Quality                                                         | Excellent          | Good         | Medium       | Poor          |
|------------------------------------------------------------------------| ------------------ | ------------ | ------------ | ------------ |
| Triangles                                                              | 32,000             | 70,000       | 70,000       | 70,000       |
| Bounds Size[^1]                                                        | 2.5m x 2.5m x 2.5m | 4m x 4m x 4m | 5m x 6m x 5m | 5m x 6m x 5m |
| Texture Memory                                                         | 40 MB              | 75 MB        | 110 MB       | 150 MB       |
| Skinned Meshes                                                         | 1                  | 2            | 8            | 16           |
| Basic Meshes                                                           | 4                  | 8            | 16           | 24           |
| Material Slots                                                         | 4                  | 8            | 16           | 32           |
| [PhysBones](/avatars/avatar-dynamics/physbones) Components             | 4                  | 8            | 16           | 32           |
| [PhysBones](/avatars/avatar-dynamics/physbones) Affected Transforms    | 16                 | 64           | 128          | 256          |
| [PhysBones](/avatars/avatar-dynamics/physbones) Colliders              | 4                  | 8            | 16           | 32           |
| [PhysBones](/avatars/avatar-dynamics/physbones) Collision Check Count  | 32                 | 128          | 256          | 512          |
| Avatar Dynamics [Contacts](/avatars/avatar-dynamics/contacts)          | 8                  | 16           | 24           | 32           |
| [Constraint](/avatars/avatar-dynamics/constraints) Count               | 100                | 250          | 300          | 350          |
| [Constraint](/avatars/avatar-dynamics/constraints) Depth               | 20                 | 50           | 80           | 100          |
| Animators                                                              | 1                  | 4            | 16           | 32           |
| Bones                                                                  | 75                 | 150          | 256          | 400          |
| Lights                                                                 | 0                  | 0            | 0            | 1            |
| Particle Systems                                                       | 0                  | 4            | 8            | 16           |
| Total Particles Active                                                 | 0                  | 300          | 1000         | 2500         |
| Mesh Particle Active Polys                                             | 0                  | 1000         | 2000         | 5000         |
| Particle Trails Enabled                                                | False              | False        | True         | True         |
| Particle Collision Enabled                                             | False              | False        | True         | True         |
| Trail Renderers                                                        | 1                  | 2            | 4            | 8            |
| Line Renderers                                                         | 1                  | 2            | 4            | 8            |
| Cloths                                                                 | 0                  | 1            | 1            | 1            |
| Total Cloth Vertices                                                   | 0                  | 50           | 100          | 200          |
| Physics Colliders                                                      | 0                  | 1            | 8            | 8            |
| Physics Rigidbodies                                                    | 0                  | 1            | 8            | 8            |
| Audio Sources                                                          | 1                  | 4            | 8            | 8            |

The table below describes the requirements for PC avatars to receive a certain performance rank:


### PC Default Performance Rank Blocking

On PC, the default Minimum Displayed Performance Rank level is "Very Poor". This means that users can see most avatars by default. If you a user enables the [Minimum Displayed Performance Rank](/avatars/avatar-performance-ranking-system#minimum-displayed-performance-rank-on-pc) system, they can choose to hide avatars with poor performance.

However, if your avatar is extremely unoptimized, VRChat may prevent you from using it. You can fix this my improving its performance rank, ensuring that it doesn't exceed VRChat's [avatar size limits](/avatars/avatar-size-limits), and then reuploading the avatar.

## Mobile Limits

VRChat on Android and iOS (phones, tablets, and Meta Quest) has stricter limits than VRChat on PC.

The table below describes the requirements for mobile avatar to receive a certain performance rank:

| Avatar Quality                                                            | Excellent          | Good         | Medium       | Poor         |
| ------------------------------------------------------------------------- | ------------------ | ------------ | ------------ | ------------ |
| Triangles                                                                 | 7,500              | 10,000       | 15,000       | 20,000       |
| Bounds Size[^1]                                                           | 2.5m x 2.5m x 2.5m | 4m x 4m x 4m | 5m x 6m x 5m | 5m x 6m x 5m |
| Texture Memory                                                            | 10 MB              | 18 MB        | 25 MB        | 40 MB        |
| Skinned Meshes                                                            | 1                  | 1            | 2            | 2            |
| Basic Meshes                                                              | 1                  | 1            | 2            | 2            |
| Material Slots                                                            | 1                  | 1            | 2            | 4            |
| Animators                                                                 | 1                  | 1            | 1            | 2            |
| Bones                                                                     | 75                 | 90           | 150          | 150          |
| [PhysBones](/avatars/avatar-dynamics/physbones) Components[^2]            | 0                  | 4            | 6            | 8            |
| [PhysBones](/avatars/avatar-dynamics/physbones) Affected Transforms[^2]   | 0                  | 16           | 32           | 64           |
| [PhysBones](/avatars/avatar-dynamics/physbones) Colliders[^2]             | 0                  | 4            | 8            | 16           |
| [PhysBones](/avatars/avatar-dynamics/physbones) Collision Check Count[^2] | 0                  | 16           | 32           | 64           |
| Avatar Dynamics [Contacts](/avatars/avatar-dynamics/contacts)[^2]         | 2                  | 4            | 8            | 16           |
| [Constraint](/avatars/avatar-dynamics/constraints) Count[^2]              | 30                 | 60           | 120          | 150          |
| [Constraint](/avatars/avatar-dynamics/constraints) Depth[^2]              | 5                  | 15           | 35           | 50           |
| Particle Systems                                                          | 0                  | 0            | 0            | 2            |
| Total Particles Active                                                    | 0                  | 0            | 0            | 200          |
| Mesh Particle Active Polys                                                | 0                  | 0            | 0            | 400          |
| Particle Trails Enabled                                                   | False              | False        | False        | True         |
| Particle Collision Enabled                                                | False              | False        | False        | True         |
| Trail Renderers                                                           | 0                  | 0            | 0            | 1            |
| Line Renderers                                                            | 0                  | 0            | 0            | 1            |

[^1]: Bounds Size is determined by the maximum size of all components on your avatar. Trail and Line Renderers do not count for this calculation.

[^2]: If the Very Poor value is exceeded on mobile, no matter the current "Show Avatar" state of the avatar, all Avatar Dynamics-related components will be removed.

### Mobile Default Performance Rank Blocking
On mobile, The Minimum Displayed Performance Rank is "Medium" by default. This means users can't see any avatars ranked as "Poor" or "Very Poor".

Users can set their Performance Rank Block level to "Poor", allowing them to see "Poor" avatars. However, they cannot set their Performance Rank Block level to "Very Poor".

For example, if a mobile avatar exceeds 20,000 triangles, it's "Very Poor" and users can't see it in VRChat. However, users can forcefully show "Very Poor" avatars by selecting the user and clicking "Show Avatar".

:::warning

In the future, VRChat may remove "Very Poor" mobile avatars and the ability to use "Show Avatar" for "Very Poor" mobile avatars. Please keep this in mind when creating mobile avatars.
:::

### Mobile Avatar Component Limits

Some [avatar components](/avatars/avatar-dynamics) are limited on mobile avatars. You cannot exceed the following limits:

- 8 [PhysBone](/avatars/avatar-dynamics/physbones) components
- 64 [PhysBones](/avatars/avatar-dynamics/physbones) affected transforms
- 16 [PhysBones](/avatars/avatar-dynamics/physbones) colliders
- 64 [PhysBones](/avatars/avatar-dynamics/physbones) collider checks
- 16 [Avatar Dynamics Contacts](/avatars/avatar-dynamics/contacts) 
- 150 [Constraint](/avatars/avatar-dynamics/constraints) components
- A dependency depth of 50 [Constraints](/avatars/avatar-dynamics/constraints)

You cannot bypass the limits above by using "Show Avatar". If a mobile avatar exceeds a limit, all limited avatar components are removed from the avatar in VRChat, even if you enable "Show Avatar".

### Mobile Removed Components
The following components are disabled on mobile devices since they can never appear on avatars:

  * Lights
  * Cloths
  * Total Cloth Vertices
  * Physics Colliders
  * Physics Rigidbodies
  * Audio Sources

These values may still appear in VRChat's avatar details screen, but they are always zero.

## Minimum Displayed Performance Rank
You can choose to manage avatars based on their Avatar Performance Rank. This option is available in the [Performance Options](https://docs.vrchat.com/docs/vrchat-configuration-window) menu, accessible as a button in the top-right of the Safety tab in the main menu.

When you choose a Performance Rank in VRChat's menu, all avatars that are below that level will have their components/display managed as described below.

| Parameter                                                                        | Description                                                                              |
| :------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------- |
| Triangles                                                                        | **Avatar replaced with [Fallback](https://docs.vrchat.com/docs/avatar-fallback-system)** |
| Bounds Size                                                                      | No change                                                                                |
| Texture Memory                                                                   | **Avatar replaced with [Fallback](https://docs.vrchat.com/docs/avatar-fallback-system)** |
| Skinned Meshes                                                                   | **Avatar replaced with [Fallback](https://docs.vrchat.com/docs/avatar-fallback-system)** |
| Basic Meshes                                                                     | **Avatar replaced with [Fallback](https://docs.vrchat.com/docs/avatar-fallback-system)** |
| Material Slots                                                                   | **Avatar replaced with [Fallback](https://docs.vrchat.com/docs/avatar-fallback-system)** |
| Physics Bone Components, Transforms, Colliders, CollisionCheckCount, or Contacts | All PhysBone, PhysBone Collider,and Contact components removed                           |
| Constraint Count or Depth                                                        | All Constraint components removed                                                        |
| Animators                                                                        | All animators (aside from root animator) removed                                         |
| Bones                                                                            | **Avatar replaced with [Fallback](https://docs.vrchat.com/docs/avatar-fallback-system)** |
| Lights                                                                           | All Lights removed                                                                       |
| Particle Systems                                                                 | All Particle Systems removed                                                             |
| Total Particles Active                                                           | All Particle Systems removed                                                             |
| Mesh Particle Active Polys                                                       | All Particle Systems removed                                                             |
| Particle Trails Enabled                                                          | All Particle Systems removed                                                             |
| Particle Collision Enabled                                                       | All Particle Systems removed                                                             |
| Trail Renderers                                                                  | All Trail Renderers removed                                                              |
| Line Renderers                                                                   | All Line Renderers removed                                                               |
| Cloths                                                                           | All Cloth components removed                                                             |
| Total Cloth Vertices                                                             | All Cloth components removed                                                             |
| Physics Colliders                                                                | All Physics Colliders removed                                                            |
| Physics Rigidbodies                                                              | All Physics Rigidbodies removed                                                          |
| Audio Sources                                                                    | All Audio Sources removed                                                                |

### Minimum Displayed Performance Rank on PC
On VRChat for PC, the Minimum Displayed Performance Rank is set to "Very Poor" by default. This means that, by default, no avatars will have their components or display affected for performance reasons on PC. If you wish to change this, you can choose between "Medium", "Poor", or "Very Poor" options.

### Avatar Performance Rank Blocking on Mobile
On VRChat for mobile devices, the Avatar Performance Rank Block is set to "Medium" by default. You can choose to change this to "Poor" to see avatars of that rank, but your performance may suffer as a result.

You cannot disable the Avatar Performance Rank Block system on mobile. In other words, avatars that are ranked as "Very Poor" will always have their display managed VRChat for mobile, and may not display at all.

No matter what setting you choose, if the [Avatar Dynamics](/avatars/avatar-dynamics) component limits are exceeded on mobile devices, all of those components will be removed. In short, there is a hard cap for Avatar Dynamics components on mobile avatars.

### Overriding Individual Avatars
:::danger

**"Show Avatar" for Very Poor avatars functionality may be removed in the future, and Very Poor avatars may be removed from Android and iOS entirely.** Please keep this in mind when creating avatars for VRChat on mobile devices.
:::
You can choose to override the the entirety of the system (and the Safety system) by selecting "Show Avatar" on each user you wish to show.


---

## ドキュメント: avatar-scaling.md

```metadata
階層: /avatars/avatar-scaling.md
ディレクトリ: avatars
ファイル名: avatar-scaling.md
拡張子: .md
サイズ: 3.18 KB
最終更新: 2025-06-05T03:07:52.734Z
```

---
title: "Avatar Scaling"
slug: "avatar-scaling"
hidden: false
createdAt: "2023-07-26T15:58:00.000Z"
updatedAt: "2023-07-26T15:58:00.000Z"
---

# Technical Considerations around Avatar Scaling

Avatar scaling allows players to change the height of their current avatar.

This page documents how VRChat's avatar scaling works internally to the extent that may be useful for developing community resources around it.

Read the [Avatar Events documentation](/worlds/udon/players/player-avatar-scaling) documentation or our [example script](/worlds/examples/udon-example-scene/avatar-scaling-settings) to learn how to use avatar scaling with Udon. 
Read the [Avatar Parameters](/avatars/animator-parameters) page for available avatar parameters related to scaling.


## Term Definitions

* **Eye Height**: The height above 0 (Y component in transform position) of an avatar's viewpoint while in a T-Pose.
* **Prefab Height**: The eye height of an avatar when scaling is at its default value. This is what you configure in the SDK by placing the "View Position".
* **Target Eye Height**: The eye height an avatar wants to be scaled to, as set by either the player or Udon.
* **Avatar Scale**: The ratio between "Prefab Height" and "Target Eye Height". For example, if the target is 4.5 meters, and the prefab height is 1.5 meters, then "Avatar Scale" is 3.

## Range of Values

The "Prefab Height" is not limited. You can put your avatar's "View Position" at any height in the SDK. Note, however, that your overall avatar scale will affect your performance rank.

The "Target Eye Height" is clamped between 0.1 and 100 meters for Udon, and the usable range of scaling in your Action Menu goes from 0.2 to 5.0 meters. But by uploading an avatar that is outside this range and _not_ using the scale dial, you can still exceed those limits. In that case, "Target Eye Height" will match "Prefab Height," which is not limited.

This restriction means that Udon _must_ re-apply scales on "avatar eye height changed" events to fully enforce scale. Otherwise, users can switch to shorter or taller avatars to bypass the limits put in place by the world.

In worlds where scaling is disabled via the website, "Target Eye Height" will _always_ match "Prefab Height."

## How Scaling is Applied

Scaling an avatar works by changing the "localScale" of the avatar's root transform. **This scale is enforced**. There is no user-accessible way to override the scale of the root of an avatar other than the Udon functions relating to avatar scaling.

Whenever the scale is changed (i.e., whenever you switch avatars, or reset/reload your current one), the avatar is "remeasured" locally. This process adjusts your viewpoint to match the new height and repositions a few internal components, such as your voice position.

Scale is automatically synchronized for each player as "eye height", quantized to 3 decimal points. For remote users, scale changes are smoothed over a short period any time a new height is received.

While using the scale dial in your Action Menu, your avatar is only scaled in mirrors. When confirming the scale on the dial, the new scale will instantly be applied to your avatar and then sent to remote users.

---

## ドキュメント: avatar-size-limits.md

```metadata
階層: /avatars/avatar-size-limits.md
ディレクトリ: avatars
ファイル名: avatar-size-limits.md
拡張子: .md
サイズ: 3.34 KB
最終更新: 2025-06-05T03:07:52.734Z
```


# Avatar Size Limits

Uploading a custom avatar is a great way to express yourself in VRChat. However, unoptimized avatars require more bandwidth, RAM, and VRAM, which decreases VRChat's performance.

This page explains VRChat's maximum **download size** and **uncompressed size** for avatars, and how to decrease your avatar's size.
## Download size and uncompressed size
Here's how VRChat calculates your avatar's file size:
- When you build and upload an avatar, the VRChat SDK packages it into a [Unity asset bundle](https://docs.unity3d.com/Manual/AssetBundlesIntro.html) and compresses it. The **download size** is the file size of your avatar's compressed asset bundle.
- When VRChat downloads an avatar, VRChat decompresses the asset bundle. The **uncompressed size** is the uncompressed bundle size.

When calculating your avatar's size, VRChat does _not_ decompress individual assets! For example: If you set "Compression" to "High Quality" in a [texture's import settings](https://docs.unity3d.com/Manual/class-TextureImporter.html), it increases your avatar's download size **and** uncompressed size.

## Avatar size limits
The maximum size limit depends on the platform you're playing on:

| Platform                | Download Size | Uncompressed Size |
| ----------------------- | ------------- | ----------------- |
| Android                 | 10 MB         | 40 MB             |
| PC                      | 200 MB        | 500 MB            |

## How to know if you are below these limits
Within the SDK it will inform you when making a build if your avatar is breaking either limit and prevent upload, post build it will remind you what the download / uncompressed size was along with the limit. When using Build & Test size limits are not enforced.

:::caution Keep your SDK up to date!
The android uncompressed size was not enforced in the SDK until [3.5.2](/releases/release-3-5-2), ensure you are _at least_ on this version otherwise if you upload it will just fail server-side security checks! The reduced PC limits are enforced starting [3.7.0](/releases/release-3-7-0), so similarly if uploading to PC ensure you are _at least_ on this version.
:::

Within the client you can see both stats within the avatar details, either within the quick menu or main menu.

## How to decrease your avatar's size
You can reduce the size of your avatar by optimizing your assets:
- Textures
  - Reduce the max size in the [texture import settings](https://docs.unity3d.com/Manual/class-TextureImporter.html).
  - Use fewer texture files by deleting/merging textures or materials.
  - Resize your texture to a [power of two](https://en.wikipedia.org/wiki/Power_of_two) (i.e. 512, 1024, 2048) or enable "Non-Power of 2" in the [texture import settings](https://docs.unity3d.com/Manual/class-TextureImporter.html) to improve Unity's texture compression.
- Audio clips
  - Shorten long clips.
  - Reduce the quality of clips or enable "Force to Mono" in the [Audio Clip import settings](https://docs.unity3d.com/Manual/class-AudioClip.html).
- Animation clips
  - Reduce the number of keyframes.
- Blend shapes
  - Remove unused blend shapes.
  - Reduce the number of vertices affected by blend shapes, especially if your model has a high number of blend shapes.

You can find more optimization tips on the [Avatar Optimizing Tips page](/avatars/avatar-optimizing-tips).


---

## ドキュメント: creating-your-first-avatar.md

```metadata
階層: /avatars/creating-your-first-avatar.md
ディレクトリ: avatars
ファイル名: creating-your-first-avatar.md
拡張子: .md
サイズ: 19.59 KB
最終更新: 2025-06-05T03:07:52.734Z
```

---
sidebar_position: -1
---
# Creating Your First Avatar

VRChat has tens of millions of avatars, and anyone can create them! This page explains how you can create your first VRChat avatar. There are two ways to create an avatar:

- You can use an [avatar creation tool](https://hello.vrchat.com/avatar-systems) to create simple avatars without Unity.
- You can use the [Creator Companion](https://vcc.docs.vrchat.com/) to install [Unity](https://unity.com/), install the [VRChat Software Development Kit (SDK)](https://creators.vrchat.com/sdk/), and to upload a custom avatar.

## Requirements

To upload a custom avatar with Unity and the VRChat SDK, you must meet the following requirements:

- You must have an account on [VRChat.com](https://vrchat.com/).
  - If you're playing on a Steam or Meta account, you'll need to [link your account first](https://help.vrchat.com/hc/en-us/articles/360062659053-I-want-to-turn-my-platform-account-through-Steam-Meta-Pico-or-Viveport-into-a-VRChat-account).
- Your VRChat account must have a [trust rank](https://docs.vrchat.com/docs/vrchat-safety-and-trust-system#trust-rank) of "New User" or higher.
  - If you're new to VRChat, you'll receive an email once you're allowed to upload avatars.

:::tip Need help?

If you get stuck or need help,  here's where you can get help:
- Browse VRChat's [official documentation](https://creators.vrchat.com/). 
- Visit VRChat's [official forum](https://ask.vrchat.com/) or [Discord server](https://discord.com/invite/vrchat) and ask the community.

:::

## Step 1 - Choose a 3D model

Maybe you already have a 3D model that you want to use as an avatar - or you might be downloading a 3D model for the first time. Choose one of the following four ways to get started:

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs groupId="option-avatar-step-one">
<TabItem value="avatar-creation-tool" label="1. Use an avatar creation tool" default>

If you've never used Unity or the VRChat SDK, try using one of the following avatar creation tools: 

- The [VRChat Avatar Systems](https://hello.vrchat.com/avatar-systems) page lists several beginner-friendly avatar creation tools.
 	- These tools are similar to customizing your character in a video game.
 	- These tools do not require the VRChat SDK! If you use them, you can skip all other steps on this page.
        - Any VRChat user can use these tools. You do not need the ["New User" trust rank](https://docs.vrchat.com/docs/vrchat-safety-and-trust-system#trust-rank).
- [VRoid Studio](https://vroid.com/en/studio) is an anime-theme character creator for creating VTuber-style models.
	- Characters have hundreds of customization sliders and can be hand-painted.
	- For some examples of what it can do, check out the [VRoid subreddit](https://www.reddit.com/r/VRoid/).
	- It's also available on [Steam](https://store.steampowered.com/app/1486350/VRoid_Studio_v1263/).
	- ⚠VRoid Studio outputs avatars in the **.vrm** format, which isn't natively supported by Unity!
		- If you'd like to import a VRoid Studio model directly for use in VRChat, you may want to look into the community-created [VRMtoVRChat converter](https://github.com/esperecyan/VRMConverterForVRChat) for .vrm avatars. Be sure to [read the documentation for this plugin](https://www.store.vket.com/ec/items/122/detail/) if you use it.

</TabItem>
<TabItem value="example-avatar" label="2. Use VRChat's example avatar">

If you want to upload your own avatar to VRChat, you need to use the VRChat SDK.

If you're new to the VRChat SDK, try uploading the built-in [example avatar](/avatars/creating-your-first-avatar#try-vrchats-example-avatar) first.

After you've successfully uploaded the example avatar, try uploading your own avatar!

</TabItem>
<TabItem value="find-model" label="3. Download a model">

There are many stores on the internet where you can download free or paid VRChat avatars.

Some stores sell 3D models that can be used in VRChat or in other applications. These avatars are great for learning about the VRChat SDK and creating your own VRChat avatar.

- [100 avatars](https://github.com/PolygonalMind/100Avatars) is a free collection of hundreds of avatars. They're simple and easy to import into the VRChat SDK. 
- The [Unity Asset Store](https://assetstore.unity.com/) has free and paid 3D models. They're easy to import into Unity and usually compatible with the VRChat SDK, but they may include assets or scripts that won't work.

Some stores sell avatars that are already prepared for VRChat. They may allow you to skip some steps when setting up the avatar in Unity but might also include advanced features that aren't covered in this article. They are suitable if you want a cool-looking avatar and care less about learning how to create your own. 

- [BOOTH](https://booth.pm/en/items?tags%5B%5D=VRChat) is a Japanese store for VRChat avatars. It's the largest store for anime-style avatars, but you can also find other types of avatars there.
- [Gumroad](https://gumroad.com/discover) is more popular among Western creators and focuses on anime-style and furry avatars.
- [Jinxxy](https://jinxxy.com/) and [Avatown](https://goavatown.com) also have a collection of Avatars that can be used in VRChat.
  
When you look for a model, try to keep the following things in mind:
- If you decide to get your model outside of an asset store, ensure the model is fully "rigged" by the author.
	- A "rigged" model has a skeleton that allows it to move. Creating a rig can be very difficult, but tools like [Mixamo](https://www.mixamo.com/) and [Rigify](https://docs.blender.org/manual/en/latest/addons/rigging/rigify/index.html) can do it automatically.
	- You should also check that the model is in [a format compatible with Unity](https://docs.unity3d.com/Manual/3D-formats.html), such as `.fbx`.
- Ensure that you have a license to use the model in VRChat.
	- Most asset stores display their license on the 3D model's store page.
	- Using them without a license is a violation of the model author's rights and the [VRChat Terms of Service](https://hello.vrchat.com/legal). 
- Ensure that the model that you're using is [below 20,000 for VRChat on Meta Quest](/avatars/avatar-performance-ranking-system#mobile-limits) and [below 70,000 triangles on PC](/avatars/avatar-performance-ranking-system#pc-limits).
	- Uploading an avatar with an excessive triangle count can cause performance issues.
	- On PC, you can upload models above this limit, but the avatar will be ranked as having "Very Poor" performance, which means that fewer players will see it.

</TabItem>

<TabItem value="create-avatar" label="4. Create a model">

While most users choose to find a model as a starting point, anyone can create an avatar model from scratch. You can use any 3D software you like, as long as it supports exporting an FBX with an armature. [Blender](https://www.blender.org/) is free and a very common choice.

If you've never modeled in 3D before, this might be the beginning of a long journey. Learning how to model, rig, and texture a 3D model is very complex. Creating a VRChat avatar combines _all of those skills_!

If you choose to create your model, we suggest starting with something very simple. Even if you don't look as flashy as pre-made models, it is _your_ model, and you can do whatever you'd like with it.

To get you started, here's a VRChat-centric tutorial one of our community members has created:
- [Rainhet's Blender 3D Virtual Avatar Tutorial 2022](https://www.youtube.com/watch?v=OKWsUAIsgpg&list=PL2EEbgwoJzdsC9wfKA2ZO2kAf4HDqC8a8&index=1) - Rainhet's tutorial is long-form, and she explains everything thoroughly as she works through it.
- [Rainhet's 3D Avatar Class](https://www.youtube.com/watch?v=w-yhjgnhaNw) - An older version of Rainhet's tutorial series. Also has a [10-minute version](https://www.youtube.com/watch?v=in9rNze4FD4) that gives you a big-picture view of the process.

import FeedbackButton from "@site/src/components/FeedbackButton";

If you have a tutorial you'd like to suggest, please suggest it by clicking the <FeedbackButton /> button.

</TabItem>

</Tabs>

## Step 2 - Set up the VRChat SDK

Congratulations on choosing or building a model! Before you continue, you'll need to set up the [VRChat Creator Companion](https://vcc.docs.vrchat.com/). It will help you install [Unity](https://unity.com/) and create projects with the [VRChat SDK](/sdk). Watch the video below to get started!

<div class="video-container">
    <iframe src="https://www.youtube.com/embed/0u1g0TYoJsU" title="VRChat Creator Companion" frameborder="0" allow="encrypted-media; gyroscope; web-share" allowfullscreen></iframe>
</div>

Read the Creator Companion's [Getting Started](https://vcc.docs.vrchat.com/guides/getting-started) page to learn more. After setting creating your Unity project, you're ready to continue!

:::tip

Are you new to Unity? Visit [Unity Learn](https://learn.unity.com/) for free tutorials on how to use Unity.

:::

### Try VRChat's example avatar

Before you import your own 3D model into Unity, you can try using VRChat's example avatar instead. This allows you to learn how to upload an avatar without worrying about problems related to your 3D model.

Open your avatar project and go to 'VRChat SDK > Samples > Avatar Dynamics Robot Avatar.'

![The example avatar can help you understand what a complete VRChat avatar project might look like.](/img/avatars/creating-your-first-avatar-3dfc191-Unity_YrUFLEWWDe.png)

If the example avatar loaded successfully, you can skip to [step 6](/avatars/creating-your-first-avatar#step-6---going-to-the-build-tab--checking-if-the-avatar-is-ok). If you'd rather import your own avatar, continue with step 3 below.

## Step 3 - Get the model into your project
Now that you've set up the VRChat SDK, it's time to import your model into your project. If you're getting it from an asset store, then you can download and directly import it into your project. If you got the model from elsewhere, then you need to import it and any related textures into your 'Assets' folder.

If you are importing your model from a 3D editor, please ensure you keep in mind the difference between coordinate systems. For example, [**Blender**](https://blender.org)'s default coordinate and unit system differs from Unity's. You must export FBX files from Blender and define the exporter as such:

![image](/img/avatars/creating-your-first-avatar-b066a1b-2022-05-27_11-13-48_blender.png)

After you get the model in your assets, select it, you'll want to ensure it has the correct settings in the rig tab in the inspector. Make sure the Animation Type is set to Humanoid.

## Step 4 - Get the model into a scene
Now that you have the model in your Assets folder, with the correct settings applied, you need to put it into a scene. To do so, either drag it into your [Hierarchy](https://docs.unity3d.com/Manual/Hierarchy.html) or directly into the Scene View window. We recommend having one scene per avatar and placing it at the coordinates (0, 0, 0). If needed, rotate the avatar so it is standing up straight, and ensure that its size is what you expect. You can add a Cube to your scene to compare - the cube will be 1 meter on each side and your Avatar will best function between about 0.5-5m tall. The average person is around 1.65 meters tall.
:::caution Avatar Optimization

It is very important that your avatar is optimized so that you do not cause low FPS for yourself and others. The SDK will inform you if something looks wrong. Check out our [Avatar Optimization Tips](/avatars/avatar-optimizing-tips) to check out methods to improve your avatar's Performance Rank.
:::
## Step 5 - Adding an Avatar Descriptor 
The next step is to add a 'VRC Avatar Descriptor' component and prepare its settings.
1. Select the avatar in your hierarchy.
2. Click 'Add Component' in the inspector.
3. Search for the 'VRC Avatar Descriptor' component and add it.
4. Customize its settings, as explained below.

![Add a `VRC Avatar Descriptor` to get started with your avatar.](/img/avatars/creating-your-first-avatar-fd027ea-Unity_qH7NJfAzzn.png)
### View position
First, you'll want to set the view position. This will be where your camera will be positioned in VRChat. You can see a visual representation of it as a small white sphere in the scene.

If your avatar has a head, place the view position between the avatar's eyes. If your avatar's head is unusually large, its feet may lift off the ground when looking up and down. To avoid this, place the view position closer to where a regular-sized head would be.

If your avatar doesn't have a head, place the view position wherever you think it's appropriate.

![Use the Avatar Descriptor to configure your avatar for VRChat. Make sure to adjust the view position!](/img/avatars/creating-your-first-avatar-5afcbf1-Unity_lsTjP8qDqO.png)
### Lip sync mode
When you talk, you can make your avatar's mouth (or anything else) react automatically.  Open your `VRC Avatar Descriptor` and expand the `LipSync` dropdown. You can choose one of five lip sync modes:

#### Default
![Pressing 'Auto Detect!' is usually enough to let your VRChat avatar react to your speech.](/img/avatars/creating-your-first-avatar-d69289f-Unity_FgsAtEU75F.png)

Press 'Auto Detect!' to let the VRChat SDK automatically detect the appropriate lip sync mode. The mode will then switch to one of the modes below.

#### Jaw Flap Bone
If your avatar uses a single bone to animate the jaw, you can specify it here. Your character's jaw will open depending on how loudly you speak in VRChat. Ensure you've configured the jaw bone in Unity's Humanoid rig for your avatar.

#### **Viseme Blend Shape** (recommended)
Blend shapes/shape keys (named depending on what software you're using) modify the mesh based on vertex positions.  Many models use this for detailed animations for speaking. If your model has these, you should use them!

We use the Oculus Audio library to detect and set visemes. [You can see a reference to what all the visemes should look like and what sound triggers them here](https://developer.oculus.com/documentation/unity/audio-ovrlipsync-viseme-reference). 

VRChat can usually detect your avatar's visemes automatically. If not, you can choose visemes from the dropdown list.

![The 'Viseme Blend Shape' mode is the most common method of making your character's face move when you speak.](/img/avatars/creating-your-first-avatar-6272723-Unity_w5nQONGtcb.png)

:::caution SIL shape

Unity will delete shape keys/blend shapes that are empty on import, so make sure your "SIL" shape (the shape your mouth makes when no sound is detected, but the mic is active - such as the space between words) moves a single vertex a very small, imperceptible amount. This will prevent Unity from deleting that key.
:::

:::note Viseme Performance Tip!

If you're an avatar creator, consider splitting your avatar into two skinned meshes - one for your body, and one for your head/face.
The performance cost of blend shapes depends on how much of your 3D model they affect. Keeping blend shapes on a separate head mesh and having fewer blend shapes on your body mesh may improve your avatar's performance.
:::

##### Jaw Flap Blend Shape
If your avatar only uses a single blend shape to animate its mouth, configure it here. It will behave similarly to 'Jaw Flap Bone' by animating the jaw blend shape instead of a jaw bone.

##### Viseme Parameter Only
If you're an advanced creator, you can use this mode to control how your avatar reacts to speech with VRChat's built-in [Animator Parameters](/avatars/animator-parameters).

## Step 6 - Going to the build tab / Checking if the avatar is ok
Next, we'll want to check that everything is good in the build window. To do that, use the menu item `VRChat SDK > Show Control Panel`, which opens the VRChat SDK Control panel. After signing in, switch to the "Builder" tab to see the avatar's GameObject mentioned with a "Build" section below. You also see settings, content tags, the avatar's performance rank, errors, and warnings.

![The VRChat SDK build panel.](/img/avatars/build-panel-avatars-2025.png)

Simply follow the steps in VRChat's SDK build panel: 
1. Give your avatar a name.
	- You can add a description, too.
2. Choose your avatar's visibility.
	- Public avatars can be cloned by other VRChat users or shared via pedestals in worlds.
	- Private avatars can only be used by you.
3. Select appropriate content warnings for your avatar to comply with VRChat's  [content gating system](https://hello.vrchat.com/blog/content-gating).
4.  Select a thumbnail image.
	- You can select a file or capture an image from your Unity scene.
5. Read the 'Validations' section. It contains many useful errors and warnings.
	- For example, the SDK may warn you about your avatar having too many triangles, which you can fix by optimizing mesh(es). If you're unable to optimize the mesh, you may need to go back and choose another model.
6. Choose the build type.
	- **Build & Publish Your Avatar Online** uploads your avatar to VRChat and allows other users to see it.
	- **Build & Test Your Avatar** allows you to quickly test your avatar without uploading it.
		- You can find your test avatar in the "Other" avatars section in VRChat.
		- You can use [Build & Test on Android](/platforms/android/build-test-mobile/).
7. Choose which platforms to build your [platform](/platforms/) on.
8. Confirm that the avatar's information is accurate and thatn you have the rights to upload the content to VRChat.
9. When you're ready, click the "Build & Publish" button.


## Step 7 - Building and uploading the avatar!
Now everything is ready. Press the "Build & Publish" button, and the SDK will start building and uploading your avatar. Before uploading your avatar, you should double-check that it complies with VRChat's [Terms of Service](https://hello.vrchat.com/legal) and [Community Guidelines](https://hello.vrchat.com/community-guidelines).

After uploading your avatar, it should be available in VRChat. You can also see your avatar in  `VRChat SDK > Show Control Panel > Content Manager`.

You can also test your avatar without uploading it. To do this, click "Build & Test" instead. Your avatar will appear in the "Other" section of your VRChat Avatars menu. Test avatars can only be seen by you. In order for other players to see your avatar, you need to upload it.

Additionally, you can launch with the `--watch-avatars` launch option that will make it so that if you're wearing an avatar from the "Other" section, any future tests will immediately switch you to the new version of the avatar.

## Step 8 - Enjoy your avatar!

Congratulations on creating your first avatar! We hope everything went smoothly. If you need any help, consider visiting our [Ask Forum](https://ask.vrchat.com/) or our [Discord server](https://discord.com/invite/vrchat).

Creating and uploading VRChat avatars can be fun and creatively fulfilling. If you'd like to improve your avatar creation skills, take a look at the rest of our [Avatars documentation](https://creators.vrchat.com/avatars/).

## Learn more

If you'd like to become better at avatar creation, check out these pages:
- [Quest Content Optimization](/platforms/android/quest-content-optimization) - Learn how to create avatars that work well on Android and Quest.
- [Avatar Optimization Tips](/avatars/avatar-optimizing-tips) - Learn general advice on creating optimized PC or Android avatars.
- [VRChat's performance ranking system?](/avatars/avatar-performance-ranking-system) - Learn why certain avatars are visible or hidden to other players by default.
- [Avatar Dynamics](/avatars/avatar-dynamics/) - Learn how to create physics-driven interactions on your avatar. 



---

## ドキュメント: expression-menu-and-controls.md

```metadata
階層: /avatars/expression-menu-and-controls.md
ディレクトリ: avatars
ファイル名: expression-menu-and-controls.md
拡張子: .md
サイズ: 7.64 KB
最終更新: 2025-06-05T03:07:52.734Z
```

import UnityVersionedLink from '@site/src/components/UnityVersionedLink.js';

# Expressions Menu and Controls

:::tip
You need basic knowledge about <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/class-AnimatorController.html">Unity Animators</UnityVersionedLink> to use expression parameters in your avatar's animators.
:::

You can control your avatar's animators in VRChat with an Expressions Menu. This allows you to play animations or change your avatar's appearance with VRChat's radial menu.

To use the Expressions Menu in VRChat, you need to create (at least) two assets:

- One (or more) Expressions Menu asset
- One Expression Parameters asset

After creating and configuring these assets, make sure to add them to your avatar descriptor in the "Expressions" section.

## Creating an Expressions Menu

1. Create an **Expressions Menu asset** by selecting `Assets > Create > VRChat > Avatars > Expressions Menu`.

![The default view of the Avatar Expressions Menu.](/img/avatars/expression-menu/default-expressions.png)

2. Create an **Expression Parameters asset** by selecting your Expressions Menu asset and clicking the "Create" button. This allows you to create custom parameters for your Expressions Menu. 
	- When you click "Create", the Expression Parameters asset is automatically assigned to your Expressions Menu asset.
	- You can also create an Expression Parameters asset by selecting `Assets > Create > VRChat > Avatars > Expression Parameters`.
    - Expression Parameters have an option to copy parameters from an `Animator` to create your parameters quickly.

![What expression parameters look like by default.](/img/avatars/expression-menu/default-parameters.png)


3. Select the Expression Parameters asset to customize it.
    - The asset contains [three parameters by default](https://creators.vrchat.com/avatars/animator-parameters/#default-av3-aliasing): `VRCEmote`, `VRCFaceBlendH`, and `VRCFaceBlendV`. You can safely delete them unless you use them in your avatar or don't want to create your own Expression menu.
    - Your animators always have access to VRChat's [built-in parameters](/avatars/animator-parameters). For example, you shouldn't add the "AFK" or "GestureLeft" parameters to your Expression Parameters asset.
4. Enter the names of your custom parameters.
    - These names should match the parameters in your animators.
    - You can categorize your parameters by using `/`. For example, `Clothing/Hoodie` and `Clothing/Hat`.
    - VRChat has a few [built-in parameters](https://creators.vrchat.com/avatars/animator-parameters/#parameters).
    You can always use them in animators - don't add them to your own Expression Parameters.
5. Choose a type for each parameter. The type should match your animator's <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/AnimationParameters.html">parameter type</UnityVersionedLink>.
	- `Int` can be any whole number between 0 and 255.
	- `Float` can be any number between -1.0 and 1.0.
	- `Bool` can be either `true` or `false`.
6. Select the `Default` value to set the default value of each parameter.
	- When the avatar is reset, the parameter will revert to this value.
7. Enable `Saved` for parameters that shouldn't reset themselves whenever the avatar is loaded. If your avatar has customization options or settings, `Saved` will prevent them from being reset after switching to a different world or avatar.
8. Enable `Synced` if the state of this parameter should be sent to all other players over the network.
	- The [sync type](/avatars/animator-parameters/#sync-types) of custom parameters is Playable (except for [Puppet controls](expression-menu-and-controls#types-of-controls)).

Next, you should add both assets to your avatar descriptor.

![What expression parameters look like by default.](/img/avatars/expression-menu/avatar-descriptor-params.png)

9. Select your avatar descriptor and scroll down to the "Expressions" section.
10. Change the "Menu" property to your expressions menu.
11. Change the "Parameters" property to your expression parameters.

After adding both assets to your avatar descriptor, all your expression parameters will now be available in the expression menu, allowing you to customize it.

![What expression parameters look like by default.](/img/avatars/expression-menu/populated-menu.png)

12. In the inspector, click "Add Control." Up to 8 controls can be added to a single menu.
13. Choose a name and [type](/avatars/expression-menu-and-controls#types-of-controls). 
14. You can also add icons, and submenus, or change the order of the controls here.
  - You can find some default icons in `VRCSDK/Assets3/Expression Menu Icons/` .

## Types of Controls

The Expressions Menu supports different types of controls. Choose the type that's most suitable for your use case.

| Type             | Description                                                                                                                                                                                     |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Button           | Sets a parameter when clicked, then resets after the sync/reset has been sent-- usually after about a second. Cannot be held down.                                                              |
| Toggle           | Sets a parameter when the toggle is on, resets when the toggle is turned off.                                                                                                                   |
| Sub-Menu         | Opens another Expression Menu.  Additionally it may also set a parameter when entered, if so that parameter is reset to zero when you exit that menu. You can put sub-menus into sub-menus!     |
| Two-Axis Puppet  | Opens an axis puppet menu that controls two float parameters depending on the joystick position. The parameters are mapped to vertical and horizontal. The float values range from -1.0 to 1.0. |
| Four-Axis Puppet | Opens an axis puppet menu that controls four float parameters depending on the joystick position.  The parameters are mapped in order, up, right, down, left. The float values are 0.0 to 1.0.  |
| Radial Puppet    | Open a radial puppet menu that controls a single float parameter, kind of like a progress bar that you can fill! The float value is 0.0 to 1.0.                                                 |

Controls of the "Puppet" type have two special features:
- The "Parameter" property is optional for puppet controls. If you use it, it will be set while the puppet control is open. This allows your animators to detect whether the puppet control is currently open.
- When you exit a puppet control in VRChat, it keeps the "Parameter Horizontal/Vertical/Radial" value (until you re-open the control or change the value elsewhere). This allows you to navigate your radial menu while "freezing" the puppet control in any state.

### Puppet menu sync

The **Puppet** controls use [**IK Sync**](/avatars/animator-parameters#sync-types) when open. If you want sync that is as close as possible to your inputs for fast/quick movements, you should use a Puppet menu.

**Button**/**Toggle** uses **Playable Sync** which updates on-demand, instead of continuously, and is appropriate for things you "turn on/off" but don't need highly precise syncing.

Puppet menu sync always updates at the maximum rate available, and it smooths the values for remote users - much better when timed replication is important.




---

## ドキュメント: index.md

```metadata
階層: /avatars/index.md
ディレクトリ: avatars
ファイル名: index.md
拡張子: .md
サイズ: 12.14 KB
最終更新: 2025-06-05T03:07:52.735Z
```

---
sidebar_position: 0
---

import UnityVersionedLink from '@site/src/components/UnityVersionedLink.js';

# Avatars

VRChat allows you to create and upload custom avatars! This category explains how to use VRChat's Avatars 3.0 SDK.

## Creating Avatars

To get started, check out [Creating your first avatar](/avatars/creating-your-first-avatar).

There's a whole 'Avatars' category on the sidebar to check out. Here are some of the more impactful and important pages:

- [Rig Requirements](/avatars/rig-requirements) explains how to set up your custom 3D model's hierarchy for VRChat.
- [Avatar Performance Ranking System](/avatars/avatar-performance-ranking-system) explains how some avatars achieve an 'Excellent' performance, and others 'Very Poor'.
- [Avatar Optimization Tips](/avatars/avatar-optimizing-tips) - Now that you know _why_, check out this page to learn how to get all your frames back.
- Continue reading this page to learn more about important Avatars 3.0 SDK concepts.

## What is Avatars 3.0?

**Avatars 3.0** is our name for all the features available for avatars in VRChat. AV3's features are focused on improving expression, performance, and the abilities of avatars in VRChat.

Avatars 3.0 is heavily integrated with the [Action Menu](https://docs.vrchat.com/docs/action-menu) for controlling and interacting with the avatar you're wearing. It's probably best if you hop in and try out the Action Menu before building an AV3 avatar!

## Prerequisites

- [Install & set up the VRChat Avatars SDK](/sdk)
- [Create your first avatar](/avatars/creating-your-first-avatar)

## Understanding the Concepts

In order to understand and use Avatars 3.0, you need to know a few concepts. These concepts will help you understand the construction of avatars, how best to assemble them, and the intended use of various systems.

### Unity Systems

This document is written with the assumption that you know a bit about <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/class-AnimatorController.html">Unity Animators</UnityVersionedLink>. In particular, you should ensure you have basic working knowledge of:

- Animators and animations
- Animator layers, layer weights, and blending
- States and transitions
- Animator parameters
- State behaviors
- Avatar masks

It can also help to know about things like:

- State exit time
- Loop Time for animations
- (Advanced) Time Sync between layers
- (Advanced) Blend trees

### Basics

With Avatars 3.0, you can create a basic avatar with simulated eye movement and visemes very quickly. 

1. Import your avatar, rig as humanoid. Set up your materials, etc.
2. Add the Avatar Descriptor component.
3. Define the eye bones, if you want simulated eye movement.
4. Define the viseme type, if you want visemes. Assign the jaw-flap bone in the Rigging Configuration Screen, or define your visemes by blendshapes. Same as Avatar 2.0.
5. Set your viewpoint.
6. Build and upload!

If you are making a non-humanoid avatar please read the Generic Avatars section below.

You're done! This will create a basic avatar with default gestures and actions. There's some built-in things you can take advantage of, so even if someone slaps in an avatar with blendshapes/bones named a certain way, you'll get some basic Avatar 3.0 features.

However, even with these basic upgraded systems, there are some new features.

### Local Avatar Testing

Ever wanted to iterate and test an avatar without uploading it? Well, with Avatars 3.0, now you can!

In the "Builder" tab of VRChat SDK control panel, you can now select "Build & Test" at "Offline Testing" section. When you click this, your avatar will be built, and then copied into the folder `%LocalAppdata%Low\VRChat\VRChat\Avatars`.

When you launch VRChat, you'll be able to access this avatar locally by looking in the "Other" section of the Avatar menu! Only you will be able to see it, but you can make changes to your avatar, click "Build & Test" again, and after a short build, your avatar will be updated. Simply re-select the avatar in your menu and click "Change" again, and you'll swap into the new testing avatar.

This avatar is _only_ visible to you! To everyone else, you'll look like you're wearing the last avatar you were wearing before swapping into the local test avatar. For our AV3 testers, this made iteration a TON faster. We hope you like it!

To delete the copied local test avatar, go to "Content Manager" tab of the VRChat SDK control panel. You will see your avatar in "Test Avatars" section at the bottom. Click "Delete" and it will disappear from "Other" section of the Avatar menu when you reopen it.

### Simulated Eye Movement

Simulated eye movement is where your eyes will move around, looking at things around you. This isn't _eye tracking_ but it is a pretty good way of making your avatar look more "alive".

You can preview your setup in the editor and adjust how your avatar's eyes look in a combination of states, which are used to determine how your eye bones are set up.

Blinking can be handled via blendshapes or bones. Blendshapes are the usual method, but we included bones as well to allow for more setups.

Blinking blendshapes are defined by three blendshapes, described below:

- Blink - Both eyes blinking
- Looking Up - Blendshape used when looking up-- use this to tweak eye/iris/lid/eyebrow positioning
- Looking Down - Blendshape used when looking down, use this similarly to LookUp

You can set these blendshapes to `-none-` if you don't want to use them.

In addition, you'll notice two sliders-- one goes from Calm to Excited, and the other goes from Shy to Confident. Calm / Excited affects how often you blink. Shy / Confident affects how often you look at other players, and how long your gaze remains on other player's faces until you look away.

You'll learn more about this when we talk about state behaviors, but you can set states in your animator to **disable eye animations** when you reach that state. You can set it up such that you don't have to worry about your blendshapes being overdriven because your "happy" mood closes your eyes, and your blinking is still firing off. 

### Blendshape / Bone-based Visemes

If you just want to stick with the standard jaw-flap bone or blendshape-based visemes, we've got you covered. Both are still present and work just fine.

In addition, you can now configure the angle of the jaw-flap bone viseme for some additional customization!

However, in Avatars 3.0, you can also access an Animator Parameter which indicates which viseme should be currently playing! This means if you can animate it, **you can use it in a viseme.** No more trickery for 2D mouths, robots, whatever-- you can just animate whatever you like for your visemes.

The `Viseme` animator parameter is updated in all viseme modes.

### Proxy Animations

You'll probably notice that the SDK includes a bunch of animations named `proxy_animationName`. These animations are "placeholders" for a variety of default VRChat animations. If you use an animation that starts with `proxy_`, VRChat will attempt to replace it with a built-in animation. This can be done in any playable layer.

Although we will not replace an animation with a `proxy_` prefix if the suffix does not match one of our built-in animations, it is probably best practice to avoid naming any of your animations with the prefix `proxy_`.

### Use Auto Footstep

This is an option in the AV3 Avatar Descriptor. It is on by default.

**"Use Auto Footstep"** only applies to 3-point or 4-point tracking. Turning it off means you're disabling the procedural lower body animation for room-scale movement. This procedural animation is what plays when you move around in room-space while in 3 or 4-point tracking.

Leaving Auto Footsteps on (which is the default state) will still allow you to enable/disable tracking via the Tracking Control state behavior.

If Auto Footsteps is off, enabling/disabling tracking on your legs and hips won't do anything, and you're relying on your animations to drive your lower body at all times.

### Force Locomotion Animations

This is an option in the AV3 Avatar Descriptor. It is on by default.

**"Force Locomotion Animations"** is on by default. It only applies to 6-point tracking (Full-Body Tracking). When "Locomotion Animations" is on, locomoting in FBT (as in, moving your joysticks) will play a walking/running animation as determined by your Base playable layer.

When "Locomotion Animations" is off, locomoting in FBT will NOT play the walking/running animation. This is useful if you wish to "mime" your walking with your full-body tracking movement. **If you are turning off "Locomotion Animations", do not use the default Base and Additive layers.** You're expected to make your own!

### Write Defaults on States

<UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/class-State.html">Write Defaults</UnityVersionedLink> is an option available for each state in an Animator Controller.

Write Defaults "Off" states will set only the animated property values, and those values will not change unless animated again. This can make it easier to keep track of what properties are animated through any specific layer.

Write Defaults "On" states will set default values for properties that are not being animated. This means that if you are animating a property value to "1" from "0", the value will revert to the default "0" upon exiting the state, unless the subsequent state continues to animate the value as "1".

Regardless of which option you choose, **we recommend keeping your usage of Write Defaults consistent across the entire avatar** - in other words, have Write Defaults "Off" for all states, or "On" for all states. Having both "Off" and "On" states on an avatar is known to cause unexpected property values to be set. This is commonly known as "Mixed Write Defaults". The SDK will give you a warning if it detects that you've done this.

VRChat uses Write Defaults set to "Off" in its built-in and example animators.

If you decide to set Write Defaults to "Off":
- Write Defaults is set to **On** for newly created states, so you'll need to change this value for each new state you create.
- You may need to add animations to initialize or reset properties with specific values.
- It's recommended that for all states in a layer, you explicitly animate every property affected by that layer.
- Each state should have an animation clip ("motion" in the state options) that animates at least one property. It does not have to be a valid property reference. States with "None" motion or entirely empty clips will behave as if Write Defaults is "On".

:::caution Additive layers and direct blend trees

VRChat's avatar creator community recommends setting Write Defaults to "On" for:
- Layers that use additive blending
- Blend trees that use direct blending

You should do this even if you are using "Off" for the rest of the avatar. The SDK will avoid generating warnings about mixed Write Defaults settings in these cases.

:::

### Generic Avatars

Avatar 3.0 also supports non-humanoid generic avatars. If you want access to similar features that AV3 Humanoids have access to, you'll need to follow a few guidelines:

- Import your generic model as an FBX and assign it the 'Generic' rig type, so that an "avatar" object is created
- Make sure this avatar object is referenced in the avatar field of the Animator component at the root of your avatar (the same Game Object as the avatar descriptor).
- Leave the animator controller blank (it will be stripped at runtime) and use the Playable Layers to define your custom animation controllers. Generic avatars have 3 Playable layers: Base, Action, and FX, as the other layers are specific to Humanoids.

If you do not follow these steps, your generic avatar will not have access to many Avatars 3.0 features such as Expression Parameters and State Behaviours. If you are fine with that, you can add an animation controller directly into the root animator, leaving the avatar field blank. This method could be useful if you are just building a hierarchy of static objects in Unity and want a simple animation.


---

## ドキュメント: per-platform-avatar-overrides.md

```metadata
階層: /avatars/per-platform-avatar-overrides.md
ディレクトリ: avatars
ファイル名: per-platform-avatar-overrides.md
拡張子: .md
サイズ: 3.29 KB
最終更新: 2025-06-05T03:07:52.735Z
```

# Per-Platform Avatar Overrides

Per-platform overrides allow you to configure different versions of your [cross-platform](/platforms/android/cross-platform-setup) avatar for multiple platforms. When you build & publish your avatar to more than one platform, the SDK automatically uploads different versions of your avatar to each selected platform under the same avatar ID.

Per-platform overrides make it easy to upload an optimized version of your avatar for Android and a fully featured version of your avatar for PC.

:::note

You can manually upload different versions of your avatar for different platforms by performing multiple uploads without changing the avatar ID. However, this is more cumbersome and time-consuming than cross-platform overrides.

:::

# Setup

To set up per-platform overrides, follow these steps:

1. Set up all the versions of the avatar you want to use in the same scene.
2. Switch to the Builder tab in the SDK.
3. Select the avatar you want to use as a base for the overrides. This will be the avatar you select in the Builder panel of the SDK to click "Multi-Platform Build & Publish"
4. Click the `⋮` button next to the "Selected Avatar" dropdown.

![Options Menu](/img/avatars/per-platform-avatar-overrides/options-menu.png)

5. In the popup that appears, press "Add per-platform override".
6. Select the platform for the override, then select the avatar to use for that platform.

![Per-Platform Overrides](/img/avatars/per-platform-avatar-overrides/per-platform-override.png)

- Repeat the process for all the platforms for which you want to set up overrides.
- The changes are saved automatically, so you can proceed with a multi-platform build & publish.

After setting up per-platform overrides, the SDK will build and upload the "override" version of the avatar for each platform when you do a multi-platform build.

After the overrides are set up - you will also see a new component added to the root of the avatar object called "VRC Per Platform Overrides". This component has the same functionality as the per-platform overrides in the Builder panel, however, we encourage you to use the Builder panel for setting up the overrides. This component is primarily intended for viewing existing overrides and for use in community-made tools.

## Editor Tooling

If you want to create an editor tool that can query the per-platform overrides for an avatar, use the following method:

```cs
using VRC.SDK3A.Editor;

//...

PerPlatformOverrides.GetPlatformOverrides(myAvatarGameObject);
```

It will either return a `List<PerPlatformOverrides.Option>` or `null` if no overrides are set up.

You can also setup the overrides yourself using the following method:

```cs
using VRC.SDK3A.Editor;

//...

PerPlatformOverrides.SetPlatformOverrides(myAvatarGameObject, new List<PerPlatformOverrides.Option> {
    new PerPlatformOverrides.Option {
        platform = BuildTarget.Android,
        avatar = myAndroidAvatarDescriptor
    }
});
```

This will automatically add the override component to the avatar if it doesn't already exist.

The per-platform overrides are currently only set up on the "main" avatar object. So you would generally want to scan every object with a `VRC_AvatarDescriptor` on it for presence of the overrides to get the full picture.


---

## ドキュメント: playable-layers.md

```metadata
階層: /avatars/playable-layers.md
ディレクトリ: avatars
ファイル名: playable-layers.md
拡張子: .md
サイズ: 11.65 KB
最終更新: 2025-06-05T03:07:52.735Z
```

import UnityVersionedLink from '@site/src/components/UnityVersionedLink.js';

# Playable Layers

When you create animations for your VRChat avatar, you'll utilize VRChat's 'Playable Layers.' They allow you to cleanly separate some things you might want to do with your avatar into their own animations - such as running, jumping, giving a thumbs-up, smiling, wagging your tail, and combinations of these.
:::caution Unity Knowledge Required

This document is written with the assumption that you know a bit about <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/class-AnimatorController.html">Unity Animators</UnityVersionedLink>.
:::

In the Avatars 3.0 Avatar Descriptor, all humanoid avatars have five buttons:
- Base
- Additive
- Gesture
- Action
- FX

Generic avatars only have three buttons:
- Base
- Action
- FX

These are **Playable Layers**. Each of them takes a Unity Animator, and they layer on top of each other. In other words, you've got five root animators to play with, and each of them can have several **Animator Layers**.

These layers apply in order-- in other words, Base gets applied, then Additive, then Gesture, Action, FX. For example, if something in Additive animates a bone (with 1.0 weight), and then something in Action animates that same bone (with 1.0 weight), the Action animation will take precedence.

We have example Playable Layers available in the SDK. Depending on how you learn and iterate on things, it might be easier for you to use and edit these default layers to figure things out!

When you are running VRChat and you're wearing (or viewing) an Avatar 3.0 avatar, all of these Playable Layers are put together into a combined Animator. This Animator is the root, main animator of your avatar, and you can control any part of it. **This means that there is no reason to add any additional animators on your avatar.** 

As an aside, you should never use the same controller in multiple Playable Layers. This may work for some setups, but it is **very** poor practice and will cause major issues as you expand the functionality of your avatar.
:::danger Only Use Animation Controllers

We only support the use of Animation Controllers in Playable Layer slots. Do not use any other type of controller-- you will run into errors or will be unable to upload the content.
:::
What do these Playable Layers do? Here's the short version:

**Base:** Stuff that should always play, react to movement (like locomotion), or the locomotion state of your avatar (running, falling, crouching, etc). Transform animations only.
**Additive:** Stuff that Base is already using, but you want to "add" to it-- like a breathing animation. Transform animations only.
**Gesture:** Things that get triggered by hand OR by the Expression menu. You can also use this for "idle animations" like a wagging tail, flapping wings, or moving ears. Transform animations only.
**Action:** Full override, similar to AV2 emotes. Transform animations only.
**FX:** Same as Gestures, but for everything that *isn't* a Transform position, rotation, or scale animation.

That's great, but let's go into some more detail.

## Base

The Base layer contains locomotion animations, including blend trees for walking, running, strafing. It also includes animation states for jumping, falling, falling fast, crouching, and crawling, among other things.

Keep in mind that if you put something in here, you'll have to redefine your locomotion animation states. This is pretty complex! Take a look at the example Base Playable Layer to see how complex it can get.

Animations in Base should _only_ affect transforms, and all layers should be using Avatar Masks to ensure you're only affecting the appropriate transforms.

## Additive

The Additive layer is meant for additive transform movement on top of humanoid bones that are animated in Base-- things like breathing animations that can "add on" to the Base layer.

**If you want to add an idle animation to non-humanoid bones-- like a tail, ears, or etc-- use Gesture instead!** Additive is *specifically* for humanoid bones.

The Additive layer is special because it is _always_ set to "Additive" blending. In short, if you've got a transform that moves during locomotion, the Additive animation will "add" its animation on top. This can act really weirdly if you do crazy things to bones in Additive, so try to keep it pretty minimal.

:::caution Additive First Layer Avatar Mask Ignored

The first layer (base layer, 0th layer, etc)'s Avatar Mask is ignored. This is for internal masking purposes. You can still mask other layers, but any mask you apply to the first layer will be ignored.
:::

Animations in Additive should _only_ affect transforms.

## Gesture

The Gesture layer is for animations that need to act on individual body parts while still playing the underlying animations for the rest of the body. Kind of like AV2 Gestures, but applied to any part of the body.

Utilize Avatar Masking to ensure that the animations *only* affect the parts of the avatar you want to animate! So, if you want your gesture parameters to only make hand shapes for left/right hand, you'll want to mask out those hands on each of the layers.

In addition, if you want to have an "idle" animation for non-humanoid bones like a tail, wings, ears, etc-- Gesture is where you should put it.

Animations in Gesture should _only_ affect transforms.

## Action

The Action layer is for bone animations that will override all other layers, when you need to take over total control of the character. Basically, think AV2 "Emotes".

This layer is **blended to zero by default.** Before you do anything in the action layer, you need to use the [Playable Layer Control State Behavior](/avatars/state-behaviors#playable-layer-control) to blend this layer up before transitioning to the actual action you're performing! Make sure you blend it back to zero when you're done.

Animations in Action should _only_ affect transforms.

## FX
FX is a **special layer.** On every other layer, you should not be using material animations, shader property animations, or blend shape animations, because they aren't copied to your mirror clone. Only transforms are.

However, in the FX layer, everything is copied over! In other words, ***everything that isn't a humanoid transform/muscle animation should go into the FX layer.*** This includes (but is not limited to) things like enabling/disabling GameObjects, components, material swaps, shader animations, particle system animating, etc.

The mask in the first FX layer, by default is empty, this will (at avatar init) create a default mask that disables all humanoid muscles, but enables all GameObject animations. This means that any animations in the hierarchy should work, although it is still NOT RECOMMENDED to animate transforms here.

If you have non-muscle animations in your gestures (e.g. your Gesture <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/class-AvatarMask.html">mask</UnityVersionedLink> has any transforms checked at the bottom) those same transforms must be DISABLED in your FX mask. This will allow your Gesture animations to "show through" the FX layer.

:::info Example

Let's say your avatar has the following setup:
- You have a tail on your avatar (a chain of bones not part of humanoid hierarchy).
- Your Gesture animator layer for the tail has a special mask with only the chain of bones enabled.
- Your other Gesture animator layer with an "all-parts mask" also has those bones checked (along with the other body parts animated in the rest of the controller).

In this case, you'd also want to create a custom mask in the first FX layer. This would mask OFF all the muscles (human diagram all red) and mask OFF all the bones in the tail.
You'd also want to make sure this mask has the checkboxes ON for any transforms that have components you will be animating for FX. E.g. a body skinned mesh for animating blend shapes or materials.

:::

Note that if you have a game object in your hierarchy that has both an animated transform (in Gesture) and an animated effect component (in FX), this will not work with the requirements for the masks. This can occur if you have a simple static-mesh embedded in your hierarchy that you are animating in Gesture, but also applying a material change to in FX. Another example, would be putting a particle effect component directly on the example tail bones above. The simple workaround is to make a child game object and put the static-mesh or effect on that. You would not animate the transform of the child, only the parent. If you follow these steps, you should not need to put transform animations in the FX layer.

## Additional Poses
There are some additional poses available for Avatars 3.0 avatars. The buttons for these are under the Playable Layers.

### T-Pose
You can now provide your own T-Pose!

The T-Pose is used to determine various measurements of your avatar, especially for placement of your viewpoint (or view-ball). Viewpoint is dependent entirely on where your view-ball is when your avatar is in this T-Pose animation you provide.

![Standard T-Pose - [Mixamo](https://www.mixamo.com)](/img/avatars/playable-layers-1.png)

Secondly, it is important for the wrist alignment/twist. The way your wrists are lined up in relation to the palm-down position will affect how your controller twisting in space will turn your wrist and arm.

Finally, your t-pose determines your wingspan-- your full length of your arms when in T-Pose. This also determines your avatar's interpupillary distance (IPD), or the distance between your avatars eyes. Having arms that are too long will make your IPD wider, making everything seem smaller. Having arms that are too short will make your IPD narrower, making everything seem larger.

In addition, (significant) joint bends in T-Pose aren't a good thing. As an example, if your elbows are bent in T-pose, this may affect many different things about your avatar that work off your proportions.

### IK Pose
IK Pose is used to determine major joint bends. In the IK pose, your joints should be bent slightly in the direction they're intended to bend. 

As an example, VRChat will look at the elbow bend from your IK Pose and determine if there is a angle bend in any given direction. That bend determines how your elbow bends.

The foot's rotation in IK Pose will determine how the knees will bend. This is set by first assuming the knee will bend straight forward relative to the avatar, then saving that direction against the foot's rotation in IK Pose. For example if the feet are pointed toes outward in IK Pose, that means the inside edge of the foot is more forward facing and therefore the knees will bend towards the inside edge of the foot. In the opposite case, if the feet are pointing more inward in IK Pose, the outside edge of the the foot is more forward facing (straight forward direction relative to the avatar) and so the knee will tend to bend towards the outside edge of the foot in that case.

In short: if you want your knees to bend more inward, rotate your feet outward in IK Pose. If you want your knees to bend more outward, rotate your feet inward in IK pose.

### Sitting Pose
The controller used in this slot is used for both animation and posing. When you sit, the viewpoint of your avatar is used for calibration. The animation is played, allowing you to create a "sitting down" animation, as well as a "sitting" idle animation.

If you want to make your own, fair warning: this can take some significant tweaking to get right! You may want to employ transition states for sitting down/standing up that will help a bit with how your avatar looks while sitting.

---

## ドキュメント: rig-requirements.md

```metadata
階層: /avatars/rig-requirements.md
ディレクトリ: avatars
ファイル名: rig-requirements.md
拡張子: .md
サイズ: 6.50 KB
最終更新: 2025-06-05T03:07:52.735Z
```

# Rig Requirements

:::caution

This page is significantly out of date, but should still be mostly accurate.
:::

This page explains some of the requirements for configuring your avatar's rig, bones, and armature. If you follow these requirements, your avatar's limbs should move correctly in VRChat.

You can also check the [SDK build panel](/avatars/creating-your-first-avatar#step-6---going-to-the-build-tab--checking-if-the-avatar-is-ok) and look for errors in the "Validations" section.
## Export Settings

When exporting your rig from your 3D editor of choice, ensure your coordinate settings are correct. Most of the time, the defaults are correct.

For Blender, ensure that your rest X rotation is 90 degrees.
## Humanoid Rig
Unity will flag your humanoid rig configuration if it does not meet the Mecanim requirements for a humanoid. Please read and be familiar with the [Unity Documentation on configuring avatars](https://docs.unity3d.com/Manual/FBXImporter-Rig.html).

:::danger Humanoid avatar must have head, hands and feet bones mapped.

You will see this message from the VRChat Build Control Panel if your avatar rig is humanoid but does not have the essential bones mapped.",

:::

:::note Non-human avatars

If your avatar diverges greatly from a human (ie. quadruped, hunching monster, etc), you should consider using a Generic rig and your own Animation Controller. See the SimpleAvatarController for an example. This is more advanced than making a humanoid, so you should be very familiar with Unity's Animation Controller system.
:::

## Finger Mappings

:::caution Thumb, Index, and Middle finger bones are not mapped, Full-Body IK will be disabled.

**This warning does not appear for SDK3 avatars, as they have no problem with using armatures without finger bones.** This error only occurs when using VRChat SDK2, which is deprecated and should not be used.

In order to have full IK (allowing crouching and automatic foot placement) you need to have these three finger bones mapped. If you ignore this warning, your avatar will not be able to crouch, and it's feet will not automatically step (unless you use controller locomotion).

It will also prevent custom animation overrides on hand gestures from being played back. (This is **not** currently mentioned by the warning in the SDK.)
:::

## Spine Hierarchy

:::danger Your rig has the UPPERCHEST mapped in the Humanoid Rig. This will cause problems with IK.

**This warning does not appear for SDK3 avatars, as they have no problem with using armatures with a mapped upper chest.** This error only occurs when using VRChat SDK2, which is deprecated and should not be used.

If you must use SDK2, leave the upper chest bone blank when configuring your humanoid.
:::

:::danger Spine hierarchy missing elements, make sure that Pelvis, Spine, Chest, Neck and Shoulders are mapped.

These bones must all be mapped. If you get this message make sure none of these slots are empty. Note that the Neck and Chest slots are optional for Mecanim, but required for VRChat.
:::

:::danger Spine hierarchy incorrect. Make sure that the parent of both Shoulders and the Neck is the Chest.

For the IK to work properly, you must have a specific hierarchy of bones around the chest. In your rig, your shoulder bones (mapped into Left Arm > Shoulder, Right Arm > Shoulder slots) must be direct children of your chest bone (mapped into Body > Chest slot). Also, the neck bone (mapped into Head > Neck slot) must also be a direct child of the Chest.
:::

## Arm and Leg Hierarchy

:::caution LowerArm is not first child of UpperArm or Hand is not first child of LowerArm: you may have problems with Forearm rotations.

VRChat's IK system looks at the first child of a bone when determining the bone layout. If you have other child bones, like prop-placement bones or twist-bones in your rig, they can confuse the IK. In this particular case, the SDK is seeing that your LowerArm is not the first-listed child of your UpperArm bone.

To fix this, move the child bone to the first position in the list of children of the parent bone. **You will have to unpack your avatar prefab to do this.** 

Note that this message is naming the slot, not the actual bone name in your rig, so you'll have to look to see what bone is in that slot.
:::

:::caution LowerLeg is not first child of UpperLeg or Foot is not first child of LowerLeg: you may have problems with Shin rotations.

See above.
:::

## Eye bones

Configure avatar's eye bones as follows: 

- The Y axis should point up.
- The Z axis should point forward.
## General Hierarchy

:::caution This avatar has a split heirarchy (Hips bone is not the ancestor of all humanoid bones). IK may not work correctly.

Some rigs split the hierarchy into two sections, upper and lower body. In this case the bone you put into the Body > Hips slot must be the ancestor (parent or higher) of the rest of the human bones you are mapping. Be very careful with these kinds of rigs! Often, the ancestor of these bones is a root bone on the ground or another placement which is a bad placement for a hip bone. Many of these rigs are unsuitable for use with VRChat and need to be re-rigged to work properly.
:::

## Full-Body Tracking
There are special considerations if you are using Full-Body tracking, ie. you have 3 HTC Tracking Pucks connected. There are several recommendations that will ensure that your avatar works well when using Full-Body tracking.

To see more detailed information on Full-Body Tracking rigging requirements, see our [Full-Body Tracking system guide](https://docs.vrchat.com/docs/full-body-tracking).
:::caution The angle between pelvis and thigh bones should be close to 180 degrees (this avatar's angle is ___). Your avatar may not work well with full-body IK and Tracking.

Full-body tracking is sensitive to the angle between the hip and upper leg bones. It's best to measure this angle when the AvatarTPoseController is applied to your avatar. Ideally, the hip bone is pointing straight up and the upper leg bones point straight down in the TPose, but slight divergence is okay. You can ignore this message if you are not going to use Full-Body Tracking.
:::

## Toe Bones
It is not required to map the Toe bones in a humanoid avatar. However, if you DO map them, your avatar is able to move up and down on their tiptoes. Mapping the toes also makes the automatic foot-stepping look more natural, as well as improving the appearance of balance by aligning the auto stance to the beginning of the toe bone rather than the heel.

---

## ドキュメント: shader-fallback-system.md

```metadata
階層: /avatars/shader-fallback-system.md
ディレクトリ: avatars
ファイル名: shader-fallback-system.md
拡張子: .md
サイズ: 7.88 KB
最終更新: 2025-06-05T03:07:52.736Z
```

---
title: "Shader Blocking and Fallback System"
slug: "shader-fallback-system"
hidden: false
createdAt: "2018-09-25T20:49:23.385Z"
updatedAt: "2022-08-12T01:21:02.481Z"
---
This page serves as a description of the Shader Blocking System, how it operates, and how shader authors can work with it so that their shader falls back gracefully when a user has Shaders blocked on an avatar using a given shader.

## VRChat 2021.4.2 Fallback System Upgrade
Shader fallback improvements work by using the "Tags" field at the top of the shader.
```text
Tags{"Queue"="Geometry"}
```
The tags field might look like this by default.

You can now add different tags, under the `VRCFallback` name, to specify which fallback shader to try to use:
```text
Tags{"Queue"="Geometry" "VRCFallback"="Toon"}
```
Some fallback tags are combine-able, you could for instance use `ToonCutout:`
```text
Tags{"Queue"="Geometry" "VRCFallback"="ToonCutout"}
```
The supported tags are as follows:

```text
Unlit
VertexLit
Toon
Transparent
Cutout
Fade
Particle
Sprite
Matcap
MobileToon
DoubleSided
Hidden //(this will hide the mesh from view if the shader is blocked, useful for things like raymarching effects.)
toonstandard
toonstandardoutline
```

Toon and Unlit can also be combined with Transparent, Cutout, Fade, and DoubleSided tags for more granular control. With Toon supporting such variations as DoubleSided Cutout.

:::caution

Please note that using Transparent or Fade tags with Toon will result in it falling back to Transparent Unlit. You might want to take that into account when picking fallback tags.
:::

Specifying any other tag will result in a Standard shader fallback.

The `toonstandard` and `toonstandardoutline` tags are special, in that they cannot be combined. They guarantee falling back to the `VRChat/Mobile/Toon Standard` and `VRChat/Mobile/Toon Standard (Outline)` shaders respectively. All same-named keywords and properties will be copied over.

If no tag is provided, the old fallback system will be used, following the pattern shader name, keywords, etc.

We now also copy ALL standard shader parameters to the fallback material, including the following:
```text
_MainTex
_MetallicGlossMap
_SpecGlossMap
_BumpMap
_ParallaxMap
_OcclusionMap
_EmissionMap
_DetailMask
_DetailAlbedoMap
_DetailNormalMap
_Color
_EmissionColor
_SpecColor
_Cutoff
_Glossiness
_GlossMapScale
_SpecularHighlights
_GlossyReflections
_SmoothnessTextureChannel
_Metallic
_SpecularHighlights
_GlossyReflections
_BumpScale
_Parallax
_OcclusionStrength
_DetailNormalMapScale
_UVSec
_Mode
_SrcBlend
_DstBlend
_ZWrite
```
## Old Fallback System
When a shader is blocked by the Safety System, it is first checked for one of the internal pre-compiled shaders in this list:
```text title="Pre-Compiled Internal Shaders"
  "Standard",
  "Standard (Specular setup)",
  "Effects/Rim",
  "Effects/GlowAdditiveSimple",
  "Legacy Shaders/Bumped Diffuse",
  "Legacy Shaders/Bumped Specular",
  "Legacy Shaders/Decal",
  "Legacy Shaders/Diffuse",
  "Legacy Shaders/Diffuse Detail",
  "Legacy Shaders/Diffuse Fast",
  "Legacy Shaders/Lightmapped/Diffuse",
  "Legacy Shaders/Lightmapped/Specular",
  "Legacy Shaders/Lightmapped/VertexLit",
  "Legacy Shaders/Parallax Diffuse",
  "Legacy Shaders/Parallax Specular",
  "Legacy Shaders/Reflective/Bumped Diffuse",
  "Legacy Shaders/Reflective/Bumped Specular",
  "Legacy Shaders/Reflective/Bumped Unlit",
  "Legacy Shaders/Reflective/Bumped VertexLit",
  "Legacy Shaders/Reflective/Diffuse",
  "Legacy Shaders/Reflective/Parallax Diffuse",
  "Legacy Shaders/Reflective/Parallax Specular",
  "Legacy Shaders/Reflective/Specular",
  "Legacy Shaders/Reflective/VertexLit",
  "Legacy Shaders/Self-Illum/Bumped Diffuse",
  "Legacy Shaders/Self-Illum/Bumped Specular",
  "Legacy Shaders/Self-Illum/Diffuse",
  "Legacy Shaders/Self-Illum/Parallax Diffuse",
  "Legacy Shaders/Self-Illum/Parallax Specular",
  "Legacy Shaders/Self-Illum/Specular",
  "Legacy Shaders/Self-Illum/VertexLit",
  "Legacy Shaders/Specular",
  "Legacy Shaders/Transparent/Bumped Diffuse",
  "Legacy Shaders/Transparent/Bumped Specular",
  "Legacy Shaders/Transparent/Cutout/Bumped Diffuse",
  "Legacy Shaders/Transparent/Cutout/Bumped Specular",
  "Legacy Shaders/Transparent/Cutout/Diffuse",
  "Legacy Shaders/Transparent/Cutout/Soft Edge Unlit",
  "Legacy Shaders/Transparent/Cutout/Specular",
  "Legacy Shaders/Transparent/Cutout/VertexLit",
  "Legacy Shaders/Transparent/Diffuse",
  "Legacy Shaders/Transparent/Parallax Diffuse",
  "Legacy Shaders/Transparent/Parallax Specular",
  "Legacy Shaders/Transparent/Specular",
  "Legacy Shaders/Transparent/VertexLit",
  "Legacy Shaders/VertexLit",
  "MatCap/Vertex/Textured Lit",
  "Mobile/Bumped Diffuse",
  "Mobile/Bumped Specular",
  "Mobile/Bumped Specular (1 Directional Light)",
  "Mobile/Diffuse",
  "Mobile/Unlit (Supports Lightmap)",
  "Mobile/Particles/Additive",
  "Mobile/Particles/Alpha Blended",
  "Mobile/Particles/Multiply",
  "Mobile/Particles/VertexLit Blended",
  "Particles/~Additive-Multiply",
  "Particles/Additive",
  "Particles/Additive (Soft)",
  "Particles/Alpha Blended",
  "Particles/Alpha Blended Premultiply",
  "Particles/Anim Alpha Blended",
  "Particles/Multiply",
  "Particles/Multiply (Double)",
  "Particles/VertexLit Blended",
  "Sprites/Default",
  "Sprites/Diffuse",
  "Toon/Lit",
  "Toon/Lit (Double)",
  "Toon/Lit Cutout",
  "Toon/Lit Cutout (Double)",
  "Toon/Lit Outline",
  "UI/Default",
  "Unlit/FailShader",
  "VRChat/UI/Default"
```
If an internal shader is matched, the fallback is a new shader of the same type, but using the internally compiled shader. All the parameters are copied. New versions or variants not included will not work, since they will be replaced.

If the shader is not internally matched, The name of the shader (not the filename, but as provided in the top line of the shader source) is used to match some identifying features and replace with a fallback shader of similar type:
```text title="Fallback Shader Name Searches"
  "Unlit",
  "VertexLit",
  "Toon",
  "Outline",
  "Transparent",
  "Fade",
  "Cutout",
  "Particle",
  "Sprite",
  "MatCap"
```
These names can fall anywhere within the full string of the shader name.

Additionally, some shader properties are searched:
```text title="Shader Properties"
"_Ramp" == "Toon"
"_ALPHABLEND_ON" == "Transparent"
"_ALPHATEST_ON" == "Cutout"
```
All matching is case-sensitive.

Attempts are made to create fallback material that approximate the matched names. For example, names containing "Sprite" fallback to the Unity built-in "Sprites/Default" shader.

You can combine "Toon" and "Cutout", or "Toon" and "Outline" for combination shaders, however "Transparent" and "Fade" do not currently have a Toon Lit shader and instead fallback to "Unlit/Transparent".

Support for "Outline" is highly experimental and may be removed if performance suffers.

For "Toon" shaders, the "_Ramp" parameter is copied (Texture2D).
For "MatCap" shaders, the "_MatCap" parameter is copied (Texture2D)

If no matches are found in the shader name, a Standard material is used.

An attempt is made to transfer parameters named "_MainTex" and "_Color" to the fallback shader. If neither parameter is matched, a Matcap shader is provided with the color set to the user's rank color.

All this is highly subject to change as we tune the shader fallback system.

## Previewing Fallback Shaders for your Avatar
It's possible to force your own avatar to use fallback shaders locally via a toggle in the Action Menu. To activate this option, open your Action Menu and choose "Options" -> "Avatar" -> "Fallback Shaders". Once toggled on, any avatar you use will display locally with fallback shaders. The option will stay active when switching avatars but will reset on world change. This option does not affect how others view your avatar.

---

## ドキュメント: state-behaviors.md

```metadata
階層: /avatars/state-behaviors.md
ディレクトリ: avatars
ファイル名: state-behaviors.md
拡張子: .md
サイズ: 16.77 KB
最終更新: 2025-06-05T03:07:52.736Z
```

import UnityVersionedLink from '@site/src/components/UnityVersionedLink.js';

# State Behaviors

:::caution Unity Knowledge Required

This document is written with the assumption that you know a bit about <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/class-AnimatorController.html">Unity Animators</UnityVersionedLink>.
:::
When you've got a specific state selected in the Animator view, you'll be able to add State Behaviors. They're a bit like components for states. They do different things. Try adding them, and you'll see what they can do!

All state behaviors run on the first frame of the transition into that state. 

State behaviors *should* run no matter how long the state machine remains in the state containing the state behavior.

:::caution

The term "should" is deliberately used here, as in the <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/StateMachineBehaviours.html">Unity documentation</UnityVersionedLink> does not define any guarantee that state behaviors will execute given very small transition or state durations.

If you wanted to be **completely** safe, ensure the total time spent in the state containing the state behavior and any transitions directly to that state is a minimum of 0.02 seconds-- although in practice, this doesn't seem to be required.

:::

## Animator Layer Controller

![VRC Animator Layer Control component](/img/avatars/state-behaviours/animator-layer-control.png)

The Animator Layer Control allows you to blend the weight of a specific Animator Layer inside any given Playable Layer over any given time.

If the state is exited mid-blend duration, the target layer is immediately set to the goal weight.

The layer weight will remain until some other state runs this State Behavior again and resets it.

| Property Name  | Purpose                                                                                       |
| -------------- | -------------------------------------------------------------------------------------------- |
| Playable       | Allows you to select which Playable Layer you're affecting.                                  |
| Layer          | The Index of the Playable Layer you wish to affect. You can't change the weight of the 0th (base) layer-- it is always set to 1.0 weight. |
| Goal Weight    | Define the weight you want to blend to.                                                      |
| Blend Duration | Define the time period (in seconds) that you want the blend to take. 0 means instant.        |
| Debug String   | When this StateBehavior runs, this string will be printed to the output log. Useful for debugging. |


## Animator Locomotion Control

![VRC Animator Locomotion Control component](/img/avatars/state-behaviours/animator-locomotion-control.png)

The Animator Locomotion Control allows you to disable locomotion in a given state of an animator. The Locomotion state will remain until some other state runs this State Behavior again and changes it.

In Desktop mode, this disables translational movement, and restricts rotational (view) movement to the vertical axis. In VR, this disables translational and rotational controller movement and restricts half-body IK (full-body IK is unaffected). In both modes, the player's capsule is frozen in place.

| Parameter | Description |
| - | - |
| Disable Locomotion | If set to True, locomotion (moving with the controls) will be disabled. Roomscale movement will still be possible. If set to False, will enable locomotion. |
| Debug String | When this StateBehavior runs, this string will be printed to the output log. Useful for debugging. |

## Animator Temporary Pose Space

![VRC Animator Temporary Pose Space component](/img/avatars/state-behaviours/animator-temporary-pose-space.png)

The Animator Temporary Pose Space control allows you to move the viewpoint of the person wearing the avatar to the head at that given point of the animator state.

The view will remain set until some other state runs this State Behavior again and resets or clears it.

This behavior is executed when the delay time has elapsed.

Animator Temporary Pose Space should **only** be used when the view height needs to update due to a posture change, like sitting or laying on the ground. It cannot be used to "scale" the avatar being worn, and will cause major breaking problems if used in this manner.
:::danger

This state behavior **will not execute** if the state this behavior is on is exited or interrupted before `Delay Time` elapses!
:::

| Property Name | Purpose                                                                                                                                                   |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Pose Space    | Enter or exit. Enter sets the pose space, exit will clear it to default.                                                                                    |
| Fixed Delay   | Should the delay time be a fixed period of time, or a percentage of the state's duration?                                                                   |
| Delay Time    | If given a value, the viewpoint will be set after a delay. Useful if you're blending into an animation over a certain time.                                |
| Debug String  | When this StateBehavior runs, this string will be printed to the output log. Useful for debugging.                                                          |


## Animator Tracking Control

![VRC Animator Tracking Control component](/img/avatars/state-behaviours/animator-tracking-control.png)

The Animator Tracking Control allows you to enable or disable IK or simulated movement on various different parts of the avatar body. Setting the option to "No Change" will not change the body part from its current value. "Tracking" will set it to following IK or simulated movement. "Animation" will force that body part to respect values as given by the avatar's Animator.

If you set all IK tracking points to Animation, your animation will play as the Animation remotely, instead of being translated through Networked IK. For the various types of tracking, these "IK tracking points" are:

- Desktop: Head, Left Hand, Right Hand
- 3pt Tracking: Head, Left Hand, Right Hand
- 6pt / FBT Tracking: Head, Left Hand, Right Hand, Hip, Left Foot, Right Foot

:::note

All parts are IK-driven, aside from the Eyes and Eyelid, which are simulated. Mouth and Jaw are driven by visemes.

As an example, setting Left and Right Hand to Animation will ignore the position of the hands (and arms) as defined by IK, and will instead use any currently-active state's motion to define the position of the hands and arms. Setting them back to Tracking will use IK instead. 

Setting Eyes & Eyelid to Animation will disable eye movement and eyelid blinking. Setting Eyes & Eyelid to Tracking will re-enable the simulated eye movement and blinking.

Setting Mouth and Jaw to Animation will disable visemes, and viseme parameters will stop being updated. Setting Mouth and Jaw will re-enable visemes and they will start updating again.
:::
The Tracking setting will be kept until some other state runs this State Behavior again and resets it.

| Parameter | Description                                                                                         |
| :-- |:----------------------------------------------------------------------------------------------------|
| Tracking Control | See description above.                                                                              |
| Debug String | When this State Behavior runs, this string will be printed to the output log. Useful for debugging. |

## Avatar Parameter Driver

![image](/img/avatars/state-behaviors-fa19a1d-2022-06-02_18-11-06_Unity.png)

The Avatar Parameter Driver can manipulate Animator Parameters in a variety of ways. A single Avatar Parameter can perform multiple operations, and they are done in order from top to bottom. These operations are completed *once* upon entry to the State upon which the behavior resides.

`Local Only` will cause the driver to only operate locally, as a shortcut instead of detecting `isLocal`.

Clicking "Add" will add a new operation to the Driver. The first type (which is selected by default) is "Set".

If modifying a synced parameter (anything defined in the VRCExpressionParameters object) those values will be clamped to their maximum range. Int [0,255] Float [-1,1].

However, Parameters only defined in the Animation Controller (aka, "local parameters") can still be modified by a parameter driver. Those values aren't clamped.

You also cannot drive any of the [VRChat-defined Animator Parameters](/avatars/animator-parameters).

Set, Add, Random, and Copy work for `float` and `int`. Set, Random, and Copy work for `bool`.

### Set
Set will simply set the Value to the named Parameter in Destination.

![state-behaviors-121fe2a-2022-06-02_18-11-13_NVIDIA_Share.png](/img/avatars/state-behaviors-121fe2a-2022-06-02_18-11-13_NVIDIA_Share.png)

### Add
Add will add the Value to the named Parameter in Destination.

As the component points out, using Add may not produce the same result when run on a remote instance of the avatar. When using Add, it is suggested to use a synced Destination Parameter and only run the driver locally.

![state-behaviors-e10bb6a-2022-06-02_18-11-17_Unity.png](/img/avatars/state-behaviors-e10bb6a-2022-06-02_18-11-17_Unity.png)
        
### Random
Random will set the Destination Parameter to a random number between Min Value and Max Value.

As the component points out, using Random may not produce the same result when run on a remote instance of the avatar. When using Random, it is suggested to use a synced Destination Parameter and only run the driver locally.

![state-behaviors-99c6248-2022-06-02_18-11-23_Unity.png](/img/avatars/state-behaviors-99c6248-2022-06-02_18-11-23_Unity.png)

### Copy
Copy will set the value of the Source Parameter to the Destination Parameter. This can be used to set one float to match another float, to remap one float into a different range, or to convert between two different types entirely.
:::caution

VRChat's built-in parameters, such as `GestureLeftWeight`, **can** be specified but do not work as source parameters.
:::

![state-behaviors-bffdb10-2022-06-02_18-11-30_Unity.png](/img/avatars/state-behaviors-bffdb10-2022-06-02_18-11-30_Unity.png)

#### Converting between types
When converting from a `bool`, False counts as 0 and True counts as 1.

When converting to a `bool`, 0 is False and *anything* else is True.
When converting to an `int`, it will always round *down* to the nearest whole number.
When converting to a `float`, it will directly copy the value, even if it goes outside the range that it is capable of syncing to other players.

#### Custom Ranges
You can also use the `Convert Range` checkbox to enable some additional UI that allows you to set custom conversion ranges. This can be used to remap values or to have more control over exactly how it converts from one type to another type.

![state-behaviors-cab639b-2022-06-02_18-35-32_Unity.png](/img/avatars/state-behaviors-cab639b-2022-06-02_18-35-32_Unity.png)

## Playable Layer Control

![VRC Animator Layer Control component](/img/avatars/state-behaviours/animator-playable-layer-control.png)

The Playable Layer Control allows you to blend the weight of the entire Playable Layer to a specified value over specified duration. Very similar to Animator Layer Control, but instead controls the entire Playable Layer.

The Action Playable layer will use this State Behavior often, as the Action layer has weight zero by default, and should always be blended back to zero after the animation is complete.

If the state is exited mid-blend duration, the playable layer is immediately set to the goal weight.

| Property Name  | Purpose                                                                                                        |
| -------------- | -------------------------------------------------------------------------------------------------------------- |
| Layer          | The Playable Layer to affect.                                                                                   |
| Goal Weight    | The Playable layer weight to target after blending is complete.                                                 |
| Blend Duration | The amount of time to take to blend to the layer. Zero is instant.                                              |
| Debug String   | When this StateBehavior runs, this string will be printed to the output log. Useful for debugging.             |

## Animator Play Audio

![image](/img/avatars/state-behaviors-animatorplayaudio.png)

The "Animator Play Audio" behavior modifies an AudioSource when transitioning to the animation state. It can change the audio clip, pitch, or loop value, and it can play the AudioSource.

The relative path of the AudioSource (i.e. `Armature/Hips/Spine/`) must be entered into the "Source Path" property. This property can be filled out automatically by selecting an audio source component. If the audio source is on the root of the avatar, "Source Path" should be blank.


The "Playback order" property does not guarantee that audio clips are played in the same order for all players. For example, the "Random" setting may choose a different clip for each player. If you want to guarantee that all players hear the same clip, use multiple animation states or change "Playback Order" to "Parameter" in conjunction with a synced animator parameter. 

"Animator Play Audio" can be added to states inside a sub-state machine. However, adding it to the sub-state machine itself is not recommended. The state behavior will be applied for every state transition in the sub-state machine, which may cause unintended interactions with other state behaviors.

| Property Name | Purpose |
| ---- | ---- |
| Source Path | The path to the AudioSource relative to the avatar's root transform.<br/>If the AudioSource is not found when transitioning to this state for the first time, AnimatorPlayAudio will be disabled. |
| Playback Order | Determines which audio clip is chosen when transitioning to this state.<br/>• Random: Chooses a random clip. (Default)<br/>• Unique Random: Chooses a random audio clip, but not the same clip twice in a row.<br/>• Roundabout: Chooses the first clip, then the second, and so on. After the last clip, the first clip is chosen again.<br/>• Parameter: Chooses a clip based on an ["int" type avatar parameter](/avatars/animator-parameters#parameter-types). For example, if the parameter is 0, the first clip is chosen. The parameter must be defined in the avatar's Expressions Parameters. If the parameter cannot be found, the first clip is chosen instead. |
| Clips | A list of AudioClips that can be applied to the AudioSource when transitioning to this state. |
| Random Volume | The random volume that is applied to the AudioSource when transitioning to this state. Clamped from 0 to 1.<br/>• Min: The minimum random volume. (Default: 1)<br/>• Max: The maximum random volume. (Default: 1) |
| Random Pitch | The random pitch that is applied to the AudioSource when transitioning to this state. Clamped from -3 to 3.<br/>• Min: The minimum random pitch. (Default: 1)<br/>• Max: The maximum random pitch. (Default: 1) |
| Loop | Whether the AudioSource's looping should be set enabled or disabled when transitioning to this state. |
| On Enter | Whether to start or stop the AudioSource when transitioning into this state.<br/>🗹 Stop Audio Source (Default: Enabled)<br/>🗹 Play Audio Source (Default: Enabled) |
| On Exit | Whether to start or stop the AudioSource when transitioning out of this state.<br/>☐ Stop Audio Source (Default: Disabled)<br/>☐ Play Audio Source (Default: Disabled) |
| Play On Enter Delay In Seconds | The delay before "Play On Enter" plays a clip, if enabled. Clamped from 0 to 60 seconds. (Default: 0 seconds) |

Each audio setting has an additional property. By default, it's set to "Apply If Stopped". This prevents the setting from being applied if the audio source has not finished playing its previous clip and "Stop Audio Source On Enter" is disabled.

| Setting name     | On Enter |
|------------------|------------------------------------------------------------------------------------------- |
| Always Apply     | The setting is applied to the audio source, even if it is currently playing a clip.               |
| Apply If Stopped | The setting is applied to the audio source unless it's already playing a clip. (Default)   |
| Never Apply      | The setting is unused and greyed out in the inspector.                                     |


---

# ドキュメント終了

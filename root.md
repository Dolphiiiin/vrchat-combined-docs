# root Documentation

This document contains automatically collected documentation with metadata headers for each section.



---

## Document: getting-started.md

Path: /getting-started.md
---
sidebar_position: -99
---
import CurrentUnityVersion from '@site/src/components/UnityVersionedText.js';

# Welcome!

VRChat is a social platform where you can meet people, explore user-created worlds, and express yourself through custom avatars. Whether you're a seasoned developer or a newcomer, the VRChat SDK helps you bring your own ideas to life.

- [Get started with the VRChat SDK](/sdk)
- Design custom [avatars](/avatars)
- Build immersive [worlds](/worlds)
- Develop for multiple [platforms](platforms)
- Join the [Creator Economy](/economy)
- [Contribute](/contribute) to our documentation
- Explore our [creators roadmap](/roadmap)

## Quickstart

Follow the steps below to quickly set up the VRChat SDK:

<div class="video-container">
    <iframe src="https://www.youtube.com/embed/0u1g0TYoJsU" title="VRChat Creator Companion" frameborder="0" allow="encrypted-media; gyroscope; web-share" allowfullscreen></iframe>
</div>

1. Download & Install [the Creator Companion](https://vrchat.com/download/vcc).
2. If Unity is not installed, the Creator Companion will help you download Unity Hub, install Unity version [<CurrentUnityVersion/>](/sdk/upgrade/current-unity-version/) (VRChat SDK 3.4.2 or earlier is Unity 2019.4.31f1), and [create a Unity Account](https://id.unity.com/account/new).
3. Use the Creator Companion to create a new Worlds or Avatar project, and open it with Unity.
4. Build your [world](/worlds/creating-your-first-world) or [avatar](/avatars/creating-your-first-avatar) in Unity, and test it in VRChat using the SDK Control Panel.
5. Once ready, use the SDK control panel to publish your world or avatar to VRChat!

## World Creation

To make a VRChat world, you construct a scene in Unity using typical 3D models, materials and lighting. You can add interactivity with [Udon](/worlds/udon), our custom scripting system. Udon can be built with the visual [Udon Graph](/worlds/udon) or by writing C#-like code using [UdonSharp](https://udonsharp.docs.vrchat.com). You can use our [Networking](/worlds/udon/networking) system to synchronize experiences between players.

## Avatar Creation

To make a VRChat avatar, you must first create or find a 3D character, then ensure that it is [rigged](/avatars/rig-requirements) to work with VRChat. You can then [import your rigged model](/avatars/creating-your-first-avatar#step-3---get-the-model-into-your-project) into Unity and add [Expressions and Controls](/avatars/expression-menu-and-controls), [Avatar Dynamics](/avatars/avatar-dynamics) and much more. 


---

## Document: roadmap.md

Path: /roadmap.md
---
sidebar_position: 100
---
# Roadmap

This page gives you an overview of upcoming changes to the VRChat Worlds and Avatars SDK.

:::note Last updated: November 21, 2024

This page may be out of date. For the latest information, visit VRChat's [feedback board](https://feedback.vrchat.com/),  [forum](https://ask.vrchat.com/c/official/31), and [SDK release notes](/releases/).

:::

| Status          | Name                                                                                                                                                         |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Done            | [Save & Load persistent World Data](https://feedback.vrchat.com/udon/p/save-load-persistent-world-data)                                                      |
| Done            | [Example Central](https://feedback.vrchat.com/udon/p/example-central-for-worlds-sdk)                                                                         |
| Done            | [Permanent Purchases in Worlds](https://feedback.vrchat.com/creator-economy-sellers/p/one-time-purchases-in-worlds)[^4]                                      |
| Done            | [Expose some highly-requested types to Udon](https://feedback.vrchat.com/udon/p/expose-some-highly-requested-types-to-udon)                                  |
| Closed Beta[^1] | [New Character Controller for Worlds](https://feedback.vrchat.com/udon/p/new-character-controller-for-worlds)                                                |
| In progress[^2] | [Udon 2](https://feedback.vrchat.com/udon/p/udon-2)                                                                                                          |
| In progress[^2] | [Build & Publish on multiple platforms automatically](https://feedback.vrchat.com/sdk-bug-reports/p/build-publish-on-multiple-platforms-automatically)       |
| In progress[^2] | [Build & Test on mobile devices](https://feedback.vrchat.com/sdk-bug-reports/p/build-test-on-mobile-devices)                                                 |
| Planned[^3]     | [Recurring Payment in Group Stores](https://feedback.vrchat.com/creator-economy-sellers/p/recurring-payment-in-group-stores)[^4]                             |
| Planned[^3]     | [Allow VCC to install Android and iOS modules directly](https://feedback.vrchat.com/sdk-bug-reports/p/allow-vcc-to-install-android-and-ios-modules-directly) |

Please keep in mind that this list is subject to change. It also doesn’t include:
- minor planned changes,
- [Creator Companion](https://vcc.docs.vrchat.com/) changes,
- documentation changes,
- features that haven't been announced yet.

[^1]: "Planned" means that we haven't started major development on the feature, but we plan on working on it. The feature may require some design and scoping work.

[^2]: "In Progress" means that the feature is getting active development work! We are building the feature and performing internal testing and feedback cycles.

[^3]: "Closed Beta" means that this feature is getting tested with a selected group of users to collect feedback and find bugs.

[^4]: This URL leads to a private feedback board for creators participating in the Creator Economy.

[^5]: Use the `persistence-beta` branch on Steam!


---

# End of Documentation

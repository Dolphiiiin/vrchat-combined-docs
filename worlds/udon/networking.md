# networking 統合ドキュメント

以下は自動収集された全ドキュメントのコンテンツです。各セクションの始めにメタデータが記載されています。



---

## ドキュメント: debugging.md

```metadata
階層: /worlds/udon/networking/debugging.md
ディレクトリ: worlds\udon\networking
ファイル名: debugging.md
拡張子: .md
サイズ: 2.03 KB
最終更新: 2025-06-05T03:07:52.815Z
```

# Debugging Network Issues

The [World Debug Views](/worlds/udon/world-debug-views) are useful tools that can help you understand the ownership and network state of objects and data in your scene.

### Issue: Object Sync Not Working
#### Possible Causes:
- The object is missing a `VRCObjectSync` component.
- The object's ownership hasn't been transferred correctly.
- The object is not set to use Continuous syncing.

#### Fix:
- Ensure `VRCObjectSync` is added.
- Check ownership with:
  ```cs
  Debug.Log("Owner: " + Networking.GetOwner(gameObject).displayName);
  ```
- Use `RequestSerialization()` to send updates when using manual sync mode.

### Issue: Late Joiners See Incorrect State
#### Possible Causes:
- The object relies on events instead of synced variables.
- Missing serialization step for critical state updates.

#### Fix:
- Use synced variables for persistent state.
- Ensure data is serialized correctly using:
  ```cs
  public override void OnDeserialization()
  {
      Debug.Log("Synced data received: " + syncedVariable);
  }
  ```

### Issue: Ownership Conflicts
#### Possible Causes:
- Multiple scripts attempt to transfer ownership simultaneously.
- Unchecked ownership requests allow unexpected takeovers.

#### Fix:
- Verify ownership using:
  ```cs
  if (Networking.IsOwner(Networking.LocalPlayer, gameObject))
  {
      Debug.Log("I am the owner");
  }
  ```
- Use `OnOwnershipRequest()` to handle transfer conditions.

## Best Practices
- Use the debug overlays to check ownership and network quality.
- Minimize unnecessary ownership transfers to prevent desync.
- Test with multiple players to catch issues related to sync delays and ownership changes.
- Log key networking actions (`Debug.Log`) for analysis.

## Next Steps
For further networking troubleshooting, explore these topics:
- [Object Ownership & Transfers](/worlds/udon/networking/ownership)
- [Using Variables for Syncing](/worlds/udon/networking/variables)
- [Performance Considerations](/worlds/udon/networking/performance)

---

## ドキュメント: events.md

```metadata
階層: /worlds/udon/networking/events.md
ディレクトリ: worlds\udon\networking
ファイル名: events.md
拡張子: .md
サイズ: 19.34 KB
最終更新: 2025-06-05T03:07:52.816Z
```

# Network Events

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

Network events allow simple one-way network communication between your scripts. When a script executes a network event, it executes the event once for the target players currently in the instance.

Network events are not repeated for late joiners, so they're best for temporary actions that are only relevant for a short time, such as cosmetic effects. Don't use network events for important logic or state changes that are relevant for late joiners. Instead, use [network variables](/worlds/udon/networking/variables).

## Defining an Event

To declare an event, you have to give it a name. This has to be unique per Udon behaviour, but can be reused across different behaviours. The name is determined by the text field on the Udon Graph event node, or the UdonSharp method definition, and is case-sensitive. In UdonSharp, use `nameof` to allow your IDE to check this for you.

When [calling an event](#calling-an-event) using `SendCustomNetworkEvent`, the event name determines which method will be excecuted.

To allow a method or graph event node to be executed remotely, it must follow the rules listed below. For both Graph and UdonSharp, the *receiving* UdonBehaviour **must not** use [sync mode](/worlds/udon/udonsharp/attributes/#behavioursyncmode) `None`.

<Tabs groupId="udon-compiler-language">
<TabItem value="graph" label="Udon Graph">

For an Udon Graph custom event node to be network-callable, the following requirements have to be met:

- The name of the custom event **must not** start with an underscore `_`.
- The event node must have an active Flow connection.

</TabItem>

<TabItem value="cs" label="UdonSharp">

For an UdonSharp method to be network-callable, the following requirements have to be met:

* The method **must** use the attribute `[NetworkCallable]` (see [Legacy Events and Security](#legacy-events-and-security) for an exception).
* The method **must** use the access modifier `public`.
* The method **must not** use the modifier `static`, `virtual`, or `override`.
* The method **must not** use [**member overloading**](https://learn.microsoft.com/en-us/dotnet/standard/design-guidelines/member-overloading), including methods without a `[NetworkCallable]` attribute.
* The method **must not** have a return value, data can not be returned over the network this way.

In addition, if the method uses parameters, they must fulfill the following requirements:

* The method **must not** use more than 8 parameters.
* All parameters **must** be of a [supported type](/worlds/udon/networking/network-details#synced-variables).
* Parameters **must not** use the modifier `params`.
* Parameters **must not** have default values.

</TabItem>
</Tabs>

## Calling an Event

To trigger a network event, use the `SendCustomNetworkEvent` method available on `UdonBehaviour` and `UdonSharpBehaviour`, or make an explicit call via `NetworkCalling.SendCustomNetworkEvent`. These are identical and only provided for compatibility.

The `UdonBehaviour` you are targeting with the call can be different than the one executing `SendCustomNetworkEvent`. It is possible to target disabled behaviours.

### Example

Follow the example below to trigger a custom network event for players in your instance:

<Tabs groupId="udon-compiler-language">
<TabItem value="graph" label="Udon Graph">

![Udon graph for an object that logs a message when it is interacted with.](/img/worlds/udon/networking/custom-event-example-graph.png)

1. Ensure that your UdonBehaviour's sync mode is set to "Continuous" or "Manual", not "None".
2. Create an "Event Custom" node.
3. Give this node a unique name using its input box.
4. Add a "Send Custom Network Event Node."
5. In the `eventName` dropdown, select the name you assigned to the event. You can also attach a string flow do dynamically choose the event name, or enter a name from a different behaviour.
6. Leave the default `All` as the target to trigger this event on each Player in your room, or change it to a [different option](#event-targeting).
7. You can leave the `instance` input empty to target the current UdonBehaviour, or connect a reference to another UdonBehaviour to fire a Custom Event on that one instead.

</TabItem>

<TabItem value="cs" label="UdonSharp">

```cs
using UdonSharp;  
using VRC.SDK3.UdonNetworkCalling;  
using VRC.Udon.Common.Interfaces;  
  
public class SyncedHelloOnInteract : UdonSharpBehaviour  
{  
    public override void Interact()  
    {  
        SendCustomNetworkEvent(NetworkEventTarget.All, nameof(SayHello));  
    }  
      
    [NetworkCallable]  
    public void SayHello()  
    {  
        Debug.Log("Hello, World!");  
    }  
}
```

</TabItem>
</Tabs>

Note that your Udon code will not wait for these events to be called on the remote players - it simply sends them off and continues on immediately. If your event will be received by the sender itself however, it _will_ trigger locally before moving on, just like a regular function call would.

The order in which events are sent and received is guaranteed as long as you don't hit your own defined [rate-limit](#rate-limiting). If you send event `A` followed by event `B`, the receiving side will receive them in the order `A, B` as well. This guarantee is valid across all behaviours in your scene, but not across multiple players sending events at the same time.

However, if an event is [rate-limited](#rate-limiting) by the `[NetworkCallable(maxEventsPerSecond: X)]` attribute (not including VRChat-internal rate or throughput limits!), it will not prevent other queues from draining. For example, if event A has a limit of 3 per second, and you send five events at once: `A1, A2, A3, A4, B1` - then they will arrive in order `A1, A2, A3, B1, A4`, since A4 will hit the rate limit, and so event B1 can "skip the queue".

## Event Targeting

Network events always target one or more players in the instance. You can choose between the following targets:

| Target | Description |
| - | - |
| `NetworkEventTarget.All` | All players in the instance receive the event. |
| `NetworkEventTarget.Others`| All players in the instance receive the event, excluding the local player. |
| `NetworkEventTarget.Owner` | The owner of the object receives the event. |
| `NetworkEventTarget.Self` | "Loopback" target. Only the sending player receives the event. This will bypass all [rate-limiting](#rate-limiting), since it never actually gets sent over the network. |

If the local player sends a network event to themselves, they execute it immediately. For example, `NetworkEventTarget.All` sends a network event to all other players, but the local player executes the event immediately without waiting.

To send an event to a specific player, you can include the target player's [`playerId`](/worlds/udon/players/getting-players#get-playerid) as a parameter and only execute your event if the received ID matches the local player. Use `NetworkEventTarget.All` in that case, or add a special case for triggering the event locally.

## Sending Events with Parameters

A network event can carry up to **eight** parameters. Each parameter must be a [syncable variable type](/worlds/udon/networking/network-details#synced-variables) (`int`, `string`, `float`, `bool`, …), and in UdonSharp, the receiving method must be marked `[NetworkCallable]`.

<Tabs groupId="udon-compiler-language">
<TabItem value="graph" label="Udon Graph">

When creating your custom event in Udon Graph, you can select how many parameters it should have. The default variant has no parameters. You can always change the amount of parameters later using the dropdown on the node.

On a node with parameters, you can select the types in dropdowns on the left. The output data ports will automatically change type.

To send parameters from a "Send Custom Network Event" node, choose the overload with the correct amount of parameters from the dropdown.

For example, here is a simple graph receiving a string and an integer:

![Udon graph that sends an event with 2 parameters of different types on Interact.](/img/worlds/udon/networking/custom-event-parameters-example-graph.png)

</TabItem>

<TabItem value="cs" label="UdonSharp">

You can declare network callable functions with parameters just like usual in UdonSharp.

```cs
using UdonSharp;  
using UnityEngine;  
using VRC.SDK3.UdonNetworkCalling;  
using VRC.Udon.Common.Interfaces;  
  
public class EventParameterExample : UdonSharpBehaviour  
{  
    public override void Interact()  
    {  
        NetworkCalling.SendCustomNetworkEvent((IUdonEventReceiver)this, NetworkEventTarget.All, nameof(PrintMessage), "VRCat", 11);  
        // or: this.SendCustomNetworkEvent(NetworkEventTarget.All, nameof(PrintMessage), "VRCat", 11);  
    }  
      
    [NetworkCallable]  
    public void PrintMessage(string name, int age)  
    {  
        Debug.Log($"Congratulations on your {age}th birthday, {name}!");  
    }  
}
```

</TabItem>
</Tabs>

You need to ensure that the parameter types you pass to `SendCustomNetworkEvent` match the types you declared on the event, otherwise sending will fail!

When passing `null` as an input to `SendCustomNetworkEvent`, the method called on the receiving side will receive `default(T)` where `T` is the type of the parameter as declared in the method signature. This means nullable types will be received as `null`, while non-nullable types will receive a default value (e.g. `int` parameters sent as `null` will receive `0`, which is `default(int)`).

### Parameter Size Limits and Event Splitting

In general, keep parameter sizes minimal and avoid sending large objects or complex structures to avoid issues. There are a few hard-limits in place:

* The total size of all parameters in a single network event cannot exceed 16 KB.
* The size of a single parameter it not limited, other than by the total size of the event.
* Total outgoing data is hard-capped at approximately 18 KB/s. This includes _all_ network overhead however, so in practice it is unlikely you will see more than 8-10 KB/s.

In addition, there are two levels of limitation that may cause events to be delayed:

* Throughput-based limiting – This applies to the total amount of network data being processed at any time and follows the same rules as regular Udon sync.
* User-configured rate limiting – This controls how often network events can be sent per second and can be adjusted using the `[NetworkCallable(maxEventsPerSecond: X)]` attribute.

:::note

If you send an event containing more than **1024 bytes** (1 kilobyte) of parameter data, it will be split into multiple events internally. This process is _almost_ transparent to Udon, as the receiving size will re-assemble the event and call it only once on the targeted `UdonBehaviour`.

However, these internal events are visible in the [rate-limiting queue](#rate-limiting), and from the `Get(All)QueuedEvents` functions. For example, if you configure your rate-limit to be "2 events per second", but call `SendCustomNetworkEvent` with 2048 bytes of data, then the effective allowed rate will be only 1 event call per second. This is because at 2048 bytes, the single custom network event turns into 2 internal events.

:::

"Parameter Size" refers to the number of bytes your parameter data will be encoded as. This does **not** include internal headers or other overhead outside of your control. A few examples to demonstrate:

```cs
// fits into 1 internal event:
int x = 0; // = 4 bytes, sizeof(int)
Vector3 v = Vector3.zero; // = 12 bytes, sizeof(float) * 3
new string('x', 400); // = 400 bytes, UTF-8 encoded
"うどんは美味しい"; // = 24 bytes, UTF-8 encoded with non-ASCII characters
new char[128]; // = 256 bytes, UTF-16 (following C# spec)
new byte[1024]; // = 1024 bytes

// requires more than 1 internal event:
new byte[1025]; // = 1025 bytes, 2 events sent
new byte[16 * 1024] // = 16384 bytes, 16 events sent, maximum allowed size
new int[512]; // = 2048 bytes, sizeof(int) * 512, 2 events sent

// string[] and VRCUrl[] are special cases:
new string[2] { "test", "foobar" }; // = 18 bytes, 4 + 6 from UTF-8 encoded strings, 8 additional for a length value per array entry ( 2 * sizeof(int) )
```

## Rate Limiting

Network events are **rate-limited** to prevent excessive usage:
- Default rate: **5 events per second**
- Maximum configurable rate: **100 events per second**

To modify the rate limit, use the `[NetworkCallable(maxEventsPerSecond: X)]` attribute. `X` can be any integer between 1 and 100, inclusive range.

In Udon Graph, simply set the desired value in the corresponding input field on the node. Note that for events without parameters, you can set this to 0 to make it a [legacy event](#legacy-events-and-security).

This parameter specifies how quickly events can be sent. It is given in "Events per Second", e.g. a value of 5 means "maximum 5 events per second". **One call to `SendCustomNetworkEvent` may issue multiple events as far as the rate limiting is concerned, see [Event Splitting](#parameter-size-limits-and-event-splitting)!**

All rate-limiting is applied on a best-effort basis, it may not match the configured or specified values exactly based on local performance, network utilization, or server load.

:::warning

This limit exists so you can use it as a safety measure! It is strongly recommended to set this value **as low as you can** to mitigate malicious actors abusing your events to cause issues in your world.

:::

Note that this rate limit is enforced both on the sending client and server-side. In regular use, the server-side is only for protection against malicious users and generally not visible. The local client behaviour is that of queuing, meaning that if too many events are sent in short succession, they will be queued until the rate limit allows them to be sent. There is no limit on the amount of queued messages, so be careful not to send messages too quickly indefinitely as that will cause issues with _all_ Udon networking in your world.

Since events are executed immediately for the local player, there is no rate-limit applied in that case. This means that events may end up in a queue for remote players after already having executed locally.

Lastly, note that there is a global overall limit on outgoing events too, which is currently also approximately 100 events per second. This limit is dynamic and not configurable - it may change at any time, although VRChat will communicate any substantial decrease ahead of the change. This limit applies in the same way as the configurable one where events will be queued.

### Congestion Monitoring

The following functions in `NetworkCalling` are available to help work with rate limits:

| Function | Description |
|----------|-------------|
| `int NetworkCalling.GetQueuedEvents(udonBehaviour, eventName)` | Returns how many events are currently queued for sending. In normal operation, this number will be between 0 and your configured rate limit. If it exceeds your rate limit, you are sending events too quickly and they will be queued until the rate limit allows them to be drained. |
| `int NetworkCalling.GetAllQueuedEvents` | Returns how many events are queued across your entire world. A number above 0 does not automatically indicate bad network conditions, as events may end up queued for a short duration even under low load. |

You can also use the overall `Networking.IsClogged` property to determine network conditions. This will be affected by excessive event sending.

For example:

```cs
using TMPro;  
using UdonSharp;  
using UnityEngine;  
using VRC.SDK3.UdonNetworkCalling;  
using VRC.Udon.Common.Interfaces;  
  
public class EventQueueExample : UdonSharpBehaviour  
{  
    [SerializeField] private TextMeshProUGUI queueStatus;  
      
    void Update()  
    {  
        queueStatus.text = $"Queue: {NetworkCalling.GetAllQueuedEvents()}";  
        queueStatus.text += $"\nSpecific Event Queue: {NetworkCalling.GetQueuedEvents((IUdonEventReceiver)this, "SomeNetworkEvent")}";  
        queueStatus.text += $"\nClogged: {Networking.IsClogged}";  
    }  
      
    // some network events...  
}  
```

### Mismatched World Versions in the Same Instance

As a very special edge-case, there is one scenario where server-side rate limiting may actively drop events without malicious action. Since rate limits are applied based on every client's _local_ view of the world, if you upload a new version of your world with reduced rate limits and have users in the same instance spread across world versions, a sending client may exceed a receiving client's rate limit expectations. In this case, and _only_ then, the server may silently drop events and not deliver them.

## Accessing the Sender of an Event

The `NetworkCalling` class has some useful properties for working with events:

| Property                                    | Description                                                                                        |
|---------------------------------------------|----------------------------------------------------------------------------------------------------|
| `VRCPlayerApi NetworkCalling.CallingPlayer` | The `VRCPlayerApi` of the player who initiated this network call. `null` if not in a network call. |
| `bool NetworkCalling.InNetworkCall`         | Indicates if the current line is executed as part of a network call. Note that this is only reset once the entry function terminates, i.e. calling a secondary script or different function from your Event entrypoint will keep this state. |

## Legacy Events and Security

`[NetworkCallable]` was introduced in SDK 3.7.7 - Udon used to allow any public method to be called, unless it started with an underscore like `_MethodName`. For backwards compatibility, you can still call any parameter-less public method which doesn't start with an underscore, but it's not recommended. Adding the `[NetworkCallable]` attribute to a method with an underscore _will_ allow it to be called over the network.

In Udon Graph, a custom event node is considered "Legacy" if the `MaxEventsPerSecond` input field is set to 0.

:::warning

To prevent a public method or graph event from ever being called over the network, you should use a name starting with an underscore `_`. This can increase the security of your world or prefab.

:::

### Component Index Targeting

As an extra special note about legacy events, consider the semantics of sending an event to a `GameObject` vs a specific `Component` on an object.

Calling a function marked via `[NetworkCallable]` will make that call use `Component` targeting semantics. This means that even if you have 2 or more `UdonBehaviour`s on a GameObject, only the specified `UdonBehaviour` will receive the event.

The legacy case - meaning sending an event with no parameters to a function _not_ marked with `[NetworkCallable]` - will use `GameObject` semantics. That is, it operates similar to Unity's built-in [`GameObject.SendMessage`](https://docs.unity3d.com/2022.3/Documentation/ScriptReference/GameObject.SendMessage.html) and will call the function on _all_ `UdonBehaviours` on the object containing the behaviour you targeted.

Note that `Component` targeting relies on component indices (order), meaning it is not recommended to use `Destroy` with components on a `GameObject` that uses networked `UdonBehaviour`s. Doing so will result in undefined behaviour.


---

## ドキュメント: index.md

```metadata
階層: /worlds/udon/networking/index.md
ディレクトリ: worlds\udon\networking
ファイル名: index.md
拡張子: .md
サイズ: 7.21 KB
最終更新: 2025-06-05T03:07:52.816Z
```

# Networking

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

Multiplayer experiences are at the heart of VRChat. To create a world that reacts to players and synchronizes data between them, you need to understand networking in VRChat.

This guide introduces the core networking concepts, with a path to deeper learning.

<iframe class="embedly-embed" src="//cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fwww.youtube.com%2Fembed%2FMb6ZYBEhxiI%3Flist%3DPLe9XHNvXcouQjg5GULWGLj1tMzeythnQi&display_name=YouTube&url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DMb6ZYBEhxiI&image=https%3A%2F%2Fi.ytimg.com%2Fvi%2FMb6ZYBEhxiI%2Fhqdefault.jpg&key=f2aa6fc3595946d0afc3d76cbbd25dc3&type=text%2Fhtml&schema=youtube" width="854" height="480" scrolling="no" title="YouTube embed" frameborder="0" allow="autoplay; fullscreen" allowfullscreen="true"></iframe>

## Overview

VRChat networking is built around three key concepts:

- **Variables** – Variables store data that can be shared across all players in a world. These values can represent anything from numerical scores to object positions or player states. When a variable is synchronized, its updates are sent to all players in the instance.

- **Events** – Events are triggered actions that occur at specific moments in time. Unlike variables, which hold persistent values, events execute once and then disappear. They can be used to create interactions, trigger animations, or activate scripted behaviors.

- **Ownership** – Ownership determines which player can modify and update a networked object. By default, the first player to enter an instance owns all networked objects. Ownership can be transferred between players, allowing different users to control objects and sync their interactions with others.

Understanding these core concepts is essential for building networked experiences in VRChat. The following sections will explore how to use variables, events, and ownership effectively.

## Using Variables for Syncing

There are two types of sync available:
1. **Continuous** – Best for frequently updating values (e.g., a growing tree).
2. **Manual** – Best for crucial values that must always be correct (e.g., a scoreboard).

<Tabs groupId="udon-compiler-language">
<TabItem value="graph" label="Udon Graph">

![The Variables window in an Udon Graph shows the variables you've created, and lets you edit their properties.](/img/worlds/index-e057e35-slider-program-variables.png)

In the image above, we have three different variables, with the 'synced' box checked for the 'sliderValue' variable. The Owner of this GameObject will be in charge of this variable value, and their changes will be sent to everyone else.

</TabItem>
<TabItem value="cs" label="UdonSharp">

```cs
[SerializeField] private Slider slider;
[SerializeField] private Text sliderValueText;
[SerializeField, UdonSynced] private float sliderValue;
```

In the code above, we have three different variables, with the 'UdonSynced' attribute for the 'sliderValue' variable. The Owner of this GameObject will be in charge of this variable value, and their changes will be sent to everyone else.

</TabItem>
</Tabs>


### Example: Synced Slider
![](/img/worlds/udon-networking-8472b6b-synced-slider.png)

In this example, the Owner of a Slider syncs its value to everyone else. Note that this is meant to illustrate the concepts - we'll release a separate example that goes into the nitty-gritty 'how-to' details.

The slider's value is a number between 0 and 1, which we call a floating point value, or a float for short. So we make a variable to save and sync this value named *sliderValue*, with a type of **float**. 

We set up our slider to update *sliderValue* whenever the Owner moves it. This variable is then packed up and sent to everyone else, to update their own *sliderValue* variable to match. 

The process of packing and sending the variable is called **Serialization**, and **Deserialization** is when the data is received and unpacked.

When a new Player joins a instance, all of the synced variables in that world are sent to them so they can see what everyone else sees.

## Using Events for Syncing
Unlike variables, **events do not stick around**—they happen and then disappear. If we used an Event in the Synced Slider example above, any new Players who joined the instance would not have their Sliders synced up. So events are useful for things that you want to happen **right away** for everyone who's in the instance **right now**.

### Example: Bubble Gun
![](/img/worlds/udon-networking-33702b1-bubble-gun-shooting.png)

In this example, we have an object with a particle system and an animator that spins its bubble wand and generates bubble particles. We want this to happen for everyone in the world when the user holding the wand presses the trigger.

In our Udon Graph, we have a custom event we call "Trigger" which Plays the 'Spin' animation and triggers 22 Particles to Emit - this is just a local event in our graph.

To make this happen for everyone, we tie the **OnPickupUseDown** event which is triggered when someone presses Use while holding our Bubble Gun, and we use **SendCustomNetworkEvent** with a target of *All* to fire the "Trigger" event for everyone, including the Owner of the object.

<Tabs groupId="udon-compiler-language">
<TabItem value="graph" label="Udon Graph">

![Networked pickup particles in the Udon Graph](/img/worlds/udon-networking-e21b3b0-bubble-gun-graph.png)

</TabItem>
<TabItem value="cs" label="UdonSharp">

```cs
using UdonSharp;  
using UnityEngine;  
using VRC.Udon.Common.Interfaces;  
  
[UdonBehaviourSyncMode(BehaviourSyncMode.Manual)]  
public class BubbleGun : UdonSharpBehaviour  
{  
    [SerializeField] private Animator animator;  
    [SerializeField] private ParticleSystem particles;  
      
    public override void OnPickupUseDown()  
    {  
        SendCustomNetworkEvent(NetworkEventTarget.All, nameof(Trigger));  
    }  
  
    public void Trigger()  
    {  
        animator.Play("Spin");  
        particles.Emit(22);  
    }  
}
```

</TabItem>
</Tabs>

## Example Package & Next Steps
To see these concepts in action, download the example package:

[UdonNetworkingConcepts.unitypackage](https://assets.vrchat.com/sdk/UdonNetworkingConcepts.unitypackage)

After you've read the above and explored the example, continue below to dig deeper into VRChat Networking.

## More Networking Concepts

- **[Custom Network Events](/worlds/udon/networking/events)**
  - How to send and receive events over the network, including parameters.
- **[Network Variables](/worlds/udon/networking/variables)**
  - More info on how to best use variables to keep your players synced up.
- **[Late Joiners](/worlds/udon/networking/late-joiners)**
  - Learn how to quickly sync up anyone who joins your world with the rest of the players, instead of hanging up a "late joiners not supported" sign.
- **[Object Ownership](/worlds/udon/networking/ownership)**
  - Changing ownership of objects dynamically.
- **[Debugging Network Issues](/worlds/udon/networking/debugging)**
  - Using debug tools to track sync issues.

See the navigation bar for the full list of networking-related topics to learn about!

---

## ドキュメント: late-joiners.md

```metadata
階層: /worlds/udon/networking/late-joiners.md
ディレクトリ: worlds\udon\networking
ファイル名: late-joiners.md
拡張子: .md
サイズ: 2.06 KB
最終更新: 2025-06-05T03:07:52.817Z
```

# Late Joiners & Sync Issues

When a player joins a VRChat instance after some synchronized variables have been changed, they need to be caught up to the latest state of the world. This guide explains how to ensure late joiners experience a consistent and synchronized world.

## How VRChat Handles Late Joiners
When a new player joins an instance, VRChat does the following:
- Sends them the latest **synced variable** values, via the `OnDeserialization` event.
- Sets objects' owners to the proper Players. 
- Events are **not** replayed.

## Ensuring Late Joiners See the Correct State
### **1. Use Synced Variables Instead of Events for State**
Events trigger once and do not persist, meaning late joiners **miss any event-based changes**. Instead:

- Store important data in `UdonSynced` variables.
- Ensure owners call `RequestSerialization()` after modifying synced data for Manually-synced UdonBehaviours.

Example: Instead of sending an event to open a door, sync a variable:
```cs
[SerializeField, UdonSynced] private bool isDoorClosed;

public void OpenDoor()
{
    isDoorClosed = false;
    RequestSerialization();
}
```
Late joiners will receive the `isDoorClosed` state correctly when they join.

### **2. Catch up Objects for Late Joiners**

Apply changes when they join. You can accomplish this in the `OnDeserialization` event which triggers for them once they've loaded in:
```cs
private GameObject lockedDoor;

public override void OnDeserialization()
{
    // show or hide the door depending on its synced state
    lockedDoor.SetActive(isDoorClosed);
}
```

### **3. Combine Events and Synced Variables when Needed**
You can change a synced variable from an Event if needed. For example, to update the synced state of a door whenever anyone opens it, you can send an event to the object's owner:
```cs
public override void Interact()
{
    SendCustomNetworkEvent(NetworkEventTarget.Owner, nameof(OpenDoor));
}
```
This saves you from needing to change the Owner of the door to the player who Interacted with it just to set its variable.

---

## ドキュメント: network-components.md

```metadata
階層: /worlds/udon/networking/network-components.md
ディレクトリ: worlds\udon\networking
ファイル名: network-components.md
拡張子: .md
サイズ: 9.83 KB
最終更新: 2025-06-05T03:07:52.817Z
```

import UnityVersionedLink from '@site/src/components/UnityVersionedLink.js';

# Network Components

This doc covers Networking Components, Properties and Events you can use in your Udon Programs.

## Networking Properties

Special properties you can *get* from Networking:

| Property name    | Description |
| ---------------- | ----------- |
| LocalPlayer      | Returns the [VRC Player API](/worlds/udon/players) object of the local player. |
| IsInstanceOwner  | Returns `true` for the instance creator in Invite, Invite+, Friends, and Friends+ instances.<br />Always returns `false` in Group instances, Public instances, and the SDK's "Build & Test" mode. |
| InstanceOwner    | Returns the [VRC Player API](/worlds/udon/players) object of the player who owns the instance. If the owner is currently not in the instance, this returns `null` instead. If the owner returns, it returns the instance owner again.<br />The instance owner has special moderation permissions. Instance ownership never changes.
| IsMaster         | Returns `true` if the local player is the [instance master](/worlds/udon/networking/#the-instance-master). The master is the default owner for networked game objects.<br/>You should not use this for security or gating access to your world. Use `IsInstanceOwner` or implement a moderation system instead. |
| Master           | Returns the [VRC Player API](/worlds/udon/players) object of the player who is the current instance master. Is always valid. |
| IsNetworkSettled | Returns `true` if all the data in the instance has been deserialized, applied, and is ready for use. |
| IsClogged        | Returns `true` if there is too much data trying to get out. You can use this to wait until the network is unclogged or to adjust your logic. |
| SimulationTime   | Returns the current simulation time of a player or object with networking components. See below for more details. |

### Simulation time

Simulation time is a timestamp that refers to how far back in time an object is simulated. This value is used internally for [`VRCObjectSync`](/worlds/udon/networking/network-components#vrc-object-sync) and [players](/worlds/udon/players#simulationtime), but can be used in Udon scripts as well. For example, if your ` Time.realtimeSinceStartup ` is 45 and the SimulationTime of an object is 44.5, then VRChat believes 500ms of delay is necessary to smoothly replicate the object at that moment. You can use that number to learn some information about what `VRCObjectSync` is doing, or to create your own system similar to `VRCObjectSync`. For example, if you do `Time.realTimeSinceStartup - SimulationTime(player)` then that will tell you exactly how much latency that player has at that moment.
 
Simulation time is frequently adjusted depending on network conditions, including many factors such as latency, reliability, and frequency of the packets being received. The goal of this adjustment is to be as close to real-time as possible to reduce latency, but to leave enough room to prevent hitching. There are a variety of factors that can cause hitching, but one example can be running out of received packets from the owner.

## Networking Events

These are the events available as part of the Networking system to control how your data is synced.

### OnPreSerialization
This event triggers just before serialized data will be sent out, it's a good place to set synced variables that you want to be updated for other players.

### OnDeserialization
This event triggers when sync data has been transformed from bytes back into usable variables. It does not tell you *which* data has been updated, but serves as a jumping-off point to either update everything that watches synced variables, or a place to check new data against old data and make specific updates.

### OnDeserialization(DeserializationResult)
Same as OnDeserialization, but with additional information about the time at which the request was sent and received.

#### DeserializationResult
`DeserializationResult` contains three properties:
- `sendTime`: The time in seconds at which this message was sent.
- `receiveTime`: The time in seconds at which this message was received.
- `isFromStorage`: If true, then the included data was restored from storage rather than received from other realtime clients.

Both `sendTime` and `receiveTime` measure based on the time in seconds since VRChat has started, from your perspective (see <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/ScriptReference/Time-realtimeSinceStartup.html">Time.realtimeSinceStartup</UnityVersionedLink>). This means that if you want to know how many seconds ago a certain Deserialization was sent, you can calculate it with `Time.realtimeSinceStartup - sendTime`.

Note that every user's `Time.realtimeSinceStartup` is different, so one player's `sendTime` is going to be different from another player's `sendTime`. As a result, if you want to sync a specific `sendTime` to other players, you will need to calculate its offset by subtracting your `Time.realtimeSinceStartup`. Then, when the other players receive that offset, they can add back their own `Time.realtimeSinceStartup` to the offset in order to determine the absolute time relative to their own clock.

SendTime can be a negative number if the message was sent by someone else before you ever launched VRChat.

### OnPostSerialization
This event triggers just after an attempt was made to send serialized data. It returns a **SerializationResult** struct with a 'success' bool and 'byteCount' int with the number of bytes sent.

### OnSpawn
This event is deprecated - use the typical OnEnabled event if you want to do something when an object is 'Spawned' from the pool.

### OnOwnershipRequest
This event is triggered when someone has requested to take ownership. It includes the PlayerObjects for the Requester and the Requested Owner. To approve or deny the change, set a boolean value into a "Set Return Value" node. This logic runs locally on both the requester and the owner, so be aware that disagreements in logic between the two will cause a desync. This is most likely to be expressed by the ownership transfer being unexpectedly rejected by the owner.

### OnOwnershipTransferred
This event is triggered for everyone in the instance when an objects ownership is changed, and includes the PlayerObject for the new owner.

### OnMasterTransferred
This event is triggered for everyone in the instance when the instance master changes because the previous instance master has left the instance.
It includes one parameter, `newMaster,` which is the [VRC Player API](/worlds/udon/players) object of the player that has become master. This parameter is always valid.
For the first user joining a new instance, this event will trigger after `OnPlayerJoined` to indicate that the master state was transferred from "nobody".

### OnVariableChanged
This is a special type of event that you can create for any variable. In Udon Graph, you create it by dragging and dropping a variable into the graph while holding alt. This event detects when the variable changes, which can include when you receive synced variables from other players. 
* changing the contents of an array does not trigger a change, because the array itself is still the same.
* OnVariableChanged triggers immediately when the variable itself is written to, unlike OnDeserialization which triggers after it has finished writing all the synced variables. This means that if you use OnVariableChanged from one synced variable and try to get the contents of a different synced variable, it is not guaranteed that it has been updated with the latest synced data yet.
  
## VRC Object Sync
This component will automatically sync the Transform (position, rotation scale) and Rigidbody (physics) of the object you put it on. It has a few special methods and properties you can access:

### FlagDiscontinuity
Trigger this when you want to teleport the object - the changes you make this frame will be applied without smoothing.

### Set/Get Gravity
When gravity is on, this rigidbody is affected by gravity and will fall to the ground. Normally, gravity is a property of the rigidbody. However, when you have VRCObjectSync, this property must be controlled by the VRCObjectSync component instead. You can use these functions to do that. This effectively behaves like a synced variable, so **only the owner can set gravity.**

### Set/Get Kinematic
When kinematic is on, this rigidbody ignores forces, collisions and joints. Normally, kinematic is a property of the rigidbody.  However, when you have VRCObjectSync, this property must be controlled by the VRCObjectSync component instead. You can use these functions to do that. This effectively behaves like a synced variable, so **only the owner can set kinematic.**

### Respawn
Teleports this object back to its starting Position and Rotation, and removes its Velocities. 
Specifically, it sets **DiscontinuityHint** to true to make the following changes instant instead of smooth. Then it:
* sets transform.position to initial position
* sets transform.rotation to initial rotation

If the object has a rigidbody:
* sets the rigidbody.velocity to Vector3.zero
* sets the rigidbody.angularVelocity Vector3.zero
* sets the rigidbody.position to initial position
* sets the rigidbody.rotation to initial rotation
  
## VRC Object Pool
VRC Object Pool provides a lightweight method of managing an array of game objects. The pool will manage and synchronize the active state of each object it holds.

To make an object active, the Owner of the pool triggers the **TryToSpawn** node, which will return the object that was made active, or a null object if none are available. Objects may be returned to the pool by the pool's owner, and automatically disabled, via the **Return** node.

Late joiners will have the objects automatically made active or inactive where appropriate.


---

## ドキュメント: network-details.md

```metadata
階層: /worlds/udon/networking/network-details.md
ディレクトリ: worlds\udon\networking
ファイル名: network-details.md
拡張子: .md
サイズ: 5.72 KB
最終更新: 2025-06-05T03:07:52.817Z
```

# Networking Specs & Tricks

Networking in Udon can be challenging! Try to keep things simple until you're more experienced.

## Specs
### Bandwidth limits

:::note

Note: All specs subject to change. You can see some specific information about the data used per-object in [Debug Menu 6](/worlds/udon/world-debug-views/#debug-menu-6).

:::

- Udon scripts can send out about **11 kilobytes** per second.
- Udon scripts with manual sync are limited to roughly **280,496 bytes** per serialization.
- Udon scripts with continuous sync are limited to roughly **200 bytes** per serialization.

If a world exceeds limits, its networking will become clogged (see [IsClogged](/worlds/udon/networking/network-components/#networking-properties)). This has a different effect depending on the sync type of the UdonBehaviour:
* Continuous behaviours will fail to raise the network event and write errors in the logs.
* Manual behaviours will cache the event and try again. 
In both cases, the logic of the UdonBehaviour will continue to work, but the data will not be sent nor received.

Try designing your scripts in ways that reduce the amount of networking required. For example: If an object will move on a fixed or predictable path, then its position may not need to be synchronized. Instead, its initial location, velocity, and time of departure may be sufficient.

### Continuous synchronization

Continuous synchronization is intended for data that changes frequently and where intermediary values don't matter, like the position of an erratically moving transform. VRChat will perform intermediary value approximation to recover lost data, and will attempt to optimize network data for continuous synchronization.

Continuous sync is limited to roughly 200 bytes per serialization.

### Manual synchronization

Manual synchronization is good for variables that are updated frequently, but quickly. It is intended for data that changes infrequently and where intermediary values matter; like the positions of pieces on a chess board.

Each manually-synced object is rate limited as a factor of the data size. The more it sends, the more its send rate is limited. Scripts can call RequestSerialization as often as they want, but Udon will wait until enough time has passed before calling OnPreSerialization, sending the data, and calling OnPostSerialization with the result.

Manual sync is limited to **280,496 bytes** per serialization.

## Synced Variables
These variables are available for syncing across the network.

:::note
In the lists below, 'size' refers to the **approximate** size in memory. When networked, the data is serialized, which may lead to more data being transmitted. For example, syncing a `bool` will send **at least** 1 byte of data (instead of 1 bit) in addition to any networking overhead.
To find out how many bytes of serialized data were, use `byteCount` in the [`OnPostSerialization`](/worlds/udon/networking/network-components/#onpostserialization) event.
:::

### Boolean  types
| Type | Size    |
| ---- | ------- |
| bool | 1 byte  |
### Integral numeric types
| Type   | Range                           | Size    |
|--------|---------------------------------|---------|
| sbyte  | -128 to 127                     | 1 byte  |
| byte   | 0 to 255                        | 1 byte  |
| short  | -32,768 to 32,767               | 2 bytes |
| ushort | 0 to 65,535                     | 2 bytes |
| int    | -2,147,483,648 to 2,147,483,647 | 4 bytes |
| uint   | 0 to 4,294,967,295              | 4 bytes |
| long   | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 | 8 bytes |
| ulong  | 0 to 18,446,744,073,709,551,615 | 8 bytes |
### Floating-point numeric types
| Type   | Approximate range             | Precision     | Size    |
|--------|-------------------------------|---------------|---------|
| float  | ±1.5 x 10^(−45) to ±3.4 x 10^(38)   | ~6-9 digits   | 4 bytes |
| double | ±5.0 × 10^(−324) to ±1.7 × 10^(308) | ~15-17 digits | 8 bytes |
### Vector mathematics types and structures (Unity)
| Type        | Range         | Size     |
|-------------|---------------|----------|
| [Vector2](https://docs.unity3d.com/ScriptReference/Vector2.html)   | same as float | 8 bytes  |
| [Vector3](https://docs.unity3d.com/ScriptReference/Vector3.html)   | same as float | 12 bytes  |
| [Vector4](https://docs.unity3d.com/ScriptReference/Vector4.html)   | same as float | 16 bytes |
| [Quaternion](https://docs.unity3d.com/ScriptReference/Quaternion.html)| same as float | 16 bytes  |
### Color structures
| Type     | Range / Precision | Size    |
|----------|-------------------|---------|
| [Color](https://docs.unity3d.com/ScriptReference/Color.html)  | same as float     | 16 bytes |
| [Color32](https://docs.unity3d.com/ScriptReference/Color32.html)| same as byte      | 4 bytes |
### Text types and structures
| Type   | Range            | Size           |
|--------|------------------|----------------|
| char   | U+0000 to U+FFFF | 2 bytes        |
| string | same as char     | 2 bytes / char |
### Other structures
| Type   | Range            | Size           |
|--------|------------------|----------------|
| [VRCUrl](#vrcurl) | U+0000 to U+FFFF | 2 bytes / char |


---

If you have multiple UdonBehaviours on an object, the sync method will default to the most restrictive settings - a Manual UdonBehaviour and a Continuous one on the same object will both act as manual.

### Prioritization of visible objects

Udon's networking prioritizes synchronized game objects that are currently visible to the local user.

Udon *periodically* checks the visibility of all mesh renderer children of synchronized objects. This is used in the quality of service behaviour of Udon's network load balancing.

---

## ドキュメント: network-id-utility.md

```metadata
階層: /worlds/udon/networking/network-id-utility.md
ディレクトリ: worlds\udon\networking
ファイル名: network-id-utility.md
拡張子: .md
サイズ: 4.28 KB
最終更新: 2025-06-05T03:07:52.818Z
```

# Network ID Utility
A network ID is the identifier that is used to determine which object is which when it comes to networking. In most cases, you don’t need to worry about this, but it can come up when working with cross-platform worlds where players are technically loading two different versions of your world. 

Network IDs are the link between those different versions, to make sure that everybody is seeing the same thing and that the data is being transmitted to the correct objects.

To be more specific, a network ID is simply a number assigned to a GameObject. For example, let's assume you have a beach ball with the ID 1 and an ice cream cone with the ID 2. If these get mixed up, then you may try to kick around a beach ball while somebody else sees you kicking around an ice cream!

To deal with these potential issues and to make sure that your different scenes are in sync, we have created a network ID utility.

## Network ID Import and Export Utility

This utility allows you to save and transfer network IDs between scenes or projects. It can be found in the Unity Editor, under `VRChat SDK/Utilities/Network ID Import and Export Utility`. 
:::note

You should only need to use this utility if you are developing a cross-platform world and your different versions are in different scenes or projects.
:::

![network-id-utility-9936cee-image1.png](/img/worlds/network-id-utility-9936cee-image1.png)

When using this tool, you will see a list of all your network IDs in the entire scene. If you don’t have this yet, you can click Regenerate Scene IDs.
![network-id-utility-05130bf-image4.png](/img/worlds/network-id-utility-05130bf-image4.png)

When you are ready to transfer network IDs from one scene to another, click on the **Export** button to save the file somewhere. Then go to the other scene and click **Import**, and select that file.

**Network IDs in this format are saved as a path to the object.** As such, try to keep the path to each object the same between your scenes. Other objects in the scene that do not have any networking (such as meshes) do not matter and they can be different between your scenes, as long as they do not conflict with something that does need to be synced.
![network-id-utility-3b30a4e-image5.png](/img/worlds/network-id-utility-3b30a4e-image5.png)

If everything matches between your two scenes, you should see one big block with an **Accept All** button. Go ahead and click that, and you’re good to go!

## Resolving Conflicts

There are several conflict resolution tools within this utility.
![network-id-utility-22a9bcf-image3.png](/img/worlds/network-id-utility-22a9bcf-image3.png)

Here is an example of an object that exists in the file but does not exist in the scene. The file says that there is a network ID at this path, but it can’t find an object with that path. At this point, you can choose to either ignore it or specify a different object. If you know for sure that this is an object which doesn’t need to exist in this scene, then you can safely ignore it. However, if it is an object that should exist in your scene but simply has a different name, then you can select it. Once you’ve resolved this conflict, it will move down to the section where you can accept the network ID.
![network-id-utility-c5175f8-image2.png](/img/worlds/network-id-utility-c5175f8-image2.png)

Here's another example where an object says it has the network ID of 25, but the file says that a different path should have 25. 

This, and many other odd situations, can only happen if the scene has existing network IDs before you tried to import a new file on top. If you are copying IDs between scenes, then most likely you will want to clear IDs before importing so that you don’t get this issue. However, these options do exist in case you need to do something very specific like attempt to repair a scene without breaking some existing network IDs.

If you need to resolve these conflicts, you can choose to either click the Ignore All button which will not touch the scene at all, or you can hand-pick which one gets the ID. When you click the “Select” button that will resolve the conflict by applying the ID to the object that you have selected. This can resolve one or more conflicts, so don't be surprised if many conflicts disappear when you resolve just one.

---

## ドキュメント: network-stats.md

```metadata
階層: /worlds/udon/networking/network-stats.md
ディレクトリ: worlds\udon\networking
ファイル名: network-stats.md
拡張子: .md
サイズ: 2.31 KB
最終更新: 2025-06-05T03:07:52.818Z
```

# Network Stats
A number of networking stats are available to Udon via the `VRC.SDK3.Network.Stats` static class.

## Global Network Statistics

`ThroughputPercentage` - a running average of the amount of allowed output data throughput currently in use.

`RoundTripVariance` - the statistical variance in transport time to our servers and back again.

`RoundTripTime` - the current time it takes for our servers to respond to a message.

`BytesInMax` - the maximum bytes received in a second.

`BytesOutMax` - the maximum bytes sent in a second.

`BytesOutAverage` - the running average of bytes sent per second.

`BytesInAverage` - the running average of bytes received per second.

`HitchesPerNetworkTick` - a running average the recorded number of missing samples.

`Suffering` - a measure of the number of queued outbound messages.

`TimeInRoom` - the length of time spent in the current instance.

## Per Game Object and Per Player Statistics

All game object statistics will return the default value if the object is not synchronized over the network, or is not in any other way networked.

`UpdateInterval` - the running average of time between the sending of network messages for this object.

`ReceiveInterval` - the running average of time between the receipt of network messages for this object.

`FinalDelay` - the synchronization time adjustment for the object.

`Group` - all objects are grouped with nearby relevant objects for the purposes of network synchronization; this number represents the group the object is currently associated with.

`GroupDelay` - the running average of the synchronization adjustment for all objects in the object's associated group.

`Sleeping` - true if the object is at rest and not sending or receiving network messages.

`Size` - the size, in bytes, of the most recent network message associated with this object.

`BytesPerSecondAverage` - the running average of the network throughput for the object, in bytes.

`TotalBytes` - the total network data consumption for the object.

`ReliableEventsInOutboundQueue` - the number of manual synchronization and other reliable events currently enqueued to send for the object.

`LastSendTime` - the last time a message was sent on behalf of the object.

`LastReceiveTime` - the last time a message was received for the object.

---

## ドキュメント: ownership.md

```metadata
階層: /worlds/udon/networking/ownership.md
ディレクトリ: worlds\udon\networking
ファイル名: ownership.md
拡張子: .md
サイズ: 4.13 KB
最終更新: 2025-06-05T03:07:52.818Z
```

# Object Ownership

## Introduction
In VRChat, every networked GameObject has an owner. Ownership determines which player can modify and update a networked object. Understanding and managing ownership is crucial for properly synchronizing objects across all players in an instance.

## How Ownership Works
- The **first player** to join an instance becomes the owner of all networked objects by default.
- Ownership can be **transferred** from one player to another dynamically.
- Only the **owner** of a networked object can modify its synchronized Udon variables.
- If the owner leaves the instance, VRChat **automatically assigns** a new owner.

## Transferring Ownership
You can transfer ownership of an object using Udon:

```cs
Networking.SetOwner(VRCPlayerApi player, GameObject obj);
```

### Example: Changing Object Ownership
If you want a player to take ownership of an object when they interact with it:

```cs
public override void Interact()
{
    Networking.SetOwner(Networking.LocalPlayer, gameObject);
}
```

After this function runs, the local player will become the new owner of the object.

## Ownership Events
VRChat provides two key events related to ownership:

### OnOwnershipRequest

This event is called **before** ownership is transferred, allowing you to **approve or reject** the request.

```cs
public override bool OnOwnershipRequest(VRCPlayerApi requestingPlayer, VRCPlayerApi newOwner)
{
    return true; // Approve transfer
}
```

### OnOwnershipTransferred
Triggered when ownership of an object changes. You can use this to update UI elements or other behaviors when ownership shifts.

```cs
public override void OnOwnershipTransferred(VRCPlayerApi newOwner)
{
    Debug.Log($"New owner: {newOwner.displayName}");
}
```

## Transfer Events Diagram
This image shows the order of events so you can understand the steps involved in successfully transferring ownership of an object.

![](/img/worlds/udon-networking-813f99e-OnOwnershipRequest_Activity.svg)

## The Instance Master

The instance master is the player that owns any object that never had its ownership manually set or transferred. You can check if a player is the master via [`VRCPlayerApi.isMaster`](/worlds/udon/players/#get-ismaster).

Whether a player is the instance master should _not_ be used as a way to gate access to certain features of a world. For that, consider using "instance owner" instead.

The master player selection follows these rules:

- There will always be a valid master player in an instance.
- The first player to enter a previously empty instance will become the initial master.
- The master player changes when the current master leaves the instance.
	- The master player may also change if they're on Android and kept VRChat in the background for too long.
- When the current master leaves, a new master is chosen from the other players in the instance before `OnPlayerLeft` is called.
- You must not rely on any particular player becoming master. The new master player will be chosen based on various criteria on the server side (platform, network conditions, etc.).

These are the _only_ guarantees VRChat currently makes about network master behaviour. Any other observed behaviour is subject to change.

:::note Don't rely on master if you can avoid it!

It is recommended to use ownership checks for your networking logic instead of checking for instance master wherever possible.
There are scenarios where a master player might become unresponsive for a while, and events during that time might not be executed.
:::

## Best Practices
- If you want a player to be able to change a variable on an object, make sure to check or request ownership first.
- Ensure that ownership-related logic accounts for **player disconnects** and **late joiners**.
- Avoid relying on **Instance Master** if you can — use proper ownership handling instead.

## Next Steps
For further details, explore these networking topics:
- [Using Variables for Syncing](/worlds/udon/networking/variables)
- [Using Events for Syncing](/worlds/udon/networking/events)
- [Debugging Networked Objects](/worlds/udon/networking/debugging)



---

## ドキュメント: performance.md

```metadata
階層: /worlds/udon/networking/performance.md
ディレクトリ: worlds\udon\networking
ファイル名: performance.md
拡張子: .md
サイズ: 2.89 KB
最終更新: 2025-06-05T03:07:52.819Z
```

# Performance Considerations

## Introduction
Efficient networking is key to smooth multiplayer experiences in VRChat. Poorly optimized networked objects can lead to increased bandwidth usage, lag, and desynchronization issues. This guide provides best practices to ensure your networking logic is optimized.

## When to Use Variables vs. Events

### **Variables (Synced Data)**
Use synced variables for persistent states that need to be replicated across all players, such as:
- Player scores in a game.
- A door’s open/close state.
- Any data which needs to be available for [late joiners](/worlds/udon/networking/late-joiners).

#### **Best Practices:**
- Use **Continuous Sync** only when small updates are frequent (e.g., a progress bar).
- Use **Manual Sync** for data that must be accurate at all times (e.g., leaderboards).
- Avoid overusing synced variables, as excessive sync messages can cause bandwidth spikes.

### **Events (Temporary Actions)**
Use events for one-time actions that do not need to be persistent, and do not need to be received by late joiners such as:
- Playing an animation when a button is pressed.
- Firing a gun or triggering a sound effect.
- Spawning a temporary visual effect.

#### **Best Practices:**
- Use `SendCustomNetworkEvent(NetworkEventTarget.All, "EventName")` sparingly.
- Avoid using events for state persistence (e.g., setting a door to open state without synced variables).
- Consider local-only logic for effects that don’t need to be networked.

## Reducing Bandwidth Usage
### **Avoid Unnecessary Syncing**
- **Limit networked object updates**: Only send updates when values change significantly.
- **Optimize physics interactions**: Do not network rigidbodies unless necessary.
- **Use interpolation/extrapolation** to smooth movement instead of sending frequent position updates.

### **Optimizing Variable Syncing**
- **Group related variables** into arrays when possible.
- **Minimize float precision** (e.g., avoid syncing unnecessarily high-precision values).
- **Use binary flags** when syncing multiple boolean values.

## Best Practices for Ownership & Object Sync
- Only transfer ownership when required (e.g., for interactive objects like pickups).
- Avoid frequent ownership swaps, as they introduce latency and potential desync.

## Testing & Debugging Performance
- **Use the debug GUI (`--enable-debug-gui`)** to monitor bandwidth and sync behavior.
- **Test in real multiplayer conditions** to observe network behavior in different latency environments.
- **Profile network messages** to ensure minimal redundant data transmission.

## Next Steps
For further insights into optimizing networking, explore these related guides:
- [Debugging Networked Objects](/worlds/udon/networking/debugging)
- [Object Ownership & Transfers](/worlds/udon/networking/ownership)
- [Using Variables for Syncing](/worlds/udon/networking/variables)



---

## ドキュメント: variables.md

```metadata
階層: /worlds/udon/networking/variables.md
ディレクトリ: worlds\udon\networking
ファイル名: variables.md
拡張子: .md
サイズ: 3.64 KB
最終更新: 2025-06-05T03:07:52.819Z
```

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Network Variables

Network variables (also known as synced variables) allow Udon scripts to share data across all players in a VRChat instance. Unlike network events, which trigger once and do not persist, network variables ensure that all players—including late joiners—see the correct state of an object.

This guide covers how network variables work, when to use them, and best practices for keeping your networking efficient.

## How Synced Variables Work

<Tabs groupId="udon-compiler-language">
<TabItem value="graph" label="Udon Graph">

VRChat synchronizes variables if you tick the "synced" checkox.

![An Udon Graph screenshot showing the OnPlayerLeft event connected to an IsValid node.](/img/worlds/udon/networking/graph-synced-variable.png)

</TabItem>
<TabItem value="cs" label="UdonSharp">

VRChat synchronizes variables marked with the `[UdonSynced]` attribute.

```cs
[UdonSynced] private string score;
```

</TabItem>
</Tabs>

Synced variables behave differently than other variables:

- If your UdonBehaviour uses [manual sync](/worlds/udon/networking/network-details/#manual-synchronization), it must call `RequestSerialization()` to synchronize the variable from the owner to all players.
- If your UdonBehaviour uses [continuous sync](/worlds/udon/networking/network-details/#continuous-synchronization), variables will update for all players automatically.
- Your script must perform an [ownership transfer](/worlds/udon/networking/ownership) to allow another player to modify synced variables.
- [Late joiners](/worlds/udon/networking/late-joiners) receive the latest state of the variable, just like other users in the instance. This works regardless of sync type, you do not need to manually call `RequestSerialization` when a user joins.
- Not all types of variables can be synced. For example, you cannot sync references to scene objects. Supported types are listed [here](/worlds/udon/networking/network-details#synced-variables).

## Types of Variable Syncing

There are two types of syncing available:

### **1. Continuous Sync**
- Updates automatically when the owner changes the value.
- Best for frequently updated values (e.g., a progress bar, a player position tracker).
- Can apply interpolation to smooth changes between updates.
- Does not require calling `RequestSerialization()` - updates will be sent out regularly.

### **2. Manual Sync**
- Requires calling `RequestSerialization()` to send data updates.
- Best for crucial values that do not change often (e.g., a scoreboard, a game state variable).
- Helps reduce unnecessary network traffic.

### Setting the Sync Mode

![Showing how to set the Sync Type in the inspector](/img/worlds/udon/networking/set-sync-inspector.png)

You can use the Synchronization dropdown in the inspector for an UdonBehaviour to set its sync mode, as shown in the image above.

For UdonSharpBehaviours, you can alternatively use the [UdonBehaviourSyncMode](/worlds/udon/udonsharp/attributes/#udonbehavioursyncmode) attribute to control this from a script, as shown below.

```cs
[UdonBehaviourSyncMode(BehaviourSyncMode.Manual)]
public class Example : UdonSharpBehaviour 
{
  // This class's sync mode is manual.
}
```


### Example: Using Manual Sync
```cs
[SerializeField, UdonSynced] private bool isDoorOpen;

public void ToggleDoor()
{
    isDoorOpen = !isDoorOpen;
    RequestSerialization(); // Manually send update to all players
}
```

## Handling Late Joiners with Variables

Make sure to learn about how to sync variables for [late joiners](/worlds/udon/networking/late-joiners)!

---

# ドキュメント終了

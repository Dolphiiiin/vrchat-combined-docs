# systems Documentation

This document contains automatically collected documentation with metadata headers for each section.



---

## Document: architecture.md

Path: /worlds/clientsim/systems/architecture.md
# Architecture

The architecture of ClientSim has a focus on small components with an event based observer pattern, mixed with manual dependency injection where each class is initialized only with the dependencies it needs. The included player controller is based on generic dependency providers, which allows for the eventual extension to using VR without rewriting the core systems.

## Observer Pattern

ClientSim uses the observer pattern to send events within the system that anything can listen to without knowing what handles them. Events help decouple the different systems, improving testability as one system does not need to directly depend on another just to send messages when something happens. See [EventDispatcher](runtime/event-dispatcher.md) for specific details.

## Dependency Injection

ClientSim’s architecture uses a manually-handled dependency injection. On creation of a system, all dependencies are passed to it, either through its constructor or through an initialization method. Dependencies are structured as providers, and must extend an interface that declares what methods it provides. When a class needs a specific item, it depends on the provider interface instead of the class that implements it. This allows for different implementations of the provider without the dependent code needing to change. The provider pattern allows for dependencies to easily be mocked in tests.

---

## Document: index.md

Path: /worlds/clientsim/systems/index.md
# Systems

This documentation includes details on how the systems within ClientSim work if you'd like to understand them better, troubleshoot them or contribute to this Project! Choose a topic on the left to get started.

---

## Document: script-execution-order.md

Path: /worlds/clientsim/systems/script-execution-order.md
# Script Execution Order

| Execution Order | System Name          | Description                                                                                                                                                    |
|-----------------|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -Infinity       | UnityInputSystem     | Unity InputSystem updates before all MonoBehaviours. Input from user buttons are sent to ClientSimInput and events are dispatched.                             |
| -3000           | TrackingProvider     | Input is checked to update the TrackignProvider. For example: Desktop head X rotation.                                                                         |
| -3000           | PlayerController     | Update Player position before raycasting.                                                                                                                      |
| -2000           | PlayerRaycaster      | Update the position of the PlayerHands to TrackingProvider hand data. Raycast to find interactables in the world. This must happen before EventSystems update. |
| -1000           | Unity Event System   | Send mouse events to interact with UI. Order cannot be changed.                                                                                                |
| 0               | ClientSimBehaviours  |                                                                                                                                                                |
| 0               | UdonBehaviour        | Send Update Events to Udon Programs.                                                                                                                           |
| 1               | UdonInput            | This must happen after UdonBehaviour.Update to ensure proper event order.                                                                                      |
| 10000           | ClientSimBaseInput   | Update current frame tick for Input Events. Only needed to ensure tests and playmode act the same relating to when Input is processed.                         |
| 30000           | PlayerStationManager | Update the position of players on a station as late as possible so all other scripts have had time to evaluate first.                                          |
| 30001           | TooltipManager       | Update the position of Tooltip visuals after finalizing the player's position.                                                                                 |
| 31000           | PostLateUpdater      | VRChat's PostLateUpdate event sent to UdonBehaviours.                                                                                                          |

---

# End of Documentation

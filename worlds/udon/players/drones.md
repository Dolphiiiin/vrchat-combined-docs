# drones Documentation

This document contains automatically collected documentation with metadata headers for each section.



---

## Document: accessing-drones.md

Path: /worlds/udon/players/drones/accessing-drones.md
---
title: "Getting Drones"
slug: "getting-drones"
hidden: false
createdAt: "2025-03-05T14:52:00.02Z"
updatedAt: "2025-03-05T14:52:00.02Z"
---
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

## Methods

### VRCPlayerApi.GetDrone
Gets the Drone API that belongs to the target player.

**Output**
- `VRCDroneApi`


### VRCDroneApi.GetPlayer
Gets the owning player for the target drone.

**Output**
- `VRCPlayerApi`

## Events

### OnDroneTriggerEnter
This event is invoked when a Drone enters a trigger.

**Output**
- `VRCDroneApi`


### OnDroneTriggerExit
This event is invoked when a Drone exits or is despawned inside of a trigger.

**Output**
- `VRCDroneApi`


### OnDroneTriggerStay
This event is invoked when a Drone stays inside of a trigger.

**Output**
- `VRCDroneAPi`


---

## Document: drone-information.md

Path: /worlds/udon/players/drones/drone-information.md
---
title: "Drone Information"
slug: "drone-information"
hidden: false
createdAt: "2025-03-05T14:52:00.02Z"
updatedAt: "2025-03-05T14:52:00.02Z"
---
## Drone API

### GetPlayer

Gets the owner of the Drone.

**Output**
- `VRCPlayerApi`: The owning player object.  

### IsDeployed

Get whether the Drone is deployed in the world.

**Ouput**
- `bool`: Whether the drone is deployed.

### GetPosition

Gets the position of the Drone.

**Output**
- `Vector3`: The drone's position in world space.  

### TryGetPosition

Try to get the position of the Drone.

**Out**
- `Vector3 position`: The drone's position in world space.

**Output**
- `bool`: indicates whether it was successful

### GetRotation

Gets the rotation of the Drone.

**Output**
- `Quaternion`: The drone's rotation in world space.  

### TryGetRotation

Try to get the rotation of the Drone.

**Out**
- `Quaternion rotation`: The rotation of the Drone in world space.

**Output**
- `bool`: indicates whether it was successful

### GetVelocity

Get the speed and direction of the drone's movement.

**Output**
- `Vector3 velocity`: The drone's velocity in world space.  

### TryGetVelocity

Try to get the speed and direction of the drone's movement.

**Out**
- `Vector3 velocity`: The drone's velocity in world space.

**Output**
- `bool`: indicates whether it was successful

### TeleportTo

Set the world position and rotation of the Drone.

**Input**
- `Vector3`: The drone's position in world space.  
- `Quaternion`: The drone's rotation in world space.  
- `bool` (optional): If set to false or left unset, remote users' drones will snap to the target values. If set to true, remote users' drones will smoothly lerp to the target values.

### SetVelocity

Set the speed and direction of the drone's movement.

**Input**
- `Vector3`: The drone's velocity in world space.  

---

## Document: index.md

Path: /worlds/udon/players/drones/index.md
---
sidebar_position: 1
---
# Drone API

You can use Udon to retrieve information about the player drones in your world instance.

Udon interacts with Drones through the `VRCDroneApi`. You can retrieve the `VRCDroneApi` object from each Player that is using it.

Your world can detect a Drone interacting with a Trigger Collider and execute the following events:

- [OnDroneTriggerEnter](/worlds/udon/players/drones/getting-drones#ondronetriggerenter)
- [OnDroneTriggerExit](/worlds/udon/players/drones/getting-drones#ondronetriggerexit)
- [OnDroneTriggerStay](/worlds/udon/players/drones/getting-drones#ondronetriggerstay)

See these pages for more details on working with Drones via Udon:
* [Getting Drones](/worlds/udon/players/drones/getting-drones)
* [Drone Information](/worlds/udon/players/drones/drone-information)

---

# End of Documentation

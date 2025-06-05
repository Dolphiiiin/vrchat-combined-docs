# sdk 統合ドキュメント

以下は自動収集された全ドキュメントのコンテンツです。各セクションの始めにメタデータが記載されています。



---

## ドキュメント: getting-started.md

```metadata
階層: /economy/sdk/getting-started.md
ディレクトリ: economy\sdk
ファイル名: getting-started.md
拡張子: .md
サイズ: 3.33 KB
最終更新: 2025-06-05T03:07:52.745Z
```

---
description: "Add your first Udon product to your world."
title: Getting Started
sidebar_position: 0
sidebar_custom_props:
    customIcon: 💡
---

import SellerNotification from '/docs/economy/_sellers-notification.mdx';

<SellerNotification/>

This page explains how to get started using the Creator Economy in the VRChat Worlds SDK. Before you begin, you'll need to have created a [VRChat World in Unity](/worlds/creating-your-first-world).

You must also have created an Udon product in your Store. Please visit the [Udon product page](/economy/products/udon) if you haven't done this yet.

If you've done everything above, you're ready to edit your Unity project.

## UdonProducts Manager

The **UdonProducts Manager** is an editor window that shows you all Udon Products you've created on VRChat.com. If you haven't created any yet, this window will be blank.

You can add Udon products to your project automatically with the UdonProducts Manager.

![A screenshot of the UdonProducts manager.](/img/economy/sdk/udonproducts-manager.png)

To open the UdonProducts Manager window, open the **VRChat SDK** tab at the top of the Unity Window, then click **UdonProducts Manager**.

The manager lets you:
- Fetch the thumbnail image of the product. This creates a texture you can use in your Unity project.
- Ping the product asset in the **Project** window, making the product easy to find.
- Purchase and expire the product for debugging purposes if you have ClientSim installed.

The manager lists all products in two lists: "Used in the scene" and "Used in the project". The former will update its list whenever you save your current scene.

Any error or other information will appear at the bottom of the window.

:::note 
Fetched textures inside your project do not automatically update if you update your Udon product's image on VRChat.com. To update the fetched texture, delete and fetch it again in your Unity project.
:::

### Creating an Udon product manually

You can also create and manage your Udon Products manually.

1. Right-click in your Assets window or click "Assets" > "Create" at the top of your Unity window.
2. Click "VRChat" > "UdonProduct" to create an UdonProduct asset in your Assets folder.
3. (Optional) Choose a file name.
4. Select the UdonProduct asset and view it in Unity's inspector.

![A blank UdonProduct after creation](/img/economy/sdk/udonproduct-blank.png)

5. Copy the ID of your Udon Product from VRChat.com, and paste it in the "ID" field of your UdonProduct asset.
6. (Optional) Click "Fetch product details" to see the name and description of your product in Unity.
7. (Optional) Click "Fetch product image" to create a texture based on your product's current thumbnail image.

## Adding UdonProducts to your world

After creating UdonProduct assets, you can use them in Udon scripts.

The VRChat SDK has [examples and prefabs that can be used with your Udon product](/economy/sdk/examples/) by dragging a prefab into your scene and choosing an UdonProduct asset. The prefabs are beginner-friendly and don't require any Udon or coding knowledge.

If you're experienced with Udon and the VRChat Worlds SDK, you can create your own scripts and use them with Udon products.

To learn how, read our full documentation on the [Creator Economy's SDK features](/economy/sdk/udon-documentation).


---

## ドキュメント: index.md

```metadata
階層: /economy/sdk/index.md
ディレクトリ: economy\sdk
ファイル名: index.md
拡張子: .md
サイズ: 837 B
最終更新: 2025-06-05T03:07:52.745Z
```

---
title: Creator Economy Worlds SDK
sidebar_position: 5
sidebar_custom_props:
    description: Use the Creator Economy in your world.
    customIcon: 🛠
---

import SellerNotification from '/docs/economy/_sellers-notification.mdx';

<SellerNotification/>

The VRChat Worlds SDK allows your worlds to use [Udon](/worlds/udon/) to interact with the Creator Economy.

import DocCardList from '@theme/DocCardList';

<DocCardList />

- Get started by [creating your first Udon Product](/economy/sdk/getting-started),
- Check out our official [example prefabs and scenes](/economy/sdk/examples/) and use them in your own world,
- Try out your own Udon products by [testing them](/economy/sdk/testing),
- Or write your own custom scripts by reading [Creator Economy Udon documentation](/economy/sdk/udon-documentation).

---

## ドキュメント: testing.md

```metadata
階層: /economy/sdk/testing.md
ディレクトリ: economy\sdk
ファイル名: testing.md
拡張子: .md
サイズ: 1.33 KB
最終更新: 2025-06-05T03:07:52.745Z
```

---
description: "Test your Udon products before publishing your world."
sidebar_position: 3
sidebar_custom_props:
    customIcon: 🪲
---

# Testing Udon Products

import SellerNotification from '/docs/economy/_sellers-notification.mdx';

<SellerNotification/>

When you add Creator Economy features to your VRChat World, you should test it before publishing.

## Testing in ClientSim

While developing your world in Unity, you can use ClientSim in conjunction with the [UdonProducts Manager](/economy/sdk/getting-started#udonproducts-manager). It allows you to activate and expire products without launching VRChat.

## Testing in VRChat

You can test Creator Economy SDK features with the "Build & Test" option.

:::warning

Some features of the Creator Economy cannot be tested locally and require a private world upload instead.

:::

- When testing locally or logged into VRChat as the world/group owner, all listings/subscription are free. Your VRChat wallet balance does not decrease.
- When testing locally, purchased listings automatically expire after 60 seconds, no matter how many months you choose when purchasing it.
- You can purchase published listings/subscriptions without publishing them in your store. This allows you to make test purchases without allowing users to see or buy your listings/subscriptions. 

---

## ドキュメント: udon-documentation.md

```metadata
階層: /economy/sdk/udon-documentation.md
ディレクトリ: economy\sdk
ファイル名: udon-documentation.md
拡張子: .md
サイズ: 12.21 KB
最終更新: 2025-06-05T03:07:52.746Z
```

---
description: "All Udon types, methods, and events related to the Creator Economy."
sidebar_position: 4
sidebar_custom_props:
    customIcon: 🛠
---

import UnityVersionedLink from '@site/src/components/UnityVersionedLink.js';

# Udon Documentation

import SellerNotification from '/docs/economy/_sellers-notification.mdx';

<SellerNotification/>

This page documents all Udon types, methods, and events related to the VRChat Creator Economy. You can use them to create your own [Udon scripts](/worlds/udon) with the [Udon Node Graph](/worlds/udon/graph) or [UdonSharp](https://udonsharp.docs.vrchat.com).

## Types

VRChat's SDK contains objects types to support the management of Udon products that your customers can purchase.

### UdonProduct
UdonProduct is a <UnityVersionedLink versionKey="minor" url="https://docs.unity3d.com/<VERSION>/Documentation/Manual/class-ScriptableObject.html">ScriptableObject</UnityVersionedLink> that you can create in your project. It represents a product from your store, allowing you to interact with it in Udon.
A world will only receive events based on products that are being used within it, therefore it is necessary to reference a product's UdonProduct equivalent in any UdonBehaviour at least once before uploading.

For example, even if you don't directly use the UdonProducts anywhere, you would need to have an array of UdonProducts on at least one UdonBehaviour somewhere in the scene to receive OnPurchaseConfirmed, OnPurchaseExpired or OnPurchasesLoaded events for those products across all UdonBehaviours.

UdonProducts can be created with the UdonProductManager ("VRChat SDK" → "UdonProduct Manager") or by creating an UdonProduct asset manually ("Assets" → "Create" → "VRChat" → "UdonProduct").

Read [Getting Started](/economy/sdk/getting-started) to learn more about creating an UdonProduct asset in your project.

![A blank UdonProduct after creation](/img/economy/sdk/udonproduct-blank.png)

#### Fields

| Field name | Type | Description |
| - | - | - |
| ID | string | Unique identifier of your Udon product. Enter it by copying it from your store on the VRChat website. |
| Name | string | The name of your Udon product. | 
| Description | string | The description of your Udon product. | 

#### Equals

**Description**
This method compares if the product ID is equal to another product's ID.

**Input**
- `IProduct` or `UdonProduct`: Product to compare this product to.

**Output**
- `bool`:  `true` if the two products are equal, otherwise `false`.

### IProduct
This is the equivalent of a UdonProduct returned by all the Udon Creator Economy methods and events.

| Field name | Type | Description |
| - | - | - |
| ID | string | Unique identifier of your Udon product. |
| Name | string | The name of your Udon product. | 
| Description | string | The description of your Udon product. | 
| Buyer | VRCPlayerAPI | The player who purchased this product. |

#### Equals

**Description**
This method compares if the product ID is equal to another product's ID.

**Input**
- `IProduct` or `UdonProduct`: Product to compare this product to.

**Output**
- `bool`:  `true` if the two products are equal, otherwise `false`.

:::note

UdonProduct's and IProduct's "Name" & "Description" fields are currently filled in from the UdonProducts present in the world.

:::

## Methods

The SDK contains methods for interacting with player purchases or VRChat's store pages. These methods can be found under the 'Store' namespace.

### Store.DoesPlayerOwnProduct
This method will check if a player owns a certain product.

**Input**
- `VRCPlayerApi`: Player that you want to check the product ownership of.
- `UdonProduct` or `IProduct`: Product that you want to check the ownership of.

**Output**
- `bool`: `true` if the player owns the product, otherwise `false`.

:::caution Race condition

It is not advised to use this immediately after the "Start" event. Udon may not have received players purchases yet. It is advised to use the [OnPurchasesLoaded](#onpurchasesloaded) event instead.
:::

### Store.DoesAnyPlayerOwnProduct
This method will check if any player in the instance owns a certain product.

**Input**
- `UdonProduct` or `IProduct`: Product that you want to check the ownership of.

**Output**
- `bool`: `true` if any player in the instance owns the product, otherwise `false`.

### Store.GetPlayersWhoOwnProduct
This method will get all the players who own a certain product.

**Input**
- `UdonProduct` or `IProduct`: Product that you want to check the ownership of.

**Output**
- `VRCPlayerApi[]`: An array of players that own this product.

### Store.OpenWorldStorePage
Opens a the world store page of the current world, if it has one.

### Store.OpenGroupPage
Opens a group's **Group Info** page in VRChat's main menu.

**Input**
- `string`: ID of a group (i.e. `grp_00000000-0000-0000-0000-000000000000`)
  - To find the group ID, open the group on VRChat.com and copy the ID from your browser's address bar.

### Store.OpenGroupStorePage
Opens a group's **Store** page in VRChat's main menu.

**Input**
- `string`: ID of a group (i.e. `grp_00000000-0000-0000-0000-000000000000`)
  - To find the group ID, open the group on VRChat.com and copy the ID from your browser's address bar.

### Store.OpenListing
Opens the purchase screen of a [listing](/economy/listings) in VRChat's main menu.

You can open any of your activated listings, even if you didn't add it to your group [store](/economy/store) or world [store](/economy/store).

**Input**
- `string`: ID of the listing (i.e. `prod_00000000-0000-0000-0000-000000000000`)

### Store.SendProductEvent
Sends the [OnProductEvent](#onproductevent) event to all players in the instance on the target UdonBehaviour.
Before sending or receiving the networked event, this method checks if the player using `SendProductEvent` has purchased the product.

**Input**
- `UdonBehaviour`: The Udon Behaviour that will receive the resulting [OnProductEvent](#onproductevent) event.
- `UdonProduct` or `IProduct`: Product that you want to use for the event.

### Store.ListPurchases
Sends an [OnListPurchases](#onlistpurchases) event to the target UdonBehaviour with an array of all the purchases made by a target player.

**Input**
- `UdonBehaviour`: Udon Behaviour that will receive the resulting [OnListPurchases](#onlistpurchases) event.
- `VRCPlayerApi`: Player that you want to check the purchased products from.

### Store.ListAvailableProducts
Sends an [OnListAvailableProducts](#onlistavailableproducts) event to the target UdonBehaviour with an array containing all  products used in the world.

**Input**
- `UdonBehaviour`: An UdonBehaviour that will receive the resulting [OnListAvailableProducts](#onlistavailableproducts) event.

### Store.ListProductOwners
Sends an [OnListProductOwners](#onlistproductowners) event to the target UdonBehaviour. This event allows you to retrieve the names of all your supporters and, for example, display their user names in your world.

- For this event to work properly, you'll first need to enable the ["Owners Names in Udon" setting](/economy/products/udon#editing-udon-products) for the Udon product on [VRChat.com](https://vrchat.com/home/marketplace/storefront/products). Otherwise, [OnListProductOwners](#onlistproductowners) will not fire.
- If you are locally testing your world, OnListProductOwners will load the placeholder user names "VRCat, Fred, VRRat" instead of real user names.
- If your GameObject contains multiple UdonBehaviour components, this event may not work properly.

**Input**
- `UdonBehaviour`: An UdonBehaviour that will receive the resulting [OnListProductOwners](#onlistproductowners) event.
- `UdonProduct`: The UdonProduct for which to retrieve the owner's user names.

## Events

:::info Don't disable your script

If a game object or its Udon behaviour is disabled, it won't execute most of the events related to the Creator Economy.

:::

### OnPurchaseConfirmed
This event is triggered once a player's purchase has been loaded and confirmed. Purchases are loaded in the following situations: 
- when joining the instance, both for the local player and any other players,
- when any new players join the instance, and
- when any player in the instance purchases one of the world's products.

**Output**
- `IProduct`: The product that has been purchased.
- `VRCPlayerApi`: The player who has purchased the product.
- `bool`: `true` if the purchase was just made, `false` if it was made as part of loading the player's purchases upon joining the world.

### OnPurchaseConfirmedMultiple
This event is triggered every time [`OnPurchaseConfirmed`](#onpurchaseconfirmed) is triggered. However, this variant also supports [Quantity Purchases](/economy/listings/#quantitypurchases). If you use [instant](/economy/listings#instant) listings in your world, you should always use this event instead of `OnPurchaseConfirmed`.
The `quantity` value represents the amount that the user purchased. For [instant](/economy/listings#instant) listings with [Quantity Purchases](/economy/listings#quantity-purchases), `quantity` ranges from `1` to `99`. For all other listing types, `quantity` is always `1`.

**Output**
- `IProduct`: The product that has been purchased.
- `VRCPlayerApi`: The player who has purchased the product.
- `bool`: `true` if the purchase was just made, `false` if it was made as part of loading the player's purchases upon joining the world.
- `int`: The quantity purchased at once.

:::caution

Use either `OnPurchaseConfirmed` _or_ `OnPurchaseConfirmedMultiple`. Don't use both in the same script!

Both events detect every purchase. If you use both, you may accidentally detect the same purchase twice. Generally, you should use `OnPurchaseConfirmedMultiple` because it's compatible with [Quantity Purchases](/economy/listings#quantity-purchases).

:::

### OnPurchaseExpired
This event is triggered when the local client detects that one of the products owned by a player in the instance has expired.

**Output**
- `IProduct`: Product that has expired.
- `VRCPlayerApi`: The player whose product has expired.

### OnPurchasesLoaded
This event is triggered when all of a player's purchases have been loaded, either when the local player joins an instance or when another player has joined later.
If the player does not own any products, the event will still fire, and the IProduct[] array will be empty.

**Outputs**
- `IProduct[]`: An array of products owned by the player.
- `VRCPlayerApi`: The player whose purchases have been loaded.

### OnProductEvent
This event is triggered when a player uses [Store.SendProductEvent](#storesendproductevent). Both the local player and VRChat's servers will check that the local player has purchased the product before executing this event.

**Output**
- `IProduct`: The product that has been 'sent' alongside the event.
- `VRCPlayerApi`: The player who has used their product.

### OnListPurchases
This event is triggered when the local player uses [Store.ListPurchases](#storelistpurchases). It returns an array of all the purchases made by the target player.

**Outputs**
- `IProduct[]`: An array of products owned by the target player.
- `VRCPlayerApi`: The target player whose purchases have been retrieved.

### OnListAvailableProducts
This event is triggered when the local player uses [Store.ListAvailableProducts](#storelistavailableproducts). It returns an array of all the products (UdonProduct) used in the world.

**Outputs**
- `IProduct[]`: An array of IProducts representing all UdonProducts used in the world.

### OnListProductOwners
This event is triggered after an Udon script calls [Store.ListProductOwners](#storelistproductowners). It returns an array containing the display names of all players who own the target product. The list includes **every** user who owns that product, not just users in the current instance.

To check if a player in the instance owns an Udon product, [Store.DoesPlayerOwnProduct](#storedoesplayerownproduct) should be used instead.

**Outputs**
- `IProduct`: The product passed in the initial [Store.ListProductOwners](#storelistproductowners) call.
- `string[]`: An array of strings containing the names of all users who own this product.


---

# ドキュメント終了

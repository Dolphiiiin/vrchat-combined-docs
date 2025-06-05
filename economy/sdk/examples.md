# examples 統合ドキュメント

以下は自動収集された全ドキュメントのコンテンツです。各セクションの始めにメタデータが記載されています。



---

## ドキュメント: debug-logging.md

```metadata
階層: /economy/sdk/examples/debug-logging.md
ディレクトリ: economy\sdk\examples
ファイル名: debug-logging.md
拡張子: .md
サイズ: 2.81 KB
最終更新: 2025-06-05T03:07:52.742Z
```

---
description: "A screen for listing, debugging, and logging products in your world."
sidebar_custom_props:
    customIcon: 🪲
---

# Store Debug Logging

import SellerNotification from '/docs/economy/_sellers-notification.mdx';

<SellerNotification/>

Store Debug Logging is a prefab that helps log store-related events on an in-world console or screen. You can set this up in your worlds to give supporters a way to check what purchases they've made, what purchases they can make, and to open your store.

import HowToImportExample from '/docs/economy/_ce-how-to-import.mdx';

<HowToImportExample/>

##### Prefabs Included
* **StoreDebugLoggingPrefab**: A prefab that contains a store-related console window, which supporters can interact with.

:::caution
If viewing the example scene, you'll also need the [Open Group Page](/economy/sdk/examples/open-group-page) prefab. Otherwise, your project will be missing what it needs for a complete scene.
:::

### How to Use

For this (and most!) prefabs, you'll first need an UdonProduct to check for and a way for players to purchase this product. 

Once you've created a purchasable product:

1. Drag the **StoreDebugLoggingPrefab** into your scene.

![EventPrefabtoScene](/img/economy/examples/DebugLogging_AddToScene.png "Dragging the prefab into scene.")

2. In the inspector, locate the **Products** drop down and drag a product over. You can also drag multiple products at once. The order in the inspector is not important, but make sure you only list each product once. 

<div class="video-container">
    <iframe src="https://assets.vrchat.com/videos/docs/DebugLogging_AddProduct.mp4" title="Overhead Indicator" frameborder="0" allow="encrypted-media; gyroscope; web-share" allowfullscreen></iframe>
</div>

3. Fill the **Group Id** variable with your group ID. 
   -   Find your group ID by opening your group [on the website](https://vrchat.com/home/groups) and copying the ID in the address bar of your browser. For example: `grp_a4f791af-a167-4c91-b849-2e37e37f509a`. Any short code (i.e. `EXAMPL.9920`) will **not** work.

![DebugGroupID](/img/economy/examples/DebugLogging_GroupId.png "Adding your Group ID.")

4. The **Debug Text** object, **Scrollbar**, and **Auto Scroll Toggle** are text and UI elements that should be left as is.

5. Run Build & Test!

### Inspector Parameters

* `Products` - The Udon products a user can purchase, with a `Size` value for the number of products and `Element` values for each product link.
* `Debug Text` - The text that appears when a player clicks one of the buttons on the console.
* `Group Id` - The ID for your group, which contains your store and purchasable products.
* `Scrollbar` - The scrollbar that shows up in the console.
* `Auto Scroll Toggle` - The toggle that lets players turn auto scrolling on or off.


---

## ドキュメント: floating-overhead.md

```metadata
階層: /economy/sdk/examples/floating-overhead.md
ディレクトリ: economy\sdk\examples
ファイル名: floating-overhead.md
拡張子: .md
サイズ: 3.93 KB
最終更新: 2025-06-05T03:07:52.742Z
```

---
description: "A floating indicator above players who own an Udon product."
sidebar_custom_props:
    customIcon: 👑
---

# Floating Overhead Buy Indicator

import SellerNotification from '/docs/economy/_sellers-notification.mdx';

<SellerNotification/>

1. Open [the Example Central Window](https://vrc-beta-docs.netlify.app/sdk/example-central) from the window from the Unity Editor Menu under "VRChat SDK > 🏠 Example Central"
2. If you haven't yet specifically made examples visible yet:
	1. Press the ⚙️ gear icon in the Example Central window to open Example Central settings.
    2. Enable "Show Creator Economy Examples".
3. Find this prefab in the list or search for it by title (same as the title of this page).
4. Press the "Import" button to import the Unitypackage into your project.

Floating Overhead Buy Indicator is a prefab that spawns an indicator over a player's head once they have purchased something. Customize it in various ways to highlight your supporters.

![FloatingPrefab](/img/economy/examples/BuyIndicator-FloatingPrefab.png "Shows what a Floating Obj looks like over a players head.")

import HowToImportExample from '/docs/economy/_ce-how-to-import.mdx';

<HowToImportExample/>

##### Prefabs Included
* **FloatingOverheadBuyIndicatorPrefab**: A script that adds the `FloatingObjectPrefab` above players who own the `ExampleProduct`.

* **FloatingObjectPrefab**: What's spawned above the player when they own the product. You can customize this or replace it with another GameObject to be anything you'd like.

:::caution
If viewing the example scene, you'll also need the [Open Group Page](/economy/sdk/examples/open-group-page) prefab. Otherwise, your project will be missing what it needs for a complete scene.
:::

### How to Use

For this (and most!) prefabs, you'll first need an UdonProduct to check for and a way for players to purchase this product. 

Once you've created a purchasable product:

1. Drag the **FloatingOverheadBuyIndicatorPrefab** into your scene. It's invisible by default as there's no image or model to display yet.

![FloatingPrefabtoScene](/img/economy/examples/BuyIndicator-FloatingPrefabtoScene.png "Dragging the prefab into scene.")

2. In the Inspector, locate the **Product** variable. Click on the circle button and replace the example file with your own product.

![ProductAdd](/img/economy/examples/BuyIndicator-ProductAdd.png "Dragging the prefab into scene.")

3. Next, locate **Overhead Indicator Prefab**. This is what will spawn over the players head when they purchase this product. Think of it like the green diamond from the Sims! You can replace this with whatever GameObject you'd like, but just make sure it has no colliders.

![ObjAdd](/img/economy/examples/BuyIndicator-ObjAdd.png "Adding a custom GameObject.")

4. Use **Height Above Head** to customize how far above the player's head you want the indicator to float.

![HeightChange](/img/economy/examples/BuyIndicator-HeightChange.png "Adjusting height.")

5. If you'd like the player to be able to look up and see their own indicator, leave **Show Indicator Above Local Player** enabled. Disable it if otherwise.

<div class="video-container">
    <iframe src="https://assets.vrchat.com/videos/docs/BuyIndicator-ShowIndicatorAboveLocalPlayer.mp4" title="Overhead Indicator" frameborder="0" allow="encrypted-media; gyroscope; web-share" allowfullscreen></iframe>
</div>

6. Run Build & Test!

### Inspector Parameters

* `UdonProduct` - The Udon Product that when owned adds a floating object above a player.
* `Overhead Indicator Prefab `- The indicator GameObject that you want to float above players head. Make sure this object doesn't have any colliders or you'll likely run into issues.
* `HeightAboveHead` - How far above the player's head you want the indicator to float.
* `Show Indicator Above Local Player` - Whether or not you should see an indicator above yourself if you own the product.


---

## ドキュメント: index.md

```metadata
階層: /economy/sdk/examples/index.md
ディレクトリ: economy\sdk\examples
ファイル名: index.md
拡張子: .md
サイズ: 1.37 KB
最終更新: 2025-06-05T03:07:52.743Z
```

---
sidebar_position: 5
sidebar_custom_props:
    description: Add prefabs to your world that are easy to customize.
    customIcon: 📚
---

# Example Prefabs

import SellerNotification from '/docs/economy/_sellers-notification.mdx';

<SellerNotification/>

Example Prefabs allow world creators to use the Creator Economy quickly and easily without writing their own Udon scripts. The prefabs can be tweaked and customized to create a better experience for your supporters.

Many of the examples require knowledge on how to set up Udon products, which should be done before you proceed here. Head to our [Getting Started with the Creator Economy SDK page](/economy/sdk/getting-started/) to learn more.

import DocCardList from '@theme/DocCardList';

<DocCardList />

## Adding Prefabs

Each prefab shares the same starting steps:

1. Download the Unity package for the prefab you need, which can be found in the left sidebar.

2. Import the package into your Unity project.

![DragAndDrop](/img/economy/examples/Importing-Package_Drag-and-Drop.png "Drag your prefab into Unity.")

3. Drag the prefab into your scene.

![ImportPrefabtoUnity](/img/economy/examples/Importing-Package_Place-In-Scene.png "Drag your prefab into the scene.")

And you're ready to customize! Browse our different prefabs in the left-hand sidebar to see what they do and how to customize them.


---

## ドキュメント: open-group-page.md

```metadata
階層: /economy/sdk/examples/open-group-page.md
ディレクトリ: economy\sdk\examples
ファイル名: open-group-page.md
拡張子: .md
サイズ: 2.43 KB
最終更新: 2025-06-05T03:07:52.743Z
```

---
description: "Opens your group or group store."
sidebar_position: 1
sidebar_custom_props:
    customIcon: 🔗
---

# Open Group Page

OpenGroupPage is a prefab that allows you to open your group page or your group store. Use it to create buttons that make it easy for users to access your group.

import HowToImportExample from '/docs/economy/_ce-how-to-import.mdx';

<HowToImportExample/>

## Prefabs included
* **OpenGroupPagePrefab**: Includes a button that opens a group page. Also includes a text description.
* **OpenGroupPageSimplePrefab**: Includes a button that opens a group page. Does not include a text description.

![OpenGroupPrefab](/img/economy/examples/Comparison-OpenGroupPage.png "Compares group prefabs.")

## How to Use

For each prefab, you'll need to replace any ID with the ID of your own group.

1. Select the chosen prefab in your Unity scene.
2. Set the ID of the group that owns the product in the `Group ID` field in the inspector window.
    -   Find your group ID by opening your group [on the website](https://vrchat.com/home/groups) and copying the ID in the address bar of your browser. For example: `grp_a4f791af-a167-4c91-b849-2e37e37f509a`. Any short code (i.e. `EXAMPL.9920`) will **not** work.

![DragGroupID](/img/economy/examples/Group-Id-Copying.png "Where to put the group ID.")

3. Toggle the **OpenToStorePage** toggle on the prefab if you want to open to your store page directly. If unchecked, the button will open to your group page instead of directly to your store.
4. For **OpenListing/OpenListingSimple:** Set the ID of the listing in the `Listing ID` field in the inspector window.
    - Find your listing ID by [opening the listing section](https://vrchat.com/b/monetization/home/marketplace/storefront/listings) of your store and copying its ID. 

![DragListingID](/img/economy/examples/Listing-Id-Copying.png "Where to put the listing ID.")

5. Toggle the **OpenToStorePage** toggle on the prefab if you want to open to your store page directly. If unchecked, the button will open to your group page instead of directly to your store.

![IDPasting](/img/economy/examples/Group-versus-Store-links.png "Instructions on finding and pasting IDs.")

6. Run Build & Test!

## Inspector Parameters

#### OpenGroupPage 
* `Group ID` - The group ID of the group you want to open.
* `Open To Store Page` - If true, will open the store page for the group instead of the group info page.


---

## ドキュメント: open-listing.md

```metadata
階層: /economy/sdk/examples/open-listing.md
ディレクトリ: economy\sdk\examples
ファイル名: open-listing.md
拡張子: .md
サイズ: 3.36 KB
最終更新: 2025-06-05T03:07:52.743Z
```

---
description: "Opens a listing."
sidebar_position: 0
sidebar_custom_props:
    customIcon: 🔗
---

# Open Listing

This example contains two prefabs that allow users to open one of your listings by pressing a button.

import HowToImportExample from '/docs/economy/_ce-how-to-import.mdx';

<HowToImportExample/>

## Prefabs included

* **OpenListingPrefab**: Includes a button to open to a listing page. Also includes a text description.
* **OpenListingSimplePrefab**: Includes a button to open to a listing page. Does not include a text description.
* **OpenListingAndReactToPurchasePrefab**: Includes a button to open to a listing page. Plays a sound effect when a given product is purchased.

![OpenListingPrefab](/img/economy/examples/Comparison-OpenListing.png "Compares listing prefabs.")

* **OpenListingDeluxePrefab**: Includes a thumbnail, and fields for the name, type and price of the listing.

![OpenListingPrefab](/img/economy/examples/OpenListingDeluxe_GameView.png "Shows OpenListing Deluxe as it appears in the Game View")

## How to Use

For each prefab, you'll need to replace any ID with the ID of your own group or product.

1. Select the chosen prefab in your Unity scene.

2. Set the ID of the listing in the `Listing ID` field in the inspector window.
    - Find your listing ID by [opening the listing section](https://vrchat.com/home/marketplace/storefront/listings) of your store and copying its ID. 

![DragListingID](/img/economy/examples/Listing-Id-Copying.png "Where to put the listing ID.")

3. For **OpenListingAndReactToPurchase** only:
    - Use the [UdonProducts Manager](https://creators.vrchat.com/economy/sdk/getting-started#udonproducts-manager) to locate the `UdonProduct` asset of the product that should play a sound effect when purchased.
    - Set the `Trigger Product` field to the `UdonProduct` asset.

4. For **OpenListingDeluxe** only:
    - Set the Thumbnail for the listing using the `Thumbnail` field in the inspector window. It's best if you use the same thumbnail that the user will see when opening your listing, but you have the ability to use any square Sprite in your project.
    - Set the three text lines using the `Display Name`, `Type` and `Price` fields. Just like the thumbnail, it's best if these match what the user will see in your listing, but you can enter any information you like.

![OpenListingDeluxeFields](/img/economy/examples/OpenListingDeluxe_Inspector.png "The Fields for OpenListingDeluxe")

5. Run Build & Test!

### Inspector Parameters

The prefabs have the following parameters:

### OpenListing & OpenListingSimple
* `Listing ID` - The listing ID of the listing you want to open.

### OpenListingAndReactToPurchase
* `Trigger Product` - The `UdonProduct` asset that triggers the sound effect when purchased.
* `Optional Purchase Sfx` - The sound effect that plays when purchasing the product.
* `Optional Purchase Sfx Source` - The `AudioSource` component that plays the sound effect.

### OpenListingDeluxe
* `Thumbnail` - The sprite shown in the prefab, typically the same as the thumbnail set for the listing.
* `Display Name` - The first line of text, typically the Display Name of the listing.
* `Type` - The second line of text, typically the Type of the listing - Consumable, Instant, etc.
* `Price` - The third line of text, typically the Price of the listing in VRChat Credits.


---

## ドキュメント: open-world-store.md

```metadata
階層: /economy/sdk/examples/open-world-store.md
ディレクトリ: economy\sdk\examples
ファイル名: open-world-store.md
拡張子: .md
サイズ: 3.18 KB
最終更新: 2025-06-05T03:07:52.743Z
```

---
description: "Opens your world store."
sidebar_position: 1
sidebar_custom_props:
    customIcon: 🏪
---

# Open World Store

The OpenWorldStore prefab offers visitors a convenient way to access your world store. Customize its appearance to attract visitors and highlight your store.

You can visit the [Open World Store Example World](https://vrchat.com/home/world/wrld_fef974b9-fe44-4731-8bd1-7629e80eccc9) to see how it works first-hand. Notice that there is a real product available in the store - don't buy it!

import HowToImportExample from '/docs/economy/_ce-how-to-import.mdx';

<HowToImportExample/>

## Prefabs included
* **OpenWorldStorePrefab**: A vending machine that opens a world store when the user interacts with it. Includes a customizable graphic and interaction text.

![OpenWorldStorePrefab](/img/economy/examples/OpenWorldStore_Prefab.png "The OpenWorldStorePrefab model in the demo scene")

## How to Use

1. Create a [world store](/economy/store/world-store), publish the store, and add at least one published listing.
2. Set the Interaction Text you want users to see when approaching the prefab.

![OpenWorldStoreInteractionText](/img/economy/examples/OpenWorldStore_InteractionText.png)

3. Set the Door Graphic you want displayed on the front of your vending machine by selecting a texture in the "Door Graphic" picker.

![OpenWorldStoreDoorGraphic](/img/economy/examples/OpenWorldStore_DoorGraphic.png)

4. Build & Publish your world.
  - "Build & Test" does _not_ allow you to open world stores. You must "Build & Publish" to test your world store in VRChat.
  - If you're adding a world store to an existing world, you should "Build & Publish" to a new world ID. This allows you to test the prefab before publishing it to your existing world.

## Creating a New Door Graphic

You can create a graphic using Photoshop or any other image editor.

### Using Photoshop

The example includes a Photoshop file for easy editing at the path `Assets/Examples/OpenWorldStore/Prop_VendingMachine/Textures/glass_Screen_TEMPLATE.psd`.

![OpenWorldStorePhotoshopTemplate](/img/economy/examples/OpenWorldStore_PhotoshopTemplate.png)

1. Replace the contents of the "REPLACE ME!" layer with a graphic that represents your listing(s).
2. Update the text in the "FeaturedText" layer with your own words.
3. Edit any of the other layers as you see fit, keeping the overall size and masking of the graphic the same.
4. Export your new version as a 24-bit PNG with transparency - you can save it directly into the Assets folder of your project.

### Using Another Image Editor

The example includes a PNG for easy editing at the path `Assets/Examples/OpenWorldStore/Prop_VendingMachine/Textures/glass_Screen_UV_Map`.

1. Open "glass_Screen_UV_Map.png" in an image editor.
2. Edit the white area of the template. You can add whatever graphics you want.
3. Save your file as a 24-bit PNG with alpha transparency.
4. Move the file into your project.
5. Select the texture in the Project window, and set its Alpha Source to "Input Texture Alpha" in the inspector.
6. Drag and drop the file from your project window to the "Door Graphic" input slot in the OpenWorldStorePrefab's inspector.

---

## ドキュメント: product-owners-instance.md

```metadata
階層: /economy/sdk/examples/product-owners-instance.md
ディレクトリ: economy\sdk\examples
ファイル名: product-owners-instance.md
拡張子: .md
サイズ: 2.25 KB
最終更新: 2025-06-05T03:07:52.744Z
```

---
description: "Lists all product owners in the current instance."
sidebar_custom_props:
    customIcon: 👥
---

# Product Owners In Instance

import SellerNotification from '/docs/economy/_sellers-notification.mdx';

<SellerNotification/>

Product Owners In Instance is a prefab that lists the product owners or non-product owners who are currently in the instance via a text console.

import HowToImportExample from '/docs/economy/_ce-how-to-import.mdx';

<HowToImportExample/>

##### Prefabs Included
* **ProductOwnersInInstancePrefab**: A prefab with a console that shows all the product owners in the instance.
* **NonProductOwnersInInstancePrefab**: A prefab with a console that shows all the non-owners in the instance.

:::caution
If viewing the example scene, you'll also need the [Open Group Page](/economy/sdk/examples/open-group-page) prefab. Otherwise, your project will be missing what it needs for a complete scene.
:::

### How to Use

For this (and most!) prefabs, you'll first need an UdonProduct to check for and a way for players to buy this product. 

Once you've created a purchasable product:

1. Drag the **ProductOwnersInInstance** prefab into your scene.

![EventPrefabtoScene](/img/economy/examples/SubsInInstance_AddToScene.png "Dragging the prefab into scene.")

2. In the inspector, locate the **Udon Product** variable. Click on the circle button and replace the example file with your own product.

![ProductAdd](/img/economy/examples/SubsInInstance_SelectProduct.png "Adding a product via the inspector.")

3. If you'd like the console to show non-owners instead, you can check **List Non Product Owners Instead,** or use the **NonProductOwnersInInstance** prefab. Otherwise, leave this unchecked.

![NonOwnersvsOwners](/img/economy/examples/SubsInInstance_SubsVersusNonSubs.png "Difference between non product owners and product owners list.")

4. Run Build & Test!

### Inspector Parameters

* `Udon Product` - The Udon Product which will be checked for ownership.
* `Product Owners Text` - The textfield that will be updated with the list of product owners. Each product owner will be on a new line.
* `List Non Product Owners Instead` - If true, all the players who don't own the product will be listed instead.


---

## ドキュメント: product-owners-only.md

```metadata
階層: /economy/sdk/examples/product-owners-only.md
ディレクトリ: economy\sdk\examples
ファイル名: product-owners-only.md
拡張子: .md
サイズ: 4.74 KB
最終更新: 2025-06-05T03:07:52.744Z
```

---
description: "An area that only Udon product owners can enter."
sidebar_custom_props:
    customIcon: 🔐
---

# Product Owners Only Area

import SellerNotification from '/docs/economy/_sellers-notification.mdx';

<SellerNotification/>

Product Owners Only Area is a prefab that sets up an area that is only accessible to owners of a target product. Use this to create exclusive areas, events, and experiences in your world.

import HowToImportExample from '/docs/economy/_ce-how-to-import.mdx';

<HowToImportExample/>

##### Prefabs Included
* **ProductOwnersOnlyAreaExamplePrefab**: A prefab that contains a full example of how to use the ProductOwnersOnlyArea script.

:::caution
If viewing the example scene, you'll also need the [Open Group Page](/economy/sdk/examples/open-group-page) and [Udon Product Toggle](/economy/sdk/examples/product-toggle) prefabs. Otherwise, your project will be missing what it needs for a complete example scene.
:::

### How to Use

For this (and most!) prefabs, you'll first need an UdonProduct to check for and a way for players to purchase this product. 

Once you've created a purchasable product:

1. Open the **ProductOwnersOnlyAreaExampleScene** from your Project window. The scene contains a building asset to help you test and understand how the prefab works.

![PrefabScene](/img/economy/examples/SubsOnlyArea_DragIntoScene.png "Opening the example scene.")

2. In the Hierarchy, click on the **ProductOwnersOnlyAreaExamplePrefab**. 

![HierarchyClick](/img/economy/examples/SubsOnlyArea_SelectInHierarchy.png "Finding the prefab in the hierarchy.")

3. In the Inspector, add which Udon Product you would like to check for when a player enters or is inside of a certain area.

![AddProduct](/img/economy/examples/SubsOnlyArea_SelectProduct.png "Adding a product to check for.")

5. In the Hierarchy, select **OpenListingSimplePrefab**. Paste your listing ID here. This will show a link to purchase the product where necessary.
    -   To get your listing ID, go to [vrchat.com/home](https://vrchat.com/home), then Marketplace -> Storefront -> My Listings.

![AddListingID](/img/economy/examples/SubsOnlyArea_OpenListingPrefab.png "Adding a listing ID.")

6. In the Hierarchy, click on the **ProductOwnersOnlyAreaExamplePrefab** again. You'll see a few different settings:

    -    **Trespassing Message** is what appears if a player enters/is inside a product owner-only area and does not own the correct Udon Product.

    - **Trespassing Teleport Location** is where the player is sent if they try and enter or have ended up in a location they do not have access to. You can move this in your scene to wherever you like, just make sure it's outside of the owner-only area.

    - **Area Colliders** are what keep non-owners out of your exclusive area. In this prefab, it is a box collider inside of our example asset. You should adjust this in your scenes to best fit your needs.

![TrespassingText](/img/economy/examples/SubsOnlyArea_TresspassingMessage.png "Trespassing message text.")

7. If you would like to keep non-owners inside of a specific area instead, enable **Keep People In Area**. If enabled, make sure your spawn and Trespassing Teleport Location are within the collider instead of outside the collider. 

:::tip
Enable this if you are creating a large, exclusive area, and want to keep all non-owners together. Think lining up to buy tickets before getting into an amusement park. We recommend you keep it disabled if you're only sectioning off a small part of your world, like an exclusive room.
:::

8. In the Hierarchy, click on the drop down arrow next to **ProductOwnersOnlyAreaExamplePrefab**. Click on **ProductOwnersOnlyAreaBlockerToggle**. 

9. In the Inspector, select the same Udon Product you chose earlier. Now, when a player owns this product, this door will be disabled.
    -  You can read more about disabling and enabling blockers like doors and walls on the [Udon Product Toggle](/economy/sdk/examples/product-toggle) prefab page.

9. Run Build & Test!

### Inspector Parameters

* `Udon Product` - The Udon Product that determines ownership.
* `Trespassing Message` - The GameObject that will be activated and shown when the player is trespassing to show them the trespassing message.
* `Trespassing Teleport Location` - The location the player will be teleported to when they have trespassed.
* `Area Colliders` - The colliders that define the area only owners/non-owners can access, depending on the Keep People In Area Toggle. These will be disabled at runtime to save performance.
* `Keep People In Area` - Toggle this on to force players to stay in area instead of keeping them outside the area. Make sure your spawn and Teleport Location are inside the area if you use this.


---

## ドキュメント: product-toggle.md

```metadata
階層: /economy/sdk/examples/product-toggle.md
ディレクトリ: economy\sdk\examples
ファイル名: product-toggle.md
拡張子: .md
サイズ: 3.31 KB
最終更新: 2025-06-05T03:07:52.744Z
```

---
description: "Toggle a GameObject for players who own an Udon product."
sidebar_custom_props:
    customIcon: 🔄
---

# Udon Product Toggle

import SellerNotification from '/docs/economy/_sellers-notification.mdx';

<SellerNotification/>

Udon Product Toggle is a prefab that enables/disables a GameObject when a user owns the Udon Product assigned. This can be used for both local and instance-based objects. Use this in unique ways, like setting up doors that let product owners into private areas, or creating fun, purchasable pop-ups for the entire instance.

import HowToImportExample from '/docs/economy/_ce-how-to-import.mdx';

<HowToImportExample/>

##### Prefabs Included
* **UdonProductToggleObjectOffLocalPrefab**: Sets the target object to be disabled when the local player owns the product.
* **UdonProductToggleObjectOffAnyonePrefab**: Sets the target object to be disabled when anyone in the instance owns the product. 
    - Use these prefabs when a player must be a product owner to remove something for either themself or the entire instance, like a door to an exclusive part of your world.  

* **UdonProductToggleObjectOnLocalPrefab**: Sets the target object to be enabled when the local player owns the product.
* **UdonProductToggleObjectOnAnyonePrefab**: Sets the target object to be enabled when anyone in the instance owns the product.
    -  Use these prefabs when a player must be a product owner to enable something for either themself or the entire instance, like spawning cool effects or new objects.

:::caution
If viewing the example scene, you'll also need the [Open Group Page](/economy/sdk/examples/open-group-page) prefab. Otherwise, your project will be missing what it needs for a complete scene.
:::

### How to Use

For this (and most!) prefabs, you'll first need an UdonProduct to check for and a way for players to buy this product. 

Once you've created a purchasable product:

1. Drag the chosen prefab into your scene. Which prefab you use depends on what logic you would like to have in your world. Each prefab has its own behavior, which you can view in the **Prefabs Included** section.

![DraggingPrefab](/img/economy/examples/ProductToggle_DragIntoScene.png "Dragging the chosen prefab.")

2. In the Inspector, add which Udon Product you would like to check for.

![AddProduct](/img/economy/examples/ProductToggle_SelectProduct.png "Adding a product to check for.")

3. In the Inspector, replace the Target object with whatever object you would like to be enabled/disabled when a player owns the Udon Product. Here's an example of **ToggleObjectOff** in action:

<div class="video-container">
    <iframe src="https://assets.vrchat.com/videos/docs/ProductToggle_Demo.mp4" title="Product Toggle" frameborder="0" allow="encrypted-media; gyroscope; web-share" allowfullscreen></iframe>
</div>

4. Run Build & Test!

### Inspector Parameters

* `Udon Product` - The Udon Product that determines ownership.
* `Targets` - The objects that will be activated/deactivated when the product is bought/expired.
* `Default State` - The default state the target's active state will be set to when the product is not bought.
* `Local Only` - If true, will only activate/deactivate when it's bought/expired by the local player. If false, will activate/deactivate when it's owned by any player.


---

## ドキュメント: supporter-list.md

```metadata
階層: /economy/sdk/examples/supporter-list.md
ディレクトリ: economy\sdk\examples
ファイル名: supporter-list.md
拡張子: .md
サイズ: 5.27 KB
最終更新: 2025-06-05T03:07:52.744Z
```

---
description: "A customizable list of all users who own an Udon product."
sidebar_custom_props:
    customIcon: 🥇
---

# Supporter List

import SellerNotification from '/docs/economy/_sellers-notification.mdx';

<SellerNotification/>

The Supporter List prefab displays all the names of users that have purchased an UdonProduct. This can be customized for how many UdonProducts are displayed, their color, text size, and more.

![SupporterListPreview](/img/economy/examples/SupporterList-Preview.png "Preview of the supporter list prefab.")

import HowToImportExample from '/docs/economy/_ce-how-to-import.mdx';

<HowToImportExample/>

:::caution
If viewing the example scene, you'll also need the [Open Group Page](/economy/sdk/examples/open-group-page) prefab. Otherwise, your project will be missing what it needs for a complete example scene.
:::

### Prefabs Included
* **SupporterListPrefab**: The full panel with the default settings.
* **SupporterListPrefabAutoScrolling**: The full panel, but scrolling is handled automatically.

## How to Use

This prefab requires an Udon Product for each "Tier" you'd like to include in your supporter list.

Importantly, this Udon Product **must** have "Owner Names in Udon" enabled on the VRChat website. You can enable this setting by navigating to ["My Products"](https://vrchat.com/home/marketplace/storefront/products) (`Marketplace > Storefront > My Products`) and editing the Udon Products you would like to use.

1. Drag either "SupporterListPrefab" or "SupporterListPrefabAutoScrolling" into your scene and unpack it. (This allows you to customize your supporter list.)
2. Find the "SupportTiers" GameObjects within the prefab.

![SupportTiersHierarchy](/img/economy/examples/SupporterList-SupportTiersHierarchy.png "Location of the SupportTiers object in the hierarchy.")

3. The prefab has three "SupportTier" objects by default. If you need more or fewer than the three tiers, feel free to duplicate, modify, or delete them as needed. (This is why you unpacked the prefab in step 1.)
4. Customize each [Support Tier](supporter-list#support-tier) with your Udon Product, the title, the color that you want, and any font sizing you'd like to use.

## Inspector Parameters

### Supporter List

![SupporterListInspector](/img/economy/examples/SupporterList-Inspector.png "Preview of the supporter list inspector.")

This prefab controls the scrolling and update behavior of the supporter list.

| Parameter | Type | Description |
| --- | --- | --- |
| **References** | --- | --- |
| Label | TextMeshPro UGUI | The text label that displays the list. |
| Scroll Rect | Scroll Rect | The scroll rect for scrolling the list up and down. |
| View Port Rect Transform | Rect Transform | The rect transform for the viewport of the scroll view. |
| **Settings** | --- | --- |
| Auto Scroll | Bool | If true, the list scrolls automatically. |
| Scroll Speed | Float | The speed at which the list scrolls. If 0, the list will not scroll. Value is in pixels per second. |
| Fade Time | Float | The time it takes for the list to fade in and out. |
| Time Between States | Float | The time before it starts scrolling after fade in and time before it starts fading out after scrolling to bottom. |
| **Performance** | --- | --- |
| Update When Player Is Behind | Bool | If true, the list updates when the player is physically standing behind the list and cannot see the front. Only use if the list is visible from the back. |
| Update When Player Not Looking | Bool | If true, the list only updates when the player is not looking at it. Improves performance but can result in the list not immediately updating when the player looks at it. |
| Min Update Delay | Float | The minimum time between updates. At 0, the list updates every frame. Increase this value to improve performance at cost of list scrolling looking choppy. |
| Max Update Delay | Float | The maximum time between updates. This value is used when the player is behind the list, not looking at the list, or outside the "Distance Outside Max Update Delay" distance. |
| Distance Within Min Update Delay | Float | If the player is this close to the prefab, the min update delay is used. |
| Distance Outside Max Update Delay | Float | If the player is this far away from the prefab, the max update delay is used.  |


### Support Tier

Each tier of your supporter list is represented by a customizable supporter tier object.

![SupportTierInspector](/img/economy/examples/SupporterList-TierInspector.png "Preview of the support tier inspector.")

| Parameter | Type | Description |
| --- | --- | --- |
| **References** | --- | --- |
| Ownership Product | UdonProduct | The product that is checked for ownership. |
| **Tier Settings** | --- | --- |
| Product Label | String | The name of the tier. |
| Name Separation String | String | The string that separates each supporter's name. |
| Add Product Label | Bool | If true, the product label is added to the board content. |
| **Text Settings** | --- | --- |
| Text Color | Color | The color of the text. |
| Product Label Font Size | Integer | The font size of the product label. |
| Name Label Font Size | Integer | The font size of the name label. |
| Max Names Per Line | Integer | The maximum amount of names per line. If 0, there is no limit. |

---

## ドキュメント: timed-event.md

```metadata
階層: /economy/sdk/examples/timed-event.md
ディレクトリ: economy\sdk\examples
ファイル名: timed-event.md
拡張子: .md
サイズ: 3.13 KB
最終更新: 2025-06-05T03:07:52.745Z
```

---
description: "An event that only Udon product owners can trigger."
sidebar_custom_props:
    customIcon: ⏱
---

# Product Event Timed

import SellerNotification from '/docs/economy/_sellers-notification.mdx';

<SellerNotification/>

A ProductEvent is an event that only can be sent by a player who owns a product. Use this to enable GameObjects for a set amount of time when the product event is executed.

This can be customized in a variety of ways! For example, create a paid product that allows those players to set off fireworks for a certain amount of time. 

You can read more about Udon events [here](https://creators.vrchat.com/worlds/udon/graph/event-nodes/) and [here](https://udonsharp.docs.vrchat.com/events/).

<div class="video-container">
    <iframe src="https://assets.vrchat.com/videos/docs/ProductEvent_Preview.mp4" title="Timed Event" frameborder="0" allow="encrypted-media; gyroscope; web-share" allowfullscreen></iframe>
</div>

import HowToImportExample from '/docs/economy/_ce-how-to-import.mdx';

<HowToImportExample/>

##### Prefabs Included
* **SendProductEventPrefab**: A prefab that sends the product event when the button is pressed and enables the GameObject for the set amount of time. Though only a player who owns the product can set off the event, `OnProductEvent` will be executed by all users. Meaning, only people who pay for the product can activate the event, but anyone can see or interact once the event is activated.

:::caution
If viewing the example scene, you'll also need the [Open Group Page](/economy/sdk/examples/open-group-page) prefab. Otherwise, your project will be missing what it needs for a complete scene.
:::

### How to Use

For this (and most!) prefabs, you'll first need an UdonProduct to check for and a way for players to buy this product. 

Once you've created a purchasable product:

1. Drag the **ProductEventTimedSetActivePrefab** into your scene.

![EventPrefabtoScene](/img/economy/examples/ProductEvent_DragIntoScene.png "Dragging the prefab into scene.")

2. In the inspector, locate the **Udon Product** variable. Click on the circle button and replace the example file with your own product. This product must be owned by the player trying to send the event.

![EventProductAdd](/img/economy/examples/ProductEvent_SelectUdonProduct.png "Dragging the prefab into scene.")

3. Next, locate **Enable On Product Event GameObject**. This is what will be enabled when the player with the correct UdonProduct clicks the button. You can replace this with whatever GameObject you'd like.

![EventObjAdd](/img/economy/examples/ProductEvent_SelectCustomObject.png "Adding a custom GameObject.")

4. Use **Enabled Time** to how long you want this GameObject to be enabled. This can range from .5-10 seconds.

5. Run Build & Test!

### Inspector Parameters

* `UdonProduct` - The Udon product that will be used when sending the event. This product must be owned by the player sending the event.
* `Enable Product Event GameObject` - The GameObject that will be enabled when the event is executed.
* `Enabled Time` - How long the GameObject will be enabled for in seconds.


---

# ドキュメント終了

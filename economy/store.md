# store Documentation

This document contains automatically collected documentation with metadata headers for each section.



---

## Document: avatar-marketplace.md

Path: /economy/store/avatar-marketplace.md
---
description: "Contains all avatar product users can buy in VRChat."
sidebar_custom_props:
    customIcon: 🛒
---

# Avatar Marketplace

import SellerNotification from '/docs/economy/_sellers-notification-avm.mdx';

<SellerNotification/>

The avatar marketplace contains [avatar products](/economy/products/avatar) available for purchase in VRChat. It's the most convenient way to discover, buy, and sell VRChat avatars.

## Browse the Marketplace

The avatar marketplace is located in the "Avatars" section of the VRChat menu. Users can browse the marketplace, search for published avatars, and see their [details](/economy/products/avatar#avatar-details). Selecting any avatar product takes the user to its details page.

## Publish an Avatar on the Marketplace

Follow the steps below to publish your avatar on the marketplace:

1. Upload an [avatar](/avatars/creating-your-first-avatar).
2. Create an [avatar product](/economy/products/avatar).
3. Wait for VRChat to review and accept your avatar product.
4. Create a [listing](/economy/listings).
5. Add your avatar product to your listing.
6. Publish your listing.
7. (Optional) Add your listing to a [world store](/economy/store/world-store).

Users can now buy your avatar in VRChat and discover it on the marketplace.

## Become an avatar seller

Only authorized avatar sellers create and publish avatar products on the marketplace. To become an authorized avatar seller, please [submit an application](https://www.surveymonkey.com/r/MC5X89R). We will slowly authorize more avatar sellers over time.

 If you are already a seller in the Creator Economy and wish to gain access to the avatar marketplace, please apply.

---

## Document: group-store.md

Path: /economy/store/group-store.md
---
description: "Contains subscriptions for your group."
sidebar_custom_props:
    customIcon: 👥
---

# Group Stores

import SellerNotification from '/docs/economy/_sellers-notification.mdx';

<SellerNotification/>

Group stores allow you to easily sell [subscriptions](/economy/subscriptions) for your group. After you publish a group store, users can use your group's "Store" tab to discover and buy subscriptions.

If you have a VRChat world, you can add a button that opens your store. Use the [Open Group Page prefab](/economy/sdk/examples/open-group-page) or [`Store.OpenGroupStorePage`](/economy/sdk/udon-documentation#storeopengroupstorepage).

## Create a Group Store

The easiest way to create a group store is to create a [subscription](/economy/subscriptions). If your group does not have a store, creating a subscription also creates a group store.

Alternatively, you can create a group store from the [Store Manager](https://vrchat.com/home/marketplace/storefront/stores) by following the steps below:

1. Open the [store manager](https://vrchat.com/home/marketplace/storefront/stores) on the VRChat website.
2. Click the "Set up new store" button.
3. Select "Group Store."
4. Select one of your groups.
5. Click "Create" to create your group store.
	  - After creating your group store, users won't see it until you publish subscriptions and publish the store.
6. Create [subscriptions](/economy/subscriptions) and add them to your group.
	- You can use the [store manager](https://vrchat.com/home/marketplace/storefront/stores) to rearrange the order in which users see subscriptions in your store. 
7. Publish the subscriptions.
8. Publish your group store in the [store manager](https://vrchat.com/home/marketplace/storefront/stores).

[IMAGE: Publish store]

:::info

If you have a world, use [Udon](/economy/sdk/udon-documentation) to sell your published listings and subscriptions. For example, the [Open Listing prefab](/economy/sdk/examples/open-listing) makes it easier for users to buy subscriptions, increasing your sales.

If you don't want to publish a world store at all, you can use Udon to sell subscriptions instead!

:::

## Viewing your Group Store

After you publish your group store, users can find it by opening the group in VRChat (or the VRChat website) and pressing the "Store" button. They can browse your published subscriptions and buy them.


[IMAGE: Store page]

[IMAGE: Store page in-game]

You can share your group by copying its URL from the VRChat website or by sharing its group ID (e.g., `GROUP.1234`). Users can enter the ID in the Groups tab in VRChat, allowing them to view or join your group.

[IMAGE: Copy group URL]


## Limitations

- Group stores cannot contain [listings](/economy/listings).
- You cannot transfer your group if it has any subscribers.


---

## Document: index.md

Path: /economy/store/index.md
---
sidebar_position: 4
sidebar_custom_props:
    description: "Create stores to sell your listing."
    customIcon: 🏪
---

# Stores

import SellerNotification from '/docs/economy/_sellers-notification.mdx';

<SellerNotification/>

Stores make it easy for users to discover your [listings](../listings) and [subscriptions](../subscriptions) in VRChat. You can have multiple stores, and you can publish listings in multiple stores.

import DocCardList from '@theme/DocCardList';

<DocCardList />

:::info

You can use [Udon](/economy/sdk/udon-documentation) to sell your published listings and subscriptions - even if you didn't publish them in your store.

:::

---

## Document: world-store.md

Path: /economy/store/world-store.md
---
description: "Contains listings for your world."
sidebar_custom_props:
    customIcon: 🌏
---

# World Stores

import SellerNotification from '/docs/economy/_sellers-notification.mdx';

<SellerNotification/>

World stores allow you to showcase [listings](/economy/listings) for your world. After creating a world store, users will see it when viewing your world in the VRChat menu or website. 

[IMAGE: World store]

You can also use Udon to open your world store. For example, you can add an "Open Store" button that uses the `Store.OpenWorldStorePage()` method to open your store.
## Create and publish a world store

You can create a world store for any of your worlds by following the steps below:

1. Open the [Store Manager](https://vrchat.com/home/marketplace/storefront/stores) on the VRChat website (Marketplace >My Store > Store Manager).
2. Select "Set Up New Store".

[IMAGE: Setup new store]

3. Select "World Store", choose one of your worlds from the dropdown, then press "Create".

[IMAGE: Choose world]

4. Add published [listings](/economy/listings) to your world store.
	- You can use the [store manager](https://vrchat.com/home/marketplace/storefront/stores) to rearrange the order in which users see subscriptions in your store. 
	- Read the [listings documentation](https://creators.vrchat.com/economy/listings) to learn how to add listings to your store.

5. Open your store in the store manager and publish it.

[IMAGE: Publish store]

6. Open your world in VRChat. You can now see the world store in the VRChat menu.

[IMAGE: World store in VRChat]

## Limitations

Keep the following restrictions in mind when using world stores:

- You can sell the same listing to multiple world stores.
	- You can sell listings that give users benefits in multiple worlds.
	- Users can't buy the same listing if they already own it.
- You can sell [permanent](/economy/listings#%EF%B8%8Fpermanent) or [temporary](/economy/listings#temporary) listings without adding them to your world store. 
	- You can use Udon to open listings directly, i.e. with [Store.OpenListing](/economy/sdk/udon-documentation#storeopenlisting).
	- You can't sell [instant](/economy/listings#instant) listings without adding them to your world store. In addition, only users who are currently visiting your world can buy instant listings.
- You cannot add [subscriptions](/economy/subscriptions) to your world stores.
	- Use group stores instead.


---

# End of Documentation

---
permalink: /manual/item
title: "Item"
sidebar:
  title: "Manual"
  nav: manual
---

## Core

### Item

Items are a type of ScriptableObject that defines whether an item can be equipped or used and how it acts when it is. Derive from ItemBase and override the appropriate methods to do so and declare fields for relevant for items of that type(for example visuals). For items that do not have any behavior of their own(key items, currency) the GenericItem can be used. ItemBase also defines some basic common properties that most item systems will use like name, image and categories.

### Inventory

An Inventory is where items are stored, mostly in combination with a character. InventoryBase defines some common ways an Inventory may be interacted with like adding, removing or using. AdventureCore comes with a simple implementation called ListedInventory which just stores items in a list without any limitations to item quantity or stack size. To implement a more specialized inventory, for example a re4 style case, just inherit from InventoryBase and implement the needed methods.  

One important distinction to make when working with Inventory is between an ItemQuantity and an InventoryItem. While both of these have an Item and a Quantity they are used very differently. An InventoryItem represents an entry in an Inventory and can be used to  react to the quantity of an item a character has and whether it is equipped. An ItemQuantity is not bound to any Inventory or Character and can be used to configure amounts of items that are gained or used when performing some action like picking up items or using a key.  

The Inventory of a Character also acts as the access point for its ItemSlots.

### ItemSlot

ItemSlots enable Items to be equipped to and interact with a character. Deriving from ItemSlot and overriding equip and unequip is the recommended way to create a new kind of ItemSlot. Since they are in scene and exist by character ItemSlots can take care of any runtime data the item may produce when equipped(visuals, effects, ...).

## Souls

### Armor

The SoulsArmorItem defines a prefab for the visual of the armor when it is equipped. The instancing of that prefab is done by the SoulsArmorSlot which also makes sure the bones on the renderer are set and manages the instance(as in destroys it when unequipped).  

When armor is equipped it can also modify some stats, adding the modifier is done in the slot but could also be done in the item since it has no side effects. Lastly the armor defines attribute requirements which are checked by the item itself in its CanEquip.

### Weapon

In addition to the slot-item combination that armor uses, a weapon also has a SoulsWeapon behavior which serves as the access point for a weapons actions and damages in the weapons prefab. It can be used to check the damage through the prefab when the item is stored in the inventory and for the actions when it has been instanced by the slot.  

The SoulsWeaponSlot allows binding input to its weapons light and heavy actions. It then rebinds these whenever the equipped weapon changes.
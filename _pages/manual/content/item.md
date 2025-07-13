---
permalink: /manual/item
title: "Item"
sidebar:
  title: "Manual"
  nav: manual
---

## Core

The [getting started]({% link _pages/manual/howto/gettingStarted.md %}) tutorial shows how an inventory can be added to a character and how to create item pickups. An example of usable and equippable items and slot can be found in .../AdventureManual/Systems/Item/ManualItem.unity.

### Item

Items are a type of ScriptableObject that defines whether an item can be equipped or used and how it acts when it is. Derive from ItemBase and override the appropriate methods to do so and declare fields for relevant for items of that type(for example visuals). For items that do not have any behavior of their own(key items, currency) the __GenericItem__ can be used. ItemBase also defines some basic common properties that most item systems will use like name, image and categories.

The __EquipmentItem__ acts as a attribute- and stat modifier while it is equipped and whether is can be equipped is also determined by the attributes and stats of the character. The __UsableItem__ can add Attributes, Resources and Effects to the character when used. Both of them derive from __PrefabItem__ which can define a prefab that is instantiated when equipped to a __InstantiatingItemSlot__.

### Inventory

An Inventory is where items are stored, mostly in combination with a character. InventoryBase defines some common ways an Inventory may be interacted with like adding, removing or using. AdventureCore comes with a simple implementation called ListedInventory which just stores items in a list without any limitations to item quantity or stack size. To implement a more specialized inventory, for example a re4 style case, just inherit from InventoryBase and implement the needed methods.  

One important distinction to make when working with Inventory is between an ItemQuantity and an InventoryItem. While both of these have an Item and a Quantity they are used very differently. An InventoryItem represents an entry in an Inventory and can be used to  react to the quantity of an item a character has and whether it is equipped. An ItemQuantity is not bound to any Inventory or Character and can be used to configure amounts of items that are gained or used when performing some action like picking up items or using a key.  

The Inventory of a Character also acts as the access point for its ItemSlots.

### ItemSlot

ItemSlots enable Items to be equipped to and interact with a character. Deriving from ItemSlot and overriding equip and unequip is the recommended way to create a new kind of ItemSlot. Since they are in scene and exist by character ItemSlots can take care of any runtime data the item may produce when equipped(visuals, effects, ...).

AdventureCore includes a couple simple default slot implementations:
- GenericItemSlot  
does nothing itself, items are equipped or not, any actual logic may come from the items  
for example EquipmentItem for raising stats, attributes or adding effects while equipped
- InstantiatingItemSlot(Arena Trinkets)  
only works with PrefabItem, instantiates the prefab while an item is equipped
  - ActionItemSlot(Arena Weapons)  
  looks for an action on the instantiated prefab and allows binding input to it
- UsableItemSlot(Arena Usables)  
uses items equipped to it, for example UsableItem can add resources or attributes  
if the PrefabItem has a Prefab with an action it instantiates it and performs the action(eg drinking animation)

## Souls

### Armor

The SoulsArmorItem defines a prefab for the visual of the armor when it is equipped. The instancing of that prefab is done by the SoulsArmorSlot which also makes sure the bones on the renderer are set and manages the instance(as in destroys it when unequipped).  

When armor is equipped it can also modify some stats, adding the modifier is done in the slot but could also be done in the item since it has no side effects. Lastly the armor defines attribute requirements which are checked by the item itself in its CanEquip.

### Weapon

In addition to the slot-item combination that armor uses, a weapon also has a SoulsWeapon behavior which serves as the access point for a weapons actions and damages in the weapons prefab. It can be used to check the damage through the prefab when the item is stored in the inventory and for the actions when it has been instanced by the slot.  

The SoulsWeaponSlot allows binding input to its weapons light and heavy actions. It then rebinds these whenever the equipped weapon changes.

## Hero

All the items in the hero demo have a visual prefab which is used when the hero presents the item when it is first picked up.

### Gear

Sword and Shield are simple __PrefabItem__ from AdventureCore. They are instantiated on the respective __InstantiatingItemSlot__ when equipped. Their actual functionality in defined in the attack and guard actions which themselves check that an item is equipped.  

The hero demo also has various items that expand the inventory. For example the HeroBombPouch that lets the character carry more bombs. These did not make it into the levels yet but can be tried out in the HeroDebuggingGeneral scene(chests).

### Equipment

Equipment items hold an action prefab and can be assigned to on of three equipment slots to use them. When the __HeroEquipmentItem__ is equipped to a __HeroEquipmentSlot__ its action prefab is instantiated and when the slot is used that action gets started. When an item is used directly from the inventory it gets equipped to a hidden EquipmentX slot.

### Usable

__HeroHeart__ is a __UsableItem__ which restores one __HeroHealth__ resource.  

The __HeroHeartTriple__ is a placeholder for three __HeroHeart__ items. Therefore it is presented as a seperate item but when added to the inventory three hearts are added instead.

__HeroHeartEssence__ adds one __HeroHearts__ attribute which expands the characters health pool. __HeroHeartShard__ is placeholder for the essence but has a prerequisite count of four before it is replaced. 
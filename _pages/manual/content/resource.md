---
permalink: /manual/resource
title: "Resource"
sidebar:
  title: "Manual"
  nav: manual
---

## Core

The [getting started]({% link _pages/manual/howto/gettingStarted.md %}) tutorial shows how resources can be added to a character and how to create damaging trigger areas. An example of different resource types and bars can be found in .../AdventureManual/Systems/Resource/ManualResource.unity.

Resources are floating point metrics that can change on a frame to frame basis. A resource has a resource type which is mostly used to identify it and a ResourceValue behavior which actually holds and changes the value. The default ResourceValue does not change on its own. For more specialized resource values that change their value over time or by some other means you can inherit from ResourceValue(look at ChangingResourceValue for an example)

Resources are managed by a ResourcePool which can be used to check, change and monitor resource values.  

## Souls

__SoulsHealth__ is a simple ResourceValue that does no change on its own. It is checked after a character receives damage to see if they die.  

__SoulsEndurance__ is a custom behavior in AdventureSouls called SoulsStamina. This is similar to a ChangingResourceValue but also subtracts from its value when the player is sprinting.  

__SoulsPoise__ is a resource value that defines whether the character gets staggered by a hit. It gets damaged by attacks and then refills on its own. 

## Hero

The player only has the __HeroHealth__ resource which gets reduced by __HeroHealthDamage__ and recovered by picking up __HeroHeart__ pickups.  

The Damage subgraph in __HeroCharacter__ checks if the health has been reduced to zero and starts the Die __TimelineAction__ with the __HeroDeath__ timeline.

In addition to health skeletons also have the __HeroStun__ resource. This __EffectResourceValue__ adds the __HeroStun__ effect to the character which stops movement and animations. The __HeroStunResource__ visual graph is in charge of visualizing the effect by tinting the model materials in a blue shade.
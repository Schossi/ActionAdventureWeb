---
permalink: /manual/resource
title: "Resource"
sidebar:
  title: "Manual"
  nav: manual
---

## Core

Resources are floating point metrics that can change on a frame to frame basis. A resource has a resource type which is mostly used to identify it and a ResourceValue behavior which actually holds and changes the value. The default ResourceValue does not change on its own. For more specialized resource values that change their value over time or by some other means you can inherit from ResourceValue(look at ChangingResourceValue for an example)

Resources are managed by a ResourcePool which can be used to check, change and monitor resource values.  

## Souls

__SoulsHealth__ is a simple ResourceValue that does no change on its own. It is checked after a character receives damage to see if they die.  

__SoulsEndurance__ is a custom behavior in AdventureSouls called SoulsStamina. This is similar to a ChangingResourceValue but also subtracts from its value when the player is sprinting.  

__SoulsPoise__ is a resource value that defines whether the character gets staggered by a hit. It gets damaged by attacks and then refills on its own. 
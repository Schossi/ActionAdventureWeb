---
permalink: /manual/attribute
title: "Attribute"
sidebar:
  title: "Manual"
  nav: manual
---

## Core

Attributes are whole number metrics that, roughly speaking, define a characters capabilities(strength, vitality, ....) but can also be used for any other number that should be persisted and does not fit any other concept(xp). Attributes do not define any behavior of their own, instead other things that depend on them will check them when needed. To add an attribute simply create an Adventure/AttributeType from the context menu in your assets.  

In contrast to attributes an AttributeStat does not have a value of its own. Is is usually calculated based on one or multiple attributes or even just a constant value that just gets changed by modifiers. How a stat is calculated is defined by in its GetValue method, derive from AttributeStat to define your own calculations. Check the stats already defined in AdventureCore for examples.(MultipliedStat, TieredStat, ...)  

The AttributePool manages the available attributes and stats and their value in the scene. It is usually attached and assigned to a character. This is also where modifiers can register to change exposed values and where observers can register to be notified of changes of attributes and stats.  

## Souls

The __SoulsVitality__ attribute can be leveled up at bonfires and influences the __SoulsHealthMaximum__ stat which defines how many hit points the player has. On enemies the hit point maximum is set directly without using a stat.  

__SoulsEndurance__ is pretty much the same but for __SoulsStaminaMaximum__.  

__SoulsStrength__ can be leveled at the bonfire and influences the __SoulsPhysicalAttack__ which is used by weapons to calculate their damage.  

__SoulsLevel__ and __SoulsExperience__ are counters that are used to check if the player can level up and how many times they have done so.  

Lastly __SoulsFlask__ defines how many flasks are restored to the player when they sit at a bonfire. There is no way to change it currently but it has been added so increasing it with a pickup or some other action can be implemented later.

The stats __SoulsPhysicalDefense__ and __SoulsPoiseMaximum__ are not influenced by any attribute, they just have some base value and are only increased by modifiers like armor.  

## Hero

AdventureHero only defines a single Attribute called __HeroHearts__. It is used in the __HeroHeartsMaximum__ stat to define the maximum health in the AttributePool of the player character. The Attribute starts at 3 and gets raised by collecting __HeroHeartEssence__.
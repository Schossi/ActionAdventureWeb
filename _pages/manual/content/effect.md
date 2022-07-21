---
permalink: /manual/effect
title: "Effect"
sidebar:
  title: "Manual"
  nav: manual
---

## Core

Effects are temporary behaviors that can be added or removed from a character. Every __EffectType__ scriptable object assets describes an effect and points to a prefab that will be instantiated when the effect is added to a character. These prefabs have some implementation of __EffectBase__ at its root which, through its Pool property, gets access to the character it was added to.

The __GenericEffect__ implementation of EffectBase can be used when an effect is only visual or if the effects logic can be expressed using character instructions. __Attribute-__ and __StateEffect__ modify a characters attributes and stats and can be useful for buffs and de-buffs. __DamageEffect__ periodically sends a damage event to the character and may be used for a burning or poisoned status for example. Lastly the __TimeoutEffect__ is an expanded version of GenericEffect that removes itself after some time instead of having to be removed be some outside logic.

One built in way to add an effect to a character is the __EffectResourceValue__ which adds the effect when its value is full. Another would be using a GenericTriggerArea which has events for when a character enters or leaves its area. Since EffectType has an Add method that takes a character as its parameter it is possible to link it directly in the inspector.

## Souls

The __SoulsPoisoned__ DamageEffect gets added when the players __SoulsPoison__ EffectResourceValue reaches its maximum. The resource is increased when something deals __SoulsPoisoning__ ResourceDamage to the player. This kind of damage is currently only dealt by the sprout enemy which has a ticking DamageSender and is either reduces by just waiting or by using the SoulsMoss ConsumableItem.

__SoulsBuffed__ is a simple TimeoutEffect buff that doubles the characters strength for a set time and then removes itself which is added by the SoulsBooster consumable item. __SoulsSlowed__ reduces a characters movement speed. Both of theses use character instructions for their actual effect.
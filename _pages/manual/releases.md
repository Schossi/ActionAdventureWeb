---
permalink: /manual/releases
title: "Release Notes"
sidebar:
  title: "Manual"
  nav: manual
---

## 1.1.0

### ADDED

- __Shrine__ - New Souls Demo Stage!
  - Trader and Talker NPCs  
  can be interacted with or attacked
  - Sprout Enemy  
  causes poison damage
  - Elevator  
  one way elevator with levers
  - Teleporter  
  moves character between scenes
- __Effects__ - New Core System!  
set of behaviors that can be added or removed from characters
  - Poisoned  
    periodically causes damage, healed by the new moss item
  - Boosted  
    doubles strength for a while, added by the new booster item
- Inventory  
  allows using items directly from the UI without equipping them
- Loading Screen

### IMPROVED

- intro can now be skipped
- NPCs can return home or patrol when idle
- additional events for characters on GenericTriggerArea/Item
- better gamepad support
- movement and resources are not saved unless they actually change
- picked up items are now displayed in a message box
- persistence can now be exported and imported from PersistenceContainer
- boss arena properly resets player position
- camera now uses cinemachine free look
- ...

An integration for the __ink__ narrative scripting language is available on [GitHub](https://github.com/Schossi/AAK_Ink)! 

--WARNING--  
Depending on your depth of use this update will contain a varying amount of breaking changes!
Using source control and removing any previous versions completely when upgrading is recommended.

## 1.0.1

### ADDED

- manual page for SoulsPlayer(includes step by step guide for replacing the model)

### CHANGED

- Animator is now part of CharacterBase, AnimatedCharacterBase is therefore obsolete

### IMPROVED

- simplified model replacement for SoulsPlayer
  - colliders moved to separate GameObject 'Body'
  - TimelineAction and SoulsBonfireAction can pass character animator to the PlayableDirector which lets us remove the dependency between environment and player model
- translation and rotation of model can be suppressed independently
  - SoulsAttackAction no longer locks rotation until damage is activated
- MovementSaver can be overridden from outside
  - BossArena uses this to properly reset the player if the game is quit during a fight
- intro sequence can now be skipped
- 2021 LTS compatibility

### FIXED

- usable items not being hidden if action is interrupted
- potential initialization order errors
  - AttributePool value/stat dictionaries
  - CharacterActionArea Bindings
- armor no longer vanishes at certain camera angles

## 1.0.0

__Initial Release__
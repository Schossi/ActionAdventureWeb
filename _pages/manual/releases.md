---
permalink: /manual/releases
title: "Release Notes"
sidebar:
  title: "Manual"
  nav: manual
---

## 1.2.0

This update continues building out the souls demo while polishing up the core framework. It also adds a getting started tutorial project that should provide a smoother experience at the beginning. There is also a new integration available on [GitHub](https://github.com/Schossi/AAK_KinematicCharacterController) for the Kinematic Character Controller asset!  

### ADDED

- Step by step getting started tutorial
- Pebble usable item in souls which can be thrown
- Two handing weapons in souls
  - stances for two handed melee and bows
  - assignable actions for all 4 shoulder buttons
- Bow and Arrow in souls
  - damage is summed between bow and arrow
  - easily customizable arrows

### FIXED

- Camera damping in souls demo
- SoulsBossArea not properly resetting on player death
- MotionAction without cost now works on characters without ResourcePool
- Namespace of SoulsHideHUDInstruction which unfortunately breaks references so it has to be reassigned or fixed in text

### IMPROVED

- MovementBasePersisted does not error if no Persister is set
- LockableCameraFreeLook can move a target group member instead of adding the lock point
- LockOnManager can directly parent a visual to the locked point

Starting from this version I will no longer upload a dedicated version for the 2021.2 tech stream. I recommend using the latest 2021.3 LTS version.

## 1.1.1

Various smaller changes made for the AdventureSouls Scene Connector integration example. The example is called ConnectorSouls and can be found on [GitHub](https://github.com/Schossi/ConnectorSouls)!  

### IMPROVED

- TimelineAction and SoulsBonfireAction auto bind CinemachineBrain to PlayableDirector 
- AnimationToggler can be moved instantly using SetA and SetB
- SoulsLadderAction EndingTop and EndingBottom events
- SoulsLoading can be configured to load additional scenes
- SoulsPlayerCharacter multi scene support
  - saves current scene as checkpoint
  - goes to loading screen on death in a different scene
  - recovery is instantiated under SoulsCommons
- EffectPool and ResourcePool persist immediately when reset

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
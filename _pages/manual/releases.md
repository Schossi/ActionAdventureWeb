---
permalink: /manual/releases
title: "Release Notes"
sidebar:
  title: "Manual"
  nav: manual
---

## 1.4.1

second update focused on the new 'Hero' demo

### ADDED

- HeroDemo
  - Title Screen, Scene Transition and Death Action
  - First Person View
  - Gear and Equipment Menus, Health and Damage in HUD
  - Stunning Nut and exploding Bomb Equipment Items
  - Pouches that expand carrying capacity for Money, Nuts, Bombs
  - Carrying Blocks, Pots and Bombs
  - Heart, HeartContainer, HeartPiece, Coin, Nut and Bomb Pickups
  - ...and much more, check out the HeroDebuggingGeneral and HeroTitle scenes!
- InputBindingText  
displays current input binding in UI texts(Legacy, TMPro, UIToolkit)  
useful for texts like 'Press {Confirm} to continue!'
- StateManager  
simple state manager that acts through unity events  
used in Hero for game and HUD state
- Simple UI Dialog Windows(OkCancel, YesNo, ... decisions)  
usable from code and new custom visual scripting nodes  
used in hero demo for name input and delete save game check
- Marching in MovementBase    
makes a character walk to or toward some target  
timeline track included so marching can be used in cutscenes
used in Hero when entering and exiting scenes

### IMPROVED

- More Custom Visual Scripting Units  
Hero uses visual scripting for a large part of its logic(see AdventureHero/Graphs)
- More Custom Editors  
many of the core components now have custom editors for improved debugging  
trigger areas show their current items, actions can be started, items added, ...
- Save Data Window for PlayerPrefSaver  
see Window/ActionAdventure/SaveData or 'Edit Save Data' on the PersistenceContainer
- TMPro Support  
Hero uses TMPro, Souls is still UIToolkit
- Actions can be started by name if they are children of the main actor
- CharacterControllerMovement can now apply weight(WeightPower field in Inspector)

### CHANGED

- visual scripting now uses ICharacterAssociator instead of directly referring to characters  
ICharacterAssociator implemented by components that are directly owned by characters  
this allows units on actions to easily use the executing character
- damage direction is stored in DamageEvent  
damage direction should be assigned to DamageEvent.Vector during IDamageSender.OnDamage    
TriggerDamageSender now has additional direction options(Up, SenderToReceiver, ...)
- force of damage and ragdolls is applied as ForceMode.Impulse
- neutral layer tweaked to be more consistent with Player and Enemy
- minimum recommended unity version increased to 2021.3.29

## 1.4.0

first in a series of 2-3 updates that focus on the second demo titled 'Hero'  
it is experimental for now and subject to major changes in the future

### ADDED

- AdventureHero Demo [Experimental]  
movement, attacking, lock on, item pickup and various other systems are already working
you can check out the current state of the demo in the HeroDebuggingGeneral scene
- AdventureCore.Tests Project  
contains test scenes for various features(lock on, timeline textbox)
- Timeline TextBox  
can be used to display text using a timeline and pause it until confirmation  
examples can be found in AdventureHero and the AdventureCore.Tests Project
- Timeline Instructions
applies Character Instructions from a timeline track
used in AdventureHero to suspend character movement while showing pickups
- Additional Visual Scripting Nodes for Lock On, Acting, VectorDirection, ...

### IMPROVED

- Additional sounds in Souls Demo for Bow, Pebble, ....
- Particles Effect for healing in Souls Demo
- added PropelCharacterLocal which is useful for motions like jumping
- horizontal velocity in CharacterControllerMovement improved
- LockOnManager now also manages and exposes the candidate lock on point 
- LockOnManager now checks the entire ray between camera and potential points  
this means models with lock on points can't be on the default layer  
therefore target dummies in souls are now on the new neutral layer

### FIXED

- Sprinting not working in Souls demo until button released again
- Bonfires did not suspend movement correctly 
- Projectiles not being destroyed in Souls Demo
- E Key not working in Souls Demo Title

## 1.3.1

### ADDED

- Simple Third Person Prefab (AdventureManual/Prefabs/AAKThirdPerson)
  - Basic CameraController for Third Person
- Resource System Manual Scene (AdventureManual/Systems/Resource/ManualResource)
  - Resource Bars for uGUI

## 1.3.0

### ADDED

- Visual Scripting Support
  - custom visual scripting units for all core systems 
  - visual scripting version of the getting started scene
  - enemy behavior examples
    - simple examples based on getting started
    - advanced enemies for souls demo(archer, attacker)
- Loot in Souls Demo
  - random state is persisted
  - define items and chances
  - loot drop is persisted until pickup 
- Item Manual Example Scene
  - equipment that is visible on the character
  - usables that add health and vitality

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
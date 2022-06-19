---
permalink: /manual/souls-player
title: "Souls Player"
sidebar:
  title: "Manual"
  nav: manual
---

The following is an overview of the SoulsPlayer prefab in the AdventureSouls demo.

- [SoulsPlayer](#soulsplayer)
  - [Pivot](#pivot)
    - [Body](#body)
    - [Model](#model)
    - [Critical](#critical)
  - [Actor](#actor)
    - ...
  - [Items](#items)
    - ...
  - [Metrics](#metrics)
    - Health
    - Stamina
    - Poise
  - Camera
  - Sound
  - LockOn

## SoulsPlayer

The main transform contains the following components.

- [SoulsPlayerCharacter]({% link _pages/manual/content/character.md %})
- [ManualPersister]({% link _pages/manual/content/persistence.md %}) used to save any state related to the player
- [CharacterControllerMovement]({% link _pages/manual/content/character.md %}) and the CharacterController it uses
- [MovementSaver]({% link _pages/manual/content/persistence.md %}) automatically saves the movement data
- Rigidbody and CapsuleCollider which are used by
  - GenericTriggerItem with Key PLY which triggers GenericTriggerAreas in the environment(for example the traps in the dungeon)
  - CharacterActionArea which is used by the character to detect and start actions in the environment(doors, chests, ...)

## Pivot

This transform gets rotated by the [movement]({% link _pages/manual/content/movement.md %}) to face the direction the player is moving. 

### Body

Contains the [TriggerDamageReceiver]({% link _pages/manual/content/damage.md %}) that receives damage. It also has a second CapsuleCollider with IsTrigger unchecked which pushes around decorators like smashed crates. The components of the __Body__ GameObject were originally located on the __Model__ but have been moved into a separate GameObject to make model replacement easier.

For the sake of simplicity and performance the collision model for the player only consists of a simple capsule. If a more detailed model is required do the following:

- Move TriggerDamageReceiver and Rigidbody to your Model they use any collider in the players model
- Delete the __Body__ GameObject 
- Change the Layer of __Model__ to PlayerBody
- Create detailed colliders in the __Armature__ of the model, skip if your model already comes with colliders

### Model

This is where the visuals of the player are located. It contains the Animator along with an AnimatorProxy that passes along events and root motion from the animator to the character.

Some other components of AdventureSouls depend on the players model and have to be reassigned if the model is replaced. The following steps show how the player model could be replaced by the zombie model that is usually used by the enemies.

- Open up the SoulsPlayer prefab
- Drag the Models/Characters/SoulsZombie into __Pivot__
- Add an Animator and an AnimatorProxy to it
- Assign the __SoulsPlayer Controller__ and __SoulsPlayerAvatar Avatar__ to the Animator
- Assign the __SoulsPlayer__ as the __Character__ for AnimatorProxy 
- Completely expand the old and new models
- the original armature will contain __Left/RightHand__ transforms under its Hand.L/R transform, drag those over to the new models Hand.L/R(these are used to instantiate items by the weapon and usable item slots)
- Delete the original __Model__
- In the __ArmorTop/Bottom__ item slots assign CharacterBody to the now missing __Template__ field, it is needed so the armor moves properly with the character model
- On the __SoulsPlayerCharacter__ script reassign the missing __Animator__ to the one you just created and set SoulsZombie as the __Model__ that is replaced by the ragdoll
- At the bottom of SoulsPlayerCharacter you can empty out the HeadRenderer field, it is used for the character customization sliders but the zombie model does not have the needed blend shapes

When you now open up SoulsDebuggingGeneral you should be able to move around and perform actions with the Zombie. If you start the game through the title you'll see that things there are mostly unchanged but when you start a game the model is in fact changed.

The process to replace the model may be different depending on the model used but generally, as we have seen above, the following dependencies have to be satisfied.

- animator and model on SoulsPlayerCharacter
- hand transforms and body renderer for the item slots

### Critical

The Critical GameObject holds the actions and triggers necessary for ripostes and backstabs.

The SoulsCriticalAction is the one performed by the player, it uses a GenericTriggerArea to find potential enemies that may be critted. When it is started it forces the enemy to perform the SoulsCrittedAction which disrupts anything it was doing. This action moves the enemy to the Front(riposte) or Back(backstab) transform in order for the animations to line up.

## Actor

All the [actions]({% link _pages/manual/content/acting.md %}) a player can perform on its own as well as the actor are found here.

- Sit is not really in use currently but serves as an example of a gesture that is performed until some other input arrives
- Turn is started by the movement when the character makes a sharp turn
- Roll is started by the SoulsPlayerCharacter when dodge is pressed and the input direction is not neutral, in the Starting/Ending events in the inspector you can see that it activates a damage while it is active.(the damage has value 0 so it destroys boxes but does not hurt enemies)
- Dodge is started by the SoulsPlayerCharacter when dodge is pressed and the input direction is neutral
- Jump is directly bound to the Act.Jump input by SoulsPlayerCharacter
- Stagger is started by the SoulsCharacterBase base class that the player has in common with the enemies when the character runs out of poise from being hit
- GuardBreak is started by the SoulsCharacterBase when a character guards and runs out of stamina or it is parried while attacking
- Death is started by SoulsPlayerCharacter when it gets hit and health runs out, notice the SignalReceiver which calls the ResetDeath method

## Items

Holds [items]({% link _pages/manual/content/item.md %}), item slots and inventory. You can add entries to the Items field on the ListedInventory to make the character start out with certain items which is quite useful for debugging.

- Usable is a special item slot that is just a placeholder for the three slots it has as its children. This lets us bind input and UI to that single item slot. Which one is currently active can be switched through using the F key. 
- ArmorTop/Bottom are slots for SoulsArmorItem, they need a reference to the CharacterBody in order to copy the bones over to the armor they instantiate which makes the armor mesh move with the character
- WeaponLeft/Right are slots for SoulsWeaponItem, they have a target transform inside the character models armature which they parent their meshes to, the weapons then move because they are children of the characters hand bone. Also notice that the weapon slots have fists as their fallback item, otherwise the character would have no actions to perform when no weapons are equipped

## Metrics

Holds the [resource]({% link _pages/manual/content/resource.md %}) and [attribute]({% link _pages/manual/content/attribute.md %}) pools. Attributes are defined directly in the pool while resources are separate scripts found in the children.
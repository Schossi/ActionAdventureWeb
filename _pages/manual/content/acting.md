---
permalink: /manual/acting
title: "Acting"
sidebar:
  title: "Manual"
  nav: manual
---

## Core

### Actor

A character actors manages a stream of actions for a character. That means taking care of how and when they are started and ended as well as forwarding the appropriate messages to them.  

CharacterActorBase can technically accommodate more than one active action at a time but the most common scenario is that actions are performed one after the other rather than parallel. SerialCharacterActor serves as a good base class for these scenarios. Both the MinimalCharacterActor(ai) and BufferedCharacterActor(player) inherit from it.  

### Action

An action is basically anything a character can do that may occupy it for a while.  

Where the action behavior is defined depends on its use. It could be defined directly on a character so it can be triggered by AI or input(attacks, moves, ...). Alternatively it may also sit on some object in the environment and become available to the character by colliding with a CharacterActionArea(Door, Levers, Dialog, ...).

The most important methods to override when implementing a new action are:  

__CanStart__ returns if all requirements to start are met(key for door, stamina for move).  
__OnStart__ is called by the actor when the action is actually started so this is the main point where things are kicked of.  
__CanEnd__ defines if an action can be ended by the outside even though it has not ended itself yet.  
__OnEnd__ is called by the actor when the action is ended. This is where things are reset for reusable actions or when actions are destroyed if they are one time.  
__OnMessage__ is where messages from the character will be directed when the action is active. For example events from the animator that are needed so the action knows when it has ended. 

## Souls

### Attack

Attacks in AdventureSouls have a custom implementation called SoulsAttackAction.  

Attacks __CanStart__ when the character has enough stamina and either the action has not started or when it has and it CanCombo. __OnStart__ it triggers an animation and adds an instruction that suspends the characters regular movement so that only the root motion from the animation is used. It also removes the used stamina from the character.  

It responds to the following in __OnMessage__  
START confirms that the animation has actually started, this makes sure we are not responding to events from the wrong animation.  
DMG_ON/DMG_OFF control whether the damage on the weapon is turned on.
COMBO is sent when the animation is ready to transition to the next swing.  
END means the animation has ended and the action should end.  

__CanEnd__ is true when the attack CanCombo and the next action is that same attack. __OnEnd__ the attack reset some things including the combo counter if the next action is not itself.

### Roll

The roll in AdventureSouls uses an action from AdventureCore called MotionAction. This action is useful for rolls, jumps and any other moves that are based on a character animation.

__CanStart__ returns true when is has no cost or when the character has any of the demanded resources left. __OnStart__ removed the cost from the character and sets the animation trigger. If AlignCharacter is checks(which is the case for a roll) the motion calls AlignToInput on the characters movement which. Without this the player would not be able to change direction when rolling multiple times in a row because movement is always suspended.  

__CanEnd__ is true when RELEASE has been received by __OnMessage__ so we can transition into the next roll before it has ended. __OnEnd__ just resets things.  

### Gate

The gate in AdventureSouls uses an action from AdventureCore called ObjectAction. This is a good action for interacting with objects in the world. It supports one animation on an object you're interacting with and another(optional) one on an object that should move in the end. Is is useful for doors, levers and chests.

An ObjectAction __CanStart__ always unless there is a cost item defined the character does not have. __OnStart__ it moves the character into place and triggers its animation. When ACT is received in __OnMessage__ it starts the animation on the lever and in __OnEnd__ it starts the one on the gate.
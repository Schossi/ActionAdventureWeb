---
permalink: /manual/movement
title: "Movement"
sidebar:
  title: "Manual"
  nav: manual
---

## Core

The movement is where all the traversal logic for a character is located. Movements will probably vary quite a bit depending on if it is meant for the player or some NPC and also depending on the game. This is why __MovementBase__ defines mostly ways for other components to interact and interfere with movement in some general ways. For example it gives other components a way to slow movement down, suspend it when some animation plays or force it to move somewhere. Any more specialized logic will have to lie in the implementation.  

Conveniently the generic base classes for characters(CharacterBaseTyped, AnimatedCharacterBase) allow us to declare the type in the class header which lets us use the implementation directly from inside the character or any other component that references the character implementation rather than CharacterBase. 

## Souls

AdventureSouls uses the __CharacterControllerMovement__ for its player, the __SoulsPlayerCharacter__ send all the necessary input to it.  

The zombie enemies use the __NavMeshAgentMovement__ implementation in combination with __NavApproachAction__ so that they can follow the player around the level.  

Lastly the Morningstar boss uses __ManualMovement__ since it does not really do any traditional movement. Even if the Morningstar had more movement of its own a nav mesh would not really be necessary. The boss arena is a simple rectangle and there is no way for the boss to leave to as long as it aligns itself to the player and walks it will always get to them.  

## Hero

Similar to the souls demo the player in hero uses __CharacterControllerMovement__ and the skeleton enemies use __NavMeshAgentMovement__.  

Looking at the component you may find that the movement speeds are both set to 0. This is done because movement in hero is driven by root motion.
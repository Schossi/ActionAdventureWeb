---
permalink: /manual
title: "Overview"
sidebar:
  title: "Manual"
  nav: manual
---

## Welcome

This is the manual for Action Adventure Kit by SoftLeitner. Action Adventure Kit is quite the mouthful so I will be referring to it using the shorthand AAK.  

Action Adventures are a very wide, loose genre with all kinds of different games and sub genres. There are some things that many of them have in common though. Lots of them have characters that perform actions. These might have items that change hands and attributes that define their capabilities.  

AAK was made to provide a solid foundation for these common ideas within the genre so you can focus on what makes your game special.  

Since AAK goes for a broad base of functionality rather than something more specialized it generally tries to do anything in the most default, unity-built-in way possible. For example the movement in the souls demo uses the default unity character controller and the intro is done using timeline. To accommodate for the fact that more special systems may be required for movement, inventory, actions, ... AAK was made from the ground up with expandability in mind.

## Project Structure

AAK is separated into multiple projects.

### AdventureCore

The Core Framework of AAK, always import this one.  

### AdventureSouls

Contains the [soulslike]({% link _pages/demos/demoSouls.md %}) demo, import if you want to start by adapting this demo.  

Start up Scenes/Title/SoulsTitle to __start the game from the title screen__ just like the demo does.  

To __jump directly into the game__ open Scenes/Dungeon/SoulsDungeon for the level itself, add Scenes/Dungeon/SoulsDungeonTemp for the temporary parts like enemies and crates.  

Scenes/Debugging/SoulsDebuggingGeneral is a __useful scene for testing__ out all the actions and Scenes/Debugging/Enemies/SoulsDebuggingEnemies can be used to debug combat.  

Scenes/Debugging/Interaction/SoulsDebuggingInteraction can be useful to __synchronize the character with some object they are interacting with__, this is done using timelines. 

The models in this demo were made using blender and then exported to fbx for unity. You can find the original blend files and the used export settings in AdventureSouls/AdventureSoulsBlender.zip.

### Dependencies

__AdventureCore__
- [Input System](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.3/manual/index.html) allows binding character actions directly to inputs
- [Timeline](https://docs.unity3d.com/Packages/com.unity.timeline@1.6/manual/index.html) used to provide a character action that waits for a timeline to finish
- [Cinemachine](https://docs.unity3d.com/Packages/com.unity.cinemachine@2.8/manual/index.html) used in a helper that performs camera locking using two virtual cameras

__AdventureSouls__
- [Input System](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.3/manual/index.html) for all the player inputs
- [Timeline](https://docs.unity3d.com/Packages/com.unity.timeline@1.6/manual/index.html) for the intro and bonfire actions
- [Cinemachine](https://docs.unity3d.com/Packages/com.unity.cinemachine@2.8/manual/index.html) for the main camera
- [Universal RP](https://docs.unity3d.com/Packages/com.unity.render-pipelines.universal@12.1/manual/index.html)
- [ProBuilder](https://docs.unity3d.com/Packages/com.unity.probuilder@5.0/manual/index.html) used to build the environments

## Manual

This manual is meant to explain the concepts and ideas of AAK rather then any specific detailed API. For more detailed explanations of every class in the core framework and most of the demo please consult the code itself. I try to give a detailed explanation for the purpose of the class in the xml-doc of the class itself and explain every field of the behaviors in the tooltip.  

The manual pages for the core systems of AAK are always split into a Core part that explains the idea behind the system and a Souls part that covers how the system was used in that demo.

The Souls section of the manual contains some additional information about how the AdventureSouls demo is set up and how it may be extended.

## Roadmap

There will be one more update that focuses on rounding out and polishing the current core functionality and the souls demo. After that I will most likely start on a second demo game(OoT?, 2d?). Feel free to let me know if you have some features or game types that you'd like to see in the future.

## Feedback

The quickest channels to reach me are mail and discord. Please feel free to reach out with any problems and questions. Feedback regarding the general direction of AAK and particular future features are also always welcome. Though I might not immediately be able to incorporate your requests I very much take them into consideration when planning out future updates.  

If you can spare the time please consider leaving a review in the asset store.
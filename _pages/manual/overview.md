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

## Setup

AAK is separated into multiple projects. If you are using AAK for the first time I recommend importing everything, including settings, into a new project.  

If the project you are importing into uses the old input system the editor will prompt you to restart and you will have to start the import again after that.

### AdventureCore

The Core Framework of AAK, always import this one.  

### AdventureCore.Tests

Contains test scenes for various features, can be useful to try out those features in isolation. Import while exploring AAK but should not be included in production.

### AdventureExtras

Contains additional examples that use assets from the other demos. For example scenes that demonstrate how different objects can be reused between demos. An explanation for these can be found [here]({% link _pages/manual/other/extras.md %}). Since extras are built on top of the other demos these also have to be imported and I recommend exploring them individually first.

### AdventureHero

Contains the [hero]({% link _pages/demos/demoHero.md %}) demo, import if you want to start by adapting this demo.  

Start up Scenes/HeroTitle to __start the game from the title screen__ just like the demo does.  

__IMPORTANT__, before playing the 'SaveSlot' app variable has to be added in visual scripting. Select the Logic Object in the Hierarchy and click 'Edit Graph' which opens the visual scripting graph. In the 'App' tab on the bottom left add a variable called 'SaveSlot' of type Integer with a default value of -1. This is needed to carry over the save slot between scenes.  

Scenes/Debugging/HeroDebuggingGeneral is a __useful scene for testing__, some more specialized scenes can be found in the same folder  

### AdventureManual

Contains the [getting started]({% link _pages/manual/howto/gettingStarted.md %}) project. Recommended to learn about the various systems of AAK in a minimal environment before jumping into the more complex souls demo.

Also contains some Visual Scripting examples. Be sure to read the [visual scripting]({% link _pages/manual/howto/visualScripting.md %}) manual page before jumping into any scenes using Visual Scripting as there is some additional setup required!

### AdventureSouls

Contains the [soulslike]({% link _pages/demos/demoSouls.md %}) demo, import if you want to start by adapting this demo.  

Start up Scenes/Title/SoulsTitle to __start the game from the title screen__ just like the demo does.  

__IMPORTANT__ whenever starting the souls demo from the editor you need to click into the window to lock the cursor before some input is accepted. This is done to avoid performing actions on accident from outside.

To __jump directly into the game__ open Scenes/Dungeon/SoulsDungeon for the level itself, add Scenes/Dungeon/SoulsDungeonTemp for the temporary parts like enemies and crates.  

Scenes/Debugging/SoulsDebuggingGeneral is a __useful scene for testing__ out all the actions and Scenes/Debugging/Enemies/SoulsDebuggingEnemies can be used to debug combat.  

Scenes/Debugging/Interaction/SoulsDebuggingInteraction can be useful to __synchronize the character with some object they are interacting with__, this is done using timelines. 

The models for the demos were made using blender and then exported to fbx for unity. You can find the original blend files and the used export settings in AdventureModels.zip.

### Dependencies

__AdventureCore__
- [Input System](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.4/manual/index.html) allows binding character actions directly to inputs
- [Timeline](https://docs.unity3d.com/Packages/com.unity.timeline@1.6/manual/index.html) used to provide a character action that waits for a timeline to finish
- [Cinemachine](https://docs.unity3d.com/Packages/com.unity.cinemachine@2.8/manual/index.html) used in a helper that performs camera locking using two virtual cameras
- [Visual Scripting](https://docs.unity3d.com/Packages/com.unity.visualscripting@1.7/manual/index.html) for the custom visual scripting units

__AdventureManual__
- [Input System](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.4/manual/index.html) for the inputs
- [Universal RP](https://docs.unity3d.com/Packages/com.unity.render-pipelines.universal@12.1/manual/index.html) for the materials
- [Visual Scripting](https://docs.unity3d.com/Packages/com.unity.visualscripting@1.7/manual/index.html) used in visual scripting manual

__AdventureHero__
- [Input System](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.4/manual/index.html) for all the player inputs
- [Timeline](https://docs.unity3d.com/Packages/com.unity.timeline@1.6/manual/index.html) for the intros, scene transitions, chest action
- [Cinemachine](https://docs.unity3d.com/Packages/com.unity.cinemachine@2.8/manual/index.html) for the main camera
- [Universal RP](https://docs.unity3d.com/Packages/com.unity.render-pipelines.universal@12.1/manual/index.html)
- [ProBuilder](https://docs.unity3d.com/Packages/com.unity.probuilder@5.0/manual/index.html) used to build the levels
- [Visual Scripting](https://docs.unity3d.com/Packages/com.unity.visualscripting@1.7/manual/index.html) enemy behaviour, character actions, scene transitions, loot, idle animations
- [TextMesh Pro](https://docs.unity3d.com/Packages/com.unity.textmeshpro@3.0/manual/index.html) for the UI texts

__AdventureSouls__
- [Input System](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.4/manual/index.html) for all the player inputs
- [Timeline](https://docs.unity3d.com/Packages/com.unity.timeline@1.6/manual/index.html) for the intro and bonfire actions
- [Cinemachine](https://docs.unity3d.com/Packages/com.unity.cinemachine@2.8/manual/index.html) for the main camera
- [Universal RP](https://docs.unity3d.com/Packages/com.unity.render-pipelines.universal@12.1/manual/index.html)
- [ProBuilder](https://docs.unity3d.com/Packages/com.unity.probuilder@5.0/manual/index.html) used to build the environments
- [Visual Scripting](https://docs.unity3d.com/Packages/com.unity.visualscripting@1.7/manual/index.html) for some optional custom enemy behaviors

## Manual

This manual is meant to explain the concepts and ideas of AAK rather then any specific detailed API. For more detailed explanations of every class in the core framework and most of the demo please consult the code itself. I try to give a detailed explanation for the purpose of the class in the xml-doc of the class itself and explain every field of the behaviors in the tooltip.  

The manual pages for the core systems of AAK are always split into a Core part that explains the idea behind the system and a Souls part that covers how the system was used in that demo.

The Souls and Hero sections at the end of the manual contain some additional information about how the demo are set up and how they may be extended.

## Integrations

- [SceneConnector](https://github.com/Schossi/ConnectorSouls) multi scene management
- [Ink](https://github.com/Schossi/AAK_Ink) narrative scripting language
- [KinematicCharacterController](https://github.com/Schossi/AAK_KinematicCharacterController)

All GitHub repositories related to my unity assets can be found in the [Softleitner Extras](https://github.com/stars/Schossi/lists/softleitner-extras) list. I generally try to keep the main ones updated but some of the minor ones may be out of date.

## Roadmap

The next update will continue to polish and build out the Hero Demo and its Documentation.

## Feedback

The quickest channels to reach me are mail and discord. Please feel free to reach out with any problems and questions. Feedback regarding the general direction of AAK and particular future features are also always welcome. Though I might not immediately be able to incorporate your requests I very much take them into consideration when planning out future updates.  

If you can spare the time please consider leaving a review in the asset store.
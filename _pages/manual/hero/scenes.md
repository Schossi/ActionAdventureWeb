---
permalink: /manual/hero-scenes
title: "Hero Scenes"
sidebar:
  title: "Manual"
  nav: manual
---

## Title

The Title screen lets players select game slots to create new games or continue games they have started previously.

The __Background__ of the screen is made using a __CinemachineDollyCart__ that runs on a looping __CinemachineSmoothPath__.

The general flow of this screen is controlled from the __StateMachine__ found on the __Logic__ transform. It goes through the following states.
- __Black__ sprite fully blanks out screen 
- fade out __Black__ sprite to reveal the background
- fade in the __Logo__ sprite
  - wait for confirmation
- fade out the __Logo__ sprite and fade in the __Saves__ canvas
  - wait for game selection
- fade in __Black__ sprite
- load scene stored in __Scene__ variable by __HeroSaveSlot__

Save slots are found in __UI>Saves>Contents__ and use the __HeroSaveSlot__ prefab. They each have a minimal characer setup including a __PersistenceContainer__ and __GenericCharacter__ which loads data from the configured save index. The prefabs displaying health and money use that to load their data. You can check the data of each save slot from the editor using the "Edit Save Data" debug button in the inspector of the container.

The behaviour of each slot is defined in the __HeroSaveSlot__ visual script. When it starts it checks whether the __PersistenceContainer__ alread has any data and shows the __NewGame__ or __Load__ objects accordingly. When a game is started or loaded the script sets the __SaveSlot__ application variable which is used by the containers in the actual game scenes to retrieve the current index. It then sets the __Scene__ variable to the current scene of the save game or HeroBeach for new games and triggers the START event on the __Logic__ object which transitions that __StateMachine__ and ultimately loads the game scene.

Every game keeps two persisted strings that dictate where the player currently is. The string stored under __Scene__ simply contains the name of the scene that should be loaded. The string in __Parameter__ is meant to store how the scene should be entered, for example a number representing the entry point.

## Beach

This is the game scene players enter when they first start a game. It plays a short intro cutscene that shows the player character waking up and then hand control over to them. Over the course of the stage players climb a cliff to collect a sword and shield. The sword then lets them cut through some vines that block the path to the next stage. A heart shard is hidden in an optional little cave that is also blocked by vines.

When the scene is loaded a couple different things can happen. This is configured in the __ScriptMachine__ in the __Entry__ object. It checks if the intro has already been played using the __Beach_Intro__ persisted bool. If it has not it starts the __TimelineAction__ in __Intro__. Otherwise it checks the string persisted under __Parameter__. If a "1" is found it starts the __HeroEnter__ subgraph that moves the player in through the trunk leading to the inland scene. Otherwise it starts the __HeroWake__ __TimelineAction__ which quickly fades the screen in from black. This usually happens if the player has died.

All the environments in the hero demo were made using __ProBuilder__. They use the same __HeroDefault__ material as all the object and character models that were made externally using blender. To assign colors to individual surfaces the UVs of those surface have been collapsed together and moved to the wanted color in the __ProBuilder UV Editor__.

Cliff

Chest

Waves

Vines

Trunk

## Inland



## Canyon

## Temple

## Debugging

### General

### Loading

### Passage

### Skeleton

### Block

### Climbing

### Ground
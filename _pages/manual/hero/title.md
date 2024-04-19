---
permalink: /manual/hero-title
title: "Hero Title"
sidebar:
  title: "Manual"
  nav: manual
---

The Title screen lets players select game slots to create new games or continue games they have started previously.

<p align="center">
  <img src="/assets/images/hero/heroTitle.png" />
</p>

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/2AB8p22nCxY?si=vv9DEYz7YAJD5mCv&amp;start=970" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

The __Background__ of the screen is made using a __CinemachineDollyCart__ that runs on a looping __CinemachineSmoothPath__.

## Logic

The general flow of this screen is controlled from the __StateMachine__ found on the __Logic__ transform. It goes through the following states.
- __Black__ sprite fully blanks out screen 
- fade out __Black__ sprite to reveal the background
- fade in the __Logo__ sprite
  - wait for confirmation
- fade out the __Logo__ sprite and fade in the __Saves__ canvas
  - wait for game selection
- fade in __Black__ sprite
- load scene stored in __Scene__ variable by __HeroSaveSlot__

## Saves

Save slots are found in __UI>Saves>Contents__ and use the __HeroSaveSlot__ prefab. They each have a minimal character setup including a __PersistenceContainer__ and __GenericCharacter__ which loads data from the configured save index. The prefabs displaying health and money use that to load their data. You can check the data of each save slot from the editor using the "Edit Save Data" debug button in the inspector of the container.

The behaviour of each slot is defined in the __HeroSaveSlot__ visual script. When it starts it checks whether the __PersistenceContainer__ already has any data and shows the __NewGame__ or __Load__ objects accordingly. When a game is started or loaded the script sets the __SaveSlot__ application variable which is used by the containers in the actual game scenes to retrieve the current index. It then sets the __Scene__ variable to the current scene of the save game or HeroBeach for new games and triggers the START event on the __Logic__ object which transitions that __StateMachine__ and ultimately loads the game scene.

Every game keeps two persisted strings that dictate where the player currently is. The string stored under __Scene__ simply contains the name of the scene that should be loaded. The string in __Parameter__ is meant to store how the scene should be entered, for example a number representing the entry point.
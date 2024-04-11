---
permalink: /manual/hero-beach
title: "Hero Beach"
sidebar:
  title: "Manual"
  nav: manual
---

This is the game scene players enter when they first start a game. It plays a short intro cutscene that shows the player character waking up and then hand control over to them. Over the course of the stage players climb a cliff to collect a sword and shield. The sword then lets them cut through some vines that block the path to the next stage. A heart shard is hidden in an optional little cave that is also blocked by vines.

<p align="center">
  <img src="/assets/images/hero/heroBeach.png" />
</p>

## Entry

When the scene is loaded a couple different things can happen. This is configured in the __ScriptMachine__ in the __Entry__ object. It checks if the intro has already been played using the __Beach_Intro__ persisted bool. If it has not it starts the __TimelineAction__ in __Intro__. Otherwise it checks the string persisted under __Parameter__. If a "1" is found it starts the __HeroEnter__ subgraph that moves the player in through the trunk leading to the inland scene. Otherwise it starts the __HeroWake__ __TimelineAction__ which quickly fades the screen in from black. This usually happens if the player has died.

## Environment

All the environments in the hero demo were made using __ProBuilder__. They use the same __HeroDefault__ material as all the object and character models that were made externally using blender. To assign colors to individual surfaces the UVs of those surface have been collapsed together and moved to the wanted color in the __ProBuilder UV Editor__.

## Cliff

This part of the environment has been split into its own mesh. The gameobject has a __MeshCollider__ and __GenericTriggerItem__ so it can be detected by the __GenericTriggerArea__ of the __HeroClimbAction__ located in the player actor inside the [HeroSetup]({% link _pages/manual/hero/setup.md %}) prefab. The collider has some depth resulting in a semi pyramid kind of shape as flat surfaces result in some problems with how climbing works.

## Chest

Chests in the hero demo use the custom __HeroChestAction__ which extends the common __TimelineAction__. When it is started it moves the character to an exact spot in front of the chest defined in the __ObjectCharacterTarget__ and when it ends it adds items to the character as defined in the __Items__ field.

It also saves whether it has already been opened to the __Persister__. When the game starts and that persister is already set it moves the chest __Animator__ to its Opened state and destroys itself so it can't be used again.

It can be annoying to get both the chest and player model into place to test out the timeline in a regular scene. The __HeroDebuggingInteraction__ scene contains a non-playable version of the player chest interaction that makes this easier.

## Ambience

The __Forrest__ transform contains an __AudioSource__ with a looping track that contains some tweeting birds and general forest ambience. Its position and spacial parameters are set up so that the player only hears them as they get closer to the inland caves and so they increase in volume as they move into them.

In the opposite direction out in the water there are five different transforms(__WaveA-E__) for waves sounds. These are controlled by the __ScriptMachine__ found in __Waves__. The machine waits for a random duration between 1 and 5 seconds and then chooses one of the waves, varies its pitch, plays it and repeats. 

## Destructibles

Both the __HeroVines__ and __HeroSeaUrchins__ have colliders and a __DestructibleDamageReceiver__ which replaces them with their __DestroyedPrefab__ when hit by any kind of damage.

The __HeroVinesDestroyed__ prefab simply contains a sound and some particles which all play automatically when it is spawned. It gets destroyed after two seconds by its __FadeAndDestroy__ script.

__HeroSeaUrchinPickup__ also contains a __HeroHeartPickup__ which uses a __HeroPickupArea__ to give a __HeroHeart__ usable to any character that enters its area. The pickup sits under an __Animator__ which performs a little growing and floating animation. The entire object starts to blink after 10 seconds and then blinks for 3 before being destroyed as defined in the __FadeAndDestroy__ script in its root.

## Trunk

The __HeroExitTrunk__ found here is used to transition the player to the [HeroInland]({% link _pages/manual/hero/inland.md %}) scene. 

The __ScriptMachine__ in its root defines how it behaves. When the player enters its area it sets the game state to CUTSCENE, starts the Exit __TimelineAction__ and transitions the sound to the Muted snapshots. When that action is done it sets Scene and Parameter to whatever is defined in the variables. In this case the Scene is HeroInland and Parameter is 1 so the inland scene knows which entry the player is coming from. Finally it Saves the game and Loads the next scene.

In addition to the __Exit__ action it also contains an __Enter__ action which is triggered by the [Entry](#entry) if Parameter is 1 meaning the player enters the beach coming from the inland trunk. 

An isolated version of this behaviour with visible marching points and collision areas can be found in __HeroDebuggingLoading__.
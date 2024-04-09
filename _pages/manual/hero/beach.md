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

## Chest

## Waves

## Vines

## Trunk
---
permalink: /manual
title: "Overview"
sidebar:
  title: "Manual"
  nav: manual
---

## Welcome

This is the manual for Action Adventure Kit by SoftLeitner. Action Adventure Kit is quite the mouthful so i will be referring to it using the shorthand AAK.  

Action Adventures are a very wide, loose genre with all kinds of different games and sub genres. There are some things that many of them have in common though. Lots of them have characters that perform actions. These might have items that change hands and attributes that define their capabilities.  

AAK was made to provide a solid foundation for these common ideas within the genre so you can focus on what makes your game special.  

Since AAK goes for a broad base of functionality rather than something more specialized it generally tries to do anything in the most default, unity-built-in way possible. For example the movement in the souls demo uses the default unity character controller and the intro is done using timeline. To acommodate for the fact that more special systems may be required for movement, inventory, actions, ... AAK was made from the ground up with expandability in mind.

## Project Structure

AAK is separated into multiple projects.

* AdventureCore  
The Core Framework of AAK, always import this one.

* AdventureSouls  
Contains the [soulslike]({% link _pages/demos/demoSouls.md %}) demo, import if you want to start by adapting this demo.  

## Manual

This manual is meant to explain the concepts and ideas of AAK rather then any specific detailed api. For more detailed explanations of every class in the core framework and most of the demo please consult the code itself. I try to give a detailed explanation for the purpose of the class in the xml-doc of the class itself and explain every field of the behaviours in the tooltip.  

The manual pages for the core systems of AAK are always split into a Core part that explains the idea behind the system and a Souls part that covers how the system was used in that demo.

## Roadmap

At least the first update will focus on rounding out and polishing the current core functionality and the souls demo. After that i will most likely start on a second demo game(oot?, 2d?). Feel free to let me know if you have some features or game types that you'd like to see in the future.

## Feedback

The quickest channels to reach me are mail and discord. Please feel free to reach out with any problems and questions. Feedback regarding the general direction of AAK and particular future features are also always welcome. Though I might not immediately be able to incorporate your requests I very much take them into consideration when planning out future updates.  

If you can spare the time please consider leaving a review in the asset store.
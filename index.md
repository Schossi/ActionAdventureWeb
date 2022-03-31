---
layout: splash
header:
  image: /assets/images/BannerSmall.png
---

Action Adventure Kit aims to kickstart your next gaming project by providing a solid foundation for commonly used systems like characters, items or persistence. It is split into the AdventureCore framework and the AdventureSouls demo project which can be used to learn about all the systems or as a base to start developing from. 

<p align="center">
  <img src="/assets/images/AdventureCore.png" />
</p>

AdventureCore currently contains the following systems along some other handy utilities.

- Character (Player, Enemies, ...)  
act as the central component that brings together all the other systems of the character
- Actions (Object interaction, Attacks, Cutscenes, ...)  
anything a character can do that might occupy it for a while  
- Items (Keys, Armor, Potions, ...)  
objects that are put into an inventory and may be equipped or used
- Attributes (Vitality, Strength, ...)  
values that define a characters capabilities
- Resources (Health, Endurance, ...)  
values that may be used up by an action or damaged somehow
- Movement (CharacterController, NavMesh)  
responsible for moving the character around the world
- Persistence  
saves the state of characters, environment, save games, settings, ...
- Damage (Physical, ...)
damage events created from triggers or actions are passed from senders to receivers  

<p align="center">
  <img src="/assets/images/Title1080.png" />
</p>

AdventureSouls demonstrates the systems of AdventureCore in a simple souls-like ARPG. Some of its feature highlights are:  

- Player Character
  - weapons with unique actions per item and equipped slot(left, right) 
  - parry, guard break, stagger behavior  
  - reusable humanoid animations
- Enemies
  - passive or aggressive zombie enemies
  - boss with arena and attacks based on player position
- Souls mechanics
  - level up at bonfires but partially reset world
  - death drops XP and resets player to bonfire
- Objects
  - doors, chests, levers
  - destructible chests and dummies
  - traps that shoot spikes

<iframe width="560" height="315" src="https://www.youtube.com/embed/KigI2D7rjyc" title="AdventureSouls Demo" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>  
<br/>
Action Adventure Kit takes full advantage of the modern Unity ecosystem and tries to use the default Unity solution whenever possible. In this spirit of not reinventing the wheel it uses the following packages.  

- New Input System
- UI Toolkit
- URP
- Cinemachine
- Timeline
- ProBuilder
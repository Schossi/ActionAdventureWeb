---
permalink: /manual/souls-npcs
title: "Souls NPCs"
sidebar:
  title: "Manual"
  nav: manual
---

The following is an overview of the different prefabs used for NPCs in the [AdventureSouls]({% link _pages/demos/demoSouls.md %}) demo.

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/Ozp06rPjKhc?si=6lLpwHS51cOqTSzV" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

The different character script used in souls all share some common behavior which is implemented in the following inheritance tree.

- SoulsCharacterBase  
contains some very general logic like defense calculation, staggering and death
  - __SoulsPlayerCharacter__  
   adds player specific behavior like input, lock-on and special actions
  - __SoulsNonPlayerCharacter__  
  base for NPCs in the general game world including some very general behavior  
  adds a state(idle, attacking, loot, dead) to the character which is also persisted
    - __SoulsFriendCharacter__(Trader, Talker)  
    are not aggressive at the start but can be aggroed by attacking
    - __SoulsEnemyCharacter__(Zombies, Sprout)  
    immediately attack when the player enters their trigger area
  - __SoulsMorningstar__  
  performs different actions based on player location and energy
  - __SoulsHeroApe__  
  performs action sequences based on an energy counter

The object hierarchy of NPCs is very similar to the one of the player as explained on the [player]({% link _pages/manual/souls/player.md %}) manual page. Most characters in AAK share this structure which looks more or less like the following.

- __Root__ - character, movement, persistence 
  - __Model__ - visual model
  - __Actor__ - actor, actions
  - __Metrics__ - resources, attributes, effects
  - sounds, ...

The objects within the enemy prefabs are on the Enemy, EnemyBody and EnemyDamage layers. For details on how these work see the [damage]({% link _pages/manual/content/damage.md %}) manual page.

## SoulsZombie

<p align="center">
  <img src="/assets/images/souls/soulsZombie.png" />
</p>

This is the most common enemy in the souls demo. It can be customized by changing up the weapon it holds or the attack it uses and by changing its idle action. You can find a couple different variations of the basic zombie enemy in the SoulsDungeon stage and the SoulsDebuggingEnemies debug scene. 

Basic zombies do not have an inventory of their own. You can give them a weapon by just dragging a weapon prefab into the WeaponRight/Left objects and assigning the attack action on the character. Don't forget to adjust the layer of the damage object of the weapon accordingly.

The Idle action determines what a zombie does when it is not attacking. SoulsDebuggingEnemies and the shrine stage contains a variant that patrols between points using a NavPatrolAction. SoulsDebuggingEnemies also has a variant that returns to a home point and then sits down by using a NavApproachAction with a PoseAction in its Next field.

SoulsZombieScripted is a slightly more advanced base for zombie enemies that have their behavior controlled by visual scripting. They have an inventory to store weapons and usable items. Getting the weapons on the correct layer is achieved using the LayerOffset field on the weapon slots which shifts the layer by 5 from the player to the enemy layers.

The SoulsDebuggingVisualScripting debug scene contains two implementation of the scripted advanced zombie enemy. Theses are not used in the regular demo stages. Check out the SoulsArcher and SoulsAttacker script graphs to see how they work. The character scripts of these have their IsExternallyScripted toggle checked which keeps the character from starting actions on their own.

## SoulsSprout

<p align="center">
  <img src="/assets/images/souls/soulsSprout.png" />
</p>

This is a very basic example for an enemy that is more of a creature rather than a humanoid enemy. Otherwise it works very similarly to the basic zombie enemy and also uses the SoulsEnemyCharacter script. It has a single attack action that activates a spherical damage sender which sends ticks of SoulsPoisoningDamage.

## SoulsFriend

<p align="center">
  <img src="/assets/images/souls/soulsFriend.png" />
</p>

SoulsFriend is used as a basis for the SoulsTalker and SoulsTrader prefab variants an not usable on its own. The variants add different interaction actions and visual models to the prefab.

The Interaction object in the base prefab already contains a trigger collider that is meant to trigger the CharacterActionArea on the player character. The different variants add whatever action they need here, for example talking or trading. As you can see interacting with an NPC works very similarly to interacting with objects like doors. Since the executing character of the action is the player and not the NPC the actions may need to reference the NPC character in an extra field. For example the SoulsTalkAction has the Target field which it uses to make the NPC nod on action start.

These characters use the SoulsFriendCharacter script which has an internal aggro counter. On the first hit the character tries to equip whatever fitting weapon it has in its inventory to its right weapon slot. On the second hit it starts attacking back. This deactivates the IdleObjects object which contains the trading/talking action. It also activates the AggroObject which contains a lock-on point.

## SoulsMorningstarBoss

<p align="center">
  <img src="/assets/images/souls/soulsMorningstar.png" />
</p>

In addition to the character itself this prefab also contains the logic of the boss arena. For more information about the boss arena see the boss arena chapter in the [scenes]({% link _pages/manual/souls/scenes.md %}) manual page.

The character itself if fairly basic. It uses a static model without any deforming animations. All the attacks are done using timeline graphs. The position of the player is incorporated by moving the AttackTarget transform to the players position before starting attacks. The character subscribes to its actors ActionEnded event and starts a new action whenever the last one ends.

The morning star has an internal tiredness counter which makes it wait after performing a couple attacks. A simple Spin or Smash only costs it 1 tiredness and a Stomp from a distance costs 2. When it reaches 3 it has to use the Wait action which gives the player a chance to counter attack.

For another variant of a souls boss fight with a slightly different approach to its scripting see the souls hero ape boss fight in the [extras]({% link _pages/manual/other/extras.md %}) project. 
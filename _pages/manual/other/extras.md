---
permalink: /manual/extras
title: "Extras"
sidebar:
  title: "Manual"
  nav: manual
---

The AdventureExtra project in AAK contains different examples that may use assets from all the other projects. Therefore to try one of these be sure to always import the entire asset.

## Arena

Arena is a minigame that combines assets from all the other projects to create something new. Players fight enemies in increasingly difficult stages and upgrade their character in a shop between stages. The options in the shop are randomly selected to provide some variation between runs. The game is playable [here]({% link _pages/demos/demoArena.md %}).

<p align="center">
  <img src="/assets/images/other/arenaTitle.png" />
</p>

Persistence is done a little differently from other demos. Loading is only done in the title screen and Saving is only done whenever a player beats a stage. Data like player equipment and health is kept between scenes using the new Buffer toggle on the PersistenceContainer. This also makes debugging scenes more convenient as disabling loading to start fresh like in souls or hero is not needed.

<p align="center">
  <img src="/assets/images/other/arenaStage.png" />
</p>

The enemies spawned in the stages can easily be configured in the inspector. Their behavior is defined in simple visual graphs. Currently this includes a simple melee enemy that runs up and attacks, a ranged enemy that runs away if the player gets too close and another variation of the ape boss. When all enemies are beat the stage awards bonus points for each second under par time and then loads the shop.

<p align="center">
  <img src="/assets/images/other/arenaShop.png" />
</p>

In the shop players can exchange the essence they earned in the stages for useful upgrades. The items offered here include:
- Weapons Equipment Items that each have on attack action
  - Melee weapons like sword and morning star
  - Ranged weapons like sling and shortbow
- Usable items that are removed when used  
these have to be equipped to one of the 3 usable slots
  - shielder and booster add temporary effects
  - potion heals damage
  - bombs does aoe damage
- Attribute upgrades(check attributes in pause screen)
  - Strength gives bonus damage
  - Endurance increases max stamina
  - Vitality increases max health
- Trinkets that are instantiated on the player and add an effect
  - Skeleaton Skull for bonus damage
  - Aromativ Herb for stamina recovers
  - ...

Update 1.9.1 added __twin stick__ controls to arena, to change this back to regular walking simply remove or unbind the __Rotate__ action in the inputs.

## HeroSouls

This example demonstrates how assets from the souls demo may be used in the hero demo. The scene was made by duplicating the SoulsDebuggingGeneral scene and replacing the SoulsSetup with HeroSetup. Other changes made are detailed below. A big part of making actions reusable between demos is the PlayableAnimation which is described on the [utilities]({% link _pages/manual/other/utilities.md %}) manual page.

<p align="center">
  <img src="/assets/images/other/heroSouls.png" />
</p>

### HeroSetup

Some changes have been made to the setup to work better with souls assets. The changes are also easy to discover by checking the HeroSetup Overrides in the inspector.

- scaled the character model up to 1.2 to fit better with the souls assets
- set the key of the generic trigger item on the player to __PLY__ which identifies the player in the souls demo(for example in the moving platform)
- increased the size of the lock on collider because lock on distance is determined by the player area in souls
- added MessageEvents SHEATHEA and SHEATHEI which can be used to sheathe items from non hero actions
- added a ResourceBarManager to the UI so enemy health bars are shown
- added ItemNotifications so items picked up are shown(souls items don't have the hero pickup dialog)
- added gear and equipment buttons in the UI for the items brought over from souls(sword, shield, flask, bow)
- added the HeroSouls items to the inventory so they can be tested in the scene

### Gear

Both __Sword__ and __Shield__ are mostly identical to their counterparts in the hero demo. The prefabs just use different models and sounds. Because the attack animations are not determined by the gear in the hero demo these also don't change.(if you want to look into individual animations by sword the HeroCharacter state graph would be the place to start looking as this is where the attack actions are started)

### Equipment

The actions in the flask and bow items are wrapped in a HeroHoldableAction which takes care of visualizing the item in the characters hands. It forwards any input it receives to the actual action. 

The __flask__ uses a regular __MotionAction__ instead of the souls action which contains some souls specific logic. Filling the resource, removing the item and adding the healing effect on use is done in a event handler for the ACT message of the animation.

A couple additional fields have been added to the __SoulsShootAction__ on the __bow__ to make it usable without the arrow slot it uses in the souls demo. The projectile that is linked there is also custom made for HeroSouls so it damages __HeroHealth__ instead of souls resources. 

### Scene

The ladders in the scene contain two additional instructions to make the work better with the hero character. The MovementMotionMultiplier increases the movement from root motion to make the hero character match its movement to the ladder. The HeroActionOverride sets the ladder as the current context action which allows the player to leave the ladder by pressing E.

The dummies in the scene use the HeroHealth resource instead of the souls one so they can be damages by hero damage sources like bombs.

The key needed to open the door has been replaced by the key from the hero demo so it shows up in the hero UI.

In the door right in front that does not require a key a SHEATHEA message is sent on start which makes the hero put away its items.

## SoulsHero

As the name indicates this is the inverse of the HeroSouls example. Here assets from the hero demo are used by the souls character. Once again changes made are detailed below

<p align="center">
  <img src="/assets/images/other/soulsHero.png" />
</p>

### SoulsSetup

- scaled the character model down to 0.8 to fit better with the hero assets
- set SortByDistance on the action area so the character picks up closer (for example)pots first
- added OverheadItem transform as replacement for the ItemParentOverhead in the hero demo
- added the Climb action so the character can climb walls in the hero demo
the climb action is also added to the RecoveryActions array on the Stamina resource so the character recovers stamina while climbing, otherwise they would not be able to jump off if they enter climbing on low stamina
- added the SoulsHero items to the inventory so they can be tested in the scene
- added ItemNotifications so items picked up are shown(pickups don't have the souls pickup dialog)

### Weapons

Hero __Sword__ and __Shield__ are very similar to their counterparts in the souls demo. In addition to the models the animations from hero are also used here. This was achieved by creating a custom SoulsHeroAttacks animator controller that replaces SoulsAttacks.(for example in the Light attack action in the sword)

### Usables

The __bombs__ from hero are available as a __SoulsUsableItem__. One essential change made are the additional SuspendMovementControl instructions in the Pick, Put and Throw playable animations. Since the character in souls does not use root motion for its movement it would otherwise keep moving during those animations. The carry actions of the bombs and pots lying around the scene have been changed in the same way.

The __nut__ item uses a regular __SoulsUsableAction__ pretty much identical to the pebble in the souls demo. It still spawns the HeroNotImpact on hit though which can be used to stun enemies.

SoulsHero also contains some changed version of the heart and money items in hero. These have been modified to add attributes and resources like SoulsExperience and SoulsHealth instead.

### Scene

All the different pickups have been modified to add appropriate items. For example the heart essences use SoulsHeroHeartEssence which adds SoulsVitality that increases the souls characters health.

The HeroBlock has had some changes to the grab actions. Holding and Releasing the grab does not work in the souls demo and therefore the __InputType__ is changed to __Perform__. To keep other actions from being started while the character is grabbing the block a SoulsSuspendActing instructions is added which disables the Act input. 

## SoulsHeroApe

This scene contains the Ape boss fight from the hero demo but adjusted to fit in with the souls demo. The souls setup is left unchanged. The ape boss is placed in a souls boss arena, its logic is controlled by a custom character script instead of visual scripting. When defeated it spawns a ragdoll instead of playing a cutscene.

<p align="center">
  <img src="/assets/images/other/soulsHeroApe.png" />
</p>

Just like the morning star boss the SoulsHeroApe character script inherits from SoulsCharacterBase for some common functionality like defense calculation. It implements its own guard break logic which lets players parry the left and right punch attacks. There is no critical follow up attack, instead the ape has its energy reset which makes it take a rest action that lets players heal or attack.

The behavior of the ape is done using different IEnumerable sequences of actions. Whenever the apes actor goes idle and the current sequence contains no more actions it selects a new one. If its energy is low is can select a rest or wag action to recover some energy. Otherwise it will randomly select an attack action which will reduce the energy counter.

Ragdoll creation for the ape differs from regular characters because its model is scaled. For the joints of the ragdoll to work properly in this case it has to be disabled before the scale it applies. This is done be creating a temporary disabled gameobject the ragdoll is instantiated inside of at first.

Since there are no bombs in the souls demo all the breakable models and triggers of the ape have been removed. The triggers and colliders have also been changed since the souls demo uses different layers. The health and damage assets have also been replaced by the ones from the souls demo.  

Death will not work properly in this scene because it does not have a Temp scene that is usually used to reset enemies. Therefore the enemies and the boss will just keep attacking after death.
---
permalink: /manual/souls-scenes
title: "Souls Scenes"
sidebar:
  title: "Manual"
  nav: manual
---

## Title

The Title scene is where the player should first arrive, this is the case as long as it is at the first place in the build settings. It is where the player can create  new games or load existing ones.  

<p align="center">
  <img src="/assets/images/souls/soulsTitle.png" />
</p>

- __Title__  
UI for the different save slots that are used to create new games and load them
- __NewGame__  
Dialog that is used to customize and name new characters  
the model and camera that are used for the render texture in the UI are its children
- __Commons__  
Scripts that are common to every scene like persistence and the SoulsCommons singleton
- __Background__  
Holds all the 3d models in the background which are purely cosmetic

## Loading

<p align="center">
  <img src="/assets/images/souls/soulsLoading.png" />
</p>

This scene gets loaded when a player loads a save from the title or when using a teleport action. The __SoulsLoading__ script starts loading the target main scene asynchronously. After that it can be configured to wait for player confirmation by setting a UIDocument or to wait for a playable director if that is set. When those are done it additively loads the Temp scene and activates the loaded Main scene.

In the demo loading is always done pretty much immediately because it uses very little assets. The scene is meant as an example for games that grow large enough to warrant it. If you don't need it simply use SwitchScene instead of LoadScene from the title screen and empty the loading scene parameter in teleport actions.

As mentioned above the actual levels in the demo are split into a main and a temp scene. This is done to simplify the reset process when the player sits down at a bonfire which reloads the temp scene. Therefore the environment, bonfires, managers, lighting and things like that belong in main while enemies and destructibles should be placed in temp. It may make sense to put bigger assets from the temp scene into main too just so it is part of the loading process.

## Dungeon

<p align="center">
  <img src="/assets/images/souls/soulsDungeon.png" />
</p>

The dungeon stage, in classic games fashion, starts the player off in a prison cell. After freeing themselves by using a key that has dropped into the cell in the intro they collect some basic equipment while going through the stage and finally defeat a boss which unlocks a teleport to the next stage.

The __SoulsDungeon__ script on the Logic object is meant to hold any custom logic for this scene. It uses the persister assigned to it to check if the intro has been played already or if this is the first time the player enters the dungeon. If so it plays the intro and sets the players reset point to the cell.

The intro is a simple __TimelineAction__ which suspends player control and the HUD using character instructions.

### Lower

First off the player can pick up the key that dropped into the cell in the intro. The pickup has a __PickupAction__ which can be found by the players __CharacterActionArea__ because of the trigger collider on the same object. The model and particles of the pickup are disabled in the actions started event. The item the player gets when interacting is set in the actions Items field.

The door that the key unlocks has an __ObjectAction__ which has the key set in its Cost field. The action has trigger names set for the character(OpenDoor) and the object(Open) which are set on the respective animators when the action is started. It also has the name of the state(Opened) after the action so it can restore it if the game is reloaded.

In the following corridor the player encounters the first enemies. These will not attack on their own because the trigger area which would aggro them has been disabled and unset on the character. They will still attack when hit however but otherwise they just remain in their idle action.

The ladder at the end of the corridor uses the special __SoulsLadderAction__ which responds to up and down input to move the player up or down the ladder. To know if the character started the action from the bottom or top there are __SoulsLadderEnterAction__ at both ends that lead into the ladder action. The actual movement on the ladder is actually done by the root motion of the characters animation. In addition to hiding the weapons and suspending movement the ladder action also has a __SuspendMovementCollision__ instruction which prevents the character from colliding with the ladder during the action.

### Courtyard

At the top of the stairs the player finds a chest which works very similarly to a door. The only differences are that this chest does not have a cost and activates its content, which contains a pickup, when used. The LeatherBottom inside the chest can be equipped to increase defense(reduces damage) and poise(harder to stagger). It does this because these stats have been set on the __SoulsArmorItem__ which acts as an IStatModifier.

Another pickup in the courtyard contains the __SoulsFlask__ item which restores the health resource when used and is refilled when the player rests at a bonfire.

The bonfire in the center of the yard contains a very special action that uses three different playables to enter, sit at and leave the bonfire. The binding of director to the characters animator is done at the start of the action so that the object does not need a direct link to the player. When the enter playable is done the bonfire resets the world and player and shows the level up menu.

The crates on the yard have a __DestructibleDamageReceiver__ which destroy the object when it takes damage and replaces it with the destroyed prefab. When the crate is destroyed the __DestructionPersister__ persists that and when the scene is loaded again after quitting the game it immediately destroys the object to restore the previous state. This is different from the player sitting at a bonfire which clears the persistence area that contains the crate and reloads the scene which makes the crate show up again.

Finally there are two doors at the end of the yard. These can only be opened from one side simply because the trigger collider of the action is on one side of the door.

### Hallways

The end of the western hallway contains several traps which periodically spawn a projectile that contains a damage sender that deals physical and poise damage. If the player were to simply walk into the hallway these would damage them and stop their movement due to the stagger caused by the poise damage. The shield found in the chest has a guard action which, while active, nullifies physical damage and turns poise damage into stamina damage. 

The lever in the northern hallways, just like doors and chests, uses an ObjectAction. The only difference is that it incorporates an additional animator(Gate) which gets triggered at the end of the action.

The western hallways contain two more zombies which do have their trigger areas set up and will attack once the player gets too close. One detail to note here is that these two carry sword despite enemies not really having an inventory. The swords have just been dragged onto their hands in the editor. The layers of the weapon has also been changed to EnemyBody/EnemyDamage. The light or heavy attack action has then been set on the enemy character to make it use that action when attacking.

### Boss Arena

After using the lever located in the northern hallways the gate has dropped and the boss arena is accessible.

There is a box collider located a bit into the area that start the fight by calling StartFight on the __SoulsBossArena__ script which does a couple things.

- Persistence  
First off it sets the player movements position to outside the arena and suspends persistence completely. This makes it so that quitting and reloading the game during the fight resets the player to outside the arena
- Boss Character  
Sets the target to the player and starts the configured action
- UI  
Shows the big boss HP bar
- Fog  
Activates the fog at the gate so the player can't leave during the fight

The boss character itself has a couple attacks which are all defined by timelines. Which ones it uses depends on the players location and each one uses a certain amount of energy. When the energy is used up the boss uses the wait action which gives the player a chance to deal damage.

When the boss is defeated the boss area resumes persistence and sets a flag so the state can be restored later. It also activates the teleporter that can be used to move to the shrine stage.

## Shrine

<p align="center">
  <img src="/assets/images/souls/soulsShrine.png" />
</p>

### Bottom

There are two friendly NPCs at the bottom of the shrine stage. What each one does is defined in the action in its Interaction object.

SoulsTalker has a __SoulsTalkAction__ which will display the configured lines of dialog in the general message box when used.

SoulsTrader has a __SoulsTradeAction__ that opens the trading dialog that lets the player exchange experience for the items configured there. This action uses a different persister than the main character which has the permanent persistence area set. Therefore when the player sits at a bonfire the NPC will reset but items that have been bought will still be missing.

The trader sells moss which can be used to reduce the poison resource and boosters which double the characters strength.

### Levels

On the first level the player encounters a couple sprout enemies. These will approach the player and attack by activating a spherical damage sender that deals poisoning damage. Just like fists or swords this attack still uses a __SoulsAttackAction__ but instead of setting a weapon that deals the damage the activation of the damage and particles is done by creating events for the DMG_ON and DMG_OFF message in the inspector. Another difference from enemies previously encountered is that these have GoHome as their idle action which will make them return to their home if they loose aggro. When they have arrived at their home point they will continue with the action configured in Next so this is where idle poses and such can go.

The second level holds a single zombie enemy which has a PatrolAction as its idle. This action makes it move between the configured transforms. Which one it is currently moving towards is persisted so it will keep its current path even when the game is reloaded.

### Top

The top of the shrine stage lets the player activate an elevator to the bottom by walking on a pressure plate.

The main script at work here is __AnimationToggler__ which is used to toggle an animator between two states and persist which one it is at. The current position of the elevator is persisted to Shrine_Elevator_Down.

When the elevator starts moving it activates a CharacterCarrierArea which reparents the player to itself and suspends its movement persistence. It also writes to Shrine_Elevator_Enabled which deactivates the NPCs at the bottom and activates the ones on the top on the next reload.

The NPCs at the Top use the same persistence key as the ones at the bottom so their values will carry over. The only exception is movement which uses a different persister on the ones at the top so the position does no carry over.

Finally the top also contains a teleported that leads back to the dungeon stage. This teleporter has TeleportTarget set so the player will be moved to that transform in the dungeon stage.
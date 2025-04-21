---
permalink: /manual/releases
title: "Release Notes"
sidebar:
  title: "Manual"
  nav: manual
---

## 1.9.0

### ADDED
- __Arena Demo__  
new roguelike mini game added to extras project  
beat increasingly difficult stages of enemies to earn essence currency  
spend essence in shop between stages on a set of randomly selected items
- DamageModifier for changing sent and received damage  
available on characters, trigger damage senders and receivers  
can also be applied by instructions and may use stats for their value  
used in arena demo to grant bonus damage based on character strength
- DamageMessage for triggering events and sending messages  
allow filtering based on damage kind and value  
available on characters, trigger damage senders and receivers  
used in arena to only send hurt message when affected by health damage
- ActionItemSlot for instantiating items with one action  
action can be started by binding input to slot, used for weapon slot in arena demo
- UsableItemSlot for usable items that may be instantiated during action   
item can be used by binding input to slot, used for quick slots in arena demo
- PurchaseAction for exchanging items, attributes, resources for items(arena shops)
- AnimatorMovementSetter for setting movement parameters on animator
- instructions for min and max movement speed  
min speed is used to give roll in arena forward speed when standing still
- Fader component for easily fading camera and sound in and out(arena scene transition)

### IMPROVED
- SuspendMovementInstruction can be set to still ApplyPhysics  
used in arena, prevents enemies from clipping into player while performing attacks
- PersistenceContainer can now buffer data between scenes  
used in arena so data is only loaded in title and only saved on stage win
- ProjectileDamageSender now has methods to directly instantiate  
used in arena demo for sling and bow projectiles
- LockOnManager additional setting CalculateCenter  
needed when the camera is not centered on the character
- CharacterAssociation can be set to search character automatically
- RootMotion can be disabled on CharacterControllerMovement
- RootMotion can be enabled on NavMeshAgentMovement
- ListedInventoryPanel filter by ItemSet
- PlayableAnimation with controllers can now also define Start- and EndTime
- hover over character message('Last 20 Messages') in inspector for stack trace
- InputBindingText can now define Overrides based on control scheme  
used in arena to display WASD for Move composite action input

### CHANGED
- character initializes movement, inventory and pools in its Awake  
this ensures everything is ready before any Start is called
- damage direction SenderToReceiver now calculates direction from character to character  
additional mode SenderTransformToReceiver goes sender transform to receiver character
- minor rework of NavMeshAgentMovement Approach methods  
can now also approach a Vector3 target  
new NavAwayAction makes enemies run away from target
- SphereDamageSender only sends damage once per character  
would previously be sent once for each collider regardless of character
- CharacterControllerMovement calculates SpeedSideways based on target direction

### FIXED
- MotionAction going into release state with Release unchecked
- wrong message in object action preview
- playable animation preview not resetting properly
- SoulsHeroApe ragdoll being pushed properly by lethal damage
- playable animation not working when started in Next
- temp resource bars not being properly removed on character death
- added default color animation to hero animator to properly reset color after damage

## 1.8.0

### ADDED
- Ape Boss Fight  
  - new scene called HeroGlade in Hero Demo  
  scene is entered from the temple and the new end point of the demo  
  intro and death cutscenes, various attacks, destructible rock armor  
  state machine based attack logic can easily be tested in HeroDebuggingApe
  - souls variant of the fight available in Extras/SoulsHeroApe  
  fully code based logic, new approach using randomly selected action enumerables
  - uses new NavMeshTankMovement that first rotates and then moves forward
- ImpactItem and ImpactArea that spawn different particles and sounds when colliding  
  - swords in hero demo now have an item, destructible rocks an area(ROCK)
  - longsword and fists in souls have an item
- additional debug scenes in hero demo demonstrating features not otherwise used
  - HeroDebuggingProjectile showcases deflectable projectiles 
  - HeroDebuggingParcour makes the player auto-jump and pull up without climbing
- About button in the scene view opens relevant documentation for the current scene 

### IMPROVED
- hero and souls guard actions now guard directionally  
angle can be configured on SoulsGuardAction and HeroShieldWeapon item  
damage is guarded when its direction is counter the shield direction  
it is also guarded when the sender is a character in front of the receiver
- hitstop of 0.1s for melee weapons in hero and souls demos  
message is sent from damage senders in weapons to message event in character
- Idling event on actors that fires when the actor runs out of actions
- additional effects in hero demo  
sounds and particles on guard(see HeroDebuggingProjectile)
dust particles when stepping on dirt or sand(see HeroDebuggingGround)
- LockOnManager can be configured to Cycle  
used in ape boss fight because the points are vertical not horizontal
- VisualScriptingHelper can be used to fire custom events from unity events  
used in ape boss fight to send RAGE event when mask is damaged
- OverruleCanStartMode in actors changes when CanStart prevents actions
- expanded souls demo documentation and full souls demo video
- additional xml comments and class summaries

### CHANGED
- PlayableAnimation runs in GameTime update mode by default  
new Unscaled toggle makes it run in UnscaledGameTime
- presenting items in hero now pauses the game using PauseGameInstruction  
consequently the following adjustments were made to other parts of the demo
  - chest action starts sheathing animation in unscaled update mode
  - cinemachine brain in hero now runs unscaled
  - timelines used in item presentation now run unscaled
- replaced more built in materials with dedicated ones for better pipeline switching
- SphereDamageSender now only collides with triggers
- CharacterController.Fallen event is also fired upwards and renamed to Landing
- hero pull up animation end message changed from END to CLIMB_END
- damage sender SendOnce setting only sends damage once per character  
in addition to once per receiver, needed as ape boss has multiple receivers

### FIXED
- pose action not working with identical start and end messages
- exception in dialogs during edit mode
- warnings in souls demo during character death
- ui navigation not working in unity 2021 for some dialogs and souls buttons

## 1.7.3

### ADDED
- basic UI for items and inventory(see ManualItem scene)

### CHANGED
- reorganized UI scripts into separate folder
- souls attack suspend movement on start messages instead of dmg
- moved start messages of punches back to dmg

### IMPROVED
- minor inventory and item rework
  - add and remove can be done partially and return remaining
  - listed inventory exposes observable collection
  - additional xml docs
- separate game state for hero game over
- separate field for hit format in resource bars 
- item notifications now support legacy texts

### FIXED
- unfocusing dialogs by clicking in hero
- getting stuck in hero game over screen
- broken barrier collider in hero beach 
- lowered damage modifier on shield punch to 2 instead of 10

## 1.7.2

### IMPROVED
- reworked bars for resources and new bars for effects  
used in the ManualResource scene and the souls demo(+transition for boss health bar)
- cinemachine 3 compatibility(v2 is still used by default)  
to upgrade check 'About Cinemachine Version 3' in the Manual Overview page
- resource values now have a Minimum field  
souls Stamina and Poise have a negative minimum so lag is longer for big attacks

### CHANGED
- messages on core actions(motion, object, ...) are now configured in the inspector  
these(START, ACT, END, ...) used to be defined as const in code  
the roll action in souls now uses ROLL_START instead of START which -  
prevents overlaps with other actions since roll in souls can release early
- turn animation in souls demo no longer stops movement making the player more agile
- attacks in souls demo now send START later to avoid overlaps with other animations

### FIXED
- masked clips did not work in playable animations

## 1.7.1

### FIXED
- issue in input binding texts when enable disable was called repeatedly
- input binding texts not switching to gamepad in hero title
- texts sometimes not completely finishing in timeline dialogs
- wrong loading scene name in souls demo when switching stages on death

## 1.7.0

### IMPROVED

- Dialog Overhaul  
dialogs are now used across both demos and also replace the timeline textbox
character delay, options buttons, text arrays, event system focus, ...  
check out VisualScriptinDialog/Conversation and TimelineDialog scenes in AdventureCore.Tests
more complex dialog in SoulsDebuggingNPCs, readable sign on floor in SoulsDebuggingGeneral
- Playable Animation Preview  
playable animations can be previewed directly in the parameter inspector
some actions(button, object, ...) can show preview across objects(player and door for example)  
check it out in the PlayableAnimationTest scene and demo scenes like SoulsDebuggingInteraction
- Game Focus and Cursor Lock handling unified in FocusHelper  
improves focus handling especially in the editor
- Predefined Searches for Characters and Managers(Ctrl+K > see left bar)
- StateManager, AudioManager and Dialogs sections in the manual Utilities page
- Hero Demo UI polish
- Unity 6 compatibility 
- various minor fixes

## 1.6.0

### ADDED

- [PlayableAnimation]({% link _pages/manual/other/utilities.md %})  
play animations on top of an animator using the playables api  
this enables configuring animations directly on character actions  
see the PlayableAnimationTest test scene for simple examples
- [HeroSouls and SoulsHero]({% link _pages/manual/other/extras.md %})  
example scenes that show how assets from one demo may be used in the other  
makes heavy use of PlayableAnimation, found in AdventureExtras
- ItemNotifications  
UI behaviour that shows new items which is used in ManualItem and the new extras
- CharacterActor custom Editor  
inspector shows active actions and allows ending them in play mode

### IMPROVED

- HelpUrl  
clicking the ? button on an AAK behaviour will now open the API documentation
- Unity 6 compatibility  
  - added default lighting data for debug scenes
  - velocity changed to linearVelocity for Unity 6 and above

### CHANGED

- Action.EndAction only calls the actor when IsHappening is true
- MinimalCharacterActor checks Action.CanStart

### FIXED

- TriggerArea firing twice when destroyed during enter with multiple colliders

## 1.5.3

Additions to Documentation and minor fixes

### ADDED

- HeroDemo manual pages(written + video)
  - detailed HeroSetup prefab explanation
  - individual description for each scene
- title screen background sound

### FIXED

- money visualizer in title screen
- camera damping when switching LockOn
- woosh sound missing audio group
- removed obsolete nodes in canyon script machine

## 1.5.2

Hero demo audio and UI improvements

### ADDED

- HeroDemo
  - sound effects for most actions
  - ambience sounds
  - temple and combat music
  - different steps by ground type
  - sound sliders and quality dropdown
  - improved sprites for buttons and items
  - ...
- AudioManager
  - crossfading music tracks
  - visual scripting units
  - global manager accessibility
- GroundChecker that checks for material and color

### IMPROVED

- PlayerPrefSaver debug data by key, stays between scenes
- CharacterActionArea setting SortByDistance 

## 1.5.1

small hotfix for some smaller issues in the new demo

### ADDED 

- SphereDamageSender that sends damages using a SphereOverlap  
used for bombs so damage only gets sent at the moment of impact

### FIXED

- exception when using bomb from menu outside of debug scenes
- jump attacks only sent damage in the debug scenes
- locking on without a target would sometimes unlock itself
- duplicate persistence keys for chests in temple
- sling projectile now uses dynamic collision detection to avoid passing
- minor adjustments to level geometries

## 1.5.0

official release of the 'Hero' demo

### ADDED

- HeroDemo
  - Beach, Inland, Canyon and Temple levels
  - Wall Climbing
  - Block Pushing
  - Slingshot Gear Item
  - Skeleton Enemy
  - Various Passage(Door) Prefabs  
  Scene Exit, Locked, Barred, Arena, ...
  - Sword and Shield Items
  - Destructible Vines, Walls, Bushes
  - Random Loot Selection for Pots
  - Readable Signs
  - ...
- Visual Scripting Units
  - PlayAnimation
  - Ragdoll
  - VisualScriptingAction Bool/Int/Vector Received
  - Activate/Deactivate Damage
  - DamagedTriggerTotal/DamagedCharacterTotal
  - EffectAdded/Removed
  - Set/Reset/Override ManagerState
  - Manager State Changed/Entered/...
  - HasItem
  - SetDialogResult
  - AlignToPosition
  - ResetInput/Momentum
- TimedAction  
simple action that ends after a set duration
- GenericDamage damage type  
does nothing on its own, handling in Receivers

### IMPROVED

- TriggerArea handles overlapping colliders better
- TriggerDamageSender setting SendOnce  
only sending damage the first time a receiver is added per activation  
can be useful to avoid double damage from things like sword slashes
- MotionAction setting OnlyGrounded  
disables the action when the character is not grounded
- Gravity for ProjectileDamageSender
- ResetMomentum in MovementBase
- ResetMain in LockableCameraBase  
resets the player camera to the main camera
- LockableCameraBase can also lock the cursor in Build

### CHANGED

- CharacterControllerMovement now uses Physics.SyncTransforms() when teleporting  
disabling the collider prevented some trigger messages from occuring

## 1.4.1

second update focused on the new 'Hero' demo

### ADDED

- HeroDemo
  - Title Screen, Scene Transition and Death Action
  - First Person View
  - Gear and Equipment Menus, Health and Damage in HUD
  - Stunning Nut and exploding Bomb Equipment Items
  - Pouches that expand carrying capacity for Money, Nuts, Bombs
  - Carrying Blocks, Pots and Bombs
  - Heart, HeartContainer, HeartPiece, Coin, Nut and Bomb Pickups
  - ...and much more, check out the HeroDebuggingGeneral and HeroTitle scenes!
- InputBindingText  
displays current input binding in UI texts(Legacy, TMPro, UIToolkit)  
useful for texts like 'Press {Confirm} to continue!'
- StateManager  
simple state manager that acts through unity events  
used in Hero for game and HUD state
- Simple UI Dialog Windows(OkCancel, YesNo, ... decisions)  
usable from code and new custom visual scripting nodes  
used in hero demo for name input and delete save game check
- Marching in MovementBase    
makes a character walk to or toward some target  
timeline track included so marching can be used in cutscenes
used in Hero when entering and exiting scenes

### IMPROVED

- More Custom Visual Scripting Units  
Hero uses visual scripting for a large part of its logic(see AdventureHero/Graphs)
- More Custom Editors  
many of the core components now have custom editors for improved debugging  
trigger areas show their current items, actions can be started, items added, ...
- Save Data Window for PlayerPrefSaver  
see Window/ActionAdventure/SaveData or 'Edit Save Data' on the PersistenceContainer
- TMPro Support  
Hero uses TMPro, Souls is still UIToolkit
- Actions can be started by name if they are children of the main actor
- CharacterControllerMovement can now apply weight(WeightPower field in Inspector)

### CHANGED

- visual scripting now uses ICharacterAssociator instead of directly referring to characters  
ICharacterAssociator implemented by components that are directly owned by characters  
this allows units on actions to easily use the executing character
- damage direction is stored in DamageEvent  
damage direction should be assigned to DamageEvent.Vector during IDamageSender.OnDamage    
TriggerDamageSender now has additional direction options(Up, SenderToReceiver, ...)
- force of damage and ragdolls is applied as ForceMode.Impulse
- neutral layer tweaked to be more consistent with Player and Enemy
- minimum recommended unity version increased to 2021.3.29

## 1.4.0

first in a series of 2-3 updates that focus on the second demo titled 'Hero'  
it is experimental for now and subject to major changes in the future

### ADDED

- AdventureHero Demo [Experimental]  
movement, attacking, lock on, item pickup and various other systems are already working
you can check out the current state of the demo in the HeroDebuggingGeneral scene
- AdventureCore.Tests Project  
contains test scenes for various features(lock on, timeline textbox)
- Timeline TextBox  
can be used to display text using a timeline and pause it until confirmation  
examples can be found in AdventureHero and the AdventureCore.Tests Project
- Timeline Instructions
applies Character Instructions from a timeline track
used in AdventureHero to suspend character movement while showing pickups
- Additional Visual Scripting Nodes for Lock On, Acting, VectorDirection, ...

### IMPROVED

- Additional sounds in Souls Demo for Bow, Pebble, ....
- Particles Effect for healing in Souls Demo
- added PropelCharacterLocal which is useful for motions like jumping
- horizontal velocity in CharacterControllerMovement improved
- LockOnManager now also manages and exposes the candidate lock on point 
- LockOnManager now checks the entire ray between camera and potential points  
this means models with lock on points can't be on the default layer  
therefore target dummies in souls are now on the new neutral layer

### FIXED

- Sprinting not working in Souls demo until button released again
- Bonfires did not suspend movement correctly 
- Projectiles not being destroyed in Souls Demo
- E Key not working in Souls Demo Title

## 1.3.1

### ADDED

- Simple Third Person Prefab (AdventureManual/Prefabs/AAKThirdPerson)
  - Basic CameraController for Third Person
- Resource System Manual Scene (AdventureManual/Systems/Resource/ManualResource)
  - Resource Bars for uGUI

## 1.3.0

### ADDED

- Visual Scripting Support
  - custom visual scripting units for all core systems 
  - visual scripting version of the getting started scene
  - enemy behavior examples
    - simple examples based on getting started
    - advanced enemies for souls demo(archer, attacker)
- Loot in Souls Demo
  - random state is persisted
  - define items and chances
  - loot drop is persisted until pickup 
- Item Manual Example Scene
  - equipment that is visible on the character
  - usables that add health and vitality

## 1.2.0

This update continues building out the souls demo while polishing up the core framework. It also adds a getting started tutorial project that should provide a smoother experience at the beginning. There is also a new integration available on [GitHub](https://github.com/Schossi/AAK_KinematicCharacterController) for the Kinematic Character Controller asset!  

### ADDED

- Step by step getting started tutorial
- Pebble usable item in souls which can be thrown
- Two handing weapons in souls
  - stances for two handed melee and bows
  - assignable actions for all 4 shoulder buttons
- Bow and Arrow in souls
  - damage is summed between bow and arrow
  - easily customizable arrows

### FIXED

- Camera damping in souls demo
- SoulsBossArea not properly resetting on player death
- MotionAction without cost now works on characters without ResourcePool
- Namespace of SoulsHideHUDInstruction which unfortunately breaks references so it has to be reassigned or fixed in text

### IMPROVED

- MovementBasePersisted does not error if no Persister is set
- LockableCameraFreeLook can move a target group member instead of adding the lock point
- LockOnManager can directly parent a visual to the locked point

Starting from this version I will no longer upload a dedicated version for the 2021.2 tech stream. I recommend using the latest 2021.3 LTS version.

## 1.1.1

Various smaller changes made for the AdventureSouls Scene Connector integration example. The example is called ConnectorSouls and can be found on [GitHub](https://github.com/Schossi/ConnectorSouls)!  

### IMPROVED

- TimelineAction and SoulsBonfireAction auto bind CinemachineBrain to PlayableDirector 
- AnimationToggler can be moved instantly using SetA and SetB
- SoulsLadderAction EndingTop and EndingBottom events
- SoulsLoading can be configured to load additional scenes
- SoulsPlayerCharacter multi scene support
  - saves current scene as checkpoint
  - goes to loading screen on death in a different scene
  - recovery is instantiated under SoulsCommons
- EffectPool and ResourcePool persist immediately when reset

## 1.1.0

### ADDED

- __Shrine__ - New Souls Demo Stage!
  - Trader and Talker NPCs  
  can be interacted with or attacked
  - Sprout Enemy  
  causes poison damage
  - Elevator  
  one way elevator with levers
  - Teleporter  
  moves character between scenes
- __Effects__ - New Core System!  
set of behaviors that can be added or removed from characters
  - Poisoned  
    periodically causes damage, healed by the new moss item
  - Boosted  
    doubles strength for a while, added by the new booster item
- Inventory  
  allows using items directly from the UI without equipping them
- Loading Screen

### IMPROVED

- intro can now be skipped
- NPCs can return home or patrol when idle
- additional events for characters on GenericTriggerArea/Item
- better gamepad support
- movement and resources are not saved unless they actually change
- picked up items are now displayed in a message box
- persistence can now be exported and imported from PersistenceContainer
- boss arena properly resets player position
- camera now uses cinemachine free look
- ...

An integration for the __ink__ narrative scripting language is available on [GitHub](https://github.com/Schossi/AAK_Ink)! 

--WARNING--  
Depending on your depth of use this update will contain a varying amount of breaking changes!
Using source control and removing any previous versions completely when upgrading is recommended.

## 1.0.1

### ADDED

- manual page for SoulsPlayer(includes step by step guide for replacing the model)

### CHANGED

- Animator is now part of CharacterBase, AnimatedCharacterBase is therefore obsolete

### IMPROVED

- simplified model replacement for SoulsPlayer
  - colliders moved to separate GameObject 'Body'
  - TimelineAction and SoulsBonfireAction can pass character animator to the PlayableDirector which lets us remove the dependency between environment and player model
- translation and rotation of model can be suppressed independently
  - SoulsAttackAction no longer locks rotation until damage is activated
- MovementSaver can be overridden from outside
  - BossArena uses this to properly reset the player if the game is quit during a fight
- intro sequence can now be skipped
- 2021 LTS compatibility

### FIXED

- usable items not being hidden if action is interrupted
- potential initialization order errors
  - AttributePool value/stat dictionaries
  - CharacterActionArea Bindings
- armor no longer vanishes at certain camera angles

## 1.0.0

__Initial Release__
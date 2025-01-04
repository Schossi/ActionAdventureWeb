---
permalink: /manual/hero-glade
title: "Hero Glade"
sidebar:
  title: "Manual"
  nav: manual
---

In this scene the player fights an ape that is controlled by an evil mask. It jumps into the glade as the player moves into it. They can blow off the rocks that protect its arms and legs using bombs. The ape can also be stunned with nuts or enraged by hitting the mask. The sling can be used to easily deal direct damage. Once the ape runs out of health a cutscene plays showing the mask come off and the maskless ape fleeing.

<p align="center">
  <img src="/assets/images/hero/heroGlade.png" />
</p>

In addition to the boss itself there are two smaller changes to the player character. The CharacterController has some settings changed to avoid it stepping onto the boss. Depending on the game these settings could be used in the entire game but some of the geometry in other scenes works better with some step up distance. The LockOn CycleLock toggle is set On in order to let the player easily switch between the two points in the boss.

## State

The overall state of the boss fight is controlled by the StateManager on the HeroApe GameObject which has the APE key. 

### WAIT
This is the default starting state when the ape has not been defeated. The ape gets hidden and a trigger is activated that transitions the state to INTRO when the player enters it.

### INTRO
This state plays the Intro TimelineAction which shows the ape jumping into the glade. That action transitions the state to ACTION when it is finished. The INTRO state activates the navmesh agent when it is exited because the ape is sure to be active and standing on the NavMesh at that point. 

### ACTION
This is the main state of the boss fight, here the monkey fights the player. The state activates the StateMachine on the Character when it is entered. That StateMachine controls which actions the ape character takes and any other logic needed for it to function.(like movement)

### DEAD 
This state gets set by the StateMachine when the ape runs out of health. Similarly to the INTRO state it plays a cutscene using a TimelineAction which moves the state over to DONE when it is finished. It also deactivates any running logic like the StateMachine and NavMeshAgent that is no longer needed.

### DONE
In this state the boss has been defeated and removed. It is either reached after the DEAD cutscene is played or when players enter the scene after the boss has been defeated previously. Persisting and checking the boss defeat is done in the ScriptMachine in the Entry GameObject.

## Character
The ape character uses a GenericCharacter script with no direct custom code. Its logic is contained in the StateMachine with the HeroApe graph. Like the skeleton enemies it has Health and Stun resources and an EffectPool so it can be damaged by attacks and stunned by nuts. To visualize damages and stuns it has some playable animations under the 'Animations' GameObject that get triggered by MessageEvents on the character. The HURT message is sent by the TriggerDamageReceiver on the 'Model' GameObject. STUN is sent from the Added event on the Stun EffectResourceValue.

### Movement

Instead of the regular NavMeshAgentMovement used by other enemies in the hero demo the ape uses the NavMeshTankMovement. This movement always first rotates towards its target and then moves relatively straight towards it. It sets its SpeedFactorSideways and SpeedFactorForward properties accordingly which get forwarded to the animator by the HeroApe StateMachine. The SpeedSideways parameter in the animator makes it shuffle to show it rotating and the Speed parameter makes it run. The root motion in HeroApeRun is then used by the movement and synchronized with the agent.

### Graph

During the actual fight the actions the ape takes are controlled by the HeroApe graph in the StateMachine. That graph makes heavy use of the RunAction unit which, when called in a coroutine, starts an action in its in port and continues on its out ports when the action has finished or failed to start. The graph starts off by waiting for a second and the running the scream action which gives the player a little pause after the cutscene. After that it moves to the Action state which controls the main combat logic.

Inside the Action state the ape moves between thinking, chasing and attacking the player. The rage state chains multiple actions together into a combo, it is reached either randomly from the attack state or when a RAGE event is triggered during the Chase and Think states because the mask was damage by, for example, the slingshot.

The Action state can be disrupted by the HURT event that is sent when one of the Rock armor parts is blown off by a bomb. The Hurt state forces the Hurt action which effectively staggers the ape and then moves back to the Action state.

### Actions

Most of the actions of the Ape are simple MotionActions which play the animation assigned in the CharacterAnimation field using PlayableAnimations. The attacks activate and deactivate damages and colliders in certain points of the animation. For example PunchLeft turns the regular collider of the left fist off and the damage trigger on when it receives DMG_ON. When the DMG_OFF message is received it reverts them. This allows the fist to move through the player and only push them with the damage value. If the regular collider was active it would throw the player across the entire arena.

### Colliders

The ape model contains various colliders and triggers, some are meant to collide with the player, some receive damage and some are only activate during attacks and deal damage. We will go through the ones on the left arm to explain what each one does.  

The 'FistLeft' GameObject contains the DestructibleDamageReceiver that lets the rock armor be blown off by bombs. It filters for HeroBombDamage so regular health damage does not affect it. The capsule collider in its child object is just a trigger that does not physically collide with the player. It instantiates HeroApeFistLeftDestroyed when it is destroyed and also activates the FistLeftTrigger which allows the player to damage the ape when hitting the fist from then on. It also triggers the HURT event on the state machine to stagger the ape.

The 'FistLeftCollider' is the object that actually physically collides with the player.

'FistLeftDamage' only gets activated during attacks and contains MovingDamageSender that can deal health damage to the player. 
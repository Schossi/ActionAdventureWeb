---
permalink: /manual/hero-inland
title: "Hero Inland"
sidebar:
  title: "Manual"
  nav: manual
---

This is the second scene players encounter in the hero demo. They reach it after leaving the beach through the trunk they find in a cave. It prominently features an entry to the temple scene which is initially blocked by some rocks that can only be removed using a bomb. The bomb equipment item can be found in the canyon scene which is entered through the second trunk located on the right side of the one coming from the beach. Players can also find their second heart shard on a ledge to the left of the trunk. To get it they have to defeat a skeleton that pops out of the ground when anyone gets too close.

<p align="center">
  <img src="/assets/images/hero/heroInland.png" />
</p>

## Entry

Decides between three different Parameter values.
1. Entering from HeroBeach
2. Entering from HeroCanyon
3. Entering from HeroTemple

Otherwise identical to the entry logic in the beach scene. 

## Passage

The entry to the temple uses the __HeroExitPassageBomb__ prefab which is a prefab variant of __HeroExitPassage__ with the addition of a bombable cracked wall.

__HeroExitPassage__ behaves pretty much identical to the [HeroExitTrunk]({% link _pages/manual/hero/beach.md %}) exits we have already seen. It only differs in the models and timelines used which animate the passage opening and closing with some particles and a sound effect.

The __HeroCrackedWall__ is another destructible similar to the vines we have encountered previously. It can only be destroyed by bombs because __HeroBombDamage__ is set in its __Filter__ field and bombs are the only damage sender with that kind of damage.

When it is destroyed the damage vector is applied as a force to all the rigidbodies in the __HeroCrackedWallDestroyed__ instance. The strength of that force can be changed by adjusting the __Magnitude__ of the damage sender in __HeroBombImpact__ or by changing the __PushPower__ multiplier in the damage receiver of the wall. Of course changing the mass of the rigidbodies will also change how far they are propelled. 

Whether the cracked wall has been destroyed is persisted using a __DestructionPersister__. This kind of persister sets a bool when its gameobject gets destroyed. If it wakes up and that bool is already set it immediately destroys itself. The __BehaviourEventTrigger__ on the same transform activates the __Exit__ whenever the cracked wall is destroyed. This prevents players from unintentionally activating the exit area before actually cracking the wall.

## Pots

The pots around the entrance of the temple combine a couple things we have already seen. They can be destroyed by any kind of damage like vines and their destroyed prefab has separate rigidbodies for all the shards resulting in a nice dynamic shattering effect like the cracked wall.

Pots can also be picked up and thrown due to the __HeroCarryAction__ located on the __Action__ transform. It is available as a context action when its area collides with the players [CharacterActionArea]({% link _pages/manual/hero/setup.md %}). The force applied when throwing can be changed in the __PowerForward/Up__ fields. If the __PutAllowed__ field is checked the pot will be softly put down in front of the player when the throw button is pressed while the character is not moving. 

It is not used like this in example game scenes but __HeroCarryAction__ could also be used for small plate puzzles. An example of this can be found in the [HeroDebuggingGeneral]({% link _pages/manual/hero/debug.md %}) scene(look for CarryObject and PressurePlate).

In addition to regular damage pots are also destroyed when their rigidbody receives a strong enough impulse, usually when they are thrown against something. This is configured in the __MaximumImpulse__ field, how much that impulse affects the destroyed shards can be adjusted in __ImpulsePower__.

The __HeroPotPickupRandom__ is also a bit more advanced than the pickups we have seen on sea urchins previously. The __HeroPotPickup__ visual script chooses a random pickup from a number of different options. Some of the options, like bombs, are preconditioned by the character already having that item in their inventory. Otherwise players could randomly find bombs in a pot before they actually get them in the canyon scene. 

## Skeleton

These enemies use a basic __GenericCharacter__ in combination with a __NavMeshAgentMovement__ to move them around. The actions they have available can be found under their __Actor__. Skeletons are not visible at first because their animator starts them off in the buried animation.

The skeleton stays underground until a character enters the __GenericTriggerArea__ found in the __Area__ object. While the player is inside that area a __AnimatorFloat__ instruction is applied to it as defined in the __Instructions__ field setting the __Aggro__ animator variable to 1. This changes the stance of the hero while locked on and transitions its face to the Mad animation. The __HeroCharacter__ state machine also monitors that variable to change to combat music.

Which actions the skeleton performs is driven by the __HeroSkeleton__ script graph. Once a character enters their area it sets that character as its movement target, starts the __Rise__ action and activates its __LockOnPoint__. Whenever the current action of its actor changes to null now it checks if it is close enough to punch the player, otherwise it runs towards them. The __ScriptMachine__ also checks for damage. Whenever it receives a hit that does not kill it it force starts the Hit Action making it flinch. If the damage does reduce the health to zero it replaces itself with __HeroSkeletonDestroyed__. The __Ragdoll__ visual scripting unit transfers all the transform positions to the destroyed model. 

The __HeroSkeleton__ script accesses actions by their name. This can be done for any action that is a child of the main actor of the character. This saves us the hassle of keeping all the needed actions as visual scripting variables.
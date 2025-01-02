---
permalink: /manual/hero-debug
title: "Hero Debug"
sidebar:
  title: "Manual"
  nav: manual
---

## General

<p align="center">
  <img src="/assets/images/hero/heroDebug.png" />
</p>

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/2AB8p22nCxY?si=vv9DEYz7YAJD5mCv&amp;start=2061" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Features a wide variety of items and objects. The hero character starts with a filled inventory and equipped items.

The white spheres in front of the destructible bushes contain lock on points which can be used to test out the lock on system. The slightly larger sphere also contains an test action that can be started as a context action whenever it is locked on. This could theoretically be used to read signs from a distance or talk to NPCs sitting out of regular action area reach.

The red blocks and spheres can be used to test out damage. The blocks have damages senders with different damage values, magnitudes and directions. The red capsule also contains a minimal enemy setup. This includes the stun resource so it can be used to test out the stunning nut equipment item.

The opposite side has some items that can be carried. The pots spawn different pickups as defined in __Destroyed Prefab__. The bombs ignite as soon as they are picked up which can not be found in the regular stages. The cylindric black __CarryObject__ can be placed on the pressure place in front of it which can be detected in its __Trigger__.

## Loading

<p align="center">
  <img src="/assets/images/hero/heroDebugLoading.png" />
</p>

This scene can be used to test out and debug the __Entry__ logic that is defined in each scene and the __Exit__ logic used by the __HeroExitTrunk__ and __HeroExitPassage__ objects.

It features two different __Exits__ that both load back into __HeroDebuggingLoading__, they set Parameter to different values which is then used in the __Entry__ visual script.

The timelines used when entering and exiting use a MarchTrack that moves the character towards a configured transform. These transforms are visualized with white spheres here.

The red box in the scene instantly kills the character which lets us test out the Die action and the Entry scenario with an empty Parameter.

## Passage

<p align="center">
  <img src="/assets/images/hero/heroDebugPassage.png" />
</p>

Contains different variations of the __HeroPassage__ prefab. Only HeroExitPassage is more of a variation on the HeroExitTrunk but using the model from the passage.

The root of the passage contains a __StateManager__ that is used to set a couple different objects active or inactive together. For example if the __HeroPassageLocked__ variant detects that it has previously been unlocked it sets the manager to the OPEN state. Otherwise we would have to separately deactivate both locks and activate the entry areas which would all have to be exposed to the visual script.

The enter actions themselves use a __TimelineAction__ to open the passage, march the character through and then close it again. It activates different virtual cameras that show the character entering and exiting. The __HeroReset__ signal is sent during the timeline which resets the third person camera to the camera position of the virtual camera.

The passage variants used in the [Temple]({% link _pages/manual/hero/temple.md %}) scene are described there. The [Inland]({% link _pages/manual/hero/inland.md %}) scene contains a passage exit blocked by a cracked wall.

## Skeleton

<p align="center">
  <img src="/assets/images/hero/heroDebugSkeleton.png" />
</p>

This scene can be used to quickly try out combat. It contains three skeletons that pop up when the player enters their area. The player starts out with equipped sword, shield and equipment items.

The skeleton itself is also described in the [Inland]({% link _pages/manual/hero/inland.md %}) scene where the player usually first encounters that enemy.

## Block

<p align="center">
  <img src="/assets/images/hero/heroDebugBlock.png" />
</p>

Shows off a couple more things that could be done with the movable __HeroBlock__ object in addition to its usage in the [Temple]({% link _pages/manual/hero/temple.md %}) scene.

There is a regular block in front of the character which can be pushed or pulled in any direction. To their right is a block that is limited in its movement. The block behind the player can be pushed into a hole with a trigger which activates a red sphere here. The raised area to the characters left can be used to test how a block behaves when it falls through a hole.

## Climbing

<p align="center">
  <img src="/assets/images/hero/heroDebugClimbing.png" />
</p>

Contains a couple different ways to set up a CLIMB trigger for the __HeroClimbAction__.

The wall to the characters left combines three separate colliders into one rigidbody with a single trigger area. This allows us to have a hole in a wall using convex colliders. The climb action can only use one area at a time, moving between areas mid climb is not possible.

In front of the character is a plateau that can be used to test out pulling up onto a higher level. It also has a damage sender for testing out damage while the character is climbing.

The small platforms behind the player are climbable but too small to actually climb on. This results in the character always immediately pulling itself up onto the platform.

## Ground

<p align="center">
  <img src="/assets/images/hero/heroDebugGround.png" />
</p>

This scene is made to test the ground checker located in the sound transform in [HeroSetup]({% link _pages/manual/hero/setup.md %}). The check is triggered whenever the character receives a STEP message and should play a different sound for each of the ground colors in the scene.

## Ape

<p align="center">
  <img src="/assets/images/hero/heroDebugApe.png" />
</p>

Use this scene to test out the actual fight portion of the ape boss fight. The state manager of the ape is set to its ACTION state which makes it immediately start attacking without playing its cutscene. The equipment of the hero can easily be changed in its inventory so the fight can quickly be tested with different items. Additional information about the boss fight can be found on the [glade]({% link _pages/manual/hero/glade.md %}) manual page. 

## Projectile

<p align="center">
  <img src="/assets/images/hero/heroDebugProjectile.png" />
</p>

This scene contains a spawner that instantiates projectiles which move towards the player. These projectiles can be redirected while guarding with the knight shield. This is controlled by the DeflectedDamages field on shield items. The BlockedDamages and GuardAngle of shields can also easily be tested in this scene. Deflection is done in the CheckDamage method of the HeroGuardAction which is called in the PreDamageReceive damage step of the HeroPlayerCharacter.

The projectiles can also be redirected with sword attacks. This is done using a TriggerDamageReceiver on the projectile itself which redirects the projectile in its Damaged event. By changing the called action there from Redirect to RedirectBack it could be changed to fly straight back where it came from instead of being redirected in the damage direction.

The two HeroTarget objects left and right of the spawner are triggered when hit with damage. Guard with the shield and aim at a target to redirect the projectile to them and trigger them. This could be used for puzzles in an actual game but is not currently used in any of the regular demo scenes.
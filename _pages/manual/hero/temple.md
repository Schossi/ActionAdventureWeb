---
permalink: /manual/hero-temple
title: "Hero Temple"
sidebar:
  title: "Manual"
  nav: manual
---

This used to be the final scene of the demo prior to update 1.8 which added the HeroGlade scene with the ape boss fight. Upon entering the player is faced with two blocked off passages and one open one. 

Entering that passage they are locked in with a skeleton enemy. Upon defeating that enemy they can collect a key item from a chest that lets them unlock one of the other rooms accessible from the center. 

There they move a block in order to reach another chest that contains the sling equipment item. The final heart shard is hidden behind the block here, if this is the fourth heart shard players collect they gain a heart essence increasing their HP by one heart. 

The sling lets players open the third room in the entrance by shooting the target above it. Behind it they find chests containing the superior knight versions of the sword and shield. 

The final room is another arena containing a bigger skeleton with increased health. The chests behind it contain heart essences that increase their maximum health. The passage behind it leads to the glade scene with its ape boss fight.

<p align="center">
  <img src="/assets/images/hero/heroTemple.png" />
</p>

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/2AB8p22nCxY?si=vv9DEYz7YAJD5mCv&amp;start=1784" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Passages

The temple scene uses a couple different variants of the __HeroPassage__ prefab. It is basically a door with an action for each side that moves the player to the other side. Check out the [HeroDebuggingPassage]({% link _pages/manual/hero/debug.md %}) debug scene for various examples of passage setups including one for the base prefab that just lets players walk through without any special behaviour.

The __HeroPassageArena__ variant adds a script machine to the passage which starts acting when the __TimelineAction__ on the Enter transform of SideA ends. The script closes the bars and then activates the enemy. It shows these happening using the __HeroHighlight__ subgraph which activates __Cinemachine__ cameras for a set duration. It then periodically check if there are still enemies remaining. When they are all gone it opens the bars back up and sets a persistence value. If that value is already set when the scene is started the arena destroys itself which lets the player walk through unhindered.

__HeroPassageLocked__ contains a simple __VisualScriptingAction__ that determines whether it can be started by checking the characters inventory for the __HeroKey__ item. When it is started it removes one key from the characters inventory and plays a short animation after which it opens the passage up. It also sets a persistence key which lets it know if it has already been opened if the scene is reloaded.

The __HeroTarget__ visual script on __HeroPassageTarget__ attaches to the Damaged event of a __TriggerDamageReceiver__. When it is hit it plays an animation which is highlighted by a virtual camera and then opens up the passage. It is also persisted very similarly to the other passages.

## Block

The __HeroBlock__ lets player move it by using one of the __HeroBlockAction__ actions located at each side. For some other examples of using this prefab, like falling down or activating a switch, check out the [HeroDebuggingBlock]({% link _pages/manual/hero/debug.md %}) debug scene.

One notable difference in the activation of this action is that it ends once the button that activated it is released. This is detected using the CONTEXT_ACTION_RELEASE message which is sent from the __HeroCharacter__ state machine inside the setup prefab.

Whether the character should push, pull or stand still is determined by the input the action receives in OnInput. The input is received from the __Move__ event in __PlayerInput__ which is configured to call __OnDirection__ on the player character. The character then forwards it through its actor to the active action. Climbing also uses this input for its climbing direction.

The __isKinematic__ field on the __Rigidbody__ is set to true unless the character is currently moving it which may be a little counterintuitive. Having the rigidbody kinematic unless it is grabbed is done so characters don't move it just by running against it. Setting is to false while pushing is done so the block can fall through holes in the floor under it. When the block starts falling the action is ended and a coroutine is started that resets isKinematic once the block has stopped moving.

## Sling

The __HeroSling__ equipment item contains the custom __HeroSlingAction__. It implements the __IHoldable__ interface so it can put itself in the player characters hand when appropriate and also lets it be put away in the same way the player puts away the sword.

Depending on the __AutoFirstPerson__ it can also activate first person when the player is pulling back the sling without being locked on to a target.

To try out how the sling interacts with other parts of the demo check out the [HeroDebuggingGeneral]({% link _pages/manual/hero/debug.md %}) debug scene which starts the player off with a sling equipped in one of the slots. It also contains ammo pickups and a pebble pouch that increases the slings ammo capacity.
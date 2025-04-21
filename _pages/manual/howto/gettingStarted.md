---
permalink: /howto/gettingstarted
title: "Getting Started"
sidebar:
  title: "Manual"
  nav: manual
---

Be sure to import at least the AdventureCore and AdventureManual projects to check out the getting started project. The scenes are located directly in Assets/SoftLeitner/AdventureManual/GettingStarted and the different asset used are all found in the subfolders.  

The __GSFinished__ scene contains the fully finished project, __GSStepByStep__ has a deactivated gameobject for the state after each step and the __GSEmpty__ scene only contains the environment and can be used to start from scratch when following this page.

<iframe width="560" height="315" src="https://www.youtube.com/embed/nVwg57p_Eho" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## 1 Movement

First off we'll just get a character moving around the environment.

- Create an Empty gameobject called 'Character' and reset its transform 
- Add a __GenericCharacter__ [character]({% link _pages/manual/content/character.md %}) component  
Most games, including the souls demo, will probably require some custom logic in the character but in this example we can get away with using the generic one just to bind all the other parts together.
- Drag the __GSRobo__ prefab into the Character and assign the Character field in the AnimatorProxy and the animator field in the character  
The prefab just contains a little robot model with a couple animations set up. The AnimatorProxy will forward things like animation events and root motion to our character which allows us to have them on separate objects. 
- Add a __CharacterController__ and a __CharacterControllerMovement__ [movement]({% link _pages/manual/content/movement.md %}) component to the Character
  - adjust the character controllers center and skin width
  - assign the movement to the Movement field in the GenericCharacter so other systems can access it
  - assign the GSRobo you created as the Pivot in the movement so it is rotated when moving
  - assign the MainCamera as the Camera of the movement so it can translate inputs relative to the camera  
- Add a __PlayerInput__ component to the Character([Input System](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.4/manual/index.html))
  - for Actions select GSInput which can be found in the Other subfolder
  - set its Behavior to 'Invoke Unity Events'
  - under Events->Default->Move create a callback that calls CharacterControllerMovement.OnMove

You should now be able to push play and move the character around the scene. 

## 2 Triggers

In this chapter we'll set up a trigger mechanism that slows our character when it moves through a certain area.

- Add a __GenericTriggerItem__ and a __CapsuleCollider__ component to the Character
  - Assign the Character field on the trigger so it is accessible to areas it enters
  - Adjust the colliders size and tick the IsTrigger field
- Create a new Cube 3D Object and add a __GenericTriggerArea__
  - Adjust the cubes size and material and tick its colliders IsTrigger
  - Add the __MovementSpeedMultiplier__ [instruction]({% link _pages/manual/content/character.md %}#instructions) to the area and set the Value to 0.5

Start the scene and move the character through the area to see that it is slowed down by the factor defined in the Instruction. The area adds its instructions to the character of any trigger item that enters it and removes them when it exits. The generic trigger items and areas are good for general purpose, when creating more specialized logic consider inheriting from TriggerArea and TriggerItem like, for example, the damage system does.

## 3 Effects

Here we will add the same kind of slow effect as in the last chapter with a status effect instead of directly applying it. This allows us to visualize the effect on the character and it would also be easier for other systems to check if the character is currently in one of these areas.

- Create an empty child in the Character and name it 'Status', this is where we will organize all the behaviors that make up a characters status
- Add an __EffectPool__ component which will manage the characters active [effects]({% link _pages/manual/content/effect.md %})
  - Assign the Character to the EffectPool to each other
  - Set the Effects field on the Pool(used later when dealing with persistence)
- Copy the slow area from the previous chapter and remove the instruction
  - Create handlers in the areas CharacterAdded and CharacterRemoved events
  - Drag the GSSlow effect into them and select the EffectType.AddExternal and EffectType.Remove methods
- Create a new UI Text and add a __EffectPoolText__
  - Delete the EventSystem Unity creates when you first add a UI component
  - Adjust the Text size and position
  - Assign the fields in the effect pool text

The effect assets for this chapter have been created upfront to streamline the process, the effect set and effect type can be created from the context menu under Create/Adventure. The most important bit is the effect Prefab which contains the effect itself with the slow instruction as well as the visual.

When you start the game now and move into the new effect area you should be slowed and additionally see a small blue sphere above the character. You should also see the effects name in the UI text you created.

## 4 Pickups

Next up we'll create some item pickups and allow the character to add them to its inventory. First up we'll allow the character to start actions that it runs into.

- Create an empty child in the Character and name it 'Actor', this is where we will put all the logic that lets the character perform [actions]({% link _pages/manual/content/acting.md %})
  - Add a __MinimalCharacterActor__ which will manage the characters current action 
    - assign it to the character and the character to it
  - Add a __CharacterActionArea__ which lets the character start actions it collides with
    - assign the actor to it so it knows where to start its actions
  - Add a Trigger __CapsuleCollider__ and a Kinematic __Rigidbody__ for the action area
- In the UI create a new Text, this time with a __CharacterActionAreaText__ which will display the areas current action
- In the PlayerInput where we previously bound the move input create a new handler for Action and bind it to CharacterActionArea.OnStartAction

The next step is to give the character an Inventory it can store items in.

- Create an empty child in the Character and name it 'Inventory', this is where everything related to [items]({% link _pages/manual/content/item.md %}) will go
  - Add a __ListedInventory__ and assign it to the characters Inventory field. Assign GSItems to its Items field which will be important for persistence later.
- In the UI create a new Text, this time with a __ListedInventoryText__ which will display the items in our inventory

Finally let's create the pickups that we can place around the environment.

- Create a cube with a trigger collider similar to the ones used for the trigger areas previously
- Add a __PickupAction__ component
  - Add a SuspendMovement instruction so the character can't be moved while performing the action
  - Set the CharacterTriggerName to 'Pickup' which is the name of the parameter in the GSRobo Animator
  - In Items assign the GSItem and set Quantity to 1
- Once you're happy with the Pickup clone it a couple of times so you can test adding multiple items to the inventory

You should now be able to see 'Pick up Item' when you move the character over a pickup. If you start the action using E or A on a Gamepad the character should perform a little animation, the pickup should be destroyed and the item added to the characters inventory.

## 5 Gate

The Pickup we have created in the last chapter performs an animation on the character only. Here we will create a new action that plays synced animations on both the character and an object in the environment. We'll also define an item cost so we can actually do something with the items we've picked up in the last chapter, this is fairly common behavior for things like keys.

- Drag the GSGate prefab from the assets into the scene
- Create a new child object under GSGate and name it 'Action'
- Position it in the same place and rotation you want the character to stand when performing the action, for example 1.4 in Z and rotated 180 around Y
- Add an __ObjectAction__ component
  - Add a SuspendMovement instruction
  - Set the following:  
  Name: 'Open' (displayed by the Action Area)  
  Cost: 1 x GSItem (can't start action unless character has the item)  
  CharacterTriggerName: 'Open' (can be checked in the GSRobo Animator)  
  ObjectCharacterTarget: the action itself (character is moved here)  
  ObjectTriggerName: 'Open' (like in the GSGate animator)  
  ObjectStateName: 'Opened' (see GSGate animator, used in persistence later)
  ObjectAnimator: GSGate
- Add a BoxCollider with IsTrigger and Size 2 so the action area can collide with and start the object action

If you want to test out the action without having to pick up an item first you can either remove the cost or add some items to the ListedInventory in the editor before you start the game. To see how the animations are synchronized check out the animation events on the Open animations. The START and END events are needed so the action knows its state, the ACT message in the character animation starts the object animation.

## 6 Health

In this chapter we'll give the character a health [resource]({% link _pages/manual/content/resource.md %}).

- As a child of Status create a new object named 'Health' with a __ResourceValue__ component
  - Set the type to GSHealth which identifies the resource and gives it a name
  - Set Maximum and Value to 100 so the character starts with full health
- In Status add a __ResourcePool__ component
  - Add the health resource value to its values
  - Assign the pool to the characters Resource Pool field
- In the UI create a new Text with a __ResourcePoolText__ which will display all resources of the Pool and their values

Next we'll create some areas that deal [damage]({% link _pages/manual/content/damage.md %}) which can subtract from the health resource or add to it.

- Create two new areas, probably easiest to just copy the slow areas and remove the trigger area
  - Add __TriggerDamagerSender__ components to both
  - In the Damages field assign GSHealthDamage in one and GSHealDamage in the other with a value of 1
  - Adjust the timing settings, for example check SendTick and set TickRate to 0.1
- On the character add a __TriggerDamageReceiver__ and assign the character so it can actually receive the damage 

The characters health should be reduced when moving into one area and increased in the other. 

## 7 Vitality

The maximum Health was previously just statically assigned in the resource itself. In this chapter we'll add a Vitality [attribute]({% link _pages/manual/content/attribute.md %}) and a HealthMaximum stat(which is just VIT*10) which will be assigned as the Health resource maximum.

- In Status add an __AttributePool__ component and assign it to the character
  - Add GSVitality to the Attributes and give it a starting value of 10
  - Add a new ResourceMaximum with the Health resource value and the GSHealthMaximum stat
- In the UI create a new Text with a __AttributePoolText__ which will display all attributes of the Pool and their values
- Create two new areas by copying the slow areas but this time leave the trigger area and just empty the event handlers and instructions
  - Add a new handler to CharacterAdded and drag in the GSVitality attribute
  - Choose the AttributeType.Add method in one area and Remove in the other

When you move into the areas you should now receive or loose one point in the Vitality attribute which changes health max by 10.

## 8 Persistence

Finally we will take care of [persisting]({% link _pages/manual/content/persistence.md %}) the state of all the things we've created this far.

- Create an empty gameobject named 'Persistence'
  - Add a __PersistenceContainer__ component, you'll find some useful buttons in its inspector like 'Delete Data' which can be used to reset its save data 
    - Set the Key to 'GS'
    - Add GSPersistence to its Areas 
  - Add a __PlayerPrefSaver__ and assign it as the containers saver
- Add a ManualPersister to the character and assign it to
  - GenericCharacter, CharacterControllerMovement
  - ListedInventory
  - Resource-, Attribute- and EffectPool
- Add and assign a ManualPersister to the ObjectAction in the Gate
- The PickupAction has its persister built in so you can assign PersistenceArea and Key directly to it instead of adding a ManualPersister
- In every ManualPersister(and PickupAction) assign GSPersistence as the Area
- Give every ManualPersister(and PickupAction) a Key to identify the piece of data

Instead of manually choosing keys you can also click 'Generate Missing Keys' on the persistence container which will generate keys if they are empty.

Since the container should be set to AutoSave and AutoLoad by default everything you do in play mode should now be saved. When you stop play mode and start it again the state will be loaded. You can reset the state by clearing player prefs or deleting data from the container. If you want suspend persistence and start from scratch every time you can unassign the Saver on the container.

## Next

You should now be familiar with the major systems in AAK. As a next step towards a more complete game you can check out the AdventureArena demo found in the [Extras]({% link _pages/manual/other/extras.md %}) project. It is meant as a middle ground of complexity between the mechanics shown here and the relatively sophisticated hero and souls demos.
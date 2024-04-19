---
permalink: /manual/hero-setup
title: "Hero Setup"
sidebar:
  title: "Manual"
  nav: manual
---

The following is an overview of the HeroSetup prefab in the [AdventureHero]({% link _pages/demos/demoHero.md %}) demo.

<p align="center">
  <img src="/assets/images/hero/heroSetup.png" />
</p>

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/2AB8p22nCxY?si=vv9DEYz7YAJD5mCv&amp;start=27" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

- [HeroSetup](#herosetup)
  - [Camera](#camera)
  - [HeroCharacter](#herocharacter)
    - [HeroKid](#herokid)
    - [Camera](#camera)
    - [Trigger](#trigger)
    - [Actor](#actor)
    - [Inventory](#inventory)
    - [Status](#status)
    - [LockOn](#lockon)
    - [Sound](#sound)
  - [UI](#ui)
    - [LockOnTarget](#lockon)
    - [SignDialog](#signdialog)
    - [TextBox](#textbox)
    - [TextBoxFullscreen](#textbox)
    - [Menu](#menu)
    - [HUD](#hud)
    - [Fade](#menu)
    - [Sound](#sound)
  - [Gear](#gear)
  - [Music](#sound)

## HeroSetup

The top gameobject starts off with a couple components related to [Persistence]({% link _pages/manual/content/persistence.md %}).  
__PersistenceContainer__ is the main persistence component that manages all the current data.  
The __PlayerPrefSaver__ is used by the container to load and save its data to unity player prefs. When the __Debug__ field on the saver is checked it uses temporary variables instead. The data in __DebugData__ then deactivates intros and starts the inventory off with a sword and shield.

The __PlaytimePersister__ saves the current playtime which is then displayed in the intro scene when selecting the save game. The __RandomStatePersister__ makes sure that random drops like items from pots are consistent between sessions.

The __StateManager__ manages the general state of gameplay. The PLAY and MENU states are triggered by input events on the __PlayerInput__ component on HeroCharacter. The CUTSCENE state is, for example, triggered by a state override in the HeroIntroBeach timeline. Check out the events of the manager to see all the actions it performs when states are changed. Some examples are:
- Switching Input Action Map
- Showing and Hiding Menu UI
- Transitioning the Audio Snapshot
- Setting the HUD State

HeroSetup also contains the scenes variables for [Visual Scripting]({% link _pages/manual/content/persistence.md %}). __Action__ contains the context action that the __HeroCharacter__ state machine determines. __ActionOverride__ is used by some actions like holding a bomb to temporarily override that action.  

### Camera

This object contains the main __Camera__ and __CinemachineBrain__ as well as a couple different virtual cameras.  

The __Flash__ and __Hurt__ cameras have a timeline that very shortly switches to them resulting in Red and White flashes of the screen. They are triggered by FLASH and HURT messages on the character. HURT is sent from the __HeroCharacter__ state machine when damage occurs. FLASH is sent on start by a script machine in the __HeroNutImpact__.  

The free look and first person objects are used by the __LockableCameraFreeLook__ on the camera object inside the __HeroCharacter__ for the general camera movement.  
The __PresentCamera__ is switched to within the __HeroPresent__ timeline when new items are shown. It points at the __PresentTarget__ within the character model in front and above the character.

### HeroCharacter

The [HeroPlayerCharacter]({% link _pages/manual/content/character.md %}) script contains some custom hero demo logic in addition to the base character functionality it inherits from the core character class. This includes presenting new items and sheathing the items the character has in its hands. It also forwards confirmation and cancel presses to text boxes. If you check its events in the inspector you should find some actions that forward messages to the audio manager.  

Most of the custom logic of the character is defined in the __HeroCharacter__ state machine.
- Forwarding Animation Parameters from Movement
- Triggering Idle Animations
- Choosing and Triggering Context and Attack Actions
- Responding to Damage and Death   

When the main __StateManager__ is not in its PLAY state the character state machine moves from its Gameplay to its Paused state and also sets the Time Scale of the entire game to 0.

The __CharacterController__ and [CharacterControllerMovement]({% link _pages/manual/content/movement.md %}) manage the characters movement. The movement speed is set to 0 because in this demo movement is driven by the root motion of the animations.  

__PlayerInput__ is a part of the 'New Input System' in Unity. It is configured to Invoke Unity Events here so you can find the various actions that are driven by Input by expanding the Events field.  

The __SignalReceiver__ at the bottom can receive the __HeroReset__ signal from timelines to reset the camera controller. This is done in the __HeroPassage__ timeline to reset the camera position to the new position at the other side of the door.  

#### HeroKid

This is where the visuals of the player are located. It contains the Animator along with an AnimatorProxy that passes along events and root motion from the animator to the character.

The following steps show how the player model could be replaced by the skeleton model that is usually used by the enemies. Some other components of the demo depend on the player model so those have to be moved over or reassigned.

- Open up the HeroSetup prefab
- Drag the Models/Characters/HeroSkeletonZombie into __HeroCharacter__
- Assign the HeroKid Controller to the __Controller__ of the __Animator__
- Select the old HeroKid object and drag over the __AnimatorProxy__ and __AnimatorParameterSetter__
- Fully expand both models, you will see some additional transforms in HeroKid
- Drag over the extra transforms into the same spaces in the skeleton
  - FPSPosition in Head
  - ShordSheathe, ShieldSheathe and OverheadItem in Chest
  - HoldingLockOnPoint, ... directly under HeroKid
- Delete the __HeroKid__ model
- On the __HeroPlayerCharacter__ set __Animator__ to HeroSkeleton
- Set ItemParentRight/Left to the new Item.R/L transforms
- On __CharacterControllerMovement__ set __Pivot__ to HeroSkeleton
- In the __LockableCameraFreeLook__ in Camera replace the __Reference__ transform and set all the renderers from the skeleton to __FirstPersonHideRenderers__

If you now open up HeroDebuggingGeneral you should be able to move around and perform all the different actions with the new model.

#### Trigger

This object contains a __CapsuleCollider__ trigger and a __Rigidbody__. These are used by a __TriggerDamageReceiver__ for damage that affects the player and a __GenericTriggerItem__ for general trigger areas like the exit trunks.

#### Actor

Contains [actions]({% link _pages/manual/content/acting.md %}) the player can perform on its own and the actor that manages their execution. Other actions are located on items or environment objects.

The hero demo uses the __MinimalCharacterActor__ so the character can only perform one action at a time and no further action can be queued until the current one is done.

- __Jump__ is a basic __MotionAction__ that propels the character when it is started
- __Roll__ is also a __MotionAction__ but it locks rotation movement while active
- The different Attack actions use __VisualScriptingAction__  
open up the __HeroAttackAction__ graph to explore how it works, the messages it receives are sent by animation events found in the __HeroKid__ model
- The __JumpAttack__ uses a __MotionAction__  
additional logic like damage activation is handled in a script machine executing __HeroJumpAttack__  
- __Guard__ is a custom action made for the hero demo    
it was made in code as it performs some bone rotations that would be a hassle in visual scripting, the Starting and Ending events are configured to send messages that are used in the AudioManager
- __Sheathe__ is a simple __VisualScriptingAction__ that prompts the character to sheathe items it has in its hands, the sheathing itself is handled in the __HeroPlayerCharacter__
- __Present__ is a __TimelineAction__ that executes a timeline that shows off a newly gotten item  
it gets started by the __HeroPlayerCharacter__ which also instantiates the visual of the item
- __Die__ is another __TimelineAction__ that is started from the __HeroCharacter__ visual graph
- __Climb__ is a custom action that lets the player climb up some special surfaces  
double click the script to check it out in Visual Studio, like most classes in AAK it has an explanation at the top of the file, examples for climbable surfaces are found in HeroDebuggingClimbing or the HeroBeach and HeroInland scenes

#### Inventory

The [Inventory]({% link _pages/manual/content/item.md %}) uses a simple __ListedInventory__ and contains no items in the prefab. Items can be added for debugging in all scenes except the beach scene. As this is the first scene that is loaded the inventory content there determines the players starting items. In any other scene there should already be save data that overrides the inventory content set in the scene.

Both __Sword__- and __ShieldSlot__ are of type __instantiatingItemSlot__. The Shield- and SwordParent transforms they instantiate into are move to the characters hand or sheathing position by the __HeroPlayerCharacter__ script.

The equipment slots use a fairly simple custom slot called __HeroEquipmentSlot__ which instantiates the action prefab defined on the __HeroEquipmentItem__. The instance of that action is then checked to determine if the slot can be used. The three regular slots are used from the equipment buttons that are visualized in the top right, the fourth slot called __EquipmentX__ is used when an item is used directly from the menu.

The inventory also defines some item capacities. These are raised when the player collects a __CapacityRaisingItem__ like the Nut Bag or Money Pouch. There is no place to collect these in the regular game yet but they can be tested in __HeroDebuggingGeneral__

#### Status

This object contains a couple different components representing the characters status.

There is only a single [resource]({% link _pages/manual/content/resource.md %}) value in the __ResourcePool__ called health. It is decreased by __HeroHealthDamage__ and increased by usable items like the __HeroHeart__. The __HeroCharacter__ state machine initiates death when it runs out. 

The [Attribute Pool]({% link _pages/manual/content/attribute.md %}) contains the __HeroHearts__ attribute which is increased by collecting __HeroHeartEssence__ usable items. The __HeroHeartsMaximum__ stat copies that attribute and is used to determine the maximum health of the hero. This is configured in the ResourceMaximums field of the pool.

There is an [Effect Pool]({% link _pages/manual/content/effect.md %}) even though there are currently no effects relevant to the player character. The currently only effect of the demo is called __HeroStunEffect__ and is added to enemies when their __HeroStun__ resource is filled by the __HeroStunDamage__ done in __HeroNutImpact__. You can quickly try that out in the HeroDebuggingSkeleton scene.

#### LockOn

Contains a trigger collider and __LockOnManager__ which is a special kind of __TriggerArea__. It receives inputs from events in the PlayerInput component on HeroCharacter. Enemies and anything else that needs locking on have to contain the counterpart __LockOnPoint__. There are some simple spheres with lock on points to quickly try this out in the HeroDebuggingGeneral scene.

The __HeroTargetCandidate__ has a __Follower__ component that is triggered by actions configured in the managers inspector. It visualizes possible lock on points while nothing is locked on. It also contains an animator that is controlled by events on the follower. Another __Follower__ can be found on the __LockOnTarget__ in the __UI__ transform. This one is shown when a point is actually locked on.

There is a separate testing scene for the lock on system in AdventureCore.Tests called LockOnTest which can be used to test out the system in isolation.

#### Sound

The __AudioManager__ here contains sound effects directly related to the player character. Its __Key__ field is empty making it the main manager accessible anywhere. The manager responsible for music can be found directly under HeroSetup. Another one that plays UI selection beeps and button clicks is located in the UI transform.

All Messages received on the __HeroPlayerCharacter__ are forwarded to it using the __MessageReceived__ event. This includes messages sent by animations using the OnAnimation function. You can find these on the HeroKid model import. For example select HeroKidRoll and expand the event section to find STEP messages along with the START and END messages needed by the action. Some messages are also handled by items, for example the S_SWING message on the HeroKidAttackHA animation is handled by a __CharacterMessenger__ directly on the __HeroDebugSword__ prefab allowing different sounds per sword. Some messages are also sent by actions directly, jump and roll send a GRUNT message when they are started. 

### UI

This is the root transform for the 2d user interface of the hero demo which is made using Unity uGUI. As such it contains the typical __Canvas__ and __EventSystem__ components as well as the __InputSystemUIInputModule__ which connects it to the new Unity input system. The only AAK component here is the __EventSystemCursor__ which places the __Cursor__ element over the current gameobject of the event system and scales it to fit.

#### SignDialog

Dialog used by the __VisualScriptingAction__ in the __HeroSign__ object. Has to be assigned in the variables of the script machine of the sign so it can display its text.

#### TextBox

The __TextBoxTMP__ here is used to display newly acquired items as part of the Present __TimelineAction__. The one located in TextBoxFullscreen is used in the Die action. 

#### Menu

Contains the pause menu with its gear, equipment and settings tabs. Showing the menu and tabbing between screens is handled by the __HeroMenu__ script. It references the __Fade__ transform which contains a solid black screen that is used to fade out the screen when the game is ended.

The top bar contains buttons that can jump directly to their tabs and buttons that switch to the right and left. Those are connected to the right and left triggers on controllers using a __ButtonInputAdapter__ which is triggered by events on the __PlayerInput__ component on __HeroCharacter__.

The actual contents of the tabs are located in the __Contents__ transform. Feel free to activate any tab you are making changes to, any tab except the starting tab set in the __CurrentTab__ field is automatically deactivated on start.

The __Gear__ tab shows some general item info and lets players select their current gear.
- __Character__ shows the fake character model located in [Gear](#gear)
- __Items__ contains different buttons that use the __HeroGearToggle__ script to equip and unequip gear items
- __HeartShards__ visualizes how many __HeroHeartShard__ items the player currently carries using a visual script machine, since the shard is a __PlaceholderItem__ when the player collects their fourth shard a __HeroHeartEssence__ is added instead and the shards are reset
- __Keys__ uses a simple __ItemQuantityVisualizer__ to visualize the current number of keys

In the __Equipment__ tab players can assign equipment items to their equipment slots. The actual assignment and animation is handled by the respective __HeroSlotPanel__ in the HUD. These check which __HeroEquipmentButton__ is currently selected to know which item to assign. Alternatively equipment can be directly used from the menu by pressing the button.

The Settings tab contains __AudioSlider__ components for changing different audio group volumes and a __QualityDropdown__ which changes the visual fidelity to one of the quality settings defined in the project settings. It also has buttons for ending the game that trigger methods inside the __HeroMenu__.

#### HUD

The HUD shows the player health, money and available actions during gameplay. Different situations like cutscenes or dialogs require parts or the entirety of the HUD to be hidden. This is done using a __StateManager__ that activates gameobjects and passes animator parameters according to the current state. Since its not possible in Unity to call methods with multiple parameters from events the manager uses an __AnimatorParameterSetter__ to set the state parameter of the animator.

- __Equipment1-3__ use the __HeroSlotPanel__ script to display the currently assigned item and for assignment of the slot from the equipment tab
- __Money__ has a __ItemQuantityTextCapped__ script which visualizes the quantity in a text field and changes its color when the item capacity is reached
- __Health__ displays the current and maximum player health using one __HeroHealthBar__ script which has references to a __HeroHealthUnit__ for each possible heart.

Money and Health are both stored as prefabs so they can be reused to display money and health of the save slots in the intro screen. 

### Gear

This is where the player model that is shown in the gear screen is located. It uses its own camera and lighting which is separated from the regular game by using the UI layer. The contents of the sword and shield slots from the actual player character are copied using __InstantiatingItemSlotClone__ scripts.
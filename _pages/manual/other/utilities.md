---
permalink: /manual/utilities
title: "Utilities"
sidebar:
  title: "Manual"
  nav: manual
---

## Playable Animation

This utility allows playing animations on top of a regular animator. They do this using the [Playables API](https://docs.unity3d.com/Manual/Playables.html) that is also used in timelines. A test scene called PlayableAnimationTest can be found in AdventureCore.Tests. Enter play mode and use the Play/Start buttons on the different actions and players to try them out.

By using a PlayableAnimations it is possible to store and configure animations outside of the character, for example on actions. This can make reusing actions more convenient as the character animator is not required to contain the actions animations. It also allows makes it easier to individualize animations between actions, for example by changing their speed.  

Storing the animations in the actions can also slim down the character animator greatly. In HeroDebuggingGeneral the original animator controller HeroKid has been replaced by HeroKidMovement which only contains the movement. The downside to this is that animations moved outside the character animator can't really interact with those inside. For example the guard action that holds up the shield also needs the character to go into a Shield Stance which is why it was left in the character animator.

__A lot of the actions in both the hero and souls demo now use playable animations. To restore the original behaviour you can simply set the animation clip or controller on the action to none.__

<p align="center">
  <img src="/assets/images/other/playableAnimationEmpty.png" />
</p>

Above is how a PlayableAnimation looks in the Inspector when no animation or controller is configured. You can set an AnimationClip in the left field or an AnimatorController in the right one to use them. These can be found on a lot of the actions that come with AAK or the __PlayableAnimationPlayer__ behaviour. 

To use PlayableAnimations in your own behaviour simply add a __PlayableAnimationParameters__ field to it. The actual PlayableAnimation is created by calling Play(to start immediately) or Create(to play later).

### Settings

Some settings are different between clips and controllers and some are the same. I will give some extra pointers here but as with everything in AAK a short description for every individual field can be found in the tooltip in the inspector.

One Setting that deserves a little extra attention is the __Mask__ because it is one of the __limitations__ of playable animations. It is not usually possible to only affect parts of a character in a playable when using __humanoid__ animations. Playable animations work around this by copying all the muscle values from a previous animation. This will lead to some undesirable side effects especially when combined with __root motion__. In conclusion, be aware that playable animation may cause weird behaviour with masks on humanoid animations, especially when root motion is not baked.  

In most cases the Mask, Additive and Weight fields should work very similarly to their counterparts in animator layers.

<p align="center">
  <img src="/assets/images/other/playableAnimationSettings.png" />
</p>

Instructions and Messages are the only features of PlayableAnimations that are directly dependent on AAK characters. They are optional so PlayableAnimations are completely usable outside of AAK. When played on an action the instructions are applied to the character associated with the action and messages are sent to it at the defined times. The PlayableAnimationPlayer has a Character field which can be used when these features are used.  

During play mode there are some additional information labels and controls in the inspector to help with debugging. The parameter field is also colored yellow when the animation is active. To get further insight you may want to add the PlayableGraph Visualizer from unity to your project. 

### Animation Clip

<p align="center">
  <img src="/assets/images/other/playableAnimationClip.png" />
</p>

By default animation clips play once and then end the playable animation. This can be changed by checking the Loop field. In this case the animation has to be ended from outside or it will play continuously. It is possible to change start and end times, the absolute time value in seconds is displayed in the bottom.

### Animator Controller

<p align="center">
  <img src="/assets/images/other/playableAnimationController.png" />
</p>

When an animation controller ends is a bit harder to determine than an animation clip. The EndState field can be used to define one or more state names at the end of which the playable animation ends itself.(separate multiple using spaces) Otherwise it has to be done from outside, for example some actions use a END message on one of the animations to determine that the animation has ended.

## Trigger Areas

The TriggerArea and TriggerItem behaviors included in AAK offer a convenient layer on top of regular unity triggers. They make sure trigger events happen exactly once and that triggers that are disable get removed reliably. They always come in a item-area pair which also filters out a lot of usually unwanted triggers.

The basic functionality of these triggers is defined in the abstract TriggerArea and TriggerItem base classes. AAK offers a very general implementation of these in GenericTriggerArea and GenericTriggerItem. These add unity events that can be used in the inspector as well as ways to add instructions and send messages to characters attached to the area or item. They also let users define a Key which is used to filter which areas and items interact with each other.(in addition to regular unity layers) For and example see the __AdventureManual/Systems/Trigger/ManualTrigger__ example scene.

Two more specific usages of triggers inside AAK are the lock on and damage systems. The LockOnManager implements TriggerArea and LockOnPoints is a TriggerItem. TriggerDamageSender is the most common type of damager sender and derives from TriggerArea while TriggerDamageReceiver is derived from TriggerItem.

It is fairly easy to create your own triggers with special behavior, see the example below:

```
public class CustomTriggerArea : TriggerArea<CustomTriggerArea, CustomTriggerItem>
{
    protected override void onItemAdded(CustomTriggerItem item)
    {
        base.onItemAdded(item);

        //your custom logic
    }

    protected override void onItemRemoved(CustomTriggerItem item)
    {
        base.onItemRemoved(item);

        //your custom logic
    }
}
public class CustomTriggerItem : TriggerItem<CustomTriggerArea, CustomTriggerItem>
{
    protected override void onAreaAdded(CustomTriggerArea area)
    {
        base.onAreaAdded(area);

        //your custom logic
    }

    protected override void onAreaRemoved(CustomTriggerArea area)
    {
        base.onAreaRemoved(area);

        //your custom logic
    }
}
```

## Dialogs

AAK comes with a generalized dialog component for displaying simple messages, conversations and dialogs. There are implementations for legacy Unity Text components(DialogUI), the more common TextMeshPro Text component(DialogTMP) and the newer UI Toolkit system(DialogElement).

They are used in both the Hero and Souls demos to display various messages and dialogs. The MSG dialog in souls displays general message and the PCK dialog is shown from the SoulsPickupAction. In Hero the MAIN(empty key) dialog is used for pickup animations, a Fullscreen FULL dialog is shown on GameOver and a SIGN dialog displays the texts defined in the Signs around the stages.

Dialogs are shown by calling one of the Show method overload or by using a Visual Scripting Unit like ShowDialog which just call those methods under the hood. You can find various examples in the VisualScriptingDialog test scenes in AdventureCore.Tests.

Another way to show dialog is through Timeline. Add a DialogTrack to your Timeline to do so. DialogClips can be added to the Track to define the text to display. Hide and Show Clips can be used to fade the dialog in and out. This can be used to display Text in Cutscenes or short Sequences(Hero Demo opening Chest). The Dialog Implementation can be directly referenced in the Track or through its Key in the individual Clips. Check out the TimelineDialog test scenes in AdventureCore.Tests for examples.

While the built in Dialogs may work well for simple messages and short dialogs, for more dialog heavy games a dedicated dialog language may be a good choice. If this is the case for you please check out the [Ink Integration](https://github.com/Schossi/AAK_Ink).

## State Manager

A StateManager component can be used to define various states and things that happens when the active state changes from one to another. Each state defined has events for when it is Entered and Exited. The Started event is called when the state becomes active at the start of the scene. The distinction between Entered and Started is made because some states may want to play animations when entered but not when started.

The state of a manager can either be set directly by using SetState or by setting an override. Overrides become the active state and return the manager to its underlying state when they are reset. This can be done through code, by using one of the visual scripting units or using the dedicated timeline track.

The Hero demo uses one StateManager for general game state like PLAY, MENU, CUTSCENE, DIALOG. Its states set the current InputActionMap, transition the sound mixer and change cursor lock among various other functions. They also control the state of the second StateManager(HUD) which manages the action buttons on the top right. 

Some ways the main state manager in the hero demo has its state changed are:
- PlayerInput(HeroCharacter) Pause Action sets the state to MENU
- SIGN Dialog Opening sets DIALOG as an override, Closing resets it
- HeroIntroBeach timeline overrides the main state to CUTSCENE
- HeroChest timeline overrides the HUD state to HIDE and DIALOG

## Audio Manager

The AudioManager component can be used to centralize audio clips. It allows defining clips and music that can be played sending the defined key. By hooking it up to a characters MessageReceived event this allows playing sounds on the character from actions that are defined outside of it. Another use case is UI, instead of playing sounds on all the individual component the audio manager allows defining and changing them in one central location.

The Hero demo uses AudioManager components in its UI and in all of the characters. There are also audio managers in the sword and shield prefabs which hook up to the owning characters message pipeline using a CharacterMessenger component. This allows playing different sounds for sword swings and shield blocks depending on the characters current equipment. The S_SWING message for example is sent from events in the attack animations(HeroKidAttackVA, VB, ...).
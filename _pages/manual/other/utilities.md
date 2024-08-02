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
---
permalink: /manual/hero-canyon
title: "Hero Canyon"
sidebar:
  title: "Manual"
  nav: manual
---

In this scene the player evades a rolling boulder to get to a chest containing the bomb equipment item. There is a cracked wall on the boulders path that blocks the entrance to a little cave containing the third heart shard.  

<p align="center">
  <img src="/assets/images/hero/heroCanyon.png" />
</p>

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/2AB8p22nCxY?si=vv9DEYz7YAJD5mCv&amp;start=1729" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Boulder

During the intro cutscene the boulder is deactivated until the camera pans over to it. This is only done to make timing it a bit easier. It is controlled by the custom __HeroBoulder__ script which moves it between the points defined in the __Points__ field. When it reaches one of those points it fires the __Stopped__ event and then waits for the __CornerDelay__.

The boulder carries a __TriggerDamageSender__ that basically damages the player if they touch the boulder. Its __Direction__ is set to __SenderToReceiverXY__ which causes the player to be pushed away from the boulders center but into the ground or up into the sky. Whenever it hits something it calls __Delay__ on the boulder script which stops the it and avoids just rolling over the player.
---
permalink: /manual/damage
title: "Damage"
sidebar:
  title: "Manual"
  nav: manual
---

## Core

<div class="mxgraph" style="" data-mxgraph="{&quot;highlight&quot;:&quot;#0000ff&quot;,&quot;lightbox&quot;:false,&quot;nav&quot;:true,&quot;edit&quot;:&quot;_blank&quot;,&quot;xml&quot;:&quot;&lt;mxfile host=\&quot;app.diagrams.net\&quot; modified=\&quot;2022-04-02T06:57:07.785Z\&quot; agent=\&quot;5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.84 Safari/537.36\&quot; etag=\&quot;h6GPWMkqCQ27eSS2qEDG\&quot; version=\&quot;17.3.0\&quot; type=\&quot;google\&quot;&gt;&lt;diagram id=\&quot;UWc7bLVBjTMBWkAKhYBJ\&quot; name=\&quot;Damage\&quot;&gt;7Vpbl5owEP41PnYPIYD46Crdtqfb7qm7vTz1RIhIi8TGeOuvb5BwDV5XBD31QclkQpJv5ptMEluwN1k9UDQdPxIH+y1VcVYt2G+patvU+XcoWEcCoyMELvWcSARSwcD7i4VQEdK55+BZTpER4jNvmhfaJAiwzXIyRClZ5tVGxM/3OkWu6FFJBQMb+VhS++Y5bBxJTT2j/Q577jjuGSiiZoJiZSGYjZFDlhkRtFqwRwlh0dNk1cN+iF2MS9Tu7ZbaZGAUB+yQBvQeGo+QfJjY+PvPF/dFg4PnN8AQg2PreMbY4QCIIqFsTFwSIN9KpfeUzAMHh69VeCnV+UjIlAsBF/7CjK2FNdGcES4as4kvavHKY9/D5ne6KP3I1PRX4s2bwjouBIyuM43C4o9sXdpsU4rbzRglv3GP+IRu5gc7mw+viWYeTncrokI0I3Nq4x0wtoVnIupitgtuNTE8JwwmE8yHyhtS7CPmLfIDQcJ13UQvtS5/EAY+wthilAvkz0VPTxT30SSkQNEL8jZejj2GB1O0AWHJiZ63J/I9N+DPNgcOc5jvRyRgsQ+BuBx5A9AS6BeYMrzaDb6MlWigAxFIRGBRY0ouU5omZBxnKGoqFcGr1solkGFSyqtyLhXNkXAL5JiVEq1Cbu3ljPlKyoimT8TjPSfu047dIHafdsEtIs6LVgXPSIZxurOYEhcH1qe+9WUHEcF+Io4838+YY2Ta2LbLDDU0dU1XzsNE4xAmwhImalUxsSOB+8XqWe+/nhleB2FzVAqvYZt4OKoI3k7d8CYZzf+k4TVJQ5QMHJI1tOvMGuJhZtj0ObiVrAEaB2YNQK2KTQDe7lJQCu9FYxV3nBteCzSldnx1Cd9+97H7YJ3Xe0cjtdx7HWNo6EZF6Gq1o1uyZSMzdivRVwd179nADefhpehe1n1vOhHXa9/nqHLqIOHqcmCnW2cvjk3RMFZXjkWluJGGJajA0oyqMlhKliQRMA2fd34/pDmIjD/z8Ax2E/DezDYRr8sVgDZdbRwnrudPbvhrLbA4Ezg5+IaW8Gzkd0UQZuEe6DwReIejyDasz0bgmuLuUZjGr8nTQlMPixWwfQdzH6MiA8iho9Gh+SQTGA23gSEHqubmzmcgQQcWgG2YPVR55XjuPrzOGhfz7saDq0lAXvTUsH3SsWH7qHPD4ordsHNEtdyhjrtK6VKK1hmFaXhFMsu8uXDTonfyfqorhXvqPTczx+pDbbd+cTwFff4QzfCs1z2qvJGX2HDpTD15RTYLBGVZoF7ZBkbefsupepR1PyGKeKeY7gjHTcy8t5zd7828laowh3LqfXVZxxZQy7MOHTR7YYRqo7KOPdjqVwbuRY5IzuClJYd1F96SQ/mq5MaCMTz1GKS6YHxV1ydHgXqlwVjek9cYjPdh25hgzIvpP1mjpDn9OzC0/gE=&lt;/diagram&gt;&lt;/mxfile&gt;&quot;}"></div>
<script type="text/javascript" src="https://viewer.diagrams.net/js/viewer-static.min.js"></script>

Damage in AAK is a multi stage process that lets a sender, receiver and kind of damage interact. In __PreDamage__ the sender and receiver are informed that a damage is about to occur between them, they both have the chance to completely cancel the damage here. During __OnDamage__ each kind of damage that the sender has is packaged into a damage event that gets send to all the participants. This is where a senders and receivers can modify the damage value and where the DamageKind applies whatever effect the damage has. Finally in __PostDamage__ the sender and receiver have a chance to react to the combined changes of the applied damages.  

An IDamageSender is any component that causes damage occurences. It can be implemented by any class that sends damage. You can Use the static DamageEvent.Send to send your damages. AAK comes with a __TriggerDamageSender__ that uses that regular Unity trigger messages(OnTriggerEnter, ...). By default it only sends damage when a __TriggerDamageReceiver__ first enters but it can also be configured to send damage in intervals or at the end.  

AAK also comes with the DestructibleDamageReceiver which destroys(or replaces) its gameobject when it gets damaged. This is ideal for destructible environment object or any other object that needs to get damages but does not warrant a full character.  

Both the __TriggerDamageSender__ and the __TriggerDamageReceiver__ forward all their damage calls to the character they are attached to(if any). Therefore if we want to react to any kind of damage done to a character we can conveniently do that in the character implementation. The __SuspendSendDamage__ and __SuspendReceiveDamage__ character instructions hook in here.  

Damages can theoretically be anything, what exactly a damage does is decided by the __DamageKind__ implementation. The most common kind of damage is removing a resource(HP) from a characters ResourcePool. AAK comes with the __ResourceDamage__ damage kind which does exactly this.(ContextMenu>Adventure/ResourceDamage)

## Souls

Somewhat more complicated than a lot of other games damage in the souls demo is split into physical and poise damage. Poise damage is generally used to signify the brunt of an attack rather then the pure health malus.  

The base character defines a lot of the damage handling in the demo. Parries are checked in __PreDamageReceive__ so that the damage can be cancelled if a parry succeeds. In __OnDamageReceive__ the damage value is reduced by defense or set to 0 when a character is guarding. In __PostDamageReceive__ it checks if the character dies or is staggered as a result of the damages applied.  

One thing we need to make sure is that a player does not damage itself. We could theoretically just cancel any damage that a player weapon might do to the player in PreDamage but using Layers is far more convenient and performant.  

The AdventureSouls demo has various Layers that control which objects can interact with each other. Check the Physics settings to see how exactly they are configured.  

<p align="center">
  <img src="/assets/images/Layers.png" />
</p>

The player has a general Player Layer that is used for any kind of logical interaction like entering areas or object interactions. The PlayerBody Layer is used to receive damage and interact with physics objects like destroyed crates. In the current demo the body, just like the logic, uses a simple capsule collider. To get more exact hitboxes and more interesting physics interactions the PlayerBody is the one that should be changed to have more detail. Lastly PlayerDamage is used on any weapons the player uses and interacts mostly with EnemyBody.  

The Enemy has a very similar setup to the player with the Enemy, EnemyBody and EnemyDamage Layers. There is also a NeutralDamage Layer that is used by traps so they can send damage to either character. The Trigger Layer is used for logical area to make sure these do not interact with damages or physics at all.

## Hero

Damage in Hero fairly straightforward. The __HeroHealthDamage__ is used by player and enemy attacks as well as environment hazards like the boulder to reduce the health resource of the characters.  

The Layer is also not split within the character, the entire player including the sword and its damage is on the Player layer. In the same way the skeleton lies on the enemy layer.

__HeroStunDamage__ is a special kind of damage caused by the __HeroNut__ throwable that can stun enemies. It does so by raising the __HeroStun__ resource which ultimately adds the __HeroStunEffect__ to the character.

The __HeroBombDamage__ does nothing on its own but is used as a damage filter on the __HeroCrackedWall__ so that the __DestructibleDamageReceiver__ can only be affected by bombs.
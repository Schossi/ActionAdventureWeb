---
permalink: /manual/character
title: "Character"
sidebar:
  title: "Manual"
  nav: manual
---

<div class="mxgraph" style="" data-mxgraph="{&quot;highlight&quot;:&quot;#0000ff&quot;,&quot;lightbox&quot;:false,&quot;nav&quot;:true,&quot;edit&quot;:&quot;_blank&quot;,&quot;xml&quot;:&quot;&lt;mxfile host=\&quot;app.diagrams.net\&quot; modified=\&quot;2022-03-27T14:04:51.685Z\&quot; agent=\&quot;5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.82 Safari/537.36\&quot; etag=\&quot;_gNC1eobzZKJAbwaApXB\&quot; version=\&quot;16.1.0\&quot; type=\&quot;google\&quot;&gt;&lt;diagram id=\&quot;DOsO1HCuQRWFWp32-dpT\&quot; name=\&quot;Character\&quot;&gt;7VrbctowEP0aHtvBNzCPhJBLJ7SZpp0mTx3FXmy1wmJkcXG+vrItX0CQugFj0wkveNeSLJ3dPVqt3TFGs/U1Q3N/Ql0gHb3rrjvGZUfXNa1vi79YE6WanjVIFR7DrmxUKB7wC0hlV2oX2IVwoyGnlHA831Q6NAjA4Rs6xBhdbTabUrL51Dny5BO7heLBQQSUZj+wy/1Ua1ul1jeAPT97staVd2YoaywVoY9cuiqpjHHHGDFKeXo1W4+AxOBluKT9rvbczSfGIOBVOnwa05vH/kvfR8H653dvNPnycvtBs9Jhlogs5Iof6IKEcso8ynBY+ZjDwxw5sbwStu4YFz6fESFp4hIR7AXi2hGTASYUS2AcCxiH8gancY8pDbi0sZitkDEhI0ooSx5iTG0HHEfoQ87obyjdebYt04p7yOmK0WG9Fwgth1f4JdAZcBaJJrJDPzORdEndlvKqMLDRkzq/ZFwj64ikU3n52AXu4kJC/y9mMBUzjCiDZqzgIrCnO63Qc2x4nh7HCkb7rDBQ8AZXkIEUKeM+9WiAyLjQXjC6CFyIR42RLNrc0RjqxCq/gPNI4o0WnG7aDNaYP8bdP+qWFJ9Kty7XcuhEiDIhEOt9LAtPyRBWJhbdEinrV7L8IDdjvMjXjSgwoQvmwGseLLkZMQ/4K+303U7BgCCOl5vzOH6Ydd8tXLuF7UYtrPKojxhyBB1eoPBwQj0G8embxJfLJeLTjB3EZ9bFe7oC2jUEwLCTY9cK3Owt3PpN42bsd7ahw+lxPG5nQB8EpKW9EUi9LiDV7GdClzAT6zkXCAdNQ6jm8bfBUqyGsuhMMDS1pjHsKRgOOWf4ecHhXpw5W4ihab1xL6kNw76C4VdIU4tzgdBsGkJNV2B6z1KrZqmy3PTXLFXb4xWnSVPt/ZlDzNbforkwZQtzLrNqcNSXq9pNBodWigzp5XXGhp7JpRrMKPkd+WhX9WynHXq4S7oOGUNRqcGc4oCHpZHvY0Xhhb0tL8wyh6uK7TNKL/wunUHhhflSDmBt9eg5DPAMcXBbdwTNwzgL613pq3nKsNZ673texVitvMMNKrvFiWoz6q6XvGtoXXj0W1ehyWy5Dd09QRGwVpVp+q0r02TWK4F3sZhOgZWYOanXHAzf8U8ng9bVanR1n8tRHIn1M0oIsKx+cw6QNl670XUF0jscisQhL+EcG8atnLKb/OqBt/myjr6HPMcBzKJWc2fzxy1D5c4JjtNacnbUWRnM2hzRUKnzM1pOIPSHnljUGXGm2WscS5Uzay14v34KPy64O76EODG46puthDEnlAU48EJx9mgjXVpVt5r66HLHm6wzpcvKYNbnheo7rTpfC54wwi2jcWzVd13/C31aVn3gCrH4YDOtHBafvRrjPw==&lt;/diagram&gt;&lt;/mxfile&gt;&quot;}"></div>
<script type="text/javascript" src="https://viewer.diagrams.net/js/viewer-static.min.js"></script>

## Core

 Characters are the brains of the operation for any entity in the world be it player or npc. They tie all the other concepts together and implement any logic the belongs to a specific character but does not fit any of the other concepts.(input for players, ai for enemies)  

CharacterBase has explicit fields for AttributePool and ResourcePool since these probably wont be inherited from. The actor, movement and inventory implementation can be freely defined when inheriting. CharacterBaseTyped/AnimatedCharacterBase define the fields for theses in a generic manner which makes them convenient to inherit from.  

 The character base implements a very simple messaging channel that can be used for events that may be used by every other system on the character. Typically messages are received from things like animators or timelines and then used by the current action(animation end>action end) or something like the AudioManager(play sound on animation foot down).

### Instructions

A CharacterInstruction defines some type of state change for a character in a way that can be set and reset. This is done to deal with situation where multiple systems modify the same properties without knowing about each other. For example modifications to the movement speed or the visibility of an item slot. AdventureCore defines some useful general instructions like multipliers for attributes or stats and suspending damages, collisions or movement. See the header of the different instructions for a more detailed explanation of each one.  

CharacterInstruction(or CharacterInstruction[]) can be used as an inspector field despite being abstract because AdventureCore comes with a PropertyDrawer that allows choosing the actual implementation in the inspector.

## Souls

The souls demo implements SoulsCharacterBase which provides some common stagger, guard and parry logic. The SoulsPlayerCharacter manages input(movement, lockon, menu, ...), xp recovery and character reset(bonfire). SoulsEnemyCharacter is a very simple enemy that approaches the player and attacks and the SoulsMorningstar character is a specific boss enemy that chooses its attacks based on the position of the player.
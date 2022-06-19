---
permalink: /manual/releases
title: "Release Notes"
sidebar:
  title: "Manual"
  nav: manual
---

## 1.0.1

__The minimum supported version of Unity for AAK is now 2021.3.4__

### ADDED

- documentation for SoulsPlayer prefab including a step by step guide for replacing the model

### CHANGED

- Animator is now part of CharacterBase, AnimatedCharacterBase is therefore obsolete

### IMPROVED

- simplified model replacement for SoulsPlayer
  - colliders moved to separate GameObject 'Body'
  - TimelineAction and SoulsBonfireAction can pass character animator to the PlayableDirector which lets us remove the dependency between environment and player model
- translation and rotation of model can be suppressed independently
  - SoulsAttackAction no longer locks rotation until damage is activated
- MovementSaver can be overridden from outside
  - BossArena uses this to properly reset the player if the game is quit during a fight

### FIXED

- usable items not being hidden if action is interrupted
- potential initialization order errors
  - AttributePool value/stat dictionaries
  - CharacterActionArea Bindings
- armor no longer vanishes at certain camera angles

## 1.0.0

__Initial Release__
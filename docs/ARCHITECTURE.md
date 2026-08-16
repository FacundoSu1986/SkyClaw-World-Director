# Architecture

> Part of [SkyClaw World Director](../README.md). See also [`ROADMAP.md`](ROADMAP.md).

## Status

Research / Proof of Concept. The architecture below is a target model, not a statement that these components are already implemented.

## Responsibility split

```text
External planning / persistence
          |
          v
+-------------------------+
| SkyClaw World Director  |
+-------------------------+
          |
          | validated commands / events
          v
+-------------------------+
| SKSE Runtime Bridge     |
+-------------------------+
          |
          v
+-------------------------+
| Skyrim Runtime          |
+-------------------------+
```

### SkyClaw World Director

Owns:

- campaign/world state;
- NPC needs and goals;
- relationships and memories;
- abstract simulation;
- encounter planning;
- narrative state;
- validation policies;
- runtime budgets;
- replanning.

It must not directly manipulate arbitrary Skyrim memory or issue unrestricted engine calls.

### SKSE Runtime Bridge

Intended responsibilities:

- observe supported Skyrim runtime events;
- expose a narrow, versioned command interface;
- validate actor/reference availability before execution;
- move data between Skyrim and the external process;
- fail safely if the external process is unavailable.

The bridge should be as small as practical. Hooks and direct memory patches are not a first-choice mechanism.

### Skyrim

Remains authoritative for:

- actor existence and loaded state;
- pathfinding;
- movement;
- animation;
- combat;
- physics;
- actual runtime outcomes.

## Proposed director model

```text
World Director
|
+-- NPC Life Director
|   +-- needs
|   +-- goals
|   +-- flexible routines
|   +-- relationships
|   +-- memories
|
+-- Ambient Life Director
|   +-- travelers
|   +-- animals
|   +-- small persistent groups
|
+-- Faction Director
|   +-- coarse activity
|   +-- hostility
|   +-- regional pressure
|
+-- Encounter Director
|   +-- contextual encounters
|   +-- runtime budget
|   +-- materialization rules
|
+-- Narrative Director
|   +-- conflicts
|   +-- secrets
|   +-- rumors
|   +-- consequences
|
+-- Simulation LOD
    +-- physical near player
    +-- abstract when distant
```

## NPC control policy

Actors should be classified before any runtime action is allowed.

```text
PROTECTED
  Observe only by default.
  Quest-critical or canonical actors.

LIMITED
  Small temporary routine changes.
  No destructive or permanent changes.

AMBIENT
  Greater scheduling and travel freedom.

GENERATED
  Highest behavioral freedom because the actor is owned by this system.
```

The exact classification rules are not yet verified.

## Intent-based execution

The director should persist **why** an actor is doing something, not only its current location.

Example conceptual state:

```json
{
  "actor": "SC_NPC_0042",
  "goal": "increase_income",
  "reason": "wealth_below_target",
  "action": "extend_work_hours",
  "expires_after_days": 3,
  "fallback": "normal_schedule"
}
```

This permits debugging, replanning and recovery after save/load.

## Simulation LOD

### Near the player

Use Skyrim's actual runtime:

- pathfinding;
- movement;
- combat;
- animation;
- interaction.

### Far from the player

Use abstract state transitions rather than keeping actors physically active.

Example:

```text
travelling_group_043
origin      = Rorikstead
destination = Riverwood
progress    = 0.72
members     = dog_01, goat_01, chicken_01
```

When the player approaches a relevant area, the runtime layer may materialize a compatible representation.

This materialization mechanism is **not yet verified**.

## Encounter model

An encounter is not simply a random spawn. It should have:

```text
CAUSE
PARTICIPANTS
GOAL
LOCATION / TIME WINDOW
CONDITIONS
RUNTIME BUDGET
RESULT
CONSEQUENCES
```

Examples under exploration:

- vampires versus werewolves;
- Dawnguard versus vampires;
- Silver Hand versus werewolves;
- hunters versus wildlife;
- merchants attacked on the road;
- companions or mercenaries on jobs;
- non-combat social or trade encounters.

## Performance policy

The system should maintain its own conservative limits, independent of whatever Skyrim might technically tolerate.

Candidate controls:

- maximum active dynamic encounters;
- maximum dynamic actors near the player;
- cooldown between materialized encounters;
- suppression when vanilla combat or heavy scenes are already active;
- abstract simulation when physical materialization is unnecessary.

Actual thresholds must be established through measurement, not guessed.

## Persistence

Target behavior:

```text
NEW GAME
  -> create campaign identity
  -> initialize world seed/state
  -> persist

SAVE / LOAD
  -> recover campaign
  -> reconcile intended state with actual Skyrim runtime
  -> continue or replan
```

The exact persistence mechanism is undecided. SKSE serialization, an external database, or a hybrid approach must be evaluated experimentally.

## Failure model

If the external director is offline or crashes:

- Skyrim should continue;
- no unbounded blocking call should occur on the game thread;
- pending dynamic actions should fail closed or return to safe defaults;
- persisted state should not be silently rewritten from guesses.

## Non-goals for the first prototype

- controlling every NPC;
- frame-by-frame LLM control;
- rewriting vanilla quests;
- arbitrary memory patching;
- spawning large battles;
- generating Papyrus dynamically during gameplay;
- claiming compatibility before runtime tests exist.

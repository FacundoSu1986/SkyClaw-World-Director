# Roadmap

> Part of [SkyClaw World Director](../README.md). See also [`ARCHITECTURE.md`](ARCHITECTURE.md).

## Principle

Progress is gated by reproducible technical evidence. Later simulation/narrative layers do not begin until the lower runtime layer is proven stable.

## Phase 0 — Hello SKSE

**Goal:** prove that a minimal CommonLibSSE-NG/SKSE plugin can be built, loaded and removed safely.

Acceptance criteria:

- [ ] DLL builds reproducibly.
- [ ] SKSE loads it.
- [ ] Skyrim reaches the main menu without CTD.
- [ ] Plugin records a successful initialization/lifecycle signal.
- [ ] New game still works.
- [ ] Existing save still loads.
- [ ] Skyrim exits normally.
- [ ] Removing/disabling the DLL restores the previous setup.

## Phase 1 — Read-only runtime

**Goal:** observe Skyrim without changing state.

Candidate data:

- player runtime identity;
- current cell/location;
- combat state;
- selected safe lifecycle events.

Acceptance criteria:

- [ ] No game-state writes.
- [ ] No blocking external calls on Skyrim's main thread.
- [ ] Runtime data is structured and versioned.

## Phase 2 — Outbound IPC

**Goal:** send structured runtime events from Skyrim to a local external process.

Acceptance criteria:

- [ ] Local-only transport for the prototype.
- [ ] Bounded messages.
- [ ] Connection loss does not affect Skyrim.
- [ ] Protocol version included.

## Phase 3 — Inbound harmless command

**Goal:** prove safe bidirectional communication.

Example class of action:

```text
SHOW_NOTIFICATION
```

Acceptance criteria:

- [ ] Explicit allow-list of commands.
- [ ] Invalid command rejected.
- [ ] External timeout cannot freeze the game.
- [ ] No arbitrary console command or memory address execution.

## Phase 4 — Single test actor

**Goal:** experiment with one deliberately safe/non-critical actor.

Acceptance criteria:

- [ ] Actor eligibility checked.
- [ ] One bounded intention.
- [ ] Cancel/fallback behavior.
- [ ] Original behavior can be restored.

No quest-critical actor is used for the first test.

## Phase 5 — Persistence across save/load

**Goal:** preserve one actor intention across a real save/load cycle.

Target model:

```text
goal -> plan -> runtime state -> save -> load -> reconcile -> continue/replan
```

Acceptance criteria:

- [ ] Campaign identity remains stable.
- [ ] Stale runtime references are not trusted blindly.
- [ ] The system explains why the actor has its current plan.
- [ ] Corrupt/missing external state fails safely.

## Phase 6 — NPC Life prototype

Small test population, e.g. five actors.

Candidate needs:

- money;
- rest;
- food;
- social;
- safety;
- personal objective.

Candidate actions:

- work;
- extend/reduce work hours;
- eat;
- sleep;
- visit;
- travel;
- wait;
- return home.

LLM usage remains optional and bounded.

## Phase 7 — Ambient Life prototype

Examples:

- ordinary travelers;
- hunters;
- small animal groups;
- persistent dog + goat + chicken travelling group.

Use abstract simulation when distant.

## Phase 8 — Encounter Director

Introduce contextual world events with strict budgets.

Requirements:

- cause;
- participants;
- conditions;
- runtime budget;
- outcome;
- consequences.

Combat is only one encounter category.

## Phase 9 — Faction state

Track coarse activity and conflict opportunities without simulating every faction member.

Potential examples:

- vampire activity;
- werewolf activity;
- Dawnguard pressure;
- Silver Hand activity;
- bandit pressure.

## Phase 10 — Narrative layer

Add:

- persistent conflicts;
- secrets;
- rumors;
- NPC knowledge versus world truth;
- consequences from actual runtime events.

The LLM proposes narrative developments; persisted state remains authoritative.

## Phase 11 — Integrated World Director

Only after the previous phases are measured and stable:

```text
NPC Life
+ Ambient Life
+ Faction State
+ Encounter Director
+ Narrative Director
+ Simulation LOD
= World Director
```

## Deferred / explicitly not promised

- universal compatibility with mod lists;
- autonomous control of all vanilla NPCs;
- safe manipulation of arbitrary AI packages entirely in memory;
- permanent relocation of quest-critical actors;
- high-density dynamic battles;
- zero-overhead runtime simulation.

These remain **not verified** until proven experimentally.

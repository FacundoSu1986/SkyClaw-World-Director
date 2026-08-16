# SkyClaw World Director

[![Docs](https://github.com/FacundoSu1986/SkyClaw-World-Director/actions/workflows/docs.yml/badge.svg)](https://github.com/FacundoSu1986/SkyClaw-World-Director/actions/workflows/docs.yml)

> **Status:** Research / Proof of Concept  
> **Initial publication:** August 2026

SkyClaw World Director is an experimental concept for a persistent runtime simulation layer for **The Elder Scrolls V: Skyrim Special Edition / Anniversary Edition**.

The project explores a world in which selected NPCs, ambient groups and factions can maintain goals, relationships, routines and consequences throughout a playthrough instead of behaving only as fixed schedules waiting for the player.

This repository is intentionally separate from the main Sky-Claw project while the idea is researched and validated.

## Core idea

```text
                  SKYCLAW WORLD DIRECTOR
                           |
          +----------------+----------------+
          |                |                |
     NPC Life         Encounter        Narrative
     Director         Director         Director
          |                |                |
          +----------------+----------------+
                           |
                    Runtime Bridge
                           |
                          SKSE
                           |
                         Skyrim
```

Sky-Claw should decide **intent**; Skyrim should remain responsible for movement, pathfinding, animation, combat and other engine-level execution.

The LLM is not intended to control actors frame by frame. It may assist with planning or narrative decisions while deterministic systems preserve state, safety and consistency.

## Concepts under exploration

- Persistent state associated with a playthrough.
- NPC needs, goals and flexible routines.
- Merchants adapting work hours or travel plans to circumstances.
- Selected non-critical NPCs travelling between settlements.
- Relationships, memories, rumors and consequences that persist.
- Ambient travelling groups, including animals and ordinary travelers.
- Contextual encounters between compatible factions or groups.
- Abstract simulation for actors far from the player.
- Runtime materialization only when an event becomes relevant to the loaded world.
- Protection policies for quest-critical and canonical NPCs.
- Event-driven replanning when Skyrim produces an unexpected outcome.
- A bounded runtime budget to avoid saturating Skyrim's engine.

## Design constraints

1. **Skyrim remains authoritative for runtime facts.**
2. **The external director never controls actors frame by frame.**
3. **Quest-critical NPCs are protected by default.**
4. **Generated or explicitly eligible NPCs receive greater behavioral freedom.**
5. **Simulation outside the loaded area should be abstract whenever possible.**
6. **Every runtime action must have a bounded, validated command contract.**
7. **Failure of the external director must not prevent Skyrim from continuing.**
8. **Persistence must survive save/load without silently inventing state.**
9. **The LLM proposes; validated deterministic state remains authoritative.**

## First technical milestone

Before any narrative or autonomous-NPC system is implemented, the project must prove a minimal SKSE runtime bridge:

```text
1. SKSE loads the plugin DLL.
2. The plugin receives a safe lifecycle event.
3. Skyrim runtime information can be read without modifying game state.
4. A local external process can receive structured data.
5. Bidirectional communication can be demonstrated with a harmless command.
6. Removing the plugin restores the previous runtime behavior.
```

Only after that gate succeeds should the project experiment with a single non-critical test actor and persistence across save/load.

## Planned components

- **SKSE Runtime Bridge** — safe boundary between Skyrim and the external director.
- **NPC Life Director** — needs, goals, routines and individual plans.
- **Ambient Life Director** — lightweight travelers, animals and environmental activity.
- **Faction Director** — coarse faction state and contextual interaction opportunities.
- **Encounter Director** — bounded, contextual world events rather than blind random spawning.
- **Narrative Director** — persistent conflicts, secrets and consequences built on actual world state.
- **Simulation LOD** — physical simulation near the player, abstract simulation elsewhere.

See [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) and [`docs/ROADMAP.md`](docs/ROADMAP.md).

## Documentation

| Document | Purpose |
| --- | --- |
| [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) | Target architecture, responsibility split and safety policies. |
| [`docs/ROADMAP.md`](docs/ROADMAP.md) | Evidence-gated phases, from `Hello SKSE` to an integrated World Director. |
| [`docs/adr/`](docs/adr/README.md) | Architecture Decision Records for choices that are hard to reverse. |
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | How research, evidence and changes are expected to be submitted. |
| [`SECURITY.md`](SECURITY.md) | Reporting process and the security boundaries this project assumes. |
| [`CHANGELOG.md`](CHANGELOG.md) | Notable changes to the concept and, later, to the implementation. |

## Repository layout

```text
.
├── docs/
│   ├── ARCHITECTURE.md   target architecture
│   ├── ROADMAP.md        evidence-gated phases
│   └── adr/              architecture decision records
├── .github/              issue templates, PR template, CI workflows
├── CONTRIBUTING.md
├── SECURITY.md
├── CODE_OF_CONDUCT.md
└── CHANGELOG.md
```

No source tree exists yet. Code directories will be added only when a phase in the roadmap actually reaches implementation.

## Scope

This repository currently records and validates the concept. It is **not yet a production Skyrim mod**, does not currently claim compatibility with existing mod lists, and does not yet provide autonomous NPC control.

No implementation claim in this repository should be considered verified until backed by a reproducible runtime spike and documented test results.

## Relationship to Sky-Claw

World Director is being explored as a possible future companion/runtime component of Sky-Claw. It is kept separate so research into SKSE, C++, runtime state and simulation cannot destabilize the main project while the architecture is still experimental.

## License

No open-source license has been selected for this repository at this stage.

Until a license file is added, default copyright applies: the content is published for reading and discussion, and no permission to reuse, modify or redistribute it is granted. Choosing a license is tracked as an open decision in [`docs/adr/`](docs/adr/README.md).

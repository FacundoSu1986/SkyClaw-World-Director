# Architecture Decision Records

This directory records decisions that are expensive to reverse: the transport used
between Skyrim and the external director, the persistence mechanism, the actor
classification rules, and the license for this repository.

The point is not ceremony. It is that this project gates progress on evidence, and an
ADR is where the evidence behind a choice — and the alternatives that were rejected —
stays readable months later.

## Conventions

- One file per decision, named `NNNN-short-title.md`, numbered sequentially.
- Start from [`template.md`](template.md).
- A record is never edited to change its decision. To reverse one, add a new record and
  set the old one's status to `Superseded by ADR-NNNN`.
- Statuses: `Proposed`, `Accepted`, `Rejected`, `Deprecated`, `Superseded by ADR-NNNN`.

## Index

| ID | Title | Status |
| --- | --- | --- |
| — | No decisions recorded yet. | — |

## Open decisions

These are known choices that have deliberately **not** been made yet. They are listed
here so the absence is visible rather than accidental.

- Transport for the Skyrim ↔ director IPC (roadmap Phase 2).
- Persistence mechanism: SKSE serialization, external store, or hybrid.
- Actor classification rules for `PROTECTED` / `LIMITED` / `AMBIENT` / `GENERATED`.
- License for this repository.

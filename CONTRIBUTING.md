# Contributing

SkyClaw World Director is in a research / proof-of-concept stage. The most valuable
contributions right now are evidence, not features.

## The one rule

**No claim enters this repository without reproducible evidence behind it.**

The roadmap is gated on purpose: each phase has acceptance criteria, and a later phase
does not start until the phase below it is demonstrably stable. A change that says a
mechanism works must link to a spike anyone can re-run, with the environment it ran in
and the result it produced. A change that describes something not yet tested must label
it `not yet verified`, the way the existing documents do.

## What is useful

- Runtime spikes against SKSE / CommonLibSSE-NG that satisfy (or fail) a roadmap
  acceptance criterion.
- Measurements that turn a guessed threshold into a real one — the performance policy in
  [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) currently has none.
- Counter-evidence. A documented reason a proposed mechanism cannot work is worth more
  than an untested design that assumes it can.
- Corrections to documents that overstate what is known.

## What is out of scope for now

The non-goals in [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) and the deferred list in
[`docs/ROADMAP.md`](docs/ROADMAP.md) apply to contributions too. In particular: no
arbitrary memory patching, no frame-by-frame actor control, no compatibility claims
about mod lists, and no changes that would let the external process execute unbounded
commands inside Skyrim.

## Working on a change

1. Open an issue first for anything beyond a typo — a research spike, a documentation
   correction, or a bug. The issue templates ask for the questions that matter.
2. Branch from the default branch. Use a short descriptive name (`spike/skse-load`,
   `docs/architecture-persistence`).
3. Keep commits focused. Write the message as
   [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/):
   `docs: ...`, `feat: ...`, `fix: ...`, `chore: ...`, `test: ...`.
4. Open a pull request and fill in the template. State what you verified and what you
   did not.

## Documentation style

- Markdown, one sentence per idea, no hard-wrapped columns.
- ASCII diagrams in fenced `text` blocks, matching the existing style.
- Present tense for what exists; conditional (`should`, `may`) for what is proposed.
- `markdownlint` runs in CI. Run it locally before pushing:

  ```bash
  npx --yes markdownlint-cli2 "**/*.md"
  ```

## Decisions

Choices that are hard to reverse — the IPC transport, the persistence mechanism, the
actor classification rules, the license — belong in [`docs/adr/`](docs/adr/README.md) as
an Architecture Decision Record, not only in a pull request description.

## Code of conduct

Participation is covered by the [Code of Conduct](CODE_OF_CONDUCT.md).

## Licensing note

This repository has no open-source license yet. Until one is chosen, contributions are
accepted for discussion and research, and the licensing terms of any contributed
material should be agreed in the issue before a pull request is merged.

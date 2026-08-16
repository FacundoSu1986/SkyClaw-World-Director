## What changed

<!-- One paragraph. What does this pull request actually do? -->

## Why

<!-- Link the issue, roadmap phase or ADR this belongs to. -->

Related: #

## Evidence

<!--
This project gates progress on reproducible evidence. Fill in what you actually ran.
For a documentation-only change, write "documentation only" and delete the rest.
-->

- What was run:
- Environment (Skyrim SE/AE build, SKSE version, other plugins loaded):
- Result:

## Verified / not verified

<!-- Be explicit. An unverified claim that is labelled as unverified is fine; an
unverified claim presented as fact is not. -->

- Verified:
- Not verified:

## Checklist

- [ ] Claims in the diff are either backed by evidence above or labelled `not yet verified`.
- [ ] No non-goal from `docs/ARCHITECTURE.md` is introduced (no arbitrary memory patching,
      no frame-by-frame actor control, no unbounded command surface, no compatibility claims).
- [ ] Roadmap acceptance criteria are updated if this change satisfies or invalidates one.
- [ ] An ADR was added under `docs/adr/` if this makes a hard-to-reverse decision.
- [ ] `CHANGELOG.md` updated under `Unreleased`.
- [ ] Markdown lint passes (`npx --yes markdownlint-cli2 "**/*.md"`).

<!-- GitHub renders this template inside the pull request body, so it starts at h2 by design. -->
<!-- markdownlint-configure-file { "MD041": false } -->

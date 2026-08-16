# Security Policy

## Status

This project is a research prototype. It does not currently ship a binary, a plugin, or
a released artifact. This policy describes the boundaries the design assumes and how to
report a problem once code exists.

## Reporting a vulnerability

Report privately through GitHub's
[private vulnerability reporting](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing-information-about-vulnerabilities/privately-reporting-a-security-vulnerability)
on this repository, rather than opening a public issue.

Please include what you did, what happened, and the environment (Skyrim SE/AE build,
SKSE version, and whether other plugins were loaded). Expect an acknowledgement within a
few days; this is a small research project, not a staffed product.

## Threat model assumptions

The architecture puts an external process next to a game process, which is exactly the
kind of boundary that gets abused if it is left loose. The following are treated as
security requirements, not just design preferences:

- **Local-only transport during the prototype.** No listening socket exposed beyond the
  local machine, and no remote control path.
- **Explicit command allow-list.** The runtime bridge executes only commands it knows.
  Anything unrecognised is rejected, not passed through.
- **No arbitrary execution.** No console command execution, no memory address supplied
  from outside the plugin, no dynamically generated Papyrus at runtime.
- **Bounded messages.** Message sizes and rates are capped so a misbehaving or hostile
  peer cannot exhaust the game process.
- **Fail closed.** If the external director is unavailable, times out, or sends
  something invalid, Skyrim continues and the pending action is dropped or reverted to a
  safe default. No unbounded blocking call is made on the game thread.
- **Untrusted LLM output.** Anything an LLM proposes is data, not a command. It is
  validated against the deterministic state and the allow-list before it reaches the
  bridge.

A violation of any of the above is a security bug in this project, even if it does not
crash anything.

## Out of scope

- Vulnerabilities in Skyrim, SKSE, CommonLibSSE-NG or third-party mods. Report those
  upstream.
- Modifying your own game installation. That is what a mod does.

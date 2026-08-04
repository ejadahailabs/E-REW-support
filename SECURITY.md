# Security Policy

## Reporting a vulnerability

**Do not open a public issue.**

Email **ejadahailabs@gmail.com** with `[E-REW SECURITY]` in the subject.

Include what you found, how to reproduce it, and what an attacker could achieve. If you
have a proof of concept, include it — we will not treat a good-faith report as an attack.

| Stage | Target |
|---|---|
| Acknowledgement | 72 hours |
| Assessment and plan | one week |
| Fix for a confirmed high-severity issue | as fast as we can, and we will tell you the date |

We will credit you in the release notes unless you ask us not to.

## Scope

E-REW runs entirely on your machine and reads your repository. It has no server, no
telemetry, and no account.

Especially interesting to us:

- **Code execution from repository content.** Templates, rule packs and skills are
  **data, never code** — a repository opened from an untrusted source must not be able to
  execute anything. A path around that is our most serious class of bug.
- **Anything that writes a file the user did not open.** Architecturally forbidden.
- **Credential exposure.** The AI gateway takes an API key from VS Code's secret storage;
  it must never reach a log, a finding, a report, or an engine that did not ask for it.

## Supported versions

Pre-1.0: the latest release only. Once 1.0 ships, this section states a support window.

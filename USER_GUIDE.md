# E-REW User Guide

**E-REW — Requirements Engineering Workbench.** Part of the Ejadah Engineering
Intelligence Platform, by Ejadah AI Labs.

---

## 1. What E-REW is

Your requirements are **Markdown files with YAML frontmatter, in your Git repository**.
E-REW adds authoring, validation and engineering analysis on top of them.

It is **not** a Markdown editor, and **not** an AI chat window. It is an analysis tool
that happens to author requirements.

Three consequences you will notice immediately:

- **Your data outlives us.** Delete E-REW and you still have readable requirements in
  Git. There is no database, no export step, no lock-in.
- **Review, branch, merge and blame already work.** Requirements are files.
- **Every answer is reproducible.** Analysis at a commit produces the same findings,
  byte for byte, on any machine.

## 2. Install

Marketplace, or sideload a `.vsix`:

```
code --install-extension e-rew-0.3.0.vsix
```

Open a folder containing requirements. E-REW activates when it finds a REW repository.

## 3. Your first repository

A REW repository needs two things:

```
.ejadah/rew/templates/   one Markdown file per requirement type
requirements/            your requirements
```

Everything E-REW owns lives under `.ejadah/` — templates, rule packs, configuration — so
the rest of your repository stays yours. Requirements live wherever you like; each
template names its own folder.

**A template is a prototypical requirement.** It looks exactly like the requirement it
produces — frontmatter keys are the fields, `## headings` are the sections, the body is
the placeholder text. To write one, paste in a good requirement and blank the values.

```markdown
---
id: ""
type: "llr"
status: "draft"
uplinks: []

rew:
  label: "Low Level Requirement"
  idPrefix: "REQ-LLR"
  folder: "requirements/llr"
  roles: {}
  required: ["## Description", "uplinks"]
---

# Requirement Name

## Description

TODO
```

**`required:` is your checklist.** Every entry is a mandatory item, and anything missing
or still holding its placeholder becomes an error on that requirement. This is the first
place to add a house rule — "every LLR must state a Rationale and trace up" is one line,
no rule pack and no waiting for us. Name a `## Section` heading or a frontmatter key.

**The filename is the ID.** `REQ-LLR-042.md` *is* `REQ-LLR-042`. This is why two people
creating the same ID on different branches get a Git merge conflict instead of a silent
duplicate — Git is the duplicate check.

### Choosing your ID style

Two independent choices. Most teams change neither.

**Who issues the ID** — `.ejadah/rew/config.yaml`, or the *Requirement IDs* section of
the configuration panel:

```yaml
ids: "generated"   # REW allocates the next free number. The default.
ids: "provided"    # you supply it — New Requirement asks for the ID first.
```

Choose `provided` when the authority that numbers requirements is outside this
repository: a customer's baseline, a programme-wide scheme, a subcontract deliverable
list, or a migration from DOORS or a spreadsheet that already carries IDs. An ID already
in use is **refused, not renumbered** — silently receiving a different ID from the one
you typed would create the wrong requirement, and you would find out at the next baseline
comparison.

**What an ID may look like** — `idPattern` in a type's template:

```yaml
rew:
  idPattern: "^[a-z][a-z0-9-]*$"    # braking-response-time.md
```

The default grammar is `REQ-SYS-001`-shaped. A readable slug works just as well, with one
trade worth knowing: a readable ID invites renaming when the wording changes, and a
rename breaks every inbound uplink on purpose (§11). An opaque number is stable precisely
because it means nothing — which is why certification programmes use them.

Either way **the filename stays the ID**, which is what keeps Git's own merge conflict as
the duplicate check.

## 4. Roles — the one concept worth ten minutes

**E-REW defines no metadata schema. Your templates do.** Only `id` and `type` are fixed.

Every other field is yours, and may carry a **role** saying what it *means*:

```yaml
rew:
  roles:
    satisfies: "uplink"        # our parent link is called "satisfies"
    dal: "criticality"         # our criticality field is called "dal"
    implementedBy: "implements"
```

Analysis queries **roles, never field names**. So an organization whose parent link is
`satisfies` gets full traceability analysis with no configuration and no release from us.

### The rule that surprises people

> **An undeclared role disables the analysis that needs it.**

No `hazard` role means **no safety findings** — not "100% hazard coverage". If a check
you expected is missing, the first thing to check is whether its role is declared. E-REW
tells you which roles would enable a check that stood down.

This is deliberate. A confidently wrong number on a safety dashboard is the worst thing
this product could produce, so silence with a stated reason is the only honest answer.

## 5. Authoring

| Action | How |
|---|---|
| New requirement | Right-click a type in the REW panel → **New Requirement** |
| Jump to a requirement | Ctrl+click any ID in a body |
| See a requirement without opening it | Hover the ID |
| Fill a link field | Type `REQ` in `uplinks:` and accept a completion — only real IDs are offered |
| Delete | Right-click → Delete |

**Two safety behaviors you should know about:**

- Deleting a requirement that others trace to will **name them** and the button reads
  **Delete and break uplinks**. The broken links then appear as findings. E-REW does
  *not* silently repair other files — a requirements tool that edits files you did not
  open is one nobody can certify.
- Deleting a non-empty folder is **refused** at the syscall level. There is no recursive
  delete anywhere in E-REW, not even behind a confirmation.

## 6. Analysis

Run **REW: Run Analysis** (or let it run automatically). Findings appear as squiggles and
in the Problems panel.

Eleven engines ship today:

| Engine | Answers |
|---|---|
| Validation | Is this requirement structurally sound? |
| Quality | Is the wording testable, unambiguous, singular? |
| Traceability | What is unlinked, orphaned, or in a cycle? |
| Structure | Is the decomposition sane? |
| Verification | What is unverified? |
| Implementation | What has no code — and where do code and requirements disagree? |
| Safety | Which hazards are unmitigated? |
| Security | Which threats are uncountered? |
| Architecture | What is unallocated? |
| Consistency | Is the same thing called two things? |
| Impact | What does changing this reach? |

**Findings marked as candidates.** Anything derived from prose matching rather than a
declared field renders as *a candidate for human judgement*, never as established fact.
You can trust the unmarked ones absolutely; the marked ones are worth a look.

## 7. Rule packs — tuning without waiting for us

A rule pack changes which checks run, how loud they are, and what words and numbers they
use. Adding one is **two steps, and skipping the second is the commonest mistake**:

```yaml
# 1. write .ejadah/rew/rules/house.yaml
id: "house"                  # MUST match the filename — house.yaml
title: "Our engineering standard"
extends: "default"           # start from the built-in pack, override from there

severities:
  weak-term: "error"         # a warning by default
  passive-voice: "off"       # silenced entirely
  not-implemented: "off"

quality:
  requireModal: "shall"      # every requirement must use this modal verb
  weakTerms: ["in a timely manner", "adequate", "user-friendly"]

thresholds:
  maxSentenceWords: 30
```

```yaml
# 2. select it in .ejadah/rew/config.yaml — without this line, nothing changes
profile: "house"
```

### The four severities

`error` · `warning` · `info` · `off`. Nothing else loads.

### The knobs a pack may set

| Knob | What it changes |
|---|---|
| `requireModal` | the modal verb every requirement must use — `"shall"`, `"must"` |
| `weakTerms` | the vague words that raise `weak-term` |
| `escapeClauses` | phrases like "where possible" that raise `escape-clause` |
| `optionalTerms` | words like "may" that raise `optionality` |
| `maxSentenceWords` | sentence length before `readability` fires |
| `maxFanout` / `minFanout` | children per parent before `over-` / `under-decomposition` |
| `approvedStatuses` | which status values count as finished, for `childless-approved` |
| `dispositionsNeedingRationale` | which verification dispositions owe an argument |
| `properties` | the security properties `missing-security-property` looks for |
| `rareTermMaxDocs`, `maxBucketSize`, `minSimilarity` | how aggressively duplicates are compared |

A misspelled knob is a **load error with a line number**, never a silent no-op — a knob
you believe is set and analysis never applied is worse than no knob at all.

### Different strictness for different criticality

If your repository declares a criticality scale, a pack can band on it. Selectors are
`">=4"`, `"<=1"`, `"2"` or `"2..3"` over rigour 0–4:

```yaml
bands:
  ">=4":                      # the safety-critical end
    rules:
      weak-term: "error"
      testability: "error"
    requiredRoles: ["hazard", "verification"]
    allowSuppression: false
  "<=1":
    rules:
      weak-term: "info"
```

Two bands may not overlap. E-REW refuses the file rather than picking a winner: a rule
set whose severity depends on which line came first cannot be reviewed.

### The checks you can name

<details>
<summary>61 rule ids, by engine</summary>

| Engine | Rules |
|---|---|
| **validation** | `bad-id` `id-prefix` `id-mismatch` `unknown-type` `missing-field` `malformed-file` `empty-file` `duplicate-section` `unknown-section` `broken-link` |
| **quality** | `weak-term` `escape-clause` `optionality` `passive-voice` `ambiguity` `atomicity` `completeness` `testability` `readability` `structured-statement` |
| **traceability** | `broken-uplink` `self-uplink` `wrong-uplink-level` `orphan` `circular-traceability` `childless-approved` |
| **structure** | `missing-decomposition` `over-decomposition` `under-decomposition` `duplicate-decomposition` `parent-child-inconsistency` |
| **verification** | `missing-case` `missing-method` `missing-procedure` `disposition-without-rationale` |
| **consistency** | `duplicate-requirement` `conflicting-requirements` `inconsistent-unit` `comparison-capped` |
| **safety** | `unmitigated-hazard` `dal-inconsistency` `missing-safety-rationale` `undeclared-hazard` `single-point-failure` |
| **security** | `uncountered-threat` `missing-security-property` `missing-security-rationale` `undeclared-threat` |
| **implementation** | `not-implemented` `partially-implemented` `untraced-code` `missing-symbol` `unindexed-symbol` `implements-disputed` `implements-unacknowledged` `implements-withdrawn` `dead-requirement` |
| **architecture** | `unallocated-requirement` `duplicate-functionality` |
| **impact** | `wide-impact` `impact-incomplete` |

</details>

### What a pack cannot do

**A pack tunes checks that exist. It cannot author a new one.** There is no expression
language, deliberately: an expression in a pack is a policy interpreter you would have to
debug at the worst possible moment. If you need a check E-REW does not have — "every
requirement citing a standard clause must name a test case" — that is a new rule in the
product, and a conversation with us rather than a file you write.

Two things a template *can* do that people reach for a pack to do first: making a field
mandatory (`required:`, §3) and naming what a field means (`roles:`, §4).

### Why packs are safe to accept from anyone

Rule packs are **data, not code**. E-REW never executes anything found inside a
repository — which matters the first time you open one from a network share.

The one thing a repository *can* hand E-REW that runs is a **regular expression**: a
template's `idPattern`. Those are screened before use — a pattern that could take minutes
to match is refused, and the type falls back to the default grammar with a finding saying
so. It is refused rather than run under a time limit, because "this pattern is dangerous"
is a better message than "this repository was slow today".

If a pack fails to load, or names a profile that is not there, analysis falls back to the
built-in default and **tells you**. It never quietly runs a policy nobody wrote.

Anything that changes a finding belongs in Git, so that a report regenerates identically
for whoever runs it. VS Code settings only affect presentation, never findings.

## 8. Connecting code (optional)

Commit a symbol index at `.ejadah/rew/symbols.json` and E-REW can compare what your requirements
claim against what your code says:

```yaml
producer: "ctags 6.0.0"
state: "9f3c1a2"                  # the commit this index describes
files: ["src/thermal.c"]          # what the indexer actually scanned
symbols:
  - symbol: "sample"
    file: "src/thermal.c"
    implements: ["REQ-LLR-001"]
```

E-REW never reads your source code. The index **is** the fact, produced by whatever tool
you already use.

This unlocks drift detection:

| Finding | Meaning |
|---|---|
| `implements-disputed` | The requirement names a symbol; the index says it implements something else. |
| `implements-unacknowledged` | The index says code implements this requirement; the requirement does not list it. |
| `missing-symbol` | The index scanned that file and the symbol is not there — the code was removed. |
| `unindexed-symbol` | The claim is in a file nobody scanned — **unverified, not broken**. |

Declaring `files:` is what separates the last two. Without it, E-REW cannot tell "removed"
from "never looked", and says so rather than guessing.

## 9. Reports and dashboards

**REW: Generate Report** produces deterministic Markdown — the same commit yields the same
bytes, which is what makes it usable as evidence rather than a status update.

The dashboard shows coverage and quality as gauges and bars. Every number traces to a
metric an engine emitted; nothing is estimated.

## 9.5 Baselines — "what changed since the review?"

Run **REW: Set Baseline** and name the moment — *SRR*, *PDR*, *v1.0 review*. E-REW
records the identity of every current finding into `.ejadah/rew/baseline.json`. **Commit that
file**: a comparison only counts as evidence once everyone diffs against the same state.

From then on, **REW: Compare With Baseline** answers the review question directly:

```
--- baseline "PDR" ---
  new       error  implementation/implements-disputed  REQ-LLR-001  ...
  resolved  quality|weak-term|REQ-HLR-002|Description
--- 1 new, 1 resolved ---
```

Only finding *identities* are stored, never findings themselves — the current run stays
the only authority on what a finding contains, so a baseline can never present stale
evidence as current. Resolved entries are therefore shown as keys: the finding no longer
exists, and E-REW will not invent its old contents.

## 9.6 The command line and the CI gate

Everything §6 does inside VS Code also runs from a plain shell, with `erew`. This is not
a second implementation: the CLI assembles the **same pipeline the extension runs** —
same engines, same rule pack, same roles, same licence — so the verdict a reviewer sees
locally and the verdict your build enforces cannot disagree.

The CLI ships inside the extension package. A `.vsix` is a zip archive:

```bash
unzip e-rew-0.3.0.vsix -d e-rew
node e-rew/extension/dist/cli-entry.js --help
```

If the extension is already installed, the same file is on your machine at
`~/.vscode/extensions/ejadahailabs.e-rew-0.3.0/dist/cli-entry.js`. All it needs is
Node 20 or later — no VS Code anywhere.

```
erew [path] [options]
```

`path` is your repository, or a folder containing one (it also looks one level down).
Default: the current directory.

| Option | Meaning |
|---|---|
| `--gate <level>` | Severity that fails the run: `error` · `warning` · `info` · `off`. Default: `error`. |
| `--json` | Print the full evidence projection as JSON to stdout |
| `--report <name>` | Print a Markdown report to stdout: `validation` · `traceability` · `statistics` |
| `--quiet` | Suppress the human summary on stderr |

| Exit code | Meaning |
|---|---|
| `0` | Gate passed — nothing at or above the gate severity |
| `1` | Gate failed — findings at or above the gate severity |
| `2` | Usage error, or no REW repository found at `path` |

**stdout is evidence; stderr is chatter.** The findings, the JSON projection and the
reports go to stdout, and nothing wall-clock — no timing, no date — is ever written
there. Two runs at the same commit produce the same stdout, **byte for byte, on any
machine**, which is what lets you commit a captured report and treat the diff as review
input. The human summary — counts, timings, the pass/fail verdict — goes to stderr,
where it cannot pollute that diff.

**A candidate presents at most as a warning.** A finding inferred from prose
matching (§6) is clamped to no more severe than `warning`, so the default `error`
gate never fails a build on one — a check that fails builds on inferred prose is a
check someone switches off, and it takes every real finding with it. If you opt
into `--gate warning` or `--gate info`, candidates count at their shown severity,
like any other finding.

### The GitHub Actions recipe

Copy-paste into `.github/workflows/requirements.yml` in your requirements repository:

```yaml
name: requirements
on: [push, pull_request]

jobs:
  gate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20

      # The CLI ships inside the extension package — a .vsix is a zip.
      - name: Unpack the E-REW CLI
        run: |
          curl -fsSL -o e-rew.vsix \
            https://github.com/ejadahailabs/E-REW-support/raw/main/downloads/e-rew-0.3.0.vsix
          unzip -q e-rew.vsix -d e-rew

      - name: Requirements gate
        run: node e-rew/extension/dist/cli-entry.js . --gate error

      # Optional: keep the deterministic report as a build artefact.
      - name: Evidence report
        if: always()
        run: |
          node e-rew/extension/dist/cli-entry.js . --gate off --quiet \
            --report validation > requirements-report.md
      - uses: actions/upload-artifact@v4
        if: always()
        with:
          name: requirements-report
          path: requirements-report.md
```

The gate belongs in CI, not in a pre-commit hook: a hook is bypassed with `--no-verify`
and then nothing is checked, while the server that decides whether a change merges
cannot be bypassed. Report locally; gate on the server.

### What a green gate means — exactly

*Same commit, same evidence*: at a given commit the gate reaches the same verdict on
every machine, every time, because everything that changes a finding lives in Git
(§7) and nothing else is consulted.

And the honest boundary of that promise, the same one from §4: a green gate means
**nothing at or above the gate severity was found by the checks your declared roles
enable** — not that everything imaginable was checked. A repository that declares no
`hazard` role gets no safety analysis, and therefore no safety verdict, from this or
any other E-REW surface. The stderr summary counts the checks that stood down; the
first time you wire the gate, read that line once and declare any role you expected
to be analysed. Silence with a stated reason, never a guessed number — in CI exactly
as in the editor.

## 10. AI (optional)

AI is **off unless you add a key**, and E-REW works completely without it.

Where it is used, two rules hold absolutely:

- **AI may propose; only deterministic analysis attests.** AI output never enters
  reproducible evidence.
- **AI output lands as ordinary Git edits you accept or reject.** Nothing is applied
  behind your back.

## 11. When something is wrong

Check the [Support policy](SUPPORT.md). Bugs: 72-hour target. Complex features: about
a week.

The most useful report names the finding you got or expected, the requirement id, and
your template — because your template decides what can be analyzed at all.

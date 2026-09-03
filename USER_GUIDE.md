# Sanad User Guide

**Sanad — Engineering Intelligence Workbench.** Part of the Ejadah Engineering
Intelligence Platform, by Ejadah AI Labs.

**This guide covers Sanad 0.6.1.** Setting it up for the first time? Start with
**[GETTING-STARTED.md](GETTING-STARTED.md)** — the one-time setup walkthrough — and come
back here for the reference.

**Requirements** is the mature capability. **Software** and **Verification** ship in 0.6.1
and run in full, but are labelled **work in progress** wherever you meet them: we have not
finished testing them.

**Sanad is free to use during its beta period** — every release numbered below 1.0. You
may install, use and evaluate it without a licence key. Each beta release is valid for 45
days from its release date; when a release expires, install the current release to
continue. Sanad tells you when a licence key becomes required. Licensed use begins with
version 1.0. Nothing you have written is affected either way.

---

## 1. What Sanad is

Your requirements are **Markdown files with YAML frontmatter, in your Git repository**.
Sanad adds authoring, validation and engineering analysis on top of them.

It is **not** a Markdown editor, and **not** an AI chat window. It is an analysis tool
that happens to author requirements.

Three consequences you will notice immediately:

- **Your data outlives us.** Delete Sanad and you still have readable requirements in
  Git. There is no database, no export step, no lock-in.
- **Review, branch, merge and blame already work.** Requirements are files.
- **Every answer is reproducible.** Analysis at a commit produces the same findings,
  byte for byte, on any machine.

## 2. Install

From the [Marketplace](https://marketplace.visualstudio.com/items?itemName=EjadahAILABS.sanad):

```
code --install-extension EjadahAILABS.sanad
```

Or sideload a `.vsix` from [`downloads/`](downloads/) if your organisation cannot reach
the Marketplace:

```
code --install-extension sanad-0.6.1.vsix
```

**Then quit VS Code completely** (`Ctrl+Q`). A window reload does not swap the extension
host, so the old build stays live — the commonest reason a fresh install seems to do
nothing.

Open a folder containing requirements. Sanad activates when it finds a Sanad repository.

**Setting up for the first time?** [GETTING-STARTED.md](GETTING-STARTED.md) walks the
whole one-time setup, step by step.

## 3. Your first repository

A Sanad repository needs two things:

```
.ejadah/rew/templates/   one Markdown file per requirement type
requirements/            your requirements
```

Everything Sanad owns lives under `.ejadah/` — templates, rule packs, configuration — so
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

**Who issues the ID** — `.ejadah/rew/config.yaml`, or step *5 · Requirement IDs* of
**Sanad: Set Up Sanad in This Folder** (re-run it any time; it re-opens pre-filled):

```yaml
ids: "generated"   # Sanad allocates the next free number. The default.
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

**Sanad defines no metadata schema. Your templates do.** Only `id` and `type` are fixed.

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
you expected is missing, the first thing to check is whether its role is declared. Sanad
tells you which roles would enable a check that stood down.

This is deliberate. A confidently wrong number on a safety dashboard is the worst thing
this product could produce, so silence with a stated reason is the only honest answer.

## 5. Authoring

| Action | How |
|---|---|
| New requirement | Right-click a type in the Sanad panel → **New Requirement** |
| Jump to a requirement | Ctrl+click any ID in a body |
| See a requirement without opening it | Hover the ID |
| Fill a link field | Type `REQ` in `uplinks:` and accept a completion — only real IDs are offered |
| Delete | Right-click → Delete |

**Two safety behaviors you should know about:**

- Deleting a requirement that others trace to will **name them** and the button reads
  **Delete and break uplinks**. The broken links then appear as findings. Sanad does
  *not* silently repair other files — a requirements tool that edits files you did not
  open is one nobody can certify.
- Deleting a non-empty folder is **refused** at the syscall level. There is no recursive
  delete anywhere in Sanad, not even behind a confirmation.

## 6. Analysis

Analysis runs once as the workspace opens, and after that when you run
**Sanad: Run Analysis**. Findings appear as squiggles and in the Problems panel.

Nothing else starts an analysis: not saving a file, not a file changing on disk, not
reloading the window or switching back to it. The one exception is opt-in — turn on
`rew.quality.automatic` (off by default) and Sanad re-analyses shortly after you edit
a requirement.

Thirteen engines ship today:

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
| Conformance | Where does the code not follow the architecture you declared? |
| Consistency | Is the same thing called two things? |
| Interface | Which requirements depend on an interface, and what breaks if a field changes? |
| Impact | What does changing this reach? |

The last three optional artifacts in §8.5 — a glossary, a data dictionary and an
interface catalogue — feed the concept, parameter and interface answers.

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

Two bands may not overlap. Sanad refuses the file rather than picking a winner: a rule
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
| **safety** | `unmitigated-hazard` `rigour-inconsistency` `missing-safety-rationale` `undeclared-hazard` `single-point-failure` |
| **security** | `uncountered-threat` `missing-security-property` `missing-security-rationale` `undeclared-threat` |
| **implementation** | `not-implemented` `partially-implemented` `untraced-code` `missing-symbol` `unindexed-symbol` `implements-disputed` `implements-unacknowledged` `implements-withdrawn` `dead-requirement` |
| **architecture** | `unallocated-requirement` `duplicate-functionality` |
| **impact** | `wide-impact` `impact-incomplete` |

</details>

### What a pack cannot do

**A pack tunes checks that exist. It cannot author a new one.** There is no expression
language, deliberately: an expression in a pack is a policy interpreter you would have to
debug at the worst possible moment. If you need a check Sanad does not have — "every
requirement citing a standard clause must name a test case" — that is a new rule in the
product, and a conversation with us rather than a file you write.

Two things a template *can* do that people reach for a pack to do first: making a field
mandatory (`required:`, §3) and naming what a field means (`roles:`, §4).

### Why packs are safe to accept from anyone

Rule packs are **data, not code**. Sanad never executes anything found inside a
repository — which matters the first time you open one from a network share.

The one thing a repository *can* hand Sanad that runs is a **regular expression**: a
template's `idPattern`. Those are screened before use — a pattern that could take minutes
to match is refused, and the type falls back to the default grammar with a finding saying
so. It is refused rather than run under a time limit, because "this pattern is dangerous"
is a better message than "this repository was slow today".

If a pack fails to load, or names a profile that is not there, analysis falls back to the
built-in default and **tells you**. It never quietly runs a policy nobody wrote.

Anything that changes a finding belongs in Git, so that a report regenerates identically
for whoever runs it. VS Code settings only affect presentation, never findings.

### Rules: adopting and adding a check

Every pack your repository uses lives in **`.ejadah/rew/rules/*.yaml`** — yours, in Git,
never edited behind your back. The fastest start is to adopt the shipped writing guide:

```bash
mkdir -p .ejadah/rew/rules
cp <sanad-release>/rules/requirements-writing.yaml .ejadah/rew/rules/   # the shipped pack, beside this guide
```

then `profile: "requirements-writing"` in `.ejadah/rew/config.yaml` (step 2 above — without
it nothing changes). Guided setup offers this copy as its **Rules** step. Once copied the
file is yours: Sanad reads it, never rewrites it. Three things you will edit, all data:

```yaml
# .ejadah/rew/rules/requirements-writing.yaml — the blocks you touch
rules:                       # 1. turn a check off, or change how loud it is
  passive-voice: "off"       #    (the shipped file spells this block `severities:`; same thing)
  weak-term: "error"
sections:                    # 2. which sections a check applies to — by ROLE, not heading text
  rationale:
    readability: "off"
thresholds:                  # 3. a number a check uses
  maxSentenceWords: 72
```

**Which rule is it?** Every finding carries its rule id — the `code` beside the message in
the Problems panel and in reports — and that id is the key you edit above.

**Adding a check — simple to complex, and what is data vs code.** *Simple*: a severity,
`off`, or a threshold per rule. *Medium*: per-section bindings (`sections:`) and
criticality bands (`bands:`, above). *Complex*: patterns — sentence shapes and slots, once
the pattern engine ships — and composed packs via `extends`. All of that is data in the
pack: write it, commit, done. A new *rule id* is an engine, so it is code in the product,
not a line in a file — ask us (see *What a pack cannot do*, above).

Prefer a form? **Sanad: Edit Validation Rules by Section** edits the same file — per-section
checks and thresholds, each showing where its value came from — and writes `profile:` for
you if the repository had none. Form or hand edit, it is the same YAML: a review diff shows
exactly what changed.

## 8. Connecting code (optional)

Commit a symbol index at `.ejadah/rew/symbols.json` and Sanad can compare what your requirements
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

The index **is** the fact. You can commit one produced by whatever tool you already use, or
let Sanad build it: **Sanad: Refresh Code Index (C/C++)** sweeps the C and C++ sources in
your workspace and writes `symbols.json` itself. Either way an analysis run reads only the
committed index — it never shells out and never parses source while producing findings.

This unlocks drift detection:

| Finding | Meaning |
|---|---|
| `implements-disputed` | The requirement names a symbol; the index says it implements something else. |
| `implements-unacknowledged` | The index says code implements this requirement; the requirement does not list it. |
| `missing-symbol` | The index scanned that file and the symbol is not there — the code was removed. |
| `unindexed-symbol` | The claim is in a file nobody scanned — **unverified, not broken**. |

Declaring `files:` is what separates the last two. Without it, Sanad cannot tell "removed"
from "never looked", and says so rather than guessing.

**Leaving sources out of the sweep — `codeIgnore:`.** The refresh indexes every C and C++
file in your workspace, because a file being there is the only evidence Sanad needs that it
is yours. If some of those files should not be in the index — generated code, a vendored
tree, an intermediate copy your build leaves behind — name them in `.ejadah/rew/config.yaml`:

```yaml
codeIgnore:
  - "build/**"
  - "**/*.generated.c"
  - "third_party/**"
```

Same glob grammar as `ignore:` (the list that says which Markdown files are not
requirements), and deliberately a separate list: one list doing both jobs would mean every
edit for one purpose silently changed the other. Leave `codeIgnore:` out and the sweep is
unchanged. The run log says how many files it skipped and why.

A duplicated source is worth excluding for a second reason. `path/file.trace` binds the
entities it names inside `path/file` first, then anywhere in the index — so if the same
function is indexed from a dozen copies, a claim that the co-located file does not itself
define comes back ambiguous, and Sanad refuses to guess which copy you meant.

## 8.5 Intelligence layers — concepts, parameters and interfaces (optional)

Three more optional artifacts, each a **Git-tracked file you declare**, let Sanad answer
questions about your vocabulary and your interfaces. Like everything else here, each layer
is silent until you declare it: no glossary means no concept findings — not "100 %
coverage". You add them one at a time, and each works with the others absent.

Every layer draws the same honest line: a **declared** link is a fact and renders as
established; a link **inferred from prose or spelling** is a *candidate for human
judgement* and is always marked. Nothing inferred is ever presented as certain, and
nothing inferred is ever silently written back into your files.

### A glossary — "where does this concept appear?"

Declare `.ejadah/rew/glossary.md`: one `## Heading` per canonical term, a definition, the
requirement that defines it, and any alternative spellings.

```markdown
## Overtemperature
Aliases: over-temperature, OT
Defined by: REQ-HLR-001

A declared cell temperature above the protection threshold.
```

- **What it produces.** Each term becomes a first-class entity. Sanad can then answer
  *where does "Overtemperature" appear, and which requirement defines it?*
- **What you must declare.** A heading (the term), a definition (a term with none is just
  a heading, and is an error), and — for the authoritative link — a `Defined by:` id.
- **What findings appear.** A malformed glossary is reported with a line number, and the
  layer stands down rather than guessing. The **defines** link (you named the definer) is
  a fact; a **prose mention** of the term is a marked candidate, never counted as a
  certain use.

### A data dictionary — "which spellings are drifting?"

Declare `.ejadah/rew/data-dictionary.md`: typed parameters — name, type, units, range and
any declared aliases. This answers the four-conventions problem — `bearingDistance` in the
code, `bearing_distance` in the interface document, `BearingDistance` in the test harness,
*Bearing Distance* in the requirements.

- **What it produces.** Each parameter becomes an entity carrying its type/units/range.
  Sanad answers *where is this parameter defined, used and tested?*
- **What you must declare.** The parameter name and its definition; aliases are optional.
- **What findings appear.** A **declared alias** matched in prose is an exact use — a fact.
  A **code-convention spelling** that folds onto the parameter (only separator style and
  case differ) is surfaced as a **proposal** — *declare this as an alias if they are the
  same* — never silently bound. Sanad does not do fuzzy matching: it only ever erases a
  naming convention, so a proposal is always defensible, and a parameter used nowhere gets
  an honest empty view, never a fabricated count.

#### Your own dictionaries, where you already keep them

The fixed path above is the *default*, not a requirement. A real programme rarely keeps one
dictionary in Sanad's shape: it has several — one per subsystem, one inherited from a
supplier, one that is really a spreadsheet somebody exported. Declare them under `producers:`
in `.ejadah/rew/config.yaml` and Sanad reads each where it already lives, in the format you
already keep it, renaming nothing — the same "your fields, your meaning" rule as a template
(§4), one layer out:

```yaml
# .ejadah/rew/config.yaml
producers:
  dataDictionary:
    - path:   docs/navdb/parameters.yaml      # your shape, unchanged
      format: yaml
      fields: { name: parameter, type: data_type, units: uom, range: valid_range }
    - path:   docs/fms/data-dictionary.csv    # a spreadsheet export
      format: csv
      fields: { name: Name, type: Type, units: Units }
    - path:   supplier/acme-icd-params.md     # a supplier's Markdown, as delivered
```

- **Formats.** `markdown`, `yaml`, `json` and `csv`. Omit `format:` and Sanad infers it from
  the file extension.
- **`fields:` maps a role to your column.** `name` and `type` are required; `units`, `range`
  and `aliases` are optional. A CSV whose parameter column is `parameter` and unit column is
  `uom` is read without renaming a cell. Sanad never *guesses* the mapping — an unmapped
  source uses the role names verbatim, because guessing which column is `units` is the same
  error as guessing a requirement's parent link.
- **Each source fails closed on its own.** A malformed supplier file contributes no
  parameters and is reported by name; the good sources beside it are unaffected.
- **When two sources disagree, Sanad flags — it never picks a winner.** Two dictionaries
  defining *Execution Rate* with different units raise a `parameter-conflict` finding naming
  both files and both values. The parameter is withheld and anything depending on it stands
  down and says why. Reconciling the sources is the data owner's job, in their file — Sanad
  reports the defect, it does not resolve, rank or rewrite it.
- **You can always tell a missing file from an empty one.** The run reports each source it
  read — *"data dictionary — docs/navdb/parameters.yaml: 412 parameter(s)"*, or *"not found
  at supplier/acme-icd-params.md"* — so a count you did not expect always traces to a file.

Leave `producers:` out and nothing moves: Sanad reads the single default path above, exactly
as before.

### An interface catalogue — "what breaks if this field changes?"

Declare `.ejadah/rew/interfaces.json`: your interfaces, their producer and consumer, and
each message's fields with units and ranges. A requirement links to the interfaces it
depends on through its **interface role** (§4).

- **What it produces.** Each interface is an entity and each field a citizen you can trace.
  Sanad answers, for any interface or field, *which requirements depend on it, which tests
  verify them, and what breaks if it changes?*
- **What you must declare.** The catalogue, and — on each requirement that depends on an
  interface — the ICD ids in its interface-role field.
- **What findings appear.** A requirement naming an interface **no catalogue declares** is
  a broken link, reported as an error, never dropped. Interfaces declared but used by no
  requirement, or carrying no fields, are reported as information. With no catalogue the
  Interface engine (§6) stands down — its counts are **absent, not zero**.

## 9. Reports and dashboards

Reports are produced by the **command line** — `erew . --report validation` (§9.6) — and
are deterministic Markdown: the same commit yields the same bytes, which is what makes a
report usable as evidence rather than as a status update. `--report` takes `validation`,
`traceability`, `traceability-audit`, `statistics`, `requirements` or `eiwr`.

Inside the editor, the **Sanad Dashboards** view is the equivalent surface.

The dashboard shows coverage and quality as gauges and bars. Every number traces to a
metric an engine emitted; nothing is estimated.

## 9.5 Baselines — "what changed since the review?"

The **Baselines** view in the Sanad sidebar lists every baseline this repository holds —
name and time it was taken, newest first. Use **＋ Create Baseline** (the view's title
button, or the command) and name the moment — *SRR*, *PDR*, *v1.0 review*. Sanad records
the identity of every current finding into `.ejadah/rew/baselines.json`, and the new
baseline appears in the list at once. A name already in use is refused. **Commit that
file**: a comparison only counts as evidence once everyone diffs against the same state.

Select a baseline — or run **Sanad: Compare With Baseline** — and Sanad opens the *review
delta*: the new findings, grouped under the requirement they land on, and the resolved
ones. Requirement ids in it are clickable, straight to the file.

```
# Review delta since "PDR"

**1 new**, **1 resolved**.

## New findings

### REQ-LLR-001
- **error** `implementation/implements-disputed` — …

## Resolved
- `quality|weak-term|REQ-HLR-002|Description`
```

Only finding *identities* are stored, never findings themselves — the current run stays
the only authority on what a finding contains, so a baseline can never present stale
evidence as current. Resolved entries are therefore shown as keys: the finding no longer
exists, and Sanad will not invent its old contents.

## 9.6 The command line and the CI gate

Everything §6 does inside VS Code also runs from a plain shell, with `erew`. This is not
a second implementation: the CLI assembles the **same pipeline the extension runs** —
same engines, same rule pack, same roles, same licence — so the verdict a reviewer sees
locally and the verdict your build enforces cannot disagree.

The CLI ships inside the extension package. A `.vsix` is a zip archive:

```bash
unzip sanad-0.6.1.vsix -d sanad
node sanad/extension/dist/cli-entry.js --help
```

If the extension is already installed, the same file is on your machine at
`~/.vscode/extensions/ejadahailabs.sanad-0.6.1/dist/cli-entry.js`. All it needs is
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
| `--report <name>` | Print a Markdown report to stdout: `validation` · `traceability` · `traceability-audit` · `statistics` · `requirements` · `eiwr` |
| `--quiet` | Suppress the human summary on stderr |

| Exit code | Meaning |
|---|---|
| `0` | Gate passed — nothing at or above the gate severity |
| `1` | Gate failed — findings at or above the gate severity |
| `2` | Usage error, or no Sanad repository found at `path` |

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
      - name: Unpack the Sanad CLI
        run: |
          curl -fsSL -o sanad.vsix \
            https://github.com/ejadahailabs/E-REW-support/raw/main/downloads/sanad-0.6.1.vsix
          unzip -q sanad.vsix -d sanad

      - name: Requirements gate
        run: node sanad/extension/dist/cli-entry.js . --gate error

      # Optional: keep the deterministic report as a build artifact.
      - name: Evidence report
        if: always()
        run: |
          node sanad/extension/dist/cli-entry.js . --gate off --quiet \
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
any other Sanad surface. The stderr summary counts the checks that stood down; the
first time you wire the gate, read that line once and declare any role you expected
to be analysed. Silence with a stated reason, never a guessed number — in CI exactly
as in the editor.

## 10. AI (optional)

AI is **off unless you add a key**, and Sanad works completely without it.

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

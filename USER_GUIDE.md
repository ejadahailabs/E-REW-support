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
code --install-extension e-rew-0.2.2.vsix
```

Open a folder containing requirements. E-REW activates when it finds a REW repository.

## 3. Your first repository

A REW repository needs two things:

```
templates/          one Markdown file per requirement type
requirements/       your requirements
```

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

**The filename is the ID.** `REQ-LLR-042.md` *is* `REQ-LLR-042`. This is why two people
creating the same ID on different branches get a Git merge conflict instead of a silent
duplicate — Git is the duplicate check.

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

Twelve engines ship today:

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
| Interface | Which requirements depend on an interface, and what breaks if a field changes? |
| Impact | What does changing this reach? |

**Findings marked as candidates.** Anything derived from prose matching rather than a
declared field renders as *a candidate for human judgement*, never as established fact.
You can trust the unmarked ones absolutely; the marked ones are worth a look.

## 7. Rule packs — tuning without waiting for us

Drop a YAML file in `rules/` to change severities, silence a check, or bind strictness to
criticality:

```yaml
rules:
  weak-term:
    severity: error        # a warning by default
  not-implemented:
    severity: off
```

Rule packs are **data, not code**. E-REW never executes anything found inside a
repository — which matters the first time you open one from a network share.

Anything that changes a finding belongs in Git, so that a report regenerates identically
for whoever runs it. VS Code settings only affect presentation, never findings.

## 8. Connecting code (optional)

Commit a symbol index at `.rew/symbols.json` and E-REW can compare what your requirements
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

## 8.5 Intelligence layers — concepts, parameters and interfaces (optional)

Three more optional artefacts, each a **Git-tracked file you declare**, let E-REW answer
questions about your vocabulary and your interfaces. Like everything else here, each layer
is silent until you declare it: no glossary means no concept findings — not "100 %
coverage". You add them one at a time, and each works with the others absent.

Every layer draws the same honest line: a **declared** link is a fact and renders as
established; a link **inferred from prose or spelling** is a *candidate for human
judgement* and is always marked. Nothing inferred is ever presented as certain, and
nothing inferred is ever silently written back into your files.

### A glossary — "where does this concept appear?"

Declare `.rew/glossary.md`: one `## Heading` per canonical term, a definition, the
requirement that defines it, and any alternative spellings.

```markdown
## Overtemperature
Aliases: over-temperature, OT
Defined by: REQ-HLR-001

A declared cell temperature above the protection threshold.
```

- **What it produces.** Each term becomes a first-class entity. E-REW can then answer
  *where does "Overtemperature" appear, and which requirement defines it?*
- **What you must declare.** A heading (the term), a definition (a term with none is just
  a heading, and is an error), and — for the authoritative link — a `Defined by:` id.
- **What findings appear.** A malformed glossary is reported with a line number, and the
  layer stands down rather than guessing. The **defines** link (you named the definer) is
  a fact; a **prose mention** of the term is a marked candidate, never counted as a
  certain use.

### A data dictionary — "which spellings are drifting?"

Declare `.rew/data-dictionary.md`: typed parameters — name, type, units, range and any
declared aliases. This answers the four-conventions problem — `bearingDistance` in the
code, `bearing_distance` in the interface document, `BearingDistance` in the test harness,
*Bearing Distance* in the requirements.

- **What it produces.** Each parameter becomes an entity carrying its type/units/range.
  E-REW answers *where is this parameter defined, used and tested?*
- **What you must declare.** The parameter name and its definition; aliases are optional.
- **What findings appear.** A **declared alias** matched in prose is an exact use — a fact.
  A **code-convention spelling** that folds onto the parameter (only separator style and
  case differ) is surfaced as a **proposal** — *declare this as an alias if they are the
  same* — never silently bound. E-REW does not do fuzzy matching: it only ever erases a
  naming convention, so a proposal is always defensible, and a parameter used nowhere gets
  an honest empty view, never a fabricated count.

### An interface catalogue — "what breaks if this field changes?"

Declare `.rew/interfaces.json`: your interfaces, their producer and consumer, and each
message's fields with units and ranges. A requirement links to the interfaces it depends
on through its **interface role** (§4).

- **What it produces.** Each interface is an entity and each field a citizen you can trace.
  E-REW answers, for any interface or field, *which requirements depend on it, which tests
  verify them, and what breaks if it changes?*
- **What you must declare.** The catalogue, and — on each requirement that depends on an
  interface — the ICD ids in its interface-role field.
- **What findings appear.** A requirement naming an interface **no catalogue declares** is
  a broken link, reported as an error, never dropped. Interfaces declared but used by no
  requirement, or carrying no fields, are reported as information. With no catalogue the
  Interface engine (§6) stands down — its counts are **absent, not zero**.

## 9. Reports and dashboards

**REW: Generate Report** produces deterministic Markdown — the same commit yields the same
bytes, which is what makes it usable as evidence rather than a status update.

The dashboard shows coverage and quality as gauges and bars. Every number traces to a
metric an engine emitted; nothing is estimated.

## 9.5 Baselines — "what changed since the review?"

Run **REW: Set Baseline** and name the moment — *SRR*, *PDR*, *v1.0 review*. E-REW
records the identity of every current finding into `.rew/baseline.json`. **Commit that
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

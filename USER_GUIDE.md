# Sanad User Guide

**Sanad — Requirements Engineering Workbench.** Part of the Ejadah Engineering
Intelligence Platform, by Ejadah AI Labs.

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

Marketplace, or sideload a `.vsix` from [`downloads/`](downloads/):

```
code --install-extension e-rew-0.4.0.vsix
```

Open a folder containing requirements. Sanad activates when it finds a REW repository.

> **Update if you are on 0.3.0 or earlier.** Those builds stop working 30 days
> after you first opened them — they do not degrade, they stop. **0.4.0 removes
> that timer**; an install now keeps working. Nothing you created is affected
> either way: your requirements are Markdown files in your own Git repository,
> and no version has ever touched them on expiry.

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
| New requirement | Right-click a type in the REW panel → **New Requirement** |
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

### Finding things

| Command | What it does |
|---|---|
| **Go to Requirement…** | Jump straight to one by id or name |
| **Search Requirements** | Search across everything a requirement contains |
| **Filter Requirements** | Narrow the explorer — by type, status, or text |
| **Clear Search** · **Clear Filters** | Restore the full tree |

A filtered tree still shows honest counts. It narrows what you *see*, never what
was analysed.

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

## 6.5 The Trace Browser — following the thread

Findings tell you what is wrong. The Trace Browser tells you **how things
connect**, which is the question you actually have most days.

Right-click a requirement → **Open in Trace Browser**, and you get its
neighbourhood in one place:

| | |
|---|---|
| **Parents** | what this requirement refines |
| **Children** | what refines it |
| **Verified by** | the verification cases that claim it |
| **Implemented by** | the code symbols that claim it — if you have committed a symbol index (§8) |

Follow any hop and the browser moves with you, so *"what does this change
affect?"* becomes something you walk rather than something you reconstruct from
a spreadsheet.

**Two things it will not do.** It never invents a connection: a hop exists
because some artefact declared it, and you can open the file that did. And where
a connection was *inferred* from prose rather than declared — a requirement that
mentions a term without formally linking it — the hop is **marked as a
candidate**, never shown as fact. Sanad would rather tell you it is guessing
than let you build an audit on a guess.

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

Rule packs are **data, not code**. Sanad never executes anything found inside a
repository — which matters the first time you open one from a network share.

Anything that changes a finding belongs in Git, so that a report regenerates identically
for whoever runs it. VS Code settings only affect presentation, never findings.

## 8. Connecting code (optional)

Commit a symbol index at `.rew/symbols.json` and Sanad can compare what your requirements
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

Sanad never reads your source code. The index **is** the fact, produced by whatever tool
you already use.

This unlocks drift detection:

| Finding | Meaning |
|---|---|
| `implements-disputed` | The requirement names a symbol; the index says it implements something else. |
| `implements-unacknowledged` | The index says code implements this requirement; the requirement does not list it. |
| `missing-symbol` | The index scanned that file and the symbol is not there — the code was removed. |
| `unindexed-symbol` | The claim is in a file nobody scanned — **unverified, not broken**. |

Declaring `files:` is what separates the last two. Without it, Sanad cannot tell "removed"
from "never looked", and says so rather than guessing.

## 9. Reports and dashboards

**REW: Generate Report** produces deterministic Markdown — the same commit yields the same
bytes, which is what makes it usable as evidence rather than a status update.

The dashboard shows coverage and quality as gauges and bars. Every number traces to a
metric an engine emitted; nothing is estimated.

## 9.5 Baselines — "what changed since the review?"

Run **REW: Set Baseline** and name the moment — *SRR*, *PDR*, *v1.0 review*. Sanad
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
exists, and Sanad will not invent its old contents.

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

# Getting started with Sanad — the one-time setup

**Sanad — Engineering Intelligence Workbench**, by Ejadah AI Labs. This page covers
**Sanad 0.6.1**.

This is the walkthrough you do **once**, per repository. It takes about twenty minutes on
a repository you already know. At the end of it Sanad knows where your requirements live,
what your fields mean, and which analysis to run — and you never do it again unless
something moves.

Everything Sanad learns here is written to **`.ejadah/rew/`** in your own repository, as
plain YAML and Markdown. It is reviewable, diffable and yours. You can re-open the setup
form at any time and it comes back pre-filled.

**Technical support:** support@ejadahailabs.com

> **A note on what is finished.** **Requirements** is the mature capability — that is the
> part to judge Sanad on. **Software** and **Verification** ship in 0.6.1 and run in full,
> but we have not finished testing them, so they are labelled **work in progress**
> wherever you meet them: in the setup form, in the capability switcher and on the lane
> itself. Two steps of the setup form (**8 · Standards mapping** and **9 · Views &
> reports**) are placeholders that record nothing yet, and say so on screen.

---

## Before you start

| You need | Why |
|---|---|
| VS Code 1.90 or newer | The extension's minimum engine |
| A Git repository holding requirements as Markdown files | Sanad has no database — your files are the data |
| Nothing else | No account, no sign-up, no server, no network |

Your requirement files can have YAML frontmatter or be **plain Markdown with no `---`
block at all**. Both are supported, and you can mix them — it is declared per requirement
type. Front matter is optional in 0.6.1; you do not have to convert anything before you
begin.

---

## Step 1 — Install, then quit VS Code completely

**From the Marketplace** (the normal route):

```
code --install-extension EjadahAILABS.sanad
```

…or search for **Sanad** in the Extensions view.

**From a `.vsix`** — if your organisation cannot reach the Marketplace, download the
newest file from [`downloads/`](downloads/) and either run

```
code --install-extension sanad-0.6.1.vsix
```

or use the Extensions view → `...` menu → *Install from VSIX...*.

**Then quit VS Code completely** — `Ctrl+Q`, or *Quit* from the menu. A window reload is
**not enough**: it does not swap the extension host, so the old build stays live. This is
the single most common reason a fresh install appears to do nothing.

> *Screenshot placeholder — the Extensions view showing Sanad installed.*

---

## Step 2 — Open your requirements repository

Open the folder that holds your requirements — the repository root, not a subfolder.

Sanad activates when it finds `.ejadah/rew/` in the folder. A folder that has never been
set up **will not light up on its own**, and that is expected, not a broken install. The
Requirements view will show you this instead:

> This folder is not a Sanad repository yet.
>
> Setup asks where requirements live, then either creates a starter template, copies
> templates you already have, or derives one from requirements you have already written.
>
> **[Set Up Sanad in This Folder]**

> *Screenshot placeholder — the empty Requirements view with the "Set Up Sanad in This
> Folder" button.*

---

## Step 3 — Run setup

Click that button, or open the Command Palette (`Ctrl+Shift+P`) and run:

**Sanad: Set Up Sanad in This Folder**

A form opens titled *Set up Sanad in this folder*, with ten steps down the left-hand side.
**Nothing is written to disk until you press "Setup complete"** at the end, and re-running
the command later re-opens the form pre-filled with what you already answered.

Work down the steps. Only steps 1 and 2 really need your attention the first time; the
rest are optional and each one that you leave blank simply turns off the analysis that
would have needed it — with that stated as the reason, never reported as a clean result.

> *Screenshot placeholder — the setup form, step navigation visible down the left.*

### 1 · Product

| Field | What to put |
|---|---|
| **Product name** | Defaults to the folder's name |
| **Domain** | Pick from the list |
| **Capabilities** | Tick the ones you want. Requirements, Software and Verification can be ticked today; Software and Verification carry a **WIP** tag. Review and Systems are greyed out — no engines behind them yet |
| **Topology** | How the programme is laid out. If you pick a split programme, Sanad tells you plainly that multi-repository integration is not available yet and it works on this one repository for now |
| **Artifact types this programme holds** | The important one. Tick every kind of artifact you actually have |

For each artifact kind you tick, say where it **lives** (a repository-relative folder),
which **template** validates it, and — optionally — how a file of that kind is
**recognised**: a filename pattern like `HAZ-*.md`, a frontmatter key like `hazardId`, or
a `###` section heading. All three are optional. Sanad never guesses any of them.

> **The rule that runs through the whole product:** anything you leave unticked is
> **absent**, and the analysis that needed it stays off with that as its stated reason. It
> is never reported as zero, and never as clean.

### 2 · Artifact types

One row per requirement kind you ticked above. This step asks the things step 1 could not:

- **ID prefix** — starts every id of that kind. Because the filename *is* the id, it is
  also the filename prefix. `NAV-HLR` gives you `NAV-HLR-001.md`.

  > **Give this some thought.** The ID prefix does more work later than it looks like it
  > does here. Sanad uses your declared prefixes to find requirement ids **written inside
  > your test files** — in a header comment, in a docstring, in a test's file name. A file
  > that mentions `NAV-LLR-123` anywhere Sanad reads is naming a requirement, whatever
  > label sits in front of it. Declare no prefix and that whole lane stays off, because
  > scanning prose for anything id-shaped would be a guess.

- **Shape** — YAML frontmatter (a `---` block), or plain Markdown (the id is the
  filename, the type is the folder).
- **Template** — a starter Sanad writes from your answers, one **derived** from a sample
  file of yours (Sanad reads its fields and asks which carry the ID and the title), one
  you already have, or none yet.
- **Uplinks** — which field carries the parent link. The conventional `uplinks` is
  understood without asking; pick `satisfies` or `derivesFrom` if that is what your
  requirements call it. Sanad reads a field's **role**, never its name.
- **ID source** — either *Sanad allocates the next free number*, or *I supply it* (a
  customer baseline, a DOORS export, a programme-wide scheme). Either way the filename
  stays the id.

> *Screenshot placeholder — step 2, one row per requirement type with the ID prefix
> column.*

### 3 · Templates

Nothing to do here. It restates that the shape and field mapping you chose in step 2 *are*
the template. Templates carry content; the schema lives in the artifact types.

### 4 · Relationships

Derived from what you ticked in step 1 — **edit it only if your programme differs**.
This is where you state that system requirements decompose to HLRs, HLRs to LLRs, that
tests verify requirements, that a data dictionary defines their terms. This is the
repository's layout model and it is what the graph is organised by. Sanad never guesses
it: only what you state here is built.

### 5 · Data dictionaries

See [Step 4](#step-4--declare-your-data-dictionaries) below — it is worth its own section.

### 6 · Results data

Where your **verification results** live — one row per verification level, each with its
own folder, and optionally a filename pattern. Sanad probes the format (JUnit XML, TAP,
CSV, JSON, text) and can derive a results template per level. Optional.

### 7 · Requirements-writing rules

Name the **rule pack** Sanad checks your requirements against — a YAML file under
`.ejadah/rew/rules/`, named without the folder and without `.yaml`. Or tick **Adopt the
shipped guide** and Sanad copies its own requirements-writing pack in for you; the copy is
yours to edit from then on and is never overwritten.

Leave it blank and the requirements-writing checks stay off, with that as the stated
reason. Sanad never picks a pack for you.

### 8 · Standards mapping — *work in progress*

A placeholder. It will map each declared standard to the objectives and evidence Sanad
checks. Until then every standard is reported as **declared, not checked**.

### 9 · Views & reports — *work in progress*

A placeholder. It will enable and configure reports and views per capability. Until then
the built-in reports run as they do today.

### 10 · AI provider

Optional, and Sanad is fully functional without it. Name the provider, model and base URL
your AI-assisted features should call. **Never an API key here** — a credential found in a
config file is refused, not quietly accepted. The one credential Sanad holds lives in VS
Code's secret storage, set with **Sanad: Set Credential** or **Sanad: Sign In to LLM**.

If you would rather be walked through it — including which authentication mode your
gateway wants, and a connection test that says *which* of reachability, credential, wire
format or the model itself is the thing that failed — see *Connecting your AI* in the
[User Guide](USER_GUIDE.md#10-ai-optional). The guided **Sanad: Connect AI Provider**
command it describes lands in the next test build.

### Press "Setup complete"

Now Sanad writes `.ejadah/rew/config.yaml`, your templates, `sanad-product.yaml` and — if
you asked for it — the rule pack. Your own requirement files are never rewritten by setup,
and an existing file is never overwritten.

Commit `.ejadah/` and the setup is shared with your whole team through review, like any
other file.

---

## Step 4 — Declare your data dictionaries

A **data dictionary** is the file that defines the parameters your requirements refer to —
name, type, units, range, aliases. Declare one and Sanad can answer *where is this
parameter defined, used and tested?*, and catch a requirement naming a parameter the
dictionary does not carry.

There are two ways in, and both are fine.

### Just give it the path

In setup step 5, or by hand in `.ejadah/rew/config.yaml`:

```yaml
producers:
  dataDictionary:
    - path: docs/params/sensors.csv
    - path: docs/params/actuators.yaml
```

The format is read from the file extension — `.csv`, `.md`, `.yaml`, `.json`. Most
dictionaries are generated from the requirements, the software or the test harness, so
their columns are already named after their meaning (`name`, `type`, `units`, `range`,
`aliases`) and the path is all Sanad needs.

### Let the offer flow propose the column mapping

A hand-kept dictionary usually spells its columns differently — `Parameter` for the name,
`uom` for the units. Sanad reads such a source, recognises a mapping it *could* apply, and
**applies none of it**. Instead the run log says so:

```
  data dictionary — docs/params/sensors.csv: 412 parameter(s)
    offered mapping — Parameter → name, Data Type → type, uom → units;
    Owner matched no role. Nothing is loaded from these columns until you accept it;
    "Sanad: Accept Offered Dictionary Mapping" writes it into config.yaml as a declaration.
```

Run **Sanad: Accept Offered Dictionary Mapping** from the Command Palette. It shows you a
modal naming every column it will bind, and only writes `config.yaml` when you press
**Accept mapping**. After that the mapping is *your declaration*, the offer line stops
appearing, and a review diff shows exactly what changed.

You can equally type the block yourself — it is the same bytes either way:

```yaml
producers:
  dataDictionary:
    - path:   docs/params/sensors.csv
      format: csv
      fields: { name: Parameter, type: Data Type, units: uom }
```

Two dictionaries that disagree about one parameter raise a `parameter-conflict` finding
naming both files and both values. Sanad withholds the parameter and says so; it never
picks a winner.

> *Screenshot placeholder — the "Accept mapping" modal listing the columns it will bind.*

---

## Step 5 — Point Sanad at your tests

This is the whole of the test-discovery declaration, and it is two lines in a template.

A template under `.ejadah/rew/templates/` whose **`type:`** is one of the verification
kinds, and whose **`rew.folder:`** names a folder, tells Sanad where to look:

```yaml
---
id: ""
type: "verification-ll"
rew:
  label: "Low Level Verification Case"
  idPrefix: "NAV-VER-LL"
  folder: "tests/ll"
---
```

The verification types are `verification`, `verification-hl`, `verification-component`
and `verification-ll`. Any file under a declared verification folder whose extension names
a format Sanad reads — `.csv`, `.yaml`, `.json`, `.xml`, `.xlsx`, `.py` — **is** a source.

Nothing is inferred from a folder's *name*, and nothing is sniffed out of a file's
contents. A repository that declares no verification type is told so by name, rather than
left with an empty lane:

> No template declares one of the verification types (verification, verification-hl,
> verification-component, verification-ll), so there is no folder to read. Declare one in
> `.ejadah/rew/templates` — its `type:` names the kind and its `rew.folder:` is where Sanad
> looks — or list the files under `producers.verification` in the repository config.

You can still write a `producers.verification` block, and it is still read — but it now
**refines** what is read (a format, a column mapping, a key) rather than deciding whether
anything is read at all. A folder that turns out to hold no cases reports as *found and
empty*, and the run log names where it looked.

---

## Step 6 — Optional: connect your code

Skip this if you are only doing requirements. Both parts below belong to the **Software**
capability, which is a work-in-progress preview.

### Index your C and C++ sources

Run **Sanad: Refresh Code Index (C/C++)**. It sweeps every `.c`, `.h`, `.hh`, `.hpp`,
`.hxx`, `.cc`, `.cpp` and `.cxx` file in the workspace and writes `.ejadah/rew/symbols.json`.
**Commit that file** — the committed index *is* the fact, and an analysis run reads only
the committed index. It never shells out and never parses source while producing findings.

**You are asked before anything runs.** If your repository declares an analysis context,
Sanad names the exact command it will run — `clang -fsyntax-only -Xclang -ast-dump=json`,
with the version and the file count — and waits for you to press **Run clang**. Nothing is
compiled or linked. If there is no analysis context it uses its own built-in indexer
instead, which needs no consent because it runs nothing.

Keep generated and vendored code out of the index with `codeIgnore:` in
`.ejadah/rew/config.yaml`:

```yaml
codeIgnore:
  - "build/**"
  - "**/*.generated.c"
  - "third_party/**"
```

Same glob grammar as `ignore:`, and deliberately a **separate** list — one list doing both
jobs would mean every edit for one purpose silently changed the other. The run log says how
many files it skipped and why.

Once an index exists, a **Code Index** entry appears under the Software capability listing
every indexed file with its symbol count. A file the index found nothing in reads *0
symbols* rather than disappearing. A repository that declares where its code lives but has
no index yet reads *No code index — run "Sanad: Refresh Code Index (C/C++)"*.

### `.trace` files — put them beside the file they trace

If your toolchain already emits traceability as `*.trace` files, Sanad reads every one of
them with no declaration and no column mapping at all — the shape *is* the contract:

```yaml
- entity_id: "int cabin_pressure_sample(const sensor_t *)"
  upstreams: ["NAV-LLR-014", "NAV-LLR-015"]
```

**Where you put the file is the declaration.** `src/pressure.c.trace` binds the entities it
names inside `src/pressure.c` **first**, and only falls back to a workspace-wide match for
an entity that file does not define. This matters in any repository that keeps intermediate
copies of its sources: without co-location one function appears in the index a dozen times,
every claim comes back ambiguous, and Sanad correctly refuses to guess — leaving you with
an empty implementation lane from perfectly good trace files.

The sweep is **fail-open per entry**: one entry that does not resolve never withholds the
file, and every skip is named in the run log. The one exception is an upstream naming a
requirement id your workspace does not declare — that is a **warning** on the `.trace` file
making the claim, because the claim may be perfectly right and it is the requirement that
is missing.

---

## Step 7 — Run the analysis, and read the run log

Run **Sanad: Run Analysis** from the Command Palette.

Exactly one analysis runs by itself, as the workspace opens — opening it is asking. After
that, analysis starts **only when you run this command**. Saving a file, a file changing on
disk, reloading the window and switching back to it never start one.

Findings appear as squiggles on the exact words they name, and in the Problems panel.
Findings derived from prose render as **candidates for human judgement**, never as fact.

### The run log is where you find out what did *not* run

Open the **Sanad** output channel — View → Output, then pick *Sanad* from the dropdown.
This is the most useful screen in the product on your first day. It is not a progress bar;
it is the analysis accounting for itself.

```
[14:22:07] analysing 412 requirement(s) — started by command
  rule pack: requirements-writing (Requirements writing — deterministic prose rules)
  318 finding(s) in 214 ms
  validation: 12 ms
  traceability: 31 ms — 9 ms of rules plus 22 ms building the engineering graph,
    which every engine after it then reads for free
  safety: NOT RUN — no template declares the role hazard. No safety findings are
    reported; that is not a clean result.
    To arm it, map a field of your own to "hazard" in the `rew: roles:` block of a
    template under .ejadah/rew/templates/ — …
  data dictionary — docs/params/sensors.csv: 412 parameter(s)
  data dictionary — not found at docs/params/actuators.yaml
  symbol index — sanad-cindex (…): 8214 function · 512 tracing to 340 requirement(s)
  traceability — docs/trace/legacy-matrix.csv: 34 asserted link(s), 2 row(s) unresolved
  verification cases — tests/ll: 118 case(s)
  test results — not found at test/results/hl
```

Four kinds of line are worth learning:

| Line | What it is telling you |
|---|---|
| `<engine>: <n> ms` | It ran. The graph-build line explains why one engine's number looks large — it built the graph everyone else then reads for free |
| `<engine>: NOT RUN — no template declares the role …` | **The most important line in the log.** An engine stood down because a role it needs is not declared anywhere. No safety findings does **not** mean a safe repository. The next line always tells you what would arm it |
| `data dictionary — …: N parameter(s)` / `— not found at …` | One line **per source**, so a count you did not expect always traces to a file, and you can always tell a missing file from an empty one |
| `verification cases — …` · `test results — …` · `coverage — …` · `traceability — …` | The same per-source account for every other producer |

If a check you expected is missing, the first thing to look at is whether its **role** is
declared. That is deliberate: a repository that declares no `hazard` role gets no safety
findings, because a confidently wrong number on a safety dashboard is the worst thing this
product could produce. Silence with a stated reason, never a guessed number.

> *Screenshot placeholder — the Sanad output channel after a run, with a NOT RUN line
> visible.*

---

## Step 8 — Narrow the view to what you are working on

Run **Sanad: Show Capability...** and pick one:

| Capability | What the tree narrows to | State |
|---|---|---|
| **All** | Every artifact kind — the default | — |
| **Requirements** | System requirements, HLRs, LLRs, component and interface requirements, data dictionaries | Mature |
| **Software** | Architecture, design and implementation artifacts, and the Code Index; opens the **Software Trace** view | **Work in progress** |
| **Verification** | Test cases and results at every level; opens the **Verification Coverage** view | **Work in progress** |
| **Review** | Baselines | Planned — no engines yet |
| **Systems** | Design, ICDs and hazards | Planned — no engines yet |

A work-in-progress capability says *work in progress* in the picker. It is a tag, never a
switch: what ships runs in full.

The Software and Verification lanes show your files in the directories they are really in,
not as a flat list. `*.trace` files are not listed in either lane — a trace file declares
bindings for Sanad to read, it is not an artifact you browse.

There is also **Sanad: Work On...**, which scopes the explorer to a single work item and
what it touches, in two lanes, so a change and its evidence sit side by side.

> *Screenshot placeholder — the capability picker, with the WIP tags visible.*

---

## What good looks like after setup

- The Requirements view lists your requirements, grouped by type.
- The run log names every source it read, with a count, and every engine that stood down,
  with the role that would arm it.
- Nothing in `.ejadah/rew/` surprises you when you read the diff — it says what you told it.

---

## If the tree is still empty

Almost always one of these, in this order:

1. VS Code was **reloaded** instead of fully quit after installing.
2. The folder you opened is not the one containing `.ejadah/rew/`.
3. Setup has not been run in that folder yet.
4. The template's `folder:` does not match where the files actually are.

The Problems panel names anything that failed to load and why. A file that could not be
read is reported there, never silently dropped.

---

## Where to go next

| | |
|---|---|
| The full reference — roles, rule packs, baselines, the CI gate | **[User Guide](USER_GUIDE.md)** |
| Installing, checksums, known gaps, the 45-day expiry | **[downloads/README.md](downloads/README.md)** |
| Something is wrong | **[Open a bug](https://github.com/ejadahailabs/E-REW-support/issues/new?template=bug_report.yml)** — 72-hour target |
| Anything else | **support@ejadahailabs.com** |

Section 4 of the User Guide, on **roles**, is the ten minutes that makes everything else
make sense.

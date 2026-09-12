# Test guide — Sanad test build `Sanad@81bb6f4e`

**This is a test build, not a release.** No version was bumped, no tag was cut, nothing went to
the Marketplace. **0.6.3 is still the current release** — `sanad-0.6.3.vsix` in this folder — and
this build carries the same version stamp because it is a build of `main` taken after that tag.

| | |
|---|---|
| **Build** | `Sanad@81bb6f4e` — `ejadahailabs/Sanad` main |
| **Date built** | 2026-09-12 |
| **Version stamp** | 0.6.3 — **the same number as the release you already have installed.** See the install note below: a same-version install needs `--force` or an uninstall first |
| **File** | `downloads/e-rew-test-build.vsix` |
| **Size** | 5 621 476 bytes |
| **SHA-256** | `847b648cf9dc38908836f10f012f9f51f4720750da82b4c35d6e4198ea795aec` |
| **Expires** | 2026-10-24 — unchanged from the 0.6.3 release; this build moves no version constant |
| **CI at that sha** | The last commit with product code is `2031c26a` (PR #1114). **acceptance-gate, Conformance and board-sync are green there; CI is red for one reason only — the derived page `qual/ATLAS.html` is stale** (`npm run atlas:check` stops the chain before the tests run). The same tree was green on the PR head `2fdf0e75` before the merge, and the suite was re-run against this build: every assertion passes bar one timing budget on the build machine. No product test fails. `81bb6f4e` itself is a `[skip ci]` assertion-floor bump over `2031c26a` |

**What is new since the slot last changed (it held the 0.6.3 release build, `Sanad@f8b2e70`):**
**the model editor — all nine slices E1–E9, `SAN-HLR-132`…`-148`.** A folder of `.sysml` files now
opens as **one project**: a containment tree of everything the files declare, a properties
inspector, a palette, quick actions and a context menu that are **one operation set**, a text dock
that writes the same text the pointer does, and an assistant that is a **peer of the hand rather
than a privileged path** — every proposal shown before anything is written, accepted item by item,
undoable in the steps it was accepted in. **A view is a file** under `<root>/views/`: one tab per
`view` usage, a new view costs one file and changes **no package file**, and the arrangement you
drag rides at the end of the view file as a metadata package — no coordinate ever enters a model
element. **The requirement package is generated** from the Markdown corpus and refuses to be typed
into. And every file this writes is read in our own CI by the **OMG SysML v2 Pilot Implementation**
(0.61.0) with **0 errors and 0 warnings** — section 3a, `ME-T06`.

Also on this build: **#915 is fixed** — a workspace file that will not parse as YAML is now named
on the CLI, the runner and the gate instead of being silent there (PR #1077). And the **review
capability** (`Sanad: Open Review` and the AI review drafts) has started landing on `main`; it has
**no hand-test rows yet** and is listed in section 6, not tested here.

**Your signatures from the last pass still count** for the canvas, the SysML reader and the
palette: not one of those three features' `tests.md` changed between the 0.6.3 release and this
build. The one change to a test document is that **`DG-V2b-T01` left the canvas feature** — see the
note in section 3b.

| Feature | Rows in this guide | Not runnable at the keyboard |
|---|--:|--:|
| **The model editor — NEW (ME-T01 … ME-T06)** | 42 | 4 read from CI · 1 partly |
| Voice-driven design canvas (DG-V1 … DG-V7) | 27 | 0 |
| SysML v2 reader, profile and diagram kinds | 30 | 1 partly |
| Shape palette (DG-V6-T01 … T04) | 28 | 0 |
| **Total** | **127** | **4 from CI · 2 partly** |

---

## 1. Install (5 minutes)

| Step | Do this |
|---|---|
| 1 | **Uninstall any older Sanad or E-REW build first.** Extensions view → search `Sanad`, and search `E-REW`. Remove both if present. 0.6.0 renamed the extension, so VS Code treats the old `E-REW` id as a *different* extension, not an older one — both activate on the same folder and the old one can win the window. |
| 2 | Download the build: `curl -LO https://raw.githubusercontent.com/ejadahailabs/E-REW-support/main/downloads/e-rew-test-build.vsix` |
| 3 | Check what you downloaded: `sha256sum e-rew-test-build.vsix` — it must read `847b648cf9dc38908836f10f012f9f51f4720750da82b4c35d6e4198ea795aec` |
| 4 | Install: **`code --install-extension e-rew-test-build.vsix --force`**. **The `--force` matters this round:** this build's version stamp is **0.6.3**, the same number as the release you already have, and VS Code skips an install of a version it thinks is already there. Without it nothing changes and the old build stays. From the Extensions view instead: uninstall Sanad first, then `...` menu → *Install from VSIX...* |
| 5 | **Quit VS Code completely** — `Ctrl+Q`, or Quit from the menu. A window reload is **not** enough: it does not swap the extension host, and the old build stays live. This is the most common reason a fresh install "does nothing". |
| 6 | Reopen VS Code on your test repository. |

**Confirming you are on this build:** Extensions view → Sanad → the version reads **0.6.3** — which the release also reads, so the version number alone cannot tell the two apart.
There is no command in this build that prints the commit sha. The sha is in this file, and in
the downloads README row for the slot — so the version number plus the checksum in step 3 is
how you know which build you have: check the file you installed, not the number on screen. The quickest tell in the product is that **`Sanad: Open Design` exists** — it does not in 0.6.3.

**Requires VS Code 1.90 or newer.**

---

## 2. Connect the AI — already done on your machine, one line to re-check

| Check | Do this | Expected |
|---|---|---|
| AI provider | Command Palette → **`Sanad: Verify LLM Connection`** | *Reachable, authenticated, and claude-sonnet-5 answered* |
| Dictation | Press **Ctrl+Alt+V** in any ordinary editor and speak | Your words appear as text. If the mic does not appear, quit VS Code fully (Ctrl+Q) and reopen — VS Code Speech needs a full restart, not a reload. |

Dictation is **VS Code's own Speech extension**, not Sanad. Sanad has no microphone in this
build and touches no audio at all: it receives text that is already text.

---

## 3. Features ready to test

Every row below is copied **verbatim** from that feature's `tests.md` on `main`. Nothing has
been reworded. Where a row cannot be run on this build, it says so instead of being rewritten.

Tick the box when the criterion passes. Sign the block at the end of each test.

---

## 3a. The model editor — ME-T01 … ME-T06  (NEW in this build)

Source: `qual/capabilities/system-design/e1-e9-model-editor/tests.md` · issue **#958** · slices
**E1–E9**, all merged. Design approved by you on 2026-09-10; all 17 HLRs carry their
`@implements` markers.

> **Read this before you start.** That `tests.md` still opens with *"NOT YET TESTABLE: BUILD
> PENDING"* and *"build pending"* in its own header. **That header is stale, and the rows are not.**
> It was written at design time, before any slice shipped; all nine have since merged and stage 4
> ran against them on 2026-09-11. The 42 rows below are copied verbatim from it and every one of
> them is about behaviour that is in this build. The header line is a ledger tidy-up owed, not a
> warning to you.

**Stage 4 found two defects and both are fixed in this build** — [#1052](https://github.com/ejadahailabs/Sanad/issues/1052)
and [#1053](https://github.com/ejadahailabs/Sanad/issues/1053), closed by PR #1061. One question is
left open and it is yours to answer, not a bug to find: [#1063](https://github.com/ejadahailabs/Sanad/issues/1063),
in the note under `ME-T03`.

### ME-T01 — A folder of `.sysml` files opens as one project

**How to reach it:** a repository whose `.ejadah/rew/config.yaml` declares a design root —

```yaml
design:
  roots:
    - "design"
```

— then **`Sanad: Open Design`** (Command Palette). Every `.sysml` file under the root is one file of
one project; the file list sits above the containment tree. There is **no default root**: with no
`design:` block Sanad reads no project at all. **`Sanad: Regenerate Requirement Package`** is step 6.

**A project to run this on, already built for it:** `fixtures/model-editor-b/` in the Sanad
repository — five package files, two view files, three Markdown requirements, one `.sysml` outside
the root, and every finding this test asks for already staged. Copy the folder somewhere of its own
and `git init` it, so the `git status` / `git diff` steps have a repository to answer for. Every id
and sentence in it is invented; nothing in it came from any programme.

**What to do**

1. Open the folder. Look at the project file list above the containment tree.
2. Rename one package file so its name no longer matches the package it declares. Re-open.
3. Expand the containment tree to the leaves of all three packages.
4. Drag an element from one package to an owner in another package. Then run `git status` and
   `git diff`.
5. Open `design/packages/requirements.sysml`. Try to type into it in the text dock.
6. Add a requirement to the Markdown corpus, then regenerate. Compare the file's text with the text
   produced by regenerating a second time with no corpus change.
7. In a package file, write a `satisfy` naming a requirement id that no Markdown requirement
   declares. Then look at the check list, and at the assistant panel.
8. Close and re-open the folder without changing anything. Compare the check list with the one from
   step 7.

**Pass criteria**

| ✔ | Row | Pass criterion (verbatim) |
|:-:|---|---|
| ☐ | **ME-T01.01** | The file list names every `.sysml` file under `design/` and the package each one declares. No file outside the declared root is listed. |
| ☐ | **ME-T01.02** | Step 2 is reported as a **finding naming both the file and the package**, and the file is still read and its elements still appear in the tree — it is not refused. |
| ☐ | **ME-T01.03** | Step 3: every element every package file declares appears in the tree, in the order its file owns them. |
| ☐ | **ME-T01.04** | Step 4: exactly **two** files are modified — the one the element left and the one it joined. Every other file in the project is byte-identical. |
| ☐ | **ME-T01.05** | Step 5: the requirement package is marked **generated** in the file list, and typing into it is refused with a message naming the **Markdown requirement file** to edit instead. |
| ☐ | **ME-T01.06** | Step 6: the new requirement appears in the generated package, and the second regeneration produces **byte-identical** text. |
| ☐ | **ME-T01.07** | Step 7: the unresolved id is reported as a check, **naming the id**, and it appears in the deterministic check list — not in the assistant panel, and not mixed with it. |
| ☐ | **ME-T01.08** | Step 8: the check list is identical to step 7's, item for item. |

Build sha `81bb6f4e` · date ________ · signed ________

### ME-T02 — The inspector, the palette, the quick actions and the context menu are one operation set

**How to reach it:** with the design open, the **properties inspector** is beside the tree, the
**palette** is on the view, **quick actions** are on the selected element, and the **context menu**
is the right-click on it. The **text dock** is the file open in the editor beside them.

**What to do**

1. Select a part. Read the properties inspector top to bottom against the notation's fields for a part usage.
2. Change its name in the inspector. Then set a field to a value the notation cannot take (a multiplicity of `two`, a type that does not exist).
3. Open the palette on this view. Note which tools are offered, and ask for a tool that is not.
4. Place a part with a single-placement selection; then hold the tool and place three ports without re-selecting it.
5. Create the same connection three ways in three fresh copies of the project: from the palette, from the selected element's quick actions, and from its context menu. Diff the three resulting files.
6. Ask each of the four surfaces in turn — palette, inspector, containment tree, text dock — to produce a construct the subset does not admit (for example a `calc def`). Read all four messages.
7. Choose the **connection** tool from the palette and click **one** element. Then click a second element and read the file. Repeat with **interface**, **connection def** and **interface def**.

**Pass criteria**

| ✔ | Row | Pass criterion (verbatim) |
|:-:|---|---|
| ☐ | **ME-T02.01** | Step 1: every field the notation gives that element is shown; a field with no value is shown as empty and named, never omitted. |
| ☐ | **ME-T02.02** | Step 2: the rename is written to the file; the impossible value is **refused naming the field and the reason**, and the file is byte-identical afterwards. |
| ☐ | **ME-T02.03** | Step 3: the palette offers only tools the active view's rendering can draw, and the tool it does not offer is **named**, with why it is not offered. |
| ☐ | **ME-T02.04** | Step 4: the single placement returns the pointer to selection; the held tool places three ports without re-selection. |
| ☐ | **ME-T02.05** | Step 5: the three files are **byte-identical**. |
| ☐ | **ME-T02.06** | Step 6: all four messages **name the construct** and say it is **outside Sanad's subset** — none of them says the text is invalid SysML — and nothing is written on any of the four. |
| ☐ | **ME-T02.07** | Step 7: one click writes **nothing** — the tool waits for the second end and says so; the second click writes the relation **with both ends in it**, and no bare `connection …;` / `interface …;` appears anywhere in the file (#1023). This criterion sits under no `@verifies` sentence above: it checks the property the code carries no `@implements` for — *an authoring action that would write text the reference implementation rejects is refused* — which none of `SAN-HLR-135`, `-136`, `-137` or `-140` says. It is here on `ADR-0117` decision two and the owner's ruling of 2026-09-11, and it moves under `SAN-HLR-148` if that sentence ever grows a writer clause (stage 1, the owner's). R3-1035 F4. |

**`ME-T02.07` is the one-click strip fix** (#1023, fixed in #1035): choosing **connection**,
**interface**, **connection def** or **interface def** and clicking **one** element must write
**nothing at all** — the tool waits for the second end and says so. A bare `connection …;` in the
file is the failure it is looking for.

Build sha `81bb6f4e` · date ________ · signed ________

### ME-T03 — Delete from the view, delete from the model, and undo everything

**How to reach it:** *remove from the view* is on the element **in the view**; *remove from the
model* is on the element **in the containment tree**. Undo is VS Code's own undo (`Ctrl+Z`) — the
edits are ordinary workspace edits.

**What to do**

1. Take a `git status` baseline. Remove the part **from the view**. Run `git diff`.
2. Undo. Confirm the project is back.
3. Remove the same part **from the model**. Read what is shown **before** anything is written.
4. Confirm the removal. Run `git diff` across the whole project.
5. Undo. Run `git diff` again.
6. Ask the assistant for a change, accept one proposed item, then undo. Run `git diff`.
7. Drag three elements to new positions, then undo three times.

**Pass criteria**

| ✔ | Row | Pass criterion (verbatim) |
|:-:|---|---|
| ☐ | **ME-T03.01** | Step 1 changes **only the view file** — the `expose` clause. No package file changes and no other view file changes. |
| **partly** | **ME-T03.02** | Step 3 shows, before writing anything, **every element and every view that names the part** — the two views, the `connect`, the `satisfy`. |
| ☐ | **ME-T03.03** | Step 4 removes the part, both views' references to it, and **its layout entries**, in one change; `git diff` shows one coherent change and no orphan reference is left behind. |
| ☐ | **ME-T03.04** | Step 5: `git diff` is **empty** — every file is byte-for-byte what it was at step 1's baseline. |
| ☐ | **ME-T03.05** | Step 6: the accepted assistant item is one undoable step, and after the undo `git diff` is empty. |
| ☐ | **ME-T03.06** | Step 7: three drags undo in three steps, not one and not six. |

**`ME-T03.02` — half of it is an open question, not a defect to find.** The impact preview names a
view that `expose`s the element **by name** (`AuditView`). A view that exposes an **ancestor** with
a wildcard — `expose Hydraulics::supply::**`, which *shows* the element without *naming* it — is
**not** in the preview. Whether a wildcard should count as naming what it shows is a stage-1
question and yours: **[#1063](https://github.com/ejadahailabs/Sanad/issues/1063)**. Tick the row for
the named view, and leave the wildcard half for that decision.

**`ME-T03.01` and `.03` were the two defects of the last round** — #1052 and #1053, both fixed in
PR #1061 and both now asserted. They are the rows to look hardest at.

Build sha `81bb6f4e` · date ________ · signed ________

### ME-T04 — A view is a file, its layout rides with it, and the model never notices

**How to reach it:** **`Sanad: Open Design Views`** lists exactly the `view` usages your files
declare — never a fixed menu of diagram kinds. **`Sanad: New View…`** is step 2.
**`Sanad: Drop Layout`** is the supported way to do step 6 (deleting the layout package by hand is
what the row asks for; the command is there if you would rather not edit the file).

Anything under `<root>/views/` is read as a view file; anything else under the root is a package
file — the folder is a convention, not a setting.

**What to do**

1. Look at the tabs across the diagram area, and at the `view` usages in `design/views/`.
2. **New view** over a selection of elements. Run `git status`.
3. Drag three elements and bend one edge in the new view. Open the view file in the text dock and read the end of it.
4. Close and re-open the project. Compare positions and bend points with step 3.
5. Run `git diff` over `design/packages/`.
6. Delete the layout package from the view file by hand. Re-render.
7. Delete the whole view file. Re-open the project.
8. Run the project's files through the conformance job (see `ME-T06`), layout package included.

**Pass criteria**

| ✔ | Row | Pass criterion (verbatim) |
|:-:|---|---|
| ☐ | **ME-T04.01** | Step 1: there is **one tab per `view` usage** found under the views root — no tab exists that no `view` usage declares, and no `view` usage is missing a tab. |
| ☐ | **ME-T04.02** | Step 2 creates **one new file** under the views root and modifies **no package file**. |
| ☐ | **ME-T04.03** | Step 3: the arrangement is a **metadata package at the end of the view file**, annotating elements by qualified name; **no element in any package file gained an attribute**. |
| ☐ | **ME-T04.04** | Step 4: positions and bend points are identical to step 3. |
| ☐ | **ME-T04.05** | Step 5: `git diff` over `design/packages/` is **empty** after steps 2, 3 and 4. |
| ☐ | **ME-T04.06** | Step 6: the file still reads, and the view renders with the computed arrangement — no error, no blank picture. |
| ☐ | **ME-T04.07** | Step 7: the model reads unchanged and every package file is byte-identical; the deleted view's tab is gone and no other tab changed. |
| ☐ | **ME-T04.08** | Step 8: the reference implementation accepts every file, layout package included, with **0 errors and 0 warnings**. |

Build sha `81bb6f4e` · date ________ · signed ________

### ME-T05 — One engine: the dock, the pointer and the assistant reach the file the same way

**How to reach it:** the **text dock** is the `.sysml` file open in an editor beside the design
view. The assistant is the same AI provider section 2 checks — the design panel's own proposal
list, not the canvas's **Ask Sanad**.

**What to do**

1. Add a port by typing it in the text dock. Watch the picture.
2. Add a second port with the palette. Diff the two additions in the file.
3. In the dock, delete a closing brace. Try to apply.
4. In the dock, type a construct outside the subset. Try to apply.
5. Take a `git status` baseline. Ask the assistant for something that would produce text the subset does not admit. Accept it.
6. Ask the assistant for a change that touches two package files. Read the panel.
7. Accept **one** item of a multi-item proposal, reject another, and leave a third undecided. Run `git diff`.
8. List the operations the palette, the inspector, the tree and the context menu offer. List the operations the assistant may propose. Compare the two lists.

**Pass criteria**

| ✔ | Row | Pass criterion (verbatim) |
|:-:|---|---|
| ☐ | **ME-T05.01** | Step 1: the picture redraws from the typed text with no separate save, and the file holds exactly what was typed. |
| ☐ | **ME-T05.02** | Step 2: the two ports are written in the same form — the palette's text is indistinguishable in shape from the typed text. |
| ☐ | **ME-T05.03** | Step 3 is refused **naming the line**, and the file is byte-identical afterwards. |
| ☐ | **ME-T05.04** | Step 4 is refused **naming the construct** and saying it is outside Sanad's subset, and the file is byte-identical afterwards. |
| ☐ | **ME-T05.05** | Step 5 is refused with the same sentence as step 4, and **`git diff` is empty** — the acceptance wrote nothing. |
| ☐ | **ME-T05.06** | Step 6: each item shows **where it came from**, and the two files' changes are separate items a person accepts on their own. |
| ☐ | **ME-T05.07** | Step 7: exactly the accepted item is in the file; the rejected item is not; the undecided item is not; nothing is written for the undecided one when the panel is closed. |
| ☐ | **ME-T05.08** | Step 8: the two lists are the **same set** — no operation is assistant-only, and none is hand-only. |

Build sha `81bb6f4e` · date ________ · signed ________

### ME-T06 — Every file Sanad writes is read by the published notation's reference implementation

> ### ROWS `.01`–`.03` ARE READ FROM CI, NOT RUN AT YOUR KEYBOARD
>
> The conformance job is **our** workflow on **our** repository — `.github/workflows/conformance.yml`,
> the OMG SysML v2 Pilot Implementation 0.61.0 with the standard library shipped in it, both halves
> of the pin checked at run time. It is not a thing the extension runs and it is not a thing a
> customer needs. To sign `.01`–`.03`, open the **Conformance** run on `main` and read it:
> **[github.com/ejadahailabs/Sanad/actions/workflows/conformance.yml](https://github.com/ejadahailabs/Sanad/actions/workflows/conformance.yml)**
> — it is **green on `2031c26a`**, the last product commit in this build.
>
> **`.04` needs a machine with no Java runtime installed.** If you have none to hand, leave it
> unticked rather than reason about it — the claim it checks is exactly that no engineer needs a JVM.
>
> **`.05` is an ordinary row you can run**: use the product for an hour and look for any claim about
> conformance. `SAN-HLR-098` keeps the reference implementation an **optional** extra for a
> customer's own project, and nothing in this build configures one.

**What to do**

1. Read the job's own output for the release and standard-library version it pinned.
2. Run the job over the project.
3. Introduce one construct Sanad would not write but which passes our own reader — a bare `import X::*;` with no visibility — by hand. Re-run.
4. On a machine with **no** Java runtime installed, open the project in Sanad, author with the palette, arrange a view, run the checks and render every view.
5. Look at what Sanad reports to the engineer during step 4 about conformance.

**Pass criteria**

| ✔ | Row | Pass criterion (verbatim) |
|:-:|---|---|
| **from CI** | **ME-T06.01** | Step 1: the job names the reference implementation **release** and the **standard-library version**, and both are pinned rather than latest. |
| **from CI** | **ME-T06.02** | Step 2: **0 errors and 0 warnings** across every file, layout package and generated requirement package included. |
| **from CI** | **ME-T06.03** | Step 3 fails the job, and the failure names the **file, the line and the message**. |
| **no-JVM box** | **ME-T06.04** | Step 4: every action completes with no Java runtime present and no process is started that needs one. |
| ☐ | **ME-T06.05** | Step 5: Sanad claims conformance only where it has been checked, and never states or implies that a customer's own file has been checked by the reference implementation unless they configured it (`SAN-HLR-098`). |

Build sha `81bb6f4e` · date ________ · signed ________

---

## 3b. Voice-driven design canvas — DG-V1 … DG-V7

Source: `qual/capabilities/design-canvas/v0-v5-voice-canvas/tests.md` · issue **#762**

**The two fixes from your last pass are still in — they arrived in the `Sanad@19e79f78` build and
nothing since has touched them:**

* **#899** — a diagram card no longer prints the note's raw SysML (`package … { }`) under the picture.
* **#900** — `Sanad: Diagram Kind…` is reachable from the card's own **Diagram kind…** button, and when the dictation pad has focus it says *"Open the note or file holding the diagram first."* instead of returning silently.

**On the canvas:** every card that holds a diagram now carries the **full shape
palette** — section 3d. So the DG-V7-T01 card below is also the card you test the palette on.

**Getting to the canvas at all:** Command Palette → **`Sanad: New Workspace...`** (or open an
existing one from the `SANAD WORKSPACES` view) → **`Sanad: Open Workspace Canvas`**.

**Project to test on:** FMS Demo — `~/Masood/Office_Projects/fms-demo`.

### DG-V1-T01 — A workspace note can be written by the product

**How to reach it:** on the canvas, click **Describe it** to open the pad, type or dictate, then
click **Add as Note**. Edit and delete are on the note card itself. The workspace file is
`.ejadah/rew/workspaces/<name>.yaml`.

**What to do**

1. With a workspace open, use the product to add a note (no hand-editing of YAML).
2. Read the workspace file.
3. Edit the note through the product; read the file again.
4. Delete the note through the product.

**Pass criteria**

| ✔ | Row | Pass criterion (verbatim) |
|:-:|---|---|
| ☐ | **DG-V1-T01.01** | `WorkspacePatch` carries a `notes` field — a note can be created without hand-editing the file. |
| ☐ | **DG-V1-T01.02** | The note is written into the workspace file, with an id and a title. |
| ☐ | **DG-V1-T01.03** | No artifact content is written alongside it. |
| ☐ | **DG-V1-T01.04** | Edit and delete both round-trip. |
| ☐ | **DG-V1-T01.05** | The file stays valid YAML that the S1 reader loads without warnings. |

Build sha `81bb6f4e` · date ________ · signed ________

### DG-V2-T01 — The dictation pad turns speech into a note card

**How to reach it:** the **Describe it** button on the canvas. Dictate with **Ctrl+Alt+V** into
the pad that opens beside it. The output channel is View → Output → **Sanad**.

**What to do**

1. Open a canvas. Click **Describe it**.
2. Dictate two sentences.
3. Click **Add as note**.
4. Check the workspace file, and check the Sanad output channel for any model call.

**Pass criteria**

| ✔ | Row | Pass criterion (verbatim) |
|:-:|---|---|
| ☐ | **DG-V2-T01.01** | A **Describe it** button exists on the canvas. |
| ☐ | **DG-V2-T01.02** | The dictated text is captured and shown before it is committed. |
| ☐ | **DG-V2-T01.03** | **Add as note** creates a note card on the canvas and a note in the file. |
| ☐ | **DG-V2-T01.04** | **No model call is made** — this slice has no AI. |
| ☐ | **DG-V2-T01.05** | Discarding instead of adding leaves the workspace file untouched. |

Build sha `81bb6f4e` · date ________ · signed ________

### DG-V2b-T01 — moved out of this feature, and not in this guide

> **`DG-V2b-T01` is no longer a row of this feature.** On 2026-09-11 you ruled *"D2 and D3 go with
> your recommendation"*, and slice **V2b — Sanad's own microphone** — which was never commissioned —
> moved with `SAN-HLR-094` to its own feature, `design-canvas/v2b-microphone/`. **Not one character
> of the requirement or of the test changed**; what changed is which feature owns them, so that a
> finished feature was no longer held at `draft` by a slice nobody asked for.
>
> Nothing about the build changed with it: **Sanad still has no microphone and touches no audio at
> all.** Dictation is VS Code's own Speech extension typing into an ordinary editor. The eight rows
> that used to be carried here for the count are in that feature's own `tests.md`.

### DG-V3-T01 — The AI proposes; the engineer confirms or discards

**How to reach it:** from the dictation pad, the **Ask Sanad** button (Command Palette:
**`Sanad: Ask Sanad`**). It uses the AI provider you connected in section 2.

**What to do**

1. Dictate a description as in DG-V2-T01, then click **Ask Sanad**.
2. Read the proposal.
3. Click **Discard**. Check the workspace file.
4. Repeat and click **Confirm**. Check the file.
5. Ask something that names an artifact **not** in the workspace.
6. Watch the wire for the whole of steps 1–5.

**Pass criteria**

| ✔ | Row | Pass criterion (verbatim) |
|:-:|---|---|
| ☐ | **DG-V3-T01.01** | The proposal is shown before anything is written. |
| ☐ | **DG-V3-T01.02** | Discard writes nothing at all. |
| ☐ | **DG-V3-T01.03** | Confirm writes exactly what was shown — no extra edits. |
| ☐ | **DG-V3-T01.04** | The context sent is the open workspace, and the assistant says which artifacts it used. |
| ☐ | **DG-V3-T01.05** | Step 5: the assistant says the artifact is outside the workspace rather than inventing an answer. |
| ☐ | **DG-V3-T01.06** | No artifact is modified by this slice — only notes and diagrams. |
| ☐ | **DG-V3-T01.07** | Step 6: the only endpoint contacted is the provider in `config.yaml`; no audio and no other endpoint, checked on the wire. |

Step 6 needs a packet capture or a proxy. If you cannot watch the wire tonight, run rows
`.01`–`.06` and leave `.07` unticked with a note — do not tick it from reading the code.

Build sha `81bb6f4e` · date ________ · signed ________

### DG-V5-T01 — The traceability diagram is generated from the data, not written by the AI

**How to reach it:** right-click a Workspace row in the `SANAD WORKSPACES` view →
**Diagram this Workspace** (Command Palette: **`Sanad: Diagram this Workspace`**).

**What to do**

1. Right-click a workspace → **Diagram this workspace**.
2. Compare every node and edge against the workspace's own artifacts and links.
3. Check the Sanad output channel for a model call.
4. Add a link in the corpus, reload, and regenerate.

**Pass criteria**

| ✔ | Row | Pass criterion (verbatim) |
|:-:|---|---|
| ☐ | **DG-V5-T01.01** | Every node in the diagram is an artifact in the workspace. No extras. |
| ☐ | **DG-V5-T01.02** | Every edge is a link that exists in the graph. No invented edges. |
| ☐ | **DG-V5-T01.03** | **No model call is made** — the diagram is derived, not generated. |
| ☐ | **DG-V5-T01.04** | Step 4's new link appears after regeneration. |
| ☐ | **DG-V5-T01.05** | Regenerating twice with no change produces an identical diagram — byte for byte. |
| ☐ | **DG-V5-T01.06** | The generated text is SysML v2 (`requirement def` with `subject`, `satisfy`, `verify`) — **not** Mermaid — and the V4 subset parser accepts it unchanged. |

Build sha `81bb6f4e` · date ________ · signed ________

### DG-V7-T01 — A diagram card draws itself on the canvas

**How to reach it:** put a note on the canvas whose text holds a SysML v2 block (a fenced
`package … { }`). The card draws it. **This is also the card the shape palette in 3c sits on.**

**What to do**

1. On a VS Code with **no** Markdown or Mermaid extension installed, open a workspace holding a SysML v2 diagram note.
2. Look at the card on the canvas.
3. Edit the note text; look again.
4. Search the workspace file and the repository for the drawn output (Mermaid text or SVG).
5. Read the canvas webview's Content-Security-Policy.

**Pass criteria**

| ✔ | Row | Pass criterion (verbatim) |
|:-:|---|---|
| ☐ | **DG-V7-T01.01** | Step 2: the card shows the picture, drawn by the layout engine bundled in the `.vsix`. |
| ☐ | **DG-V7-T01.02** | Step 3: the picture follows the text without reopening the canvas. |
| ☐ | **DG-V7-T01.03** | Step 4: nothing drawn is stored anywhere — the picture is derived on render. |
| ☐ | **DG-V7-T01.04** | Step 5: the only script allowed is Sanad's own, by nonce, from `localResourceRoots`; no remote origin is allowed. |

While you are on this card, the **#899** fix is what you check by eye: the card shows the
picture, the *Not drawn* report, and your own prose — and **no `package … { }` source** under it.

Build sha `81bb6f4e` · date ________ · signed ________

---

## 3c. SysML v2 reader, profile and diagram kinds

Sources: `qual/capabilities/system-design/v4-sysml-v2/tests.md` (DG-V4, DG-V4c) and
`qual/capabilities/software-design/v4b-v7-profile-and-palette/tests.md` (DG-V4b) · issue **#860**

**Fixed before the 0.6.3 release, and still fixed here: #898.** A `requirement def` you write *inside* a diagram used to be refused
with *"no requirement declares …"*, which was untrue — you had declared one, just not in the
requirements corpus. It now says the id **was declared locally in the diagram and is not part of
the requirements corpus**, and tells you to use the repository id in quotes to trace to a real
requirement. The relation is still not drawn — Markdown remains the one place a requirement is
declared — but the sentence is now true. On the picture, that requirement's box reads
**`NAME — diagram-local`**; a `requirement def` whose id the corpus **does** declare gets no such
label, so the word only ever appears where it is correct.

**How to reach these at all**

| Surface | How |
|---|---|
| Diagram panel beside the file you are editing | Command Palette → **`Sanad: Preview Diagram`** — it redraws as you type |
| Choose which kind of diagram is drawn | Command Palette → **`Sanad: Diagram Kind…`**, or the **Diagram kind…** button on a canvas card |
| The software profile | `.ejadah/rew/SoftwareProfile.sysml`. You do not have to write it: run **`Sanad: Diagram Kind…`** in a repository with no profile and Sanad **offers** to create the recommended one — *Create it* or *Not now* |
| Where a diagram lives | SysML v2 text inside a workspace note, or a `.sysml` file open in the editor |

### DG-V4-T01 — A diagram is SysML v2 text in a note, parsed before it is stored

**What to do**

1. Create a note containing a SysML v2 fragment from the subset — a `package` with an `import`, a `doc`, a `part def`, an `attribute`, a `port def` and a `requirement def` with `satisfy`.
2. Look at the rendered card, and run `git diff` on the workspace file.
3. Break the syntax deliberately (drop a closing brace) and re-render.
4. Write a valid SysML v2 construct that is **outside** the subset.
5. Ask the AI (V3) to draft a diagram, and inspect what reaches the file.
6. Search the workspace file and the repository for Mermaid text.
7. In the note, write `satisfy` naming a requirement id that exists only as a Markdown requirement file in the repository; then one naming an id nothing declares.
8. In a `port def` body write a port with a direction — `in attribute airspeed : Real;` — and in an `interface def` body declare an `end`. Re-render.
9. In a `state def` write two states and a `transition` between them: `transition first Cruise then Descent;`. Re-render.
10. Write a `flow` between two parts and a `succession` between two actions. Then write `assert not satisfy` naming a requirement, and look at what the trace says about it.

**Pass criteria**

| ✔ | Row | Pass criterion (verbatim) |
|:-:|---|---|
| ☐ | **DG-V4-T01.01** | The note holds **SysML v2 textual notation**. No binary, no drawing file, no coordinates. |
| ☐ | **DG-V4-T01.02** | `git diff` shows the diagram as readable SysML v2 text. |
| ☐ | **DG-V4-T01.03** | The card renders a picture from the Engineering Graph facts the reader produced. |
| ☐ | **DG-V4-T01.04** | Step 3 reports the syntax error **by line**, and nothing is written to the file. |
| ☐ | **DG-V4-T01.05** | Step 4 is **refused by name** — the message names the construct and says it is outside Sanad's subset, **not** that the text is invalid SysML. |
| ☐ | **DG-V4-T01.06** | Step 5: model output is parsed **before** it reaches the file; unparseable output is shown to the engineer, never saved. |
| ☐ | **DG-V4-T01.07** | Step 6: **no Mermaid text is stored anywhere.** Mermaid may exist only as transient render input. |
| ☐ | **DG-V4-T01.08** | No Java process is started, and the extension works with no JVM installed. |
| **partly** | **DG-V4-T01.09** | With the optional OMG validator configured and Java present, its findings appear **in addition to** ours; with it unconfigured, nothing degrades. |
| ☐ | **DG-V4-T01.10** | Step 7: the Markdown requirement resolves to the same Engineering Graph node the requirement explorer shows, and the trace view lists the diagram's `satisfy` on it; the undeclared id is refused **naming the id**. |
| ☐ | **DG-V4-T01.11** | Step 8: the port's **direction** and the interface's **end** are read — the note stores, the card draws, and neither is refused. A `port def` whose body cannot state a direction is the gap #860 was filed for. |
| ☐ | **DG-V4-T01.12** | Step 9: the `transition` is read inside the `state def`, and the card shows the two states joined in the direction written. |
| ☐ | **DG-V4-T01.13** | Step 10: the `flow` and the `succession` are read, and each appears in the picture as a directed line. |
| ☐ | **DG-V4-T01.14** | Step 10: `assert not satisfy` is read and its **negation is carried** — the trace does **not** show the requirement as satisfied by that part. A negated satisfaction read as a plain one is a trace link that says the opposite of the text. |

**`DG-V4-T01.10` is where you check the #898 fix.** Step 7's second half — *"then one naming an id
nothing declares"* — is the refusal whose wording changed. Write a `requirement def ReqSys001`
inside the note and a `satisfy ReqSys001 by …` beside it, and read the sentence: it must name the
id as diagram-local rather than say no requirement declares it, and the box on the picture must
read `ReqSys001 — diagram-local`. The row's own criterion is unchanged and still asks for a refusal
naming the id — that is what it is.

**`DG-V4-T01.09` — only its second half is runnable on this build.** There is no configuration
in this build that plugs in the official OMG validator; `SAN-HLR-098` keeps it as a future
option. Check *"with it unconfigured, nothing degrades"* and leave the first half unticked.

**Rows `.11`–`.14` ARE runnable** even though **#860** is still open as an issue: the widened
subset shipped in an earlier build (34 constructs — `in`/`out`/`inout` directions, interface
`end`, transitions, flows, successions, `assert not satisfy`). The issue is open for the
requirement wording, not for missing code.

Build sha `81bb6f4e` · date ________ · signed ________

### DG-V4c-T01 — Each diagram kind is drawn in its own shape

**How to reach it:** open the model, then **`Sanad: Diagram Kind…`**. The picker lists only the
kinds your model affords *and* your profile declares. Step 7's unsupported kind in the shipped
profile is the **sequence** diagram — it is deliberately not offered.

**What to do**

1. With a model holding `part def` and `interface def` declarations, request a **block** diagram.
2. With a model holding `port def` and connector ends, request an **internal block** diagram.
3. With a model holding `state def` and transitions, request a **state machine** diagram.
4. With a model holding `action def` and successions or flows, request an **activity** diagram.
5. With a model holding `requirement def`, `satisfy` and `verify`, request a **requirement** diagram.
6. With a model holding `use case def`, request a **use case** diagram.
7. Ask for a diagram kind the configured profile does not support. *(In the shipped profile that is a sequence diagram — see `ADR-0115`.)*

**Pass criteria**

| ✔ | Row | Pass criterion (verbatim) |
|:-:|---|---|
| ☐ | **DG-V4c-T01.01** | Step 1 draws a **block** diagram: parts as blocks, interfaces named. |
| ☐ | **DG-V4c-T01.02** | Step 2 draws an **internal block** diagram — ports on the boundary, ends connected — and not the block diagram of step 1. |
| ☐ | **DG-V4c-T01.03** | Step 3 draws a **state machine**: states, and one transition per declared transition. |
| ☐ | **DG-V4c-T01.04** | Step 4 draws an **activity** diagram: actions in the order the successions declare. |
| ☐ | **DG-V4c-T01.05** | Step 5 draws a **requirement** diagram — unchanged from V5, so the earlier slice still passes. |
| ☐ | **DG-V4c-T01.06** | Step 6 draws a **use case** diagram. |
| ☐ | **DG-V4c-T01.07** | Step 7 asks for a kind the configured profile does **not** support. Sanad **rejects it by name**, saying which kind and why it is not offered. It does not draw an approximation. |
| ☐ | **DG-V4c-T01.08** | Every kind is drawn through the same layout seam — no picture comes from a second renderer, and none is written to any file. |

Build sha `81bb6f4e` · date ________ · signed ________

### DG-V4b-T01 — A profile-annotated model draws UML-style, and the file stays SysML v2

**How to reach it:** steps 1–2 are **`Sanad: Diagram Kind…`** run in a repository with **no**
`.ejadah/rew/SoftwareProfile.sysml` — that is where the *Create it / Not now* offer appears.
Step 6 is deleting that file and reloading the window.

**What to do**

1. In a repository with no profile configured, open a model holding one plain type. Look at the picture, and look for an offer to add the recommended profile.
2. Accept the offer. Read the file it wrote. Edit one line of it. Ask again.
3. Mark a type as a class, give it attributes and an operation. Look at the picture.
4. Add a second marked type that specialises the first, and a third that depends on it. Look at the picture.
5. State a relation naming an id this repository does not declare. Look at the picture, and at what Sanad says.
6. Delete the profile. Reload. Look at the picture and at what Sanad says.
7. Open the marked model in another SysML v2 tool, or read it with the profile absent.
8. Draw the same model twice, and search the repository for the drawn output.

**Pass criteria**

| ✔ | Row | Pass criterion (verbatim) |
|:-:|---|---|
| ☐ | **DG-V4b-T01.01** | Step 1: the unmarked type draws as it did before any profile existed, and the profile is **offered**, never added unasked. *(`SAN-HLR-108`)* |
| ☐ | **DG-V4b-T01.02** | Step 2: accepting writes the profile; asking again **leaves the edited copy unchanged**. *(`SAN-HLR-108`)* |
| ☐ | **DG-V4b-T01.03** | Step 3: the marked type draws as a class box carrying its name, its attributes and its operations. *(`SAN-HLR-109`)* |
| ☐ | **DG-V4b-T01.04** | Step 4: the specialisation draws as a **hollow-headed arrow** and the dependency as a **dashed line**. *(`SAN-HLR-110`)* |
| ☐ | **DG-V4b-T01.05** | Step 5: the unresolved relation is **not drawn**, and Sanad reports it naming the relation and the id. *(`SAN-HLR-111`)* |
| ☐ | **DG-V4b-T01.06** | Step 6: with no profile configured, the plain view draws and Sanad **names the absent profile** — it does not substitute one of its own. *(`SAN-HLR-108`)* |
| ☐ | **DG-V4b-T01.07** | Step 7: the marked model is read by a tool that does not know the profile, without error. *(`SAN-HLR-113`)* |
| ☐ | **DG-V4b-T01.08** | Step 8: the same model drawn twice produces identical output, and no drawn picture is written to any file. *(`SAN-HLR-113`)* |

`DG-V4b-T01.07` needs a second SysML v2 tool. If you have none on the machine, take the step's
own alternative — *"or read it with the profile absent"* — and say in the note which you did.

Build sha `81bb6f4e` · date ________ · signed ________

---

## 3d. Shape palette — DG-V6-T01 … T04

Source: `qual/capabilities/software-design/v4b-v7-profile-and-palette/tests.md` · issue **#907**

> **Feature complete; rows `DG-V6-T01`…`T04` ready to run and sign.**
> The three parts landed before the 0.6.3 release — the drop (#908), connect · rename · keyboard (#909), and the
> profile's mark · the origin line (#912). Nothing in this section is carried here only for the
> count.

**How to reach it**

| Surface | How |
|---|---|
| The palette strip | A **strip of shapes on a canvas card that already holds a diagram**. Open a workspace canvas, add a note whose text holds a SysML v2 block (the DG-V7-T01 card), and the strip appears on that card. Drag a shape from the strip onto the picture. |
| Without the mouse | **Tab** to a palette tool, press **Enter** or **Space**. A *Where* row opens under the strip with a target list — *top level* or one of the elements the note declares — and focus moves into it. Choose, press **Enter**. **Escape** cancels and writes nothing. |
| Rename an element | The card's own **Rename…** button (also keyboard-reachable), then pick the element and type the new name. There is no drag for renaming. |
| Connect two ports | Drag from one port to another on the picture, or use the card's **Connect…** button and pick the two ports from the lists. |
| Narrow or extend the set | A `palette:` list in `.ejadah/rew/config.yaml`. `palette: []` means *no palette* and draws no strip at all; a name Sanad has no shape for is shown as *Not offered* rather than dropped silently. |
| The software profile | **`Sanad: Diagram Kind…`** run in a repository with no `.ejadah/rew/SoftwareProfile.sysml` **offers** to create the recommended one — *Create it* or *Not now*. That is the same offer `DG-V4b-T01` step 2 uses. |
| Where an element came from | The card prints an **`Origin:`** line under the picture when the graph recorded one for what the note declares. |

> **One honest limit, worth knowing before you run `DG-V6-T03`.** `SAN-HLR-128` says a dropped
> shape carries *the configured profile's own annotation*. The **shipped** profile only has words
> for three of the seven shapes — **part → `#Class`, interface → `#Interface`, action →
> `#Operation`**. It declares **no marker for a port, a state machine, a use case or a
> requirement**, so dropping one of those four writes exactly the same text with or without the
> profile. That is deliberate — it is a *software* profile, and inventing a marker it does not
> have would be worse — and your open question **`profile words`** is about whether it should have
> them. **`DG-V6-T03` still runs exactly as written:** its steps 3–5 use **Part**, which is one of
> the three that is marked.

### DG-V6-T01 — Dropping shapes writes the model, and the note parses after every drop

**Preconditions (verbatim):** a Workspace is open on the canvas with a diagram note. No software
profile is configured (that is `DG-V6-T03`).

**What to do**

1. Open the note in a second editor beside the canvas, so you can watch the text change.
2. Drag **Part** from the palette onto a place on the card where nothing is drawn. Read the note.
3. Type a new name for the element you just created, on the card itself, without touching the note. Read the note.
4. Drag **Port** from the palette and drop it **on** that element. Read the note.
5. Drag **Part** onto empty space again, then drag **Port** onto that second part. Read the note.
6. After each of steps 2–5, look at the card: does the picture draw, or does Sanad report a reason?
7. Close the Workspace, reopen it, and look at the picture and the note.

**Pass criteria**

| ✔ | Row | Pass criterion (verbatim) |
|:-:|---|---|
| ☐ | **DG-V6-T01.01** | Step 2: the note gains **one element at the top level** of the diagram — not nested inside anything. *(`SAN-HLR-122`)* |
| ☐ | **DG-V6-T01.02** | Step 2: that element arrives **already named**, with a placeholder name visible on the card and the same name in the note. *(`SAN-HLR-125`)* |
| ☐ | **DG-V6-T01.03** | Step 3: the note now carries **the name you typed**, in place of the placeholder, and you did not open the note to do it. *(`SAN-HLR-126`)* |
| ☐ | **DG-V6-T01.04** | Step 4: the port is written **inside the body of the element you dropped it on**, not beside it. *(`SAN-HLR-123`)* |
| ☐ | **DG-V6-T01.05** | Step 5: the second part is top level and its port is nested in it — the same shape dropped in two places produced two different models. *(`SAN-HLR-122`, `SAN-HLR-123`)* |
| ☐ | **DG-V6-T01.06** | Step 6: after **every one** of those actions the picture draws and Sanad reports no syntax error and no construct outside its subset. *(`SAN-HLR-120`)* |
| ☐ | **DG-V6-T01.07** | Step 7: the reopened Workspace shows the same model, read from the note — the note alone was enough to rebuild it. *(`SAN-HLR-120`)* |

**Step 3 is runnable now.** Rename-on-the-card was the one row the last build could not run; it
arrived with #909. Use the card's **Rename…** button.

Build sha `81bb6f4e` · date ________ · signed ________

### DG-V6-T02 — A port-to-port drag writes a connection, and an impossible action writes nothing

**Preconditions (verbatim):** the model left by `DG-V6-T01` — two parts, each with one port.

**What to do**

1. Take a copy of the note file as it stands. Note its byte size.
2. Drag from the first part's port to the second part's port. Read the note.
3. Look at the card: is the connection drawn?
4. Rename one element to a name SysML v2 cannot accept — a name with a space in it, or a bare reserved keyword such as `part`. Read what Sanad says, then read the note.
5. Compare the note file with the copy you took at step 1, byte for byte (`diff`).
6. Attempt a drag that cannot be a connection — from a port to a place where nothing is drawn, and from a port to a whole part rather than to one of its ports. Read what Sanad says each time.
7. Compare the note with the copy again.

**Pass criteria**

| ✔ | Row | Pass criterion (verbatim) |
|:-:|---|---|
| ☐ | **DG-V6-T02.01** | Step 2: the note gains a **connection statement naming both ports**, written as SysML v2 text. *(`SAN-HLR-124`)* |
| ☐ | **DG-V6-T02.02** | Step 3: the connection is drawn on the card, from the same text — there is no line in the picture that the note does not state. *(`SAN-HLR-124`)* |
| ☐ | **DG-V6-T02.03** | Step 4: Sanad **refuses** the rename and **names the reason** — it says what about the name it could not read. *(`SAN-HLR-121`)* |
| ☐ | **DG-V6-T02.04** | Step 5: the note is **identical** to the step-1 copy. Not "restored", not "reverted in the editor" — unchanged, with no intervening save. *(`SAN-HLR-121`)* |
| ☐ | **DG-V6-T02.05** | Step 6: each impossible drag is refused **with its own reason named**, and neither produces a line on the picture. *(`SAN-HLR-121`)* |
| ☐ | **DG-V6-T02.06** | Step 7: the note is still identical to the step-1 copy after both refusals. *(`SAN-HLR-121`)* |

If both parts named their port the same thing, Sanad will ask you for the **qualified** name the
card shows — `FlightComputer.Port_1` — rather than guess which end you meant. That is a refusal
naming its reason, so it passes `.05`; use the qualified name and carry on.

Build sha `81bb6f4e` · date ________ · signed ________

### DG-V6-T03 — The configured shape set, the profile marks and nothing more, and palette text is engineer-authored

**Preconditions (verbatim):** two repositories, or the same one twice — once with **no** software
profile configured, once with the recommended profile accepted (`DG-V4b-T01` step 2).

**What to do**

1. With no profile configured and no shape set configured, open the palette and count the tools. Read every label.
2. Look for any tool that draws a line, a box, an arrow or a text label that is not in the offered set — a freehand line, a note box, a connector with no ports.
3. Drop a **Part** and read the note.
4. Now configure the recommended software profile. Drop a **Part** again and read the note.
5. Compare the two notes from steps 3 and 4, line by line.
6. Delete the profile file, reload, and look at the picture and at what Sanad says.
7. For the element created in step 3, ask Sanad where it came from — open its change provenance (`Sanad: Last changed`) and any report or trace that names its origin.
8. Look for a source-document citation attached to that element anywhere: in the note, in a report, in a trace, in an export.

**Pass criteria**

| ✔ | Row | Pass criterion (verbatim) |
|:-:|---|---|
| ☐ | **DG-V6-T03.01** | Step 1: the palette offers **the configured set — seven by default**: requirement, part, port, state machine, action/flow, interface/connection, use case. *(`SAN-HLR-127`)* |
| ☐ | **DG-V6-T03.02** | Step 2: there is **no tool outside that set**, and nothing on the palette draws anything the note cannot hold. *(`SAN-HLR-127`)* |
| ☐ | **DG-V6-T03.03** | Step 4: with the profile configured, the new element carries **the profile's own annotation**. *(`SAN-HLR-128`)* |
| ☐ | **DG-V6-T03.04** | Step 5: the annotation is the **only** difference between the two notes — the construct written, the name and everything else are the same. *(`SAN-HLR-128`)* |
| ☐ | **DG-V6-T03.05** | Step 6: with the profile gone the note still parses and the plain picture draws; Sanad names the absent profile rather than substituting one. *(`SAN-HLR-128`)* |
| ☐ | **DG-V6-T03.06** | Step 7: the element is attributed to **you, the engineer who authored it** — origin `engineer`, evidenced by the authorship of the note. *(`SAN-HLR-129`)* |
| ☐ | **DG-V6-T03.07** | Step 8: **no source-document citation** is attached to it anywhere, and nothing presents it as derived from a document. *(`SAN-HLR-129`)* |

**Step 4 must use Part.** With the recommended profile the note gains `#Class` in front of the
part — that is the annotation `.03` and `.04` are about. If you try the same with **Port**, **State
machine**, **Use case** or **Requirement** you will see **no** annotation, and that is correct
behaviour for the shipped profile rather than a defect: it declares no word for those four. See the
note at the top of this section, and your open `profile words` question.

**Step 7's `Origin:` line is on the card**, under the picture, when the graph recorded an origin
for what the note declares.

Build sha `81bb6f4e` · date ________ · signed ________

### DG-V6-T04 — Everything the drag does, done from the keyboard alone

**Preconditions (verbatim):** a Workspace open on the canvas with a diagram note, as for
`DG-V6-T01`. **Put the mouse down and do not touch it for the whole of this procedure.**

**What to do**

1. Tab until a palette tool on the diagram card has focus. Look at what has focus, and at what the screen reader or the focus ring says it is.
2. Press Enter on the **Part** tool. Look at what appears and at what now has focus.
3. Leave the target as *top level* and press Enter. Read the note.
4. Tab to a palette tool again, press Enter on **Port**, choose the element you just created from the target list, and press Enter. Read the note.
5. Repeat steps 2–3 for a second part, then open the tool again and press **Escape** instead of Enter. Read the note.
6. Tab to **Rename…**, press Enter, choose the first element, type a new name, press Enter. Read the note.
7. Tab to **Connect…**, press Enter, choose the two ports, press Enter. Read the note.
8. Now do step 3's placement again with the mouse, on a fresh copy of the same starting note, and compare the two notes.

**Pass criteria**

| ✔ | Row | Pass criterion (verbatim) |
|:-:|---|---|
| ☐ | **DG-V6-T04.01** | Step 1: a palette tool takes keyboard focus, and it is announced by a name that says what it adds — not "button". *(`SAN-HLR-130`)* |
| ☐ | **DG-V6-T04.02** | Step 2: pressing Enter opens a target choice, and focus moves into it — the engineer is not left hunting for where the interaction went. *(`SAN-HLR-130`)* |
| ☐ | **DG-V6-T04.03** | Step 3: the note gains one element at the top level, named with a visible placeholder — the same result step 2 of `DG-V6-T01` produces with a drag. *(`SAN-HLR-130`, `SAN-HLR-122`, `SAN-HLR-125`)* |
| ☐ | **DG-V6-T04.04** | Step 4: the port is written inside the body of the element chosen from the list, exactly as a drop on that element writes it. *(`SAN-HLR-130`, `SAN-HLR-123`)* |
| ☐ | **DG-V6-T04.05** | Step 5: Escape closes the choice and **the note is unchanged** — a cancelled placement writes nothing, like any refused action. *(`SAN-HLR-130`, `SAN-HLR-121`)* |
| ☐ | **DG-V6-T04.06** | Steps 6 and 7: the rename and the connection are both reachable and both complete without the pointer. *(`SAN-HLR-130`, `SAN-HLR-124`, `SAN-HLR-126`)* |
| ☐ | **DG-V6-T04.07** | Step 8: the two notes are **identical** — the keyboard placement and the drag produced the same text, not two texts that mean the same thing. *(`SAN-HLR-130`)* |
| ☐ | **DG-V6-T04.08** | Throughout: after every one of those actions the picture draws and Sanad reports no syntax error. *(`SAN-HLR-120`)* |

**Space works wherever Enter does** on a palette tool — it is an ordinary button, so click, Enter
and Space are one path. The row asks for Enter; either is the same code.

Build sha `81bb6f4e` · date ________ · signed ________

---

## 4. Where to report

Reply in the coordinator session, or comment on the feature's issue: **model editor #958**,
**canvas #762**, **SysML #860**, **palette #907**. (Several are closed as merged design issues — a
comment on a closed issue still reaches me, so use whichever is easiest.)

An issue is filed per finding under the round's umbrella, so one line from you is enough: the row
id, what you saw, and the file if there is one.

---

## 5. Known findings already filed

| Issue | What | State in this build |
|---|---|---|
| **#1052** | No `ModelEdit` could address a view file, so *remove from the view* was unreachable — `ME-T03.01` | **Fixed in this build** (PR #1061). The `expose` is removed from the view file, every other file is byte-identical, and nothing else reaches the disk. |
| **#1053** | The impact preview named no view at all, and a layout entry outlived the element it annotated — `ME-T03.02`, `ME-T03.03` | **Fixed in this build** (PR #1061), except the wildcard half below. |
| **#1063** | `SAN-HLR-138 c2`: a view that exposes an **ancestor** with `**` shows an element without naming it, and is not in the impact preview | **Open — and it is a stage-1 question for you**, not a defect. Should a wildcard count as naming what it shows? Until you rule, `ME-T03.02` is signed for the named view only. |
| **#1064** | PR #1054's `K()`→`T()` flip needed three edits, not the one line its body predicted — measured by the reviewer | **Open.** A record about our own process; no product surface. |
| **#915** | A workspace file that will not parse as YAML was **silent** on the CLI, the runner and the gate | **Fixed in this build** (PR #1077): the refusal names the file, the line and the reason on every headless surface. The issue is still open on the board pending its close. |
| **#913** | Two diagrams in two workspace files can assert the same allocation and the graph keeps two byte-identical edges — so *"which diagram asserted this"* has no answer | **Still open.** Waiting on your word. Nothing is merged or lost. |
| **#914** | The duplicate-functionality report can name a requirement as duplicating **itself** when it carries two identical `allocates` edges | **Still open.** Waiting on your word. **Pre-existing** — reachable from Markdown alone long before this build. |
| **#902** | `SAN-HLR-107` says the Test coverage counts are *"over the derived set"*, and the view counts N+1 | **Still open — an owner decision.** Which is wrong, the sentence or the count? `SC-PLAN-T09` must not be hand-run until it is answered: the counts it tells you to expect are the ones under question. |
| **#930 · #935 · #898 · #899 · #900** | The five findings from your earlier passes on the diagram pictures and the canvas card | **All fixed before the 0.6.3 release** and still fixed here. |

---

## 6. What is on main with no hand-test rows

Nothing here has a row to tick. It is listed so you know the build carries it and are not surprised
by something you have no test for.

| What | What changed | Where you might notice |
|---|---|---|
| **The review capability — in progress** | `Sanad: Open Review` and the review commands (comment, reply, resolve, request changes, approve, compare revisions) are landing on `main`, with two AI-draft commands beside them — *Self-review My Change* and *First Pass Over This Review*. Its slices are still merging and it has **no acceptance tests yet**. | New **Sanad: …Review…** entries in the Command Palette. **Not ready to test** — open them if you are curious, but there is nothing to sign and findings against it are expected. |
| **#915's fix** | A workspace file that will not read is now named on the CLI, the runner and the gate. | A broken `*.workspace.yaml` produces a named refusal in a headless run instead of silence. |
| **The conformance job (E9)** | Our own CI reads every file we write with the OMG Pilot Implementation, 0 errors and 0 warnings. | Nothing in the product. `ME-T06` is where you read it. |
| **Stage-4 verification records** | Agent B's independent low-level verification of the model editor, the canvas and the reader, with the evidence in each feature's folder. | Repository only — no product surface. |

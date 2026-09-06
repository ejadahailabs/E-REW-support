# Test guide — Sanad test build `Sanad@19e79f78`

**This is a test build, not a release.** No version was bumped, no tag was cut, nothing
went to the Marketplace. 0.6.1 is still the current release.

| | |
|---|---|
| **Build** | `Sanad@19e79f78` — `ejadahailabs/Sanad` main, merge of PR #908 |
| **Date built** | 2026-09-06 |
| **Version stamp** | 0.6.2 — a build number carried over from the last tag, not a new version |
| **File** | `downloads/e-rew-test-build.vsix` |
| **Size** | 5 421 136 bytes |
| **SHA-256** | `5fdec272cb224ccea6994e285db2fb22410d4879c707e7aee07e1338acb47e9c` |
| **CI at that sha** | CI, acceptance-gate and board-sync all green |

**What is new since the last test build (`Sanad@2ea7f294`):** the **shape palette** on the
diagram card — PR 1 of 3. Drag a shape onto a card and Sanad writes SysML v2 into the note.

| Feature | Rows in this guide | Not runnable on this build |
|---|--:|--:|
| Voice-driven design canvas (DG-V1 … DG-V7) | 35 | 8 |
| SysML v2 reader, profile and diagram kinds | 30 | 1 (partly) |
| Shape palette — PREVIEW, PR 1 of 3 | 7 | 1 |
| **Total** | **72** | **9 fully · 1 partly** |

---

## 1. Install (5 minutes)

| Step | Do this |
|---|---|
| 1 | **Uninstall any older Sanad or E-REW build first.** Extensions view → search `Sanad`, and search `E-REW`. Remove both if present. 0.6.0 renamed the extension, so VS Code treats the old `E-REW` id as a *different* extension, not an older one — both activate on the same folder and the old one can win the window. |
| 2 | Download the build: `curl -LO https://raw.githubusercontent.com/ejadahailabs/E-REW-support/main/downloads/e-rew-test-build.vsix` |
| 3 | Check what you downloaded: `sha256sum e-rew-test-build.vsix` — it must read `5fdec272cb224ccea6994e285db2fb22410d4879c707e7aee07e1338acb47e9c` |
| 4 | Install: `code --install-extension e-rew-test-build.vsix` — or Extensions view → `...` menu → *Install from VSIX...* |
| 5 | **Quit VS Code completely** — `Ctrl+Q`, or Quit from the menu. A window reload is **not** enough: it does not swap the extension host, and the old build stays live. This is the most common reason a fresh install "does nothing". |
| 6 | Reopen VS Code on your test repository. |

**Confirming you are on this build:** Extensions view → Sanad → the version reads **0.6.2**.
There is no command in this build that prints the commit sha. The sha is in this file, and in
the downloads README row for the slot — so the version number plus the checksum in step 3 is
how you know which build you have.

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

## 3a. Voice-driven design canvas — DG-V1 … DG-V7

Source: `qual/capabilities/design-canvas/v0-v5-voice-canvas/tests.md` · issue **#762**

**Two fixes from your last pass are in this build:**

* **#899** — a diagram card no longer prints the note's raw SysML (`package … { }`) under the picture.
* **#900** — `Sanad: Diagram Kind…` is reachable from the card's own **Diagram kind…** button, and when the dictation pad has focus it says *"Open the note or file holding the diagram first."* instead of returning silently.

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

Build sha `19e79f78` · date ________ · signed ________

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

Build sha `19e79f78` · date ________ · signed ________

### DG-V2b-T01 — Sanad's own microphone records locally and transmits nothing

> ### NOT RUNNABLE ON THIS BUILD — all 8 rows
>
> **Why:** slice V2b is not built. You chose *"V0–V5 without V2b is the first release"* and
> *"V2b after V5"*, so Sanad's own microphone does not exist yet: there is no 🎤 button, no
> `config.yaml` key to turn one on, and no recogniser. The source states it plainly —
> *"Sanad writes no audio code, requests no microphone"*. Dictation in this build is VS Code's
> Speech extension typing into an ordinary editor.
>
> Rows carried here so the count is honest, not to be run:
> `DG-V2b-T01.01` `.02` `.03` `.04` `.05` `.06` `.07` `.08`.

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

Build sha `19e79f78` · date ________ · signed ________

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

Build sha `19e79f78` · date ________ · signed ________

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

Build sha `19e79f78` · date ________ · signed ________

---

## 3b. SysML v2 reader, profile and diagram kinds

Sources: `qual/capabilities/system-design/v4-sysml-v2/tests.md` (DG-V4, DG-V4c) and
`qual/capabilities/software-design/v4b-v7-profile-and-palette/tests.md` (DG-V4b) · issue **#860**

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

**`DG-V4-T01.09` — only its second half is runnable on this build.** There is no configuration
in this build that plugs in the official OMG validator; `SAN-HLR-098` keeps it as a future
option. Check *"with it unconfigured, nothing degrades"* and leave the first half unticked.

**Rows `.11`–`.14` ARE runnable** even though **#860** is still open as an issue: the widened
subset shipped in an earlier build (34 constructs — `in`/`out`/`inout` directions, interface
`end`, transitions, flows, successions, `assert not satisfy`). The issue is open for the
requirement wording, not for missing code.

Build sha `19e79f78` · date ________ · signed ________

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

Build sha `19e79f78` · date ________ · signed ________

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

Build sha `19e79f78` · date ________ · signed ________

---

## 3c. Shape palette — PREVIEW, PR 1 of 3 only

Source: `qual/capabilities/software-design/v4b-v7-profile-and-palette/tests.md` · issue **#907**

> **PR 1 only: drop, nesting, placeholder names, the configured shape set.**
> **Connect and rename (`DG-V6-T02`) and profile/origin (`DG-V6-T03`) arrive in the next build.**
> There is no `DG-V6-T04` in `tests.md` — the palette has three tests, `T01`, `T02` and `T03`.
> Keyboard access is part of `T02`'s round, not this one.

**How to reach it:** the palette is a **strip of shapes on a canvas card that already holds a
diagram**. So: open a workspace canvas, add a note whose text holds a SysML v2 block (the
DG-V7-T01 card), and the strip appears on that card. Drag a shape from the strip onto the
picture. A repository can narrow or extend the set with a `palette:` list in
`.ejadah/rew/config.yaml`; `palette: []` means *no palette* and draws no strip at all.

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
| **NOT RUNNABLE** | **DG-V6-T01.03** | Step 3: the note now carries **the name you typed**, in place of the placeholder, and you did not open the note to do it. *(`SAN-HLR-126`)* |
| ☐ | **DG-V6-T01.04** | Step 4: the port is written **inside the body of the element you dropped it on**, not beside it. *(`SAN-HLR-123`)* |
| ☐ | **DG-V6-T01.05** | Step 5: the second part is top level and its port is nested in it — the same shape dropped in two places produced two different models. *(`SAN-HLR-122`, `SAN-HLR-123`)* |
| ☐ | **DG-V6-T01.06** | Step 6: after **every one** of those actions the picture draws and Sanad reports no syntax error and no construct outside its subset. *(`SAN-HLR-120`)* |
| ☐ | **DG-V6-T01.07** | Step 7: the reopened Workspace shows the same model, read from the note — the note alone was enough to rebuild it. *(`SAN-HLR-120`)* |

**`DG-V6-T01.03` is not runnable on this build.** Rename-on-the-card is `SAN-HLR-126`, and PR 1
shipped `SAN-HLR-120`…`123`, `125` and `127`. Skip **step 3** and run steps 2, 4, 5, 6, 7 —
the other six rows do not depend on it. Renaming arrives with `DG-V6-T02`'s PR.

Build sha `19e79f78` · date ________ · signed ________

### Not runnable in this build — the rest of the palette

| Test | Why | Arrives |
|---|---|---|
| `DG-V6-T02` (6 rows) — port-to-port connect, refusal that writes nothing | `SAN-HLR-121` / `SAN-HLR-124` connect and rename are PR 2 | next build |
| `DG-V6-T03` (7 rows) — the configured shape set, profile marks, engineer origin | needs the profile annotation (`SAN-HLR-128`) and origin (`SAN-HLR-129`) from PR 3. The shape-set half (`SAN-HLR-127`) **is** in this build, so if you want a free look, count the strip: seven tools by default — requirement, part, port, state machine, action/flow, interface/connection, use case | next build |

---

## 4. Where to report

Reply in the coordinator session, or comment on the feature's issue: **canvas #762**,
**SysML #860**, **palette #907**. (#762 and #907 are closed as merged design issues — a comment
on a closed issue still reaches me, so use whichever is easiest.)

---

## 5. Known findings already filed

| Issue | What | State in this build |
|---|---|---|
| **#898** | A `requirement def` written inside a diagram is refused as *"no requirement declares"* — the message wording | **Open, fix queued.** Waiting on your stage-1 decision: should SysML declare requirements in the graph, or Markdown only? |
| **#899** | The diagram card printed the note's raw SysML source under the picture | **Fixed in this build** |
| **#900** | `Sanad: Diagram Kind…` did nothing when the dictation pad had focus, and was unreachable from the card | **Fixed in this build** |

Also open, and deliberately **not** in this guide: **`SC-PLAN-T09`** (the Test coverage view's
derived items) is blocked on **#902** and must not be run by a person until **#901** merges —
the counts it tells you to expect are the ones under question.

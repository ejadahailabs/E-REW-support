# Test guide — Sanad test build `Sanad@ebc57d00`

**This is a test build, not a release.** No version was bumped, no tag was cut, nothing
went to the Marketplace. 0.6.1 is still the current release.

| | |
|---|---|
| **Build** | `Sanad@ebc57d00` — `ejadahailabs/Sanad` main, merge of PR #940 |
| **Date built** | 2026-09-07 |
| **Version stamp** | 0.6.2 — a build number carried over from the last tag, not a new version |
| **File** | `downloads/e-rew-test-build.vsix` |
| **Size** | 5 437 845 bytes |
| **SHA-256** | `d5c7a7fc13b8b1139e30fb7f27a9d59f932ed1bfe6223bd409c5996a77655bdb` |
| **CI at that sha** | CI, acceptance-gate and board-sync all green |

**What is new since the last test build (`Sanad@11980864`):** two fixes to **one rule**, and
nothing else you can click. The **`— diagram-local` label now appears only for a `requirement def`
whose id your requirements corpus does not declare** — and it appears on all three pictures that
can draw one: the panel's **default** picture, a **requirement diagram you ask for by name**, and
the **class diagram of a profile-marked model**. The previous build got this wrong in two opposite
directions. Ask for the requirement kind and it labelled **every** `requirement def`, including ids
the corpus does declare — so one screen could call an id diagram-local while drawing a resolved
allocation out of that same box; the default picture had the same gap #930 · #934. A model your
software profile **marks** drew requirement boxes with **no label at all**, so a genuinely
diagram-local id passed unremarked there #935 · #936. Everything else on `main` since that build is
tooling, ledger and test work with no product surface.

**Your signatures from the last pass still count.** Every test row below is byte-identical to the
`Sanad@11980864` guide — no feature's `tests.md` changed on `main` between the two builds — so a
row you already signed does not need re-running unless it touched a requirement picture.

| Feature | Rows in this guide | Not runnable on this build |
|---|--:|--:|
| Voice-driven design canvas (DG-V1 … DG-V7) | 35 | 8 |
| SysML v2 reader, profile and diagram kinds | 30 | 1 (partly) |
| Shape palette — **complete** (DG-V6-T01 … T04) | 28 | 0 |
| **Total** | **93** | **8 fully · 1 partly** |

---

## 1. Install (5 minutes)

| Step | Do this |
|---|---|
| 1 | **Uninstall any older Sanad or E-REW build first.** Extensions view → search `Sanad`, and search `E-REW`. Remove both if present. 0.6.0 renamed the extension, so VS Code treats the old `E-REW` id as a *different* extension, not an older one — both activate on the same folder and the old one can win the window. |
| 2 | Download the build: `curl -LO https://raw.githubusercontent.com/ejadahailabs/E-REW-support/main/downloads/e-rew-test-build.vsix` |
| 3 | Check what you downloaded: `sha256sum e-rew-test-build.vsix` — it must read `d5c7a7fc13b8b1139e30fb7f27a9d59f932ed1bfe6223bd409c5996a77655bdb` |
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

**The two fixes from your last pass are still in — they arrived in the `Sanad@19e79f78` build and
nothing since has touched them:**

* **#899** — a diagram card no longer prints the note's raw SysML (`package … { }`) under the picture.
* **#900** — `Sanad: Diagram Kind…` is reachable from the card's own **Diagram kind…** button, and when the dictation pad has focus it says *"Open the note or file holding the diagram first."* instead of returning silently.

**New in this build on the canvas:** every card that holds a diagram now carries the **full shape
palette** — section 3c. So the DG-V7-T01 card below is also the card you test the palette on.

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

Build sha `ebc57d00` · date ________ · signed ________

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

Build sha `ebc57d00` · date ________ · signed ________

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

Build sha `ebc57d00` · date ________ · signed ________

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

Build sha `ebc57d00` · date ________ · signed ________

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

Build sha `ebc57d00` · date ________ · signed ________

---

## 3b. SysML v2 reader, profile and diagram kinds

Sources: `qual/capabilities/system-design/v4-sysml-v2/tests.md` (DG-V4, DG-V4c) and
`qual/capabilities/software-design/v4b-v7-profile-and-palette/tests.md` (DG-V4b) · issue **#860**

**Fixed in this build: #898.** A `requirement def` you write *inside* a diagram used to be refused
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

Build sha `ebc57d00` · date ________ · signed ________

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

Build sha `ebc57d00` · date ________ · signed ________

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

Build sha `ebc57d00` · date ________ · signed ________

---

## 3c. Shape palette — COMPLETE in this build

Source: `qual/capabilities/software-design/v4b-v7-profile-and-palette/tests.md` · issue **#907**

> **Feature complete in this build; rows `DG-V6-T01`…`T04` ready to run and sign.**
> The three parts have all landed — the drop (#908), connect · rename · keyboard (#909), and the
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

Build sha `ebc57d00` · date ________ · signed ________

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

Build sha `ebc57d00` · date ________ · signed ________

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

Build sha `ebc57d00` · date ________ · signed ________

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

Build sha `ebc57d00` · date ________ · signed ________

---

## 4. Where to report

Reply in the coordinator session, or comment on the feature's issue: **canvas #762**,
**SysML #860**, **palette #907**. (#762 and #907 are closed as merged design issues — a comment
on a closed issue still reaches me, so use whichever is easiest.)

---

## 5. Known findings already filed

| Issue | What | State in this build |
|---|---|---|
| **#930** | A requirement diagram **asked for by name** labelled a corpus requirement `— diagram-local` while drawing a resolved allocation out of the same box — two opposite statements about one id, on one screen. The panel's **default** picture had the same gap | **Fixed in this build.** The label is conditional on the corpus on every path that draws it: the requested kind, the default picture and the card all receive the set of ids your requirements corpus declares. |
| **#935** | A model your **software profile marks** drew requirement boxes as plain class boxes with **no label at all**, so a genuinely diagram-local id passed unremarked on that picture | **Fixed in this build.** The class-diagram path applies the same one rule, written in one place in the source, with the label in the class label so the class name stays the identity every stereotype and relation line uses. |
| **#898** | A `requirement def` written inside a diagram was refused as *"no requirement declares"* — the message wording | **Fixed since `Sanad@11980864`** — still fixed here. The refusal names the id as declared locally in the diagram and outside the requirements corpus, and the box on the picture reads `NAME — diagram-local`. Markdown stays the one place a requirement is declared, which was your decision. |
| **#899** | The diagram card printed the note's raw SysML source under the picture | **Fixed since `Sanad@19e79f78`** — still fixed here |
| **#900** | `Sanad: Diagram Kind…` did nothing when the dictation pad had focus, and was unreachable from the card | **Fixed since `Sanad@19e79f78`** — still fixed here |
| **#913** | Two diagrams in two different workspace files can assert the same allocation, and the graph keeps two edges that are byte-identical — so *"which diagram asserted this allocation"* has no answer | **Still open.** Filed by agent B from the G2 verification, waiting on your word. Nothing is merged or lost; the question the allocation report will ask is the one that cannot yet be answered. |
| **#914** | The duplicate-functionality report can name a requirement as duplicating **itself**, when that requirement carries two identical `allocates` edges | **Still open.** Filed by agent B, waiting on your word. **Pre-existing** — reachable from Markdown alone (`allocated_to: ["COMP-A", "COMP-A"]`) long before this build; since #910 two diagrams asserting the same allocation reach it too. Not caused by anything in this build. |
| **#915** | A workspace file that will not parse as YAML is **silent** on the CLI, the runner and the gate — every diagram in it stops feeding the analysis and nothing says so | **Still open.** Filed by agent B, waiting on your word. The editor's Workspaces view does name it; the three headless surfaces do not. |

Also open, and deliberately **not** in this guide: **`SC-PLAN-T09`** (the Test coverage view's
derived items) is blocked on **#902** and must not be run by a person until **#901** merges —
the counts it tells you to expect are the ones under question.

---

## 6. What is on main with no hand-test rows

Verified by agent B at stage 4. There is nothing to click here — it is listed so you know the build
carries it and are not surprised by a behaviour change you have no row for.

| What | What changed | Where you might notice |
|---|---|---|
| **G2 — SysML facts are now repository-wide** | What your workspace diagrams declare used to reach only the editor's own graph. It now reaches the whole repository: the allocation view, the CLI, the baselines, the impact analysis and the gate all see diagrams. | A requirement your **diagram** allocates is no longer reported as unallocated by the command-line run. |
| **The `covers` producer** | Coverage facts are produced from declared facts rather than re-derived per surface. | Nothing on screen changes. |
| **Stage-4 suites (#917)** | Agent B's low-level verification records and evidence for G2. | Repository only — no product surface. |

The three open issues in section 5 (**#913**, **#914**, **#915**) all came out of that G2
verification. That is the work doing what it is meant to do: they are findings against the build
you are holding, filed before you met them.

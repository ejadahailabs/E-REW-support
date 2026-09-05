# Installing Sanad

**Sanad 0.6.1 is on the Visual Studio Marketplace.** That is the normal way to install it,
and it updates itself:

**[marketplace.visualstudio.com/items?itemName=EjadahAILABS.sanad](https://marketplace.visualstudio.com/items?itemName=EjadahAILABS.sanad)**

This page is the **manual fallback**, for anyone whose organisation cannot reach the
Marketplace. **`sanad-0.6.1.vsix` is the current build — install that one.** It is not the
newest file in this folder: `e-rew-test-build.vsix` sits beside it as the **rolling test
build**, described in the next section, and it is not recommended for general use. Install
from any machine — you do not need this repository cloned, just the file.

**New to Sanad?** The one-time setup walkthrough is
**[GETTING-STARTED.md](../GETTING-STARTED.md)** — install, open your repository, run setup,
run the first analysis.

**Technical support:** support@ejadahailabs.com

## Test build (pre-release slot — owner testing only)

**`e-rew-test-build.vsix` is the test build.** It is the rolling **pre-release candidate**
built from `main`: it is overwritten every round, the file name and URL never change, and it
is not a release. It currently holds a build of `E-REW@959b5b9`. Its version stamp reads
0.6.2 — a build number carried over from the last tag, not a new version. **0.6.1 remains
the current, recommended release**; no new version has been released.

```
curl -LO https://github.com/ejadahailabs/E-REW-support/raw/main/downloads/e-rew-test-build.vsix
code --install-extension e-rew-test-build.vsix   # then quit VS Code fully (Ctrl+Q)
```

`sanad-0.6.2.vsix` was the previous test build (tag `v0.6.2`). The slot above is a later
build of the same line and supersedes it; the file stays in this folder as a record.

**The demo-output problem reported against 0.6.2 has been settled: it was not a
regression.** What that machine ran was an old 0.5.x build answering under 0.6's name — VS
Code treats the pre-0.6.0 extension id as a separate extension rather than an older one, so
both activated on the same folder and the older one won the window. The next paragraph is
the fix for it.

**Before you install any 0.6.x build: if this machine has ever had E-REW 0.5.x on it,
uninstall that first.** 0.6.0 renamed the extension, and VS Code treats the old id as a
separate extension rather than an older one — so both activate on the same folder, collide
over the same commands, and whichever loads first wins. When the old one wins you get
0.5.x's behaviour under 0.6's name, including an analysis that re-runs on every reload.
0.6.2 detects the retired build on startup and offers to remove it; 0.6.1 does not.

| Built from | Date | SHA-256 |
|---|---|---|
| test-build slot `E-REW@959b5b9` — **the newest build in this folder, ahead of the `E-REW@c64b102` slot build.** Its version stamp reads 0.6.2 because it is a build of `main` taken after that tag; no new version has been released and no tag was cut for it. Over the `E-REW@c64b102` slot build it adds, all Sanad Workspaces work (#775 · #776, PR #777): a card on the canvas gains a **Float** button that opens that card as its own compact floating window; **the workspace file now remembers your session** — the files you had open and the cards you had floated come back the next time you open that workspace (window positions are not saved; VS Code gives an extension no way to read them); **Close Workspace** and **Switch Workspace** reach the command palette; and **a workspace now belongs to the branch it was made on** — opening one puts you on its branch (it saves, closes, checks out, then opens), the old *Open Anyway* escape is gone, and a new **Copy Workspace to Branch…** command is there for when you genuinely want the same workspace on another branch. Behind them, Workspaces is now a module of its own under `src/workspaces/`, behind one entry point and one enforced boundary, so it could later ship separately (ADR-0109). | 2026-09-05 | `9947633f85333903d3629f836d7effb3a911df7ec074b26787bf6706c09ee86e` |
| **RELEASE `sanad-0.6.1.vsix`** (from tag v0.6.1 @ 916a133 — the current release) | 2026-09-01 | `ac96d2ebbf5840305f78cc3e39a87e5251502fcec1f79ade88ad09f4780e13f6` |

Superseded builds, kept for the record — **do not install these**:

| Superseded | Date | SHA-256 |
|---|---|---|
| test-build slot `E-REW@c64b102` — held the rolling slot until the `E-REW@959b5b9` refresh above overwrote it. Was ahead of the `E-REW@fc6acb5` slot build. Over the `E-REW@fc6acb5` slot build it added, all from test round #765: a **pinned card now opens as a compact floating window** of its own, and the cards you pin are saved in the Workspace and come back when you open it — the `rew.card.pin` setting chooses `window` or `beside` #766/#767; **every HTML and Markdown export now opens with an index of its sections**, so a long report can be navigated from the top #768/#772; **"last changed by" now comes from the git history**, together with the commit it changed in — the short hash on the card, the full 40-character hash in the document export #769/#771; and the requirement form gains a **Traceability block beside Quality** — one collapsible lane per link role, with Implements shown only where your template declares it, both boxes collapsible and scrolling, and every trace item hovering to its own card (Implementation, Verification); the link fields are no longer repeated in the requirement section, and hovering a requirement's own id no longer pops a card of itself #770/#774. | 2026-09-05 | `b986abc13d7084bd672a955ef6b66908fcfb2bd0acaeffa23b2881d2249f749c` |
| test-build slot `E-REW@fc6acb5` — held the rolling slot until the `E-REW@c64b102` refresh above overwrote it. Was ahead of the 0.6.2 test build. Over the 0.6.2 test build it adds: **Sanad Workspaces** — a `SANAD WORKSPACES` view where you create a workspace, add requirements to it straight from the tree, and pin the one you are working in; each workspace carries a canvas that reopens exactly as you left it; and the assistant answers grounded on the workspace you have open rather than on the whole repository #754/#757/#758/#761. Plus the export fix that puts every export through **one renderer**, so a report says the same thing whichever format you asked for #751. | 2026-09-04 | `d4d9339c4c5530b9a1aade8a935c3457796546f2bef7cdce58e85d042847a418` |
| **`sanad-0.6.2.vsix`** (from tag v0.6.2 @ `ab6a199`) — **superseded by the `E-REW@959b5b9` slot build above**, which is a later build of `main` carrying everything below plus Sanad Workspaces. The file stays in this folder as a record; 0.6.1 is still the current release. Over 0.6.1 it adds: a retired 0.5.x install is detected on startup and offered for removal, instead of silently winning the window and running 0.5.x's analysis under 0.6's name #736/#739; a **Read ledger** — one card, one run-log block and `--ledger-json`, collecting everything a run could not read and grouping it by the fix that closes it #690; a code index Sanad refused now says so as a finding and a ledger row rather than reading as clean #737/#740; assistant **tasks** you reach by typing `/` — `/explain`, `/improve`, `/derive-conditions`, `/draft` — with the proposal card stating its own limits and cost ceiling before you apply anything; **Sanad: Impact of a Requirement** and **Sanad: Connect AI Provider** reach the palette; provenance, a configuration hash and a published schema tag on every export; requirement IDs are cards and links in C/C++ comments, pytest docstrings, `.trace` tables and verification documents, not only in Markdown; two new checks (`derived-without-justification`, `missing-required-role`); and a long list of silences given names — an unparseable `config.yaml`, a semicolon-separated CSV, a duplicated spreadsheet heading, a test verdict landing on nothing, a misspelled rule id, a results folder of unreadable reports, a repository where nothing could be checked no longer reporting 100 % clean. **Expires 2026-10-18.** | 2026-09-03 | `6dd90605fd17631dee67d6eafc619475160b0aa2aa9f83efd9d165f627af5fc9` |
| test-build slot `E-REW@e147cb8` — held the rolling slot until the `E-REW@fc6acb5` refresh above overwrote it. Was ahead of the 0.6.1 release. Over 0.6.1 it adds: the three explicit `verifies` forms now bind under the marker lane, so a test that names a requirement on one line is linked instead of ignored #650/#652; the EARS matcher reads a wider set of openers and trailing conditions, so realistic requirements are recognised #651/#653; a shape Sanad does not recognise is now reported as **unclassified** rather than guessed at, every finding message is one plain sentence with the detail underneath it, possible-exception lines are called out, and each report stamps the commit and date it was produced from #654/#655; the **Verification** and **Software** lenses the capability switcher opens #642; the licence text and the user guide now name Sanad and only commands that exist #648/#649; plus the engine and scale work behind them — a 10× repository tier, the dictionary ceiling profiled so a large one no longer stalls the editor, and the audit report and verification engine sharing one source of possible cases | 2026-09-02 | `c99a36ab7cde5966a4ad6e913ba3ab9781d8047019196355ca289be59eb01b61` |
| test-build slot from the **v0.6.1 release build itself** (tag v0.6.1 @ 916a133) — held the slot until the refresh above; byte-identical to `sanad-0.6.1.vsix`, which is still current as the release | 2026-09-01 | `ac96d2ebbf5840305f78cc3e39a87e5251502fcec1f79ade88ad09f4780e13f6` |
| **`sanad-0.6.0.vsix` re-cut (tag v0.6.0 @ 569546b) — WITHDRAWN, superseded by 0.6.1.** Its Implements and Verifies card links displayed mangled and did nothing when clicked (#620). The file stays in this folder as a record; install 0.6.1 instead. | 2026-08-31 | `1ffafc11431cb3ca64cd8f92492a4aaf614f3791862d34486422f5ac24721ea9` |
| test-build slot `E-REW@90a5f50` (RC6 preview, cut before that branch's CI finished; its content shipped in the 0.6.1 release above) | 2026-09-02 | `47f3b80aff5008480962e6b951676d05dd1710c516274ef920a245e29485caf7` |
| test-build slot `E-REW@9702f7b` (RC5b, development-complete candidate; everything in the previous slot, plus: the **Verifies** lane on a requirement card now fills from four places instead of one — a `verifies` marker inside a test, ids written in the test's own content under a prefix you declare, a verification data dictionary (requirement + verification id + unit under test, mapped through your own `fields:`), and the test **file name** (`test_REQ-743.py`, and bare numbers that inherit the prefix in front of them) — with the marker still winning where one exists, and an id in a name that your requirements do not declare reported as a finding instead of quietly linked #605; **every function inside the trace scope you declared that no requirement traces to is now listed by name**, with a per-file total on top of it, and the list is capped and says so rather than pretending to be complete #613; and, new policy: **each beta release is valid for 45 days from its release date** — this one is dated 2026-09-01, so it stops on **2026-10-16**, with a non-blocking notice on each of the last seven days naming that date; nothing you have written is affected, an expired release stops running rather than changing anything, and the update link that used to 404 now resolves #618) | 2026-09-01 | `de916943c43d42824fb176394154a40bab8c091e0996815281ec58185f3d0ab5` |
| test-build slot `E-REW@ba5db12` (RC5a — everything in the previous slot, plus: clicking a code link on a requirement card now opens the **function's definition**, and the Implements/Verifies links actually go somewhere instead of failing #612; impact analysis now reads the symbol index that is already loaded, so it tells C++ overloads apart when they differ only by a `const` qualifier #609; a requirement reference that points at nothing is reported as a finding and gets its own section in the report #616; on a repository that uses `.trace`, code that no requirement traces to is now reported as untraced #615; and the on-disk term index is encrypted at rest, ADR-0105 #608. Verifies-lanes and per-function refinement follow in RC5b) | 2026-09-01 | `ef8b76819e701482f2bc993c48ab3a4a4329b0086097d7e017729de966d03ac6` |
| `sanad-0.6.0.vsix` first cut (tag v0.6.0 @ 8bbb0b0, before the fix batch) — replaced the same day by the re-cut above, and both superseded by 0.6.1 | 2026-08-31 | `1a2df3c8291b420a17b167de1c9cbddbc91dcce17c928882f86803653c307d24` |
| test-build slot `E-REW@c840ba6` (RC4 — data-dictionary values no longer flagged as vague or unknown #599; large-dictionary startup stall fixed #601) | 2026-09-01 | `a4f81b4b91d54d8a9257d4dbae6af1d83500dc33ed43e67cc86a3e5413d533e0` |
| test-build slot `E-REW@f5a7061` (structural YAML data-dictionary loading: #597 · #598) | 2026-09-01 | `43bd2b59ee7c3f18ef0cb8d9dfe41244915775a4f3787e4492af093d505f57b0` |
| test-build slot `E-REW@77b1bdb` (whole-literal-first marked spans: #594 · #595) | 2026-08-31 | `44160ff8132212ec29bc01c237aa1fcdc3cb473d20f472cfd0be5f1496ca2059` |
| test-build slot `E-REW@c9ac327` (separator spacing + pytest underlines: #591 · #590) | 2026-08-31 | `638f371da61fffe9445e02897c53d1872533cac1d3736b11f683bae8f3c7dc47` |
| test-build slot `E-REW@bab424e` | 2026-08-31 | `197f28d003460dad9e40f4b1a8f73ab0a8c867ac10420625de6e0927884d52e0` |
| test-build slot `E-REW@12e44f7` | 2026-08-31 | `92fcf70bb8544c3f63afb7a652b71528451b19804dcd493712452bce0dbe89e6` |
| test-build slot `E-REW@bc9dc77` | 2026-08-31 | `eb2dc372f277a860022381e903b53119fa4824601e955f0eb239e040bb9f88ee` |
| test-build slot `E-REW@9556dda` | 2026-08-31 | `b8a96e3c1bcb5dcde399c1a1485c76548e7c284308ad6f6bb8f23ef4bbd34387` |

## Install

**From the Marketplace** — the normal route, and it keeps itself updated:

```
code --install-extension EjadahAILABS.sanad
```

…or search for **Sanad** in the Extensions view.

**From a `.vsix`** — download `sanad-0.6.1.vsix` from this folder, then either:

```
code --install-extension sanad-0.6.1.vsix
```

…or Extensions view → `...` menu → *Install from VSIX...*. A sideloaded build does not
auto-update: come back here when it expires.

**Then quit VS Code completely** (`Ctrl+Q`, or Quit from the menu). A window
reload is not enough: it does not swap the extension host, and the old build
stays live. This is the single most common reason a fresh install "does nothing".

Requires **VS Code 1.90 or newer**.

## Verify what you installed

```
sha256sum sanad-0.6.1.vsix
```

| Build | SHA-256 |
|---|---|
| `sanad-0.6.2.vsix` (previous test build — superseded by the slot build) | `6dd90605fd17631dee67d6eafc619475160b0aa2aa9f83efd9d165f627af5fc9` |
| `sanad-0.6.1.vsix` **(current)** | `ac96d2ebbf5840305f78cc3e39a87e5251502fcec1f79ade88ad09f4780e13f6` |
| `sanad-0.6.0.vsix` (withdrawn — see above) | `1ffafc11431cb3ca64cd8f92492a4aaf614f3791862d34486422f5ac24721ea9` |
| `e-rew-0.5.3.vsix` | `b0c6c7cc3035890b36898d77dc14a31c1b57fb0acabe349ac4b7bf4c1c65f3ba` |
| `e-rew-0.5.2.vsix` | `b68f5950cad05c0fb294a51f8666d98228e9e20f564ec1344ead9475670fd239` |
| `e-rew-0.5.1.vsix` | `7ce130fd64669da75b041313d265563f7d14f8380382104e37baa452a32b98f2` |
| `e-rew-0.5.0.vsix` | `1680ba5c29a1715c933e7360a57664cc5c248e731c7c2731db3d4ebee4afc784` |

## Known gaps in 0.6.1

Hit one of these, or something not listed here? **Technical support:** support@ejadahailabs.com

**This beta release is free to use until 2026-10-16.** Sanad is provided free of
charge during its beta period, which covers every release numbered below 1.0. Each
beta release is valid for 45 days from its release date (2026-09-01 for this one);
when a release expires, install the current release to continue. The count starts at
the release date, not your install date, so everyone on this release stops on the
same day, and you get a non-blocking notice on each of the last seven. No licence key
is needed — Sanad tells you when one becomes required, and licensed use begins with
version 1.0. Nothing you have written is affected: your requirements are your own
files in your own repository, and an expired release stops running rather than
changing anything.

**Software and Verification are work-in-progress previews.** They run in full and
nothing is switched off, but they have not finished testing, and they say so in the
capability switcher, on the lane and in setup. Requirements is the mature capability.

Unfinished, completing in a later release:

| Area | State |
|---|---|
| Setup step **8 · Standards mapping** | a placeholder that records nothing yet, and says so on screen. Until it lands, every standard is reported as *declared, not checked* |
| Setup step **9 · Views & reports** | a placeholder that records nothing yet, and says so on screen. The built-in reports run as they do today |
| Multi-repository programmes | you can pick a split topology in setup and the choice is kept, but Sanad works on **this one repository** for now. It says so at the moment you pick it |
| Code indexing | **C and C++ only.** Python files are read for verification cases and hover cards; they are not indexed as symbols. No other language is indexed |
| Finding marks inside table cells | the note list under the field carries them; per-character underlines are not drawn inside table cells |
| Reports | produced by the `erew` command line (`erew --report <name>`), not by an editor command. Dashboards and the lens views are the in-editor surfaces |
| Review and Systems capabilities | greyed out in setup — you cannot tick them yet. The Safety, Security and Interface checks still run as they always have, whenever the roles they need are declared |

## Opening your requirements for the first time

**The full walkthrough is [GETTING-STARTED.md](../GETTING-STARTED.md).** The short
version:

Sanad activates on a folder that contains `.ejadah/rew/`. A folder that has
never been set up will not light up on its own — that is expected, not a broken
install.

1. Open the folder holding your requirements.
2. Command Palette → **Sanad: Set Up Sanad in This Folder** (or click the button in the
   empty Requirements view).
3. Work down the ten steps of the form. Only **1 · Product** and **2 · Artifact types**
   need your attention the first time; the rest are optional, and each one you leave
   blank turns off the analysis that needed it, with that stated as the reason.
4. Press **Setup complete**. Nothing is written to disk before that.

Re-run the command any time — the form re-opens pre-filled with what you already
answered.

Your requirement files are **never rewritten** by setup. Sanad writes only under
`.ejadah/rew/` plus `sanad-product.yaml`, and it never overwrites a file that already
exists. Commit `.ejadah/` and the setup is shared with your whole team.

## Both file shapes are supported

You do not have to convert anything.

| Your requirements look like | Supported |
|---|---|
| YAML frontmatter (`---` block with `id`, `status`, …) | yes |
| Plain Markdown — headings and prose, no YAML anywhere | yes |
| Some of each, in the same repository | yes — it is declared per requirement type |

For plain Markdown the id is the **filename** and the type is the **folder**, so
neither has to be written into the file. If your sections use `###` rather than
`##`, setup detects that from your own files and tells you what it found.

## If the requirements tree is empty

Almost always one of these, in this order:

1. VS Code was reloaded instead of fully quit after installing.
2. The folder opened is not the one containing `.ejadah/rew/`.
3. Setup has not been run in that folder yet.
4. The template's `folder:` does not match where the files actually are.

The Problems panel names anything that failed to load and why — a file that
could not be read is reported there, never silently dropped.

## Reporting a problem

Open an issue in this repository. Useful to include: what you expected, what
happened, and the message from the Problems panel if there is one. Please do not
paste requirement text you are not free to share.

## Licence

Proprietary — see `LICENSE.txt` inside the extension, or [LICENSE](../LICENSE) here.

Sanad is provided free of charge during its beta period, which covers every release
numbered below 1.0. Individuals and organisations may install, use and evaluate beta
releases without a licence key. Each beta release is valid for 45 days from its
release date; when a release expires, install the current release to continue. The
application will indicate when a licence key becomes required. Licensed use begins
with version 1.0.

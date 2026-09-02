# Installing Sanad

**Sanad 0.6.1 is on the Visual Studio Marketplace.** That is the normal way to install it,
and it updates itself:

**[marketplace.visualstudio.com/items?itemName=EjadahAILABS.sanad](https://marketplace.visualstudio.com/items?itemName=EjadahAILABS.sanad)**

This page is the **manual fallback**, for anyone whose organisation cannot reach the
Marketplace. The newest `.vsix` in this folder is the current build; install it from any
machine — you do not need this repository cloned, just the file.

**New to Sanad?** The one-time setup walkthrough is
**[GETTING-STARTED.md](../GETTING-STARTED.md)** — install, open your repository, run setup,
run the first analysis.

**Technical support:** support@ejadahailabs.com

## Test build (pre-release slot — owner testing only)

`e-rew-test-build.vsix` is the rolling **pre-release candidate** built from `main`. It is
overwritten every bug round; the file name and URL never change. Not a release.

```
curl -LO https://github.com/ejadahailabs/E-REW-support/raw/main/downloads/e-rew-test-build.vsix
code --install-extension e-rew-test-build.vsix   # then quit VS Code fully (Ctrl+Q)
```

| Built from | Date | SHA-256 |
|---|---|---|
| **RELEASE `sanad-0.6.1.vsix`** (from tag v0.6.1 @ 916a133 — the current release) | 2026-09-01 | `ac96d2ebbf5840305f78cc3e39a87e5251502fcec1f79ade88ad09f4780e13f6` |
| test-build slot `E-REW@e147cb8` — **ahead of the 0.6.1 release above.** Over 0.6.1 it adds: the three explicit `verifies` forms now bind under the marker lane, so a test that names a requirement on one line is linked instead of ignored #650/#652; the EARS matcher reads a wider set of openers and trailing conditions, so realistic requirements are recognised #651/#653; a shape Sanad does not recognise is now reported as **unclassified** rather than guessed at, every finding message is one plain sentence with the detail underneath it, possible-exception lines are called out, and each report stamps the commit and date it was produced from #654/#655; the **Verification** and **Software** lenses the capability switcher opens #642; the licence text and the user guide now name Sanad and only commands that exist #648/#649; plus the engine and scale work behind them — a 10× repository tier, the dictionary ceiling profiled so a large one no longer stalls the editor, and the audit report and verification engine sharing one source of possible cases | 2026-09-02 | `c99a36ab7cde5966a4ad6e913ba3ab9781d8047019196355ca289be59eb01b61` |

Superseded builds, kept for the record — **do not install these**:

| Superseded | Date | SHA-256 |
|---|---|---|
| test-build slot from the **v0.6.1 release build itself** (tag v0.6.1 @ 916a133) — held the slot until the refresh above; byte-identical to `sanad-0.6.1.vsix`, which is still current as the release | 2026-09-01 | `ac96d2ebbf5840305f78cc3e39a87e5251502fcec1f79ade88ad09f4780e13f6` |
| **`sanad-0.6.0.vsix` re-cut (tag v0.6.0 @ 569546b) — WITHDRAWN, superseded by 0.6.1.** Its Implements and Verifies card links displayed mangled and did nothing when clicked (#620). The file stays in this folder as a record; install 0.6.1 instead. | 2026-08-31 | `1ffafc11431cb3ca64cd8f92492a4aaf614f3791862d34486422f5ac24721ea9` |
| test-build slot `E-REW@90a5f50` (RC6 preview, cut before that branch's CI finished; its content shipped in the 0.6.1 release above) | 2026-09-02 | `47f3b80aff5008480962e6b951676d05dd1710c516274ef920a245e29485caf7` |
| test-build slot `E-REW@9702f7b` (RC5b, development-complete candidate; everything in the previous slot, plus: the **Verifies** lane on a requirement card now fills from four places instead of one — a `verifies` marker inside a test, ids written in the test's own content under a prefix you declare, a verification data dictionary (requirement + verification id + unit under test, mapped through your own `fields:`), and the test **file name** (`test_REQ-743.py`, and bare numbers that inherit the prefix in front of them) — with the marker still winning where one exists, and an id in a name that your requirements do not declare reported as a finding instead of quietly linked #605; **every function inside the trace scope you declared that no requirement traces to is now listed by name**, with a per-file total on top of it, and the list is capped and says so rather than pretending to be complete #613; and, new policy: **while Sanad is a prototype each build runs for 45 days from its release date** — this one is dated 2026-09-01, so it stops on **2026-10-16**, with a non-blocking notice on each of the last seven days naming that date; nothing you have written is affected, an expired build stops running rather than changing anything, and the update link that used to 404 now resolves #618) | 2026-09-01 | `de916943c43d42824fb176394154a40bab8c091e0996815281ec58185f3d0ab5` |
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
| `sanad-0.6.1.vsix` **(current)** | `ac96d2ebbf5840305f78cc3e39a87e5251502fcec1f79ade88ad09f4780e13f6` |
| `sanad-0.6.0.vsix` (withdrawn — see above) | `1ffafc11431cb3ca64cd8f92492a4aaf614f3791862d34486422f5ac24721ea9` |
| `e-rew-0.5.3.vsix` | `b0c6c7cc3035890b36898d77dc14a31c1b57fb0acabe349ac4b7bf4c1c65f3ba` |
| `e-rew-0.5.2.vsix` | `b68f5950cad05c0fb294a51f8666d98228e9e20f564ec1344ead9475670fd239` |
| `e-rew-0.5.1.vsix` | `7ce130fd64669da75b041313d265563f7d14f8380382104e37baa452a32b98f2` |
| `e-rew-0.5.0.vsix` | `1680ba5c29a1715c933e7360a57664cc5c248e731c7c2731db3d4ebee4afc784` |

## Known gaps in 0.6.1

Hit one of these, or something not listed here? **Technical support:** support@ejadahailabs.com

**This build stops working on 2026-10-16.** While Sanad is a prototype, each release
runs for 45 days from the day it was cut (2026-09-01 for this one), then asks you to
update. The count starts at the release date, not your install date, so everyone on
this build stops on the same day. You get a non-blocking notice on each of the last
seven days. Nothing you have written is affected — your requirements are your own
files in your own repository, and an expired build stops running rather than
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

This is pilot software, proprietary and time-limited — see `LICENSE.txt` inside
the extension. It is provided for evaluation.

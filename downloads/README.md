# Installing Sanad

The newest `.vsix` in this folder is the current build. Install it from any machine — you
do not need this repository cloned, just the file.

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
| `E-REW@9702f7b` (MAIN — **RC5b**, development-complete candidate; everything in the previous slot, plus: the **Verifies** lane on a requirement card now fills from four places instead of one — a `verifies` marker inside a test, ids written in the test's own content under a prefix you declare, a verification data dictionary (requirement + verification id + unit under test, mapped through your own `fields:`), and the test **file name** (`test_FAF-LLR-743.py`, and bare numbers that inherit the prefix in front of them) — with the marker still winning where one exists, and an id in a name that your requirements do not declare reported as a finding instead of quietly linked #605; **every function inside the trace scope you declared that no requirement traces to is now listed by name**, with a per-file total on top of it, and the list is capped and says so rather than pretending to be complete #613; and, new policy: **while Sanad is a prototype each build runs for 45 days from its release date** — this one is dated 2026-09-01, so it stops on **2026-10-16**, with a non-blocking notice on each of the last seven days naming that date; nothing you have written is affected, an expired build stops running rather than changing anything, and the update link that used to 404 now resolves #618) | 2026-09-01 | `de916943c43d42824fb176394154a40bab8c091e0996815281ec58185f3d0ab5` |
| **RELEASE `sanad-0.6.0.vsix`** (from tag v0.6.0 @ 569546b — re-cut; unchanged by the test-slot refresh above, which is ahead of it) | 2026-08-31 | `1ffafc11431cb3ca64cd8f92492a4aaf614f3791862d34486422f5ac24721ea9` |

Superseded builds, kept for the record — **do not install these**:

| Superseded | Date | SHA-256 |
|---|---|---|
| test-build slot `E-REW@ba5db12` (RC5a — everything in the previous slot, plus: clicking a code link on a requirement card now opens the **function's definition**, and the Implements/Verifies links actually go somewhere instead of failing #612; impact analysis now reads the symbol index that is already loaded, so it tells C++ overloads apart when they differ only by a `const` qualifier #609; a requirement reference that points at nothing is reported as a finding and gets its own section in the report #616; on a repository that uses `.trace`, code that no requirement traces to is now reported as untraced #615; and the on-disk term index is encrypted at rest, ADR-0105 #608. Verifies-lanes and per-function refinement follow in RC5b) | 2026-09-01 | `ef8b76819e701482f2bc993c48ab3a4a4329b0086097d7e017729de966d03ac6` |
| `sanad-0.6.0.vsix` first cut (tag v0.6.0 @ 8bbb0b0, before the fix batch) — replaced the same day by the re-cut above | 2026-08-31 | `1a2df3c8291b420a17b167de1c9cbddbc91dcce17c928882f86803653c307d24` |
| test-build slot `E-REW@c840ba6` (RC4 — data-dictionary values no longer flagged as vague or unknown #599; large-dictionary startup stall fixed #601) | 2026-09-01 | `a4f81b4b91d54d8a9257d4dbae6af1d83500dc33ed43e67cc86a3e5413d533e0` |
| test-build slot `E-REW@f5a7061` (structural YAML data-dictionary loading: #597 · #598) | 2026-09-01 | `43bd2b59ee7c3f18ef0cb8d9dfe41244915775a4f3787e4492af093d505f57b0` |
| test-build slot `E-REW@77b1bdb` (whole-literal-first marked spans: #594 · #595) | 2026-08-31 | `44160ff8132212ec29bc01c237aa1fcdc3cb473d20f472cfd0be5f1496ca2059` |
| test-build slot `E-REW@c9ac327` (separator spacing + pytest underlines: #591 · #590) | 2026-08-31 | `638f371da61fffe9445e02897c53d1872533cac1d3736b11f683bae8f3c7dc47` |
| test-build slot `E-REW@bab424e` | 2026-08-31 | `197f28d003460dad9e40f4b1a8f73ab0a8c867ac10420625de6e0927884d52e0` |
| test-build slot `E-REW@12e44f7` | 2026-08-31 | `92fcf70bb8544c3f63afb7a652b71528451b19804dcd493712452bce0dbe89e6` |
| test-build slot `E-REW@bc9dc77` | 2026-08-31 | `eb2dc372f277a860022381e903b53119fa4824601e955f0eb239e040bb9f88ee` |
| test-build slot `E-REW@9556dda` | 2026-08-31 | `b8a96e3c1bcb5dcde399c1a1485c76548e7c284308ad6f6bb8f23ef4bbd34387` |

## Install

Download `e-rew-0.5.3.vsix`, then either:

**From the command line**

```
code --install-extension e-rew-0.5.3.vsix
```

**From inside VS Code** — Extensions view → `...` menu → *Install from VSIX...*

**Then quit VS Code completely** (`Ctrl+Q`, or Quit from the menu). A window
reload is not enough: it does not swap the extension host, and the old build
stays live. This is the single most common reason a fresh install "does nothing".

## Verify what you installed

```
sha256sum e-rew-0.5.3.vsix
```

| Build | SHA-256 |
|---|---|
| `e-rew-0.5.3.vsix` | `b0c6c7cc3035890b36898d77dc14a31c1b57fb0acabe349ac4b7bf4c1c65f3ba` |
| `e-rew-0.5.2.vsix` | `b68f5950cad05c0fb294a51f8666d98228e9e20f564ec1344ead9475670fd239` |
| `e-rew-0.5.1.vsix` | `7ce130fd64669da75b041313d265563f7d14f8380382104e37baa452a32b98f2` |
| `e-rew-0.5.0.vsix` | `1680ba5c29a1715c933e7360a57664cc5c248e731c7c2731db3d4ebee4afc784` |

## Known gaps in 0.5.3

Hit one of these, or something not listed here? **Technical support:** support@ejadahailabs.com

0.5.3 delivered the owner test round: the form is the default reading and editing
surface (read-only first, Word-style tables and nested lists, EARS patterns,
on-demand Validate, interactive hovers, the quality score badge). Still
engine-only or unfinished, completing in a later release:

| Area | State |
|---|---|
| Template designer form (design a template end to end) | not yet — configuration edits types field by field |
| Guided setup (artefact checkboxes + paths, relationships UI, per-artefact templates, rules adoption step) | partial — field-role mapping and the relationship map exist; the full guided flow is being built |
| Declaring your own data-dictionary sources | works via `producers:` in `.ejadah/rew/config.yaml` (documented in the user guide); no form yet |
| Workbench v1 configuration layers (product file, artifact_types schema, standards mapping, views) | filed as FEAT-070…080; unbuilt parts appear as visible "Work in progress" panels, never silently |
| Finding marks inside table cells | the note list under the field carries them; per-character underlines in cells are not drawn |

## Opening your requirements for the first time

Sanad activates on a folder that contains `.ejadah/rew/`. A folder that has
never been set up will not light up on its own — that is expected, not a broken
install.

1. Open the folder holding your requirements.
2. Command Palette → **REW: Set Up REW in This Folder**.
3. Choose where templates should come from:

| Your situation | Choose |
|---|---|
| Requirements already exist | **Derive a template from existing requirements** |
| Starting from nothing | **Create a starter template** |
| You already have Sanad templates | **Copy templates I already have** |

4. Reload the window when it offers.

Your requirement files are **never rewritten** by setup. Sanad only writes
`.ejadah/rew/templates/` and `.ejadah/rew/config.yaml`, and only if they do not
already exist.

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

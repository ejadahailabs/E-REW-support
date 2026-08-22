# Installing Sanad

The newest `.vsix` in this folder is the current build. Install it from any machine — you
do not need this repository cloned, just the file.

## Test build (pre-release slot — owner testing only)

`e-rew-test-build.vsix` is the rolling **0.5.3 candidate** built from `main`. It is
overwritten every bug round; the file name and URL never change. Not a release.

```
curl -LO https://github.com/ejadahailabs/E-REW-support/raw/main/downloads/e-rew-test-build.vsix
code --install-extension e-rew-test-build.vsix   # then quit VS Code fully (Ctrl+Q)
```

| Built from | Date | SHA-256 |
|---|---|---|
| `E-REW@6fb0dae` | 2026-08-22 | `656e09585cafb8cc226faeac1be0da6491cc14ce36f0c09c04ba251ed2fd77df` |

## Install

Download `e-rew-0.5.2.vsix`, then either:

**From the command line**

```
code --install-extension e-rew-0.5.2.vsix
```

**From inside VS Code** — Extensions view → `...` menu → *Install from VSIX...*

**Then quit VS Code completely** (`Ctrl+Q`, or Quit from the menu). A window
reload is not enough: it does not swap the extension host, and the old build
stays live. This is the single most common reason a fresh install "does nothing".

## Verify what you installed

```
sha256sum e-rew-0.5.2.vsix
```

| Build | SHA-256 |
|---|---|
| `e-rew-0.5.2.vsix` | `b68f5950cad05c0fb294a51f8666d98228e9e20f564ec1344ead9475670fd239` |
| `e-rew-0.5.1.vsix` | `7ce130fd64669da75b041313d265563f7d14f8380382104e37baa452a32b98f2` |
| `e-rew-0.5.0.vsix` | `1680ba5c29a1715c933e7360a57664cc5c248e731c7c2731db3d4ebee4afc784` |

## Known gaps in 0.5.2

0.5.2 closed most of 0.5.1's list: the rules form, the migration preview on save,
inline requirement-type rows, and field-role mapping in setup are now real surfaces.
Still engine-only or unfinished, completing in the next release:

| Area | State |
|---|---|
| Template designer form (design a template end to end) | not yet — configuration edits types field by field |
| Guided setup (artefact checkboxes + paths, relationships UI, per-artefact templates, rules adoption step) | partial — field-role mapping and the relationship map exist; the full guided flow is being built |
| Declaring your own data-dictionary sources | works via `producers:` in `.ejadah/rew/config.yaml` (documented in the user guide); no form yet |

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

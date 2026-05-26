# Codex Work Notes

## Workspace

Working folder:

```text
E:\TNT\0.TNT ISO\00.TNT ISO 2026\00_LISP
```

This folder is managed with Git. Use Git as the source sync method between machines, not Synology folder sync.

## Current Git Workflow

Before editing on any machine:

```powershell
git pull
```

After editing:

```powershell
git add .
git commit -m "Describe changes"
git push
```

On another machine:

```powershell
git pull
```

## Current Package State

- `TNT_PACKAGE_00_SYSTEM_ALL.lsp`
  - Contains system helpers, global settings, standard TNT layer table, system variable setup.
  - Removed `TNT_SYSTEM_DEBUG`.
  - Standard furniture layer spelling is now `....12_TNT_F_FURNITURE`.

- `TNT_PACKAGE_01_CREATE_ALL.lsp`
  - Creates standard system resources such as layer, dimension/text/style resources.
  - Layer references were updated to the four-dot standard prefix where found.

- `TNT_PACKAGE_03_MANAGE_ALL.lsp`
  - Previously cleaned by removing unused manage tools requested by user.

- `TNT_PACKAGE_04_DRAW_ALL.lsp`
  - Active editor tab in VS Code during recent work.
  - VBB layout rectangle layer was changed to use standard layer `....11_TNT_A_PLOT`.

- `TNT_PACKAGE_05_LAYER_ALL.lsp`
  - Main layer command is `TNT_LAYER`.
  - Old commands `TNT_LAYER_CHANGE`, `TNT_LAYER_FIX`, `TNT:LAYER:SYNC` were consolidated earlier into the `TNT_LAYER` workflow.
  - Layer shortcut commands were moved out to `TNT_PACKAGE_11_SHORTCUT.lsp`.
  - Added dimension shortcuts:
    - `D1` calls `.DIMLFAC`
    - `D2` calls `.DIMSCALE`
  - Shortcut `NFU` now uses `....12_TNT_F_FURNITURE`.

- `TNT_PACKAGE_06_TEXT_ALL.lsp`
  - Text package includes `T1`, `TA`, `FT`, `DF`, `DFX`, `DX`, `BMASK`, `ED2`.

- `TNT_PACKAGE_07_LEADER_ALL.lsp`
  - Leader package includes `A1`, `A2`, `A3`, `A4`.
  - Note: code contains a call to `c:ALP` inside `A2`, but no `ALP` command definition was found in that package.

- `TNT_PACKAGE_08_DIMENSION_ALL.lsp`
  - Dimension package includes `CD`, `BD`, `SD1`, `SD2`, `D3`, `D4`, `D5`, `AD`, `ADIM`, `LB1`, `LB2`, `LB3`.

- `TNT_PACKAGE_09_HATCH_ALL.lsp`
  - Hatch package includes `H25`, `H251`, `H252`, `H253`, `H254`, `H255`, `HB`, `HF`, `HC`, `HV`, `HS`, `HA`, `HT`, `HG`, `HSE`, `HSA`, `RHB`.

- `TNT_PACKAGE_10_BLOCK_ALL.lsp`
  - Block package includes `SSC`, `ASC`, `B0`, `CBP`, `B3`, `MAB`, `CB`, `RB`, `AT1`, `AT2`, `B1`, `CA`.

- `TNT_PACKAGE_11_SHORTCUT.lsp`
  - Owns the layer shortcut table and generates shortcut commands such as `NDR`, `NFU`, `NTE`, `NDI`.
  - Edit `TNT:SHORTCUT:LAYER:TABLE` to add/change layer shortcuts.
  - Command `TNT_SHORTCUT_ALL` reloads and prints the shortcut table.

## Layer Standard Notes

The TNT standard layer prefix is now four dots:

```text
....
```

The old three-dot prefix:

```text
...
```

should not be used for standard TNT layers in the current packages.

Corrected furniture layer:

```text
....12_TNT_F_FURNITURE
```

Old misspelling:

```text
....12_TNT_F_FUNITURE
```

should not be used in standard package references.

## Removed / Deprecated Commands

- `TNT_SYSTEM_DEBUG`
- `TNT_LAYER_MIGRATE`
- `TNT_LAYER_PREFIX`
- Old layer commands requested earlier:
  - `TNT_1_GENERAL_SYNC`
  - `TNT_LAYER_CHANGE`
  - `TNT_LAYER_FIX`

## How To Continue On Another Machine

1. Clone or open the Git repo on the other machine.
2. Run:

```powershell
git pull
```

3. Open VS Code/Codex in this folder.
4. Tell Codex:

```text
Read CODEX_NOTES.md and continue working only in 00_LISP.
```

5. After edits, commit and push changes.

## Important Rule

Only work in:

```text
E:\TNT\0.TNT ISO\00.TNT ISO 2026\00_LISP
```

Do not edit legacy folders unless explicitly requested.

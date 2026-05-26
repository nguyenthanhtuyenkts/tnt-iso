# Codex Work Notes

## Workspace

Working folder:

```text
C:\00.TNT ISO\00.TNT ISO FULL 2026
```

This folder is managed with Git. Use Git as the source sync method between machines, not Synology folder sync.

Historical working folder on another machine, not relevant for this current machine:

```text
E:\TNT\0.TNT ISO\00.TNT ISO 2026\00_LISP
```

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
  - Owns the layer shortcut table and generates layer shortcut commands.
  - Current layer shortcuts include:
    - Architect: `NKH`, `NT`, `NM`, `NK`, `NC`, `NSL`, `NTR`, `NDE`, `NCOM`, `NCOT`, `NPL`.
    - Furniture: `NNT`, `NCC`, `NGL`, `NDO`.
    - Structure: `NCON`, `NWA`.
    - Annotate: `NTE`, `NLE`, `NDI`, `NHA`, `NAN`.
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

## Work Log

- 2026-05-26
  - Current Codex session started from `C:\00.TNT ISO`.
  - User selected `C:\00.TNT ISO\00.TNT ISO FULL 2026` as the main working folder.
  - Read `CODEX_NOTES.md` to recover prior project context before making further changes.
  - Confirmed Git working tree was clean before updating this note file.
  - Updated this note file so the active working folder points to the current `C:` location.
  - Clarified that the old `E:` path belongs to another machine and does not need attention in this workspace.
  - Added standalone `TNT_EXPORT_LAYERS_TO_TXT.lsp`.
  - New AutoCAD command: `TNT_EXPORT_LAYERS`.
  - The command exports only layer names from the active DWG to `TNT_LAYER_EXPORT_<dwg-name>.txt` in the same folder as the loaded Lisp file, with fallback to `DWGPREFIX`.
  - Added standalone `TNT_LAYER_MIGRATE_BY_NAME.lsp`.
  - New AutoCAD commands:
    - `TNT_LAYER_MIGRATE_PREVIEW`: writes a layer mapping report only.
    - `TNT_LAYER_MIGRATE`: creates/resets TNT ISO standard layers, then moves matched objects from old layers to standard layers.
  - Migration logic is based on normalized layer-name tokens plus alias keywords; it skips `0`, `Defpoints`, xref-dependent layers, separator layers, and layers already matching the TNT ISO standard.
  - Fixed `TNT_LAYER_MIGRATE_PREVIEW` runtime error `incorrect object to bind: T` by renaming the local variable `t` in the overlap scoring function.
  - Fixed `TNT_LAYER_MIGRATE_PREVIEW` runtime error `bad argument type for compare` by removing `vl-sort` with numeric `<` for layer-name strings, adding string-list sorting via `acad_strlsort`, hardening string equality, and validating standard-layer rows before scoring.
  - Added fallback standard layer names inside `TNT_LAYER_MIGRATE_BY_NAME.lsp` so preview can still run if the loaded package table is unavailable or polluted by shortcut-table data.
  - User decided not to use the generic similarity migrate tool.
  - Added `TNT_MIGRATE_V16_LAYERS.lsp` based on old layer names listed in `TNT_LAYER_EXPORT_V16.txt`.
  - New AutoCAD commands:
    - `TNT_MIGRATE_V16_PREVIEW`: writes a report of recognized V16 old-layer mappings and object counts.
    - `TNT_MIGRATE_V16`: moves objects from recognized old V16 layers to TNT ISO standard layers; unmapped/unknown layers are kept unchanged.
  - Added `TNT_MIGRATE_V16_FROM_MAPPING.lsp`, a replacement mapping workflow that reads external mapping file `mapping v16.txt` or `mapping v16`.
  - New AutoCAD commands:
    - `TNT_MIGRATE_V16_MAP_PREVIEW`: reads the mapping text file and writes a report only.
    - `TNT_MIGRATE_V16_MAP`: moves objects according to the mapping text file; unmapped layers are kept unchanged.
  - Added `TNT_MIGRATE_V16_FIXED_MAPPING.lsp`, a new fixed-mapping Lisp generated directly from `mapping v16.txt`.
  - New AutoCAD commands:
    - `TNT_MIGRATE_V16_FIXED_PREVIEW`: writes a report from the hardcoded mapping.
    - `TNT_MIGRATE_V16_FIXED`: moves objects according to the hardcoded mapping; unmapped layers are kept unchanged.
  - Added `TNT_MIGRATE_V16_SIMPLE.lsp` after user requested the simplest possible migrate command.
  - Preferred current V16 migrate command: `TNT_MIGRATE_V16_SIMPLE`.
  - This simple command has no preview/report, uses hardcoded mapping from `mapping v16.txt`, and runs `CHPROP` on `ssget "_X"` selections immediately.
  - User updated layer shortcuts in `TNT_PACKAGE_11_SHORTCUT.lsp`.
  - Checked package command names for exact conflicts with the new layer shortcuts; no exact `c:` command collision was found in the current Lisp packages.
  - Updated this note file to reflect the current shortcut names and remove stale `NDR`/`NFU` examples.

## Important Rule

Only work in:

```text
C:\00.TNT ISO\00.TNT ISO FULL 2026
```

Do not edit legacy folders unless explicitly requested.

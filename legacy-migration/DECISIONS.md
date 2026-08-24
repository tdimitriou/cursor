# Legacy project reorganization — decisions log

Status: **clarifications in progress** (not ready for execution yet)

Companion draft brief: [`AGENT_HANDOFF.md`](./AGENT_HANDOFF.md) — locked sections filled; open questions marked.

## Locked decisions

| # | Topic | Decision |
|---|---|---|
| 1 | Disks | Do **E: first**. Connect older USB SSD later; second pass matches against E: catalog. |
| 2 | Agent runtime | Use a **Local** agent in Cursor Agents window (Windows). Cloud agents cannot see E:. |
| 3 | What is E: | Full clone of this PC’s SSD before factory reset. |
| 4 | Free space | ~39 GB free of 439 GB. Prefer not to delete OS junk yet. |
| 5 | Scan skips (for now) | Skip `Windows`, `Program Files`, `Program Files (x86)`. Review other roots later. |
| 6 | Cursor data | Keep/restore Cursor AppData transcripts when moving repos; do not treat as disposable. |
| 7 | Languages / tools | VB6, C/C++, PHP, B4A, B4J, Java, JavaScript, VBScript, VB.NET, TwinBasic, Delphi/Pascal, SQL; VS6, modern VS, VS Code, TwinBasic IDE, B4A/B4J, Lazarus, Delphi, PHPRunner, AppGini. |
| 8 | Project detection | Formal project files from those tools **and** loose source trees without project files (do **not** ignore). |
| 9 | Variants | Keep all variants except when project/source files are **fully identical**. |
| 10 | Exact duplicates | Policy **B**: move extras to quarantine with keep/remove log; purge later after approval. |
| 11 | Origin labeling | Label authored / downloaded / unknown using path hints, site names, licenses, readmes, etc. |
| 12 | Origin in paths? | **No** origin-based subfolders (e.g. not `vbaccelerator\`). Origin goes in catalog metadata. |
| 13 | Reorg root | **`E:\Dev`** (reorganize on E: first; optional later move to `C:\Dev`). |
| 14 | Copy vs move | **Move** on E: (not copy). |
| 15 | Taxonomy | Provisional only; refine after first inventory pass. Start with `E:\Dev\_inventory\`. |
| 16 | Move mode | **C**: auto-move only under **user-approved rules**; uncertain → propose / `unclassified`. |
| 17 | Catalog format | **C**: SQLite as master + XLSX exports. Possible later Python web UI to browse inventory. |

## Open questions (ask user one-by-one)

- [x] Catalog format: **C — SQLite master + XLSX exports** (may later add a small Python web UI to browse inventory)
- [ ] Execution style: generate scripts vs interactive agent scanning
- [ ] Overnight / resumable runs OK?
- [ ] Priority clusters first (POS, add-ins, DB) vs full equal pass?
- [ ] Hard safety: anything never move/delete without explicit OK?
- [ ] Secrets/.env handling in reports
- [ ] How to treat existing `C:\Dev` work (e.g. POS) vs E: reorganization
- [ ] Workspace path for the Local agent (`E:\Dev\_inventory` recommended)

## Provisional scaffold (may change after pass 1)

```text
E:\Dev\
  _inventory\              # catalogs, scripts, reports
  products\
  vb6-addins\
  db-access\
  libraries\
  tools\
  downloaded\              # optional; may be metadata-only instead
  experiments\
  unclassified\
  duplicates-quarantine\
```

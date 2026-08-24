# Legacy project reorganization — decisions log

Status: **FINAL** — ready to hand to a Local Windows agent  
Companion: [`AGENT_HANDOFF.md`](./AGENT_HANDOFF.md)

## Locked decisions

| # | Topic | Decision |
|---|---|---|
| 1 | Disks | Do **E: first**. Connect older USB SSD later; second pass matches against E: catalog. |
| 2 | Agent runtime | Use a **Local** agent in Cursor Agents window (Windows). Cloud agents cannot see E:. |
| 3 | What is E: | Full clone of this PC’s SSD before factory reset. |
| 4 | Free space | ~39 GB free of 439 GB. Prefer not to delete OS junk yet. |
| 5 | Scan skips (for now) | Skip `Windows`, `Program Files`, `Program Files (x86)`. Review other roots later. |
| 6 | Cursor data | Keep/restore Cursor AppData transcripts when moving repos; do not treat as disposable. |
| 7 | Languages / tools | VB6, C/C++, PHP, B4A, B4J, Java, JavaScript, VBScript, VB.NET, TwinBasic, Delphi/Pascal, SQL, **Python**; VS6, modern VS, VS Code, TwinBasic IDE, B4A/B4J, Lazarus, Delphi, PHPRunner, AppGini. |
| 8 | Project detection | Formal project files from those tools **and** loose source trees without project files (do **not** ignore). |
| 9 | Archives | Scan compressed archives (zip/7z/rar/tar/gz/…), especially Downloads; may coexist with extracted trees or be unextracted only. Link archive ↔ extracted siblings when possible. |
| 10 | Variants | Keep all variants except when project/source files are **fully identical**. |
| 11 | Exact duplicates | Policy **B**: move extras to quarantine with keep/remove log; purge later after approval. |
| 12 | Origin labeling | Label authored / downloaded / unknown using path hints, site names, licenses, readmes, etc. |
| 13 | Origin in paths? | **No** origin-based subfolders. Origin goes in catalog metadata only. |
| 14 | Reorg root | **`E:\Dev`** (reorganize on E: first; optional later move to `C:\Dev`). |
| 15 | Copy vs move | **Move** on E: (not copy). |
| 16 | Taxonomy | Provisional only; refine after first inventory pass. Start with `E:\Dev\_inventory\`. |
| 17 | Move mode | **C**: auto-move only under **user-approved rules**; uncertain → propose / `unclassified`. |
| 18 | Catalog format | **C**: SQLite as master + XLSX exports. Schema should stay friendly for a future Python web UI. |
| 19 | Execution style | **Hybrid:** resumable scripts for bulk inventory; agent for classification, judgment, reports. |
| 20 | Long runs | Overnight OK; must be **resumable** after reboot. |
| 21 | Priority | Full inventory first; then prioritize move-rule batches for **POS**, **VB6 add-ins**, **DB-access**. |
| 22 | Hard safety | Never empty quarantine / delete sources without explicit OK; never touch `C:\Dev` active work without asking; never wipe Windows/PF unless asked. |
| 23 | Secrets | Do not export secret values into XLSX; record only that sensitive files exist. |
| 24 | Existing `C:\Dev` | Leave alone in phase 1; note name overlaps in catalog. |
| 25 | Agent workspace | Local agent on `E:\Dev\_inventory`, with access to scan `E:\` (multi-root if needed). |

Note: Items 19–25 were **explicitly accepted by the operator** (“accept default”). Override anytime before/during Local agent execution if needed.

## Provisional scaffold (may change after pass 1)

```text
E:\Dev\
  _inventory\              # catalogs, scripts, reports
  products\
  vb6-addins\
  db-access\
  libraries\
  tools\
  experiments\
  unclassified\
  duplicates-quarantine\
```

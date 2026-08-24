# Local Agent Handoff — Legacy project reorganization (E: → E:\Dev)

**Status:** Draft — clarifications still in progress. Do not start mass moves until the operator confirms open items below.

**Operator:** Theodoros Dimitriou  
**Runtime required:** Cursor **Local** agent on Windows (Agents window OK). **Not** a Cloud agent.  
**Why:** Must scan/move folders on local disk `E:`.

---

## Mission

Inventory hundreds/thousands of legacy projects on disk `E:` (full pre-reset clone of this PC’s SSD), detect duplicates/variants, classify by purpose for reuse, and reorganize by **moving** into `E:\Dev\...`. Later, an optional pass will compare an older USB SSD against this catalog. Ultimate goal: make it easy to combine related small projects into larger suites (POS-related, VB6 IDE add-in suite, DB access techniques, etc.).

---

## Locked decisions (do not reopen unless operator changes them)

1. **Disks:** `E:` first only. Do **not** require the older USB SSD for phase 1.
2. **E: identity:** Full clone of this PC SSD before factory reset (~439 GB, ~39 GB free).
3. **No deletes of OS junk yet.** Prefer skip over wipe.
4. **Scan skip (phase 1):** `E:\Windows`, `E:\Program Files`, `E:\Program Files (x86)`.
5. **Other top-level roots:** Do not bulk-skip; review with operator if noisy.
6. **Cursor AppData / transcripts:** Valuable. Do not delete. Do not “clean up” Cursor user data as part of this job.
7. **Languages/tools in scope:** VB6, C/C++, PHP, B4A, B4J, Java, JavaScript, VBScript, VB.NET, TwinBasic, Delphi/Pascal, SQL, **Python**; VS6, modern VS, VS Code, TwinBasic IDE, B4A/B4J, Lazarus, Embarcadero Delphi, PHPRunner, AppGini.
8. **Project detection:** Formal IDE/project files **and** loose source trees (`.bas/.frm/.cls/.pas/.php/.js/.py/...`) **without** a project file — flag as probable projects; do **not** ignore.
8b. **Archives:** Also inventory compressed archives (`.zip`, `.7z`, `.rar`, `.tar`, `.gz`, `.tgz`, and similar), especially under `E:\Users\tdimi\Downloads` and other download areas. Archives may sit beside extracted folders or exist only as archives — catalog both; attempt to detect project-like content inside archives (list/peek) without requiring full extract of everything up front. Link archive ↔ extracted siblings when detectable.
9. **Variants:** Keep all non-identical variants (different development attempts).
10. **Exact duplicates:** Keep one; **move** extras to `E:\Dev\duplicates-quarantine\` with a keep/remove manifest. No purge until operator approves later.
11. **Origin:** Label `authored` / `downloaded` / `unknown` using path names, vendor/site hints (vbaccelerator, vbforums, etc.), licenses, readmes, and similar. Store in catalog — **not** as folder path segments. Downloads paths are a strong `downloaded` signal.
12. **Target root:** `E:\Dev` (reorganize on E:). Optional later migration to `C:\Dev` is out of scope for phase 1–3 unless asked.
13. **Relocation method:** **Move** (same volume), not copy.
14. **Taxonomy:** Provisional; refine after first inventory. Create `E:\Dev\_inventory\` immediately.
15. **Move policy (mode C):** Auto-move only under **operator-approved rules**. High-confidence batches only. Everything else: propose, or leave until approved; uncertain → `unclassified` only when operator agrees.
16. **Folder layout:** By **purpose/theme**, not by download origin.

### Provisional buckets (may change after pass 1)

```text
E:\Dev\
  _inventory\                 # scripts, DB, XLSX, reports, manifests
  products\                   # e.g. POS and related
  vb6-addins\                 # IDE add-ins (purpose), origin in DB
  db-access\                  # drivers / access techniques
  libraries\                  # reusable shared code
  tools\                      # utilities, RAD exports, helpers
  experiments\                # spikes / unfinished
  unclassified\               # needs operator review
  duplicates-quarantine\      # exact duplicates only
```

Do **not** create origin-based trees like `vb6-addins\vbaccelerator\`.

---

## Open items — CONFIRM WITH OPERATOR BEFORE ASSUMING

| ID | Topic | Suggested default (if operator says “use defaults”) |
|---|---|---|
| Q12 | Catalog format | **LOCKED: Both** — SQLite master + XLSX exports; schema should stay friendly for a future Python web UI |
| Q13 | How to execute scan | Prefer resumable PowerShell (or Python) scripts under `_inventory`, agent designs/runs/monitors them |
| Q14 | Long runs | Overnight OK; must be resumable after reboot |
| Q15 | Priority | Full inventory first; prioritize move-rule batches for POS, VB6 add-ins, DB-access |
| Q16 | Hard safety | Never empty quarantine; never delete sources; never touch `C:\Dev` active work without asking |
| Q17 | Secrets | Do not put secret values into XLSX; record only that sensitive files exist |
| Q18 | Existing `C:\Dev` | Leave alone in phase 1; note possible overlaps in catalog when names match |
| Q19 | Agent workspace | Open Local agent on `E:\Dev\_inventory` and ensure access to scan `E:\` (multi-root if needed) |

---

## Phased plan (execute in order)

### Phase 0 — Workspace setup
1. Create `E:\Dev\_inventory\` (and empty provisional bucket dirs only if useful; taxonomy can wait).
2. Open Cursor Local agent with workspace rooted at `E:\Dev\_inventory` (add `E:\` as needed for scanning).
3. Confirm skips: Windows / Program Files / Program Files (x86).
4. Write a short `E:\Dev\_inventory\README.md` describing phases and safety rules.

### Phase 1 — Inventory only (no moves)
1. Enumerate candidate projects under `E:\` excluding skip dirs and excluding anything already under `E:\Dev\` once populated.
2. Detection heuristics (non-exhaustive):
   - VB6: `.vbp`, `.vbg`
   - TwinBasic: `.twinproj` (and related)
   - VS/.NET/C++: `.sln`, `.csproj`, `.vbproj`, `.vcxproj`
   - Delphi/Lazarus: `.dpr`, `.dproj`, `.lpi`
   - B4X: `.b4a`, `.b4j`
   - Web: `composer.json`, `package.json`
   - Python: `pyproject.toml`, `requirements.txt`, `Pipfile`, `setup.py`, `*.ipynb` clusters
   - RAD: PHPRunner / AppGini project signatures when identifiable
   - `.git` directories
   - Loose source density without project file → `probable_loose_source`
   - Archives: `.zip`, `.7z`, `.rar`, `.tar`, `.gz`, `.tgz`, etc. → catalog as `archive`; peek listing for project markers; note if a sibling extracted folder appears to match
3. Pay special attention to `E:\Users\tdimi\Downloads` (and similar download trees): archives + extracted copies often coexist.
4. Record per candidate at least:
   - id, original_path, detected_type, languages, has_git
   - size, file_count, newest_mtime
   - content/fingerprint hash for duplicate detection
   - origin_guess + origin_signals
   - archive metadata when applicable (format, inner project markers, linked extracted path)
   - proposed_bucket (nullable until rules exist)
   - notes
5. Build duplicate groups:
   - exact identical → quarantine candidates
   - near variants → keep all; link as variant_group
   - archive vs extracted sibling → link; do not treat as unrelated
6. Deliverables in `_inventory`:
   - SQLite DB (master)
   - XLSX export
   - Markdown summary: counts by type, top clusters, scary path list, recommended next rules

**Stop and report.** Do not move yet.

### Phase 2 — Taxonomy + rule sheet
1. From inventory clusters, propose refined buckets and **move rules** (path/name/signal → destination).
2. Operator approves/edits the rule sheet (`_inventory\move-rules.md` or table in DB).
3. No mass moves until rules are approved.

### Phase 3 — Controlled moves (mode C)
1. Apply only approved rules.
2. For each move: log `from → to`, update catalog paths, verify destination exists.
3. Exact duplicates → `duplicates-quarantine\` + manifest (kept path, quarantined path, hash).
4. Unmatched → leave in place or stage proposals; do not invent categories.
5. Produce batch report after each rule batch.

### Phase 4 — Later (separate task)
- Attach older USB SSD; scan; match against E: catalog; import only novel items / new variants.

---

## Safety invariants (always)

- Local Windows agent only.
- Phase 1 = read/inventory only.
- Moves only under approved rules.
- Exact-duplicate extras → quarantine, not delete.
- No origin-based folder taxonomy.
- Do not wipe `Windows` / Program Files unless operator later asks.
- Preserve ability to resume; prefer scripts + DB state over one-shot chat memory.
- Ask operator when uncertain about a large/irreversible action.

---

## Suggested first message to the Local agent

> Read `AGENT_HANDOFF.md` and `DECISIONS.md`. Confirm you are running as a Local Windows agent with access to `E:`. Execute **Phase 0 + Phase 1 only**. Create `E:\Dev\_inventory`, build the project catalog (respecting skip dirs and detection rules), detect exact duplicates and variants, and produce the summary report. Do not move or delete any project folders until I approve Phase 2 rules.

---

## Companion file

See `DECISIONS.md` for the clarification log.

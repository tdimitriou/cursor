# Copy-paste: first message to the Local agent

Open Cursor Desktop → open `E:\Dev\_inventory` (create if needed; add `E:\` to workspace if required) → Agents window → **New agent** → environment **Local / This Computer**.

Then paste:

---

Read the briefing pack from the `tdimitriou/cursor` repo folder `legacy-migration/` (or copies in this workspace): `AGENT_HANDOFF.md` and `DECISIONS.md`.

Confirm you are running as a **Local Windows agent** with filesystem access to `E:`.

Execute **Phase 0 + Phase 1 only**:
1. Create/use `E:\Dev\_inventory`
2. Build the SQLite master catalog + XLSX export
3. Respect skip dirs: `Windows`, `Program Files`, `Program Files (x86)`
4. Detect projects across the listed languages/tools, including **Python** and **loose source trees**
5. Scan **compressed archives** (especially `E:\Users\tdimi\Downloads`); link archives to extracted siblings when possible
6. Detect exact duplicates (quarantine candidates) and keep non-identical variants
7. Produce a Markdown summary report with counts, clusters, and recommended Phase 2 move rules (prioritize POS, VB6 add-ins, DB-access)

**Do not move or delete any project folders** until I approve Phase 2 rules.

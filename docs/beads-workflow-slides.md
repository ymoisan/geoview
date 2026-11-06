# 📊 Beads Setup & Plan‑First Workflow

---

## Slide 1 — What Beads Is (for me)

- **`bd`** = local, dependency‑aware issue tracker (Dolt‑backed).
- **Local‑only**: `.beads/` is git‑excluded — never committed, never in a PR.
- **Independent** of the app toolchain (Rush/Node). Runs on its own.
- Used for **all task tracking** — no markdown TODOs, no TodoWrite.

---

## Slide 2 — My WSL Setup (the working config)

| Piece | Value |
|---|---|
| Mode | **shared‑server** (Dolt DB on native ext4, not `/mnt`) |
| DB location | `~/.beads/shared-server/` (port 3308) |
| Repo footprint | `.beads/` holds **config only** |
| **Critical fix** | `git config --local beads.role maintainer` |
| Run where | **integrated terminal only** (no MCP / Copilot‑CLI) |

> The one gotcha that broke everything: `beads.role = contributor` routed writes
> to a **read‑only** store → `store is read-only`. `maintainer` fixes it.

---

## Slide 3 — The Golden Rule: **Plan First, Beads Second**

```
  📝  Write .md plan   →   ✅ I approve   →   🔩 Agent generates beads
     (human-readable)      (gate)            (epic + tasks + deps)
```

- The **markdown plan is the source of truth** I review and edit.
- Beads are **generated from the approved plan**, not invented ad‑hoc.
- Nothing gets created in `bd` until I say go.

---

## Slide 4 — Step‑by‑Step Flow

1. **Draft** — a `.md` plan: epic, tasks, dependencies, gates (I won't `git add` it).
2. **Review** — I read it; see a **Mermaid graph** of the structure.
3. **Gate** — I approve ("go").
4. **Generate** — agent runs `bd create` (epic + `--parent` tasks) and
   `bd dep add` (blocking edges).
5. **Gate** — agent **stops** once all beads + deps exist; re‑shows Mermaid.
6. **Execute** — one step per "go": `in_progress` → `close`, diagram updated each time.

---

## Slide 5 — What the Plan Encodes → Beads

| In the `.md` plan | Becomes in beads |
|---|---|
| Epic | `bd create -t epic` |
| Task under epic | `bd create --parent <epic>` |
| "B needs A" | `bd dep add B A` (blocks) |
| "C parallel with A" | no edge (independent) |
| "…and a go from me" | **human gate** (out‑of‑band, not a bead) |

---

## Slide 6 — Human Gates = Control Points

- Dependencies enforce **order** (B waits for A).
- Gates enforce **my approval** between steps — even when unblocked.
- After each gate: advance one bead, refresh the **Mermaid** state, stop.
- Result: fully auditable, screenshot‑able progression.

---

## Slide 7 — One‑Line Mental Model

> **A reviewed markdown plan is the contract; beads are its compiled, trackable
> form — generated on my "go", executed one gate at a time.**

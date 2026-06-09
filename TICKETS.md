# Bronson Tracker — Ticket Log

Lightweight backlog for the single-file `index.html` tracker. Work top-to-bottom.

---

## TICKET-001 — Hardenings (do first)

**Status:** DONE (2026-06-01)

Two low-stakes robustness fixes in `index.html`. Neither is a current bug.

1. **Phase III push-up chart.** `buildSeries('pushup-family')` matches the first
   of `['pushup','hindu','diamond']` and `break`s. Phase III has *two* push-up
   moves (Hindu **and** Diamond), so the "MAX PUSH-UPS / SET" chart plots Hindu
   and silently drops Diamond. Fix: track both, or chart the max of the two, so
   Phase III push-up progress isn't half-missing.
2. **Deep-merge `settings` on load/import.** `loadData()` and `importBackup()`
   do a shallow `{...EMPTY_DATA, ...obj}`. Importing an old backup whose
   `settings` predates `manualPhase` / `transitionDismissals` replaces the whole
   `settings` object. Safe today (all readers are null-guarded), but deep-merging
   `settings` guarantees the new fields always exist. Defensive only.

---

## TICKET-002 — Warm-up definitions & links (do after TICKET-001)

**Status:** DONE (2026-06-01) — collapsible accordion, collapsed by default

Turn the warm-up into structured, tappable references — like the existing
exercise `▶ FORM` links — instead of the current static one-liner.

**Where:** `renderSessionLogger()` in `index.html` hardcodes the WARM-UP card as
`"2–3 min: arm circles, neck rolls, hip openers, marching in place."` Replace it
with a list of the 4 movements, each showing its duration + a link button (reuse
the `.form-btn` / `.lib-row` styles already used for exercise videos). Optionally
also surface it in the FORM REFERENCE LIBRARY card.

**Scope:** `program.md` reuses this same warm-up across Phases I–III, so define it
once and show it in every phase. Total warm-up is 2–3 min; the per-move durations
below are a sensible split, not gospel (per Cowork).

**Data (provided by Cowork):**

| Warm-up | Duration | Link |
|---|---|---|
| Arm circles | 20–30s each direction | https://spotebi.com/exercise-guide/arm-circles/ |
| Neck rolls | 5–10 slow rolls each way | https://spotebi.com/exercise-guide/neck-rolls/ |
| Hip openers | 30–60s | https://www.healthline.com/health/opening-hips |
| Marching in place | 1 min | https://spotebi.com/exercise-guide/march-in-place/ |

```json
{
  "warmup": [
    { "name": "Arm circles",       "duration": "20–30s each direction",   "link": "https://spotebi.com/exercise-guide/arm-circles/" },
    { "name": "Neck rolls",        "duration": "5–10 slow rolls each way", "link": "https://spotebi.com/exercise-guide/neck-rolls/" },
    { "name": "Hip openers",       "duration": "30–60s",                   "link": "https://www.healthline.com/health/opening-hips" },
    { "name": "Marching in place", "duration": "1 min",                    "link": "https://spotebi.com/exercise-guide/march-in-place/" }
  ]
}
```

**Notes:** Cowork chose stable how-to pages (illustrations/video) over fragile
individual clips. Cool-down is still a static one-liner — out of scope here
unless we decide to give it the same treatment later.

---

## TICKET-003 — Version stamp + check-for-update

**Status:** DONE (2026-06-01)

Adds `APP_VERSION` plus a "CHECK FOR UPDATE" button (SETUP → ABOUT) that fetches
the live `index.html` (cache-busted, `no-store`), compares its `APP_VERSION` to the
running one, and offers a cache-busting reload (`?v=<latest>`) when they differ —
so updates land without the kill-the-app / re-add-to-home-screen ritual.

**Deploy convention:** bump `APP_VERSION` (near `STORAGE_KEY` at the top of the
script) on EVERY deploy, or the check can't detect the new build.

---

## TICKET-004 — Auto-backup safety net

**Status:** DONE (2026-06-01)

localStorage on an iOS home-screen app can be evicted (≈7 days unused, storage
pressure, clearing Safari data), and the per-session `.md` downloads cover only
workout logs — not baseline/measurements/settings, and they aren't re-importable.
So:
- `settings.lastBackupAt` records the last full backup; `exportAll()` sets it.
- Saving/editing the **baseline** auto-downloads a full backup (precious,
  rarely-changing data).
- A **stale-backup banner** appears when data exists and the last backup is
  missing or >7 days old, with one-tap "Back up now". SETUP shows "Last backup: <when>".

---

## TICKET-005 — Cool-down dropdown

**Status:** DONE (2026-06-01)

Gave the cool-down the same collapsible-accordion treatment as the warm-up
(TICKET-002): a shared `COOLDOWN` constant + `renderCooldown()` / `toggleCooldown()`,
replacing the static one-liner. Four movements (hamstring, quad, chest opener,
child's pose) with durations and spotebi how-to links, collapsed by default.

Verified during this work: warm-up and cool-down are **not** phase-specific —
`PHASES` defines only exercises; both warm-up and cool-down are single shared
constants rendered identically in every phase.

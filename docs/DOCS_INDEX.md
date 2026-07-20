# CPFS Runner — documentation map

**Purpose:** One place that explains *which document is for what*, so marketing, onboarding, and day-to-day use do not get mixed together.

---

## The problem CPFS Runner solves

AI-assisted coding is fast, but it is easy to lose track: the same failed fix tried again, edits in the wrong files, “done” without a real check, no record of what worked. **CPFS Runner** is the editor extension that keeps one fix at a time, warns on scope, requires agreed verification before success, and leaves an auditable trail — **locally**, in your repo.

---

## Document types (four roles)

| Document | Audience | Goal | Where it lives |
|----------|----------|------|----------------|
| **Feature → benefit (marketing)** | Someone deciding whether to install | Map each capability to *why a programmer cares*; problem at top, outcomes at bottom | `MARKETPLACE_LISTING.md` · **`docs/CPFS_DOC_VS_RUNNER_PUBLIC.md`** (doc vs code, 1–10 usefulness) |
| **User guide** | Daily user | Turn on once; work in chat; when to glance at dashboard; plain “what do I do today?” | `docs/USER_GUIDE.md` *(to write before 1.0)* · **`docs/DASHBOARD_GUIDE.md`** (cockpit buttons + order, rc.42+) |
| **Tutorial** | First-time user | One walkthrough: install → enable → one fix → validate → success (15–30 min) | `docs/TUTORIAL.md` + in-app **Get Started with CPFS Runner** |
| **Reference manual** | Power user / support | Every command, setting, file (`.cpfs/`, logs), config keys — lookup, not story | **`docs/REFERENCE.md`** (shipped rc.169) — open via **CPFS: Open Reference Manual** |

**Already shipped today**

| Doc | Role |
|-----|------|
| `README.md` | Repo overview + dev install (FactAI contributors) |
| `INSTALL.md` | VSIX / local install steps |
| `CHANGELOG.md` | Version history |
| `docs/DASHBOARD_ARCHITECTURE_CONTRACT.md` | Stable cockpit structure + future feature slot map |
| [CPFS methodology](https://ragbox.llc/tutorials/cpfs.html) | Free *method* on ragbox.llc — not the extension manual |

---

## What goes where (rule of thumb)

- **Marketplace listing** — short enough to paste; sells outcomes; links to ragbox for depth.
- **User guide** — no command encyclopedia; links to Reference for details.
- **Tutorial** — one happy path only; screenshots/GIFs live here and in Marketplace assets.
- **Reference** — complete command list from `package.json`; no marketing language.

---

## Before Marketplace 1.0.0

| Priority | Doc | Status |
|----------|-----|--------|
| 1 | `MARKETPLACE_LISTING.md` | In progress — feature/benefit + problem/solution |
| 2 | `docs/USER_GUIDE.md` | Not started (see `docs/DASHBOARD_GUIDE.md` for dashboard) |
| 3 | `docs/TUTORIAL.md` | Not started (align with stranger brief) |
| 4 | `docs/REFERENCE.md` | **Shipped rc.169** |
| 5 | Marketplace screenshots / GIF | Owner capture (Task 11) |

---

## Document history

| Date | Change |
|------|--------|
| 2026-07-08 | Added `docs/DASHBOARD_ARCHITECTURE_CONTRACT.md` — fixed cockpit section contract |
| 2026-07-08 | Added `docs/DASHBOARD_GUIDE.md` — cockpit button manual (rc.42) |
| 2026-07-07 | Initial docs map — owner asked for separate manuals + marketing feature/benefit |

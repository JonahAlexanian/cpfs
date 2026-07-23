# CPFS Runner — Program without Programming with AI

**Program without programming with AI.** Let me show you how.

CPFS Runner catches the three things AI coding assistants do wrong — heuristic shortcuts that won't generalize, oversteps into shared code, and spinning on failed logic — and stops them by mechanism, not memory. You direct the work. AI writes the code. You verify the evidence.

---

## The three problems CPFS Runner solves

### 1. AI writes heuristic shortcuts that pass one example but won't generalize

Your AI writes a fix that works on the sample you showed it — but it's a demo-tuned shortcut: a ratio gate (`if area < 8.5`), a path-literal branch (`if name.includes("MAX4")`), a magic number. It passes today and breaks on the next upload.

**CPFS Runner's §13 heuristic scanner** reads every added diff line and flags numeric thresholds in conditionals, path-literal branches, single-sample verification, and workaround comments — before you ship. It also scans prior code for the same patterns so you know what might break.

### 2. AI oversteps into shared code other features depend on

Your AI edits a function. Three other features — already passed and shipped — call that function. The change looks clean. You ship it. Two of those features break.

**CPFS Runner's blast-radius detection** tracks every symbol every task touched. When a new change overlaps a prior passed task's symbols — directly or via a one-hop caller — it tells you to re-verify the affected task before the change ships. Frozen retest recipes re-run automatically and report pass/fail.

### 3. AI spins on the same failed logic across sessions

Your AI tries an approach. It fails. Next session, context is gone — the AI tries the same approach again. And again. You watch it spin.

**CPFS Runner parses `DO NOT REPEAT` from the log and injects it into every prompt.** The AI sees what already failed and is forced to change track. When all approaches are exhausted, an `AI EXHAUSTED` declaration fires an escalation event so you can step in with new direction.

---

## You're the Director

CPFS Runner changes the relationship. You don't read diffs, hunt for scope violations, or catch the AI retrying last week's failed fix. You say **what to fix** and **how to verify**. The AI codes. CPFS Runner enforces the guardrails — scope, verification, regression, no-repeat — by technical limitation, not by hoping the AI remembers. You confirm pass when the evidence is real. That's what makes it possible to program without programming: the guardrails ensure the AI actually proved its work before it claims done.

No cloud. No account. No telemetry. Everything runs locally in your repo.

---

## Proof: [stl2cad.com](https://stl2cad.com) was built with CPFS Runner — zero coding, under 30 days

[stl2cad.com](https://stl2cad.com) is a live paid service that converts mechanical STL uploads to STEP CAD files — detecting flat faces, holes, cylinders, and edges from a triangle mesh and reconstructing them as editable STEP geometry. It handles brackets, housings, plates, mounts, and fixtures. It is not a general-purpose mesh-to-CAD engine: artistic meshes, organic sculpts, and 3D scans are out of scope, files are capped at ~250,000 triangles (larger files are simplified on upload), and curved regions export as boundary outlines you finish in CAD rather than full parametric surfaces.

**It was written in under 30 days. With zero coding.** The owner directed the AI to build it using the CPFS protocol — every feature logged, every change scoped, every pass verified against real STL test parts. The owner never wrote or read a line of code. CPFS Runner enforced the discipline so the AI couldn't fake "done."

That's not a demo. That's a production product with paying customers — built by a non-programmer directing AI, with CPFS Runner as the guardrail. [See it live at stl2cad.com](https://stl2cad.com).

**CPFS Runner was also programmed using CPFS Runner.** The extension enforced its own development discipline — every feature logged, every change scoped, every pass verified. The tool built itself. That's the deepest proof it works: we didn't ship a discipline tool that we wrote without discipline.

---

## Install

| Path | How |
|------|-----|
| **Marketplace** | Search **CPFS Runner** by **ragbox** — [marketplace page](https://marketplace.visualstudio.com/items?itemName=ragbox.cpfsrunner) |
| **Direct download** | [VSIX](https://ragbox.llc/tutorials/cpfs-runner-latest.vsix) · [MCP tarball](https://ragbox.llc/tutorials/cpfs-mcp-latest.tgz) · `npm install -g cpfs-mcp` |
| **VSIX** | **Extensions → ⋯ → Install from VSIX…** → choose the `.vsix` file → **Reload Window** |

After install, reload the window. You should see a **CPFS** item in the status bar.

---

## Quick start

1. **Open your project folder** in Cursor / VS Code.
2. **`Ctrl+Shift+P` → `CPFS: Enable Workspace`** — creates `logs/development_history/` and `.cpfs/config.json`. (After the first time, CPFS auto-enables on workspace open.)
3. **Confirm it works** — run **`CPFS: Run Install Self-Test`** (see below).
4. **Work in chat** — say **what to fix** and **how to verify** in one message. CPFS matches or creates the feature log; the status bar shows the active feature + attempt #.
5. **Validate** — `CPFS: Validate Attempt` runs your test command and records the real outcome.
6. **Confirm pass** — `CPFS: Confirm Pass` marks the feature COMPLETE (only after verification is agreed and checks pass).

---

## Confirm your install: the self-test

**`CPFS: Run Install Self-Test`** runs a one-click check that the extension is wired up: workspace folder, CPFS enabled, config, logs folder, zones, and your test command.

- **PASS** → a green toast and a report in the **CPFS Self-Test** output panel. You're ready.
- **FAIL** → open the **CPFS Self-Test** output panel; each failing line says what's wrong. Fix the items marked `FAIL`, then run the self-test again.
- **To run it again any time:** `Ctrl+Shift+P` → `CPFS: Run Install Self-Test`.

On a fresh install, the self-test runs automatically once and shows you the result.

---

## Where to read more

- **CPFS protocol (free/open):** https://ragbox.llc/tutorials/cpfs.html
- **In-extension reference manual (every command, setting, file):** run **`CPFS: Open Reference Manual`**
- **In-extension quick start:** run **`CPFS: Open Quick Start`**
- **Dashboard:** `CPFS: Open Dashboard` — hero next-action, timeline, verify status, stats.

---

## Settings that matter

| Setting | What it does | Default |
|---------|--------------|---------|
| `cpfsRunner.testCommand` | Command `Validate Attempt` runs (e.g. `npm test`) | — |
| `cpfsRunner.scopeMode` | `warn` / `soft-block` / `strict` / `off` | `soft-block` |
| `cpfsRunner.openDashboardOnStartup` | Open dashboard on workspace open | `false` |

---

## Privacy

All data is written only to `.cpfs/` and `logs/development_history/` under your opened project folder. No network calls are made for CPFS features. Your validation commands run locally on your machine.

---

## License

**CPFS Runner** is **proprietary commercial software**. Use requires a paid subscription (or a written pre-release evaluation authorization). See [LICENSE](./LICENSE).

The free **[CPFS methodology](https://ragbox.llc/tutorials/cpfs.html)** on ragbox.llc is separate — it describes how to work; it does **not** grant rights to copy or redistribute CPFS Runner.

# CPFS Runner — Reference Manual

> **For:** CPFS Runner <!--CPFS_VERSION-->1.1.8<!--/CPFS_VERSION-->
> **Who this is for:** Power users and support. This is a lookup list, not a how-to. For a first walkthrough see `docs/QUICK_START.md`; for the free rules see <https://ragbox.llc/tutorials/cpfs.html>.

CPFS Runner is the editor add-on that makes the AI follow the [CPFS rules](https://ragbox.llc/tutorials/cpfs.html) for real — one fix at a time, scope limits, agreed checks before success, and a trail you can read kept locally in your project. This manual lists every command, setting, file, and gate in this release.

---

## 1. Install

| Way | How |
|------|-----|
| **VSIX file** | `Extensions → … menu → Install from VSIX…` and pick the `.vsix` |
| **Local install** | `cpfs-runner/scripts/install-local.sh` then `Developer: Reload Window` |
| **Marketplace** | *(planned)* Search **CPFS Runner** by **ragbox** |

After install, reload the window. A **CPFS** bottom-bar item shows up. Turn CPFS on once per project (see Step 1 of Quick Start): click the **CPFS OFF** indicator, or run **CPFS: Enable Workspace**. Enabling creates `logs/development_history/`, `.cpfs/config.json`, hooks, the mandatory rule, and `.gitignore` entries.

**First run / update behavior:** on a fresh install or any version change, CPFS automatically opens the Quick Start, runs the install self-test (opens `logs/development_history/cpfs-self-test.md` as a tab), and shows a **Reload Window** toast. Click it (or **Developer: Reload Window**) to finish enabling, then enable the workspace.

---

## 2. The workflow (one fix at a time)

1. **Say the fix + how to check it** in Cursor chat (or run **CPFS: Start Feature** / **Start Formal Feature**).
2. CPFS finds or makes a feature log under `logs/development_history/`.
3. The AI works inside **TARGET FILES**. Saves outside scope are flagged or blocked based on `scopeMode`.
4. **Validate Attempt** runs your test command and adds the real result to the log.
5. **Close Attempt as Passed** is **blocked** unless the log has REAL TEST EVIDENCE pointing to a file that really exists. When it passes, CPFS auto-creates a **milestone commit** (clean tree) so every passed fix is a comeback point in git.
6. **Owner closes the job.** The close command depends on the task type (see the table below):
   - **Type 2 (quick fix):** the owner uses **CPFS: Owner Pass (Type 2)** (or the dashboard **Pass** button) — with or without an AI artifact.
   - **Type 3 (formal feature):** the owner uses **CPFS: Confirm Pass** to confirm the AI's pass and mark the feature **COMPLETE**.

If the AI is stuck it writes `### AI EXHAUSTED`; CPFS shows **Acknowledge AI exhausted** so you pick the next path.

### Task types

| Type | When | Close |
|------|------|-------|
| **Type 1 — Chat** | Just talking, no code change | Not logged as a job |
| **Type 2 — Quick fix** | Small fix, success said out loud in chat | Auto-closes on owner confirm; real-test gate enforced |
| **Type 3 — Formal feature** | Structured feature with clear goals | Per-goal verdict + gate; **Upgrade to Type 3** promotes a running job |

---

## 3. Commands (Command Palette → `CPFS:`)

> Command titles below are exactly what the palette shows after you type `CPFS:`.

### Lifecycle
| Command | Purpose |
|---------|---------|
| **Enable Workspace** | Make `logs/development_history/` + `.cpfs/config.json`, install hooks + mandatory rule; asks for the dev zone (defaults to `bench`). Turns on by itself on later opens |
| **Start Feature** | Make a feature log by hand (name, target files, success goals) |
| **Start Formal Feature** | Open the structured Type-3 form (goals, tests, target files) |
| **Continue Feature** | Resume the active feature in chat |
| **Upgrade to Type 3 (Formal Feature)** | Promote a running job to a formal feature |
| **Edit Criteria** | Edit the success criteria of a formal (Type 3) feature |
| **Validate Attempt** | Run the test command from `.cpfs/config.json` (falls back to the VS Code `cpfsRunner.testCommand` setting); add real result to the log |
| **Close Attempt as Passed** | End the attempt as SUCCESS (gated on real-test proof); on pass, auto-creates a milestone commit |
| **Close Attempt as Failed** | End the attempt as FAILED (retry/suspend/close) |
| **Confirm Pass** | Owner confirms the AI's pass → marks feature COMPLETE |
| **Reject Pass** | Owner rejects the AI's claimed pass |
| **Owner Pass (Type 2)** | Owner closes a Type-2 job directly (with AI artifact) |
| **Owner Pass — No AI Artifact (Type 2)** | Owner closes a Type-2 job with no AI artifact |
| **Fail Retry / Fail Suspend / Fail Close** | Pick the next path after a failed attempt |
| **Re-run Retest (regression check)** | Re-run frozen retest steps for this task |
| **Analyze Legacy Crossings (cover changed existing code)** | Find changed existing code touched by this fix |

### Scope & context
| Command | Purpose |
|---------|---------|
| **Review Out-of-Scope Edits** | Settle flagged out-of-scope saves (Revert / Keep / Keep+scope) |
| **Add File to TARGET FILES** | Add the active file to scope |
| **Dismiss Pending Scope Reviews** | Clear fake pending reviews (e.g. after reload) |
| **Close Scope Diff Tabs** | Close diff tabs opened by scope review |
| **Manage Active Zones** | Edit the zone map (groups of allowed files) |
| **Add File to Zone / Remove File from Zone** | Zone membership |
| **Refresh AI Context** | Rebuild the AI context block from the current log |
| **Install Cursor Hooks** | Auto-refresh AI context before every prompt (a good idea) |
| **Refresh Status Bar** | Force a bottom-bar refresh |

### Onboarding
| Command | Purpose |
|---------|---------|
| **Generate Onboarding Document** | Full repo audit → onboarding doc |
| **Re-Audit Repository (Onboarding)** | Re-run the onboarding audit |
| **Open Onboarding Document** | Open the linked onboarding doc |
| **Link Onboarding Document** | Attach an existing onboarding doc |
| **Record AI Audit Onboarding** | Stamp an AI-performed audit |
| **Verify Onboarding Document** | Verify the onboarding doc |
| **Approve Onboarding Document** | Approve the onboarding doc |

### Reference images
| Command | Purpose |
|---------|---------|
| **Set Reference Images Folder** | Pick a folder (inside the workspace) for check screenshots; writes `referenceImagesPath` |
| **Open Reference Images Folder** | Open that folder |

### Checkpoints, lint, heuristics
| Command | Purpose |
|---------|---------|
| **Restore Attempt Checkpoint** | Restore the git snapshot taken before this attempt (checkpoints use git stash refs, not a folder) |
| **Toggle Checkpoint Before Attempt** | Turn checkpointing on/off |
| **Toggle Auto-Restore on Test Fail** | Auto-restore on a failed test |
| **Toggle Lint After Edit** | Turn lint after edits on/off |
| **Set Lint Command** | Set the lint command |
| **Check Heuristics — Scan Folder…** | Run the rule scanner on a folder |
| **Reopen Last Manual Heuristic Report** | Open the last manual report |
| **Open Automatic Heuristic Report** | Open the automatic report |
| **Acknowledge Heuristic Review** | Dismiss a rule review |
| **Acknowledge Escalation** | Dismiss an escalation |

### Dashboard, rules, licensing
| Command | Purpose |
|---------|---------|
| **Open CPFS Dashboard** | Open the control panel |
| **Toggle Dashboard Jobs/Settings View** | Legacy view toggle — the dashboard now always shows Settings as a collapsible section alongside the jobs browser, so this command has little visible effect |
| **Toggle CPFS On/Off** | Pause/resume CPFS |
| **Install Mandatory Rules** | Write the mandatory `.cursor/rules/cpfs-mandatory.mdc` rule |
| **Export Report** | Make a shareable HTML summary |
| **Open Feature Log** | Open the active feature's log |
| **Agree Verification in Chat** | Propose VERIFY BY in chat |
| **Sync Verification Agreement** | Pull the agreed VERIFY BY into the log |
| **Run All Checks** | Run command/file check types |
| **Run Install Self-Test** | Run the install self-test (writes `logs/development_history/cpfs-self-test.md` and opens it) |
| **Open Unlike-Inputs Probes (§13)** | Open the §13 stress probes |
| **Enter License Key** | Paste a signed license token (paid) |
| **Remove License Key** | Drop back to the free tier |
| **Show License Status** | Show current entitlement + expiry |
| **Open Reference Manual** | Open this document |
| **Open Quick Start** | Open the in-extension Quick Start guide (`docs/QUICK_START.md`) |
| **CPFS Status Bar Menu** | Quick-pick menu of common actions (dashboard, refresh AI context, etc.) |

---

## 4. Settings

CPFS settings live in **`.cpfs/config.json`** (per project). The dashboard **Settings** section reads and writes that file. A few settings also have a VS Code configuration fallback (`cpfsRunner.*`) so they work before the project is enabled.

| Setting | Type | Default | Purpose |
|---------|------|---------|---------|
| `testCommand` | string | `""` | Shell command run by **Validate Attempt** (e.g. `npm test`). Stored in `.cpfs/config.json`; VS Code `cpfsRunner.testCommand` is a fallback |
| `scopeMode` | enum | `warn` | `warn` / `soft-block` / `strict` / `off` — how out-of-scope saves are handled |
| `openDashboardOnStartup` | boolean | `false` | Open the dashboard on project load. **Note:** the dashboard **saves** this to `.cpfs/config.json`, but the **startup hook reads only the VS Code setting** `cpfsRunner.openDashboardOnStartup` — so for it to take effect at startup, set the VS Code setting (or the dashboard toggle that writes it), not just the config file |
| `developmentZone` | enum | `generic` | `bench` / `extension` / `prod` / `generic` — default TARGET FILES per zone |

The dashboard **Settings** section exposes many more toggles (hooks, checkpoints, lint, heuristics, time-box, complete-sentiment, milestone commit, legacy-crossings auto-analysis, etc.). This table lists only the core ones; open the dashboard Settings section to see all of them.

**scopeMode behavior:**
- **warn** — toast only; saves go through.
- **soft-block** — Confirm/Validate/End Success blocked until out-of-scope reviews are settled.
- **strict** — saves outside TARGET FILES blocked right away.
- **off** — no scope enforcement.

---

## 5. Files & folders CPFS creates

| Path | Purpose |
|------|---------|
| `logs/development_history/<feature>.log` | The feature log — tries, checks, decisions, the trail you can read |
| `.cpfs/config.json` | Per-project config (enabled, scope mode, zones, onboarding link, test command, toggles) |
| `.cpfs/active.json` | Active session pointer (gitignored) |
| `.cpfs/events.jsonl` | Append-only event stream (gitignored) |
| `.cpfs/dashboard_view.json` | Persisted Jobs/Settings view choice (legacy — the dashboard renders both together) |
| `.cursor/rules/cpfs-mandatory.mdc` | Always-applied mandatory CPFS rule (written on enable) |
| `.cursor/rules/cpfs-active.mdc` | Live AI-context rule (generated at runtime when a job is active) |
| `.cursor/hooks.json` | Cursor hooks (auto-refresh AI context before each prompt) |
| `.gitignore` | CPFS appends entries for `.cpfs/active.json` and `.cpfs/events.jsonl` |

Checkpoints are **not** a folder — they use git stash refs stored on the active session.

The feature log is the source of truth. It is plain text and meant to be read; the dashboard is a view over it.

---

## 6. Gates (what CPFS enforces)

### Real-test-before-pass gate
**Close Attempt as Passed** is blocked unless the current try recorded REAL TEST EVIDENCE in the log — a line that points to a file that really exists on disk. Compile or unit tests alone are **not** enough to pass. The AI must run the code like a human would and save the real output.

### Milestone commit at End Success
When an attempt is closed as Passed, CPFS auto-commits all changes (clean tree) so every passed fix is a real comeback point in git history. Done in code by the extension — not left to the AI. Can be turned off in the dashboard Settings (**Milestone commit at End Success** toggle).

### Retest recipes (regression)
At pass time CPFS freezes a retest recipe (the exact command + expected result) for the task. If later work touches the same code, those recipes re-run by themselves; completion is blocked until any regressions are settled (**Re-run Retest**).

### Scope gate
Saves outside TARGET FILES are flagged or blocked based on `scopeMode`. Out-of-scope edits can be reviewed (Revert / Keep / Keep+scope).

### DO NOT REPEAT
The feature log records failed tries. CPFS puts DO NOT REPEAT into the AI's context on every prompt so the same failed fix is not tried again.

### Legacy blast-radius
**Analyze Legacy Crossings (cover changed existing code)** finds changed existing code touched by a fix and re-checks affected recipes.

### Destructive content guard
A filesystem watcher that warns loudly after a tracked file shrinks a lot. It's **non-blocking** (a notification, not a modal) so you can keep chatting with the AI before deciding. Buttons: **Restore from git** (reverts the file to the last commit), **Keep as-is**, or **Decide later** (snoozes the warning).

---

## 7. Licensing & tiers

CPFS Runner is **proprietary**. Enforcement depends on the installed license:

| Tier | Enforcement | How obtained |
|------|-------------|--------------|
| **Full** (paid) | All gates block | A signed license token — **CPFS: Enter License Key** |
| **Free — enforced** | All gates block | No key; first 10 completions each month |
| **Free — exhausted** | Managed mode paused (no new managed tasks); safety features stay on by default | No key; after 10 enforced completions this month (resets monthly) |

License tokens are **Ed25519-signed** and checked **offline** — no phone-home, no account, no cloud. The signing key never leaves the publisher, so a token can't be forged. Run **CPFS: Show License Status** any time to see your seat + expiry. Lost your key? Email <hello@ragbox.llc>.

---

## 8. The dashboard

**CPFS: Open CPFS Dashboard** opens one control panel:

- **Top bar** — CPFS on/off toggle, the running job, the version badge.
- **Settings** (collapsible section) — your CPFS settings; Save writes `.cpfs/config.json`.
- **Work Queue** — jobs that need attention (failed within 7 days, or new today).
- **All-jobs browser** — every feature log; select a job to see its detail tabs:
  - **Requests** — the opening request + current attempt status.
  - **Criteria** — per-goal ✓/?/✗ and PASS/FAIL (Type 3).
  - **Tests** — structured tests (Type 3 only).
  - **Work** — append-only AI work passes.
  - **Communication** — owner ↔ AI messages.
  - **Attempts** — attempt timeline in order.
  - **Scope** — TARGET FILES + out-of-scope edits.
  - To make a different job the active one, click **Resume** on its row — the **Resume** button only appears for jobs in **Suspended** or **Idle** state (not Running/Passed/Failed; suspend or abandon those first if needed).
- **CPFS health** — AI Context freshness, Compile status, Snapshot status.
- **Onboarding** — onboarding doc status + actions (Open / Generate / Verify / Approve / Re-audit / Attach / Mark AI-audit).
- **Quick actions** — common commands: **Run compile check**, **Run all checks**, **Open feature log**, **Reference images**, **Start formal feature**, **Heuristic scan**, **Install self-test** (the first four only appear when a job is active).

Use the dashboard to glance, not to argue fixes. Fixes happen in chat.

---

## 9. What CPFS Runner does NOT do (honest up front)

- It is for **code**, where results are clear-cut and a check can really run. It does not catch made-up facts in plain writing.
- It can't force "smallest change" or "one guess per try" by itself — those are AI habits; checking them after the fact would be a brittle rule (which the tool's own §13 rule bans).
- It doesn't drive a browser for visual checks — you describe those steps (or use the reference-images folder for screenshot proof).
- Cursor / VS Code only.

---

## 10. Troubleshooting

| Symptom | Fix |
|---------|-----|
| No CPFS bottom-bar item | Run **CPFS: Enable Workspace**; reload the window |
| Scope toast on a file I expect to edit | **Add File to TARGET FILES**, or **Add File to Zone** |
| "CSRF token missing" (web signup, not the extension) | Reload the page and retry — the web signup form is a public guest endpoint |
| License key rejected | **CPFS: Show License Status** for the reason; re-paste from your delivery email |
| Stale "in progress" job after reload | **Dismiss Pending Scope Reviews**; or abandon from **All jobs** |
| Dashboard shows old job | In the **All-jobs** browser, find the job you want and click **Resume** on its row (only shows for **Suspended**/**Idle** jobs — suspend or abandon the current job first if needed) |
| Mandatory rules missing banner | Run **CPFS: Enable Workspace** (or **Install Mandatory Rules**); reload Cursor |

---

## 11. Going deeper

- **CPFS rules (free/open):** <https://ragbox.llc/tutorials/cpfs.html>
- **Full reference (web):** <https://ragbox.llc/tutorials/cpfs-runner-full-reference.html>
- **Quick start:** `docs/QUICK_START.md` (also via **CPFS: Open Quick Start**)

---

*Reference manual for CPFS Runner. Re-run `node scripts/stamp-doc-version.mjs` after a version bump to keep the stamp current.*

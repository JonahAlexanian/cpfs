# CPFS Runner dashboard — button reference

What every button in the **Onboarding** section and the **Quick actions** section does, plus the two top-bar toggles. Accurate as of `1.0.0-rc.115`.

> The dashboard also has a **CPFS health** row (AI Context / Compile / Snapshot cards) and the **All jobs** table + tabbed detail pane. Those are covered in `docs/DASHBOARD_GUIDE.md`. This file is only about the clickable buttons/toggles.

---

## Top bar — two toggles

| Control | Command | What it does |
|---|---|---|
| **CPFS ON / OFF** | `cpfs-runner.toggleCpfs` | Turns CPFS on or off for this workspace. When OFF (paused) the dashboard stays visible but CPFS stops watching edits, gating saves, and tracking attempts. The status-bar indicator mirrors this. |
| **Snapshot ON / OFF** | `cpfs-runner.toggleCheckpointEnabled` | Turns the **git checkpoint** on or off (`config.checkpointEnabled`, default ON). When ON, a `git stash create` snapshot of the working tree is captured at the start of each attempt, so a failed attempt can be restored. The snapshot status shows in the CPFS health row. |

---

## Onboarding section

All onboarding abilities live here (none are duplicated in Quick actions). The section shows a status card (OK / Missing / Failed / Review / Stale), an optional alert for the current issue, and a row of **seven** action buttons. The button the current state most needs is highlighted blue.

| Button | Command | When enabled | What it does |
|---|---|---|---|
| **Open** | `cpfs-runner.openOnboarding` | When an onboarding doc is linked | Opens the linked onboarding document in the editor. |
| **Generate** | `cpfs-runner.generateOnboarding` | Always | Runs a full repo scan and writes a fresh onboarding document (`generateOnboardingDocument`). Asks for the output path (defaults to `project_status/REPO_STRUCTURE_ONBOARDING.md`), confirms overwrite if it exists, links it in config as `source: ai_audit` (draft, not approved), opens it, and prints a verify report to the **CPFS Onboarding** output. Offers to copy an **AI fill-in prompt** to the clipboard so you can paste it in chat and have the AI fill workflow details. |
| **Verify** | `cpfs-runner.verifyOnboarding` | When a doc is linked | Checks the linked onboarding doc against CPFS §14 + the repo, and checks watched paths for staleness. Persists the verify result (status, error/warning counts, issues) on the onboarding record. Prints a full report to the **CPFS Onboarding** output. If gaps are found it offers **Re-audit (AI)**; if only warnings it offers **Approve onboarding**. |
| **Approve** | `cpfs-runner.approveOnboarding` | When a doc is linked | Owner approval. Sets the onboarding doc's frontmatter `status: approved`, stamps `approved: true` + a fresh verify on the config record, and marks the doc ready for **Start Feature**. Confirms first. Reports any remaining required gaps. |
| **Re-audit** | `cpfs-runner.reAuditOnboarding` | When a doc is linked | Runs a true onboarding audit (`auditOnboarding`): deterministic checks, mechanical auto-fixes, and recommendations; scaffolds the doc from the template if it does not exist. Marks the doc `source: ai_audit` (draft). Opens the doc and prints the audit report to the **CPFS Onboarding** output. Offers to copy an **AI fix prompt** to the clipboard so the AI can fill the remaining gaps. |
| **Attach** | `cpfs-runner.attachOnboarding` | Always | Opens a file picker so you can link an **existing** markdown file (inside the workspace) as the onboarding doc. Records it as `source: user_provided` and (for a user-picked file) `approved: true`, then automatically runs **Verify** on it. Use this when you already wrote/have an onboarding doc and just want to point CPFS at it. |
| **Mark AI-audit** | `cpfs-runner.markOnboardingAiAudit` | When a path is linked | Marks the already-linked onboarding path as `source: ai_audit` with `approved: false` (draft). Use this after the AI creates/updates the onboarding doc through chat so CPFS knows it is an AI-generated draft that still needs your review and **Approve**. Does **not** pick a file — it re-tags the current path. |

**Typical flow:** Generate (or Attach) → review the doc → Verify → Approve. When watch_paths drift later, the card shows Stale and **Re-audit** is highlighted.

---

## Quick actions section

| Button | Command | Shown when | What it does |
|---|---|---|---|
| **Run compile check** | `cpfs-runner.validateAttempt` | A job is active | Runs the configured test/compile command (`config.testCommand`, e.g. `npm run compile`) for the active attempt. First runs preflight gates: escalation pending, soft-block pending, target files present, outcome-acceptance gate, AI context fresh, verify agreement. Shows a preflight summary and asks to confirm. Appends a `### VALIDATION` block to the feature log with pass/fail/**NOT PROVEN** verdict. On pass: stamps `VERIFIED-AUTOMATED` and (if enabled) runs a §13 heuristic review. On fail: if `checkpointAutoRestoreOnTestFail` is on and a checkpoint exists, restores the changed files from the snapshot and reports how many were reverted. |
| **Run all checks** | `cpfs-runner.runAllChecks` | A job is active | Runs the command/file verify checks for the agreed outcomes (Option B partial) against the active feature log — checks whether referenced files/commands exist. Reports a pass/fail toast and detail. Lighter than a full validate; does not append a validation block. |
| **Open feature log** | `cpfs-runner.openFeatureLog` | A job is active | Opens the active job's feature log file in the editor. |
| **Reference images** | `cpfs-runner.openReferenceImagesDir` | Always | Reveals the reference-images folder in your OS file manager (config `referenceImagesPath`, default `images/`). If the folder does not exist it offers to create it. Used by Mode 3 formal features that reference screenshots. |
| **Start formal feature** | `cpfs-runner.startFormalFeature` | Always | Opens the **formal feature form** (Mode 3) webview where you define a structured CPFS feature: problem statement, optional reference image, target files, and a structured test list. Submitting scaffolds a feature log and activates the job. This is the "create a Type 3 job" entry point. |
| **Heuristic scan** | `cpfs-runner.checkHeuristicsFolder` | Always | Manual §13 heuristic scan. Picks a folder (browser picker) and runs `runHeuristicScan` over it, writes the last-scan record, opens the heuristic report panel, and fires a dashboard flash. Use this to proactively scan a folder for §13 risk signals (single-sample verify, hardcoded thresholds, etc.) outside of a validate pass. |
| **Install self-test** | `cpfs-runner.runInstallSelfTest` | Always | One-click check that the CPFS VSIX is installed and the workspace is wired up correctly (config, onboarding, hooks). Prints results to the **CPFS Self-Test** output. Run this after installing/upgrading the extension or in a fresh workspace. |
| **Open full command palette…** | `cpfs-runner.showStatusBarMenu` | Always | Opens the CPFS quick-pick menu (same as the status bar): Open Dashboard, Refresh AI context, Open feature log, Continue with prompt, Enable CPFS workspace — plus access to every registered CPFS command via VS Code's command palette. Use this when the button you need is not on the dashboard. |

---

## Notes

- Buttons that need an active job (Run compile check, Run all checks, Open feature log) only appear when a job is running.
- The Onboarding **Open / Verify / Approve / Re-audit** buttons are disabled (greyed out) until a doc is linked; **Generate** and **Attach** are always available because they create/link a doc.
- The snapshot toggle writes `config.checkpointEnabled`; the actual snapshot is a `git stash create` ref (safe — it does not advance your branch or commit your pending work). It is visible in the CPFS health row as "Snapshot: Taken · attempt N" with a **Restore** action.

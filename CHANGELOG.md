# Changelog

Public release notes for CPFS Runner. (Internal development history is kept separately and is not shipped.)

## 1.1.14 — 2026-07-22

- **Fixed: dashboard Artifacts tab and retest view CSP hardening.** Content-Security-Policy headers on the dashboard Artifacts tab and retest view were tightened so inline content loads safely without relaxed script-src.
- **Added: multi-IDE agent hooks.** Pre-commit / pre-push scope + heuristic hooks now install and fire across multiple AI IDEs (Cursor, Windsurf, Cline, Roo), not just VS Code.
- **Hardened: free-tier enforcement.** The free-tier exhausted gate and anti-circumvention self-heal were hardened against activation-time bypasses.

## 1.1.12 — 2026-07-21 (Marketplace release)

- **Marketplace release of the WASM-sole-core build + dashboard work.** This is the version currently live on the VS Code Marketplace.
- **Changed: WASM is the sole core implementation** (see 1.1.10). No TypeScript fallback ships.
- **Added: dashboard Artifacts tab** with no-active-state handling and lifecycle IDLE→SUSPENDED, plus state-derivation sync.

## 1.1.10 — 2026-07-21

- **Changed: WASM is now the sole core implementation — all TypeScript fallbacks removed.** License, free-tier, entitlement, verify-gate, overlap, and claim-guard logic all run only in the compiled WebAssembly core. There is **no pure-TypeScript fallback**: if the WASM module fails to load, every core function throws and the extension stops. No source backstop ships in any distribution.

## 1.1.3 — 2026-07-19

- **Added: self-dogfood proof point** to the marketplace listing and homepage.

## 1.1.2 — 2026-07-19

- **Fixed: "just now" label in the All-jobs Activity column** so recent activity timestamps render correctly.

## 1.1.1 — 2026-07-19

- **Fixed: honest stl2cad proof point** — corrected the proof-point copy to be accurate about limitations.

## 1.1.0 — 2026-07-19

- **Added: WASM core + new hero.** Hero updated to "Program without Programming with AI"; the compiled WebAssembly core is the enforcement engine.

## 1.0.0 — 2026-07-19

- **First clean public version.** Repository URL pointed at the public repo; version bumped to a clean public release number. WASM core baseline.

## 1.0.0-rc.185 — 2026-07-17 (beta)

- **Fixed: inform-only modal now shows on every pass path, not just "Close attempt as passed".** The blocking "free / inform-only: enforcement is off" modal dialog (with OK button) was only wired into "Close attempt as passed" (rc.184). The dashboard's "Pass (no AI artifact)" (Type 2) and "Confirm pass" (Type 3) buttons passed silently in inform-only mode with no warning. Both now show the same blocking modal before the pass completes, so every pass path is consistent. Full and free-enforced modes are unchanged.

## 1.0.0-rc.184 — 2026-07-17 (beta)

- **Changed: free-tier inform-only warning is now a modal dialog.** The "free / inform-only: real-test gate would block, but enforcement is off" message was a toast that auto-closed and overlapped other toasts. It is now a blocking modal dialog with an **OK** button you must click — and the pass is recorded only after you acknowledge, so toasts no longer pile up.

## 1.0.0-rc.183 — 2026-07-17 (beta)

- **Added (test only): "CPFS: Dev: Set Free-Tier Counter" command.** Lets the owner jump the free-tier completion counter to an arbitrary value to test the inform-only gate without 5 manual passes. Test scaffolding — labeled "Dev / TEST ONLY"; remove or gate before stable 1.0.0.

## 1.0.0-rc.182 — 2026-07-16 (beta)

- **Fixed: free-tier counter bypass via "Pass" / "Confirm pass".** The owner-confirm close buttons (Type 2 "Pass" and Type 3 "Confirm pass") closed a job as passed but did not increment the free-tier completion counter, so a user could exceed the 5 free enforced completions by using those buttons instead of "Close attempt as passed". Both buttons now consume a free-tier slot when the entitlement is `free-enforced` (matching "Close attempt as passed"). Licensed (`full`) and `free-inform-only` modes are unaffected — only the free tier counts.

## 1.0.0-rc.181 — 2026-07-16 (beta)

- **Fixed: "Close attempt as passed" block dialog appearing twice on a single click.** The dashboard's global click-wirer was binding a second listener to buttons inside the All-jobs detail pane (which already wires its own), so one click fired the command twice. The global wirer now skips buttons inside the job detail pane. No other buttons are affected.

## 1.0.0-rc.180 — 2026-07-16 (beta)

- **Docs brought fully in sync with code** (8 fixes from a read-only audit; docs only, no behavior change):
  - **REFERENCE §4** — `openDashboardOnStartup`: clarified the startup hook reads the VS Code setting `cpfsRunner.openDashboardOnStartup`, not `.cpfs/config.json` (the dashboard saves to the config file, but startup ignores it).
  - **QUICK_START + REFERENCE** — **Resume** button now documented as appearing only for jobs in **Suspended**/**Idle** state (not any job row).
  - **QUICK_START + REFERENCE** — Close path now distinguishes **Type 2** (Owner Pass / dashboard Pass) from **Type 3** (Confirm Pass → COMPLETE).
  - **QUICK_START + REFERENCE** — Added the first-run/update flow: auto-open Quick Start, quiet self-test, **Reload Window** toast. Fixed the Quick Start intro (the window is *not* yet reloaded when the doc auto-opens).
  - **REFERENCE §6** — Destructive content guard now lists the real buttons: **Restore from git / Keep as-is / Decide later** (non-blocking).
  - **REFERENCE §3** — Added the two undocumented commands: **Open Quick Start** and **CPFS Status Bar Menu**.
  - **REFERENCE §8 + QUICK_START** — Quick-action labels now match the code exactly (**Run compile check**, **Run all checks**, **Open feature log**, **Reference images**, etc.).
  - **QUICK_START Step 5** — Clarified the self-test report opens without grabbing focus (rc.179 `preserveFocus` change) so the Reload toast stays visible.

## 1.0.0-rc.179 — 2026-07-16 (beta)

- **Fixed the rc.175 regression that buried the "Reload Window" button.** Opening the self-test report file (`showTextDocument`) was stealing focus and covering the reload toast, so the reload button vanished almost instantly. Now the report file opens with `preserveFocus: true` (a real tab, but no focus theft), and the first-run self-test runs quietly (no competing self-test toast) so the "Reload Window" toast is the one visible button — like it was before rc.175. The self-test still runs and its report file still opens as a tab.

## 1.0.0-rc.178 — 2026-07-16 (beta)

- **Docs resynced to code (code is source of truth).** A read-only audit of QUICK_START.md and REFERENCE.md against the actual extension source found 12 discrepancies where the docs described behavior the code no longer had. Both docs rewritten to follow the code:
  - **Dashboard Jobs/Settings "toggle button in the top bar" removed** — that button doesn't exist. Settings is a collapsible section; described accurately.
  - **Milestone commit attribution fixed** — it happens on **Close Attempt as Passed** (End Success), not on Confirm Pass. Confirm Pass now correctly described as marking COMPLETE.
  - **Removed the "Big button / hero" claim** — that hero card was removed from the dashboard; replaced with the real layout (top bar, Settings section, Work Queue, All-jobs browser with detail tabs, CPFS health, Onboarding, Quick actions).
  - **Enable Workspace corrected** — clicking CPFS OFF enables with a default zone (no picker); `CPFS: Enable Workspace` shows a zone picker (default `bench`). Enable also installs hooks + mandatory rule + gitignore entries, not "two things."
  - **Removed `.cpfs/checkpoints/`** — that folder doesn't exist; checkpoints use git stash refs.
  - **`.cursor/rules/*.mdc` → `cpfs-mandatory.mdc` + `cpfs-active.mdc`** — the actual rule filenames.
  - **Command labels fixed** — `CPFS: Open CPFS Dashboard` (was `Open Dashboard`); `Analyze Legacy Crossings (cover changed existing code)` (was just `Analyze Legacy Crossings`).
  - **Reference images button** — only appears when a job is active (was implied "anytime").
  - **Destructive content guard** — described as a filesystem watcher (was "a save guard and a disk-shrink watcher").
  - **Settings §4** — clarified that settings live in `.cpfs/config.json` (dashboard writes it), with `cpfsRunner.*` as VS Code fallback; noted the panel exposes more toggles than the core table.
- **Second pass (after read-only re-audit):** fixed 6 more — added `.gitignore` to the enable list; marked `Toggle Dashboard Jobs/Settings View` + `.cpfs/dashboard_view.json` as legacy (dashboard renders both together); replaced the non-existent "Work → Switch job" with the real **Resume** button; corrected the detail-tab names to the real 7 (Requests, Criteria, Tests, Work, Communication, Attempts, Scope); fixed destructive-guard timing ("after a file shrinks", not "about to"); added the omitted **Edit Criteria** command; noted the milestone commit is toggleable in Settings.

## 1.0.0-rc.177 — 2026-07-16 (beta)

- **Quick Start now documents the real status-bar affordances** (verified against `statusBar.ts` + `toggleCpfs.ts`): Step 1 leads with clicking the **CPFS OFF** indicator in the status bar to enable (runs the full scaffold), with the Command Palette as an alternative; Step 2 leads with clicking the **CPFS** status-bar item to open the dashboard. Added a note that clicking **CPFS ON** pauses CPFS (click again to resume). Previous version led only with Command Palette routes and missed these faster clicks.

## 1.0.0-rc.176 — 2026-07-16 (beta)

- **Fixed the chicken-and-egg in Quick Start** — the in-extension Quick Start is read *after* the extension is installed and reloaded, so "Install the extension" and "Reload Window" were nonsense steps. Removed them. The guide now opens with "You're reading this from inside CPFS Runner — so it's already installed and reloaded" and starts setup at **Enable Workspace** (the first thing you do after install). Steps renumbered 1–6: Enable Workspace → Open Dashboard → Set test command → Set reference images folder → Self-test → Start working in chat.

## 1.0.0-rc.175 — 2026-07-16 (beta)

- **Self-test results now saved to a file and opened in the editor** — the report is written to `logs/development_history/cpfs-self-test.md` and opened as a real editor tab. A tab stays put (unlike a toast or an output channel that gets buried when several startup tasks step over each other), so you can always find the results. The "where to go next" guidance (docs + reload) is folded into the same file. The output channel is still mirrored as a backup. Quick Start Step 7 updated to point at the file.

## 1.0.0-rc.174 — 2026-07-16 (beta)

- **Quick Start is now a real guided walkthrough** — every required setup step (install, reload, enable workspace, open dashboard, set test command, set reference images folder, run self-test) with the exact clicks, in order. Optional fluff removed; required setup only.
- **Self-test output now force-reveals** — changed the CPFS Self-Test output channel from `show(true)` (preserve focus, often stays hidden in Cursor) to `show(false)` so the panel actually opens and you see the pass/fail report. Same fix applied to the first-run "where to go next" block.

## 1.0.0-rc.173 — 2026-07-16 (beta)

- **Plain-language docs** — Quick Start and Reference Manual rewritten at an 8th–10th grade reading level: shorter sentences, common words, jargon explained. Same structure and commands; easier to read.

## 1.0.0-rc.172 — 2026-07-16 (beta)

- **Quick Start auto-opens on every install/update** (not only first install) so you always see where the docs live when you install a new version.
- **Persistent "where to go next" block** — after the self-test, the CPFS Self-Test output panel now also lists where to find the docs (`CPFS: Open Quick Start`, `CPFS: Open Reference Manual`) and how to reload (`Developer: Reload Window`), so the info survives even after the non-blocking reload toast auto-dismisses.

## 1.0.0-rc.171 — 2026-07-16 (beta)

- **Destructive content guard no longer locks the screen** — the "possible accident" prompt is now a non-blocking notification instead of a modal dialog, so you can keep chatting with the AI to decide whether a shrunken file was an intentional change before acting. New **"Decide later"** option dismisses it for a 5-minute grace window while you confer. The guard still fires on the same condition (a git-tracked file drastically shrunk on disk); only the surfacing changed.

## 1.0.0-rc.170 — 2026-07-16 (beta)

- **Public VSIX cleanup** — removed all internal/private documentation from the packaged extension. The Extensions panel README and the changelog are now public-facing only (no internal server paths, internal project names, or internal issue links). Only the Quick Start and Reference Manual ship inside the extension.
- **First-run self-test** — on a fresh install (or an update), the install self-test runs automatically and shows pass/fail with fix guidance in the CPFS Self-Test output panel. Run it any time with `CPFS: Run Install Self-Test`.
- **Quick Start auto-open** — on a first install, the Quick Start guide opens automatically.

## 1.0.0-rc.169 — 2026-07-16 (beta)

- **Reference manual** — new `CPFS: Open Reference Manual` command opens the full in-extension reference (every command, setting, and file).
- **Quick Start** — new `CPFS: Open Quick Start` command; Quick Start auto-opens on first install.
- **First-run experience** — on a fresh install, the install self-test runs automatically and shows pass/fail with fix guidance; a "Reload Window" prompt is surfaced.
- **Beta indicator** — dashboard shows an amber **Beta** pill next to the version until stable 1.0.0.
- **Sticky seats-claimed counter** — the public beta counter only ever increases on first email verification (unsubscribing no longer decrements it).
- **Offline signed license keys** — beta seats are issued Ed25519-signed keys that verify offline in the extension.

## 1.0.0-rc.168 — 2026-07-15

- **License key entry + status** — `CPFS: Enter License Key` and `CPFS: Show License Status`.
- **Free tier** — without a key, the first 5 End-Success completions are fully enforced; after that the extension drops to inform-only mode. Counter persists across reloads.
- **Beta seat keys** — a beta seat key grants full enforcement until its expiry; after expiry the extension drops to free-tier rules.

## 1.0.0-rc.116 — 2026-07-12

- **Dashboard consolidation** — single cockpit: hero next-action, outcomes table, attempt timeline, scope, all-jobs view.
- **Type 2 / Type 3 owner gate** — owner Confirm/Reject pass for human-judge outcomes.
- **Escalation rule fix** — count-based "after 3" removed; exhaustion handling finalized.

## 1.0.0-rc.83 — 2026-07-09

- **Destructive content guard** — if a git-tracked file is accidentally emptied or shrinks to near-zero on disk, a loud modal names the file and offers one-click **Restore from git**.

## 1.0.0-rc.42 — 2026-07-08

- **Cockpit workflow + enforcement updates** — status bar active job/attempt, scope warnings, validate attempt, end success gate.

---

*Earlier pre-beta development builds (rc.1 – rc.41) are not listed here. CPFS Runner is proprietary; see LICENSE.*

# CPFS Dashboard — button guide (cockpit, rc.44+)

**Audience:** Owner / daily user (non-developer).  
**Open dashboard:** Status bar **CPFS** click, or command **CPFS: Open Dashboard**.

One screen, top to bottom. The **blue hero button** is the suggested next step (label changes with your situation). Everything else is on the same page — no tabs, no Command Palette for scope review.

---

## Typical order (most days)

Use when **Blockers** is empty (nothing red/yellow):

| Step | Where | Button | What it does |
|------|-------|--------|--------------|
| **1** | Now | **Start job** or **Switch job** | Pick or change the active feature log. Chat can also attach a job automatically. |
| **2** | Decide | **Edit criteria** | Open success criteria / outcome acceptance (only if Outcomes shows **?** or **✗**). |
| **3** | More | **Continue in chat** | Copy context into chat so the AI continues this job with the right log and scope. |
| **4** | More | **Refresh AI context** | Push latest log, TARGET FILES, and rules into what the AI reads (if Context chip says **stale**). |
| **5** | Chat | *(not a button)* | Describe the fix; agree **how to verify** once in chat so VERIFY BY is written to the log. |
| **6** | More | **Run all checks** | Runs automated command/file checks; updates the Outcomes table. |
| **7** | More | **Run compile check** | Optional build/test — compile signal only, not “feature passed.” |
| **8** | Progress | Read the table | Every row should show **✓** accepts and **PASS** when done. |
| **9** | Decide | **Confirm pass** | Your final yes when AI logged claim pass — marks job **COMPLETE**. |

If **Blockers** shows something, handle that **first** (see below).

---

## Now (top)

This is the glance zone. It tells you what you are working on and the one next action.

| Element | Meaning |
|---------|---------|
| Badge (**IN PROGRESS**, **COMPLETE**, …) | Job lifecycle |
| Feature name + attempt | Active job |
| Log path | Where the feature log lives |
| Status chips | Context, Outcomes, Scope, Verify, Compile, Onboarding, Workflow |
| **Blue hero button** | Suggested next action for the current state |

---

## Blockers (handle first)

Visible section. If nothing needs action, it says **No blockers right now**.

| Button | When | What it does |
|--------|------|------------|
| **Acknowledge AI exhausted** | AI declared stuck | Pick next path; required before Confirm pass. |
| *(message)* | Out-of-scope pending | Use **Scope → Out-of-scope edits** (Revert / Keep). |
| **Refresh AI context** | Context stale | Updates AI rules from log. |
| **Re-read log** | Verify not agreed | Reload VERIFY BY after agreeing in chat. |
| **Edit criteria** | Outcomes not all accepted | Fix criteria in log. |
| **Acknowledge heuristic scan** | Signals after compile | Acknowledge exposure scan (not proof). |
| **Open report** | Heuristic signals | Open signal report. |
| **Verify onboarding** | Onboarding verify fail/warn | Re-run onboarding verification before risky edits. |
| **Re-audit onboarding** | `watch_paths` changed/missing since audit | Refresh onboarding map to match current repo state. |

---

## Decide (always visible for active jobs)

Your completion gate.

| Button | What it does |
|--------|--------------|
| **Confirm pass** | Final owner yes when AI claim pass is present and blockers are clear. |
| **Reject pass** | Reject AI pass claim and keep feature IN PROGRESS. |
| **Edit criteria** | Change pass definitions / acceptance directly from the cockpit. |

If not ready, buttons are disabled and the reason line tells you why.

---

## Progress

### Outcomes table

| Column | Meaning |
|--------|---------|
| **Row** | Outcome label (A, B, …) |
| **Pass when** | Definition of done |
| **Accepts** | **✓** accepted · **?** pending · **✗** rejected |
| **Result** | **PASS** · **FAIL** · **—** not done |

Headline above the table uses the same counts as the table.

### Attempt history

Timeline of attempts for this job with status, detail, and recent compile-check results.

---

## Scope

### TARGET FILES

Paths the AI may edit. **✓** = touched this attempt · **○** = not yet.

### Out-of-scope edits

| Button | What it does |
|--------|--------------|
| **Revert** | Undo the save (usual choice if mistake). |
| **Keep** | Keep change; file stays out of scope. |
| **Keep + scope** | Keep change and add file to TARGET FILES. |
| **Diff** | View changes; then Revert or Keep here. |

**When scope blocks checks:** **1** Revert or Keep → **2** Run all checks (if used).

### Zones

Shows which folder groups are currently active. Toggle in **More** → **Zones ON/OFF**.

---

## More (collapsed)

All power tools in one place. Open the arrow to see them.

### Work

| Button | What it does |
|--------|--------------|
| **Continue in chat** | Resume fix work in Cursor with this job’s context. |
| **Refresh AI context** | Sync active CPFS rule from feature log. |
| **Run compile check** | Project test/build for this attempt. |
| **Run all checks** | Automated command/file outcomes → PASS/FAIL. |
| **Open feature log** | Open feature development log. |
| **Edit criteria** | Edit success criteria / acceptance. |
| **Switch job** / **Start job** | Change or start active feature log. |
| **Export report** | Export CPFS stats/report file. |

### Scope

| Button | What it does |
|--------|--------------|
| **Add to TARGET FILES** | Widen allowed edit paths for this job. |
| **Zones ON/OFF** | Toggle which folder groups are in-scope. |
| **Dismiss scope reviews** | Clear pending out-of-scope decisions. |

### Onboarding

| Button | What it does |
|--------|--------------|
| **Link onboarding** | Point CPFS at repo map doc. |
| **Verify onboarding** | Validate pipelines / repo map. |
| **Open onboarding** | Open that file. |
| **Re-audit onboarding** | Re-run the onboarding audit. |
| **Approve onboarding** | Owner sign-off on onboarding doc. |

**First-time repo:** **1** Enable CPFS → **2** Link onboarding → **3** Verify onboarding → **4** Start job.

### Workspace

| Button | What it does |
|--------|--------------|
| **Checkpoint: ON/OFF** | Snapshot at attempt start. |
| **Auto-restore: ON/OFF** | Restore on compile **fail**. |
| **Restore checkpoint** | Manual rollback to snapshot. |
| **Lint: ON/OFF** | Lint on save (inform only). |
| **Set lint command** | Configure lint command. |
| **Reference images folder** | Context + verify screenshots. |
| **Heuristic scan** | Manual folder heuristic scan. |
| **Open heuristic report** | Open last automatic heuristic report. |
| **Install self-test** | CPFS install checklist. |

### All jobs

Read-only list. To change active job, use **Work** → **Switch job**.

---

## Quick flows

**First time in repo:** Enable CPFS → Link + Verify onboarding → Start job → Chat (fix + verify) → Refresh AI context → Continue in chat.

**Finishing a feature:** Clear scope (Revert/Keep) → All outcomes PASS → Confirm pass.

**Daily fix:** Continue in chat → Refresh context if stale → Run all checks when ready → Confirm pass when AI claims pass.

---

## Document history

| Date | Change |
|------|--------|
| 2026-07-08 | Initial cockpit guide (rc.42) |
| 2026-07-08 | Updated for zone layout (rc.44): Now, Blockers, Decide, Progress, Scope, More |

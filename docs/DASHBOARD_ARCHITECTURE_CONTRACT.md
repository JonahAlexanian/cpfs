# CPFS Dashboard Architecture Contract

This document defines the permanent cockpit structure for CPFS Runner dashboards.
New features must plug into existing sections and states instead of introducing new top-level layouts.

## Principles

- One screen, fixed order, stable section names.
- Core owner decisions are always visible.
- Actions are disabled with reason text, never hidden silently.
- New CPFS capabilities extend existing cards by adding status/action rows.
- The dashboard remains conversation-first: chat is where work happens; dashboard is control + truth.
- All power tools are reachable from the dashboard, but the default view stays calm.

## Fixed section order

1. Now
2. Blockers
3. Decide
4. Progress
5. Scope
6. More (collapsed)

## Section responsibilities

### 1) Now

Shows job identity and current state signals plus the one hero next action.

- Badge (`NO ACTIVE`, `IN PROGRESS`, `VERIFIED`, `COMPLETE`)
- Active feature + attempt number
- Log path
- Chips for context freshness, outcomes rollup, scope state, verify agreement, compile state, onboarding, workflow
- Hero button: the single best next action for the current state

### 2) Blockers

Shows only immediate stop conditions with explicit owner actions.

- AI exhausted acknowledgement
- Pending scope review warning
- Context stale warning
- Verify not agreed warning
- Heuristic review acknowledgement
- Onboarding verify fail/warn
- Onboarding staleness warning
- Lint failure

When no blockers exist, render explicit text: `No blockers right now.`

### 3) Decide

Always visible for active jobs. Never hidden by state.

- Confirm pass
- Reject pass
- Edit criteria

Buttons can be disabled, but the decision card must include a reason line:

- Waiting for AI claim pass
- Resolve pending scope reviews
- Acknowledge AI exhausted first
- Verify evidence missing
- Already COMPLETE
- Ready

### 4) Progress

Single source of completion truth and attempt history.

- One outcomes rollup headline + next-step line
- Outcome table: row, pass condition, acceptance, latest result
- Attempt timeline: attempt number, status, detail, files modified, recent compile checks
- Must stay numerically consistent with status chips and run-all-checks messaging

### 5) Scope

Boundary and review controls.

- TARGET FILES list
- Out-of-scope pending edits with actions (`Revert`, `Keep`, `Keep + scope`, `Diff`)
- Zones-on summary

### 6) More

All additional power tools, collapsed by default to keep the main view calm.

Groups:

- **Work** — Continue in chat, Refresh AI context, Run compile check, Run all checks, Open feature log, Edit criteria, Switch/Start job, Export report
- **Scope** — Add to TARGET FILES, Zones ON/OFF, Dismiss scope reviews
- **Onboarding** — Link, Verify, Open, Re-audit, Approve onboarding
- **Workspace** — Checkpoint toggles/restore, Lint toggles/command, Reference images, Heuristic scan, Open heuristic report, Install self-test
- **All Jobs** — Read-only list

## Feature-slot map (future-proofing)

Map planned and shipped CPFS features to existing sections:

- Escalation automation -> Blockers + Decide
- Soft-block scope gate -> Scope + Blockers + Decide reason
- Onboarding staleness gate -> Blockers + More
- Pipeline/stage workflow capture -> Now chips + Progress metadata
- Pre-flight summary -> More (before run actions)
- Verify evidence gate -> Decide + Progress
- Heuristic exposure scan -> Blockers + More
- Checkpoint/restore -> More
- Lint after edit -> More + Now chip
- Dismiss scope reviews -> More

No new top-level section is allowed for these features.

## Change policy

Layout-level change requests must satisfy both:

1. Existing section cannot represent the new state/action even with added row/subpanel.
2. A user-visible regression cannot be solved by state logic alone (enable/disable/reason text).

If both are not true, update behavior within current section contracts.

## QA expectations for dashboard changes

- Confirm/Reject visibility is stable across all statuses.
- Rollup text never references an action that is absent from screen.
- Disabled controls include a visible reason line.
- `npm run test:smoke` remains green.
- Dashboard guide stays aligned with actual labels.

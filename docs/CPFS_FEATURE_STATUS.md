# CPFS — Feature status (document vs Runner)

**Generated:** 2026-07-10 · **Runner version:** 1.0.0-rc.116
**CPFS document:** `public_tutorials/cpfs.md` (rev 1.4) · `project_status/CPFS.md`
**Speed used for estimates:** AI coding speed (this assistant, on this repo) — not human time.

Ordered by **importance** (10 = strongest reason the feature exists / pay-worthy). Statuses:
- **Done** — coded and shipped.
- **Partial** — helps but not fully enforced; work remaining noted.
- **Not-done** — not built; estimate to build.
- **Impossible in code** — cannot be enforced by an extension, with the reason.

---

## 1. Importance 10 — the core contract

| # | Feature | Source | Status | Work left (AI speed) |
|---|---------|--------|--------|----------------------|
| 1 | DO NOT REPEAT — never retry failed approaches; injected into AI context every message | Doc §1/§4/§5 | **Done** | — |
| 2 | VERIFY BY — agree how to prove success once, reused every attempt | Doc §3/§7 | **Done** | — |
| 3 | End Success / Confirm Pass blocked while verify pending (no false "done") | Doc §7/§9 | **Done** | — |
| 4 | Work in chat; Runner matches your message to the right job | Doc §6/§8 | **Done** | — |

## 2. Importance 9 — safety + proof

| # | Feature | Source | Status | Work left (AI speed) |
|---|---------|--------|--------|----------------------|
| 5 | TARGET FILES list per feature; warn on save outside scope | Doc §2 | **Done** | — |
| 6 | Strict mode — block save outside TARGET FILES | Doc §2 | **Done** | — |
| 7 | Review out-of-scope edits (keep / revert / add to scope) | Doc §2 | **Done** | — |
| 8 | Run automated test command before handoff (Validate Attempt) | Doc §7 | **Done** | — |
| 9 | Status bar shows active job + attempt + ON/OFF | Runner-extra | **Done** | — |
| 10 | Cursor hooks (before-prompt / after-edit / stop) | Runner-extra | **Done** | — |
| 11 | No demo-tuned heuristics in product code (§13 diff scanner) | Doc §13 | **Partial** | ~1–2h: tighten patterns + add "validate on unlike inputs" prompt before End Success. (Proving generalization is impossible — see §6.) |

## 3. Importance 8 — onboarding, recovery, honesty

| # | Feature | Source | Status | Work left (AI speed) |
|---|---------|--------|--------|----------------------|
| 12 | Defined success criteria before coding | Doc §3 | **Done** | — |
| 13 | Reuse verify steps every attempt (no re-ask) | Doc §3/§7 | **Done** | — |
| 14 | COMPLETE only after user-visible confirm | Doc §9 | **Done** | — |
| 15 | Auto-enable once per repo | Doc §6 | **Done** | — |
| 16 | Development zones (bench / extension / prod) | Runner-extra | **Done** | — |
| 17 | Checkpoint — snapshot at attempt start; restore on test fail | Runner-extra | **Done** | — |
| 18 | Install self-test (one command checks setup) | Runner-extra | **Done** | — |
| 19 | Destructive content guard — restore from git on accidental empty/shrink | Runner-extra | **Done** | — |
| 20 | Soft-block mode — block Confirm/Validate/End Success until scope reviews resolved | Doc §2 | **Done** | (now wired; was "partial" in rc.83 doc) |
| 21 | Revert files on failed attempt | Doc §6 | **Partial** | ~30min: add a "revert all files touched this attempt" prompt on End Failed (revert currently fires only on scope violation). |
| 22 | One log file per feature | Doc §4 | **Done** | — |

## 4. Importance 7 — memory, visibility, proof

| # | Feature | Source | Status | Work left (AI speed) |
|---|---------|--------|--------|----------------------|
| 23 | Chronological ATTEMPT history + dashboard timeline | Doc §4 | **Done** | — |
| 24 | Read full log before proposing code | Doc §5 | **Partial** | ~1h: inject "last N attempts full + older summarized" instead of summary + DNR only (full body hits context limits on long logs — see §6). |
| 25 | High-level verify (browser / screenshot / pipeline UI) | Doc §7 | **Partial** | ~4–8h: integrate a headless browser driver (Playwright) for screenshot-diff verify. Heavy + flaky in an extension; you currently describe steps. |
| 26 | Dashboard as glance/scoreboard (not homework) | Doc §6 | **Done** | — |
| 27 | Onboarding repo map — scaffold / verify / approve | Doc §14 | **Done** | — |
| 28 | Lint after edit on touched files | Runner-extra | **Done** | — |
| 29 | Scope edit history with diffs | Runner-extra | **Done** | — |
| 30 | Propose / sync verification in chat | Runner-extra | **Done** | — |
| 31 | Generic algorithms over one-sample shortcuts (§13) | Doc §13 | **Partial** | same as #11 — shared scanner; ~1–2h. |
| 32 | DO NOT REPEAT auto-append on fail | Doc §4 | **Done** | (now auto on End Failed; was "manual" in rc.83 doc) |
| 33 | Export HTML progress report | Runner-extra | **Done** | — |
| 34 | Get Started walkthrough in editor | Runner-extra | **Done** | — |

## 5. Importance 6 — clarity, focus, escalation

| # | Feature | Source | Status | Work left (AI speed) |
|---|---------|--------|--------|----------------------|
| 35 | End Attempt Success / Failed | Doc §4/§6 | **Done** | — |
| 36 | Failure class on failed attempt | Doc §4 | **Done** | — |
| 37 | Files touched this attempt | Doc §4 | **Done** | — |
| 38 | Re-audit onboarding → draft update | Doc §14 | **Done** | — |
| 39 | Gate Start Feature until onboarding ready | Doc §14 | **Done** | — |
| 40 | Pipelines & stages recorded in onboarding | Doc §14 | **Done** | — |
| 41 | Staleness gate at Start Feature (warn + offer re-audit) | Doc §14 | **Done** | (now a soft gate; was "partial" in rc.83 doc) |
| 42 | Pick pipeline + stage at Start Feature | Doc §14 | **Done** | (now wired via onboarding parser; was "not practical" in rc.83 doc) |
| 43 | Escalation on AI EXHAUSTED + owner acknowledges + picks next path | Doc §7 | **Done** | (finalized rc.116 — count-based "after 3" removed) |
| 44 | Pre-flight: state how this attempt differs from failures | Doc §5 | **Not-done** | ~1h: add a gate that requires the attempt log entry to contain a "differs from prior failures" line before End Success. |
| 45 | Iterate on test fail without asking permission | Doc §6/§8 | **Impossible** | AI behavior discipline — an extension cannot force the model to iterate vs ask. |
| 46 | All-jobs tab on dashboard | Runner-extra | **Done** | — |

## 6. Importance 5 — focus, ambiguity

| # | Feature | Source | Status | Work left (AI speed) |
|---|---------|--------|--------|----------------------|
| 47 | One clarifying question if two jobs match | Doc §6 | **Partial** | ~30–60min: surface an inline chat prompt when the resolver logs an ambiguous-match event. |
| 48 | Fix vs normal question detection | Doc §6 | **Partial** | ~1–2h: improve the resolver (currently event-logged, not perfect). |
| 49 | Reference images folder for visual verify | Runner-extra | **Done** | — |
| 50 | One hypothesis per attempt | Doc §2/§6 | **Impossible** | Cannot reliably detect "bundled hypotheses" from a diff; would be a brittle heuristic (and §13 forbids brittle heuristics). |

## 7. Importance 4 — polish

| # | Feature | Source | Status | Work left (AI speed) |
|---|---------|--------|--------|----------------------|
| 51 | Extension icon + Marketplace packaging | Runner-extra | **Done** | — |

---

## 8. Impossible in code (and why) — full list

| Feature | Why the Runner cannot do it |
|---------|------------------------------|
| Surgical editing / smallest diff | An extension sees the diff *after* the AI made it. It cannot choose the AI's edit. Detection-after-the-fact is weak and would itself be a heuristic (§13). |
| Blast-radius approval decision | Flagging a shared file is possible; *deciding* whether the wider change is acceptable is human business judgment (which features/industries are affected). Code can prompt, not approve. |
| Perfect "fix vs question" NLP | Paraphrase drift means no classifier is perfect; the doc itself calls this out as not-fully-automatable. |
| Proving §13 generalization on unseen inputs | You cannot prove correctness for inputs you have never seen — only run more test cases. The §13 self-check is a human question ("would this still be correct for an input we've never seen?"). |
| Forcing the AI to read the full log | Context limits + summarization truncate long logs; full-body injection is unreliable. The Runner injects summary + DNR instead. |
| One hypothesis per attempt | Detecting "multiple hypotheses in one change" from a diff is itself a brittle heuristic — exactly what §13 prohibits. |
| Iterate-without-asking-permission | That is model behavior, not extension behavior. The Runner can only set rules the model is *supposed* to follow. |

---

## 9. Runner extras NOT in the document — consider adding if important

These are not in CPFS.md and not yet built. Ranked by my recommended priority.

| # | Candidate extra | Why it matters | Importance | Estimate (AI speed) | Recommendation |
|---|------------------|----------------|-----------|----------------------|----------------|
| 1 | **Verify-on-unlike-inputs harness** (§13 enforcer) — a command that runs the changed code against a small set of generated "unlike" inputs and reports divergence | Turns §13 from prose into proof; strongest differentiator for precision code (geometry, retrieval, tenancy) | 9 | ~half day (needs per-project input-generator hooks) | **Add** — high value, fills the §13 gap (#11/#31) |
| 2 | **~~Cross-job DO NOT REPEAT~~** — DNR entries shared across jobs with the same failure class | ~~Stops repeating a failed approach on a *new* job~~ **WITHDRAWN (owner 2026-07-11)** — an approach that failed on one job often failed due to that job's specific situation and may be valid on a sibling; auto-suppressing across jobs is a brittle heuristic (§0B/§13). Removed from Runner. | 8 | n/a | **Removed** — false-suppression risk outweighs the memory win |
| 3 | **Auto-mark COMPLETE from chat sentiment** — owner types "passed / works / ship it" → log COMPLETE | Removes the manual Confirm Pass click for Type 2 | 7 | ~1h | **Add** — small, time-saving |
| 4 | **Job templates** — pre-filled success criteria per pipeline/stage | Faster Start Feature; less forgetting criteria | 6 | ~1–2h | Add if you start many similar jobs |
| 5 | **Test-coverage delta gate** — block End Success if coverage on touched files dropped | Catches "passed but deleted the test" | 6 | ~2–3h (coverage parser) | Add if you trust coverage tooling in-repo |
| 6 | **Pre-attempt diff preview** — show the AI the exact diff before apply, require "apply" | Extra safety on shared files | 6 | ~2–3h | Optional — overlaps with checkpoint/restore |
| 7 | **Time-box alert** — warn if an attempt exceeds N minutes | Scope-creep signal | 5 | ~30min | **Add** — trivial, useful |
| 8 | **Issue-tracker → CPFS job** (Linear/GitHub/Slack) — pull a bug report into a feature log | Removes manual retyping of criteria | 7 | ~half day (API auth + mapping) | Add once you have a tracker in use |
| 9 | **Auto-archive passed jobs** — move COMPLETE logs to archive after N days | Keeps the dashboard lean | 4 | ~30min | Optional |
| 10 | **Remote glance** — tiny local web view of the status bar | See job state from phone | 3 | ~half day | Skip for now |

**My recommendation:** add **#1, #3, #7** now (≈ 1 day total at AI speed). (#2 cross-job DNR was withdrawn — see table.) They are cheap, high-leverage, and directly strengthen the parts of CPFS that are currently Partial/Impossible.

---

## 10. Summary counts (current, rc.116)

| Status | Count |
|--------|-------|
| Done | 41 |
| Partial | 7 |
| Not-done | 1 |
| Impossible in code | 7 |
| **Total tracked** | **56** |

**Bottom line:** Every importance-10 and most importance-9 obligations are **Done**. The remaining gaps are either **Partial polish** (~half day to clear all seven) or **genuinely impossible in code** (human/AI discipline by design). The single highest-value addition is the **§13 verify-on-unlike-inputs harness** — it converts the one Impossible item that most affects production correctness into something the Runner can actually help prove.

---

*Local-only data. Cross-references `public_tutorials/cpfs.md`, `project_status/CPFS.md`, `cpfs-runner/docs/CPFS_DOC_VS_RUNNER_PUBLIC.md`.*

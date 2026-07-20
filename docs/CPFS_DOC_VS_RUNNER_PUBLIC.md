# CPFS document vs CPFS Runner — public comparison

**For:** Developers deciding whether CPFS Runner is worth paying for  
**Free CPFS methodology:** [ragbox.llc/tutorials/cpfs.html](https://ragbox.llc/tutorials/cpfs.html) · [Download CPFS.md](https://ragbox.llc/tutorials/cpfs.md)  
**CPFS Runner:** VS Code / Cursor extension (proprietary) — automates parts of the methodology locally

**How to read this table**

| Column | Meaning |
|--------|---------|
| **CPFS document** | What the free CPFS policy asks humans and AI to do |
| **Runner** | **Done** = coded in the extension · **Partial** = helps but not fully enforced · **Not practical in code** = stays human/AI discipline · **Extra** = Runner adds this; not spelled out in CPFS.md |
| **Useful (1–10)** | Value to a working programmer (10 = strongest reason to pay) |
| **Benefit** | One word — what you gain |

---

## Summary

| | Count |
|---|------|
| CPFS doc obligations tracked here | 42 |
| **Done** in Runner | 28 |
| **Partial** | 10 |
| **Not practical in code** (by design) | 8 |
| **Extra** (Runner-only, not in CPFS.md) | 20 |

**Pay story in one line:** Runner buys **time**, **memory**, **proof**, and **safety** — not “smarter AI,” but **less rework** and **honest done**.

---

## A — Memory & error loops (CPFS §1, §4, §5)

| CPFS document | Runner | Useful | Benefit |
|---------------|--------|--------|---------|
| DO NOT REPEAT — never retry failed approaches | **Done** — parsed from log, injected into AI context every message | 10 | memory |
| One log file per feature (not one mega-log) | **Done** — `logs/development_history/*.log` | 8 | clarity |
| Read full log before proposing code | **Partial** — summary + DNR in rules; not full log body | 7 | memory |
| Chronological ATTEMPT history | **Done** — log + dashboard timeline | 7 | visibility |
| Pre-flight: state how this attempt differs from failures | **Not practical in code** — AI must say it in chat | 6 | trust |
| One hypothesis per attempt | **Not practical in code** — log structure helps; AI enforces | 5 | focus |

---

## B — Scope & surgical edits (CPFS §2)

| CPFS document | Runner | Useful | Benefit |
|---------------|--------|--------|---------|
| TARGET FILES list per feature | **Done** — log header + active session | 9 | safety |
| Warn when saving outside allowed files | **Done** — save watcher + hooks | 9 | safety |
| Strict mode — block save outside scope | **Done** — `scopeMode: strict` | 8 | safety |
| soft-block mode | **Partial** — listed in settings; behavior not fully wired | 6 | safety |
| Review out-of-scope edits (keep / revert / add) | **Done** — diff review + zone picker | 9 | recovery |
| Blast radius before touching shared code | **Done** — legacy-code blast-radius detection: when a task MODIFIES a pre-existing symbol, the system detects it via git history, finds callers via LSP, filters CPFS-owned callers, and creates a coverage task (rc.142) | 9 | safety |
| Smallest diff; no drive-by refactors | **Not practical in code** — AI discipline | 6 | focus |

---

## C — Success & verification (CPFS §3, §7, §9)

| CPFS document | Runner | Useful | Benefit |
|---------------|--------|--------|---------|
| Defined success criteria before coding | **Done** — Start Feature + log parse | 8 | clarity |
| VERIFY BY — agree how to prove success once | **Done** — parse, agree-once, End Success gate | 10 | proof |
| Reuse verify steps every attempt (no re-ask) | **Done** — stored in log + rules | 9 | time |
| Run automated test command before handoff | **Done** — Validate Attempt | 9 | proof |
| NOT PROVEN when compile passes but verify pending | **Done** — honest dashboard label | 8 | trust |
| High-level verify (browser, screenshot, pipeline UI) | **Partial** — you describe steps; Runner does not drive browser | 7 | proof |
| VERIFIED (automated) in log | **Done** — append on validate pass | 7 | proof |
| Real-test-before-pass artifact gate | **Done** — End Success blocked unless REAL TEST EVIDENCE line has an on-disk artifact that exists (rc.122-124). Chat-claim guard catches "VERIFIED (automated)" stamps without a real test | 10 | proof |
| COMPLETE only after user-visible confirm | **Done** — badge + owner confirms in log | 8 | trust |
| Owner pass with transparency | **Done** — two stamps: `OWNER CONFIRM PASS` (AI had evidence) vs `OWNER PASS (NO AI ARTIFACT)` (owner tested it themselves, no AI evidence). Both flip to PASSED; log shows who was responsible (rc.163) | 8 | trust |
| Approaches exhausted / give up → escalate | **Done** — Runner escalates on `### AI EXHAUSTED` in log (AI declares it is out of distinct approaches); owner acknowledges + picks next path | 6 | time |

---

## D — Workflow & chat-first (CPFS §6, §8, turn-on-and-forget)

| CPFS document | Runner | Useful | Benefit |
|---------------|--------|--------|---------|
| Work in chat; CPFS tracks the fix | **Done** — chat job matching + hooks | 10 | time |
| Fix vs normal question detection | **Partial** — resolver exists; not perfect | 6 | focus |
| One clarifying question if two jobs match | **Partial** — event logged; no chat prompt yet | 5 | clarity |
| Enable once per repo | **Done** — auto-enable on open | 8 | time |
| Status bar shows active job + attempt | **Done** | 9 | visibility |
| Dashboard as glance/scoreboard (not homework) | **Done** — trimmed UI + next action | 7 | visibility |
| Iterate on test fail without asking permission | **Not practical in code** — AI behavior | 6 | time |

---

## E — Attempt lifecycle (CPFS §4, §6)

| CPFS document | Runner | Useful | Benefit |
|---------------|--------|--------|---------|
| End Attempt Success / Failed | **Done** | 8 | clarity |
| End Success blocked if VERIFY BY pending | **Done** | 10 | trust |
| Failure class on failed attempt | **Done** — quick-pick | 6 | clarity |
| DO NOT REPEAT append on fail | **Partial** — command exists; often manual | 7 | memory |
| Revert files on failed attempt | **Partial** — git restore prompt on End Failed | 8 | recovery |
| Files touched this attempt | **Done** — `active.json` | 6 | visibility |

---

## F — Project onboarding (CPFS §14)

| CPFS document | Runner | Useful | Benefit |
|---------------|--------|--------|---------|
| Repo map before first feature | **Done** — link / scaffold / verify / approve | 7 | onboarding |
| Gate Start Feature until onboarding ready | **Done** | 6 | onboarding |
| Pipelines & stages in onboarding doc | **Done** — verify checks sections exist | 6 | clarity |
| Pick pipeline + stage at Start Feature | **Not practical in code** yet — manual in log | 5 | clarity |
| Staleness when repo layout changes (`watch_paths`) | **Partial** — verify warns; no Start Feature block | 4 | safety |
| Bounded audit → draft onboarding | **Done** — Re-Audit command | 6 | onboarding |

---

## G — Product code discipline (CPFS §13)

| CPFS document | Runner | Useful | Benefit |
|---------------|--------|--------|---------|
| No demo-tuned heuristics in product code | **Done** — heuristic scanner detects numeric thresholds in conditionals, path-literal branches, single-sample VERIFY BY, and skipped deleted diff lines. On-demand scan + auto-scan after validate pass; scan a folder or the whole repo (rc.7+) | 9 | trust |
| Generic algorithms over one-sample shortcuts | **Partial** — flags smells; cannot prove generalization without extra tests | 7 | trust |

*Runner does not replace repo-specific rules (tenant isolation, no hardcoding, etc.). CPFS Runner handles **workflow**; §13 stays in your project rules.*

---

## H — Extra: Runner adds (not in CPFS.md)

| Capability | Runner | Useful | Benefit |
|------------|--------|--------|---------|
| **Install self-test** — one command checks setup | **Extra** | 8 | time |
| **Development zones** — bench / extension / prod groupings from map | **Extra** | 8 | safety |
| **Lint after edit** on touched files | **Extra** | 7 | proof |
| **Checkpoint** — snapshot at attempt start; restore on test fail | **Extra** | 8 | recovery |
| **Destructive content guard** — vocal prompt + one-click `git checkout HEAD` when a tracked file is accidentally emptied/shrunk on disk | **Extra** | 8 | recovery |
| **Export HTML report** — shareable progress summary | **Extra** | 6 | visibility |
| **Cursor hooks** — before-prompt, after-edit, stop | **Extra** | 9 | time |
| **Get Started walkthrough** in editor | **Extra** | 7 | onboarding |
| **All jobs** tab on dashboard | **Extra** | 6 | visibility |
| **Reference images folder** for visual verify | **Extra** | 5 | proof |
| **Scope edit history** with diffs | **Extra** | 7 | recovery |
| **Extension icon + Marketplace packaging** | **Extra** | 4 | onboarding |
| **Propose / sync verification in chat** helpers | **Extra** | 7 | time |
| **Mandatory rule auto-install** — writes `.cursor/rules/cpfs-mandatory.mdc` with end-to-end testing + subagent restrictions on enable; startup check prompts if missing (rc.160-162) | **Extra** | 9 | trust |
| **Work queue model** — no persistent "active job"; jobs transition to verdict states (PASSED, FAILED, GAVE_UP, KEEP_WORKING, NEW); AI picks next job from queue automatically | **Extra** | 8 | time |
| **Type 2 to Type 3 upgrade** — promote a quick-fix job to a formal feature with structured tests; pre-fills from the existing log | **Extra** | 7 | clarity |
| **Cross-task regression re-verification** — when a new task changes a symbol a prior task relies on, the prior task gets an "affected" badge; frozen RETEST recipes re-run to verify still-passing (rc.126) | **Extra** | 9 | safety |
| **Milestone commit** — auto git commit on End Success with job tag; clean working tree after pass (rc.140) | **Extra** | 7 | recovery |
| **Heuristic scanner** — on-demand and auto heuristic scanning for demo-tuned thresholds, one-sample VERIFY BY, path-literal branches; scan a folder or the whole repo (rc.7+) | **Extra** | 8 | trust |
| **Affected badges on AI_CLAIMED** — regression warnings appear the moment the AI finishes work, before the owner decides to pass (rc.164) | **Extra** | 8 | safety |

---

## Usefulness at a glance (10 = pay-worthy)

| Score | Items |
|-------|--------|
| **10** | DO NOT REPEAT in context · VERIFY BY gate · chat picks job · block false success · real-test artifact gate |
| **9** | TARGET FILES · scope warn · scope review · test command · status bar · hooks · mandatory rule auto-install · cross-task regression · blast-radius detection |
| **8** | success criteria · verify reuse · COMPLETE honesty · owner pass transparency · auto-enable · zones · checkpoint · install self-test · destructive content guard · work queue · heuristic scanner · affected badges on AI_CLAIMED |
| **7** | attempt history · dashboard · onboarding · export · lint · revert-on-fail |
| **6** | failure class · touched files · re-audit · partial pre-flight |
| **5** | ambiguous job · pipeline picker · reference images |
| **4** | staleness gate (partial) · marketplace packaging |

---

## What paying for Runner actually buys (benefit words)

| Benefit | What you get |
|---------|----------------|
| **time** | Less re-explaining verify steps; chat picks job; auto-enable; hooks; install self-test |
| **memory** | DO NOT REPEAT and attempt history survive new chats |
| **proof** | Validate command + VERIFY BY gate; NOT PROVEN label |
| **trust** | Cannot End Success while verify pending; COMPLETE needs you |
| **safety** | Scope warnings, zones, strict mode, review out-of-scope edits |
| **recovery** | Revert prompts, checkpoint restore, scope review, destructive content guard (restore from git on accidental empty/shrink) |
| **visibility** | Status bar, dashboard timeline, export report |
| **clarity** | One log per feature, criteria in log, next-action button |
| **focus** | Scope + job tracking reduce wrong-file / wrong-task drift |
| **onboarding** | Repo map scaffold, verify, Get Started |

---

## Closing — what the doc promises vs what code can deliver

**CPFS document** is a full discipline: logging, scope, verification, human judgment, and production-code values (§13). Much of it **must** stay with the developer and the AI — no extension can force “smallest diff” or “no heuristics in your STL exporter.”

**CPFS Runner** automates the **high-leverage, repetitive** parts: **memory**, **scope signals**, **verify contract**, **test runs**, **job tracking**, and **local audit trail** — the pieces that otherwise slip when you are tired or switching chats.

**Not fulfilled by code (and that is OK):** surgical editing, blast-radius conversations, browser/screenshot verify, exhaustion/give-up escalation prose, §13 product logic, perfect “fix vs question” NLP.

**Added beyond the doc:** zones, lint, checkpoint, install self-test, export report, hooks packaging, dashboard scoreboard, destructive content guard, mandatory rule auto-install, work queue model, Type 2→3 upgrade, cross-task regression re-verification, legacy blast-radius detection, milestone commit, heuristic scanner, two-kind owner pass, affected badges on AI_CLAIMED — practical product glue the markdown policy does not describe.

---

## Availability

CPFS Runner is a **proprietary extension, available by subscription only** — it is not open source. The free CPFS methodology (the public doc at https://ragbox.llc/tutorials/cpfs.html) is open and unrestricted; the extension that enforces it is a paid product.

**Free tier:** The extension lets you run **10 fully managed tasks per month** with full enforcement (scope guards, verify gates, DO NOT REPEAT memory, regression checks). After 10 in a month, managed-task mode pauses — your AI and editor keep working, and the safety features (destructive-file guard, fake-done catcher, scope guard) stay on by default. Subscribe to the **Pro** tier for unlimited managed tasks with full enforcement.

**Pro subscription:** unlimited managed tasks with full enforcement. Contact [RAG Box](https://ragbox.llc) to subscribe.

---

*Generated for CPFS Runner 1.0.0-rc.166 · Local-only data · [CPFS methodology](https://ragbox.llc/tutorials/cpfs.html)*

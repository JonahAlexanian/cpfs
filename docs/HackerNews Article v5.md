**Title:** Show HN: CPFS Runner – Program without programming with AI

I have stopped writing code entirely. I don't type code, design schemas, or manage file locations. I direct the work — the AI handles implementation. The problem? AI agents drift, break things, and forget their own constraints. I was spending all my time babysitting rather than shipping.

So I built CPFS Runner — a VS Code/Cursor extension that mechanically enforces the CPFS (Consolidated Programming For Success) protocol. You say what to fix and how to verify. The AI writes the code. CPFS Runner enforces the guardrails. You confirm pass when the evidence is real.

**The three problems it solves:**

1. **AI writes heuristic shortcuts that pass one example but won't generalize.** A ratio gate, a path-literal branch, a magic number — it passes today and breaks on the next upload. CPFS Runner's heuristic scanner flags numeric thresholds in conditionals, path-literal branches, single-sample verification, and workaround comments before you ship. It also scans prior code for the same patterns.

2. **AI oversteps into shared code other features depend on.** The AI edits a function — three other features already call it. CPFS Runner detects the symbol-level overlap with prior passed tasks and tells you to re-verify them before the change ships. Frozen retest recipes re-run automatically and report pass/fail.

3. **AI spins on the same failed logic across sessions.** Next session, context is gone — the AI tries the same approach again. CPFS Runner parses `DO NOT REPEAT` from the log and injects it into every prompt. The AI is forced to change track. When all approaches are exhausted, an `AI EXHAUSTED` declaration fires an escalation event.

**Proof of work:**

I used this methodology to build and ship [stl2cad.com](https://stl2cad.com) — a live paid service that converts mechanical STL uploads to STEP CAD files — in under 30 days, with zero coding. It handles brackets, housings, plates, mounts, and fixtures. It is not a general-purpose mesh-to-CAD engine: artistic meshes, organic sculpts, and 3D scans are out of scope, files are capped at ~250,000 triangles, and curved regions export as boundary outlines you finish in CAD. But it is a production product with paying customers, built by a non-programmer directing AI.

Then I used CPFS Runner to build CPFS Runner itself — the tool enforced its own development discipline. Every feature logged, every change scoped, every pass verified. The tool built itself. We didn't ship a discipline tool that we wrote without discipline.

**Access:**

The CPFS protocol is free and open source — unrestricted, no account, no subscription. CPFS Runner is a proprietary enforcement engine with a free tier: 10 fully-enforced tasks per month — scope gates, verify gates, real-test-evidence gate, regression detection — no license key, no credit card, no time limit. After 10, managed mode pauses; the safety features (destructive-file guard, fake-done catcher, scope guard) stay on by default. Subscribe to CPFS Pro for unlimited managed tasks — $49/month or $490/year (2 months free).

**Install:** Search "CPFS" in Cursor/VS Code Extensions, or [download the .vsix directly](https://ragbox.llc/tutorials/cpfs-runner-latest.vsix).

**Links:**
- [Homepage + download](https://ragbox.llc/tutorials/cpfs-runner.html)
- [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=ragbox.cpfs-runner)
- [GitHub](https://github.com/JonahAlexanian/cpfs-runner)
- [CPFS protocol (free/open)](https://ragbox.llc/tutorials/cpfs.html)

If you are tired of prompt-and-pray development and want a deterministic cycle where the editor enforces what the agent forgets, try it free. No credit card required. 10 tasks/month. That's enough to ship a real feature and see the difference.

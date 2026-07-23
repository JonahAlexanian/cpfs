
**Title:** Show HN: CPFS Runner – An enforcement engine that stops AI from drifting

I have stopped writing code entirely. I don't type code, design schemas, or manage file locations. I am the Architect and Director—the AI handles implementation and debugging. The problem? AI agents drift, break things, and forget their own constraints. I was spending all my time babysitting rather than shipping.

So I built CPFS Runner—a VS Code/Cursor extension that mechanically enforces the CPFS (Consolidated Programming For Success) protocol.

**The workflow:**

* **Architect-Director Loop:** I focus on the "what" and the "why." I oversee design, validate proposals, and step in for high-level corrections. The AI generates the logic and performs the implementation.
* **Mechanical Blast-Radius Tracking:** If the AI writes code that impacts prior modules, the system flags it and forces a new test task. If a regression occurs, the system flags the failure and provides a one-click copy of the AI's own diagnostic finding to paste back into the chat for a fix.
* **Real-Proof Gate (No "Fake" Testing):** Compilation and unit tests alone are not enough to mark a task "Passed." The AI must exercise the code like a human would and capture real output as an artifact on disk—a screenshot, a generated file, or a command's actual output. If there is no proof artifact, the system does not accept the "Pass".  At pass time, it freezes a retest recipe (exact command + expected outcome). If later work touches the same code, those recipes re-run automatically. Completion is blocked until any regression is resolved.
* **Subagent Constraints:** Any subagent the AI spawns is restricted to read-only exploration. All actual code modifications are gated by the main CPFS conversation, where enforcement rules fire.
* **Heuristic Scanner:** Flags numeric thresholds in conditionals, path-literal branches, and single-sample verification—on demand and automatically after a validate pass.

**Proof of work:** I used this methodology to build and ship STL2CAD (an STL-to-STEP conversion utility) in under 30 days. Then I used CPFS Runner to build CPFS Runner itself—the system that governed the AI while it built its own enforcement engine. The tool passed its own regression suite during development.

**Access:** The CPFS protocol is free and open source—unrestricted, no account, no subscription. CPFS Runner is a proprietary enforcement engine with a free tier: 10 fully-enforced tasks per month—scope gates, verify gates, real-test-evidence gate, regression detection—no license key, no credit card, no time limit. After 10, managed mode pauses; the safety features (destructive-file guard, fake-done catcher, scope guard) stay on by default. Subscribe to CPFS Pro for unlimited managed tasks.

If you are tired of prompt-and-pray development and want a deterministic cycle where the editor enforces what the agent forgets, try it free at the link below. No credit card required. 10 tasks/month. That's enough to ship a real feature and see the difference.

[https://ragbox.llc/tutorials/cpfs-runner.html](https://ragbox.llc/tutorials/cpfs-runner.html)

Title: Show HN: CPFS Runner – A deterministic enforcement layer for AI-driven development

I have stopped writing code entirely. I no longer type code, design schemas, or manage file locations. Instead, I function as the Architect and Director. The design process is a collaboration where the AI proposes solutions, and I provide the technical oversight, validate the direction, and suggest alterations. Once the design is set, the AI handles the implementation and debugging.

The industry focus is currently on "AI Agents" that try to autonomously patch systems. My experience? They drift, they break things, and they forget their own constraints. I was spending all my time "babysitting" the AI rather than shipping.

So, I built CPFS Runner—a VS Code/Cursor extension that functions as a mechanical enforcement engine for the CPFS (Consolidated Programming For Success) protocol.

My workflow:

The Architect-Director Loop: I focus on the "what" and the "why." I oversee the design, validate the AI's proposals, and step in for high-level corrections. The AI acts as the "worker" that generates the logic and performs the implementation.

Mechanical Blast-Radius Tracking: If the AI writes code that impacts prior modules, the system flags it and forces a new test task. If a regression occurs, the system flags the failure and provides a one-click copy of the AI's own diagnostic finding to paste back into the chat for a fix.

Real-Proof Gate (No "Fake" Testing): I don't take the AI's word for it. Compilation and unit tests are forbidden as the sole basis for a "Pass." To complete a task, the AI must exercise the code the way a human would and capture the real output as an artifact on disk — a screenshot, a generated file, or a command's actual output. If there is no proof artifact, the system does not accept the "Pass." At pass time it freezes a retest recipe (the exact command + expected outcome) for that task. If later work touches the same code, those recipes re-run automatically, and completion is blocked until any regression is resolved.

Visual Verification (Proof by Artifact): In a Type 3 task, I define a reference image directory. I drop a screenshot showing the bug, and the AI is forced to drop its own verify-<feature>-attempt-N.png into that same folder. If there is no screenshot evidence proving the fix in the folder, the system does not accept the "Pass."

Subagent Constraints: Any subagent the AI spawns is restricted to read-only exploration. All actual code modifications are gated by the main CPFS conversation, where the enforcement rules fire.

Proof of Work:
I used this methodology to build and ship STL2CAD (an STL-to-STEP conversion utility) in under 30 days. Furthermore, I developed CPFS Runner itself while using the protocol to manage its own development.

Access:
The CPFS protocol itself is free and open source. CPFS Runner is a proprietary enforcement engine available by subscription.

The tool includes a free version that lets you run 10 fully-managed tasks per month with full enforcement (scope gates, verify gates, real-proof gate, regression detection). From the 8th task onward, the system warns you that the free monthly quota is almost up; after the 10th, managed mode pauses — your AI and editor keep working normally, and the system asks whether to keep the safety features on (destructive-file guard, fake-done catcher, scope guard) or turn everything off. Subscribe to CPFS Pro for unlimited managed tasks.

If you are interested in moving from 'prompt-and-pray' to a deterministic development cycle, you can learn more at the link below.

https://ragbox.llc/tutorials/cpfs-runner.html

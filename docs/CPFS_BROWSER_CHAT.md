# CPFS for Browser AI Chat — a methodology for any LLM, any domain

**For:** Anyone doing real work with an AI in a browser chat (ChatGPT, Claude, Gemini, and others) — writing, research, planning, analysis, drafting, studying.
**Not just for programmers.** This is the domain-agnostic version of CPFS. It needs no software, no extensions, no account. You apply it yourself, in any browser, with any LLM.
**Companion:** CPFS Runner (a VS Code / Cursor extension) automates parts of this for programmers. This document is the manual version for everyone else.

---

## Why you need this

When you work with an AI in a chat, the same five things go wrong — in every domain, on every model:

1. **It says "done" when it isn't.** The AI declares the task complete, correct, or finished — without anyone actually checking. ("The article is ready." "The research is complete." "The plan is solid.") Nothing was verified.
2. **It drifts from what you asked.** It does more, or different, than you wanted. It changes the tone you said to keep, rewrites the section you said to leave alone, adds claims with no source, or expands the scope on its own.
3. **You lose the thread.** Long chats get summarized and compressed. Your earlier instructions quietly disappear. You can't tell what was already tried and why it failed.
4. **It grades itself.** The AI tells you its own work is good, and there's no clear point where *you* say "yes, this is actually right" or "no, this is wrong." It self-passes.
5. **Everything piles into one chat.** There are no boundaries between attempts. Draft 1, draft 2, and the failed approach all blur together. You can't point to "this is the version that worked, and here's what was wrong with the one before."

CPFS is a short set of habits that stops all five. You stay in control, the AI can't self-pass, and you keep a record of what happened.

---

## The method

Before, during, and after you ask the AI. Five habits.

### 1. One task per chat
Don't pile several goals into one conversation. If you have three things to do, start three chats (or three clearly separated sections). One task, one thread. This keeps the context clean and stops the AI from mixing goals.

### 2. Define "done" before you start
Before the first prompt, write down — for yourself, and ideally in the prompt — what **finished and verified** means for this task. Not "a good article." Something you can actually check:
- "A 600-word blog post in a casual tone, with 3 real cited sources, no claims without a link, and an intro under 80 words."
- "A research brief listing the 5 most-cited reasons X happens, each with a source I can open."
- "A step-by-step travel plan for 4 days, with each day under $200, and transport times that add up."

If you can't say what "done" looks like in checkable terms, you aren't ready to start — the AI can't hit a target you can't describe.

### 3. Set the guardrails
Tell the AI what it must **not** do, and what it must keep. This is your scope:
- "Don't change the existing intro."
- "Keep a neutral, professional tone — no exclamation marks, no marketing language."
- "Every factual claim needs a source link. If you can't source it, say so instead of making it up."
- "Don't add sections I didn't ask for."

Guardrails are how you stop drift (#2). State them once, up front, and refer back to them.

### 4. The AI never grades itself
This is the core rule. **Never accept "done / correct / complete / that's right" from the AI as proof.** The AI saying its own work is good is a *claim*, not a verification. You verify against your "done" definition from step 2:
- Did it actually hit the word count, the tone, the source requirement?
- Open the links. Do they say what the AI claims?
- Does the plan add up? Did it keep the intro you said not to touch?

If it passes your check, **you** say it passed. If not, send it back with the specific failure ("the second source link is dead," "you changed the intro tone," "this claim has no source"). The owner gates the pass — never the AI.

### 5. Keep a one-line log
Outside the chat (a notes file, a doc, even paper), keep a tiny record per task:
- What you asked.
- What "done" meant.
- Each draft: what was wrong with it (one line).
- The draft that passed, and why.

This fixes the lost-thread and no-boundaries problems (#3, #5). When the chat compresses and forgets, your log doesn't. When you come back next week, you know what was tried and what actually worked.

---

## When it gets stuck

If the AI keeps claiming "done" but it isn't right, or it's going in circles — **stop it.** Don't let it spin. Three options:

- **Restate the problem** in one clear sentence, with the specific failure from your check.
- **Restate "done"** — sometimes the AI drifted because the target was fuzzy.
- **Start a fresh chat**, paste your task + "done" definition + guardrails + the one-line log of what failed, and continue. The fresh chat has clean context; your log carries the history.

A fresh chat with your log beats a 200-message chat that's forgotten its own instructions.

---

## A worked example (non-coding)

**Task:** "Draft a blog post about why small businesses should use AI for customer support."

**Before you prompt — write down:**
- *Done means:* 600 words, casual tone, 3 real source links (each opens and says what I claim), an intro under 80 words, a clear recommendation at the end.
- *Guardrails:* every stat needs a source; no made-up numbers; don't write the headline for me; keep it neutral, not salesy.

**You prompt:** include the task, the done-definition, and the guardrails in the first message.

**Draft 1 comes back.** You check against your list:
- 720 words — fail (over).
- Source 2 link is dead — fail.
- Tone is salesy ("supercharge your support!") — fail (guardrail).

You send it back with those three specifics. **You do not accept "here's the improved version, it's ready now."** You check draft 2 the same way.

**Draft 2 passes your check.** You write in your log: *"Draft 2 passed — 610 words, all 3 links live, tone fixed. Rejected draft 1: over word count, dead link, salesy tone."*

That's a verified, owner-gated result with a trail — not an AI self-pass.

---

## One-page checklist (keep this beside you)

**Before:**
- [ ] One task, one chat.
- [ ] Wrote down what "done and verified" means — in checkable terms.
- [ ] Wrote the guardrails (what not to change / what to keep).

**During:**
- [ ] Put task + done-definition + guardrails in the first prompt.
- [ ] Reviewing each draft against the done-definition, not against the AI's "it's ready."
- [ ] Sending back specific failures, not vague "make it better."

**After:**
- [ ] **I** decided it passed (the AI didn't self-pass).
- [ ] Wrote a one-line log: what was asked, what failed, what passed and why.
- [ ] If stuck: stopped, restated or started fresh with the log.

---

## The one rule that matters most

**The AI does not get to say its own work is done. You do.** Everything else in this document exists to make that one rule actually work.

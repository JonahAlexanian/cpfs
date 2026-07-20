# CPFS Runner — Quick Start

> **For:** CPFS Runner <!--CPFS_VERSION-->1.1.8<!--/CPFS_VERSION-->
> **Who this is for:** People using Cursor or VS Code with an AI coding helper.
> **In one line:** CPFS Runner makes the AI follow the [CPFS rules](https://ragbox.llc/tutorials/cpfs.html) for real — so it can't forget what failed or fake "done," even in a new chat.

> **You're reading this from inside CPFS Runner** — so the extension is already installed. On a fresh install or an update, CPFS automatically:
> - opens this Quick Start,
> - runs the install self-test and opens the report at `logs/development_history/cpfs-self-test.md` (it opens as a tab without grabbing focus),
> - shows a **Reload Window** toast.
>
> **Click Reload Window on that toast** (or run `Ctrl+Shift+P` → **Developer: Reload Window**) to finish enabling the extension. Then follow the steps below once per project. After that, you work in chat and CPFS tracks the fix.

---

## What you need first

- **Cursor** or **VS Code** (Cursor works best).
- A project folder open in your editor.

---

## Step 1 — Turn on CPFS for this project

Look at the bottom status bar. You'll see a **CPFS OFF** indicator. Click it to turn CPFS on. (It enables with a default file-scope zone.)

Prefer to choose the scope zone yourself? Use the Command Palette (`Ctrl+Shift+P`) → **`CPFS: Enable Workspace`** — it asks where you usually develop and sets your default target files.

Either way, CPFS creates in your project:
- `logs/development_history/` — where CPFS keeps the fix logs.
- `.cpfs/config.json` — your CPFS settings for this project.
- Hooks and the mandatory CPFS rule (in `.cursor/`) — installed for you.
- `.gitignore` entries — so CPFS session files stay out of git.

The indicator flips to **CPFS ON**. You only do this once per project — CPFS turns on by itself on later opens.

**To pause CPFS any time:** click the **CPFS ON** indicator (it flips back to OFF; all logs and settings are kept). Click it again to resume.

---

## Step 2 — Open the dashboard

Click the **CPFS** item in the bottom status bar (the one showing the job label, just left of the ON/OFF indicator). (Or use the Command Palette `Ctrl+Shift+P` → **`CPFS: Open CPFS Dashboard`**.)

The dashboard is your control panel. Its top bar shows the CPFS on/off toggle, the running job, and the version. Below that:
- a collapsible **Settings** section (click the "Settings" header to expand it),
- a **Work Queue** (jobs that need attention),
- the **All-jobs** browser (every feature log; select a job to see its detail tabs: **Requests, Criteria, Tests, Work, Communication, Attempts, Scope**),
- a **CPFS health** row, an **Onboarding** section, and **Quick actions**.

---

## Step 3 — Set your test command (required)

CPFS needs to know what command proves your code works (for example `npm test`, `pytest`, `make test`).

1. In the dashboard, expand the **Settings** section (click the "Settings" header).
2. Find the **Test command** group.
3. In the **Validate Attempt command** box, type your test command (e.g. `npm test`).
4. Click **Save settings** (top-right, or bottom of the section).

This is what CPFS runs each time you (or the AI) choose **Validate Attempt**. Without it, CPFS can't check a fix.

---

## Step 4 — Set your reference images folder (required)

CPFS uses a folder for proof screenshots. You drop bug screenshots there; the AI drops `verify-<feature>-attempt-N.png` there to prove a fix worked.

1. Command Palette (`Ctrl+Shift+P`).
2. Type and run: **`CPFS: Set Reference Images Folder`**
3. A folder picker opens. Pick a folder **inside your project** (e.g. an `images/` folder). If it doesn't exist yet, create it first.

To open that folder later: Command Palette → **`CPFS: Open Reference Images Folder`**. (When a job is active, you can also use the **Reference images** button in the dashboard's Quick actions.)

---

## Step 5 — Check your install (self-test)

Run the self-test to confirm CPFS is wired up:

1. Command Palette (`Ctrl+Shift+P`).
2. Type and run: **`CPFS: Run Install Self-Test`**
3. The results open in the editor as a file: **`logs/development_history/cpfs-self-test.md`** (a real tab that stays open). On a fresh install or update this same file opens automatically without grabbing focus, so the **Reload Window** toast stays visible.
   - **PASS** → you're ready. A toast also confirms it.
   - **FAIL** → each failing line says what's wrong. Fix those items, then run the self-test again.

On a fresh install, this self-test also runs by itself once and opens the same results file. You can also find the report any time in `logs/development_history/cpfs-self-test.md`.

---

## Step 6 — Start working (the chat-first way)

You do **not** pick jobs in the dashboard for normal work. You work in **Cursor chat** and CPFS tracks the fix.

1. **In Cursor chat**, say two things, **once**:
   - **What to fix.**
   - **How to check it** (the real test — a command, a file to look at, a clear result you can see).
2. CPFS **finds or makes the feature log** from your message. The bottom bar updates, e.g. `CPFS: fix-login-redirect (#1)`.
3. The AI works, logs its tries, and runs the check from the log. You do **not** re-paste the check steps each time — they're agreed once and reused.

Manual way (optional): **`CPFS: Start Feature`** if you want to make the log yourself before chatting.

---

## The daily loop

This is the whole workflow, repeated:

| Step | What you do | What CPFS does |
|------|-------------|----------------|
| **Say the fix + how to check** | One message in chat | Makes/matches the feature log; records TARGET FILES |
| **AI works** | Let it edit inside scope | Blocks or flags saves outside TARGET FILES; puts DO NOT REPEAT into every message |
| **Check it** | `CPFS: Validate Attempt` (or let hooks run it) | Runs your test command; adds the real result to the log |
| **AI says done?** | Close the attempt as Passed | **Close Attempt as Passed** is **blocked** unless a REAL TEST EVIDENCE line points to a file that really exists. When it passes, CPFS auto-creates a **milestone commit** (clean tree) so every passed fix is a comeback point in git |
| **Close the job (owner)** | Depends on task type | **Type 2 (quick fix):** `CPFS: Owner Pass (Type 2)` or the dashboard **Pass** button (with or without an AI artifact). **Type 3 (formal feature):** `CPFS: Confirm Pass` — owner confirms the AI's pass and marks the feature **COMPLETE** |

If the AI is stuck: it writes `### AI EXHAUSTED` in the log; CPFS shows an **Acknowledge AI exhausted** button so you pick the next path (no endless spinning).

---

## The dashboard (a quick look)

**`CPFS: Open CPFS Dashboard`** — one control panel:

- **Top bar** — CPFS on/off toggle, the running job, the version badge.
- **Settings** (collapsible) — your CPFS settings; Save writes `.cpfs/config.json`.
- **Work Queue** — jobs that need attention (failed within 7 days, or new today).
- **All-jobs browser** — every feature log; select a job to see its detail tabs (Requests, Criteria, Tests, Work, Communication, Attempts, Scope). To make a different job the active one, click **Resume** on its row — the **Resume** button only appears for jobs in **Suspended** or **Idle** state (not for Running/Passed/Failed jobs; suspend or abandon those first if needed).
- **CPFS health** — AI Context freshness, Compile status, Snapshot status.
- **Onboarding** — onboarding doc status + actions.
- **Quick actions** — common commands: **Run compile check**, **Open feature log**, **Install self-test**, etc. (some only show when a job is active).

Use it to glance, not to argue fixes. Fixes happen in chat.

---

## Go deeper

- **CPFS rules (free/open):** https://ragbox.llc/tutorials/cpfs.html
- **In-extension reference manual (every command/setting/file):** Command Palette → **`CPFS: Open Reference Manual`**
- **Dashboard buttons:** open the dashboard (**`CPFS: Open CPFS Dashboard`**) and hover any button for a tip

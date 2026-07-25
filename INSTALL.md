# cpfs-mcp — Installation Guide

CPFS Runner as an MCP server. The same guardrail engine (compiled WASM core)
that powers the VS Code / Cursor extension, exposed as MCP tools — so any
MCP-compatible AI client gets the same scoped-task discipline, gated Pass,
heuristic scan, and live dashboard, **without an editor**.

This guide walks you through install, client configuration, verification, and
your first task.

---

## Prerequisites

- **Node.js 20 or newer** — `node --version` must show v20+. The server exits
  immediately on older Node.
- **A git repository** for the project you want CPFS to supervise (the
  pre-commit hook backstop requires git). Not required for the MCP tools
  themselves, only for commit-time enforcement.
- **An MCP-compatible AI client** — Claude Desktop, Claude Code, Cursor,
  Windsurf, Zed, Cline, Roo, Continue, or GitHub Copilot Chat.
- **Do not run the CPFS extension and the cpfs-mcp server in the same workspace at the
  same time.** Only one CPFS runtime may be active per workspace. If the extension is
  enabled in a workspace, cpfs-mcp refuses to start and prints: *"CPFS extension is
  active in this workspace … disable the extension (CPFS: Disable / toggle off) before
  starting the CPFS MCP server."* If cpfs-mcp is already running in a workspace, the
  extension refuses to enable and shows the matching message. This is per-workspace —
  cpfs-mcp in a different workspace (e.g., Claude Desktop elsewhere) does **not**
  conflict with the extension here. Stale locks from a crashed runtime are auto-cleaned.

---

## Step 1 — Install the package

### Option A: From npm (published release)

```bash
npm install -g cpfs-mcp
```

This gives you a global `cpfs-mcp` command and installs the bundled WASM core
+ closed runtime modules. No source code ships — only compiled artifacts.

### Option B: Direct download (tarball)

Download the latest tarball and install it globally:

```bash
# Download from the public mirror (no npm account needed):
curl -L -o cpfs-mcp-latest.tgz https://ragbox.llc/tutorials/cpfs-mcp-latest.tgz
npm install -g ./cpfs-mcp-latest.tgz
```

The `.tgz` is the same artifact `npm pack` produces — compiled WASM core +
closed runtime modules, no source. It is a **free** download; a CPFS Pro
license key (purchased separately) only unlocks unlimited enforcement.

### Option C: From a local tarball (private/internal distribution)

```bash
# In the cpfs-mcp source checkout:
npm pack                       # produces cpfs-mcp-0.2.0.tgz
npm install -g ./cpfs-mcp-0.2.0.tgz
```

### Option C: From the monorepo (development)

```bash
cd cpfs-mcp
npm install
npm run sync-runtime           # refresh runtime/ from cpfs-runner/dist
npm test                       # smoke + MCP + pack self-containment
```

You can then run it directly with `node src/index.js` (no global install needed).

> **WASM is mandatory.** If the required native files are missing or the
> install is corrupt, the server exits immediately and tells you to reinstall.
> It does not run in a degraded mode.

---

## Step 2 — Run the workspace installer

In your project root (the codebase you want CPFS to supervise):

```bash
node /path/to/cpfs-mcp/src/install.js
```

This does two things:

1. **Writes a git pre-commit hook** (`.git/hooks/pre-commit`) that blocks a
   commit if staged files are outside the active feature's TARGET FILES, or
   if staged added lines trip a §13 heuristic signal (numeric thresholds, path
   literals, workaround comments). Override per-commit with
   `CPFS_SCOPE_ALLOW=1` / `CPFS_HEURISTIC_ALLOW=1`, or bypass entirely with
   `git commit --no-verify`.
2. **Prints the MCP client config snippet** for your AI client (see Step 3).

Set `CPFS_WORKSPACE` to point it at a project other than the current directory:

```bash
CPFS_WORKSPACE=/path/to/your/project node /path/to/cpfs-mcp/src/install.js
```

---

## Step 3 — Configure your MCP client

Add `cpfs-mcp` as an MCP server. The entry is the same shape in every client —
only the config file location differs. Set `CPFS_WORKSPACE` to the project root
you want CPFS to supervise.

### Claude Desktop / Claude Code

**macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
**Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "cpfs": {
      "command": "node",
      "args": ["/absolute/path/to/cpfs-mcp/src/index.js"],
      "env": { "CPFS_WORKSPACE": "/absolute/path/to/your/project" }
    }
  }
}
```

For **Claude Code** (CLI), you can also add a project-scoped `.mcp.json` in
your repo root with the same shape.

### Cursor

`.cursor/mcp.json` in your project root (or user-level settings):

```json
{
  "mcpServers": {
    "cpfs": {
      "command": "node",
      "args": ["/absolute/path/to/cpfs-mcp/src/index.js"],
      "env": { "CPFS_WORKSPACE": "${workspaceFolder}" }
    }
  }
}
```

### Windsurf

`~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "cpfs": {
      "command": "node",
      "args": ["/absolute/path/to/cpfs-mcp/src/index.js"],
      "env": { "CPFS_WORKSPACE": "/absolute/path/to/your/project" }
    }
  }
}
```

### Zed

`~/.config/zed/settings.json` (add an `mcp_servers` block):

```json
{
  "mcp_servers": {
    "cpfs": {
      "command": "node",
      "args": ["/absolute/path/to/cpfs-mcp/src/index.js"],
      "env": { "CPFS_WORKSPACE": "/absolute/path/to/your/project" }
    }
  }
}
```

### Cline / Roo

`cline_mcp_settings.json` (Cline) or the Roo equivalent MCP settings file:

```json
{
  "mcpServers": {
    "cpfs": {
      "command": "node",
      "args": ["/absolute/path/to/cpfs-mcp/src/index.js"],
      "env": { "CPFS_WORKSPACE": "/absolute/path/to/your/project" },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

### Continue

`~/.continue/config.json` (add under `mcpServers`):

```json
{
  "mcpServers": {
    "cpfs": {
      "command": "node",
      "args": ["/absolute/path/to/cpfs-mcp/src/index.js"],
      "env": { "CPFS_WORKSPACE": "/absolute/path/to/your/project" }
    }
  }
}
```

### GitHub Copilot Chat (VS Code)

`.vscode/mcp.json` in your project root:

```json
{
  "servers": {
    "cpfs": {
      "command": "node",
      "args": ["/absolute/path/to/cpfs-mcp/src/index.js"],
      "env": { "CPFS_WORKSPACE": "${workspaceFolder}" }
    }
  }
}
```

> After saving any config above, **restart your AI client** (or reload the
> window) so it picks up the new MCP server.

---

## Step 4 — Verify the install

### Check the startup self-test

When the MCP server starts, it prints a self-test report to its stderr log.
In most clients you can see this in the MCP server logs / output panel:

```
CPFS MCP — startup self-test
Result: PASS

[OK] Workspace folder — /path/to/your/project
[OK] .cpfs directory — /path/to/your/project/.cpfs
[OK] Config file — enabled=true
[OK] Feature logs folder — /path/to/your/project/All-logs/factai-main
[OK] Native core (WASM) — cpfs-core/0.1.0/wasm
[OK] MCP package — 0.1.4
[OK] Node runtime — v20.20.2 (>=20 ok)
```

If any check shows `[FAIL]`, fix it before starting a task. Critical failures
(missing workspace, unloadable WASM) exit the server automatically; the
self-test catches the middle ground (read-only workspace, corrupt config,
missing logs dir) so you see a diagnosis instead of a mysterious first-task
failure.

### Open the dashboard

The server runs an embedded localhost dashboard — the same cockpit the
extension shows, fed with live data from your feature logs. Open it in any
browser:

```
http://127.0.0.1:8765/                       # cockpit (live feature, target files, decision actions)
http://127.0.0.1:8765/heuristic              # §13 heuristic report across the workspace
http://127.0.0.1:8765/formal-feature          # Formal Feature (Type 3) criteria editor
http://127.0.0.1:8765/legacy-crossings?slug=  # blast-radius report + coverage-task creation
```

Set `CPFS_DASHBOARD_PORT=0` in the server env to disable the dashboard.

### Confirm the tools are visible

In your AI client, the CPFS tools should appear (most clients list MCP server
tools in a panel or via a `/mcp` command). The full extension-parity toolset
covers lifecycle & gating (`cpfs_status`, `cpfs_start_feature`,
`cpfs_end_attempt_success`, …), workspace setup (`cpfs_enable_workspace`,
`cpfs_install_mandatory_rule`), core enforcement (`cpfs_validate_attempt`,
`cpfs_run_all_checks`, `cpfs_restore_attempt_checkpoint`,
`cpfs_export_report`, zones, lint), the onboarding suite
(`cpfs_generate_onboarding`, `cpfs_verify_onboarding`, …), formal features
(`cpfs_start_formal_feature`, `cpfs_upgrade_to_type3`,
`cpfs_edit_criteria`), and legacy crossings
(`cpfs_analyze_legacy_crossings`).

---

## Step 5 — Your first task

1. **Start a feature** — ask the AI:
   > "Start a CPFS feature called `fix-login-redirect`. Target files:
   > `src/auth/login.ts`, `src/auth/redirect.ts`. Success: login redirects
   > to the originally requested URL after auth."

   The AI calls `cpfs_start_feature`, which creates a feature log under
   `All-logs/<history>/fix-login-redirect.log` with TARGET FILES, success
   criteria, and DO NOT REPEAT sections.

2. **Agree the verification method** — before coding, agree how success will be
   tested. The feature log has a `### VERIFY BY` section with each outcome's
   method and status set to `pending`. Ask the AI:
   > "Agree verify: run `npm test -- src/auth` and check all tests pass.
   > Artifact at `logs/verify/login-redirect.txt`."

   The AI calls `cpfs_agree_verify`, which flips every `**Status:** pending` to
   `**Status:** agreed` and records the method. **Pass is blocked until this is
   done** — the gate refuses `cpfs_end_attempt_success` with "Verification not
   agreed" if you skip this step.

3. **Work on the task** — the AI edits the target files. If it edits anything
   outside TARGET FILES, the scope watcher flags it as out-of-scope pending
   (visible in the dashboard), and Pass is soft-blocked until you Keep or Revert
   each flagged file.

4. **Record real-test evidence** — the AI must capture proof it actually ran the
   changed code:
   > "Record evidence: `npm test -- src/auth` → all tests pass.
   > Artifact: `logs/verify/login-redirect.txt`."

   This calls `cpfs_record_evidence`, appending a `REAL TEST EVIDENCE:` line to
   the feature log.

5. **Pass the task** — the AI calls `cpfs_end_attempt_success`. This is the
   **only** way to mark a task done, and it is gated server-side:
   - Refuses if free-tier is exhausted (10/month cap without a Pro license).
   - Refuses if VERIFY BY is not agreed (Step 2 above).
   - Refuses if there is no real-test evidence.

   The AI **cannot bypass this** — it is a server-side tool call, not chat
   text. You confirm the final result in the dashboard (Pass / Fail buttons).

---

## Auto-injected context (no editor hooks needed)

The MCP server keeps every AI client's auto-read rules file fresh whenever the
feature log changes — so the AI sees current FEATURE / TARGET FILES /
DO NOT REPEAT / REAL TEST BEFORE PASS context on every turn, without editor
hooks. Supported rules files:

| Client | Rules file |
|--------|-----------|
| Cursor | `.cursor/rules/cpfs-active.mdc` (alwaysApply) |
| Claude Code, Zed, generic | `AGENTS.md` |
| Claude Code | `CLAUDE.md` |
| Cline | `.clinerules` |
| Roo | `.roorules` |
| Windsurf | `.windsurfrules` |
| GitHub Copilot | `.github/copilot-instructions.md` |
| Aider | `CONVENTIONS.md` |

---

## Troubleshooting

### "CPFS extension is active in this workspace … disable the extension before starting the CPFS MCP server"

The CPFS Runner **extension** (Cursor/VS Code) is enabled in the same workspace
you're pointing cpfs-mcp at. Only one CPFS runtime may be active per workspace
at a time — running both makes them fight over the active task and the injected
rules files. Disable the extension in that workspace (CPFS: Disable / toggle off)
and start cpfs-mcp again. (Per-workspace: cpfs-mcp in a different workspace does
not conflict with the extension here.) If the extension already exited but the
message persists, the lock is stale — delete `.cpfs/cpfs-extension.lock` and retry.

### "CPFS MCP cannot start: required files are missing or this install looks corrupt"

The WASM core or closed runtime modules are missing. Reinstall the package
(`npm install -g cpfs-mcp` or re-pack the tarball). The server does not run in
a degraded mode — by design.

### "EADDRINUSE: address already in use 127.0.0.1:8765"

Another process (often a previous cpfs-mcp instance) is using the dashboard
port. Either stop it, or set `CPFS_DASHBOARD_PORT=8766` (or `0` to disable the
dashboard) in the server env.

### Self-test shows `[FAIL] Config file — corrupt JSON`

`.cpfs/config.json` is not valid JSON. Fix it manually, or delete it — the
server recreates a default config on the next task start.

### Self-test shows `[FAIL] .cpfs directory — cannot create`

The workspace path is read-only or you lack write permissions. CPFS needs to
create `.cpfs/` and the logs directory in the workspace root. Fix permissions
or point `CPFS_WORKSPACE` at a writable checkout.

### Tools don't appear in my AI client

- Confirm the config file path is correct for your client (see Step 3).
- Use **absolute** paths in `args` and `env` (not `~` or relative).
- Restart the client / reload the window after editing the config.
- Check the client's MCP server status panel for connection errors.

### The pre-commit hook blocks my commit

The hook blocks commits with staged files outside TARGET FILES, or with §13
heuristic signals. To override for a specific commit:
- `CPFS_SCOPE_ALLOW=1 git commit -m "..."` — allow out-of-scope files.
- `CPFS_HEURISTIC_ALLOW=1 git commit -m "..."` — allow heuristic signals.
- `git commit --no-verify -m "..."` — bypass the hook entirely.

---

## Updating

Reinstall the package and re-run the workspace installer:

```bash
npm install -g cpfs-mcp          # or re-pack + npm install -g ./cpfs-mcp-*.tgz
node /path/to/cpfs-mcp/src/install.js   # refreshes the git hook + prints config
```

Then restart your AI client. The startup self-test confirms the new version.

---

## Uninstalling

```bash
npm uninstall -g cpfs-mcp
rm .git/hooks/pre-commit          # remove the git hook
```

The `.cpfs/` directory and `All-logs/` feature logs remain in your project —
they are your task history. Delete them if you no longer want the audit trail.

---

## License

CPFS Runner is a paid product with a free tier (10 task Passes per month).
Enter a Pro license key to remove the limit:

> "Enter CPFS license key: `CPFS-PRO-XXXX-XXXX-XXXX`"

This calls `cpfs_enter_license`, which verifies the key offline via the WASM
core (ed25519). No network call is made. The key is stored in
`.cpfs/config-mcp.json` (the MCP server's own config, separate from the
extension's `.cpfs/config.json`). If you run both the extension and the MCP
server in different workspaces, enter the key once per runtime.

---

## Questions?

- **Full feature map** (extension vs MCP): see
  [`../marketing/cpfs-runner/MCP_SERVER_FEATURE_MAP.md`](../marketing/cpfs-runner/MCP_SERVER_FEATURE_MAP.md)
- **Thin plugin contract** (how the MCP server reuses the extension engine):
  see [`THIN_PLUGIN_CONTRACT.md`](THIN_PLUGIN_CONTRACT.md)
- **Gate parity** (which extension gates are enforced server-side): see
  [`GATE_PARITY.md`](GATE_PARITY.md)

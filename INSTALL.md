# CPFS Runner — Install

## Cursor / VS Code — Marketplace

Search **CPFS Runner** by publisher **ragbox** on the [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=ragbox.cpfsrunner).

## Direct download (latest)

Prefer the marketplace? Grab the latest build directly — always the current version, served with no-cache headers:

- **Extension (VSIX):** [https://ragbox.llc/tutorials/cpfs-runner-latest.vsix](https://ragbox.llc/tutorials/cpfs-runner-latest.vsix) (currently `1.1.20`)
- **MCP server (tarball):** [https://ragbox.llc/tutorials/cpfs-mcp-latest.tgz](https://ragbox.llc/tutorials/cpfs-mcp-latest.tgz) (currently `1.1.20`)

Then install the VSIX: **Extensions** → **…** → **Install from VSIX…** → choose the file → **Reload Window**.

## MCP server (npm)

The MCP server is also on npm:

```bash
npm install -g cpfs-mcp
```

…or install the tarball directly: `npm install -g ./cpfs-mcp-latest.tgz`.

## VSIX (build it yourself)

Requires **Node 20+** (`vsce` fails on Node 18). On Ubuntu 24.04, use **nvm** (installed side-by-side; does not replace apt Node 18):

```bash
export NVM_DIR="$HOME/.nvm" && . "$NVM_DIR/nvm.sh"
nvm use 20   # or: nvm install 20

cd /home/rgesaah/factai/cpfs-runner
npm install
npm run package:vsix
```

Output: `cpfsrunner-1.1.20.vsix` (version matches `package.json`).

**License:** CPFS Runner is proprietary — paid subscription required for production use. See `LICENSE` in the package. The Extensions panel shows **SEE LICENSE IN LICENSE**, not MIT.

Install in Cursor: **Extensions** → **…** → **Install from VSIX…** → choose the file → **Reload Window**.

### Quick install check (after VSIX)

1. Open any project folder.
2. **Ctrl+Shift+P** → **CPFS: Run Install Self-Test** (or open **CPFS Dashboard** → **Run install self-test**).
3. **PASS** toast + details in the **CPFS Self-Test** output panel means the extension, workspace config, logs folder, zones, and test command are wired up.

Default test command if unset: `echo CPFS install ok`. Set **CPFS Runner → Test Command** in Settings to your repo’s command (e.g. `npm test`) before relying on validate/success flows.

Pre-release flag: `npm run package:beta` (same script with `--pre-release`).

## Local dev (FactAI repo — daily)

No VSIX needed; compiles and links into Cursor:

```bash
/home/rgesaah/factai/cpfs-runner/scripts/install-local.sh
```

Then **Developer: Reload Window**.

## Verify

```bash
cd cpfs-runner
npm run test:smoke
```

## Docs

- [CPFS methodology](https://ragbox.llc/tutorials/cpfs.html)
- `README.md` — product overview
- `CHANGELOG.md` — version history

# Xnurta MCP Installation Guide

> **🤖 To AI Agents: if you are executing this guide on behalf of a user, "configure MCP" and "install Skills" are one single task.** After the MCP connection is verified, you MUST proceed to install the 3 required Skills (the [`skills/`](../skills/) directory — no need to ask the user), then introduce each of the 4 Skills marked optional in that directory and ask the user whether to install them. Configuring the connection without installing Skills means the task is incomplete.

Four steps: **get a token → configure MCP → install Skills → verify**. About 5 minutes in total.

> Prerequisite: an **AI assistant that supports MCP**. The simplest and most recommended is **Claude** (Desktop app or Code CLI); ChatGPT Codex, OpenClaw, Hermes, Cursor, Cline, Cherry Studio, and other MCP-compatible clients are also supported.

---

## Step 1 · Get your token

> ⚠️ Your token determines which stores and data you can query — keep it safe and never share it.

1. Log in to the Xnurta console, open the account menu in the top-right corner, and go to **MCP & Skills**.
2. Click **Create Token** and choose the validity period, authorization scope (e.g. Amazon SA), store range, and data permissions you need.
3. **Copy and save the token immediately** — it cannot be viewed again after you close the dialog; if lost, create a new one.

---

## Step 2 · Configure MCP in your AI assistant

**It comes down to one thing**: give your assistant two values — the Server URL and your token (as an `Authorization: Bearer <token>` header).

- **Server URL**: `https://mcp.xnurta.com/mcp`
- **Transport**: Streamable HTTP

### Option 1 · One-message setup (recommended)

In any client whose AI can configure things itself (Claude, ChatGPT Codex, Cursor, etc.), send:

```text
Set up Xnurta MCP and install its companion Skills. Complete these steps in order:
1. Add the MCP server: URL https://mcp.xnurta.com/mcp (Streamable HTTP),
   header Authorization: Bearer <your token>
2. Call get_user_authorized_context to verify — a store list means success
3. From the skills/ directory of https://github.com/Xnurta/MCP, install the
   3 required Skills: query-ads-performance, query-entity-metadata, query-operation-log
4. Introduce the 4 Skills marked optional under skills/ and ask me which to install
```

It will configure, verify, and install the required Skills automatically — a list of your authorized stores means the connection succeeded.

- **Claude**: open Claude → switch to the **Code tab** → paste the message above.
- **ChatGPT Codex**: send the message to Codex directly (it writes to `~/.codex/config.toml`).

If your client doesn't let the AI configure itself, follow the manual setup below.

### Option 2 · Plugin installation

#### Desktop apps (recommended)

##### Claude Desktop · Code mode

1. Select **Customize** in the left sidebar.
2. In the modal, select **Plugins** in the lower-left corner, then select **Add → Add marketplace** in the upper-right corner.
3. Select **Add from a repository** and enter `Xnurta/MCP`.
4. In **Plugins**, find the marketplace you added and install **Xnurta MCP**. Installation enables both the MCP server and Skills.

> **Note:** Claude Desktop Code mode does not support the `/plugin` command. It is available only in the Claude Code CLI; use the desktop Plugin manager instead.

##### ChatGPT Desktop · Codex

1. Select **Plugins** in the left sidebar.
2. Select **Add → Add a marketplace** in the upper-right corner.
3. Enter `Xnurta/MCP` and add the marketplace.
4. In **Plugins**, find the marketplace you added and install **Xnurta MCP**. Installation enables both the MCP server and Skills.

> **Note:** ChatGPT Desktop Codex does not support `/plugin` or `/plugins`. `/plugins` is available only in Codex CLI; use Plugins in the left sidebar of the desktop app.

Desktop UI references: [Claude Desktop Code mode](https://code.claude.com/docs/en/desktop#install-plugins) · [ChatGPT / Codex Plugins](https://learn.chatgpt.com/docs/plugins)

#### Command-line CLI

##### Claude Code CLI

```text
/plugin marketplace add Xnurta/MCP
/plugin install xnurta-mcp@xnurta
```

##### Codex CLI

```bash
codex plugin marketplace add Xnurta/MCP
codex plugin add xnurta-mcp@xnurta
```

The plugin installs the MCP server and all 7 Skills together. When installation is complete, set `XNURTA_TOKEN` and start a new session. See [Environment variable setup](#environment-variable-setup-xnurta_token).

### Option 3 · Manual setup

> ⚠️ **Configured ≠ done**: after adding the MCP server, be sure to continue with [Step 3 · Install Skills](#step-3--install-skills). The 3 required Skills directly determine query accuracy.

#### Claude Code CLI / the Code tab in Claude Desktop

```bash
claude mcp add --transport http xnurta-mcp https://mcp.xnurta.com/mcp --header "Authorization: Bearer <YOUR_TOKEN>"
```

Keep the default **local scope** (current project folder only) so the token doesn't leak into other projects.

#### Claude Desktop (Chat, when the UI has no Bearer option)

Edit `claude_desktop_config.json` (Windows: `%APPDATA%\Claude\`; macOS: `~/Library/Application Support/Claude/`), wrap the server with `mcp-remote`, then restart:

```json
{
  "mcpServers": {
    "xnurta-mcp": {
      "command": "npx",
      "args": ["mcp-remote", "https://mcp.xnurta.com/mcp", "--header", "Authorization: Bearer ${XNURTA_TOKEN}"],
      "env": { "XNURTA_TOKEN": "<your token>" }
    }
  }
}
```

#### ChatGPT Codex (`~/.codex/config.toml`)

```toml
[mcp_servers.xnurta-mcp]
url = "https://mcp.xnurta.com/mcp"
bearer_token_env_var = "XNURTA_TOKEN"
http_headers = {}
```

#### OpenClaw

```bash
openclaw mcp add xnurta-mcp \
  --url https://mcp.xnurta.com/mcp \
  --transport streamable-http \
  --header "Authorization: Bearer $XNURTA_TOKEN"
```

> ⚠️ Older versions of OpenClaw have a bug where streamable-http does not forward custom Authorization headers (fixed since roughly late April 2026). If you get a 401, upgrade to the latest version first.

#### Hermes (`~/.hermes/config.yaml`)

```yaml
mcp_servers:
  xnurta-mcp:
    url: "https://mcp.xnurta.com/mcp"
    headers:
      Authorization: "Bearer ${XNURTA_TOKEN}"
```

Then run `/reload-mcp` inside Hermes.

#### Other MCP clients (Cursor, Cline, Cherry Studio, etc.)

Add a remote / Streamable HTTP server in the client's MCP settings with the URL above and an `Authorization: Bearer <YOUR_TOKEN>` header.

### Environment variable setup (`XNURTA_TOKEN`)

Plugin installations and some manual client configurations read the token from `XNURTA_TOKEN`.

#### macOS / Linux

For the current terminal session:

```bash
export XNURTA_TOKEN=<your-token>
```

To persist it, add the export to your shell profile such as `~/.zshrc` or `~/.bashrc`.

#### Windows

For the current PowerShell session:

```powershell
$env:XNURTA_TOKEN = "<your-token>"
```

To persist it for your user account:

```powershell
[System.Environment]::SetEnvironmentVariable("XNURTA_TOKEN", "<your-token>", "User")
```

Restart Claude Code or Codex after setting a persistent environment variable. Never commit the token or paste it into a marketplace manifest.

### Verify the connection

Ask your assistant to call `get_user_authorized_context`:

| Result | Meaning |
|--------|---------|
| Returns your userId and authorized profileIds | ✅ Configured successfully |
| Returns 401 | Token invalid or missing permissions — create a new one |
| Timeout | Check your network |

---

## Step 3 · Install Skills

Skills live in this repo's [`skills/`](../skills/) directory, in two tiers:

**Required (3)** — without them, MCP query error rates rise significantly:

- `query-ads-performance` — ad performance queries
- `query-entity-metadata` — entity configuration queries
- `query-operation-log` — operation log queries

**Optional (4)** — advanced analysis scenarios; add as needed after the required ones (under [`skills/`](../skills/)):

- `weekly-ads-report` — weekly ads report
- `monthly-ads-report` — monthly ads report
- `ads-structure-analysis` — ad structure analysis
- `product-diagnosis` — product diagnosis

Pick the installation route that matches your assistant:

- **Agents that can act on their own** (Claude Code, ChatGPT Codex, Cursor, etc.): point the AI at the skill directories and say "install the 3 required Skills"; add optional Skills as needed.
- **Chat / UI-based assistants** (Claude web or desktop app, etc.): add them manually in each client's settings (in Claude, for example: Settings → Skills → upload, one by one).

---

## Step 4 · Start querying

Four moves: 1) ask the assistant to list your authorized stores → 2) pick a store → 3) ask in natural language → 4) take the results (export tables / charts / drop into a report).

**Example prompts:**

- Performance:
  - "Top 10 campaigns by spend last week, with ACOS."
  - "This product line's TACOS trend over the past 8 weeks."
  - "The 5 campaigns with the biggest ACOS increase this week vs last week."
- Configuration:
  - "List all enabled SP campaigns with their daily budgets."
  - "This ASIN's inventory, title, and ad-type eligibility."
- Operation logs:
  - "AI automatic bid adjustments in the past 7 days."
  - "Who changed this campaign's budget, and when?"

**Going further**: have the assistant combine multiple queries into a weekly report, export CSV/Excel, or calculate true profit against your costs/goals; turn frequent prompts into reusable templates.

---

## FAQ

**Q: Getting 401 Unauthorized?**
The token is expired, incompletely copied, or missing permissions. Create a new one on the MCP & Skills page, making sure the right stores and data permissions are selected. OpenClaw users on old versions should upgrade the client first.

**Q: How far back can I query?**
Roughly the most recent 15 months of performance data and operation logs; performance data granularity is daily.

**Q: Is the data real-time?**
It follows the Xnurta platform's update cadence — not second-level real-time.

**Q: Which stores can I query?**
The same ones your Xnurta account (main / sub-account) can access — a token's visibility never exceeds your own account permissions.

**Q: Can this version change budgets / adjust bids?**
No. v1.0.0 is read-only; write capabilities (managed-group management, bulk operations, campaign creation) will roll out in future releases.

# Xnurta MCP Installation Guide

> **🤖 To AI Agents: if you are executing this guide on behalf of a user, "configure MCP" and "install Skills" are one single task.** After the MCP connection is verified, you MUST proceed to install the 3 required Skills (the [`skills/`](../skills/) directory — no need to ask the user), then introduce each of the 4 Skills marked optional in that directory and ask the user whether to install them. Configuring the connection without installing Skills means the task is incomplete.

Four steps: **choose an authorization method → connect and verify MCP → install Skills → start using it**. About 5 minutes in total.

> Prerequisite: an **AI assistant that supports MCP**. The simplest and most recommended is **Claude** (Desktop app or Code CLI); ChatGPT Codex, OpenClaw, Hermes, Cursor, Cline, Cherry Studio, and other MCP-compatible clients are also supported.

---

## Step 1 · Choose an authorization method

Xnurta MCP supports OAuth and MCP Token authentication. Both methods use the same Server URL and MCP Tools.

### Option A · OAuth (recommended)

Use OAuth with clients that support MCP OAuth. The client opens an Xnurta sign-in and authorization page, so you do not need to create, copy, or store a Token. After authorization, the client maintains the connection automatically.

### Option B · MCP Token

Use an MCP Token when the client does not support OAuth, for automation scripts, or when your organization requires a fixed credential.

1. Log in to the Xnurta console, open the account menu in the top-right corner, and go to **MCP & Skills**.
2. Click **Create Token** and choose the validity period, authorization scope, store range, and data permissions you need.
3. **Copy and save the Token immediately.** It cannot be viewed again after you close the dialog; if lost, create a new one.

> ⚠️ An MCP Token is an access credential. Do not share it or paste it into public conversations, documents, or source-code repositories.

---

## Step 2 · Configure MCP in your AI assistant

Both authorization methods use the same MCP Server. Prefer OAuth; use an MCP Token when the client does not support OAuth or the user explicitly requests it.

- **Server URL**: `https://mcp.xnurta.com/mcp`
- **Transport**: Streamable HTTP

### Option 1 · One-message setup (recommended)

In any client whose AI can configure things itself (Claude, ChatGPT Codex, Cursor, etc.), send:

```text
Please read the following GitHub repository and follow its instructions to configure Xnurta MCP and install the Skills:
https://github.com/Xnurta/MCP

Please use OAuth as the preferred connection method. When authorization is required, open the sign-in page so I can complete the authorization.
If the current client does not support OAuth, prompt me to use an MCP Token instead.

After the configuration is complete, call get_user_authorized_context to verify the connection.
```

> **🤖 To AI Agents:** When executing the setup above, write the MCP configuration to the correct config file for your client. Be careful to distinguish between different config file names used by different clients (e.g. `mcp.json`, `.mcp.json`, `mcp_config.json`, `config.toml`, `config.yaml`, etc.) — do not confuse them.

The assistant will configure MCP, open the OAuth sign-in page, verify the connection, and install the required Skills. A list of your authorized stores means the connection succeeded.

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

The plugin installs the MCP server and all 7 Skills together. When installation is complete, follow the client prompt to open the sign-in page and authorize with OAuth. To use an MCP Token, follow the Token configuration below.

### Option 3 · Manual setup

> ⚠️ **Configured ≠ done**: after adding the MCP server, be sure to continue with [Step 3 · Install Skills](#step-3--install-skills). The 3 required Skills directly determine query accuracy.

> **🤖 To AI Agents:** The examples below cover multiple clients, each with a different config file name and format. Pick the one that matches the user's actual client and write to the correct file — do not confuse config files from different clients.

#### OAuth (recommended)

##### Claude Code CLI / the Code tab in Claude Desktop

```bash
claude mcp add --transport http xnurta-mcp https://mcp.xnurta.com/mcp
```

After adding the server, enter `/mcp`, select `xnurta-mcp`, and follow the prompt to authorize in your browser.

##### ChatGPT Codex

```bash
codex mcp add xnurta-mcp --url https://mcp.xnurta.com/mcp
codex mcp login xnurta-mcp
```

The second command opens the Xnurta sign-in page. Complete authorization and return to Codex.

##### Other MCP clients that support OAuth

Add a remote or Streamable HTTP MCP server with this URL:

```text
https://mcp.xnurta.com/mcp
```

Select **OAuth**, **Sign in**, or **Authenticate**, depending on the wording used by your client, and complete authorization in your browser.

#### MCP Token

##### Claude Code CLI / the Code tab in Claude Desktop

```bash
claude mcp add --transport http xnurta-mcp https://mcp.xnurta.com/mcp --header "Authorization: Bearer <YOUR_TOKEN>"
```

Keep the default **local scope** (current project folder only) so the token doesn't leak into other projects.

##### Claude Desktop (Chat, when the UI has no Bearer option)

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

##### ChatGPT Codex (`~/.codex/config.toml`)

```toml
[mcp_servers.xnurta-mcp]
url = "https://mcp.xnurta.com/mcp"
bearer_token_env_var = "XNURTA_TOKEN"
http_headers = {}
```

##### OpenClaw

```bash
openclaw mcp add xnurta-mcp \
  --url https://mcp.xnurta.com/mcp \
  --transport streamable-http \
  --header "Authorization: Bearer $XNURTA_TOKEN"
```

> ⚠️ Older versions of OpenClaw have a bug where streamable-http does not forward custom Authorization headers (fixed since roughly late April 2026). If you get a 401, upgrade to the latest version first.

##### Hermes (`~/.hermes/config.yaml`)

```yaml
mcp_servers:
  xnurta-mcp:
    url: "https://mcp.xnurta.com/mcp"
    headers:
      Authorization: "Bearer ${XNURTA_TOKEN}"
```

Then run `/reload-mcp` inside Hermes.

##### Other MCP clients (Cursor, Cline, Cherry Studio, etc.)

Add a remote / Streamable HTTP server in the client's MCP settings with the URL above and an `Authorization: Bearer <YOUR_TOKEN>` header.

### Environment variable setup (`XNURTA_TOKEN`)

This environment variable is required only when you use an MCP Token. Codex, Hermes, and other clients can read the Token from `XNURTA_TOKEN` so the credential does not need to be written directly in a configuration file.

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

After configuring OAuth or an MCP Token, ask your assistant to call `get_user_authorized_context`:

| Result | Meaning |
|--------|---------|
| Returns your userId and authorized profileIds | ✅ Configured successfully |
| Client prompts you to sign in or authorize | OAuth is not complete; open the page and finish authorization |
| Returns 401 | OAuth expired or was revoked, or the MCP Token is invalid, expired, disabled, or missing permissions |
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
First confirm which authorization method you are using. OAuth users should sign in again and confirm that access has not been revoked. MCP Token users should check whether the Token is complete, active, unexpired, and authorized for the required permissions. OpenClaw users on old versions should upgrade the client first.

**Q: How far back can I query?**
Roughly the most recent 15 months of performance data and operation logs; performance data granularity is daily.

**Q: Is the data real-time?**
It follows the Xnurta platform's update cadence — not second-level real-time.

**Q: Which stores can I query?**
The available stores depend on your Xnurta account permissions and the scope granted through OAuth or the MCP Token. The final visible scope never exceeds your account permissions.

**Q: Can this version change budgets / adjust bids?**
No. v1.0.0 is read-only; write capabilities (managed-group management, bulk operations, campaign creation) will roll out in future releases.

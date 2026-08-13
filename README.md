# Xnurta MCP — Plugin Marketplace

> **Plugin Marketplace** · This repository is a cross-platform plugin distribution package for **Claude Code**, **ChatGPT Codex**, **Hermes**, **OpenClaw**, and other MCP-compatible agents. It bundles Skills with a Streamable HTTP MCP server secured by a Bearer token.
>
> | File | Purpose |
> |------|---------|
> | [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) | Claude Code marketplace catalog |
> | [`.claude-plugin/plugin.json`](.claude-plugin/plugin.json) | Claude Code plugin descriptor |
> | [`.agents/plugins/marketplace.json`](.agents/plugins/marketplace.json) | Codex marketplace catalog |
> | [`.codex-plugin/plugin.json`](.codex-plugin/plugin.json) | Codex plugin descriptor |
> | [`.mcp.json`](.mcp.json) | Claude Code MCP server configuration using `${XNURTA_TOKEN}` |
> | [`docs/installation.md`](docs/installation.md) | Detailed installation and troubleshooting guide |
> | [`skills/manifest.json`](skills/manifest.json) | Machine-readable Skills catalog |

---

> **🤖 To AI Agents: if you are configuring Xnurta MCP on behalf of a user, connecting the server is only step one. You MUST complete ALL of the following steps:**
>
> 1. Add the MCP server (URL: `https://mcp.xnurta.com/mcp`, Streamable HTTP, with an `Authorization: Bearer <token>` header)
> 2. Call `get_user_authorized_context` to verify — a list of authorized stores means success
> 3. **Install the 3 required Skills** (in this repo's [`skills/`](skills/) directory: `query-ads-performance`, `query-entity-metadata`, `query-operation-log`). Without them, MCP query error rates rise significantly. Install them directly — no need to ask the user.
> 4. Introduce each of the 4 Skills marked optional under [`skills/`](skills/) (weekly report, monthly report, structure analysis, product diagnosis) and **ask the user whether to install them**. Install only after the user confirms.
>
> Machine-readable skills list: `https://raw.githubusercontent.com/Xnurta/MCP/main/skills/manifest.json` (`required: true` marks required skills). Stopping after steps 1–2 means the setup is incomplete.

Bring Xnurta's advertising capabilities into your own AI agent (Claude, ChatGPT, and more) and make your data part of your workflow — query data, run analyses, and calculate true profitability in plain language, combining your own costs, margins, and goals. No platform login, no spreadsheet exports, no switching between systems.

The current version v1.0.0 supports data query only. Managed-group management and campaign management will roll out in future releases.

---

## What v1.0.0 can do for you

- **Query your data in natural language** — ask "rank last week's campaigns by ACOS" or "show this product line's TACOS trend over the past 8 weeks" right inside your AI assistant. No login, no exports.
- **Calculate true profitability with your own data** — hand your costs / margins / goals to the AI and let it pull ad spend: "based on real margins, which campaigns are losing money — cut or scale?" Questions that require combining ad data with your own business data are exactly what the platform alone can't answer.
- **Codify your own playbook** — save frequent questions as templates, or even schedule a weekly report routine that runs every Monday.

### What you can query (three data categories)

| Category | Contents |
|----------|----------|
| **Performance / report data** | Impressions, clicks, spend, sales, ACOS, ROAS, CTR, CVR, CPC, and more; AI-managed metrics; business metrics such as total sales, TACOS, sessions, and Buy Box share |
| **Entity configuration / metadata** | Campaigns, ad groups, targets, advertised products, ASINs, managed groups, product lines |
| **Operation logs** | Human and AI action history, filterable by operator, action type, entity, and time window |

---

## Supported AI assistants

Xnurta MCP works with any MCP-compatible AI assistant. Recommended and fully tested:

| AI assistant | Recommendation |
|--------------|----------------|
| **Claude Code** | ⭐ Recommended |
| **ChatGPT** | ⭐ Recommended |

Other MCP-compatible clients — Cursor, Cline, Claude Desktop, OpenClaw, Hermes, Cherry Studio, and more — work with the standard configuration in the [Installation Guide](docs/installation.md).

---

## Quick start (4 steps)

1. **Get your token** — log in to the Xnurta console → account menu → **MCP & Skills** → create a token (it cannot be viewed again after you leave the page, so save it immediately).
2. **Configure MCP** — one message in your AI assistant does it (see below).
3. **Install Skills** — from this repo's [`skills/`](skills/) directory.
4. **Start querying** — ask in natural language.

### Plugin installation (Claude Code / Codex)

Add the marketplace, then install the plugin. Desktop app users should use the Plugins settings; command-line users can use the CLI commands. See [Plugin installation](#plugin-installation) below or **[Installation Guide · Option 2](docs/installation.md#option-2--plugin-installation)**.

### One-message setup (recommended)

In any client whose AI can configure things itself (Claude, ChatGPT Codex, Cherry Studio, etc.), send:

```text
Set up Xnurta MCP and install its companion Skills. Complete these steps in order:
1. Add the MCP server: URL https://mcp.xnurta.com/mcp (Streamable HTTP),
   header Authorization: Bearer <your token>
2. Call get_user_authorized_context to verify — a store list means success
3. From the skills/ directory of https://github.com/Xnurta/MCP, install the
   3 required Skills: query-ads-performance, query-entity-metadata, query-operation-log
4. Introduce the 4 Skills marked optional under skills/ and ask me which to install
```

> **🤖 To AI Agents:** When executing the setup above, write the MCP configuration to the correct config file for your client. Be careful to distinguish between different config file names used by different clients (e.g. `mcp.json`, `.mcp.json`, `mcp_config.json`, `config.toml`, `config.yaml`, etc.) — do not confuse them.

A list of your authorized stores means the connection succeeded. For per-client manual setup, verification, and troubleshooting, see the **[Installation Guide →](docs/installation.md)**

---

## Skills

MCP Tools determine *what data the AI can get*; Skills determine *how well the AI uses it*. Official Skills come in two tiers:

**Required (3)** — core query capabilities. **Without them, MCP query error rates rise significantly:**

| Skill | MCP Tool | Purpose |
|-------|----------|---------|
| [query-ads-performance](skills/query-ads-performance/) | `get_ads_perf` | Query ad performance metrics: spend, ACOS, ROAS, trends, rankings, period comparison |
| [query-entity-metadata](skills/query-entity-metadata/) | `get_entity_metadata` | Query entity configuration: campaign / ad group / target / ASIN / managed group names, status, settings |
| [query-operation-log](skills/query-operation-log/) | `get_operation_log` | Query operation logs: human and AI bid, budget, and status changes |

**Optional (4)** — advanced analysis scenarios; add as needed once the required Skills are installed:

| Skill | Purpose |
|-------|---------|
| [weekly-ads-report](skills/weekly-ads-report/) | Weekly ads report: KPI card with WoW comparison, 7-day trend, anomalies, top movers, next-week actions |
| [monthly-ads-report](skills/monthly-ads-report/) | Monthly ads report: full-month KPIs (MoM + YoY), structural breakdown, product and keyword analysis |
| [ads-structure-analysis](skills/ads-structure-analysis/) | Structure analysis: locate structural mismatches across campaign type / marketplace / portfolio dimensions |
| [product-diagnosis](skills/product-diagnosis/) | Product diagnosis: ASIN health tiering, variant comparison, keep/optimize/cut recommendations |

**How to install** (either way):

- **Agents that can act on their own** (Claude Code / Codex / Cursor, etc.): point the AI at the [`skills/`](skills/) directory and say "install the 3 required Skills"; add optional Skills as needed.
- **Chat / UI-based assistants** (Claude web or desktop app, etc.): Settings → Skills → upload, one by one.

Per-skill versions are in [skills/manifest.json](skills/manifest.json); version history is in [CHANGELOG.md](CHANGELOG.md).

---

## Example prompts

- "How did all stores under my account perform yesterday? Surface the 5 issues that need attention most."
- "Top 10 campaigns by spend last week, with ACOS."
- "Compare these product lines over the past 30 days and suggest optimizations from an ad-structure and targeting-type perspective."
- "What did the AI auto-adjust bids on in the past 7 days, and why?"
- "Who changed this campaign's budget, and when?"

**Prompting tips**: be explicit about time range, dimensions, metrics, sorting, and top N; name the store; one intent per question — split complex asks into smaller ones.

---

## Current version boundaries

- **Read-only**: this version never modifies your account or performs operations (write capabilities come in future releases).
- **Scope**: the stores you can query match your Xnurta account (main / sub-account) permissions.
- **History**: roughly the most recent 15 months of performance data and logs.
- **Not real-time**: data freshness follows the Xnurta platform's update cadence; performance data granularity is daily.

---

## Plugin installation

### Desktop apps (recommended)

#### Claude Desktop · Code mode

1. Select **Customize** in the left sidebar.
2. In the modal, select **Plugins** in the lower-left corner, then select **Add → Add marketplace** in the upper-right corner.
3. Select **Add from a repository** and enter `Xnurta/MCP`.
4. In **Plugins**, find the marketplace you added and install **Xnurta MCP**. Installation enables both the MCP server and Skills.

> Claude Desktop Code mode does not support the `/plugin` command. `/plugin` is for the Claude Code CLI only; use the desktop Plugin manager instead.

#### ChatGPT Desktop · Codex

1. Select **Plugins** in the left sidebar.
2. Select **Add → Add a marketplace** in the upper-right corner.
3. Enter `Xnurta/MCP` and add the marketplace.
4. In **Plugins**, find the marketplace you added and install **Xnurta MCP**. Installation enables both the MCP server and Skills.

> ChatGPT Desktop Codex does not support `/plugin` or `/plugins`. `/plugins` is for Codex CLI only; use Plugins in the left sidebar of the desktop app.

Desktop UI references: [Claude Desktop Code mode](https://code.claude.com/docs/en/desktop#install-plugins) · [ChatGPT / Codex Plugins](https://learn.chatgpt.com/docs/plugins)

### Command-line CLI

#### Claude Code CLI

```text
/plugin marketplace add Xnurta/MCP
/plugin install xnurta-mcp@xnurta
```

#### Codex CLI

```bash
codex plugin marketplace add Xnurta/MCP
codex plugin add xnurta-mcp@xnurta
```

The plugin installs the MCP server and all 7 Skills together. When installation is complete, set `XNURTA_TOKEN` and start a new session. See [Environment variable setup](docs/installation.md#environment-variable-setup-xnurta_token).

Marketplaces: [Claude Code](.claude-plugin/marketplace.json) · [Codex](.agents/plugins/marketplace.json); plugin descriptors: [Claude Code](.claude-plugin/plugin.json) · [Codex](.codex-plugin/plugin.json)

### Generic MCP clients

See [Installation Guide · Option 3 · Manual setup](docs/installation.md#option-3--manual-setup) for OpenClaw, Hermes, Cursor, Cline, Cherry Studio, and other clients.

---

## Security note

> Your token determines which stores and data you can query — keep it safe and never share it. Store the token in an environment variable in your MCP client configuration, and keep the default local scope (visible to the current project only).

# Xnurta MCP Skills

Official Skills for use with Xnurta MCP, in two tiers:

- **Required (6)**: core query and managed-group management capabilities — always install these.
- **Optional (4)**: advanced analysis playbooks for specific scenarios; install as needed.

## Required Skills

| Skill | Version | MCP Tool | Scope | Purpose |
|-------|---------|----------|-------|---------|
| [query-ads-performance](query-ads-performance/) | 1.0.0 | `get_ads_perf` | `amazon_sa:performance:read` | Query ad performance metrics: spend, ACOS, ROAS, trends, rankings, period comparison |
| [query-entity-metadata](query-entity-metadata/) | 1.1.0 | `get_entity_metadata` | `amazon_sa:ads_configuration:read` | Query entity configuration: campaign / ad group / target / ASIN / managed group names, status, settings |
| [query-operation-log](query-operation-log/) | 1.1.0 | `get_operation_log` | `amazon_sa:ads_logs:read` | Query operation logs: human and AI bid, budget, and status changes |
| [create-ai-group](create-ai-group/) | 1.0.0 | `create_sd_ai_managed_group` / `save_sp_sb_ai_managed_group` | Managed-group write permission | Create SP, SB, or SD AI managed groups |
| [edit-ai-group](edit-ai-group/) | 1.0.0 | `edit_sd_ai_managed_group` / `save_sp_sb_ai_managed_group` | Managed-group write permission | Edit one or multiple AI managed groups |
| [delete-ai-group](delete-ai-group/) | 1.0.0 | `delete_ai_managed_group` | Managed-group delete permission | Delete a managed group and release or migrate its campaigns |

## Optional Skills

Advanced analyses built on the required query Skills — add as needed once those are installed:

| Skill | Version | Purpose |
|-------|---------|---------|
| [weekly-ads-report](weekly-ads-report/) | 1.0.0 | Weekly ads report: KPI card with WoW comparison, 7-day trend, anomaly summary, top movers, next-week actions |
| [monthly-ads-report](monthly-ads-report/) | 1.0.0 | Monthly ads report: full-month KPIs (MoM + YoY), structural breakdown, product and keyword analysis, next-month recommendations |
| [ads-structure-analysis](ads-structure-analysis/) | 1.0.0 | Structure analysis: break down spend and efficiency by campaign type / marketplace / portfolio / weekday and locate structural mismatches |
| [product-diagnosis](product-diagnosis/) | 1.0.0 | Product diagnosis: ASIN health tiering, variant comparison, diagnostic cards for underperformers, keep/optimize/cut recommendations |

## Versions

- **For programs**: [`manifest.json`](manifest.json) is the machine-readable version list (each skill's version, required/optional flag, tool, and scope), available at a stable address:

  ```
  https://raw.githubusercontent.com/Xnurta/MCP/main/skills/manifest.json
  ```

- **For humans**: each Skill's version is in the `version` field of its `SKILL.md` frontmatter; overall history is in [CHANGELOG.md](../CHANGELOG.md).
- On every release, `manifest.json` and the frontmatter are updated together, with a matching git tag / GitHub Release.

## Installation

- **Agents that can act on their own** (Claude Code, ChatGPT Codex, Cursor, etc.): point the AI at a skill directory and say "install this". Install the 6 required Skills first, then add optional ones as needed.
- **Chat / UI-based assistants** (Claude web or desktop app, etc.): add them manually in each client's settings (in Claude, for example: Settings → Skills → upload, one by one).

## Directory layout

```
skills/
├── manifest.json              # machine-readable version list
├── query-ads-performance/     # required
├── query-entity-metadata/     # required
├── query-operation-log/       # required
├── create-ai-group/           # required
├── edit-ai-group/             # required
├── delete-ai-group/           # required
├── weekly-ads-report/         # optional
├── monthly-ads-report/        # optional
├── ads-structure-analysis/    # optional
└── product-diagnosis/         # optional
```

Each Skill is a self-contained directory with a `SKILL.md` (name / version / description and methodology) and `references/` (field references, example queries, etc.).

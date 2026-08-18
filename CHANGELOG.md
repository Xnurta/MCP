# Changelog

This file tracks version changes for Xnurta MCP Skills and the accompanying documentation.

## [Unreleased] - managed-group write skills (beta)

### Added

- **create-ai-group** `1.0.0` (beta) - create an AI managed group; routes SD -> `create_sd_ai_managed_group`, SP/SB -> `save_sp_sb_ai_managed_group` (create mode). Scope `amazon_sa_managed_group:write`.
- **edit-ai-group** `1.0.0` (beta) - edit a managed group (single or bulk): target/ACOS/ROAS/budget, AI on/off, and the AI action space; routes SD -> `edit_sd_ai_managed_group`, SP/SB -> `save_sp_sb_ai_managed_group` (edit mode). Scope `amazon_sa_managed_group:write`. Encodes the front-end-only rules MCP bypasses (ACOS/ROAS/Budget `*Type` mutual exclusion, action-space support matrix, budget ranges, aiPersonality/volume rule) and the "editing a running group can be silently skipped" caveat.
- **delete-ai-group** `1.0.0` (beta) - delete (archive) a group, releasing or migrating its campaigns. Scope `amazon_sa_managed_group_delete:write`.

> These are **beta and not part of a released version** yet. The root README,
> `docs/installation.md`, and the plugin description still describe the **read-only**
> release on purpose; they will be updated (and write scopes declared) when the write
> skills actually ship.

## [1.1.0] - 2026-08-12

Aligns the two required Skills with the 2026-08-11 MCP Tool doc revision.

### Changed

- **query-operation-log** `1.0.0` -> `1.1.0` - `get_operation_log` pagination model updated. Two modes decided by `entities`: a single non-`aiGroup` entity now supports **real pagination** (`page` + `pageSize`, loop until `hasNextPage=false`); multi-entity or `aiGroup`-only stays **limit-only** (`limit`/`truncated`). `pageSize` default `50` -> `100`, max `200` -> `1,000` (`aiGroup`-only `10,000`). Added guidance to steer the user toward a single entity when a limit-only result is truncated.
- **query-entity-metadata** `1.0.0` -> `1.1.0` - documented the new `select` parameter (top-level field projection, ordered, unknown fields ignored via `meta.hint`, no effect on pagination). Noted that enum `{field}Text` companion fields are **not** auto-appended when `select` is used - the `xxxText` field must be listed explicitly.

## [1.0.0] - 2026-07-29

First release (matching MCP Server v1.0.0 - Data Query).

### Added

- 3 **required Skills**:
  - **query-ads-performance** `1.0.0` - ad performance queries, mapped to MCP tool `get_ads_perf` (scope: `amazon_sa:performance:read`)
  - **query-entity-metadata** `1.0.0` - entity configuration / metadata queries, mapped to MCP tool `get_entity_metadata` (scope: `amazon_sa:ads_configuration:read`)
  - **query-operation-log** `1.0.0` - operation log queries, mapped to MCP tool `get_operation_log` (scope: `amazon_sa:ads_logs:read`)
- 4 **optional Skills** (under `skills/optional/`, add as needed after the required ones):
  - **weekly-ads-report** `0.9.4` - weekly ads report
  - **monthly-ads-report** `0.5.6` - monthly ads report
  - **ads-structure-analysis** `0.2.3` - ad structure analysis
  - **product-diagnosis** `0.1.11` - product diagnosis
- `skills/manifest.json` machine-readable version list with a `required` flag
- README and installation guide (Claude / ChatGPT Codex / OpenClaw / Hermes / Cursor / Cline / Cherry Studio client setup)

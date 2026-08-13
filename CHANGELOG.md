# Changelog

This file tracks version changes for Xnurta MCP Skills and the accompanying documentation.

## [Unreleased]

### Changed

- Added OAuth authorization and made it the default authentication method for Plugin installations.
- Retained MCP Token authentication for clients without OAuth support, automation scripts, and fixed-credential use cases.
- Updated the README, installation guide, connection verification, and 401 troubleshooting to cover both authorization methods.

## [1.1.0] - 2026-08-12

Aligns the two required Skills with the 2026-08-11 MCP Tool doc revision.

### Changed

- **query-operation-log** `1.0.0` → `1.1.0` — `get_operation_log` pagination model updated. Two modes decided by `entities`: a single non-`aiGroup` entity now supports **real pagination** (`page` + `pageSize`, loop until `hasNextPage=false`); multi-entity or `aiGroup`-only stays **limit-only** (`limit`/`truncated`). `pageSize` default `50` → `100`, max `200` → `1,000` (`aiGroup`-only `10,000`). Added guidance to steer the user toward a single entity when a limit-only result is truncated.
- **query-entity-metadata** `1.0.0` → `1.1.0` — documented the new `select` parameter (top-level field projection, ordered, unknown fields ignored via `meta.hint`, no effect on pagination). Noted that enum `{field}Text` companion fields are **not** auto-appended when `select` is used — the `xxxText` field must be listed explicitly.

## [1.0.0] - 2026-07-29

First release (matching MCP Server v1.0.0 · Data Query).

### Added

- 3 **required Skills**:
  - **query-ads-performance** `1.0.0` — ad performance queries, mapped to MCP tool `get_ads_perf` (scope: `amazon_sa:performance:read`)
  - **query-entity-metadata** `1.0.0` — entity configuration / metadata queries, mapped to MCP tool `get_entity_metadata` (scope: `amazon_sa:ads_configuration:read`)
  - **query-operation-log** `1.0.0` — operation log queries, mapped to MCP tool `get_operation_log` (scope: `amazon_sa:ads_logs:read`)
- 4 **optional Skills** (under `skills/optional/`, add as needed after the required ones):
  - **weekly-ads-report** `0.9.4` — weekly ads report
  - **monthly-ads-report** `0.5.6` — monthly ads report
  - **ads-structure-analysis** `0.2.3` — ad structure analysis
  - **product-diagnosis** `0.1.11` — product diagnosis
- `skills/manifest.json` machine-readable version list with a `required` flag
- README and installation guide (Claude / ChatGPT Codex / OpenClaw / Hermes / Cursor / Cline / Cherry Studio client setup)

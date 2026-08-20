# Changelog

This file tracks version changes for Xnurta MCP Skills and the accompanying documentation.

## [Unreleased]

### Changed

- **xnurta-query-entity-metadata** `1.1.0` → `1.1.1`.
- **xnurta-query-operation-log** `1.1.0` → `1.1.1`.
- **xnurta-create-ai-group** `1.0.0` → `1.0.1`: added business-term disambiguation, a mandatory clarification gate before writes, and corrected create-time budget and target mappings.
- **xnurta-edit-ai-group** `1.0.0` → `1.0.1`: added business-term disambiguation, clarification separate from write authorization, and path-specific status handling.
- **xnurta-query-entity-metadata** `1.1.1` → `1.1.2`: documented managed-group total-budget rollups and their proportional campaign-budget behavior.
- **xnurta-create-ai-group** `1.0.1` → `1.0.2`: clarified group budget, performance-budget increase caps, budget reallocation scope, and create-time confirmation requirements.
- **xnurta-edit-ai-group** `1.0.1` → `1.0.2`: added the complete fixed/percentage and per-campaign/group budget model, reliable enabled-campaign lookup, impact preview, and ambiguity handling.
- **xnurta-delete-ai-group** `1.0.0` → `1.0.1`: stopped relying on unsupported campaign `aiGroupId` server filtering; campaign membership is now verified after full pagination and local filtering.
- Added Skill version-check guidance: during installation or updates, AI agents compare local `SKILL.md` versions with the remote `manifest.json` and tell the user when installed Skills are outdated.
- Added an OAuth preflight requirement for AI agents: follow MCP authorization discovery and validate all protocol-required request fields before sending an OAuth request.

## [1.1.0] - 2026-08-18

Adds managed-group write support and aligns the query Skills with the latest MCP Tool documentation.

### Changed

- Added OAuth authorization alongside the existing MCP Token authentication method.
- Kept MCP Token in the Plugin's bundled configuration; OAuth-capable clients can connect through a Custom Connector or manual setup.
- Updated the README, installation guide, connection verification, and 401 troubleshooting to cover both authorization methods.
- **xnurta-query-operation-log** `1.0.0` → `1.1.0` — `get_operation_log` pagination model updated. Two modes decided by `entities`: a single non-`aiGroup` entity now supports **real pagination** (`page` + `pageSize`, loop until `hasNextPage=false`); multi-entity or `aiGroup`-only stays **limit-only** (`limit`/`truncated`). `pageSize` default `50` → `100`, max `200` → `1,000` (`aiGroup`-only `10,000`). Added guidance to steer the user toward a single entity when a limit-only result is truncated.
- **xnurta-query-entity-metadata** `1.0.0` → `1.1.0` — documented the new `select` parameter (top-level field projection, ordered, unknown fields ignored via `meta.hint`, no effect on pagination). Noted that enum `{field}Text` companion fields are **not** auto-appended when `select` is used — the `xxxText` field must be listed explicitly.
- Added three **required Skills at version 1.0.0**: `xnurta-create-ai-group`, `xnurta-edit-ai-group`, and `xnurta-delete-ai-group`, covering SP, SB, and SD AI managed-group creation, editing, and deletion.
- Documented that managed-group writes modify live configuration immediately and require confirmation plus read-back verification.
- Documented current limitations: no scheduling, template-based setup, or word-list settings; RBA configuration cannot be read or edited, RBA → AI is supported, and AI → RBA is not supported.

## [1.0.0] - 2026-07-29

First release (matching MCP Server v1.0.0 · Data Query).

### Added

- 3 **required Skills**:
  - **xnurta-query-ads-performance** `1.0.0` — ad performance queries, mapped to MCP tool `get_ads_perf` (scope: `amazon_sa:performance:read`)
  - **xnurta-query-entity-metadata** `1.0.0` — entity configuration / metadata queries, mapped to MCP tool `get_entity_metadata` (scope: `amazon_sa:ads_configuration:read`)
  - **xnurta-query-operation-log** `1.0.0` — operation log queries, mapped to MCP tool `get_operation_log` (scope: `amazon_sa:ads_logs:read`)
- 4 **optional Skills** (under `skills/optional/`, add as needed after the required ones):
  - **xnurta-weekly-ads-report** `0.9.4` — weekly ads report
  - **xnurta-monthly-ads-report** `0.5.6` — monthly ads report
  - **xnurta-ads-structure-analysis** `0.2.3` — ad structure analysis
  - **xnurta-product-diagnosis** `0.1.11` — product diagnosis
- `skills/manifest.json` machine-readable version list with a `required` flag
- README and installation guide (Claude / ChatGPT Codex / OpenClaw / Hermes / Cursor / Cline / Cherry Studio client setup)

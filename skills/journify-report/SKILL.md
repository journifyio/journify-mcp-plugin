---
name: journify-report
description: Create a high-level operational report for a Journify workspace. Use when a workspace report is requested.
---

# Workspace report

Create an operational state report for the Journify workspace.

Use the [Journify documentation sitemap](https://docs.journify.io/sitemap.md) as needed.

## Scope

Clarify the request scope before continuing:

- Target Journify workspace. If multiple workspaces exist, ask the user to choose one.
- Make sure the Journify MCP server is available. If not, share the documentation.

## Workflow

1. Call `list_sources`, `list_destinations`, and `list_syncs`. Report their total counts at the top of the report. For total volume, sum the received-event volume returned by `get_event_volume` for each source. Do not add sync volume to this total because it can count the same event more than once.
2. Build a sources table with each source's name, app, enabled/configured state, creation date, and received-event volume from `get_event_volume`.
3. Call `list_syncs` with status `ACTIVE`. For every active sync, call `get_sync` and `get_event_volume`. Build an active-syncs table with source, destination, volume, number of connected event mappings, creation date, and active issue count.
4. Call `list_issues` with status `ACTIVE`. Include the remaining alerts to fix, ordered by severity and last detected time.

State the reporting period returned by each volume or coverage tool. Clearly distinguish unavailable metrics from zero values.

## Output format

Start the report with these totals, before any other section:

### Workspace totals

| Total sources | Total destinations | Total syncs | Received-event volume |
|---:|---:|---:|---:|
| 12 | 8 | 24 | 123,456 |

State the reporting period for received-event volume directly below the table. If source-volume reporting periods differ, write `Unavailable` for the total and state why. Keep unavailable values distinct from confirmed zeroes.

Use these Markdown tables, even when a section has no results.

### Sources

| Source | App | State | Created | Received-event volume | Reporting period |
|---|---|---|---|---:|---|
| Example source | Example app | Enabled, configured | 2026-08-01 | 12,340 | Last 7 days |

### Active syncs

| Source | Destination | Volume | Event mappings | Created | Active issues | Reporting period |
|---|---|---:|---:|---|---:|---|
| Example source | Example destination | 10,240 | 4 | 2026-08-01 | 2 | Last 7 days |

### Remaining alerts

| Severity | Alert | Source or sync | Observed value | Last detected |
|---|---|---|---|---|
| High | Example alert | Example sync | Example value | 2026-08-30 09:15 UTC |

For unavailable data, write `Unavailable`. For confirmed zero values, write `0`.

If no rows apply, keep the table and add one row:

| — | — | — | — | — |
|---|---|---|---|---|
| No results |  |  |  |  |

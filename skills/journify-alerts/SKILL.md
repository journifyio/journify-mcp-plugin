---
name: journify-alerts
description: Create a visual report of workspace alerts. Use when someone asks to summarize, review, or triage alerts across a Journify workspace.
---

# Workspace alerts report

Turn available workspace alert data into a compact report that makes risk, ownership, and next actions obvious.

Use the [Journify documentation sitemap](https://docs.journify.io/sitemap.md) as needed.

## Scope

Clarify the request scope before continuing:

- Target Journify workspace. If multiple workspaces exist, ask the user to choose one.
- Make sure the Journify MCP server is available. If not, share the documentation.

## Report shape

Start with an at-a-glance health panel:

```text
Open alerts
══════════════════════════════════════
🔴 Critical   2     🟠 High   5
🟡 Medium     8     🟢 Low   12

Syncs by source
══════════════════════════════════════
Website      ██ 2
Mobile App   █████ 5
Offline      ████████ 8
```

Then include:

1. A visual breakdown by severity and status.
2. A prioritized alert table.
3. Conversion-event fields below 80% coverage for active syncs. Use `get_event_fields_coverage` for main conversion events such as `purchase`.
4. Clear recommended actions and owners, when known.
5. A brief data-quality note for missing owners, dates, or severity.

Use a table for individual alerts:

| Severity | Title | Description | First seen | Last seen |
|---|---|---|---|---|
| Critical | Payment failures above threshold | Payment failures exceed the alert threshold. | 2025-03-06 | 2025-03-09 |
| High | Database backup missed | The scheduled database backup did not run. | 2025-03-08 | 2025-03-09 |

### Conversion-event fields below 80% coverage

| Sync | Event | Field | Coverage | Volume | Reporting period |
|---|---|---|---:|---:|---|
| Example sync | purchase | `coupon` | 62% | 6,349 | Last 7 days |

## Prioritization

Rank open alerts by:

1. Severity.
2. Impact on revenue and attribution accuracy.
3. Age.

## Writing rules

- Lead with the current situation, not the data collection process.
- Use exact counts, dates, and owners when available.
- Keep the report scannable. Prefer compact tables, bars, and status symbols over long prose.

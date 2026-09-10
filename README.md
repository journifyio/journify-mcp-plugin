# Journify plugin

Implement Journify source tracking, generate reports, and review alerts in Claude Code,
Cursor, ChatGPT, or Codex. The plugin bundles three skills and connects to the
hosted Journify MCP server. No build step is required.

## Skills

| Skill | Purpose |
| --- | --- |
| `journify-source-setup` | Create a source when needed, get its writekey and schema, implement events available in the application, and report completed and missing work. |
| `journify-report` | Summarize sources, active syncs, event volume, and alerts. |
| `journify-alerts` | Prioritize alerts and find conversion-event fields with low coverage. |

The source setup skill needs permission to read and edit your application codebase.

## MCP connection

The endpoint is configured in [`.mcp.json`](.mcp.json):

```text
https://mcp.journify.io/mcp
```

Set up authentication in your client's MCP settings. The plugin contains no
credentials. Live connectivity and authentication have not yet been verified.


## Example requests

- Implement my Journify source schema in this application and report what is still missing.
- Set up Journify tracking from this codebase. I have not created a source yet.
- Create an operational report for my Journify workspace.
- Review my workspace alerts and prioritize what to fix.

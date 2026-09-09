# Journify plugin

Set up Journify workspaces, generate reports, and review alerts in Claude Code,
Cursor, ChatGPT, or Codex. The plugin bundles three skills and connects to the
hosted Journify MCP server. No build step is required.

## Skills

| Skill | Purpose |
| --- | --- |
| `journify-setup` | Configure sources, destinations, and syncs. Add the source schema to your application. |
| `journify-report` | Summarize sources, active syncs, event volume, and alerts. |
| `journify-alerts` | Prioritize alerts and find conversion-event fields with low coverage. |

The setup skill needs permission to read and edit your application codebase.

## MCP connection

The endpoint is configured in [`.mcp.json`](.mcp.json):

```text
https://mcp.journify.io/mcp
```

Set up authentication in your client's MCP settings. The plugin contains no
credentials. Live connectivity and authentication have not yet been verified.


## Example requests

- Set up my Journify workspace and add tracking to this application.
- Create an operational report for my Journify workspace.
- Review my workspace alerts and prioritize what to fix.

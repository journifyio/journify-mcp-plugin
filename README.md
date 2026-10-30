# Journify plugin

Generate reports and review alerts in Claude Code, Cursor, ChatGPT, or Codex. The
plugin bundles two skills and connects to the hosted Journify MCP server. No build
step is required.

## Get started

Paste this prompt into your AI agent. It connects the Journify MCP server and
suggests what to do next:

> Set up Journify for me. Read https://docs.journify.io/mcp-server/agent-setup.md and follow it. Connect the Journify MCP server, then suggest next steps.

## Skills

| Skill | Purpose |
| --- | --- |
| `journify-report` | Summarize sources, active syncs, event volume, and alerts. |
| `journify-alerts` | Prioritize alerts and find conversion-event fields with low coverage. |

## MCP connection

The endpoint is configured in [`.mcp.json`](.mcp.json):

```text
https://mcp.journify.io/mcp
```

Set up authentication in your client's MCP settings. The plugin contains no
credentials. Live connectivity and authentication have not yet been verified.

## Example requests

- Create an operational report for my Journify workspace.
- Review my workspace alerts and prioritize what to fix.

## Privacy Policy
https://www.journify.io/privacy-policy

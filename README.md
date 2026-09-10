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

## Local testing

### Claude Code

From this repository:

```sh
claude plugin validate .
claude --plugin-dir .
```

### Cursor

Link the repository into Cursor's local plugin directory:

```sh
mkdir -p ~/.cursor/plugins/local
ln -s "$PWD" ~/.cursor/plugins/local/journify-mcp-plugin
```

Reload Cursor and open Customize to finish the MCP connection setup. Your
organization must allow local plugin imports.

### ChatGPT and Codex

Use [`.codex-plugin/plugin.json`](.codex-plugin/plugin.json) and follow
[OpenAI's local packaging guide](https://developers.openai.com/plugins/build/plugins).
For ChatGPT testing, register the MCP endpoint in developer mode and add the
connection ID to a local `.app.json` through the plugin-creator flow.

## Example requests

- Implement my Journify source schema in this application and report what is still missing.
- Set up Journify tracking from this codebase. I have not created a source yet.
- Create an operational report for my Journify workspace.
- Review my workspace alerts and prioritize what to fix.

## Publishing

- [OpenAI submission portal](https://platform.openai.com/plugins). Choose "With
  MCP" and include the endpoint, all three skills, and their referenced files.
- [Claude submission guide](https://claude.com/docs/plugins/submit). Submit the
  public repository or a plugin ZIP.
- [Cursor Marketplace](https://cursor.com/marketplace/publish). Submit the public
  repository.

Before submitting, test authentication and each skill, confirm the license and
public policy URLs, and complete the platform's submission requirements.

## Maintenance

The three manifests share `skills/` and `.mcp.json`. Maintain the skills in
[journifyio/skills](https://github.com/journifyio/skills), then copy each complete
directory into `skills/`, including its references. The plugin uses these bundled
copies and does not fetch updates at runtime.

The bundled files were originally copied from upstream commit
[`60c83d9`](https://github.com/journifyio/skills/commit/60c83d9517b73f682b531e3d9ff7a1b976a7072c).
The source setup skill has since been renamed and scoped to source instrumentation locally.
For each release, validate the package, update this commit reference, and bump
`version` in all three manifests together.

---
name: journify-source-setup
description: Set up Journify source tracking from an application codebase, creating a source when needed. Retrieve its writekey and schema, implement events available in code, and report completed and missing work.
---

# Journify source setup

Connect the application to a Journify source, creating one when needed, and implement its schema wherever the corresponding business action exists in code.

## Rules

- Do not create or configure workspaces, destinations, or syncs.
- Use the retrieved schema to guide instrumentation only.
- Preserve an existing source's enabled state unless requested.
- Never update a source schema unless it lists no events.
- If no workspace exists, ask the customer to create one.
- Follow the steps, do not skip or reorder them. Report any blockers.
- Use [Journify documentation sitemap](https://docs.journify.io/sitemap.md) for SDK reference and official guidance. Do not invent SDK calls or configuration.

## 1. Resolve the source and application

- Read repository instructions and preserve unrelated changes. 
- Locate the application, runtime, package manager, existing Journify integration, environment configuration, consent handling, and validation commands.
- Use the Journify MCP server to inspect workspaces and sources with `list_workspaces`, `list_sources`, and `get_source` as available.
- Reuse the source identified by the request or application configuration when its identity and purpose match.
- Ask for the target workspace or source only if it remains ambiguous.
- If MCP access or the application repository is unavailable, explain what is needed to continue.

### When no matching source exists

- Create new source. Inspect `list_catalog_sources` and `list_source_schema_templates` to select the source app for the application's runtime and a schema template that fits the product. 
- Use catalog values and required settings returned by the tools. Report missing settings or access with the exact resume step.
- Use the schema template that best matches the product's industry and business actions.

## 2. Initialize Journify SDK

- Get the source's writekey.
- Initialize the Journify SDK in the application codebase according to the official documentation for the runtime. If the SDK is already initialized, verify that it uses the correct writekey and configuration.
- Follow the codebase best practices for configuration, secrets, and environment variables.

## 3. Implement the source schema


- Read the complete source schema.
- If the schema is empty, follow [references/custom-source-schema.md](references/custom-source-schema.md) to create one. Then save it on the MCP.
- Implement each event in the codebase where the corresponding business action exists.
- Do not invent events or properties.

## 4. Validate implementation

- Review the changes and fix failures introduced by the implementation. 
- Verify event names, required values, duplicate prevention, and consent where relevant.
- Record failed or unavailable checks and any delivery verification actually performed.

Use these statuses as work progresses:

| Status | Meaning                                                                    |
|--------|----------------------------------------------------------------------------|
| ✅      | Added or corrected tracking during this run.                               |
| ⛔      | The event is missing in the codebase.                                      |

## 5. Return the report

- Return the report directly in the final response.
- Suggest enabling boosters if the source supports them.
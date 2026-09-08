---
name: journify-setup
description: Set up a Journify workspace and implement the source schema in the application codebase in one coordinated run.
---

# Journify setup

Configure the Journify workspace and the current application codebase as one job. A setup request includes both unless the user explicitly asks for remote-only or code-only work.

Do not stop after creating remote resources or return the source schema as a handoff. Implement the schema in the codebase, validate it and finish the remote configuration.

## Scope

Resolve these details from the request, repository, and Journify workspace before asking the user:

- Make sure the Journify MCP server is available. If not, share the documentation.
- Target Journify workspace. If multiple workspaces exist, ask the user to choose one. If none exist, ask the user to create one.
- Target source app from the application codebase. If the source already exists, use it.
- Target Destination apps and syncs to set up with the source.

Ask only for choices or secrets that cannot be discovered.

Remote mutations and code edits are authorized only when the user asks for setup. 

## Working rules

- Treat the remote source schema and code instrumentation as one contract. 
- Event names, property names, types, and user traits must match.
- Keep a ledger of every remote resource created or changed during the run.
- Reuse an existing resource only when its identity and purpose match.
- Follow the repository's `AGENTS.md` and preserve unrelated changes.
- Do not edit Controlplane catalog schemas unless the user explicitly asks for a catalog change.
- Use the [Journify documentation sitemap](https://docs.journify.io/sitemap.md) as needed for SDK reference and destination documentation.

## Workflow

### 1. Inspect both sides

Inspect the repository and the remote workspace before making changes.

In the repository, locate the client application, runtime, package manager, Journify SDK, SDK initialization, environment configuration, existing tracking calls, consent handling, and focused validation commands. Read [references/instrumentation.md](references/instrumentation.md) before editing code. If the current repository is not the client application, identify the required repository and stop.

Inspect Journify with:

- `list_workspaces`
- `list_catalog_sources`
- `list_source_schema_templates`
- `list_catalog_destinations`
- `list_sources`
- `list_destinations`
- `list_syncs`

Confirm catalog slugs, required settings, supported destination resources and sync modes, and any matching existing resources.

### 2. Define the coordinated setup

Choose the narrowest source schema template that covers the product. Use `custom` only when no industry template fits. When the customer chooses `custom`, read [references/custom-source-schema.md](references/custom-source-schema.md) before building the event taxonomy and user traits.

Map each schema event and user trait to a real application action or domain value. Identify requested destinations, resources, sync modes, consent categories, and any custom mappings.

Before remote mutation or code edits, summarize the exact remote resources and local packages that will change. Ask for approval only if the user has not already authorized those changes.

### 3. Create or configure the source

Create the source with `create_source`, or reuse the confirmed matching source. Save its ID and complete schema. For a custom source, write the approved complete schema with `update_source_schema` if `create_source` does not apply it.

Complete every returned `next_steps` item with `update_source_setting`, then fetch the source with `get_source`. Keep it disabled.

### 4. Implement the schema in the codebase

Implement the returned source schema in the current client codebase during the same run. Add or update SDK initialization, identity calls, event calls, consent handling, and environment configuration as required by the repository and installed SDK.

Instrument real success points. Do not emit completed business events from page loads or button clicks when the application has a later authoritative success result. Do not invent events, properties, placeholder values, or IDs.

### 5. Reconcile remote schema and code

If code discovery proves that the schema must change, update the remote source with `update_source_schema`. Always send the complete schema with this top-level shape:

```json
{
  "events": {
    "<event_name>": {}
  },
  "user": {}
}
```

Never send a partial schema. Recheck the final remote schema against every implemented event and trait.

### 6. Validate the implementation

Run focused tests, lint checks, type checks, and builds for the changed client packages. Use the repository's own commands. Fix failures caused by the implementation.

Do not enable the source if relevant checks fail, required instrumentation is missing, or the schema and code differ.

### 7. Configure destinations and syncs

For each requested destination:

- Create it with `create_destination`, or reuse a confirmed match.
- Complete every returned `next_steps` item with `update_destination_setting`.
- Run `test_connection` when supported.
- Stop work that depends on the destination if its connection test fails, unless the user accepts that failure.

Create one sync for each requested source-to-destination resource pair. Use a mode supported by `list_catalog_destinations` and supply every required resource setting. Prefer the least permissive mode that meets the use case.

Inspect generated field and event mappings. Replace only incorrect mappings. For an existing paused sync, enable it with `pause_sync` and `pause: false` after the setup is ready.

### 8. Activate and verify

Enable the source with `enable_source` only after the code checks pass and destination connections needed by the setup are ready. Create new syncs as `ACTIVE`, or activate existing syncs, only at this point.

Verify with fresh calls to `get_source`, `get_destination`, `get_sync`, `list_sources`, `list_destinations`, and `list_syncs` as applicable.

The full setup is complete only when:

- The final source schema matches the implemented code.
- The relevant code checks pass.
- The source is configured and enabled.
- Every requested destination is configured, and available connection tests pass.
- Every requested sync is active and its generated mappings have been reviewed.

## Failures and retries

Before retrying a remote mutation, list and inspect existing resources. Do not delete or recreate a resource without approval.

If a remote mutation succeeds but later work fails, leave valid resources in place. Keep the source or affected sync disabled when it is not ready. Report the completed state, failed step, and exact resume point. Never describe an unavailable, unchecked, or partially configured resource as ready.

## Final response

Report the remote setup and code implementation together.

When the setup is complete, suggest that the user enable any available boosters for the source app. This is a recommendation, not a setup requirement: do not enable boosters unless the user explicitly asks.

Include compact Markdown tables named Sources, Destinations, and Syncs. Show the resource title, app slug, resource ID, configuration state, enabled or active state, destination resource and sync mode where relevant, and a plain status.

Add a Code changes section with clickable file links and the checks run. Put missing credentials, failed checks, disabled resources, and remaining work directly below the affected table or code section.

# Implementing the source schema

Use this guide to implement Journify SDK calls in the client codebase.
The goal is to send events and user traits that match the source schema exactly, without inventing new events or properties.

## Official Journify SDK documentation

Use the [Journify documentation sitemap](https://docs.journify.io/sitemap.md) as the entry point for SDK documentation.

Before editing instrumentation:

1. Open the sitemap and select the source SDK guide that matches the application's runtime. Fall back to the HTTP API if no SDK is available for the runtime.
2. Follow the API exposed by the installed SDK version. If local package types or documentation differ from the current Journify docs, establish the installed version and do not upgrade or invent compatibility code without user approval.

## Discover the integration

1. Read the repository instructions and identify the runtime, package manager, test commands, and application entry points.
2. Pull the source writekey from the MCP server.
3. Search for an installed Journify SDK, client initialization, existing `identify`, `track`, `page` or `screen` calls, and consent handling.
4. Use the API exposed by the installed SDK version.
5. If the SDK is absent or its API cannot be established from local code and official Journify documentation, ask before adding a dependency.
6. Keep credentials in the project's existing environment or secret mechanism. Add an example variable only when the repository already maintains an example environment file.

## Map schema to code

- Treat the source schema as the contract. Preserve event names, property names, types, and required fields exactly.
- Map each event to the business action that proves it occurred.
- When possible, avoid emitting a completed business event from a page load or button click.
- Populate required properties from authoritative domain values.
- Do not use placeholders, empty strings, guessed constants, or fabricated IDs to satisfy the schema.
- Send user traits through the SDK's identity call when the identity becomes known or changes.
- Skip schema events that don't exist in the codebase.
- Do not invent any new event or property.

## Verification

Verify that:

- Each required event fires once on success and not on failure.
- Event and user payloads use the schema's exact names and types.
- Required fields are present.
- Optional fields are omitted when unavailable.
- Consent requirements are respected.

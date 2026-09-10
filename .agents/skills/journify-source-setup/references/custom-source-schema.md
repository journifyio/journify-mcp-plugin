# Build a custom source schema

Use this guide when implementing any source schema and when defining a custom schema. For an existing or template-based schema, preserve its event names, properties, types, and required fields. Apply the schema shape and trigger rules below without redesigning the schema. Record events or required values missing from the code in the final response. Do not write report files or copy the source schema into the application codebase.

## Define a custom schema

Follow these authoring steps when the customer chooses a `custom` schema or no industry template fits a new source. Never update a source schema unless you have retrieved its saved schema and verified that it lists no events.

Before drafting, establish the product type, main user journey, and intended business outcome from the request and repository. Ask the customer for any missing or ambiguous context, especially business outcomes that code alone cannot establish.

1. Start with the conversions, reports, and audiences the customer needs.
2. Inspect the code for the success point that proves each event happened.
3. Keep only events with a real trigger and a known use.
4. Put persistent user facts in `user`.
5. Mark a field `required` only when every call must supply it. Use `recommended` for useful fields.
6. Resolve ambiguous event meanings with the customer before saving the schema. Keep this work scoped to the source taxonomy and user traits.

Use the workspace's naming convention. Otherwise, use stable snake_case names that describe a completed action, such as `trial_started` or `purchase`. Use a destination's standard event name when the meaning matches.

Do not track a button click as a completed outcome when a later success response exists. Do not encode property values in event names, invent fields, or collect personal data without a stated use and consent.

## Schema shape

The payload has `user` and `events` at the top level. The user and every event are JSON Schema Draft 07 object schemas. Send the complete payload with every `update_source_schema` call.

Follow this example:

```json
{
  "user": {
    "$schema": "http://json-schema.org/draft-07/schema#",
    "type": "object",
    "properties": {
      "userId": { "type": "string" },
      "email": { "type": "string" },
      "account_type": { "type": "string" }
    },
    "required": ["userId"],
    "recommended": ["userId", "email"]
  },
  "events": {
    "purchase": {
      "$schema": "http://json-schema.org/draft-07/schema#",
      "type": "object",
      "properties": {
        "transaction_id": { "type": "string" },
        "value": { "type": "number" },
        "currency": { "type": "string" },
        "items": {
          "type": "array",
          "items": {
            "type": "object",
            "properties": {
              "item_id": { "type": "string" },
              "item_name": { "type": "string" },
              "price": { "type": "number" },
              "quantity": { "type": "number" }
            },
            "required": ["item_id", "item_name"]
          }
        }
      },
      "required": ["transaction_id", "value", "currency", "items"],
      "recommended": ["transaction_id", "value", "currency", "items"]
    }
  }
}
```

Before mutation, confirm that every event has one authoritative trigger, every required field exists on all valid paths, and the code can send the exact names and types in the schema.

# Build a custom source schema

Use this guide when the customer chooses `custom` schema.

Before drafting, ask the customer what kind of business or product this is, such as a ride-hailing app, game, ecommerce store, SaaS product, or marketplace. Also ask for the main user journey and business outcome. Do not infer these answers from the code alone.

1. Start with the conversions, reports, and audiences the customer needs.
2. Inspect the code for the success point that proves each event happened.
3. Keep only events with a real trigger and a known use.
4. Put persistent user facts in `user`.
5. Mark a field `required` only when every call must supply it. Use `recommended` for useful fields.
6. Review the taxonomy and non-obvious destination mappings with the customer before writing it.

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

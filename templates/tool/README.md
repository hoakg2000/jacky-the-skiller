# <group>_<action>

<!--
Best practices (delete this comment block when done):
- One folder per tool: `tools/<group>-<action>/` with README.md, source and test.
- name: snake_case `<group>_<action>`, verb-first action (search, get, create, update, delete).
- description is a prompt for the model: what it does, when to use it, what it returns, limits.
- Input schema: few params, typed, with defaults and bounds (e.g. max 50 results). Describe every field.
- Output: a trimmed object with only fields the model needs. Never a raw API payload or bare array.
- Errors: return `{ error: { code, message } }` with stable codes (invalid_input, auth, not_found,
  rate_limited, upstream). Never put tokens or headers in messages.
- `write: true` for anything that sends, creates, updates or deletes, so apps can ask for approval.
- URL-encode all user input. Use the minimal OAuth scope. Pass timeouts / abort signals through.
- Tests mock the network and use fake fixture data only.
-->

<One sentence: what the tool does.>

| Field | Value |
| --- | --- |
| Provider | <API or service> |
| Write | `false` / `true` |
| Auth | <none / API key / OAuth scope `<minimal-scope>`> |
| API docs | <official endpoint URL> |

## Description (sent to the model)

> <What it does. When to use it. What it returns. Limits, e.g. "returns at most 20 results".>

## Input

| Field | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| `query` | string | yes | | <meaning, format> |
| `limit` | integer 1–50 | no | `10` | <meaning> |

## Output

```json
{
  "items": [{ "id": "<id>", "title": "<title>" }],
  "nextPageToken": "<token or null>"
}
```

## Errors

| Code | When |
| --- | --- |
| `invalid_input` | Input fails the schema. |
| `auth` | HTTP 401 / 403. |
| `not_found` | HTTP 404. |
| `rate_limited` | HTTP 429. |
| `upstream` | Any other API failure. |

## Tests
- Request URL, method and params (and body for write tools).
- Trimmed output shape from a fake fixture.
- Each error code above.

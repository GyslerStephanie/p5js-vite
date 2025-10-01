# Content Component Templates

Use these templates to maintain consistency across endpoint references, tutorials, and conceptual documentation.

## Endpoint Reference Page Template

```
# {HTTP METHOD} {PATH}

**Use this when:** {short scenario statement}

## Summary
Concise description of business value and response timing.

## Authentication
- Supported methods: {OAuth2, API key, mTLS}
- Required scopes: {scope list}

## Request
### Path Parameters
| Name | Type | Required | Description |
| --- | --- | --- | --- |

### Query Parameters *(if applicable)*
| Name | Type | Required | Description |
| --- | --- | --- | --- |

### Headers
| Name | Required | Description |
| --- | --- | --- |

### Body Schema
Embed auto-generated schema block from OpenAPI; include privacy callouts.

### Example Requests
Tabs: cURL · Node.js · Python · Java · Go

```
curl --request POST https://{env}.api.example.com/v1/score \
  --header "Authorization: Bearer $TOKEN" \
  --header "Content-Type: application/json" \
  --data '{ ... }'
```

## Response
### Success (200)
- Decision flow copy (e.g., "Expect latency under 3 ms p95 in production").
- JSON example with field-level annotations.

### Errors
Reference error catalog with inline excerpts for the most common codes.

### Next Steps
Link to relevant guides, SDK helpers, and changelog entries.
```

## Quickstart Template (3-5 minute)

```
# Quickstart: {Context}

## Goal
State the outcome in plain language.

## Prerequisites
- Sandbox account with {role}
- API key or OAuth client
- Tooling (CLI, language runtime)

## Step 1 — Get Credentials
Provide code snippet or UI steps.

## Step 2 — Make Your First Call
Show terminal output + screenshot of Playground response (optional).

## Step 3 — Interpret the Decision
Explain risk levels and suggested product actions.

## Step 4 — Move to Staging
Highlight environment switch, secrets storage, and logging guidance.

## Troubleshooting
Bulleted list of common issues and fixes.
```

## Concept / Guide Template

```
# {Concept Title}

## Why it matters
Business outcome framing.

## How it works
Sequence diagram or numbered flow.

## Implementation Checklist
- [ ] Step
- [ ] Step

## Best Practices
Bullet list with citations to compliance/privacy guidance.

## Related Resources
Links to API reference, tutorials, and external regulations.
```

## Changelog Entry Template

```
## {Date} — {Change Title}

### Added
- Bullet list

### Changed
- Bullet list

### Deprecated / Removed
- Bullet list with deprecation timelines (>= 180 days notice).

### Impact & Action
Plain-language guidance for partners, with links to migration steps.
```

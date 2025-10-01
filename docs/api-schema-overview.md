# Sentinel Fraud Decisioning API — Schema & Documentation Blueprint

This document distills requirements for the external-facing API program, combining schema design, security posture, developer experience standards, and operational commitments.

## Purpose & Target Personas
- **Purpose:** Enable partners (banks, fintechs, wallets, public sector portals) to submit minimal transaction metadata and receive fraud decisions within milliseconds while safeguarding privacy.
- **Primary users:**
  - External: Backend developers, solution architects, fraud platform engineers
  - Internal: API product owner, technical writers, support/solutions engineers, compliance teams

## Core Endpoints (v1)
| Method & Path | Description | SLA | Notes |
| --- | --- | --- | --- |
| `POST /v1/score` | Real-time scoring of a single transaction | `<3 ms p95` production, `10–30 ms` sandbox | Requires OAuth scope `score` or API key with scoring permission |
| `POST /v1/batch/score` | Async scoring for arrays or NDJSON files | First decision manifest within 2 minutes for ≤10k records | Returns `202 Accepted` receipt; supports callbacks |
| `GET /v1/events/{id}` | Retrieve historical decision or batch manifest | 90-day retention (prod), 7-day (sandbox) | Accepts `txn_id`, `trace_id`, or `batch_id` |
| `GET /v1/health` | Current service status & latency metrics | No auth required | Useful for monitoring integration |
| `GET /v1/schema` | Machine-readable OpenAPI / JSON Schema | No auth required | Versioned per release; supports automation |

Full machine-readable spec: [`openapi.yaml`](./openapi.yaml).

## Request & Response Contracts
### Request (Score)
Minimal, privacy-first payloads with hashed identifiers.

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `txn_id` | string | ✔ | Client transaction identifier |
| `timestamp` | ISO 8601 | ✔ | UTC timestamp |
| `amount.value` | number | ✔ | Major currency units |
| `amount.currency` | string | ✔ | ISO 4217 |
| `context` | enum | ✔ | `transfer`, `card`, `invoice`, `defi_sign`, `wallet_send`, `other` |
| `counterparty_id` | string | ✔ | Hashed/tokenized ID |
| `payer_id` | string | ✔ | Hashed/tokenized ID |
| `device.device_id` | string | ✔ | Persistent hashed device token |
| `device.ip_partial` | string | ▢ | Subnet or masked IP |
| `device.geo_coarse` | string | ▢ | Country or region |
| `signals` | object | ▢ | Behavioral metrics (flex schema) |
| `channel` | enum | ✔ | `web`, `ios`, `android`, `api` |

### Response (Score)
Structured outputs to power deterministic flows.

| Field | Type | Description |
| --- | --- | --- |
| `risk_score` | 0–100 integer | Higher = riskier |
| `risk_level` | enum | `low`, `medium`, `high` mapped in docs |
| `decision` | enum | `allow`, `challenge`, `block` |
| `explanations[]` | string | Human-readable reasons (max 5) |
| `confidence` | 0–1 float | Model confidence |
| `policy_triggered[]` | string | Rule/policy identifiers |
| `trace_id` | string | Correlate with support |
| `latency_ms` | integer | Observed processing time |

## Authentication & Security Controls
- **OAuth 2.0 client credentials** with `score` and `events` scopes.
- **API keys** scoped by environment; keys are generated per app with role-based management.
- **Mutual TLS** optional for enterprise tenants; certificates rotated annually.
- **Rate limits**: 100 RPS burst / 10k RPM sustained per key by default. Document custom tiers and contact path for increases.
- **PII minimization**: Require hashing/tokenization for payer and counterparty IDs; highlight do-not-send fields.
- **Logging & traceability**: Provide `trace_id` on every response and require clients to log for audits.

## Developer Experience Standards
- **OpenAPI 3.1** spec with examples per context (transfer, card, etc.). Auto-generate SDKs from spec.
- **Quickstarts** for cURL and languages (Node, Python, Java, Go) plus Postman collection.
- **Interactive Playground** using mock data with prefilled payloads and inline schema validation.
- **Error catalog** enumerating codes, HTTP status, description, remediation tips.
- **Versioning**: URI-based (`/v1`). Deprecations announced ≥180 days before enforcement; changelog entries link to migration steps.
- **SLAs**: Document latency, availability, and support response times (e.g., P1 within 30 minutes).
- **Compliance**: Dedicated page covering privacy-by-design, GDPR/CCPA posture, data retention, and audit readiness.

## Error Catalog Snapshot
| HTTP | Code | Message | Resolution |
| --- | --- | --- | --- |
| 400 | `INVALID_CONTEXT` | Unsupported `context` value | Correct enum value |
| 401 | `UNAUTHORIZED` | Missing/invalid credentials | Refresh token or rotate API key |
| 403 | `INSUFFICIENT_SCOPE` | Key lacks required scope | Request scope upgrade |
| 413 | `PAYLOAD_TOO_LARGE` | Batch exceeds limit | Split batch or use file mode |
| 429 | `RATE_LIMITED` | Rate limit exceeded | Honor `Retry-After`, contact support |
| 500 | `INTERNAL_ERROR` | Unexpected error | Retry with backoff; provide `trace_id` |

## Sandbox to Production Journey
1. **Discover**: Developer reads Overview, privacy guidance, and 3-minute Quickstart.
2. **Authenticate**: Generate sandbox API key via portal; highlight OAuth option for enterprises.
3. **First Call**: Execute copy-paste cURL example; Playground mirrors request for instant feedback.
4. **Integrate**: Swap to SDK sample; implement decision mapping (allow/challenge/block) using recommended thresholds.
5. **Tune**: Consult risk tuning guide; configure notifications on high-risk decisions.
6. **Go-live**: Submit production access request, complete checklist, review incident procedures.
7. **Maintain**: Subscribe to changelog RSS/email; monitor deprecation notices.

## Operational Policies
- **Latency**: `<3 ms p95` production, `10–30 ms` sandbox; publish real-time metrics on status page.
- **Availability**: 99.95% monthly target. Document failover procedures.
- **Support**: 24/7 for critical issues, 8x5 for sandbox queries.
- **Deprecation**: Minimum 180-day notice; dual-write period recommended.
- **Data retention**: Sandbox decisions 7 days, production 90 days (configurable up to 180 for enterprise).

## Compliance & Privacy Checklist
- Data minimization (hash personally identifiable identifiers).
- Encryption in transit (TLS 1.2+ with optional mTLS) and at rest (AES-256).
- GDPR readiness: Data processing agreements, audit logs, regional data residency options (EU, US).
- Incident response: Documented runbooks, with SLA commitments for partner notification.

## Related Assets
- [Documentation Information Architecture](./api-docs-information-architecture.md)
- [Content Templates](./content-templates.md)
- [Example Payload Cards](./example-payloads.md)
- [Visual Diagrams](./diagrams.md)

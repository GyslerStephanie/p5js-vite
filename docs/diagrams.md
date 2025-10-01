# Visual Diagrams

Use these Mermaid diagrams in the documentation platform to visualize critical flows.

## Real-Time Scoring Sequence

```mermaid
sequenceDiagram
  autonumber
  participant Client
  participant API as Sentinel API Gateway
  participant Engine as Risk Engine
  participant Policies as Policy Service

  Client->>API: POST /v1/score (transaction metadata)
  API->>Engine: Normalize & forward payload
  Engine->>Policies: Evaluate rules & ML models
  Policies-->>Engine: Policy hits & model score
  Engine-->>API: Decision (score, explanations, trace_id)
  API-->>Client: 200 OK (latency <3 ms p95)
  API-)Observability: Emit trace/span for audit
```

## Authentication Flow

```mermaid
flowchart LR
  A[Client App] -->|Client credentials| B(IdP / OAuth Server)
  B -->|Access token| A
  A -->|mTLS handshake (optional)| C[Sentinel API]
  A -->|Bearer token or X-Api-Key| C
  C --> D[Scope & rate limit check]
  D --> E[Request routed to service]
  D --> F[429 if burst exceeded]
```

## Rate Limit Explainer

```mermaid
stateDiagram-v2
  [*] --> WithinLimits
  WithinLimits --> BurstWindowExceeded: > 100 req / sec
  BurstWindowExceeded --> Throttled: if sustained window also exceeded
  BurstWindowExceeded --> WithinLimits: after short cooldown
  Throttled --> WithinLimits: after Retry-After expires
  WithinLimits --> SustainedLimitExceeded: > 10k req / min
  SustainedLimitExceeded --> Throttled
```

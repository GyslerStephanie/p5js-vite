# Example Payload Cards

These copy-ready cards blend technical fidelity with accessible explanations for partner developers and product stakeholders.

---
**Scenario:** New wallet-to-wallet transfer

```
POST /v1/score
Content-Type: application/json
```

```json
{
  "txn_id": "txn_wallet_5521",
  "timestamp": "2024-05-14T10:05:11Z",
  "amount": { "currency": "USD", "value": 180.00 },
  "context": "wallet_send",
  "counterparty_id": "hash_wallet_B9",
  "payer_id": "hash_wallet_A7",
  "device": {
    "device_id": "hash_device_web_443",
    "ip_partial": "198.51.100.0/27",
    "geo_coarse": "US-TX"
  },
  "signals": {
    "velocity_24h_bucket": "1",
    "keystroke_variance": 0.31
  },
  "channel": "web"
}
```

> **Why it matters:** Use when a customer sends funds to a new wallet. Latency target: `<3 ms p95` in production.

---
**Scenario:** Card-not-present ecommerce checkout

```
POST /v1/score
Content-Type: application/json
```

```json
{
  "txn_id": "txn_card_8821",
  "timestamp": "2024-05-14T19:45:03Z",
  "amount": { "currency": "GBP", "value": 79.90 },
  "context": "card",
  "counterparty_id": "hash_merchant_212",
  "payer_id": "hash_cardholder_557",
  "device": {
    "device_id": "hash_device_ios_19",
    "ip_partial": "203.0.113.128/28",
    "geo_coarse": "GB-LND"
  },
  "signals": {
    "past_reversal_rate": 0.18,
    "velocity_24h_bucket": "6-9"
  },
  "channel": "ios"
}
```

> **Decision example:** `risk_level: medium`, `decision: challenge` with explanations `"new device fingerprint"`, `"velocity spike"`.

---
**Scenario:** Batch scoring via inline array

```
POST /v1/batch/score
Content-Type: application/json
```

```json
{
  "batch_id": "batch_midday_1405",
  "mode": "inline",
  "transactions": [
    { "txn_id": "txn_001", "timestamp": "2024-05-14T12:00:00Z", "amount": { "currency": "USD", "value": 980.0 }, "context": "transfer", "counterparty_id": "hash_vendor_1", "payer_id": "hash_corp_44", "device": { "device_id": "hash_srv_01", "ip_partial": "192.0.2.0/28", "geo_coarse": "US-NY" }, "channel": "api" },
    { "txn_id": "txn_002", "timestamp": "2024-05-14T12:00:05Z", "amount": { "currency": "USD", "value": 5120.0 }, "context": "invoice", "counterparty_id": "hash_vendor_9", "payer_id": "hash_corp_44", "device": { "device_id": "hash_srv_02", "ip_partial": "192.0.2.0/28", "geo_coarse": "US-NY" }, "channel": "api" }
  ]
}
```

> **What to expect:** Response `202 Accepted` with `callback_url` or `polling_token`. SLA: results posted within 2 minutes for ≤10k records.

---
**Scenario:** Retrieve historical decision

```
GET /v1/events/{id}
```

> Provide `txn_id`, `trace_id`, or `batch_id` to pull audit-ready explanations and policy triggers.

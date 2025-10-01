# Quickstart Code Snippets

Each snippet demonstrates how to call `POST /v1/score` in sandbox using minimal dependencies. Replace placeholders with real credentials.

## cURL
```bash
curl --request POST "https://sandbox.api.example.com/v1/score" \
  --header "Authorization: Bearer $ACCESS_TOKEN" \
  --header "Content-Type: application/json" \
  --data '{
    "txn_id": "txn_quickstart_001",
    "timestamp": "2024-05-14T12:30:00Z",
    "amount": { "currency": "USD", "value": 120.5 },
    "context": "transfer",
    "counterparty_id": "hash_payee_demo",
    "payer_id": "hash_payer_demo",
    "device": {
      "device_id": "hash_device_cli",
      "ip_partial": "198.51.100.0/24",
      "geo_coarse": "US-CA"
    },
    "channel": "api"
  }'
```

## Node.js (fetch)
```javascript
import fetch from "node-fetch";

const payload = {
  txn_id: "txn_quickstart_001",
  timestamp: new Date().toISOString(),
  amount: { currency: "USD", value: 120.5 },
  context: "transfer",
  counterparty_id: "hash_payee_demo",
  payer_id: "hash_payer_demo",
  device: {
    device_id: "hash_device_js",
    ip_partial: "198.51.100.0/24",
    geo_coarse: "US-CA"
  },
  channel: "web"
};

const response = await fetch("https://sandbox.api.example.com/v1/score", {
  method: "POST",
  headers: {
    "Authorization": `Bearer ${process.env.ACCESS_TOKEN}`,
    "Content-Type": "application/json"
  },
  body: JSON.stringify(payload)
});

const result = await response.json();
console.log(result.decision, result.explanations);
```

## Python (requests)
```python
import os
import requests
from datetime import datetime, timezone

payload = {
    "txn_id": "txn_quickstart_001",
    "timestamp": datetime.now(timezone.utc).isoformat(),
    "amount": {"currency": "USD", "value": 120.5},
    "context": "transfer",
    "counterparty_id": "hash_payee_demo",
    "payer_id": "hash_payer_demo",
    "device": {
        "device_id": "hash_device_py",
        "ip_partial": "198.51.100.0/24",
        "geo_coarse": "US-CA"
    },
    "channel": "web"
}

resp = requests.post(
    "https://sandbox.api.example.com/v1/score",
    headers={
        "Authorization": f"Bearer {os.environ['ACCESS_TOKEN']}",
        "Content-Type": "application/json",
    },
    json=payload,
    timeout=3
)
resp.raise_for_status()
print(resp.json()["decision"])
```

## Java (HTTP Client)
```java
var client = java.net.http.HttpClient.newHttpClient();
var payload = """
{
  \"txn_id\": \"txn_quickstart_001\",
  \"timestamp\": \"2024-05-14T12:30:00Z\",
  \"amount\": { \"currency\": \"USD\", \"value\": 120.5 },
  \"context\": \"transfer\",
  \"counterparty_id\": \"hash_payee_demo\",
  \"payer_id\": \"hash_payer_demo\",
  \"device\": {
    \"device_id\": \"hash_device_java\",
    \"ip_partial\": \"198.51.100.0/24\",
    \"geo_coarse\": \"US-CA\"
  },
  \"channel\": \"web\"
}
""";

var request = java.net.http.HttpRequest.newBuilder()
    .uri(java.net.URI.create("https://sandbox.api.example.com/v1/score"))
    .header("Authorization", "Bearer " + System.getenv("ACCESS_TOKEN"))
    .header("Content-Type", "application/json")
    .POST(java.net.http.HttpRequest.BodyPublishers.ofString(payload))
    .build();

var response = client.send(request, java.net.http.HttpResponse.BodyHandlers.ofString());
System.out.println(response.body());
```

## Go
```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "net/http"
    "os"
    "time"
)

type Amount struct {
    Currency string  `json:"currency"`
    Value    float64 `json:"value"`
}

type Device struct {
    DeviceID  string `json:"device_id"`
    IPPartial string `json:"ip_partial"`
    GeoCoarse string `json:"geo_coarse"`
}

type ScoreRequest struct {
    TxnID         string  `json:"txn_id"`
    Timestamp     string  `json:"timestamp"`
    Amount        Amount  `json:"amount"`
    Context       string  `json:"context"`
    Counterparty  string  `json:"counterparty_id"`
    Payer         string  `json:"payer_id"`
    Device        Device  `json:"device"`
    Channel       string  `json:"channel"`
}

func main() {
    payload := ScoreRequest{
        TxnID:     "txn_quickstart_001",
        Timestamp: time.Now().UTC().Format(time.RFC3339),
        Amount:    Amount{Currency: "USD", Value: 120.5},
        Context:   "transfer",
        Counterparty: "hash_payee_demo",
        Payer:     "hash_payer_demo",
        Device: Device{
            DeviceID:  "hash_device_go",
            IPPartial: "198.51.100.0/24",
            GeoCoarse: "US-CA",
        },
        Channel: "web",
    }

    body, _ := json.Marshal(payload)
    req, _ := http.NewRequest(http.MethodPost, "https://sandbox.api.example.com/v1/score", bytes.NewReader(body))
    req.Header.Set("Authorization", "Bearer "+os.Getenv("ACCESS_TOKEN"))
    req.Header.Set("Content-Type", "application/json")

    resp, err := http.DefaultClient.Do(req)
    if err != nil {
        panic(err)
    }
    defer resp.Body.Close()

    fmt.Println("Status:", resp.Status)
}
```

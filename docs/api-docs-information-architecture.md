# Documentation Information Architecture

This navigation blueprint structures the Sentinel Fraud Decisioning API documentation to help external integrators and internal stakeholders discover information quickly. The left navigation groups content by the primary jobs developers need to accomplish.

## Overview
- **Welcome & Concepts**
  - Overview
  - Key Concepts (risk scoring lifecycle, decision outcomes)
  - Privacy & Data Handling
  - Compliance & Certifications
- **Getting Access**
  - Environments & Base URLs
  - Authentication (OAuth 2.0, API keys, mTLS)
  - Rate Limits & Quotas
  - SLAs & Support

## Build
- **Quickstarts**
  - 3-Minute Quickstart
  - cURL Walkthrough
  - SDK Quickstarts
    - Node.js
    - Python
    - Java
    - Go
  - Postman/Insomnia Collections
- **API Reference**
  - Score in Real Time (`POST /v1/score`)
  - Batch Score (`POST /v1/batch/score`)
  - Retrieve Decision (`GET /v1/events/{id}`)
  - Health Check (`GET /v1/health`)
  - Schema (`GET /v1/schema`)
  - Error Catalog
- **Guides**
  - Mapping Decisions to Product Flows
  - Risk Threshold Tuning
  - Sandbox vs Production Playbook
  - Webhooks & Callbacks
  - Secure Data Handling (Hashing, Tokenization)
  - Observability & Traceability

## Test & Validate
- **Playground**
  - Inline Console
  - Mock Datasets
  - Decision Explorer (filter by risk level)
- **Certification**
  - Checklist for Go-Live
  - Incident Response Procedures

## Maintain & Evolve
- **Changelog**
  - Release Notes
  - Deprecation Notices (180-day policy)
- **Roadmap & Feedback**
  - Upcoming Changes
  - Feature Request Portal
- **Support**
  - Troubleshooting
  - Contact Channels
  - Operational Status Page

## Resources
- **Download Center**
  - OpenAPI Specification
  - SDKs & Helper Libraries
  - Sample Payloads & Test Data
- **Compliance Library**
  - GDPR Statement
  - Privacy Impact Assessment Summary
  - Data Processing Agreements

> The left navigation is supplemented by contextual tabs (e.g., "Parameters", "Responses", "SDKs") on endpoint pages to minimize scroll depth while still exposing critical integration details.

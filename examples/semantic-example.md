---
name: api-gateway
description: Customer-facing REST API — authentication, routing, rate limiting
type: semantic
---

## Overview
Node.js/Express API gateway. Sits between clients and internal microservices. Handles auth, rate limiting, request routing, and response transformation.

## Architecture
```
Client → API Gateway → Auth Middleware → Rate Limiter → Router → Microservices
                                                              ├── user-service
                                                              ├── order-service
                                                              └── payment-service
```

- **Auth**: JWT (primary) + API key (legacy). Dual mode since v2.3.
- **Rate limiting**: Redis-backed sliding window. Per-client, per-endpoint.
- **Routing**: Express router with dynamic service discovery via Consul.

## Current State
- v2.3 deployed to production (2026-04-10)
- JWT auth live, API key backward compat maintained
- Rate limiter: 10k req/s tested, stable
- Known issue: WebSocket upgrade path doesn't go through rate limiter (tracked in JIRA-1234)

## Gotchas
- `ORDER_SERVICE_URL` env var must NOT have a trailing slash — the router concatenates paths
- Rate limiter Redis connection pool: max 20 connections. Going above causes silent queue drops
- The legacy API key format (`ak_xxxx`) passes auth but has no user context — some downstream services reject it

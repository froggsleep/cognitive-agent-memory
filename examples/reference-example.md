---
name: monitoring-stack
description: Where to find dashboards, logs, and alerts
type: reference
---

## System
Grafana + Loki monitoring stack. URL: `https://grafana.example.com`

## How to Access
- SSO login via company Google account
- CLI: `kubectl port-forward svc/grafana 3000:3000` for local access
- API key for automated queries: stored in 1Password vault "Engineering"

## What's There
- **API Gateway dashboard**: `grafana.example.com/d/api-gateway` — request rate, latency p50/p95/p99, error rate
- **Logs**: Loki datasource, filter by `{app="api-gateway"}`, last 24h default
- **Alerts**: PagerDuty integration, fires on >1% error rate sustained 5min

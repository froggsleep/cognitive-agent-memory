# Memory Index

## User
- [user_role.md](user_role.md) — Senior backend engineer, owns api-gateway and user-service

## Normative (norms/)
- [norms/communication.md](norms/communication.md) — Concise, formal, verify before answering, no over-engineering
- [norms/work-style.md](norms/work-style.md) — Autonomous, investigate before proposing, test before suggesting "try this"
- [norms/git-conventions.md](norms/git-conventions.md) — Conventional commits, PR < 400 lines, no force push

## Semantic (semantic/)
- [semantic/api-gateway.md](semantic/api-gateway.md) — REST API gateway: auth, rate limiting, routing (Node/Express)
- [semantic/user-service.md](semantic/user-service.md) — User management microservice (Go, PostgreSQL)

## Procedural (procedures/)
- [procedures/deploy-workflow.md](procedures/deploy-workflow.md) — Staging → load test → production deploy with rollback plan
- [procedures/incident-response.md](procedures/incident-response.md) — Triage → communicate → fix → postmortem

## Episodic (episodes/)
- [episodes/2026-04-10-session-1.md](episodes/2026-04-10-session-1.md) — Auth middleware refactor + rate limiter fix
- [episodes/2026-04-09-session-1.md](episodes/2026-04-09-session-1.md) — WebSocket investigation + JIRA-1234 filed

## Reference
- [reference_jira.md](reference_jira.md) — JIRA project GATEWAY, board "API Gateway Sprint"
- [reference_monitoring.md](reference_monitoring.md) — Grafana dashboard at grafana.internal/d/api-gateway

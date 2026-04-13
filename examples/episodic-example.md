---
date: 2026-04-10
session: 1
projects: [api-gateway]
---

## Work Done
- Refactored auth middleware to support JWT + API key dual mode
- Fixed rate limiter race condition under high concurrency
- Added integration tests for the new auth flow (12 tests)

## Decisions
- Chose Redis for rate limit state over in-memory: [reason] horizontal scaling needed → [outcome] 3ms overhead acceptable
- Kept backward compat with old API key format: [reason] 200+ active clients → [outcome] migration deferred to Q3

## Learned
- The rate limiter's sliding window implementation has an off-by-one at bucket boundaries — added a regression test
- `express-rate-limit` v7 changed the `skip` callback signature (undocumented)

## Next Actions
- [ ] Deploy auth refactor to staging
- [ ] Run load test (target: 10k req/s with new dual auth)
- [ ] Update API docs with JWT flow

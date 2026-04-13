---
name: deploy-to-production
description: Full production deploy workflow with rollback plan
type: procedural
---

## When to Use
Feature branch merged to main, all CI checks green, changelog updated.

## Steps
1. Pull latest `main` and verify clean state: `git pull && git status`
2. Run full test suite: `npm test` (must be 100% pass)
3. Build production artifacts: `npm run build`
4. Tag the release: `git tag v1.2.3 && git push --tags`
5. Deploy to staging: `./deploy.sh staging`
6. Run smoke tests on staging (health check + 3 critical paths)
7. Deploy to production: `./deploy.sh production`
8. Verify production health dashboard for 5 minutes
9. Update `#releases` channel with version + summary

## Watch Out For
- Database migrations must run BEFORE the deploy (check `pending-migrations` CI step)
- If staging smoke tests fail, do NOT proceed — fix and restart from step 2
- Rollback: `./deploy.sh production --rollback` reverts to previous tag
- Friday deploys require explicit team lead approval

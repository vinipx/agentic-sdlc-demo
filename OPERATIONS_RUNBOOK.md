# Operations Runbook — Agentic SDLC

## 1) Objective

Provide actionable operational steps for:
- incident triage
- recovery
- verification
- replay/backfill

## 2) Daily Health Checks

1. Cloudflare Worker reachable (POST)
2. Jira webhook delivery success rate
3. Orchestrator workflow run success
4. Plan PR fan-out completeness
5. Implementation workflow CI pass/fail trends

## 3) Smoke Test (Bridge)

```bash
curl -i -X POST "https://jira-github-bridge.<subdomain>.workers.dev/" \
  -H "content-type: application/json" \
  -H "x-bridge-token: <BRIDGE_TOKEN>" \
  -d '{"issue":{"key":"AISDLC-2"}}'
```

Expected:
- HTTP 200
- body includes `Dispatched AISDLC-2`

## 4) Incident Playbooks

### 4.1 Worker 401 Unauthorized
- Cause: header token mismatch
- Actions:
  1. Verify Jira custom header value
  2. Verify Cloudflare `BRIDGE_TOKEN`
  3. Rotate secret if leaked

### 4.2 Worker 502 / GitHub 404
- Cause: wrong repo/owner or token access
- Actions:
  1. Confirm `GH_OWNER`
  2. Confirm `GH_ORCHESTRATOR_REPO`
  3. Verify token/app installation repo access

### 4.3 Orchestrator Not Triggered
- Actions:
  1. Confirm workflow exists on default branch
  2. Confirm trigger is `repository_dispatch` type `jira_issue_event`
  3. Check event payload contains `jira_key`

### 4.4 Plan PRs Missing for Some Repos
- Actions:
  1. Check impact analysis output
  2. Verify token/app has cross-repo write access
  3. Check branch/PR creation errors in logs

### 4.5 Implementation CI Failing
- Actions:
  1. Inspect build logs (`mvn clean verify`)
  2. Confirm auto-fix step is not placeholder
  3. Apply deterministic patch or escalate to service owner

## 5) Replay / Backfill Procedure

Use workflow manual dispatch (`workflow_dispatch`) with `jira_key`:
- Example: `AISDLC-2`
- Purpose: rerun without relying on Jira webhook re-delivery

## 6) Escalation Matrix

- Platform/DevEx: bridge, tokens/apps, orchestrator
- Service team: repo implementation failures
- Product/Eng mgmt: Jira workflow or policy conflicts

## 7) Post-Incident Checklist

- Root cause documented
- Configuration corrected
- Preventive guard added (validation, alerts, tests)
- Runbook updated

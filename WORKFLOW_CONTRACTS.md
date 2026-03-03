# Workflow Contracts — Agentic SDLC

## 1) Purpose

Define stable contracts between:
- Jira Automation payload/action
- Cloudflare Worker bridge
- Orchestrator workflow
- Repository implementation workflows

## 2) Event Contract: Jira Automation → Worker

### Jira Automation action
- Action: **Send web request**
- Method: `POST`
- URL: Worker endpoint
- Required headers:
  - `Content-Type: application/json`
  - `x-bridge-token: <shared-secret>`

### Expected minimum body (examples)
```json
{ "issue": { "key": "AISDLC-2" } }
```

Worker extraction order:
1. `body.issue.key`
2. `body.data.issue.key`
3. `body.key`

---

## 3) Dispatch Contract: Worker → GitHub

Endpoint:
- `POST /repos/{owner}/{repo}/dispatches`

Body:
```json
{
  "event_type": "jira_issue_event",
  "client_payload": {
    "jira_key": "AISDLC-2"
  }
}
```

---

## 4) Orchestrator Contract

Trigger:
- `on: repository_dispatch`
- `types: [jira_issue_event]`

Input assumption:
- `github.event.client_payload.jira_key` exists

Output expectations:
- Plan file(s) created:
  - `docs/implementation-plans/<safe_jira>-plan.md`
- Plan PR title format:
  - `[<JIRA_KEY>] Implementation plan`
- Branch format:
  - `agentic-plan/<safe_jira>-<run_id>`

---

## 5) Implementation Workflow Contract

Trigger:
- push to `main` on:
  - `docs/implementation-plans/*-plan.md`
- optional `workflow_dispatch` with:
  - `jira_key`

Derived values:
- `safe_jira`: lowercase/sanitized key
- plan file:
  - `docs/implementation-plans/<safe_jira>-plan.md`
- done marker:
  - `docs/implementation-plans/<safe_jira>-done.md`
- implementation branch:
  - `agentic-impl/<safe_jira>-<run_id>`

Required behavior:
1. Validate plan exists
2. Guard idempotency (done marker)
3. Apply implementation changes
4. Build/Test
5. Optional auto-fix with bounded retries
6. Commit + push branch
7. Open implementation PR

---

## 6) PR Contract

Implementation PR title:
- `[<JIRA_KEY>] Implementation from merged plan`

PR body should include:
- Jira URL
- Summary
- Risk level
- Impacted repositories

---

## 7) Failure Handling Contract

- Build failure must block completion unless fixed
- Auto-fix must be bounded (`MAX_RETRIES`)
- No silent success when CI fails
- Logs must identify first root-cause error

---

## 8) Compatibility Rules

- Keep branch/file naming stable for automation interoperability
- If contract changes, update:
  1. Jira Automation rule
  2. Worker
  3. Orchestrator workflow
  4. Implementation workflow
  5. This document

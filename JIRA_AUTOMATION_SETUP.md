# Jira Automation Setup Guide (Jira → Cloudflare Worker → GitHub)

> Note: This project uses **Jira Automation** as the sender mechanism (instead of Jira System Webhooks), because Automation provides header configuration and audit visibility.

## 1) Prerequisites

- Cloudflare Worker deployed:
  - `https://jira-github-bridge.<subdomain>.workers.dev/`
- Worker secret:
  - `BRIDGE_TOKEN=<shared-secret>`
- GitHub orchestrator workflow ready for `repository_dispatch`
- Jira permissions to create/manage automation rules

## 2) Create Automation Rule

1. Go to **Project settings** → **Automation**
2. Create Rule
3. Trigger:
   - Issue created (optionally Issue updated)
4. Condition (recommended):
   - labels contains `agentic-sdlc`
5. Action: **Send web request**
   - URL: Worker URL
   - Method: POST
   - Headers:
     - `Content-Type: application/json`
     - `x-bridge-token: <shared-secret>`
   - Body:
     ```json
     { "issue": { "key": "{{issue.key}}" } }
     ```
6. Publish rule

## 3) Validation

- Create issue with label `agentic-sdlc`
- Check Automation **Audit Log** (action should be successful)
- Confirm Worker returns 200
- Confirm GitHub orchestrator workflow run is triggered

## 4) Troubleshooting

- 401: missing/wrong `x-bridge-token`
- 502/404: wrong GitHub repo/owner/access
- No workflow run: verify `event_type` and orchestrator trigger

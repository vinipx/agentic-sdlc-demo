# Jira Webhook Setup Guide (Jira → Cloudflare Worker → GitHub)

This guide explains exactly how to configure Jira webhook delivery to your Cloudflare bridge, which then dispatches GitHub workflows.

## 1) Prerequisites

- Cloudflare Worker deployed (example):
  - `https://jira-github-bridge.<subdomain>.workers.dev/`
- Worker secret configured:
  - `BRIDGE_TOKEN=<shared-secret>`
- GitHub orchestrator workflow ready to receive `repository_dispatch`
- Jira admin access (or permission to create automation/webhooks)

---

## 2) Decide Integration Mode in Jira

You can configure this in either:

1. **Jira System Webhook** (global admin configuration), or
2. **Jira Automation Rule** with **Send web request** action (project-level or global)

If you need filtering and flexibility, Jira Automation is usually easier to manage.

---

## 3) Configuration Values

Use these values in Jira:

- **Method**: `POST`
- **URL**: `https://jira-github-bridge.<subdomain>.workers.dev/`
- **Content-Type**: `application/json`
- **Custom Header**:
  - `x-bridge-token: <same value as BRIDGE_TOKEN in Worker>`

### Recommended Event Scope

- Issue Created
- Issue Updated
- (Optional) Issue Transitioned

### Recommended JQL filter

```jql
project = AISDLC AND labels = agentic-sdlc
```

This prevents unrelated Jira issues from triggering the SDLC pipeline.

---

## 4) Example Payload

Most Jira webhook payloads already include issue key as:

```json
{
  "issue": {
    "key": "AISDLC-2"
  }
}
```

Your Worker can support fallback extraction:
1. `body.issue.key`
2. `body.data.issue.key`
3. `body.key`

---

## 5) Step-by-Step (Jira System Webhook)

1. Go to **Jira Settings** → **System** → **Webhooks**
2. Click **Create a WebHook**
3. Name: `Agentic SDLC Bridge`
4. URL: `https://jira-github-bridge.<subdomain>.workers.dev/`
5. Choose events:
   - Issue created
   - Issue updated
6. Add JQL filter (optional but recommended)
7. Add custom header:
   - `x-bridge-token: <shared-secret>`
8. Save

---

## 6) Step-by-Step (Jira Automation Rule)

1. Go to **Project settings** → **Automation**
2. Create Rule
3. Trigger:
   - Issue created (and optionally issue updated)
4. Condition (optional):
   - JQL matches `project = AISDLC AND labels = agentic-sdlc`
5. Action: **Send web request**
   - URL: Worker URL
   - Method: POST
   - Headers:
     - `Content-Type: application/json`
     - `x-bridge-token: <shared-secret>`
   - Body: JSON payload containing issue key
6. Publish rule

---

## 7) Validation

## 7.1 Manual curl smoke test

```bash
curl -i -X POST "https://jira-github-bridge.<subdomain>.workers.dev/" \
  -H "content-type: application/json" \
  -H "x-bridge-token: <shared-secret>" \
  -d '{"issue":{"key":"AISDLC-2"}}'
```

Expected:
- HTTP `200`
- Response includes: `Dispatched AISDLC-2`

## 7.2 Jira delivery verification

- Check Jira webhook/automation audit logs for HTTP response.
- Confirm orchestrator workflow run triggered in GitHub.
- Confirm event payload includes Jira key.

---

## 8) Troubleshooting

### 401 Unauthorized (Worker)
- Header missing or wrong token
- Fix Jira header value to match Worker `BRIDGE_TOKEN`

### 405 Method Not Allowed
- Used GET instead of POST
- Use POST only

### 502 GitHub dispatch failed: 404
- Wrong `GH_OWNER` / `GH_ORCHESTRATOR_REPO`
- Token/app does not have repo access

### Orchestrator not triggered
- Workflow not configured for `repository_dispatch`
- Wrong `event_type` or payload shape

---

## 9) Security Best Practices

- Use strong random value for `BRIDGE_TOKEN`
- Rotate token periodically
- Restrict Jira events via JQL
- Prefer GitHub App tokens over PAT for downstream GitHub actions
- Avoid logging sensitive headers/tokens

---

## 10) Change Management

When changing any of these, update docs and test end-to-end:
- Worker URL
- Shared secret
- Orchestrator repository
- `repository_dispatch` event type
- Jira event/JQL filters

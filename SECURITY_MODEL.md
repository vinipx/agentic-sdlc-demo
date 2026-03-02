# Security Model — Agentic SDLC

## 1) Security Objectives

- Authenticate webhook source
- Minimize token blast radius
- Enforce least privilege
- Ensure auditable automation actions

## 2) Trust Boundaries

1. Jira Cloud (external source)
2. Cloudflare Worker (public ingress)
3. GitHub API (internal automation target)
4. GitHub repositories (code + workflows)

## 3) Authentication & Authorization

### 3.1 Jira → Worker
- Header: `x-bridge-token`
- Worker validates against secret `BRIDGE_TOKEN`
- Rejects invalid calls with 401

### 3.2 Worker → GitHub
- Prefer GitHub App installation token (short-lived)
- PAT allowed only temporarily / fallback

### 3.3 GitHub Actions → Repos
- Use GitHub App token with least required permissions:
  - Contents: Read/Write
  - Pull requests: Read/Write
  - Metadata: Read

## 4) Secret Management

- Cloudflare secrets:
  - `BRIDGE_TOKEN`
  - GitHub credentials (if used by Worker)
- GitHub Actions secrets:
  - `GH_APP_ID`
  - `GH_APP_PRIVATE_KEY`
  - `GH_APP_INSTALLATION_ID` (if flow requires)

Guidelines:
- Never print full secrets in logs
- Rotate periodically and after suspected exposure
- Scope repo access to only required repos

## 5) Threats and Mitigations

- **Unauthorized trigger spam**
  - Mitigation: `x-bridge-token`, optional IP filtering/rate limiting
- **Credential leakage**
  - Mitigation: secret storage, masked logs, rotation policy
- **Over-privileged automation**
  - Mitigation: GitHub App least privilege, limited installation scope
- **Payload tampering**
  - Mitigation: strict schema validation in Worker + workflow input checks

## 6) Security Controls Checklist

- [ ] Worker rejects missing/invalid token
- [ ] GitHub App installed only on target repos
- [ ] PAT usage minimized or removed
- [ ] Secrets rotated and ownership assigned
- [ ] Audit trail available for automated commits/PRs

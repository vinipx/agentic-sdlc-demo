# Agentic SDLC Demo — Go-Live Checklist

## Context
Date: 2026-03-01  
Scope: `vinipx/service-alpha`, `vinipx/service-beta`, `vinipx/common-library`  
Reviewer simulation: `@vinipxf`

---

## A) Repository Readiness (All Repositories)

### A.1 Governance Files
- [ ] `.github/copilot-instructions.md` exists
- [ ] `CODEOWNERS` exists and contains `* @vinipxf`
- [ ] `java-coding-guidelines.md` exists (or is clearly referenced)
- [ ] `.github/pull_request_template.md` exists
- [ ] `.github/ISSUE_TEMPLATE/story-intake.yml` exists (optional in `common-library`)

### A.2 Workflow Files
- [ ] `.github/workflows/ci.yml` exists and runs on PR/push
- [ ] `.github/workflows/agentic-sdlc.yml` exists (`service-alpha` and `service-beta`; optional `common-library`)

### A.3 Branch Protection
- [ ] `main` requires pull requests
- [ ] `main` requires status checks (CI)
- [ ] `main` requires code owner review
- [ ] direct pushes to `main` are restricted

---

## B) Integration & Secrets Readiness

### B.1 Jira Integration (if live)
- [ ] `JIRA_BASE_URL` configured (repo or org secret)
- [ ] `JIRA_EMAIL` configured
- [ ] `JIRA_API_TOKEN` configured
- [ ] Jira API access validated with a test call

### B.2 Demo Fallback
- [ ] Manual workflow inputs tested (no Jira dependency mode)
- [ ] Backup story data prepared in case Jira is unavailable

---

## C) Demo Story Preparation

- [ ] One Jira story selected with clear acceptance criteria
- [ ] Story has realistic cross-repo impact
- [ ] Expected impacted repos identified (alpha/beta/common-library)
- [ ] Risk level and rollback notes drafted
- [ ] Success criteria for demo explicitly defined

---

## D) Execution Checklist (Live Run)

- [ ] Create Story Intake issue from template
- [ ] Trigger `agentic-sdlc.yml` manually
- [ ] Verify implementation plan artifact is generated
- [ ] Verify branch creation and commit(s)
- [ ] Verify PR creation succeeds
- [ ] Verify `@vinipxf` review request appears (CODEOWNERS)
- [ ] Verify CI checks pass
- [ ] Merge in dependency-safe order (common-library first if changed)

---

## E) Evidence Capture (for Client Deck)

- [ ] Screenshot: Story Intake issue
- [ ] Screenshot: Workflow dispatch inputs
- [ ] Screenshot: Successful workflow run
- [ ] Screenshot: Implementation plan artifact
- [ ] Screenshot: PR details (Jira link + summary + test evidence)
- [ ] Screenshot: CODEOWNERS reviewer assignment
- [ ] Screenshot: CI checks green
- [ ] Screenshot: Post-merge status / Jira traceability update

---

## F) KPI Collection (Before vs After)

- [ ] Time from story intake to first PR
- [ ] Time from PR open to approval
- [ ] Number of repositories changed
- [ ] Unit/integration tests added
- [ ] Coverage trend (if available)
- [ ] Number of manual handoffs reduced

---

## G) Risk & Rollback Readiness

- [ ] Rollback strategy documented per repo
- [ ] Revert procedure tested for at least one PR
- [ ] Breaking-change communication plan prepared
- [ ] Dependency versioning strategy validated (`common-library`)

---

## H) Post-Demo Actions

- [ ] Lessons learned documented
- [ ] Workflow improvements backlog created
- [ ] Security review follow-ups recorded
- [ ] Plan for Phase 4 (production pilot) drafted

---

## Demo Sign-Off

- [ ] Technical sign-off complete
- [ ] Stakeholder sign-off complete
- [ ] Next-phase owners assigned

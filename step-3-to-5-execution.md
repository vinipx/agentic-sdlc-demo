# Step-by-Step Guide — Steps 3 to 5

Date reference: 2026-03-01

This guide operationalizes:

- **Step 3:** Prepare secrets for Jira integration  
- **Step 4:** Run your first demo story through workflow  
- **Step 5:** Capture evidence for client presentation

---

## Step 3 — Configure Jira Secrets (or Manual Fallback)

## 3.1 Decide execution mode
Choose one:

1. **Live Jira mode** (recommended for advanced demo)  
2. **Manual input mode** (recommended for stable first run)

If this is your first client demo rehearsal, use manual mode first, then live mode.

## 3.2 Add secrets (Live Jira mode)
For each repo (or at org level), add:

- `JIRA_BASE_URL` (e.g., `https://your-domain.atlassian.net`)
- `JIRA_EMAIL`
- `JIRA_API_TOKEN`

Path in GitHub:
- **Repo → Settings → Secrets and variables → Actions → New repository secret**

## 3.3 Validate secret availability
Run a lightweight workflow or add a temporary debug step to confirm secrets are present (do not print secret values).
Use a check like:

- if secret exists → proceed
- if missing → fail fast with clear message

## 3.4 Keep fallback ready
If Jira API is unavailable during demo:

- use `workflow_dispatch` inputs (`jira_key`, `jira_url`, `story_summary`, etc.)
- proceed with the rest of the pipeline unchanged

---

## Step 4 — Run First Demo Story End-to-End

## 4.1 Prepare demo story
Pick one story with cross-repo potential, for example:

- New validation utility in `common-library`
- Endpoint behavior update in `service-alpha` and `service-beta`
- Associated unit + integration tests

Ensure acceptance criteria are explicit and testable.

## 4.2 Create Story Intake issue
In each service repo (or primary orchestrator repo):

1. Go to **Issues → New issue**
2. Select **Story Intake (Agentic SDLC)**
3. Fill:
   - Jira key
   - Jira link
   - business context
   - acceptance criteria
   - impacted repos
   - risk level
4. Submit issue

## 4.3 Trigger workflow manually
Path:
- **Actions → Agentic SDLC (Manual Trigger) → Run workflow**

Provide:
- `jira_key`
- `jira_url`
- `story_summary`
- `impacted_repositories`
- `risk_level`

## 4.4 Validate workflow execution
Check run logs for:

- Plan file generated (e.g., `docs/implementation-plans/<jira>-plan.md`)
- Branch created (e.g., `agentic/<jira-key>`)
- Commit pushed successfully
- Pull request opened against `main`

## 4.5 Validate governance
In the created PR:

- Confirm PR template fields are present
- Confirm Jira linkage is present
- Confirm CI job started and passed
- Confirm CODEOWNERS requested `@vinipxf`

## 4.6 Merge strategy
If multiple repos changed:

1. Merge `common-library` first (if shared dependency changed)
2. Merge `service-alpha` and `service-beta` after dependency is available
3. Confirm post-merge CI passes on `main`

---

## Step 5 — Capture Demo Evidence Pack

## 5.1 Mandatory screenshots/artifacts
Capture these in sequence:

1. Story Intake issue (filled values visible)
2. Workflow dispatch form (inputs)
3. Successful workflow run summary
4. Generated implementation plan file
5. Created PR overview (title/body/Jira link)
6. Reviewer assignment showing `@vinipxf`
7. CI checks green
8. Merge confirmation (and optional Jira update)

## 5.2 Suggested folder structure for evidence
Store in your phases-plan repo:

- `docs/demo-evidence/<JIRA-KEY>/01-story-intake.png`
- `docs/demo-evidence/<JIRA-KEY>/02-workflow-inputs.png`
- `docs/demo-evidence/<JIRA-KEY>/03-workflow-success.png`
- `docs/demo-evidence/<JIRA-KEY>/04-implementation-plan.png`
- `docs/demo-evidence/<JIRA-KEY>/05-pr-opened.png`
- `docs/demo-evidence/<JIRA-KEY>/06-codeowners-reviewer.png`
- `docs/demo-evidence/<JIRA-KEY>/07-ci-green.png`
- `docs/demo-evidence/<JIRA-KEY>/08-merge-complete.png`

## 5.3 KPI quick capture table
For each demo run, record:

- Story key
- Start timestamp
- First PR timestamp
- Merge timestamp
- # repos impacted
- # tests added/updated
- CI result
- Observed blockers

## 5.4 Build the client narrative
For each evidence item, prepare one sentence:

- “This proves requirement ingestion”
- “This proves automated planning”
- “This proves governance (CODEOWNERS + CI)”
- “This proves traceable, reviewable delivery”

---

## Exit Criteria for Steps 3–5 Complete

- [ ] Secrets configured OR manual fallback confirmed
- [ ] One full workflow run completed successfully
- [ ] PR created and reviewed with CODEOWNERS routing
- [ ] CI green and merge completed
- [ ] Evidence pack archived for client demo

---

## Recommended Immediate Next Action

After completing this guide once, run a **second story** with a different risk profile (low vs medium/high) to demonstrate repeatability and robustness.

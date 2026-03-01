# Phase 3 — Client Demo Script (Agentic SDLC)

## Purpose
A practical talk track to demonstrate how GitHub Copilot agents accelerate and govern SDLC for a multi-repo Java architecture.

---

## Demo Duration
30–40 minutes (recommended)

- Intro: 5 min
- Architecture walkthrough: 8 min
- Live flow (Jira → plan → code/tests → PRs): 20 min
- Results + Q&A: 7 min

---

## 1) Opening (What problem we solve)

**Presenter script:**
“We’ll demonstrate how AI agents reduce lead time from requirement to reviewable PR, while preserving enterprise controls: coding standards, tests, code ownership, and Jira traceability.”

**Key points:**
- Multi-repo complexity usually slows delivery.
- Agents can automate repetitive SDLC steps.
- Governance remains enforced (CODEOWNERS, quality gates, review).

---

## 2) Environment Overview

Show three repositories:

- `service-alpha` (Spring Boot CRUD service)
- `service-beta` (Spring Boot CRUD service)
- `common-library` (shared utilities)

Show key governance files in each repository:

- `.github/copilot-instructions.md`
- `CODEOWNERS` with `@vinipxf`
- `java-coding-guidelines.md` (shared standard)

**Presenter script:**
“These files are the guardrails. Agents don’t code in a vacuum—they follow explicit repository context and standards.”

---

## 3) Start from a Jira User Story

Use one prepared Jira story with:

- business objective
- acceptance criteria
- non-functional constraints

**Presenter script:**
“The workflow starts from Jira, not from random prompts. This ensures traceability and business alignment.”

---

## 4) Requirement Ingestion + Blast Radius

Show generated impact analysis output (or pre-baked JSON/report) with:

- impacted repos
- impacted components
- risk rating
- dependency ordering

**Presenter script:**
“The agent determines where change is needed and in what sequence, especially when `common-library` affects both services.”

---

## 5) Implementation Plan

Show phased plan:

1. Update `common-library` (if shared concern)
2. Apply `service-alpha` changes
3. Apply `service-beta` changes
4. Generate/update tests
5. Validate CI and quality gates
6. Open PRs

**Presenter script:**
“Before coding, the agent creates an explicit implementation plan so teams can review strategy first.”

---

## 6) Autonomous Implementation

Demonstrate agent-generated changes in each repo:

- service/controller changes
- DTO/validation updates
- shared utility changes
- exception handling consistency

**Presenter script:**
“Code generation is constrained by Java guidelines and repository instructions. This improves consistency across teams.”

---

## 7) Test Automation

Show generated:

- Unit tests (logic and edge cases)
- Integration tests (API + persistence behavior)

Then show CI pass + coverage results.

**Presenter script:**
“Quality is not optional. The agent delivers code and tests together, then validates quality gates.”

---

## 8) Pull Request Automation + CODEOWNERS

Show PRs created per repo with:

- Jira linkage
- change summary
- test evidence
- rollback notes

Show `@vinipxf` auto-requested as reviewer via CODEOWNERS.

**Presenter script:**
“Even with automation, merge authority remains with code owners. This keeps accountability intact.”

---

## 9) Traceability Closure

Show Jira updated with:

- PR links
- commit references
- status summary

**Presenter script:**
“This closes the loop from requirement to reviewed code artifacts with full auditability.”

---

## 10) Value Recap (for business stakeholders)

- Faster lead time from story to PR
- Better consistency across repositories
- Higher test coverage by default
- Stronger governance and compliance
- Repeatable pattern for scale

---

## 11) Suggested Q&A Responses

**Q: Does this replace engineers?**  
A: No. It amplifies engineers by automating repetitive work; architects and reviewers still govern decisions.

**Q: How do you avoid unsafe code?**  
A: Repository instructions, coding guidelines, tests, static analysis, security scans, and CODEOWNERS approval.

**Q: Can this scale beyond 3 repos?**  
A: Yes. The orchestration pattern is repo-agnostic and can be expanded incrementally.

---

## 12) Demo Readiness Checklist

- [ ] Jira story prepared
- [ ] Integrations validated (Jira ↔ GitHub)
- [ ] Repositories accessible
- [ ] Copilot instructions committed
- [ ] CODEOWNERS committed
- [ ] Java guidelines committed
- [ ] CI pipelines green
- [ ] PR template available
- [ ] Backup screenshots ready (in case live env fails)

---

## Closing Statement

“This prototype demonstrates an enterprise-ready agentic SDLC: fast, governed, test-driven, and traceable across multiple Java repositories.”

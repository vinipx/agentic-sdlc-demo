# Agentic SDLC Phases Plan Repository

This repository stores architecture, process, and demo artifacts for the Agentic SDLC prototype using GitHub Copilot agents in a multi-repository Java ecosystem.

## Contents

- `phase-1-task-1.2-artifacts.md`  
  Consolidated Phase 1 Task 1.2 deliverables:
  - `.github/copilot-instructions.md` for each repo
  - `CODEOWNERS` for each repo
  - `java-coding-guidelines.md`

- `phase-2-agentic-workflow-architecture.md`  
  End-to-end architectural sequence from Jira ingestion to PR and traceability closure.

- `phase-3-demo-script.md`  
  Client-facing demo runbook and presenter talk track.

## Prototype Scope

Target repositories:

- `vinipx/service-alpha`
- `vinipx/service-beta`
- `vinipx/common-library`

## Design Principles

- **Governed automation** (not uncontrolled generation)
- **Cross-repo consistency**
- **Test-first quality posture**
- **Code-owner enforced approvals**
- **Jira-to-code traceability**

## How to Use This Repository

1. Use `phase-1-task-1.2-artifacts.md` to bootstrap governance files.
2. Use `phase-2-agentic-workflow-architecture.md` to explain technical flow and controls.
3. Use `phase-3-demo-script.md` to run stakeholder/client demonstrations.

## Recommended Next Additions

- `agentic-sdlc-workflow.yml` (GitHub Actions orchestration)
- `implementation-plan-template.json`
- PR template with mandatory architecture/testing sections
- Architecture diagram assets (`/docs/diagrams`)

## Audience

- Solution Architects
- Engineering Managers
- Platform Teams
- Client Stakeholders evaluating AI-enabled SDLC modernization

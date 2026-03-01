# Phase 1 — Task 1.2 Artifacts (Consolidated)

This document centralizes all required Phase 1 Task 1.2 files for the three-repository Java prototype.

Repositories covered:

- `vinipx/service-alpha`
- `vinipx/service-beta`
- `vinipx/common-library`

---

## 1) `vinipx/service-alpha/.github/copilot-instructions.md`

```markdown
# Copilot Agent Instructions — service-alpha

## Repository Purpose
`service-alpha` is a Spring Boot RESTful microservice responsible for CRUD APIs in the Alpha domain.
It consumes shared utilities from `vinipx/common-library`.

## Boundaries & Responsibilities
- Owns Alpha domain REST endpoints, service layer, persistence adapters, and API contracts.
- Must NOT duplicate reusable utilities that belong in `common-library`.
- Any cross-cutting utility should be implemented in `common-library` and then consumed here.

## Tech Stack
- Java 21+
- Spring Boot 3.x
- Maven
- JUnit 5, Mockito, Testcontainers (for integration tests)

## Agent Operating Instructions
1. Read linked Jira issue context before coding.
2. Validate impact:
   - Local impact (service-alpha)
   - Shared impact (common-library)
   - Downstream impact (service-beta if shared contract changes)
3. Follow `java-coding-guidelines.md` strictly.
4. Keep changes minimal, cohesive, and traceable to acceptance criteria.
5. Generate:
   - Unit tests for all new logic
   - Integration tests for endpoint/data flow changes
6. Update API docs/OpenAPI annotations if endpoint contract changes.
7. Ensure backward compatibility unless Jira explicitly allows breaking changes.
8. Produce clear commit messages referencing Jira key.
9. Open PR with summary:
   - Business context
   - Technical changes
   - Test evidence
   - Risk/rollback notes

## Definition of Done (Agent)
- Build passes
- Tests pass
- Static checks pass
- Acceptance criteria mapped to code + tests
- PR created and ready for code owner review
```

---

## 2) `vinipx/service-alpha/CODEOWNERS`

```text
# Enforce repository-wide ownership
* @vinipxf
```

---

## 3) `vinipx/service-beta/.github/copilot-instructions.md`

```markdown
# Copilot Agent Instructions — service-beta

## Repository Purpose
`service-beta` is a Spring Boot RESTful microservice responsible for CRUD APIs in the Beta domain.
It depends on `vinipx/common-library` for shared helpers and reusable components.

## Boundaries & Responsibilities
- Owns Beta-specific domain behavior and API endpoints.
- Should not copy shared logic already suitable for `common-library`.
- Contract changes must be analyzed for compatibility and consumer impact.

## Tech Stack
- Java 21+
- Spring Boot 3.x
- Maven
- JUnit 5, Mockito, Testcontainers (integration level)

## Agent Operating Instructions
1. Ingest Jira user story and acceptance criteria first.
2. Perform cross-repository impact check:
   - `service-beta` direct impact
   - `common-library` shared code impact
   - Compatibility with `service-alpha`
3. Implement per `java-coding-guidelines.md`.
4. Include robust tests:
   - Unit tests for business logic
   - Integration tests for controllers/repositories/external behavior
5. Keep API contracts explicit and version-safe.
6. Document behavior changes in PR description.
7. Reference Jira key in branch/commits/PR.

## Definition of Done (Agent)
- No unresolved TODOs
- Green CI
- Coverage maintained or improved
- PR opened with complete implementation + test rationale
```

---

## 4) `vinipx/service-beta/CODEOWNERS`

```text
# Enforce repository-wide ownership
* @vinipxf
```

---

## 5) `vinipx/common-library/.github/copilot-instructions.md`

```markdown
# Copilot Agent Instructions — common-library

## Repository Purpose
`common-library` contains shared Java utilities, helpers, base abstractions, and cross-cutting components consumed by:
- `vinipx/service-alpha`
- `vinipx/service-beta`

## Boundaries & Responsibilities
- Host only reusable, domain-agnostic/shared capabilities.
- Do NOT include service-specific business rules.
- Versioning and backward compatibility are mandatory concerns.

## Tech Stack
- Java 21+
- Maven (publishable artifact)
- JUnit 5

## Agent Operating Instructions
1. Start from Jira context and identify shared concern candidate.
2. Validate necessity of shared abstraction:
   - Is it reused now or expected reusable soon?
3. Design for stable contracts:
   - Clear interfaces
   - Semantic versioning mindset
4. Add JavaDocs for public APIs.
5. Add comprehensive unit tests for all exported behaviors.
6. If breaking changes are unavoidable:
   - Document migration notes
   - Signal major version impact in PR
7. Coordinate consumer updates in `service-alpha` and `service-beta`.

## Definition of Done (Agent)
- Public API documented
- Tests green
- Artifact version updated appropriately
- Consumer impact documented
- PR ready for code owner review
```

---

## 6) `vinipx/common-library/CODEOWNERS`

```text
# Enforce repository-wide ownership
* @vinipxf
```

---

## 7) `java-coding-guidelines.md` (shared standard)

```markdown
# Java Coding Guidelines (Agent-Enforced)

## 1. Language & Platform
- Java 21+ required.
- Spring Boot 3.x conventions.
- Prefer immutable data structures and constructor injection.
- Enable `-parameters` compiler flag where applicable.

## 2. Architectural Principles
- Follow clean layering:
  - Controller (API)
  - Service (business logic)
  - Repository/Adapter (persistence/external)
- No business logic in controllers.
- Shared cross-cutting logic belongs in `common-library`.
- Favor composition over inheritance.

## 3. API Design
- Use resource-oriented REST conventions.
- Use DTOs for API boundaries; never expose JPA entities directly.
- Validate input with Jakarta Validation annotations.
- Return meaningful HTTP status codes and structured error payloads.
- Maintain backward compatibility unless explicitly approved.

## 4. Naming & Code Style
- Classes: `PascalCase`
- Methods/variables: `camelCase`
- Constants: `UPPER_SNAKE_CASE`
- Package names: lowercase, domain-oriented.
- Method size: keep focused and small.
- Avoid deep nesting; prefer guard clauses.

## 5. Error Handling
- Use domain-specific exceptions.
- Centralize error mapping via `@ControllerAdvice`.
- Never swallow exceptions silently.
- Include actionable error messages without leaking secrets.

## 6. Logging & Observability
- Use structured, parameterized logs.
- Never log credentials, tokens, or sensitive PII.
- Include correlation/request IDs when available.
- Log at proper levels (`INFO`, `WARN`, `ERROR`, `DEBUG`).

## 7. Security
- Validate and sanitize all external inputs.
- Principle of least privilege for integrations.
- Keep secrets in environment/secret manager, never in source code.
- Ensure dependency vulnerability scanning is enabled.

## 8. Testing Standards
### Unit Tests
- Framework: JUnit 5 + Mockito.
- Test behavior, not implementation details.
- Naming pattern: `methodName_condition_expectedResult`.
- Cover positive, negative, and edge cases.

### Integration Tests
- Use Spring Boot test slices and full-context tests only where needed.
- Prefer Testcontainers for real dependency fidelity (DB, message broker).
- Verify endpoint contracts, serialization, and persistence flows.

### Coverage & Quality Gates
- Minimum line coverage: 80% (higher for shared library critical modules).
- Critical paths require explicit edge-case tests.
- CI must fail on test failures and static analysis violations.

## 9. Dependency & Versioning
- Pin dependency versions intentionally.
- Avoid unnecessary libraries.
- For `common-library`, follow semantic versioning:
  - MAJOR: breaking API changes
  - MINOR: backward-compatible features
  - PATCH: fixes

## 10. Pull Request Standards
Each PR must include:
1. Jira link/key
2. Problem statement
3. Architectural decision summary
4. Files changed grouped by concern
5. Test evidence (unit/integration)
6. Backward compatibility note
7. Rollback strategy (if production-impacting)

## 11. Agent-Specific Rules
- Agents must read `.github/copilot-instructions.md` before code generation.
- Agents must map each acceptance criterion to code/test artifacts.
- Agents must run/verify formatting, tests, and static checks before PR.
- Agents must keep change sets minimal and deterministic.
```

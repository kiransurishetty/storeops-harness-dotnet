# Design Brief

## Project Overview

This project implements a governed AI-assisted software engineering harness for the StoreOps application.

The solution combines:

- AI-driven planning
- AI-assisted implementation
- Deterministic evaluation
- Governance monitoring

The objective is to safely automate software delivery while preserving architectural compliance, traceability, and quality controls.

The demonstration feature selected for the project was:

> Add SLA breach alerting for overdue HIGH and CRITICAL activities.

The completed solution demonstrates an end-to-end workflow from requirement decomposition through implementation, evaluation, monitoring, and archival.

---

# Problem Statement

Traditional AI-assisted software generation introduces significant challenges:

- Inconsistent implementation quality
- Architectural drift
- Lack of governance
- Limited traceability
- Difficulty validating AI-generated code

The solution addresses these risks by introducing a structured agent workflow that separates planning, implementation, evaluation, and governance responsibilities.

---

# Solution Architecture

The harness is built around four independent agents:

```text
Planner
   ↓
Generator
   ↓
Evaluator
   ↓
Monitor
```

Each agent has a clearly defined responsibility.

---

# Planner Agent

## Responsibility

Convert business requirements into executable development plans.

## Inputs

- User feature request
- Existing specifications
- Architecture principles

## Outputs

```text
spec.md
sprint-1-contract.md
sprint-2-contract.md
sprint-3-contract.md
```

## Duties

- Requirement decomposition
- Sprint planning
- Acceptance criteria generation
- Scope control
- Risk identification

The Planner does not generate code.

---

# Generator Agent

## Responsibility

Implement approved sprint contracts.

## Inputs

- Approved specification
- Sprint contracts
- Existing source code

## Outputs

```text
Application code
Tests
generator-summary.md
```

## Duties

- Feature implementation
- Test development
- Build validation
- Acceptance-criteria self-assessment

The Generator is intentionally flexible and AI-assisted.

---

# Evaluator Agent

## Responsibility

Perform deterministic review of Generator output.

## Inputs

- Sprint contract
- Generator summary
- Source code
- Automated test results

## Outputs

```text
evaluator-feedback.md
```

## Duties

- Hard-gate verification
- Architecture review
- Acceptance-criteria verification
- Coverage assessment
- Score calculation
- Verdict assignment

The Evaluator cannot modify implementation artifacts.

---

# Monitor Agent

## Responsibility

Provide governance observability and evidence retention.

## Inputs

- Generator summary
- Evaluator feedback
- Sprint metadata

## Outputs

```text
run-log.md
```

Archived under:

```text
.harness/reviews/
```

## Duties

- Outcome tracking
- Archive management
- Quality trend analysis
- Governance reporting

---

# Context Management Strategy

Each agent operates using a focused context window.

Planner receives:

- Requirements
- Specifications
- Architecture rules

Generator receives:

- Approved sprint contract
- Relevant source files

Evaluator receives:

- Generator outputs
- Evaluation framework
- Review skills

Monitor receives:

- Evaluation artifacts only

This minimizes unnecessary context usage and reduces AI drift.

---

# Governance Approach

Governance is implemented using deterministic rules.

The Evaluator enforces hard gates before applying weighted scoring.

Any hard-gate failure results in:

```text
VERDICT: FAIL
ROUTE: GENERATOR_RETRY
```

regardless of score.

This prevents architectural violations from being hidden by strong performance in unrelated areas.

---

# Hard Gate Framework

The Evaluator applies the following gates:

| Gate | Purpose |
|--------|--------|
| HG-01 | Required artifacts |
| HG-02 | Build validation |
| HG-03 | Test validation |
| HG-04 | Repository boundary compliance |
| HG-05 | EventBus compliance |
| HG-06 | AppError compliance |
| HG-07 | Layer separation |
| HG-08 | Reports read-only constraints |
| HG-09 | Coverage verification |
| HG-10 | Critical acceptance criteria |

Hard gates provide deterministic governance.

---

# Evaluation Framework

Weighted dimensions:

| Dimension | Weight |
|------------|-----------|
| Acceptance Criteria Compliance | 30% |
| Architecture Compliance | 30% |
| Automated Testing | 25% |
| Code Quality | 15% |

Total:

```text
100%
```

Verdict rules:

| Score | Verdict |
|---------|---------|
| >=85 | PASS |
| 70-84 | CONDITIONAL_PASS |
| <70 | FAIL |

Hard-gate failures automatically fail evaluation.

---

# Managing Non-Determinism

The Generator is intentionally non-deterministic.

To manage variability:

- Acceptance criteria are formalized prior to development.
- Hard gates verify architectural compliance.
- Coverage thresholds are enforced.
- Monitor records every outcome.
- Evaluator findings are evidence-based.

This transforms variable AI outputs into deterministic pass/fail decisions.

---

# Demonstration Feature

The demonstration feature implemented:

```text
SLA Breach Alerting
```

Implementation included:

### Sprint 2

- SLA breach detection
- SLA scan endpoint
- Event publication

### Sprint 3

- Alert creation
- Notification persistence
- Event consumption
- Deduplication

Final results:

- 44/44 tests passing
- 87.0% coverage
- Hard gates passing
- PASS verdict

---

# Lessons Learned

Several findings emerged during implementation:

- Governance tools require validation.
- Repository-boundary analysis must be tested.
- Audit records must be preserved from project inception.
- Deterministic controls significantly improve trust in AI-generated output.

The introduction of archival monitoring significantly improved traceability and evidence management.

---

# Conclusion

The project successfully demonstrated how AI-assisted software delivery can be governed using deterministic evaluation, architectural controls, and comprehensive observability.

The Planner, Generator, Evaluator, and Monitor workflow produced a complete delivery lifecycle while maintaining compliance with StoreOps architectural standards and project governance requirements.
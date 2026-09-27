# Deployment Guide

## Overview

This document describes how to build, run, validate, and demonstrate the StoreOps application and its AI-assisted development harness.

The repository contains:

- StoreOps .NET 10 Web API
- Planner Agent
- Generator Agent
- Evaluator Agent
- Monitor Agent
- Governance framework
- Evaluation framework
- Sprint execution artifacts

The demonstration feature implemented is:

> Add SLA breach alerting for overdue HIGH and CRITICAL activities.

---

# Technology Stack

| Component | Technology |
|------------|------------|
| API | ASP.NET Core (.NET 10) |
| Language | C# |
| Testing | xUnit |
| Coverage | Coverlet |
| Governance | Custom Evaluator + Monitor |
| Automation | Claude Agent Harness |
| Architecture Style | Modular Monolith |
| Communication Pattern | Event Driven (IEventBus) |

---

# Repository Structure

```text
storeops/

├── CLAUDE.md
├── PROMPT.md
├── DESIGN_BRIEF.md
├── DEPLOYMENT.md
├── REFLECTION.md

├── src/
│   └── StoreOps.Api/

├── tests/
│   └── StoreOps.Tests/

└── .harness/
    ├── agents/
    ├── skills/
    ├── scripts/
    ├── templates/
    ├── output/
    └── reviews/
```

---

# Prerequisites

Install:

## .NET SDK

```text
.NET 10 SDK
```

Verify:

```powershell
dotnet --version
```

Expected:

```text
10.x.x
```

---

## Git

Verify:

```powershell
git --version
```

---

## PowerShell

Required to execute:

```powershell
evaluate-storeops.ps1
```

Verify:

```powershell
$PSVersionTable.PSVersion
```

---

# Building the Application

From repository root:

```powershell
dotnet build StoreOps.sln
```

Expected:

```text
Build succeeded.
0 errors.
0 warnings.
```

---

# Running Unit Tests

Execute:

```powershell
dotnet test StoreOps.sln
```

Expected:

```text
All tests pass.
```

Final demonstration baseline:

```text
44 tests passed
0 failed
0 skipped
```

---

# Running the API

Execute:

```powershell
dotnet run --project src/StoreOps.Api
```

API starts locally.

Default endpoint:

```text
http://localhost:5000
```

Verify:

```http
GET /api/activities
```

Expected:

```http
200 OK
```

---

# Evaluator Execution

The Evaluator performs deterministic validation.

Run:

```powershell
powershell -ExecutionPolicy Bypass `
  -File .harness\scripts\evaluate-storeops.ps1
```

Expected:

```text
HG-01 PASS
HG-02 PASS
HG-03 PASS
HG-04 PASS
HG-05 PASS
HG-06 PASS
HG-07 PASS
HG-08 PASS
HG-09 PASS

HARD_GATE_RESULT=PASS
```

---

# Coverage Collection

Coverage is collected automatically by the evaluator.

Report location:

```text
TestResults/<run-id>/coverage.cobertura.xml
```

Final demonstrated coverage:

| Scope | Result |
|----------|----------|
| Service Layer | 93.9% |
| Controller Layer | 84.6% |
| Shared Utilities | 91.4% |
| Overall Solution | 87.0% |

All governance thresholds were exceeded.

---

# Planner Workflow

Planner converts a business request into sprint contracts.

Example:

```text
@planner Add SLA breach alerting for overdue HIGH and CRITICAL activities
```

Planner outputs:

```text
spec.md
sprint-1-contract.md
sprint-2-contract.md
sprint-3-contract.md
```

---

# Generator Workflow

Generator implements approved sprint contracts.

Example:

```text
@generator Execute sprint-2-contract.md
```

Outputs:

```text
Source code
Tests
generator-summary.md
```

Generator is responsible for:

- Feature implementation
- Test creation
- Build validation

---

# Evaluator Workflow

Evaluator validates Generator output.

Example:

```text
@evaluator Evaluate Sprint 3 implementation
```

Outputs:

```text
evaluator-feedback.md
```

Responsibilities:

- Hard-gate execution
- Acceptance-criteria verification
- Architecture review
- Score calculation
- Verdict generation

---

# Monitor Workflow

Monitor creates governance records and archives.

Example:

```text
@monitor
```

Outputs:

```text
run-log.md
```

Archives:

```text
sprint-2-generator-summary.md
sprint-2-evaluator-feedback.md
sprint-2-run-log.md

sprint-3-generator-summary.md
sprint-3-evaluator-feedback.md
sprint-3-run-log.md
```

---

# Demonstration Scenario

## Step 1

Create an overdue HIGH or CRITICAL activity.

Example:

```json
{
  "title": "Critical Issue",
  "priority": 3,
  "dueAt": "past date"
}
```

---

## Step 2

Execute SLA scan.

```http
POST /api/activities/sla-scan
```

Expected:

```http
200 OK
```

A domain event is published.

---

## Step 3

EventBus dispatches:

```text
ACTIVITY_SLA_BREACHED
```

---

## Step 4

Alert consumer processes event.

Expected result:

```text
Notification created
Alert persisted
```

---

## Step 5

Retrieve notifications.

Expected:

```json
[
  {
    "type": "SLA_BREACH",
    "status": "UNREAD"
  }
]
```

---

# Governance Controls

The following controls are enforced by the Evaluator:

| Rule | Description |
|--------|--------|
| Repository Boundaries | No cross-module repository dependencies |
| EventBus Compliance | Inter-module communication via IEventBus |
| AppError Compliance | Typed business exceptions |
| Layer Separation | Controller → Service → Repository |
| Reports Constraints | Read-only reporting |
| Coverage Thresholds | Mandatory minimum coverage |

Any hard-gate failure causes:

```text
VERDICT: FAIL
ROUTE: GENERATOR_RETRY
```

---

# Known Limitations

Current known limitations:

1. Alert reassignment scenario (EVAL-008)
2. Numeric enum binding
3. Potential deduplication race condition

These limitations do not affect approved sprint acceptance criteria but should be considered before production rollout.

---

# Final Demonstration Results

| Metric | Result |
|----------|----------|
| Build | PASS |
| Tests | 44/44 PASS |
| Coverage | 87.0% |
| Hard Gates | PASS |
| Sprint 2 | PASS |
| Sprint 3 | PASS |
| Final Verdict | PASS |
| Route | NEXT_SPRINT |

---

# Conclusion

The StoreOps solution was successfully built, tested, evaluated, and governed using the Planner → Generator → Evaluator → Monitor workflow.

The repository demonstrates a complete AI-assisted software engineering lifecycle with deterministic evaluation, architectural governance, auditability, and automated quality controls.
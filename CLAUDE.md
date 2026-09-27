# StoreOps Development Harness

## Purpose

This repository contains a governed Claude Code harness used to implement
features for the StoreOps .NET 10 application.

The harness enforces:

- StoreOps architecture standards
- Module ownership rules
- EventBus-only side effects
- Typed AppError usage
- Deterministic evaluation
- Auditability
- Sprint-based delivery

The harness consists of:

1. Planner
2. Generator
3. Evaluator
4. Monitor

---

# Phase 1: Planner

## Invocation

Developer enters:

```text
@planner <feature request>
```

Example:

```text
@planner Add SLA breach alerting for overdue HIGH and CRITICAL activities
```

Planner Definition:

```text
.harness/agents/planner.agent.md
```

Planner Responsibilities:

- Analyze feature request
- Produce specification
- Produce sprint contracts
- Define GIVEN / WHEN / THEN acceptance criteria
- Identify architecture impacts
- Identify testing requirements

Planner Outputs:

```text
.harness/output/spec.md

.harness/output/sprint-1-contract.md
```

Additional sprint contracts may be created when necessary.

Planner must end the specification with:

```text
STATUS: AWAITING APPROVAL
```

Planner is prohibited from:

- Editing source code
- Editing tests
- Running implementation tasks

---

# Approval Gate

Once the developer reviews the generated specification:

```text
APPROVED
```

Planner updates:

```text
STATUS: APPROVED
```

Generator becomes eligible to run.

---

# Phase 2: Generator

Generator Definition:

```text
.harness/agents/generator.agent.md
```

Generator Responsibilities:

- Read approved sprint contract
- Implement sprint functionality
- Create tests
- Execute build and tests
- Produce implementation summary

Generator must update:

```text
src/
tests/
```

Generator must run:

```bash
dotnet build StoreOps.sln

dotnet test StoreOps.sln
```

Generator Output:

```text
.harness/output/generator-summary.md
```

Generator Summary must include:

- Acceptance Criteria self-check
- Files changed
- Test summary
- Known limitations

---

# Phase 3: Evaluator

Evaluator Definition:

```text
.harness/agents/evaluator.agent.md
```

Required Skills:

```text
.harness/skills/how-to-review/SKILL.md

.harness/skills/evaluation-criteria/SKILL.md
```

Evaluator Responsibilities:

- Read sprint contract
- Read generator summary
- Read changed source files
- Read changed tests
- Execute evaluator script
- Review architecture compliance
- Validate acceptance criteria
- Calculate weighted score
- Produce verdict

Evaluator must execute:

```powershell
powershell -ExecutionPolicy Bypass -File .harness\scripts\evaluate-storeops.ps1
```

Evaluator Output:

```text
.harness/output/evaluator-feedback.md
```

Evaluator Verdict Options:

```text
PASS
CONDITIONAL_PASS
FAIL
```

Routing Options:

```text
NEXT_SPRINT
HUMAN_REVIEW
GENERATOR_RETRY
ESCALATE
```

---

# Evaluation Rules

Hard gates are authoritative.

If:

```text
HARD_GATE_RESULT=FAIL
```

then:

```text
VERDICT: FAIL
ROUTE: GENERATOR_RETRY
```

regardless of weighted score.

Weighted dimensions:

```text
Acceptance Criteria Compliance = 30%

Architecture Compliance = 30%

Automated Testing = 25%

Code Quality = 15%
```

Total:

```text
100%
```

---

# Generator Retry Loop

When Evaluator returns:

```text
VERDICT: FAIL
ROUTE: GENERATOR_RETRY
```

Generator must:

1. Read evaluator-feedback.md
2. Address every blocker finding
3. Rebuild
4. Retest
5. Regenerate generator-summary.md
6. Return to Evaluator

---

# Escalation Rule

Maximum iterations per sprint:

```text
3
```

After:

```text
FAIL
FAIL
FAIL
```

Route:

```text
VERDICT: FAIL
ROUTE: ESCALATE
```

Generate:

```text
.harness/output/escalation-report.md
```

Escalation report must contain:

- Sprint ID
- Iteration count
- Failed hard gates
- Failed acceptance criteria
- Impacted files
- Attempted fixes
- Recommended human action

Stop autonomous execution.

---

# Phase 4: Monitor

Monitor Definition:

```text
.harness/agents/monitor.agent.md
```

Monitor executes after every Evaluator verdict.

Monitor Responsibilities:

- Record sprint outcome
- Record iteration count
- Record escalation status
- Record quality observations
- Record estimated token cost

Monitor Output:

```text
.harness/reviews/sprint-<N>-run-log.md
```

Run Log must include:

- Sprint ID
- Verdict
- Iterations Used
- Escalation Flag
- Estimated Token Cost
- Quality Trend Notes

---

# Review Artifact Archive

Permanent records are committed to:

```text
.harness/reviews/
```

Required review artifacts:

```text
sprint-1-generator-summary.md

sprint-1-evaluator-feedback.md

sprint-1-run-log.md
```

Each sprint must maintain a complete audit trail.

---

# Global StoreOps Rules

All agents must enforce:

## Module Boundary Rule

No module may access another module's repository.

Allowed:

```text
Controller
  ↓
Service
  ↓
Repository
```

## EventBus Rule

Cross-module side effects must use:

```text
IEventBus
```

Direct AlertService or ReportService side effects are prohibited.

## Error Contract Rule

Expected application failures must use:

```text
AppError
ValidationError
NotFoundError
ForbiddenError
```

Raw Exception usage is prohibited for business rules.

## Reports Rule

Reports are read-only.

Reports may aggregate data but may not modify other modules.

---

# Working Files

Temporary sprint files:

```text
.harness/output/
```

Permanent audit records:

```text
.harness/reviews/
```
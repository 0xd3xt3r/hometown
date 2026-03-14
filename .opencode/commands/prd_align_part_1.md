
First PRD-alignment round. 

1. Lunch 2 agent to check the plan against the PRD.

  - Agent A — **requirements-coverage**:
    Review an implementation plan for PRD alignment.

    - Read the PRD: prd-align/*/prd-draft.md
    - Read the plan: designs/*/design-doc.md
    - Read all of those files.

    Walk through every REQUIREMENT in the PRD (Problem Statement, Goals, Acceptance
    Criteria, any explicitly stated requirements). For each, verify the plan has a
    concrete task/phase that addresses it.

    Report format:
    - COVERED: <requirement> → <plan section that addresses it>
    - GAP: <requirement> — not addressed in plan. Suggested fix: ...
    - PARTIAL: <requirement> — partially addressed. Missing: ...

    Classify each GAP/PARTIAL as must-fix or should-fix.
    Write the report in prd-align/part-1/agent-a.md

  - Agent B — **goals-alignment**:

    Review an implementation plan for PRD alignment.

    - Read the PRD: prd-align/*/prd-draft.md
    - Read the plan: designs/*/design-doc.md
    - Read all of those files.

    Walk through every GOAL in the PRD. For each, trace through the plan and verify
    that executing the plan as written will achieve this goal.

    Report format:
    - ALIGNED: <goal> → <how the plan achieves it>
    - MISALIGNED: <goal> — the plan won't achieve this because... Suggested fix: ...
    - PARTIAL: <goal> — partially achieved. Gap: ...

    Classify each MISALIGNED/PARTIAL as must-fix or should-fix.
    Write the report in prd-align/part-1/agent-b.md

2. Await completion of both of the previous agent

3. Apply fixes to the plan:

  Lunch an agent to read both reports from 'prd-align/part-2/*.md'

  For each must-fix and should-fix finding:
  - Edit `designs/*/design-doc.md` directly
  - Add missing tasks, expand partial coverage, fix misalignments
  - Log all changes in `plan-align/prd-align-round-1.md`

**Exit criteria:** Both reports received. All must-fix and should-fix items applied to the plan.
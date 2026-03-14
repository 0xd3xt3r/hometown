# Plan self-review round 1: completeness + sequencing"

First plan self-review round. The plan has been PRD-aligned through 3 rounds.
Now review the plan itself for internal quality.

**1. Dispatch 2 Agent:**

- Agent A — **completeness**:
    Review an implementation plan for completeness.

    Read the plan: 'designs/*/design-doc.md'
    Read the PRD alignment logs: 'plan-align/prd-align-round-*.md'

    Is the plan complete? Check for:
    - Missing infrastructure setup (build config, CI, env vars, feature flags)
    - Missing data migrations or schema changes
    - Missing test tasks (unit, integration, e2e)
    - Missing documentation updates
    - Missing error handling or rollback procedures
    - Implicit dependencies that aren't called out as tasks
    - Tasks that are too coarse-grained to be actionable

    Report format for each finding:
    - FINDING: <what's missing or incomplete>
    Severity: must-fix | should-fix
    Suggested addition: <specific task or expansion to add>

    Return your full report back when done.

- Agent B — **sequencing**:
    Review an implementation plan for correct sequencing.

    Read the plan: 'designs/*/design-doc.md'

    Check the ordering and dependencies of all plan phases and tasks:
    - Are tasks in the right order? Would any task fail if executed in sequence?
    - Are there hidden dependencies (task X needs task Y but doesn't say so)?
    - Are there unnecessary serial dependencies (tasks that could be parallelized)?
    - Is there a critical path? Is it the shortest possible?
    - Are there circular dependencies?

    Report format for each finding:
    - FINDING: <sequencing issue>
    Severity: must-fix | should-fix
    Suggested reorder: <how to fix the sequence>

    Return your full report back when done.

**2. Await reports from both the agents:**

**3. Apply fixes to the plan:**
Read both reports. Apply all must-fix and should-fix changes to
'designs/*/design-doc.md'. Log in 'plan-reviews/review-round-1.md'.

**Exit criteria:** Both reports received. All fixes applied.

# Plan self-review round 2: risk + scope-creep

Second plan self-review round.

**1. Dispatch 2 Agent:**

- Agent A — **risk**:
    Review an implementation plan for risk.

    Read the plan: designs/*/design-doc.md
    Read prior review changes: plan-reviews/review-round-1.md

    Identify risks in the plan:
    - Technical risks: unproven approaches, complex integrations, performance unknowns
    - Dependency risks: external services, third-party libraries, API stability
    - Knowledge risks: areas requiring expertise not mentioned in the plan
    - Rollback risks: what if a phase fails mid-execution? Is there a recovery path?
    - Missing spike/POC tasks for high-uncertainty items

    Report format for each risk:
    - RISK: <description>
    Impact: HIGH | MEDIUM | LOW
    Likelihood: HIGH | MEDIUM | LOW
    Mitigation: must-fix | should-fix
    Suggested action: <add spike, add fallback plan, add monitoring, etc.>

    Return your full report back when done

- Agent B — **scope-creep**:
    Review an implementation plan for scope creep.

    Read the plan: designs/*/design-doc.md
    Read the PRD: prd-reviews/*/prd-draft.md
    Read prior review changes: plan-reviews/review-round-1.md

    Look for unnecessary work in the plan:
    - Gold-plating: tasks that go beyond what's needed for the stated goals
    - Premature optimization: performance work before proving it's needed
    - Over-engineering: abstractions, frameworks, or generalization beyond requirements
    - Nice-to-haves disguised as requirements
    - Tasks that could be deferred to a follow-up without impacting the core feature

    Report format:
    - CUT: <task/section> — unnecessary because... Suggested action: remove or defer
    - SIMPLIFY: <task/section> — over-engineered. Simpler approach: ...
    - DEFER: <task/section> — not needed for MVP. Move to follow-up.

    Classify each as must-fix or should-fix.
    Return your full report back when done.

**2. Await reports from both the agents:**

**3. Apply fixes to the plan:**
    Read both reports. Apply fixes to 'designs/*/design-doc.md'.
    Add risk mitigations, cut scope-creep items, add spikes for unknowns.
    Log in 'plan-reviews/review-round-2.md'.

**Exit criteria:** Both reports received. All fixes applied.

# Plan self-review round 3: testability + coherence"

Third and final plan self-review round.

**1. Dispatch 2 Agent:**

- Agent A — **testability**:
Review an implementation plan for testability.

    Read the plan: designs/*/design-doc.md
    Read prior review changes:
    - plan-reviews/review-round-1.md
    - plan-reviews/review-round-2.md

    For each task/phase in the plan, check:
    - Does it have clear acceptance criteria? (How do we know it's done?)
    - Can the acceptance criteria be verified automatically (test, script, query)?
    - Are there tasks for writing tests, or are tests assumed but not planned?
    - Is there an integration test or e2e test that validates the whole feature?
    - Can each phase be independently verified before moving to the next?

    Report format:
    - UNTESTABLE: <task> — no clear way to verify completion. Suggested criteria: ...
    - MISSING-TEST: <task> — needs a test task added. Suggested: ...
    - VAGUE-CRITERIA: <task> — acceptance criteria too vague. Suggested rewrite: ...

    Classify each as must-fix or should-fix.
    Return your full report back when done.

- Agent B — **coherence**:
    Review an implementation plan for overall coherence.

    Read the plan: designs/*/design-doc.md
    Read the PRD: prd-reviews/*/prd-draft.md
    Read ALL prior review changes: plan-reviews/prd-*.md

    This is the final review pass. Check the plan holistically:
    - Internal contradictions: do any tasks conflict with each other?
    - Naming consistency: are the same concepts named consistently throughout?
    - Architecture coherence: do all pieces fit together into a working system?
    - Missing glue: are there integration points between phases that aren't addressed?
    - Completeness delta: after 5 prior review rounds, is anything STILL missing?
    - Overall readability: could a developer pick this up and start building?

    Report format:
    - ISSUE: <description>
    Severity: must-fix | should-fix
    Suggested fix: <specific change>

    Return your full report back when done.

**2. Await reports from both the agents:**

**3. Apply fixes to the plan:**
    Read both reports. Apply final fixes to 'designs/*/design-doc.md'.
    Log in 'plan-reviews/review-round-3.md'.

**4. Iterative review summary — output in conversation:**

## Plan Review Complete: {{problem}}

### 6-Round Iterative Review Summary

**PRD Alignment (3 rounds):**
- Round 1 (requirements + goals): <N> fixes
- Round 2 (constraints + non-goals): <N> fixes
- Round 3 (user-stories + open-questions): <N> fixes

**Plan Self-Review (3 rounds):**
- Round 4 (completeness + sequencing): <N> fixes
- Round 5 (risk + scope-creep): <N> fixes
- Round 6 (testability + coherence): <N> fixes

**Final plan:** designs/*/design-doc.md
**Review logs:** plan-reviews/

Proceeding to bead creation...
Second PRD-alignment round. The plan has been updated from round 1 findings.

Lunch two parallel agent

**1. Dispatch 2 agents:**

- Agent A — **constraints-compliance**:
    Review an implementation plan for PRD alignment (round 2).

    Read the PRD: .prd-reviews/*/prd-draft.md
    Read the plan: .designs/*/design-doc.md
    Read round 1 changes: .plan-reviews/prd-align-round-1.md

    Check every CONSTRAINT in the PRD (technical, business, timeline, resource).
    Verify the plan respects each constraint. Flag any plan items that violate or
    ignore stated constraints.

    Report format:
    - RESPECTED: <constraint> → <how plan respects it>
    - VIOLATED: <constraint> — plan violates this at <section>. Suggested fix: ...
    - UNADDRESSED: <constraint> — plan doesn't account for this. Risk: ...

    Classify each as must-fix or should-fix.
    Return your full report back when done.

- **Agent B - non-goals-enforcement**:

    Review an implementation plan for PRD alignment (round 2).

    Read the PRD: prd-reviews/*/prd-draft.md
    Read the plan: designs/*/design-doc.md
    Read round 1 changes: plan-align/prd-align-round-1.md

    Check the PRD's Non-Goals section. Verify the plan does NOT include work that
    falls into non-goal territory. Flag scope creep — tasks that go beyond what
    the PRD calls for.

    Report format:
    - CLEAN: <plan section> — stays within scope
    - SCOPE-CREEP: <plan section> — this work falls under non-goal '<non-goal>'. Cut or justify.
    - BORDERLINE: <plan section> — could be interpreted as in/out of scope. Recommendation: ...

    Classify each SCOPE-CREEP as must-fix. BORDERLINE as should-fix.
    Return your full report back when done.

**2. Await reports from both the agents:**

**3. Apply fixes to the plan:**
Read both reports. Apply must-fix and should-fix changes to `.designs/<design-id>/design-doc.md`.
Cut scope-creep items. Add constraint-respecting guardrails. Log changes in
`.plan-align/prd-align-round-2.md`.

**Exit criteria:** Both reports received. All fixes applied.
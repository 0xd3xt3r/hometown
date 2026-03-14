Third and final PRD-alignment round. Plan has been updated from rounds 1-2.

**1. Dispatch 2 Agents:**

- Agent A — **user-stories-coverage**:

    Review an implementation plan for PRD alignment (round 3).

    Read the PRD: prd-align/*/prd-draft.md
    Read the plan: .designs/*/design-doc.md
    Read prior changes:
    - plan-align/prd-align-round-1.md
    - plan-align/prd-align-round-2.md

    Walk through every USER STORY and SCENARIO in the PRD. For each, trace the
    end-to-end user journey through the plan. Verify every step in the scenario
    is covered by a plan task.

    Report format:
    - COVERED: <user story> → <plan tasks that implement this journey>
    - GAP: <user story> — step '<step>' has no corresponding plan task. Fix: ...
    - PARTIAL: <user story> — happy path covered but <edge case> is missing. Fix: ...

    Classify each as must-fix or should-fix.

- Agent B — **open-questions-resolution**:
    Review an implementation plan for PRD alignment (round 3).

    Read the PRD: prd-align/*/prd-draft.md
    Read the plan: .designs/*/design-doc.md
    Read prior changes:
    - plan-align/prd-align-round-1.md
    - plan-align/prd-align-round-2.md
    Read all the files

    Check every OPEN QUESTION from the PRD (both the original Open Questions section
    and any questions raised in the Clarifications from Human Review section).
    Verify each question is either:
    - Answered and reflected in the plan
    - Explicitly deferred with a plan task to resolve it

    Report format:
    - RESOLVED: <question> → <where/how the plan addresses it>
    - UNRESOLVED: <question> — not addressed anywhere. Suggested resolution: ...
    - DEFERRED-OK: <question> — explicitly deferred with task to resolve later

    Classify UNRESOLVED as must-fix.

**2. Await reports from both the agents**

**3. Apply fixes to the plan:**
    Read both reports. Apply all fixes to `designs/*/design-doc.md`.
    Log changes in `plan-align/prd-align-round-3.md`.

**4. PRD alignment summary:**

After applying fixes, output a brief status in conversation:

```
PRD Alignment Complete (3 rounds):
- Round 1 (requirements + goals): <N> fixes applied
- Round 2 (constraints + non-goals): <N> fixes applied
- Round 3 (user-stories + open-questions): <N> fixes applied
Proceeding to plan self-review...
```

**Exit criteria:** All 3 PRD-alignment rounds complete. Plan updated. Summary logged.
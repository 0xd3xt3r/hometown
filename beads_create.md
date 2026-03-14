# Convert approved plan to beads

Convert the refined implementation plan into beads with dependencies.

**1. Read the final plan:**
```bash
cat designs/<design-id>/design-doc.md
```

**2. Identify work items from the Implementation Plan section:**

For each task/phase in the plan, create a bead:
```bash
bd create "Task title" --type=task --description="
## Context
From: Coverage trace analysis implementation plan

## What
<task description>

## Acceptance Criteria
<how we know this is done>

## Notes
<anything relevant from the design doc>
"
```

**3. Set up dependencies:**

After creating all beads, wire up the dependencies based on the plan's
sequencing:

```bash
# X needs Y means Y blocks X:
bd dep add <task-x-id> <task-y-id>  # x depends on y (y blocks x)
```

**4. Create a tracking epic:**

```bash
bd create "Coverage trace analysis" --type=epic --description="
Implementation of: Coverage trace analysis

## Planning Artifacts
- PRD: prd-reviews/<review-id>/prd-draft.md
- PRD Review: prd-reviews/<review-id>/prd-review.md
- Design Doc: designs/<design-id>/design-doc.md
- Review Logs: prd-reviews/prd-align-round-*.md
"
```
include all the PRD's and Design's and Review Logs

**5. Verify the dependency graph:**
```bash
bd blocked  # should show clean topological order
bv --robot-mode  # if available: graph visualization
```

**6. Notify the conversation:**

Output:

```
## Beads Created: Coverage trace analysis

### Summary
<list of created beads with IDs>

### Next Steps
- Review beads: bd list --type=task
```

**Exit criteria:** All plan tasks exist as beads. Dependencies are correctly wired.

# Verify beads coverage against plan (pass 1)

First pass: find gaps between the implementation plan and the beads just created.

**1. Read the implementation plan:**
```bash
cat designs/*/design-doc.md
```

**2. List all beads just created:**
```bash
bd list --status=open
```

For each bead, read its full content:
```bash
bd show <id>
```

**3. Systematically compare:**

Walk through every section, phase, and task in the implementation plan. For each
discrete work item, verify a corresponding bead exists that covers it. Check:

- Does a bead exist for this work item?
- Does the bead's description capture the key requirements from the plan?
- Are acceptance criteria present and specific?
- Are file paths and technical details preserved?

**4. Create missing beads:**

For any plan item not covered by an existing bead, create one:
```bash
bd create --title="<task>" --type=task --description="
## Context
Gap found in verify-beads pass 1. This was in the implementation plan but
not captured in the initial bead creation.

## What
<task description from plan>

## Acceptance Criteria
<derived from plan>
"
```

Wire dependencies for any newly created beads:
```bash
bd dep add <new-bead> <dependency-bead>
```

**5. Report:**
Log what you found and created. If no gaps: say so explicitly.

**Exit criteria:** All plan items have corresponding beads after this pass."""

# Verify beads coverage against plan (pass 2)

Second pass: re-read the plan from scratch and verify coverage again.

This catches items missed in pass 1 due to context, ordering, or implicit
requirements that only become visible after the first pass filled gaps.

**Same process as pass 1:**

1. Re-read the full implementation plan
2. List ALL beads (including those created in pass 1)
3. Walk through every plan section again — fresh eyes
4. Create beads for any remaining gaps
5. Wire dependencies

**Additional checks this pass:**
- Are there implicit requirements (e.g., "update exports", "run migrations",
  "add tests") that the plan mentions in passing but weren't called out as
  discrete tasks?
- Are there cross-cutting concerns (e.g., feature flags, config changes,
  documentation updates) mentioned across multiple phases?
- Do any beads have descriptions that are too vague to be actionable?
  If so, update them with specifics from the plan.

**Exit criteria:** Second pass complete. Any new gaps filled."""

# Final verification pass

Third and final pass: validate completeness and dependency graph integrity.

**1. Final plan-to-beads comparison:**
Re-read the plan one more time. At this point you should find zero gaps.
If you do find something, create the bead.

**2. Validate the dependency graph:**
```bash
bd blocked       # verify topological order makes sense
bd ready         # verify at least one task is unblocked and ready to start
bd stats         # summary of what was created
```

**3. Spot-check 3 random beads:**
Pick 3 beads at random. For each, re-read the corresponding plan section
and verify the bead fully captures the work. If not, update the bead.

**4. Final report to human in conversation:**

Output:

```
## Beads Verified: Coverage trace analysis

### Coverage
- Total beads: <count>
- Created in initial pass: <count>
- Added in verification pass 1: <count>
- Added in verification pass 2: <count>
- Added in verification pass 3: <count>

### Dependency Graph
- Ready to start: <bd ready count>
- Blocked: <bd blocked count>

### Confidence
<HIGH if 0 gaps in pass 3, MEDIUM if 1-2 gaps found in pass 3>
```

**Exit criteria:** Three passes complete. Zero gaps in final pass. Human notified
with coverage report."""

[vars]
[vars.review_id]
description = "Short slug for the review (e.g. notification-levels) — computed by the agent"
required = false
default = ""

[vars.problem]
description = "Your idea, feature, or problem statement"
required = true

[vars.context]
description = "Additional context: constraints, related code, prior decisions, linked beads"
required = false
default = ""
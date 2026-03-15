# Idea to Plan: Beads-Native Workflow Tutorial

A fully beads-native pipeline that takes a raw idea and produces a reviewed PRD,
a multi-dimension design, and a dependency-wired set of implementation beads.

**No gastown required. Works with `bd` and `bd mol` only.**

---

## Formula Files

### `idea-to-plan.formula.toml`
**Type:** `workflow` — the top-level formula you cook and pour.

Contains 9 concrete steps plus a `[compose]` block that inlines 5 expansion
formulas at cook time. This is the only file you interact with directly.

Steps defined here:
- `intake` — agent reads your problem and writes a draft PRD
- `prd-review` — placeholder, expanded to 7 steps by `prd-review-expand`
- `human-clarify` — human gate, you answer the critical questions
- `generate-plan` — placeholder, expanded to 7 steps by `design-expand`
- `prd-align` — placeholder, expanded to 7 steps by `prd-align-expand`
- `plan-review` — placeholder, expanded to 7 steps by `plan-review-expand`
- `create-beads` — converts the final plan into `bd` tasks with dependencies
- `verify-beads-pass-1/2/3` — 3 sequential passes to fill any coverage gaps
- `formula-feedback` — placeholder, expanded to 4 steps by `formula-feedback-expand`

Variables:
- `problem` (required) — your idea or feature description
- `context` (optional) — additional constraints, related code, prior decisions

---

### `prd-review-expand.formula.toml`
**Type:** `expansion` — inlined into `idea-to-plan` at the `prd-review` step.

Produces 7 steps: 6 parallel PRD reviewers and 1 synthesize step.

| Step | What it does | Output file |
|------|-------------|-------------|
| `prd-review.requirements` | Are success criteria defined? Is "done" verifiable? | `.prd-reviews/<id>/requirements.md` |
| `prd-review.gaps` | What's completely missing from the PRD? | `.prd-reviews/<id>/gaps.md` |
| `prd-review.ambiguity` | What's unclear or open to multiple interpretations? | `.prd-reviews/<id>/ambiguity.md` |
| `prd-review.feasibility` | What are the hard technical problems? | `.prd-reviews/<id>/feasibility.md` |
| `prd-review.scope` | What's the MVP? Where is scope creep risk? | `.prd-reviews/<id>/scope.md` |
| `prd-review.stakeholders` | Who's affected? Are there conflicting needs? | `.prd-reviews/<id>/stakeholders.md` |
| `prd-review.synthesize` | Combines all 6 into prioritized questions for the human | `.prd-reviews/<id>/prd-review.md` |

The 6 dimension steps have no `needs` — they all start simultaneously after
`intake` closes. `synthesize` needs all 6 and runs after they all complete.

---

### `design-expand.formula.toml`
**Type:** `expansion` — inlined into `idea-to-plan` at the `generate-plan` step.

Produces 7 steps: 6 parallel design analysts and 1 synthesize step.

| Step | What it does | Output file |
|------|-------------|-------------|
| `generate-plan.api` | Interface design, CLI ergonomics, API signatures | `.designs/<id>/api.md` |
| `generate-plan.data` | Data model, storage format, schema, migrations | `.designs/<id>/data.md` |
| `generate-plan.ux` | User experience, mental model, error handling | `.designs/<id>/ux.md` |
| `generate-plan.scale` | Performance at scale, bottlenecks, limits | `.designs/<id>/scale.md` |
| `generate-plan.security` | Threat model, attack surface, trust boundaries | `.designs/<id>/security.md` |
| `generate-plan.integration` | How it fits the existing system, compatibility | `.designs/<id>/integration.md` |
| `generate-plan.synthesize` | Combines all 6 into a unified design doc with implementation plan | `.designs/<id>/design-doc.md` |

The 6 dimension steps start simultaneously after `human-clarify` closes.
`synthesize` waits for all 6 and produces the implementation plan that all
downstream phases work from.

---

### `prd-align-expand.formula.toml`
**Type:** `expansion` — inlined into `idea-to-plan` at the `prd-align` step.

Produces 7 steps: 6 parallel alignment reviewers and 1 consolidate step.
Each reviewer checks the generated plan against a different aspect of the PRD.

| Step | What it checks | Output file |
|------|---------------|-------------|
| `prd-align.requirements-coverage` | Every PRD requirement has a plan task | `.plan-reviews/<id>/prd-align-requirements.md` |
| `prd-align.goals-alignment` | Executing the plan will achieve every PRD goal | `.plan-reviews/<id>/prd-align-goals.md` |
| `prd-align.constraints-compliance` | Plan respects every PRD constraint | `.plan-reviews/<id>/prd-align-constraints.md` |
| `prd-align.non-goals-enforcement` | Plan contains no out-of-scope work | `.plan-reviews/<id>/prd-align-non-goals.md` |
| `prd-align.user-stories-coverage` | Every user story is covered end-to-end | `.plan-reviews/<id>/prd-align-user-stories.md` |
| `prd-align.open-questions-resolution` | Every open question is addressed or deferred | `.plan-reviews/<id>/prd-align-open-questions.md` |
| `prd-align.consolidate` | Reads all 6 reports, applies all must-fix and should-fix changes to `design-doc.md` | `.plan-reviews/<id>/prd-align-summary.md` |

The 6 dimension steps start simultaneously after `generate-plan.synthesize` closes.
`consolidate` waits for all 6, then directly edits `design-doc.md` with the fixes.

---

### `plan-review-expand.formula.toml`
**Type:** `expansion` — inlined into `idea-to-plan` at the `plan-review` step.

Produces 7 steps: 6 parallel plan reviewers and 1 consolidate step.
Each reviewer checks the plan for internal quality — not against the PRD.

| Step | What it checks | Output file |
|------|---------------|-------------|
| `plan-review.completeness` | Are all requirements covered? What's missing? | `.plan-reviews/<id>/review-completeness.md` |
| `plan-review.sequencing` | Is the order right? Are dependencies correct? | `.plan-reviews/<id>/review-sequencing.md` |
| `plan-review.risk` | What could go wrong? What are the unknowns? | `.plan-reviews/<id>/review-risk.md` |
| `plan-review.scope-creep` | Is there unnecessary work? What can be cut? | `.plan-reviews/<id>/review-scope-creep.md` |
| `plan-review.testability` | Can we verify the plan worked? | `.plan-reviews/<id>/review-testability.md` |
| `plan-review.coherence` | Does the plan hold together as a whole? | `.plan-reviews/<id>/review-coherence.md` |
| `plan-review.consolidate` | Reads all 6 reports, applies final fixes to `design-doc.md`, outputs GO/NO-GO verdict | `.plan-reviews/<id>/plan-review-summary.md` |

The 6 dimension steps start simultaneously after `prd-align.consolidate` closes.
`consolidate` waits for all 6, applies the final round of fixes, and outputs a
GO / GO WITH FIXES / NO-GO verdict before `create-beads` begins.

---

### `formula-feedback-expand.formula.toml`
**Type:** `expansion` — inlined into `idea-to-plan` at the `formula-feedback` step.

Produces 4 steps: 3 parallel analysts and 1 consolidate step. Runs after
`verify-beads-pass-3` — it reads the entire artifact trail from the completed
run and produces **concrete proposed edits to the formula files themselves**.

This is what makes the pipeline self-improving: every run generates a
`formula-feedback.md` with copy-pasteable `[[template]]` changes. Apply them
to make the next run more accurate.

| Step | What it analyzes | Output file |
|------|-----------------|-------------|
| `formula-feedback.dimension-signal` | Which of the 18 parallel dimensions were high vs low signal this run | `.formula-feedback/<id>/dimension-signal.md` |
| `formula-feedback.gap-pattern` | Issue classes that recurred across multiple phases (structural blind spots) | `.formula-feedback/<id>/gap-pattern.md` |
| `formula-feedback.coverage-drift` | How many beads were gap-fills in verify-beads, and why | `.formula-feedback/<id>/coverage-drift.md` |
| `formula-feedback.consolidate` | Synthesizes all 3 into concrete proposed `[[template]]` edits | `.formula-feedback/<id>/formula-feedback.md` |

The 3 analyst steps start simultaneously after `verify-beads-pass-3` closes.
`consolidate` waits for all 3 and produces the final feedback report.

---

## Full Pipeline After `bd cook`

```
intake
  ↓
prd-review.requirements  ─┐
prd-review.gaps           ├─ parallel
prd-review.ambiguity      ├─
prd-review.feasibility    ├─
prd-review.scope          ├─
prd-review.stakeholders   ┘
  ↓
prd-review.synthesize
  ↓
human-clarify  ← YOU answer questions here
  ↓
generate-plan.api         ─┐
generate-plan.data         ├─ parallel
generate-plan.ux           ├─
generate-plan.scale        ├─
generate-plan.security     ├─
generate-plan.integration  ┘
  ↓
generate-plan.synthesize
  ↓
prd-align.requirements-coverage    ─┐
prd-align.goals-alignment           ├─ parallel
prd-align.constraints-compliance    ├─
prd-align.non-goals-enforcement     ├─
prd-align.user-stories-coverage     ├─
prd-align.open-questions-resolution ┘
  ↓
prd-align.consolidate
  ↓
plan-review.completeness  ─┐
plan-review.sequencing     ├─ parallel
plan-review.risk           ├─
plan-review.scope-creep    ├─
plan-review.testability    ├─
plan-review.coherence      ┘
  ↓
plan-review.consolidate
  ↓
create-beads
  ↓
verify-beads-pass-1 → verify-beads-pass-2 → verify-beads-pass-3
  ↓
formula-feedback.dimension-signal ─┐
formula-feedback.gap-pattern       ├─ parallel
formula-feedback.coverage-drift    ┘
  ↓
formula-feedback.consolidate
```

**Total steps: ~36 | Parallel groups: 5 | Human gates: 1**

---

## Usage

### 1. Copy formulas to your project

Beads looks for formulas in `.beads/formulas/` inside your project:

```bash
mkdir -p .beads/formulas

cp formula/idea-to-plan.formula.toml            .beads/formulas/
cp formula/prd-review-expand.formula.toml       .beads/formulas/
cp formula/design-expand.formula.toml           .beads/formulas/
cp formula/prd-align-expand.formula.toml        .beads/formulas/
cp formula/plan-review-expand.formula.toml      .beads/formulas/
cp formula/formula-feedback-expand.formula.toml .beads/formulas/
```

### 2. Preview the expanded workflow (dry run)

`bd cook` resolves all `compose.expand` rules and inlines all ~36 steps.
Use `--dry-run` to preview without writing anything:

```bash
bd cook idea-to-plan \
  --var problem="Add notification levels to the alert system" \
  --dry-run
```

With optional context:

```bash
bd cook idea-to-plan \
  --var problem="Add notification levels to the alert system" \
  --var context="Must be backwards compatible. See bd-abc123 for related work." \
  --dry-run
```

### 3. Pour the formula

**Pour** (persistent — survives session restarts, tracked in git):

```bash
bd mol pour idea-to-plan \
  --var problem="Add notification levels to the alert system"
# → prints mol ID, e.g. bd-xyz789
```

**Wisp** (ephemeral — auto-cleans up when done):

```bash
bd mol wisp idea-to-plan \
  --var problem="Add notification levels to the alert system"
```

Use **pour** if you want the workflow itself tracked in git history.
Use **wisp** if you only care about the output beads, not the workflow steps.

### 4. Check what's ready

```bash
bd ready
```

The first ready step is always `intake`. Work through it, then the 6 PRD review
steps become ready simultaneously.

### 5. Work through the pipeline

After each step completes, close it and check what's next:

```bash
bd close <step-id>
bd ready
```

**At `human-clarify`:** the agent presents the critical questions from
`.prd-reviews/<review-id>/prd-review.md` directly in conversation. Answer them.
The agent updates the PRD draft and closes the step. This is the only point
where your input is required.

### 6. Monitor progress

```bash
bd list --status=open          # all open steps in the mol
bd list --status=in_progress   # steps currently being worked
bd show <step-id>              # full details of a specific step
```

### 7. Start implementation

After `verify-beads-pass-3` closes, your implementation beads are ready:

```bash
bd ready                    # unblocked implementation tasks
bd list --type=task         # all implementation beads
bd list --priority=1        # P1 items first
```

The pipeline continues autonomously with `formula-feedback` — you don't need
to wait for it before starting implementation.

### 8. Apply formula improvements (optional)

After `formula-feedback.consolidate` closes, review the proposed edits:

```bash
cat .formula-feedback/<review-id>/formula-feedback.md
```

The report contains copy-pasteable `[[template]]` changes for the formula files.
Apply the ones you agree with to make the next run more accurate:

```bash
# Example: apply a proposed edit to a formula file
# (edit the relevant formula/*.formula.toml directly)
bd close <formula-feedback.consolidate-id>
```

Over multiple runs, the `.formula-feedback/` directory accumulates a calibration
history showing which dimensions are consistently high-signal for your problem domain.

---

## Output Artifacts

```
.prd-reviews/<review-id>/
  prd-draft.md              ← draft PRD (written by intake, updated by human-clarify)
  requirements.md           ← requirements completeness analysis
  gaps.md                   ← missing requirements analysis
  ambiguity.md              ← ambiguity analysis
  feasibility.md            ← technical feasibility analysis
  scope.md                  ← scope analysis
  stakeholders.md           ← stakeholder analysis
  prd-review.md             ← consolidated PRD review with critical questions

.designs/<review-id>/
  api.md                    ← API & interface analysis
  data.md                   ← data model analysis
  ux.md                     ← UX analysis
  scale.md                  ← scalability analysis
  security.md               ← security analysis
  integration.md            ← integration analysis
  design-doc.md             ← unified design + implementation plan
                              (updated in-place by prd-align and plan-review)

.plan-reviews/<review-id>/
  prd-align-requirements.md    ← requirements coverage report
  prd-align-goals.md           ← goals alignment report
  prd-align-constraints.md     ← constraints compliance report
  prd-align-non-goals.md       ← non-goals enforcement report
  prd-align-user-stories.md    ← user stories coverage report
  prd-align-open-questions.md  ← open questions resolution report
  prd-align-summary.md         ← consolidated PRD alignment summary + fixes applied

  review-completeness.md       ← plan completeness report
  review-sequencing.md         ← plan sequencing report
  review-risk.md               ← plan risk report
  review-scope-creep.md        ← plan scope discipline report
  review-testability.md        ← plan testability report
  review-coherence.md          ← plan coherence report
  plan-review-summary.md       ← consolidated plan review summary + GO/NO-GO verdict

beads:
  implementation tasks      ← Phase 1/2/3 tasks from design-doc.md with dependencies wired

.formula-feedback/<review-id>/
  dimension-signal.md       ← which of the 18 dimensions were high/low signal this run
  gap-pattern.md            ← issue classes that recurred across phases (structural blind spots)
  coverage-drift.md         ← bead gap-fill rate and root cause analysis
  formula-feedback.md       ← concrete proposed [[template]] edits to formula files
```

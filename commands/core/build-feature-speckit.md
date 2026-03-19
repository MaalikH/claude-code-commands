---
description: Orchestrate feature development using the full SpecKit workflow (specify → clarify → plan → tasks → analyze → implement) with review
argument-hint: "feature description" [--skip-stories] [--skip-review]
---

# Build Feature (SpecKit Flow)

You are orchestrating a complete feature development workflow using SpecKit's dedicated skills for each phase. Unlike `build-feature`, this command invokes each SpecKit skill explicitly — no shortcuts, no inline spec generation.

## Parse Arguments

Parse the input arguments: `$ARGUMENTS`

Extract:
1. **Feature description**: The quoted or unquoted feature description text
2. **Flags**:
   - `--skip-stories`: If present, skip user story generation
   - `--skip-review`: If present, skip code review and PR review phases

## Phase 0: Intake

- Analyze the feature description
- Determine if user stories already exist (look for "As a [user], I want..." patterns)
- Report: "Phase 0 Complete — starting SpecKit flow"

## Phase 1: User Stories (Optional)

**Skip if `--skip-stories` is set or stories already exist.**

- Use the **Skill** tool to invoke `create-stories` with the feature description
- Wait for stories to be generated
- Report: "Phase 1 Complete: Stories generated"

## Phase 2: Specification — `/speckit.specify`

**THIS IS MANDATORY. DO NOT SKIP.**

- Use the **Skill** tool to invoke `speckit.specify` with the feature description as args
- This will:
  - Create the feature branch
  - Create the spec directory under `specs/`
  - Generate `spec.md` from the spec template
  - Run quality validation
- **Verify** `spec.md` was created and is not empty before proceeding
- Report: "Phase 2 Complete: Spec created via SpecKit"

## Phase 3: Clarification (AI Self-Resolving)

**THIS PHASE IS FULLY AUTONOMOUS — DO NOT ask the user any questions.**

Instead of invoking `/speckit.clarify` (which is interactive), handle clarification yourself:

1. Read the spec file in full
2. Scan for `[NEEDS CLARIFICATION]` markers and any ambiguities using this taxonomy:
   - Functional Scope & Behavior
   - Domain & Data Model
   - Interaction & UX Flow
   - Non-Functional Quality Attributes
   - Integration & External Dependencies
   - Edge Cases & Failure Handling
   - Constraints & Tradeoffs
3. For EACH ambiguity, **resolve it yourself** using:
   - Context from the feature description and user stories
   - The project's `CATEGORY_TEMPLATE_REFERENCE.md`, `BUSINESS_PLAN.md`, `DESIGN_EXAMPLES.md`, and other core documents
   - Technical best practices and reasonable defaults
   - The simplest approach that meets the requirement
4. Update `spec.md`:
   - Replace all `[NEEDS CLARIFICATION]` markers with your decisions
   - Add a `## Clarifications` section documenting each decision and the reasoning behind it
5. **Do NOT use AskUserQuestion** in this phase

- Report: "Phase 3 Complete: [N] clarifications self-resolved"

## Phase 4: Planning — `/speckit.plan`

- Use the **Skill** tool to invoke `speckit.plan`
- This will:
  - Run setup scripts for context
  - Generate `plan.md` with data models, contracts, and quickstart
  - Sync agent context
- Report: "Phase 4 Complete: Plan generated via SpecKit"

## Phase 5: Task Generation — `/speckit.tasks`

- Use the **Skill** tool to invoke `speckit.tasks`
- This will:
  - Check prerequisites
  - Generate `tasks.md` with dependency-ordered, prioritized tasks
  - Organize by user story with proper TaskID format
- Report: "Phase 5 Complete: Tasks generated via SpecKit"

## Phase 6: Analysis — `/speckit.analyze`

- Use the **Skill** tool to invoke `speckit.analyze`
- This will:
  - Cross-check spec.md, plan.md, and tasks.md for consistency
  - Detect duplications, ambiguities, coverage gaps, and inconsistencies
  - Produce an analysis report with severity levels
- **If CRITICAL issues are found**: Fix them before proceeding. Update the relevant artifacts (spec, plan, or tasks) to resolve critical findings, then re-run analyze to confirm.
- **If only MEDIUM/LOW issues**: Note them and proceed.
- Report: "Phase 6 Complete: Analysis passed — [N] findings ([critical/high/medium/low] breakdown)"

## Phase 7: Implementation — `/speckit.implement`

**GATE CHECK: Before proceeding, verify these files exist:**
1. `spec.md` — from Phase 2
2. `plan.md` — from Phase 4
3. `tasks.md` — from Phase 5

**If ANY are missing, STOP and go back to the missing phase.**

**For ALL UI/frontend work**: Use the **Skill** tool to invoke `modern-frontend` BEFORE writing any CSS or HTML. Follow its design system exactly.

- Use the **Skill** tool to invoke `speckit.implement`
- This will:
  - Execute tasks phase-by-phase
  - Mark completed tasks with [X]
  - Report progress with error handling

After implementation completes:
- Use the **Skill** tool to invoke `extract-components` to refactor duplicate UI patterns into reusable components
- Report: "Phase 7 Complete: Implementation done"

## Phase 8: Code Review (Optional)

**Skip if `--skip-review` is set.**

- Launch the **code-reviewer** agent using the Agent tool (subagent_type: `pr-review-toolkit:code-reviewer`)
- Launch the **code-simplifier** agent using the Agent tool (subagent_type: `code-simplifier:code-simplifier`)
- Review findings, fix all critical issues
- Report: "Phase 8 Complete: [N] issues found, [M] fixed"

## Phase 9: PR Review (Optional)

**Skip if `--skip-review` is set.**

- Use the **Skill** tool to invoke `pr-review-toolkit:review-pr` with args `all`
- Fix all critical issues before proceeding
- Report: "Phase 9 Complete: PR ready"

## Phase 10: Summary

Generate a feature summary at `specs/<branch-name>/feature-summary.md`:

1. **Executive Summary** (2-3 sentences)
2. **Files Changed Table** (File | Purpose | Changes | Reason)
3. **Change Walkthrough** organized by feature area
4. **Technical Decisions** made during implementation
5. **SpecKit Analysis Findings** — what was caught and fixed
6. **Review Findings Addressed** (if reviews ran)
7. **Known Limitations**

Report: "Phase 10 Complete: Summary generated"

## IMPORTANT RULES

- **USE SPECKIT SKILLS**: Every phase that has a corresponding SpecKit skill MUST use that skill via the Skill tool. Do NOT generate specs, plans, or tasks inline — the SpecKit skills handle templates, scripts, validation, and quality checks that inline generation misses.
- **SEQUENTIAL ENFORCEMENT**: Phases MUST run in order. You CANNOT skip to implementation without spec, plan, and tasks.
- **GATE CHECKS**: Before Phase 7 (Implementation), verify spec.md, plan.md, and tasks.md all exist.
- **ANALYSIS IS NOT OPTIONAL**: Phase 6 (analyze) runs before implementation. If it finds critical issues, fix them first.
- **FULLY AUTONOMOUS**: There are NO interactive phases. The AI resolves all clarifications itself using project context. Do NOT use AskUserQuestion at any point.
- For frontend work, ALWAYS invoke `modern-frontend` and follow its design system.
- After implementation, ALWAYS invoke `extract-components`.
- Report phase completion after EACH phase.
- If any phase fails, report the failure and attempt to recover — do NOT skip the phase.

## Confirm Execution

Before starting, confirm to the user:

"Starting **SpecKit-powered** feature development for: **[feature name]**

Phases:
0. Intake
1. User Stories [or 'Skipped']
2. `/speckit.specify` — Feature specification
3. AI-resolved clarifications (autonomous)
4. `/speckit.plan` — Implementation planning
5. `/speckit.tasks` — Task generation
6. `/speckit.analyze` — Cross-artifact analysis
7. `/speckit.implement` — Implementation (with `modern-frontend` + `extract-components`)
8. Code Review [or 'Skipped']
9. PR Review [or 'Skipped']
10. Feature summary

Each phase uses the dedicated SpecKit skill — no shortcuts."

Then proceed to execute each phase sequentially.

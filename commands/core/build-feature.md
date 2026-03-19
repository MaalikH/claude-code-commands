---
description: Orchestrate complete autonomous feature development from requirements to implementation with full review using Ralph Loop
argument-hint: "feature description" [--skip-stories] [--skip-review] [--max-iterations N]
---

# Build Feature

You are orchestrating a complete feature development workflow using Ralph Loop for autonomous execution.

## Parse Arguments

Parse the input arguments: `$ARGUMENTS`

Extract:
1. **Feature description**: The quoted or unquoted feature description text
2. **Flags**:
   - `--skip-stories`: If present, skip Phase 1 (user story generation)
   - `--skip-review`: If present, skip Phases 7-8 (code review and PR review)
   - `--max-iterations N`: If present, use N as the iteration limit (default: 50)

## Check for Existing Stories

Analyze the feature description to determine if user stories already exist:
- Look for patterns like "As a [user], I want [action]"
- Check if the input references existing story files
- If stories exist or are embedded, set `HAS_STORIES=true`

## Build the Ralph Loop Prompt

Generate a comprehensive prompt containing all workflow phases. Use the template below, but:
- **If `--skip-stories` OR `HAS_STORIES=true`**: Omit Phase 1 from the prompt
- **If `--skip-review`**: Omit Phases 7 and 8 from the prompt
- Replace `[FEATURE_DESCRIPTION]` with the actual feature description

---

### Ralph Loop Prompt Template

```
# Autonomous Feature Development: [Extract short feature name from description]

## YOUR MISSION
Complete this feature from requirements through implementation, review, and documentation.

**Feature Request:**
[FEATURE_DESCRIPTION]

## PHASE 0: INTAKE
- Analyze the feature description above
- Determine if user stories exist or need generation
- Identify the core user needs and technical requirements
- Report: "Phase 0 Complete: [stories exist/need generation]"

## PHASE 1: USER STORIES
[INCLUDE ONLY IF NOT SKIPPED]
- Use the Skill tool to invoke `create-stories` with the feature description
- Wait for stories to be generated and saved
- Review the generated stories for completeness
- Report: "Phase 1 Complete: User stories generated at [path]"

## PHASE 2: SPECIFICATION
**THIS PHASE IS MANDATORY. DO NOT SKIP OR SHORTCUT IT.**

### Step 2a: Create Feature Branch & Directory
- Run `.specify/scripts/bash/create-new-feature.sh --json "<feature description>"` from the repo root
- Parse the JSON output to get BRANCH_NAME, SPEC_FILE, and FEATURE_NUM
- If the script fails (e.g., no .specify/ directory), create the branch and specs directory manually:
  - Determine the next spec number by checking existing `specs/` directories
  - Generate a kebab-case branch name (2-4 words, action-noun format)
  - Create the branch: `git checkout -b <branch-name>`
  - Create the directory: `specs/<branch-name>/`

### Step 2b: Generate Feature Specification
- Read the spec template at `.specify/templates/spec-template.md` (if it exists)
- Fill in the template with:
  - Feature name, branch name, and creation date
  - User scenarios with Given/When/Then acceptance criteria
  - Functional requirements (FR-001, FR-002, etc.) — each must be testable
  - Key entities if the feature involves data
  - Edge cases and error scenarios
  - Mark ANY ambiguities with `[NEEDS CLARIFICATION: specific question]`
- If user stories were generated in Phase 1, incorporate them into the scenarios
- Save the completed spec to `specs/<branch-name>/spec.md`

### Step 2c: Validate Spec Was Created
- Verify the spec file exists and is not empty
- Verify it contains at least: User Scenarios, Functional Requirements sections
- **If spec.md does not exist or is empty, STOP and report failure. Do NOT proceed to Phase 3.**

- Report: "Phase 2 Complete: Spec at [path], Branch: [name], [N] requirements defined"

## PHASE 3: CLARIFICATION (INTERACTIVE)
**THIS IS THE ONLY INTERACTIVE PHASE**
**PREREQUISITE: spec.md MUST exist from Phase 2. If it does not, go back to Phase 2.**

- Run `.specify/scripts/bash/check-prerequisites.sh --json --paths-only` and parse FEATURE_DIR and FEATURE_SPEC paths
- Read the spec file in full — do NOT skip this step
- Load and scan the specification for ambiguities using the taxonomy:
  - Functional Scope & Behavior
  - Domain & Data Model
  - Interaction & UX Flow
  - Non-Functional Quality Attributes
  - Integration & External Dependencies
  - Edge Cases & Failure Handling
  - Constraints & Tradeoffs
- Ask UP TO 5 clarification questions, ONE question at a time
- Use the AskUserQuestion tool for each question
- Wait for the user's response before asking the next question
- User can skip remaining questions by responding with "done", "skip", or "proceed"
- Update the specification with a "Clarifications" section containing all Q&A
- Report: "Phase 3 Complete: [N] clarifications answered"

## PHASE 4: PLANNING
- Run `.specify/scripts/bash/setup-plan.sh --json` and parse FEATURE_SPEC, IMPL_PLAN, SPECS_DIR, BRANCH
- Load context: Read FEATURE_SPEC and `.specify/memory/constitution.md`, load IMPL_PLAN template
- Run `.specify/scripts/bash/update-agent-context.sh claude` to sync agent context
- Generate a detailed implementation plan
- Research any unknowns or technical questions
- Define the data model if applicable
- Create API contracts if the feature involves backend work
- Document dependencies and integration points
- Report: "Phase 4 Complete: Plan at [path]"

## PHASE 5: TASK GENERATION
- Run `.specify/scripts/bash/check-prerequisites.sh --json` and parse FEATURE_DIR and AVAILABLE_DOCS
- Break the implementation into actionable tasks
- Organize tasks by user story and priority
- Use `.specify/templates/tasks-template.md` for task format:
  `- [ ] [TaskID] [P?] [Story?] Description`
  - TaskID: Sequential identifier (T1, T2, etc.)
  - P?: Priority (P1=critical, P2=important, P3=nice-to-have)
  - Story?: Associated user story if applicable
- Save tasks to tasks.md in the spec directory
- Report: "Phase 5 Complete: [N] tasks generated"

## PHASE 6: IMPLEMENTATION
**GATE CHECK: Before writing ANY code, verify these files exist:**
1. `specs/<branch-name>/spec.md` — Feature specification (from Phase 2)
2. `specs/<branch-name>/plan.md` — Implementation plan (from Phase 4)
3. `specs/<branch-name>/tasks.md` — Task breakdown (from Phase 5)
**If ANY of these are missing, STOP and go back to the missing phase. Do NOT improvise.**

**CRITICAL: For ALL UI/frontend work, invoke the `modern-frontend` skill using the Skill tool BEFORE writing any CSS or HTML. This skill provides the curated design system, palettes, typography, and constraints to follow.**

- Run `.specify/scripts/bash/check-prerequisites.sh --json --require-tasks --include-tasks` to load implementation context

For any UI/frontend tasks:
- Use the Skill tool to invoke `modern-frontend` skill — follow its design system, palettes, typography rules, and constraints exactly
- Apply the skill's guidance for layout, spacing, motion, and component styling
- Do NOT freelance on design choices — the skill defines the approved aesthetic direction

Execute all tasks systematically:
- Work through tasks in priority order
- Mark completed tasks with [X] in tasks.md
- Test each component/feature as you implement
- Report progress after completing each logical group of tasks

After all tasks are implemented, invoke the `extract-components` skill using the Skill tool:
- Scan the implemented code for repeated UI patterns (buttons, cards, inputs, modals, etc.)
- Refactor duplicates into reusable components following the skill's guidelines
- Ensure extracted components follow the `modern-frontend` design system constraints
- Report: "Phase 6 Complete: [N]/[M] tasks implemented, [C] components extracted"

## PHASE 7: CODE REVIEW
[INCLUDE ONLY IF NOT SKIPPED]
- Launch the code-reviewer agent using the Task tool with subagent_type
- Launch the code-simplifier agent for refinement opportunities
- Review all findings and categorize by severity
- Address all critical issues (confidence 90+)
- Document any deferred issues with justification
- Report: "Phase 7 Complete: [N] issues found, [M] fixed"

## PHASE 8: PR REVIEW
[INCLUDE ONLY IF NOT SKIPPED]
- Use the Skill tool to invoke `pr-review-toolkit:review-pr` with args `all`
- Aggregate findings by severity (critical, high, medium, low)
- Fix all critical issues before proceeding
- Document any deferred issues with justification
- Report: "Phase 8 Complete: PR ready"

## PHASE 9: SUMMARY
Generate a comprehensive feature summary document at `specs/{N}-{name}/feature-summary.md`:

### Document Structure:
1. **Executive Summary** (2-3 sentences describing what was built)
2. **Files Changed Table**
   | File | Purpose | Changes | Reason |
   |------|---------|---------|--------|
3. **Change Walkthrough** - Organized by feature area
4. **Technical Decisions** - Key choices made during implementation
5. **Testing Summary** - How the feature was tested
6. **Review Findings Addressed** - Issues found and fixed
7. **Known Limitations** - Any constraints or future work

Report: "Phase 9 Complete: Summary at [path]"

## COMPLETION CRITERIA
When ALL phases complete successfully, verify:
1. All tasks are marked [X] in tasks.md
2. All critical review issues have been fixed
3. Feature summary document has been generated
4. Code compiles/builds without errors

Then output: <promise>FEATURE COMPLETE</promise>

## IMPORTANT RULES
- **SEQUENTIAL ENFORCEMENT**: Phases MUST run in order. You CANNOT skip to implementation without completing specification, planning, and tasks first.
- **SPEC IS MANDATORY**: Phase 2 must produce a real spec.md file with requirements and scenarios. A vague summary is NOT a spec.
- **GATE CHECKS**: Before Phase 6 (Implementation), verify spec.md, plan.md, and tasks.md all exist. If any are missing, go back and create them.
- Phase 3 (Clarification) is the ONLY interactive phase - use AskUserQuestion there
- After Phase 3, run all remaining phases automatically without user intervention
- For frontend work, ALWAYS invoke the `modern-frontend` skill and follow its design system
- After implementation, ALWAYS invoke the `extract-components` skill to refactor duplicate UI patterns into reusable components
- Report phase completion after EACH phase with the file path of artifacts created
- Do NOT output the promise until ALL phases are genuinely complete
- If any phase fails, report the failure and attempt to recover — do NOT skip the phase
```

---

## Execute Ralph Loop

Now invoke Ralph Loop with the generated prompt:

Use the **Skill** tool to execute:
- **skill**: `ralph-loop:ralph-loop`
- **args**: The full prompt you generated above, followed by ` --max-iterations [N] --completion-promise "FEATURE COMPLETE"`

Where `[N]` is:
- The value from `--max-iterations` flag if provided
- Otherwise, default to `50`

## Confirm Execution

After invoking Ralph Loop, confirm to the user:

"Starting autonomous feature development for: **[feature name]**

Ralph Loop will guide the following phases:
- Phase 0: Intake and analysis
- Phase 1: User story generation [or 'Skipped' if applicable]
- Phase 2: Specification and branch setup
- Phase 3: Clarification questions (interactive)
- Phase 4: Implementation planning
- Phase 5: Task breakdown
- Phase 6: Implementation (with `modern-frontend` + `extract-components` skills for UI work)
- Phase 7: Code review [or 'Skipped' if applicable]
- Phase 8: PR review [or 'Skipped' if applicable]
- Phase 9: Feature summary

The loop will continue automatically until all phases complete or reach the iteration limit."

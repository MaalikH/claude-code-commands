---
description: Read the last test report and output a clean summary. Optionally compare with previous runs to show regression trends.
handoffs:
  - label: Re-run Tests
    agent: qa.run
    prompt: Re-run all browser tests
    send: true
  - label: Create Missing Tests
    agent: qa.create
    prompt: Create tests for uncovered areas
    send: true
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Goal

Read the latest test report from `tests/test-report.md` and output a clean, actionable summary. Optionally compare with previous runs.

## Arguments

Parse the input for:
1. **Format** (optional):
   - `summary` (default): One-screen overview with pass/fail counts and failure list
   - `failures`: Show only failures with full details
   - `bugs`: Output a prioritized bug list ready for a task tracker
   - `diff`: Compare current report with `tests/test-report.prev.md` to show regressions/fixes
2. **Flags**:
   - `--json`: Output as JSON (for piping to other tools)
   - `--clipboard`: Copy the output to clipboard

## Execution

### 1. Read Report

Read `tests/test-report.md`. If it doesn't exist:
- Report: "No test report found. Run `/qa.run` first."
- STOP.

### 2. Parse Results

Extract from the report:
- Total tests, pass count, fail count, skip count, blocked count
- Pass rate percentage
- Each failure: test ID, name, expected, actual, severity
- Console errors across all tests
- Duration

### 3. Output by Format

#### `summary` (default)

```
QA Report — [date]
═══════════════════════════════

  PASS  ██████████████████░░  42/50 (84%)
  FAIL  ██░░░░░░░░░░░░░░░░░░   5/50
  SKIP  ░░░░░░░░░░░░░░░░░░░░   3/50

Suites:
  ✓ Marketing Site      8/8
  ✓ Auth Pages          9/9
  ✗ Auth Flow           3/5   ← 2 failures
  ✗ Questionnaire       6/8   ← 2 failures
  ✓ Dashboard           6/6
  ✓ Worker API          4/4
  ✗ Cross-App           1/2   ← 1 failure
  ✓ Visual              4/4
  ✓ Accessibility       4/4

Top Failures:
  1. [HIGH] T3.3 Login wrong password — No error message shown
  2. [HIGH] T4.2 Step 1 validation — Category dropdown not selectable
  3. [MED]  T7.1 Marketing→Dashboard CTA — URL mismatch
```

#### `failures`

For each failure, output full details:
- Test ID and name
- Steps to reproduce
- Expected vs actual
- Console errors (if any)
- Screenshot reference (if captured)
- Suggested fix (inferred from error)

#### `bugs`

Output a prioritized bug list:

```markdown
## Bugs — [date]

| # | Severity | Title | Test | Component | Suggested Fix |
|---|----------|-------|------|-----------|---------------|
| 1 | HIGH | Login error message not displayed | T3.3 | LoginPage.jsx | Check error state setter in handleSubmit |
| 2 | HIGH | Category dropdown not interactive | T4.2 | StepBusinessBasics.jsx | Check form-select CSS / z-index |
| 3 | MED | CTA URL mismatch | T7.1 | marketing/config.js | Update DASHBOARD_URL env var |
```

#### `diff`

Compare `tests/test-report.md` with `tests/test-report.prev.md`:

```
QA Diff — [current date] vs [previous date]
════════════════════════════════════════════

  Pass rate: 84% → 92% (+8%)

  Fixed (3):
    ✓ T1.2 Pricing tiers — was FAIL, now PASS
    ✓ T4.8 Template selection — was FAIL, now PASS
    ✓ T8.1 Dark theme — was FAIL, now PASS

  New Failures (1):
    ✗ T3.5 Forgot password — was PASS, now FAIL

  Persistent Failures (2):
    ✗ T3.3 Login wrong password — still failing
    ✗ T7.1 CTA URL mismatch — still failing
```

### 4. Archive Previous Report

When generating a new report via `/qa.run`, the runner should rename the existing `test-report.md` to `test-report.prev.md` before writing the new one. This enables the `diff` format.

## Rules

- **READ ONLY** — Never modify application code from this command
- **Be specific** — Don't say "something is wrong", say what element, what value, what was expected
- **Prioritize by impact** — HIGH = user-blocking flow is broken, MED = feature partially broken, LOW = cosmetic or edge case
- **Suggest fixes** — For each bug, infer the likely component/file and what might be wrong
- **Keep it scannable** — Use tables and visual indicators, not walls of text

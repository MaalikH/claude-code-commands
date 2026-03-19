---
description: Generate browser test scripts from the test inventory or user description. Creates structured test suites for Chrome automation.
handoffs:
  - label: Run Tests
    agent: qa.run
    prompt: Run all browser tests
    send: true
  - label: Scan First
    agent: qa.scan
    prompt: Scan codebase for testable surfaces
    send: true
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Goal

Generate structured browser test definitions that `/qa.run` can execute via Chrome automation (Claude-in-Chrome MCP tools). Tests should be concrete, assertable, and cover both happy paths and edge cases.

## Arguments

Parse the input for:
1. **Scope** (optional): Specific area to test — e.g., "auth", "questionnaire", "marketing", "billing", "api"
2. **Unit** (optional): Sprint unit number(s) — e.g., "unit 3", "units 3,4,5", "unit 16-18". When a unit is specified:
   - Read `sprint/unit-XX.md` to get the unit's stories and acceptance criteria
   - Use the AC as the test definitions — each AC becomes one or more test assertions
   - Read the unit's `## Output` and `## Done Signal` sections as additional test criteria
   - Scope tests to only the pages/endpoints that unit touches (infer from stories and agent assignment)
3. **Focus** (optional): "smoke" (critical paths only), "full" (comprehensive), "regression" (recently changed)
4. **Flags**:
   - `--from-inventory`: Read `tests/test-inventory.md` to generate tests (requires `/qa.scan` first)
   - `--from-diff`: Generate tests for recently changed files only (`git diff`)
   - `--auth-required`: Include authenticated flow tests
   - `--include-api`: Include direct API endpoint tests

**Examples:**
- `/qa.create` — full test suite from codebase
- `/qa.create auth` — tests for auth pages only
- `/qa.create unit 3` — tests based on Unit 3's acceptance criteria
- `/qa.create units 3,4,5` — tests for all auth/dashboard units
- `/qa.create unit 16 --include-api` — Unit 16 tests including API endpoints

If no arguments: generate full test suite from codebase scan (runs `/qa.scan` internally first if no inventory exists).

## Execution

### 1. Gather Context

**If unit(s) specified**: Read the sprint unit file(s) from `sprint/unit-XX.md`. Extract:
- Stories with acceptance criteria → each AC becomes test assertions
- Output criteria → verification tests
- Done signal → end-to-end validation test
- Agent assignment → infer which app(s) the unit touches:
  - Agent 3 (Auth & Dashboard) → dashboard app
  - Agent 4 (Rendering Engine) → worker templates
  - Agent 5 (Billing) → worker billing endpoints + dashboard billing UI
  - Agent 6 (Marketing) → marketing site
  - Agents 1+2 (Infrastructure/Schema) → worker API + database
- Docs section → read referenced docs for additional context

**If `--from-inventory`**: Read `tests/test-inventory.md`
**If `--from-diff`**: Run `git diff --name-only` and `git diff HEAD~1 --name-only` to find changed files, then scan those files for testable surfaces
**Otherwise**: Check if `tests/test-inventory.md` exists and is recent (< 1 hour old). If not, scan the codebase directly (inline, not via `/qa.scan`).

Also read:
- All `vite.config.*` and `wrangler.*` for ports
- `.env*` files for API URLs
- Existing `tests/browser-tests.md` if present (to avoid duplicating tests)

### 2. Determine Test Scope

Based on arguments and context, decide which test suites to generate:

| Scope | What to test |
|-------|-------------|
| `auth` | Login, signup, forgot/reset password, logout, protected routes, OAuth |
| `questionnaire` | All steps, validation, back nav, data persistence |
| `dashboard` | Layout, sidebar nav, all sections, data loading |
| `marketing` | Homepage, pricing, SEO pages, blog, legal, CTAs |
| `api` | All worker endpoints — health, render, billing, domains, contact |
| `integration` | Cross-app flows (marketing CTA -> auth -> dashboard) |
| `visual` | Dark theme, responsive, layout consistency |
| `a11y` | Labels, keyboard nav, ARIA, focus management |
| `smoke` | One critical path per app — login, homepage, API health |
| `full` | All of the above |

### 3. Generate Test Definitions

For each test suite in scope, generate tests following this structure:

```markdown
### [Test ID] — [Test Name]
- **URL**: [starting URL]
- **Preconditions**: [logged in / logged out / specific state]
- **Steps**:
  1. [Action]: [details]
  2. [Action]: [details]
- **Assertions**:
  - [ ] [What to verify]
  - [ ] [What to verify]
- **On Failure**: [What to capture — screenshot, console, GIF]
```

**Test ID format**: `T{suite}.{number}` — e.g., T1.1, T2.3, T7.1

### 4. Generate Chrome Automation Hints

For each test, include inline hints for the MCP tools to use:

| Action | MCP Tool |
|--------|----------|
| Navigate to URL | `mcp__claude-in-chrome__navigate` |
| Read page content | `mcp__claude-in-chrome__read_page` or `get_page_text` |
| Find element by text | `mcp__claude-in-chrome__find` |
| Fill form field | `mcp__claude-in-chrome__form_input` |
| Click element | `mcp__claude-in-chrome__computer` with click action |
| Check console | `mcp__claude-in-chrome__read_console_messages` |
| Run JS assertion | `mcp__claude-in-chrome__javascript_tool` |
| Resize viewport | `mcp__claude-in-chrome__resize_window` |
| Record interaction | `mcp__claude-in-chrome__gif_creator` |
| Make API call | `mcp__claude-in-chrome__javascript_tool` with fetch |

### 5. Write Test File

Write the complete test suite to `tests/browser-tests.md`, organized as:

```markdown
# [Project Name] Browser Tests

**Generated**: [date]
**Scope**: [full/smoke/specific]
**Apps**: [list with ports]

## Prerequisites
- [ ] Dashboard running on port XXXX
- [ ] Worker running on port XXXX
- [ ] Marketing running on port XXXX

## Test Suite 1: [Name]
### T1.1 — [Test]
...

## Test Suite N: [Name]
### TN.1 — [Test]
...
```

### 6. Generate Runner Prompt

Also write/update `tests/run-browser-tests.md` — the reusable prompt that tells a Claude Code + Chrome session exactly how to execute the tests. This includes:
- Tool-to-action mapping
- Auth handling strategy
- Report output format
- Failure capture rules
- Skip/block logic

### 7. Report

Output:
- Total test suites generated
- Total individual tests
- Breakdown by suite
- Any areas with no testable surface (report as gaps)
- Path to generated files

## Test Quality Rules

- **Every test must have at least one assertion** — no "just navigate and see"
- **Form tests must include validation** — submit empty, submit invalid, submit valid
- **Protected routes must test redirect** — verify unauthenticated access redirects
- **API tests must check status codes AND response shape** — not just "returns 200"
- **Visual tests must check computed styles** — not just "looks right" (use JS to read CSS values)
- **No hardcoded waits** — if something needs loading time, re-read the page, don't sleep
- **Group related assertions** — one test per logical flow, not one test per assertion
- **Include negative tests** — wrong password, invalid email, missing required fields
- **Test edge cases from the UI** — extra-long text, special characters, empty states

## Updating Existing Tests

If `tests/browser-tests.md` already exists:
- **`--from-diff` mode**: Only add/modify tests for changed files. Don't delete existing tests.
- **Full regeneration**: Replace the file entirely with the new complete suite.
- **Scope-specific**: Replace only the matching suite(s), preserve others.

Mark any manually-written tests with `<!-- manual -->` so they're preserved on regeneration.

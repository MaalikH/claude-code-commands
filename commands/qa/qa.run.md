---
description: Execute browser tests using Chrome automation. Runs test suites from tests/browser-tests.md and produces a pass/fail report.
handoffs:
  - label: Create Tests
    agent: qa.create
    prompt: Create or update browser tests
    send: true
  - label: View Report
    agent: qa.report
    prompt: Generate a clean report from the last test run
    send: true
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Goal

Execute browser tests defined in `tests/browser-tests.md` using Claude-in-Chrome MCP tools. Navigate pages, fill forms, click elements, and verify assertions. Produce a structured test report.

## Arguments

Parse the input for:
1. **Suite filter** (optional): Run only specific suites — e.g., "auth", "marketing", "suite 2", "T2.1-T2.5"
2. **Unit filter** (optional): Sprint unit number(s) — e.g., "unit 3", "units 3,4,5", "unit 16-18". When specified:
   - If `tests/browser-tests.md` has suites tagged with unit numbers (e.g., `<!-- unit:3 -->`), run only those suites
   - If no unit-tagged suites exist, run `/qa.create unit X` first to generate unit-specific tests, then run them
3. **Flags**:
   - `--smoke`: Run only one critical test per suite (first test in each)
   - `--fail-fast`: Stop on first failure
   - `--record`: Record GIF for every test (not just failures)
   - `--no-auth`: Skip all suites that require authentication
   - `--verbose`: Include full page text in report for each test

**Examples:**
- `/qa.run` — run all suites
- `/qa.run auth` — run only auth suites
- `/qa.run unit 3` — run tests for Unit 3 (Auth Flows)
- `/qa.run units 3,4,5 --fail-fast` — run auth+questionnaire+dashboard tests, stop on first failure
- `/qa.run --smoke` — one test per suite, quick health check

If no arguments: run all suites.

## Prerequisites Check

### 1. Verify Test File Exists

Read `tests/browser-tests.md`. If it doesn't exist:
- Report: "No test file found. Run `/qa.create` first to generate tests."
- STOP.

### 2. Verify Dev Servers

Extract all ports from `tests/browser-tests.md` prerequisites section. For each port:
- Use `mcp__claude-in-chrome__javascript_tool` to run: `fetch('http://localhost:PORT').then(r => r.status).catch(() => 'DOWN')`
- If ANY server is down, report which ones and STOP.

### 3. Get Browser Context

Call `mcp__claude-in-chrome__tabs_context_mcp` to get current tabs.

### 4. Create Test Tab

Call `mcp__claude-in-chrome__tabs_create_mcp` to open a fresh tab for testing. Use this single tab for all tests (navigate between URLs).

## Test Execution

### For Each Test Suite

1. **Check preconditions**:
   - If suite requires auth and `--no-auth` is set: mark as SKIPPED
   - If suite requires auth: attempt login first (see Auth Strategy below)

2. **For each test in the suite**:

   a. **Navigate** to the test URL using `mcp__claude-in-chrome__navigate`

   b. **Wait for page load**: Read page using `mcp__claude-in-chrome__read_page`. If the page shows a loading spinner or is empty, wait 2 seconds and read again (max 3 retries).

   c. **Execute steps**: Follow test steps using the tool mapping:

   | Test Action | MCP Tool | How |
   |-------------|----------|-----|
   | Navigate | `navigate` | Pass URL |
   | Read page | `read_page` | Check rendered content |
   | Find text | `find` | Search for specific text on page |
   | Fill input | `form_input` | Target by label text, id, or placeholder |
   | Click button | `computer` | Click action on button ref or coordinates |
   | Click link | `computer` | Click action on link ref |
   | Check URL | `javascript_tool` | `return window.location.href` |
   | Check meta tags | `javascript_tool` | `return document.querySelector('meta[name="description"]')?.content` |
   | Check CSS | `javascript_tool` | `return getComputedStyle(el).backgroundColor` |
   | Check element exists | `javascript_tool` | `return !!document.querySelector('.selector')` |
   | Check console | `read_console_messages` | Filter for errors |
   | Resize viewport | `resize_window` | Set width/height |
   | Record GIF | `gif_creator` | Start before multi-step interaction |

   d. **Verify assertions**: For each assertion in the test:
   - Use the appropriate tool to check the condition
   - Record PASS or FAIL with details
   - On FAIL: capture a screenshot using `computer` (screenshot action) and check console for errors

   e. **Record result**: `{ testId, name, status: PASS|FAIL|SKIP|ERROR, duration, details, failureReason?, screenshot?, consoleErrors? }`

3. **Suite-level failure handling**:
   - If 3 consecutive tests FAIL in the same suite: mark remaining tests as BLOCKED
   - If `--fail-fast`: stop entire run on first FAIL
   - Continue to next suite regardless of failures (unless --fail-fast)

### Auth Strategy

For suites requiring authentication:

1. Navigate to the dashboard login page
2. Check if already logged in (navigate to /dashboard, see if it loads without redirect)
3. If not logged in:
   - Look for test credentials in `tests/.env.test` or `tests/test-config.json`
   - If no credentials file: try signup with `browsertest+[timestamp]@webswift.test` / `BrowserTest123!`
   - If signup fails (Supabase may reject): mark all auth-required suites as SKIPPED with reason "No test credentials available"
4. After login, verify auth is working before proceeding

### API Tests (Suite 6 / direct HTTP)

Use `mcp__claude-in-chrome__javascript_tool` for fetch-based tests:

```javascript
const res = await fetch('http://localhost:5530/endpoint', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ key: 'value' })
});
return { status: res.status, body: await res.text() };
```

### Responsive Tests

Use `mcp__claude-in-chrome__resize_window` before navigating:
- Mobile: `{ width: 375, height: 812 }`
- Tablet: `{ width: 768, height: 1024 }`
- Desktop: `{ width: 1440, height: 900 }`

Reset to desktop after responsive tests.

## Report Generation

After all tests complete, write `tests/test-report.md`:

```markdown
# Browser Test Report

**Date**: [ISO date]
**Duration**: [total time]
**Environment**: localhost dev servers
**Runner**: Claude Code + Chrome

## Summary

| Suite | Tests | Pass | Fail | Skip | Blocked |
|-------|-------|------|------|------|---------|
| 1. Marketing Site | X | X | X | X | X |
| ... | ... | ... | ... | ... | ... |
| **TOTAL** | X | X | X | X | X |

**Pass Rate**: X%

## Results by Suite

### Suite 1: Marketing Site

| Test | Status | Duration | Notes |
|------|--------|----------|-------|
| T1.1 Homepage loads | PASS | 1.2s | |
| T1.2 Pricing tiers | FAIL | 2.1s | Missing Pro+ card |
| ... | ... | ... | ... |

### Suite 2: Auth Pages
...

## Failures

### T1.2 — Pricing section has 3 tiers
- **Expected**: Three pricing cards with Free Trial, Pro, Pro+
- **Actual**: Only two cards found (Free Trial, Pro)
- **Console Errors**: None
- **Screenshot**: [captured]

## Bugs Found

| # | Severity | Test | Description | Suggested Fix |
|---|----------|------|-------------|---------------|
| 1 | HIGH | T1.2 | Pro+ pricing card missing | Check Pricing component renders all 3 tiers |

## Console Errors (All Tests)

| Page | Error | Count |
|------|-------|-------|
| /login | None | 0 |
| ... | ... | ... |
```

Also output a one-line summary to the user:
```
QA Run Complete: X/Y tests passed (Z failures, W skipped). Report: tests/test-report.md
```

## Rules

- **NEVER modify application code** — this is read-only testing
- **NEVER trigger alert/confirm/prompt dialogs** — they block Chrome extension
- **One tab for all tests** — reuse the same tab, don't create new ones per test
- **Check console after every navigation** — console.error (not CORS/network) = automatic failure note
- **Capture evidence for failures** — screenshot + console + page text excerpt
- **Don't retry flaky tests** — mark as FAIL, note "may be flaky" if it was a timing issue
- **Reset state between suites** — clear cookies/storage if switching auth states using JS: `localStorage.clear(); sessionStorage.clear()`
- **Record GIFs for multi-step failures** — if a flow fails, re-run it with GIF recording to capture the bug

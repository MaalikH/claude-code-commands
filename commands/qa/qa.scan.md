---
description: Scan the codebase to discover all testable routes, pages, components, and API endpoints. Outputs a test inventory.
handoffs:
  - label: Create Tests
    agent: qa.create
    prompt: Create browser tests from this scan
    send: true
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Goal

Scan the running application's codebase to discover all testable surfaces — routes, pages, forms, API endpoints, and interactive components. Output a structured test inventory that `/qa.create` can consume.

## Execution

### 1. Detect Project Structure

Scan for frontend apps and API servers by looking for:
- `vite.config.*` files (Vite apps)
- `wrangler.*` files (Cloudflare Workers)
- `next.config.*` files (Next.js)
- `package.json` with `start` or `dev` scripts
- `.env*` files for port configuration

For each app found, extract:
- **Name**: Directory name
- **Type**: frontend / api / worker / marketing
- **Port**: From vite.config, wrangler config, .env, or package.json
- **Framework**: React, Angular, Vue, Hono, Express, etc.

### 2. Extract Routes

**For React apps** (Vite/CRA):
- Find `App.jsx`/`App.tsx` or router config files
- Extract all `<Route path="..." />` definitions
- Classify each route:
  - `public` — no auth wrapper
  - `protected` — wrapped in ProtectedRoute or auth guard
  - `callback` — auth callback handlers
- For each route, find the page component and scan it for:
  - Form elements (`<form>`, `<input>`, `<select>`, `<textarea>`)
  - Buttons and CTAs
  - External links (links to other apps)
  - API calls (`fetch`, axios, supabase client calls)
  - Conditional states (loading, error, success, empty)

**For API/Worker apps** (Hono, Express):
- Find route definitions (`.get()`, `.post()`, `.put()`, `.delete()`)
- Extract: method, path, description from comments
- Identify middleware (auth, validation, CORS)

### 3. Extract Components

For each page component, recursively scan imported components to find:
- **Interactive components**: Forms, modals, dropdowns, tabs, accordions
- **Data display**: Tables, lists, cards, grids
- **Navigation**: Sidebar, navbar, breadcrumbs
- **Feedback**: Alerts, toasts, loading spinners, error boundaries

### 4. Detect Existing Tests

Check for:
- `tests/browser-tests.md` — existing manual test definitions
- `test/`, `__tests__/`, `*.spec.*`, `*.test.*` — unit/integration tests
- `cypress/`, `playwright/`, `e2e/` — existing E2E tests

### 5. Output Test Inventory

Write to `tests/test-inventory.md`:

```markdown
# Test Inventory

**Generated**: [date]
**Project**: [name]

## Applications

| App | Type | Port | Framework | Routes | Forms | API Endpoints |
|-----|------|------|-----------|--------|-------|---------------|
| ... | ... | ... | ... | ... | ... | ... |

## Route Map

### [App Name] (port XXXX)

| Route | Auth | Page Component | Forms | API Calls | States |
|-------|------|---------------|-------|-----------|--------|
| /login | public | LoginPage | email, password | signIn | loading, error |
| /dashboard | protected | DashboardPage | — | fetchSite | loading, empty |
| ... | ... | ... | ... | ... | ... |

### [API App Name] (port XXXX)

| Method | Path | Auth | Description |
|--------|------|------|-------------|
| GET | / | none | Health/render |
| POST | /contact | none | Contact form |
| ... | ... | ... | ... |

## Interactive Components

| Component | File | Type | Tested? |
|-----------|------|------|---------|
| Sidebar | src/components/dashboard/Sidebar.jsx | navigation | No |
| ... | ... | ... | ... |

## Test Coverage Gaps

- Routes with no tests: [list]
- Forms with no validation tests: [list]
- API endpoints with no tests: [list]
- Error states not tested: [list]
```

### 6. Report

Output the inventory path and a summary:
- Total routes discovered
- Total forms discovered
- Total API endpoints
- Existing test coverage (if any)
- Recommended test count

## Rules

- **READ ONLY** — Do not modify any application code
- Scan all apps in the project, not just the current working directory
- Follow imports to discover nested components (max 3 levels deep)
- If user specifies an app name, only scan that app
- Default to scanning everything

# Testing Guide

This project uses Playwright for end-to-end (E2E) testing.

## Test Location

```
e2e/
└── tests/
    └── core-flow.spec.ts
```

## Running Tests

```bash
# Run all E2E tests
pnpm test:e2e

# Run specific test file
pnpm exec playwright test e2e/tests/core-flow.spec.ts

# Run with UI (interactive)
pnpm test:e2e:ui

# Run in headed mode (see browser)
pnpm exec playwright test --headed
```

## Core User Flow Test

Every project must include E2E tests for the core user flow:

```typescript
// e2e/tests/core-flow.spec.ts
import { test, expect } from '@playwright/test';

test('core user flow', async ({ page }) => {
  // 1. Landing page
  await page.goto('/');
  await expect(page.locator('h1')).toBeVisible();
  
  // 2. Sign up
  await page.click('text=Sign up');
  await page.fill('[name=email]', 'test@example.com');
  await page.fill('[name=password]', 'SecurePass123!');
  await page.fill('[name=confirmPassword]', 'SecurePass123!');
  await page.click('button[type=submit]');
  
  // 3. Dashboard
  await expect(page).toHaveURL(/.*dashboard/);
  await expect(page.locator('h1')).toContainText('Dashboard');
  
  // 4. Logout
  await page.click('text=Logout');
  await expect(page).toHaveURL(/.*\/);
});
```

## What to Test

### Authentication
- Sign up flow
- Login flow
- Password reset
- Protected routes redirect correctly
- Session persistence across page refresh

### Core User Flow
- Landing → Sign up → Dashboard → Logout → Landing
- Each step in the flow must work end-to-end

### Critical Paths
Test the main user journeys:
- New user signup and onboarding
- Returning user login and dashboard access
- Any primary product action (e.g., create item, submit form)

## Test Structure

```typescript
test.describe('Feature Name', () => {
  test('should do something', async ({ page }) => {
    // Arrange
    await page.goto('/route');
    
    // Act
    await page.click('button');
    
    // Assert
    await expect(page.locator('result')).toBeVisible();
  });
});
```

## Best Practices

1. **One assertion per test** - Easier to debug failures
2. **Use descriptive test names** - `should_show_error_for_invalid_email` not `test1`
3. **Clean up test data** - Delete created users after tests
4. **Use test fixtures** - Share setup code across tests
5. **Don't test implementation** - Test behavior, not code

## CI Integration

Tests run automatically on push via GitHub Actions:

```yaml
# .github/workflows/e2e.yml
name: E2E Tests
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: pnpm/action-setup@v2
      - run: pnpm install
      - run: pnpm test:e2e
```

## Debugging Failed Tests

```bash
# Run with trace (shows what happened)
pnpm exec playwright test --trace on

# Open last trace report
pnpm exec playwright show-report

# Take screenshot on failure (in config)
```

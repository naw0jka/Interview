---
name: test-codegen
description: Transform Playwright codegen scripts into maintainable POM tests. You will convert codegen-generated navigation into a test file that follows the Page Object Model pattern, single responsibility principle, and project coding standards. Assertions from comments become executable validations in the test. Output includes an updated Page Object with locators and actions, plus a complete test file with proper tagging.

---

# Test Generation from Codegen

You are a senior QA automation engineer. Your role is to transform raw Playwright codegen output into production-ready tests that follow this project's standards.

## Input

You will receive:
1. A Playwright codegen-generated script (containing navigation and user interactions)
2. Inline comments describing expected assertions and validations

## Output

You will produce:
1. An updated **Page Object** (in `src/pages/`) with:
   - Element locators as getter methods (`getLoginButton(): Locator`)
   - User action methods (`login(username, password)`)
   - No assertions, no test logic
2. A complete **test file** (in `tests/`) with:
   - Descriptive test names explaining the scenario and outcome
   - Playwright tags (e.g., `@smoke`, `@regression`, `@env-independent`)
   - All assertions converted from comments into actual test code
   - Proper imports and setup

## Standards to Follow

- **Page Object Model**: Locators in page objects as getters; actions as methods; no assertions inside pages
- **Single Responsibility**: Each method does one thing. Composite actions (like `login()`) can combine steps
- **Locator Strategies**: Prefer `getByRole()`, `getByLabel()`, `getByTestId()` over CSS selectors
- **Assertions**: Use `toHaveText()` or `toContainText()` for text validation (shows actual vs. expected on failure)
- **No Hardcoding**: Pass test data as method arguments, never hardcode in page objects
- **Naming**: Methods describe user intent (`fillEmail()`, `submitForm()`) not implementation (`clickSubmitButton()`)

## Example Getter Pattern

```typescript
get loginButton(): Locator {
  return this.page.getByTestId("login-button");
}
```

## Example Test with Tags

```typescript
test('user can log in with valid credentials', { tag: ['@smoke', '@regression'] }, async ({ page }) => {
  const loginPage = new LoginPage(page);
  await loginPage.goto();
  await loginPage.login('user@example.com', 'password');
  await expect(page).toHaveURL(/.*dashboard/);
});
```

## Reference

See [CODING_STANDARDS.md](../../CODING_STANDARDS.md) for complete project standards and [playwright.config.ts](../../playwright.config.ts) for test configuration.

# Coding Standards

## Page Object Pattern

### Structure

- One class per page, placed in `src/pages/`.
- Class name must match the page it represents (e.g. `RegistrationPage`).
- Accept `Page` as a constructor argument.

### Methods

- **Navigation** – `goto()` handles routing to the page's URL.
- **Actions** – one method per user action (e.g. `fillEmail()`, `submitForm()`).
- **Composite actions** – combine individual steps into a higher-level method (e.g. `register()`).
- **Locators** – expose element locators as getter methods returning a `Locator` (e.g. `getSuccessMessage()`).

### Rules

| Rule                                                  | Reason                                                         |
| ----------------------------------------------------- | -------------------------------------------------------------- |
| **No assertions inside Page Objects.**                | Keeps page objects reusable and free of test logic.            |
| **All assertions go in test files only.**             | Tests stay the single source of truth for expected behavior.   |
| **No test data hardcoded in page objects.**           | Pass data as method arguments so tests control the input.      |
| **Methods describe user intent, not implementation.** | Use names like `register()` rather than `clickSubmitButton()`. |

## Test Files

- Use descriptive test names that explain the scenario and expected outcome.
- **Tagging**: Each test should include appropriate tags representing its scope or characteristics using Playwright's tagging feature (e.g., `{ tag: ['@smoke', '@regression', '@env-independent'] }`).
- **Text assertions**: When asserting displayed messages or text content, prefer `toHaveText()` or `toContainText()` over `toBeVisible()`. This way, on failure the output shows both the expected and the actual text, making it clear what was displayed instead of just that the expected text was absent.

## API Request Classes

### Structure

- One class per endpoint, placed in `src/api-request/`.
- File name describes the action (e.g. `createEntity.ts`, `deleteItems.ts`).
- Class name follows the pattern `Api<Action>` (e.g. `ApiCreateEntity`, `ApiDeleteItems`).
- Accept `APIRequestContext` as a constructor argument.

### Methods

- **`sendRequest()`** – each class exposes a single `sendRequest()` method that calls the endpoint and returns the response or parsed result.
- Define typed interfaces for request options and response shapes in the same file.

### Rules

| Rule                                                                  | Reason                                                                       |
| --------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| **One endpoint per file.**                                            | Keeps classes small, focused, and independently reusable.                    |
| **No assertions inside API request classes.**                         | Tests own all assertion logic; API classes only handle HTTP calls.           |
| **Throw on failure with a descriptive error message.**                | Fail fast with context so test output is immediately actionable.             |
| **Use environment variables for URLs (`API_URL`, `BACKOFFICE_URL`).** | Keeps classes portable across environments without code changes.             |
| **Set standard headers inside the class.**                            | `origin` and `content-type` are consistent; avoid duplicating them in tests. |

You are an expert test‑automation architect. Design and scaffold a maintainable Playwright test automation framework for Web UI testing that uses the Page Object Model (POM), supports multi‑env configs, tagged test execution, fixtures/test data management, parallel execution, retries, and rich reporting. Produce code, file structure, and explanations inline where appropriate.
Constraints & Preferences

Language: <typescript | javascript | csharp> → default to TypeScript unless I say otherwise.
Test runner: Playwright Test (built‑in).
Design: Strict POM, DRY, SOLID where applicable; readable locators and resilient waits (avoid sleep).
Quality gates: Linting/formatting, type safety (if TS), pre‑commit hooks.
CI: Provide a minimal workflow for GitHub Actions (and a note for Azure Pipelines) with matrix for browsers and environment inputs.
Docs: Include a short README.md with how to run locally/CI, tags, and folder conventions.
Security: No secrets in code; use env vars / .env (Node) or user‑secrets (C#).
Extensibility: Easy to add new pages/specs without touching core plumbing.
Cross‑browser: Chromium, WebKit, Firefox; headed/headless toggle.
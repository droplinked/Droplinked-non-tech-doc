# Shopfront Cart System - Strategic Plan for AI-Driven Test Automation

**Date:** February 16, 2026  
**Scope:** `test case/shop front/Shopfront (New version) - Cart System (New).csv`

## 1. Executive Summary

You can automate most regression testing from your CSV-based test cases and reduce manual QA effort significantly, but the best-performing model is **not "AI-only"**. The most reliable strategy is:

1. Deterministic automation owns release gates (API, contract, E2E, accessibility, performance, security).
2. AI accelerates test generation, maintenance, and failure triage under strict guardrails.
3. Human QA shifts from repetitive regression execution to exploratory testing, risk modeling, and quality governance.

From your current file structure, this is feasible immediately because test cases already include strong behavioral fields (`Given`, `When`, `Then`, `Expected Result`).

## 2. Current Dataset Readiness

Quick scan of `Shopfront (New version) - Cart System (New).csv`:

- Total identified test cases: about **190** (`TC-*`).
- Functional domains:
1. `01 - ADD TO CART`
2. `02 - CART OVERLAY`
3. `03 - CART PAGE`
4. `04 - RULESET ENGINE (WEB3)`
- Existing structure includes:
1. Behavior fields (`Given`, `When`, `Then`, `Expected Result`)
2. Test metadata (`Type`, `Precondition`)
3. Existing execution history columns (`Last Sprint test`, unit/integration/API indicators)

This is a strong base for automated conversion into executable tests.

## 3. Target Operating Model (Near-Zero Manual Regression)

## 3.1 Quality Ownership Model

- `Unit tests`: developers own behavior correctness at component/function level.
- `API + contract tests`: QA engineering + backend define service correctness and integration safety.
- `UI E2E`: QA engineering validates critical user journeys and cross-browser behavior.
- `Manual QA`: limited to exploratory and ambiguous UX/risk areas, not repetitive regression.

## 3.2 Test Layer Mix (Recommended)

- **55%** unit tests
- **25%** API/integration tests
- **15%** UI E2E tests
- **5%** non-functional checks (a11y, performance, security smoke)

Reason: faster, less flaky pipelines while preserving end-user confidence.

## 4. Tooling Strategy

Recommended core stack (pragmatic, scalable):

| Capability | Recommended Tool | Why |
|---|---|---|
| Browser E2E | Playwright | Built-in auto-waiting, retries, tracing, parallelism, codegen |
| API functional testing | Playwright APIRequestContext or Newman/Supertest | Reuse auth/data setup with CI-friendly execution |
| Contract testing | Pact | Prevent frontend-backend breakage before deployment |
| Load/performance baseline | k6 | CI-friendly performance thresholds |
| Security baseline (DAST) | OWASP ZAP baseline scan | Fast automated web security smoke |
| Test reports | Allure or Playwright HTML + JUnit artifacts | Better diagnostics and trend tracking |
| CI orchestration | GitHub Actions matrix + artifacts | Parallel runs and auditable pipelines |
| AI generation/triage | LLM via Structured Outputs + Evals discipline | Controlled generation with schema safety |

Alternative options:

- Cypress can be used if team experience is higher there.
- mabl/Testim-type AI platforms can speed setup, but require governance on generated steps and costs.

## 5. AI Usage Model (Safe and Effective)

Use AI as a **copilot pipeline**, not autonomous release authority.

## 5.1 Where AI Adds High Value

1. Convert CSV rows into canonical test specs (JSON/YAML).
2. Generate first-draft API and E2E test code.
3. Propose selector updates when UI changes.
4. Classify failures (product bug vs flaky test vs environment issue).
5. Detect coverage gaps from BAC phrases and historical escapes.

## 5.2 Guardrails (Mandatory)

1. AI-generated tests must pass lint + dry-run + reviewer checks before merge.
2. No direct auto-merge from AI commits to protected branches.
3. Stable selectors (`data-testid`) required for self-healing suggestions.
4. Flaky tests are quarantined automatically and excluded from hard release gates until fixed.
5. AI quality measured continuously with evaluation datasets (prompt and output scoring).

## 6. Implementation Blueprint for Your CSV Workflow

## 6.1 Canonicalization Pipeline

Build a converter:

`CSV -> canonical JSON -> generated test artifacts`

Canonical schema example:

```json
{
  "id": "TC-CART-CREATE-001",
  "module": "add_to_cart",
  "title": "Verify Cart Creation on First Add to Cart",
  "type": "functional",
  "precondition": "Customer has no cart yet",
  "given": "Customer selects SKU + quantity on PDP",
  "when": "Customer clicks Add to Cart",
  "then": "System checks if cart exists",
  "expected": "Cart is created and product added",
  "priority": "P0",
  "tags": ["cart", "creation", "smoke"]
}
```

Then map by type:

- `Functional + backend behavior` -> API/integration tests.
- `User flow + UI behavior` -> Playwright UI tests.
- `Ruleset/Web3 gating` -> contract/API tests first, UI second.

## 6.2 Suggested Repository Structure

```text
qa/
  test-specs/
    cart-system.json
  generators/
    csv-to-spec.ts
    spec-to-playwright.ts
  tests/
    api/
    e2e/
    contracts/
    a11y/
    perf/
    security/
  fixtures/
  reports/
```

## 6.3 CI Pipeline Gates

Per PR:

1. Lint + unit tests
2. API + contract suite
3. Critical E2E smoke (chromium)
4. Accessibility checks (critical pages)

Nightly:

1. Full E2E matrix (chromium/firefox/webkit)
2. Extended API/regression suite
3. Visual snapshot suite
4. k6 baseline load test
5. ZAP baseline security scan

Release gate example:

- P0/P1 tests pass rate >= 99%
- flaky rate < 2%
- no critical accessibility violations
- no high-severity ZAP findings

## 7. 90-Day Rollout Plan

## Phase 1 (Weeks 1-2): Foundations

1. Clean and normalize CSV schema across all shopfront files.
2. Add risk tags (`P0/P1/P2`, `smoke/regression`, `api/ui`).
3. Define stable selector policy (`data-testid`) with frontend team.

Deliverable: machine-readable canonical test spec and governance rules.

## Phase 2 (Weeks 3-6): Core Automation

1. Automate top P0/P1 cart cases via API and UI (first 60-80 cases).
2. Implement CI PR gates + nightly jobs.
3. Add failure artifacts (trace/video/screenshots/logs).

Deliverable: automated regression for core cart journeys.

## Phase 3 (Weeks 7-10): AI Acceleration

1. Generate draft tests from remaining CSV rows.
2. Add AI triage assistant for failed runs.
3. Add contract tests for ruleset engine dependencies.

Deliverable: high test authoring velocity with controlled quality.

## Phase 4 (Weeks 11-13): Hardening and Scale

1. Add performance + security + a11y baselines.
2. Tune flaky quarantine and retry policy.
3. Define release SLO dashboards and ownership.

Deliverable: near-zero manual regression execution model.

## 8. KPI Framework

Track weekly:

1. Automation coverage: automated cases / total active cases
2. Escaped defects: production bugs missed by pre-release tests
3. Flaky rate: flaky failures / total failures
4. Mean feedback time: commit to test result
5. MTTR for red pipeline
6. Manual QA hours spent on regression (target strong reduction)

Practical success target after 90 days:

- >= 80% of active cart regression cases automated
- >= 90% of release-critical scenarios fully automated
- manual regression execution reduced by 60-80%

## 9. Risks and Mitigations

1. **Flaky UI tests**  
Mitigation: API-first coverage, stable selectors, trace-based triage, quarantine workflow.

2. **AI-generated low-quality tests**  
Mitigation: schema-constrained outputs, mandatory review, eval dataset, no autonomous merge.

3. **Environment instability**  
Mitigation: ephemeral test environments, deterministic seed data, contract tests at service boundaries.

4. **Over-reliance on E2E only**  
Mitigation: enforce test pyramid ratios and fail fast on API/contract layer.

## 10. What "No Manual QA" Should Mean in Practice

Do not interpret it as "zero humans in quality". Interpret it as:

- No repetitive manual regression execution.
- Human effort focused on exploratory, UX-risk, and release decision governance.
- Automated evidence as primary release input.

This model is realistic, scalable, and significantly safer than trying to fully replace human QA judgment.

## 11. Next Actions for Your Team

1. Approve the core stack (Playwright-centered or Cypress-centered).
2. Build CSV normalization script and canonical schema this week.
3. Select top 30 P0 cart cases and automate them first.
4. Implement PR and nightly pipeline gates.
5. Introduce AI generation only after baseline deterministic pipeline is stable.

## References

- Playwright docs (overview, features, and guides): https://playwright.dev/docs/intro
- Playwright auto-waiting: https://playwright.dev/docs/auto-waiting
- Playwright retries: https://playwright.dev/docs/test-retries
- Playwright trace viewer: https://playwright.dev/docs/trace-viewer
- Playwright codegen: https://playwright.dev/docs/codegen
- Playwright accessibility testing guide: https://playwright.dev/docs/accessibility-testing
- Cypress docs: https://docs.cypress.io/
- Selenium WebDriver docs: https://www.selenium.dev/documentation/webdriver/
- Pact documentation: https://docs.pact.io/
- can-i-deploy (Pact Broker): https://docs.pact.io/pact_broker/can_i_deploy
- k6 thresholds: https://grafana.com/docs/k6/latest/using-k6/thresholds/
- OWASP ZAP baseline scan (Docker): https://www.zaproxy.org/docs/docker/baseline-scan/
- GitHub Actions matrix builds: https://docs.github.com/actions/using-jobs/using-a-matrix-for-your-jobs
- GitHub Actions artifacts: https://docs.github.com/actions/using-workflows/storing-workflow-data-as-artifacts
- OpenAI Structured Outputs: https://openai.com/index/introducing-structured-outputs-in-the-api/
- OpenAI function calling guide: https://platform.openai.com/docs/guides/function-calling
- OpenAI eval-driven development cookbook: https://cookbook.openai.com/examples/evaluation/use-cases/evalsapi_tool_calling
- mabl AI-generated test notes: https://help.mabl.com/hc/en-us/articles/29989414013716-Generate-test-steps-using-AI
- OpenAPI Specification: https://spec.openapis.org/oas/latest.html
- Schemathesis docs: https://schemathesis.readthedocs.io/

# Complete Research Report: AI-Driven Test Automation for Shopfront QA

**Prepared for:** Droplinked Shopfront QA modernization  
**Date:** February 16, 2026  
**Repository scope analyzed:**
- `test case/shop front/Shopfront (New version) - Cart System (New).csv`
- `test case/shop front/Shopfront (New version) - Product Pages (new).csv`
- `test case/shop front/NEW_TEST_CASES_PRODUCT_UPDATE.csv`

## 1. Purpose

This report defines a practical strategy to automate your CSV-based test cases using deterministic automation plus AI assistance, with the goal of removing repetitive manual regression work and making releases test-gated by machine evidence.

Important boundary: fully removing humans from quality decisions is not recommended. Manual execution of repetitive regression can be minimized, while humans focus on exploratory and risk-based validation.

## 2. Current-State Assessment of Your Test Assets

### 2.1 Dataset inventory

Based on repository scan of the three shopfront CSV files:

| File | Valid `TC-*` IDs detected | Notes |
|---|---:|---|
| `NEW_TEST_CASES_PRODUCT_UPDATE.csv` | 47 | Mostly PLP visibility, stock, status, SKU updates |
| `Shopfront (New version) - Cart System (New).csv` | 190 | Cart create/merge/overlay/page/ruleset-web3 |
| `Shopfront (New version) - Product Pages (new).csv` | 186 | PLP and PDP heavy |
| **Total** | **423** | Usable automated-seed cases |

### 2.2 Data quality observations

1. The files are automation-friendly because they already include behavior fields (`Given`, `When`, `Then`, `Expected Result`).
2. Some sheet summaries do not perfectly match parseable `TC-*` rows (example: header range in product pages vs detected IDs), so normalization is required before generation.
3. Existing status columns (`Pass`, `Failed`, sprint columns) are useful as priors for risk scoring and prioritization.

## 3. Research Findings (Tools and Methods)

## 3.1 Browser and UI automation backbone

Playwright is a strong default for your use case because it supports Chromium, Firefox, and WebKit, includes code generation, and provides traces/screenshots/video for debugging [1].

For flake control, Playwright auto-waits on actionability conditions (visible, stable, receiving events, enabled), reducing timing-related instability [2]. Its retry model also classifies tests as `passed`, `flaky`, or `failed`, which is useful for quarantine policies [3].

## 3.2 Accessibility coverage

Playwright supports accessibility-focused testing patterns and integration with Axe, but manual checks are still required for full coverage [4]. Use automation for repeatable checks and manual review for semantic/contextual issues. For policy baselines, align to WCAG 2.2 [5].

## 3.3 API, schema, and contract reliability

1. Use Playwright APIRequestContext for service-level tests that can run fast in PRs [6].
2. Treat OpenAPI as the executable contract. Current OAS latest is 3.2.0 (published September 19, 2025), so target that where possible [7].
3. Add Schemathesis on top of OpenAPI/GraphQL specs to generate negative and edge API tests automatically [8].
4. Add consumer-driven contracts with Pact to prevent integration drift [9].
5. Gate deployments with Pact `can-i-deploy` checks in CI/CD [10].

## 3.4 Performance and security baselines in CI

1. Use k6 thresholds so performance tests produce objective pass/fail outcomes [11].
2. Use OWASP ZAP baseline scans nightly; baseline mode runs passive checks and does not actively attack the target [12].

## 3.5 CI/CD orchestration

GitHub Actions matrix strategy is suitable for browser and environment parallelization [13]. Store traces/reports as artifacts for triage, and note default artifact/log retention is 90 days unless configured otherwise [14].

## 3.6 AI in test automation: where it helps and where to constrain it

OpenAI Structured Outputs can force schema-conformant generation (`strict: true`) for reliable CSV-to-test-spec conversion [15]. A key caveat is that strict mode should avoid parallel function calls for compatibility [15].

For quality governance, use eval-driven development with graders so prompt and generator changes are measured before rollout [16]. Prompt caching can also reduce repeated prompt cost/latency for large recurring templates [17].

Commercial AI alternatives exist (for example mabl step generation), but have operational constraints such as model/add-on limits and language quality caveats [18].

## 3.7 Web3-specific implications for your Ruleset Engine tests

Because your cart suite includes wallet-gated and discount-gated cases, test design should include wallet provider standards and local chain simulation.

1. EIP-1193 defines the provider API and `request` RPC flow for wallets [19].
2. MetaMask provider docs expose provider behavior/events used by web apps (`window.ethereum`) [20].
3. Hardhat Network provides a local Ethereum network for deterministic contract and permission tests [21].

## 4. Recommended Target Architecture

## 4.1 End-to-end flow

`CSV test cases -> normalized canonical JSON -> generated API/E2E/contract tests -> CI gates -> artifacts + AI triage -> release decision`

## 4.2 Canonical schema

Use a strict schema per row:

```json
{
  "id": "TC-CART-CREATE-001",
  "domain": "cart",
  "module": "create",
  "title": "Verify Cart Creation on First Add to Cart",
  "priority": "P0",
  "layer": "api|ui|contract",
  "precondition": "...",
  "given": "...",
  "when": "...",
  "then": "...",
  "expected": "...",
  "data": {},
  "tags": ["smoke", "regression"]
}
```

## 4.3 Layer allocation policy

1. `UI-only rendering/navigation`: UI E2E.
2. `Business rules and data correctness`: API or integration first.
3. `Frontend-backend compatibility`: contract tests.
4. `Wallet/ruleset access control`: contract + API + minimal UI confirmation.

This keeps E2E volume controlled and execution stable.

## 5. Strategy to Remove Manual Regression Dependency

## 5.1 Define success precisely

"No manual QA dependency" should mean:

1. No repetitive manual regression before release.
2. Release gates are fully automated for P0/P1 behavior.
3. Manual QA remains for exploratory, new feature ambiguity, and incident investigations.

## 5.2 Quality gates

Per PR (hard gates):

1. Unit + lint
2. API smoke + contract verification
3. Critical UI smoke (cart create, update, checkout-adjacent flows)
4. Static accessibility checks

Nightly (broad regression):

1. Full UI matrix (Chromium/Firefox/WebKit)
2. Extended API + Schemathesis
3. Pact verification + can-i-deploy
4. k6 threshold suite
5. ZAP baseline scan

## 5.3 Flake and failure governance

1. Auto-retry once for PR and twice for nightly.
2. Mark `flaky` distinctly and quarantine after threshold breaches.
3. Block release on deterministic failures only, but require owner SLA to fix flaky tests.
4. Always attach traces, screenshot, and console logs to failed runs.

## 6. 12-Week Implementation Roadmap

## Phase 1 (Weeks 1-2): Data and standards

1. Normalize CSV format and enforce single header layout.
2. Add mandatory columns: `Priority`, `Layer`, `Owner`, `AutomationStatus`.
3. Define `data-testid` policy with frontend.

Output: parseable canonical dataset for all 423 current test IDs.

## Phase 2 (Weeks 3-5): Core pipeline

1. Build `csv-to-json` normalizer.
2. Scaffold Playwright UI + API projects.
3. Add Pact contract setup for key cart and PLP/PDP APIs.
4. Add GitHub Actions matrix pipeline and artifacts.

Output: automated PR gates for P0 smoke.

## Phase 3 (Weeks 6-8): Scale and stabilize

1. Automate top 150 high-risk cases.
2. Add Schemathesis against OpenAPI specs.
3. Implement flaky quarantine and dashboarding.

Output: stable nightly regression and measurable release confidence.

## Phase 4 (Weeks 9-12): AI acceleration and governance

1. Add AI-assisted spec completion and first-draft test generation with strict schema outputs.
2. Add evaluator suite for generated test quality.
3. Add AI triage for failed run clustering and owner assignment suggestions.

Output: faster maintenance velocity with controlled risk.

## 7. KPI and SLO Model

Track weekly:

1. Automation coverage = automated active cases / active cases total
2. P0/P1 release readiness pass rate
3. Flaky rate
4. Mean PR feedback time
5. Escaped defects to production
6. Manual regression hours per sprint

90-day targets (realistic for current dataset):

1. >= 80% automated coverage on active 423-case pool
2. >= 95% coverage of P0/P1 cart and product critical paths
3. <= 2% flaky rate
4. >= 60% reduction in manual regression execution hours

## 8. Operating Model and Team Design

Recommended minimal team during rollout:

1. 1 QA automation lead
2. 1 SDET/QA engineer
3. 1 backend engineer (API/contract ownership)
4. 1 frontend engineer (selectors and UI stability)
5. 1 platform engineer part-time (CI throughput and observability)

Ownership principle:

- Developers own unit tests and selector contracts.
- QA automation owns cross-layer regression strategy.
- Product and engineering leads own release quality SLOs.

## 9. Risk Register and Mitigation

1. **CSV inconsistency blocks generation**  
Mitigation: normalization stage with validation and fail-fast checks.

2. **E2E suite gets slow/flaky**  
Mitigation: move assertions down to API/contract layer; keep E2E to critical journeys.

3. **AI introduces noisy tests**  
Mitigation: strict schema outputs, mandatory human review, eval-based acceptance.

4. **Web3 wallet tests unstable in CI**  
Mitigation: local deterministic chain + mocked provider flows + minimal live-wallet smoke.

## 10. Final Recommendation

Adopt a **deterministic-first + AI-accelerated** model:

1. Build strong deterministic test gates first (API, contract, critical UI).
2. Use AI only to speed authoring, mapping, and triage after gates are stable.
3. Transition manual QA from regression execution to high-value exploratory governance.

This path is the fastest way to cut manual QA dependency without sacrificing release confidence.

## References (checked February 16, 2026)

1. Playwright intro: https://playwright.dev/docs/intro  
2. Playwright auto-waiting: https://playwright.dev/docs/actionability  
3. Playwright retries and flaky classification: https://playwright.dev/docs/test-retries  
4. Playwright accessibility testing: https://playwright.dev/docs/accessibility-testing  
5. WCAG 2.2 recommendation: https://www.w3.org/TR/WCAG22/  
6. Playwright API testing: https://playwright.dev/docs/api-testing  
7. OpenAPI Specification (latest 3.2.0): https://spec.openapis.org/oas/latest.html  
8. Schemathesis docs: https://schemathesis.readthedocs.io/en/stable/  
9. Pact docs: https://docs.pact.io/  
10. Pact can-i-deploy: https://docs.pact.io/pact_broker/can_i_deploy  
11. k6 thresholds: https://grafana.com/docs/k6/latest/using-k6/thresholds/  
12. OWASP ZAP baseline scan: https://www.zaproxy.org/docs/docker/baseline-scan/  
13. GitHub Actions matrix strategy: https://docs.github.com/actions/using-jobs/using-a-matrix-for-your-jobs  
14. GitHub artifact retention defaults: https://docs.github.com/actions/managing-workflow-runs/downloading-workflow-artifacts  
15. OpenAI Structured Outputs: https://openai.com/index/introducing-structured-outputs-in-the-api/  
16. OpenAI cookbook eval-driven tool calling workflow: https://github.com/openai/openai-cookbook/blob/main/examples/evaluation/use-cases/evalsapi_tool_calling.ipynb  
17. OpenAI prompt caching: https://openai.com/index/api-prompt-caching/  
18. mabl AI step generation notes: https://help.mabl.com/hc/en-us/articles/29989414013716-Generate-test-steps-using-AI  
19. EIP-1193 provider API: https://eips.ethereum.org/EIPS/eip-1193  
20. MetaMask provider API: https://docs.metamask.io/wallet/reference/provider-api/  
21. Hardhat Network overview: https://hardhat.org/hardhat-network/docs/overview

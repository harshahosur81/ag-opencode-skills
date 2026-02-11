---
name: qa-engineer
description: QA Engineer Skill
---

## The Four Phases

You MUST complete each phase before proceeding to the next.

### Phase 1: Test Planning (Shift Left)

**BEFORE code is written:**

1.  **Analyze the Requirements (Static Testing)**
    - Read the PRD/Ticket.
    - **Find Logical Holes:** "What happens if the user has no internet?" "What if the date is in the past?"
    - Challenge the PM/Dev: "How can we test this?"
    - **Goal:** Prevent bugs *before* they are coded.

2.  **Define the Test Strategy**
    - What is the scope? (UI only? API? Database?)
    - What devices/browsers must we support?
    - Do we need test data? (e.g., a user with an expired credit card).

3.  **Risk Assessment**
    - What is the impact of failure? (Critical/High/Low).
    - Focus effort on the high-risk areas. You cannot test everything.

### Phase 2: Test Execution (Manual/Exploratory)

**Finding the unknown unknowns:**

1.  **Sanity / Smoke Test**
    - Does the build even launch?
    - **Runtime Symbol Audit (MANDATORY):** Verify all imported symbols (e.g., `crypto`, `fs`) are defined.
    - **Dry-Run Execution:** Verify server reaches "Ready" without ReferenceErrors.
    - If this fails, reject the build immediately.

2.  **Exploratory Testing**
    - Don't just follow a script. Be a detective.
    - Try to break it: Double click buttons. Enter emojis in name fields. Use back buttons.
    - Change network speed (Throttling) to see how it handles slow loading.

3.  **Cross-Platform Verification**
    - Test on Mobile (iOS/Android).
    - Test on Desktop (Chrome/Safari).
    - **Responsive Check:** Does the layout break on small screens?

### Phase 3: Automation & Regression (The Safety Net)

**Codifying the knowns:**

1.  **Automate Stable Features**
    - **Rule:** Do not automate a feature that is still changing.
    - Write E2E tests (Cypress/Playwright) for the "Happy Path."
    - Write API tests for backend logic (faster/reliable).

2.  **Manage Flakiness**
    - A test that fails randomly is worse than no test.
    - **Action:** If a test is flaky, fix it or delete it. Do not ignore it.

3.  **Regression Suite**
    - Run the suite to ensure new code didn't break old features.
    - Focus on "Critical Business Flows" (Login, Checkout, Signup).

### Phase 3.5: Visual Regression Testing (2026)

**Catch UI bugs that functional tests miss:**

1.  **Screenshot Comparison**
    - **Tools:** Percy, Chromatic (for Storybook), Playwright screenshots
    - **How:** Capture baseline, compare on every PR
    - **Threshold:** Allow 0.1% pixel difference (antialiasing)
    - **When:** Design system components, marketing pages

2.  **Visual Review Workflow**
    - Baseline approved by designer
    - PR shows visual diff automatically
    - Approve or request changes before merge
    - **Benefit:** Prevents unintended CSS changes

3.  **Cross-Browser Visual Testing**
    - **Challenge:** Fonts render differently (Chrome vs Safari)
    - **Solution:** Test on actual browsers, not emulators
    - **Tools:** BrowserStack, Sauce Labs, Percy

### Phase 3.6: Chaos Engineering

**Test resilience by breaking things on purpose:**

1.  **Chaos Principles**
    - **Hypothesis:** "If Redis fails, app degrades gracefully (slow, not down)"
    - **Bl ast Radius:** Test in staging first, production during low traffic
    - **Automated:** Run chaos tests in CI/CD weekly

2.  **Failure Injection Scenarios**
    - Kill random pods/containers
    - Introduce network latency (500ms)
    - CPU/Memory pressure
    - Database connection pool exhaustion

3.  **Tools**
    - **Chaos Mesh:** Kubernetes-native
    - **Gremlin:** Enterprise chaos engineering platform
    - **Toxiproxy:** Network failure simulation
    - **AWS Fault Injection Simulator:** Cloud-native

### Phase 4: Reporting & Advocacy

**The Gatekeeper:**

1.  **Bug Reporting**
    - **Clear Title:** `[Component] Action results in Error`
    - **Steps to Reproduce:** Exact steps. 1, 2, 3.
    - **Evidence:** Screenshots, Video, Console Logs, API responses.
    - **Severity vs. Priority:** Severity = Impact (Crash). Priority = Urgency (Fix now).

2.  **Release Decision**
    - Provide a "Go / No-Go" recommendation based on data.
    - "We have 0 Critical bugs, but 5 Visual bugs. Recommendation: Ship."
    - **MANDATORY: Verify that a 'fine-toothed comb' code review has been completed by the specific personas (PE, PM, Designer).**

3.  **Root Cause Analysis (Post-Bug)**
    - Ask the Dev: "How did we miss this? Did we lack a unit test?"
    - Improve the process so this bug type doesn't return.

## Red Flags - STOP and Follow Process

If you catch yourself thinking:
- "The dev said they tested it, I'll trust them."
- "I'll just test the happy path to get this merged."
- "I'll write the bug report later." (You'll forget details).
- "This test fails sometimes, just re-run it until it passes." (Flaky test).
- "I don't need to check logs, the UI looks fine." (Hidden errors).
- "I'll test everything manually forever." (Unscalable).
- **Skipping the 'fine-toothed comb' pre-deployment code review.**

**ALL of these mean: STOP. Return to Phase 1.**

## Your Human Partner's Signals You're Doing It Wrong

**Watch for these complaints:**
- **Dev:** "I can't reproduce your bug." (Your report lacks Steps/Evidence).
- **PM:** "Why is QA taking so long?" (You are manually testing what should be automated).
- **Team:** "The build is always red." (Flaky tests).
- **Users:** "The app crashes on iPhone." (You skipped Cross-Platform tests).
- **Dev:** "You're finding typos, not logic bugs." (You're focusing on Low Priority issues).

**When you see these:** STOP. Refine your strategy.

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "It's a small change, no need to test" | Small changes cause big outages. Smoke test it. |
| "I don't have time to automate" | Then you will spend all your time manually testing regression. |
| "It works in Staging" | Staging is not Production. Data/Config might differ. |
| "Documentation is outdated" | Then clarify requirements *before* testing. |

## Quick Reference

| Phase | Key Activities | Success Criteria |
|-------|---------------|------------------|
| **1. Planning** | Requirements Review, Strategy | Potential bugs found in specs |
| **2. Execution** | Exploratory, Cross-browser | Bugs logged with evidence |
| **3. Automation** | E2E Scripts, API Tests | Green regression suite |
| **4. Reporting** | Bug Triage, Go/No-Go | Informed shipping decision |

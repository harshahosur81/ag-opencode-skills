---
name: code-review
description: Code Review Skill
---

## The Four Phases

You MUST complete each phase before proceeding to the next.

### Phase 1: Context & Pre-Flight

**BEFORE looking at a single line of code:**

1.  **Verify the "Why"**
    - Read the PR description and linked ticket/issue.
    - Does this solve the stated problem?
    - Is the solution proportionate to the problem? (e.g., don't re-architect the DB for a typo fix).

2.  **Check the Automated Gates**
    - **STOP if CI is failing.** Do not review code that doesn't build/pass tests.
    - Check linting results and static analysis.
    - Verify code coverage hasn't dropped.

3.  **Assess the Scope**
    - Is the PR too large (>400 lines)?
    - Does it mix refactoring with feature work? (Violates "Single Responsibility Principle" of PRs).
    - Is it consistent with the existing patterns in the codebase?
- **CRITICAL: This review MUST be a 'fine-toothed comb' pass. Surface-level checks are forbidden.**
- **Action:** If too big, request a split immediately. Do not attempt to review "spaghetti PRs".

### Phase 2: The Architectural Pass (The "Macro" View)

**Look at the shape, not the syntax:**

1.  **Architecture & Design**
    - Does this code belong in this file/module?
    - Does it introduce circular dependencies?
    - Is it consistent with the existing patterns in the codebase?
    - **Action:** If the architecture is wrong, stop reviewing details. Reject the approach first.

2.  **Security & Safety**
    - Are user inputs sanitized?
    - Are permissions/authorizations checked?
    - Are secrets hardcoded?
    - Is there PII (Personal Identifiable Information) exposure?

3.  **Performance & Scale**
    - Look for N+1 queries.
    - Loops inside loops or expensive operations on the main thread.
    - Unbounded list rendering (missing pagination).

### Phase 3: The Logic & Implementation Pass (The "Micro" View)

**Now inspect the lines:**

1.  **Correctness & Edge Cases**
    - **Don't just verify the Happy Path.**
    - What happens if the array is empty? If the API returns 500? If the value is null?
    - Check for "Off-by-one" errors.

2.  **Readability & Maintainability**
    - Are variable names descriptive? (`data` vs `userProfilePayload`).
    - Are functions short and single-purpose?
    - *The "Six Month Rule":* Will we understand this code in 6 months?

3.  **Test Quality**
    - Do the tests actually fail if the logic breaks? (Look for `assert(true)` or weak expectations).
    - Are they testing implementation details (bad) or behavior (good)?
    - Is the test code clean, or is it a mess of mocks?

### Phase 4: Feedback & Delivery

**How you say it matters as much as what you say:**

1.  **Categorize Your Comments**
    - **[BLOCKER]:** Bug, security risk, spec violation. PR cannot merge.
    - **[IMPORTANT]:** Technical debt, strong architectural preference. Discuss before merge.
    - **[NIT]:** Formatting, renaming, minor polish. Optional/Non-blocking.
    - **[QUESTION]:** "I don't understand this part, can you explain?"

2.  **Critique the Code, Not the Person**
    - Bad: "You didn't handle the error here."
    - Good: "This block might throw an exception; should we add a try/catch?"
    - Use "We" instead of "You" (e.g., "We should handle...")

3.  **Explicit Approval Status**
    - Don't leave them guessing. Explicitly "Request Changes" or "Approve".
    - If you left comments but they are non-blocking, Approve with comments.

## Red Flags - STOP and Request Changes

If you see these patterns:
- **PR > 500 Lines:** "Please split this into smaller, logical PRs."
- **No Description/Screenshots:** "Please add context/proof of work."
- **"WIP" or "Draft" status:** Do not review until ready.
- **Commented out code:** "Delete it or use a feature flag."
- **Console logs left in:** "Remove debug logging."
- **Refactoring mixed with Logic:** "Please separate the rename/refactor into a separate PR to make this diff readable."
- **Magic Numbers/Strings:** "Extract this to a constant."

**ALL of these mean: STOP. Do not offer a full review until fixed.**

## Your Human Partner's Signals You're Doing It Wrong

**Watch for these complaints:**
- "You're just nitpicking style." - You missed Phase 2 (Architecture).
- "Why did this break production? You approved it!" - You skipped Phase 3 (Edge cases).
- "I didn't know that was a blocker." - You failed Phase 4 (Categorization).
- "This review took 3 days." - You are blocking the pipeline. Review is a high-priority task.

**When you see these:** STOP. Re-evaluate your review priorities.

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "It's a hotfix, just merge it" | Hotfixes bypass safety and cause the *next* outage. |
| "I trust them, they are a senior dev" | Seniors make complex bugs. Everyone needs review. |
| "I'll fix the style in my next PR" | Technical debt accrues instantly. Fix it now. |
| "It works on my machine" | But does it work in the CI/Production environment? |
| "The PR is too big to split now" | It is too big to review safely. Sunk cost fallacy. |
| "LGTM" (Looks Good To Me) after 1 min | You didn't read it. You are rubber-stamping. |
| "It's a small change" | Small changes need 'fine-toothed comb' reviews too. |

## Quick Reference

| Phase | Key Activities | Success Criteria |
|-------|---------------|------------------|
| **1. Context** | Check CI, Description, Scope | Understanding of goal, clean CI |
| **2. Architecture** | Design patterns, Security, Perf | Safe, scalable, consistent design |
| **3. Logic** | Edge cases, Naming, Tests | Correct logic, clean code, strong tests |
| **4. Delivery** | Tag comments (NIT vs BLOCKER) | Clear, actionable, kind feedback |

## When The PR is "Unreviewable"

If the code is a mess, the logic is impossible to follow, or the architecture is fundamentally flawed:

1.  **Stop reviewing individual lines.**
2.  Write a high-level summary comment explaining the fundamental issue.
3.  Offer a synchronous call (Zoom/Huddle) to discuss the approach.
4.  **Mark as "Request Changes".**

**Do not waste time nitpicking syntax on code that needs to be deleted.**

## Supporting Techniques

- **`superpowers:clean-code`** - Reference for what "Good" looks like.
- **`superpowers:security-audit`** - Checklist for Phase 2.
- **`superpowers:empathetic-communication`** - For delivering Phase 4 effectively.

## Real-World Impact

- **Rubber Stamp Reviews:** 30% production bug rate, knowledge silos.
- **Phased Reviews:** <5% production bug rate, shared ownership, team mentorship.
- **Catching a bug in Review:** 10x cheaper than catching it in QA, 100x cheaper than Production.

##  AI-Assisted Code Review (2026)

### AI Tooling
- **GitHub Copilot:** Code review suggestions
- **Qodo:** Auto-generated tests
- **SonarQube:** Static analysis + AI

### What AI Can Do
- Better naming, complexity detection
- Security pattern recognition

### What Only Humans Can Do
-Business logic correctness
- Architectural coherence

##  Automated Security

### Tools
- **Snyk** (dependencies), **Semgrep** (SAST)
- **Trivy** (containers), **GitGuardian** (secrets)

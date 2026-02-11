---
name: root-cause-tracing
description: Root Cause tracing
---

# Technique: Root Cause Tracing

**Concept:** Bugs are rarely where they explode. The explosion (Stack Trace) is just where the bad data finally violated a constraint.

## The Protocol

**Do not fix the line in the stack trace yet.** Follow the data upstream.

### Step 1: Identify the "Contraband"
What specific value caused the crash?
* *Example:* A `null` user ID.
* *Example:* An empty string `""` where a date was expected.
* *Example:* A value of `-1` in a price field.

### Step 2: The Upstream Walk (The "5 Whys")
Open your IDE. Use "Find Usages" or "Call Hierarchy."

1.  **Level 0 (Crash Site):** `processPayment(user.id)` -> Crashed because `user.id` is null.
    * *Question:* Who called `processPayment`?
2.  **Level 1:** `CheckoutService.submitOrder` called it.
    * *Question:* Where did `CheckoutService` get the `user` object?
3.  **Level 2:** It was passed in from `SessionManager.getCurrentUser()`.
    * *Question:* Why did `SessionManager` return a user object with a null ID?
4.  **Level 3:** `SessionManager` hydrated it from the Redis Cache.
    * *Question:* Why is the data in Redis corrupt?
5.  **Level 4 (Root Cause):** The `LoginHandler` wrote the user to Redis *before* the database generated the ID.

### Step 3: Verify the Origin
* **Hypothesis:** The bug is in `LoginHandler` (Level 4), not `processPayment` (Level 0).
* **Test:** Fix the `LoginHandler` ordering.
* **Result:** The crash at Level 0 disappears because the data is now correct.

## Common Traps
* **The "Null Check" Trap:** Adding `if (user.id == null) return;` at Level 0 fixes the crash, but it creates a "Zombie Order" (an order that fails silently). **Do not do this.**
* **The "Sanitizer" Trap:** Cleaning the string at Level 1 hides the fact that the database at Level 5 is corrupt.

**Rule:** Fix the data generation, not the data consumption.

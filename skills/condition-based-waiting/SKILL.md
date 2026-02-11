---
name: condition-based-waiting
description: condition based waiting
---

# Technique: Condition-Based Waiting

**Concept:** Never assume "2 seconds is enough." Computers vary in speed. Network latency varies. Hardcoded sleeps are the #1 cause of flaky debugging and CI failures.

## The Anti-Pattern
```python
# ❌ BAD: Guessing
submit_form()
time.sleep(2) # "Should be done by now..."
assert_success()
```

## The Pattern: Polling with Timeout
Instead of waiting for *time*, wait for *truth*.

### Structure
```python
# ✅ GOOD: Knowing
submit_form()

start = time.time()
while time.time() - start < 10:  # Maximum Timeout (Safety Valve)
    if check_if_success_message_is_visible():
        break  # Success! Exit immediately (Fast)
    time.sleep(0.1)  # Tiny polling interval
else:
    raise TimeoutError("Form did not submit within 10 seconds")

assert_success()
```

## Implementation Strategies

### 1. UI Testing (Selenium/Cypress)
* **Don't use:** `sleep(5000)`
* **Use:** `await waitFor(() => expect(element).toBeVisible())`
* **Why:** If it loads in 0.1s, the test finishes in 0.1s. If it takes 4s, the test passes.

### 2. Async Backend Jobs
* **Don't use:** "Wait 5 seconds for the queue to process."
* **Use:**
    ```python
    # Poll the database for the status change
    wait_for_condition(
        lambda: db.get_order(id).status == 'PROCESSED',
        timeout=10
    )
    ```

### 3. API Consistency (Eventual Consistency)
* **Scenario:** You create a user, but `GET /users` doesn't show them yet.
* **Fix:**
    ```python
    def get_user_safely(user_id):
        # Retry 3 times with exponential backoff
        for i in range(3):
            try:
                return api.get_user(user_id)
            except NotFound:
                sleep(2 ** i) # 1s, 2s, 4s
        raise NotFound
    ```

## Summary
* **Sleeps** are for humans.
* **Conditions** are for machines.
* Always define: "What specifically am I waiting to happen?" and wait for *that*.

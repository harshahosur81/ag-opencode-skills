---
name: defense-in-depth
description: Defense in Depth Skill
---

# Technique: Defense in Depth (Swiss Cheese Model)

**Concept:** After fixing the root cause, you must add "Armor" to the system to catch this specific class of error in the future.

## The Three Layers of Defense

Apply at least **two** of these layers for every critical bug.

### Layer 1: The API Boundary (The "Bouncer")
**Stop bad data from entering your house.**
* **Technique:** Schema Validation (Zod, Joi, Pydantic).
* **Action:** If `price` must be positive, enforce `price > 0` at the JSON parsing layer.
* **Result:** The system rejects the request with `400 Bad Request` instantly. No internal logic runs.

### Layer 2: The Service Boundary (The "Contract")
**Stop bad data from moving between rooms.**
* **Technique:** Domain Types / Value Objects.
* **Action:** Don't pass `string email`. Pass a `EmailAddress` class that throws an error in its constructor if the format is invalid.
* **Result:** You cannot instantiate an invalid object deep in the business logic.

### Layer 3: The Database Constraint (The "Vault")
**Stop bad data from being written to history.**
* **Technique:** SQL Constraints / Foreign Keys.
* **Action:** Add `NOT NULL` constraints. Add `CHECK (price >= 0)`.
* **Result:** Even if code fails, the database refuses to accept the corruption.

## The Checklist
When closing a bug, ask:
1.  [ ] Did I fix the root cause?
2.  [ ] **Defense:** Did I add a validation check so this inputs fails *early* next time?
3.  [ ] **Defense:** Did I verify the database prevents this state?

**Example:**
* **Bug:** User verified with empty email.
* **Fix:** Updated regex in signup logic.
* **Defense 1:** Added `class EmailAddress { ... }` which validates on creation.
* **Defense 2:** Added `CHECK (length(email) > 5)` to Postgres migration.

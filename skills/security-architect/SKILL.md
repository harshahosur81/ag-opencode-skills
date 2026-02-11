---
name: security-architect
description: Security Architect Skill
---

## The Four Phases

You MUST complete each phase before proceeding to the next.

### Phase 1: Authentication Architecture (The Handshake)

**BEFORE writing a single line of auth code:**

1.  **Select the Flow**
    - **Mobile/SPA:** MUST use **Authorization Code Flow with PKCE** (Proof Key for Code Exchange).
    - **Backend:** Authorization Code Flow.
    - **Implicit Flow:** **FORBIDDEN.** Never use it. It returns tokens in the URL.
    - **Device Flow:** Only for input-constrained devices (TVs/IoT).

2.  **Scope Strategy (Least Privilege)**
    - Define exactly which permissions are needed from YouTube (`youtube.readonly`) vs Twitch (`chat:read`).
    - **Incremental Auth:** Do not ask for all scopes at signup. Ask for `youtube.upload` only when the user actually clicks "Upload."
    - **Justification:** Be ready to explain to the user *why* you need this access.

3.  **The "No-Credential" Rule**
    - **Principle:** We never see, touch, or store the user's password.
    - **Identity Provider (IdP):** Delegate login to the provider (Google/Twitch).
    - **Redirect URIs:** strict allow-listing. No wildcards (`*`).

### Phase 2: Token Management & Storage (The Vault)

**Protecting the delegated access:**

1.  **Storage Hierarchy**
    - **Passwords:** NEVER STORED.
    - **Client Secrets:** NEVER in frontend code (Mobile/React). Backend/BFF (Backend for Frontend) only.
    - **Access Tokens (Short-lived):** Keep in memory (frontend) or `HttpOnly` Secure Cookies. Never `localStorage`.
    - **Refresh Tokens (Long-lived):** Encrypt at Rest (AES-256 or Cloud KMS) in the database. Never plaintext.

2.  **Token Rotation Strategy**
    - Detect reused tokens. If a Refresh Token is used twice, revoke the entire chain (Rotation Policy).
    - Handle `invalid_grant` errors gracefully (prompt user to re-login).
    - **Revocation:** If the user deletes their account, call the provider's `revoke` endpoint immediately.

3.  **State & Nonce Validation**
    - **CSRF Protection:** Always send a unique, random `state` parameter during the auth request.
    - Verify the `state` matches upon return.
    - Use `nonce` (OIDC) to prevent Replay Attacks.

### Phase 3: Multi-Provider Mesh (The Integration)

**Managing the Identity Map:**

1.  **Account Linking Logic**
    - **Primary Identity:** The user logs in with "Main Account" (e.g., Gmail).
    - **Connected Accounts:** User connects Twitch as a *secondary* resource.
    - **The Map:** `User(ID: 123)` -> `LinkedAccount(Provider: Twitch, Token: ***)` + `LinkedAccount(Provider: YouTube, Token: ***)`.
    - **Rule:** Never merge accounts automatically based on email match (Security risk). Require explicit linking verification.

2.  **The "Refresh Loop" Middleware**
    - **Pattern:** Before calling the Twitch API, check Access Token expiration.
    - **Expired?** Use the Encrypted Refresh Token to get a new Access Token transparently.
    - **Failed?** (User revoked access externally). Pause the integration and notify the user: "Please reconnect Twitch."
    - **Concurrency:** Handle race conditions if multiple requests try to refresh the token simultaneously.

3.  **Rate Limit Isolation**
    - YouTube and Twitch have different API quotas.
    - Track usage *per provider*. Don't let a Twitch outage crash the YouTube integration.

### Phase 4: Threat Modeling & Compliance (The Guard)

**Assuming breach:**

1.  **Dependency Scanning**
    - Audit auth libraries (e.g., `passport`, `next-auth`) for vulnerabilities weekly.
    - **Supply Chain:** Pin versions. Authentication logic is a high-value target for hackers.

2.  **Anomaly Detection**
    - Log auth failures. "User 123 failed Twitch refresh 50 times in 1 minute." -> Alert.
    - Monitor for "Token Export" attempts (large volume of token reads).
    - **Audit Logs:** Log *who* linked *what* and *when*. (Immutable logs).

3.  **Penetration Testing (Self)**
    - Try to swap the `code` parameter from one user to another.
    - Try to manipulate the `redirect_uri` to send the token to `evil.com`.
    - Try to bypass the `state` check.

## Red Flags - STOP and Follow Process

If you catch yourself thinking:
- "I'll store the Access Token in LocalStorage so it persists." (XSS Vulnerability).
- "I'll just ask for `scope: all` just in case we need it later." (Privacy violation).
- "I'll use the same `state` string for everyone." (CSRF Vulnerability).
- "I'll hardcode the Client Secret in the React app." (Secret Leak).
- "I'll decrypt the token, use it, and leave it in the logs." (Data Leak).
- "If the refresh fails, I'll just crash the app." (Bad UX).
- "I'll merge these two accounts because they have the same email." (Account Takeover Risk).

**ALL of these mean: STOP. Return to Phase 1.**

## Your Human Partner's Signals You're Doing It Wrong

**Watch for these complaints:**
- **User:** "Why is it asking to read my emails? I just wanted to share a video." (Bad Scoping).
- **Dev:** "The token expired and the background job failed." (Missing Refresh Loop).
- **Sec:** "I found your Twitch Client Secret on GitHub." (Failed Secret Management).
- **User:** "I revoked the app in Google Settings, but your app still says connected." (Missing Error Handling).
- **Dev:** "I can't reproduce this login bug." (Missing Audit Logs).

**When you see these:** STOP. Audit the Auth Flow.

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "Implicit Flow is easier" | It is insecure and deprecated by IETF. Use PKCE. |
| "Encryption slows down the DB" | Leaking tokens shuts down the company. |
| "I'll handle token rotation later" | You will be blocked by the API provider quickly. |
| "HTTPS protects the token in URL" | Browser history and server logs still see it. |
| "We trust our database admin" | Defense in Depth. Even admins shouldn't see plaintext tokens. |

## Quick Reference

| Phase | Key Activities | Success Criteria |
|-------|---------------|------------------|
| **1. Architecture** | PKCE, Scopes, Redirects | Secure flow defined |
| **2. Storage** | Encryption, HTTPOnly Cookies | No secrets in frontend/logs |
| **3. Integration** | Refresh Loops, Linking | Seamless multi-API calls |
| **4. Defense** | CSRF, Audit Logs, Scanning | Resilient to attacks |

## When The Provider Changes The Rules

If Google or Twitch changes their auth policies (e.g., "OOB flow deprecation"):

1.  **Subscribe to Security News:** (Google Identity Blog, Twitch Developers).
2.  **Deprecation is a P0:** Drop everything and migrate. Auth breakage means 0 users.
3.  **Graceful Degrade:** If an API feature is removed, disable that specific button, don't crash the whole login.

## Supporting Techniques

- **`superpowers:oauth-debugging`** - Using tools like `jwt.io` and OIDC Debugger.
- **`superpowers:threat-modeling`** - STRIDE model for identity flows.
- **`superpowers:encryption-at-rest`** - Using AWS KMS / Vault for token storage.

## Real-World Impact

- **"Naive" implementation:** Storing tokens in local storage -> XSS attack -> Attacker drains user's bank/Twitch bits.
- **"Secure" implementation:** Tokens are encrypted cookies -> XSS attack fails to steal identity -> User is safe, Platform is trusted.

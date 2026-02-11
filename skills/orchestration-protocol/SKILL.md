---
name: orchestration-protocol
description: Orchestration Workflow Skill
---

## The Orchestration Protocol

**Goal:** Transform a group of experts into a single shipping machine.

### Phase 1: Alignment (The "Why" & "How Much")

**Participants:** Founder, Product Manager (PM), Solution Architect, Engineering Manager (EM).

1.  **The Strategic Handshake (Founder ↔ PM)**
    - **Founder:** Sets the Vision ("We need to capture the Gen-Z market") and Runway constraints ("We have 6 months").
    - **PM:** Translates Vision into concrete Problems ("Gen-Z needs a faster way to share video clips").
    - **Output:** The High-Level Roadmap.

2.  **The Feasibility Check (PM ↔ Architect)**
    - **PM:** "We need live streaming with < 1s latency."
    - **Architect:** "That requires WebRTC, which increases cost by 10x. Is that acceptable?"
    - **Resolution:** Negotiation of Constraints.
    - **Output:** The PRD (Product Requirement Doc) with approved constraints.

3.  **The Capacity Plan (PM ↔ EM)**
    - **PM:** "I need this in Q3."
    - **EM:** "We have 3 devs. We can do it in Q3 if we drop the Chat feature."
    - **Output:** The Resource Plan.

### Phase 2: Definition (The "What" & "How")

**Participants:** PM, UX Designer, Product Designer, UI Designer, Tech Leads (Backend/Mobile).

1.  **The Discovery Loop (PM ↔ UX ↔ Tech Leads)**
    - **UX:** Interviews users, maps the "Happy Path."
    - **Tech Leads:** Validate the data model *during* wireframing. "Do we actually have this data available?"
    - **Output:** Low-Fidelity Wireframes & Data Entity Diagram (ERD).

2.  **The Visual Blueprint (Product/UI Designer ↔ Mobile/Frontend)**
    - **UI:** Applies the Design System.
    - **Dev:** Reviews for "Component Reuse." "If you change this button slightly, it's 3 days of work. Keep it standard, it's 1 hour."
    - **Output:** High-Fidelity Mocks & API Contract (Swagger/OpenAPI).

3.  **The Integration Plan (Architect ↔ Integration/Media Engineers)**
    - **Integration:** "Stripe requires a webhook here."
    - **Media:** "We need to pre-transcode these assets."
    - **Output:** The Technical Design Doc (TDD).

### Phase 3: Execution (The Build)

**Participants:** Developers (Mobile/Backend), Code Reviewers, Integration Engineer, Media Engineer.

1.  **The Parallel Build (Backend ↔ Mobile)**
    - **Backend:** Ships the API Mock or Interface first.
    - **Mobile:** Builds UI against the Mock.
    - **Sync Point:** Integration testing once the real API is live.

2.  **The Specialist Integration (Devs ↔ Middleware/Media)**
    - **Mobile Dev:** "I need to play this video."
    - **Media Engineer:** "Use this HLS URL and this specific ExoPlayer config for DRM."
    - **Backend Dev:** "I need to charge the user."
    - **Integration Engineer:** "Call my internal payment service wrapper, don't call Stripe directly."

3.  **The Quality Gate (Author ↔ Code Reviewer)**
    - **Reviewer:** Checks against the Architecture and Security rules.
    - **Rule:** No code merges without 1 approval.
    - **Output:** Merged Code in the `Staging` branch.

### Phase 4: Delivery (The Launch)

**Participants:** QA Engineer, DevOps, EM, PM, Founder.

1.  **The Verification (QA ↔ PM ↔ Design)**
    - **QA:** Runs the Regression Suite. "The old features still work."
    - **Design:** "Visual QA" check. "The pixels are correct."
    - **PM:** "Acceptance Test." "The problem is solved."
    - **Output:** The Release Candidate (RC).

2.  **The Deployment (DevOps ↔ Devs)**
    - **DevOps:** "Deploying to Prod in 3... 2... 1..." (Using the CI/CD pipeline).
    - **Devs:** Monitoring logs for immediate errors (The "Post-Deploy Watch").
    - **Output:** Live Production Code.

3.  **The Feedback Loop (Founder ↔ PM ↔ Data)**
    - **Founder:** "Did the metric move?"
    - **PM:** Analyzes the data.
    - **Decision:** Pivot, Persevere, or Kill?

---

## Interaction Matrix: Who asks Whom?

| I am a... | I have a question about... | I ask the... |
| :--- | :--- | :--- |
| **Mobile Dev** | The JSON response format | **Backend Engineer** |
| **Mobile Dev** | How the button should behave | **Product Designer** |
| **Backend Dev** | Stripe/Twitch API limits | **Integration Engineer** |
| **Backend Dev** | Video encoding formats | **Media Engineer** |
| **PM** | If a feature is technically possible | **Solution Architect** |
| **PM** | When the feature will be ready | **Engineering Manager** |
| **Founder** | Why the bill went up | **DevOps Engineer** |
| **UX Designer** | If users are clicking the button | **PM (Data)** |
| **QA** | If this behavior is a bug or feature | **PM** |

---

## Conflict Resolution Protocol

**When two roles disagree, use this hierarchy to resolve it:**

1.  **Scope vs. Time:** (PM wants more features, EM wants date).
    * **Winner:** **Founder/Strategy.** Is the deadline hard (holiday launch)? Cut scope. Is the feature critical? Move date.

2.  **User Experience vs. Engineering Cost:** (Designer wants custom animation, Dev wants standard).
    * **Winner:** **PM.** Does this animation drive the core business metric? If yes, spend the money. If no, use standard.

3.  **Speed vs. Stability/Architecture:** (PM wants it now, Architect wants to refactor).
    * **Winner:** **Solution Architect.** You cannot negotiate physics or technical debt. Bad architecture kills the product later.

4.  **Security vs. Features:** (PM wants easy login, Security wants MFA).
    * **Winner:** **Security/Architect.** Security is non-negotiable.

---

## Red Flags - STOP and Re-Align

If you see these behaviors, the Orchestration is broken:
- **"The Handoff Wall":** Designers throwing mocks over a wall to Devs without talking.
- **"The Hidden Refactor":** Devs rewriting code without telling PM (delays release).
- **"The Surprise Bill":** Founder seeing cloud costs only after they hit the credit card.
- **"The HiPPO Override":** Founder overriding the PM/Architect based on opinion, not data.
- **"Integration Hell":** Mobile devs waiting until the last day to test against the real Backend.

**ALL of these mean: Stop. Call an "Alignment Meeting" (Phase 1).**

## Supporting Techniques

- **`superpowers:rfc-process`** - For Technical Decision making.
- **`superpowers:design-handoff`** - For Design -> Dev transfer.
- **`superpowers:post-mortem`** - For learning when the process fails.

##  Modern Collaboration Stack (2026)

### Async (Default)
- **Docs:** Notion, Linear, GitHub
- **Video:** Loom (async recording)
- **Design:** Figma (live collab)

### Sync (Exceptions)
- Zoom, VS Code Live Share, Miro
- **Rule:** Record all meetings

##  Remote Patterns
- **Overlap:** 2-4 hours required
- **Async by default:** Write first
- **Rotate meeting times:** Fair to all zones

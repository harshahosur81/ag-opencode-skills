---
name: program-manager
description: Product Manager Skill
---

## 🔗 Lifecycle Triggers (Orchestration Integration)

**Incoming Dependencies:**
- **From Founder:** Clear "Runway/Budget" and "Vision" constraints.
- **From Data:** Validation of previous features.

**Outgoing Handshakes (The "Kickoff"):**
- **To Engineering:** You must present the PRD and ask: "Is this feasible in the timeframe?"
- **To Design:** You must provide the "Problem Statement," not the solution.

**Definition of Done:**
- **Analytics Check:** Events are firing correctly in the data platform.
- **Security Check:** Deep-dive "fine-toothed comb" code review completed by multiple personas (QA, PE, Designer).
- **GTM Sign-off:** Marketing/Sales have the assets they need.
## The Four Phases

You MUST complete each phase before proceeding to the next.

### Phase 1: Problem Discovery (The "Why")

**BEFORE discussing features or solutions:**

1.  **Define the User Problem**
    - Who is the specific persona?
    - What pain point are they experiencing?
    - **Rule:** If you can't articulate the problem in one sentence without mentioning a "feature," you don't understand it yet.
    - *Bad:* "They need a dashboard."
    - *Good:* "They cannot see their daily spend without exporting to Excel."

2.  **Verify with Data/Insights**
    - **Qualitative:** Do you have 3-5 customer interview notes confirming this pain?
    - **Quantitative:** Does the analytics data support this? (e.g., High drop-off rate, support ticket volume).

3.  **Map the Opportunity**
    - Is this aligned with company goals (OKRs)?
    - Is the market segment large enough to matter?
    - **Action:** If it doesn't move a needle, kill it now.

### Phase 2: Solution Validation (The "What")

**Test the hypothesis cheaply:**

1.  **Divergent Thinking (Brainstorming)**
    - Collaborate with Engineering and Design *now*, not later.
    - "How might we solve this?" (Generate multiple options).
    - Assess Feasibility (Eng) and Usability (Design) early.

2.  **Prototype & Test**
    - Don't build code. Build a mockup, wireframe, or "Painted Door".
    - Put it in front of users.
    - **Success Criteria:** Do they understand it? Do they want it?

3.  **Define Success Metrics (KPIs)**
    - How will we know if this worked *after* launch?
    - Define the Primary Metric (e.g., Conversion Rate) and Counter Metric (e.g., Latency/Uninstalls).
    - **Rule:** If you can't measure it, don't build it.

### Phase 3: Definition & Alignment (The "How")

**Translate value into requirements:**

1.  **Write the PRD / One-Pager**
    - **Context:** Why we are doing this.
    - **User Stories:** "As a [type], I want to [action], so that [benefit]."
    - **Acceptance Criteria:** The definition of done. Be binary (Pass/Fail).
    - **Out of Scope:** Explicitly state what we are NOT building.

2.  **Negotiate the Scope (MVP)**
    - What is the absolute minimum to learn/solve the core problem?
    - Cut the "Nice to haves."
    - **Goal:** Minimize Time-to-Value.

3.  **Stakeholder Buy-in**
    - Review with Engineering Lead (Feasibility check).
    - Review with Design Lead (UX check).
    - Review with Sales/Marketing (Go-To-Market check).
    - **Get explicit "Go" signals.**

### Phase 3.5: AI-Assisted Product Work (2026)

**Leverage AI to amplify your impact:**

1.  **AI for User Research Synthesis**
    - Upload interview transcripts to ChatGPT/Claude for theme extraction
    - "Analyze these 10 user interviews and identify the top 3 pain points"
    - Cross-reference AI findings with manual analysis
    - **Rule:** AI accelerates, but YOU validate the insights

2.  **Automated Competitive Analysis**
    - Use AI web scrapers (Apify + GPT-4) to track competitor features
    - Set up alerts for competitor product updates
    - Generate comparison matrices automatically
    - **Tool:** Perplexity AI for research aggregation

3.  **Data-Driven Prioritization**
    - Feed historical data to AI: "Which features drove retention in past releases?"
    - Predictive analytics for feature impact
    - Automated RICE score calculation from user data
    - **Caution:** AI suggests, YOU decide (business context matters)

4.  **Documentation Assistance**
    - AI-generated first drafts of PRDs
    - Auto-generate user stories from requirements
    - Meeting notes → Action items (Otter.ai, Fireflies)
    - **Rule:** Always review and humanize AI outputs

### Phase 4: Execution & Analysis (The Loop)

**Shepherd the ship:**

1.  **Unblock the Team**
    - Be available for clarification.
    - Make trade-off decisions quickly (Speed > Perfection).
    - Manage "Scope Creep" aggressively (Say "No" or "Next Release").

2.  **Go-To-Market (GTM) Enablement**
    - Train support.
    - Write release notes.
    - Update documentation.
    - Feature flags strategy (Rollout % plan).

3.  **Measure & Learn (Post-Launch)**
    - Look at the KPIs defined in Phase 2.
    - Did we solve the problem?
    - **Decision:** Iterate, Pivot, or Kill?
    - **Crucial:** Share the *outcome* (good or bad) with the team.

## Red Flags - STOP and Follow Process

If you catch yourself thinking:
- "The CEO wants this, just write the ticket."
- "Competitor X has this, so we need it to reach parity."
- "I know what users want, I am a user."
- "We'll figure out the metrics after we launch."
- "Let's just squeeze this extra feature in, it's small."
- "Engineering can figure out the edge cases."
- **Writing JIRA tickets without a clear "Why" or PRD.**
- **Pushing to deployment without a "fine-toothed comb" code review.**

**ALL of these mean: STOP. Return to Phase 1.**

## Your Human Partner's Signals You're Doing It Wrong

**Watch for these complaints:**
- **Engineering:** "Why are we building this?" (You failed Phase 1).
- **Design:** "You're treating me like a pixel pusher." (You skipped Collab in Phase 2).
- **Sales:** "I can't sell this / This isn't what I asked for." (You missed Alignment in Phase 3).
- **Leadership:** "What was the ROI of that release?" (You failed Phase 4 Analysis).
- **Team:** "The requirements keep changing every day." (You failed Phase 3 Definition).

**When you see these:** STOP. Re-align on the problem statement.

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "It's just an MVP, quality doesn't matter" | MVP means "Minimum Viable," not "Broken." |
| "We don't have time for discovery" | You have time to build the wrong thing twice? |
| "I'll update the specs later" | No you won't. Eng is building based on rumors now. |
| "Data takes too long to gather" | Guessing costs more. |
| "Users don't know what they want" | True, but they know their problems. It's your job to find the solution. |
| "We need to launch by [Date]" | Dates are constraints, not requirements. Adjust scope. |

## Quick Reference

| Phase | Key Activities | Success Criteria |
|-------|---------------|------------------|
| **1. Discovery** | Interviews, Data Analysis, Persona | Clear Problem Statement |
| **2. Validation** | Prototyping, Feasibility check | Validated Solution & KPIs |
| **3. Definition** | PRD, User Stories, Scoping | Engineering "Ready" signal |
| **4. Execution** | Unblocking, GTM, Post-Mortem | Outcome achieved (not just output) |

## When The "HiPPO" Attacks

When the **Hi**ghest **P**aid **P**erson's **O**pinion forces a feature without Phase 1/2:

1.  **Do not say "No". Say "Yes, and..."**
2.  "Yes, we can look at that. To prioritize it, which of the current roadmap items should we drop?"
3.  "I'd like to run a quick 2-day test to validate this before we commit 3 months of engineering."
4.  **Document the risk.** If forced to build, ensure the decision trail is clear.

## 🛠️ Modern PM Stack (2026)

### Analytics & Data Tools
- **Product Analytics:** Amplitude, Mixpanel, PostHog
- **Session Replay:** FullStory, LogRocket, Hotjar
- **A/B Testing:** LaunchDarkly, Optimizely, GrowthBook
- **User Feedback:** Dovetail (research), UserTesting, Maze
- **SQL Tools:** Mode Analytics, Metabase, Hex

### AI-Powered PM Tools
- **Research Synthesis:** ChatGPT Code Interpreter, Claude
- **Competitive Intel:** Perplexity, Crayon
- **Documentation:** Notion AI, Gamma (presentations)
- **Prioritization:** ProductBoard, Aha!, Linear

### Communication
- **Async:** Loom (video), Notion, Linear
- **Roadmaps:** ProductBoard, Productplan
- **Prototyping:** Figma, Maze (testing)

## 📊 Metrics Framework

### The Analytics Hierarchy
```
North Star Metric (e.g., Weekly Active Users)
    ↓
L1 Drivers (e.g., Retention Day 7, Feature Adoption)
    ↓
L2 Metrics (e.g., Session Duration, Invite Rate)
    ↓
Counter Metrics (e.g., Load Time, Error Rate)
```

### Essential Dashboards
| Dashboard | Metrics | Cadence |
|-----------|---------|----------|
| **Business Health** | Revenue, Active Users, Churn | Daily |
| **Engagement** | DAU/MAU, Session Length, Stickiness | Daily |
| **Acquisition** | Signups, Conversion Rate, CAC | Weekly |
| **Product Quality** | Crash Rate, Error Rate, Load Time | Daily |
| **Feature Performance** | Adoption, Retention, Impact | Per Release |

## Supporting Techniques

- **`superpowers:user-interviews`** - How to ask questions that don't bias the user.
- **`superpowers:sql-analytics`** - Querying your own data to find the truth.
- **`superpowers:prioritization-frameworks`** - RICE, Kano, or MoSCoW methods.
- **`superpowers:ai-research-synthesis`** - Using AI to process qualitative data at scale.

## Real-World Impact

- **Feature Factory PM:** Ships 10 features/quarter. Usage flat. Team burned out.
- **Product Discovery PM:** Ships 3 features/quarter. Usage up 20%. Team empowered.
- **Outcome:** Engineers love PMs who bring them *problems to solve*, not *solutions to build*.

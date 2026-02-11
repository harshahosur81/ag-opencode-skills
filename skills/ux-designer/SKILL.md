---
name: ux-designer
description: UX Designer Skill 
---

## 🔗 Lifecycle Triggers (Orchestration Integration)

**Incoming Dependencies:**
- **From PM:** "Problem Statement" and "User Persona."
- **From Dev:** "Feasibility Constraint" (e.g., "We can't do blur effects on Android").

**Outgoing Handshakes (The "Handoff"):**
- **To Devs:** You must walk them through the Figma file. A link is not enough.
- **To QA:** You must provide the "Expected Behavior" for edge cases.

**Definition of Done:**
- **VQA (Visual QA):** You have manually reviewed the Staging build and signed off on the pixels.

## The Four Phases

You MUST complete each phase before proceeding to the next.

### Phase 1: Empathy & Definition (The "Who")

**BEFORE opening Figma or Sketch:**

1.  **Immerse in the Problem**
    - Review the PM's brief/PRD.
    - Listen to user interview recordings (don't just read summaries).
    - **Goal:** Understand the *context*, not just the task. "Why are they doing this? Where are they? What are they stressed about?"

2.  **Audit the Current State**
    - Walk through the existing flow yourself.
    - Take screenshots. Note friction points.
    - Look at competitors—not to copy, but to understand mental models users already have.

3.  **Define the "Job to be Done"**
    - Write the core user story: "When [situation], I want to [action], so I can [outcome]."
    - **Rule:** If you don't know *why* the user is clicking the button, you can't design the button.

### Phase 2: Structure & Logic (Low-Fidelity)

**Design the flow, not the pixels:**

1.  **Information Architecture (IA)**
    - Draw a Site Map or User Flow diagram.
    - Organize the data hierarchy. What is the most important piece of info?
    - **Test:** Can you explain the flow using only boxes and arrows?

2.  **Wireframing (The "Skeleton")**
    - Use pen/paper or grayscale blocks only. **No colors. No images.**
    - Focus on layout, spacing, and content placement.
    - **Goal:** Validate the *logic* with the PM and Engineers.
    - *Crucial:* Ask Engineers now: "Is this data structure possible?"

3.  **Edge Case Planning**
    - Don't just design the "Happy Path."
    - Design the "Empty State" (0 items).
    - Design the "Error State" (Failed to load).
    - Design the "Loading State" (Skeleton screens).
    - Design the "Extreme State" (User has 5,000 items or a 50-character name).

### Phase 3: Interaction & Fidelity (High-Fidelity)


**Make it intuitive and accessible:**

1.  **Apply the Design System**
    - Use existing components (Buttons, Inputs) to ensure consistency.
    - **Rule:** Do not invent a new specific shade of blue if one exists.
    - Adhere to the grid system.

2.  **Accessibility (a11y) Check**
    - **Color Contrast:** Does text pass WCAG AA standards?
    - **Touch Targets:** Are buttons at least 44x44px?
    - **Focus States:** What happens when a keyboard user tabs to this element?

3.  **Prototyping**
    - Connect the screens. Make it clickable.
    - Focus on transitions: Does the modal slide up or fade in? (This tells the user where they are).
    - **Goal:** A prototype that feels real enough to test.

### Phase 4: Validation & Handoff

**The job isn't done until it's built:**

1.  **Usability Testing**
    - Put the prototype in front of 5 users.
    - Give them a task ("Find your invoice"). **Do not guide them.**
    - Watch where they get stuck.
    - **If they fail, the design failed.** Go back to Phase 2.

2.  **Dev Handoff (The "Redline")**
    - Annotate everything.
    - "This animation is 300ms ease-out."
    - "This error message comes from API error code 409."
    - "On mobile, this column stacks below that one."
    - **Rule:** A silent handoff is a failed handoff. Walk the devs through it.

3.  **Design QA (VQA)**
    - Once devs build it, review the staging environment.
    - Hunt for "Visual Bugs" (padding issues, wrong fonts).
    - Hunt for "Interaction Bugs" (hover states missing).

### Phase 4.5: Quantitative UX Research (2026)

**Data-driven design validation:**

1.  **Behavioral Analytics**
    - **Heatmaps:** Where users click (Hotjar, Microsoft Clarity)
    - **Session Replay:** Watch real user behavior (FullStory, LogRocket)
    - **Scroll Depth:** How far users read (Google Analytics)
    - **Funnel Analysis:** Where users drop off (Amplitude, Mixpanel)

2.  **A/B Testing Framework**
    - **Hypothesis:** "Changing CTA from blue to green will increase clicks by 10%"
    - **Statistical Significance:** Need 95% confidence, 80% power
    - **Sample Size:** Calculate before starting (not "run for 1 week")
    - **Tools:** LaunchDarkly, Optimizely, PostHog, GrowthBook
    - **Metrics:** Conversion rate, time to task, error rate

3.  **UX Metrics That Matter**
    | Metric | What It Measures | Good Target |
    |--------|-----------------|-------------|
    | **Task Success Rate** | % who complete goal | >80% |
    | **Time on Task** | Seconds to complete | <target time |
    | **Error Rate** | Mistakes per session | <5% |
    | **SUS Score** | System Usability Scale | >68/100 |
    | **NPS** | Net Promoter Score | >30 |

4.  **Continuous Discovery**
    - **Weekly user interviews:** 5 users/week minimum
    - **In-app surveys:** "How would you rate this?" (1-5)
    - **Feature voting:** Let users prioritize roadmap
    - **Support ticket analysis:** Common pain points

## Red Flags - STOP and Follow Process

If you catch yourself thinking:
- "I'll skip wireframing and just start in high-res."
- "I'll make the font light gray because it looks cleaner." (Fails accessibility).
- "I don't need to ask developers, they can build anything."
- "I'll figure out the error state later."
- "Users will know to click that because it looks cool."
- "The text will never be that long."
- **Designing for the "Dribbble Shot" instead of the actual product.**

**ALL of these mean: STOP. Return to Phase 1.**

## Your Human Partner's Signals You're Doing It Wrong

**Watch for these complaints:**
- **Dev:** "I don't know what happens when I click this." (Missing Prototype/States).
- **PM:** "This looks great, but it doesn't solve the core problem." (Failed Phase 1).
- **User:** "Where is the button? I can't read this." (Failed Accessibility).
- **Dev:** "We can't build this custom animation in the timeline." (Failed Feasibility Check).
- **Stakeholder:** "Why is this button round here but square there?" (Inconsistent Design System).

**When you see these:** STOP. Return to the Design System/Wireframes.

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "It looks better without labels" | Icons without labels are ambiguous. Clarity > Beauty. |
| "I'm the expert, I know what works" | You are not the user. Test it. |
| "We'll fix the accessibility later" | Accessibility is structural. Hard to bolt on later. |
| "Lorem Ipsum is fine for now" | Real content breaks designs. Use real data in designs. |
| "I don't have time to prototype" | Explaining static mocks to devs takes 3x longer. |
| "It's just a small UI tweak" | Small tweaks can break user mental models. |

## Quick Reference

| Phase | Key Activities | Success Criteria |
|-------|---------------|------------------|
| **1. Empathy** | User Research, Job Stories | Understanding the "Why" |
| **2. Structure** | Site Maps, Wireframes, Logic | Validated flow (logic works) |
| **3. Fidelity** | UI Design, a11y, Design System | Accessible, consistent visuals |
| **4. Validation** | Usability Test, Handoff Specs | Validated by users, clear for devs |

## When Process Reveals "Feasibility Blockers"

If Engineering says "We can't build this":

1.  **Don't fight blindly.** Ask "Why?" (Performance? Backend limitation? Time?)
2.  **Negotiate the trade-off.** "Can we keep the interaction but simplify the visual?"
3.  **Find the root value.** "The user needs to see X. How can we show X cheaply?"
4.  **Adapt.** Good design embraces constraints.

## Supporting Techniques

- **`superpowers:usability-testing`** - Scripting and running unbiased tests.
- **`superpowers:design-systems`** - Atomic design principles.
- **`superpowers:accessibility-audit`** - Using tools like Stark or Contrast Checker.

## Real-World Impact

- **"Pretty" Designer:** Everything looks great, but users get lost, devs hate the handoff, and metrics don't move.
- **"UX" Designer:** Interfaces might be simple, but tasks are completed 50% faster, accessibility lawsuits are avoided, and devs build exactly what was intended.

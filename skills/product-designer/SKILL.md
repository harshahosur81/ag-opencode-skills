---
name: product-designer
description: Product Designer Skill
---

## The Four Phases

You MUST complete each phase before proceeding to the next.

### Phase 1: Problem Framing (The "Why")

**BEFORE designing pixels:**

1.  **Deconstruct the Request**
    - Don't just take the ticket. Ask: "What business metric are we trying to move?" (Retention? Conversion?)
    - Ask: "Is this a design problem, or a content/marketing problem?"
    - **Deliverable:** A clear problem statement accepted by the PM.

2.  **Low-Fi Exploration**
    - **Sketch first.** Whiteboard or pen/paper.
    - Explore at least 3 distinct approaches (divergent thinking).
    - **Rule:** If you fall in love with your first idea, you are wrong.

3.  **Feasibility Check (The "Gut Check")**
    - Show the rough sketches to an Engineer.
    - "Is this hard to build?" "Do we have this data?"
    - Pivot early if the technical cost is too high for the business value.

### Phase 2: Interaction & Flow (The "How")

**Mapping the journey:**

1.  **Define the Happy Path & The "Unhappy" Path**
    - Design the success state.
    - Design the error states, empty states, and "internet disconnected" states.
    - **Rule:** A design that breaks when data is missing is a broken design.

2.  **Prototype for Reality**
    - Build a clickable prototype in Figma.
    - **Crucial:** Use *real* data lengths (e.g., German text, long names), not perfect Lorem Ipsum.
    - Ensure the navigation makes sense without explanation.

3.  **Usability Validation**
    - Watch a colleague (or user) try to complete the task.
    - Count the clicks. Count the confusion points.
    - **Iterate:** Fix the friction before polishing the visuals.

### Phase 3: Visual Polish (The "What")

**Applying the system:**

1.  **System Compliance**
    - Use the Design System components. 90% of the UI should be "Lego blocks" from the library.
    - Only break the system if the value is immense.
    - Check accessibility (Contrast, Touch targets).

2.  **Visual Hierarchy**
    - Does the primary button look different from the secondary button?
    - Is the most important data point the boldest/largest?
    - **The Squint Test:** Blur your eyes. The call-to-action must still be visible.

3.  **Dev Readiness**
    - Auto-layout everything. (Devs need to know how it stretches).
    - Name your layers.
    - Define responsive breakpoints (Mobile vs Desktop).

### Phase 4: Delivery & QA (The "Build")

**You own the quality until shipping:**

1.  **The Handoff Walkthrough**
    - Don't just send a link. Walk the dev through the logic.
    - Explain the "Why" so they can make micro-decisions correctly.
    - Document the edge cases in the Figma file.
    - **Participate in 'fine-toothed comb' code reviews to ensure design intent matches implementation exactly.**

2.  **Design QA (VQA)**
    - Test the staging build.
    - **Bug Triage:** Log visual bugs.
    - Be pragmatic: Differentiate between "Blocker" (Unusable) and "Polish" (2px off).

3.  **Post-Launch Review**
    - Look at the metrics. Did users actually click it?
    - If not, was the design wrong, or was the problem framing wrong?

## Red Flags - STOP and Follow Process

If you catch yourself thinking:
- "I'll skip the wireframes and just do high-fidelity."
- "The devs will figure out the mobile view."
- "I don't have time to test this."
- "This animation looks cool, let's force it in."
- "I'll just invent a new font size."
- "The stakeholders liked it, so it's good." (Stakeholders aren't users).
- **Skipping the 'fine-toothed comb' pre-deployment code review.**

**ALL of these mean: STOP. Return to Phase 1.**

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "It's just an MVP" | MVP means "valuable," not "ugly and broken." |
| "I'll clean up the file later" | You won't. The dev will implement your mess. |
| "Real data looks ugly" | Then your design is fragile. Design for reality. |
| "It's obvious how to use it" | It's obvious to *you*. You created it. |

## Quick Reference

| Phase | Key Activities | Success Criteria |
|-------|---------------|------------------|
| **1. Framing** | Sketching, Feasibility | Problem defined, Eng approved |
| **2. Flow** | Prototyping, Usability | Flow validated by humans |
| **3. Polish** | UI System, Accessibility | Production-ready visuals |
| **4. Delivery** | Handoff, VQA | Build matches Design |

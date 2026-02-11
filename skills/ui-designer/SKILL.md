---
name: ui-designer
description: UI Designer Skill 
---

## The Four Phases

You MUST complete each phase before proceeding to the next.

### Phase 1: The Design System (The "Palette")

**BEFORE designing a single screen:**

1.  **Audit the Atoms**
    - Do not invent new colors. Use the defined palette (Primary, Secondary, Neutral, Semantic/Error).
    - Do not pick arbitrary font sizes. Use the Type Scale (H1, H2, Body, Caption).
    - **Rule:** If you are using a hex code or font size that isn't in the library, you are creating technical debt.

2.  **Establish the Grid**
    - Define the underlying structure (e.g., 8pt/4pt grid).
    - **Spacing is not random.** Margins and padding should be multiples of the base unit (8, 16, 24, 32px).
    - Define breakpoints for responsiveness (Mobile, Tablet, Desktop).

3.  **Check for Existing Components**
    - Need a dropdown? Check the library.
    - Need a card? Check the library.
    - **Rule:** Reuse > Modify > Create. Only build a new component if the existing one fundamentally fails the use case.

### Phase 2: Visual Hierarchy & Composition

**Guide the user's eye:**

1.  **The "Squint Test"**
    - Squint at your design (or blur it). What stands out?
    - The most important element (Primary Action) must carry the most visual weight.
    - If everything is bold, nothing is bold.

2.  **Contrast & Accessibility**
    - **Text Contrast:** Check WCAG AA compliance (4.5:1 ratio).
    - **Color Independence:** Don't use color alone to convey meaning (e.g., Error state needs red color + icon/text).
    - **Focus States:** Design the blue ring/outline for keyboard users.

3.  **White Space (Negative Space)**
    - Use space to group related items (Law of Proximity).
    - Use space to separate distinct sections.
    - **Rule:** Clutter increases cognitive load. When in doubt, add more padding.

### Phase 3: Interactivity & States (The "Behavior")

**The interface is not a static poster:**

1.  **Define All States**
    - **Default:** How it looks initially.
    - **Hover:** How it invites interaction (desktop).
    - **Pressed/Active:** The tactile feedback.
    - **Disabled:** Why is it disabled? (Tooltip?).
    - **Loading:** Spinner or Skeleton?
    - **Error:** How does it scream "Fix me"?

2.  **Micro-Interactions & Motion**
    - Animations must have purpose (guiding the eye), not just flair.
    - **Duration:** 200ms-300ms is the sweet spot.
    - **Easing:** Use `ease-out` for entering elements, `ease-in` for exiting.

3.  **Responsive Adaptation**
    - How does this 3-column grid stack on mobile?
    - Do tables turn into cards? Does the menu become a hamburger?
    - **Mock it up.** Don't make the developer guess.

### Phase 3.5: Modern CSS Capabilities (2026)

**New CSS superpowers:**

1.  **Container Queries (Responsive Components)**
    ```css
    .card-container {
      container-type: inline-size;
    }
    @container (min-width: 400px) {
      .card { grid-template-columns: 1fr 2fr; }
    }
    ```
    - **Why:** Responsive based on parent, not viewport
    - **Use Case:** Card component that adapts to sidebar vs main content

2.  **:has() Selector (Parent Selectors)**
    ```css
    /* Style card differently if it has an image */
    .card:has(img) { padding-top: 0; }
    ```
    - **Why:** Previously impossible without JavaScript
    - **Support:** Safari 15.4+, Chrome 105+, Firefox 121+

3.  **CSS Layers (@layer)**
    ```css
    @layer reset, base, components, utilities;
    @layer components { .button { ... } }
    ```
    - **Why:** Control cascade order explicitly
    - **Use Case:** Prevent utility classes from being overridden

4.  **Variable Fonts**
    - **Single file, multiple styles:** 100-900 weight, normal-italic
    - **Performance:** One request vs 10 font files
    - **Animation:** Smoothly animate font-weight
    - **Tools:** Google Fonts (many now variable), v-fonts.com

### Phase 4: Handoff & Visual QA (The "Polish")

**Pixel perfection in code, not just Figma:**

1.  **Asset Preparation**
    - Export SVGs for icons (scalable).
    - Optimize images (WebP/PNG).
    - Name your layers/assets logically (`icon/user-profile` not `Vector 14 copy`).

2.  **Redlining / Spec-ing**
    - If you use Figma Dev Mode, ensure variables are mapped correctly.
    - Annotate specific behaviors ("Sticky header stops here").
    - Annotate responsive rules ("On mobile, hide this image").

3.  **Visual QA (VQA)**
    - Sit with the developer.
    - Compare the build to the design.
    - Check for "Layout Shift" (things jumping around).
    - **Rule:** You own the visual quality of the final product, not the developer.

## Red Flags - STOP and Follow Process

If you catch yourself thinking:
- "I'll just eyeball the padding, it looks fine."
- "This light grey text looks elegant." (It's unreadable).
- "I don't need to design the hover state, they know standard behavior."
- "I'll make this button slightly different to make it pop." (Inconsistency).
- "I'm bored of this layout, let's change the style." (Brand fragmentation).
- "I'll use Lorem Ipsum." (Real content will break your layout).
- **Designing "Mobile" last.**

**ALL of these mean: STOP. Return to Phase 1.**

## Your Human Partner's Signals You're Doing It Wrong

**Watch for these complaints:**
- **Dev:** "I don't have this font weight in the system." (You broke the system).
- **UX:** "The user didn't see the error message." (Bad hierarchy/contrast).
- **Dev:** "Is this padding 15px or 16px?" (Inconsistent spacing).
- **PM:** "The text gets cut off in German." (You didn't test content length).
- **User:** "I didn't know I could click that." (Weak affordance).

**When you see these:** STOP. Audit your hierarchy and system usage.

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "Strict grids stifle creativity" | Grids enable consistency. Creativity lives within constraints. |
| "Developers should know to stack it" | They might, but they might stack it wrong. Define it. |
| "It looks good on my retina screen" | Most users have cheap monitors with bad contrast. |
| "I'll clean up the layer names later" | Messy files lead to messy code and wrong assets. |
| "Accessibility ruins the aesthetic" | Inaccessible design is broken design. |
| "Motion is just extra polish" | Motion conveys state change. It is functional. |

## Quick Reference

| Phase | Key Activities | Success Criteria |
|-------|---------------|------------------|
| **1. System** | Tokens, Grid, Type Scale | Consistent foundation |
| **2. Hierarchy** | Contrast, Spacing, Layout | Clear "Squint Test" results |
| **3. States** | Hover, Error, Loading, Mobile | Complete component lifecycle |
| **4. Handoff** | Specs, VQA, Assets | Build matches Design |

## When The "Brand" Clashes with "Usability"

When Marketing wants a "Branded Experience" that hurts UI patterns:

1.  **Prioritize Clarity:** If the brand font is illegible at small sizes, use the system font for UI/Body text and brand font for Headings only.
2.  **Test it:** Show the "on-brand" button vs the "accessible" button to users.
3.  **Compromise:** Use brand colors for delight/accents, not for core navigation if they lack contrast.

## Supporting Techniques

- **`superpowers:design-tokens`** - Mapping hex codes to semantic names (`primary-action-bg`).
- **`superpowers:atomic-design`** - Building from Atoms -> Molecules -> Organisms.
- **`superpowers:responsive-layouts`** - Understanding Flexbox and Grid limitations.

## Real-World Impact

- **"Artist" UI:** Looks great on Dribbble, falls apart with real data, confuses users, expensive to build/refactor.
- **"System" UI:** Scales infinitely, developers build 2x faster (reusable components), users feel "at home" instantly due to consistency.

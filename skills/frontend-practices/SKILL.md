---
name: frontend-practices
description: Engineering-robustness standards for user interfaces — load when designing, building, or changing frontend/UI. Covers responsive layouts, no unintended overflow, real-viewport handling, all UI states, accessibility, and performance. Complements the code constitution and any aesthetic design guidance.
---

# Frontend Practices

The frontend layer of the constitution — what UI code must get right beyond general quality. Apply alongside `engineering-standards` (Gate A/B). This is about *robustness*, not aesthetics; visual direction is a separate concern.

## Responsive

- Media queries are the primary tool for responsive behavior — explicit and predictable, and easier to reason about and test than computed fluid values. Use fluid sizing to complement them where it genuinely helps, not as a replacement.
- Leave no gaps: the breakpoints together must cover the whole size range with no band where the layout breaks. Verify across the range, not at a couple of points (see `verify`).
- Design any UI with mobile, landscape and portrait monitors, and laptops of varying screen sizes in mind. UIs should be robust and account for things like browser top bars or scroll bars eating up a bit of space.

## No unintended overflow

- Content fits, or scrolls by *deliberate* design — never overflows or scrolls by accident. When space is tight, compact rather than clip or spill.
- Confirm against a range of viewport sizes (see `verify`), including the smallest realistic ones.

## Every state, not just the happy path

- Handle loading, empty, error, and success states, and make sure each fits and reads clearly. Dynamic content (a notice appearing, a list growing) must not break the layout.

## Accessibility

- Semantic HTML first; ARIA only to fill gaps. Announce dynamic changes with `aria-live`.
- Keyboard operable, visible focus states, sufficient color contrast.
- Decorative elements `aria-hidden`; every control has an accessible name.

## Fit the design system

- Reuse existing tokens, components, and patterns (spacing, color, type scale, radii) rather than one-offs. New UI should look and behave like it belongs.

## Performance

- Avoid layout thrash and needless re-renders; lazy-load heavy assets and routes; keep the critical bundle small. Measure, don't guess.

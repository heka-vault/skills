---
name: aaa-redesign
description: >-
  Redesign an application's whole UI/UX to a polished, cohesive, production
  standard: audit every screen, benchmark against best-in-class apps (Mobbin),
  redesign with at most 4 parallel implementation agents and 1 shared design
  reviewer, then one critical review and one final validation. Use when the
  user asks to redesign, restyle, polish or "make it look great" across an app,
  not for a single component tweak.
---

# AAA redesign

A bounded, whole-app redesign: one audit, one benchmark, one redesign pass per
workstream, one critical review, one final validation. No perfection loops.

## The brief

The brief is the one part of this skill that changes from app to app. Take it
from the user's message. If they gave none, use the default below and say so.

> **Default brief:** Redesign my application's UI/UX to achieve AAA-quality
> design: minimalist, elegant, sharp, cohesive, and visually exceptional.

A good brief names the look and feel the user wants (for example "dense and
data-heavy like Linear", "warm and playful", "match our brand colors in
`docs/brand.md`"), the audience, and anything off limits. Everything below
stays the same whatever the brief says.

## Before you start

- **Mobbin MCP** for benchmarking against real apps. If it isn't connected, say
  so and benchmark from your own knowledge of best-in-class apps instead.
- **`/design:design-critique`** (the `design` plugin) for structured critique.
  If it isn't installed, run the same critique by hand: hierarchy, typography,
  spacing, color, consistency, usability, polish.
- A way to see the running app (a browser or preview tool) so you can take
  screenshots before and after.

## Process

1. **Audit:** Identify all existing screens and key flows. Capture screenshots
   of the current UI.
2. **Benchmark:** Use Mobbin (MCP) and `/design:design-critique` to evaluate the
   current design against best-in-class applications.
3. **Redesign:** Implement improvements across the application, prioritizing
   visual hierarchy, typography, spacing, consistency, usability, and polish.
4. **Review:** Conduct one critical design review and address the most
   significant issues.
5. **Validate:** Perform one final holistic review to ensure visual consistency
   and functional integrity across the application.

## Execution rules

- Use a maximum of **4 parallel implementation subagents** and **1 shared
  design reviewer**.
- Group related screens/features into workstreams rather than assigning an
  agent per screen.
- Limit each workstream to **one implementation pass and one refinement pass**.
- Reuse existing components and establish shared design patterns to maintain
  consistency.
- Preserve existing functionality and avoid unnecessary architectural changes.
- Cover all existing screens and key user flows; do not expand scope into
  speculative future features.
- Prioritize meaningful improvements over endless perfection loops.

Settle the shared patterns (type scale, spacing scale, color tokens, core
components) before the workstreams start, so four agents don't invent four
design systems.

## Goal

Deliver a polished, cohesive, production-quality application with minimal
agent overhead and no unnecessary iteration. Finish with before and after
screenshots of the key screens and a short list of what changed.

# AAA redesign: copy-paste prompt

For agents without the skill installed. Paste everything below the line into
your agent. **Change the first paragraph (the brief) to describe the design you
want.** Leave the Process and Execution Rules as they are.

---

Redesign my application's UI/UX to achieve AAA-quality design: minimalist, elegant, sharp, cohesive, and visually exceptional.

### Process

1. **Audit:** Identify all existing screens and key flows. Capture screenshots of the current UI.
2. **Benchmark:** Use Mobbin (MCP) and `/design:design-critique` to evaluate the current design against best-in-class applications.
3. **Redesign:** Implement improvements across the application, prioritizing visual hierarchy, typography, spacing, consistency, usability, and polish.
4. **Review:** Conduct one critical design review and address the most significant issues.
5. **Validate:** Perform one final holistic review to ensure visual consistency and functional integrity across the application.

### Execution Rules

- Use a maximum of **4 parallel implementation subagents** and **1 shared design reviewer**.
- Group related screens/features into workstreams rather than assigning an agent per screen.
- Limit each workstream to **one implementation pass and one refinement pass**.
- Reuse existing components and establish shared design patterns to maintain consistency.
- Preserve existing functionality and avoid unnecessary architectural changes.
- Cover all existing screens and key user flows; do not expand scope into speculative future features.
- Prioritize meaningful improvements over endless perfection loops.

**Goal:** Deliver a polished, cohesive, production-quality application with minimal agent overhead and no unnecessary iteration.

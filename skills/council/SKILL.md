---
name: council
description: >-
  Convene a council: three delegates, one flagship model from each of three AI
  providers, independently review or give feedback on the current work, score
  it, and re-review after fixes. Use when the user says "council".
---

# Council

A council is three delegates, each a flagship model from a different provider,
giving independent, read-only reviews of the same work. You are the chair: you
brief them, merge what they say, apply what you agree with, and defend scope.

## Seats

This table is the part to edit for your own setup. Everything below it stays
the same.

| Seat | Default model | Reached through | Budget |
| --- | --- | --- | --- |
| Claude | latest Claude Opus | Claude (Claude Code) | every round |
| GPT | latest GPT flagship, e.g. GPT Sol | Codex | every round |
| Gemini | latest Gemini Pro | Cursor (**metered**) | round 1 and the sign-off round |

- **Route each seat through its own row.** Several providers offer the same
  model (Cursor also offers Claude and GPT; Codex also offers Claude). Taking
  a model through a metered provider when its own provider is available spends
  quota for nothing.
- **A metered seat runs on a capped subscription.** It sits in at most two
  rounds per council: round 1, where a third independent read is worth the
  most, and the sign-off round. It skips the rounds that re-check fixes in
  between. Spend more of it only when the user asks.
- **Members the user names replace the defaults.** Keep the budget column for
  any named member reached through a metered provider.
- Run every seat at high reasoning effort. The option is named differently per
  provider (`effort`, `reasoningEffort`, `reasoning_effort`); read the names
  from the provider's capabilities.
- **An unavailable seat stays empty.** If a model or provider is missing, or a
  metered seat's quota is spent, say so and run the council with the seats you
  have. Fill a seat with a different model only when the user names it.

## Before round 1

Bring the work up to date: address outstanding PR comments, fix known issues,
and run the relevant checks. The council reviews the current implementation;
known fixes land before the review, not after it. If the user wants to discard
the implementation and explore new ideas, skip fixes to code being replaced.

## Rounds

A round is one full review pass by every seat budgeted for it. A fresh review
after fixes starts a new round.

1. **Start the round's seats in parallel.** In T3 Code, call
   `orchestrator_capabilities` once to resolve current model ids and options,
   then start one `delegate_task` per seat (`role: "review"`). Elsewhere, use
   the harness's subagent tools.
2. **Give every seat the same brief**: the request, the goal, where the work
   is (diff range, files, PR), and what is out of scope. Leave out your own
   opinion and other seats' findings so each review is independent. For
   review or feedback, the work is read-only: no edits, commits or comments.
   For a code review, ask for a score out of 10 and findings ranked by
   severity, each marked blocking or not.
3. **Merge the responses**: useful findings, agreements, disagreements, and
   recommended next steps.
4. **Fix what you agree with.** Decline findings outside the request's scope
   and say why; repeated reviews pull toward scope creep, and holding the line
   is your job.
5. **Decide the next round.** When your fixes leave nothing blocking open, the
   next round is the **sign-off round**: every seat, including the metered one.
   Otherwise run a re-check round with the unmetered seats.

The council ends when every seat in the sign-off round is satisfied: no
blocking findings. Three rounds is the cap. If seats are still unsatisfied
after round 3, stop and hand the user the open disagreements to decide.

## Results

If the work ends in a pull request, put the results in the PR description,
before the model and harness line at the end:

```markdown
## Council review

Rounds: 3

| Delegate | Round 1 | Round 2 | Round 3 |
| --- | --- | --- | --- |
| Claude Opus | 7/10 | 8/10 | 9/10 |
| GPT Sol | 8/10 | 9/10 | 10/10 |
| Gemini Pro | 7/10 | skipped | 9/10 |
```

Write `skipped` for a round a seat sat out by budget, `no score` when a seat
reviewed but gave no score, and `unavailable` when the seat could not run.
Every cell is a real result from that seat.

If the PR is open, edit its description; the results go there, not in a
comment. If there is no PR, put the same results in the final reply.

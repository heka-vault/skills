---
name: change-metrics
description: >-
  Measure a pull request before anyone reviews it: how big it is, where it
  lands, what it touches that the title does not say, its merge risk, and how
  deep a review it needs. Rates each group of a grouped or stacked pull request
  as well as the whole. Use when opening or reviewing a large change, before a
  code review, or when the user asks for merge risk, review depth or change
  metrics.
metadata:
  credits:
    skill: merge-risk
    author: Warp
    url: "https://github.com/warpdotdev/common-skills/blob/8792394ff1b655e0af451b8fabb68ef0cfcbd208/.agents/skills/merge-risk/SKILL.md"
---

# Change metrics

You are writing for the next reader of this change: a person or an agent about
to review it. Before they read a line of code they should know four things:
how big the change is, what it touches that is not obvious from the title, how
likely it is to hurt, and how hard they need to look. Your job is to lower
their cognitive load. Put numbers and verdicts first, use plain words and the
repository's own terms, and leave nothing for them to decode.

Run it before a code review, yours or someone else's, so the depth of the
review is a choice and not a habit. Put the result in the pull request
description of any change bigger than a handful of files.

## Your setup

These two lists are the part to edit for your own codebase. Everything below
them stays the same.

**Sensitive areas.** A change here raises Impact and calls for a deep review
of those files, whatever the size of the diff: authentication, permissions,
payments, personal data, secrets, data migrations and schema, dependencies
(vendored code and third-party agent skills included), build steps and their
config, CI and deploy files, infrastructure and runtime config, and anything
that decides what users see by default (feature flags, defaults, routing).

**Generated files.** Count these apart from the change and never as review
work: lockfiles, snapshots, build output, recorded fixtures. Vendored code is
not generated: someone chose it, so it counts, under Sensitive areas.

## 1. Find the groups

We group work. One pull request holds change sets of a similar kind: the same
area, the same functionality, the same type of implementation. A schema change
and its migration are one kind; a refactor is another; a pass over the UI is a
third. Several groups can ride together in one pull request, or stack as one
pull request per layer.

Name the groups before you measure anything:

- Use the stack's layers, or the commits and their messages, when they already
  mark the groups.
- Otherwise group by area and type of change, and say that you did.
- A file that belongs to no group, or a group that does not belong in this pull
  request, is a finding under Cohesion.

A pull request with one kind of change is one group. Do not split it to fill
the table.

## 2. Measure

Measurements are facts from the diff, not judgment. Take them per group and
for the whole pull request, against the merge base
(`git diff --numstat <base>...<head>`, `git log <base>..<head>`):

- **Size:** files changed, lines added and removed. Leave out generated files
  and list them on their own line.
- **Areas:** the top-level modules, packages or apps touched.
- **Mix:** lines in production code, tests, docs and config.
- **Surface:** public interfaces added or changed, such as exported functions,
  routes, CLI flags, events, schema, environment variables and config keys.
- **Sensitive areas** touched, from your setup.
- **Dependencies** added, removed or bumped.
- **Checks:** which ran on this change and their results. Write "not run" for
  any you could not find a result for.

Never estimate a number. If you cannot measure something, say so.

## 3. Rate

Rate each group and the whole pull request on six categories. Under each
category, list the gaps that raise its risk: short bullets, with links to the
files.

- **Cohesion (mixed ⚠️ / mostly 👀 / one kind ✅):** each group holds one kind
  of change, and the groups belong together in this pull request. A drive-by
  fix in another area, or a refactor mixed into a feature, makes every file
  harder to review.
- **Alignment (poor ⚠️ / decent 👀 / good ✅):** the code does what the
  description and the source issue say. Assumptions about behavior and any
  drive-by changes are written down.
- **Impact (large ⚠️ / medium 👀 / small ✅):** the scope is contained and away
  from sensitive areas. Consider the blast radius: what breaks for consumers,
  what shifts in the layout, what changes on mobile, what another service
  depends on. Flag every sensitive area touched, and call out anything that
  widens access, weakens validation or logs sensitive data.
- **Verification (poor ⚠️ / decent 👀 / good ✅):** behavior is proven. A change
  people see needs screenshots or a recording, before and after. A change that
  could affect performance needs a measurement. Edge cases are tested, or
  written down when they are not; link the code when only the note exists.
- **Rollback cost (high ⚠️ / medium 👀 / low ✅):** the change can be undone
  after it merges. A two-way door can be walked back through. A one-way door,
  such as a destructive migration, deleted data or a published interface
  others now depend on, cannot.
- **Error detection (poor ⚠️ / decent 👀 / good ✅ / not applicable ✅):** if
  this breaks in production, someone finds out before a user reports it. Look
  for swallowed errors and fallbacks that hide a failure.

Score each verdict ⚠️ = 3, 👀 = 2, ✅ = 1, and weight it:

| Category | Weight |
| --- | --- |
| Impact | 2 |
| Verification | 2 |
| Rollback cost | 1.5 |
| Cohesion | 1 |
| Alignment | 1 |
| Error detection | 1 |

Risk is the weighted average: low below 1.5, medium from 1.5 to below 2.25,
high from 2.25. The whole pull request is never rated lower than its riskiest
group. Keep the arithmetic to yourself; the reader needs the verdicts.

## 4. Choose the review depth

The point of the numbers is to tell the reviewer how to spend their attention.
Choose a depth for each group. This is where grouping pays: a pull request
with one risky group and two simple ones gets one deep review and two skims,
not three deep ones.

| Depth | When | What the reviewer does |
| --- | --- | --- |
| **Skim** | Low risk | Read the description, these metrics and the tests. Spot-check the code. |
| **Read** | Medium risk | Read every changed production file once. Run it or look at the evidence. |
| **Deep** | High risk | Read line by line, reproduce the verification, and get a second reviewer. |

Two rules override the score:

- A sensitive area or a one-way door makes the review **deep for those files**,
  even when the rest of the change is a skim.
- Say where the time goes. Name the files to review first and why, so the
  reviewer starts with the part that can hurt.

An agent reviewing the change uses the same depth: skim is a quick pass for
the few findings it is most sure of, deep is its most thorough review of the
named files.

## Output

Lead with one line, then the table, then the categories. Write the categories
once, for the whole pull request, and mark each gap with its group. Fit an
ordinary pull request on one screen. No preamble and no summary of the code:
the reviewer is about to read it.

```markdown
**Merge risk: high · Review depth: deep on group 1, skim group 2**

| Group | Files | Lines | Areas | Risk | Depth |
| --- | --- | --- | --- | --- | --- |
| 1. Add the invoices table | 4 | +180 −2 | db, api | high | deep |
| 2. Invoice list page | 9 | +410 −120 | web | low | skim |
| **Whole pull request** | **13** | **+590 −122** | **3** | **high** | |

Not counted: lockfile (+1,204 −310), 2 snapshots.

**Review first**
- `db/migrations/0042_invoices.sql`: a one-way door; it drops `legacy_totals`.
- `api/routes/invoices.ts`: a new public route with no rate limit.

**Cohesion:** one kind ✅

**Alignment:** good ✅

**Impact:** large ⚠️
- Group 1 touches a data migration and adds a public route.

**Verification:** decent 👀
- Group 2: the list page has before and after screenshots; the empty state
  has none.

**Rollback cost:** high ⚠️
- Group 1: the migration drops a column. Reverting the code does not bring
  it back.

**Error detection:** good ✅
```

The example is illustrative. Every number you write comes from the diff.

For a stacked pull request, put each layer's own block in its description and
the whole stack's table in the top layer. When the change is pushed again,
measure again and replace the block; do not append a second one.

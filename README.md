# heka skills

Agent skills from [heka](https://hekafactory.com). Each skill is a folder under
[`skills/`](skills/) with a `SKILL.md`, in the open
[Agent Skills](https://agentskills.io) format, so they work in Claude Code,
Cursor, Codex and anything else that reads that format.

## Skills

| Skill | What it does |
|-------|--------------|
| [`aaa-redesign`](skills/aaa-redesign/SKILL.md) | Redesigns a whole app's UI/UX to a polished, cohesive standard with a bounded process: audit, benchmark (Mobbin), redesign with at most 4 parallel agents and 1 reviewer, one review, one validation. |
| [`council`](skills/council/SKILL.md) | Reviews work with a council of three flagship models from three providers: independent read-only reviews, scores out of 10, fix and re-review rounds (capped at 3), results in the PR description. Rations a seat on a metered subscription. |
| [`change-metrics`](skills/change-metrics/SKILL.md) | Measures a pull request before anyone reviews it: size, areas, sensitive paths, merge risk and how deep a review each group needs. Built for grouped and stacked pull requests. |

## Install

With the [skills CLI](https://skills.sh):

```bash
npx skills add heka-vault/skills
```

Or copy a skill folder by hand into your agent's skills directory:

| Agent | Personal | Per project |
|-------|----------|-------------|
| Claude Code | `~/.claude/skills/` | `.claude/skills/` |
| Cursor | `~/.cursor/skills/` | `.cursor/skills/` or `.agents/skills/` |
| Codex | `~/.agents/skills/` | `.agents/skills/` |

```bash
git clone https://github.com/heka-vault/skills.git
cp -r skills/skills/aaa-redesign ~/.claude/skills/
```

## Using `aaa-redesign`

**Change the brief.** The skill ships with a default brief ("minimalist,
elegant, sharp, cohesive, and visually exceptional"). That line is the part you
are meant to replace with the design you actually want. The process and
execution rules stay the same.

With the skill installed, put your brief in the message:

> Use aaa-redesign. Brief: a calm, editorial look for a reading app; generous
> whitespace, serif headings, one accent color from `src/theme.ts`. Mobile
> first.

Without the skill, paste [`PROMPT.md`](skills/aaa-redesign/PROMPT.md) into your
agent and edit its first paragraph.

It works best with the Mobbin MCP server connected and the `design` plugin
installed (for `/design:design-critique`). Without them the agent falls back to
its own knowledge and a manual critique.

## Using `council`

**Change the seats.** The `Seats` table at the top of the skill says which
model sits in each seat, which provider reaches it, and how often it runs. The
defaults are Claude Opus (Claude Code), the GPT flagship (Codex) and Gemini Pro
(through Cursor). The Gemini seat is marked metered, so it sits in only round 1
and the sign-off round. Edit the table to match the providers you pay for.

Say "council" in a message. Name members to override the table for that run:

> council: review this PR with Claude Opus, GPT Sol and Grok.

It is written for [T3 Code](https://github.com/pingdotgg/t3code)'s
`delegate_task`, and falls back to any harness's subagent tools.

## Using `change-metrics`

**Change the lists.** The `Your setup` section at the top of the skill lists
the sensitive areas (a change there always gets a deep review) and the
generated files (counted apart, never as review work). Edit both to match your
codebase.

Ask for it before a review, or when you open a pull request:

> change-metrics on this branch, then put the result in the PR description.

It reads the groups in a pull request from its commits or its stack, rates
each group and the whole, and tells the reviewer where to look first. It is
our version of Warp's
[`merge-risk`](https://github.com/warpdotdev/common-skills/blob/main/.agents/skills/merge-risk/SKILL.md),
which builds on Matt Pocock's
[`pr`](https://github.com/mattpocock/skills/blob/main/skills/engineering/pr/SKILL.md)
skill. We added grouping, the measurements and the review depth.

## License

[MIT](LICENSE)

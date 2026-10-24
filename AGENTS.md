# heka skills

Public repo of agent skills (open Agent Skills format). No code, no build.

## Layout

- `skills/<name>/SKILL.md` is the skill: YAML frontmatter with `name` (matches
  the folder) and `description` (what it does and when to use it), then the
  instructions.
- Extra files a skill needs (a copy-paste prompt, `scripts/`, `references/`)
  live in the same folder.

## Adding a skill

1. Create `skills/<name>/SKILL.md`. Keep it agent-neutral: no paths, hosts or
   secrets from any one machine; this repo is public.
2. If the skill has a part users are expected to change (like the brief in
   `aaa-redesign`), say so plainly in the skill and in the README.
3. Add a row to the Skills table in `README.md`.

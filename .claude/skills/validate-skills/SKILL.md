---
name: validate-skills
description: Validate this repo's Agent Skills (.claude/skills/*/SKILL.md) against the Agent Skills specification using the skills-ref reference CLI. Use after creating or editing any skill, or when asked to verify, lint, or check the skills.
---

# Validate skills

Run `skills-ref validate` (the reference validator from
https://github.com/agentskills/agentskills, `skills-ref/` directory) against
the skills in `.claude/skills/`. It checks that each `SKILL.md` has valid
frontmatter and follows the spec's naming rules (lowercase/digits/hyphens
`name`, max 64 chars, matches the directory name, `description` 1-1024
chars). It does not test whether a skill works well.

Arguments (`$ARGUMENTS`, optional): one or more skill directories to check. If
omitted, check every directory in `.claude/skills/`.

## Steps

1. **Find an existing install.** If `skills-ref` is on `PATH`, or a previous
   clone exists at `/tmp/agentskills-*/repo/skills-ref/.venv/bin/skills-ref`,
   use that and skip to step 3.
2. **Otherwise install it outside this repo.** Tell the user what you are about
   to do, since this downloads and installs third-party code, then:
   - `D=$(mktemp -d /tmp/agentskills-XXXXXX)`
   - `git clone --depth 1 https://github.com/agentskills/agentskills "$D/repo"`
   - `cd "$D/repo/skills-ref" && uv sync` (requires `uv`)

   Run these as separate commands. If a command is denied by the permission
   classifier, do not work around it: stop and give the user the exact command
   to run themselves with the `!` prefix.
3. **Validate each skill directory**, one at a time, so a failure in one does
   not hide the others:
   `<path-to>/skills-ref validate .claude/skills/<name>`
4. **Report** one line per skill (valid / the error message). Do not edit any
   skill file to fix a failure unless the user asks.

## Notes

- Never install into this repo; keep clones in a fresh `/tmp` directory.
- Other subcommands exist: `skills-ref read-properties <skill>` (frontmatter as
  JSON) and `skills-ref to-prompt <skill>...` (`<available_skills>` XML).
- The upstream README says the library is for demonstration purposes only.

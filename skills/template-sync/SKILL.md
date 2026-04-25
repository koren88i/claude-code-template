---
name: template-sync
description: Use when syncing a project's `CLAUDE.md` or `.claude/skills/` with the upstream `claude-code-template` repo, either by pulling template updates into a project or contributing general improvements back upstream.
---

# Template Sync

## Goal
Keep a project's skills and CLAUDE.md aligned with the upstream template, and contribute improvements back when they are generalizable.

## Use this when
- The template repo has updated or added skills you want to pull into this project
- You've improved a skill or CLAUDE.md section in this project and want to contribute it back
- A skill listed under "Skills to add" in CLAUDE.md has now been written and should be pushed upstream

---

## File path mapping

The template repo and a project that uses it store files in different locations:

| Template repo | Project using the template |
|---|---|
| `CLAUDE.template.md` | `CLAUDE.md` (project root) |
| `skills/<name>/SKILL.md` | `.claude/skills/<name>/SKILL.md` |

Keep this mapping in mind when copying in either direction.

---

## Direction 1: Pull updates from template → this project

Use this when the template repo has improvements you want to bring in.

### 1) Add the template as a remote (once per project)
```bash
git remote add template https://github.com/koren88i/claude-code-template.git
git fetch template
```

### 2) See what has changed
```bash
# Compare skills
git diff HEAD template/master -- skills/

# Compare the base template file
git diff HEAD template/master -- CLAUDE.template.md
```

### 3) Pull in specific files selectively
Never blindly merge — the project has customizations that must be preserved.

To pull a specific skill:
```bash
git checkout template/master -- skills/<name>/SKILL.md
# then move it to the correct project path:
# cp skills/<name>/SKILL.md .claude/skills/<name>/SKILL.md
```

To review CLAUDE.template.md changes and apply only the relevant parts manually:
```bash
git show template/master:CLAUDE.template.md
```
Compare section by section. Copy improvements into `CLAUDE.md` without overwriting project-specific content (mission, tech stack, constraints, anti-patterns, git scopes).

### 4) Review before committing
- Confirm no project-specific content was overwritten
- Confirm the updated skill still makes sense in this project's context
- Run any relevant verification if the skill change affects active workflows

### 5) Commit
```bash
git add .claude/skills/<name>/SKILL.md
git commit -m "chore: pull <name> skill update from upstream template"
```

---

## Direction 2: Contribute improvements from this project → template

Use this when you've refined a skill or CLAUDE.md section and the improvement is generalizable — not specific to this project.

### 1) Identify what is generalizable
Ask:
- Does this change depend on project-specific knowledge (internal hostnames, domain rules, team conventions)?
- Would it make the template better for any project, or just this one?

If the answer is "any project" — it belongs in the template. If it's project-specific — it belongs only in `CLAUDE.md` or the local skill copy.

### 2) Strip project-specific content
Before contributing back, remove or replace:
- Internal service names, hostnames, URLs
- Business domain language specific to this project
- Examples that only make sense in this codebase

Replace with generic equivalents or placeholder examples.

### 3) Apply the change to the template repo

If you have write access to the template repo:
```bash
cd /path/to/claude-code-template
git checkout -b improve/<skill-name>
# copy the cleaned-up file into the template's skills/ folder
cp /path/to/project/.claude/skills/<name>/SKILL.md skills/<name>/SKILL.md
git add skills/<name>/SKILL.md
git commit -m "feat(skills): <description of improvement>"
git push origin improve/<skill-name>
# open a PR or merge directly if appropriate
```

If you do not have write access:
- Fork the template repo, apply the change, and open a pull request.
- In the PR body: describe what the improvement is, what prompted it, and what was stripped to make it generic.

### 4) Update the template's CLAUDE.template.md if needed
If the improvement affects the skills index or adds a new skill, also update:
- The `## Skills available` section in `CLAUDE.template.md`
- Remove the skill from `## Skills to add` if it was listed there

---

## Guardrails
- Never overwrite `CLAUDE.md` wholesale from `CLAUDE.template.md` — the project sections (mission, stack, constraints, anti-patterns) will be lost.
- Never contribute project-specific content to the template — sanitize first.
- Pull selectively by file, not by branch merge.
- When in doubt about whether something is generalizable, leave it in the project and note it for later review.

# claude-code-template

A `CLAUDE.md` template and skill library for [Claude Code](https://claude.ai/code) projects. Copy `CLAUDE.template.md` to your repo root as `CLAUDE.md`, copy `skills/` to `.claude/skills/`, fill in the project-specific sections, and Claude Code will pick it up automatically at the start of every session.

## Skills included

| Skill | Purpose |
|---|---|
| `bug-investigation` | Disciplined bug workflow: understand → reproduce → isolate → protect → fix → scan for related instances → verify → capture lesson |
| `refactor-safely` | Cleanup only when behavior is already protected by tests |
| `deliverable-verification` | Checklist for shipping: scope, verification path, evidence, known gaps |
| `docker` | Docker and Compose best practices: ports, resource limits, health checks, Dockerfile rules |
| `session-close` | End-of-session ritual: mark progress, capture lessons, clean git state |
| `template-sync` | Pull skill updates from this repo into a project, or contribute improvements back |

## Possible future work

- **`IMPORTANT:` / `YOU MUST` escalation** — add explicit all-caps markers to the highest-priority rules (design constraints, hard-coding policy) to signal to Claude that violations are not acceptable under any framing of the request.
- **Specificity guidelines** — add a note to the fill-in sections nudging toward concrete rules over principles. "Name booleans with `is_` / `has_` prefix" works better than "use clear names."
- **"Where things live" section** — a small addition to the architecture section (or its own section) listing canonical file paths for key types of code. Consistently one of the highest-signal things in well-regarded CLAUDE.md examples.
- **Global vs project CLAUDE.md split** — Claude Code supports a global `~/.claude/CLAUDE.md` that applies across all projects. The engineering mindset and default working style sections are good candidates to move there, keeping each project's CLAUDE.md leaner and more focused on project-specific context.

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

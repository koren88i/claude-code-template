# claude-code-template

A `CLAUDE.md` template and skill library for [Claude Code](https://claude.ai/code) projects. Copy `CLAUDE.template.md` to your repo root as `CLAUDE.md`, copy `skills/` to `.claude/skills/`, fill in the project-specific sections, and Claude Code will pick it up automatically at the start of every session. For brand-new repos, the `project-intake` skill can bootstrap the critical sections first instead of forcing you to complete the whole file up front.

## What Is This

Wearing my “mid-level developer” hat, I’m sharing my claude.md with you.

It’s the result of 6–7 small projects I ran and learned from, plus some research mode work and my own reading of various posts on the topic.

It includes all the standard headings that are worth having, along with a few things that helped me (some only in hindsight) make the project development smoother and better aligned with what matters to me.

Here are some of the “maybe unique” aspects:

- Background about the environment in which the project will live (e.g., on-prem), who the target audience is, and who will maintain it.
- Thinking and working like an “engineer” — e.g., “what happens when you add another instance…”.
- The whole part about forcing it to break down large tasks into smaller steps, and working with a plan that preserves state between runs and updates the .md, etc.
- The debugging skill helps a lot (not jumping straight to writing code — first explain the bug, then create a failing test, and only then fix it, and extract insights into the .md).
- Refactoring skills also help — not just “here, I wrote it” 🙂


## Skills included

| Skill | Purpose |
|---|---|
| `project-intake` | Bootstrap a new project's `CLAUDE.md`: inspect the repo first, ask only the highest-value questions, and make the critical sections actionable without blocking on every TODO |
| `bug-investigation` | Disciplined bug workflow: understand → reproduce → isolate → protect → fix → scan for related instances → verify → capture lesson |
| `refactor-safely` | Cleanup only when behavior is already protected by tests |
| `deliverable-verification` | Checklist for shipping: scope, verification path, evidence, known gaps |
| `docker` | Docker and Compose best practices: ports, resource limits, health checks, Dockerfile rules |
| `session-close` | End-of-session ritual: mark progress, capture lessons, clean git state |
| `template-sync` | Promote reusable `CLAUDE.md` and skill improvements from a real project back into this template repo |

## Possible future work

- **`IMPORTANT:` / `YOU MUST` escalation** — add explicit all-caps markers to the highest-priority rules (design constraints, hard-coding policy) to signal to Claude that violations are not acceptable under any framing of the request.
- **Specificity guidelines** — add a note to the fill-in sections nudging toward concrete rules over principles. "Name booleans with `is_` / `has_` prefix" works better than "use clear names."
- **"Where things live" section** — a small addition to the architecture section (or its own section) listing canonical file paths for key types of code. Consistently one of the highest-signal things in well-regarded CLAUDE.md examples.
- **Global vs project CLAUDE.md split** — Claude Code supports a global `~/.claude/CLAUDE.md` that applies across all projects. The engineering mindset and default working style sections are good candidates to move there, keeping each project's CLAUDE.md leaner and more focused on project-specific context.

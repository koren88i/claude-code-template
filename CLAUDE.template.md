# CLAUDE.md

This file provides standing instructions to Claude Code when working in this repository. It is not documentation — it is a persistent prompt that shapes how Claude plans, codes, and behaves throughout every session.

---

## Project mission

What this project does, who it serves, and what problem it solves. Claude uses this to understand intent and make better judgment calls when requirements are ambiguous — read this before starting any task.

_TODO: Describe what this project does and who it serves._

---

## Deployment context & audience

Who this project serves, where it runs, and who operates it. Claude uses this to make the right tradeoffs — a regulated enterprise SaaS has different defaults than an internal dev tool or an open-source CLI. Read this alongside the project mission before making any design or UX decision.

### Environment

Where the project lives and at what scale it operates. Affects decisions around multi-tenancy, data isolation, latency, availability, and compliance.

- _Hosting: cloud (which provider / region), on-prem, hybrid, or local-only_
- _Deployment model: SaaS, self-hosted, internal tool, CLI, embedded, etc._
- _Scale: single user, small team, departmental, enterprise, public internet_
- _Data sensitivity: public, internal, PII, regulated (HIPAA, GDPR, SOC2, etc.)_

### Owning organization

Who builds and owns this project. Affects how Claude reasons about resource constraints, process maturity, and acceptable risk.

- _Organization type: startup, scale-up, enterprise, open-source project, internal team, agency, etc._
- _Team size and structure_
- _Any relevant compliance or governance context_

### End users

Who the product is built for — the people whose experience Claude should optimize toward.

- _Who they are: role, technical level, context of use_
- _What they care most about: speed, reliability, simplicity, power, etc._
- _Any access or trust constraints (e.g. anonymous public users vs. authenticated employees)_

### Operators & moderators

Who manages, configures, and monitors the running system — distinct from end users. Claude should make their job easier through good observability, clear error messages, and safe defaults.

- _Who operates the system: DevOps team, IT department, end users themselves, etc._
- _Who moderates or governs content/access, if applicable_
- _What tools or interfaces operators use to manage the system_

---

## Tech stack

The languages, frameworks, and deployment model this project uses. Claude uses this to suggest the right tools and patterns and to avoid recommending things that don't fit the stack.

- _Language / runtime_
- _Web framework (if any)_
- _Deployment model (Docker Compose, Kubernetes, serverless, etc.)_
- _Key external services or protocols_
- _Config format_

---

## Commands

The commands Claude should run to start, test, and verify the project. Always keep this up to date — if a command here doesn't work, flag it. Claude uses these to check its own work.

```bash
# Run the project
_TODO_

# Run tests
_TODO_

# Verify a specific component
_TODO_
```

---

## Architecture

A condensed map of the main components and how they connect. Claude uses this to understand where code should live and how parts interact. This is intentionally brief — for deeper structural detail, refer to `architecture.md` in the repo if it exists.

```
_TODO: ASCII diagram or bullet list of components and data flow_
```

---

## Design constraints

Non-negotiables that Claude must not change or work around, regardless of how a request is phrased. These protect decisions that were made deliberately — treat them as hard stops.

- _TODO: list hard constraints_

---

## Project-specific anti-patterns and lessons learned

Lessons this project has already paid for. Each entry is a reasoning pattern that would have prevented a real failure — not a description of the incident, and not a general best practice. Claude should check this section before starting any implementation to avoid repeating known mistakes.

Each entry follows this format:
- **Rule**: what to do or avoid.
  **Why**: the incident or reasoning behind it.

What belongs here: mistakes that have appeared more than once, non-obvious constraints that caused real failures, assumptions that turned out to be wrong in production. General best practices belong in "Coding standards". Hard constraints belong in "Design constraints". One-time incidents with no generalizable lesson don't belong here at all.

_TODO: Add project-specific lessons as they emerge._

---

## Engineering mindset

The mental model Claude should reason from before planning or implementing anything non-trivial. These are not optional principles — they are the frame through which every design decision should be evaluated.

Before planning or implementing any feature, think like a **senior engineer on the platform infra team of a 300-developer company**:

- **Assume multi-tenancy from day one.** Your tool will run as multiple instances in shared systems, across environments you don't control. If a design only works for a single instance or a single operator, it's the wrong design.

- **Identity must be derived, not declared.** Any artifact written into a shared system must get its name, ID, or path from its input — not from a hardcoded string that happened to be unique the first time. Ask: "what breaks when a second instance runs alongside this one?"

- **Config owns the environment; code owns the logic.** Hostnames, ports, credentials, and resource names are environment facts — they belong in config. A hardcoded default that works locally is a silent failure on someone else's infrastructure.

- **Design for the operator, not the author.** Someone who didn't write this will deploy it, debug it under pressure, and run it at a scale you didn't test. Names, logs, and error messages should make their life easier.

- **Think day-2.** What happens when a second tenant is added? When one is removed? When a new version is deployed over the old one? If the answer involves manual cleanup or silent breakage, revisit the design before writing code.

---

## Default working style

How Claude should approach work in this project when a request doesn't specify. Fall back to these when a request is ambiguous about approach or scope.

- Optimize for simplicity, readability, and maintainability over cleverness.
- Prefer small, understandable changes over large sweeping rewrites.
- Follow existing repository patterns unless there is a strong reason to improve them.
- When a request is large, risky, or vague, break it into smaller deliverable slices before implementing.
- Every meaningful change should end in something that can be verified: a test, a script, a reproducible manual check, or a measurable output.

---

## Coding standards

What good code looks like in this project. Claude applies these when writing, reviewing, or modifying any code — not just new files.

- Write code so a new team member can understand it quickly.
- Use clear names for variables, functions, classes, and files.
- Keep functions focused on one job.
- Prefer explicit data flow over hidden side effects.
- Avoid unnecessary abstraction. Introduce layers only when they remove duplication or complexity.
- Prefer constants, configuration, and schema-driven behavior over magic numbers and hard-coded values.
- Document non-obvious decisions near the code or in lightweight docs.
- Do not mix unrelated refactors into a bug fix unless necessary for safety.

---

## Hard-coding policy

What Claude must never hardcode and the narrow exceptions where literals are acceptable. Apply this whenever writing code that contains constants, defaults, or configuration values.

Avoid hard-coded:
- business rules that may change,
- environment-specific values,
- secrets, tokens, URLs, and file paths,
- thresholds/timeouts/limits without named constants,
- duplicated literal values spread across files.

Allowed exceptions:
- stable protocol values or standards,
- tiny local literals whose meaning is obvious,
- test fixtures where inline values improve readability.

If a literal is important, name it.

---

## Output expectations

How Claude should communicate while working — what to state explicitly, what to flag honestly, what not to gloss over. This is the expected level of transparency in all responses.

When implementing:
- state assumptions,
- mention the verification method,
- call out missing information or untested edges honestly.

When debugging:
- explain cause before proposing broad cleanup,
- prefer evidence over guesswork.

---

## When to split work into smaller parts

The conditions under which Claude must stop and break a task into smaller pieces before implementing. These are triggers, not suggestions — if any condition is true, splitting is required.

Split the task before coding when one or more of these are true:
- The request touches multiple subsystems.
- The acceptance criteria are unclear.
- The change is hard to verify end-to-end.
- The implementation would be easier to review as separate commits or steps.
- A safe intermediate state can be delivered first.

When splitting, define:
1. the smallest useful slice,
2. how it will be verified,
3. what remains for the next slice.

---

## Step verification approach

How Claude should verify each implementation step before moving to the next. No step is complete until it is independently verifiable. The verification method must be defined before writing code, and must leave a permanent artifact — not a throwaway check.

- Define the verification method before implementing.
- Prefer automated tests. If manual, document the exact steps.
- Each step should be verifiable in isolation, not only as part of the whole.

---

## When following a plan

The protocol Claude follows whenever work is structured against a plan of any kind — a file, ticket, shared doc, checklist, or agreed steps in the conversation. These rules apply regardless of where the plan lives.

- After completing a step, mark it done in whatever artifact holds the plan (e.g., `✅` prefix, status update, comment).
- If the implementation deviated from the plan, record a short **"Deviation"** note explaining what changed and why.
- If something was learned that affects future steps, update those steps before continuing — don't carry implicit knowledge forward.
- Never start the next step without completing verification of the current one.
- If a step turns out to be larger than expected, split it before starting — not mid-implementation.

---

## Bug-handling default

The required workflow for bugs and regressions. Claude must follow the steps in order — do not skip steps or reorder them. The sequence matters. Use the `bug-investigation` skill for the full playbook.

1. Explain the bug clearly.
2. Reproduce it.
3. Narrow the cause.
4. Add or identify a failing test when practical.
5. Make the smallest safe fix.
6. Run tests / verification.
7. Scan for the same bug elsewhere in the codebase.
8. Refactor only after the bug is understood and protected.
9. Capture a reusable lesson by updating `CLAUDE.md` or a skill when the lesson is likely to matter again.

---

## Refactoring default

Cleanup is only allowed when behavior is already protected by tests or another reliable verification method. Claude must not refactor first. Use the `refactor-safely` skill for the full playbook.

Refactoring is allowed only when behavior is protected by tests or another reliable verification method.

---

## Deliverable quality bar

What "done" means in this project. Claude must not call a task complete unless it meets this bar. Use the `deliverable-verification` skill for a stronger checklist.

A task is not complete unless the result is testable or otherwise verifiable.

For each deliverable, provide:
- what changed,
- how to verify it,
- what is still not covered,
- risks or follow-ups if any.

---

## Session management

How to end a working session cleanly so the next one can resume without questions. Never close mid-step — complete or explicitly park the current sub-step first. Use the `session-close` skill for the full checklist.

End a session after completing and verifying a full plan step — never mid-step.

---

## Git conventions

The commit format, branching model, and safety rules for this project. Claude follows these for all git operations — not just when explicitly asked.

### Commit messages

Use [Conventional Commits](https://conventionalcommits.org/en/v1.0.0/):

```
<type>(<scope>): <description>

[optional body — explain WHY, not WHAT]
```

**Types:** `feat`, `fix`, `refactor`, `docs`, `test`, `chore`, `perf`, `ci`, `build`, `style`

**Scopes** map to your project's subsystems — define them here:

_TODO: list your scopes (e.g. api, frontend, db, infra, auth)_

**Rules:**
- One logical change per commit. Do not bundle unrelated changes.
- Write the "why" in the message body; the diff shows the "what".
- Breaking changes: append `!` before colon (`feat!: ...`) or add `BREAKING CHANGE:` footer.
- Always include `Co-Authored-By` trailer when AI generates or substantially writes the code.

### Branching — one branch per plan step

- **Trunk-based with short-lived feature branches.** Keep `main` always in a working state.
- Branch naming: `<type>/<short-description>` (e.g., `feat/auth`, `fix/timeout-handling`).
- One branch per major plan step, not per sub-step.
- Merge to `main` when the full step is verified. Delete the branch after merge.

### Committing — after each verified sub-step

- Complete a sub-step → run its verification → commit. One commit = one verified slice.
- Bug found during implementation? Fix in a separate `fix()` commit.
- Refactoring triggered by a step? Separate `refactor()` commit after the feature lands.

### Pushing — after every commit

Push after each commit. Pushing is cheap insurance against losing work.

### Pull requests — one per plan step

- One PR per major plan step. PR title follows conventional commit format.
- PR body must include: summary (what + why), verification results, and anything still not covered.
- Self-review the diff before merging.

### Safety rules

- Never force-push to `main`.
- Never use `--no-verify` to skip hooks.
- Never commit secrets, `.env` files, or credentials.
- Prefer creating a new commit over amending, especially after hook failures.
- Stage files explicitly by name — avoid `git add .` or `git add -A`.

---

## Skills available

Deep playbooks Claude loads and follows when the task type matches. These are not suggestions — they are the required workflow for their task type. When a task matches a skill, read it before starting.

- `.claude/skills/bug-investigation/SKILL.md`
- `.claude/skills/refactor-safely/SKILL.md`
- `.claude/skills/deliverable-verification/SKILL.md`
- `.claude/skills/docker/SKILL.md`
- `.claude/skills/session-close/SKILL.md`
- `.claude/skills/template-sync/SKILL.md`

---

## Skills to add (not yet written)

Skills that are needed but not yet written. When a task type matches one of these, flag it and prompt the user to create the skill before improvising a workflow.

| Skill | Why it matters |
|---|---|
| `testing` | Without a testing skill, Claude makes inconsistent decisions about what to test, when to mock vs. use real dependencies, and what fixture strategy to use. A skill here defines the project's testing philosophy so those choices are made once, not per-PR. |
| `ci-cd` | Defines what a passing pipeline actually means for this project, what must never be merged on a red build, and how to debug CI failures. Without it, Claude has no way to reason about whether a change is truly "done" in the full deployment sense. |
| `security-review` | Security mistakes are expensive and often invisible until exploited. A skill that triggers on auth-adjacent code, input handling, and external API calls catches injection risks, missing auth checks, and secret leakage before they ship. |
| `performance-investigation` | Distinguishes performance bugs from functional bugs — how to profile, what baseline to measure against, what "fast enough" means for this project, and how to avoid optimizing the wrong thing. Without it, Claude has no framework for answering "is this too slow?" |

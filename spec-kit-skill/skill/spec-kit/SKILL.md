---
name: spec-kit
description: Use for spec-driven development, bug diagnosis and fixes, and idea assessment. Create traceable specs, plans, tasks, verification reports, and decisions without requiring the Spec Kit CLI.
---

# Spec Kit (Claude edition)

Run a disciplined, artifact-first version of GitHub Spec Kit inside Claude and Claude Code. Use Claude's available file and coding capabilities rather than assuming a specific CLI, output directory, or presentation tool.

## Route the request

Choose exactly one process:
- Build, add, or create a feature/app -> Spec-Driven Development (SDD)
- Something is broken, crashes, regressed, or behaves incorrectly -> Bug fixing
- Decide whether an idea is worth building -> Idea assessment
- If the user names a stage, perform that stage only after checking prerequisites.

Read only the selected reference:
- SDD -> `references/sdd.md`
- Bug fixing -> `references/bugfix.md`
- Idea assessment -> `references/assess.md`

## Core rules

1. **One stage at a time with a review gate.** Produce one artifact, summarize it briefly, then wait before the next unless the user explicitly asks to run/chain the workflow.
2. **What/why before how.** Specs describe user value, behavior, and measurable outcomes. Technical choices belong in the plan.
3. **Do not guess material facts.** Mark important unknowns as `[NEEDS CLARIFICATION: ...]`. Make minor assumptions explicit.
4. **Artifacts are the source of truth.** Later stages read earlier artifacts from the project/workspace. User-edited artifacts always win.
5. **Traceability.** Requirements use IDs such as `FR-001` and `SC-001`; tasks reference user stories such as `[US1]`; verification traces back to requirements or symptoms.
6. **No fake success.** Never call a bug fixed or a workflow converged without verification evidence.
7. **Match the user's language and skill level.** Explain unfamiliar SDD terms briefly when needed.

## Artifact layout

Use the current project/workspace as the primary location:

```
.specify/memory/constitution.md
specs/<NNN-feature-slug>/spec.md
specs/<NNN-feature-slug>/plan.md
specs/<NNN-feature-slug>/research.md
specs/<NNN-feature-slug>/data-model.md
specs/<NNN-feature-slug>/contracts/
specs/<NNN-feature-slug>/quickstart.md
specs/<NNN-feature-slug>/tasks.md
.specify/bugs/<slug>/
.specify/assessments/<slug>/
```

Create or update real markdown files in the current project/workspace when Claude has file access. Do not depend on a hard-coded `/mnt/user-data/outputs/` path or a tool named `present_files`.

## Existing projects and implementation

Before creating an artifact, inspect relevant existing files. Do not overwrite user changes blindly.

For coding stages:
- Follow tasks in dependency/phase order and respect `[P]`.
- Mark tasks complete only after the change and checks succeed.
- Prefer focused changes over unrelated refactors.
- Run available tests/checks when execution is available.
- Report commands/checks actually run and their real results.
- If execution is unavailable, mark verification as not run.

If the project is unavailable, state what is missing and do not invent project-specific facts.

## Reference files

Detailed stage rules and templates:
- `references/sdd.md`
- `references/bugfix.md`
- `references/assess.md`

Load only the reference required for the selected process.

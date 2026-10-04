---
name: spec-kit
description: Run GitHub Spec Kit's structured processes directly in chat, producing markdown artifacts (constitution.md, spec.md, plan.md, tasks.md, bug assessments, idea assessments) without installing the specify CLI. Covers three independent processes - Spec-Driven Development (constitution, specify, clarify, plan, tasks, analyze, implement, converge), bug fixing (assess, fix, test with a verified/partial/failed verdict), and idea assessment (intake, research, define, shape, decide with a go/needs-clarification/kill decision). Use this skill whenever the user mentions spec-kit, speckit, spec-driven development, SDD, "write a spec before coding", PRD to plan to tasks, a feature spec, a technical plan, a task breakdown, "diagnose then fix this bug properly", or "is this idea worth building", even if they never name Spec Kit. Also use it when the user wants to turn a rough feature or app idea into spec.md, plan.md and tasks.md files.
---

# Spec Kit (chat edition)

Spec Kit (github.com/github/spec-kit) gives AI coding agents structured, gated processes with documented outcomes. This skill runs those processes inside a chat: no CLI install, the same stage order, the same artifact names, delivered as downloadable markdown files.

There are three **independent entry points**, not three mandatory phases:

| User needs | Process | Ends with | Reference |
|---|---|---|---|
| Build a feature or app | Spec-Driven Development (SDD) | spec, plan, tasks, implementation, convergence | `references/sdd.md` |
| Fix broken behavior | Bug fixing | assessed cause, scoped fix, recorded verification | `references/bugfix.md` |
| Decide if an idea deserves investment | Idea assessment | go / needs-clarification / kill | `references/assess.md` |

## Step 0: Route

Pick the process from the request, then read ONLY that reference file.
- "Build / add / create a feature or app" -> SDD
- "X is broken / crashes / regression" -> bug fixing
- "Should I build this / is this worth it / validate this idea" -> assessment
- Unclear -> ask one short question. An assessment `go` can hand off to SDD; stopping with a documented reason is also a valid result.
- If the user names a stage ("write the plan", "just tasks"), do that stage. Check the prerequisite artifacts exist (in the conversation or uploads); if missing, say so and offer to create them first rather than silently inventing them.

## Core rules (apply to every process)

1. **One stage at a time, with a review gate.** Produce one artifact, summarize it in 3-6 lines, and stop for the user's review before the next stage. Do not chain stages unless the user says "run it all" or "lean path".
2. **What/why before how.** Specs contain user value, behavior, and measurable outcomes, never tech stack. Tech choices belong to the plan.
3. **Mark unknowns, don't guess.** Use `[NEEDS CLARIFICATION: <specific question>]` inline for material gaps (keep to at most 3 in a first draft; make informed assumptions for minor ones and list them under Assumptions). Never fabricate requirements, metrics, or evidence.
4. **Artifacts are the source of truth.** Later stages read earlier artifacts, not memory of the chat. If the user edits an artifact, the edited version wins. Refine existing artifacts in place instead of regenerating whole stages.
5. **Traceability.** Requirements get IDs (FR-001, SC-001); tasks reference user stories (US1); verification references the original symptom or requirement.
6. **Missing verification is not success.** Never report "done/fixed" without stating how it was checked.
7. **Match the user's language and skill level.** Explain terms like "constitution" or "convergence" in one line the first time if the user seems new to SDD.

## File layout (mirrors upstream)

```
.specify/memory/constitution.md          # once per project
specs/<NNN-feature-slug>/spec.md         # SDD per feature
specs/<NNN-feature-slug>/plan.md  (+ research.md, data-model.md, contracts/, quickstart.md)
specs/<NNN-feature-slug>/tasks.md
.specify/bugs/<slug>/                    # bug fixing reports
.specify/assessments/<slug>/             # idea assessment artifacts
```

Slugs are short kebab-case (`login-crash`, `offline-mode`). Feature folders are numbered (`001-photo-organizer`).

## Delivering output

- Write each artifact as a real `.md` file under `/mnt/user-data/outputs/` using that layout, then call `present_files` so the user can download it. Don't paste the whole file into chat as well; give the short summary and the open questions.
- For small edits to an earlier artifact, edit the file and re-present it.
- If the user has the project in their own agent (Copilot, Claude Code, etc.), mention once that these files drop straight into a Spec Kit project, and the real CLI is `uv tool install specify-cli` then `specify init <project> --integration <agent>`. Don't push it.
- Coding stages (`implement`, bug `fix`): in chat, produce the code as files, task by task, ticking tasks off in `tasks.md`. If the user has a real repo they will apply it themselves, give the diff or files plus the exact tasks completed.

## Fidelity note

This is a chat adaptation of Spec Kit's published process and templates; stage names and artifact names follow the upstream repo, but wording of upstream templates may differ and the repo evolves. If the user needs the exact current templates, point them to the `templates/` folder of the repo.

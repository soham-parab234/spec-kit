# 🌱 spec-kit-skill

> A Claude Skill that runs [GitHub Spec Kit](https://github.com/github/spec-kit)'s structured processes **directly in chat**: no CLI install, no setup. Claude produces the markdown artifacts (`spec.md`, `plan.md`, `tasks.md`, bug reports, idea assessments) and hands them back as files.

Made by **Soham**. Not affiliated with or endorsed by GitHub. See [Credits](#credits).

---

## What it does

Spec Kit gives AI coding agents *gated, documented* workflows instead of "vibe coding". This skill teaches Claude those workflows. It covers three **independent** processes:

| You need to... | Process | Stages | Ends with |
|---|---|---|---|
| Build a feature or app | **Spec-Driven Development** | constitution → specify → clarify → plan → tasks → analyze → implement → converge | A spec carried through plan, tasks and verified code |
| Fix broken behavior | **Bug fixing** | assess → fix → test | Verdict: `verified` / `partial` / `failed` |
| Decide if an idea is worth building | **Idea assessment** | intake → research → define → shape → decide | Decision: `go` / `needs-clarification` / `kill` |

### Rules Claude follows when the skill is active

- **One stage at a time**, with a review gate before the next.
- **What/why before how**: specs never contain tech stack; that's the plan's job.
- **No guessing**: unknowns are marked `[NEEDS CLARIFICATION: ...]`.
- **Traceability**: requirements get IDs (`FR-001`, `SC-001`), tasks link to user stories (`T012 [P] [US1]`).
- **No fake success**: a bug is never "fixed" without recorded verification.

---

## Install

### Claude.ai / Claude app
1. Download [`spec-kit.skill`](./spec-kit.skill) from this repo.
2. Open it in a Claude chat; the file card shows a **Save skill** button (if your organization allows skill creation), or upload it from your skills settings.
3. Once saved, the skill **activates automatically** when your request matches. No slash command needed.

### Claude Code
Copy the skill folder into your skills directory:

```bash
# personal (all projects)
cp -r skill/spec-kit ~/.claude/skills/spec-kit

# or per project
cp -r skill/spec-kit .claude/skills/spec-kit
```

---

## Usage

Just talk to Claude naturally. Examples that trigger the skill:

```text
I want to build a habit tracker with streaks and daily reminders. Write the spec first.
```
```text
Here's the spec. Now write the technical plan using Next.js and SQLite.
```
```text
Break the plan into tasks.
```
```text
Submitting an empty password crashes the login form. Assess it properly before fixing.
```
```text
Is "offline mode with sync" worth building? Run an idea assessment.
```

Claude produces one artifact, summarizes it, lists open questions, and waits for you before moving on. Say "run it all" if you want it to chain stages.

### Files it creates (mirrors upstream Spec Kit)

```text
.specify/memory/constitution.md
specs/001-habit-tracker/spec.md
specs/001-habit-tracker/plan.md      (+ research.md, data-model.md, contracts/, quickstart.md)
specs/001-habit-tracker/tasks.md
.specify/bugs/<slug>/                (assessment, fix, verification)
.specify/assessments/<slug>/         (intake, research, define, shape, decision)
```

These drop straight into a real Spec Kit project if you later use the CLI.

---

## Repo layout

```text
spec-kit-skill/
├── README.md
├── LICENSE
├── spec-kit.skill            # packaged skill, ready to install
├── skill/spec-kit/
│   ├── SKILL.md              # triggers, routing, core rules
│   └── references/
│       ├── sdd.md            # Spec-Driven Development stages + templates
│       ├── bugfix.md         # assess → fix → test
│       └── assess.md         # idea assessment
└── examples/
    ├── habit-tracker/spec.md       # sample "specify" output
    └── login-crash/assessment.md   # sample bug assessment
```

---

## Examples

- [`examples/habit-tracker/spec.md`](./examples/habit-tracker/spec.md): prioritized, independently testable user stories, FR/SC IDs, edge cases, and clarification markers, with no tech stack.
- [`examples/login-crash/assessment.md`](./examples/login-crash/assessment.md): with no code provided, Claude ranks candidate causes and marks the root cause *undetermined* instead of inventing a fix.

---

## Limitations

- This is a **chat adaptation** of Spec Kit's published process. Stage and artifact names follow the upstream repo, but template wording may differ, and upstream evolves. For the exact current templates see [`github/spec-kit/templates`](https://github.com/github/spec-kit/tree/main/templates).
- Only the *specify* and bug *assess* stages have been tested so far. Plan, tasks, analyze and converge are written from the upstream README and documentation and are still to be exercised end to end.
- Claude can't run your repo. For `implement`/`fix` it produces files or diffs for you to apply; `converge` needs you to provide the code.

## Roadmap

- [ ] Test the full specify → plan → tasks → implement → converge chain
- [ ] Add a constitution example
- [ ] Add trigger-description evals

---

## Credits

Based on the process and templates of **[GitHub Spec Kit](https://github.com/github/spec-kit)** (MIT licensed). All credit for the methodology goes to the Spec Kit authors. This repository only packages that workflow as a Claude Skill.

## License

[MIT](./LICENSE) © 2026 Soham

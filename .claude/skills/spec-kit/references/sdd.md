# Spec-Driven Development (SDD)

Define **what and why** before **how**. Flow: **constitution once per project; specify -> plan -> tasks -> implement -> converge per feature.** Optional quality gates: clarify (before plan), analyze (after tasks), checklist (any time).

Lean path for experiments: specify -> plan -> tasks -> implement. Use clarify/checklist/analyze for production-grade features.

## Contents
1. constitution
2. specify
3. clarify (optional gate)
4. plan
5. tasks
6. analyze (optional gate)
7. checklist (optional)
8. implement
9. converge

---

## 1. constitution -> `.specify/memory/constitution.md`
Once per project. Ask what principles matter (code quality, testing, performance, UX consistency, security, simplicity) or take them from the user's prompt. Write 3-7 principles, each with: name, a testable rule (MUST/SHOULD), and a one-line rationale. Add Governance (how it's amended). Later stages must check against it; `plan` includes a "Constitution Check" and `converge` treats it as governing constraints.

## 2. specify -> `specs/<NNN-slug>/spec.md`
Input: the user's feature description. Output sections, in this order:

```
# Feature Specification: <name>
**Feature Branch**: `<NNN-slug>`  **Created**: <date>  **Status**: Draft
**Input**: User description: "<verbatim>"

## User Scenarios & Testing (mandatory)
### User Story 1 - <title> (Priority: P1)
<journey in plain language>
**Why this priority**: ...
**Independent Test**: how this story alone can be tested and what value it delivers
**Acceptance Scenarios**:
1. **Given** ..., **When** ..., **Then** ...
(### User Story 2 (P2), 3 (P3)... each independently testable and shippable)
### Edge Cases
- What happens when <boundary>? / How does the system handle <error>?

## Requirements (mandatory)
### Functional Requirements
- **FR-001**: System MUST ...
- **FR-00N**: ... [NEEDS CLARIFICATION: <specific question>]
### Key Entities (if data is involved)
- **<Entity>**: what it represents, relationships, no implementation detail

## Success Criteria (mandatory)
- **SC-001**: measurable, technology-agnostic outcome (time, rate, count, %)

## Assumptions
- target users, scope boundaries (what's out of v1), environment, dependencies
```
Rules: no frameworks/languages/DBs/APIs in the spec; every requirement testable; stories prioritized so P1 alone is a viable MVP; at most 3 NEEDS CLARIFICATION markers, prioritized by scope > security/privacy > UX > technical detail.
After writing: show the 3-6 line summary, list the open markers as questions, stop.

## 3. clarify (optional) -> edits `spec.md`
Ask up to 5 targeted questions, one at a time or as one short numbered list, each with a recommended answer and 2-3 options. Record answers in a `## Clarifications` section (`Session <date>: Q -> A`) and fold them into the relevant requirements; remove resolved markers. Run before `plan`.

## 4. plan -> `specs/<NNN-slug>/plan.md` (+ supporting files)
Input: spec.md + the user's stated stack/constraints (ask if none given; propose a default and mark it as a proposal). Produce:
- **plan.md**: Summary; Technical Context (language/version, dependencies, storage, testing, target platform, project type, performance goals, constraints, scale); **Constitution Check** (each principle: pass/violation + justification; gate before research and re-check after design); Project Structure (docs tree and source tree with real paths); Complexity Tracking (only justified violations).
- **research.md**: decisions with rationale and alternatives considered; resolve every "unknown" from Technical Context. If web search is available, use it for library/version facts instead of guessing.
- **data-model.md**: entities, fields, relationships, validation rules, state transitions (from Key Entities).
- **contracts/**: API/interface contracts (endpoints, request/response, errors), one per interface.
- **quickstart.md**: how to run and validate the main scenario end to end.
Every plan decision should trace back to an FR or SC. Stop for review.

## 5. tasks -> `specs/<NNN-slug>/tasks.md`
Input: spec.md + plan.md (+ data-model, contracts). Format each task exactly:
`- [ ] T001 [P] [US1] Description with exact file path`
- `[P]` = parallelizable (different files, no dependency on an incomplete task). `[US#]` only in user-story phases.
- Phases in order: **Phase 1 Setup** -> **Phase 2 Foundational** (blocks all stories) -> **Phase 3+ one phase per user story in priority order** (each independently testable; include tests first if tests were requested, and make them fail before implementing) -> **Final Phase Polish & cross-cutting**.
- Add a Dependencies section (phase order, story independence) and a Parallel example; state the MVP scope (usually just US1) and incremental delivery order.
- Tests are included only if the spec/user asked for them.

## 6. analyze (optional) -> report in chat (or `analysis.md`)
Read-only consistency check across spec, plan, tasks, constitution. Report findings in a table: ID, category (duplication, ambiguity, underspecification, constitution conflict, coverage gap, inconsistency), severity (CRITICAL/HIGH/MEDIUM/LOW), location, recommendation. Include a coverage table (each FR/SC -> tasks) and unmapped tasks. Do not edit files; propose fixes and ask. Constitution conflicts are always CRITICAL.

## 7. checklist (optional) -> `specs/<NNN-slug>/checklists/<domain>.md`
"Unit tests for requirements": items test the *quality of the written requirements* (complete? unambiguous? measurable? consistent?), NOT whether code works. Good: "Is 'fast' quantified with a threshold? [Clarity, Spec FR-003]". Bad: "Verify the button works". Group by quality dimension, cite the spec section, use `- [ ] CHK001 ...`.

## 8. implement
Execute `tasks.md` in phase order, respecting dependencies and `[P]`. Before starting, check any checklists are complete (if not, ask whether to proceed). For each task: do it, then mark `- [X]` in tasks.md. Stop at each phase end with a checkpoint (what works, how to test it). Report failures plainly and don't mark failed tasks done.

## 9. converge -> appends to `tasks.md`
After implementation, audit the **actual code against spec.md, plan.md, tasks.md and the constitution** (these are the sole source of intent). For each FR/SC/acceptance scenario: Met / Partial / Missing / Contradicted with evidence (file and behavior). Append remaining work as new tasks under a `## Convergence Round N` heading. Verdict: **Converged** (nothing remaining) or **Not converged** (list tasks). Repeat implement -> converge until Converged. If the user hasn't given code, ask for it; never judge convergence from the plan alone.

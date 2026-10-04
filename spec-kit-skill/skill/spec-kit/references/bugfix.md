# Bug fixing: assess -> fix -> test

Keep diagnosis, repair, and verification **separate** so the fix targets the assessed cause and the original symptom is re-checked. No SDD feature workflow is required first. Reports live in `.specify/bugs/<slug>/`.

Run the three stages as separate turns with a review between them. Never merge them into one "here's the fix".

## 1. assess -> `.specify/bugs/<slug>/assessment.md`
Input: symptom description (+ logs, stack traces, code the user provides). Ask for missing essentials (expected vs actual, steps to reproduce, environment) in one short list if absent. Write:
- **Symptom** (verbatim) and **Expected behavior**
- **Reproduction** (steps; mark "not yet reproduced" honestly if so)
- **Evidence** examined (files, lines, logs)
- **Candidate causes** ranked, each with confidence and what would confirm/refute it
- **Assessed root cause** (or "undetermined" plus the next diagnostic step; do not guess a cause)
- **Scope / blast radius**: what else could be affected
- **Proposed fix scope**: smallest change that addresses the root cause; explicitly out of scope items
- **Verification plan**: how the original symptom will be re-checked, plus regression checks
Stop for review.

## 2. fix -> `.specify/bugs/<slug>/fix.md` (+ code files/diff)
Only proceed from an assessment with an identified cause (or the user's explicit override). Implement the **scoped** fix only; no drive-by refactors. Record: root cause addressed, files changed with a diff or full file, why this resolves the cause, risks, anything deliberately not changed. If the fix contradicts the assessment, stop and update the assessment first.

## 3. test -> `.specify/bugs/<slug>/verification.md`
Re-run the verification plan against the **original symptom**, plus regression checks. If you can execute code in the sandbox, do so and quote real output; if not, give the exact steps/tests for the user to run and mark results "not yet run". Record each check: what, how, result (pass/fail/not run).
Final verdict, one of:
- **verified**: original symptom no longer reproduces and checks passed with evidence
- **partial**: some checks passed or symptom improved but gaps remain (list them)
- **failed**: symptom persists or the fix caused regressions
Missing verification is never "verified". If partial/failed, loop back to assess with the new evidence.

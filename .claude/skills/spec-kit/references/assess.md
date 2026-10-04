# Idea assessment: intake -> research -> define -> shape -> decide

Gather evidence before committing to an idea, whether or not it becomes software. Works with no code present. Artifacts live in `.specify/assessments/<slug>/`, ending in a **go / needs-clarification / kill** decision. Run one stage per turn; resolve unknowns by refining existing artifacts rather than regenerating whole stages.

## 1. intake -> `intake.md`
Capture the idea in the user's words, then structure: idea statement, who it's for, the problem/job-to-be-done, why now, constraints (time, money, skills, platform), what the user already believes (**stated beliefs, to be tested**), and open questions. Ask at most 3 questions if essentials are missing.

## 2. research -> `research.md`
Gather **evidence**, not opinions. Use web search when available (existing solutions/competitors, demand signals, user pain, pricing, technical feasibility, risks, regulation). For each finding: claim, source (URL/name), confidence, and whether it supports or undermines the idea. Include a "what we could not find" section. Separate facts from inference. Cite sources; never invent data, market sizes, or competitors. Quote nothing at length; summarize in your own words.

## 3. define -> `define.md`
From the evidence: sharp problem statement, target user and segment, value proposition, **key assumptions** (each with a risk rating and how to test it cheaply), success metrics, non-goals. Flag which assumptions are evidence-backed and which are still guesses.

## 4. shape -> `shape.md`
Turn the defined idea into a bounded, testable shape: the smallest version that tests the riskiest assumption (MVP/experiment), 2-3 alternative shapes with trade-offs, rough effort/cost appetite, rabbit holes and risks, and what would have to be true to proceed.

## 5. decide -> `decision.md`
One decision with reasons traced to evidence:
- **go**: evidence supports building; list the first step. Offer to hand off to SDD (`specify` using define/shape as input).
- **needs-clarification**: name the specific unknowns, who/what resolves them, and the cheapest test. Don't pad with a "maybe".
- **kill**: stop, with the documented reason. This is a good outcome, not a failure.
Include: decision, top 3 reasons, strongest evidence against, confidence level, and revisit triggers (what new evidence would change it). Be honest if the evidence is thin; thin evidence means needs-clarification, not go.

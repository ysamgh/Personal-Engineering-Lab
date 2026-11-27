# Learning Method — Deep Apprenticeship Protocol

## Lesson structure

Each lesson will use the relevant parts of this structure, not filler:

1. **Why it exists:** historical/problem context.
2. **Outcomes:** specific skills to demonstrate.
3. **Prerequisites:** concepts that must already be clear.
4. **Mental model:** first simple model, then limitations.
5. **Mechanism:** detailed internals appropriate to level.
6. **Vocabulary:** English technical terms and precise definitions.
7. **Diagrams:** architecture, flow, states or packet/process paths where helpful.
8. **Observation:** commands, tools, UI, logs, metrics or experiments.
9. **Lab:** controlled and reproducible.
10. **Command review:** intent, syntax, scope, privileges, effects, risk, verification, undo.
11. **Failure modes:** symptoms, possible causes, evidence, isolation strategy.
12. **Engineering decisions:** alternatives, trade-offs and constraints.
13. **Challenge/transfer:** apply to a new scenario; no answer reveal before learner attempt.
14. **Teach-back:** explain without reading.
15. **Artifact:** note, diagram, command output (redacted), test or code.
16. **Leitner cards:** durable knowledge only, in the spaced-repetition deck.
17. **Exit evidence:** record practical evidence and remaining unknowns.

## Quiz policy

**No standalone quizzes.** No random quizzes, lesson-end quiz sets, or score-based progression.

Question/answer retrieval is done only through the Leitner spaced-repetition system. Cards should be:
- atomic (one core idea);
- answerable without ambiguity;
- meaningful, not trivia;
- focused on a mental model, mechanism, distinction, decision, diagnostic, or safety rule;
- short enough for active recall;
- linked to the originating lesson and concept;
- reviewed with a user rating that drives scheduling.

Practical challenges, labs and teach-back are not quizzes; they assess transfer and understanding using real tasks and artifacts.

## Leitner spaced repetition (planned app feature)

The app will implement a local-first Leitner scheduler with configurable box intervals. Initial policy:

- Box 1: due after 1 day
- Box 2: due after 3 days
- Box 3: due after 7 days
- Box 4: due after 14 days
- Box 5: due after 30 days

Correct recall moves a card one box forward; missed/weak recall returns it to Box 1. A card never reviewed stays Box 1. The review UI must reveal the answer only after the learner requests it, then ask for a recall rating. Scheduling uses local browser date/time. The system must show due count and card-level next review. Card data is version-controlled in the repository; scheduling state stays local in the browser for MVP.

Intervals are a starting policy, not a scientific guarantee. Future versions may support more nuanced recall ratings, configurable intervals, export/import, and sync.

## Interactive rules

- Never reveal a challenge answer before the learner attempts it.
- Present one primary challenge at a time.
- For commands: explain intent and risks before execution.
- Use safe, read-only observation before modifications.
- Never direct users to run unknown/destructive commands on production.
- Analyze the learner's actual outputs; do not invent results.
- If a tool is unavailable, explain alternatives and how to verify availability.
- Debug one hypothesis at a time.
- Avoid asking multiple unrelated questions in one turn.

## Command Review Card

For each unfamiliar command:
- Intent
- Syntax and parsing
- Scope
- Privileges
- Side effects
- Risk
- Verification
- Undo / rollback

## AI collaboration contract

AI should distinguish facts, assumptions and unknowns; produce small testable changes; provide tests; review diffs; avoid secrets; ask for approval for sensitive/irreversible actions; and never claim verification without evidence.

## Mastery levels

- L0 Recognition
- L1 Explain
- L2 Observe
- L3 Operate
- L4 Diagnose
- L5 Design

Levels are evidence-based and are recorded per concept or stage, not inferred from reading progress.

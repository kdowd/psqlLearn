---
name: learning-designer
description: Design runnable, self-checking instruction on a technical topic (default SQL/PostgreSQL). Use when asked to teach or explain a topic, build a lesson, tutorial, exercise set, worked example, or practice problems, or turn a subject into a step-by-step learning path. Produces objectives, a worked example, a graded exercise set with a hint ladder, and verification queries. Do not use for one-off factual questions.
---

# Learning Designer

You are designing instruction, not delivering a lecture. The deliverable is an artifact a
learner can run, break, and check for themselves. Explanatory prose is scaffolding around
practice, never the product.

## Establish four inputs before writing

1. **Topic and scope** — one bounded skill, not a syllabus. "Filter rows with `WHERE`", not "SQL".
2. **Audience and prior knowledge** — what they can already do. This sets the starting rung.
3. **Time budget** — 15 minutes and 3 hours are different designs, not different lengths.
4. **Environment and data** — what they will actually type into, and against what data.

If the user left any of these unspecified, pick a sensible default, state your assumption in
one line at the top of the output, and proceed. Do not stall a first draft on questions; ask
at the end what to adjust.

Default environment for this repo: PostgreSQL via `psql`, using the sample data in
`contacts.csv` at the repo root (`first,last,phone,email` — note there is **no header row**,
which makes it a good teaching case for `HEADER` handling and for naming columns explicitly).

## Procedure

1. **Write objectives as observable outcomes.** Each starts with a verb the learner performs
   and names the artifact they produce. "Write a `GROUP BY` query that returns one row per
   city, sorted by count descending" — not "understand grouping." Three to five objectives.
2. **Choose a spine and stay on it.** One dataset and one query family carried through every
   step. Switching datasets mid-lesson forces learners to re-learn context instead of the concept.
3. **Fade the support: I do, we do, you do.** First a complete worked example with the reasoning
   narrated. Then a near-transfer exercise where you supply the structure and they fill gaps.
   Then independent problems with no structure given.
4. **Build a difficulty ladder.** Each exercise changes exactly one thing from the last: same
   query, new filter; same filter, new aggregate; same aggregate, new join. If two exercises
   differ in more than one dimension, you have a gap, not a rung.
5. **Give every exercise a three-step hint ladder** (rules below).
6. **Make everything verifiable.** Every exercise states either the expected result shape (row
   count and columns) or ships a check query. A learner who cannot tell whether they are right
   will stop trusting the lesson.
7. **Name the misconceptions.** Two or three wrong mental models this topic reliably produces,
   each with the smallest query that exposes it. `NULL` propagation, `WHERE` versus `HAVING`,
   and integer division are the usual suspects in SQL.
8. **Close with one transfer task** that changes the surface story but not the underlying
   structure, so the learner must recognize the pattern rather than recall the answer.

## Hint ladder rules

Hints reveal thinking, never syntax to copy. Escalate by narrowing the search space:

- **Hint 1 — restate.** Re-express the goal in the vocabulary of the concept. No new information.
- **Hint 2 — point.** Name the clause, function, or operation involved and what it acts on.
- **Hint 3 — edge.** Name the specific trap or the one construct they are missing, short of a
  complete answer. "You need an aggregate here, and `WHERE` runs before it."

Never let a hint be a working query, and never let hint 3 be the full answer with the table
name swapped. Keep all three visible together under the exercise so a learner can choose how
much help to take.

## Output contract

Produce the lesson as a single Markdown file with these sections in this order. `lesson-template.md`
in this skill's directory is the fill-in scaffold — read it and follow its structure when
producing a full lesson.

1. **Assumptions** — one line, only if you had to default any of the four inputs.
2. **Objectives** — three to five observable outcomes.
3. **Setup** — exact commands to create or load the data and confirm the environment works.
4. **Worked example** — complete solution with reasoning narrated step by step.
5. **Guided exercise** — partial structure supplied, gaps left open.
6. **Exercises** — three to five, each with: the problem, expected result shape, a check query,
   and the three hints.
7. **Solutions** — real, runnable SQL, placed at the end under a clear heading.
8. **Misconceptions** — wrong model, why it is tempting, the query that disproves it.
9. **Transfer task** — same structure, different story.

Write SQL that actually runs `psql`-clean, one statement per fenced block, and end file-loading
examples with the terminator your environment needs. State the expected row count for any
result you assert, so a mismatch is visible.

## Self-check before you finish

- Every objective is observable and appears somewhere in the exercises.
- Every exercise has exactly the same number of hints, and no hint is a full answer.
- Every exercise is checkable without asking you.
- The difficulty ladder never jumps two concepts at once.
- Solutions are runnable as written, against the stated dataset, with correct column names.
- The lesson fits the stated time budget; cut exercises rather than hints.

## Anti-patterns

- **Lecturing.** Pages of prose before the learner runs anything. Invert it: a query, then the explanation.
- **Answer-key hints.** A hint that is the solution teaches nothing and removes the productive struggle.
- **Uncheckable exercises.** "Try joining these tables" with no way to confirm success.
- **Toy data with no traps.** Realistic data with `NULL`s, duplicates, and casing surprises is where learning happens.
- **Scope creep.** Adding one more clause "while we're here" breaks the ladder and the time budget.
- **Persona theatre.** Do not pad output with encouragement, motivational framing, or claims about
  your expertise. The learner needs a runnable artifact.

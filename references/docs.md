# Project docs

Agents read the project's docs instead of guessing; a fact they need and cannot find becomes a guess repeated in every card. Each fact has one home; everything else links to it.

## Roles

| Role | Holds | Read by | Default path |
|---|---|---|---|
| Entry | what the project is; index of the docs below; rules every agent follows | every agent | `AGENTS.md` |
| Spec | users, goals, scope and what is out of it, main journeys, product-level acceptance, open questions | plan, plan review | `docs/SPEC.md` |
| Architecture | code map; modules and their responsibilities; interfaces; data flow; one-way doors with their reasons | plan, card `[Background]` | `docs/ARCHITECTURE.md` |
| Development | setup; verified build, test, lint and format commands; a runnable `verify` command that drives the real entry point (browser/simulator for UI, real request for a service, CLI call, caller example for a library; `n/a` for doc-only repos) against Spec's main journeys; verification scope by kind of change; release steps if any | card `[Self-check]` | `docs/DEVELOPMENT.md` |
| Design | visual, interaction and copy rules; only for projects with a UI | UI cards, copy adviser | `docs/DESIGN.md` |
| Decisions | user decisions and one-way doors: date, choice, why | everyone | next to DISPATCH: `docs/agents/decisions.md` |
| Tracker | current; blocked, with the smallest unblocking step; deferred; suggestions carried to later cards, by area; recently done. It is also the handoff notes | lead at session start | next to DISPATCH: `docs/agents/handoff.md` |
| Plans | one per medium or complex piece of work ([planning](planning.md#the-plan)) | plan review, cards | `docs/plans/<slug>.md` |
| Dispatch, lesson | see [init](init.md) | lead (`lesson` also read by reviewer at `origin/main`) | `docs/agents/` |

A role becomes a file only when it has material of its own: a small project may keep spec and architecture as sections of `AGENTS.md`, and an existing issue tracker can be the tracker.

## Rules

- Current behaviour comes from code, config and scripts; expected behaviour from the spec and decisions. Where they disagree, record an open question and leave both sides as they are.
- Record only what is verified: a command goes in after it ran; content read off the code is marked inferred; an unknown is an open question naming who settles it (the user or a planned card).
- Docs say where things are and why; the code says how.

## New project

1. Write Entry, Spec and Development from the user's brief; every unknown is an open question.
2. Architecture is the first complex piece of work ([planning](planning.md)): its one-way doors fill Architecture and Decisions.
3. The first card is the tracer bullet; Development records each command once that card has made it run.

## Existing project

1. Map the existing docs (README, design docs, ADRs, wiki pages, issue tracker, CI config) to the roles. Each keeps its place and name; Entry points to it.
2. Fill only the gaps: a role with no home gets a file at the default path, or a section in the closest existing doc. Mark existing content that disagrees with the code as an open question.
3. Keep current behaviour, decisions and future work apart; decisions the user wrote stay as written.
4. Verify Development's commands: send agy Gemini Neon (or the fast model the user names) to run each one in a fresh worktree on a machine the user cleared for builds, reporting the exit code and last output lines (run the `verify` smoke command twice to confirm consistent results; if the repo has none, record it as an open question). Record the ones that passed; the rest become open questions.

# Init: set a project up for teamwork

Goal: the project has a DISPATCH file and a lesson file the user has confirmed, and its `AGENTS.md` points to them and tells worker agents how cards work.

**Ask, never guess.** Anything the project files and Paseo do not state (concurrency, privacy limits, which host is which machine, models on remote hosts) goes into the confirmation message as a question with your recommendation.

## Steps

1. **Read the project.** Language and stack; build, test, lint and format commands (from `package.json`, Makefile, CI config, existing docs); existing checks and hooks; the docs commit convention; where docs live and in what language; where decisions are recorded. Done when you can state how a change is verified and committed here.
2. **Read the fleet.** Paseo `list_hosts`, `list_providers`, `list_models` per provider, `list_profiles`.
   - `list_models` covers this host only. Models and providers on other hosts are unknown: ask the user.
   - A family offered by two providers (e.g. Sol on Codex and on pi) gives a first choice (the newest id) and a fallback line with separate quota.
   - Profiles show which models the user has used; they may be stale. Modes and thinking levels follow [models](models.md), not the profile.
   - Models with no entry in [models](models.md): list them as available in the confirmation message; route work to them only if the user says so.
   - A host that is offline or restricted: record it from what the user tells you; do not probe it.
3. **Draft and confirm** in one message; each item is a proposal with your recommendation, and every unknown is an explicit question:
   - **Ask-first list**, and what never needs asking. Respect git rules the project already states.
   - **Machines and boundaries**: which host builds, runs devices, does reviews; concurrency limits; privacy or resource limits per machine and the exact clause cards to that machine must carry (ask the user; offer this draft: work only inside the repository; do not inspect the machine's network, files outside the repo or system details; keep machine details out of replies and files; ask when a tool or fact is missing).
   - **Model routing** by capability: strong models review plans and medium or complex code; fast mid-tier models implement, verify and research. Rows: implementation by kind of work, fallback line, plan reviewer, who reviews whom, fact-checker, independent planner, copy adviser, standing assistant. Start from [models](models.md) temperaments and step 2.
   - **Blocking issues** and **sensitive areas** (where simple cards still get a review and reviews use the highest thinking level).
   - **Compatibility**: users or data that depend on old behaviour. Default none: replaced code is deleted without migration.
   - **Decision log** location (propose one next to the dispatch file if the project has none).
   - **Card conventions**: branch naming, commit convention, card numbering, the verify commands a card's self-check runs, where the common card clauses file and reports live.
   - **Coordinator writes code?** Default no.
4. **After the user confirms**, write the files below; append the `AGENTS.md` section (update it if one exists) and add the dispatch file to the project's docs index if it has one. Show the changes, then commit with the project's docs-commit convention.
5. **Verify**: re-read every written file as a fresh coordinator would. Done when every template slot is filled or marked n/a, and every value the project and Paseo could not supply came from the user's answer.

## Where and in what language

- DISPATCH and lesson go next to the project's existing docs (e.g. `docs/<area>/agents/`), else `docs/agents/`, written in that docs folder's language.
- The `AGENTS.md` addition is written in `AGENTS.md`'s own language.
- The templates below give structure; translate headings and wording as needed.

## DISPATCH template

```markdown
# Dispatch config (for the lead)

> Method: team-lead skill. This file holds only this project's config and wins on conflict.

## 0. Ask first
- Ask the user: …
- No need to ask: …

## 1. Machines and boundaries
| Machine | Paseo host | Can do | Cannot do |
- Limits per machine; the clause every card to a restricted machine carries, verbatim.

## 2. Concurrency and working directories
- Worktree location and how to create one; max agents at once; what only runs in the main checkout and how it is scheduled.

## 3. Model routing
| Kind of work | First choice | Fallback | Thinking (agy: in the model id) |
(rows: each implementation kind, build/device, verification and review, plan review, fact-check and search, independent planner, copy adviser, standing assistant)
- Who reviews whom.

## 4. Blocking issues, sensitive areas, compatibility
- Only these block a merge, with a concrete trigger path: …
- Sensitive areas (simple cards still reviewed; reviews at the highest thinking level): …
- Compatibility: users or data that depend on old behaviour (none: replaced code is deleted without migration).

## 5. Card clauses
- Branch naming, commit convention, card numbering.
- Verify commands for self-checks (tests, typecheck, lint, format).
- Common card clauses: path of the file appended to every card; point to project docs instead of copying them.
- Report files: where an agent writes its detailed evidence; reply length cap.

## 6. Decision log
- Where user decisions are recorded.
```

## lesson template

Start it empty; rules the project already documents stay in those docs.

```markdown
# Project lessons

> Only lessons that still hold and belong to this project, grouped by topic, first-seen date in brackets. Model temperament goes to the team-lead skill's models.md. A lesson guarded by a check shrinks to one line naming the check.
```

## AGENTS.md addition

```markdown
## Task cards

The lead dispatches work as task cards; its dispatch config is in <path to DISPATCH>. A card's scope, acceptance and stop conditions take precedence over the general conventions here. Finish the whole card before replying; the last reply and any report follow the card's `[Delivery]`.
```

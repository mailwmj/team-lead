# Planning and plan review

Quality is set before dispatch: a defect fixed in the plan costs a sentence, the same defect found in code costs a fix round.

## Complexity

Rate the piece of work for its planning and each card for its checks; the highest signal present wins.

| Complexity | Signals | Before code | After code |
|---|---|---|---|
| Simple | one card on an existing pattern; no new interface or dependency | write the card | implementer self-verifies with evidence from the final artefact; code review only in DISPATCH sensitive areas; docs get no extra check |
| Medium | several cards, or a new interface inside known technology | [plan](#the-plan) → [plan review](#plan-review) | read-only code review by a strong model |
| Complex | any one-way door; unknown platform or library behaviour; concurrency, state machines, cross-module contracts; runtime state shared across cards (scroll, focus, navigation); gestures and frame timing; phase gates | [research](#research) → plan, plus [dual planning](#dual-planning) on a direction fork → plan review; spike cards for the unknowns land first | code review and independent probe verification, dispatched in parallel on the same commit |

A **one-way door** is a choice costly to undo: data model, module boundaries, public interfaces, persistence formats, a new dependency.

Cards with visible UI: the reviewer checks the screenshots; a verifier adds a visual check only on Complex cards.

A change to code that passed probe verification is rated at least as complex as the card that verified it, whatever its size: a cleanup that touches it is a rewrite.

Each card gets exactly the checks its own signals set. Skip none to save time: in one phase nearly every card's review or verification caught a real defect.

## Research

Before the plan, send a fast model to answer, each answer with file:line or a source:

- which existing project code and dependencies already cover it;
- which mature, maintained libraries do;
- how established products solve it;
- which platform or library behaviour is unknown or surprising.

Citations go to a fact-checker before the plan uses them.

## The plan

Write it yourself and save it where plans live ([docs § roles](docs.md#roles)). It holds:

- the end state and what is out of scope;
- the research findings that shaped it;
- every one-way door: the choice, why, the alternatives rejected;
- modules, one concern each, and the interface signatures between them;
- risks: each unknown gets a spike card that lands before the cards depending on it;
- cards with their dependency graph and parallel lanes; parallel cards share no files, tests included;
- for runtime state two cards both write (scroll position, focus, navigation stack), one owner card; the other goes through its interface, since cards that pass alone can overwrite each other in the same frame once merged;
- acceptance scenarios per card ([card § acceptance](card.md#acceptance)).

### Design bar

The plan, its review and the cards hold to:

- One-way doors are decided for the long run now. Everything else takes the simplest form that meets today's need, with no abstraction for an imagined one.
- The first card is a tracer bullet: the thinnest end-to-end path that runs. Later cards thicken it instead of re-cutting what works.
- Reuse before writing: existing project code and dependencies, then a mature maintained library, then a pattern proven in established products; new code last.
- Replaced code is deleted, with no migration, compatibility shim or fallback path, unless DISPATCH records users or data that depend on the old behaviour.

## Plan review

Send the plan and the batch's card drafts to a strong reviewer of another model family, read-only. It reports blocking items and suggestions ([review § blocking vs suggestion](review.md#blocking-vs-suggestion)) on:

- breaches of the design bar, and weak one-way-door choices;
- the card split, and file or runtime-state overlap between parallel cards;
- per card, a wrong implementation that passes every acceptance item. Each one found is a hole: close it in the acceptance.

Before it reads, a fast model checks that every function, file and path the cards name exists. Fix the blocking items, then dispatch; review again only if a one-way door changed.

## Dual planning

On a direction fork (architecture, phase gate, redesign), the plan is made this way; plan review follows as usual:

1. Write your own plan.
2. In parallel, send the same problem to an independent planner of a different model family (the project's routing names it). Give it only the user's original words, the must-read files and decisions already made; never your draft or leaning. It delivers a plan and changes no files. Paseo's `paseo-committee` or `paseo-advisor` skills can run this step.
3. Merge. Where the plans disagree, code, spec and device or production evidence decide. Claims it marks as guesses, and code it cites, go to a fact-checker first.
4. Give the user one merged plan stating: where both agree; what you adopted from the other plan; what you rejected and why; open forks where both sides hold up, laid out for the user to decide.

## UI copy and interaction advice

When a card changes user-facing text or an interaction flow, after implementation send it to a model strong at copy (the project's routing names it) as an adviser. Give it: who the screen is for, what they need to get done, the current text and interaction, the project's design rules. Check each suggestion against how the feature is really used before it goes into a fix card. Skip it for a few-word change.

# Review and judgement

## Risk tiers

| Tier | For | Process |
|---|---|---|
| Full | architecture decisions, phase gates, anything shipped to users | implement → independent verification (fresh session, tests from the final artefact) + read-only code review → lead judges against raw evidence |
| Light | well-scoped fixes, small features | implementer self-verifies with evidence from the final artefact; add review only in the project's DISPATCH sensitive areas |
| None | docs, scripts, config | no extra check |

Later cards start from the earlier card's branch instead of waiting for its merge.

## Independence

- Verifier and reviewer: a different model from the implementer, a fresh session, read-only, never shown the implementer's self-assessment or PASS.
- The implementer's PASS is a hint. The verdict comes from evidence you can see: command output, file:line, artefact hash.
- A verdict holds for one commit. The review or verification card names the commit hash and the verdict repeats it; a reply without a verdict and that hash counts as no review. Before landing, the branch head must equal the judged hash, or every commit after it must be a fix you checked against the review items; otherwise review again.
- The last reply's first word decides what it is: only `DONE` is a delivery to judge. `PARTIAL` goes back to the same session to finish; `NEEDS-INPUT` and `STUCK` get your answer or a revised card.
- When a diff changes shared tooling, config, CI, hooks or lint rules, run the corresponding contract or plugin tests.
- Research and code claims from careless models are checked by a fact-checker (file:line plus quoted text) before you use them.

## Blocking vs suggestion

- Blocking: only issues that hit the project's DISPATCH blocking list, violate the card's stated claim or scope, or break an existing test or contract, and only with a concrete trigger path (inputs, state or failing check).
- Everything else, including out-of-scope edge variants (with reproduction), is a suggestion: re-grade over-scoped findings and record them in the card's delivery or a follow-up card; they do not hold the merge.
- Review thinking level: high by default; the highest level only for files the project marks sensitive.

## Rounds

Send each blocking item back to the implementer as the reviewer wrote it (trigger path, file:line, failing output), not paraphrased. You check fixes against the review items yourself. Blocking items get one re-review after fixing. At most two rounds: if blocking items remain after the second, take it to the user with your recommendation.

## Mistakes become checks

Every user correction and every rejected review item is closed in the card that fixes it:

1. Mechanically checkable → a check (guard test, lint rule, script assertion) whose failure message says how to fix it.
2. Else, if an interface or directory change can make the wrong form unwritable → change the structure.
3. Else → a lesson line, plus one sentence in the delivery on why no check was possible.

When adding a check, existing violations go into an allowlist that may only shrink; new code has zero. A lesson or rule already guarded by a check shrinks to one line naming the check; a rule the check fully guards is deleted.

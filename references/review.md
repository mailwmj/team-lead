# Review and judgement

Which checks a card gets is set by its complexity ([planning § complexity](planning.md#complexity)).

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

An APPROVE lands. A fix round opens only for blocking items; suggestions go to the delivery notes or to a later card touching the same area. Review and verification of one commit feed one fix round: send the first blocking result at once and steer later ones into the same round.

Send each blocking item back to the implementer as the reviewer wrote it (trigger path, file:line, failing output), not paraphrased. A fix to a blocking item, a rewrite, or any change to code a verifier already probed goes back to the original reviewer and verifier, in parallel; a small fix to a suggestion is checked against the review items by you or the assistant. At most two rounds: if blocking items remain after the second, take it to the user with your recommendation.

## Land

1. Merge in a temporary worktree off `origin/main`, never in a checkout another session writes.
2. The branch head equals the judged hash ([independence](#independence)).
3. After the merge, `git diff --quiet HEAD <judged commit>`: same tree, the branch's CI run is the evidence; otherwise wait for main's CI.
4. Once main is green, remove the worktree and the card's local and remote branches; a verifier's worktree stays until its verdict is in.

## Mistakes become checks

Every user correction and every rejected review item is closed in the card that fixes it:

1. Mechanically checkable → a check (guard test, lint rule, script assertion) whose failure message says how to fix it.
2. Else, if an interface or directory change can make the wrong form unwritable → change the structure.
3. Else → a lesson line, plus one sentence in the delivery on why no check was possible.

Every blocking item found after dispatch also answers: which line of the plan or card should have stopped it? Close the gap where it lives: the card template or [acceptance](card.md#acceptance) practice if general, the project lesson if project-only, [models](models.md) if it is model temperament. Two models missing the same issue means the card was underspecified: fix the card before switching model.

When adding a check, existing violations go into an allowlist that may only shrink; new code has zero. A lesson or rule already guarded by a check shrinks to one line naming the check; a rule the check fully guards is deleted.

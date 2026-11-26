# Review and judgement

Which checks a card gets is set by its complexity ([planning § complexity](planning.md#complexity)).

## Independence

- Verifier and reviewer: a different model from the implementer, a fresh session, read-only, never shown the implementer's self-assessment or PASS.
- The implementer's PASS is a hint. The verdict comes from evidence you can see: command output, file:line, artefact hash.
- A verdict holds for one commit. The review or verification card names the commit hash and the verdict repeats it; a reply without a verdict and that hash counts as no review. Before landing, the branch head must equal the judged hash, or every commit after it must be a fix you checked against the review items; otherwise review again.
- The last reply's first word decides what it is: only `DONE` is a delivery to judge. `PARTIAL` goes back to the same session to finish; `NEEDS-INPUT` and `STUCK` get your answer or a revised card.
- When a diff changes shared tooling, config, CI, hooks or lint rules, run the corresponding contract or plugin tests.
- Research and code claims from careless models are checked by a fact-checker (file:line plus quoted text) before you use them.

## Review and verification cards

A review or verification card is the fixed body below plus at most 10 lines of its own: branch, commit hash, path of the card under review, and this card's focus points. Append the project's common card clauses and restricted-machine clause as for any card.

```
[Role] Independent reviewer | verifier of one commit. Read-only: leave code and branches as they are.
[Read] The card under review and its acceptance, the project lesson file at `origin/main`, then the diff at the named hash. Leave the implementer's report unread.
[Reviewer] Judge the diff against the card's scope and acceptance (including any change to existing tests, baselines, allowlists or CI/lint/hook config against the card's scope list); check the lesson items whose trigger matches the diff; on UI cards, look at the screenshots.
[Verifier] Write probes from the card's claims before reading the implementer's tests; run them on the named hash or merge branch; on UI cards, capture and look at screenshots.
[Grade] Blocking: hits the project's blocking list, breaks the card's claim or scope (including loosening an unlisted existing check), or breaks an existing test or contract, each with a trigger path, file:line and failing output. Everything else is a suggestion; tag it `pre-existing` when it reproduces on `origin/main`.
[Delivery] First word APPROVE or BLOCK (NEEDS-INPUT or STUCK when it cannot judge); the judged hash; one line `lessons: N matched, …` (item-by-item notes in the report file); one line per blocking item, then `pre-existing` and suggestions; detail to the report file DISPATCH names.
```

## Blocking vs suggestion

- Blocking: only issues that hit the project's DISPATCH blocking list, violate the card's stated claim or scope (including loosening an unlisted existing test, baseline, allowlist or CI/lint/hook check), or break an existing test or contract, and only with a concrete trigger path (inputs, state or failing check).
- Everything else, including out-of-scope edge variants (with reproduction), is a suggestion: re-grade over-scoped findings and record them in the tracker's carried suggestions; they do not hold the merge. Findings tagged `pre-existing` (reproduced on `origin/main`) still do not block, but go into the tracker as standalone bugs rather than carried suggestions.
- Review thinking level: high by default; the highest level only for files the project marks sensitive.

## Rounds

An APPROVE lands. A fix round opens only for blocking items; suggestions go to the tracker's carried suggestions ([docs § roles](docs.md#roles)) for a later card touching the same area. Review and verification of one commit feed one fix round: send the first blocking result at once and steer later ones into the same round.

Send each blocking item back to the implementer as the reviewer wrote it (trigger path, file:line, failing output), not paraphrased. A fix to a blocking item goes back to the original reviewer, and to the original verifier too when the verifier raised it or the fix changes code it probed; a rewrite goes back to both, in parallel. Put the first round's blocking items and the diff since the round-1 hash (and its callers) as the second-round card's focus points: it need not re-audit unchanged areas or raise new style suggestions, though real blocking issues spotted elsewhere with a trigger path are still reported. A small fix to a suggestion is checked against the review items by you or the assistant. At most two rounds: if blocking items remain after the second, take it to the user with your recommendation.

## Land

1. Merge in a temporary worktree off `origin/main`, never in a checkout another session writes.
2. The branch head equals the judged hash ([independence](#independence)). Changes to existing checks stay within the card's `[Scope]` list: a reviewed card's verdict covers it; on an unreviewed card, if `git diff --numstat origin/main...<hash>` shows any change to allowlists or CI/lint/hook config, or deletions in existing tests or baselines outside the list, have the assistant inspect them before landing.
3. Cards that write the same runtime state: before the later one lands, the verifier probes it merged with the other.
4. Record a `Verified:` trailer on the commit the lead creates at merge (merge or squash commit per project convention; `--no-ff` when the project has none; include `self` for self-verified cards and `none` only for direct lead doc commits). If the project uses fast-forward or rebase merges with no merge commit, record it in the tracker's recently-done entry instead.
5. After the merge, `git diff --quiet HEAD <judged commit>`: same tree, the branch's CI run is the evidence; otherwise wait for main's CI. If CI fails only on a check untouched by the diff that also fails on `origin/main`, file it as `pre-existing` in the tracker and proceed rather than blindly rerunning.
6. Once main is green (or only fails on a filed `pre-existing` check), remove the worktree and the card's local and remote branches; a verifier's worktree stays until its verdict is in.

## Mistakes become checks

Every user correction and every rejected review item is closed in the card that fixes it:

1. Mechanically checkable → a check (guard test, lint rule, script assertion) whose failure message says how to fix it.
2. Else, if an interface or directory change can make the wrong form unwritable → change the structure.
3. Else → a lesson line, plus one sentence in the delivery on why no check was possible.

Every blocking item found after dispatch — and every bug found after landing — also answers: which line of the plan, card or verification chain should have stopped it? Close the gap where it lives: the card template or [acceptance](card.md#acceptance) practice if general, the project lesson if project-only, [models](models.md) if it is model temperament. Two models missing the same issue means the card was underspecified: fix the card before switching model.

When adding a check, existing violations go into an allowlist that may only shrink; new code has zero. A lesson or rule already guarded by a check shrinks to one line naming the check; a rule the check fully guards is deleted.

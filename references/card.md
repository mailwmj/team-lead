# Card template

A card is the whole contract: the agent sees only the card and the worktree. A card missing any field below is not sent; review and verification cards use [their fixed body](review.md#review-and-verification-cards) instead. Write it in the project's doc language (translate the field names too); append the project's common card clauses (DISPATCH §5) and, for restricted machines, their boundary clause verbatim.

```
[Role] You are … (one sentence)
[Boundaries] Work only inside the repository; what this card must not do; restricted machines' clause, verbatim.
[Background] Must-read files (exact paths, code map first); base branch and commit; decisions already made; known flaky checks from the tracker, if any.
[Goal] One sentence: the state when done.
[Scope] Files, functions and lines to change; existing code to follow; functions and components it must call; what is explicitly out of scope.
[Steps] Numbered, concrete steps.
[Self-check] Run the card's tests locally until green before pushing; CI is final evidence, not a debugger. Commands after each step: only the tests it wrote or changed; typecheck, lint and format once at the end; when the card changes runnable behaviour, drive it once through the project's `verify` command and capture what it showed (the screenshots the reviewer checks on UI cards); the full suite is left to CI where the project has one; run long build/test commands synchronously or with an adequate timeout.
[Acceptance] A checklist of scenarios (see [acceptance](#acceptance)); each item names its evidence (command output, file:line, screenshot, hash).
[Guard the class] Required on fix cards: which check or structure stops this class of mistake; if none is possible, why.
[Stop conditions] Stop and report after 3 different approaches fail on the same item; never change acceptance criteria, skip checks, or make a failing check pass via sleeps, longer timeouts, retries or reruns (diagnostic reruns to prove a failure is unrelated are fine, never as passing evidence). On an unrelated failing check outside `[Scope]` and not listed as a known flaky check in `[Background]`, commit and push completed work, then stop with NEEDS-INPUT; known flaky checks are reported in `[Delivery]` without blocking.
[Delivery] Branch name and commit convention; a short conclusion-first last reply (first word, verbatim: DONE, PARTIAL, NEEDS-INPUT or STUCK; then verdict, commit hashes, one line per acceptance or review item, open items; length cap per DISPATCH, else ~1200 chars). Detailed evidence, logs and API lists go to the report file DISPATCH names, its path in the reply; with no report file, after the summary.
[Cleanup] Delete only the temp files and directories this session created.
```

## Acceptance

Acceptance is the spec the implementer optimises for: write it so only a correct implementation passes.

- Each item is a scenario: input or state → expected behaviour → how it is observed (test name, command, screenshot).
- A wrong implementation fails at least one item. "Tests added" or "suite green" is evidence, never acceptance.
- Scenarios come from the plan's risks: platform semantics (what the platform keys by, callbacks firing twice, late async results, main-thread delivery, known quirks), hangs and timeouts, interleavings, and the counter-examples of past failures.
- Test doubles of platform APIs model those semantics; the card names which.
- The implementer writes the scenarios as failing tests first. The verifier writes its own probes from the card's claims before reading the implementer's tests.
- For invariants, round-trip encoding, idempotency, state machines or optimisations, prefer property-based tests (generated inputs break author/test bias). For rewrites or migrations, run differential tests against the old implementation inside the same card, then delete the old code (and update its listed tests in `[Scope]`) before delivering, keeping golden fixtures if useful.

## Writing the card

- Name functions, not intentions: "call `buildGroups()` from `x.ts`", not "reuse the grouping logic". Agents that may rewrite instead of reuse get an explicit "do not rewrite X" and a cap on lines changed outside the card's scope.
- The implementer adds no dependency the card does not name, no abstraction beyond the card's interfaces, and no test-only code in production; it uses the platform's standard components, and a custom one needs a stated reason. Past ~1.5x the estimated lines, it simplifies before delivering.
- Acceptance evidence comes from the final artefact (built package, merged branch), with commit hash and sha256 where there is a binary.
- `[Scope]` lists every existing check the card may change — tests, screenshot baselines, allowlists, lint/CI/hook config (e.g. those a behaviour replacement invalidates); adding checks needs no listing. Any other loosening of an existing check, or any failure outside that list, is a stop; on a behaviour replacement without that list, the implementer stops with NEEDS-INPUT before starting.
- Rules and docs that judge the change (spec excerpts, acceptance, project lessons) go into the card or as `git show origin/main:<path>`, never from the feature branch.
- Long tasks start with "do not end the turn before the whole card is committed and pushed" when the model tends to stop early (see [models](models.md)).
- Physical steps the user must do are one checklist done in one go, with the agent already capturing logs and the user replying "done" at the end; anything a command can do is automated first.
- Finish notifications truncate: keep `[Delivery]` conclusion-first so the truncated reply stays immediately actionable without an extra activity fetch.
- When a later card builds on this one, ask for its API list (signatures, file:line) in the report.

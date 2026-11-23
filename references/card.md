# Card template

A card is the whole contract: the agent sees only the card and the worktree. A card missing any field below is not sent. Write it in the project's doc language (translate the field names too); add the project's DISPATCH §5 clauses and, for restricted machines, their boundary clause verbatim.

```
[Role] You are … (one sentence)
[Boundaries] Work only inside the repository; what this card must not do; restricted machines' clause, verbatim.
[Background] Must-read files (exact paths, code map first); base branch and commit; decisions already made.
[Goal] One sentence: the state when done.
[Scope] Files, functions and lines to change; existing code to follow; functions and components it must call; what is explicitly out of scope.
[Steps] Numbered, concrete steps.
[Self-check] Commands after each step: only the tests it wrote or changed; typecheck, lint and format once at the end; run long build/test commands synchronously or with an adequate timeout.
[Acceptance] A checklist; each item names its evidence (command output, file:line, screenshot, hash).
[Guard the class] Required on fix cards: which check or structure stops this class of mistake; if none is possible, why.
[Stop conditions] Stop and report after 3 different approaches fail on the same item; never change acceptance criteria or skip checks.
[Delivery] Branch name and commit convention; conclusion-first last reply (first word, verbatim: DONE, PARTIAL, NEEDS-INPUT or STUCK; first ~1200 chars: verdict, commit hashes, one-line-per-item blocking/suggestion list; detailed evidence and logs after).
[Cleanup] Delete only the temp files and directories this session created.
```

## Writing the card

- Name functions, not intentions: "call `buildGroups()` from `x.ts`", not "reuse the grouping logic". Agents that may rewrite instead of reuse get an explicit "do not rewrite X" and a cap on lines changed outside the card's scope.
- Acceptance evidence comes from the final artefact (built package, merged branch), with commit hash and sha256 where there is a binary.
- Spec excerpts go into the card or as `git show origin/main:<path>`: a feature branch may not have the latest docs.
- Long tasks start with "do not end the turn before the whole card is committed and pushed" when the model tends to stop early (see [models](models.md)).
- Physical steps the user must do are one checklist done in one go, with the agent already capturing logs and the user replying "done" at the end; anything a command can do is automated first.
- Finish notifications truncate: keep `[Delivery]` conclusion-first so the truncated reply stays immediately actionable without an extra activity fetch.
- When a later card builds on this one, ask for the API list in the last reply.
- Two models missing the same issue means the card was underspecified: fix the card before switching model.

# Dual planning and design advice

## Dual planning

For architecture decisions, phase gates and redesigns (not for well-scoped cards):

1. Write your own plan.
2. In parallel, send the same problem to an independent planner of a different model family (the project's routing names it). Give it only the user's original words, the must-read files and decisions already made; never your draft or leaning. It delivers a plan and changes no files. Paseo's `paseo-committee` or `paseo-advisor` skills can run this step.
3. Merge. Where the plans disagree, code, spec and device or production evidence decide. Claims it marks as guesses, and code it cites, go to a fact-checker first.
4. Give the user one merged plan stating: where both agree; what you adopted from the other plan; what you rejected and why; open forks where both sides hold up, laid out for the user to decide.

## UI copy and interaction advice

When a card changes user-facing text or an interaction flow, after implementation send it to a model strong at copy (the project's routing names it) as an adviser. Give it: who the screen is for, what they need to get done, the current text and interaction, the project's design rules. Check each suggestion against how the feature is really used before it goes into a fix card. Skip it for a few-word change.

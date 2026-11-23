# Card template

A card is the whole contract: the agent sees only the card and the worktree. A card missing any field below is not sent. Write it in the project's doc language; add the project's DISPATCH §5 clauses and, for restricted machines, their boundary clause verbatim.

```
【角色】你是……（一句话）
【边界】只在仓库目录内工作；本卡不准做的事；受限机器的条款原文。
【背景】必读文件（具体路径，先给代码地图）；基线分支和提交号；已经定下的决策。
【目标】一句话：完成后的状态。
【范围】要改的文件、函数和行号，要照着写的现有代码，必须调用的函数和组件；明确不做的事。
【步骤】编号的具体步骤。
【自查】每步后跑的命令：只跑自己新写和改动的测试；收尾 typecheck、lint、format 各一次。
【验收】可勾选的清单，每项写明证据是什么（命令输出、文件行号、截图、sha256）。
【拦住同类】修复卡必填：这一类错误用什么检查或结构拦住；做不成检查的写明原因。
【停止条件】同一项用 3 种不同思路都失败就停下报告；不准改验收标准，不准跳过检查。
【交付】分支名、提交规范；结论、证据、提交号写在最后一条回复里。
【收尾】只删本会话自己建的临时文件和目录。
```

## Writing the card

- Name functions, not intentions: "call `buildGroups()` from `x.ts`", not "reuse the grouping logic". Agents that may rewrite instead of reuse get an explicit "do not rewrite X" and a line cap on upstream files.
- Acceptance evidence comes from the final artefact (built package, merged branch), with commit hash and sha256 where there is a binary.
- Spec excerpts go into the card or as `git show origin/main:<path>`: a feature branch may not have the latest docs.
- Long tasks start with "做完整张卡、提交并 push 前不要结束回合" when the model tends to stop early (see [models](models.md)).
- Physical steps the user must do are one checklist done in one go, with the agent already capturing logs and the user replying "done" at the end; anything a command can do is automated first.
- When a later card builds on this one, ask for the API list in the last reply.
- Two models missing the same issue means the card was underspecified: fix the card before switching model.

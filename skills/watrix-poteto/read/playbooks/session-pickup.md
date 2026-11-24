### Session pickup
<!-- 中文：接手既有会话 -->

**You own the resume point. Read the prior trail, don't redo it.**
<!-- 中文：你负责确定恢复起点。 -->
<!-- 中文：先读此前的记录，不要重做。 -->

1. Locate the prior trail. A local transcript in the active workspace's session directory (in Cursor, the `agent-transcripts/` directory the system prompt names. Other harnesses: see **Harness** in SKILL.md. Do not glob across every project's directory, that crosses workspace boundaries and reads private chats from unrelated projects), a cloud-agent URL, or a pushed branch. Read the metadata overview and last messages first, then scan back for the decision points. Parse a long transcript in a subagent and keep the reduced timeline in the main thread (the **principle-guard-the-context-window** skill). Pick each subagent's reasoning level for the job, per the Subagents section.
   <!-- 中文：先找到旧记录。 -->
   <!-- 中文：可用当前工作区会话目录中的本地记录、云端 Agent 链接或已推送的分支；Cursor 的本地记录在系统提示给出的 agent-transcripts 目录。 -->
   <!-- 中文：其他宿主参见 ../../build/skills/poteto-mode/skill.md 的 Harness 一节。 -->
   <!-- 中文：不要在所有项目的目录里搜索，那会越出工作区、读到无关项目的私有对话。 -->
   <!-- 中文：先看元数据摘要和最后几条消息，再往回找关键决策。 -->
   <!-- 中文：由子 Agent 解析长记录，主对话保留提炼后的时间线，对应 principle-guard-the-context-window。 -->
   <!-- 中文：按 Subagents 一节为每个子 Agent 选适合任务的推理级别。 -->
2. Reconstruct operational state. The branch and worktree, what already landed (`git log`, `git diff` against the base), the open todos, the decisions made. The prior trail is authoritative input. Resist the bias to re-derive it.
   <!-- 中文：还原运行状态。 -->
   <!-- 中文：确认分支和 worktree、已落地的内容（git log 和相对基点的 git diff）、待办项及已做的决策。 -->
   <!-- 中文：旧记录是此阶段的权威输入。 -->
   <!-- 中文：别习惯性地重新推导一遍。 -->
3. Diff done vs pending. Compare what shipped against what was planned, name the resume point, do not re-run the prior repro or redo completed work. A "let me verify from scratch" pass means you're treating the trail as untrustworthy when it's authoritative.
   <!-- 中文：区分已完成和待做。 -->
   <!-- 中文：对照原计划与已交付内容，指出恢复点，不重跑此前的复现，不重做已完成的实现。 -->
   <!-- 中文：当旧记录是权威输入时，从头验证一遍等于不采信已有记录。 -->
4. Route the remaining work to the matching playbook and pick the verdict: continue the execution, ship a finished recommendation, ratify or override a prior conclusion, or postmortem a failed run. The pickup playbook ends here. The routed playbook owns the rest.
   <!-- 中文：把剩余工作交给合适的 playbook，并决定是继续执行、交付已经写好的建议、认可或推翻此前结论，还是复盘失败。 -->
   <!-- 中文：接手流程到这里结束。 -->
   <!-- 中文：后续由接手的 playbook 负责。 -->
5. Verify the inherited claims against the original goal on the real artifact (the **principle-prove-it-works** skill). A passing prior self-report is not the proof.
   <!-- 中文：在真实产物上对照最初目标核实继承的结论，对应 principle-prove-it-works。 -->
   <!-- 中文：此前 Agent 自报通过，不能当作证明。 -->

**Reply:** where the prior agent stopped, what you inherited vs redid (ideally nothing redone), the resume point, and the outcome.
<!-- 中文：回复：前一个 Agent 停在哪里，哪些内容继承、哪些重做（最好没有重做），恢复点以及结果。 -->

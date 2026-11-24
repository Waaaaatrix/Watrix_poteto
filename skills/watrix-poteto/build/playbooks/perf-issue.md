### Perf issue
<!-- 中文：性能问题 -->

**You own the measurement story. Plan, review, verify the numbers.** Tie every fix to a measurement, don't read source instead of measuring.
<!-- 中文：你负责测量的完整过程。 -->
<!-- 中文：规划、评审并验证数字。 -->
<!-- 中文：每个修复都要对应测量；别用看源码代替实测。 -->

1. Capture a baseline trace via the matching control skill. Vet the baseline, and each later number, with the **benchmark-checklist** skill.
   <!-- 中文：用匹配的操控技能采集基线 trace。 -->
   <!-- 中文：用 benchmark-checklist 审查基线和此后每一个数字。 -->
2. Trace the code to ground hypotheses. Don't claim a perf ceiling without running it first.
   <!-- 中文：追踪代码，为假设提供依据。 -->
   <!-- 中文：先运行实测，再声称存在性能上限。 -->
   Try the performance mantras in order, cheapest first:
   <!-- 中文：按成本从低到高依次尝试下面的性能策略： -->
   1. Don't do it. Stop work whose result nothing uses rather than cheapening it.
      <!-- 中文：不做。 -->
      <!-- 中文：没有调用方使用结果的工作，直接取消，不必降低它的成本。 -->
   2. Do it, but don't do it again.
      <!-- 中文：只做一次，避免重复。 -->
   3. Do it less.
      <!-- 中文：少做一些。 -->
   4. Do it later.
      <!-- 中文：晚点做。 -->
   5. Do it when they're not looking.
      <!-- 中文：在用户没留意的时候做。 -->
   6. Do it concurrently.
      <!-- 中文：并发做。 -->
   7. Do it cheaper.
      <!-- 中文：用更低的成本做。 -->

   When an earlier mantra meets the target, stop.
   <!-- 中文：有较早的策略达到目标，就停止。 -->
3. Plan the fix from the trace. If it crosses a function boundary, `architect` first. Delegate implementation to a subagent using your configured perf-issue model (default `claude-opus-5-5-xhigh-fast`). Pick each subagent's reasoning level for the job, per the Subagents section. Review the diff. Capture a post-fix trace.
   <!-- 中文：根据 trace 制定修复方案。 -->
   <!-- 中文：跨函数边界时，先用 architect。 -->
   <!-- 中文：用配置的 perf-issue 模型把实现委派给子 Agent，默认是原文列出的 Opus 模型。 -->
   <!-- 中文：按 Subagents 一节，为每个子 Agent 选适合任务的推理级别。 -->
   <!-- 中文：评审 diff。 -->
   <!-- 中文：采集修复后的 trace。 -->
   Apply the **sequence-verifiable-units** principle skill, verifying each attempt before trying the next.
   <!-- 中文：应用 sequence-verifiable-units 原则，每次尝试先验证，再试下一个。 -->
4. Parse and compare the artifacts (JSON to sqlite, diff). "Inconclusive" or wrong-surface is not a pass. Flag it.
   <!-- 中文：解析并比较两个产物：把 JSON 转为 SQLite 数据再比较。 -->
   <!-- 中文：无法得出结论或验证场景不匹配，都不算通过。 -->
   <!-- 中文：明确标注。 -->
5. Cite the measurement in the PR.
   <!-- 中文：在 PR 中引用测量结果。 -->
6. Run **Opening a PR**.
   <!-- 中文：执行 Opening a PR 流程。 -->


**Reply:** baseline number, post-fix number, delta, artifact path.
<!-- 中文：回复：基线数值、修复后数值、差值和产物路径。 -->

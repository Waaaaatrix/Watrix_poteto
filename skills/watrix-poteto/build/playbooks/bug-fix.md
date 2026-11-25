### Bug fix
<!-- 中文：缺陷修复 -->

**You own this task. Plan, review, verify.** Delegate investigation and the fix to subagents, stay in the lead. Pick each subagent's reasoning level for the job, per the Subagents section.
<!-- 中文：你负责这项任务。 -->
<!-- 中文：规划、评审、验证。 -->
<!-- 中文：把调查和修复委派给子 Agent，但仍由你主导。 -->
<!-- 中文：按 Subagents 一节，为每个子 Agent 选适合任务的推理级别。 -->

Be scientific. Every shipped line traces to runtime evidence. Belt-and-suspenders that "might help" is a hypothesis, not a fix. It does not ship. When evidence refutes a hypothesis, revert what it motivated. The smallest change the evidence justifies ships, nothing more.
<!-- 中文：按证据做事。 -->
<!-- 中文：每一行要交付的代码都应有运行证据支持。 -->
<!-- 中文：多加保险「也许有用」只是个假设，不是修复。 -->
<!-- 中文：不要交付它。 -->
<!-- 中文：证据推翻假设后，撤回由此引入的改动。 -->
<!-- 中文：只交付证据能支持的最小改动。 -->

1. Reproduce it yourself on the matching surface via the control skill (Non-negotiables), even when a debug or instrumentation protocol says to ask the user to reproduce. Ask the user only with a stated, specific reason the control surface cannot reach the target, and only after driving it as far as it goes. If it won't reproduce directly, synthesize the trigger, tighten conditions, or instrument until it fires.
   <!-- 中文：先通过 Non-negotiables 里的操控技能，在实际发生问题的场景亲自复现，即使调试或插桩流程建议先让用户复现。 -->
   <!-- 中文：仅当工具已用到极限，仍因具体原因够不到目标时，才请用户协助，并说清原因。 -->
   <!-- 中文：不能直接复现时，构造触发器、收窄条件或增加插桩，直到问题出现。 -->
2. Binary-search the cause. Form the candidate hypotheses, then rule them out until one survives. Seed them with a trace of the affected subsystem and the **why** skill for regression history. Each pass, take the split that cuts the most remaining problem space, get runtime evidence, eliminate. When program state is unclear, add instrumentation or logging and read it as the code runs. Don't guess. Drive a long or stubborn hunt with Cursor's `/loop` command. Confirm the surviving *mechanism* with runtime evidence before step 3's `architect` or an `interrogate` fan-out.
   <!-- 中文：按二分思路缩小原因。 -->
   <!-- 中文：提出候选假设，逐个排除，直到只剩一个。 -->
   <!-- 中文：用受影响子系统的跟踪结果和 why 查到的回归历史提出假设。 -->
   <!-- 中文：每一轮都选择最能缩小剩余范围的切分，拿到运行证据，再排除错误情况。 -->
   <!-- 中文：程序状态不明确时，增加插桩或日志，在代码运行时查看。 -->
   <!-- 中文：不要猜。 -->
   <!-- 中文：耗时或难以定位时，用 Cursor 的 /loop 持续调查。 -->
   <!-- 中文：启动第三步的 architect 或委派 interrogate 前，要先用运行证据确认剩下的机制。 -->
3. Plan the fix. If it crosses a function boundary, `architect` first. Delegate implementation to a subagent using your configured bug-fix model (see the [code default](../skills/poteto-mode/skill.md#subagents)) with a specific scope.
   <!-- 中文：规划修复。 -->
   <!-- 中文：如果跨函数边界，先运行 architect。 -->
   <!-- 中文：用配置的 bug-fix 模型委派实现，默认是原文列出的 Opus 模型，并给出明确范围。 -->
4. Verify on the same surface. The original repro now passes. "Inconclusive" or wrong-surface is not a pass. Flag it. Unit tests show branch behavior, not bug absence.
   <!-- 中文：在同一个实际场景验证。 -->
   <!-- 中文：原来的复现现在应通过。 -->
   <!-- 中文：无法得出结论或跑错场景，都不算通过。 -->
   <!-- 中文：明确标注。 -->
   <!-- 中文：单元测试能说明特定代码分支按预期工作，不能单独证明缺陷已消失。 -->
5. Stage the commits so the failing repro lands before the fix in git history. See the **tdd** skill for the failing-test-first cadence when the bug has a cheap local test path. Skip it when the test would be expensive, integration-heavy, or unclear.
   <!-- 中文：整理提交，让会失败的复现先于修复进入 Git 历史。 -->
   <!-- 中文：缺陷容易用本地测试复现时，遵循 tdd 的失败测试优先节奏。 -->
   <!-- 中文：测试代价高、强依赖集成环境或做法不明确时，就跳过。 -->
   This is the canonical **sequence-verifiable-units** principle skill, the failing test first and the fix on top.
   <!-- 中文：这就是 sequence-verifiable-units 原则的典型用法：先提交失败测试，再在其上叠加修复。 -->
6. Run **Opening a PR**.
   <!-- 中文：执行 Opening a PR 流程。 -->

**Reply:** what was broken, root cause, fix, how you verified. Paste failing-then-passing repro output verbatim.
<!-- 中文：回复：哪里坏了、根因、修复和验证方法。 -->
<!-- 中文：原样贴出复现从失败到通过的输出。 -->

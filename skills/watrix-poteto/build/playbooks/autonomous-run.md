### Autonomous run
<!-- 中文：自主执行长任务 -->

**You own the exit condition. Define done, then drive to it without stopping.**
<!-- 中文：你负责退出条件。 -->
<!-- 中文：先定义怎样算完成，然后持续做到该条件满足。 -->

1. State the exit condition as a checkable predicate before the first iteration (tests green, repro fixed, all N PRs merged, pixel-diff zero).
   <!-- 中文：第一次迭代前，把退出条件写成可检查的判断式，例如测试全通过、复现已修复、全部 N 个 PR 已合入、像素差为零。 -->
2. Pick the wake mechanism using your harness's `/loop` command (built into Cursor and Claude Code, not a pstack skill. Without one, see **Harness** in SKILL.md). An event to watch (CI, a merge, a ref advancing) gets a watcher subagent that wakes you on the event, with a long time-based heartbeat as fallback. No event gets a fixed-interval heartbeat sized to when the result is worth re-checking. Pick each subagent's reasoning level for the job, per the Subagents section.
   <!-- 中文：选定唤醒方式：可用宿主的 /loop，它是 Cursor 和 Claude Code 的内置命令，不是 pstack Skill。 -->
   <!-- 中文：宿主没有该命令时，参见 ../skills/poteto-mode/skill.md 的 Harness 一节。 -->
   <!-- 中文：如果要等 CI、合入或 Git ref 更新等事件，就派观察者子 Agent 在事件发生时唤醒，并加一个长周期定时检查作后备。 -->
   <!-- 中文：没有事件时，用固定间隔检查，间隔以结果到什么时候值得重查为准。 -->
   <!-- 中文：按 Subagents 一节，为每个子 Agent 选适合任务的推理级别。 -->
3. Each iteration makes the smallest change the evidence justifies, verifies it against the predicate, commits if it advanced, discards changes that didn't help. Belt-and-suspenders that "might help" gets reverted, not left to ride.
   <!-- 中文：每轮只做证据支持的最小改动，用判断式验证；有进展就提交，没用的改动就丢弃。 -->
   <!-- 中文：撤回「也许有用」的多重保险，不要留着。 -->
   Sequence the work via the **sequence-verifiable-units** principle skill, verifying each unit before the next instead of batching checks at the end.
   <!-- 中文：按 sequence-verifiable-units 原则安排工作，每个单元验证完成再推进，不把检查都堆到末尾。 -->
4. Mid-run discoveries that support the goal are yours. Address broken skills, related bugs, flaky verifiers, review noise, tooling failures, orphaned follow-ups, and fixable drift yourself via poteto-mode. Put out-of-band fixes in their own PR. Do not park reversible work for the human or use `AskQuestion`. Surface only irreversible actions, genuine product or preference calls no experiment can settle, or a real dead end. Keep the predicate as the main drive, and return to it after each side fix.
   <!-- 中文：运行过程中发现有助于达成目标的问题，由你负责。 -->
   <!-- 中文：用 poteto-mode 处理出错的 Skill、相关 bug、不稳定的验证工具、评审噪声、工具故障、无人跟进的事项和可纠正的偏移。 -->
   <!-- 中文：范围之外的修复放到独立 PR。 -->
   <!-- 中文：不要把可撤回的工作搁下等人处理，也不要用 AskQuestion。 -->
   <!-- 中文：只上报不可逆的行动、实测无法决定的真实产品或偏好选择，以及确实无法继续的阻塞。 -->
   <!-- 中文：以退出条件为主线，每次解决旁支问题后都回到主线。 -->
5. Checkpoint every iteration via the **show-me-your-work** skill, a row for what changed and whether the predicate moved.
   <!-- 中文：用 show-me-your-work 为每轮留记录，每行写改了什么，以及是否向退出条件推进。 -->
6. Stop when the predicate is met. A plateau is not a stop, so keep going and pivot your approach to push past it. Surface a genuine dead end rather than spinning, and never relax the predicate to declare victory.
   <!-- 中文：退出条件满足时停止。 -->
   <!-- 中文：停滞本身不是停止条件，应继续并换一种做法突破。 -->
   <!-- 中文：真的走不通就报告，不要空转，也不要放宽条件来宣称完成。 -->

**Reply:** the exit condition, iterations run, what landed, what was discarded, final predicate state.
<!-- 中文：回复：退出条件、跑过的迭代、落地和放弃的内容，以及最终是否满足条件。 -->

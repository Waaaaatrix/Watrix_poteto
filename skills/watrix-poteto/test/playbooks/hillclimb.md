### Hillclimb
<!-- 中文：围绕指标持续优化 -->

**You own the metric and the experiment's integrity. Supervise and review. Delegate the attempts.** For sustained, iterative improvement of one measurable thing against a target. A one-off fix is Bug fix or Perf issue. This is the loop.
<!-- 中文：你负责指标和实验的可信度。 -->
<!-- 中文：监督并评审。 -->
<!-- 中文：把各次尝试委派出去。 -->
<!-- 中文：本流程用于围绕同一个可测目标持续迭代改进。 -->
<!-- 中文：一次性修复走 Bug fix 或 Perf issue。 -->
<!-- 中文：本流程定义反复迭代的做法。 -->

Core discipline: one change, one measurement, keep or revert. Never stack untested changes, and never claim a win from code inspection (the **prove-it-works** principle skill).
<!-- 中文：核心纪律是一次改一项、测一次，然后保留或撤回。 -->
<!-- 中文：不叠加未经测试的改动，也不只靠读代码就宣称有收益，对应 prove-it-works 原则。 -->

1. Ground the workload and architecture before choosing the metric. Trace the target, name the realistic workload dimensions that can move the result (data size, history, state, concurrency), and select a case that reproduces the user's complaint. If no case reproduces it, fix the repro instead of hillclimbing. Then fix one metric, the direction that counts as better, and a checkable stop predicate that pairs a target with a floor on attempts so a lucky early win can't end the run (the example "at least 50% better than baseline and at least 10 iterations" is this shape). Use the user's numbers when given, otherwise agree them.
   <!-- 中文：选指标之前，先了解工作负载和架构。 -->
   <!-- 中文：追踪目标路径，列出可能改变结果的实际负载维度，例如数据规模、历史、状态和并发，再选择能复现用户问题的场景。 -->
   <!-- 中文：没有任何场景能复现时，先修正复现方法，不进入优化循环。 -->
   <!-- 中文：然后固定一个指标、改善方向，以及可检查的停止条件，将目标和最少尝试次数绑定，避免偶然的早期收益提前结束任务；例如至少比基线好 50%，且至少迭代 10 次。 -->
   <!-- 中文：用户已经给出数值时用用户的数值，否则先约定。 -->
2. Build the measurement harness, prove its sensitivity, then freeze it (the **build-the-lever** principle skill). Run contrasting realistic workloads and confirm the target case reproduces the symptom while easier cases separate as expected. If the harness cannot distinguish them, revise the workload or metric. Vet the harness with the **benchmark-checklist** skill before you freeze it, and make it print its error count and a count of the work done. Once frozen, one repeatable command emits the metric, sampled enough to clear the noise (median of N, not a single run). Record the baseline metric and a green run of the regression gate (the tests that must keep passing) before any change.
   <!-- 中文：构建测量工具，证明它能区分目标差异，然后冻结测量方法，对应 build-the-lever 原则。 -->
   <!-- 中文：跑有差异的真实负载，确认目标场景能复现问题，简单场景也按预期与其区分。 -->
   <!-- 中文：工具区分不了时，先调整负载或指标。 -->
   <!-- 中文：冻结前用 benchmark-checklist 审查，并让工具输出错误数和实际完成的工作量。 -->
   <!-- 中文：冻结后，一条可重复执行的命令就应输出指标；采样须足以盖过噪声，例如用 N 次测量的中位数，而非单次结果。 -->
   <!-- 中文：任何改动前记录基线数值，并跑一遍须持续通过的回归测试，确认结果为绿。 -->
3. Open the decision log via the **show-me-your-work** skill. A `decision.tsv`, one row per attempt: id, hypothesis, change, before, after, delta, tests, verdict (kept or reverted), note. Read it before each attempt. Keep it out of the tree (gitignored).
   <!-- 中文：用 show-me-your-work 创建决策记录。 -->
   <!-- 中文：在 decision.tsv 中每次尝试记一行：编号、假设、改动、改前、改后、差值、测试、结论（保留或撤回）和备注。 -->
   <!-- 中文：每次尝试前先读它。 -->
   <!-- 中文：让 Git 忽略该文件，不提交进仓库。 -->
4. Ground each hypothesis in the architecture model from step 1, so it names a specific mechanism ("defer X off the boot path because it blocks first paint"), not "try memoizing something". For a perf metric, order hypotheses by the performance mantras in step 2 of the Perf issue playbook (`../../build/playbooks/perf-issue.md`). Borrow only their order, not that step's stop rule.
   <!-- 中文：每个假设都要依据第一步的架构理解，指向具体机制，例如把 X 移出启动路径，因为它阻塞首次渲染，而不是笼统地试试缓存。 -->
   <!-- 中文：性能指标的假设按 Perf issue 第二步（../../build/playbooks/perf-issue.md）的性能策略排序。 -->
   <!-- 中文：只借用顺序，不借用那一步的停止规则。 -->
5. Loop, one hypothesis per iteration:
   <!-- 中文：循环执行，每轮只验证一个假设： -->
   - Hand the change to a subagent using your configured hillclimb model (default `claude-opus-5-5-xhigh-fast`) with a tight scope. Pick each subagent's reasoning level for the job, per the Subagents section. Supervise and review the diff rather than typing it (the **guard-the-context-window** principle skill). When several independent hypotheses are live, fan them to parallel subagents, each in its own worktree (the **separate-before-serializing-shared-state** principle skill).
     <!-- 中文：用已配置的 hillclimb 模型把改动交给子 Agent，默认 claude-opus-5-5-xhigh-fast，明确限定范围。 -->
     <!-- 中文：按 Subagents 一节，为每个子 Agent 选适合任务的推理级别。 -->
     <!-- 中文：主 Agent 监督并评审 diff，不亲自敲实现，对应 guard-the-context-window 原则。 -->
     <!-- 中文：同时有多个互不依赖的假设时，分派给并行子 Agent，各用独立 worktree，对应 separate-before-serializing-shared-state 原则。 -->
   - Measure before and after with the frozen harness, and run the regression gate.
     <!-- 中文：用冻结的测量工具测改前和改后，并运行回归测试关卡。 -->
   - Accept only when the metric moves past noise and the gate stays green. Otherwise revert the change in full. A tweak that "might help" is not kept.
     <!-- 中文：只有指标变化超过噪声、且回归测试仍通过时，才接受改动。 -->
     <!-- 中文：否则完整撤回。 -->
     <!-- 中文：不保留「也许有用」的微调。 -->
   - One commit per accepted fix, staging only the files you changed (`git add <files>`, never `-A`). Log the row either way, kept or reverted.
     <!-- 中文：每个被接受的修复单独做一次提交，仅暂存自己改动的文件，用 git add <files>，不用 -A。 -->
     <!-- 中文：无论保留还是撤回，都要记录一行结果。 -->
   Each iteration ends in a check before the next begins (the **sequence-verifiable-units** principle skill). If the run is unattended, borrow only the wake mechanism from the Autonomous run playbook (`../../build/playbooks/autonomous-run.md`), not its stop rule.
   <!-- 中文：每轮必须以一次检查结束，再开始下一轮，对应 sequence-verifiable-units 原则。 -->
   <!-- 中文：无人值守时，只借用 Autonomous run 流程（../../build/playbooks/autonomous-run.md）的唤醒机制，不沿用它的停止规则。 -->
6. Push past the first plateau. On a stall, several rejects in a row, pivot category, combine near-misses, re-read the source, or try something more radical before concluding the hill is climbed. Correctness and simplicity outrank the number. Revert a win that breaks behavior, and keep a simplification that holds the number (the **laziness-protocol** principle skill).
   <!-- 中文：突破第一个平台期。 -->
   <!-- 中文：遇到停滞或连续几次不通过时，在宣称优化完成前，换一类尝试、结合接近成功的方案、重读源码，或试更彻底的做法。 -->
   <!-- 中文：正确性和简洁性优先于指标数值。 -->
   <!-- 中文：破坏行为的收益要撤回；指标不退步而能简化代码的改动应保留，对应 laziness-protocol 原则。 -->
7. Stop when the predicate is met, or when the remaining ideas are marginal and not worth their cost. Don't relax the predicate to meet it, and don't quit while cheap untried hypotheses remain. If you are stuck, surface it instead of spinning.
   <!-- 中文：达到停止条件时停止；剩余思路收益很小、不值成本时也可以停。 -->
   <!-- 中文：不要放宽停止条件，也不要在还有低成本假设未试时就结束。 -->
   <!-- 中文：真的卡住就报告，不要空转。 -->
8. Run **Opening a PR** with the accepted commits stacked in the order they landed.
   <!-- 中文：执行 Opening a PR 流程，把接受的提交按落地顺序叠放。 -->

**Reply:** the metric and target, baseline to final with the percent delta, iterations run (kept vs reverted), each accepted fix on one line, the `decision.tsv` path, and the best idea you would try next if pushed further.
<!-- 中文：回复：指标和目标、基线到最终的变化及百分比、迭代次数和保留或撤回的数量、每项接受的改动各用一行、decision.tsv 路径，以及若继续会优先试的方案。 -->

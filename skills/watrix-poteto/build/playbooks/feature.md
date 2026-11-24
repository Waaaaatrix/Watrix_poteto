### Feature
<!-- 中文：功能开发 -->

**You own the design. Plan, review, verify.** Delegate implementation. Stay in the lead.
<!-- 中文：你负责设计。 -->
<!-- 中文：规划、评审、验证。 -->
<!-- 中文：把实现委派出去。 -->
<!-- 中文：你继续主导。 -->

1. Trace the affected subsystem.
   <!-- 中文：追踪受影响的子系统。 -->
2. `architect` for the shape.
   <!-- 中文：用 architect 确定结构。 -->
3. Write the throughput checkpoint as four todo items. A dimension that genuinely does not apply (single file, no fan-out) keeps its item with `n/a: <reason>` rather than being dropped:
   <!-- 中文：把吞吐检查写成四项待办。 -->
   <!-- 中文：确实不适用的维度，例如只涉及一份文件、无需分派，保留条目并写明不适用的原因： -->
   - **Blocking first steps.** Gates run before fan-out.
     <!-- 中文：先做阻塞步骤。 -->
     <!-- 中文：必须先通过这些关卡，再分派任务。 -->
   - **Independent workstreams.** Disjoint files, services, or layers parallelize. Shared writes serialize.
     <!-- 中文：划分独立工作流。 -->
     <!-- 中文：不同文件、服务或层可以并行。 -->
     <!-- 中文：共享写入应串行处理。 -->
   - **Shared mutable state.** Default to splitting the target (the **separate-before-serializing-shared-state** principle skill). Serialize only for real invariants.
     <!-- 中文：处理共享可变状态。 -->
     <!-- 中文：默认拆分工作目标，对应 separate-before-serializing-shared-state 原则。 -->
     <!-- 中文：只对确实需要维护的约束使用串行。 -->
   - **Smallest safe decomposition.** If one worker is best, name why.
     <!-- 中文：找出最小且安全的拆分方式。 -->
     <!-- 中文：由一名执行者来做最好时，写明原因。 -->
4. Delegate code-writing to a subagent using your configured feature model (default `claude-opus-5-5-xhigh-fast`) with a specific scope (file paths, named data shape and its organizing structure per **principle-model-the-domain**, a state machine over scattered booleans, a table/registry over branching, a typed model over repeated shape assumptions, chosen before the delegate writes logic, and success criteria). Pick each subagent's reasoning level for the job, per the Subagents section. When the implementation admits multiple valid shapes (error handling, abstraction layer, test structure), delegate via the **arena** skill instead so the runners surface the alternatives and the cross-judge guards the pick. Mandatory: no skip-with-reason escape, and Laziness Protocol does not override it (the gain is review separation, not lines saved). A subagent forbidden to spawn satisfies this by owning the diff directly with the same review separation. No "standing by" reply that waits on a nested agent. Surgical edits, re-ground against the source for upstream-derived files. Port shared-primitive improvements to all consumers and verify each. Commit liberally.
   <!-- 中文：用已配置的 feature 模型把代码实现委派给子 Agent，默认是原文列出的 Opus 模型，并限定文件路径、数据形态、组织结构和验收条件。 -->
   <!-- 中文：按 principle-model-the-domain，在委派前选定结构：用状态机替代散落的布尔值，用表或注册表替代分支，用类型模型替代反复假定数据结构。 -->
   <!-- 中文：按 Subagents 一节，给每个子 Agent 选合适的推理级别。 -->
   <!-- 中文：当错误处理、抽象层或测试结构有多个合理形态时，用 arena 委派，比较各方案，再让另一模型评审选择。 -->
   <!-- 中文：这一条必须做，不能靠说明理由跳过；Laziness Protocol 也不能覆盖，因为收益是隔离评审和实现，不是少写几行。 -->
   <!-- 中文：不能再派子 Agent 的执行者直接负责 diff，也要保持同样的评审分离。 -->
   <!-- 中文：不能回复「等待中」就等下层 Agent。 -->
   <!-- 中文：精准修改；源自上游的文件要重新以源码为依据。 -->
   <!-- 中文：把共享基础组件的改进迁移到所有调用方，并逐一验证。 -->
   <!-- 中文：可以多做小提交。 -->
5. Verify on the matching surface. "Inconclusive" or wrong-surface is not a pass. Flag it.
   <!-- 中文：在匹配的实际场景验证。 -->
   <!-- 中文：无法得出结论，或验证场景与问题不一致，都不算通过。 -->
   <!-- 中文：明确标注。 -->
6. Rebase into small, ordered commits. Stack follow-ups.
   <!-- 中文：通过 rebase 整理成小且有序的提交。 -->
   <!-- 中文：后续改动按依赖叠加。 -->
   Use the **sequence-verifiable-units** principle skill, building, verifying, and committing each small unit before the next.
   <!-- 中文：应用 sequence-verifiable-units 原则：每个小单元都先实现、验证和提交，再做下一个。 -->
7. If the design is contested, `interrogate` before shipping.
   <!-- 中文：如果设计有争议，交付前用 interrogate 审查。 -->
8. Run **Opening a PR**.
   <!-- 中文：执行 Opening a PR 流程。 -->

Code-coupled work (one feature, one migration) goes to a single owner with the checkpoint inline. That owner fans out internally after the blocking phase. Parent-level fan-out is for slices that produce independent artifacts (audits, cross-subsystem investigations, competing experiments). Rewrite the checkpoint at phase boundaries. Spawn a fresh owner rather than chaining interrupts.
<!-- 中文：代码耦合的工作，例如一个功能或一次迁移，由单一负责人处理，并随任务附上检查项。 -->
<!-- 中文：负责人完成阻塞阶段后，再在内部并行。 -->
<!-- 中文：主 Agent 只并行分配能独立产出成果的工作，例如审查、跨子系统调查和竞争性实验。 -->
<!-- 中文：每个阶段边界都重新写吞吐检查。 -->
<!-- 中文：启动新的负责人，不要连续打断同一个人。 -->

**Reply:** what you built, what you chose and why, the throughput checkpoint, open decisions. Tables for design alternatives.
<!-- 中文：回复：实现了什么，选了什么以及为何选择，吞吐检查和未决事项。 -->
<!-- 中文：设计备选方案用表格比较。 -->

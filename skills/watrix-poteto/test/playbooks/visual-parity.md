### Visual parity
<!-- 中文：视觉一致性 -->

**You own pixel-exact equivalence. The baseline is the spec. You do not touch it.** Equivalence is verified by image diff, not by eye.
<!-- 中文：你负责逐像素等价。 -->
<!-- 中文：基线就是规格。 -->
<!-- 中文：不要改动基线。 -->
<!-- 中文：用图像差分验证等价，不能仅凭目测。 -->

1. Establish the baseline first, before any migration: a visual regression harness that screenshots the current component across its states, plus the target when matching two implementations. No baseline, no parity claim. A blocking prerequisite, not a follow-up.
   <!-- 中文：迁移前先建立基线：用视觉回归工具对当前组件的各个状态截图；对齐两种实现时，还要捕获目标实现。 -->
   <!-- 中文：没有基线就不能宣称一致。 -->
   <!-- 中文：这是阻塞性前提，不是事后补项。 -->
2. Anti-shortcut clauses, stated and held: no harness modifications, no baseline tampering, no component restructuring to make a diff pass. If the baseline looks wrong, stop and ask, don't edit it.
   <!-- 中文：明确并坚持防取巧规则：不修改验证工具，不篡改基线，不为让 diff 通过而重组组件。 -->
   <!-- 中文：觉得基线有误时，停下来询问，不要直接改。 -->
3. Migrate one component at a time. Parallelize across worktrees, one owner per component (the **separate-before-serializing-shared-state** principle skill). Shared primitives migrate first as a blocking phase.
   <!-- 中文：每次迁移一个组件。 -->
   <!-- 中文：按 worktree 并行，每个组件有一名负责人，对应 separate-before-serializing-shared-state 原则。 -->
   <!-- 中文：先迁移共享基础组件，作为后续工作的前置步骤。 -->
4. Verify each component against its baseline via image diff on the matching surface via the control skill. A nonzero diff is a fail. Investigate the pixel delta. `/loop` per component until the diff is zero.
   <!-- 中文：通过操控技能，在匹配的实际界面逐一做图像差分，将组件与基线比较。 -->
   <!-- 中文：差分非零即失败。 -->
   <!-- 中文：找出像素差异原因。 -->
   <!-- 中文：为每个组件运行 /loop，直到差分为零。 -->
5. Run **Opening a PR** per component or per safe batch.
   <!-- 中文：每个组件或每个安全批次都执行 Opening a PR 流程。 -->

**Reply:** components migrated, the diff result for each, the baseline harness location, what's left.
<!-- 中文：回复：已迁移的组件、各组件的差分结果、基线验证工具的位置，以及剩余事项。 -->

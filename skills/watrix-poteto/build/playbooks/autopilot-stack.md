### Autopilot-stack
<!-- 中文：自动构建 PR 栈 -->

Run the Autopilot-full playbook, but do not merge.
<!-- 中文：按 Autopilot-full 流程执行，但不合入。 -->

1. **In place of step 5, stop at verified.** Stop when each PR is verified and green at its current head, then hand the PRs to the operator. After the handoff, nobody pushes without the operator's go. Every loop tick prompt names this file, so each tick sees the no-merge rule.
   <!-- 中文：用「验证通过后停止」替代第五步。 -->
   <!-- 中文：每个 PR 的当前 head 都通过验证且检查为绿时，停止并交给操作者。 -->
   <!-- 中文：交接后，没有操作者许可，任何人都不推送。 -->
   <!-- 中文：每次循环的提示词都指明本文件，确保每轮都看到不合入的规则。 -->
2. **Stack a PR only when it builds on another PR.** This is the one exception to step 3. Base the PR on the other PR's branch in place of trunk, per `opening-a-pr.md`. In place of step 2's rebase onto trunk, each PR rebases onto its parent PR's branch, and the stack root rebases onto trunk. A new head on the lower PR restacks the PRs above it, and a PR whose `git patch-id` the restack leaves unchanged keeps its verdict.
   <!-- 中文：只有某个 PR 依赖另一个 PR 的改动时，才把它们叠成栈。 -->
   <!-- 中文：这是第三步唯一的例外。 -->
   <!-- 中文：按 opening-a-pr.md，用下层 PR 的分支作为基点，而不是直接基于主干。 -->
   <!-- 中文：用以下规则替代第二步直接基于主干的 rebase：每个 PR 基于父 PR 分支 rebase，栈根基于主干 rebase。 -->
   <!-- 中文：下层 PR 的 head 更新时，重排上方的 PR；重排后 git patch-id 未变的 PR 可以保留原验证结论。 -->

**Reply:** list each PR with its verdict, in place of what merged.
<!-- 中文：回复：列出各 PR 及其验证结论，而不是汇报合入了什么。 -->

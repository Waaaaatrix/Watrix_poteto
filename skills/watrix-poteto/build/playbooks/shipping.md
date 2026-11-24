### Shipping
<!-- 中文：交付合入 -->

**You own what lands. Verify each PR independently, land only the verified run from the root, then keep your hands off the stack.**
<!-- 中文：你负责最终合入的内容。 -->
<!-- 中文：独立验证每个 PR，只从栈底开始合入连续通过验证的一段，随后不要再碰这一栈。 -->

1. **Resolve the forge, then verify each PR independently before it lands.** GitHub CLI (`gh`) is the default. If `command -v origin` succeeds and Origin can resolve the repository, use `origin pr` for every PR operation. Otherwise stay on `gh` and record the fallback. Never require Graphite (`gt`). One verifier per PR, each a Cursor cloud agent, runs the change the way a user would with the matching control skill (such as `control-ui` or `control-cli` from `cursor-team-kit`), parent against head, and posts `PASS`, `PASS+NOTES`, or `FAIL` on that PR. Only a verdict from an agent that did not write the code counts, because an author checks what it meant to build. Green CI and an approving bot review are not verdicts, because neither runs the change as a user would.
   <!-- 中文：先确定使用哪个代码托管平台，再逐个独立验证 PR，验证完成后才合入。 -->
   <!-- 中文：默认用 GitHub CLI（gh）。 -->
   <!-- 中文：如果系统能找到 origin 命令，且 Origin 能识别仓库，就用 origin pr 执行所有 PR 操作。 -->
   <!-- 中文：否则继续用 gh，并记录这次回退。 -->
   <!-- 中文：不依赖 Graphite（gt）。 -->
   <!-- 中文：每个 PR 配一名 Cursor 云端验证 Agent，用对应的操控技能从用户视角操作，把父版本和 head 做对照，并在该 PR 上发布 PASS、PASS+NOTES 或 FAIL；操控技能可用 cursor-team-kit 中的 control-ui 或 control-cli。 -->
   <!-- 中文：只有没写过这段代码的 Agent 给出的结论才算数，因为作者倾向于检查自己原本想做的事。 -->
   <!-- 中文：CI 为绿或机器人评审通过都不等于验证结论，它们没有像用户那样实际运行改动。 -->
2. **Land only the contiguous verified run rooted at the bottom.** Walk up from the lowest unmerged PR and stop at the first one without `PASS` or `PASS+NOTES`. The PRs below it are the run, and the top one is the ceiling. A verified PR above an unverified one waits, because it cannot land without the code below it. One whose branch neither holds nor needs that code can move onto trunk and land on its own. It needs a fresh verdict first, because its old one ran with that code beneath it and trunk does not have it.
   <!-- 中文：只合入从栈底开始、连续通过验证的一段。 -->
   <!-- 中文：从最低的未合入 PR 向上检查，遇到第一个没有 PASS 或 PASS+NOTES 的 PR 就停。 -->
   <!-- 中文：它下面的 PR 是本轮合入段，其中最高的一项是本轮上限。 -->
   <!-- 中文：未验证 PR 上方的已验证 PR 也要等，因为它无法脱离下层代码先合入。 -->
   <!-- 中文：如果某个分支既不包含也不需要下层代码，可以移到主干上独立合入。 -->
   <!-- 中文：它必须先取得新的验证结论，因为旧结论是在包含下层代码的环境得出的，主干并不包含那些代码。 -->
3. **Re-check that the verdicts still describe the code.** A verdict covers a patch, and a chain rebase moves every PR above it to a new SHA without failing any check. Compare `git patch-id` at the verdict SHA with the current head before trusting an older verdict. When the patches differ only in tests, docs, or lint config, a lane's result stands if what it ran builds the same at both SHAs apart from noise. Noise is a difference that also shows up between two builds of the verdict SHA, or an embedded commit SHA. Report each kind of noise with its files, so a reader can check the call. The change itself still gets fresh checks and a review, because the old verdict never saw it. A lane with no build output, such as a dev server, cannot show this, so rerun it, and re-verify anything else that drifted.
   <!-- 中文：重新确认验证结论仍覆盖当前代码。 -->
   <!-- 中文：验证结论覆盖的是补丁；一次链式 rebase 会把上层每个 PR 移到新 SHA，即使所有检查仍为绿。 -->
   <!-- 中文：依赖旧结论前，比较结论所对应的 SHA 和当前 head 的 git patch-id。 -->
   <!-- 中文：两份补丁只在测试、文档或 Lint 配置上不同，并且该验证分支在两个 SHA 上的构建产物除噪声外相同时，原验证分支结果可以沿用。 -->
   <!-- 中文：噪声指对同一验证 SHA 构建两次也出现的差异，或产物中嵌入的提交 SHA。 -->
   <!-- 中文：报告每类噪声及涉及的文件，便于读者复核。 -->
   <!-- 中文：变动本身仍要重新检查和评审，因为旧结论从未覆盖它。 -->
   <!-- 中文：没有构建产物的验证分支，例如直接跑开发服务，无法证明这一点，就重跑它；其他有变化的验证也要重做。 -->
4. **Land bottom-up, one PR at a time.** Always merge the run's lowest unmerged PR, with `origin pr merge <pr>` or `gh pr merge <pr> --squash`. On Origin the plain command also merges every PR below the one you name, and on GitHub a PR whose base is still its parent's branch merges into that branch, not trunk. Read the PR back for the result (`origin pr view <pr>` or `gh pr view <pr> --json state,mergedAt,baseRefName`), because a merge command can report success when a merge queue only queued the PR, and a requested auto-merge is not a merge. A landed PR reads merged with trunk as its base, not its parent's branch. Arm `--auto` (Origin merge-when-ready or GitHub auto-merge) only on a PR whose verdict covers its current head, because it lands whatever head is ready. After a merge lands, retarget the PR whose base was the merged branch to trunk (`origin pr edit <pr> --base <trunk>` or `gh pr edit <pr> --base <trunk>`), move its patch onto the merge result, and push with `--force-with-lease`. The verdict covers the patch, not its commits, so step 3's patch-id rule decides whether the verdict survives. Merge it once its checks pass and its verdict survives.
   <!-- 中文：从底部逐个合入，每次一项。 -->
   <!-- 中文：总是先合入本轮最低的未合入 PR，使用原文列出的 origin pr merge 或 gh pr merge 压缩合并命令。 -->
   <!-- 中文：在 Origin 上，普通合入命令也会合入所指定 PR 下方的所有 PR；在 GitHub 上，如果 PR 仍以父分支为目标，会合入该父分支而非主干。 -->
   <!-- 中文：再用原文给出的 PR 查询命令回读结果，因为合入命令可能在只加入合入队列时就报告成功，请求自动合入也不代表已经合入。 -->
   <!-- 中文：已落地的 PR 应显示为已合入，且目标是主干而非父分支。 -->
   <!-- 中文：只有验证结论覆盖当前 head 时才启用自动合入，因为齐备时会合入届时的 head。 -->
   <!-- 中文：合入后，把以已合入分支为目标的下一项 PR 改为以主干为目标，把其补丁移到合入后的结果之上，再用带租约的强制推送发布。 -->
   <!-- 中文：结论覆盖补丁而非提交本身，因此按第三步的 patch-id 规则确定结论是否沿用。 -->
   <!-- 中文：检查通过且验证结论仍有效时才合入。 -->
5. **Leave the stack alone while it lands.** Step 4's move of the next PR is the only change to a branch or base, because a merge lands whatever its branch and base hold at that moment, verdict or not. On GitHub, watch the run with `../skills/poteto-mode/scripts/watch-pr/watch-pr --queued-stack` under `/loop` until it reports `COMPLETE` at the ceiling. Pass the run's PR numbers, bottom-to-top, in `--stack-prs`, and pass the same list each time you restart the watcher. On Origin, re-read the current PR with `origin pr view <pr> --checks` each time `origin pr checks <pr> --watch` returns, until the ceiling reads merged. A base that moves to trunk when its parent merges is expected, not damage to repair. If a merge stalls, find out why before you change anything.
   <!-- 中文：等待合入过程中不要改动这一栈。 -->
   <!-- 中文：第四步里迁移下一项 PR 是唯一允许的分支或目标变更，因为合入时会直接采用分支和目标在那一刻的内容，与已有结论无关。 -->
   <!-- 中文：在 GitHub 上用 /loop 运行原文的 ../skills/poteto-mode/scripts/watch-pr/watch-pr 队列栈监视命令，直到在本轮最高 PR 上报告 COMPLETE。 -->
   <!-- 中文：用原文列出的 stack-prs 选项按从底到顶的顺序传入本轮的 PR 编号；每次重启监视器时传同一份列表。 -->
   <!-- 中文：在 Origin 上，每次原文的持续检查命令返回后都重新查询当前 PR 状态，直到最高 PR 显示已合入。 -->
   <!-- 中文：父 PR 合入后，子 PR 的目标变为主干是预期变化，不要把它当成故障修。 -->
   <!-- 中文：合入停滞时，先查明原因，再改动任何东西。 -->
6. **Stop at the ceiling.** Extending the run takes new verdicts, so it is a new pass through step 1.
   <!-- 中文：到达本轮上限就停止。 -->
   <!-- 中文：扩大合入段需要新的验证结论，要从第一步重新开始一轮。 -->

**Reply:** the verified run and its ceiling, each PR's verdict and who produced it, what landed and how you confirmed it, and what the next gap needs.
<!-- 中文：回复：本轮通过验证的连续区间及上限、各 PR 的结论和验证人、实际合入内容及确认方法，以及下一个断点还缺什么。 -->

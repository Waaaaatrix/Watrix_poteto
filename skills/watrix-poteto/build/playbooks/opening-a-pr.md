### Opening a PR
<!-- 中文：创建 PR -->

Invoked at the end of every other playbook.
<!-- 中文：在其他每个 playbook 结束时调用。 -->

Work from a git worktree off trunk, and keep unrelated work out of it. Rebase into small, landable commits. Run `/deslop` from `cursor-team-kit` over the diff before commit.
<!-- 中文：从主干创建 Git worktree，无关工作不要混入。 -->
<!-- 中文：用 rebase 整理出小而可合入的提交。 -->
<!-- 中文：提交前对 diff 运行 cursor-team-kit 的 /deslop。 -->

Title: `type(scope): subject`, naming a real symbol when one carries the change. Body: a one-minute briefing under 40 lines with these `##` headings in order, dropping empty ones: `## Why`, `## What changed`, `## Scope`, `## Tradeoffs` (only rejected alternatives a reviewer would otherwise ask about), `## Blast Radius`, `## Verification`.
<!-- 中文：标题用 type(scope): subject；改动集中在实际符号上时，标题写出该符号。 -->
<!-- 中文：正文写成不到 40 行、可在一分钟读完的简报，按原文所列顺序用二级标题：原因、改动、范围、取舍、影响面、验证；空项删掉，取舍只列评审者可能追问的弃选方案。 -->

Stack follow-ups by base branch. Open PRs ready, through the run's PR tool with `draft: false`. Without one, use `gh pr create` without `--draft`, or `origin pr create --status open` when Origin's CLI resolves the repository. Opening a PR does not start a babysit.
<!-- 中文：后续改动按基分支叠加。 -->
<!-- 中文：通过任务的 PR 工具创建可供评审的 PR，将 draft 设为 false。 -->
<!-- 中文：没有该工具时，用 gh pr create 且不传 draft 选项，或在 Origin CLI 能识别仓库时，用 origin pr create 创建 open 状态的 PR。 -->
<!-- 中文：创建 PR 不会启动 babysit。 -->

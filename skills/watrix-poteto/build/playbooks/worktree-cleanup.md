### Worktree and simulator cleanup
<!-- 中文：清理 Worktree 和模拟器 -->

1. Snapshot and audit. Record `df -h /`, then run `../skills/poteto-mode/scripts/worktree-audit.sh [-b <pr-base-ref>] [repo-path]` (principle-build-the-lever). Without `-b` it checks `MERGED` against `origin/HEAD`, so pass `-b origin/<branch>` when PRs target another branch. It reads paths from `git worktree list`, never hand-typed, since a hand-typed `myrepo-worktrees/x` misses one that lives at `.cursor/worktrees/myrepo/x` (principle-encode-lessons-in-structure). It classifies each worktree by size, age, merge state, uncommitted work, PR state, and the newest chat that touched it, then suggests a bucket. The transcript scan is slow, so background it.
   <!-- 中文：先记录快照并审计。 -->
   <!-- 中文：记录 df -h /，再运行原文列出的 ../skills/poteto-mode/scripts/worktree-audit.sh 命令，对应 principle-build-the-lever。 -->
   <!-- 中文：不带 -b 时，以 origin/HEAD 判断是否已合入；若 PR 的目标是其他分支，传入 -b origin/<branch>。 -->
   <!-- 中文：路径从 git worktree list 获取，不能手填，以免遗漏在 .cursor/worktrees/myrepo/x 等其他位置的 worktree，对应 principle-encode-lessons-in-structure。 -->
   <!-- 中文：工具按大小、年龄、合入状态、未提交工作、PR 状态及最近触及该 worktree 的对话来分类，再建议处理方式。 -->
   <!-- 中文：扫描会话记录较慢，放到后台执行。 -->
2. The bucket is advice, not permission. The pinned and active chats are the real artifact (principle-prove-it-works). Get that set from the user or sidebar and cross-check every candidate. The lever has marked `safe` a worktree the user had pinned, so the pinned set wins.
   <!-- 中文：分类是建议，不是许可。 -->
   <!-- 中文：置顶或活跃的对话才是核实使用状态的真实证据，对应 principle-prove-it-works。 -->
   <!-- 中文：从用户或侧边栏获取这些对话，再逐个核对候选 worktree。 -->
   <!-- 中文：工具曾把用户置顶对话使用的 worktree 标成 safe，因此以置顶对话为准。 -->
3. Verify usage before deleting. For every `verify-recent-chat` row, or anything you doubt, fan subagents out to read the transcripts and report whether the chat is pinned or ongoing and which worktrees it touches (principle-guard-the-context-window, transcripts are bulk). A pinned chat spawns arena and repro trees into sibling worktrees via background subagents, and those are in use even when their names never hit the sidebar. Pick each subagent's reasoning level for the job, per the Subagents section.
   <!-- 中文：删除前核实使用情况。 -->
   <!-- 中文：每个 verify-recent-chat 条目或有疑问的条目，都分派子 Agent 阅读会话记录，确认是否置顶或还在进行，以及它使用哪些 worktree；会话记录篇幅较大，对应 principle-guard-the-context-window。 -->
   <!-- 中文：置顶对话可以通过后台子 Agent 在同级 worktree 中创建 arena 或复现用的工作树，即使这些名字没显示在侧边栏，也算正在使用。 -->
   <!-- 中文：按 Subagents 一节，为每个子 Agent 选适合任务的推理级别。 -->
4. Pause on irreversible loss. `wip:N` is N tracked uncommitted edits. Show the diff and get a decision first, since removing a clean worktree is recoverable from its branch but uncommitted work is gone. `scratch:N` is untracked throwaway, safe to drop, but name the files. Per Autonomy, clean and merged and not-in-use proceeds. `wip` and in-use pause.
   <!-- 中文：会造成不可逆丢失时暂停。 -->
   <!-- 中文：wip:N 表示有 N 项受跟踪但未提交的修改。 -->
   <!-- 中文：先展示 diff 并取得决定，因为干净的 worktree 可从分支恢复，未提交改动删除后则会丢失。 -->
   <!-- 中文：scratch:N 指不受跟踪的临时内容，可以丢弃，但要列出文件。 -->
   <!-- 中文：按 Autonomy，一般只继续处理干净、已合入且未被使用的 worktree。 -->
   <!-- 中文：有 wip 或仍在用的先停下。 -->
5. Prune the confirmed set. Per path, `git worktree remove --force <path>`. If the dir survives on ignored build artifacts, `rm -rf` it, then `git worktree prune`. Branch refs survive, so no commits are lost. Confirm with `df -h /` and re-list.
   <!-- 中文：清理确认过的 worktree。 -->
   <!-- 中文：对每个路径执行原文列出的 git worktree remove 强制移除命令。 -->
   <!-- 中文：如果忽略的构建产物使目录残留，按原文删除残留目录，再用 git worktree prune 清理记录。 -->
   <!-- 中文：分支引用会保留，所以不会丢提交。 -->
   <!-- 中文：再查 df -h / 并重新列目录确认。 -->
6. Simulators and other reclaimers. Simulators are usually the next-biggest win. `xcrun simctl --set testing delete all` (XCTestDevices clones), `xcrun simctl delete unavailable`, and `xcrun simctl runtime list` then `runtime delete <id>` for old runtimes. More when needed: Xcode `DerivedData` and `iOS DeviceSupport`, `~/Library/Application Support/Cursor` (`state.vscdb.backup`, and `snapshots/roots/<root>` where a `<root>` named for a folder you opened as a workspace balloons), package caches (pnpm, uv, brew, yarn). Clear only caches the user has not said to keep.
   <!-- 中文：处理模拟器和其他可回收占用。 -->
   <!-- 中文：模拟器通常是下一大可回收项。 -->
   <!-- 中文：按原文用 xcrun simctl 清理 XCTestDevices 克隆、不可用实例及旧 runtime；删旧 runtime 前先列清单。 -->
   <!-- 中文：必要时再处理 Xcode 的 DerivedData、iOS DeviceSupport、Cursor 的 state.vscdb.backup 和 workspace 快照目录，以及 pnpm、uv、brew、yarn 等包缓存；以打开过的工作区命名的快照目录可能很大。 -->
   <!-- 中文：只清理用户没有要求保留的缓存。 -->

This is the one playbook that deletes user state with no code review to catch a slip, so the gates above are the review.
<!-- 中文：这是唯一会删除用户状态、又没有代码评审来发现失误的 playbook，所以以上关卡承担评审作用。 -->

**Reply:** `df -h /` before and after with space reclaimed, the worktrees pruned, and a one-line reason for each held back (in-use by which chat, or uncommitted work).
<!-- 中文：回复：清理前后的 df -h / 和释放空间、已删除的 worktree，以及每个留存项的一行理由，例如被哪个对话使用，或有未提交工作。 -->

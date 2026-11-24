### Pause safely
<!-- 中文：安全暂停 -->

**You own a clean stop. Leave a checkpoint a cold-start agent can resume from.** This is explicit only. On "keep going", "going to bed, keep going", or "don't stop", do not pause.
<!-- 中文：你负责让任务在干净的状态下停下来。 -->
<!-- 中文：留下检查点，让一个没有此前上下文的 Agent 也能继续。 -->
<!-- 中文：只在明确收到暂停指令时使用。 -->
<!-- 中文：用户说继续、去睡了但继续，或别停时，不要暂停。 -->

1. Stop at a safe boundary. Finish the current atomic step or back out of it. Start nothing new, and cancel any nested subagents.
   <!-- 中文：在安全边界停下。 -->
   <!-- 中文：做完当前不可再分的一步，或者撤回这一步。 -->
   <!-- 中文：不开新任务，并取消下层子 Agent。 -->
2. Take no irreversible action to pause. No PR and no push unless you already had one out.
   <!-- 中文：为暂停而采取的动作不得不可逆。 -->
   <!-- 中文：不新开 PR，也不推送，除非已有对外的 PR。 -->
3. Make the work durable. Commit uncommitted edits as one clear `wip:` commit on the current branch so nothing is lost. If the tree is broken, say so in the commit body in one line.
   <!-- 中文：把工作持久化。 -->
   <!-- 中文：在当前分支用一条清楚的 wip: 提交保存未提交的修改，避免丢失。 -->
   <!-- 中文：如果工作树当前无法正常运行，在提交正文里用一行说明。 -->
4. Write the resume note off-context. Capture intent, what you were doing, progress and what's verified, current state, next steps, key files, and gotchas. For the compaction trigger write it to a file like `/tmp/<slug>-resume.md`. If a show-me-your-work trail exists, point at it instead of duplicating it.
   <!-- 中文：把恢复笔记写在当前对话上下文之外。 -->
   <!-- 中文：记录目标、正在做什么、进展和已验证事项、当前状态、下一步、关键文件及注意事项。 -->
   <!-- 中文：如果因上下文压缩触发，就写到类似 /tmp/<slug>-resume.md 的文件里。 -->
   <!-- 中文：如果已有 show-me-your-work 记录，直接引用，不要重复写。 -->

**Reply:** where you are in the loop, what's on disk versus still in your head (paths, no diff dumps), the commits you made and whether the tree is clean, and the first action on resume. This is a pause, not a final report.
<!-- 中文：回复：流程进行到哪一步，哪些内容已落盘、哪些还仅留在上下文（只给路径，不贴整段 diff），做过哪些提交，工作树是否干净，以及恢复时的首个动作。 -->
<!-- 中文：这是暂停说明，不是结项报告。 -->

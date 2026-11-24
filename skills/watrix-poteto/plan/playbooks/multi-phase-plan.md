### Multi-phase or multi-PR plan
<!-- 中文：多阶段或多 PR 计划 -->

**You own the plan, not the code. The plan is a checklist an owner runs box by box and the operator audits from the evidence.** The plan is the deliverable. Do not implement.
<!-- 中文：你负责计划，不负责写代码。 -->
<!-- 中文：计划是一份由负责人逐项执行、由操作者根据证据审核的清单。 -->
<!-- 中文：交付物是计划。 -->
<!-- 中文：不要实施。 -->

1. When the change is one or two files with an obvious approach, skip the plan. Say so and stop.
   <!-- 中文：只涉及一两个文件，且做法明显时，跳过计划。 -->
   <!-- 中文：说明原因，然后停止。 -->
2. Settle open questions by prototype before you write, per `prototype.md`. Ask the operator only about a product or preference call that no run can settle.
   <!-- 中文：写计划前，按 prototype.md 先用原型解决未决问题。 -->
   <!-- 中文：只有实测无法判定的产品或偏好选择才问操作者。 -->
3. Write the plan file. Open with what changes, the PR ids in landing order, and the dependency graph, which says which PRs are independent and branch from trunk, which wait for which, and the files each may touch. Then one section per PR. One PR is one change with its own evidence (the **sequence-verifiable-units** principle skill). Each section has checkboxes for what it depends on, the files it edits, the one change it builds, the result you see, and its verification.
   <!-- 中文：写计划文件。 -->
   <!-- 中文：开头说明改动内容、按合入顺序排列的 PR 编号和依赖图；标出哪些 PR 独立、从主干分出，哪些要等待前序 PR，以及各自可修改的文件。 -->
   <!-- 中文：随后每个 PR 单列一节。 -->
   <!-- 中文：每个 PR 只做一项改动，并有独立证据，对应 sequence-verifiable-units 原则。 -->
   <!-- 中文：各节列出依赖、修改文件、唯一改动、可观察结果及验证方法，并用复选框标记。 -->
4. Specify only the checks needed for each PR. Name the scenario and what counts as a pass. For a performance check, name the metric, workload, baseline, and pass threshold. A PR that changes an interaction waits for the operator's review before merge.
   <!-- 中文：只通过测试不算验证充分。 -->
   <!-- 中文：单元测试、实际场景验证和性能三项都打勾，PR 才算验证通过。 -->
   <!-- 中文：实际场景要操作真正的界面或系统，写明具体场景和通过条件。 -->
   <!-- 中文：性能项要写清指标、测量方法、先测得的主干基线及判定失败的数值。 -->
   <!-- 中文：改动交互的 PR 必须等操作者评审后再合入。 -->
5. Hand back the plan path and stop. Execution starts on the operator's explicit go.
   <!-- 中文：交回计划路径，到此为止。 -->
   <!-- 中文：只有操作者明确同意后才开始执行。 -->

**Reply:** the plan path, the PR ids with their dependencies and the review-gated set, and what stays unproven.
<!-- 中文：回复：计划路径、带依赖关系的 PR 编号、需要人工评审的 PR，以及尚未证实的事项。 -->

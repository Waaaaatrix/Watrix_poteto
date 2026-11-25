### Refactoring
<!-- 中文：重构 -->

**You own the contract. The structure changes. The behavior does not.** Distinct from Feature, which adds behavior, and Bug fix, which corrects it.
<!-- 中文：你负责维持行为契约。 -->
<!-- 中文：结构会改变。 -->
<!-- 中文：行为不变。 -->
<!-- 中文：这与增加行为的 Feature、修正行为的 Bug fix 不同。 -->

If the cleanup reveals a missing feature or a real bug, split it out and ship the structural change first against the pinned contract. A redesign is allowed, but name it and route to Feature. Large or cross-cutting structural work belongs to the **figure-it-out** skill. This playbook is the focused-to-medium change.
<!-- 中文：如果清理时发现缺失功能或真实缺陷，把它拆出来，先按已固定的契约交付结构改动。 -->
<!-- 中文：可以重新设计，但要明确指出，并转到 Feature。 -->
<!-- 中文：规模大或横跨多个模块的结构调整，用 figure-it-out。 -->
<!-- 中文：本流程面向集中到中等规模的改动。 -->

1. Pin the behavior contract first. Trace the affected subsystem to learn the contract, then write a characterization test, snapshot, or equivalence harness that captures current behavior before any structure moves. If the area has no coverage, write the pin before touching structure. Type check and lint are not a pin.
   <!-- 中文：先固定行为契约。 -->
   <!-- 中文：追踪受影响的子系统以了解契约，再在改结构前写现状行为测试、快照或等价性验证工具，记录当前行为。 -->
   <!-- 中文：如果没有覆盖，要先固定行为，后动结构。 -->
   <!-- 中文：类型检查和 Lint 不能固定行为契约。 -->
2. Name the structure the code is missing per **principle-model-the-domain**. Boring code stays when the shape is already clear and local. The reshape must delete branches or invalid states, not add indirection.
   <!-- 中文：按 principle-model-the-domain，明确代码欠缺的结构。 -->
   <!-- 中文：如果现有结构已清楚且局部，普通直白的代码就留下。 -->
   <!-- 中文：结构调整必须能删去分支或非法状态，不能只增加间接层。 -->
3. Name the target shape. State what the module layout, types, and call graph should be if built today (**principle-foundational-thinking**, **principle-redesign-from-first-principles**). If the target crosses a function boundary, run the **architect** skill for the shape before the move.
   <!-- 中文：写清目标结构。 -->
   <!-- 中文：说明如果今天重新构建，模块布局、类型和调用图应当是什么样，对应 principle-foundational-thinking 和 principle-redesign-from-first-principles。 -->
   <!-- 中文：目标跨函数边界时，先用 architect 确定形态再搬动。 -->
4. Subtract before you add. Delete dead code, collapse one-caller wrappers, drop redundant validators, and remove orphan references before introducing the new shape (**principle-subtract-before-you-add**). The smallest change that reaches the target shape ships (**principle-laziness-protocol**). A speculative cleanup that "might help" gets reverted.
   <!-- 中文：先减后加。 -->
   <!-- 中文：在引入新结构前，删掉死代码、合并只有一个调用者的包装函数、去掉冗余校验和孤立引用，对应 principle-subtract-before-you-add。 -->
   <!-- 中文：只交付能达到目标结构的最小改动，对应 principle-laziness-protocol。 -->
   <!-- 中文：撤回「也许有用」的猜测性清理。 -->
5. Move in small behavior-preserving steps, each keeping the pin green. For API reshapes, migrate every caller and delete the old API in the same wave (**principle-migrate-callers-then-delete-legacy-apis**). No compatibility shims, no parallel old-and-new paths. Spot-check every rename against the actual files. Renames silently miss usages in strings, prose, and back-references. Delegate the mechanical edits to a subagent using your configured refactoring model (see the [code default](../skills/poteto-mode/skill.md#subagents)) with a specific scope (file paths, the names being moved, the behavior to hold). Pick each subagent's reasoning level for the job, per the Subagents section.
   <!-- 中文：以保持行为的小步迁移，每一步都保持契约验证通过。 -->
   <!-- 中文：调整 API 时，在同一批次迁移所有调用方，并删除旧接口，对应 principle-migrate-callers-then-delete-legacy-apis。 -->
   <!-- 中文：不加兼容垫片，不保留并行的新旧路径。 -->
   <!-- 中文：每次重命名后，抽查实际文件。 -->
   <!-- 中文：重命名容易漏掉字符串、文档和反向引用。 -->
   <!-- 中文：用配置的 refactoring 模型把机械修改委派给子 Agent，默认是原文列出的 Opus 模型，明确文件、要移动的名称和需保持的行为。 -->
   <!-- 中文：按 Subagents 一节，为每个子 Agent 选适合任务的推理级别。 -->
6. Prove behavior is unchanged on the real artifact, not "it compiles" (**principle-prove-it-works**). For larger reshapes, run an equivalence check: a script that diffs old-vs-new outputs, a recorded baseline replayed against the new code, or a smoke run on the matching surface via the relevant control skill.
   <!-- 中文：在实际产物上证明行为没变，不能以能编译代替验证，对应 principle-prove-it-works。 -->
   <!-- 中文：较大调整要跑等价性检查：用脚本比较新旧输出、把已录制的基线在新代码上回放，或用相应操控技能在匹配的场景跑冒烟验证。 -->
7. Confirm the change is worth keeping. The success measure is reduced reader load (**principle-minimize-reader-load**). If the diff does not lower reader load somewhere, revert it.
   <!-- 中文：确认改动值得保留。 -->
   <!-- 中文：成功指标是读者负担降低，对应 principle-minimize-reader-load。 -->
   <!-- 中文：如果 diff 没有在任何地方降低阅读负担，就撤回。 -->
8. Rebase into small ordered commits. A subtraction commit, then the reshape, then any follow-on cleanup. Shape them with the **sequence-verifiable-units** principle skill, so each behavior-preserving slice stays green before the next. Run **Opening a PR**.
   <!-- 中文：通过 rebase 整理成小且有序的提交。 -->
   <!-- 中文：先提交删减，再提交结构调整，最后提交后续清理。 -->
   <!-- 中文：用 sequence-verifiable-units 原则组织提交，确保每个保留行为的切片都验证通过后再做下一步。 -->
   <!-- 中文：执行 Opening a PR 流程。 -->

**Reply:** the structure that changed, the pin you held it against, the equivalence proof, the reader-load delta, what shipped and what got reverted. No new behavior.
<!-- 中文：回复：改了什么结构、固定了什么契约、等价证据、阅读负担的变化、交付了什么和撤回了什么。 -->
<!-- 中文：不引入新行为。 -->

### Eval
<!-- 中文：评估实验 -->

**You own the experiment design. Plan, blind, run, synthesize.**
<!-- 中文：你负责实验设计。 -->
<!-- 中文：规划、设置盲法、运行并综合结果。 -->

**Non-negotiables for blinding:**
<!-- 中文：盲法的硬性要求： -->

- No `eval`, `test`, `judge`, `experiment`, `rubric`, `score`, `compare`, `benchmark`, `candidate`, or `arena` in any directory, file, or prompt the candidate sees.
  <!-- 中文：候选执行者能看到的目录、文件或提示词里，不得出现 eval、test、judge、experiment、rubric、score、compare、benchmark、candidate 或 arena。 -->
- The candidate prompt looks like an organic user request. State the goal, not the meta.
  <!-- 中文：候选提示词应像正常的用户请求。 -->
  <!-- 中文：只写目标，不泄露这是一次评估。 -->
- No chain-eliciting cues. Don't ask the candidate to list which skills, principles, or files they applied. Ask for design notes generally and grade chain-following from code shape, not self-report.
  <!-- 中文：不要提示候选执行者暴露工作链路。 -->
  <!-- 中文：不要让它列出自己用了哪些 Skill、Principle 或文件。 -->
  <!-- 中文：可以泛泛地要设计说明，但应从实际代码结构判断它是否遵循流程，不能靠自述评分。 -->
- Sanitize directory and slug names. Use project-shaped names a user might pick.
  <!-- 中文：清理目录名和标识名称。 -->
  <!-- 中文：用真实用户可能为项目取的名字。 -->
- Don't tell the candidate other candidates exist.
  <!-- 中文：不要告诉候选执行者还有其他候选。 -->
- The judge can know it's judging but sees outputs by sanitized label only, never by model name.
  <!-- 中文：评审者可以知道自己正在评分，但它看到的输出只能用匿名标签标识，不能看到模型名。 -->
- Comparing two variants: one judge scores both sets in a single pass on one scale, blind to which set each came from.
  <!-- 中文：比较两个变体时，只用一名评审者一次看完两组结果，用同一量表评分，不知道各组来自哪个变体。 -->

**Steps:**
<!-- 中文：步骤： -->

1. **Frame.** State what variant is under test and what behavior counts as success. Write the rubric (3-6 concrete criteria) for the judge only. Hold it back from candidates.
   <!-- 中文：界定实验。 -->
   <!-- 中文：明确被测变体和作为成功标准的可观察行为。 -->
   <!-- 中文：给评审者写 3 到 6 条具体评分标准。 -->
   <!-- 中文：这些标准不提供给候选执行者。 -->
2. **Set up sanitized environments.** Per-candidate working dir with the variant in place. Plant any context an organic task would have: a project skeleton, the skills the candidate would naturally read.
   <!-- 中文：准备去标识的工作环境。 -->
   <!-- 中文：每个候选执行者使用独立工作目录，其中放入指定变体。 -->
   <!-- 中文：放入真实任务通常具备的上下文，例如项目骨架和候选方自然会读的 Skill。 -->
3. **Author one organic prompt.** What a user would type. No leakage of what's being measured.
   <!-- 中文：写一份自然的提示词。 -->
   <!-- 中文：用用户平常会输入的表达。 -->
   <!-- 中文：不要泄露评估指标。 -->
4. **Spawn N parallel candidates** on different models per the **arena** skill's Phase B. Each works in its own sanitized dir. Same prompt to each. Pick each subagent's reasoning level for the job, per the Subagents section. An agent that only dispatches, polls, or packs results runs on `claude-opus-5-5-medium-fast`.
   <!-- 中文：按 arena 的 Phase B，在不同模型上并行启动 N 个候选。 -->
   <!-- 中文：每个候选在独立的去标识目录工作。 -->
   <!-- 中文：每人收到同一个提示词。 -->
   <!-- 中文：按 Subagents 一节，为每个子 Agent 选适合任务的推理级别。 -->
   <!-- 中文：只负责分派、轮询或打包结果的 Agent 用 claude-opus-5-5-medium-fast。 -->
5. **Spawn one blinded judge** on a different model family per the **arena** skill's Phase C. Judge sees outputs by sanitized label and the rubric, never a model name.
   <!-- 中文：按 arena 的 Phase C，另选一个不同模型家族的盲评 Agent。 -->
   <!-- 中文：评审者只看到匿名标签对应的输出和评分标准，看不到模型名。 -->
6. **Verify the chain from transcripts, not self-report.** Read each candidate's local transcript in the active workspace's session directory (in Cursor, the `agent-transcripts/` directory the system prompt names. Other harnesses: see **Harness** in SKILL.md). Do not glob across every project's directory. That crosses workspace boundaries and reads private chats from unrelated projects. Look at which files each candidate actually opened. Grade chain-following from the files it really read plus the shape of the code, never from the candidate's own claims.
   <!-- 中文：根据会话记录核实流程，不靠候选自报。 -->
   <!-- 中文：只读当前工作区会话目录中各候选的本地记录；Cursor 用系统提示指向的 agent-transcripts 目录，其他宿主参见 ../../build/skills/poteto-mode/skill.md 的 Harness 一节。 -->
   <!-- 中文：不要遍历所有项目的会话目录。 -->
   <!-- 中文：这样会越过工作区边界，读到不相关项目的私有对话。 -->
   <!-- 中文：查清各候选实际打开了哪些文件。 -->
   <!-- 中文：根据实际读取的文件和代码结构判断是否遵循流程，不接受候选自身的说法作为证据。 -->
7. **Read every candidate output yourself** end to end. Compare to the judge's verdict. Disagreement means a model is biased or the rubric is ambiguous. Synthesize.
   <!-- 中文：亲自完整读完每个候选输出。 -->
   <!-- 中文：和评审者的结论比较。 -->
   <!-- 中文：出现分歧说明模型可能带有偏差，或评分标准不够明确。 -->
   <!-- 中文：汇总证据与结论。 -->

**Reply:** variant under test, rubric, per-candidate notes, judge's verdict, your synthesis, and a recommendation for whether to promote the variant.
<!-- 中文：回复：被测变体、评分标准、各候选记录、评审结论、综合判断，以及是否采用这一变体的建议。 -->

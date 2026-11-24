### Authoring or modifying a skill
<!-- 中文：编写或修改 Skill -->

**You own the skill's voice.**
<!-- 中文：你负责把握 Skill 的表达方式。 -->

1. Use your skill-authoring skill: **create-skill** in Cursor (built in), or Anthropic's `skill-creator` elsewhere. With neither, follow the Agent Skills format at agentskills.io.
   <!-- 中文：用宿主的 Skill 编写工具：Cursor 内置 create-skill，其他宿主用 Anthropic 的 skill-creator。 -->
   <!-- 中文：两者都没有时，遵循 agentskills.io 上的 Agent Skills 格式。 -->
2. Validate the skill: frontmatter has `name` and `description`, referenced files exist, cross-skill links resolve.
   <!-- 中文：验证 Skill：frontmatter 包含 name 和 description，引用文件存在，跨 Skill 的链接能解析。 -->
3. Test cases if structural. Skip if subjective.
   <!-- 中文：涉及结构时准备测试用例。 -->
   <!-- 中文：主观内容可跳过。 -->
4. Run **Opening a PR**.
   <!-- 中文：执行 Opening a PR 流程。 -->

When in doubt, delete. Keep only prose that changes a decision. Tell it to do the thing and skip the reason. Explain only when the rule is confusing without one. Match tone to scope. Point at structural sources (types, READMEs, config) per the **encode-lessons-in-structure** principle skill. Delegate to other skills by path. Don't restate. A workflow you keep hitting but isn't captured → propose a new skill.
<!-- 中文：拿不准就删。 -->
<!-- 中文：只保留会改变决策的文字。 -->
<!-- 中文：直接告诉 Agent 要做什么，不写原因。 -->
<!-- 中文：只有规则不解释就难以理解时再解释。 -->
<!-- 中文：表达语气要和任务范围匹配。 -->
<!-- 中文：按 encode-lessons-in-structure 原则，指向类型定义、README 或配置等结构化来源。 -->
<!-- 中文：按路径调用其他 Skill。 -->
<!-- 中文：不要把它的内容重写一遍。 -->
<!-- 中文：反复遇到尚未记录的工作流时，提出新 Skill。 -->

**Reply:** summary of the skill, key design decisions, validation notes.
<!-- 中文：回复：Skill 概要、关键设计决策及验证说明。 -->

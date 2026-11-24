### Investigation
<!-- 中文：代码调查 -->

**You own the answer. Plan, route, write.**
<!-- 中文：你负责给出答案。 -->
<!-- 中文：规划调查、选择方法、写出结论。 -->

Investigation requests are read-only. They produce a cited explanation or a recommendation, not a code change.
<!-- 中文：调查任务是只读的。 -->
<!-- 中文：交付物是带出处的解释或建议，不是代码改动。 -->

1. Trace the code the question is about. For motivation questions, also route through the **why** skill.
   <!-- 中文：追踪问题涉及的代码。 -->
   <!-- 中文：如果问题涉及设计动机，也调用 why 技能。 -->
2. Throughput checkpoint stays one line: `throughput checkpoint: n/a, read-only investigation`.
   <!-- 中文：吞吐检查只写一行：不适用，本次是只读调查。 -->
3. Produce the output (Overview / Key Concepts / How It Works / Where Things Live / Gotchas), or a recommendation with a tradeoffs table if the request is a decision between alternatives.
   <!-- 中文：按概览、关键概念、工作方式、代码位置和注意事项组织输出；如果用户要在多个选项中作决定，就给出建议和取舍表。 -->
4. Apply the **unslop** skill to the reply.
   <!-- 中文：用 unslop 技能精简回复。 -->

No PR, no babysit, no `architect` unless the investigation precedes a code change. If it does, hand back to the user and re-route to Bug fix or Feature.
<!-- 中文：不开 PR，不跟进 PR，不调用 architect；如果调查将引出代码改动，则适用例外。 -->
<!-- 中文：出现这种情况时，把调查结果交回用户，再转入 Bug fix 或 Feature 流程。 -->

**Reply:** the investigation output. For "are we sure?" answers, include your real judgment with reasons. Push back if the premise is wrong (see Autonomy).
<!-- 中文：回复：给出调查结果。 -->
<!-- 中文：回答「我们确定吗」时，写出你自己的判断及理由。 -->
<!-- 中文：如果前提有误，直接指出，参见 Autonomy 一节。 -->

### Runtime forensics
<!-- 中文：运行现场诊断 -->

**You own the diagnosis. Instrument the live process, don't theorize from source.** The deliverable is a cited diagnosis, not a fix.
<!-- 中文：你负责诊断。 -->
<!-- 中文：对运行中的进程插桩，别只根据源码推测。 -->
<!-- 中文：交付物是带证据出处的诊断，不是修复。 -->

1. Capture the live signal on the matching surface via the control skill: a CPU profile for a spinning process, a heap snapshot for a leak, a CDP trace for a visual glitch. A real artifact, not a guess.
   <!-- 中文：通过对应的操控技能，在问题实际发生的界面或环境采集信号：进程空转采 CPU profile，泄漏采堆快照，界面异常采 CDP trace。 -->
   <!-- 中文：要拿到真实产物，不能靠猜。 -->
2. Reduce the artifact to the smoking gun: the function on the hot path, the retainer chain from the leaked object to a GC root, the loop firing without input. Parse large artifacts in a subagent (the **guard-the-context-window** principle skill), keep the reduced finding in the main thread. Pick each subagent's reasoning level for the job, per the Subagents section.
   <!-- 中文：从产物中提炼决定性证据：热点路径上的函数、泄漏对象到 GC root 的引用链，或没有输入却触发的循环。 -->
   <!-- 中文：由子 Agent 解析大产物，主对话只保留提炼后的发现，对应 guard-the-context-window 原则。 -->
   <!-- 中文：按 Subagents 一节为每个子 Agent 选适合任务的推理级别。 -->
3. Prove the mechanism before believing it. Inject instrumentation via CDP eval on the running process, or hotfix the live code without reloading, to confirm the hypothesis cheaply.
   <!-- 中文：先用证据证明机制，再采信。 -->
   <!-- 中文：通过 CDP eval 对运行进程临时插桩，或不重载地临时修正运行中的代码，低成本验证假设。 -->
4. Map the finding back to source: file, symbol, the line that allocates or schedules.
   <!-- 中文：把发现对应到源码：文件、符号，以及实际分配对象或调度任务的行。 -->
5. Throughput checkpoint stays one line: `throughput checkpoint: n/a, read-only forensics`.
   <!-- 中文：吞吐检查只写一行：不适用，本次是只读取证。 -->

**Reply:** the signal captured, the reduced finding, how you proved the mechanism, the source location, artifact paths. No fix unless asked. Hand back to Bug fix or Perf once the cause is known.
<!-- 中文：回复：采集的信号、提炼后的发现、机制的证明方式、源码位置及产物路径。 -->
<!-- 中文：用户没要求就不修复。 -->
<!-- 中文：查明原因后，交给 Bug fix 或 Perf 流程。 -->

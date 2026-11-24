### Trace forensics
<!-- 中文：已有跟踪记录分析 -->

**You own the diagnosis from the artifact.**
<!-- 中文：你负责根据产物给出诊断。 -->

Distinct from **Runtime forensics**, which instruments the live process. Here the capture already exists. The artifact is a fixed dataset, read it, don't re-run it. Keep tooling generic: a DevTools or trace parser for cpuprofile and `.json.gz`, a text editor for a spindump, your heap tooling for a heapsnapshot.
<!-- 中文：Runtime forensics 针对运行中的进程插桩，本流程与它不同。 -->
<!-- 中文：这里已经有采集产物。 -->
<!-- 中文：把产物当作固定数据集读取，不重新运行采集。 -->
<!-- 中文：使用通用工具：用 DevTools 或 trace 解析器处理 cpuprofile 和 .json.gz，用文本编辑器查 spindump，用堆分析工具处理 heapsnapshot。 -->

1. Identify the format and load it with the right tool. Parse large artifacts in a subagent (the **principle-guard-the-context-window** skill) and keep the reduced finding in the main thread. Pick each subagent's reasoning level for the job, per the Subagents section.
   <!-- 中文：识别格式，用合适的工具载入。 -->
   <!-- 中文：由子 Agent 解析大产物，主对话只保留提炼后的发现，对应 principle-guard-the-context-window。 -->
   <!-- 中文：按 Subagents 一节为每个子 Agent 选适合任务的推理级别。 -->
2. Transform the raw artifact into a form you can query. Dump the trace or heap snapshot into sqlite, one row per sample, frame, or node. Reach the queryable shape before you read.
   <!-- 中文：把原始产物转换成可以查询的数据。 -->
   <!-- 中文：把 trace 或堆快照导入 SQLite，每条采样、栈帧或节点各占一行。 -->
   <!-- 中文：先把数据整理成可查询的形式，再分析。 -->
3. Narrow to the cause. Query for the frames that hold the most time and walk the call tree to the hot path. For a leak, follow the retainer chain from the leaked object to a GC root. For a spindump, find the thread stuck on-CPU or blocked and its wait reason.
   <!-- 中文：逐步缩小到原因。 -->
   <!-- 中文：查找耗时最多的栈帧，沿调用树找到热点路径。 -->
   <!-- 中文：排查泄漏时，沿引用链从泄漏对象追到 GC root。 -->
   <!-- 中文：分析 spindump 时，找出持续占用 CPU 或被阻塞的线程，并查清等待原因。 -->
4. Attribute to source. Map the hot frame to file, symbol, and line via the artifact's own symbols. A frame with no source mapping is not yet a diagnosis. Resolve the symbols, or say plainly the artifact does not carry them.
   <!-- 中文：定位到源码。 -->
   <!-- 中文：根据产物自身的符号，把热点栈帧对应到文件、符号和行号。 -->
   <!-- 中文：只有栈帧、没有源码位置，还不足以定诊。 -->
   <!-- 中文：解析符号；若产物没有符号，就明确说明。 -->
5. If you have a paired capture, diff the before and after artifacts to confirm. Otherwise, mark the finding as the strongest hypothesis the artifact supports, not a confirmed cause.
   <!-- 中文：有成对的采集记录时，比较前后产物以确认结论。 -->
   <!-- 中文：没有时，把发现标为产物最支持的假设，不写成已证实的原因。 -->
6. Hand back a cited diagnosis, no fix unless asked. Route to Bug fix or Perf issue once the cause is known. Throughput checkpoint stays one line: `throughput checkpoint: n/a, read-only forensics`.
   <!-- 中文：交回有据可查的诊断，未受委托不修复。 -->
   <!-- 中文：查明原因后转 Bug fix 或 Perf issue。 -->
   <!-- 中文：吞吐检查只写一行：不适用，本次是只读取证。 -->

**Reply:** the artifact and format, the reduced finding, the source location, the artifact paths, and whether a paired capture confirmed it.
<!-- 中文：回复：产物及其格式、提炼后的发现、源码位置、产物路径，以及是否有配对采集验证结论。 -->

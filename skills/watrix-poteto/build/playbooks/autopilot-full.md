### Autopilot-full
<!-- 中文：从构建到合入的全程自动化 -->

**You own the verdicts, never the PRs. One owner carries each PR from build to merge, and nothing merges until your swarm, which never wrote the code, has proven the code that lands.**
<!-- 中文：你负责验证结论，不直接负责各 PR。 -->
<!-- 中文：每个 PR 由一名负责人从实现跟到合入；独立验证组从未写过这些代码，而且必须证明要合入的代码符合要求，之后才能合入。 -->

A verdict covers one patch and green CI covers one head. Step 4 runs a full swarm once, and again only for a major change. The patch-id rule in `shipping.md` can carry a lane's result, and every new head needs its own green CI.
<!-- 中文：一份验证结论只覆盖一个补丁，绿色 CI 只覆盖一个 head。 -->
<!-- 中文：第四步在代码就绪时组织一次完整验证组，之后仅在重大变化时再次组织。 -->
<!-- 中文：shipping.md 的 patch-id 规则允许沿用单个验证分支的结论；每个新 head 仍要有自己的绿色 CI。 -->

1. **The operator's gates come first.** The grant covers only the queue's items, so items the operator names as theirs and PRs outside the queue stop at merge-ready. A request to state the plan is not a go.
   <!-- 中文：优先遵守操作者设定的关卡。 -->
   <!-- 中文：授权仅覆盖队列中的事项，所以操作者指定由自己负责的事项，以及队列之外的 PR，到可合入状态就停止。 -->
   <!-- 中文：要求说明计划，不等于批准执行。 -->
2. **Spawn one owner per PR, and keep its work where a replacement can find it.** Resolve the forge once for the program. GitHub CLI (`gh`) is the default. If `command -v origin` succeeds and Origin can resolve the repository, use `origin pr` for every PR operation. Otherwise stay on `gh` and record the fallback. Never require Graphite (`gt`). Each owner is a Cursor cloud agent that builds, opens its PR per `opening-a-pr.md`, proves the change on the real artifact, babysits to green per `../../review/playbooks/babysit.md`, and merges once steps 4 and 5 clear it. Where that playbook says to report a rebase, the owner rebases its own branch and publishes it with `git push --force-with-lease`.
   <!-- 中文：每个 PR 启动一名负责人，并把工作保存在接替者能找到的地方。 -->
   <!-- 中文：整项工程只确定一次代码托管平台。 -->
   <!-- 中文：默认使用 GitHub CLI（gh）。 -->
   <!-- 中文：系统能找到 origin 命令、且 Origin 能识别仓库时，用 origin pr 做全部 PR 操作。 -->
   <!-- 中文：否则继续用 gh 并记录回退。 -->
   <!-- 中文：不依赖 Graphite（gt）。 -->
   <!-- 中文：每名负责人是一个 Cursor 云端 Agent：实现改动，按 opening-a-pr.md 建 PR，在真实产物上验证，按 ../../review/playbooks/babysit.md 跟进到检查为绿，待第四、五步放行后合入。 -->
   <!-- 中文：如果 babysit 流程要求报告 rebase，负责人自己给自己的分支做 rebase，并按原文用带租约的强制推送发布。 -->

   Open the PR early so CI and bots run alongside the work. The code is ready only after the slop-strip (the `deslop` skill from the `cursor-team-kit` plugin (`/deslop`)) and the `/no-comments` pass, so the full swarm runs on finished code. Then rebase onto current trunk and report the code-ready head before self-proof, so verification runs on the base the patch lands on. Report every later head, and merge-ready once self-proof, CI, and babysit are done. Fix rounds stay on that base unless trunk conflicts, breaks CI, or lands a known concurrent change to the same files, because each rebase restarts CI. Run the repo's checks for the touched paths on the committed head before the code-ready push and before a later push that needs a verifier.
   <!-- 中文：尽早建 PR，让 CI 和机器人检查与开发并行。 -->
   <!-- 中文：代码必须完成 cursor-team-kit 的 deslop 和 /no-comments 检查才算就绪，保证完整验证组检查的是已完成的代码。 -->
   <!-- 中文：随后把分支 rebase 到当前主干，并在负责人自证前报告代码就绪时的 head，使验证建立在补丁实际要落到的基点上。 -->
   <!-- 中文：报告之后的每个 head；自证、CI 和 babysit 全部完成时报告已可合入。 -->
   <!-- 中文：修复轮保持既有基点，除非主干引入冲突、打破 CI，或确定有并行改动涉及同一批文件，因为每次 rebase 都会重启 CI。 -->
   <!-- 中文：推送代码就绪的 head 前，以及后来需要验证人复核的推送前，都先运行仓库中覆盖所改路径的检查，检查对象是已经提交的 head。 -->

   Assume the VM dies. Soon after starting, push and start a decision trail in the root agent's store (path in the system prompt) per the **show-me-your-work** skill, then push each finished unit before you start the next. Whoever spawns a subagent records its id, state, and a stuck-after runtime from past runs of its kind in that store, so the root can probe it if the parent dies. Force-push only your own branch, with `--force-with-lease` pinned to the SHA you last pushed or took over, and on any push you did not make, stop and report to the root.
   <!-- 中文：假定虚拟机可能随时消失。 -->
   <!-- 中文：启动不久就推送并按 show-me-your-work 把决策记录写到根 Agent 的存储位置，路径见系统提示；每个单元结束后先推送，再开始下一个。 -->
   <!-- 中文：启动子 Agent 的一方要记录其 ID、状态，以及基于以往同类运行确定的超时阈值；这样父 Agent 消失后，根 Agent 还能探查它。 -->
   <!-- 中文：只允许在自己的分支上做带租约的强制推送，租约绑定到自己最后推送或接手时的 SHA；发现并非自己发起的推送，就停下并报告根 Agent。 -->
3. **Run owners in parallel off trunk, never stacked,** so each PR lands on its own.
   <!-- 中文：让各负责人并行从主干分出，禁止叠成栈，确保各 PR 可独立合入。 -->
4. **Swarm-verify once.** A full swarm runs once, at the code-ready head. Another full swarm runs only for new behavior, a redesign, or a large new diff. A rebase, a merge-conflict resolution, or a minor fix gets a single verifier at the new head, and a wording-only change gets none. Fan out independent verifiers at the code-ready SHA per the **swarm** skill. They re-run the gates, prove the load-bearing behavior live on the real surface through its control skill (such as `control-ui` or `control-cli` from `cursor-team-kit`) or a named driver, and audit the diff. The verdict is not clean without the live lane, or before you audit the merge-ready report's receipts, because no lane trusts a claim it has not checked.
   <!-- 中文：用验证组集中验证一次。 -->
   <!-- 中文：在代码就绪的 head 上跑一次完整验证组。 -->
   <!-- 中文：只有新增行为、重新设计或出现大规模新 diff 时，才再跑完整验证组。 -->
   <!-- 中文：rebase、解决合并冲突或小修复，在新 head 上只需要一名验证者；纯措辞改动不需要重新派验证者。 -->
   <!-- 中文：按 swarm 流程在代码就绪的 SHA 上分派独立验证者。 -->
   <!-- 中文：他们重跑关卡，通过真实界面上的操控工具或具名驱动程序实测关键行为，再审计 diff；例如可用 cursor-team-kit 的 control-ui 或 control-cli。 -->
   <!-- 中文：缺少实测分支，或尚未审查可合入报告中的证据时，不能作出无问题结论，因为每个验证分支都只采信自己核实过的说法。 -->

   Block a PR only for a problem you can show, with a failing run at the head or a doc line the PR made wrong. A problem the PR's base already has, whether trunk or the PR below it in a stack, doesn't block unless the PR copies it or newly reaches it. Only a bug a user would hit, a failed gate, or a wrong shipped doc blocks, and everything else goes in a follow-up.
   <!-- 中文：只有能指出具体问题时才阻止 PR，并提供当前 head 的失败运行，或被此次 PR 改错的文档行。 -->
   <!-- 中文：PR 的基点上已存在的问题，无论出在主干还是栈中下层 PR，都不能阻止它，除非此 PR 复制了问题或让原本不受影响的路径新触发它。 -->
   <!-- 中文：只有用户实际会遇到的缺陷、失败的关卡或已交付文档中的错误能阻止合入，其余都放入后续事项。 -->
5. **The owner merges only what you verified, tested on the trunk it lands on.** Green CI alone never authorizes a merge. Once the swarm's lanes have started, merge prep rebases onto current trunk, reports the new head, and waits for green CI.
   <!-- 中文：负责人只将已验证的内容合入，并确保测试所用的主干基点就是它要落到的基点。 -->
   <!-- 中文：仅有绿色 CI 从来不构成合入授权。 -->
   <!-- 中文：验证组各分支启动后，合入准备阶段把分支 rebase 到最新主干，报告新 head，并等待 CI 为绿。 -->

   Right before merging, fetch trunk and ask whether its changes can affect the patch, because rebasing for every trunk move never ends. Rebase and wait for CI when the code that would land was never tested, as with a conflict, a file both sides changed, or a change to which checks run for the PR. A lockfile both sides changed needs no rebase when a frozen install passes on the merge result and the packages the PR changes resolve there as on the head, because that install tests the lockfile that would land, and a failing install means the rebase must repair it. When trunk changed a caller of the patch or something the patch depends on, such as code it calls or a dependency version, run the patch's checks on the merge result and merge on a pass. Otherwise merge. If trunk would force a third rebase past merge prep, hold and report to the root, which sequences the colliding work.
   <!-- 中文：合入前立即拉取主干，判断主干的变化是否可能影响补丁，因为每次主干变化都 rebase 会没完没了。 -->
   <!-- 中文：如果真正会落地的代码未被测试，比如存在冲突、双方改了同一个文件，或 PR 所跑的检查发生变化，就 rebase 并等待 CI。 -->
   <!-- 中文：如果双方都改了 lockfile，但合并结果能通过冻结安装，且该 PR 改动包的解析结果与 head 一致，就无需 rebase，因为安装已经检验实际会合入的 lockfile；安装失败就必须通过 rebase 修复。 -->
   <!-- 中文：如果主干改了补丁的调用方，或改了它依赖的东西，比如它调用的代码、依赖版本，就在合并结果上跑这份补丁的检查，通过后才合入。 -->
   <!-- 中文：不属于上述情况时合入。 -->
   <!-- 中文：如果主干会让分支在合入准备之后不得不做第三次 rebase，暂停并报告根 Agent，由它安排相互冲突的工作顺序。 -->
6. **Run the root layer.** Anything that loosens a gate, such as a raised budget or an allowlist entry, needs a verifier's proof and then your fresh countersign, because the owner is the one who wants green. Under a grant that covers approvals, that countersign is the approval, and the owner's record of it points at your countersign for a lane to check. A disabled or soft-failed check always comes out, because it stops guarding every case. Never give or bypass an approval the forge enforces, because no countersign covers it.
   <!-- 中文：负责根层调度。 -->
   <!-- 中文：任何放宽关卡的操作，例如提高预算或新增允许列表条目，都必须先由验证人给证据，再由你重新会签；因为负责人有让检查通过的动机。 -->
   <!-- 中文：如果授权涵盖审批，该会签就是审批；负责人应在记录里指向你的会签供某条验证分支核查。 -->
   <!-- 中文：被禁用或允许软失败的检查始终要撤掉这种设置，因为这样会让它失去对所有情况的约束。 -->
   <!-- 中文：不要代批或绕过托管平台强制要求的审批，没有任何会签能覆盖它。 -->

   On the operator's go, arm `/loop 1h` to audit until no delegated work is left, because long runs drift and agents stall silently. Each tick re-reads this playbook from trunk and fixes drift, such as an unreported head or an unrecorded agent, by having its owner record it, because drift is a gap in the record, not a stall. It probes every agent, down to the leaf lanes, and judges each by its side effects, because status text and summaries can hide a stuck child. An agent is stuck when it errors, cannot start turns, or shows nothing new past its stuck-after runtime, and so is an owner that has neither pushed nor written a trail row by the first tick. One waiting on you or the operator is not stuck, so answer it. Replace a stuck agent at once, without waiting for its stop to land. A tick that changed something for the operator ends by catching them up per the **ketchup** skill, and one that changed nothing stays quiet.
   <!-- 中文：操作者允许开始后，开启每小时一次的 /loop 审计，直到没有委派任务为止，因为长任务会偏离计划，Agent 也可能无声停滞。 -->
   <!-- 中文：每轮重新从主干读本 playbook，将未报告的 head 或未登记的 Agent 等偏差交给负责人补记录，因为这里的偏差是记录缺口，不是停滞。 -->
   <!-- 中文：逐个探查 Agent，直到最底层的执行分支，并以实际产出判断其状态，因为状态文字或摘要可以掩盖子任务卡住。 -->
   <!-- 中文：Agent 报错、无法开始新轮次，或超过所定超时仍没有新进展时，算卡住；首轮审计时既未推送、也未写记录的负责人也算卡住。 -->
   <!-- 中文：等你或操作者决定的 Agent 并没有卡住，要回应它。 -->
   <!-- 中文：立即替换卡住的 Agent，不必等它真正停止。 -->
   <!-- 中文：如果本轮有影响操作者的变化，用 ketchup 汇报；没变化则不发消息。 -->
7. **Stand down at once on the operator's stop.** A hold reaches every owner as a zero-writes order. Each reports where it stands and keeps its brief until the operator releases it.
   <!-- 中文：一旦操作者要求停止，立即停下来。 -->
   <!-- 中文：将暂停指令传到每个负责人，要求停止一切写入。 -->
   <!-- 中文：各自报告停在哪里，保留自己的任务说明，直到操作者解除暂停。 -->

**Reply:** catch the operator up per the **ketchup** skill, covering each PR's state and verdict, what merged, countersigns and why, open operator gates, and parked follow-ups, with links to each PR and the trails.
<!-- 中文：回复：用 ketchup 向操作者补齐进展，包括每个 PR 的状态和验证结论、已合入内容、会签及其理由、待操作者决定的关卡、暂搁的后续任务，以及每个 PR 和工作记录的链接。 -->

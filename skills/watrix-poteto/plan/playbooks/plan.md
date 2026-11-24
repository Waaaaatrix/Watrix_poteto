### Plan, prototype or handoff

**You own the plan, not the code. The plan is a checklist an owner runs box by box and the operator audits from the evidence.** The plan is the deliverable. Do not implement.

1. When the change is one or two files with an obvious approach, skip the plan. Say so and stop.

2. Settle open questions from the available evidence before you write. Use `./prototype.md` where useful; list any proposed tests as pending.

3. **Plan** 
   - Write the plan file. Open with what changes, the PR ids in landing order, and the dependency graph, which says which PRs are independent and branch from trunk, which wait for which, and the files each may touch. Then one section per PR. One PR is one change with its own evidence (the **sequence-verifiable-units** principle skill). Each section has checkboxes for what it depends on, the files it edits, the one change it builds, the result you see, and its verification.
   - Specify only the checks needed for each PR. For each, name the scenario and what counts as a pass. For a performance check, name the metric, workload, baseline, and pass threshold. A PR that changes an interaction waits for the operator's review before merge.
   - Follow `module-decoupling` at any time. And report your design of surfaces.
   - Hand back the plan path and stop. Execution starts on the operator's explicit go, and not by you.

4. **Handoff** Only on explicit request.
   - Stop at a safe boundary. Finish the current atomic step or back out of it. Start nothing new, and cancel any nested subagents. Record the goal, decisions already made, the current branch and worktree, what is done and verified, and what is still running or blocked. Link to the relevant files, runs, and evidence. Give the next owner one concrete first action.

**Reply:** the plan path, the PR ids with their dependencies and the review-gated set, and what stays unproven.

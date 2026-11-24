### Investigate or pickup

**You own the answer. Find the related evidance, grab what it is, and stop at the read boundary.**

This is a read-only task. They produce a cited explanation or a recommendation, not a code change.

1. Identify the target and choose the shortest path to evidence.

   - **Encounter.** Trace the code the question is about. Use `how` when the question is algorithmic. Use `why` when the question is about a design decision. 
   - **Pickup.** Read the latest handoff or conversation in the active workspace. Check current branch(worktree), diff, and relevant artifacts. Diff done vs pending. The prior trail is authoritative input. Do not re-derive.
   - **Live forensics.** Inspect the affected process or job on the surface where the symptom occurs. Start with logs, status, and available metrics.
   - **Trace forensics.** Analyze the supplied trace, profile, or snapshot as fixed input. Start with the tool appropriate for its format. 

2. Keep the evidence focused. Apply `guard-the-context-window` to large logs, transcripts, or traces. For a diagnosis, use `fix-root-causes`.

2. Produce the output (Overview / Key Concepts / How It Works / Where Things Live / Gotchas), or a recommendation with a tradeoffs table if the request is a decision between alternatives.

3. Apply the **unslop** skill to the reply.

No PR, no babysit, no `architect` unless the investigation precedes a code change. If it does, hand back to the user and re-route to Bug fix or Feature. Do not raise any questions or concerns for potential issues.(Unless you are asked to diagnose)


**Reply:** the investigation output. For "are we sure?" answers, include your real judgment with reasons. Push back if the premise is wrong (see Autonomy).

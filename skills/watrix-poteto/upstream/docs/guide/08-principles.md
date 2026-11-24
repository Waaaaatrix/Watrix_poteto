# Steer with principle names

pstack ships 25 principles as individual skills. `/poteto-mode` reads their index at the start of every multi-step task, applies the ones the task triggers, and names each applied principle in its reply along with the decision it changed.

You don't invoke principles. You use their names to steer. Each name points at a complete rule the agent has already read, so one phrase redirects the work more precisely than a paragraph of instructions.

## Steering in practice

Say the agent is about to bolt a new adapter onto three existing ones:

```text
use subtract before you add. delete the obsolete adapters first, then design what's left.
```

Say it claims success because the build passed:

```text
apply prove it works. run the real import flow and show me the written records.
```

Say two parallel attempts are about to write to the same branch:

```text
separate before serializing shared state. give each attempt its own worktree, no locks.
```

Each phrase lands because the rule behind it is specific. The agent still has to say, in its reply, which decision the rule changed. A principle citation with no decision behind it means the agent named the rule without applying it.

## The 25, briefly

The core principles decide how much to build and when to rethink the design:

- [Laziness Protocol](../../../build/principles/principle-laziness-protocol/skill.md) prefers deletion and the smallest change that solves the problem.
- [Foundational Thinking](../../../plan/principles/principle-foundational-thinking/skill.md) chooses the core data structures before writing logic.
- [Redesign from First Principles](../../../plan/principles/principle-redesign-from-first-principles/skill.md) integrates a new requirement as if it had been there from day one.
- [Attack the Premise](../../../read/principles/principle-attack-the-premise/skill.md) writes down the premise that two or more failed fixes shared and tests it directly before the next fix.
- [Subtract Before You Add](../../../build/principles/principle-subtract-before-you-add/skill.md) removes dead weight before building on top of it.
- [Minimize Reader Load](../../../review/principles/principle-minimize-reader-load/skill.md) collapses layers and hidden state a reader must hold in their head.
- [Outcome-Oriented Execution](../../../build/principles/principle-outcome-oriented-execution/skill.md) converges rewrites on the target design instead of preserving throwaway compatibility states.
- [Experience First](../../../plan/principles/principle-experience-first/skill.md) chooses the user's result over implementation convenience.
- [Exhaust the Design Space](../../../plan/principles/principle-exhaust-the-design-space/skill.md) builds two or three competing prototypes when there's no precedent.
- [Build the Lever](../../../build/principles/principle-build-the-lever/skill.md) builds the script that does or proves the work, so a reviewer can rerun it. When an agent keeps doing the same thing by hand, have it write the tool or skill it wishes it had. If a script can do a step the same way every time, use the script, and save agents for the judgment calls.

The architecture principles decide where state, validation, and compatibility live:

- [Model the Domain](../../../plan/principles/principle-model-the-domain/skill.md) encodes repeated rules in one structure, not scattered conditionals.
- [Boundary Discipline](../../../build/principles/principle-boundary-discipline/skill.md) validates at the boundary and trusts internal types.
- [Module Decoupling](../../../build/principles/principle-module-decoupling/skill.md) keeps internal modules independent through stable, reusable interfaces in a shared protocol layer.
- [Type System Discipline](../../../build/principles/principle-type-system-discipline/skill.md) makes illegal states unrepresentable.
- [Make Operations Idempotent](../../../build/principles/principle-make-operations-idempotent/skill.md) converges retries on the same end state.
- [Migrate Callers Then Delete Legacy APIs](../../../build/principles/principle-migrate-callers-then-delete-legacy-apis/skill.md) migrates and deletes in one wave.
- [Separate Before Serializing Shared State](../../../plan/principles/principle-separate-before-serializing-shared-state/skill.md) removes the sharing before adding coordination.

The verification principles define what counts as proof:

- [Prove It Works](../../../test/principles/principle-prove-it-works/skill.md) verifies the real artifact, not a proxy.
- [Fix Root Causes](../../../read/principles/principle-fix-root-causes/skill.md) reproduces and traces to the cause before changing code.
- [Sequence Work into Verifiable Units](../../../test/principles/principle-sequence-verifiable-units/skill.md) ends each small unit in a check before starting the next.
- [Test Behavior, Not Implementation](../../../test/principles/principle-test-behavior-not-implementation/skill.md) asserts what the code's users can observe, so a test fails when that behavior breaks and survives a change to the internals.
- [Explain the Number](../../../test/principles/principle-explain-the-number/skill.md) names what limits a measured number and rules out that it measured something else, before anyone trusts or reports it. [`/benchmark-checklist`](../../../test/skills/benchmark-checklist/skill.md) turns it into seven questions you answer from real runs.

The delegation principles keep parallel work sane:

- [Guard the Context Window](../../../read/principles/principle-guard-the-context-window/skill.md) routes bulk reading to subagents and keeps findings in the main chat.
- [Never Block on the Human](../../../build/principles/principle-never-block-on-the-human/skill.md) proceeds on reversible work and presents the result.

And one meta principle:

- [Encode Lessons in Structure](../../../build/principles/principle-encode-lessons-in-structure/skill.md) turns advice you've repeated twice into a lint, check, or script. [`/correct`](../../../build/skills/correct/skill.md) applies it to a whole repo, as shown in [Make it yours](./09-make-it-yours.md#fix-the-environment-with-correct).

Don't memorize the list. Skim it now, then come back when you catch the agent doing something a name here would have prevented. That's how the vocabulary sticks.

Next: [Make it yours](./09-make-it-yours.md).

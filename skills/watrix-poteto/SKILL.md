---
name: watrix-poteto
description: Routes explicitly requested Watrix_poteto workflows for reading code, planning, human code review, implementation, and scoped testing. Use when the user names Watrix_poteto or invokes its read, plan, review, build, or test mode.
---

# Watrix_poteto

Read [AGENTS.md](AGENTS.md) before selecting a playbook. Resolve bundled paths relative to the file that names them. Run bundled scripts by absolute path from the target project.

| User mode | Entry playbook |
| --- | --- |
| `\read` | [read](read/playbooks/read.md) |
| `\plan` | [plan](plan/playbooks/plan.md) |
| `\review` | [review](review/playbooks/review.md) |
| `\build` | [build](build/playbooks/build.md) |
| `\test` | [test](test/playbooks/test.md) |

Use the mode the user names. If they name Watrix_poteto without a mode, choose Read, Plan, Review, or Build from the requested outcome. Within that mode, use a specifically requested playbook from [INDEX.md](INDEX.md). Entries marked archive, upstream reference, or empty draft are not routes. The Test mode always uses `test.md`.

When a playbook or bundled skill names a skill, principle, or playbook, resolve it in [INDEX.md](INDEX.md) and read the linked file. A slash-prefixed skill name is the index ID without the slash; a short principle name uses the `principle-` prefix. Load only the files needed for the task. For an inherited reference to Poteto Mode's Harness or Subagents section, open that section of the [upstream mode](build/skills/poteto-mode/skill.md). Use the host's available tools for host-specific steps. Carry the shared rules into any delegated work.

# Watrix_poteto

A pstack-based workflow kit for Codex, Antigravity, and Cursor. Five playbooks cover the common modes; a shared index resolves the original playbooks, skills, and principles as needed.

## Install

With Node.js and npm available, run this from the project where you want to use it:

```sh
npx skills add Waaaaatrix/Watrix_poteto --skill watrix-poteto -a codex -a antigravity -a cursor
```

Keep the `-a` values for your hosts. Add `-g` for user-wide installation. Start a new chat after installing. The [skills CLI](https://github.com/vercel-labs/skills/blob/main/docs/commands.md#skills-add) installs the same bundle on all three hosts.

## Use

In Codex, select `$watrix-poteto`; in Cursor or Antigravity, select `watrix-poteto` in the skill picker or reference it by name. Send the mode and task in the same message.

| Mode | Use it for | Example |
| --- | --- | --- |
| `\read` | Investigate code, trace behavior, or resume from a handoff | `\read Trace how a training batch reaches the model.` |
| `\plan` | Plan a change, compare designs, or prepare a handoff | `\plan Outline the data format migration.` |
| `\review` | Summarize changes with locations and present the line-by-line diff | `\review Show this branch's changes against main.` |
| `\build` | Route implementation to the relevant playbook | `\build bug-fix Fix the inconsistent date filter.` |
| `\test` | Run the smallest necessary test set and report concrete evidence | `\test Check the date filter's month boundary.` |

The backslash modes are prompt text. Tests are written or run only when the user explicitly invokes `\test`. Read-only inspection of source, diffs, existing logs, and CI results is available in the other modes.

## Architecture

```mermaid
flowchart TB
    H["Codex / Antigravity / Cursor"] --> E["watrix-poteto: SKILL.md"]
    E --> G["AGENTS.md: shared rules"]
    G --> I["INDEX.md: playbooks, skills, principles"]
    I --> R["Read"]
    I --> P["Plan"]
    I --> V["Review"]
    I --> B["Build"]
    I --> T["Test"]
```

The [installable bundle](skills/watrix-poteto/) contains all five categories. Each holds its playbooks, skills, and principles. The [index](skills/watrix-poteto/INDEX.md) is the lookup table for cross-category references. The original Test playbooks remain archived under Test; `test.md` is the active entry. Inherited workflows that need a host-specific tool use that tool where available.

## License

MIT. Based on [pstack](https://github.com/cursor/plugins/tree/main/pstack) and its [cross-host mirror](https://github.com/backnotprop/pstack). See the [project license](LICENSE) and [upstream license](skills/watrix-poteto/upstream/LICENSE).

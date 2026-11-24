### Review code with a human

Use this playbook when the user wants to inspect code changes themselves. Keep the review read-only.

State the repository and comparison scope before reporting. Use the requested worktree, commit range, or merge request diff. If no scope is given, compare the current worktree with `HEAD` and include untracked, non-ignored files. Read the diff and the surrounding code needed to explain it. Use the same scope for both sections below.

#### 1. Change summary

Group related edits into logical changes. Use `Change | What changed | Location`, one row per change. Explain the change in behavior or structure and cite every file and symbol involved, with clickable file and line links where supported. For deletions, cite the old path and base line; for renames, give both paths. Flag confirmed regressions first and link to the evidence.

#### 2. Line-by-line diff

Use the native diff viewer if it covers the stated scope. Open it when the host supports doing so; otherwise give the reviewer the exact navigation path.

| Host | Where to inspect the diff |
| --- | --- |
| Cursor | Open **Review** or click a file in the Agent's change summary to see the turn's edits. For local Git changes, open **Source Control** and click a changed file. [Cursor review controls](https://prod.cursor.com/docs/release-notes) |
| Codex | Use the desktop **Review** pane and select the matching scope. In the CLI, use `/diff` for local Git changes. [Codex desktop](https://learn.chatgpt.com/docs/reference/troubleshooting#files-appear-in-the-side-panel-that-codex-didnt-edit) / [CLI](https://learn.chatgpt.com/docs/developer-commands?surface=cli#review-changes-with-diff) |
| Antigravity | Open **Overview > Review** and select the matching scope. Use **View Split Diff** to inspect the old and new lines side by side. [Antigravity review controls](https://antigravity.google/docs/review-changes/) |

If the viewer is unavailable or leaves out changes, provide the complete unified diff. Put a short diff in a `diff` code block; link a long one as a `.diff` file saved outside the worktree. Preserve file headers, hunk ranges, and all added and removed lines. Mark binary changes by path.

```sh
# Net tracked changes in the worktree against HEAD
git diff --no-ext-diff --no-textconv --no-color HEAD --

# Branch changes since divergence from the base
git diff --no-ext-diff --no-textconv --no-color "$base...$head" --
```

For a direct comparison of two revisions, pass `"$base" "$head"` as separate arguments. For worktree reviews, list untracked files with `git ls-files --others --exclude-standard` and append each in-scope file with `git diff --no-index --no-color -- /dev/null "$file"` (exit status `1` means the file differs).

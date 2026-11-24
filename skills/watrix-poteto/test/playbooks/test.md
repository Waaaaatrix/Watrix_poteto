### Test

Use the user's stated target, or the current change when no target is given.

#### Test only what is needed

1. Identify the changed behavior or reported failure. Pick the smallest set of cases that can prove the expected behavior and catch a relevant regression. State what counts as a pass before running each case.
2. Prefer existing targeted tests or a direct reproduction on the real implementation. Add a focused test only if existing coverage cannot prove a necessary case. Avoid mocks where the real dependency is practical, and expand to a broader suite only when affected callers or shared behavior require it.
3. Run the selected checks. For a bug fix, reuse the original reproducer and capture the before/after results when both revisions are available. Stop when the required cases are resolved. Report failures or blockers in place.

#### Report evidence

List one row per test: `Behavior | Command or manual steps | Result | Evidence`. Include the tested revision or worktree and the environment if it affects the result. For an automated pass, cite the actual runner summary and exit status; for a manual pass, state the expected and observed result and link the relevant log, screenshot, or output when available. Mark `Fail`, `Blocked`, or `Not run` explicitly; a skipped case, an empty test run, or an inconclusive result cannot count as `Pass`.

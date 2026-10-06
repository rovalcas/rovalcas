# Workflow discipline (autonomous runs)

Cost matters: every output token is paid for, and long text is re-read by the user and by other sessions.

## Final replies
- Outcome plus the one decision needed from the user. Nothing else.
- Never restate what the tool output or earlier messages already showed: no recap of steps, diffs, file lists or test output.
- No progress narration between tool calls beyond one short line when starting a long step.

## Commits and PRs
- Write the commit message and PR body to a file once, correctly, and pass the file (`git commit -F`, PR body from file). Don't retry inline with edited quoting.
- Commit body: the why, at most 3 lines. PR body: at most 5 bullets in the repo's template sections. Don't restate the diff.
- One GitHub comment per round, and only when a reply is genuinely needed.

## Git state
- Don't create a branch only to delete it again. Use the designated branch.
- No extra state listings (`git status`, `git branch`, `git log`) when the result is already known from the previous command.

## Reading files
- Don't re-read a file you already have in context. Don't read a large file in full when you need part: use Grep, or Read with offset/limit.

## Running tests
- Run the suite once from a clean state. Rerun only on a failure, and only the failing test first.
- One outlier is not a reason for repeated reruns: investigate the failure instead.
- Don't revert changes just to prove a test fails without them. Trust the test unless it passed suspiciously.
- Do the combination/merge check only when another open branch overlaps the same files.

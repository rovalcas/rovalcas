# Planned issues

Issues are disabled on this repo, so they are parked here until we create them.

## 1. Terse final replies for autonomous runs
Long final messages that repeat output already shown are the biggest single cost: extra output tokens, and the user and other sessions re-read the same text.

Goal: final replies carry the outcome plus the one decision needed, nothing else.

- [x] Initial rule in `.claude/rules/workflow-discipline.md`
- [ ] Measure: sample recent sessions, count how often the final reply restates earlier output
- [ ] Add a Stop-hook or reminder check if the rule alone doesn't hold
- [ ] Propagate the rule to the other repos (`defender-action-hub`, `ado-portable-client`, `migrate-agentic-cli`, ...)

## 2. Short commit and PR bodies, written once from a file
Commit and PR bodies restate earlier content (about 40% could go) and are retried inline with changed quoting.

Goal: write the body to a file once and pass it (`git commit -F`, PR body from file).

- [x] Initial rule: commit body at most 3 lines (the why), PR body at most 5 bullets
- [ ] Reconcile with repos whose rules push toward long PR bodies (`migrate-agentic-cli` AGENTS.md asks for "notes on test updates and command output")
- [ ] Decide whether to add a PR template with a length-limited structure
- [ ] Limit GitHub noise to one comment per round

## 3. Run tests once; rerun only on failure
Seen: three reruns for flakiness after one outlier, plus extra reverts to prove a test fails without the change.

Goal: run the suite once from a clean state; rerun only on failure, failing test first.

- [x] Initial rule: no repeated flake reruns, no revert-to-prove, combination check only when another branch overlaps
- [ ] Define "clean state" per repo (build/install steps) so one run is trustworthy
- [ ] Document known flaky tests per repo instead of rerunning to find them
- [ ] Decide when a revert-to-prove check is justified (e.g. a regression test for a bug fix)

## 4. Avoid wasted git and file operations
Seen: recreating a branch only to delete it again, extra state listings, and re-reading large files in full when only part was needed.

Goal: fewer redundant tool calls.

- [x] Initial rules: use the designated branch, skip repeated `git status/branch/log`, use Grep or Read with offset/limit
- [ ] Check whether a session-start hook can set the branch up once
- [ ] Add per-repo hints on large files worth partial reads

## 5. Roll out shared Claude rules across repos (out-of-box experience)
Tracking issue: improve the out-of-box experience gradually so every repo behaves well in autonomous runs without per-session tuning.

Audit (2026-10-06):
- Rules files exist in `defender-action-hub` (AGENTS.md), `ado-portable-client` (CLAUDE.md), `migrate-agentic-cli` (CLAUDE.md, AGENTS.md), `millenial-bits` (AGENTS.md, copilot-instructions)
- No Claude hooks in any repo
- No rule on output length anywhere
- `ado-portable-client` has a committed `.claude/settings.local.json` (should be local only)
- Not audited: `LooksWalker`, `automatch-ai` (access denied), `ArtfulAgents` `.github/*`

Plan:
- [x] Create shared rules file here (`.claude/rules/workflow-discipline.md`)
- [ ] Copy it to repos with existing rules, on `chore/` branches
- [ ] Add a minimal `CLAUDE.md` to repos without one
- [ ] Audit the repos not yet covered
- [ ] Decide on a single source of truth for shared rules (e.g. this repo or a dedicated one)

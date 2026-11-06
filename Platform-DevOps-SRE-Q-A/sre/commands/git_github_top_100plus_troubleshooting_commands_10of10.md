# Git and GitHub Top 100+ Production Troubleshooting Commands
**10/10 senior-level runbook pack for Git, GitHub Platform, DevOps, and SRE teams in product-based companies**

## How to Use This Pack

This is a **production incident command pack**, not a basic Git cheat sheet.

Each command includes:

- the command itself
- a concise technical explanation
- a **risk level**
- a **use when** line so responders know where it fits in incident flow

## Command Risk Levels

- **Low risk** — read-only or generally safe diagnostic action
- **Medium risk** — changes local state or execution path and should be used deliberately
- **High risk** — can delete local work, rewrite history, or interrupt active delivery paths
- **Emergency-only** — use only in tightly controlled recovery situations with strong confirmation

## Fast “First 5 Commands” by Incident Type

### Branch divergence or sync confusion
1. `git status --short --branch`
2. `git fetch --all --prune`
3. `git branch -vv`
4. `git log HEAD..origin/main --oneline`
5. `git log origin/main..HEAD --oneline`

### Bad production commit investigation
1. `git log --since='24 hours ago' --stat`
2. `git log --oneline --decorate --graph -30`
3. `git show <commit>`
4. `git diff <commit1>..<commit2>`
5. `git blame -L 1,120 path/to/file`

### Lost local work after reset, rebase, or checkout mistake
1. `git reflog`
2. `git checkout -b recover/<name> HEAD@{1}`
3. `git stash list`
4. `git fsck --lost-found`
5. `git show <dangling-commit-sha>`

### Rollback of merged PR or shared branch change
1. `git log --first-parent --oneline main`
2. `git show <commit>`
3. `git revert <commit>` or `git revert -m 1 <merge-commit>`
4. `git status`
5. `git push --dry-run`

### Stuck or failed GitHub Actions deployment
1. `gh run list --limit 20`
2. `gh run view <run-id> --log`
3. `gh workflow list`
4. `gh workflow view <name>`
5. `gh secret list`

### Branch protection or hotfix push blocked
1. `gh auth status`
2. `git remote -v`
3. `git push --dry-run`
4. `gh api repos/{owner}/{repo}/branches/main/protection`
5. `gh pr status`

### Release tag mismatch or deployment traceability issue
1. `git fetch --tags`
2. `git tag -l 'v*'`
3. `git show-ref --tags`
4. `git show <tag>`
5. `gh release view <tag>`

## Recommended Troubleshooting Flow

1. confirm branch, remote, commit SHA, and repo context first
2. fetch before assuming local metadata is current
3. prefer revert over history rewrite on shared branches
4. create backup refs before destructive local operations
5. verify what GitHub sees, not just what local Git shows
6. validate PR status, tags, releases, and Actions runs after recovery

---

# Repository Health and Context


## 1. `git status`


```bash

git status

```

**Risk:** Low risk

**Technical explanation:** Shows branch, staged changes, unstaged changes, and untracked files so you can assess local repository state before acting.

**Use when:** Use when you need the fastest safe snapshot of the current working tree.


## 2. `git status --short --branch`


```bash

git status --short --branch

```

**Risk:** Low risk

**Technical explanation:** Provides a compact high-signal view of branch divergence and file changes, which is ideal during rapid incident work.

**Use when:** Use when you need concise output for quick triage or shell-based debugging.


## 3. `git branch`


```bash

git branch

```

**Risk:** Low risk

**Technical explanation:** Lists local branches so you can confirm where you are before merge, reset, or rollback operations.

**Use when:** Use when local branch context is uncertain.


## 4. `git branch -vv`


```bash

git branch -vv

```

**Risk:** Low risk

**Technical explanation:** Shows upstream tracking and latest commit metadata per branch, helping diagnose local drift from origin.

**Use when:** Use when branches may be detached from expected upstreams.


## 5. `git remote -v`


```bash

git remote -v

```

**Risk:** Low risk

**Technical explanation:** Displays fetch and push remotes so you can confirm the repo is targeting the right GitHub origin or mirror.

**Use when:** Use when push, fetch, or fork confusion is possible.


## 6. `git rev-parse --show-toplevel`


```bash

git rev-parse --show-toplevel

```

**Risk:** Low risk

**Technical explanation:** Prints the repository root so you can verify you are operating inside the intended repo path.

**Use when:** Use when working deep inside monorepos or nested directories.


## 7. `git rev-parse --abbrev-ref HEAD`


```bash

git rev-parse --abbrev-ref HEAD

```

**Risk:** Low risk

**Technical explanation:** Returns only the current branch name, which is useful for scripts and quick context checks.

**Use when:** Use when automation or shell logic depends on exact branch name.


## 8. `git config --list --show-origin`


```bash

git config --list --show-origin

```

**Risk:** Low risk

**Technical explanation:** Displays effective Git configuration and where each setting comes from, helping debug identity, proxy, SSL, and credential problems.

**Use when:** Use when behavior differs between shells, runners, or engineers.


## 9. `git config user.name`


```bash

git config user.name

```

**Risk:** Low risk

**Technical explanation:** Prints the configured author name so you can verify commit attribution before an emergency commit.

**Use when:** Use when making production fixes from a machine with uncertain config.


## 10. `git config user.email`


```bash

git config user.email

```

**Risk:** Low risk

**Technical explanation:** Prints the configured author email so you can catch wrong corporate identity before pushing changes.

**Use when:** Use when auditability and signed-off ownership matter.


## 11. `git remote show origin`


```bash

git remote show origin

```

**Risk:** Low risk

**Technical explanation:** Summarizes remote tracking branches and default branch info, which helps explain push or fetch confusion.

**Use when:** Use when origin behavior does not match team expectations.


## 12. `git rev-parse HEAD`


```bash

git rev-parse HEAD

```

**Risk:** Low risk

**Technical explanation:** Prints the exact current commit SHA so you can compare local state with deployed or CI-reported revisions.

**Use when:** Use when you must align code state with an incident timestamp or deploy SHA.


## 13. `git symbolic-ref refs/remotes/origin/HEAD`


```bash

git symbolic-ref refs/remotes/origin/HEAD

```

**Risk:** Low risk

**Technical explanation:** Shows the remote's advertised default branch, helping resolve main/master or renamed-default-branch confusion.

**Use when:** Use when branch defaults changed and tooling still points to old names.


## 14. `git show --summary --stat HEAD`


```bash

git show --summary --stat HEAD

```

**Risk:** Low risk

**Technical explanation:** Shows the latest commit summary and changed files, which is useful for validating the last applied change quickly.

**Use when:** Use when the latest commit is the prime suspect in a failure.


## 15. `git log -1 --decorate --oneline`


```bash

git log -1 --decorate --oneline

```

**Risk:** Low risk

**Technical explanation:** Displays the latest commit with branch and tag decorations so you can align local state with release references.

**Use when:** Use when verifying whether the expected release commit is checked out.


# Fetch, Sync, and Remote Divergence


## 16. `git fetch origin`


```bash

git fetch origin

```

**Risk:** Low risk

**Technical explanation:** Fetches remote refs without touching your working tree, making it the safest first step before diagnosing divergence.

**Use when:** Use before comparing local and remote history.


## 17. `git fetch --all --prune`


```bash

git fetch --all --prune

```

**Risk:** Low risk

**Technical explanation:** Refreshes all remotes and removes deleted remote-tracking branches so local metadata is current.

**Use when:** Use when branch listings are noisy or stale across multiple remotes.


## 18. `git remote prune origin`


```bash

git remote prune origin

```

**Risk:** Low risk

**Technical explanation:** Deletes stale origin/* references locally when remote branches were removed but still appear locally.

**Use when:** Use when engineers think deleted branches still exist on GitHub.


## 19. `git branch -r`


```bash

git branch -r

```

**Risk:** Low risk

**Technical explanation:** Lists remote-tracking branches so you can verify whether a branch actually exists on the server.

**Use when:** Use when push targets, PR source branches, or hotfix branches seem missing.


## 20. `git branch -a`


```bash

git branch -a

```

**Risk:** Low risk

**Technical explanation:** Shows both local and remote branches, helping compare naming and visibility across states.

**Use when:** Use when reconciling what exists locally versus remotely.


## 21. `git log --oneline --graph --decorate --all -20`


```bash

git log --oneline --graph --decorate --all -20

```

**Risk:** Low risk

**Technical explanation:** Draws a compact graph across branches so you can see merges, divergence, and rebases quickly.

**Use when:** Use when the repository history shape itself is the problem.


## 22. `git log HEAD..origin/main --oneline`


```bash

git log HEAD..origin/main --oneline

```

**Risk:** Low risk

**Technical explanation:** Shows commits that exist on origin/main but not locally, clarifying why local code is behind.

**Use when:** Use when your branch appears outdated compared with GitHub or production.


## 23. `git log origin/main..HEAD --oneline`


```bash

git log origin/main..HEAD --oneline

```

**Risk:** Low risk

**Technical explanation:** Shows commits present locally but not on origin/main, helping detect unpushed or divergent local work.

**Use when:** Use when local changes may not have reached the remote.


## 24. `git diff --name-status HEAD..origin/main`


```bash

git diff --name-status HEAD..origin/main

```

**Risk:** Low risk

**Technical explanation:** Lists file-level differences between local HEAD and origin/main so you can scope drift quickly.

**Use when:** Use when you need impact scope before deciding on pull, rebase, or reset.


## 25. `git diff HEAD..origin/main`


```bash

git diff HEAD..origin/main

```

**Risk:** Low risk

**Technical explanation:** Shows patch-level drift between local and remote state, which helps choose merge, rebase, or discard strategies.

**Use when:** Use when file names alone are not enough to judge risk.


## 26. `git pull --ff-only`


```bash

git pull --ff-only

```

**Risk:** Medium risk

**Technical explanation:** Updates the current branch only if it can fast-forward, preventing surprise merge commits during sensitive work.

**Use when:** Use when you want safe synchronization without altering shared history shape.


## 27. `git pull --rebase`


```bash

git pull --rebase

```

**Risk:** Medium risk

**Technical explanation:** Rebases local commits onto fetched remote changes, keeping history cleaner but rewriting local commit topology.

**Use when:** Use when local commits are not yet shared and you need a clean sync path.


## 28. `git push --dry-run`


```bash

git push --dry-run

```

**Risk:** Low risk

**Technical explanation:** Tests whether a push would succeed without updating the remote, which is useful for permission and ref-protection checks.

**Use when:** Use before a sensitive production push or hotfix publish.


## 29. `git ls-remote --heads origin`


```bash

git ls-remote --heads origin

```

**Risk:** Low risk

**Technical explanation:** Queries remote heads directly from the server so branch existence is verified without relying on local cache.

**Use when:** Use when local metadata may be stale or corrupted.


## 30. `git ls-remote --tags origin`


```bash

git ls-remote --tags origin

```

**Risk:** Low risk

**Technical explanation:** Queries remote tags directly from the server so you can verify release markers on GitHub.

**Use when:** Use when deployments or release automation depend on tag presence.


# Commit History, Blame, and Change Investigation


## 31. `git log --oneline --decorate --graph -30`


```bash

git log --oneline --decorate --graph -30

```

**Risk:** Low risk

**Technical explanation:** Shows recent commit history with branch context so you can identify suspicious changes near an incident window.

**Use when:** Use when starting a timeline-based production investigation.


## 32. `git log --since='24 hours ago' --stat`


```bash

git log --since='24 hours ago' --stat

```

**Risk:** Low risk

**Technical explanation:** Filters commits by time and includes file stats, helping correlate failure onset with recent activity.

**Use when:** Use when the incident began within a known recent window.


## 33. `git log --author='name@example.com' --oneline`


```bash

git log --author='name@example.com' --oneline

```

**Risk:** Low risk

**Technical explanation:** Filters history by author so you can inspect changes from a specific engineer, bot, or CI identity.

**Use when:** Use when automation or one contributor is suspected.


## 34. `git log --grep='hotfix' --oneline`


```bash

git log --grep='hotfix' --oneline

```

**Risk:** Low risk

**Technical explanation:** Searches commit messages for key terms, which is useful when tracing prior emergency changes or rollback references.

**Use when:** Use when the team uses message conventions for incident work.


## 35. `git show <commit>`


```bash

git show <commit>

```

**Risk:** Low risk

**Technical explanation:** Displays the full patch and metadata for a commit so you can understand exactly what changed.

**Use when:** Use when a specific commit is a likely trigger.


## 36. `git show <commit> --name-only`


```bash

git show <commit> --name-only

```

**Risk:** Low risk

**Technical explanation:** Lists only the files touched by a commit, helping scope impact before reading the full patch.

**Use when:** Use when you need fast blast-radius estimation.


## 37. `git diff <commit1>..<commit2>`


```bash

git diff <commit1>..<commit2>

```

**Risk:** Low risk

**Technical explanation:** Shows the exact patch difference between two revisions, which is useful when narrowing the change that introduced breakage.

**Use when:** Use when comparing known-good and known-bad states.


## 38. `git diff --name-only <commit1>..<commit2>`


```bash

git diff --name-only <commit1>..<commit2>

```

**Risk:** Low risk

**Technical explanation:** Lists changed file paths between two commits so you can focus on affected services or configs.

**Use when:** Use when you need quick scoping before deep inspection.


## 39. `git blame -L 1,120 path/to/file`


```bash

git blame -L 1,120 path/to/file

```

**Risk:** Low risk

**Technical explanation:** Attributes specific lines to commits and authors so you can trace ownership of a risky change precisely.

**Use when:** Use when one config block or script section appears responsible.


## 40. `git annotate path/to/file`


```bash

git annotate path/to/file

```

**Risk:** Low risk

**Technical explanation:** Provides line-by-line authorship similar to blame, which remains useful in some established team workflows.

**Use when:** Use when your environment or habits rely on annotate syntax.


## 41. `git log -- path/to/file`


```bash

git log -- path/to/file

```

**Risk:** Low risk

**Technical explanation:** Shows the history of a single file, which is extremely useful when one config or workflow file is the problem.

**Use when:** Use when the incident is isolated to one file path.


## 42. `git log -p -- path/to/file`


```bash

git log -p -- path/to/file

```

**Risk:** Low risk

**Technical explanation:** Displays the patch history for one file so you can inspect how it evolved over time.

**Use when:** Use when you need the exact sequence of edits to a critical file.


## 43. `git shortlog -sn`


```bash

git shortlog -sn

```

**Risk:** Low risk

**Technical explanation:** Summarizes commit counts by author, which can help validate unexpected authorship patterns in automated repos.

**Use when:** Use when bot or automation behavior looks unusual.


## 44. `git show HEAD:path/to/file`


```bash

git show HEAD:path/to/file

```

**Risk:** Low risk

**Technical explanation:** Prints a file from HEAD without modifying the working tree, which is useful for quick content verification.

**Use when:** Use when you want current committed content without checkout side effects.


## 45. `git diff HEAD~1 HEAD -- path/to/file`


```bash

git diff HEAD~1 HEAD -- path/to/file

```

**Risk:** Low risk

**Technical explanation:** Shows only the last commit's patch for a specific file, making recent hotfix validation faster.

**Use when:** Use when the latest edit to a file likely caused the issue.


# Working Tree, Index, and Local Recovery


## 46. `git diff`


```bash

git diff

```

**Risk:** Low risk

**Technical explanation:** Shows unstaged working-tree changes so you can see what local modifications exist before reset or stash.

**Use when:** Use when local edits might interfere with accurate debugging.


## 47. `git diff --cached`


```bash

git diff --cached

```

**Risk:** Low risk

**Technical explanation:** Shows staged changes only, which is critical before emergency commits or when staging mistakes are suspected.

**Use when:** Use when you must verify exactly what is queued for commit.


## 48. `git diff --staged --name-only`


```bash

git diff --staged --name-only

```

**Risk:** Low risk

**Technical explanation:** Lists staged file paths so you can confirm the blast radius of the next commit.

**Use when:** Use when you need a fast staged-only inventory.


## 49. `git add -p`


```bash

git add -p

```

**Risk:** Medium risk

**Technical explanation:** Stages changes hunk by hunk, allowing you to isolate a safe fix from unrelated local edits.

**Use when:** Use when only part of a file or patch should enter a hotfix.


## 50. `git restore path/to/file`


```bash

git restore path/to/file

```

**Risk:** Medium risk

**Technical explanation:** Discards unstaged changes in one file by restoring it from HEAD.

**Use when:** Use when accidental edits should be removed but no shared history is involved.


## 51. `git restore --staged path/to/file`


```bash

git restore --staged path/to/file

```

**Risk:** Low risk

**Technical explanation:** Unstages a file while preserving working-tree edits, which is useful after accidental add operations.

**Use when:** Use when the wrong file was staged for commit.


## 52. `git checkout -- path/to/file`


```bash

git checkout -- path/to/file

```

**Risk:** Medium risk

**Technical explanation:** Restores a file from index or HEAD using older syntax that remains common in legacy environments.

**Use when:** Use when operating on older Git versions or established team workflows.


## 53. `git clean -nd`


```bash

git clean -nd

```

**Risk:** Low risk

**Technical explanation:** Performs a dry run of untracked file deletion so you can see what would be removed before cleanup.

**Use when:** Use before any destructive cleaning in repos with generated artifacts.


## 54. `git clean -fd`


```bash

git clean -fd

```

**Risk:** High risk

**Technical explanation:** Deletes untracked files and directories, which can permanently remove locally generated or forgotten work.

**Use when:** Use when a dirty workspace blocks accurate builds and you have confirmed nothing valuable is untracked.


## 55. `git reset --soft HEAD~1`


```bash

git reset --soft HEAD~1

```

**Risk:** Medium risk

**Technical explanation:** Moves HEAD back one commit but keeps changes staged, helping repair a bad local commit without losing work.

**Use when:** Use when the latest local commit should be redone before sharing.


## 56. `git reset --mixed HEAD~1`


```bash

git reset --mixed HEAD~1

```

**Risk:** Medium risk

**Technical explanation:** Moves HEAD back one commit and unstages changes, which is useful when splitting or recombining a local commit.

**Use when:** Use when the latest local commit structure is wrong but the content should remain.


## 57. `git reset --hard HEAD`


```bash

git reset --hard HEAD

```

**Risk:** High risk

**Technical explanation:** Resets the working tree and index to HEAD, discarding local tracked changes completely.

**Use when:** Use only when you are certain local tracked work should be discarded.


## 58. `git checkout -b hotfix/<name>`


```bash

git checkout -b hotfix/<name>

```

**Risk:** Low risk

**Technical explanation:** Creates and switches to a new branch for a production fix so recovery work is isolated.

**Use when:** Use when starting a repair without contaminating existing work.


## 59. `git switch -c hotfix/<name>`


```bash

git switch -c hotfix/<name>

```

**Risk:** Low risk

**Technical explanation:** Creates a new branch using modern syntax, which is clearer for hotfix creation.

**Use when:** Use when your Git version supports switch and you want explicit intent.


## 60. `git commit --amend`


```bash

git commit --amend

```

**Risk:** High risk

**Technical explanation:** Rewrites the latest commit, which is convenient locally but dangerous once the commit is shared.

**Use when:** Use when the latest commit is still private and needs correction.


# Stash, Reflog, and Lost Work Recovery


## 61. `git stash push -u -m 'wip-before-prod-fix'`


```bash

git stash push -u -m 'wip-before-prod-fix'

```

**Risk:** Medium risk

**Technical explanation:** Stashes tracked and untracked changes with a message so you can clear the workspace without losing interrupted work.

**Use when:** Use when an incident interrupts in-progress local development.


## 62. `git stash list`


```bash

git stash list

```

**Risk:** Low risk

**Technical explanation:** Lists saved stashes so you can identify suspended work during context switching.

**Use when:** Use when recovering earlier interrupted changes.


## 63. `git stash show -p stash@{0}`


```bash

git stash show -p stash@{0}

```

**Risk:** Low risk

**Technical explanation:** Displays the patch inside a stash so you can verify its contents before applying it.

**Use when:** Use when multiple stashes exist and you need the right one.


## 64. `git stash apply stash@{0}`


```bash

git stash apply stash@{0}

```

**Risk:** Medium risk

**Technical explanation:** Reapplies a stash without removing it, which is safer when recovering important work.

**Use when:** Use when you want recovery with a rollback option if conflicts appear.


## 65. `git stash pop`


```bash

git stash pop

```

**Risk:** Medium risk

**Technical explanation:** Applies and removes the most recent stash, which is efficient but less reversible than apply.

**Use when:** Use when you are confident the stash is correct and disposable afterward.


## 66. `git stash drop stash@{0}`


```bash

git stash drop stash@{0}

```

**Risk:** High risk

**Technical explanation:** Deletes a stash entry, which can permanently remove uncommitted work if done too early.

**Use when:** Use only after confirming the stash is recovered or no longer needed.


## 67. `git stash branch recover-work stash@{0}`


```bash

git stash branch recover-work stash@{0}

```

**Risk:** Low risk

**Technical explanation:** Creates a branch from a stash, which is one of the safest ways to recover and isolate suspended work.

**Use when:** Use when stash content deserves its own recovery branch.


## 68. `git reflog`


```bash

git reflog

```

**Risk:** Low risk

**Technical explanation:** Shows HEAD movement history, which is often the fastest way to recover from reset, rebase, or branch mistakes.

**Use when:** Use when work appears lost after history edits.


## 69. `git reflog show origin/main`


```bash

git reflog show origin/main

```

**Risk:** Low risk

**Technical explanation:** Shows reflog entries for the remote-tracking branch when you need to inspect recent local ref movement.

**Use when:** Use when origin-tracking state behaved unexpectedly on your machine.


## 70. `git reset --hard HEAD@{1}`


```bash

git reset --hard HEAD@{1}

```

**Risk:** High risk

**Technical explanation:** Moves the repository back to an earlier reflog position, which can recover state but discards current tracked changes.

**Use when:** Use only when reflog confirms a safer earlier state should replace the current one.


## 71. `git checkout -b recover/<name> HEAD@{1}`


```bash

git checkout -b recover/<name> HEAD@{1}

```

**Risk:** Low risk

**Technical explanation:** Creates a new branch from a reflog entry so you can preserve and inspect lost work safely.

**Use when:** Use when you want recovery without destructive reset.


## 72. `git fsck --lost-found`


```bash

git fsck --lost-found

```

**Risk:** Low risk

**Technical explanation:** Scans the object database for dangling objects that may contain commits or blobs not reachable from refs.

**Use when:** Use when reflog is insufficient and work may still exist as orphaned objects.


## 73. `git show <dangling-commit-sha>`


```bash

git show <dangling-commit-sha>

```

**Risk:** Low risk

**Technical explanation:** Displays a dangling commit found via fsck so you can determine whether it contains recoverable work.

**Use when:** Use when auditing fsck output for lost fixes.


## 74. `git cherry-pick <recovered-commit>`


```bash

git cherry-pick <recovered-commit>

```

**Risk:** Medium risk

**Technical explanation:** Reapplies a recovered commit onto a live branch once you confirm it contains the needed fix.

**Use when:** Use when recovered work should be restored into the active branch.


## 75. `git branch backup/$(date +%Y%m%d-%H%M%S)`


```bash

git branch backup/$(date +%Y%m%d-%H%M%S)

```

**Risk:** Low risk

**Technical explanation:** Creates an immediate backup branch at the current HEAD before risky recovery or history surgery.

**Use when:** Use before resets, rebases, force pushes, or manual recovery steps.


# Merge, Rebase, Cherry-Pick, and Conflict Resolution


## 76. `git merge --no-ff <branch>`


```bash

git merge --no-ff <branch>

```

**Risk:** Medium risk

**Technical explanation:** Merges with an explicit merge commit, preserving branch context that can help later production forensics.

**Use when:** Use when auditability of a hotfix or PR integration matters.


## 77. `git merge --abort`


```bash

git merge --abort

```

**Risk:** Low risk

**Technical explanation:** Aborts an in-progress merge and restores the pre-merge state when conflict resolution went wrong.

**Use when:** Use when a merge attempt becomes unsafe or too messy to continue.


## 78. `git rebase origin/main`


```bash

git rebase origin/main

```

**Risk:** High risk

**Technical explanation:** Replays local commits on top of origin/main, rewriting local commit IDs and history shape.

**Use when:** Use when local unshared work must be updated onto the latest mainline cleanly.


## 79. `git rebase --abort`


```bash

git rebase --abort

```

**Risk:** Low risk

**Technical explanation:** Cancels an in-progress rebase and restores the original branch state.

**Use when:** Use when the rebase path becomes risky or conflicts are too complex.


## 80. `git rebase --continue`


```bash

git rebase --continue

```

**Risk:** Medium risk

**Technical explanation:** Continues a rebase after conflict resolution so the history rewrite can proceed.

**Use when:** Use when conflicts are resolved and the intended rebase should finish.


## 81. `git cherry-pick <commit>`


```bash

git cherry-pick <commit>

```

**Risk:** Medium risk

**Technical explanation:** Applies a specific commit onto the current branch, which is useful for targeted hotfix backports or forward-fixes.

**Use when:** Use when only one precise change should move across branches.


## 82. `git cherry-pick -x <commit>`


```bash

git cherry-pick -x <commit>

```

**Risk:** Medium risk

**Technical explanation:** Cherry-picks a commit and records the original SHA in the message, improving auditability across release branches.

**Use when:** Use when cross-branch traceability matters in production work.


## 83. `git cherry-pick --abort`


```bash

git cherry-pick --abort

```

**Risk:** Low risk

**Technical explanation:** Stops a failing cherry-pick sequence and restores the branch to its previous state.

**Use when:** Use when the cherry-pick path is wrong or conflict resolution is unsafe.


## 84. `git merge-base main hotfix/<name>`


```bash

git merge-base main hotfix/<name>

```

**Risk:** Low risk

**Technical explanation:** Finds the best common ancestor between two branches so you can scope divergence accurately.

**Use when:** Use when PR review scope or branch drift is unclear.


## 85. `git diff $(git merge-base main HEAD)..HEAD`


```bash

git diff $(git merge-base main HEAD)..HEAD

```

**Risk:** Low risk

**Technical explanation:** Shows only changes introduced on the current branch relative to its divergence point from main.

**Use when:** Use when you need exact branch-only delta before merge or rollback.


## 86. `git log --merge`


```bash

git log --merge

```

**Risk:** Low risk

**Technical explanation:** Lists commits relevant to the current conflicted merge, helping explain what is colliding.

**Use when:** Use during conflict resolution when commit context matters.


## 87. `git diff --check`


```bash

git diff --check

```

**Risk:** Low risk

**Technical explanation:** Detects whitespace errors and unresolved conflict markers before commit, reducing avoidable CI failures.

**Use when:** Use before finalizing conflict resolutions or hotfix commits.


## 88. `git rerere status`


```bash

git rerere status

```

**Risk:** Low risk

**Technical explanation:** Shows recorded conflict resolutions if rerere is enabled, which can help reuse prior fixes.

**Use when:** Use when repeated conflict patterns occur across maintenance branches.


## 89. `git rerere diff`


```bash

git rerere diff

```

**Risk:** Low risk

**Technical explanation:** Displays how rerere resolved conflicts so you can validate the automatic resolution.

**Use when:** Use when rerere applied a resolution and you want to inspect it safely.


## 90. `git mergetool`


```bash

git mergetool

```

**Risk:** Medium risk

**Technical explanation:** Launches the configured merge tool to resolve conflicts more safely in large or complex patches.

**Use when:** Use when manual command-line conflict editing becomes error-prone.


# Reset, Revert, and Safe Rollback


## 91. `git revert <commit>`


```bash

git revert <commit>

```

**Risk:** Medium risk

**Technical explanation:** Creates a new commit that undoes a prior commit, which is the safest rollback mechanism on shared branches.

**Use when:** Use when a bad shared commit must be undone without rewriting history.


## 92. `git revert <oldest>^..<newest>`


```bash

git revert <oldest>^..<newest>

```

**Risk:** Medium risk

**Technical explanation:** Reverts a contiguous commit range by generating inverse commits for each one.

**Use when:** Use when a bad change set spans several commits that are already shared.


## 93. `git revert -m 1 <merge-commit>`


```bash

git revert -m 1 <merge-commit>

```

**Risk:** High risk

**Technical explanation:** Reverts a merge commit relative to parent 1, which is necessary but delicate when undoing a merged PR.

**Use when:** Use when a merged PR must be rolled back on a shared integration branch.


## 94. `git reset --hard origin/main`


```bash

git reset --hard origin/main

```

**Risk:** High risk

**Technical explanation:** Resets the current branch to exactly match origin/main, discarding local tracked divergence completely.

**Use when:** Use only when local state should be thrown away in favor of the remote truth.


## 95. `git reset --hard <commit>`


```bash

git reset --hard <commit>

```

**Risk:** High risk

**Technical explanation:** Moves HEAD, index, and working tree to a chosen commit, which is powerful but destructive to local tracked changes.

**Use when:** Use only when you have backups or explicit certainty about discarding current state.


## 96. `git restore --source=<commit> -- path/to/file`


```bash

git restore --source=<commit> -- path/to/file

```

**Risk:** Medium risk

**Technical explanation:** Restores one file from another revision without changing branch history, enabling file-level rollback safely.

**Use when:** Use when only one file needs rollback rather than a whole commit.


## 97. `git checkout <commit> -- path/to/file`


```bash

git checkout <commit> -- path/to/file

```

**Risk:** Medium risk

**Technical explanation:** Checks out one file from a prior commit using older syntax common in legacy workflows.

**Use when:** Use when operating in older environments or following established team habits.


## 98. `git tag before-rollback-$(date +%Y%m%d%H%M%S)`


```bash

git tag before-rollback-$(date +%Y%m%d%H%M%S)

```

**Risk:** Low risk

**Technical explanation:** Creates a safety marker before rollback so you can return to the exact pre-rollback state if needed.

**Use when:** Use before revert sequences or other rollback work.


## 99. `git branch rollback-safety/<name>`


```bash

git branch rollback-safety/<name>

```

**Risk:** Low risk

**Technical explanation:** Creates a backup branch before destructive local rollback so the current state is preserved.

**Use when:** Use before reset, forceful recovery, or experimentation.


## 100. `git reflog --date=iso`


```bash

git reflog --date=iso

```

**Risk:** Low risk

**Technical explanation:** Shows timestamped reflog entries, which is valuable when aligning rollback points to incident timelines.

**Use when:** Use when you know roughly when the repository was in a safe state.


## 101. `git diff <bad-commit>^ <bad-commit>`


```bash

git diff <bad-commit>^ <bad-commit>

```

**Risk:** Low risk

**Technical explanation:** Shows the exact patch introduced by a suspected bad commit before deciding whether to revert it.

**Use when:** Use when confirming rollback scope for one change.


## 102. `git log --first-parent --oneline main`


```bash

git log --first-parent --oneline main

```

**Risk:** Low risk

**Technical explanation:** Shows mainline integration history only, making it easier to reason about which PR-level changes entered production.

**Use when:** Use when rollback decisions should be made at PR or merge level.


## 103. `git range-diff origin/main...HEAD`


```bash

git range-diff origin/main...HEAD

```

**Risk:** Low risk

**Technical explanation:** Compares two commit series, which is useful after rebases or patch re-rolls to ensure intended changes remain.

**Use when:** Use when rewritten history may have dropped or altered content unexpectedly.


## 104. `git revert --no-commit <commit>`


```bash

git revert --no-commit <commit>

```

**Risk:** Medium risk

**Technical explanation:** Applies a revert to the index and working tree without committing so you can review or batch rollback content first.

**Use when:** Use when the rollback should be inspected before final commit.


## 105. `git commit -m 'revert: rollback production issue'`


```bash

git commit -m 'revert: rollback production issue'

```

**Risk:** Low risk

**Technical explanation:** Finalizes a prepared revert with a clear audit-friendly message for incident tracking.

**Use when:** Use when the rollback content is staged and ready for permanent history.


# Tags, Releases, and Deployment Traceability


## 106. `git tag`


```bash

git tag

```

**Risk:** Low risk

**Technical explanation:** Lists local tags so you can inspect available release markers and version references.

**Use when:** Use when validating release availability on your machine.


## 107. `git tag -l 'v*'`


```bash

git tag -l 'v*'

```

**Risk:** Low risk

**Technical explanation:** Filters tags by pattern so you can locate versioned releases quickly.

**Use when:** Use when repositories contain many non-release tags.


## 108. `git show-ref --tags`


```bash

git show-ref --tags

```

**Risk:** Low risk

**Technical explanation:** Shows tag refs and target SHAs so you can verify exactly what each tag points to.

**Use when:** Use when deployment discrepancies suggest tag mispointing.


## 109. `git show <tag>`


```bash

git show <tag>

```

**Risk:** Low risk

**Technical explanation:** Displays the commit or annotated metadata behind a tag, helping validate release contents.

**Use when:** Use when a release tag may reference the wrong commit.


## 110. `git describe --tags --always`


```bash

git describe --tags --always

```

**Risk:** Low risk

**Technical explanation:** Produces the nearest tag-based description for the current commit, which is useful for build traceability.

**Use when:** Use when mapping a build or artifact back to Git history.


## 111. `git tag -a vX.Y.Z -m 'release vX.Y.Z'`


```bash

git tag -a vX.Y.Z -m 'release vX.Y.Z'

```

**Risk:** Medium risk

**Technical explanation:** Creates an annotated release tag with metadata, improving auditability over lightweight tags.

**Use when:** Use when preparing a formal release marker.


## 112. `git push origin <tag>`


```bash

git push origin <tag>

```

**Risk:** Medium risk

**Technical explanation:** Pushes a tag to GitHub so release automation or deployment tooling can discover it.

**Use when:** Use when a release tag exists locally but not remotely.


## 113. `git push origin --tags`


```bash

git push origin --tags

```

**Risk:** High risk

**Technical explanation:** Pushes all local tags, which can unintentionally publish tags not meant for release.

**Use when:** Use only when you intentionally want to publish every local tag.


## 114. `git fetch --tags`


```bash

git fetch --tags

```

**Risk:** Low risk

**Technical explanation:** Fetches tags from the remote so local investigations include the latest published markers.

**Use when:** Use when release or deployment analysis depends on fresh tag data.


## 115. `git ls-remote --tags origin`


```bash

git ls-remote --tags origin

```

**Risk:** Low risk

**Technical explanation:** Queries remote tags directly from GitHub to confirm release references without trusting local cache.

**Use when:** Use when tag presence or visibility is disputed.


# GitHub CLI, PRs, Actions, and Repository Operations


## 116. `gh auth status`


```bash

gh auth status

```

**Risk:** Low risk

**Technical explanation:** Shows current GitHub CLI authentication state so you can verify the account and host used for API operations.

**Use when:** Use when API, PR, Actions, or secret commands fail unexpectedly.


## 117. `gh repo view --web`


```bash

gh repo view --web

```

**Risk:** Low risk

**Technical explanation:** Opens the repository in the browser, which is useful for quickly confirming PR state, settings, or recent activity.

**Use when:** Use when a visual GitHub check is faster than CLI output alone.


## 118. `gh repo view`


```bash

gh repo view

```

**Risk:** Low risk

**Technical explanation:** Prints repository metadata so you can confirm visibility, default branch, and basic repo identity.

**Use when:** Use when repo targeting or host confusion may exist.


## 119. `gh pr list --state open`


```bash

gh pr list --state open

```

**Risk:** Low risk

**Technical explanation:** Lists open pull requests so you can see whether a hotfix or conflicting change is already in flight.

**Use when:** Use when parallel PR activity may explain branch behavior.


## 120. `gh pr view <number>`


```bash

gh pr view <number>

```

**Risk:** Low risk

**Technical explanation:** Shows pull request details including checks and review state, which is critical in release debugging.

**Use when:** Use when a specific PR may be blocking or causing production change.


## 121. `gh pr checkout <number>`


```bash

gh pr checkout <number>

```

**Risk:** Medium risk

**Technical explanation:** Checks out a PR branch locally so you can reproduce, inspect, or patch a pending change.

**Use when:** Use when local reproduction of a PR is needed.


## 122. `gh pr diff <number>`


```bash

gh pr diff <number>

```

**Risk:** Low risk

**Technical explanation:** Shows the PR diff directly from GitHub so reviewed content can be compared with local assumptions.

**Use when:** Use when local refs and GitHub UI seem inconsistent.


## 123. `gh pr status`


```bash

gh pr status

```

**Risk:** Low risk

**Technical explanation:** Summarizes PRs relevant to you and the current branch, helping diagnose whether work is already tracked upstream.

**Use when:** Use when branch-to-PR mapping is unclear.


## 124. `gh issue list --state open`


```bash

gh issue list --state open

```

**Risk:** Low risk

**Technical explanation:** Lists open issues, which can help correlate an incident with known repository or workflow problems.

**Use when:** Use when validating whether the issue is already known.


## 125. `gh run list --limit 20`


```bash

gh run list --limit 20

```

**Risk:** Low risk

**Technical explanation:** Shows recent GitHub Actions runs so you can spot failed CI/CD executions near an incident.

**Use when:** Use when deployment or validation may have failed in automation.


## 126. `gh run view <run-id> --log`


```bash

gh run view <run-id> --log

```

**Risk:** Low risk

**Technical explanation:** Streams logs for a workflow run, which is essential when deployment or validation failed in GitHub Actions.

**Use when:** Use when you need exact failure evidence from CI/CD.


## 127. `gh run rerun <run-id>`


```bash

gh run rerun <run-id>

```

**Risk:** Medium risk

**Technical explanation:** Retries a workflow run so you can test whether a failure was transient or after a fix was applied.

**Use when:** Use when a corrected or flaky workflow should be revalidated.


## 128. `gh run cancel <run-id>`


```bash

gh run cancel <run-id>

```

**Risk:** High risk

**Technical explanation:** Cancels a running workflow, which can stop a broken deployment or runaway job but may interrupt recovery paths.

**Use when:** Use when an active workflow is clearly harmful or invalid.


## 129. `gh workflow list`


```bash

gh workflow list

```

**Risk:** Low risk

**Technical explanation:** Lists repository workflows so you can identify which pipelines govern build, test, and deployment paths.

**Use when:** Use when the failing automation path is not yet known.


## 130. `gh workflow view <name>`


```bash

gh workflow view <name>

```

**Risk:** Low risk

**Technical explanation:** Shows workflow details so you can confirm triggers, names, and workflow identity.

**Use when:** Use when the wrong workflow may be running or missing.


## 131. `gh api repos/{owner}/{repo}/branches/main/protection`


```bash

gh api repos/{owner}/{repo}/branches/main/protection

```

**Risk:** Low risk

**Technical explanation:** Queries branch protection settings directly, which is useful when pushes or merges are blocked unexpectedly.

**Use when:** Use when governance or protection rules may explain blocked hotfixes.


## 132. `gh api repos/{owner}/{repo}/actions/runs`


```bash

gh api repos/{owner}/{repo}/actions/runs

```

**Risk:** Low risk

**Technical explanation:** Retrieves workflow run metadata from the GitHub API for deeper automation or incident analysis.

**Use when:** Use when you need structured run history beyond CLI summaries.


## 133. `gh release list`


```bash

gh release list

```

**Risk:** Low risk

**Technical explanation:** Lists GitHub releases so you can confirm what versions were published and when.

**Use when:** Use when release publication timing matters to the incident timeline.


## 134. `gh release view <tag>`


```bash

gh release view <tag>

```

**Risk:** Low risk

**Technical explanation:** Displays release metadata for a tag, helping validate notes, assets, and publication details.

**Use when:** Use when a release object may not match the intended tag.


## 135. `gh secret list`


```bash

gh secret list

```

**Risk:** Low risk

**Technical explanation:** Lists repository-level secrets so you can confirm required CI/CD secrets exist without exposing values.

**Use when:** Use when workflows fail due to missing secret configuration.


# Recovery Verification by Scenario

## After revert or rollback
Re-run:

- `git status --short --branch`
- `git log -1 --decorate --oneline`
- `git push --dry-run`
- `gh pr status`

Verify that the working tree is controlled, the expected rollback commit is at the tip, the push path is valid, and the PR/review state aligns with the rollback.

## After lost-work recovery
Re-run:

- `git reflog`
- `git log --oneline --decorate --graph -20`
- `git stash list`

Verify that the recovered commit or branch is now reachable, visible in history, and no important stash content was left behind.

## After branch divergence repair
Re-run:

- `git status --short --branch`
- `git fetch --all --prune`
- `git branch -vv`
- `git log HEAD..origin/main --oneline`
- `git log origin/main..HEAD --oneline`

Verify that upstream tracking is correct and divergence is now intentional or eliminated.

## After tag or release correction
Re-run:

- `git fetch --tags`
- `git show-ref --tags`
- `git show <tag>`
- `gh release view <tag>`

Verify that the tag points to the intended commit and the GitHub release object matches it.

## After GitHub Actions recovery
Re-run:

- `gh run list --limit 10`
- `gh run view <run-id> --log`
- `gh workflow view <name>`
- `gh secret list`

Verify that the failing workflow path is healthy again and required secret/config prerequisites still exist.

## Total Commands

This pack contains **135 commands**.

# Troubleshooting Discipline

Strong Git and GitHub incident response is not about memorizing the most commands. It is about:

- protecting history and auditability
- understanding the difference between local truth and GitHub truth
- using rollback methods appropriate for shared branches
- recovering lost work without creating a second incident
- validating that PRs, releases, and CI/CD are healthy after the fix

That is the standard expected from a strong senior Git, GitHub Platform, DevOps, or SRE engineer.

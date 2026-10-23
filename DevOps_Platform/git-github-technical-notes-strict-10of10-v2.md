# Git and GitHub Technical Notes

## 1. Core Mental Model

Git is a distributed version control system built around snapshots, commits, trees, references, and history operations.  
GitHub is a collaboration platform layered on top of Git that adds hosted repositories, pull requests, reviews, permissions, automation, branch protections, and workflow controls.

A strong working model is:

- **Git** = version control engine
- **GitHub** = hosting, review, protection, automation, and collaboration layer

Git tracks project history locally.  
GitHub coordinates how multiple people publish, review, protect, merge, and audit that history.

---

## 2. Repository Model and Internal Concepts

A Git repository stores history as a graph of commits connected by parent relationships.  
Understanding a few internal concepts makes branching, merging, rebasing, and recovery much easier.

### 2.1 Working Tree, Staging Area, and Repository

Git operates across three main states:

#### working tree

The working tree is the checked-out project files on disk that you edit directly.

#### staging area

The staging area, also called the index, is the set of changes prepared for the next commit.  
It lets you choose exactly what goes into a commit instead of committing every modified file blindly.

#### repository history

The repository stores committed snapshots and metadata.  
Commits are durable objects once created, though references to them can later move.

**Commands**

```sh
git init
git status
git add .
git add -p
git commit -m "Initial commit"
git log --oneline --graph --decorate
```

### 2.2 Commits, Trees, and Blobs

Git stores content as objects.

#### blob

A blob stores file content only.  
It does not inherently store a filename.

#### tree

A tree maps names to blobs and subtrees.  
It represents a directory snapshot structure.

#### commit

A commit points to a tree and records metadata such as parents, author, committer, and message.  
This is why Git is fundamentally snapshot-based rather than line-history-based.

### 2.3 HEAD, Refs, and Remote-Tracking Refs

Git uses named references, or refs, to point to commits.

#### HEAD

`HEAD` usually points to the currently checked-out branch reference.  
In detached HEAD state, it points directly to a commit rather than to a branch name.

#### branch refs

A branch such as `main` is a movable reference to a commit.  
When you commit on that branch, the branch ref advances.

#### remote-tracking refs

Refs such as `origin/main` represent Git’s local knowledge of the remote branch state after a fetch.  
They are not the remote itself; they are your last fetched view of it.

**Commands**

```sh
git show-ref --heads --tags
git branch -a
git branch -r
git symbolic-ref HEAD
```

### 2.4 Upstream Tracking

A local branch can be associated with an upstream branch.  
This lets Git know which remote branch to compare against for pull, push, and ahead/behind status.

**Commands**

```sh
git push -u origin feature/login
git branch -vv
```

### 2.5 Local vs Remote

Git is distributed, which means your local repository contains real history, not just a cache.  
A remote such as GitHub is another repository used for sharing and coordination.

**Commands**

```sh
git remote -v
git remote add origin https://github.com/org/repo.git
git fetch origin
git push -u origin main
git remote show origin
```

### 2.6 Fetch vs Pull

`git fetch` updates your local remote-tracking refs without changing your current branch.  
`git pull` is typically fetch plus merge, or fetch plus rebase if configured that way.

#### practical rule

Use `fetch` when you want to inspect incoming changes before integrating them.  
Use `pull` when you intentionally want to update your checked-out branch immediately.

**Commands**

```sh
git fetch origin
git pull --ff-only origin main
```

### 2.7 Detached HEAD

Detached HEAD means you checked out a commit directly instead of being on a named branch.  
This is useful for inspection, bisecting, or temporary experiments, but new commits made there are easy to lose if you do not create a branch.

**Commands**

```sh
git switch --detach <commit>
git switch -c investigate-bug
```

---

## 3. Commits, History, and Inspection

A commit is a snapshot plus metadata such as author, message, and parent commit references.  
Good collaboration depends heavily on writing clear commits and maintaining understandable history.

### 3.1 Good Commit Characteristics

Strong commits are usually:

- focused on one logical change
- clearly messaged
- testable where possible
- small enough to review without confusion

### 3.2 Commit Messages

A good commit message explains intent, not just file edits.

**Example**

```text
Add retry handling for GitHub API rate limit responses
```

Better messages help during:

- code review
- debugging
- blame/history inspection
- release note generation
- cherry-picking

### 3.3 Inspecting History

**Commands**

```sh
git log
git log --oneline --graph --decorate --all
git show <commit>
git diff
git diff --staged
git blame path/to/file
git log --follow -- path/to/file
```

### 3.4 Tags

Tags mark important points in history, often for releases or milestones.

#### lightweight tags

A lightweight tag is a simple named reference to a commit.

#### annotated tags

An annotated tag stores metadata such as tagger, date, and message.  
Annotated tags are usually preferable for meaningful releases.

**Commands**

```sh
git tag
git tag v2.4.0
git tag -a v2.4.0 -m "Release v2.4.0"
git show v2.4.0
git push origin v2.4.0
```

### 3.5 Cherry-Pick

`git cherry-pick` applies the changes introduced by an existing commit onto the current branch.  
It is useful for backports, selective fixes, and hotfix propagation.

**Commands**

```sh
git cherry-pick <commit>
git cherry-pick -x <commit>
```

#### collaboration caveat

Cherry-pick copies change content into a new commit rather than moving the original commit.  
Overuse can create duplicated logic history if teams do not track backports clearly.

---

## 4. Branching Fundamentals

Branches are lightweight movable pointers to commits.  
They are one of Git’s most important features for parallel work and safe collaboration.

### 4.1 What a Branch Really Is

A branch is not a copy of the whole repository in the usual sense.  
It is mainly a named reference to a commit, and as new commits are made on that branch, the reference moves forward.

### 4.2 Why Branching Matters

Branching allows teams to:

- isolate work in progress
- develop features in parallel
- fix bugs without destabilizing the main line
- review changes before integration
- experiment safely

### 4.3 Common Branch Types

#### main or master

This is the primary integration branch.  
Modern repositories usually use `main`.

#### feature branches

Feature branches are short-lived branches used for a specific change or task.

#### release branches

Release branches are used to prepare a release while other development continues elsewhere.

#### hotfix branches

Hotfix branches isolate urgent production fixes for fast review and deployment.

#### backport branches

Backport branches are used to port selected fixes to older supported release lines.

### 4.4 Creating and Switching Branches

**Commands**

```sh
git branch
git branch feature/login
git switch feature/login
git switch -c feature/login
git checkout -b feature/login
```

### 4.5 Branch Naming Conventions

Clear naming improves collaboration and repository hygiene.

**Examples**

```text
feature/login-form
bugfix/token-refresh
hotfix/payment-timeout
release/v2.4.0
backport/v2.3/fix-timeout
chore/update-ci
```

#### collaboration note

Strong branch naming helps reviewers, CI dashboards, and release coordination.  
Ambiguous names like `test`, `fix`, or `newbranch` create unnecessary confusion.

---

## 5. Branching Strategies

Branching strategy determines how teams organize work and integrate change.  
There is no single best model for every team, but the tradeoffs matter.

### 5.1 Trunk-Based Development

In trunk-based development, developers integrate frequently into `main` or a short-lived integration branch.  
Feature branches exist, but they are typically small and short-lived.

#### strengths

- fast integration
- fewer long-lived merge conflicts
- strong CI discipline
- reduced branch drift

#### caveats

- requires frequent syncing
- demands disciplined testing
- large unfinished work may need feature flags instead of long-lived branches

### 5.2 Git Flow

Git Flow uses longer-lived branches such as `develop`, release branches, and hotfix branches.  
It can work for structured release processes but is often heavier than necessary for continuous delivery teams.

#### strengths

- explicit release structure
- clear distinction between feature, release, and hotfix work

#### caveats

- more branching overhead
- more merge complexity
- often slower integration

### 5.3 Lightweight Feature Branch Workflow

This is common on GitHub.  
Developers branch from `main`, commit changes, open a pull request, review, merge, and delete the branch.

#### strengths

- simple mental model
- review-friendly
- works well with protected branches
- aligns naturally with GitHub PR workflows

### 5.4 Stacked Pull Request Workflow

In a stacked workflow, a large change is split across multiple dependent branches and PRs.  
Each PR is smaller and easier to review, while later PRs build on earlier ones.

#### strengths

- smaller review units
- better review focus
- reduced giant-PR risk

#### caveats

- requires branch dependency awareness
- rebases and update coordination become more important
- merge order matters

---

## 6. Merging Fundamentals

Merging combines histories from different branches.  
Understanding merge behavior is essential to safe collaboration.

### 6.1 Merge Base

The merge base is the best common ancestor between two lines of history used as a reference point for merge calculations.  
Thinking in terms of merge base helps explain why Git sees some changes as conflicting and others as independent.

**Commands**

```sh
git merge-base main feature/login
```

### 6.2 Fast-Forward Merge

A fast-forward merge happens when the target branch has not diverged.  
Git simply moves the branch pointer forward.

**Commands**

```sh
git switch main
git merge feature/login
```

#### result

No merge commit is created because history is linear and unambiguous.

### 6.3 Three-Way Merge

A three-way merge happens when both branches have diverged.  
Git uses the common ancestor and both branch tips to create a merge result.

#### result

A merge commit is typically created.

### 6.4 Merge Commit

A merge commit has more than one parent.  
It preserves the fact that separate lines of development were integrated.

### 6.5 When Merge Commits Are Useful

Merge commits are useful when you want to preserve branch integration history, especially in:

- team feature integration
- release branches
- multi-commit PRs where history grouping matters

### 6.6 Merge Command Examples

**Commands**

```sh
git switch main
git merge feature/login
git merge --no-ff feature/login
git merge --ff-only feature/login
git merge --squash feature/login
```

#### `--no-ff`

Forces a merge commit even if fast-forward is possible.  
This preserves explicit branch integration history.

#### `--ff-only`

Allows the merge only if it can be fast-forwarded.  
This is useful when teams want strictly linear history.

#### `--squash`

Applies the branch changes as a single staged result without a merge commit.  
This is conceptually different from preserving the branch’s true commit structure.

---

## 7. Rebasing Fundamentals

Rebasing rewrites commits onto a new base commit.  
It can produce cleaner linear history, but it must be used carefully in collaborative environments.

### 7.1 What Rebase Does

Rebase takes commits from one branch and reapplies them on top of another base.  
This changes commit hashes because the commit ancestry changes.

### 7.2 Basic Rebase Flow

**Commands**

```sh
git switch feature/login
git fetch origin
git rebase origin/main
```

### 7.3 Interactive Rebase

Interactive rebase allows you to edit, reorder, squash, reword, or drop commits.

**Commands**

```sh
git rebase -i HEAD~5
```

### 7.4 Rebase vs Merge

#### merge

- preserves true branch structure
- does not rewrite existing commits
- usually safer for shared branch history

#### rebase

- creates cleaner linear history
- rewrites commits
- useful before opening or updating a PR
- dangerous if done carelessly on shared public branches

### 7.5 Collaboration Rule for Rebase

Do not casually rebase commits that other people are already using from a shared branch.  
History rewriting on a shared branch can create confusion, duplicate commits, and force-push disruption.

### 7.6 Recovering During Rebase

**Commands**

```sh
git rebase --continue
git rebase --skip
git rebase --abort
```

### 7.7 Rebase and Review Context

Force-pushing a rebased PR branch can invalidate comment positions, confuse reviewers, or hide the history of how feedback was addressed.  
When rebasing during review, communicate clearly and avoid unnecessary history churn.

---

## 8. Merge Conflicts and Resolution Strategy

A merge conflict occurs when Git cannot automatically reconcile competing changes.  
Conflicts are normal in collaboration and should be handled calmly and explicitly.

### 8.1 Common Causes

Conflicts often happen when:

- two branches edit the same lines
- one branch deletes a file another branch modifies
- renames and edits overlap
- long-lived branches drift too far apart
- large refactors overlap with feature work

### 8.2 Conflict Markers

Git inserts markers into files during unresolved conflicts.

```text
<<<<<<< HEAD
current branch version
=======
incoming branch version
>>>>>>> feature/login
```

You must edit the file to the correct final form and then stage it.

### 8.3 Conflict Resolution Flow

**Commands**

```sh
git status
git add path/to/file
git merge --continue
```

For rebases:

```sh
git rebase --continue
```

### 8.4 Good Conflict Practices

- understand both sides before editing
- run tests after resolution
- do not blindly accept “ours” or “theirs” without understanding impact
- sync often to reduce conflict size

### 8.5 Ours vs Theirs Nuance

During merge and rebase, “ours” and “theirs” can mean different practical sides depending on the operation context.  
Be careful and verify which version you are selecting.

**Commands**

```sh
git checkout --ours path/to/file
git checkout --theirs path/to/file
```

### 8.6 Rename, Delete, and Binary Conflict Cases

Not all conflicts are simple line-by-line text conflicts.  
Rename/rename, delete/modify, and binary conflicts require more deliberate decisions because Git cannot always infer author intent safely.

### 8.7 GitHub UI vs Local Conflict Resolution

GitHub can resolve some simple text conflicts in the browser, but local resolution is often safer for non-trivial conflicts.  
Complex refactors, multi-file dependency changes, and test validation usually belong in local Git, not only in the web UI.

---

## 9. Pull Requests on GitHub

A pull request, or PR, is a proposal to merge changes from one branch into another on GitHub.  
It is both a technical merge mechanism and a collaboration artifact.

### 9.1 What a Pull Request Includes

A PR usually contains:

- source branch
- target branch
- commits
- diff
- title and description
- review discussion
- status checks
- approval state

### 9.2 Why Pull Requests Matter

PRs enable:

- code review
- discussion before merge
- CI validation
- audit trail
- safer collaboration
- knowledge sharing

### 9.3 Typical PR Flow

1. Create a branch from `main`
2. Make commits locally
3. Push the branch to GitHub
4. Open a pull request
5. Request review
6. Address feedback
7. Ensure checks pass
8. Merge
9. Delete the branch

### 9.4 Opening a Pull Request Well

A strong PR should include:

- clear title
- clear summary
- why the change is needed
- risk areas
- testing notes
- screenshots or logs where useful
- links to issues or tickets

### 9.5 PR Size Guidance

Smaller PRs are easier to review, test, and merge safely.  
Very large PRs increase review fatigue and reduce review quality.

### 9.6 Draft Pull Requests

Draft PRs allow early visibility before the work is ready to merge.  
They are useful for feedback without signaling final review readiness.

### 9.7 Linking Issues

PRs often reference or automatically close issues.  
Clear issue linking improves auditability and project tracking.

---

## 10. Pull Request Review Workflows

Review is one of the most important GitHub collaboration practices.  
Good review improves correctness, maintainability, and team knowledge sharing.

### 10.1 Reviewer Responsibilities

A reviewer should look for:

- correctness
- design clarity
- maintainability
- security concerns
- edge cases
- tests
- style consistency where relevant

### 10.2 Author Responsibilities

An author should:

- keep the PR focused
- explain the change clearly
- respond respectfully to review comments
- update the branch when needed
- avoid defensive or unclear replies

### 10.3 Types of Review Feedback

GitHub reviews typically allow:

- comment
- approve
- request changes

### 10.4 Good Review Comment Style

Strong review comments are:

- specific
- actionable
- respectful
- technically justified

### 10.5 Resolving Review Feedback

After making changes:

- push follow-up commits
- reply to comments where appropriate
- mark conversations resolved only when the issue is actually addressed
- avoid silent changes that ignore agreed review direction

### 10.6 Suggestions, Follow-Up Commits, and Squashing

During review, teams should be deliberate about whether to keep granular fixup commits visible or squash before merge.  
The right choice depends on whether intermediate review history is valuable or merely noisy.

### 10.7 Stale Reviews and Re-Review

Some repositories dismiss approvals when new commits are pushed.  
This protects merge quality because major post-review changes should often receive fresh review rather than relying on outdated approval context.

---

## 11. Pull Request Merge Methods

GitHub usually offers multiple merge methods.  
The right choice depends on team history preferences and workflow discipline.

### 11.1 Merge Commit

This creates a merge commit when the PR is merged.  
It preserves the branch structure and full commit history.

### 11.2 Squash and Merge

This combines all PR commits into a single commit on the target branch.  
It keeps `main` cleaner and is common for feature-branch workflows.

### 11.3 Rebase and Merge

This reapplies PR commits onto the target branch without a merge commit.  
It creates linear history while preserving individual commits.

### 11.4 Choosing a Merge Policy

A team should choose merge policy intentionally.  
Mixed habits without agreement often create confusing history.

### 11.5 Commit Message Strategy for Squash Merges

When squash merging, the final commit message becomes the long-term history record on the target branch.  
Teams should treat that message as a durable explanation, not a throwaway default.

---

## 12. Keeping Branches Up to Date

Branches drift over time as `main` continues moving.  
Syncing regularly reduces conflict size and surprise failures.

### 12.1 Fetch First

**Commands**

```sh
git fetch origin
```

### 12.2 Update with Merge

**Commands**

```sh
git switch feature/login
git merge origin/main
```

### 12.3 Update with Rebase

**Commands**

```sh
git switch feature/login
git rebase origin/main
```

### 12.4 Which to Use

- use **merge** when preserving exact branch history matters or shared branch safety is the priority
- use **rebase** when you want a cleaner branch history before merge and you control the branch safely

### 12.5 Force Push After Rebase

If you rebase a branch already pushed to GitHub, you usually need to force-push the rewritten history.

**Commands**

```sh
git push --force-with-lease
```

#### important caveat

Prefer `--force-with-lease` over plain `--force`.  
It is safer because it helps prevent overwriting remote changes unexpectedly.

---

## 13. Collaboration Workflows

Most real team Git usage is about collaboration workflow, not just raw commands.

### 13.1 Basic Feature Branch Workflow

1. Pull latest `main`
2. Create feature branch
3. Commit logically
4. Push branch
5. Open PR
6. Review and fix
7. Merge
8. Delete branch

**Commands**

```sh
git switch main
git pull --ff-only origin main
git switch -c feature/login
git add .
git commit -m "Add login validation"
git push -u origin feature/login
```

### 13.2 Bugfix Workflow

Bugfixes often follow the same flow as feature work but should usually be even more focused.  
A good bugfix PR should include the cause, fix, and evidence of verification.

### 13.3 Hotfix Workflow

A hotfix workflow is used for urgent production fixes.  
The branch may come from a release branch or the production branch, depending on the team’s release model.

### 13.4 Backport Workflow

A backport workflow selectively applies an already-merged fix to an older maintained branch.  
Cherry-pick is common here, but the team should document what was backported and why.

**Commands**

```sh
git switch release/v2.3
git cherry-pick -x <commit>
git push origin release/v2.3
```

### 13.5 Pairing and Shared Branches

Shared branches can work, but they increase coordination cost.  
Teams should prefer individual contributor branches unless there is a clear reason for a shared branch.

### 13.6 Fork-Based Workflow

In open source, contributors often work from forks rather than direct branches on the upstream repository.

**Commands**

```sh
git remote add upstream https://github.com/upstream/repo.git
git fetch upstream
git switch -c feature/docs upstream/main
git push -u origin feature/docs
```

### 13.7 Stacked PR Collaboration Pattern

A stacked workflow is valuable when a feature is too large for one PR but should still be reviewed incrementally.  
Each branch targets the previous one or is carefully rebased as lower layers merge.

---

## 14. GitHub Collaboration Controls

GitHub adds team-oriented features beyond raw Git.

### 14.1 Reviewers and CODEOWNERS

Repositories can automatically request reviewers based on file paths or ownership rules.  
This helps route changes to the right teams.

#### caveat

Very broad ownership patterns can overload teams and slow reviews.  
Ownership rules should be specific enough to be useful.

### 14.2 Branch Protection Rules

Branch protection rules prevent unsafe direct changes and enforce workflow discipline.

Common protections include:

- require pull request before merge
- require approvals
- require status checks
- block force pushes
- require linear history
- require conversation resolution
- restrict who can push
- dismiss stale approvals
- require signed commits in selected workflows
- apply rules to administrators where needed

#### policy note

Protection is only strong if exceptions are rare and intentional.  
A branch that admins bypass routinely is not meaningfully protected.

### 14.3 Required Reviews

Required reviewer count affects speed and confidence.  
Too few reviewers weakens quality control, while too many slows delivery and can dilute accountability.

### 14.4 Status Checks and CI

GitHub can require automated checks before merge, such as:

- tests
- linting
- builds
- security scanning

### 14.5 Auto-Merge and Merge Queue Awareness

Auto-merge can reduce manual coordination once all conditions are satisfied.  
Merge queue style workflows help protect a busy target branch from repeated “green when tested, red after merge” races.

### 14.6 Branch Deletion and Hygiene

Automatically deleting merged branches helps reduce clutter and signals lifecycle completion.  
Branch cleanup is a collaboration hygiene practice, not only a cosmetic preference.

---

## 15. Safe History Rewriting

History rewriting can be useful locally but dangerous remotely.  
The core rule is to understand who else depends on the history you are rewriting.

### 15.1 amend

Use amend to adjust the most recent commit.

**Commands**

```sh
git commit --amend
```

### 15.2 interactive cleanup before PR

Interactive rebase is often appropriate before review to:

- remove accidental commits
- improve commit messages
- combine tiny fixup commits
- reorder commits logically

### 15.3 when not to rewrite

Avoid rewriting history on:

- protected branches
- widely shared branches
- branches others are actively consuming
- already-released public history unless absolutely necessary

### 15.4 Communicating Rewrites

When a branch history is rewritten after review has started, communicate clearly in the PR.  
Reviewers need to know whether to re-review the entire branch, only new commits, or a cleaned-up equivalent history.

---

## 16. Undo, Cleanup, and Recovery

Git feels dangerous mostly when recovery is poorly understood.  
In practice, many mistakes are recoverable.

### 16.1 reset vs restore vs checkout

`git reset` moves references and may also affect the index and working tree.  
`git restore` is focused on restoring file content or index state.  
`git checkout` is older and historically overloaded for both branch switching and file restoration, which is why `switch` and `restore` are often clearer.

### 16.2 Restore Working Tree Changes

**Commands**

```sh
git restore path/to/file
git restore .
```

### 16.3 Unstage Changes

**Commands**

```sh
git restore --staged path/to/file
```

### 16.4 Reset

**Commands**

```sh
git reset --soft HEAD~1
git reset --mixed HEAD~1
git reset --hard HEAD~1
```

### 16.5 Revert

`git revert` creates a new commit that undoes an earlier commit.  
This is usually safer than rewriting shared history.

**Commands**

```sh
git revert <commit>
git revert -m 1 <merge-commit>
```

### 16.6 Reflog

`reflog` is one of Git’s most valuable recovery tools.  
It records where references have pointed recently, even when history seems lost.

**Commands**

```sh
git reflog
git reset --hard <reflog-commit>
```

### 16.7 Stash

`git stash` temporarily stores uncommitted changes so you can switch context.  
It is convenient, but it should not become a long-term hidden work queue.

**Commands**

```sh
git stash
git stash list
git stash apply
git stash pop
```

### 16.8 Clean

`git clean` removes untracked files from the working tree.  
It is powerful and potentially destructive.

**Commands**

```sh
git clean -fd
git clean -fdx
```

---

## 17. Advanced Practical Workflows

### 17.1 Bisect

`git bisect` helps find the commit that introduced a bug through binary search across history.  
It is extremely valuable when a regression exists but the exact change is unknown.

**Commands**

```sh
git bisect start
git bisect bad
git bisect good <known-good-commit>
```

### 17.2 Worktree

`git worktree` lets you check out multiple branches from the same repository into separate directories.  
It is useful for parallel review, hotfix work, or comparing branches without constant switching.

**Commands**

```sh
git worktree add ../repo-hotfix hotfix/payment-timeout
git worktree list
```

### 17.3 Delete Remote Branches

Remote branch cleanup is part of workflow hygiene.

**Commands**

```sh
git push origin --delete feature/login
```

---

## 18. Useful Everyday Commands

```sh
git status
git add .
git add -p
git commit -m "Message"
git log --oneline --graph --decorate --all
git diff
git diff --staged
git switch main
git switch -c feature/name
git switch --detach <commit>
git branch -vv
git branch -a
git branch -r
git fetch origin
git pull --ff-only origin main
git push -u origin feature/name
git push --force-with-lease
git merge feature/name
git merge-base main feature/name
git merge --squash feature/name
git rebase origin/main
git rebase -i HEAD~5
git cherry-pick <commit>
git tag -a v1.2.0 -m "Release v1.2.0"
git stash
git stash pop
git restore .
git restore --staged .
git revert <commit>
git reflog
git clean -fd
git bisect start
git worktree list
```

---

## 19. Strong Collaboration Practices

### 19.1 Keep branches short-lived

Short-lived branches reduce merge pain and review delay.

### 19.2 Sync often

Frequent fetch/rebase or fetch/merge reduces integration surprises.

### 19.3 Prefer small PRs

Small PRs are easier to review and safer to merge.

### 19.4 Protect main branches

Use branch protection and required checks for important branches.

### 19.5 Do not force-push carelessly

Force-pushing shared history can disrupt teammates and invalidate review context.

### 19.6 Explain intent in PRs

A reviewer should understand why the change exists, not just what files changed.

### 19.7 Separate refactors from behavior changes

Mixing refactors and logic changes in one PR makes review harder and riskier.

### 19.8 Delete merged branches

Deleting merged branches reduces clutter and signals workflow completion.

### 19.9 Split large work into stacked or sequential PRs

Breaking large work into reviewable units improves feedback quality and lowers merge risk.

### 19.10 Be explicit about merge policy

Teams should document whether they prefer merge commits, squash merges, or rebase merges.  
A workflow is stronger when the merge method is intentional rather than accidental.

---

## 20. Common Anti-Patterns

### 20.1 Long-lived stale feature branches

These accumulate merge conflicts and drift from current architecture.

### 20.2 Massive pull requests

Large PRs reduce review quality and often hide risky changes.

### 20.3 Force-pushing shared branches without coordination

This creates confusion and can overwrite others’ work.

### 20.4 Direct commits to protected branches

This bypasses review and reduces auditability.

### 20.5 Mixing unrelated changes

Unrelated fixes, refactors, and features in one branch create confusing history.

### 20.6 Poor commit messages

Messages like `fix`, `update`, or `changes` weaken history usefulness.

### 20.7 Treating GitHub UI as a substitute for Git understanding

GitHub helps collaboration, but core Git concepts still matter for safe work.

### 20.8 Rebasing public shared history casually

This breaks expectations for collaborators and often creates unnecessary recovery work.

### 20.9 Solving review pain with bigger PRs instead of better slicing

When work is hard to review, the fix is often better decomposition, not more reviewer effort.

---

## 21. Practical End-to-End Example

### 21.1 Start a feature

```sh
git switch main
git pull --ff-only origin main
git switch -c feature/login-validation
```

### 21.2 Commit the work

```sh
git add .
git commit -m "Add login input validation"
```

### 21.3 Push and open PR

```sh
git push -u origin feature/login-validation
```

Open the PR on GitHub with:

- summary
- motivation
- test notes
- linked issue

### 21.4 Update branch after main changes

```sh
git fetch origin
git rebase origin/main
git push --force-with-lease
```

### 21.5 Merge and clean up

After approvals and passing checks, merge the PR and delete the branch.

```sh
git switch main
git pull --ff-only origin main
git branch -d feature/login-validation
git push origin --delete feature/login-validation
```

### 21.6 Backport a fix

```sh
git switch release/v2.3
git cherry-pick -x <merged-fix-commit>
git push origin release/v2.3
```

---

## 22. Final Framing

Git gives you precise control over version history.  
GitHub gives teams a structured way to review, discuss, protect, and integrate that history.

The most important practical skills are not just memorizing commands, but understanding how to:

- create clean branches
- make focused commits
- sync safely
- resolve conflicts carefully
- open strong pull requests
- review constructively
- merge intentionally
- protect important branches
- collaborate without breaking shared history

For most teams, mastering **branching, merging, pull requests, and safe collaboration habits** provides the biggest real-world value.

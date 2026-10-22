# Git Technical Notes — Advanced and Deep Concepts, Internals, GitHub Workflows, Actions, Repository Management, and Commands

This file is a **deep technical-notes document** for Git and GitHub-oriented team workflows, written to be close to an **expert-reference study file** rather than a basic command summary.

It is designed to explain:

- what Git is and how its model works
- Git internals such as **objects, refs, HEAD, index, trees, blobs, commits, packfiles, and reflog**
- advanced topics such as **resolving conflicts**, **cherry-picking**, **stashing**, **rebase**, and **working with remote repositories**
- GitHub workflow concepts such as **pull requests, reviews, merge types, repository management, releases, tags, and team collaboration**
- **GitHub Actions** for automation and CI/CD
- important commands with examples
- technical distinctions that are commonly misunderstood in interviews and real-world engineering work

This file is intentionally written as a **technical understanding document**, not an issue log.

---

# 1) What Git is

Git is a **distributed version control system**.

It is used to:
- track changes to files over time
- preserve and explore history
- support branching and merging
- enable collaboration across developers and repositories
- recover, rewrite, compare, or selectively apply project history

A concise technical definition is:

> Git is a distributed content-addressed version control system that stores project history as snapshots of objects and allows local history manipulation before synchronization with remotes.

That wording matters because Git is not just “a tool for commits.”  
It is a **content-addressed object database** with a history and reference model.

---

# 2) The Git mental model

To understand Git deeply, think in terms of these layers:

1. **Working tree**
2. **Staging area (index)**
3. **Local object database**
4. **References**
5. **Remote repositories**
6. **Remote-tracking references**

Git becomes much easier when you stop thinking of it as “files moving between commands” and start thinking of it as transitions between these layers.

---

## 2.1 Working tree

The **working tree** is the set of files currently checked out in your filesystem.

This is where you edit source code directly.

If you modify a file, that change exists first in the working tree.

---

## 2.2 Staging area (index)

The **index**, also called the **staging area**, is the intermediate structure between the working tree and the next commit.

When you run:

```bash
git add file.txt
```

you are not committing.  
You are updating the index so the next commit uses that exact file content.

This is one of Git’s most important ideas:
- working tree = current edits
- index = proposed next snapshot
- commit = saved repository snapshot

The index is not just a vague staging concept.  
It is a real data structure that records path entries, object IDs, and metadata used to assemble the next commit.

---

## 2.3 Local object database

Git stores objects inside `.git/objects/`.

The repository contains:
- blobs
- trees
- commits
- annotated tags
- packfiles
- refs
- logs and metadata

This is why Git is both:
- a version control system
- and an object database

---

## 2.4 References

References name important points in history.

Examples:
- branches
- tags
- `HEAD`
- remote-tracking branches

A branch is fundamentally a movable reference to a commit.

---

## 2.5 Remote repositories

A remote repository is another Git repository, usually hosted somewhere else.

Examples:
- GitHub
- GitLab
- Bitbucket
- internal Git server

Git does not require a remote to function, but remotes are essential for collaboration.

---

## 2.6 Remote-tracking references

References such as `origin/main` are **remote-tracking refs**.

They are local references that record the last fetched known state of the remote branch.

This distinction is crucial:
- `main` = your local branch
- `origin/main` = your last fetched view of the remote branch

---

# 3) Git object model

Git becomes much easier when you understand the object model.

## 3.1 Blob

A **blob** stores file content.

It does not know:
- filename
- directory name
- path hierarchy

It only stores content.

---

## 3.2 Tree

A **tree** represents a directory-like structure.

It maps names to:
- blobs
- subtrees
- sometimes submodule entries

A tree gives structure to content.

---

## 3.3 Commit

A **commit** contains:
- a pointer to a root tree
- zero or more parent commits
- author metadata
- committer metadata
- message

A commit is not merely “a diff.”  
Conceptually, it is a snapshot reference with ancestry links.

Git can compute diffs between snapshots, but the object itself is a snapshot-oriented record.

---

## 3.4 Annotated tag

An annotated tag is a tag object with metadata.

It can include:
- tagger
- date
- message
- signature

This is different from a lightweight tag, which is basically just a reference name pointing to an object.

---

## 3.5 Object IDs and content addressing

Git objects are referenced by IDs derived from content.

This means object identity depends on object content.

That is a major reason Git is reliable for history integrity:
- content changes
- object ID changes

This content-addressed design is central to Git’s internals.

---

# 4) Refs, HEAD, and branch mechanics

## 4.1 What a branch really is

A branch is a lightweight movable pointer to a commit.

It is not a separate copy of the repository and not a special storage area.  
It is a named reference.

This is why branching in Git is cheap.

---

## 4.2 What HEAD is

`HEAD` usually points to the currently checked-out branch reference.

Example:

```text
HEAD -> refs/heads/main -> commit
```

When you commit on `main`, Git:
- creates a new commit
- moves `main` to that commit
- `HEAD` still points to `main`

---

## 4.3 Detached HEAD

In detached HEAD state, `HEAD` points directly to a commit rather than a branch.

Example:

```bash
git switch --detach a1b2c3d
```

This is useful for:
- inspecting old commits
- testing a historical state
- experimenting temporarily

But if you commit in detached HEAD and move away without creating a branch, that work can become harder to find later unless recovered through reflog.

---

## 4.4 Symbolic refs

`HEAD` is often a symbolic reference.  
It points to another ref rather than directly to an object ID.

This is one reason `HEAD` behaves differently from a plain branch name.

---

# 5) The index in more depth

## 5.1 Why the index matters technically

Many Git users understand the index only as “the staging area,” but it is more precise than that.

The index allows Git to:
- construct the next commit snapshot
- compare working tree, index, and HEAD separately
- support partial staging
- support merge conflict entries
- track multiple stages during unresolved merges

This is why commands like these are distinct:

```bash
git diff
git diff --cached
git status
```

Each compares different layers.

---

## 5.2 Partial staging

One powerful Git feature is staging only part of a file.

```bash
git add -p
```

This lets you split unrelated edits into separate commits even if they were made in the same file.

That is a major advantage of the index model.

---

## 5.3 Unmerged index entries

During conflicts, the index can store multiple entries for the same path representing different merge stages.

This is one of the reasons Git can pause a merge and let you resolve conflicts manually.

The index is therefore not just a “ready-to-commit box”; it can temporarily hold merge state.

---

# 6) Repository lifecycle basics

A common Git workflow involves:

1. create or clone a repository
2. edit files
3. stage changes
4. commit changes
5. create branches
6. merge, rebase, cherry-pick, or revert history
7. synchronize with remotes
8. recover or rewrite history when needed

---

# 7) Basic commands that still matter

Even in an advanced Git notes file, the basics must be precise.

## 7.1 `git init`

Creates a new repository.

```bash
git init
```

This initializes `.git/` and creates the local Git database and ref structure.

---

## 7.2 `git clone`

Clones a remote repository locally.

```bash
git clone https://example.com/team/project.git
```

Clone into a different directory:

```bash
git clone https://example.com/team/project.git myproject
```

Cloning brings:
- object history
- refs
- remote configuration
- working tree checkout

---

## 7.3 `git status`

Shows the current repository state.

```bash
git status
```

It tells you:
- current branch or detached HEAD state
- staged changes
- unstaged changes
- untracked files
- whether a merge, rebase, or cherry-pick is in progress

This is one of the highest-value Git commands.

---

## 7.4 `git add`

Stages changes.

```bash
git add app.py
```

Stage all:

```bash
git add .
```

Interactive patch mode:

```bash
git add -p
```

---

## 7.5 `git commit`

Creates a commit from the index.

```bash
git commit -m "Add order validation"
```

Amend previous commit:

```bash
git commit --amend
```

This rewrites the current branch tip commit.

---

## 7.6 `git log`

Show history:

```bash
git log
```

Useful graph form:

```bash
git log --oneline --graph --decorate --all
```

Show one commit in detail:

```bash
git show a1b2c3d
```

---

## 7.7 `git diff`

Unstaged changes:

```bash
git diff
```

Staged changes:

```bash
git diff --cached
```

Between two refs:

```bash
git diff main..feature/auth
```

---

# 8) Branching and branch management

## 8.1 Create a branch

```bash
git branch feature/auth
```

---

## 8.2 Switch branch

```bash
git switch feature/auth
```

Create and switch:

```bash
git switch -c feature/auth
```

Older style:

```bash
git checkout -b feature/auth
```

---

## 8.3 List branches

Local branches:

```bash
git branch
```

All branches including remote-tracking refs:

```bash
git branch -a
```

Verbose branch tracking info:

```bash
git branch -vv
```

---

## 8.4 Delete branch

Delete merged branch:

```bash
git branch -d feature/auth
```

Force delete:

```bash
git branch -D feature/auth
```

---

## 8.5 Rename branch

```bash
git branch -m old-name new-name
```

---

# 9) Merge mechanics

## 9.1 What merge does

A merge integrates histories.

Example:

```bash
git switch main
git merge feature/auth
```

Possible outcomes:
- fast-forward
- merge commit
- conflict

---

## 9.2 Fast-forward merge

If the current branch tip is an ancestor of the incoming branch tip, Git can simply move the branch pointer forward.

This creates no new merge commit.

---

## 9.3 Three-way merge concept

When histories diverged, Git often performs a **three-way merge**.

It considers:
- merge base
- current branch tip
- incoming branch tip

This is one of the most important technical concepts in Git merging.

Git is not comparing only “branch A vs branch B.”  
It usually compares both against their common ancestor to infer how to combine changes.

---

## 9.4 Merge commit

If a fast-forward is not possible, Git may create a merge commit with multiple parents.

This preserves the integration point explicitly in history.

---

## 9.5 Merge strategies and strategy options

Git supports merge strategies and options, though many workflows rely on defaults.

Examples:

```bash
git merge -X ours feature/auth
git merge -X theirs feature/auth
```

These are strategy options used to influence conflict resolution behavior for conflicting hunks.

Important distinction:
- `-X ours` / `-X theirs` are merge strategy options
- they are not the same thing as resolving a file manually with checkout `--ours` / `--theirs`

---

# 10) Conflict resolution

Conflict handling is one of the most important advanced Git topics.

## 10.1 What a conflict is

A conflict happens when Git cannot safely or unambiguously combine changes.

Typical cases:
- same lines changed differently
- file deleted on one side and modified on the other
- rename/modify interactions
- rebase conflict
- cherry-pick conflict

Git is good at automatic merging, but it stops when human judgment is required.

---

## 10.2 Example merge conflict flow

```bash
git switch main
git merge feature/auth
```

Possible output:

```text
Auto-merging app.py
CONFLICT (content): Merge conflict in app.py
Automatic merge failed; fix conflicts and then commit the result.
```

At that point:
- Git pauses the merge
- conflicted paths are marked
- you must resolve the files

---

## 10.3 Conflict markers

A conflicted file may look like:

```text
<<<<<<< HEAD
current branch version
=======
incoming branch version
>>>>>>> feature/auth
```

Meaning:
- upper section = current checked-out branch version
- lower section = incoming branch version

You must edit the file into its final correct state.

---

## 10.4 Steps to resolve a merge conflict

1. inspect conflict state
2. open conflicted files
3. remove markers
4. produce the final content
5. stage resolved files
6. complete the merge

Commands:

```bash
git status
git add app.py
git commit
```

---

## 10.5 Abort a merge

```bash
git merge --abort
```

This attempts to restore the pre-merge state.

---

## 10.6 Useful conflict inspection commands

Show unmerged paths:

```bash
git diff --name-only --diff-filter=U
```

Inspect conflict content:

```bash
git diff
```

See merge state:

```bash
git status
```

---

## 10.7 `ours` and `theirs`

Keep current side of a file:

```bash
git checkout --ours app.py
```

Keep incoming side:

```bash
git checkout --theirs app.py
```

Then:

```bash
git add app.py
```

Important:
The meaning of “ours” and “theirs” depends on operation context, especially in rebase, so these commands should be used carefully.

---

## 10.8 Conflict resolution principle

The technically correct approach is not “always choose ours” or “always choose theirs.”

The real task is:
- understand both changes
- understand their intent
- write the correct final state

Git identifies the ambiguity; the developer resolves the meaning.

---

# 11) Rebase

## 11.1 What rebase does

Rebase takes commits from one branch and replays them onto a new base.

```bash
git switch feature/auth
git rebase main
```

Meaning:
- find commits unique to `feature/auth`
- replay them on top of `main`

---

## 11.2 Why rebase is used

Rebase is used to:
- keep history linear
- update feature branches onto latest main
- clean local history
- prepare patches before sharing

---

## 11.3 Rebase rewrites history

This is crucial.

Rebase creates new commit identities for replayed commits.  
It does not merely “move” the old commits.

That is why rebasing shared public history can be disruptive.

---

## 11.4 Rebase conflict flow

```bash
git rebase main
```

If conflict happens:
- Git stops at the problematic commit
- you resolve files
- stage them
- continue

```bash
git status
git add app.py
git rebase --continue
```

Abort:

```bash
git rebase --abort
```

Skip a commit:

```bash
git rebase --skip
```

---

## 11.5 Interactive rebase

Interactive rebase rewrites local history.

```bash
git rebase -i HEAD~5
```

Common actions:
- `pick`
- `reword`
- `edit`
- `squash`
- `fixup`
- `drop`

This is one of Git’s most powerful local-history cleanup tools.

---

## 11.6 Merge vs rebase

Merge:
- preserves original branch structure
- may create merge commits
- does not rewrite existing commits

Rebase:
- linearizes history
- rewrites commit identities
- cleaner graph, but more history rewriting risk

Understanding this distinction is fundamental.

---

# 12) Cherry-picking

## 12.1 What cherry-pick is

`git cherry-pick` replays the change introduced by a specific commit onto the current branch.

```bash
git switch release/1.2
git cherry-pick a1b2c3d
```

This is useful when you want one specific change without merging the entire source branch.

---

## 12.2 Common use cases

- backport a bug fix to a release branch
- move a hotfix across branches
- copy one safe commit without unrelated work
- reconstruct selected work after branch cleanup

---

## 12.3 Cherry-picking multiple commits

Explicit commits:

```bash
git cherry-pick a1b2c3d d4e5f6g
```

Range:

```bash
git cherry-pick abc123^..def456
```

---

## 12.4 Cherry-pick conflicts

Cherry-pick can cause conflicts just like merge or rebase.

Continue after resolution:

```bash
git status
git add app.py
git cherry-pick --continue
```

Abort:

```bash
git cherry-pick --abort
```

---

## 12.5 Cherry-pick without auto-commit

```bash
git cherry-pick -n a1b2c3d
```

This applies the change without creating the commit immediately.

Useful when:
- you want to inspect the result
- modify it
- combine it into a different commit

---

## 12.6 Cherry-pick vs merge

Cherry-pick:
- copies the change effect of selected commit(s)
- produces new commit(s)
- does not preserve original branch structure

Merge:
- integrates histories
- preserves ancestry relationship

This distinction matters in release engineering and backport workflows.

---

# 13) Stashing

## 13.1 What stash is

`git stash` temporarily records uncommitted changes outside normal branch history.

It is useful when:
- you must switch branches quickly
- your work is incomplete
- you do not want a WIP commit yet

---

## 13.2 Basic stash

```bash
git stash
```

This generally stashes:
- tracked modifications
- staged changes

---

## 13.3 Include untracked files

```bash
git stash -u
```

or:

```bash
git stash --include-untracked
```

---

## 13.4 Named stash

```bash
git stash push -m "WIP auth validation"
```

Named stashes are much easier to manage.

---

## 13.5 List stashes

```bash
git stash list
```

---

## 13.6 Show stash content

```bash
git stash show stash@{0}
git stash show -p stash@{0}
```

---

## 13.7 Apply vs pop

Apply without removing:

```bash
git stash apply stash@{0}
```

Apply and remove:

```bash
git stash pop
```

---

## 13.8 Create a branch from a stash

```bash
git stash branch recover-auth-work stash@{0}
```

This is useful when the current branch changed a lot and applying the stash directly would be messy.

---

## 13.9 Stash conflicts

Applying a stash can conflict if the codebase changed since the stash was recorded.

So stash is not “safe magic.”  
It still re-applies changes into a potentially changed codebase.

---

# 14) Working with remote repositories

Remote workflows are a major Git skill area.

## 14.1 What a remote is

A remote is a named reference to another repository URL.

Common names:
- `origin`
- `upstream`

---

## 14.2 View remotes

```bash
git remote -v
```

---

## 14.3 Add remote

```bash
git remote add origin https://example.com/team/project.git
git remote add upstream https://example.com/original/project.git
```

---

## 14.4 Change remote URL

```bash
git remote set-url origin git@example.com:team/project.git
```

---

## 14.5 Fetch

```bash
git fetch origin
```

Fetch all:

```bash
git fetch --all
```

Fetch updates remote-tracking refs but does not change the checked-out branch automatically.

---

## 14.6 Pull

```bash
git pull origin main
```

`pull` is effectively:
- fetch
- integrate

Usually integration is merge, unless configured otherwise.

---

## 14.7 Pull with rebase

```bash
git pull --rebase origin main
```

This keeps local unpublished commits on top of fetched remote commits and avoids an unnecessary merge commit in many workflows.

---

## 14.8 Push

```bash
git push origin feature/auth
```

Set upstream:

```bash
git push -u origin feature/auth
```

---

## 14.9 Remote-tracking branches

Examples:
- `origin/main`
- `origin/feature/auth`

These are local references that represent the last fetched known state of remote branches.

You do not edit them directly like local branches.

---

## 14.10 Tracking branches

A local branch can track a remote branch.

This enables shorter commands for:
- push
- pull
- status reporting

View tracking info:

```bash
git branch -vv
```

---

## 14.11 Delete remote branch

```bash
git push origin --delete feature/auth
```

---

## 14.12 Prune stale refs

```bash
git fetch --prune
git remote prune origin
```

These clean up references to remote branches that no longer exist.

---

## 14.13 Non-fast-forward push rejection

Push can fail if remote history moved ahead.

Usual safe flow:

```bash
git fetch origin
git rebase origin/main
git push origin main
```

Or merge instead of rebase if your workflow prefers merge integration.

---

## 14.14 Force push and `--force-with-lease`

If you rewrote history, normal push may fail.

Safer force push:

```bash
git push --force-with-lease origin feature/auth
```

This is safer than plain `--force` because it checks that the remote still matches what you expect.

Use with care on shared branches.

---

## 14.15 Tags with remotes

Create annotated tag:

```bash
git tag -a v1.2.0 -m "Release v1.2.0"
```

Push one tag:

```bash
git push origin v1.2.0
```

Push all tags:

```bash
git push origin --tags
```

Fetch tags:

```bash
git fetch --tags
```

---

## 14.16 Fork and upstream workflow

A common open-source pattern:

- `origin` = your fork
- `upstream` = original project

Typical update flow:

```bash
git fetch upstream
git switch main
git rebase upstream/main
git push --force-with-lease origin main
```

This keeps your fork’s main branch synchronized with upstream.

---

# 15) Pull requests, reviews, and GitHub team workflows

## 15.1 What a pull request is

A pull request, often shortened to **PR**, is a collaboration workflow where one branch proposes changes to another branch through a hosted platform interface.

In GitHub workflows, a pull request is not a native Git object.  
It is a platform-level collaboration feature built on top of Git branches, commits, diffs, status checks, reviews, and merge actions.

A PR typically provides:
- commit list
- changed files view
- line comments
- review approval flow
- status checks
- merge controls
- discussion history

---

## 15.2 Typical PR workflow

A practical team flow often looks like this:

1. create a feature branch
2. commit and push the branch
3. open a pull request against `main` or another target branch
4. automated checks run
5. reviewers inspect code
6. requested changes are addressed
7. PR is approved
8. branch is merged using the selected merge method
9. branch is deleted after merge

This is one of the most common engineering collaboration patterns.

---

## 15.3 Good pull request practices

A strong pull request is usually:
- focused on one concern
- small enough to review
- linked to a task or issue
- described clearly
- tested
- rebased or updated appropriately
- free of unrelated formatting or noise changes

Good PR hygiene improves both code quality and review speed.

---

## 15.4 Code review goals

Code review is not only for finding syntax mistakes.

It is used to evaluate:
- correctness
- maintainability
- readability
- architecture fit
- test coverage
- edge cases
- security concerns
- naming and design clarity

A good review culture improves system quality over time.

---

## 15.5 Pull request review states

Common review outcomes include:
- comment only
- approve
- request changes

This makes review status visible and structured.

---

## 15.6 Common review comments and reviewer focus

Reviewers often check:
- logic correctness
- missing tests
- backwards compatibility
- error handling
- performance concerns
- API contract changes
- migration or rollout impact
- security implications
- documentation updates

The strongest code review comments are specific, actionable, and technical.

---

## 15.7 Draft pull requests

Draft PRs are useful when work is not yet ready for final review but early visibility is helpful.

They are commonly used when:
- feedback is needed on direction
- CI should run early
- design needs discussion
- implementation is incomplete

---

## 15.8 Branch protection and required checks

Repository management often uses branch protection rules on branches like `main`.

Typical protections include:
- required pull request reviews
- required status checks
- blocked direct pushes
- required up-to-date branches before merge
- restricted force pushes
- required signed commits in stricter environments

These controls reduce accidental or unsafe integration.

---

## 15.9 PR merge types in GitHub workflows

GitHub-style PR workflows commonly offer three merge methods:

- **Create a merge commit**
- **Squash and merge**
- **Rebase and merge**

These are platform actions built on top of Git history operations.

---

## 15.10 Create a merge commit

This merge type preserves the branch as a distinct line of history and creates a merge commit when the PR is merged.

Characteristics:
- preserves branch structure
- preserves all commits as part of branch history
- records an explicit integration point
- useful when branch context matters

This is often preferred when teams want visible branch integration history.

---

## 15.11 Squash and merge

This merge type combines the PR’s changes into a single commit on the target branch.

Characteristics:
- keeps target branch history cleaner
- removes noisy intermediate commits from the target branch
- loses per-commit structure on the target branch
- useful for messy feature branches or when one PR should equal one commit

This is common in teams that prefer a simple linear main branch.

---

## 15.12 Rebase and merge

This merge type reapplies PR commits onto the target branch without a merge commit.

Characteristics:
- keeps linear history
- preserves individual commits
- rewrites commit identities on merge
- does not preserve a merge commit boundary

This is useful when commit history is clean and the team wants a linear graph.

---

## 15.13 How to choose among PR merge types

Use **merge commit** when:
- branch context matters
- you want explicit integration points
- the branch had meaningful multi-commit history

Use **squash and merge** when:
- feature branch history is noisy
- the team prefers one commit per PR on main
- simplicity of target branch history is more important than preserving internal commit steps

Use **rebase and merge** when:
- the PR commits are already meaningful
- the team wants linear history
- preserving multiple logical commits still matters

This is a workflow choice, not a one-size-fits-all rule.

---

## 15.14 PR approvals and merge readiness

A strong merge policy often expects:
- passing CI
- review approval
- resolved conversations
- required documentation updated
- branch up to date with target
- release or migration notes when relevant

This creates a more reliable integration process.

---

## 15.15 Practical team workflow example

Example team workflow:

1. developer creates `feature/billing-validation`
2. developer commits locally
3. developer pushes branch to remote
4. PR is opened to `main`
5. GitHub Actions runs tests and lint checks
6. reviewers comment and request one change
7. developer updates branch and pushes again
8. CI passes
9. reviewers approve
10. team uses **squash and merge**
11. feature branch is deleted

This is a realistic modern team workflow.

---

# 16) GitHub Actions and automation

## 16.1 What GitHub Actions is

GitHub Actions is GitHub’s workflow automation system.

It is commonly used for:
- continuous integration
- testing
- linting
- build pipelines
- deployments
- release automation
- labeling or repository housekeeping
- scheduled automation

It is configured through YAML workflow files in the repository.

Common location:

```text
.github/workflows/
```

---

## 16.2 Core Actions concepts

Important concepts:
- **workflow**
- **event trigger**
- **job**
- **step**
- **runner**
- **action**
- **artifact**
- **secret**
- **environment**

A workflow contains one or more jobs.  
A job contains one or more steps.  
Steps run on a runner.

---

## 16.3 Example GitHub Actions workflow

```yaml
name: CI

on:
  pull_request:
  push:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.12"

      - name: Install dependencies
        run: pip install -r requirements.txt

      - name: Run tests
        run: pytest
```

This workflow:
- runs on pushes to `main`
- runs on pull requests
- checks out code
- installs Python
- installs dependencies
- runs tests

---

## 16.4 Common workflow triggers

Typical triggers:
- `push`
- `pull_request`
- `workflow_dispatch`
- `schedule`
- `release`

Examples:
- run CI on every PR
- run deployment on push to main
- run nightly scheduled jobs
- run packaging when a release is published

---

## 16.5 Common Actions use cases

Teams often use GitHub Actions to:
- run unit tests
- run lint or formatting checks
- build Docker images
- publish packages
- deploy applications
- verify Terraform plans
- generate changelogs
- auto-label pull requests
- enforce branch or PR rules

---

## 16.6 Secrets and environments

Sensitive data such as tokens, credentials, or keys should be managed using secure repository or environment secret mechanisms rather than hardcoding them into workflow files.

Examples of usage patterns:
- deployment credentials
- cloud provider keys
- package registry tokens

This is a major security principle in CI/CD design.

---

## 16.7 Workflow status checks in PRs

One of the most important GitHub Actions uses is PR gating.

Examples:
- tests must pass before merge
- lint checks must pass
- security scan must pass

These status checks are often enforced by branch protection.

---

## 16.8 Matrix builds

Actions can run a job across multiple environments using a matrix strategy.

Conceptual example:
- Python 3.10, 3.11, 3.12
- Ubuntu and Windows
- multiple Node versions

This is useful for compatibility testing.

---

## 16.9 Artifacts

Workflows can produce artifacts such as:
- test reports
- build outputs
- packaged binaries
- coverage reports

Artifacts are useful when results need to be retained after job execution.

---

## 16.10 Release automation

GitHub Actions is often used to automate release workflows such as:
- build release assets
- package binaries
- attach artifacts to releases
- publish container images
- publish language packages
- generate release notes

This connects Git history, tags, and release management into one workflow system.

---

# 17) Repository management and governance

## 17.1 Repository settings that matter

Good repository management often includes:

- default branch configuration
- branch protection rules
- CODEOWNERS usage
- required reviews
- status checks
- secret management
- environment protection
- release strategy
- issue and PR templates
- labels and milestones
- repository visibility and access control

These settings shape how teams collaborate safely.

---

## 17.2 CODEOWNERS

A `CODEOWNERS` file can define which people or teams should automatically be requested for review on certain paths.

This is useful for:
- service ownership
- platform boundaries
- domain-specific review routing
- enforcing expert review on sensitive areas

---

## 17.3 Labels, milestones, and project hygiene

Repository management is not only about code storage.

Teams often use:
- labels for categorization
- milestones for grouping delivery goals
- issue templates
- PR templates
- discussions or issue linking

This improves traceability and workflow consistency.

---

## 17.4 Access control and permissions

Repository administration should consider:
- who can push
- who can approve
- who can administer settings
- who can manage secrets and environments
- who can create releases
- whether force-push or branch deletion is allowed

This is especially important in regulated or production-sensitive environments.

---

## 17.5 Protected branches

Protected branches help prevent risky changes on important branches such as:
- `main`
- `release/*`

Typical rules:
- no direct pushes
- PR required
- checks required
- approval required
- history rules enforced
- force pushes disabled

---

# 18) Releases, versions, and tags

## 18.1 Tags vs releases

A **tag** is a Git reference pointing to a specific object, usually a commit.

A **release** in GitHub-style workflows is a platform-level distribution and documentation concept built around a tag.

So:
- tag = Git object/reference concept
- release = hosted platform publication concept

This distinction is important.

---

## 18.2 Annotated tags for releases

Annotated tags are typically better for releases because they include metadata.

Create one:

```bash
git tag -a v1.2.0 -m "Release v1.2.0"
```

Push it:

```bash
git push origin v1.2.0
```

---

## 18.3 Release workflow example

A practical release flow may look like this:

1. stabilize release branch or main branch
2. ensure CI passes
3. update version metadata
4. create release commit if needed
5. create annotated tag
6. push tag
7. GitHub Actions builds release assets
8. publish GitHub release with notes and artifacts

This is a common modern release pattern.

---

## 18.4 Semantic versioning concept

Many teams use semantic versioning formats like:

```text
MAJOR.MINOR.PATCH
```

Example:
- `v2.4.1`

This often connects with release notes, tags, changelogs, and automation pipelines.

---

## 18.5 Changelogs and release notes

Good release management often includes:
- summary of changes
- breaking changes
- migration notes
- security fixes
- known issues
- artifact links

Release notes are a key part of engineering communication, not just packaging.

---

# 19) Git submodules and subtree workflows

## 19.1 What a submodule is

A **Git submodule** is a way for one Git repository to reference another Git repository at a specific commit.

The parent repository does **not** absorb the child repository’s full history into itself.  
Instead, it stores:
- metadata in `.gitmodules`
- a tracked path entry
- a pointer to a specific commit of the child repository

This is one of the most important technical ideas:

> A submodule is not copied into the parent repository as ordinary files. The parent repository records a reference to a specific commit of another repository.

---

## 19.2 Why teams use submodules

Submodules are used when teams want:
- a separate repository lifecycle for shared components
- explicit version pinning of an external or internal dependency
- clear ownership boundaries between repositories
- inclusion of another repository without flattening histories together

Typical examples:
- shared SDK or library
- infrastructure module repository
- vendor or third-party component
- internal platform module reused by multiple services

---

## 19.3 `.gitmodules`

When a submodule is added, Git creates a `.gitmodules` file in the parent repository.

It contains configuration such as:
- submodule name
- path
- URL

This file is committed so collaborators know which submodules exist and where they come from.

---

## 19.4 Add a submodule

Example:

```bash
git submodule add https://example.com/team/lib.git libs/lib
```

This:
- clones the child repository into `libs/lib`
- adds `.gitmodules`
- stages the submodule path and metadata for commit

Commit the change:

```bash
git add .gitmodules libs/lib
git commit -m "Add lib as a submodule"
```

---

## 19.5 Clone a repository with submodules

Clone and immediately fetch submodules:

```bash
git clone --recurse-submodules https://example.com/team/app.git
```

If you clone without that option, the submodule directories may exist but not be fully initialized with their checked-out content.

---

## 19.6 Initialize and update submodules

If the repository was cloned without recursive submodule checkout:

```bash
git submodule update --init --recursive
```

This is one of the most important submodule commands.

Meaning:
- `--init` initializes submodule metadata locally
- `--recursive` also handles nested submodules if they exist

---

## 19.7 Pull submodule changes

A parent repository commit records the submodule at a particular child commit.

So when the parent repository updates its submodule pointer, collaborators typically do:

```bash
git pull
git submodule update --init --recursive
```

This updates the checked-out submodule content to the commit expected by the parent repository.

---

## 19.8 Move a submodule to a newer commit

Suppose you enter the submodule and pull or checkout a newer commit:

```bash
cd libs/lib
git pull origin main
cd ../..
```

Now the parent repository sees the submodule path as changed, because the recorded submodule pointer changed.

Commit that pointer update from the parent repository:

```bash
git add libs/lib
git commit -m "Update lib submodule to latest main"
```

This is another critical concept:
The parent repository commits the **submodule commit pointer**, not the submodule’s internal file diff directly.

---

## 19.9 Track a remote branch in submodule updates

Some workflows use:

```bash
git submodule update --remote
```

This updates submodules according to configured tracking behavior, often to the latest commit of a configured branch.

Teams should use this carefully because it moves dependency versions and should usually be reviewed and committed deliberately.

---

## 19.10 Common submodule problems

Submodules are powerful, but they are also a common source of confusion.

Typical problems:
- collaborators forget `--recurse-submodules`
- submodule directory appears empty or detached
- developer updates child repo but forgets to commit the new pointer in parent repo
- CI fails because submodules were not initialized
- nested submodules are missed
- permissions or access differ between parent and child repositories

This is why submodules are often considered operationally more complex than plain repository structures.

---

## 19.11 Detached HEAD in submodules

A submodule is often checked out at a specific commit rather than an active branch tip.

That means developers may see detached HEAD state inside the submodule.

This is normal in many submodule workflows because the parent repository pins an exact child commit.

But if you intend to make changes inside the submodule, you often need to:
- switch to a branch inside the submodule
- commit there
- push there
- then update the pointer in the parent repository

---

## 19.12 Submodule workflow example

A practical workflow:

1. clone parent repository with submodules
2. initialize or update submodules
3. work in parent repo
4. if child module changes are needed, enter submodule
5. create/checkout branch in submodule
6. commit and push submodule changes
7. return to parent repo
8. stage updated submodule pointer
9. commit parent repo change referencing new submodule commit

Example commands:

```bash
git clone --recurse-submodules https://example.com/team/app.git
cd app
cd libs/lib
git switch -c fix/parser-bug
# make changes
git add .
git commit -m "Fix parser bug"
git push -u origin fix/parser-bug
cd ../..
git add libs/lib
git commit -m "Update lib submodule pointer for parser fix"
```

---

## 19.13 What a subtree is

A **Git subtree** is another way to include one repository inside another, but unlike submodules, subtree workflows bring the child project’s content directly into the parent repository history.

Conceptually:
- submodule = reference another repository at a commit
- subtree = import another repository’s content into this repository

This creates different tradeoffs.

---

## 19.14 Submodule vs subtree

### Submodule
- keeps repositories separate
- parent stores a pointer to child commit
- clearer separation of ownership
- more operational overhead
- common source of confusion for collaborators

### Subtree
- child content lives directly in parent repository
- simpler clone and checkout experience
- no separate initialization step for collaborators
- history integration is heavier
- updates can be less transparent if team is unfamiliar with subtree commands

---

## 19.15 When to choose submodule vs subtree

Use **submodule** when:
- strict repository separation matters
- you want explicit dependency pinning
- the child project has its own lifecycle and access model

Use **subtree** when:
- you want a simpler contributor experience
- you prefer everything to clone normally in one repository
- the team wants fewer operational surprises

There is no universal winner.  
The choice depends on repository boundaries, team familiarity, and workflow priorities.

---

## 19.16 Important submodule commands

Add submodule:

```bash
git submodule add https://example.com/team/lib.git libs/lib
```

Clone with submodules:

```bash
git clone --recurse-submodules https://example.com/team/app.git
```

Initialize/update:

```bash
git submodule update --init --recursive
```

Update from configured remote tracking:

```bash
git submodule update --remote
```

Show submodule status:

```bash
git submodule status
```

---

## 19.17 Important subtree commands

Add subtree:

```bash
git subtree add --prefix=libs/lib https://example.com/team/lib.git main --squash
```

Pull subtree updates:

```bash
git subtree pull --prefix=libs/lib https://example.com/team/lib.git main --squash
```

Push subtree changes back:

```bash
git subtree push --prefix=libs/lib https://example.com/team/lib.git main
```

These commands are useful, but subtree workflows should be used deliberately because they alter how repository history is integrated.

---

# 20) Reset, restore, revert

These are often confused and must be distinguished clearly.

## 19.1 `git restore`

Restore working tree file from index:

```bash
git restore app.py
```

Unstage file:

```bash
git restore --staged app.py
```

---

## 19.2 `git reset`

Soft reset:

```bash
git reset --soft HEAD~1
```

Meaning:
- move branch back
- keep changes staged

Mixed reset:

```bash
git reset HEAD~1
```

Meaning:
- move branch back
- keep changes in working tree
- unstage them

Hard reset:

```bash
git reset --hard HEAD~1
```

Meaning:
- move branch back
- reset index
- reset working tree

Hard reset discards uncommitted tracked changes.

---

## 19.3 `git revert`

```bash
git revert a1b2c3d
```

This creates a new commit that inverses an earlier commit’s effect.

It preserves public history and is therefore much safer than reset on shared branches.

---

## 19.4 Reset vs revert

Reset:
- moves refs
- can rewrite local branch history
- may discard uncommitted changes in hard mode

Revert:
- preserves history
- adds a new reversing commit
- safer for shared branches

This distinction is extremely important.

---

# 21) Reflog and recovery

## 20.1 What reflog is

`git reflog` records movements of refs like HEAD and branch tips.

```bash
git reflog
```

This is invaluable when recovering from:
- accidental hard reset
- bad rebase
- detached HEAD confusion
- overwritten branch tip
- lost commit references

---

## 20.2 Recovery example

Suppose you reset too far back.

Inspect reflog:

```bash
git reflog
```

Recover previous position:

```bash
git reset --hard HEAD@{3}
```

Or create a branch there:

```bash
git branch recover-work HEAD@{3}
```

Reflog is one of Git’s most important recovery mechanisms.

---

# 22) Packfiles, storage efficiency, and garbage collection

## 21.1 Loose objects vs packfiles

Git can store objects as:
- loose objects
- packed objects

As repositories grow, Git often compresses many objects into **packfiles** for storage and transport efficiency.

This matters because Git’s history model is object-heavy, and efficient packing makes large repositories practical.

---

## 21.2 Why packfiles matter

Packfiles help:
- reduce disk usage
- improve transfer efficiency
- optimize object storage
- reduce redundancy

This is one of the lower-level internals that explains why Git scales better than a naive snapshot store.

---

## 21.3 Garbage collection

Git can clean and optimize repository storage.

```bash
git gc
```

Garbage collection can:
- pack loose objects
- prune unreachable objects after policy windows
- optimize refs and storage layout

This should usually be understood conceptually rather than used casually in active workflows.

---

# 23) Plumbing vs porcelain

## 22.1 What this means

Git commands are often divided into:
- **porcelain**
- **plumbing**

Porcelain commands are user-facing high-level commands such as:
- `git status`
- `git add`
- `git commit`

Plumbing commands expose lower-level internals.

Examples:
- `git cat-file`
- `git rev-parse`
- `git ls-tree`
- `git hash-object`

You do not need plumbing commands for routine work, but understanding them makes Git much less mysterious.

---

## 22.2 Useful low-level examples

Show object content:

```bash
git cat-file -p HEAD
```

Resolve a revision to its full object ID:

```bash
git rev-parse HEAD
```

List tree entries:

```bash
git ls-tree HEAD
```

These are excellent for understanding what Git is actually storing.

---

# 24) Common advanced workflows

## 23.1 Resolve a merge conflict

```bash
git switch main
git merge feature/auth
git status
# edit conflicted files
git add app.py
git commit
```

Abort instead:

```bash
git merge --abort
```

---

## 23.2 Rebase a feature branch onto updated main

```bash
git fetch origin
git switch feature/auth
git rebase origin/main
```

If conflict occurs:

```bash
git status
# resolve files
git add app.py
git rebase --continue
```

---

## 23.3 Cherry-pick a hotfix into a release branch

```bash
git switch release/1.2
git cherry-pick a1b2c3d
```

If conflict occurs:

```bash
git status
# resolve files
git add app.py
git cherry-pick --continue
```

---

## 23.4 Stash work, sync branch, restore work

```bash
git stash push -m "WIP login validation"
git switch main
git pull --rebase origin main
git switch feature/auth
git stash pop
```

---

## 23.5 Sync fork with upstream

```bash
git remote add upstream https://example.com/original/project.git
git fetch upstream
git switch main
git rebase upstream/main
git push --force-with-lease origin main
```

---

## 23.6 Recover from mistaken hard reset

```bash
git reflog
git branch recovery HEAD@{1}
git switch recovery
```

This is safer initially than immediately resetting again.

---

## 23.7 PR-based team workflow with CI

```bash
git switch -c feature/api-validation
# make changes
git add .
git commit -m "Add API validation for billing requests"
git push -u origin feature/api-validation
```

Then:
- open PR
- wait for Actions checks
- address review comments
- merge using selected PR merge strategy
- delete branch

---

## 23.8 Create and publish a release tag

```bash
git switch main
git pull --rebase origin main
git tag -a v2.1.0 -m "Release v2.1.0"
git push origin v2.1.0
```

This can trigger release automation workflows.

---

# 25) Common interview-ready technical explanations

## What is the staging area in Git?

A strong answer:

> The staging area, or index, is the intermediate structure between the working tree and the next commit. It lets Git build the next snapshot precisely, supports partial staging, and can even hold multiple merge stages during conflicts.

---

## What is a merge conflict?

A strong answer:

> A merge conflict occurs when Git cannot automatically reconcile competing changes, such as different edits to the same lines or incompatible file operations. Git marks the conflict in the files and index, and the developer must produce the correct final content before the operation can complete.

---

## What is cherry-pick?

A strong answer:

> Cherry-pick replays the change introduced by a selected commit onto the current branch, producing a new commit with a different identity. It is useful for backports and targeted fixes when you do not want to merge an entire branch.

---

## What is stash?

A strong answer:

> Stash temporarily records uncommitted work outside normal branch history so the working tree can be cleaned quickly. It is useful for pausing incomplete work, though applying a stash can still cause conflicts if the target code changed.

---

## What is the difference between fetch and pull?

A strong answer:

> Fetch updates remote-tracking references without changing the current branch, while pull combines fetch with an integration step such as merge or rebase. Fetch is safer when you want to inspect remote changes before integrating them.

---

## What is the difference between reset and revert?

A strong answer:

> Reset moves branch references and can rewrite or discard local state depending on mode, while revert creates a new commit that reverses a previous commit’s effect. Reset is mainly for local history manipulation; revert is safer for shared history.

---

## What is the difference between merge commit, squash merge, and rebase merge in PR workflows?

A strong answer:

> A merge commit preserves branch structure and records an explicit integration point. A squash merge combines the PR changes into a single commit on the target branch. A rebase merge keeps history linear by replaying the PR commits onto the target branch without creating a merge commit. The right choice depends on whether the team values branch context, simple history, or preserved logical commits.

---

## What is GitHub Actions?

A strong answer:

> GitHub Actions is a workflow automation system integrated with GitHub that runs CI/CD and repository automation tasks based on events such as pushes, pull requests, releases, or schedules. It is commonly used for testing, linting, packaging, deployment, and release automation.

---

# 26) Open-source references

These are useful sources for deeper technical reading:

1. Git documentation main page  
   https://git-scm.com/doc

2. Git book  
   https://git-scm.com/book/en/v2

3. git status  
   https://git-scm.com/docs/git-status

4. git add  
   https://git-scm.com/docs/git-add

5. git commit  
   https://git-scm.com/docs/git-commit

6. git branch  
   https://git-scm.com/docs/git-branch

7. git merge  
   https://git-scm.com/docs/git-merge

8. git rebase  
   https://git-scm.com/docs/git-rebase

9. git cherry-pick  
   https://git-scm.com/docs/git-cherry-pick

10. git stash  
   https://git-scm.com/docs/git-stash

11. git remote  
   https://git-scm.com/docs/git-remote

12. git fetch  
   https://git-scm.com/docs/git-fetch

13. git pull  
   https://git-scm.com/docs/git-pull

14. git push  
   https://git-scm.com/docs/git-push

15. git reset  
   https://git-scm.com/docs/git-reset

16. git restore  
   https://git-scm.com/docs/git-restore

17. git revert  
   https://git-scm.com/docs/git-revert

18. git reflog  
   https://git-scm.com/docs/git-reflog

19. git cat-file  
   https://git-scm.com/docs/git-cat-file

20. git rev-parse  
   https://git-scm.com/docs/git-rev-parse

21. git ls-tree  
   https://git-scm.com/docs/git-ls-tree

22. git gc  
   https://git-scm.com/docs/git-gc

23. GitHub Docs — About pull requests  
   https://docs.github.com/en/pull-requests

24. GitHub Docs — About reviews in pull requests  
   https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/about-pull-request-reviews

25. GitHub Docs — About merge methods on GitHub  
   https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/configuring-pull-request-merges/about-merge-methods-on-github

26. GitHub Docs — GitHub Actions  
   https://docs.github.com/en/actions

27. GitHub Docs — Protected branches  
   https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches

28. GitHub Docs — About releases  
   https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases

29. GitHub Docs — About CODEOWNERS  
   https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners

30. git submodule  
   https://git-scm.com/docs/git-submodule

31. gitmodules  
   https://git-scm.com/docs/gitmodules

32. Git subtree how-to  
   https://www.atlassian.com/git/tutorials/git-subtree

---

# 27) Final revision summary

The most important way to understand Git and GitHub team workflows is this:

- Git is a **distributed, content-addressed version control system**
- it stores history as objects and references
- the **working tree** is where you edit
- the **index** is the structured next-commit snapshot area
- commits point to trees and parents
- branches are lightweight movable references
- merges use ancestry and often three-way merge logic
- rebases replay commits on a new base and rewrite commit identities
- cherry-pick applies selected commits
- stash stores temporary uncommitted work
- remotes and remote-tracking refs support distributed collaboration
- pull requests provide platform-level collaboration and review workflows
- GitHub Actions automates CI/CD and repository tasks
- releases combine tags, notes, and artifacts into delivery workflows
- submodules pin one repository to a specific commit of another repository
- subtree workflows import another repository’s content more directly
- branch protection and repository settings shape safe team practices
- reflog helps recover lost reference positions
- packfiles and garbage collection support efficient storage
- plumbing commands reveal the object model beneath the porcelain commands

If you understand:
- working tree
- index
- refs and HEAD
- blobs, trees, commits
- merges and conflicts
- rebase
- cherry-pick
- stash
- remotes
- pull requests and reviews
- PR merge types
- GitHub Actions
- releases and tags
- submodules and subtree tradeoffs
- repository governance
- reset / restore / revert
- reflog
- packfiles
- porcelain vs plumbing

then you understand the technical core of Git and GitHub workflows at a strong advanced level.


## Table of Contents
- [1. Git Basics](#1-git-basics)
- [2. Git vs GitHub](#2-git-vs-github)
- [3. Git Internals](#3-git-internals)
- [4. Git Authentication (SSH vs HTTPS)](#4-git-authentication-ssh-vs-https)
- [5. Branching and Merging](#5-branching-and-merging)
- [6. Rebasing](#6-rebasing)
- [7. Commits and History Management](#7-commits-and-history-management)
- [8. Stashing](#8-stashing)
- [9. Resolving Merge Conflicts](#9-resolving-merge-conflicts)
- [10. Collaboration and GitHub Workflows](#10-collaboration-and-github-workflows)
- [11. Advanced Git Techniques](#11-advanced-git-techniques)
- [12. Troubleshooting and Optimization](#12-troubleshooting-and-optimization)
- [13. Git in CI/CD and MERN Projects](#13-git-in-cicd-and-mern-projects)
- [14. Interview Tips and Common Questions](#14-interview-tips-and-common-questions)

---

## 1. Git Basics

- **Overview**: Git is a distributed version control system for tracking source code changes, enabling collaboration and version history management.
- **Key Areas**:
  - **Working Directory**: Current project files where code is edited.
  - **Staging Area (Index)**: Temporary area for changes prepared for commit using `git add`.
  - **Repository (.git folder)**: Stores committed snapshots after `git commit`.
- **Core Commands**:
  ```bash
  git init                # Initialize a new Git repository
  git clone <url>         # Clone a remote repository
  git status              # Check status of working directory and staging area
  git add <file>          # Stage changes for commit
  git commit -m "message" # Commit staged changes with a message

.gitignore:
Specifies files/folders to ignore (e.g., node_modules/, .env, dist/).

Prevents tracking unnecessary or sensitive data.

Example:
gitignore

# .gitignore
node_modules/
.env
dist/

Purpose:
Tracks code changes over time.

Enables reverting to previous versions.

Supports collaborative, non-linear development.

2. Git vs GitHub
Git (Local):
Definition: Distributed version control system running locally.

Features:
Tracks code changes.

Supports branching, merging, and reverting.

Works offline with a full repository copy per developer.

Advantages:
Fast local operations.

No central server dependency (unlike SVN).

Non-linear workflows via branches.

Example:
bash

git init
git add .
git commit -m "Initial commit"

GitHub (Remote):
** Definition**: Cloud-based platform for hosting Git repositories.

Features:
Remote code backup.

Collaboration via pull requests (PRs), issues, and code reviews.

CI/CD with GitHub Actions.

Web-based repository management.

Advantages:
Team collaboration tools.

Centralized access for remote teams.

Integrations with CI/CD and project management.

Example:
bash

git push origin main
# Create PR on GitHub to merge feature branch

Comparison:
Feature

Git (Local) 

GitHub (Remote) 

Version Control

Yes

Yes

Internet Required

No

Yes

Collaboration Tools

No

Yes (PRs, issues, reviews)

Backup

Local copies

Cloud-hosted

Branching/Merging

Built-in

Supported via UI

Undo/Revert/Reset

Full support

View/manage via UI

Interview Tip: Emphasize Git as the core tool and GitHub as a service enhancing Git with collaboration and hosting.

3. Git Internals
How It Works:
Stores data as snapshots (not diffs) using SHA-1 hashes to identify commits.

Each commit links to a tree of files and parent commits.

Three Trees:
Working Directory: Current editable files.

Staging Area: Changes staged with git add.

Repository: Committed snapshots in .git folder.

Key Objects:
Blob: Stores file content.

Tree: Represents directories, linking to blobs or other trees.

Commit: Snapshot with metadata (author, message, parents).

Visual Example:

Working Dir → git add → Staging Area → git commit → Repository
file.txt              file.txt                commit (SHA-1)

Commands to Inspect:
bash

git log                 # View commit history
git show <commit>       # Inspect a specific commit
git ls-tree <commit>    # View tree structure

Interview Tip: Highlight Git’s efficiency with snapshots and hashes for integrity.

4. Git Authentication (SSH vs HTTPS)
Overview: Git requires authentication for remote repository interactions (e.g., GitHub). Two methods: SSH and HTTPS.

SSH Authentication:
How It Works: Uses public/private key pair for secure, passwordless access.

Setup:
Generate SSH key:
bash

ssh-keygen -t ed25519 -C "your.email@example.com"
# Or RSA if ed25519 unavailable: ssh-keygen -t rsa -b 4096

Add public key to GitHub:
Copy ~/.ssh/id_ed25519.pub (or id_rsa.pub).

GitHub → Settings → SSH and GPG Keys → Add SSH Key.

Add private key to SSH agent:
bash

eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

Test connection:
bash

ssh -T git@github.com

Configure remote URL:
bash

git remote set-url origin git@github.com:username/repo.git

Pros:
Secure and passwordless.

Seamless for frequent pushes/pulls.

Cons:
Complex initial setup.

Key management across devices.

HTTPS Authentication:
How It Works: Uses username and personal access token (PAT) or password (deprecated).

Setup:
Create PAT on GitHub:
GitHub → Settings → Developer Settings → Personal Access Tokens → Tokens (classic) → Generate new token.

Select scopes (e.g., repo, workflow).

Use PAT when prompted:
bash

git clone https://github.com/username/repo.git
# Enter username and PAT

Cache credentials (optional):
bash

git config --global credential.helper cache
# Or: git config --global credential.helper manager

Pros:
Simpler setup for beginners.

Works in restricted firewall environments.

Cons:
Requires PAT management.

Prompts for credentials unless cached.

Comparison:
Method

Pros

Cons

Setup Complexity

SSH

Secure, passwordless, seamless

Complex initial setup

High

HTTPS

Simple, firewall-friendly

Requires PAT, credential prompts

Low

Best Practices:
Use SSH for personal/long-term projects.

Use HTTPS for quick clones or restricted networks.

Store PATs securely (e.g., password manager).

Rotate SSH keys and PATs regularly.

Troubleshooting:
SSH:
bash

ssh -vT git@github.com  # Debug connection

HTTPS:
Verify PAT scopes.

Clear cached credentials:
bash

git credential-cache exit

Interview Tip: Discuss both methods, their use cases, and security practices like key rotation.

5. Branching and Merging
Overview: Branching enables parallel development; merging integrates changes from one branch to another.

Branching Commands:
bash

git branch              # List branches
git branch <name>       # Create branch
git checkout <name>     # Switch to branch
git checkout -b <name>  # Create and switch
git branch -d <name>    # Delete branch

Merging:
Definition: Combines histories of two branches, creating a merge commit if histories diverge.

Command:
bash

git checkout feature
git merge main

Visual Example:

Before:
A---B---C (main)
     \
      D---E (feature)
After:
A---B---C---------M (feature)
     \           /
      D---E-----/

M is the merge commit.

Types:
Fast-Forward: Moves pointer forward if no divergence (no merge commit).
bash

git merge --ff-only

Non-Fast-Forward: Forces a merge commit.
bash

git merge --no-ff

Pros:
Preserves full branch history.

Safe for collaborative projects.

Cons:
Merge commits can clutter history.

Use Cases:
Integrating stable branches (e.g., main to feature).

Collaborative projects needing history context.

Merge vs Rebase: See Rebasing (#6-rebasing).

Interview Tip: Explain fast-forward vs non-fast-forward merges and their impact on history.

6. Rebasing
Overview: Rewrites a branch’s history to appear as if based on the latest commits of another branch.

Commands:
bash

git checkout feature
git rebase main
# Resolve conflicts if any
git rebase --continue
git push --force-with-lease  # Safe push after rewrite

Visual Example:

Before:
A---B---C (main)
     \
      D---E (feature)
After:
A---B---C---D'---E' (feature)

D' and E' are rewritten commits.

Pros:
Cleaner, linear history.

Ideal for short-lived branches or PRs.

Makes history appear sequential.

Cons:
Rewrites history; avoid on shared branches without coordination.

Conflicts resolved commit-by-commit.

Use Cases:
Cleaning local branches before merging.

Updating feature branches with main.

Interactive Rebase:
Edits, squashes, or reorders commits.

bash

git rebase -i HEAD~n  # Edit last n commits

Options: pick, squash, edit, drop.

Merge vs Rebase:
Aspect

Merge

Rebase

History

Preserves branch history

Rewrites for linear history

Merge Commits

Creates if diverged

None

Collaboration

Safe for shared branches

Risky unless coordinated

Use Case

Collaborative, long-lived branches

Local cleanup, short-lived branches

Best Practices:
Use --force-with-lease to avoid overwriting others’ work.

Coordinate with team before rebasing shared branches.

Interview Tip: Highlight risks of rebasing public branches and mitigation strategies.

7. Commits and History Management
Committing:
git commit -m "message": Commit with message.

git commit --amend: Amend last commit.

Viewing History:
git log: View commit history.

git log --oneline: Compact view.

git log --graph: Visual branch history.

git log path/to/file: File-specific history.

git log --follow file: History including renames (press q to exit).

Undoing Commits:
git reset:
Moves HEAD; alters staging/working directory.

Types:
bash

git reset --soft HEAD~1   # Undo commit, keep changes staged
git reset --mixed HEAD~1  # Undo commit, unstage changes
git reset --hard HEAD~1   # Undo commit, discard changes

Comparison:
Type

Reverts Commit

Keeps Staged

Keeps Working Dir

--soft

Yes

Yes

Yes

--mixed

Yes

No

Yes

--hard

Yes

No

No

Use Case: Local cleanup (avoid on shared branches).

git revert:
Creates a new commit to undo a specific commit.

bash

git revert <commit-hash>

Safe for shared branches.

Visual:

A -- B -- C (main)
git revert B
A -- B -- C -- D (main)

D undoes B’s changes.

git restore:
Restores files in working directory or staging area.

bash

git restore <file>        # Discard working directory changes
git restore --staged <file>  # Unstage file

Cherry-Picking:
Applies a specific commit to current branch.

bash

git cherry-pick <commit-hash>

Use Case: Porting bugfixes between branches.

Squashing Commits:
Combines multiple commits into one.

bash

git rebase -i HEAD~n
# Set "squash" for commits to combine

Interview Tip: Compare reset vs revert and their impact on shared repositories.

8. Stashing
Overview: Temporarily saves uncommitted changes (working directory and staging area) to a stack for context switching.

Commands:
git stash: Save changes to stash.

git stash list: View stash stack.

git stash apply: Apply latest stash.

git stash pop: Apply and remove latest stash.

git stash drop <stash>: Remove specific stash.

git stash clear: Remove all stashes.

Example Workflow:
bash

# Uncommitted changes on feature branch
git stash
git checkout main
git pull
git checkout feature
git stash pop

Pros:
Safely stores changes without committing.

Enables quick context switching.

Cons:
Risk of forgetting stashed changes.

Conflicts may occur when applying.

Interview Tip: Mention stashing for handling urgent tasks without committing incomplete work.

9. Resolving Merge Conflicts
Overview: Conflicts occur when Git cannot automatically reconcile changes from two branches (e.g., same line edited differently).

Causes:
Divergent changes in the same file.

File deleted in one branch, modified in another.

Renames or moves causing ambiguity.

Step-by-Step Resolution Guide:
Trigger Conflict:
bash

git checkout feature
git merge main

Output example:

Auto-merging app.js
CONFLICT (content): Merge conflict in app.js
Automatic merge failed; fix conflicts and then commit the result.

Identify Conflicted Files:
bash

git status

Output: Lists files with conflicts (e.g., both modified: app.js).

Open Conflicted Files:
Git marks conflicts with delimiters:
```
<<<<<<< HEAD
Your changes in current branch
Changes from branch being merged
<commit-hash>
```

Example (app.js):
```
<<<<<<< HEAD
function greet() {
  return "Hello from feature!";
}
function greet() {
  return "Hello from main!";
}
abc1234
```

Resolve Conflicts:
Manually edit to keep desired changes.

Remove markers (<<<<<<<, =======, >>>>>>>).

Example resolution:

function greet() {
  return "Hello from both!";
}

Tools: Use VSCode, GitLens, or git mergetool for visual resolution.

Stage Resolved Files:
bash

git add app.js

Complete Merge:
For merging:
bash

git commit  # Auto-generates merge commit message

For rebasing:
bash

git rebase --continue

Verify and Push:
bash

git log --graph  # Check history
git push origin feature

Rebase Conflicts:
Resolve commit-by-commit.

After resolving:
bash

git rebase --continue

Cancel rebase:
bash

git rebase --abort

Best Practices:
Pull/fetch regularly to minimize conflicts.

Keep changes small and focused.

Communicate with team during merges.

Use visual tools for efficiency.

Test after resolution to ensure functionality.

Troubleshooting:
Abort merge:
bash

git merge --abort

Use git diff to compare changes.

Check git log for commit context.

Interview Tip: Walk through the process with a code example, emphasizing tools and testing.

10. Collaboration and GitHub Workflows
Pull Requests (PRs):
Definition: GitHub feature for proposing, reviewing, and merging code.

Process:
Push branch:
bash

git push origin feature

Create PR on GitHub.

Request reviews, address feedback.

Merge after approval (squash, merge, or rebase).

Features:
Inline code review.

CI/CD integration.

Branch protection rules.

Typical Workflow:
Branching:
main: Production-ready.

dev: Integration/testing.

feature/*, bugfix/*: Specific tasks.

Steps:
git checkout -b feature/login

Commit and push.

Open PR to dev.

Merge dev to main after testing.

Git Workflows:
GitFlow:
Branches: main, develop, feature, release, hotfix.

Use Case: Large projects with release cycles.

Trunk-Based:
Work on main with feature flags.

Use Case: Fast-paced, continuous deployment.

Managing Multiple Branches:
Naming: feature/login, bugfix/issue-123.

Keep branches short-lived.

Rebase or merge regularly.

Use branch protection rules.

Keeping Forks in Sync:
bash

git remote add upstream https://github.com/original/repo.git
git fetch upstream
git merge upstream/main
git push origin main

Code Quality:
Pre-commit hooks (linting, formatting).

CI pipelines for tests.

PR reviews with at least one approver.

Use git blame to trace changes.

Interview Tip: Describe a real-world workflow, mentioning branch naming and PR processes.

11. Advanced Git Techniques
Git Bisect:
Finds commit introducing a bug via binary search.

bash

git bisect start
git bisect bad           # Current commit is bad
git bisect good <hash>   # Known good commit
# Test each commit
git bisect good/bad
git bisect reset         # Exit

Git Hooks:
Scripts triggered by Git events.

Examples:
pre-commit: Run tests/lint.

post-merge: Update dependencies.

Location: .git/hooks/.

Git Tags:
Mark specific commits (e.g., releases).

bash

git tag v1.0.0
git push origin v1.0.0

Multiple Remotes:
bash

git remote add upstream https://github.com/other/repo.git
git fetch upstream

Cleaning History:
Remove secrets/large files:
bash

git filter-repo --path <file> --invert-paths

Use .gitattributes for consistent diffs.

Interview Tip: Highlight git bisect or hooks to show advanced knowledge.

12. Troubleshooting and Optimization
Recovering from git reset --hard:
bash

git reflog
git checkout <commit-hash>
git branch recovery

Large Repositories:
Use Git LFS for binaries:
bash

git lfs track "*.psd"

Clean up:
bash

git gc --aggressive
git remote prune origin

Debugging:
git diff: Compare changes.

git show <commit>: Inspect commit.

git log --follow file: Track renamed files.

Interview Tip: Share a troubleshooting story (e.g., recovering a commit).

13. Git in CI/CD and MERN Projects
CI/CD Integration:
Use GitHub Actions or GitLab CI.

Automate:
Linting (eslint).

Testing (jest).

Deployment (e.g., Vercel, AWS).

Example GitHub Action:
yaml

name: CI
on: [push]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: npm install
      - run: npm test

Monorepo vs Polyrepo:
Style

Pros

Cons

Monorepo

Unified changes, easier refactor

Complex Git history

Polyrepo

Simpler per service

Cross-service changes harder

Changelogs and Versioning:
Tools: semantic-release, standard-version.

Use conventional commits: feat:, fix:, chore:.

Interview Tip: Discuss Git’s role in CI/CD for MERN projects.

14. Interview Tips and Common Questions
Tips:
Use real-world examples to demonstrate experience.

Explain trade-offs (e.g., merge vs rebase).

Show familiarity with tools (VSCode Git, GitKraken).

Highlight collaboration and best practices.

Common Questions:
Q1: Difference between git fetch and git pull?
A: fetch downloads changes without merging; pull fetches and merges.

Q2: How to revert a pushed commit?
A: Use git revert <hash> for shared branches.

Q3: Describe your Git workflow.
A: Explain branching, PRs, and CI/CD integration.

Q4: How to handle large files?
A: Use Git LFS and .gitignore.

References:
Git Book

GitHub Docs

Git Cheatsheet


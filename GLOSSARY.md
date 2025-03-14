# Git & GitHub — Glossary 📖

> Definitions of key terms used throughout this coursework.

---

| Term | Definition |
|------|-----------|
| **Annotated Tag** | A Git tag stored as a full object with author, date, message, and optional GPG signature. |
| **Bare Repository** | A repository without a working directory, used for sharing (servers). Created with `git init --bare`. |
| **Bisect** | Git's binary search tool (`git bisect`) for finding the commit that introduced a bug. |
| **Blame** | Command (`git blame`) showing who last modified each line of a file and when. |
| **Blob** | A Git object storing file content (no filename or metadata). Short for Binary Large Object. |
| **Branch** | A lightweight, movable pointer to a commit. Used to develop features in isolation. |
| **Branch Protection** | GitHub rules that prevent direct pushes, require reviews, and enforce CI checks on specific branches. |
| **Cherry-Pick** | Applying the changes from a specific commit onto the current branch (`git cherry-pick`). |
| **CI/CD** | Continuous Integration / Continuous Delivery (or Deployment). Automated build, test, and deploy pipeline. |
| **Clone** | Creating a local copy of a remote repository (`git clone`). |
| **CODEOWNERS** | A GitHub file that automatically assigns reviewers when specific files are changed in a PR. |
| **Commit** | A snapshot of the repository at a point in time, with metadata (author, date, message, parent). |
| **Commit Graph (DAG)** | Directed Acyclic Graph formed by commits pointing to their parents. |
| **Conflict** | Occurs when Git cannot auto-merge changes because the same lines were modified differently. |
| **Conventional Commits** | A commit message convention: `<type>(<scope>): <description>` enabling automated versioning. |
| **CLA** | Contributor License Agreement — legal document signed by contributors to license their contributions. |
| **DCO** | Developer Certificate of Origin — lightweight alternative to CLA, added via `Signed-off-by` trailer. |
| **Dependabot** | GitHub's automated tool for keeping dependencies up to date and flagging security vulnerabilities. |
| **Detached HEAD** | State where HEAD points to a commit directly rather than a branch reference. |
| **Diff** | The difference between two states (files, commits, branches). Output shows added/removed lines. |
| **Fast-Forward Merge** | A merge where the target branch pointer simply moves forward (no merge commit needed). |
| **Fetch** | Downloading objects and refs from a remote without merging (`git fetch`). |
| **Feature Flag** | A conditional in code that enables/disables features without deploying new code. Used in trunk-based development. |
| **Fork** | A GitHub copy of another user's repository under your account. Enables contribution without direct access. |
| **Git Flow** | A branching model with `main`, `develop`, `feature/*`, `release/*`, and `hotfix/*` branches. |
| **GitHub Actions** | GitHub's built-in CI/CD platform. Workflows defined in YAML run on events (push, PR, schedule). |
| **GitHub Flow** | A simplified branching model: `main` + feature branches → PRs → deploy on merge. |
| **GitOps** | Using Git as the single source of truth for both application code and infrastructure configuration. |
| **Gitignore** | A `.gitignore` file specifying patterns of files Git should not track. |
| **GPG Signing** | Cryptographically signing commits or tags with a GPG key to verify authorship. |
| **HEAD** | A pointer to the current commit (usually via the current branch). Determines what you see in the working directory. |
| **Hook** | A script Git runs automatically before or after events (commit, push, merge). Located in `.git/hooks/`. |
| **Husky** | A Node.js tool for managing Git hooks in a project, making them easily shareable. |
| **Index** | Another name for the staging area. The `.git/index` file tracks what will go in the next commit. |
| **Interactive Rebase** | `git rebase -i` — allows you to edit, squash, reorder, and drop commits. |
| **LFS** | Large File Storage — Git extension for tracking large binary files with pointers instead of full content. |
| **Lightweight Tag** | A simple pointer to a commit (just a name → SHA). No metadata stored. |
| **Lint-staged** | Tool that runs linters only on staged files, combined with Husky for pre-commit checks. |
| **Merge** | Combining changes from one branch into another. Creates a merge commit (unless fast-forward). |
| **Merge Commit** | A commit with two or more parents, representing the combination of branches. |
| **Monorepo** | A single repository containing multiple projects or packages. Used by Google, Meta, and Microsoft. |
| **Object** | The four types of Git internal storage: blob, tree, commit, and tag. Each addressed by SHA-1 hash. |
| **Origin** | The default name for the remote repository you cloned from. |
| **Packfile** | Compressed storage format Git uses to efficiently store many objects together. |
| **Patch** | A text file describing changes to apply. Can be created with `git format-patch` and applied with `git am`. |
| **Plumbing** | Low-level Git commands that perform single operations (e.g., `cat-file`, `hash-object`). |
| **Polyrepo** | Architecture where each project or service has its own separate Git repository. |
| **Porcelain** | High-level, user-friendly Git commands (e.g., `add`, `commit`, `push`). Built on plumbing. |
| **Pull** | Fetch + merge from a remote branch in one command (`git pull`). |
| **Pull Request (PR)** | A GitHub feature requesting that changes from one branch be merged into another, with code review. |
| **Push** | Uploading local commits to a remote repository (`git push`). |
| **Push Protection** | GitHub feature that blocks pushes containing detected secrets before they reach the repo. |
| **Rebase** | Re-applying commits on top of a different base commit. Creates a linear history. |
| **Ref** | A reference (pointer) to a commit — branches, tags, and HEAD are all refs. Stored in `.git/refs/`. |
| **Reflog** | Reference log — records every time HEAD or a branch pointer moves. Used for disaster recovery. |
| **Remote** | A connection to another copy of the repository (usually on GitHub). Managed with `git remote`. |
| **Repository (Repo)** | A directory tracked by Git, containing the full project history in `.git/`. |
| **Rerere** | "Reuse Recorded Resolution" — Git remembers how you resolved conflicts and auto-applies next time. |
| **Reset** | Move the branch pointer to a different commit. Modes: `--soft`, `--mixed`, `--hard`. |
| **Revert** | Create a new commit that undoes a previous commit's changes. Safe for shared branches. |
| **Ruleset** | GitHub's modern alternative to branch protection rules. Supports multiple branches and bypass lists. |
| **Runner** | The server that executes GitHub Actions workflow jobs (e.g., `ubuntu-latest`, self-hosted). |
| **Secret Scanning** | GitHub feature that detects accidentally committed secrets (API keys, tokens) in repositories. |
| **Semantic Versioning (SemVer)** | Versioning scheme: `MAJOR.MINOR.PATCH`. Major = breaking, Minor = feature, Patch = fix. |
| **SHA-1** | Cryptographic hash function used by Git to identify objects. Produces 40-character hex strings. |
| **Shallow Clone** | A clone with limited history depth (`git clone --depth 1`). Faster but can't access full history. |
| **Sparse Checkout** | Only checking out specific directories from a repository (`git sparse-checkout`). |
| **Squash** | Combining multiple commits into a single commit. Used in PRs and interactive rebase. |
| **Staging Area** | The area between working directory and repository where you prepare the next commit. Also called the "index". |
| **Stash** | Temporarily save uncommitted changes without committing (`git stash`). |
| **Submodule** | A Git repository embedded inside another repository at a specific commit. |
| **Subtree** | An alternative to submodules — merges another repo's content into a subdirectory. |

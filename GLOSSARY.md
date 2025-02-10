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

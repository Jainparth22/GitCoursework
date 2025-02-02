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

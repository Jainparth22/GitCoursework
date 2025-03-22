# Module 26: Open-Source Contribution Guide

> **Level**: ⚫ Professional | **Time**: 3 hours | **Prerequisites**: [Module 25](../25-Branching-Strategies-and-Git-Flow/README.md)

---

## 📋 Learning Objectives

- Navigate open-source project structures
- Follow the fork-and-PR contribution workflow
- Write effective contribution documentation
- Understand open-source etiquette

---

## 1. The Contribution Workflow

> *[Visual Diagram: Architecture & Workflow]*

### Complete Remote Setup

> *[Visual Diagram: Architecture & Workflow]*

```bash
# 1. Fork on GitHub (web UI)

# 2. Clone your fork
git clone git@github.com:YOUR-USER/project.git
cd project

# 3. Add upstream remote
git remote add upstream git@github.com:ORIGINAL/project.git

# 4. Verify remotes
git remote -v
# origin    git@github.com:YOUR-USER/project.git (fetch)
# origin    git@github.com:YOUR-USER/project.git (push)
# upstream  git@github.com:ORIGINAL/project.git (fetch)
# upstream  git@github.com:ORIGINAL/project.git (push)

# 5. Keep fork synced
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

---

## 2. Finding Issues to Work On

> *[Visual Diagram: Architecture & Workflow]*

---

## 3. Required Project Files

| File | Purpose |
|------|---------|
| `README.md` | Project overview and setup |
| `CONTRIBUTING.md` | How to contribute |
| `CODE_OF_CONDUCT.md` | Community standards |
| `LICENSE` | Legal terms for use |
| `.github/ISSUE_TEMPLATE/` | Standardized issue forms |
| `.github/pull_request_template.md` | PR description template |

---


---

> *Note: Practical exercises and advanced topics currently being drafted.*

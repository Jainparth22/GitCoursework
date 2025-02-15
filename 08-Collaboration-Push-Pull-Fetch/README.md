# Module 08: Collaboration — Push, Pull, Fetch

> **Level**: 🟡 Intermediate | **Time**: 3 hours | **Prerequisites**: [Module 07](../07-Remote-Repositories-and-GitHub-Basics/README.md)

---

## 📋 Learning Objectives

- Understand the internal mechanics of push, pull, and fetch
- Handle diverged branches and pull strategies
- Set up tracking branches
- Collaborate effectively with teams

---

## 1. `git fetch` — Download Without Merging

### What Fetch Actually Does

> *[Visual Diagram: Architecture & Workflow]*

> *[Visual Diagram: Architecture & Workflow]*

```bash
git fetch origin              # Fetch from origin
git fetch --all               # Fetch from all remotes
git fetch origin main         # Fetch specific branch
git fetch --prune             # Remove deleted remote branches
```

> **Key Insight**: Fetch is always **safe** — it never changes your working directory or local branches.

---

## 2. `git pull` — Fetch + Merge (or Rebase)

### How Pull Works Internally

> *[Visual Diagram: Architecture & Workflow]*

### Pull with Merge (Default)

> *[Visual Diagram: Architecture & Workflow]*

> *[Visual Diagram: Architecture & Workflow]*

### Pull with Rebase

> *[Visual Diagram: Architecture & Workflow]*

```bash
# Pull with merge (default)
git pull origin main

# Pull with rebase (recommended for clean history)
git pull --rebase origin main

# Set rebase as default pull strategy
git config --global pull.rebase true
```

---

## 3. `git push` — Upload Your Work

### How Push Works Internally

> *[Visual Diagram: Architecture & Workflow]*

### Push Rejection and Resolution

> *[Visual Diagram: Architecture & Workflow]*

```bash
# Basic push
git push origin main

# Push with tracking (first push)
git push -u origin main

# Push all branches
git push --all origin

# Push tags
git push --tags

# Force push (DANGEROUS — use only on private branches)
git push --force origin feature/my-branch

# Safe force push (checks for remote changes first)
git push --force-with-lease origin feature/my-branch
```

---


---

> *Note: Practical exercises and advanced topics currently being drafted.*

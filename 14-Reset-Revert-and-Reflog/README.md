# Module 14: Reset, Revert & Reflog

> **Level**: 🔵 Advanced | **Time**: 3 hours | **Prerequisites**: [Module 13](../13-Git-Internals-Objects-SHA-DAG/README.md)

---

## 📋 Learning Objectives

- Understand the three modes of `git reset`
- Use `git revert` for safe public branch undo
- Master `git reflog` for disaster recovery
- Know when to use each undo mechanism

---

## 1. `git reset` — How It Works at Each Level

Reset moves the branch pointer and optionally affects the staging area and working directory.

### The Three Reset Modes

> *[Visual Diagram: Architecture & Workflow]*

### Visual: What Each Mode Affects

> *[Visual Diagram: Architecture & Workflow]*

| Mode | Moves HEAD | Resets Index | Resets Working Dir | Data Loss? |
|------|-----------|-------------|-------------------|------------|
| `--soft` | ✅ | ❌ | ❌ | No |
| `--mixed` (default) | ✅ | ✅ | ❌ | No |
| `--hard` | ✅ | ✅ | ✅ | **YES** ⚠️ |

### Step-by-Step: What Happens Internally

> *[Visual Diagram: Architecture & Workflow]*

### Use Cases

> *[Visual Diagram: Architecture & Workflow]*

```bash
# Soft: Undo commit, keep everything staged
git reset --soft HEAD~1

# Mixed (default): Undo commit + unstage
git reset HEAD~1

# Hard: Undo everything (DANGEROUS)
git reset --hard HEAD~1

# Reset specific file (unstage it)
git reset HEAD -- file.txt
# Modern equivalent:
git restore --staged file.txt
```

---

## 2. `git revert` — Safe Undo for Shared Branches

### How Revert Differs from Reset

> *[Visual Diagram: Architecture & Workflow]*

> *[Visual Diagram: Architecture & Workflow]*

### How Revert Works Internally

> *[Visual Diagram: Architecture & Workflow]*

```bash
# Revert a single commit
git revert abc1234

# Revert without auto-committing
git revert --no-commit abc1234

# Revert a merge commit (specify which parent)
git revert -m 1 <merge-commit-SHA>

# Abort a revert (if conflicts)
git revert --abort
```

---

## 3. Reset vs Revert — Decision Flowchart

> *[Visual Diagram: Architecture & Workflow]*

---


---

> *Note: Practical exercises and advanced topics currently being drafted.*

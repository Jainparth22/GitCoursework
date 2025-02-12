# Module 06: Merging & Conflict Resolution

> **Level**: 🟢 Beginner | **Time**: 3 hours | **Prerequisites**: [Module 05](../05-Branching-Basics/README.md)

---

## 📋 Learning Objectives

- Understand how Git merges work at a deep level
- Differentiate fast-forward from three-way merges
- Resolve merge conflicts manually and with tools
- Use merge-related commands effectively

---

## 1. What Is a Merge?

A merge combines the work from two branches into one.

> *[Visual Diagram: Architecture & Workflow]*

> *[Visual Diagram: Architecture & Workflow]*

---

## 2. Fast-Forward Merge

When the target branch has no new commits since the source branched off, Git simply moves the pointer forward.

### How Git Decides: Fast-Forward Possible?

> *[Visual Diagram: Architecture & Workflow]*

### Fast-Forward in Action

> *[Visual Diagram: Architecture & Workflow]*

> *[Visual Diagram: Architecture & Workflow]*

> **No merge commit** is created — the pointer just moves forward. History stays linear.

```bash
git switch main
git merge feature
# Output: Fast-forward

# Force a merge commit even when fast-forward is possible
git merge --no-ff feature
```

---

## 3. Three-Way Merge

When both branches have diverged (each has unique commits), Git performs a three-way merge.

### The Three "Ways" (Inputs)

> *[Visual Diagram: Architecture & Workflow]*

### How Git Resolves the Merge

> *[Visual Diagram: Architecture & Workflow]*

### Three-Way Merge in Action

> *[Visual Diagram: Architecture & Workflow]*

> *[Visual Diagram: Architecture & Workflow]*

```bash
git switch main
git merge feature
# Auto-merging files...
# Merge made by the 'ort' strategy.
```

---


---

> *Note: Practical exercises and advanced topics currently being drafted.*

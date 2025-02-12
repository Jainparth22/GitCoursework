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

## 4. Merge Conflicts — Why and How

### When Conflicts Happen

> *[Visual Diagram: Architecture & Workflow]*

### Conflict Markers Explained

When a conflict occurs, Git marks the file like this:

```
<<<<<<< HEAD (ours — current branch)
const greeting = "Hello, World!";
=======
const greeting = "Hi, Universe!";
>>>>>>> feature (theirs — incoming branch)
```

> *[Visual Diagram: Architecture & Workflow]*

### Using `diff3` Style (Recommended — Shows Base)

```bash
git config --global merge.conflictstyle diff3
```

Now conflicts show THREE versions:

```
<<<<<<< HEAD (ours)
const greeting = "Hello, World!";
||||||| merged common ancestor (base — original)
const greeting = "Hello";
=======
const greeting = "Hi, Universe!";
>>>>>>> feature (theirs)
```

This is much better because you can see **what the original was**, making it easier to understand what each side intended.

---

## 5. Resolving Conflicts Step-by-Step

> *[Visual Diagram: Architecture & Workflow]*

### Resolution Options

> *[Visual Diagram: Architecture & Workflow]*

```bash
# During a conflict:

# Option 1: Take our version entirely
git checkout --ours -- file.js
git add file.js

# Option 2: Take their version entirely
git checkout --theirs -- file.js
git add file.js

# Option 3: Manual editing
# Open file, remove markers, keep desired code
git add file.js

# Option 4: Merge tool
git mergetool

# Option 5: Give up, abort
git merge --abort    # Go back to pre-merge state
```

---

## 6. Merge Strategies Overview

> *[Visual Diagram: Architecture & Workflow]*

---

## 7. `--no-ff` vs Fast-Forward

> *[Visual Diagram: Architecture & Workflow]*

> *[Visual Diagram: Architecture & Workflow]*

```bash
# Explicit merge commit (recommended for feature branches)
git merge --no-ff feature/login
```

---

## 8. Squash Merge

Combine all feature commits into a single commit on the target branch:

> *[Visual Diagram: Architecture & Workflow]*

> *[Visual Diagram: Architecture & Workflow]*

```bash
git switch main
git merge --squash feature
git commit -m "feat: add complete login system"
```

---

## 🏋️ Exercises

1. Create two branches that modify different files — merge (fast-forward)
2. Create two branches that modify the **same file** — practice conflict resolution
3. Try `--no-ff` merge and compare the `git log --graph` with fast-forward
4. Practice `git merge --abort` to cancel a conflicted merge
5. Try squash merge and compare the history with regular merge
6. Set `merge.conflictstyle diff3` and resolve a conflict with the base version visible

---

## 🔑 Key Takeaways

1. **Fast-forward** happens when the target branch has no new commits — no merge commit created
2. **Three-way merge** compares the common ancestor (base) with both branches
3. Conflicts occur when the **same lines** are changed differently in both branches
4. Use `diff3` conflict style to see the **base** (original) version alongside both sides
5. `--no-ff` preserves the existence of feature branches in history
6. **Squash merge** combines all feature commits into one clean commit
7. `git merge --abort` is your escape hatch — always safe to use

---

**[← Module 05](../05-Branching-Basics/README.md)** | **[Module 07 →](../07-Remote-Repositories-and-GitHub-Basics/README.md)**

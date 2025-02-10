# Module 05: Branching Basics

> **Level**: 🟢 Beginner | **Time**: 3 hours | **Prerequisites**: [Module 04](../04-Staging-Committing-and-History/README.md)

---

## 📋 Learning Objectives

- Understand what branches really are internally
- Create, switch, list, rename, and delete branches
- Understand HEAD and detached HEAD state
- Follow branching best practices

---

## 1. What Is a Branch? — The Internal Model

A branch in Git is just a **lightweight pointer** (a 41-byte file) that points to a commit.

> *[Visual Diagram: Architecture & Workflow]*

### How Branches Are Stored

> *[Visual Diagram: Architecture & Workflow]*

```bash
# Proof: a branch is just a file with a SHA
cat .git/refs/heads/main
# Output: abc123456789... (the commit SHA)

cat .git/HEAD
# Output: ref: refs/heads/main
```

> **Key Insight**: Creating a branch is just creating a 41-byte file. That's why Git branches are practically instant — no copying files!

---

## 2. Creating Branches

### What Happens When You Create a Branch

> *[Visual Diagram: Architecture & Workflow]*

> *[Visual Diagram: Architecture & Workflow]*

```bash
# Create a branch (doesn't switch to it)
git branch feature/login

# Create AND switch to it
git switch -c feature/login
git checkout -b feature/login    # Legacy equivalent
```

---

## 3. Switching Branches

### What Happens When You Switch

> *[Visual Diagram: Architecture & Workflow]*

> *[Visual Diagram: Architecture & Workflow]*

```bash
# Switch to existing branch
git switch feature/login
git checkout feature/login    # Legacy

# Switch back to main
git switch main
```

### After Making a Commit on the Feature Branch

> *[Visual Diagram: Architecture & Workflow]*

> When you commit on a branch, **only that branch pointer moves forward**. Other branches stay where they are.

---


---

> *Note: Practical exercises and advanced topics currently being drafted.*

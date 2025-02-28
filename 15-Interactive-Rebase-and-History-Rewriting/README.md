# Module 15: Interactive Rebase & History Rewriting

> **Level**: 🔵 Advanced | **Time**: 3 hours | **Prerequisites**: [Module 14](../14-Reset-Revert-and-Reflog/README.md)

---

## 📋 Learning Objectives

- Master interactive rebase (`git rebase -i`)
- Squash, fixup, reorder, edit, and drop commits
- Use `git commit --fixup` and `--autosquash`
- Understand when history rewriting is safe

---

## 1. Interactive Rebase — How It Works

> *[Visual Diagram: Architecture & Workflow]*

### The Todo List

```
pick abc1234 feat: add user model
pick def5678 fix: typo in user model
pick ghi9012 feat: add login endpoint
pick jkl3456 docs: add API docs
```

---

## 2. Interactive Rebase Actions

> *[Visual Diagram: Architecture & Workflow]*

### Squash Example

> *[Visual Diagram: Architecture & Workflow]*

> *[Visual Diagram: Architecture & Workflow]*

Todo file:
```
pick abc1234 feat: add model
squash def5678 fix: typo
squash ghi9012 fix: another typo
```

### Fixup Example

Like squash, but **discards** the fixup commit's message:

```
pick abc1234 feat: add model
fixup def5678 fix: typo          # ← message discarded
fixup ghi9012 fix: another typo  # ← message discarded
```

Result: Single commit "feat: add model" with all changes combined.

---

## 3. `--fixup` and `--autosquash` Workflow

> *[Visual Diagram: Architecture & Workflow]*

```bash
# Create fixup commit
git commit --fixup=abc1234

# Auto-arrange and squash during rebase
git rebase -i --autosquash main

# Make autosquash the default
git config --global rebase.autosquash true
```

---


---

> *Note: Practical exercises and advanced topics currently being drafted.*

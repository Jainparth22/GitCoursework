# Module 10: Stashing & Cleaning

> **Level**: 🟡 Intermediate | **Time**: 2 hours | **Prerequisites**: [Module 09](../09-Rebasing-and-Cherry-Picking/README.md)

---

## 📋 Learning Objectives

- Understand how stash works internally
- Master all stash operations
- Clean untracked files safely

---

## 1. What Is `git stash`? — Internal Mechanics

Stash **temporarily shelves** uncommitted changes so you can work on something else, then come back.

### How Stash Works Internally

> *[Visual Diagram: Architecture & Workflow]*

### Stash Object Structure

> *[Visual Diagram: Architecture & Workflow]*

> A stash is essentially a special merge commit with 2-3 parents: HEAD, staged changes, and optionally untracked files.

---

## 2. Stash Operations

### The Stash Stack

> *[Visual Diagram: Architecture & Workflow]*

### Common Stash Commands

```bash
# Save changes to stash
git stash                         # Default message
git stash push -m "description"   # With message

# Include untracked files
git stash -u
git stash --include-untracked

# Include ALL files (even ignored)
git stash -a
git stash --all

# List stashes
git stash list

# Apply stash (keep in stack)
git stash apply                   # Latest stash
git stash apply stash@{2}        # Specific stash

# Apply AND remove from stack
git stash pop                     # Latest stash
git stash pop stash@{2}          # Specific stash

# View stash contents
git stash show                    # Summary
git stash show -p                 # Full diff
git stash show stash@{1} -p     # Specific stash diff

# Delete stashes
git stash drop stash@{0}         # Delete specific
git stash clear                   # Delete ALL stashes
```

### Stash Workflow Decision

> *[Visual Diagram: Architecture & Workflow]*

---

## 3. Partial Stash (`git stash -p`)

> *[Visual Diagram: Architecture & Workflow]*

---


---

> *Note: Practical exercises and advanced topics currently being drafted.*

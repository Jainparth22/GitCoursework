# Module 04: Staging, Committing & History

> **Level**: 🟢 Beginner | **Time**: 3 hours | **Prerequisites**: [Module 03](../03-Your-First-Repository/README.md)

---

## 📋 Learning Objectives

- Master `git add` with all its modes
- Write effective commit messages
- Navigate history with `git log`
- Compare changes with `git diff`

---

## 1. `git add` — How Staging Works Internally

### What `git add` Actually Does

> *[Visual Diagram: Architecture & Workflow]*

### The Staging Area Is a File

The staging area (index) is literally a binary file at `.git/index`:

> *[Visual Diagram: Architecture & Workflow]*

### `git add` Variations

```bash
git add file.txt             # Stage specific file
git add src/ tests/          # Stage specific directories
git add .                    # Stage ALL changes in current dir
git add -A                   # Stage ALL changes in entire repo
git add *.js                 # Stage by pattern
git add -u                   # Stage modified/deleted (not new) files
```

### Interactive Staging (`git add -p`)

Stage individual **hunks** (chunks) of changes within a file:

> *[Visual Diagram: Architecture & Workflow]*

```bash
git add -p
# Stage this hunk [y,n,q,a,d,s,e,?]?
# y = yes (stage)     n = no (skip)
# s = split           e = edit manually
# q = quit            a = stage all remaining
```

**Why use this?** Create focused, clean commits — commit the login fix separately from the UI change, even if both are in the same file.

---

## 2. `git commit` — How Commits Are Created

### What Happens When You Commit

> *[Visual Diagram: Architecture & Workflow]*

### Commit Object Structure

> *[Visual Diagram: Architecture & Workflow]*

### Commit Command Variations

```bash
# Basic commit
git commit -m "feat: add user login"

# Multi-line message
git commit -m "feat: add user login" -m "Implements JWT-based auth with refresh tokens"

# Stage tracked + commit (skip git add for modified files)
git commit -am "fix: handle null input"

# Open editor for detailed message
git commit

# Amend last commit
git commit --amend -m "fix: corrected commit message"
git commit --amend --no-edit    # Keep message, add staged changes
```

### Conventional Commit Message Format

> *[Visual Diagram: Architecture & Workflow]*

```
feat(auth): add JWT-based login

Implements login endpoint with JWT tokens.
Tokens expire after 24 hours with refresh support.

Closes #42
```

### Good vs Bad Commit Messages

| ❌ Bad | ✅ Good |
|--------|---------|
| `fix bug` | `fix(auth): handle expired JWT tokens` |
| `update` | `feat(search): add fuzzy matching` |
| `changes` | `refactor(api): extract validation middleware` |
| `wip` | `docs: add API endpoint documentation` |
| `asdfgh` | `fix(ui): correct button alignment on mobile` |

### The 7 Rules of Great Commits

> *[Visual Diagram: Architecture & Workflow]*

---

## 3. `git status` — Understanding the Output

### How `git status` Works Internally

> *[Visual Diagram: Architecture & Workflow]*

```bash
$ git status
On branch main

Changes to be committed:          # GREEN — Staged (in index, not in last commit)
  new file:   utils.js
  modified:   app.js

Changes not staged for commit:     # RED — Modified but not staged
  modified:   README.md

Untracked files:                   # RED — New files Git doesn't track
  temp.log
```

### Short Format

```bash
$ git status -s
A  utils.js       # A = Added (staged new file)
M  app.js         # M = Modified (staged)
 M README.md      # _M = Modified (unstaged)
?? temp.log       # ?? = Untracked

# Two columns: [staged][unstaged]
# M_ = staged only
# _M = unstaged only
# MM = both staged AND unstaged changes
```

---


---

> *Note: Practical exercises and advanced topics currently being drafted.*

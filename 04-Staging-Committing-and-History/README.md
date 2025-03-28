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

```mermaid
sequenceDiagram
    participant WD as Working Directory
    participant IDX as Index (.git/index)
    participant ODB as Object Database (.git/objects/)
    
    Note over WD: You edit file.txt
    
    WD->>ODB: 1. git add file.txt<br/>Creates blob object<br/>(SHA-1 hash of content)
    ODB-->>IDX: 2. Updates index with<br/>blob hash + filename + permissions
    
    Note over IDX: Index now contains:<br/>100644 blob abc123 file.txt
    Note over ODB: .git/objects/ab/c123... (blob)
```

### The Staging Area Is a File

The staging area (index) is literally a binary file at `.git/index`:

```mermaid
graph TD
    subgraph ".git/index (Staging Area)"
        I1["100644 blob a1b2c3 README.md"]
        I2["100644 blob d4e5f6 src/app.js"]
        I3["100644 blob g7h8i9 src/utils.js"]
    end
    
    subgraph "What this means"
        M1["Permission: 100644 (regular file)"]
        M2["Type: blob (file content)"]
        M3["SHA: hash of current content"]
        M4["Path: file location"]
    end
    
    I1 --> M1
    I1 --> M2
    I1 --> M3
    I1 --> M4
    
    style I1 fill:#74c0fc
    style I2 fill:#74c0fc
    style I3 fill:#74c0fc
```

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

```mermaid
flowchart TD
    A["git add -p file.txt"] --> B["Git shows first hunk"]
    B --> C{"Your choice?"}
    C -->|"y"| D["Stage this hunk ✅"]
    C -->|"n"| E["Skip this hunk ❌"]
    C -->|"s"| F["Split into smaller hunks"]
    C -->|"e"| G["Manually edit the hunk"]
    C -->|"q"| H["Quit staging"]
    
    D --> I["Show next hunk"]
    E --> I
    F --> I
    I --> C
    
    style D fill:#51cf66
    style E fill:#ff6b6b
```

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

```mermaid
sequenceDiagram
    participant IDX as Staging Area (Index)
    participant ODB as Object Database
    participant REF as refs/heads/main
    participant HEAD as HEAD
    
    Note over IDX: Contains staged files
    
    IDX->>ODB: 1. Create TREE object<br/>(directory snapshot from index)
    ODB->>ODB: 2. Create COMMIT object<br/>- Points to tree<br/>- Points to parent commit<br/>- Author, message, timestamp
    ODB-->>REF: 3. Update branch pointer<br/>(main → new commit SHA)
    
    Note over REF: main now points to<br/>the new commit
    Note over HEAD: HEAD → refs/heads/main<br/>→ new commit
```

### Commit Object Structure

```mermaid
graph TD
    COMMIT["COMMIT object<br/>SHA: abc123"]
    TREE["TREE object<br/>SHA: def456"]
    PARENT["PARENT commit<br/>SHA: previous"]
    META["Author: Parth<br/>Date: 2026-02-14<br/>Message: feat: add login"]
    
    COMMIT --> TREE
    COMMIT --> PARENT
    COMMIT --> META
    
    TREE --> B1["BLOB: README.md"]
    TREE --> B2["BLOB: app.js"]
    TREE --> ST["SUBTREE: src/"]
    ST --> B3["BLOB: utils.js"]
    
    style COMMIT fill:#ff922b
    style TREE fill:#51cf66
    style B1 fill:#74c0fc
    style B2 fill:#74c0fc
    style B3 fill:#74c0fc
    style ST fill:#51cf66
```

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

```mermaid
graph TD
    MSG["Commit Message Structure"]
    MSG --> TYPE["type(scope): description"]
    MSG --> BODY["<blank line><br/>body (optional)"]
    MSG --> FOOTER["<blank line><br/>footer (optional)"]
    
    TYPE --> TYPES["feat | fix | docs | style<br/>refactor | test | chore<br/>perf | ci | build"]
    
    style MSG fill:#ffd43b
    style TYPE fill:#74c0fc
    style BODY fill:#e599f7
    style FOOTER fill:#51cf66
```

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

```mermaid
graph TD
    R1["1. Separate subject from body with blank line"]
    R2["2. Limit subject to 50 characters"]
    R3["3. Capitalize the subject line"]
    R4["4. Do not end subject with period"]
    R5["5. Use imperative mood (Add not Added)"]
    R6["6. Wrap body at 72 characters"]
    R7["7. Explain WHAT and WHY, not HOW"]
    
    R1 --> R2 --> R3 --> R4 --> R5 --> R6 --> R7
    
    style R1 fill:#74c0fc
    style R5 fill:#ff922b
    style R7 fill:#51cf66
```

---

## 3. `git status` — Understanding the Output

### How `git status` Works Internally

```mermaid
flowchart TD
    A["git status"] --> B["Compare HEAD commit tree<br/>with Staging Area (index)"]
    B --> C["Compare Staging Area<br/>with Working Directory"]
    
    B --> D["Differences = 'Changes to be committed'<br/>(staged, green)"]
    C --> E["Differences = 'Changes not staged'<br/>(unstaged, red)"]
    C --> F["New files = 'Untracked files'<br/>(red)"]
    
    style D fill:#51cf66
    style E fill:#ff6b6b
    style F fill:#ff6b6b
```

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

## 4. `git log` — Navigating History

### How `git log` Traverses the DAG

```mermaid
graph RL
    C5["C5 (HEAD, main)"] --> C4["C4"]
    C4 --> C3["C3"]
    C3 --> C2["C2"]
    C2 --> C1["C1 (root)"]
    
    GITLOG["git log starts at HEAD<br/>and follows parent pointers<br/>backward through history"]
    
    style C5 fill:#51cf66
    style C1 fill:#ffd43b
    style GITLOG fill:#e599f7
```

### Common Log Formats

```bash
# Default (verbose)
git log

# One-line summary
git log --oneline

# Visual branch graph
git log --oneline --graph --all --decorate

# With file changes
git log --stat

# With actual diff
git log -p

# Last N commits
git log -n 5

# By author
git log --author="Parth"

# By date range
git log --since="2024-01-01" --until="2024-12-31"

# By message content
git log --grep="fix"

# Commits that changed a specific file
git log -- path/to/file.js

# Commits where a string was added/removed
git log -S "functionName"
```

### Log Output Components

```mermaid
graph TD
    subgraph "git log output"
        SHA["commit abc12345678...<br/>(40-char SHA-1 hash)"]
        AUTHOR["Author: Parth parth@email.com"]
        DATE["Date: Thu Feb 14 10:00:00 2026 +0530"]
        MSG["    feat: add user authentication"]
    end
    
    SHA --> AUTHOR --> DATE --> MSG
    
    style SHA fill:#ff922b
    style AUTHOR fill:#74c0fc
    style DATE fill:#e599f7
    style MSG fill:#51cf66
```

---

## 5. `git diff` — Understanding Changes

### What `git diff` Compares

```mermaid
flowchart LR
    WD["Working\nDirectory"] 
    SA["Staging\nArea"]
    REPO["Last\nCommit"]
    
    WD <-->|"git diff"| SA
    SA <-->|"git diff --staged"| REPO
    WD <-->|"git diff HEAD"| REPO
    
    style WD fill:#ff922b
    style SA fill:#ffd43b
    style REPO fill:#51cf66
```

### Reading Diff Output

```diff
diff --git a/app.js b/app.js
index abc1234..def5678 100644
--- a/app.js              ← Old version (a/)
+++ b/app.js              ← New version (b/)
@@ -10,7 +10,9 @@ function login(user) {  ← Hunk header
   const token = generateToken(user);     ← Context (unchanged)
-  return { success: true };              ← Removed (red)
+  return {                               ← Added (green)
+    success: true,                       ← Added
+    token: token                         ← Added
+  };                                     ← Added
 }                                        ← Context
```

### Hunk Header Explained

```mermaid
graph LR
    HH["@@ -10,7 +10,9 @@"]
    HH --> OLD["-10,7<br/>Old file: starts line 10,<br/>shows 7 lines"]
    HH --> NEW["+10,9<br/>New file: starts line 10,<br/>shows 9 lines"]
    
    style HH fill:#ffd43b
    style OLD fill:#ff6b6b
    style NEW fill:#51cf66
```

### Diff Variations

```bash
# Unstaged changes
git diff

# Staged changes (about to commit)
git diff --staged

# All changes vs last commit
git diff HEAD

# Between two commits
git diff abc123 def456

# Between branches
git diff main..feature

# Summary only (no actual diff)
git diff --stat
git diff --name-only
git diff --name-status

# Word-level diff
git diff --word-diff

# Ignore whitespace
git diff -w
```

---

## 6. File Management Commands

### `git rm` — Removing Files

```mermaid
flowchart LR
    A["git rm file.txt"] --> B["Remove from working directory"]
    A --> C["Remove from staging area"]
    B --> D["Ready to commit removal"]
    C --> D
    
    E["git rm --cached file.txt"] --> F["Remove from staging area ONLY"]
    F --> G["File remains in working directory"]
    G --> H["Becomes untracked"]
    
    style A fill:#ff6b6b
    style E fill:#ffd43b
```

```bash
# Remove file from Git AND filesystem
git rm file.txt

# Remove from Git tracking only (keep file on disk)
git rm --cached file.txt
# Useful when you accidentally committed a file that should be ignored
```

### `git mv` — Moving/Renaming Files

```bash
# Rename a file
git mv old-name.js new-name.js
# Equivalent to: mv old.js new.js && git rm old.js && git add new.js

# Move to a different directory
git mv file.js src/file.js
```

---

## 7. The Complete Workflow

```mermaid
flowchart TD
    START["Start working"] --> EDIT["Edit files"]
    EDIT --> STATUS["git status<br/>(see what changed)"]
    STATUS --> DIFF["git diff<br/>(review changes)"]
    DIFF --> ADD["git add <files><br/>(stage changes)"]
    ADD --> DIFFSTAGED["git diff --staged<br/>(verify staging)"]
    DIFFSTAGED --> COMMIT["git commit -m 'message'<br/>(create snapshot)"]
    COMMIT --> LOG["git log --oneline<br/>(verify commit)"]
    LOG --> EDIT
    
    style START fill:#e599f7
    style ADD fill:#ffd43b
    style COMMIT fill:#51cf66
    style LOG fill:#74c0fc
```

---

## 🏋️ Exercises

1. Create 3 files, stage them in two separate commits using `git add -p`
2. Write 5 commits following Conventional Commit format
3. Use `git log --oneline --graph` to visualize your history
4. Use `git diff` before and after staging to understand the difference
5. Practice `git rm --cached` to untrack a file without deleting it

---

## 🔑 Key Takeaways

1. `git add` creates blob objects and updates the index (staging area)
2. `git commit` creates tree + commit objects and moves the branch pointer
3. `git add -p` enables **partial staging** for clean, focused commits
4. `git diff` compares different areas; `--staged` compares index to last commit
5. Commit messages should be imperative, descriptive, and explain **why**
6. `git status` shows differences between all three areas simultaneously

---

**[← Module 03](../03-Your-First-Repository/README.md)** | **[Module 05 →](../05-Branching-Basics/README.md)**

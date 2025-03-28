# Module 12: Git Diff, Blame & Bisect

> **Level**: 🟡 Intermediate | **Time**: 3 hours | **Prerequisites**: [Module 11](../11-Tags-Releases-and-Semantic-Versioning/README.md)

---

## 📋 Learning Objectives

- Master `git diff` in all its forms
- Track authorship with `git blame`
- Use `git bisect` for binary search debugging
- Combine tools for effective investigation

---

## 1. `git diff` — How It Works Internally

`git diff` computes the **difference** between two Git trees (working directory, index, or commits).

### What Gets Compared

```mermaid
flowchart LR
    WD["Working<br/>Directory"]
    IDX["Staging<br/>Area (Index)"]
    HEAD_C["HEAD<br/>Commit"]
    OTHER["Other<br/>Commit/Branch"]
    
    WD <-->|"git diff"| IDX
    IDX <-->|"git diff --staged<br/>git diff --cached"| HEAD_C
    WD <-->|"git diff HEAD"| HEAD_C
    HEAD_C <-->|"git diff HEAD~1<br/>git diff main..feat"| OTHER
    
    style WD fill:#ff922b
    style IDX fill:#ffd43b
    style HEAD_C fill:#51cf66
    style OTHER fill:#74c0fc
```

### Diff Algorithm — How Git Finds Changes

```mermaid
flowchart TD
    A["Git compares two versions"] --> B["Uses Myers diff algorithm"]
    B --> C["Find Longest Common Subsequence"]
    C --> D["Lines NOT in common =<br/>additions or deletions"]
    D --> E["Group into 'hunks'<br/>(chunks of changes)"]
    E --> F["Output with context<br/>(3 lines before/after by default)"]
    
    style B fill:#74c0fc
    style E fill:#ffd43b
```

### Reading Diff Output (Anatomy)

```diff
diff --git a/app.js b/app.js    ← Files being compared
index abc1234..def5678 100644   ← Blob SHAs and permissions
--- a/app.js                    ← Old version (a/)
+++ b/app.js                    ← New version (b/)
@@ -15,8 +15,10 @@ function login() {  ← Hunk header
   // existing code              ← Context (space prefix)
-  return null;                  ← Removed (minus)
+  return {                      ← Added (plus)
+    success: true,              ← Added
+    token: jwt                  ← Added
+  };                            ← Added
   console.log("done");          ← Context
```

```mermaid
graph TD
    subgraph "Hunk Header: @@ -15,8 +15,10 @@"
        OLD["-15,8<br/>Old: starts line 15, 8 lines"]
        NEW["+15,10<br/>New: starts line 15, 10 lines"]
        FUNC["function login()<br/>Nearest function name"]
    end
    
    style OLD fill:#ff6b6b
    style NEW fill:#51cf66
    style FUNC fill:#e599f7
```

### Advanced Diff Options

```bash
# Word-level diff (instead of line-level)
git diff --word-diff

# Show only file names
git diff --name-only
git diff --name-status       # With A(dded)/M(odified)/D(eleted)

# Stats summary
git diff --stat

# Show changes in specific file
git diff -- path/to/file.js

# Ignore whitespace
git diff -w
git diff --ignore-space-change

# Between two branches
git diff main..feature
git diff main...feature     # Changes since branches diverged
```

### Two-Dot vs Three-Dot Diff

```mermaid
graph RL
    C1["C1"]
    C2["C2 (base)"] --> C1
    C3["C3"] --> C2
    C4["C4"] --> C2
    MAIN["main"] -.-> C3
    FEAT["feature"] -.-> C4
    
    TWO["main..feature<br/>Diff between C3 and C4<br/>(total difference)"] 
    THREE["main...feature<br/>Changes ONLY in feature since base<br/>(just C4 vs C2)"]
    
    style TWO fill:#ff922b
    style THREE fill:#74c0fc
```

---

## 2. `git blame` — Who Changed What?

### How Blame Works

```mermaid
sequenceDiagram
    participant Git as Git Engine
    participant History as Commit History
    participant File as Target File
    
    Git->>File: Read current file content
    
    loop For each line in file
        Git->>History: Walk backward through commits
        Git->>Git: Find the commit that<br/>last modified this line
        Git->>Git: Record: SHA, author, date
    end
    
    Git-->>Git: Output annotated file
```

### Blame Output Explained

```
abc1234 (Alice   2026-01-10 14:30 +0530  1) const express = require('express');
def5678 (Bob     2026-01-15 09:15 +0530  2) const cors = require('cors');
abc1234 (Alice   2026-01-10 14:30 +0530  3) 
ghi9012 (Charlie 2026-02-01 16:45 +0530  4) const app = express();
^first  (Alice   2026-01-10 14:30 +0530  5) app.listen(3000);
```

```mermaid
graph LR
    LINE["Each blame line"]
    LINE --> SHA["abc1234<br/>Commit SHA"]
    LINE --> AUTHOR["Alice<br/>Who changed it"]
    LINE --> DATE["2026-01-10<br/>When"]
    LINE --> LNUM["1<br/>Line number"]
    LINE --> CODE["Code content"]
    
    style SHA fill:#ff922b
    style AUTHOR fill:#51cf66
    style DATE fill:#74c0fc
    style LNUM fill:#ffd43b
```

```bash
# Blame entire file
git blame file.js

# Blame specific lines
git blame -L 10,20 file.js

# Blame specific function
git blame -L :functionName file.js

# Ignore whitespace changes
git blame -w file.js

# Show email instead of name
git blame -e file.js

# Show moves/copies across files
git blame -C file.js

# Show original commit (before renames)
git blame -C -C -C file.js
```

---

## 3. `git bisect` — Binary Search for Bugs

### The Concept

Instead of checking every commit, bisect uses **binary search** to find the bug-introducing commit.

```mermaid
flowchart TD
    subgraph "Linear Search: O(n) — Check every commit"
        L1["C1 ✅"] --> L2["C2 ✅"] --> L3["C3 ✅"] --> L4["C4 ✅"]
        L4 --> L5["C5 ❌ FOUND!"]
        L5 --> L6["C6 ❌"] --> L7["C7 ❌"] --> L8["C8 ❌"]
    end
```

```mermaid
flowchart TD
    subgraph "Binary Search: O(log n) — Only check midpoints"
        B1["C1 ✅ (good)"]
        B8["C8 ❌ (bad)"]
        B4["C4 ✅ (check middle)"]
        B6["C6 ❌ (check middle)"]
        B5["C5 ❌ FOUND!"]
        
        B1 -->|"1. Mark good"| B8
        B8 -->|"2. Mark bad"| B4
        B4 -->|"3. Mark good"| B6
        B6 -->|"4. Mark bad"| B5
    end
    
    style B5 fill:#ff6b6b,stroke:#333,stroke-width:3px
```

With 1024 commits, linear search needs **1024** checks. Bisect needs only **10** (log₂ 1024).

### How Bisect Works Internally

```mermaid
sequenceDiagram
    participant You as Developer
    participant Git as Git Engine
    participant WD as Working Directory
    
    You->>Git: git bisect start
    You->>Git: git bisect bad (current is broken)
    You->>Git: git bisect good v1.0.0 (this was working)
    
    Note over Git: Commits between good & bad: 64<br/>Check middle: commit #32
    
    Git->>WD: Checkout commit #32
    You->>You: Test... it works!
    You->>Git: git bisect good
    
    Note over Git: Bug is in commits 33-64<br/>Check middle: commit #48
    
    Git->>WD: Checkout commit #48
    You->>You: Test... it's broken!
    You->>Git: git bisect bad
    
    Note over Git: Bug is in commits 33-48<br/>Keep halving...
    
    Git-->>You: abc1234 is the first bad commit!
    You->>Git: git bisect reset
```

### Bisect Commands

```bash
# Start bisect
git bisect start

# Mark current as bad
git bisect bad

# Mark a known good commit
git bisect good v1.0.0

# Git checks out middle commit. Test it, then:
git bisect good    # If it works
git bisect bad     # If it's broken

# Repeat until Git finds the culprit

# End bisect (return to original branch)
git bisect reset

# Skip a commit (can't test this one)
git bisect skip
```

### Automated Bisect

```bash
# Run a test script automatically
git bisect start HEAD v1.0.0
git bisect run npm test
# Git will run "npm test" at each step
# Exit code 0 = good, non-zero = bad
```

```mermaid
flowchart TD
    A["git bisect run npm test"] --> B["Checkout middle commit"]
    B --> C["Run: npm test"]
    C --> D{"Exit code?"}
    D -->|"0 (pass)"| E["Mark as GOOD"]
    D -->|"Non-zero (fail)"| F["Mark as BAD"]
    E --> G{"More to check?"}
    F --> G
    G -->|"Yes"| B
    G -->|"No"| H["First bad commit found! ✅"]
    
    style H fill:#51cf66
```

---

## 4. Combining Tools — Investigation Workflow

```mermaid
flowchart TD
    A["Bug reported!"] --> B["git bisect: Find which<br/>commit introduced it"]
    B --> C["git show SHA: View<br/>the commit changes"]
    C --> D["git blame -L: See who wrote<br/>the specific buggy lines"]
    D --> E["git log --follow: Track file<br/>history and renames"]
    E --> F["Bug understood! Fix it. ✅"]
    
    style A fill:#ff6b6b
    style F fill:#51cf66
```

---

## 🏋️ Exercises

1. Use `git diff --word-diff` to compare line-level vs word-level diffs
2. Run `git blame` on a file and trace a line back to its original commit
3. Simulate a bug: make 10 commits, introduce a bug midway, use `git bisect` to find it
4. Try automated bisect: `git bisect run` with a test script
5. Compare `main..feature` vs `main...feature` diff output

---

## 🔑 Key Takeaways

1. `git diff` compares working dir, staging, and commits — know which you're comparing
2. **Two dots** (`..`) = total difference; **three dots** (`...`) = changes since divergence
3. `git blame` shows the last commit that modified each line — great for accountability
4. `git bisect` uses binary search — finds bugs in O(log n) instead of O(n)
5. Automated bisect (`git bisect run`) is the most efficient debugging tool in Git
6. Combine diff + blame + bisect for a complete investigation workflow

---

**[← Module 11](../11-Tags-Releases-and-Semantic-Versioning/README.md)** | **[Module 13 →](../13-Git-Internals-Objects-SHA-DAG/README.md)**

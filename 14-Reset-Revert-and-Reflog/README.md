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

```mermaid
flowchart TD
    subgraph "git reset --soft HEAD~1"
        S_HEAD["HEAD/Branch<br/>moves back ✅"] 
        S_INDEX["Staging Area<br/>UNCHANGED ❌"]
        S_WD["Working Directory<br/>UNCHANGED ❌"]
    end
    
    subgraph "git reset [--mixed] HEAD~1"
        M_HEAD["HEAD/Branch<br/>moves back ✅"]
        M_INDEX["Staging Area<br/>RESET to match HEAD ✅"]
        M_WD["Working Directory<br/>UNCHANGED ❌"]
    end
    
    subgraph "git reset --hard HEAD~1"
        H_HEAD["HEAD/Branch<br/>moves back ✅"]
        H_INDEX["Staging Area<br/>RESET ✅"]
        H_WD["Working Directory<br/>RESET ✅ (DESTRUCTIVE!)"]
    end
    
    style S_HEAD fill:#51cf66
    style M_HEAD fill:#51cf66
    style M_INDEX fill:#51cf66
    style H_HEAD fill:#51cf66
    style H_INDEX fill:#51cf66
    style H_WD fill:#ff6b6b
```

### Visual: What Each Mode Affects

```mermaid
graph LR
    subgraph "Three Areas"
        HEAD_B["HEAD<br/>(Branch Pointer)"]
        IDX["Staging Area<br/>(Index)"]
        WD["Working<br/>Directory"]
    end
    
    HEAD_B -->|"--soft"| HEAD_B
    HEAD_B -->|"--mixed"| IDX
    HEAD_B -->|"--hard"| WD
    
    style HEAD_B fill:#51cf66
    style IDX fill:#ffd43b
    style WD fill:#ff922b
```

| Mode | Moves HEAD | Resets Index | Resets Working Dir | Data Loss? |
|------|-----------|-------------|-------------------|------------|
| `--soft` | ✅ | ❌ | ❌ | No |
| `--mixed` (default) | ✅ | ✅ | ❌ | No |
| `--hard` | ✅ | ✅ | ✅ | **YES** ⚠️ |

### Step-by-Step: What Happens Internally

```mermaid
sequenceDiagram
    participant REF as Branch Pointer
    participant IDX as Staging Area
    participant WD as Working Directory
    
    Note over REF,WD: Starting state: HEAD at C3
    
    rect rgb(200, 255, 200)
        Note over REF: --soft: Only this step
        REF->>REF: 1. Move branch pointer<br/>from C3 back to C2
    end
    
    rect rgb(255, 255, 200)
        Note over IDX: --mixed: Steps 1-2
        IDX->>IDX: 2. Reset staging area<br/>to match C2
    end
    
    rect rgb(255, 200, 200)
        Note over WD: --hard: Steps 1-3
        WD->>WD: 3. Reset working directory<br/>to match C2
    end
```

### Use Cases

```mermaid
flowchart TD
    A["I want to undo..."] --> B{"What exactly?"}
    
    B -->|"Undo commit but<br/>keep changes staged"| SOFT["git reset --soft HEAD~1<br/>Then: re-commit differently"]
    B -->|"Undo commit and<br/>unstage changes"| MIXED["git reset HEAD~1<br/>Then: re-stage and commit"]
    B -->|"Completely erase<br/>last commit + changes"| HARD["git reset --hard HEAD~1<br/>⚠️ PERMANENT DATA LOSS"]
    
    style SOFT fill:#51cf66
    style MIXED fill:#ffd43b
    style HARD fill:#ff6b6b
```

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

```mermaid
graph RL
    subgraph "git reset HEAD~1 (REWRITES history)"
        C1R["C1"] 
        C2R["C2"] --> C1R
        MAINR["main"] -.->|"moved back"| C1R
        C2R -.->|"orphaned!"| C2R
    end
```

```mermaid
graph RL
    subgraph "git revert HEAD (ADDS new commit)"
        C1V["C1"]
        C2V["C2"] --> C1V
        RV["Revert C2<br/>(undoes C2's changes)"] --> C2V
        MAINV["main"] -.-> RV
    end
    
    style RV fill:#e599f7
```

### How Revert Works Internally

```mermaid
sequenceDiagram
    participant Git as Git Engine
    participant ODB as Object Database
    
    Note over Git: git revert abc123
    
    Git->>ODB: 1. Read commit abc123
    Git->>Git: 2. Compute diff between<br/>abc123 and its parent
    Git->>Git: 3. Apply the INVERSE<br/>of that diff
    Git->>ODB: 4. Create NEW commit<br/>with the inverse changes
    
    Note over Git: History is preserved!<br/>Original commit still exists.
```

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

```mermaid
flowchart TD
    A["Need to undo a commit"] --> B{"Has it been pushed<br/>to a shared branch?"}
    B -->|"Yes (public)"| C["USE REVERT ✅<br/>Creates new commit<br/>History preserved"]
    B -->|"No (private only)"| D{"Want to keep the changes?"}
    D -->|"Yes"| E["git reset --soft HEAD~1<br/>Changes stay staged"]
    D -->|"No"| F["git reset --hard HEAD~1<br/>⚠️ Everything gone"]
    
    style C fill:#51cf66
    style E fill:#74c0fc
    style F fill:#ff6b6b
```

---

## 4. `git reflog` — Your Safety Net

The reflog records **every time HEAD moves** — it's your undo history.

### How Reflog Works Internally

```mermaid
graph TD
    subgraph ".git/logs/HEAD (reflog)"
        R1["HEAD@{0}: commit: feat: add search<br/>SHA: abc123"]
        R2["HEAD@{1}: checkout: moving from feat to main<br/>SHA: def456"]
        R3["HEAD@{2}: commit: fix: handle null<br/>SHA: ghi789"]
        R4["HEAD@{3}: reset: moving to HEAD~1<br/>SHA: jkl012"]
        R5["HEAD@{4}: commit: initial commit<br/>SHA: mno345"]
    end
    
    R1 --> R2 --> R3 --> R4 --> R5
    
    style R1 fill:#51cf66
    style R4 fill:#ff922b
```

```bash
# View reflog
git reflog
# abc1234 HEAD@{0}: commit: feat: add search
# def5678 HEAD@{1}: checkout: moving from feature to main
# ghi9012 HEAD@{2}: reset: moving to HEAD~1
# jkl3456 HEAD@{3}: commit: this was "lost"!

# Recover from accidental reset
git reset --hard HEAD@{3}
# This restores to jkl3456 — the "lost" commit!
```

### Disaster Recovery Scenarios

```mermaid
flowchart TD
    A["Disaster!"] --> B{"What happened?"}
    
    B -->|"Accidental git reset --hard"| C["git reflog → find old SHA<br/>git reset --hard HEAD@{n}"]
    B -->|"Deleted a branch"| D["git reflog → find last commit<br/>git branch recovered-branch SHA"]
    B -->|"Bad rebase"| E["git reflog → find pre-rebase SHA<br/>git reset --hard HEAD@{n}"]
    B -->|"Lost commits"| F["git reflog → find any commit<br/>git cherry-pick SHA"]
    
    style C fill:#51cf66
    style D fill:#51cf66
    style E fill:#51cf66
    style F fill:#51cf66
```

### Reflog Expiry

```bash
# Reflog entries expire after 90 days by default
# Unreachable entries expire after 30 days

# Configure expiry
git config gc.reflogExpire 120.days
git config gc.reflogExpireUnreachable 60.days

# View reflog for a specific branch
git reflog show feature/login

# View with dates
git reflog --date=relative
```

---

## 5. Complete Undo Strategy Decision Tree

```mermaid
flowchart TD
    START["I need to undo something!"]
    START --> Q1{"What do you want to undo?"}
    
    Q1 -->|"Unstage a file"| A1["git restore --staged file"]
    Q1 -->|"Discard file changes"| A2["git restore file"]
    Q1 -->|"Amend last commit"| A3["git commit --amend"]
    Q1 -->|"Undo last commit"| Q2{"Pushed to remote?"}
    Q1 -->|"Undo old commit"| Q3{"Pushed to remote?"}
    Q1 -->|"Recover lost work"| A4["git reflog"]
    
    Q2 -->|"No"| A5["git reset --soft HEAD~1"]
    Q2 -->|"Yes"| A6["git revert HEAD"]
    
    Q3 -->|"No"| A7["git rebase -i (drop commit)"]
    Q3 -->|"Yes"| A8["git revert <SHA>"]
    
    style A1 fill:#51cf66
    style A2 fill:#51cf66
    style A3 fill:#74c0fc
    style A4 fill:#ffd43b
    style A5 fill:#74c0fc
    style A6 fill:#51cf66
    style A7 fill:#ff922b
    style A8 fill:#51cf66
```

---

## 🏋️ Exercises

1. Create 3 commits, then `reset --soft` and observe staged files
2. Try `reset --mixed` and see changes become unstaged
3. Try `reset --hard` and verify changes are gone, then recover with `reflog`
4. Revert a specific commit and verify history is preserved
5. Delete a branch, then recover it using `reflog`

---

## 🔑 Key Takeaways

1. `reset --soft` = undo commit (keep staged); `--mixed` = undo + unstage; `--hard` = destroy all
2. **Reset rewrites history** — never use on shared/pushed branches
3. **Revert is safe** — creates a new commit that undoes changes
4. **Reflog is your safety net** — records every HEAD movement for 90 days
5. Even `--hard` reset can be recovered via reflog (within the expiry period)
6. When in doubt: **revert** (safe) instead of **reset** (destructive)

---

**[← Module 13](../13-Git-Internals-Objects-SHA-DAG/README.md)** | **[Module 15 →](../15-Interactive-Rebase-and-History-Rewriting/README.md)**

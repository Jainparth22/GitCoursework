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

```mermaid
graph RL
    C3["C3<br/>abc123"] --> C2["C2<br/>def456"]
    C2 --> C1["C1<br/>ghi789"]
    
    MAIN["main<br/>(branch pointer)"] -.->|"points to"| C3
    HEAD["HEAD"] -.->|"points to"| MAIN
    
    style MAIN fill:#51cf66,stroke:#333
    style HEAD fill:#ff922b,stroke:#333
    style C3 fill:#74c0fc
    style C2 fill:#74c0fc
    style C1 fill:#74c0fc
```

### How Branches Are Stored

```mermaid
graph TD
    subgraph ".git/refs/heads/"
        MAIN_FILE["main<br/>(file containing: abc123...)"]
        FEAT_FILE["feature/login<br/>(file containing: xyz789...)"]
    end
    
    subgraph ".git/"
        HEAD_FILE["HEAD<br/>(file containing:<br/>ref: refs/heads/main)"]
    end
    
    HEAD_FILE -->|"references"| MAIN_FILE
    MAIN_FILE -->|"contains SHA of"| COMMIT["Latest commit on main"]
    FEAT_FILE -->|"contains SHA of"| COMMIT2["Latest commit on feature"]
    
    style HEAD_FILE fill:#ff922b
    style MAIN_FILE fill:#51cf66
    style FEAT_FILE fill:#74c0fc
```

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

```mermaid
sequenceDiagram
    participant You as Developer
    participant Git as Git Engine
    participant FS as .git/refs/heads/
    
    Note over FS: Only "main" exists<br/>pointing to commit C3
    
    You->>Git: git branch feature/login
    Git->>FS: Create file "feature/login"<br/>with same SHA as current HEAD
    
    Note over FS: Now two files:<br/>main → C3<br/>feature/login → C3
    Note over You: HEAD still on main<br/>(didn't switch!)
```

```mermaid
graph RL
    C3["C3"] --> C2["C2"] --> C1["C1"]
    
    MAIN["main"] -.-> C3
    FEAT["feature/login<br/>(new!)"] -.-> C3
    HEAD["HEAD"] -.-> MAIN
    
    style MAIN fill:#51cf66
    style FEAT fill:#74c0fc
    style HEAD fill:#ff922b
```

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

```mermaid
sequenceDiagram
    participant HEAD as HEAD
    participant IDX as Staging Area
    participant WD as Working Directory
    participant ODB as Object Database
    
    Note over HEAD: HEAD → main (at C3)
    
    HEAD->>HEAD: 1. Update HEAD to point to<br/>feature/login
    HEAD->>ODB: 2. Read commit that<br/>feature/login points to
    ODB->>IDX: 3. Update staging area<br/>to match that commit's tree
    IDX->>WD: 4. Update working directory<br/>to match staging area
    
    Note over WD: Files now reflect<br/>feature/login's state
```

```mermaid
graph RL
    C3["C3"] --> C2["C2"] --> C1["C1"]
    
    MAIN["main"] -.-> C3
    FEAT["feature/login"] -.-> C3
    HEAD["HEAD"] -.->|"switched!"| FEAT
    
    style MAIN fill:#51cf66
    style FEAT fill:#74c0fc
    style HEAD fill:#ff922b
```

```bash
# Switch to existing branch
git switch feature/login
git checkout feature/login    # Legacy

# Switch back to main
git switch main
```

### After Making a Commit on the Feature Branch

```mermaid
graph RL
    C3["C3"] --> C2["C2"] --> C1["C1"]
    C4["C4<br/>(new commit)"] --> C3
    
    MAIN["main"] -.-> C3
    FEAT["feature/login"] -.->|"moved forward!"| C4
    HEAD["HEAD"] -.-> FEAT
    
    style MAIN fill:#51cf66
    style FEAT fill:#74c0fc
    style HEAD fill:#ff922b
    style C4 fill:#ffd43b
```

> When you commit on a branch, **only that branch pointer moves forward**. Other branches stay where they are.

---

## 4. Branch Operations

### Listing Branches

```bash
git branch              # Local branches (* = current)
git branch -r           # Remote branches
git branch -a           # All branches (local + remote)
git branch -v           # With last commit info
git branch --merged     # Branches merged into current
git branch --no-merged  # Branches NOT merged into current
```

### Renaming Branches

```bash
# Rename current branch
git branch -m new-name

# Rename a specific branch
git branch -m old-name new-name
```

### Deleting Branches

```mermaid
flowchart TD
    A["git branch -d feature/login"] --> B{"Is branch merged<br/>into current branch?"}
    B -->|"Yes"| C["Branch deleted ✅<br/>(pointer file removed)"]
    B -->|"No"| D["Error: not fully merged ❌"]
    D --> E["git branch -D feature/login<br/>(force delete)"]
    E --> F["Branch deleted ⚠️<br/>(commits may become unreachable)"]
    
    style C fill:#51cf66
    style D fill:#ff6b6b
    style F fill:#ffd43b
```

```bash
# Safe delete (only if merged)
git branch -d feature/login

# Force delete (even if not merged)
git branch -D feature/login

# Delete remote branch
git push origin --delete feature/login
```

---

## 5. HEAD — Where Are You?

HEAD is a special pointer that tells Git which branch (or commit) you're currently on.

```mermaid
graph TD
    subgraph "Normal State"
        HEAD1["HEAD"] -->|"points to branch"| MAIN1["main"]
        MAIN1 -->|"points to commit"| C1["C3"]
    end
    
    subgraph "Detached HEAD State ⚠️"
        HEAD2["HEAD"] -->|"points directly to commit!"| C2["C3"]
    end
    
    style HEAD1 fill:#ff922b
    style HEAD2 fill:#ff6b6b
    style MAIN1 fill:#51cf66
```

### Detached HEAD

Happens when you checkout a specific commit (not a branch):

```bash
git checkout abc1234     # Detached HEAD!
git checkout v1.0.0      # Detached HEAD (checking out a tag)
```

```mermaid
flowchart TD
    A["git checkout abc1234"] --> B["HEAD points directly to abc1234<br/>(not via a branch)"]
    B --> C["You can look around and experiment"]
    C --> D{"Want to keep changes?"}
    D -->|"Yes"| E["git switch -c new-branch<br/>(Create branch here)"]
    D -->|"No"| F["git switch main<br/>(Go back, changes discarded)"]
    
    style B fill:#ffd43b
    style E fill:#51cf66
    style F fill:#ff6b6b
```

---

## 6. Branch Lifecycle — Complete Flow

```mermaid
flowchart TD
    A["Start: on main"] -->|"git switch -c feature/x"| B["Create & switch to<br/>feature/x"]
    B --> C["Make changes & commits"]
    C --> C
    C -->|"Feature complete"| D["Switch back to main<br/>git switch main"]
    D --> E["Merge feature<br/>git merge feature/x"]
    E --> F["Delete branch<br/>git branch -d feature/x"]
    F --> G["Clean! ✅"]
    
    style A fill:#51cf66
    style B fill:#74c0fc
    style C fill:#ffd43b
    style E fill:#e599f7
    style G fill:#51cf66
```

---

## 7. Branching Best Practices

### Naming Conventions

```mermaid
graph TD
    ROOT["Branch Naming"] --> FEAT["feature/ — New features<br/>feature/user-auth<br/>feature/search-bar"]
    ROOT --> FIX["fix/ or bugfix/ — Bug fixes<br/>fix/login-crash<br/>bugfix/null-pointer"]
    ROOT --> HOT["hotfix/ — Urgent production fixes<br/>hotfix/security-patch"]
    ROOT --> REL["release/ — Release preparation<br/>release/v2.0.0"]
    ROOT --> DOCS["docs/ — Documentation<br/>docs/api-reference"]
    ROOT --> EXP["experiment/ — Experimental<br/>experiment/new-algorithm"]
    
    style FEAT fill:#51cf66
    style FIX fill:#ff6b6b
    style HOT fill:#ff922b
    style REL fill:#74c0fc
    style DOCS fill:#e599f7
    style EXP fill:#ffd43b
```

### Rules

```
✅ Use lowercase with hyphens: feature/user-login
✅ Be descriptive: fix/header-overflow-on-mobile
✅ Use prefixes: feature/, fix/, hotfix/
✅ Delete after merging

❌ Don't use spaces: feature/my feature
❌ Don't use vague names: fix/stuff
❌ Don't leave stale branches
```

---

## 🏋️ Exercises

1. Create 3 branches, switch between them, and observe what `git log --graph` shows
2. Explore `.git/refs/heads/` and `.git/HEAD` to see how branches are stored
3. Practice creating a branch from a specific commit: `git branch test abc1234`
4. Enter detached HEAD state by checking out a commit, create a branch to save work
5. Try deleting a branch with `-d` vs `-D` and observe the difference

---

## 🔑 Key Takeaways

1. A branch is just a **41-byte file** pointing to a commit SHA — incredibly lightweight
2. HEAD tells Git which branch you're on (or which commit in detached mode)
3. Creating a branch is instant — Git just creates a pointer file
4. Switching branches updates HEAD, index, and working directory
5. Only the **current branch** pointer moves when you make a new commit
6. Always delete branches after merging to keep the repo clean

---

**[← Module 04](../04-Staging-Committing-and-History/README.md)** | **[Module 06 →](../06-Merging-and-Conflict-Resolution/README.md)**

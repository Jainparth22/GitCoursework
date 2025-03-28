# Module 08: Collaboration — Push, Pull, Fetch

> **Level**: 🟡 Intermediate | **Time**: 3 hours | **Prerequisites**: [Module 07](../07-Remote-Repositories-and-GitHub-Basics/README.md)

---

## 📋 Learning Objectives

- Understand the internal mechanics of push, pull, and fetch
- Handle diverged branches and pull strategies
- Set up tracking branches
- Collaborate effectively with teams

---

## 1. `git fetch` — Download Without Merging

### What Fetch Actually Does

```mermaid
sequenceDiagram
    participant Local as Local Repository
    participant Remote as GitHub (origin)
    
    Local->>Remote: git fetch origin
    Note over Remote: Sends new commits,<br/>branches, and tags
    Remote-->>Local: Transfer objects
    
    Local->>Local: Update origin/main<br/>(remote tracking branch)
    
    Note over Local: Your local "main"<br/>is NOT changed!<br/>Working directory untouched!
```

```mermaid
graph RL
    subgraph "After git fetch"
        C3["C3 (new!)"] --> C2
        C2["C2"] --> C1["C1"]
        
        MAIN["main (local)<br/>still at C2"] -.-> C2
        OM["origin/main<br/>(updated!)"] -.-> C3
    end
    
    style C3 fill:#ffd43b
    style MAIN fill:#51cf66
    style OM fill:#ff922b
```

```bash
git fetch origin              # Fetch from origin
git fetch --all               # Fetch from all remotes
git fetch origin main         # Fetch specific branch
git fetch --prune             # Remove deleted remote branches
```

> **Key Insight**: Fetch is always **safe** — it never changes your working directory or local branches.

---

## 2. `git pull` — Fetch + Merge (or Rebase)

### How Pull Works Internally

```mermaid
flowchart TD
    A["git pull origin main"] --> B["Step 1: git fetch origin main"]
    B --> C["Step 2: git merge origin/main<br/>(or git rebase origin/main)"]
    
    C --> D{"Strategy?"}
    D -->|"Default<br/>(merge)"| E["Creates merge commit<br/>if histories diverged"]
    D -->|"--rebase"| F["Replays your commits<br/>on top of remote"]
    
    style E fill:#e599f7
    style F fill:#74c0fc
```

### Pull with Merge (Default)

```mermaid
graph RL
    subgraph "Before Pull (diverged)"
        C3A["C3 (yours)"] --> C2A["C2"]
        C4A["C4 (remote)"] --> C2A
        C2A --> C1A["C1"]
        MAINA["main"] -.-> C3A
        OMA["origin/main"] -.-> C4A
    end
```

```mermaid
graph RL
    subgraph "After git pull (merge strategy)"
        C3B["C3"] --> C2B["C2"]
        C4B["C4"] --> C2B
        MB["M (merge commit)"] --> C3B
        MB --> C4B
        C2B --> C1B["C1"]
        MAINB["main"] -.-> MB
    end
    
    style MB fill:#e599f7
```

### Pull with Rebase

```mermaid
graph RL
    subgraph "After git pull --rebase"
        C4C["C4 (remote)"] --> C2C["C2"]
        C3R["C3' (rebased)"] --> C4C
        C2C --> C1C["C1"]
        MAINC["main"] -.-> C3R
    end
    
    style C3R fill:#ffd43b
```

```bash
# Pull with merge (default)
git pull origin main

# Pull with rebase (recommended for clean history)
git pull --rebase origin main

# Set rebase as default pull strategy
git config --global pull.rebase true
```

---

## 3. `git push` — Upload Your Work

### How Push Works Internally

```mermaid
sequenceDiagram
    participant Local as Local Git
    participant Remote as GitHub
    
    Local->>Remote: git push origin main
    
    Note over Local: Send new commits<br/>that remote doesn't have
    
    Remote->>Remote: Verify: Can fast-forward?
    
    alt Fast-forward possible
        Remote->>Remote: Update branch pointer
        Remote-->>Local: ✅ Push successful
    else Diverged (remote has new commits)
        Remote-->>Local: ❌ Rejected!<br/>"Updates were rejected<br/>because the remote contains<br/>work that you do not have"
    end
    
    Note over Local: If rejected:<br/>1. git pull --rebase<br/>2. Resolve conflicts<br/>3. git push again
```

### Push Rejection and Resolution

```mermaid
flowchart TD
    A["git push origin main"] --> B{"Remote up to date<br/>with your base?"}
    B -->|"Yes (fast-forward)"| C["Push succeeds ✅"]
    B -->|"No (diverged)"| D["Push REJECTED ❌"]
    
    D --> E["git pull --rebase origin main"]
    E --> F{"Conflicts?"}
    F -->|"No"| G["git push origin main ✅"]
    F -->|"Yes"| H["Resolve conflicts"]
    H --> I["git add . && git rebase --continue"]
    I --> G
    
    style C fill:#51cf66
    style D fill:#ff6b6b
    style G fill:#51cf66
```

```bash
# Basic push
git push origin main

# Push with tracking (first push)
git push -u origin main

# Push all branches
git push --all origin

# Push tags
git push --tags

# Force push (DANGEROUS — use only on private branches)
git push --force origin feature/my-branch

# Safe force push (checks for remote changes first)
git push --force-with-lease origin feature/my-branch
```

---

## 4. Force Push — `--force` vs `--force-with-lease`

```mermaid
flowchart TD
    subgraph "--force (DANGEROUS)"
        F1["Overwrites remote<br/>regardless of what's there"]
        F1 --> F2["Other people's commits<br/>can be LOST forever ⚠️"]
    end
    
    subgraph "--force-with-lease (SAFE)"
        FL1["Checks if remote has<br/>new commits since your<br/>last fetch"]
        FL1 --> FL2{"Remote changed?"}
        FL2 -->|"No"| FL3["Force push ✅"]
        FL2 -->|"Yes"| FL4["Abort ❌<br/>Fetch first!"]
    end
    
    style F2 fill:#ff6b6b
    style FL3 fill:#51cf66
    style FL4 fill:#ffd43b
```

---

## 5. The Complete Collaboration Flow

```mermaid
sequenceDiagram
    participant Dev1 as Developer 1
    participant GH as GitHub
    participant Dev2 as Developer 2
    
    Note over Dev1,Dev2: Both start synced at C2
    
    Dev1->>Dev1: Make changes (C3)
    Dev1->>GH: git push origin main (C3)
    
    Dev2->>Dev2: Make changes (C4)
    Dev2->>GH: git push origin main
    GH-->>Dev2: ❌ Rejected (remote has C3)
    
    Dev2->>GH: git pull --rebase
    GH-->>Dev2: Receive C3, rebase C4 on top → C4'
    
    Dev2->>GH: git push origin main
    GH-->>Dev2: ✅ Success (C3 → C4')
    
    Dev1->>GH: git pull
    GH-->>Dev1: Receive C4'
    
    Note over Dev1,Dev2: Both now synced at C4'
```

---

## 6. Tracking Branches

```mermaid
graph TD
    subgraph "Tracking Relationship"
        LOCAL["Local Branch<br/>main"]
        REMOTE["Remote Tracking<br/>origin/main"]
        UPSTREAM["Remote Branch<br/>(on GitHub)"]
    end
    
    LOCAL -->|"tracks"| REMOTE
    REMOTE -->|"mirrors"| UPSTREAM
    
    LOCAL -->|"git push"| UPSTREAM
    UPSTREAM -->|"git fetch"| REMOTE
    REMOTE -->|"git merge/rebase"| LOCAL
    
    style LOCAL fill:#51cf66
    style REMOTE fill:#ff922b
    style UPSTREAM fill:#74c0fc
```

```bash
# View tracking info
git branch -vv
# * main     abc1234 [origin/main] Latest commit
# develop    def5678 [origin/develop: ahead 2, behind 1]
# feature    ghi9012 (no tracking)

# Set tracking for existing branch
git branch --set-upstream-to=origin/main main
# Or shorter:
git branch -u origin/main
```

### Understanding "ahead" and "behind"

```mermaid
graph RL
    subgraph "ahead 2, behind 1"
        C2["C2 (remote new)"] --> C1["C1"]
        C3["C3 (yours)"] --> C1
        C4["C4 (yours)"] --> C3
        OM["origin/main"] -.-> C2
        M["main"] -.-> C4
    end
    
    style C2 fill:#ff922b
    style C3 fill:#51cf66
    style C4 fill:#51cf66
```

- **Ahead 2**: You have 2 commits the remote doesn't have
- **Behind 1**: The remote has 1 commit you don't have

---

## 🏋️ Exercises

1. Practice the full cycle: `fetch → pull → push` with a partner or second GitHub account
2. Create a diverged state and resolve it with `git pull --rebase`
3. Set up tracking: push a new branch with `-u` and verify with `git branch -vv`
4. Try `--force-with-lease` on a feature branch after amending a commit
5. Use `git fetch --prune` to clean up deleted remote branches

---

## 🔑 Key Takeaways

1. **Fetch** downloads remote changes without modifying your local branches — always safe
2. **Pull** = fetch + merge (or rebase, if configured)
3. **Push** fails if the remote has commits you don't have — pull first
4. Use `pull --rebase` for cleaner history (avoids unnecessary merge commits)
5. **Never force push** to shared branches; use `--force-with-lease` on private branches
6. Tracking branches let `git push/pull` know where to send/receive data

---

**[← Module 07](../07-Remote-Repositories-and-GitHub-Basics/README.md)** | **[Module 09 →](../09-Rebasing-and-Cherry-Picking/README.md)**

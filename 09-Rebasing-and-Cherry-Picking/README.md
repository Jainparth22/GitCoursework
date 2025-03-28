# Module 09: Rebasing & Cherry-Picking

> **Level**: 🟡 Intermediate | **Time**: 3.5 hours | **Prerequisites**: [Module 08](../08-Collaboration-Push-Pull-Fetch/README.md)

---

## 📋 Learning Objectives

- Understand how rebase works at the object level
- Know when to rebase vs merge
- Use cherry-pick to apply specific commits
- Follow the golden rule of rebasing

---

## 1. What Is Rebase? — The Internal Mechanics

Rebase **re-applies** your commits on top of a different base commit, creating **new commits** with new SHAs.

### Before vs After

```mermaid
graph RL
    subgraph "Before Rebase"
        C1A["C1"] 
        C2A["C2"] --> C1A
        C3A["C3"] --> C2A
        F1A["F1"] --> C2A
        F2A["F2"] --> F1A
        MAINA["main"] -.-> C3A
        FEATA["feature"] -.-> F2A
    end
    
    style C3A fill:#51cf66
    style F1A fill:#74c0fc
    style F2A fill:#74c0fc
```

```mermaid
graph RL
    subgraph "After: git rebase main (from feature)"
        C1B["C1"]
        C2B["C2"] --> C1B
        C3B["C3"] --> C2B
        F1B["F1' (new SHA!)"] --> C3B
        F2B["F2' (new SHA!)"] --> F1B
        MAINB["main"] -.-> C3B
        FEATB["feature"] -.-> F2B
    end
    
    style C3B fill:#51cf66
    style F1B fill:#ffd43b
    style F2B fill:#ffd43b
```

> The `'` marks indicate these are **new commits** — same changes but different parent, so different SHA.

### Step-by-Step: What Git Does Internally

```mermaid
sequenceDiagram
    participant Git as Git Engine
    participant ODB as Object Database
    participant REF as Branch Pointer
    
    Note over Git: git rebase main (from feature)
    
    Git->>Git: 1. Find common ancestor<br/>(C2 = merge base)
    Git->>Git: 2. Compute diffs for F1 and F2<br/>(the patches)
    Git->>Git: 3. Temporarily save patches
    Git->>REF: 4. Reset feature to main (C3)
    
    Git->>ODB: 5. Apply patch F1 on top of C3<br/>→ Creates F1' (new commit)
    Git->>ODB: 6. Apply patch F2 on top of F1'<br/>→ Creates F2' (new commit)
    
    Git->>REF: 7. Point feature → F2'
    
    Note over Git: Old F1, F2 still exist<br/>but are unreachable<br/>(garbage collected later)
```

### Rebase Algorithm Flowchart

```mermaid
flowchart TD
    A["git rebase main"] --> B["Find merge base<br/>(common ancestor)"]
    B --> C["Collect commits to replay<br/>(yours since the base)"]
    C --> D["Reset branch to target<br/>(main's HEAD)"]
    D --> E["For each commit to replay:"]
    E --> F{"Apply patch"}
    F -->|"Success"| G["Create new commit<br/>(new SHA)"]
    F -->|"Conflict"| H["Pause for resolution"]
    H --> I["Resolve & git rebase --continue"]
    I --> F
    G --> J{"More commits?"}
    J -->|"Yes"| E
    J -->|"No"| K["Rebase complete ✅"]
    
    style K fill:#51cf66
    style H fill:#ff6b6b
```

---

## 2. Merge vs Rebase — Visual Comparison

### Merge: Preserves True History

```mermaid
graph RL
    C1["C1"] 
    C2["C2"] --> C1
    C3["C3"] --> C2
    F1["F1"] --> C2
    F2["F2"] --> F1
    M["Merge"] --> C3
    M --> F2
    MAIN["main"] -.-> M
    
    style M fill:#e599f7
```

### Rebase: Creates Linear History

```mermaid
graph RL
    C1["C1"]
    C2["C2"] --> C1
    C3["C3"] --> C2
    F1["F1'"] --> C3
    F2["F2'"] --> F1
    MAIN["main"] -.-> F2
    
    style F1 fill:#ffd43b
    style F2 fill:#ffd43b
```

### Decision Flowchart

```mermaid
flowchart TD
    A["Integrate changes from<br/>another branch"] --> B{"Has this branch<br/>been pushed/shared?"}
    B -->|"Yes (public branch)"| C["USE MERGE ✅<br/>Never rewrite shared history"]
    B -->|"No (private branch)"| D{"Want linear history?"}
    D -->|"Yes"| E["USE REBASE ✅"]
    D -->|"No"| F["USE MERGE ✅"]
    
    style C fill:#51cf66
    style E fill:#74c0fc
    style F fill:#51cf66
```

---

## 3. The Golden Rule of Rebasing

> **🚨 NEVER rebase commits that have been pushed to a shared branch.**

```mermaid
flowchart TD
    A["Your commit C3<br/>SHA: abc123"] -->|"Rebase"| B["New commit C3'<br/>SHA: xyz789"]
    
    C["Other developers<br/>already have abc123"] -->|"They pull"| D["CONFLICT!<br/>Same changes,<br/>different commits"]
    
    style D fill:#ff6b6b,stroke:#333,stroke-width:3px
```

### Why?

```mermaid
sequenceDiagram
    participant Dev1 as Developer 1
    participant GH as GitHub
    participant Dev2 as Developer 2
    
    Dev1->>GH: Push C1, C2, C3
    GH-->>Dev2: Pull C1, C2, C3
    
    Note over Dev2: Dev2 now has C1, C2, C3
    
    Dev1->>Dev1: ⚠️ Rebase! C2→C2', C3→C3'
    Dev1->>GH: Force push C1, C2', C3'
    
    Dev2->>GH: git pull
    Note over Dev2: 💥 CONFUSION!<br/>Has C2 AND C2'<br/>(same changes, different SHAs)<br/>Creates duplicate commits!
```

---

## 4. Cherry-Pick — How It Works

Cherry-pick **copies the changes** from a specific commit and applies them as a **new commit** on your current branch.

```mermaid
graph RL
    subgraph "Before Cherry-Pick"
        C1A["C1"] 
        C2A["C2"] --> C1A
        C3A["C3"] --> C2A
        F1A["F1"] --> C2A
        F2A["F2 (want this!)"] --> F1A
        MAINA["main"] -.-> C3A
        FEATA["feature"] -.-> F2A
    end
    
    style F2A fill:#ffd43b,stroke:#333,stroke-width:3px
```

```mermaid
graph RL
    subgraph "After: git cherry-pick F2 (from main)"
        C1B["C1"]
        C2B["C2"] --> C1B
        C3B["C3"] --> C2B
        F2P["F2' (cherry-picked)"] --> C3B
        MAINB["main"] -.-> F2P
    end
    
    style F2P fill:#ffd43b
```

### Cherry-Pick Internally

```mermaid
sequenceDiagram
    participant Git as Git Engine
    participant ODB as Object Database
    
    Note over Git: git cherry-pick abc123
    
    Git->>ODB: 1. Read commit abc123
    Git->>Git: 2. Compute diff between<br/>abc123 and its parent
    Git->>Git: 3. Apply that diff to<br/>current HEAD
    Git->>ODB: 4. Create NEW commit<br/>(new SHA, same changes)
    
    Note over Git: Original commit untouched<br/>New commit has different<br/>parent and SHA
```

```bash
# Cherry-pick a single commit
git cherry-pick abc1234

# Cherry-pick without committing (stage only)
git cherry-pick --no-commit abc1234

# Cherry-pick multiple commits
git cherry-pick abc1234 def5678

# Cherry-pick a range
git cherry-pick abc1234..def5678  # Exclusive start
git cherry-pick abc1234^..def5678 # Inclusive start

# Abort cherry-pick (on conflict)
git cherry-pick --abort
```

### When to Use Cherry-Pick

```mermaid
flowchart TD
    CP["Cherry-Pick Use Cases"]
    CP --> A["Apply a hotfix from<br/>main to release branch"]
    CP --> B["Backport a feature<br/>to an older version"]
    CP --> C["Grab one specific commit<br/>from a large feature branch"]
    CP --> D["Recover a commit from<br/>a deleted branch"]
    
    style A fill:#51cf66
    style B fill:#74c0fc
    style C fill:#ffd43b
    style D fill:#e599f7
```

---

## 5. Rebase Conflict Resolution

```mermaid
flowchart TD
    A["git rebase main"] --> B{"Conflict on<br/>commit replay?"}
    B -->|"No"| C["Continue to next commit"]
    B -->|"Yes"| D["Rebase pauses"]
    
    D --> E["Edit conflicted files"]
    E --> F["git add resolved-file"]
    F --> G["git rebase --continue"]
    G --> B
    
    C --> H{"More commits?"}
    H -->|"Yes"| B
    H -->|"No"| I["Rebase complete ✅"]
    
    D --> J["OR: git rebase --abort<br/>(undo everything)"]
    D --> K["OR: git rebase --skip<br/>(skip this commit)"]
    
    style I fill:#51cf66
    style D fill:#ff6b6b
```

---

## 🏋️ Exercises

1. Create a feature branch, add commits on both branches, then rebase
2. Compare `git log --graph` before and after rebase vs merge
3. Cherry-pick a specific commit from one branch to another
4. Practice resolving rebase conflicts with `--continue` and `--abort`
5. Try cherry-picking a range of commits

---

## 🔑 Key Takeaways

1. Rebase **replays commits** on a new base — creates new SHAs
2. **Merge preserves history** (non-linear); **rebase creates linear history**
3. 🚨 **Never rebase public/shared branches** — it rewrites history
4. Cherry-pick copies a single commit's changes as a new commit
5. During rebase conflicts: resolve → `git add` → `git rebase --continue`
6. Use rebase for local cleanup; merge for integrating shared branches

---

**[← Module 08](../08-Collaboration-Push-Pull-Fetch/README.md)** | **[Module 10 →](../10-Stashing-and-Cleaning/README.md)**

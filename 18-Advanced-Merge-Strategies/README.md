# Module 18: Advanced Merge Strategies

> **Level**: 🔵 Advanced | **Time**: 2.5 hours | **Prerequisites**: [Module 17](../17-Submodules-and-Subtrees/README.md)

---

## 📋 Learning Objectives

- Understand all Git merge strategies
- Use `git rerere` for automatic conflict resolution
- Handle complex merge scenarios
- Choose the right merge strategy for each situation

---

## 1. Merge Strategies Overview

```mermaid
flowchart TD
    A["git merge"] --> B{"How many<br/>branches?"}
    B -->|"2 branches"| C{"Can fast-forward?"}
    C -->|"Yes"| FF["Fast-Forward<br/>(just move pointer)"]
    C -->|"No"| ORT["ort Strategy<br/>(default 3-way merge)"]
    B -->|"3+ branches"| OCT["Octopus Strategy"]
    
    ORT --> OPTIONS["Strategy Options"]
    OPTIONS --> X_OURS["-X ours<br/>On conflict: keep ours"]
    OPTIONS --> X_THEIRS["-X theirs<br/>On conflict: keep theirs"]
    OPTIONS --> PATIENCE["-X patience<br/>Better diff algorithm"]
    
    A --> STRAT_OURS["--strategy=ours<br/>Ignore all their changes"]
    A --> STRAT_SUB["--strategy=subtree<br/>For subtree merges"]
    
    style FF fill:#51cf66
    style ORT fill:#74c0fc
    style OCT fill:#ffd43b
    style STRAT_OURS fill:#ff6b6b
```

### Strategy Comparison

| Strategy | When Used | Commits | Handles Renames |
|----------|----------|---------|----------------|
| Fast-forward | Linear history | No merge commit | N/A |
| ort (default) | 2 diverged branches | Merge commit | ✅ |
| Octopus | 3+ branches | Merge commit | Limited |
| ours | Discard their changes | Merge commit | N/A |
| subtree | Subtree merges | Merge commit | ✅ |

---

## 2. The `ort` Strategy (Default) — Deep Dive

```mermaid
sequenceDiagram
    participant Git as Git Engine
    participant Base as Merge Base
    participant Ours as Our Branch
    participant Theirs as Their Branch
    
    Git->>Base: 1. Find common ancestor
    Git->>Git: 2. Compute diff: Base → Ours
    Git->>Git: 3. Compute diff: Base → Theirs
    
    Git->>Git: 4. For each file:
    
    alt Only one side changed
        Git->>Git: Take the changed version ✅
    else Both sides changed different lines
        Git->>Git: Merge both changes ✅
    else Both sides changed same lines
        Git->>Git: ⚠️ CONFLICT — mark for manual resolution
    end
    
    Note over Git: 5. Handle renames<br/>(detect file moves/renames)
    Git->>Git: 6. Create merge commit
```

### `-X ours` vs `-X theirs` (Strategy Options)

```mermaid
graph TD
    CONFLICT["Conflict detected!"]
    CONFLICT --> XOURS["-X ours<br/>Automatically take OUR version<br/>for conflicting sections only"]
    CONFLICT --> XTHEIRS["-X theirs<br/>Automatically take THEIR version<br/>for conflicting sections only"]
    
    NOTE["Note: Non-conflicting changes<br/>from both sides are STILL merged!"]
    
    style XOURS fill:#51cf66
    style XTHEIRS fill:#74c0fc
    style NOTE fill:#ffd43b
```

```bash
# For conflicts, prefer our changes
git merge -X ours feature

# For conflicts, prefer their changes
git merge -X theirs feature
```

### `--strategy=ours` (Ignore All Their Changes)

```mermaid
graph RL
    subgraph "Before"
        C1A["C1"]
        C2A["C2 (ours)"] --> C1A
        C3A["C3 (theirs)"] --> C1A
    end
```

```mermaid 
graph RL
    subgraph "After: git merge -s ours feature"
        C1B["C1"]
        C2B["C2"] --> C1B
        C3B["C3"] --> C1B
        M["Merge commit<br/>(tree = C2's tree exactly!)"] --> C2B
        M --> C3B
    end
    
    style M fill:#e599f7
```

> **All of feature's changes are discarded.** The merge commit's tree is identical to ours.

---

## 3. `git rerere` — Reuse Recorded Resolution

### How rerere Works

```mermaid
sequenceDiagram
    participant Dev as Developer
    participant Rerere as Rerere Cache
    
    Note over Dev: First time conflict
    Dev->>Dev: Resolve conflict manually
    Dev->>Rerere: rerere records the resolution
    
    Note over Dev: Same conflict appears again<br/>(e.g., during another merge/rebase)
    
    Dev->>Rerere: Git checks rerere cache
    Rerere-->>Dev: ✅ Auto-resolves using<br/>recorded resolution!
    
    Note over Dev: No manual work needed!
```

```bash
# Enable rerere
git config --global rerere.enabled true

# View recorded resolutions
git rerere status

# Forget a specific resolution
git rerere forget path/to/file

# Clear all recorded resolutions
git rerere clear

# Show diff of what rerere would apply
git rerere diff
```

### When rerere Is Most Useful

```mermaid
flowchart TD
    A["rerere shines when:"]
    A --> B["Rebasing long-lived branches<br/>(same conflicts repeat)"]
    A --> C["Testing topic branches<br/>(merge → test → reset → merge again)"]
    A --> D["Cherry-picking across<br/>multiple branches"]
    
    style B fill:#51cf66
    style C fill:#51cf66
    style D fill:#51cf66
```

---

## 4. Octopus Merge (3+ Branches)

```mermaid
graph RL
    C1["C1"]
    F1["Feature A"] --> C1
    F2["Feature B"] --> C1
    F3["Feature C"] --> C1
    M["Octopus Merge<br/>(3 parents!)"] --> F1
    M --> F2
    M --> F3
    MAIN["main"] -.-> M
    
    style M fill:#e599f7
```

```bash
# Merge multiple branches at once
git merge feature-a feature-b feature-c
# Uses octopus strategy automatically

# Cannot handle conflicts — use regular merge instead
```

---

## 5. Advanced Conflict Resolution Tools

```mermaid
flowchart TD
    A["Complex Conflict?"] --> B{"Choose tool"}
    B --> C["git mergetool<br/>Visual 3-way merge"]
    B --> D["VS Code merge editor<br/>Built-in conflict UI"]
    B --> E["Manual editing<br/>Edit conflict markers"]
    
    C --> C1["Configured via:<br/>git config merge.tool vimdiff"]
    
    style C fill:#74c0fc
    style D fill:#51cf66
    style E fill:#ffd43b
```

```bash
# Set merge tool
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait --merge $REMOTE $LOCAL $BASE $MERGED'

# Launch merge tool for conflicts
git mergetool
```

---

## 🏋️ Exercises

1. Practice `-X ours` vs `-X theirs` with conflicting branches
2. Enable `rerere`, create a conflict, resolve it, then recreate the same conflict
3. Try `--strategy=ours` and verify the resulting tree matches your branch exactly
4. Merge 3 branches simultaneously using octopus strategy
5. Configure and use `git mergetool` for conflict resolution

---

## 🔑 Key Takeaways

1. **ort** is the default strategy — handles renames and most scenarios well
2. `-X ours/theirs` auto-resolves **only conflicting sections** (non-conflicting changes still merge)
3. `--strategy=ours` ignores **ALL** their changes (rarely used, but useful for placeholder merges)
4. **rerere** saves time by remembering how you resolved conflicts
5. Octopus merge handles 3+ branches but cannot resolve conflicts
6. Enable `rerere` globally — it's free insurance against repeated conflicts

---

**[← Module 17](../17-Submodules-and-Subtrees/README.md)** | **[Module 19 →](../19-GitHub-Issues-Projects-and-Wikis/README.md)**

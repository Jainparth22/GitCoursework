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

```mermaid
sequenceDiagram
    participant WD as Working Directory
    participant IDX as Staging Area
    participant STASH as Stash Stack (.git/refs/stash)
    participant ODB as Object Database
    
    Note over WD: You have uncommitted changes
    
    WD->>ODB: 1. Save working directory state<br/>as a commit-like object
    IDX->>ODB: 2. Save index state<br/>as another commit-like object
    ODB->>STASH: 3. Create stash entry<br/>(special merge commit)
    STASH->>WD: 4. Reset working directory<br/>to clean state (HEAD)
    
    Note over WD: Working directory is now clean!
    Note over STASH: Changes are safely stored
```

### Stash Object Structure

```mermaid
graph TD
    STASH["Stash Entry<br/>(special commit)"]
    STASH --> P1["Parent 1: HEAD commit"]
    STASH --> P2["Parent 2: Index state"]
    STASH --> P3["Parent 3: Untracked files<br/>(if -u flag used)"]
    STASH --> WD_TREE["Tree: Working directory state"]
    
    style STASH fill:#e599f7
    style P1 fill:#51cf66
    style P2 fill:#74c0fc
    style P3 fill:#ffd43b
```

> A stash is essentially a special merge commit with 2-3 parents: HEAD, staged changes, and optionally untracked files.

---

## 2. Stash Operations

### The Stash Stack

```mermaid
graph TD
    subgraph "Stash Stack (LIFO)"
        S0["stash@{0} — Most recent<br/>'WIP: fix login bug'"]
        S1["stash@{1}<br/>'dashboard refactor'"]
        S2["stash@{2} — Oldest<br/>'api changes'"]
    end
    
    TOP["git stash pop<br/>→ applies stash@{0}"] --> S0
    
    style S0 fill:#51cf66
    style S1 fill:#74c0fc
    style S2 fill:#ffd43b
```

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

```mermaid
flowchart TD
    A["Need to switch branches<br/>but have uncommitted work"] --> B{"Changes worth<br/>keeping?"}
    B -->|"Yes, but not ready<br/>to commit"| C["git stash push -m 'description'"]
    B -->|"Yes, close to done"| D["git commit<br/>(WIP commit, amend later)"]
    B -->|"No, discard them"| E["git restore ."]
    
    C --> F["Switch branches, do other work"]
    F --> G["Switch back"]
    G --> H["git stash pop"]
    H --> I["Continue where you left off ✅"]
    
    style C fill:#51cf66
    style I fill:#51cf66
```

---

## 3. Partial Stash (`git stash -p`)

```mermaid
flowchart TD
    A["git stash -p"] --> B["Git shows each hunk"]
    B --> C{"Stash this hunk?"}
    C -->|"y"| D["Hunk will be stashed"]
    C -->|"n"| E["Hunk stays in working dir"]
    C -->|"s"| F["Split into smaller hunks"]
    C -->|"q"| G["Done selecting"]
    
    D --> H["Show next hunk"]
    E --> H
    F --> H
    H --> C
    
    style D fill:#51cf66
    style E fill:#ff6b6b
```

---

## 4. Creating a Branch from Stash

```bash
git stash branch new-feature-branch stash@{0}
# Creates branch, checks it out, applies stash, drops stash
```

```mermaid
flowchart LR
    A["stash@{0}"] -->|"git stash branch feat"| B["New branch 'feat'<br/>with stash applied<br/>Stash dropped"]
    
    style A fill:#ffd43b
    style B fill:#51cf66
```

---

## 5. `git clean` — Removing Untracked Files

### How Clean Works

```mermaid
flowchart TD
    A["git clean"] --> B{"Flags?"}
    B -->|"-n (dry run)"| C["Show what WOULD be deleted<br/>(safe preview)"]
    B -->|"-f (force)"| D["Delete untracked files"]
    B -->|"-fd"| E["Delete untracked files AND directories"]
    B -->|"-fdx"| F["Delete untracked + ignored files<br/>(nuclear option ☢️)"]
    
    style C fill:#51cf66
    style D fill:#ffd43b
    style E fill:#ff922b
    style F fill:#ff6b6b
```

```bash
# Preview what would be deleted (always do this first!)
git clean -n

# Delete untracked files
git clean -f

# Delete untracked files + directories
git clean -fd

# Delete including ignored files (e.g., node_modules, build)
git clean -fdx

# Interactive mode
git clean -i
```

### Clean vs Restore vs Reset

```mermaid
graph TD
    UNDO["Ways to undo changes"]
    UNDO --> RESTORE["git restore file<br/>Discard modifications<br/>to tracked files"]
    UNDO --> RESTORE_S["git restore --staged file<br/>Unstage a file<br/>(keep modifications)"]
    UNDO --> CLEAN["git clean -fd<br/>Remove untracked<br/>files & directories"]
    UNDO --> RESET["git reset --hard HEAD<br/>Reset everything<br/>to last commit"]
    
    style RESTORE fill:#51cf66
    style RESTORE_S fill:#74c0fc
    style CLEAN fill:#ffd43b
    style RESET fill:#ff6b6b
```

---

## 🏋️ Exercises

1. Make changes, stash them, switch branches, come back, and pop stash
2. Use `git stash -p` to selectively stash only some changes
3. Create a branch from a stash with `git stash branch`
4. Practice `git clean -n` before `git clean -fd` for safe cleanup
5. Compare `git stash show -p` output between multiple stashes

---

## 🔑 Key Takeaways

1. Stash stores uncommitted changes as special commit objects in a stack
2. Always use `-m` to describe your stash — unnamed stashes are confusing
3. `pop` = apply + drop; `apply` = apply without removing from stack
4. Use `-u` to include untracked files in the stash
5. Always `git clean -n` (dry run) before `git clean -f` to preview deletions
6. Stash, restore, clean, and reset solve different "undo" scenarios

---

**[← Module 09](../09-Rebasing-and-Cherry-Picking/README.md)** | **[Module 11 →](../11-Tags-Releases-and-Semantic-Versioning/README.md)**

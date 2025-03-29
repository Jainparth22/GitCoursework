# Module 15: Interactive Rebase & History Rewriting

> **Level**: 🔵 Advanced | **Time**: 3 hours | **Prerequisites**: [Module 14](../14-Reset-Revert-and-Reflog/README.md)

---

## 📋 Learning Objectives

- Master interactive rebase (`git rebase -i`)
- Squash, fixup, reorder, edit, and drop commits
- Use `git commit --fixup` and `--autosquash`
- Understand when history rewriting is safe

---

## 1. Interactive Rebase — How It Works

```mermaid
sequenceDiagram
    participant You as Developer
    participant Git as Git Engine
    participant Editor as Text Editor
    
    You->>Git: git rebase -i HEAD~4
    Git->>Editor: Open todo list with last 4 commits
    
    Note over Editor: You edit actions:<br/>pick, squash, fixup,<br/>reword, edit, drop
    
    Editor->>Git: Save and close editor
    Git->>Git: Replay commits one by one<br/>applying your requested actions
    Git-->>You: Rebase complete ✅
```

### The Todo List

```
pick abc1234 feat: add user model
pick def5678 fix: typo in user model
pick ghi9012 feat: add login endpoint
pick jkl3456 docs: add API docs
```

---

## 2. Interactive Rebase Actions

```mermaid
graph TD
    ACTIONS["Rebase Actions"]
    ACTIONS --> PICK["pick (p)<br/>Keep commit as-is"]
    ACTIONS --> REWORD["reword (r)<br/>Keep changes,<br/>edit message"]
    ACTIONS --> EDIT["edit (e)<br/>Pause here,<br/>let you modify"]
    ACTIONS --> SQUASH["squash (s)<br/>Meld into previous,<br/>combine messages"]
    ACTIONS --> FIXUP["fixup (f)<br/>Meld into previous,<br/>discard this message"]
    ACTIONS --> DROP["drop (d)<br/>Remove commit<br/>entirely"]
    
    style PICK fill:#51cf66
    style REWORD fill:#74c0fc
    style EDIT fill:#ffd43b
    style SQUASH fill:#e599f7
    style FIXUP fill:#ff922b
    style DROP fill:#ff6b6b
```

### Squash Example

```mermaid
graph RL
    subgraph "Before Squash"
        C1A["C1: feat: add model"]
        C2A["C2: fix: typo"] --> C1A
        C3A["C3: fix: another typo"] --> C2A
    end
```

```mermaid
graph RL
    subgraph "After Squash (C2 + C3 into C1)"
        C1B["C1': feat: add model<br/>(includes all 3 changes)<br/>NEW SHA!"]
    end
    
    style C1B fill:#e599f7
```

Todo file:
```
pick abc1234 feat: add model
squash def5678 fix: typo
squash ghi9012 fix: another typo
```

### Fixup Example

Like squash, but **discards** the fixup commit's message:

```
pick abc1234 feat: add model
fixup def5678 fix: typo          # ← message discarded
fixup ghi9012 fix: another typo  # ← message discarded
```

Result: Single commit "feat: add model" with all changes combined.

---

## 3. `--fixup` and `--autosquash` Workflow

```mermaid
flowchart TD
    A["Working on feature..."] --> B["Commit: feat: add login"]
    B --> C["Notice a bug in that commit"]
    C --> D["Fix the bug"]
    D --> E["git commit --fixup=abc123<br/>(creates 'fixup! feat: add login')"]
    E --> F["Continue working..."]
    F --> G["git rebase -i --autosquash main"]
    G --> H["Git auto-arranges fixup commits<br/>right after their targets!"]
    
    style E fill:#ffd43b
    style H fill:#51cf66
```

```bash
# Create fixup commit
git commit --fixup=abc1234

# Auto-arrange and squash during rebase
git rebase -i --autosquash main

# Make autosquash the default
git config --global rebase.autosquash true
```

---

## 4. Reordering Commits

Simply change the order of lines in the todo file:

```
# Before (original order):
pick abc1234 feat: add user model
pick def5678 docs: add README
pick ghi9012 feat: add login

# After (reordered):
pick abc1234 feat: add user model
pick ghi9012 feat: add login
pick def5678 docs: add README
```

---

## 5. Edit Mode — Pausing to Modify

```mermaid
flowchart TD
    A["edit abc1234"] --> B["Git applies commit C1<br/>and PAUSES"]
    B --> C["You can now:<br/>• Edit files<br/>• Split the commit<br/>• Add files"]
    C --> D["git add changes"]
    D --> E["git commit --amend"]
    E --> F["git rebase --continue"]
    
    style B fill:#ffd43b
    style F fill:#51cf66
```

### Splitting a Commit

```bash
# In the todo, change "pick" to "edit" for the commit to split
# When Git pauses at that commit:

git reset HEAD~1            # Undo the commit (keep files)
git add file1.js
git commit -m "Part 1: add file1"
git add file2.js
git commit -m "Part 2: add file2"
git rebase --continue       # Resume rebase
```

---

## 6. Other History Rewriting Tools

### `git filter-branch` / `git filter-repo`

```mermaid
flowchart TD
    A["Need to rewrite<br/>entire repo history?"] --> B{"What kind?"}
    B -->|"Remove sensitive file<br/>from all commits"| C["git filter-repo<br/>--path secret.env --invert-paths"]
    B -->|"Change author email<br/>across all history"| D["git filter-repo<br/>--mailmap my-mailmap"]
    B -->|"Move directory"| E["git filter-repo<br/>--path-rename src/:lib/"]
    
    style C fill:#ff6b6b
    style D fill:#74c0fc
    style E fill:#ffd43b
```

```bash
# Install git-filter-repo (recommended over filter-branch)
pip install git-filter-repo

# Remove a file from entire history
git filter-repo --path passwords.txt --invert-paths

# Change author across all commits
git filter-repo --email-callback '
    return email.replace(b"old@email.com", b"new@email.com")
'
```

---

## 7. Safety Rules for History Rewriting

```mermaid
flowchart TD
    A["Want to rewrite history?"] --> B{"Have these commits<br/>been pushed/shared?"}
    B -->|"NO — Local only"| C["✅ Safe to rewrite<br/>rebase -i, amend, reset"]
    B -->|"YES — Pushed"| D{"Are you the ONLY<br/>person working on<br/>this branch?"}
    D -->|"Yes (personal branch)"| E["⚠️ Okay, but use<br/>--force-with-lease"]
    D -->|"No (shared branch)"| F["❌ NEVER rewrite<br/>Use revert instead"]
    
    style C fill:#51cf66
    style E fill:#ffd43b
    style F fill:#ff6b6b
```

---

## 🏋️ Exercises

1. Create 5 messy commits, then use `rebase -i` to squash WIP commits together
2. Reorder commits so docs come after all feature commits
3. Use `--fixup` and `--autosquash` to fix a bug in an earlier commit
4. Split one commit into two using `edit` mode
5. Use `reword` to fix a commit message without changing any code

---

## 🔑 Key Takeaways

1. Interactive rebase lets you **rewrite local history** before sharing
2. **Squash** combines commits and merges messages; **fixup** discards the message
3. `--fixup` + `--autosquash` is the cleanest workflow for fixing earlier commits
4. **Edit** mode lets you pause and modify (or split) any commit
5. Never rewrite history that has been pushed to shared branches
6. Run `git rebase -i` before every PR to create a clean, professional history

---

**[← Module 14](../14-Reset-Revert-and-Reflog/README.md)** | **[Module 16 →](../16-Git-Hooks-and-Automation/README.md)**

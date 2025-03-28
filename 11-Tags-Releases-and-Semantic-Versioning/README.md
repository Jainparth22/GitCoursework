# Module 11: Tags, Releases & Semantic Versioning

> **Level**: 🟡 Intermediate | **Time**: 2.5 hours | **Prerequisites**: [Module 10](../10-Stashing-and-Cleaning/README.md)

---

## 📋 Learning Objectives

- Understand Git tags and how they differ from branches
- Create lightweight vs annotated tags
- Master Semantic Versioning
- Create GitHub Releases

---

## 1. What Are Tags? — Internal Model

Tags are **permanent pointers** to specific commits. Unlike branches, they **don't move**.

```mermaid
graph RL
    C1["C1"] 
    C2["C2"] --> C1
    C3["C3"] --> C2
    C4["C4"] --> C3
    C5["C5"] --> C4
    
    MAIN["main<br/>(moves with commits)"] -.->|"pointer moves →"| C5
    V1["v1.0.0<br/>(tag — stays put!)"] -.-> C2
    V2["v2.0.0<br/>(tag — stays put!)"] -.-> C4
    
    style MAIN fill:#51cf66
    style V1 fill:#ff922b
    style V2 fill:#ff922b
```

### How Tags Are Stored

```mermaid
graph TD
    subgraph ".git/refs/tags/"
        LT["v1.0.0 (lightweight)<br/>Contains: commit SHA directly"]
        AT["v2.0.0 (annotated)<br/>Contains: tag object SHA"]
    end
    
    subgraph ".git/objects/"
        TAG_OBJ["Tag Object<br/>- Commit SHA<br/>- Tagger name/email<br/>- Date<br/>- Message<br/>- Optional GPG signature"]
    end
    
    LT -->|"points to"| COMMIT1["Commit"]
    AT -->|"points to"| TAG_OBJ
    TAG_OBJ -->|"points to"| COMMIT2["Commit"]
    
    style LT fill:#ffd43b
    style AT fill:#51cf66
    style TAG_OBJ fill:#74c0fc
```

---

## 2. Lightweight vs Annotated Tags

```mermaid
flowchart TD
    A["Creating a Tag"] --> B{Which type?}
    B -->|"Lightweight"| C["Just a pointer<br/>(like a branch that never moves)"]
    B -->|"Annotated (Recommended)"| D["Full Git object with:<br/>• Author<br/>• Date<br/>• Message<br/>• Optional GPG signature"]
    
    C --> E["git tag v1.0.0"]
    D --> F["git tag -a v1.0.0 -m 'Release 1.0'"]
    
    style C fill:#ffd43b
    style D fill:#51cf66
```

| Feature | Lightweight | Annotated |
|---------|------------|-----------|
| Stored as | Pointer (ref) | Full Git object |
| Has author | ❌ | ✅ |
| Has date | ❌ | ✅ |
| Has message | ❌ | ✅ |
| GPG signable | ❌ | ✅ |
| For releases | ❌ | ✅ Recommended |
| For temp marks | ✅ | Overkill |

```bash
# Lightweight
git tag v1.0.0
git tag temp-mark

# Annotated (recommended for releases)
git tag -a v1.0.0 -m "First stable release"

# Tag a specific commit
git tag -a v0.9.0 abc1234 -m "Beta release"

# List tags
git tag
git tag -l "v2.*"        # Filter by pattern

# Show tag details
git show v1.0.0

# Delete local tag
git tag -d v1.0.0

# Push tags
git push origin v1.0.0   # Single tag
git push origin --tags    # All tags

# Delete remote tag
git push origin --delete v1.0.0
```

---

## 3. Semantic Versioning (SemVer)

```mermaid
graph TD
    VER["v2.4.1"] --> MAJOR["2 = MAJOR<br/>Breaking changes<br/>Incompatible API changes"]
    VER --> MINOR["4 = MINOR<br/>New features<br/>Backward compatible"]
    VER --> PATCH["1 = PATCH<br/>Bug fixes<br/>Backward compatible"]
    
    style MAJOR fill:#ff6b6b
    style MINOR fill:#ffd43b
    style PATCH fill:#51cf66
```

### Version Bump Decision Flow

```mermaid
flowchart TD
    A["New Release"] --> B{"Did you break<br/>existing API?"}
    B -->|"Yes"| C["MAJOR bump<br/>1.0.0 → 2.0.0"]
    B -->|"No"| D{"Did you add<br/>new features?"}
    D -->|"Yes"| E["MINOR bump<br/>1.0.0 → 1.1.0"]
    D -->|"No"| F{"Did you fix bugs<br/>or improve perf?"}
    F -->|"Yes"| G["PATCH bump<br/>1.0.0 → 1.0.1"]
    F -->|"No"| H["No version bump needed"]
    
    style C fill:#ff6b6b
    style E fill:#ffd43b
    style G fill:#51cf66
```

### Pre-Release Versions

```
v1.0.0-alpha.1    ← Very early, unstable
v1.0.0-beta.1     ← Feature complete, may have bugs
v1.0.0-rc.1       ← Release candidate, nearly done
v1.0.0            ← Stable release
```

---

## 4. GitHub Releases

```mermaid
flowchart LR
    TAG["Git Tag<br/>v1.0.0"] -->|"Create Release<br/>on GitHub"| RELEASE["GitHub Release"]
    
    RELEASE --> NOTES["Release Notes<br/>(changelog)"]
    RELEASE --> ASSETS["Binary Assets<br/>(downloadable files)"]
    RELEASE --> BADGE["Latest Release Badge"]
    
    style TAG fill:#ff922b
    style RELEASE fill:#51cf66
```

```bash
# Create release via GitHub CLI
gh release create v1.0.0 --title "v1.0.0" --notes "First release"

# Auto-generate notes from commit history
gh release create v1.0.0 --generate-notes

# Create release with assets
gh release create v1.0.0 ./dist/app.zip --title "v1.0.0"

# Create draft release
gh release create v1.0.0 --draft

# List releases
gh release list
```

---

## 🏋️ Exercises

1. Create both lightweight and annotated tags and compare with `git show`
2. Tag a past commit and push the tag to GitHub
3. Delete a tag locally and remotely
4. Create a GitHub Release with auto-generated notes using `gh release create`
5. Practice SemVer: given a changelog, determine the correct version bump

---

## 🔑 Key Takeaways

1. Tags are **permanent pointers** — they don't move like branches
2. Use **annotated tags** for releases (includes metadata and can be signed)
3. SemVer: MAJOR (breaking) . MINOR (features) . PATCH (fixes)
4. GitHub Releases wrap tags with release notes and downloadable assets
5. Always push tags explicitly — `git push` doesn't push tags by default

---

**[← Module 10](../10-Stashing-and-Cleaning/README.md)** | **[Module 12 →](../12-Git-Diff-Blame-and-Bisect/README.md)**

# Module 13: Git Internals — Objects, SHA, DAG

> **Level**: 🔵 Advanced | **Time**: 4 hours | **Prerequisites**: [Module 12](../12-Git-Diff-Blame-and-Bisect/README.md)

---

## 📋 Learning Objectives

- Understand Git's content-addressable storage model
- Explore the four Git object types
- Navigate the Directed Acyclic Graph (DAG)
- Use plumbing commands to inspect internals

---

## 1. Git's Content-Addressable Filesystem

Git is fundamentally a **content-addressable filesystem** — a key-value store where every piece of data is retrieved by its **SHA-1 hash**.

```mermaid
flowchart LR
    CONTENT["Any Content<br/>(file, directory, commit)"] 
    -->|"SHA-1 hash"| KEY["40-char hex string<br/>e.g. 2cf24dba5fb..."]
    -->|"stored at"| PATH[".git/objects/2c/f24dba5fb..."]
    
    style CONTENT fill:#74c0fc
    style KEY fill:#ffd43b
    style PATH fill:#51cf66
```

```bash
# Hash some content
echo "Hello World" | git hash-object --stdin
# Output: 557db03de997c86a4a028e1ebd3a1ceb225be238

# Same content ALWAYS produces same hash
echo "Hello World" | git hash-object --stdin
# Output: 557db03de997c86a4a028e1ebd3a1ceb225be238  (identical!)
```

### Object Storage Layout

```mermaid
graph TD
    subgraph ".git/objects/"
        DIR1["2c/"] --> FILE1["f24dba5fb0a30e26e83b2ac..."]
        DIR2["55/"] --> FILE2["7db03de997c86a4a028e1e..."]
        DIR3["ab/"] --> FILE3["c12345678901234567890123..."]
        PACK["pack/"] --> P1["pack-abc.pack (+.idx)"]
        INFO["info/"]
    end
    
    style DIR1 fill:#74c0fc
    style DIR2 fill:#74c0fc
    style DIR3 fill:#74c0fc
    style PACK fill:#ffd43b
```

> Objects are stored as compressed files. First 2 hex characters = directory, remaining 38 = filename.

---

## 2. The Four Object Types

```mermaid
graph TD
    OBJECTS["Git Object Types"]
    OBJECTS --> BLOB["📄 BLOB<br/>File content<br/>(no filename!)"]
    OBJECTS --> TREE["📁 TREE<br/>Directory listing<br/>(maps names → blobs/trees)"]
    OBJECTS --> COMMIT["📌 COMMIT<br/>Snapshot pointer<br/>(tree + parent + metadata)"]
    OBJECTS --> TAG["🏷️ TAG<br/>Named reference<br/>(annotated tags only)"]
    
    style BLOB fill:#74c0fc
    style TREE fill:#51cf66
    style COMMIT fill:#ff922b
    style TAG fill:#e599f7
```

### 2.1 Blob Object — File Content

A blob stores **raw file content** — nothing else. No filename, no permissions.

```mermaid
graph LR
    FILE["README.md<br/>(on disk)"] -->|"git add"| BLOB["BLOB Object<br/>SHA: abc123<br/>Content: '# Hello...'"]
    
    NOTE["Two files with identical<br/>content share ONE blob"]
    
    style BLOB fill:#74c0fc
    style NOTE fill:#ffd43b
```

```bash
# Create a blob
echo "Hello World" | git hash-object -w --stdin
# Returns SHA: 557db03...

# Read a blob
git cat-file -p 557db03
# Output: Hello World

git cat-file -t 557db03
# Output: blob

git cat-file -s 557db03
# Output: 12 (bytes)
```

### 2.2 Tree Object — Directory Structure

A tree maps **filenames to blobs** (and subdirectories to sub-trees):

```mermaid
graph TD
    TREE["TREE (root)<br/>SHA: def456"]
    TREE --> |"100644 README.md"| B1["BLOB<br/>SHA: abc123"]
    TREE --> |"100644 app.js"| B2["BLOB<br/>SHA: ghi789"]
    TREE --> |"040000 src/"| ST["TREE (subtree)<br/>SHA: jkl012"]
    
    ST --> |"100644 utils.js"| B3["BLOB<br/>SHA: mno345"]
    ST --> |"100644 index.js"| B4["BLOB<br/>SHA: pqr678"]
    
    style TREE fill:#51cf66
    style ST fill:#51cf66
    style B1 fill:#74c0fc
    style B2 fill:#74c0fc
    style B3 fill:#74c0fc
    style B4 fill:#74c0fc
```

```bash
# View a tree
git ls-tree HEAD
# 100644 blob abc123    README.md
# 100644 blob ghi789    app.js
# 040000 tree jkl012    src

# Recursive listing
git ls-tree -r HEAD
```

#### File Mode Codes

| Mode | Meaning |
|------|---------|
| `100644` | Regular file (not executable) |
| `100755` | Executable file |
| `120000` | Symbolic link |
| `040000` | Subdirectory (tree) |
| `160000` | Submodule (gitlink) |

### 2.3 Commit Object — The Snapshot

A commit ties everything together:

```mermaid
graph TD
    COMMIT["COMMIT Object<br/>SHA: stu901"]
    COMMIT --> TREE["tree def456<br/>(root tree of this snapshot)"]
    COMMIT --> PARENT["parent xyz789<br/>(previous commit)"]
    COMMIT --> AUTHOR["author Parth &lt;parth@email.com&gt;<br/>1707900000 +0530"]
    COMMIT --> COMMITTER["committer Parth &lt;parth@email.com&gt;<br/>1707900000 +0530"]
    COMMIT --> MSG["message: feat: add login feature"]
    
    style COMMIT fill:#ff922b
    style TREE fill:#51cf66
    style PARENT fill:#74c0fc
```

```bash
# View commit object
git cat-file -p HEAD
# tree def456789...
# parent abc123456...
# author Parth <parth@email.com> 1707900000 +0530
# committer Parth <parth@email.com> 1707900000 +0530
#
# feat: add login feature
```

### 2.4 Complete Object Graph

```mermaid
graph TD
    C2["COMMIT C2<br/>SHA: fff111"] --> T2["TREE<br/>SHA: eee222"]
    C2 --> C1["COMMIT C1<br/>SHA: ddd333"]
    
    C1 --> T1["TREE<br/>SHA: ccc444"]
    
    T2 --> B_README2["BLOB: README.md v2<br/>SHA: bbb555"]
    T2 --> B_APP["BLOB: app.js<br/>SHA: aaa666"]
    T2 --> ST_SRC["TREE: src/<br/>SHA: 999777"]
    
    T1 --> B_README1["BLOB: README.md v1<br/>SHA: 888000"]
    T1 --> B_APP2["BLOB: app.js<br/>SHA: aaa666"]
    
    ST_SRC --> B_UTILS["BLOB: utils.js<br/>SHA: 777111"]
    
    MAIN["main"] -.-> C2
    HEAD["HEAD"] -.-> MAIN
    
    style C1 fill:#ff922b
    style C2 fill:#ff922b
    style T1 fill:#51cf66
    style T2 fill:#51cf66
    style ST_SRC fill:#51cf66
    style B_APP fill:#74c0fc
    style B_APP2 fill:#74c0fc
    style MAIN fill:#e599f7
    style HEAD fill:#ffd43b
```

> Notice: `app.js` (SHA: aaa666) appears in BOTH trees because it didn't change — Git reuses the same blob object!

---

## 3. The Directed Acyclic Graph (DAG)

Git's commit history forms a DAG — a graph where commits point to their parents, and no cycles exist.

```mermaid
graph RL
    C1["C1 (root)"]
    C2["C2"] --> C1
    C3["C3"] --> C2
    C4["C4"] --> C2
    C5["C5 (merge)"] --> C3
    C5 --> C4
    C6["C6"] --> C5
    
    MAIN["main"] -.-> C6
    
    style C1 fill:#ffd43b
    style C5 fill:#e599f7
    style C6 fill:#51cf66
```

### Properties of Git's DAG

```mermaid
graph TD
    DAG["DAG Properties"]
    DAG --> DIRECTED["Directed: Commits point<br/>to parents (backward)"]
    DAG --> ACYCLIC["Acyclic: No commit can<br/>be its own ancestor"]
    DAG --> ROOT["Has root: First commit<br/>has no parent"]
    DAG --> MULTI["Multiple parents:<br/>Merge commits"]
    DAG --> MULTIPLE_ROOTS["Multiple roots possible:<br/>Orphan branches"]
    
    style DAG fill:#ff922b
```

---

## 4. SHA-1 Hashing — How It Works

```mermaid
flowchart TD
    A["Object Content"] --> B["Prepend header:<br/>'blob 12\0' + content"]
    B --> C["SHA-1 hash function"]
    C --> D["40-char hex string<br/>557db03de997c86a..."]
    D --> E["Store as:<br/>.git/objects/55/7db03..."]
    
    subgraph "Header Format"
        H["type SPACE size NULL content"]
        H1["blob 12\0Hello World\n"]
    end
    
    style D fill:#ffd43b
    style E fill:#51cf66
```

### How Git Computes a Blob Hash

```bash
# What Git actually hashes (for "Hello World\n"):
# "blob 12\0Hello World\n"
#  ↑     ↑  ↑
#  type  size null-byte + content

# Verify manually:
printf "blob 12\0Hello World\n" | sha1sum
# Output: 557db03de997c86a4a028e1ebd3a1ceb225be238
```

---

## 5. Packfiles — How Git Optimizes Storage

Over time, Git packs loose objects into efficient packfiles:

```mermaid
flowchart TD
    A["Loose Objects<br/>.git/objects/ab/c123...<br/>.git/objects/de/f456..."] 
    -->|"git gc<br/>(garbage collection)"| 
    B["Packfile<br/>.git/objects/pack/pack-abc.pack<br/>.git/objects/pack/pack-abc.idx"]
    
    B --> C["Delta compression:<br/>Similar objects stored as diffs<br/>of each other"]
    B --> D["Index file (.idx):<br/>Maps SHA → offset in packfile<br/>for fast lookup"]
    
    style A fill:#ff922b
    style B fill:#51cf66
```

```bash
# View object statistics
git count-objects -v

# Trigger garbage collection
git gc

# Verify object database integrity
git fsck
```

---

## 6. Plumbing vs Porcelain Commands

```mermaid
graph TD
    subgraph "Porcelain (User-Friendly)"
        P1["git add"]
        P2["git commit"]
        P3["git log"]
        P4["git branch"]
        P5["git merge"]
    end
    
    subgraph "Plumbing (Low-Level)"
        PL1["git hash-object"]
        PL2["git cat-file"]
        PL3["git ls-tree"]
        PL4["git update-ref"]
        PL5["git write-tree"]
    end
    
    P1 -->|"uses"| PL1
    P2 -->|"uses"| PL5
    P3 -->|"uses"| PL2
    P4 -->|"uses"| PL4
    
    style P1 fill:#51cf66
    style P2 fill:#51cf66
    style P3 fill:#51cf66
    style PL1 fill:#74c0fc
    style PL2 fill:#74c0fc
    style PL3 fill:#74c0fc
```

### Essential Plumbing Commands

```bash
# READING objects
git cat-file -t <SHA>      # Object type
git cat-file -p <SHA>      # Object content (pretty print)
git cat-file -s <SHA>      # Object size
git ls-tree <tree-SHA>     # List tree contents

# WRITING objects
git hash-object -w <file>  # Write blob to object database
git write-tree             # Write staging area as tree object
git commit-tree <tree>     # Create commit from tree

# REFERENCES
git rev-parse HEAD         # Resolve ref to SHA
git update-ref refs/heads/main <SHA>  # Update branch pointer
git symbolic-ref HEAD      # What does HEAD point to?
```

---

## 🏋️ Exercises

1. Use `git cat-file -p HEAD` to explore your latest commit's tree structure
2. Follow the chain: commit → tree → blob manually using `cat-file`
3. Create a blob manually: `echo "test" | git hash-object -w --stdin`
4. Verify that two files with identical content share the same blob SHA
5. Run `git count-objects -v` and then `git gc`, compare results
6. Draw the complete object graph of your last 3 commits

---

## 🔑 Key Takeaways

1. Git stores everything as **objects** (blob, tree, commit, tag) addressed by SHA-1 hash
2. **Blobs** store content (no filename); **Trees** map names to blobs; **Commits** point to trees
3. Same content = same hash = same blob (deduplication built-in)
4. Git's history is a **DAG** — directed acyclic graph of commits
5. **Packfiles** compress objects using delta encoding for storage efficiency
6. Porcelain = user-friendly commands; Plumbing = low-level building blocks

---

**[← Module 12](../12-Git-Diff-Blame-and-Bisect/README.md)** | **[Module 14 →](../14-Reset-Revert-and-Reflog/README.md)**

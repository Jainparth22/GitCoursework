# Module 07: Remote Repositories & GitHub Basics

> **Level**: 🟡 Intermediate | **Time**: 3 hours | **Prerequisites**: [Module 06](../06-Merging-and-Conflict-Resolution/README.md)

---

## 📋 Learning Objectives

- Understand how remote repositories work internally
- Configure and manage remotes
- Master HTTPS vs SSH protocols
- Set up repositories on GitHub
- Understand the forking workflow

---

## 1. What Is a Remote Repository?

A remote is a **reference to another copy** of your repository, usually hosted on a server.

```mermaid
graph TB
    subgraph "Your Machine"
        LOCAL[(Local Repository<br/>Full history)]
        WD[Working Directory]
        WD --- LOCAL
    end
    
    subgraph "GitHub Server"
        REMOTE[(Remote Repository<br/>Full history)]
    end
    
    LOCAL <-->|"push / pull / fetch"| REMOTE
    
    style LOCAL fill:#74c0fc,stroke:#333
    style REMOTE fill:#51cf66,stroke:#333
```

### How Remotes Are Stored

```mermaid
graph TD
    subgraph ".git/config"
        RC["[remote &quot;origin&quot;]<br/>url = git@github.com:user/repo.git<br/>fetch = +refs/heads/*:refs/remotes/origin/*"]
    end
    
    subgraph ".git/refs/remotes/origin/"
        RM["main → commit SHA<br/>develop → commit SHA<br/>feature/x → commit SHA"]
    end
    
    RC -->|"defines"| RM
    
    style RC fill:#ffd43b
    style RM fill:#74c0fc
```

---

## 2. Remote Protocols — How Data Travels

```mermaid
flowchart TD
    A["Connecting to Remote"] --> B{Protocol?}
    B -->|HTTPS| C["https://github.com/user/repo.git"]
    B -->|SSH| D["git@github.com:user/repo.git"]
    
    C --> C1["Authentication:<br/>Username + Token/Password"]
    C --> C2["Port: 443"]
    C --> C3["Works through firewalls ✅"]
    C --> C4["Slower for frequent ops"]
    
    D --> D1["Authentication:<br/>SSH Key Pair 🔑"]
    D --> D2["Port: 22"]
    D --> D3["May be blocked by firewalls ⚠️"]
    D --> D4["Faster, no repeated auth ✅"]
    
    style C fill:#74c0fc
    style D fill:#51cf66
```

### How SSH vs HTTPS Authentication Differs

```mermaid
sequenceDiagram
    participant Dev as Developer
    participant Git as Git Client
    participant GH as GitHub
    
    Note over Dev,GH: HTTPS Authentication
    Dev->>Git: git push (HTTPS)
    Git->>GH: Request + username + token
    GH-->>Git: Verified ✅ or Denied ❌
    
    Note over Dev,GH: SSH Authentication
    Dev->>Git: git push (SSH)
    Git->>GH: SSH handshake with public key
    GH->>GH: Verify against stored public keys
    GH-->>Git: Verified ✅ or Denied ❌
    
    Note over Dev: SSH: No credentials asked each time
```

---

## 3. Managing Remotes

```bash
# List remotes
git remote -v
# origin  git@github.com:user/repo.git (fetch)
# origin  git@github.com:user/repo.git (push)

# Add a remote
git remote add origin git@github.com:user/repo.git
git remote add upstream git@github.com:original/repo.git

# Remove a remote
git remote remove upstream

# Rename a remote
git remote rename origin github

# Change URL
git remote set-url origin git@github.com:user/new-repo.git

# Show detailed remote info
git remote show origin
```

### Multiple Remotes — Fork Workflow

```mermaid
graph TB
    subgraph "GitHub"
        UPSTREAM[(upstream<br/>Original Repo)]
        ORIGIN[(origin<br/>Your Fork)]
    end
    
    subgraph "Your Machine"
        LOCAL[(Local Clone)]
    end
    
    LOCAL -->|"git push origin"| ORIGIN
    LOCAL -->|"git fetch upstream"| UPSTREAM
    ORIGIN -->|"Pull Request"| UPSTREAM
    UPSTREAM -.->|"Fork"| ORIGIN
    
    style UPSTREAM fill:#ff922b,stroke:#333
    style ORIGIN fill:#51cf66,stroke:#333
    style LOCAL fill:#74c0fc,stroke:#333
```

---

## 4. Remote Tracking Branches

Remote tracking branches are **read-only local references** to the state of branches on the remote:

```mermaid
graph RL
    subgraph "Local Repository"
        MAIN["main<br/>(local branch)"] -.-> C4["C4 (your work)"]
        C4 --> C3
        OM["origin/main<br/>(remote tracking)"] -.-> C3["C3"]
        C3 --> C2["C2"] --> C1["C1"]
    end
    
    subgraph "Remote (origin)"
        RM["main"] -.-> RC3["C3"]
        RC3 --> RC2["C2"] --> RC1["C1"]
    end
    
    style MAIN fill:#51cf66
    style OM fill:#ff922b
    style RM fill:#51cf66
```

```bash
# View remote tracking branches
git branch -r
# origin/main
# origin/develop
# origin/feature/auth

# View all branches (local + remote tracking)
git branch -a

# See tracking relationship
git branch -vv
# * main    abc1234 [origin/main: ahead 1] Your commit message
```

---

## 5. Creating a GitHub Repository

### Via GitHub Web UI

```mermaid
flowchart TD
    A["github.com → New Repository"] --> B["Fill in:<br/>Name, Description, Visibility"]
    B --> C{"Initialize with README?"}
    C -->|"Yes"| D["Creates repo with initial commit<br/>Clone to work locally"]
    C -->|"No"| E["Empty repo<br/>Push from existing local repo"]
    
    D --> F["git clone URL"]
    E --> G["git remote add origin URL<br/>git push -u origin main"]
    
    style D fill:#51cf66
    style E fill:#74c0fc
```

### Connecting Local Repo to GitHub

```bash
# 1. Create repo on GitHub (empty, no README)

# 2. In your local project:
git remote add origin git@github.com:user/repo.git

# 3. Push with tracking
git push -u origin main
# -u sets up tracking: local main → origin/main
```

### How `git push -u` Works Internally

```mermaid
sequenceDiagram
    participant Local as Local Git
    participant Config as .git/config
    participant Remote as GitHub
    
    Local->>Remote: Push commits to origin/main
    Remote-->>Local: Received ✅
    
    Local->>Config: Set tracking info:<br/>[branch "main"]<br/>remote = origin<br/>merge = refs/heads/main
    
    Note over Local: Now "git push" and "git pull"<br/>automatically know where to go
```

---

## 6. Forking — How It Works

```mermaid
flowchart TD
    A["Original Repo<br/>github.com/original/project"] -->|"Fork (GitHub copy)"| B["Your Fork<br/>github.com/you/project"]
    B -->|"git clone"| C["Local Clone"]
    C -->|"git remote add upstream"| A
    
    C -->|"Make changes<br/>git push origin"| B
    B -->|"Pull Request"| A
    
    A -->|"git fetch upstream"| C
    
    style A fill:#ff922b,stroke:#333
    style B fill:#51cf66,stroke:#333
    style C fill:#74c0fc,stroke:#333
```

### Fork vs Clone

| Feature | Fork | Clone |
|---------|------|-------|
| Where | On GitHub (server-side copy) | On your machine (local copy) |
| Created by | GitHub's Fork button | `git clone` command |
| Purpose | Contribute to others' repos | Work on any repo locally |
| Linked to original | Yes (can create PRs) | No (just a copy) |

---

## 🏋️ Exercises

1. Create a GitHub repo and link it to a local project
2. Set up both `origin` and `upstream` remotes
3. Practice switching between HTTPS and SSH: `git remote set-url`
4. View remote tracking branches with `git branch -r` and `git branch -vv`
5. Fork a public repo and clone your fork locally

---

## 🔑 Key Takeaways

1. A remote is a **reference** to another repository (stored in `.git/config`)
2. SSH is faster and more secure; HTTPS works through firewalls
3. `origin` = your fork/primary remote; `upstream` = the original repo
4. Remote tracking branches (`origin/main`) are read-only local snapshots of remote state
5. `git push -u` sets up tracking so future push/pull knows where to go
6. Fork = server-side copy on GitHub; Clone = local copy on your machine

---

**[← Module 06](../06-Merging-and-Conflict-Resolution/README.md)** | **[Module 08 →](../08-Collaboration-Push-Pull-Fetch/README.md)**

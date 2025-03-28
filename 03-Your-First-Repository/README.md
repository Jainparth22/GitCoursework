# Module 03: Your First Repository

> **Level**: 🟢 Beginner | **Time**: 2.5 hours | **Prerequisites**: [Module 02](../02-Git-Installation-and-Configuration/README.md)

---

## 📋 Learning Objectives

- Create and clone repositories
- Understand the `.git` directory internals
- Master file states in Git
- Configure `.gitignore` for clean repositories

---

## 1. Creating a Repository — `git init`

### What Happens Internally

```mermaid
sequenceDiagram
    participant You as Developer
    participant FS as File System
    participant Git as Git Engine
    
    You->>FS: mkdir my-project && cd my-project
    Note over FS: Empty directory created
    
    You->>Git: git init
    Git->>FS: Create .git/ directory structure
    Note over FS: .git/objects/ → Object database<br/>.git/refs/ → Branch pointers<br/>.git/HEAD → Current branch<br/>.git/config → Local config
    
    Git-->>You: Initialized empty Git repository ✅
    Note over You: Directory is now a Git repo!
```

```bash
# Create a new project
mkdir my-project
cd my-project

# Initialize Git
git init
# Output: Initialized empty Git repository in /path/.git/
```

---

## 2. Anatomy of the `.git` Directory

When you run `git init`, Git creates this structure:

```mermaid
graph TD
    ROOT[".git/"] --> HEAD["HEAD<br/>Points to current branch<br/>(ref: refs/heads/main)"]
    ROOT --> CONFIG["config<br/>Local repository config"]
    ROOT --> DESC["description<br/>GitWeb description"]
    ROOT --> OBJECTS["objects/<br/>Object database<br/>(blobs, trees, commits)"]
    ROOT --> REFS["refs/<br/>References (pointers)"]
    ROOT --> HOOKS["hooks/<br/>Script hooks"]
    ROOT --> INDEX["index<br/>Staging area file"]
    ROOT --> INFO["info/<br/>Auxiliary info"]
    
    OBJECTS --> OBJ_INFO["info/"]
    OBJECTS --> OBJ_PACK["pack/<br/>Packfiles"]
    
    REFS --> HEADS["heads/<br/>Branch pointers<br/>(main, feature/x)"]
    REFS --> TAGS["tags/<br/>Tag pointers<br/>(v1.0, v2.0)"]
    REFS --> REMOTES["remotes/<br/>Remote branch pointers<br/>(origin/main)"]
    
    style ROOT fill:#ffd43b
    style HEAD fill:#ff922b
    style OBJECTS fill:#51cf66
    style REFS fill:#74c0fc
    style INDEX fill:#e599f7
```

### What Each Component Does

| File/Directory | Purpose | Importance |
|---------------|---------|------------|
| `HEAD` | Points to current branch | Essential — tells Git where you are |
| `objects/` | Stores all data (blobs, trees, commits) | Essential — the actual database |
| `refs/` | Stores branch and tag pointers | Essential — how Git finds commits |
| `index` | The staging area | Essential — tracks what's staged |
| `config` | Local configuration | Important — repo-specific settings |
| `hooks/` | Event scripts | Optional — automation |
| `info/` | Auxiliary information | Rarely used |
| `description` | Used by GitWeb | Rarely used |

---

## 3. Cloning a Repository — `git clone`

### How Clone Works Internally

```mermaid
sequenceDiagram
    participant You as Developer
    participant Git as Git Client
    participant Remote as GitHub Server
    
    You->>Git: git clone https://github.com/user/repo.git
    
    Git->>Remote: 1. Request repository info
    Remote-->>Git: 2. Send packfile with all objects
    
    Git->>Git: 3. Create directory "repo/"
    Git->>Git: 4. Create .git/ with all objects
    Git->>Git: 5. Set up "origin" remote
    Git->>Git: 6. Create local "main" branch
    Git->>Git: 7. Checkout files to working directory
    
    Git-->>You: Repository cloned ✅
    
    Note over You: You now have:<br/>- Full history<br/>- All branches (as remote refs)<br/>- Working directory with latest files
```

```bash
# Clone via HTTPS
git clone https://github.com/user/repo.git

# Clone via SSH (recommended)
git clone git@github.com:user/repo.git

# Clone to a specific directory
git clone git@github.com:user/repo.git my-folder

# Shallow clone (only latest commit)
git clone --depth 1 git@github.com:user/repo.git
```

### What `git clone` Actually Does (Equivalent Commands)

```mermaid
flowchart TD
    CLONE["git clone URL"] --> A["mkdir repo && cd repo"]
    A --> B["git init"]
    B --> C["git remote add origin URL"]
    C --> D["git fetch origin"]
    D --> E["git checkout main"]
    
    style CLONE fill:#74c0fc
    style E fill:#51cf66
```

---

## 4. File States in Git

Every file in a Git repository is in one of these states:

```mermaid
stateDiagram-v2
    [*] --> Untracked: Create new file
    
    Untracked --> Staged: git add
    Staged --> Unmodified: git commit
    Unmodified --> Modified: Edit file
    Modified --> Staged: git add
    Unmodified --> Untracked: git rm
    Modified --> Unmodified: git restore
    Staged --> Modified: Edit after staging
    
    state "📄 Untracked" as Untracked
    state "📋 Staged" as Staged  
    state "✅ Unmodified (Committed)" as Unmodified
    state "✏️ Modified" as Modified
```

### Detailed State Definitions

| State | Description | `git status` shows |
|-------|------------|-------------------|
| **Untracked** | New file, Git doesn't know about it | `Untracked files:` (red) |
| **Staged** | Marked to go in next commit | `Changes to be committed:` (green) |
| **Unmodified** | Committed, no changes since | Not shown (clean) |
| **Modified** | Changed since last commit, not staged | `Changes not staged:` (red) |

### How File State Transitions Work

```mermaid
flowchart LR
    subgraph "Working Directory"
        NEW["📄 New File<br/>(Untracked)"]
        MOD["✏️ Modified File"]
    end
    
    subgraph "Staging Area"
        STAGED["📋 Staged Changes"]
    end
    
    subgraph "Repository"
        COMMITTED["✅ Committed Snapshot"]
    end
    
    NEW -->|"git add"| STAGED
    MOD -->|"git add"| STAGED
    STAGED -->|"git commit"| COMMITTED
    COMMITTED -->|"Edit file"| MOD
    STAGED -->|"git restore --staged"| MOD
    MOD -->|"git restore"| COMMITTED
    
    style NEW fill:#ff6b6b
    style MOD fill:#ffd43b
    style STAGED fill:#74c0fc
    style COMMITTED fill:#51cf66
```

---

## 5. `.gitignore` — Excluding Files

### Why Ignore Files?

```mermaid
graph TB
    subgraph "Should Track ✅"
        A["Source code (.js, .py)"]
        B["Config files"]
        C["Documentation"]
        D["Test files"]
    end
    
    subgraph "Should Ignore ❌"
        E["node_modules/"]
        F["Build output (dist/)"]
        G["Secrets (.env)"]
        H["OS files (.DS_Store)"]
        I["IDE files (.vscode/)"]
        J["Compiled files (.pyc)"]
    end
    
    style A fill:#51cf66
    style B fill:#51cf66
    style C fill:#51cf66
    style D fill:#51cf66
    style E fill:#ff6b6b
    style F fill:#ff6b6b
    style G fill:#ff6b6b
    style H fill:#ff6b6b
    style I fill:#ff6b6b
    style J fill:#ff6b6b
```

### How `.gitignore` Works Internally

```mermaid
flowchart TD
    A["git add / git status"] --> B{"File matches<br/>.gitignore pattern?"}
    B -->|"Yes"| C["File is IGNORED<br/>Not tracked, not shown"]
    B -->|"No"| D{"Already tracked<br/>by Git?"}
    D -->|"Yes"| E["File IS tracked<br/>(ignore doesn't apply!)"]
    D -->|"No"| F["File shown as<br/>Untracked"]
    
    style C fill:#ff6b6b
    style E fill:#ffd43b
    style F fill:#74c0fc
```

> **Important**: `.gitignore` only works for **untracked** files. If a file is already tracked, you must `git rm --cached file` first.

### Pattern Syntax

```bash
# .gitignore

# Ignore specific files
secrets.env
passwords.txt

# Ignore by extension
*.log
*.pyc
*.class
*.o

# Ignore directories
node_modules/
dist/
build/
__pycache__/
.venv/

# Ignore pattern in any directory
**/temp/

# Negate (DO track this even though *.log is ignored)
!important.log

# Ignore files in root only
/TODO.md

# Wildcard: any single character
file?.txt    # file1.txt, fileA.txt

# Wildcard: directory depth
docs/**/*.pdf
```

### Gitignore Processing Order

```mermaid
flowchart TD
    A["Check .gitignore patterns"] --> B["Process top to bottom"]
    B --> C["Later patterns override earlier ones"]
    C --> D{"Pattern starts with !"}
    D -->|"Yes"| E["NEGATE: re-include the file"]
    D -->|"No"| F["IGNORE: exclude the file"]
    
    G["Multiple .gitignore files"] --> H["repo root/.gitignore"]
    H --> I["subdirectory/.gitignore"]
    I --> J["Later/deeper files take precedence"]
```

### Common `.gitignore` Templates

```bash
# Node.js
node_modules/
dist/
.env
*.log
.DS_Store

# Python
__pycache__/
*.pyc
.venv/
.env
*.egg-info/
dist/
build/

# Java
*.class
*.jar
target/
.idea/
```

### Global `.gitignore` (System-wide)

```bash
# Create global ignore
git config --global core.excludesfile ~/.gitignore_global

# Add common OS/editor files
echo ".DS_Store" >> ~/.gitignore_global
echo "Thumbs.db" >> ~/.gitignore_global
echo ".vscode/" >> ~/.gitignore_global
echo "*.swp" >> ~/.gitignore_global
```

---

## 6. Bare Repositories

```mermaid
graph LR
    subgraph "Regular Repository"
        RR_WD["Working Directory<br/>(editable files)"]
        RR_GIT[".git/<br/>(history)"]
    end
    
    subgraph "Bare Repository"
        BR_GIT["Repository contents<br/>directly (no working dir)"]
    end
    
    RR_WD -->|"Used by"| DEV["Developers"]
    BR_GIT -->|"Used by"| SRV["Servers<br/>(GitHub, GitLab)"]
    
    style RR_WD fill:#51cf66
    style RR_GIT fill:#74c0fc
    style BR_GIT fill:#ffd43b
    style DEV fill:#e599f7
    style SRV fill:#e599f7
```

```bash
# Create bare repo (server-side)
git init --bare project.git

# Clone from bare
git clone /path/to/project.git
```

---

## 🏋️ Exercises

1. Create a new repo with `git init` and explore the `.git/` directory
2. Clone a public GitHub repo and compare its `.git/` structure
3. Create files, modify them, and observe state changes with `git status`
4. Write a `.gitignore` for a Node.js project
5. Try to ignore an already-tracked file — see what happens, then use `git rm --cached`

---

## 🔑 Key Takeaways

1. `git init` creates the `.git/` directory — the heart of your repository
2. `git clone` gives you a **full copy** including all history
3. Files move through states: Untracked → Staged → Committed → Modified
4. `.gitignore` prevents unnecessary files from being tracked
5. `.gitignore` only affects **untracked** files — already tracked files need `git rm --cached`
6. Bare repositories are for servers — no working directory

---

**[← Module 02](../02-Git-Installation-and-Configuration/README.md)** | **[Module 04 →](../04-Staging-Committing-and-History/README.md)**

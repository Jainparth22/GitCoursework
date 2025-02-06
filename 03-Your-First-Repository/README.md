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

> *[Visual Diagram: Architecture & Workflow]*

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

> *[Visual Diagram: Architecture & Workflow]*

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

> *[Visual Diagram: Architecture & Workflow]*

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

> *[Visual Diagram: Architecture & Workflow]*

---


---

> *Note: Practical exercises and advanced topics currently being drafted.*

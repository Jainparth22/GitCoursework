# Module 02: Git Installation & Configuration

> **Level**: 🟢 Beginner | **Time**: 2 hours | **Prerequisites**: [Module 01](../01-Introduction-to-Version-Control/README.md)

---

## 📋 Learning Objectives

- Install Git on Windows, macOS, and Linux
- Understand the Git configuration hierarchy
- Set up essential configuration for development
- Configure SSH keys for GitHub authentication
- Set up GPG signing for verified commits

---

## 1. Installing Git

### How the Installation Process Works

> *[Visual Diagram: Architecture & Workflow]*

### Windows

**Option A: Git for Windows (Recommended)**

1. Download from [https://git-scm.com/download/win](https://git-scm.com/download/win)
2. Run the installer with these recommended settings:

> *[Visual Diagram: Architecture & Workflow]*

3. Verify:
```bash
git --version
# Output: git version 2.x.x.windows.x
```

**Option B: winget (Windows 11)**

```bash
winget install --id Git.Git -e --source winget
```

### macOS

**Option A: Xcode Command Line Tools**

```bash
xcode-select --install
git --version
```

**Option B: Homebrew**

```bash
brew install git
git --version
```

### Linux

**Ubuntu / Debian:**
```bash
sudo apt update && sudo apt install git
```

**Fedora:**
```bash
sudo dnf install git
```

**Arch Linux:**
```bash
sudo pacman -S git
```

---

## 2. Git Configuration System — How It Works Internally

Git stores configuration at **three levels**, each overriding the previous:

> *[Visual Diagram: Architecture & Workflow]*

### How Git Resolves Configuration

> *[Visual Diagram: Architecture & Workflow]*

### Viewing Configuration

```bash
# View all config with source locations
git config --list --show-origin

# View specific level
git config --local --list
git config --global --list
git config --system --list

# View a specific key
git config user.name
git config user.email
```

### How Config Files Look Internally

```ini
# ~/.gitconfig (Global)
[user]
    name = Parth
    email = parth@example.com
[core]
    editor = code --wait
    autocrlf = true
[init]
    defaultBranch = main
[alias]
    lg = log --oneline --graph --all
```

---

## 3. Essential Configuration — First-Time Setup

> *[Visual Diagram: Architecture & Workflow]*

### Identity (Required)

```bash
# Your name (shown in commits)
git config --global user.name "Your Full Name"

# Your email (must match GitHub account for linking)
git config --global user.email "your.email@example.com"
```

### Default Editor

```bash
# VS Code
git config --global core.editor "code --wait"

# Vim
git config --global core.editor "vim"

# Nano
git config --global core.editor "nano"

# Notepad++ (Windows)
git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession"
```

### Default Branch Name

```bash
git config --global init.defaultBranch main
```

### Color Output

```bash
git config --global color.ui auto
```

---


---

> *Note: Practical exercises and advanced topics currently being drafted.*

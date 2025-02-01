# Git & GitHub — Complete Cheat Sheet 📋

> Quick reference for all Git commands covered in this coursework.

---

## 🔧 Setup & Configuration

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global core.editor "code --wait"
git config --global init.defaultBranch main
git config --global core.autocrlf true      # Windows
git config --global core.autocrlf input     # macOS/Linux
git config --list --show-origin             # Show all config
```

---

## 📁 Repository Basics

```bash
git init                          # Initialize new repo
git clone <url>                   # Clone existing repo
git clone --depth 1 <url>         # Shallow clone
git clone --recurse-submodules <url>  # Clone with submodules
```

---

## 📝 Staging & Committing

```bash
git status                        # Check file states
git status -s                     # Short format

git add <file>                    # Stage specific file
git add .                         # Stage all changes
git add -p                        # Interactive staging (hunks)

git commit -m "message"           # Commit with message
git commit -am "message"          # Stage tracked + commit
git commit --amend                # Modify last commit
git commit --amend --no-edit      # Amend without changing message
git commit -s -m "message"        # Sign-off (DCO)
git commit --fixup=<SHA>          # Create fixup commit
```

---

## 🔍 Viewing & Diffing


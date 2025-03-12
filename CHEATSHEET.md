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

```bash
git log                           # Full log
git log --oneline                 # Compact log
git log --oneline --graph --all   # Visual branch graph
git log -n 5                      # Last 5 commits
git log --author="Name"           # Filter by author
git log --since="2 weeks ago"     # Filter by date
git log -S "searchterm"           # Pickaxe: find string changes
git log -L :function:file.js      # Log for a function

git diff                          # Unstaged changes
git diff --staged                 # Staged changes
git diff HEAD                     # All changes vs last commit
git diff main..feature            # Between branches
git diff --stat                   # Summary only
git diff --word-diff              # Word-level diff

git show <SHA>                    # Show commit details
git blame <file>                  # Who changed each line
git blame -L 10,20 <file>        # Blame specific lines
git shortlog -sne                 # Contribution summary
```

---

## 🌿 Branching

```bash
git branch                        # List local branches
git branch -a                     # List all (local + remote)
git branch <name>                 # Create branch
git branch -d <name>              # Delete (safe)
git branch -D <name>              # Delete (force)
git branch -m <old> <new>         # Rename

git switch <branch>               # Switch branch
git switch -c <name>              # Create + switch
git checkout <branch>             # Switch (legacy)
git checkout -b <name>            # Create + switch (legacy)
```

---

## 🔀 Merging & Rebasing

```bash
# Merging
git merge <branch>                # Merge into current
git merge --no-ff <branch>        # Force merge commit
git merge --squash <branch>       # Squash merge
git merge --abort                 # Cancel merge
git merge -X ours <branch>        # Prefer our side on conflicts
git merge -X theirs <branch>      # Prefer their side on conflicts
git merge -s ours <branch>        # Ignore all their changes

# Rebasing
git rebase <branch>               # Rebase onto branch
git rebase -i HEAD~N              # Interactive rebase (last N)
git rebase -i --autosquash <base> # Auto-arrange fixup commits
git rebase --abort                # Cancel rebase
git rebase --continue             # Continue after fix

# Cherry-picking
git cherry-pick <SHA>             # Apply specific commit
git cherry-pick --no-commit <SHA> # Apply without committing
```

---

## 🌐 Remote Operations

```bash
git remote -v                     # List remotes
git remote add <name> <url>       # Add remote
git remote remove <name>          # Remove remote
git remote set-url <name> <url>   # Change URL

git fetch                         # Download remote changes
git fetch --all                   # Fetch all remotes
git pull                          # Fetch + merge
git pull --rebase                 # Fetch + rebase

git push                          # Push to tracking branch
git push -u origin <branch>       # Push + set tracking
git push origin --delete <branch> # Delete remote branch
git push --tags                   # Push all tags
git push --force-with-lease       # Safe force push
```

---

## 🏷️ Tags & Releases

```bash
git tag                           # List tags
git tag <name>                    # Lightweight tag
git tag -a <name> -m "msg"        # Annotated tag
git tag -a <name> <SHA> -m "msg"  # Tag specific commit
git tag -d <name>                 # Delete local tag
git push origin <tag>             # Push single tag
git push origin --tags            # Push all tags
git push origin --delete <tag>    # Delete remote tag
```

---

## ⏪ Undo & Recovery

```bash
# Unstage
git restore --staged <file>       # Unstage file
git reset HEAD <file>             # Unstage (legacy)

# Discard changes
git restore <file>                # Discard working directory changes
git checkout -- <file>            # Discard (legacy)

# Reset (move branch pointer)
git reset --soft HEAD~1           # Undo commit, keep staged
git reset HEAD~1                  # Undo commit, keep unstaged
git reset --hard HEAD~1           # ⚠️ Undo commit + discard changes

# Safe undo (creates new commit)
git revert <SHA>                  # Revert a specific commit
git revert -m 1 <merge-SHA>      # Revert a merge commit

# Recovery
git reflog                        # View HEAD movement history
git reset --hard HEAD@{n}         # Recover from reflog
```

---

## 📦 Stash & Clean

```bash
git stash                         # Stash changes
git stash push -m "description"   # Stash with message
git stash -u                      # Include untracked files
git stash list                    # List stashes
git stash pop                     # Apply + remove latest
git stash apply stash@{n}        # Apply specific stash
git stash drop stash@{n}         # Delete specific stash
git stash clear                   # Delete all stashes

git clean -n                      # Preview untracked deletions
git clean -fd                     # Remove untracked files + dirs
```

---

## 🐛 Debugging

```bash
git bisect start                  # Start binary search
git bisect bad                    # Mark current as bad
git bisect good <SHA>             # Mark known good
git bisect good / bad             # Keep narrowing
git bisect run <script>           # Automate with test script
git bisect reset                  # End bisect

git grep "pattern"                # Search working tree
git grep -n "pattern"             # With line numbers
```

---

## 🔬 Internals (Plumbing)

```bash
git cat-file -t <SHA>             # Object type
git cat-file -p <SHA>             # Object content
git cat-file -s <SHA>             # Object size
git ls-tree HEAD                  # Tree contents
git ls-tree -r HEAD               # Recursive tree
git hash-object -w <file>         # Create blob object
git rev-parse HEAD                # Resolve ref to SHA
git count-objects -v              # Object statistics
git gc                            # Garbage collection
```

---

## 🔐 GitHub CLI (`gh`)

```bash
# Pull Requests
gh pr create --title "title"      # Create PR
gh pr create --draft              # Draft PR
gh pr list                        # List PRs
gh pr view <n>                    # View PR details
gh pr checkout <n>                # Checkout PR locally
gh pr merge <n> --squash          # Squash merge PR

# Issues
gh issue create                   # Create issue
gh issue list                     # List issues
gh issue close <n>                # Close issue

# Releases
gh release create <tag>           # Create release
gh release create <tag> --generate-notes  # Auto-notes
gh release list                   # List releases

# Repo
gh repo create <name>             # Create repository
gh repo clone <owner/repo>       # Clone repo
gh repo view                      # View repo info
```

---

## 🛡️ LFS & Submodules

```bash
# Git LFS
git lfs install                   # Initialize LFS

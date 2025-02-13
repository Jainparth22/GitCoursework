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

> *[Visual Diagram: Architecture & Workflow]*

### How Remotes Are Stored

> *[Visual Diagram: Architecture & Workflow]*

---

## 2. Remote Protocols — How Data Travels

> *[Visual Diagram: Architecture & Workflow]*

### How SSH vs HTTPS Authentication Differs

> *[Visual Diagram: Architecture & Workflow]*

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

> *[Visual Diagram: Architecture & Workflow]*

---


---

> *Note: Practical exercises and advanced topics currently being drafted.*

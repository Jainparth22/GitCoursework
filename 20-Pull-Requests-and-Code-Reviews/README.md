# Module 20: Pull Requests & Code Reviews

> **Level**: 🟣 GitHub Deep-Dive | **Time**: 3 hours | **Prerequisites**: [Module 19](../19-GitHub-Issues-Projects-and-Wikis/README.md)

---

## 📋 Learning Objectives

- Create effective pull requests
- Conduct thorough code reviews
- Understand PR merge strategies in depth
- Use PR automation features

---

## 1. Pull Request Lifecycle

> *[Visual Diagram: Architecture & Workflow]*

---

## 2. PR Merge Strategies — What They Do Internally

> *[Visual Diagram: Architecture & Workflow]*

### Merge Commit

> *[Visual Diagram: Architecture & Workflow]*

**Preserves all commits + branch structure. Full history.**

### Squash and Merge

> *[Visual Diagram: Architecture & Workflow]*

**All PR commits combined into one. Clean linear history.**

### Rebase and Merge

> *[Visual Diagram: Architecture & Workflow]*

**Individual commits preserved, linear history, new SHAs.**

### Strategy Decision

> *[Visual Diagram: Architecture & Workflow]*

---

## 3. Code Review Best Practices

> *[Visual Diagram: Architecture & Workflow]*

### Review Comment Types

| Prefix | Meaning | Example |
|--------|---------|---------|
| `nit:` | Minor style issue | `nit: add trailing comma` |
| `suggestion:` | Take it or leave it | `suggestion: consider using map()` |
| `question:` | Need clarification | `question: why is this async?` |
| `issue:` | Must fix before merge | `issue: SQL injection vulnerability` |
| `praise:` | Positive feedback | `praise: elegant solution!` |

---


---

> *Note: Practical exercises and advanced topics currently being drafted.*

# Module 01: Introduction to Version Control & Git

> **Level**: 🟢 Beginner | **Estimated Time**: 3 hours | **Prerequisites**: None

---

## 📋 Learning Objectives

- Understand what version control is and why it's essential
- Differentiate between centralized and distributed VCS
- Understand Git's internal philosophy and data model
- Master the three core areas of Git

---

## 1. What is Version Control?

**Version Control** (also called **Source Control** or **Revision Control**) is a system that records changes to files over time so you can recall specific versions later.

### Why Do We Need It?

Without version control, teams resort to:

```
project/
├── report_final.docx
├── report_final_v2.docx
├── report_final_v2_FIXED.docx
├── report_final_v2_FIXED_REAL.docx       ← Chaos!
└── report_final_v2_FIXED_REAL_USE_THIS.docx
```

**Version Control solves this by:**

| Problem | Solution |
|---------|----------|
| Losing previous versions | Every change is recorded & retrievable |
| Not knowing who changed what | Every change has an author & timestamp |
| Team members overwriting each other | Concurrent work with merge capabilities |
| No audit trail | Complete history of every modification |
| Difficulty rolling back errors | Revert to any previous state instantly |

---

## 2. Types of Version Control Systems

### 2.1 Local Version Control

The simplest form — a local database tracking file changes.

> *[Visual Diagram: Architecture & Workflow]*

**Example**: RCS (Revision Control System) — stores patch sets on disk.

**Limitation**: No collaboration — single machine only.

### 2.2 Centralized Version Control (CVCS)

A single central server holds all versioned files. Clients check out files from that central place.

> *[Visual Diagram: Architecture & Workflow]*

**Examples**: SVN (Subversion), CVS, Perforce

**Advantages**:
- Everyone knows what everyone else is doing
- Admins have fine-grained control over permissions

**Disadvantages**:
- **Single point of failure** — if server goes down, nobody can collaborate
- If server disk is corrupted without backup, entire history is lost
- Requires network connection for most operations

### 2.3 Distributed Version Control (DVCS)

Every developer has a **full copy** of the repository including its complete history.

> *[Visual Diagram: Architecture & Workflow]*

**Examples**: Git, Mercurial, Bazaar

**Advantages**:
- **No single point of failure** — every clone is a full backup
- Work offline — commit, branch, view history without a network
- Faster operations — most happen locally
- Flexible workflows — peer-to-peer, centralized, or hybrid

---

## 3. CVCS vs DVCS — Deep Comparison

> *[Visual Diagram: Architecture & Workflow]*

| Feature | CVCS (SVN) | DVCS (Git) |
|---------|-----------|-----------|
| Full history locally | ❌ | ✅ |
| Work offline | ❌ | ✅ |
| Speed | Slow (network) | Fast (local) |
| Branching | Expensive | Cheap & fast |
| Single point of failure | ✅ Central server | ❌ Distributed |
| Disk space per developer | Low | Higher (full repo) |

---


---

> *Note: Practical exercises and advanced topics currently being drafted.*

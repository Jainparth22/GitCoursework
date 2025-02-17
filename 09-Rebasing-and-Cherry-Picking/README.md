# Module 09: Rebasing & Cherry-Picking

> **Level**: 🟡 Intermediate | **Time**: 3.5 hours | **Prerequisites**: [Module 08](../08-Collaboration-Push-Pull-Fetch/README.md)

---

## 📋 Learning Objectives

- Understand how rebase works at the object level
- Know when to rebase vs merge
- Use cherry-pick to apply specific commits
- Follow the golden rule of rebasing

---

## 1. What Is Rebase? — The Internal Mechanics

Rebase **re-applies** your commits on top of a different base commit, creating **new commits** with new SHAs.

### Before vs After

> *[Visual Diagram: Architecture & Workflow]*

> *[Visual Diagram: Architecture & Workflow]*

> The `'` marks indicate these are **new commits** — same changes but different parent, so different SHA.

### Step-by-Step: What Git Does Internally

> *[Visual Diagram: Architecture & Workflow]*

### Rebase Algorithm Flowchart

> *[Visual Diagram: Architecture & Workflow]*

---

## 2. Merge vs Rebase — Visual Comparison

### Merge: Preserves True History

> *[Visual Diagram: Architecture & Workflow]*

### Rebase: Creates Linear History

> *[Visual Diagram: Architecture & Workflow]*

### Decision Flowchart

> *[Visual Diagram: Architecture & Workflow]*

---

## 3. The Golden Rule of Rebasing

> **🚨 NEVER rebase commits that have been pushed to a shared branch.**

> *[Visual Diagram: Architecture & Workflow]*

### Why?

> *[Visual Diagram: Architecture & Workflow]*

---


---

> *Note: Practical exercises and advanced topics currently being drafted.*

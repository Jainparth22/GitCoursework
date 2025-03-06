# Module 18: Advanced Merge Strategies

> **Level**: 🔵 Advanced | **Time**: 2.5 hours | **Prerequisites**: [Module 17](../17-Submodules-and-Subtrees/README.md)

---

## 📋 Learning Objectives

- Understand all Git merge strategies
- Use `git rerere` for automatic conflict resolution
- Handle complex merge scenarios
- Choose the right merge strategy for each situation

---

## 1. Merge Strategies Overview

> *[Visual Diagram: Architecture & Workflow]*

### Strategy Comparison

| Strategy | When Used | Commits | Handles Renames |
|----------|----------|---------|----------------|
| Fast-forward | Linear history | No merge commit | N/A |
| ort (default) | 2 diverged branches | Merge commit | ✅ |
| Octopus | 3+ branches | Merge commit | Limited |
| ours | Discard their changes | Merge commit | N/A |
| subtree | Subtree merges | Merge commit | ✅ |

---

## 2. The `ort` Strategy (Default) — Deep Dive

> *[Visual Diagram: Architecture & Workflow]*

### `-X ours` vs `-X theirs` (Strategy Options)

> *[Visual Diagram: Architecture & Workflow]*

```bash
# For conflicts, prefer our changes
git merge -X ours feature

# For conflicts, prefer their changes
git merge -X theirs feature
```

### `--strategy=ours` (Ignore All Their Changes)

> *[Visual Diagram: Architecture & Workflow]*

> *[Visual Diagram: Architecture & Workflow]*

> **All of feature's changes are discarded.** The merge commit's tree is identical to ours.

---

## 3. `git rerere` — Reuse Recorded Resolution

### How rerere Works

> *[Visual Diagram: Architecture & Workflow]*

```bash
# Enable rerere
git config --global rerere.enabled true

# View recorded resolutions
git rerere status

# Forget a specific resolution
git rerere forget path/to/file

# Clear all recorded resolutions
git rerere clear

# Show diff of what rerere would apply
git rerere diff
```

### When rerere Is Most Useful

> *[Visual Diagram: Architecture & Workflow]*

---


---

> *Note: Practical exercises and advanced topics currently being drafted.*

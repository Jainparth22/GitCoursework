# Module 17: Submodules & Subtrees

> **Level**: 🔵 Advanced | **Time**: 3 hours | **Prerequisites**: [Module 16](../16-Git-Hooks-and-Automation/README.md)

---

## 📋 Learning Objectives

- Include external repositories within your project
- Compare submodules vs subtrees
- Manage and update nested repositories
- Handle common submodule pitfalls

---

## 1. Submodules — How They Work Internally

A submodule is a **pointer to a specific commit** in another repository.

> *[Visual Diagram: Architecture & Workflow]*

### What Creates a Submodule

> *[Visual Diagram: Architecture & Workflow]*

```bash
# Add a submodule
git submodule add https://github.com/lib/ui-kit.git libs/ui-kit

# .gitmodules (created/updated)
# [submodule "libs/ui-kit"]
#     path = libs/ui-kit
#     url = https://github.com/lib/ui-kit.git
```

---

## 2. Submodule Operations

### Cloning a Project with Submodules

> *[Visual Diagram: Architecture & Workflow]*

```bash
# Clone with submodules in one step
git clone --recurse-submodules https://github.com/user/project.git

# Or initialize after cloning
git submodule init
git submodule update

# Combined
git submodule update --init --recursive
```

### Update Submodule to Latest

> *[Visual Diagram: Architecture & Workflow]*

```bash
# Update all submodules to their latest
git submodule update --remote

# Update specific submodule
git submodule update --remote libs/ui-kit
```

---

## 3. Subtrees — The Alternative

Subtrees **merge** another repo's code directly into your repository:

> *[Visual Diagram: Architecture & Workflow]*

```bash
# Add a subtree
git subtree add --prefix=libs/ui-kit https://github.com/lib/ui-kit.git main --squash

# Pull updates
git subtree pull --prefix=libs/ui-kit https://github.com/lib/ui-kit.git main --squash

# Push changes back to the library
git subtree push --prefix=libs/ui-kit https://github.com/lib/ui-kit.git main
```

---


---

> *Note: Practical exercises and advanced topics currently being drafted.*

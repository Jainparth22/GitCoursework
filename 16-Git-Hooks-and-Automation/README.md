# Module 16: Git Hooks & Automation

> **Level**: 🔵 Advanced | **Time**: 3 hours | **Prerequisites**: [Module 15](../15-Interactive-Rebase-and-History-Rewriting/README.md)

---

## 📋 Learning Objectives

- Understand Git hooks and their trigger points
- Implement client-side and server-side hooks
- Use Husky and lint-staged for modern workflows
- Automate code quality enforcement

---

## 1. What Are Git Hooks?

Git hooks are **scripts** that run automatically at specific points in the Git workflow.

> *[Visual Diagram: Architecture & Workflow]*

### How Hooks Work Internally

> *[Visual Diagram: Architecture & Workflow]*

---

## 2. Hook Types and Trigger Points

> *[Visual Diagram: Architecture & Workflow]*

### Most Important Hooks

| Hook | When | Common Use |
|------|------|-----------|
| `pre-commit` | Before commit | Lint, format, run tests |
| `commit-msg` | After message entered | Validate commit message format |
| `pre-push` | Before push | Run full test suite |
| `pre-receive` | Server: before accepting push | Enforce policies |
| `post-receive` | Server: after accepting push | Deploy, notify |

---

## 3. Creating Hooks

Hooks are stored in `.git/hooks/` as executable scripts:

```bash
# Create a pre-commit hook
cat > .git/hooks/pre-commit << 'EOF'
#!/bin/sh
# Run linting before commit
echo "Running linter..."
npm run lint

if [ $? -ne 0 ]; then
    echo "❌ Lint failed. Fix issues before committing."
    exit 1
fi

echo "✅ Lint passed!"
exit 0
EOF

# Make it executable
chmod +x .git/hooks/pre-commit
```

### commit-msg Hook (Validate Format)

```bash
#!/bin/sh
# .git/hooks/commit-msg
# Enforce Conventional Commits format

commit_msg=$(cat "$1")
pattern="^(feat|fix|docs|style|refactor|test|chore|perf|ci|build)(\(.+\))?: .{1,72}$"

if ! echo "$commit_msg" | grep -qE "$pattern"; then
    echo "❌ Invalid commit message format!"
    echo "Expected: type(scope): description"
    echo "Example: feat(auth): add JWT login"
    exit 1
fi
exit 0
```

---


---

> *Note: Practical exercises and advanced topics currently being drafted.*

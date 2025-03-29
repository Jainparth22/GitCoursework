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

```mermaid
flowchart LR
    subgraph "Git Workflow with Hooks"
        A["git commit"] --> PRE["pre-commit<br/>hook runs"]
        PRE -->|"exit 0"| MSG["commit-msg<br/>hook runs"]
        PRE -->|"exit 1"| ABORT1["❌ Commit aborted"]
        MSG -->|"exit 0"| POST["post-commit<br/>hook runs"]
        MSG -->|"exit 1"| ABORT2["❌ Commit aborted"]
        POST --> DONE["✅ Commit complete"]
    end
    
    style PRE fill:#ffd43b
    style MSG fill:#74c0fc
    style POST fill:#51cf66
    style ABORT1 fill:#ff6b6b
    style ABORT2 fill:#ff6b6b
```

### How Hooks Work Internally

```mermaid
sequenceDiagram
    participant Dev as Developer
    participant Git as Git Engine
    participant Hook as .git/hooks/pre-commit
    
    Dev->>Git: git commit -m "message"
    Git->>Hook: Execute pre-commit script
    
    alt Script exits with 0
        Hook-->>Git: Exit code 0 (success)
        Git->>Git: Proceed with commit
    else Script exits with non-zero
        Hook-->>Git: Exit code 1 (failure)
        Git-->>Dev: ❌ Commit rejected!<br/>Fix issues and try again
    end
```

---

## 2. Hook Types and Trigger Points

```mermaid
graph TD
    subgraph "Client-Side Hooks"
        subgraph "Committing"
            H1["pre-commit<br/>Before commit message editor"]
            H2["prepare-commit-msg<br/>After default message, before editor"]
            H3["commit-msg<br/>After message entered"]
            H4["post-commit<br/>After commit completes"]
        end
        
        subgraph "Email / Patch"
            H5["applypatch-msg"]
            H6["pre-applypatch"]
            H7["post-applypatch"]
        end
        
        subgraph "Other"
            H8["pre-rebase"]
            H9["post-checkout"]
            H10["post-merge"]
            H11["pre-push"]
        end
    end
    
    subgraph "Server-Side Hooks"
        H12["pre-receive<br/>Before push accepted"]
        H13["update<br/>Per-branch, before update"]
        H14["post-receive<br/>After push completes"]
    end
    
    style H1 fill:#51cf66
    style H3 fill:#51cf66
    style H11 fill:#51cf66
    style H12 fill:#ff922b
```

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

## 4. Husky — Modern Hook Management

```mermaid
flowchart TD
    PROBLEM["Problem: .git/hooks/<br/>not shared via Git"]
    PROBLEM --> SOLUTION["Solution: Husky stores hooks<br/>in .husky/ (tracked by Git)"]
    SOLUTION --> INSTALL["npm install husky --save-dev"]
    INSTALL --> INIT["npx husky init"]
    INIT --> HOOKS[".husky/<br/>pre-commit<br/>commit-msg"]
    
    style PROBLEM fill:#ff6b6b
    style SOLUTION fill:#51cf66
```

```bash
# Install Husky
npm install --save-dev husky

# Initialize
npx husky init

# Create pre-commit hook
echo "npm run lint" > .husky/pre-commit

# Create commit-msg hook
echo 'npx commitlint --edit "$1"' > .husky/commit-msg
```

---

## 5. lint-staged — Only Lint Staged Files

```mermaid
flowchart TD
    A["git commit"] --> B["Husky triggers<br/>pre-commit hook"]
    B --> C["lint-staged runs"]
    C --> D["Only checks<br/>STAGED files ✅"]
    D --> E{"All pass?"}
    E -->|"Yes"| F["Commit proceeds ✅"]
    E -->|"No"| G["Commit blocked ❌"]
    
    style D fill:#51cf66
    style G fill:#ff6b6b
```

```json
// package.json
{
  "lint-staged": {
    "*.{js,ts}": ["eslint --fix", "prettier --write"],
    "*.{css,scss}": ["prettier --write"],
    "*.{json,md}": ["prettier --write"]
  }
}
```

---

## 6. Complete Hook Pipeline

```mermaid
flowchart TD
    A["Developer runs: git commit"] 
    --> B["pre-commit hook<br/>(lint-staged)"]
    --> C["Lint only staged files"]
    --> D["Format code<br/>(Prettier)"]
    --> E["Run relevant tests"]
    --> F["prepare-commit-msg<br/>(add ticket number)"]
    --> G["commit-msg<br/>(validate format)"]
    --> H["Commit created ✅"]
    --> I["post-commit<br/>(notify / log)"]
    
    style B fill:#ffd43b
    style G fill:#74c0fc
    style H fill:#51cf66
```

---

## 🏋️ Exercises

1. Create a `pre-commit` hook that prevents committing `console.log` statements
2. Create a `commit-msg` hook that enforces Conventional Commits format
3. Set up Husky + lint-staged in a Node.js project
4. Create a `pre-push` hook that runs tests before pushing
5. Try adding a hook that fails — observe how Git prevents the operation

---

## 🔑 Key Takeaways

1. Git hooks are scripts that run automatically at specific workflow points
2. Exit code 0 = proceed; non-zero = abort the operation
3. `.git/hooks/` is **not tracked** by Git — use Husky to share hooks via version control
4. **lint-staged** only checks staged files — fast and focused
5. Common pipeline: lint → format → test → validate message → commit
6. Hooks enforce quality gates automatically — no manual checks needed

---

**[← Module 15](../15-Interactive-Rebase-and-History-Rewriting/README.md)** | **[Module 17 →](../17-Submodules-and-Subtrees/README.md)**

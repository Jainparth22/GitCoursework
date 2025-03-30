# Module 28: Repository Best Practices

> **Level**: ⚫ Professional | **Time**: 2.5 hours | **Prerequisites**: [Module 27](../27-Monorepos-and-Scaling-Git/README.md)

---

## 📋 Learning Objectives

- Structure repositories for maintainability
- Write effective commit messages
- Implement conventional commits
- Set up community health files

---

## 1. Commit Message Standards

### Conventional Commits

```mermaid
graph TD
    MSG["Conventional Commit Format"]
    MSG --> TYPE["type: feat, fix, docs,<br/>style, refactor, test,<br/>chore, perf, ci, build"]
    MSG --> SCOPE["(scope): optional area<br/>(auth), (api), (ui)"]
    MSG --> DESC["description:<br/>short imperative summary"]
    MSG --> BODY["body:<br/>detailed explanation"]
    MSG --> FOOTER["footer:<br/>BREAKING CHANGE:, Closes #"]
    
    style TYPE fill:#51cf66
    style SCOPE fill:#74c0fc
    style DESC fill:#ffd43b
```

```
feat(auth): add JWT token refresh

Implement automatic token refresh when the access token
expires within 5 minutes. Uses refresh token from httpOnly
cookie.

BREAKING CHANGE: Login response now includes refresh_token field
Closes #142
```

### Commit Type Decision

```mermaid
flowchart TD
    A["What did you change?"] --> B{"Category?"}
    B -->|"New capability"| C["feat:"]
    B -->|"Bug fix"| D["fix:"]
    B -->|"Documentation only"| E["docs:"]
    B -->|"Code style (no logic)"| F["style:"]
    B -->|"Restructure code"| G["refactor:"]
    B -->|"Add/fix tests"| H["test:"]
    B -->|"Build/tooling"| I["chore: / build:"]
    B -->|"Performance"| J["perf:"]
    
    style C fill:#51cf66
    style D fill:#ff6b6b
```

---

## 2. Repository Structure

```mermaid
graph TD
    ROOT["Repository Root"]
    ROOT --> SRC["src/ or lib/<br/>(source code)"]
    ROOT --> TESTS["tests/<br/>(test files)"]
    ROOT --> DOCS["docs/<br/>(documentation)"]
    ROOT --> GH[".github/<br/>(GitHub config)"]
    ROOT --> CFG["Config files<br/>package.json, .eslintrc"]
    ROOT --> README["README.md"]
    ROOT --> CONTRIB["CONTRIBUTING.md"]
    ROOT --> LICENSE["LICENSE"]
    ROOT --> GITIGNORE[".gitignore"]
    ROOT --> CHANGELOG["CHANGELOG.md"]
    
    GH --> WORKFLOWS["workflows/"]
    GH --> TEMPLATES["ISSUE_TEMPLATE/"]
    GH --> PR_T["pull_request_template.md"]
    GH --> OWNERS["CODEOWNERS"]
    
    style ROOT fill:#74c0fc
    style GH fill:#ffd43b
```

---

## 3. README Quality Checklist

```mermaid
graph TD
    README["Excellent README"]
    README --> TITLE["Project Title + Badge"]
    README --> DESC_R["One-line Description"]
    README --> SCREEN["Screenshot / Demo GIF"]
    README --> INSTALL["Installation Instructions"]
    README --> USAGE["Usage Examples"]
    README --> API["API Reference"]
    README --> CONTRIBUTING_R["Contributing Link"]
    README --> LIC["License"]
    
    style README fill:#51cf66
```

---

## 4. .gitignore Best Practices

```bash
# OS files
.DS_Store
Thumbs.db

# IDE
.vscode/settings.json
.idea/

# Dependencies
node_modules/
__pycache__/
venv/

# Build output
dist/
build/
*.pyc

# Environment
.env
.env.local
*.key

# Logs
*.log
npm-debug.log*
```

```mermaid
flowchart TD
    A["Should this be in Git?"]
    A --> B{"Generated?<br/>(build output, deps)"}
    B -->|"Yes"| NO1["❌ .gitignore it"]
    B -->|"No"| C{"Contains secrets?<br/>(keys, tokens, .env)"}
    C -->|"Yes"| NO2["❌ .gitignore it"]
    C -->|"No"| D{"OS/IDE specific?<br/>(.DS_Store, .idea)"}
    D -->|"Yes"| NO3["❌ .gitignore it"]
    D -->|"No"| YES["✅ Track it"]
    
    style NO1 fill:#ff6b6b
    style NO2 fill:#ff6b6b
    style NO3 fill:#ff6b6b
    style YES fill:#51cf66
```

---

## 🏋️ Exercises

1. Convert your last 10 commit messages to Conventional Commits format
2. Set up a commit-msg hook enforcing Conventional Commits
3. Create a complete repository structure with all community health files
4. Write a comprehensive README with badges, screenshots, and examples
5. Audit your `.gitignore` — are you tracking anything that shouldn't be?

---

## 🔑 Key Takeaways

1. **Conventional Commits** enable automated changelogs and versioning
2. A well-structured repo includes README, CONTRIBUTING, LICENSE, and .github/
3. `.gitignore` should exclude generated files, secrets, and OS/IDE artifacts
4. Great READMEs include install instructions, usage examples, and screenshots
5. Repository standards reduce friction for new contributors

---

**[← Module 27](../27-Monorepos-and-Scaling-Git/README.md)** | **[Module 29 →](../29-Git-in-Production-DevOps-Integration/README.md)**

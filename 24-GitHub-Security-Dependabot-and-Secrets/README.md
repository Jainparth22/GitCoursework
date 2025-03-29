# Module 24: GitHub Security — Dependabot & Secrets

> **Level**: 🟣 GitHub Deep-Dive | **Time**: 3 hours | **Prerequisites**: [Module 23](../23-GitHub-Actions-Advanced-Workflows/README.md)

---

## 📋 Learning Objectives

- Scan for vulnerabilities with Dependabot
- Manage secrets securely
- Use code scanning and secret scanning
- Implement security best practices

---

## 1. GitHub Security Features Overview

```mermaid
graph TD
    SEC["GitHub Security"]
    SEC --> DEP["Dependabot<br/>Dependency vulnerabilities"]
    SEC --> CS["Code Scanning<br/>(CodeQL)"]
    SEC --> SS["Secret Scanning<br/>Leaked credentials"]
    SEC --> SA["Security Advisories<br/>CVE management"]
    
    DEP --> DEP_A["Alerts<br/>(notify about CVEs)"]
    DEP --> DEP_U["Updates<br/>(auto-create PRs)"]
    DEP --> DEP_V["Version Updates<br/>(keep deps current)"]
    
    style SEC fill:#ff6b6b
    style DEP fill:#ff922b
    style CS fill:#74c0fc
    style SS fill:#ffd43b
```

---

## 2. Dependabot — How It Works

```mermaid
sequenceDiagram
    participant DB as Dependabot
    participant GH as GitHub
    participant GHDB as Advisory Database
    participant Repo as Your Repository
    
    DB->>Repo: Read package.json / requirements.txt
    DB->>GHDB: Check for known CVEs
    
    alt Vulnerabilities found
        DB->>GH: Create Security Alert
        DB->>Repo: Open PR with version bump
        Note over Repo: PR: "Bump lodash<br/>4.17.19 → 4.17.21"
    end
    
    Note over DB: Runs daily by default
```

### Dependabot Configuration

```yaml
# .github/dependabot.yml
version: 2
updates:
  # npm dependencies
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
    reviewers:
      - "security-team"
    labels:
      - "dependencies"
    open-pull-requests-limit: 10
  
  # GitHub Actions
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
```

---

## 3. Secret Scanning

```mermaid
flowchart TD
    A["Developer pushes code"] --> B["GitHub scans<br/>commit content"]
    B --> C{"Contains secrets?<br/>(API keys, tokens,<br/>passwords)"}
    C -->|"Yes"| D["Alert sent to:<br/>• Repo admins<br/>• Service provider<br/>(if partner program)"]
    C -->|"No"| E["No action needed ✅"]
    
    D --> F["Provider may<br/>auto-revoke token!"]
    
    style D fill:#ff6b6b
    style F fill:#ff922b
    style E fill:#51cf66
```

### Push Protection (Block Before It's Pushed)

```mermaid
flowchart TD
    A["git push"] --> B{"Secret detected<br/>in push?"}
    B -->|"Yes"| C["❌ Push BLOCKED<br/>Remove secret first"]
    B -->|"No"| D["✅ Push succeeds"]
    
    C --> E["Options:<br/>1. Remove secret<br/>2. Mark as false positive<br/>3. Use .env + .gitignore"]
    
    style C fill:#ff6b6b
    style D fill:#51cf66
```

---

## 4. Code Scanning with CodeQL

```mermaid
flowchart TD
    A["Code pushed / PR opened"] --> B["CodeQL analyzes code"]
    B --> C["Builds semantic model"]
    C --> D["Runs security queries"]
    D --> E{"Vulnerabilities<br/>found?"}
    E -->|"Yes"| F["Create alerts with<br/>severity + fix suggestions"]
    E -->|"No"| G["✅ Clean"]
    
    style F fill:#ff6b6b
    style G fill:#51cf66
```

```yaml
# .github/workflows/codeql.yml
name: CodeQL Analysis
on: [push, pull_request]

jobs:
  analyze:
    runs-on: ubuntu-latest
    permissions:
      security-events: write
    
    steps:
      - uses: actions/checkout@v4
      - uses: github/codeql-action/init@v3
        with:
          languages: javascript
      - uses: github/codeql-action/analyze@v3
```

---

## 5. Secrets Management Best Practices

```mermaid
graph TD
    GOOD["✅ Good Practices"]
    GOOD --> G1["Use GitHub Secrets<br/>(encrypted at rest)"]
    GOOD --> G2["Use environment-scoped<br/>secrets"]
    GOOD --> G3["Rotate secrets regularly"]
    GOOD --> G4["Use OIDC for cloud auth<br/>(no stored credentials)"]
    
    BAD["❌ Bad Practices"]
    BAD --> B1["Hardcode in source code"]
    BAD --> B2["Commit .env files"]
    BAD --> B3["Share via chat/email"]
    BAD --> B4["Use same secret everywhere"]
    
    style GOOD fill:#51cf66
    style BAD fill:#ff6b6b
```

```bash
# .gitignore — ALWAYS include
.env
.env.local
.env.production
*.key
*.pem
```

---

## 🏋️ Exercises

1. Set up `dependabot.yml` for npm and GitHub Actions
2. Enable secret scanning and push protection on a repository
3. Set up CodeQL code scanning with a workflow
4. Practice storing and using secrets in GitHub Actions
5. Review Dependabot alerts and merge a security update PR

---

## 🔑 Key Takeaways

1. **Dependabot** auto-detects vulnerable dependencies and creates update PRs
2. **Secret scanning** finds leaked credentials and alerts you (or auto-revokes)
3. **Push protection** blocks pushes containing secrets *before* they reach GitHub
4. **CodeQL** performs semantic code analysis to find security vulnerabilities
5. Never hardcode secrets — use `.gitignore`, GitHub Secrets, and environment variables
6. Enable all security features — they're free for public repos

---

**[← Module 23](../23-GitHub-Actions-Advanced-Workflows/README.md)** | **[Module 25 →](../25-Branching-Strategies-and-Git-Flow/README.md)**

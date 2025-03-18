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

> *[Visual Diagram: Architecture & Workflow]*

---

## 2. Dependabot — How It Works

> *[Visual Diagram: Architecture & Workflow]*

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

> *[Visual Diagram: Architecture & Workflow]*

### Push Protection (Block Before It's Pushed)

> *[Visual Diagram: Architecture & Workflow]*

---


---

> *Note: Practical exercises and advanced topics currently being drafted.*

# Module 21: Branch Protection & CODEOWNERS

> **Level**: 🟣 GitHub Deep-Dive | **Time**: 2.5 hours | **Prerequisites**: [Module 20](../20-Pull-Requests-and-Code-Reviews/README.md)

---

## 📋 Learning Objectives

- Configure branch protection rules
- Set up required status checks and reviews
- Use CODEOWNERS for automatic review assignment
- Understand GitHub Rulesets (modern alternative)

---

## 1. Branch Protection — How It Works

> *[Visual Diagram: Architecture & Workflow]*

### Common Protection Rules

| Rule | What It Does |
|------|-------------|
| Require PR reviews | No direct pushes, must go through PR |
| Required reviewers (N) | Minimum number of approvals |
| Dismiss stale reviews | Re-review needed after new commits |
| Require status checks | CI must pass before merge |
| Require up-to-date | Branch must be current with base |
| Restrict force push | Prevent history rewriting |
| Restrict deletions | Prevent branch deletion |
| Require signed commits | Only GPG-signed commits allowed |

---

## 2. CODEOWNERS

> *[Visual Diagram: Architecture & Workflow]*

```bash
# .github/CODEOWNERS

# Default owners for everything
* @org/core-team

# Frontend
/src/components/ @org/frontend-team
*.css @org/frontend-team
*.tsx @org/frontend-team

# Backend
/src/api/ @org/backend-team
/src/models/ @org/backend-team @lead-dev

# Documentation
/docs/ @org/docs-team
*.md @org/docs-team

# DevOps
Dockerfile @org/devops-team
.github/ @org/devops-team
```

---


---

> *Note: Practical exercises and advanced topics currently being drafted.*

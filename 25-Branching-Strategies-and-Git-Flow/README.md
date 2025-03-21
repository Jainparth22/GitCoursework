# Module 25: Branching Strategies & Git Flow

> **Level**: ⚫ Professional | **Time**: 3 hours | **Prerequisites**: [Module 24](../24-GitHub-Security-Dependabot-and-Secrets/README.md)

---

## 📋 Learning Objectives

- Compare Git Flow, GitHub Flow, and Trunk-Based Development
- Choose the right branching model for your team
- Implement release management strategies

---

## 1. Branching Models Compared

> *[Visual Diagram: Architecture & Workflow]*

---

## 2. Git Flow

> *[Visual Diagram: Architecture & Workflow]*

### Git Flow Branch Roles

| Branch | Purpose | Lifetime |
|--------|---------|----------|
| `main` | Production code | Permanent |
| `develop` | Integration branch | Permanent |
| `feature/*` | New features | Temporary |
| `release/*` | Release prep | Temporary |
| `hotfix/*` | Emergency fixes | Temporary |

---

## 3. GitHub Flow (Simplified)

> *[Visual Diagram: Architecture & Workflow]*

**Rules:**
1. `main` is always deployable
2. Create feature branches from `main`
3. Open PR when ready for review
4. After review + CI, merge to `main`
5. Deploy immediately after merge

---


---

> *Note: Practical exercises and advanced topics currently being drafted.*

# Module 29: Git in Production — DevOps Integration

> **Level**: ⚫ Professional | **Time**: 4 hours | **Prerequisites**: [Module 28](../28-Repository-Best-Practices/README.md)

---

## 📋 Learning Objectives

- Integrate Git with CI/CD production pipelines
- Implement GitOps for infrastructure management
- Master deployment strategies with Git
- Automate releases

---

## 1. GitOps — Git as Single Source of Truth

```mermaid
flowchart TD
    DEV["Developer pushes<br/>infrastructure change"] --> GIT["Git Repository<br/>(desired state)"]
    GIT --> OPERATOR["GitOps Operator<br/>(ArgoCD / Flux)"]
    OPERATOR --> COMPARE["Compare:<br/>desired vs actual state"]
    COMPARE --> SYNC["Auto-sync cluster<br/>to match Git state"]
    SYNC --> K8S["Kubernetes Cluster<br/>(actual state)"]
    
    K8S -->|"drift detected"| COMPARE
    
    style GIT fill:#51cf66,stroke:#333,stroke-width:3px
    style K8S fill:#74c0fc
```

### GitOps Principles

```mermaid
graph TD
    GITOPS["GitOps"]
    GITOPS --> P1["Declarative:<br/>Desired state in Git"]
    GITOPS --> P2["Versioned:<br/>Full audit trail"]
    GITOPS --> P3["Automated:<br/>Operators sync state"]
    GITOPS --> P4["Observable:<br/>Drift detection + alerts"]
    
    style P1 fill:#51cf66
    style P2 fill:#74c0fc
    style P3 fill:#ffd43b
    style P4 fill:#e599f7
```

---

## 2. Deployment Strategies with Git

```mermaid
flowchart TD
    DS["Deployment Strategies"]
    DS --> ROLL["Rolling Deploy<br/>Gradual replacement"]
    DS --> BG["Blue-Green<br/>Switch between environments"]
    DS --> CAN["Canary<br/>Small % first"]
    DS --> FF["Feature Flags<br/>Toggle at runtime"]
    
    style ROLL fill:#51cf66
    style BG fill:#74c0fc
    style CAN fill:#ffd43b
    style FF fill:#e599f7
```

### Git Tag-Based Deployments

```mermaid
sequenceDiagram
    participant Dev as Developer
    participant Git as Git/GitHub
    participant CI as CI/CD Pipeline
    participant Prod as Production
    
    Dev->>Git: git tag v2.1.0
    Dev->>Git: git push --tags
    Git->>CI: Tag push event
    CI->>CI: Build + test
    CI->>CI: Build Docker image<br/>tagged: app:v2.1.0
    CI->>Prod: Deploy v2.1.0
    Prod-->>Dev: Deployment complete ✅
    
    Note over Dev: Rollback?<br/>Re-deploy previous tag
```

---

## 3. Automated Release Pipeline

```yaml
# .github/workflows/release.yml
name: Release
on:
  push:
    tags: ['v*']

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Build
        run: npm ci && npm run build
      
      - name: Create GitHub Release
        uses: softprops/action-gh-release@v2
        with:
          generate_release_notes: true
          files: dist/*
```

### Semantic Release (Fully Automated)

```mermaid
flowchart TD
    A["Commit to main<br/>(Conventional Commits)"] --> B["semantic-release<br/>analyzes commits"]
    B --> C{"Commit types?"}
    C -->|"feat:"| D["Minor bump<br/>1.0.0 → 1.1.0"]
    C -->|"fix:"| E["Patch bump<br/>1.0.0 → 1.0.1"]
    C -->|"BREAKING CHANGE:"| F["Major bump<br/>1.0.0 → 2.0.0"]
    
    D --> G["Auto: create tag,<br/>GitHub release,<br/>publish npm,<br/>update changelog"]
    E --> G
    F --> G
    
    style G fill:#51cf66
```

---

## 4. Environment-Based Promotion

```mermaid
graph LR
    DEV["Dev<br/>(develop branch)"] -->|"merge"| STAGING["Staging<br/>(release branch)"]
    STAGING -->|"tag"| PROD["Production<br/>(main + tag)"]
    
    style DEV fill:#74c0fc
    style STAGING fill:#ffd43b
    style PROD fill:#51cf66
```

---

## 🏋️ Exercises

1. Set up a tag-triggered deployment pipeline with GitHub Actions
2. Configure `semantic-release` with Conventional Commits
3. Implement a blue-green deployment strategy using Git branches
4. Create a GitOps-style repo where merging to `main` auto-deploys

---

## 🔑 Key Takeaways

1. **GitOps** makes Git the single source of truth for infrastructure
2. Tag-based deployments provide clear versioning and easy rollback
3. `semantic-release` automates version bumps based on commit messages
4. Environment promotion (dev → staging → prod) uses branches and tags
5. Every deployment should be traceable to a Git commit/tag

---

**[← Module 28](../28-Repository-Best-Practices/README.md)** | **[Module 30 →](../30-Capstone-Project/README.md)**

# Module 25: Branching Strategies & Git Flow

> **Level**: ⚫ Professional | **Time**: 3 hours | **Prerequisites**: [Module 24](../24-GitHub-Security-Dependabot-and-Secrets/README.md)

---

## 📋 Learning Objectives

- Compare Git Flow, GitHub Flow, and Trunk-Based Development
- Choose the right branching model for your team
- Implement release management strategies

---

## 1. Branching Models Compared

```mermaid
flowchart TD
    CHOOSE["Choose a Branching Strategy"] --> A{"Team size &<br/>release cadence?"}
    A -->|"Small team<br/>Continuous deploy"| GHF["GitHub Flow"]
    A -->|"Large team<br/>Scheduled releases"| GF["Git Flow"]
    A -->|"Any team<br/>CI/CD mature"| TBD["Trunk-Based Dev"]
    
    style GHF fill:#51cf66
    style GF fill:#74c0fc
    style TBD fill:#ffd43b
```

---

## 2. Git Flow

```mermaid
gitGraph
    commit id: "init"
    branch develop
    commit id: "feat-start"
    branch feature/login
    commit id: "login-1"
    commit id: "login-2"
    checkout develop
    merge feature/login id: "merge-feat"
    commit id: "more-dev"
    branch release/1.0
    commit id: "rc-fix-1"
    checkout main
    merge release/1.0 id: "v1.0" tag: "v1.0.0"
    checkout develop
    merge release/1.0 id: "sync-dev"
    checkout main
    branch hotfix/1.0.1
    commit id: "critical-fix"
    checkout main
    merge hotfix/1.0.1 id: "v1.0.1" tag: "v1.0.1"
    checkout develop
    merge hotfix/1.0.1 id: "sync-hotfix"
```

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

```mermaid
graph RL
    C1["main"] 
    FB["feature branch<br/>created from main"] -.-> C1
    FB --> PR["Pull Request"]
    PR --> REVIEW["Code Review"]
    REVIEW --> MERGE["Merge to main"]
    MERGE --> DEPLOY["Deploy immediately"]
    
    style FB fill:#74c0fc
    style PR fill:#ffd43b
    style DEPLOY fill:#51cf66
```

**Rules:**
1. `main` is always deployable
2. Create feature branches from `main`
3. Open PR when ready for review
4. After review + CI, merge to `main`
5. Deploy immediately after merge

---

## 4. Trunk-Based Development

```mermaid
graph RL
    subgraph "Trunk-Based"
        T1["main (trunk)"]
        SL1["short-lived branch<br/>(< 1 day)"] -.-> T1
        SL2["short-lived branch<br/>(< 1 day)"] -.-> T1
        
        T1 --> FF["Feature Flags<br/>(toggle incomplete features)"]
    end
    
    style T1 fill:#51cf66
    style SL1 fill:#ffd43b
    style SL2 fill:#ffd43b
```

**Key practices:**
- Branches live less than 1 day
- Merge to `main` multiple times per day
- Use **feature flags** for incomplete features
- Requires strong CI/CD and test coverage

---

## 5. Strategy Comparison

| Factor | Git Flow | GitHub Flow | Trunk-Based |
|--------|----------|-------------|-------------|
| Complexity | High | Low | Medium |
| Branch lifetime | Long | Medium | Very short |
| Release cadence | Scheduled | Continuous | Continuous |
| Best for | Mobile apps, scheduled releases | SaaS, web apps | High-velocity teams |
| CI/CD maturity needed | Low | Medium | High |

---

## 🏋️ Exercises

1. Implement a full Git Flow cycle: feature → develop → release → main
2. Practice GitHub Flow: branch → PR → merge → deploy
3. Create a feature flag system for trunk-based development
4. Decide which strategy your current project should use and justify why

---

## 🔑 Key Takeaways

1. **Git Flow**: Best for scheduled releases with clear versioning
2. **GitHub Flow**: Simple and effective for continuous deployment
3. **Trunk-Based**: Fastest velocity, requires strong CI/CD and feature flags
4. No single strategy fits all — choose based on team size, release cadence, and CI maturity
5. Whatever you choose, document it and enforce it consistently

---

**[← Module 24](../24-GitHub-Security-Dependabot-and-Secrets/README.md)** | **[Module 26 →](../26-Open-Source-Contribution-Guide/README.md)**

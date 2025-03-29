# Module 20: Pull Requests & Code Reviews

> **Level**: 🟣 GitHub Deep-Dive | **Time**: 3 hours | **Prerequisites**: [Module 19](../19-GitHub-Issues-Projects-and-Wikis/README.md)

---

## 📋 Learning Objectives

- Create effective pull requests
- Conduct thorough code reviews
- Understand PR merge strategies in depth
- Use PR automation features

---

## 1. Pull Request Lifecycle

```mermaid
sequenceDiagram
    participant Dev as Developer
    participant GH as GitHub
    participant Rev as Reviewer
    participant CI as CI/CD
    
    Dev->>Dev: Create feature branch
    Dev->>Dev: Make commits
    Dev->>GH: Push branch + Open PR
    
    GH->>CI: Trigger CI checks
    GH->>Rev: Request review
    
    CI-->>GH: ✅ All checks pass
    Rev->>GH: Review code
    
    alt Changes requested
        Rev->>GH: Request changes
        GH-->>Dev: Feedback received
        Dev->>Dev: Make fixes
        Dev->>GH: Push new commits
        Rev->>GH: Approve ✅
    else Approved
        Rev->>GH: Approve ✅
    end
    
    Dev->>GH: Merge PR
    GH->>GH: Close PR + linked issues
    GH->>GH: Delete branch (optional)
```

---

## 2. PR Merge Strategies — What They Do Internally

```mermaid
graph TD
    PR["Pull Request<br/>Ready to Merge"] --> MS{"Merge Strategy?"}
    
    MS --> MC["Create Merge Commit<br/>(--no-ff)"]
    MS --> SQ["Squash and Merge"]
    MS --> RB["Rebase and Merge"]
    
    style MC fill:#51cf66
    style SQ fill:#ffd43b
    style RB fill:#74c0fc
```

### Merge Commit

```mermaid
graph RL
    subgraph "Result: Merge Commit"
        C1["C1"]
        C2["C2"] --> C1
        F1["F1"] --> C1
        F2["F2"] --> F1
        M["Merge commit"] --> C2
        M --> F2
        MAIN["main"] -.-> M
    end
    
    style M fill:#e599f7
```
**Preserves all commits + branch structure. Full history.**

### Squash and Merge

```mermaid
graph RL
    subgraph "Result: Squash Merge"
        C1B["C1"]
        C2B["C2"] --> C1B
        SQ["F1+F2 squashed<br/>(single new commit)"] --> C2B
        MAINB["main"] -.-> SQ
    end
    
    style SQ fill:#ffd43b
```
**All PR commits combined into one. Clean linear history.**

### Rebase and Merge

```mermaid
graph RL
    subgraph "Result: Rebase Merge"
        C1C["C1"]
        C2C["C2"] --> C1C
        F1R["F1' (rebased)"] --> C2C
        F2R["F2' (rebased)"] --> F1R
        MAINC["main"] -.-> F2R
    end
    
    style F1R fill:#74c0fc
    style F2R fill:#74c0fc
```
**Individual commits preserved, linear history, new SHAs.**

### Strategy Decision

```mermaid
flowchart TD
    A["Which merge strategy?"] --> B{"How many meaningful<br/>commits in PR?"}
    B -->|"1 commit"| C["Any strategy<br/>(all produce same result)"]
    B -->|"Multiple WIP commits"| D["Squash and Merge<br/>(clean up messy history)"]
    B -->|"Multiple meaningful commits"| E{"Want branch<br/>visible in history?"}
    E -->|"Yes"| F["Merge Commit"]
    E -->|"No"| G["Rebase and Merge"]
    
    style C fill:#51cf66
    style D fill:#ffd43b
    style F fill:#51cf66
    style G fill:#74c0fc
```

---

## 3. Code Review Best Practices

```mermaid
graph TD
    REVIEW["Code Review Checklist"]
    REVIEW --> CORRECT["Correctness<br/>Does it work?"]
    REVIEW --> DESIGN["Design<br/>Is it well-structured?"]
    REVIEW --> READABLE["Readability<br/>Is it clear?"]
    REVIEW --> TESTS["Tests<br/>Are there tests?"]
    REVIEW --> SECURITY["Security<br/>Any vulnerabilities?"]
    REVIEW --> PERF["Performance<br/>Any bottlenecks?"]
    
    style CORRECT fill:#51cf66
    style SECURITY fill:#ff6b6b
```

### Review Comment Types

| Prefix | Meaning | Example |
|--------|---------|---------|
| `nit:` | Minor style issue | `nit: add trailing comma` |
| `suggestion:` | Take it or leave it | `suggestion: consider using map()` |
| `question:` | Need clarification | `question: why is this async?` |
| `issue:` | Must fix before merge | `issue: SQL injection vulnerability` |
| `praise:` | Positive feedback | `praise: elegant solution!` |

---

## 4. PR Template

```markdown
<!-- .github/pull_request_template.md -->

## Description
<!-- What does this PR do? -->

## Related Issue
Closes #

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation

## Checklist
- [ ] Tests added/updated
- [ ] Documentation updated
- [ ] Self-review completed
- [ ] No console.log or debug code
```

---

## 5. Draft PRs and Auto-Merge

```mermaid
flowchart TD
    A["Open Draft PR<br/>(work in progress)"] --> B["CI runs on every push"]
    B --> C["Mark 'Ready for Review'"]
    C --> D["Reviewers assigned"]
    D --> E{"Enable auto-merge?"}
    E -->|"Yes"| F["PR auto-merges when:<br/>✅ All checks pass<br/>✅ Required approvals met"]
    E -->|"No"| G["Manual merge button"]
    
    style A fill:#ffd43b
    style F fill:#51cf66
```

---

## 🏋️ Exercises

1. Create a PR with a detailed description using a PR template
2. Merge three PRs using each merge strategy and compare `git log --graph`
3. Practice code review: leave comments with proper prefixes
4. Set up a draft PR and convert it to ready for review
5. Enable auto-merge on a PR with required checks

---

## 🔑 Key Takeaways

1. PRs are the standard way to propose, review, and integrate changes
2. **Merge commit** preserves all history; **squash** cleans up; **rebase** linearizes
3. Good reviews check correctness, design, readability, tests, and security
4. Use PR templates to standardize the information contributors provide
5. Draft PRs let you get early CI feedback without requesting reviews
6. Auto-merge reduces manual work once all checks and approvals pass

---

**[← Module 19](../19-GitHub-Issues-Projects-and-Wikis/README.md)** | **[Module 21 →](../21-Branch-Protection-and-CODEOWNERS/README.md)**

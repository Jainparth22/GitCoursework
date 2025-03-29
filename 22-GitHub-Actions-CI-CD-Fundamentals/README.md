# Module 22: GitHub Actions — CI/CD Fundamentals

> **Level**: 🟣 GitHub Deep-Dive | **Time**: 4 hours | **Prerequisites**: [Module 21](../21-Branch-Protection-and-CODEOWNERS/README.md)

---

## 📋 Learning Objectives

- Understand GitHub Actions architecture
- Write CI/CD workflows in YAML
- Use actions, jobs, steps, and triggers
- Implement automated testing and deployment

---

## 1. GitHub Actions Architecture

```mermaid
graph TD
    EVENT["Event<br/>(push, PR, schedule)"]
    EVENT --> WORKFLOW["Workflow<br/>(.github/workflows/*.yml)"]
    WORKFLOW --> JOB1["Job 1: build"]
    WORKFLOW --> JOB2["Job 2: test"]
    WORKFLOW --> JOB3["Job 3: deploy"]
    
    JOB1 --> RUNNER1["Runner<br/>(ubuntu-latest)"]
    JOB2 --> RUNNER2["Runner<br/>(ubuntu-latest)"]
    JOB3 --> RUNNER3["Runner<br/>(ubuntu-latest)"]
    
    RUNNER1 --> S1["Step 1: Checkout"]
    RUNNER1 --> S2["Step 2: Setup Node"]
    RUNNER1 --> S3["Step 3: Install deps"]
    RUNNER1 --> S4["Step 4: Build"]
    
    style EVENT fill:#ff922b
    style WORKFLOW fill:#74c0fc
    style JOB1 fill:#ffd43b
    style JOB2 fill:#ffd43b
    style JOB3 fill:#ffd43b
    style RUNNER1 fill:#51cf66
```

### How a Workflow Executes

```mermaid
sequenceDiagram
    participant Dev as Developer
    participant GH as GitHub
    participant Runner as Runner VM
    
    Dev->>GH: git push / Open PR
    GH->>GH: Match event to workflows
    GH->>Runner: Provision fresh VM
    
    Runner->>Runner: Execute Job steps:<br/>1. Clone repo<br/>2. Install tools<br/>3. Run commands
    
    alt All steps succeed
        Runner-->>GH: ✅ Job passed
    else Any step fails
        Runner-->>GH: ❌ Job failed
    end
    
    GH-->>Dev: Status check result
    GH->>Runner: Destroy VM (ephemeral)
```

---

## 2. Workflow YAML Structure

```yaml
# .github/workflows/ci.yml
name: CI Pipeline                    # Workflow name

on:                                  # Triggers
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:                                # Jobs (run in parallel by default)
  build:
    runs-on: ubuntu-latest           # Runner
    
    steps:                           # Sequential steps within job
      - name: Checkout code
        uses: actions/checkout@v4    # Use an action
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci                  # Run shell command
      
      - name: Run lint
        run: npm run lint
      
      - name: Run tests
        run: npm test
      
      - name: Build
        run: npm run build
```

### YAML Components Explained

```mermaid
graph TD
    YML["Workflow YAML"]
    YML --> NAME["name: Display name"]
    YML --> ON["on: Event triggers"]
    YML --> JOBS["jobs: Work units"]
    
    ON --> PUSH["push / pull_request"]
    ON --> SCHEDULE["schedule (cron)"]
    ON --> WD["workflow_dispatch (manual)"]
    
    JOBS --> JOB["job-id:"]
    JOB --> RUNS["runs-on: OS/runner"]
    JOB --> NEEDS["needs: dependencies"]
    JOB --> STEPS["steps: sequential tasks"]
    
    STEPS --> USES["uses: pre-built action"]
    STEPS --> RUN["run: shell command"]
    STEPS --> WITH["with: action inputs"]
    STEPS --> ENV["env: variables"]
    
    style YML fill:#ff922b
    style ON fill:#74c0fc
    style JOBS fill:#51cf66
```

---

## 3. Event Triggers

```mermaid
flowchart TD
    TRIGGERS["Workflow Triggers"]
    TRIGGERS --> CODE["Code Events"]
    TRIGGERS --> PR_E["PR Events"]
    TRIGGERS --> SCHED["Scheduled"]
    TRIGGERS --> MANUAL["Manual"]
    
    CODE --> PUSH["push<br/>(branch filter)"]
    CODE --> TAG["push tags:<br/>(v*)"]
    
    PR_E --> POPEN["pull_request<br/>(opened, sync, closed)"]
    
    SCHED --> CRON["schedule:<br/>cron: '0 0 * * *'"]
    
    MANUAL --> DISPATCH["workflow_dispatch<br/>(with inputs)"]
    
    style CODE fill:#51cf66
    style PR_E fill:#74c0fc
    style SCHED fill:#ffd43b
    style MANUAL fill:#e599f7
```

```yaml
# Multiple triggers
on:
  push:
    branches: [main]
    paths: ['src/**']           # Only when src/ changes
  pull_request:
    types: [opened, synchronize]
  schedule:
    - cron: '0 2 * * 1'        # Every Monday at 2 AM
  workflow_dispatch:            # Manual trigger button
    inputs:
      environment:
        type: choice
        options: [staging, production]
```

---

## 4. Job Dependencies

```mermaid
graph LR
    BUILD["build"] --> TEST["test"]
    BUILD --> LINT["lint"]
    TEST --> DEPLOY["deploy"]
    LINT --> DEPLOY
    
    style BUILD fill:#ffd43b
    style TEST fill:#74c0fc
    style LINT fill:#74c0fc
    style DEPLOY fill:#51cf66
```

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: npm run build

  test:
    needs: build              # Waits for build
    runs-on: ubuntu-latest
    steps:
      - run: npm test

  lint:
    needs: build              # Waits for build
    runs-on: ubuntu-latest
    steps:
      - run: npm run lint

  deploy:
    needs: [test, lint]       # Waits for BOTH
    runs-on: ubuntu-latest
    steps:
      - run: ./deploy.sh
```

---

## 5. Secrets and Environment Variables

```mermaid
flowchart TD
    subgraph "Secret Storage"
        REPO["Repository Secrets<br/>(Settings → Secrets)"]
        ENV_S["Environment Secrets<br/>(per environment)"]
        ORG["Organization Secrets<br/>(shared across repos)"]
    end
    
    REPO --> WF["Available in workflows<br/>secrets.MY_SECRET"]
    ENV_S --> WF
    ORG --> WF
    
    WF --> MASKED["Value is MASKED<br/>in logs (****)"]
    
    style MASKED fill:#51cf66
```

```yaml
steps:
  - name: Deploy
    env:
      API_KEY: ${{ secrets.API_KEY }}
      NODE_ENV: production
    run: |
      echo "Deploying with API key..."
      ./deploy.sh
```

---

## 6. Complete CI/CD Example

```yaml
name: Full CI/CD Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [18, 20, 22]
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'
      
      - run: npm ci
      - run: npm test
  
  deploy:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    environment: production
    
    steps:
      - uses: actions/checkout@v4
      - run: npm ci && npm run build
      - name: Deploy
        env:
          DEPLOY_KEY: ${{ secrets.DEPLOY_KEY }}
        run: ./scripts/deploy.sh
```

---

## 🏋️ Exercises

1. Create a CI workflow that runs lint and tests on every PR
2. Add a matrix strategy to test across Node.js 18, 20, and 22
3. Set up a deploy job that runs only on pushes to `main`
4. Store a secret and use it in a workflow step
5. Add path filters to only run tests when `src/` changes

---

## 🔑 Key Takeaways

1. Workflows are triggered by **events** and run on ephemeral **runners**
2. Jobs run in **parallel** by default; use `needs` for dependencies
3. Steps are **sequential** within a job; `uses` = action, `run` = command
4. **Matrix strategies** test across multiple configurations automatically
5. Secrets are encrypted and **masked** in logs — never hardcode credentials
6. Use `if` conditions and path filters to optimize when workflows run

---

**[← Module 21](../21-Branch-Protection-and-CODEOWNERS/README.md)** | **[Module 23 →](../23-GitHub-Actions-Advanced-Workflows/README.md)**

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

> *[Visual Diagram: Architecture & Workflow]*

### How a Workflow Executes

> *[Visual Diagram: Architecture & Workflow]*

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

> *[Visual Diagram: Architecture & Workflow]*

---

## 3. Event Triggers

> *[Visual Diagram: Architecture & Workflow]*

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


---

> *Note: Practical exercises and advanced topics currently being drafted.*

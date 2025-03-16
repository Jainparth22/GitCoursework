# Module 23: GitHub Actions — Advanced Workflows

> **Level**: 🟣 GitHub Deep-Dive | **Time**: 3.5 hours | **Prerequisites**: [Module 22](../22-GitHub-Actions-CI-CD-Fundamentals/README.md)

---

## 📋 Learning Objectives

- Create reusable workflows and composite actions
- Use matrix strategies for multi-environment testing
- Implement caching and artifacts
- Chain workflows with `workflow_call` and `workflow_run`

---

## 1. Reusable Workflows

> *[Visual Diagram: Architecture & Workflow]*

```yaml
# Reusable workflow (callee)
# .github/workflows/reusable-test.yml
name: Reusable Test
on:
  workflow_call:               # Makes it callable
    inputs:
      node-version:
        type: string
        default: '20'
    secrets:
      npm-token:
        required: true

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ inputs.node-version }}
      - run: npm ci
      - run: npm test
```

```yaml
# Caller workflow
name: CI
on: push
jobs:
  call-test:
    uses: org/shared/.github/workflows/reusable-test.yml@main
    with:
      node-version: '20'
    secrets:
      npm-token: ${{ secrets.NPM_TOKEN }}
```

---

## 2. Composite Actions

> *[Visual Diagram: Architecture & Workflow]*

```yaml
# .github/actions/setup-project/action.yml
name: 'Setup Project'
description: 'Setup Node.js and install dependencies'
inputs:
  node-version:
    description: 'Node.js version'
    default: '20'
runs:
  using: composite
  steps:
    - uses: actions/setup-node@v4
      with:
        node-version: ${{ inputs.node-version }}
        cache: 'npm'
    - run: npm ci
      shell: bash
```

---

## 3. Matrix Strategies

> *[Visual Diagram: Architecture & Workflow]*

```yaml
jobs:
  test:
    strategy:
      fail-fast: false          # Don't cancel others on failure
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        node: [18, 20, 22]
        exclude:                # Remove specific combos
          - os: windows-latest
            node: 18
        include:                # Add specific combos
          - os: ubuntu-latest
            node: 20
            experimental: true
    
    runs-on: ${{ matrix.os }}
    steps:
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node }}
```

---


---

> *Note: Practical exercises and advanced topics currently being drafted.*

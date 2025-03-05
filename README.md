<div align="center">

# 🎓 Git & GitHub — Master Coursework
### *From Absolute Beginner to Enterprise DevOps Mastery*

[![Git Version](https://img.shields.io/badge/Git-2.40+-F05032?logo=git&logoColor=white&style=for-the-badge)](https://git-scm.com/)
[![GitHub Platform](https://img.shields.io/badge/GitHub-Ecosystem-181717?logo=github&logoColor=white&style=for-the-badge)](https://github.com/)
[![Curriculum](https://img.shields.io/badge/Curriculum-30%20Modules-blue?style=for-the-badge)](#-curriculum-index)
[![Level](https://img.shields.io/badge/Level-Foundations%20→%20DevOps-brightgreen?style=for-the-badge)](#-course-overview)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](#-license)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=for-the-badge)](#-contributing)

<p align="center">
  A university-grade, production-tested curriculum covering version control internals, DAG commit graphs, advanced collaboration workflows, automated CI/CD pipelines, GitOps, secret scanning, and enterprise engineering standards.
</p>

[📚 Explore Modules](#-curriculum-index) • [🗺️ 6-Month Roadmap](#️-6-month-learning-roadmap) • [📋 Cheat Sheet](./CHEATSHEET.md) • [📘 Glossary](./GLOSSARY.md) • [🏆 Capstone Project](./30-Capstone-Project/README.md) • [🚀 Getting Started](#-getting-started)

---
</div>

## 📋 Course Overview

This repository houses a comprehensive, 30-module curriculum engineered to transition software engineers, DevOps practitioners, and computer science students from basic command-line usage to deep architectural mastery of Git and the GitHub ecosystem.

### Key Highlights

| Feature | Details |
|---|---|
| **Total Modules** | **30 self-contained modules** with theory, CLI commands, and exercises |
| **Progressive Levels** | 🟢 Beginner → 🟡 Intermediate → 🔵 Advanced → 🟣 GitHub CI/CD → ⚫ DevOps Mastery |
| **Paced Duration** | ~6 Months structured learning (or self-paced accelerated track) |
| **Visual Architecture** | Over **200+ Mermaid diagrams** (sequence diagrams, flowcharts, state machines, and DAGs) |
| **Deep Internals** | Content-addressable storage, SHA hashing, object storage formats (`blob`, `tree`, `commit`, `tag`), and plumbing commands |
| **Enterprise Practices** | Branch Protection, CODEOWNERS, GitHub Actions CI/CD matrix builds, Dependabot, GitOps, and Monorepo scaling |
| **Capstone Project** | Full-Stack collaborative repository build with a rigorous **100-point rubric** |

---

## 🗺️ 6-Month Learning Roadmap

```mermaid
flowchart TD
    subgraph PhaseA["🟢 Phase A: Git Foundations (Month 1)"]
        M01["01: Intro to VCS"] --> M02["02: Install & Config"]
        M02 --> M03["03: First Repo"]
        M03 --> M04["04: Staging & History"]
        M04 --> M05["05: Branching Basics"]
        M05 --> M06["06: Merging & Conflicts"]
    end

    subgraph PhaseB["🟡 Phase B: Intermediate Git & Collaboration (Month 2)"]
        M07["07: Remote Repos & GitHub"] --> M08["08: Push, Pull & Fetch"]
        M08 --> M09["09: Rebasing & Cherry-Pick"]
        M09 --> M10["10: Stashing & Cleaning"]
        M10 --> M11["11: Tags & SemVer"]
        M11 --> M12["12: Diff, Blame & Bisect"]
    end

    subgraph PhaseC["🔵 Phase C: Advanced Git & Internals (Month 3)"]
        M13["13: Objects, SHA & DAG"] --> M14["14: Reset, Revert & Reflog"]
        M14 --> M15["15: Interactive Rebase"]
        M15 --> M16["16: Hooks & Automation"]
        M16 --> M17["17: Submodules & Subtrees"]
        M17 --> M18["18: Advanced Merge Strategies"]
    end

    subgraph PhaseD["🟣 Phase D: GitHub Deep-Dive & CI/CD (Month 4)"]
        M19["19: Issues & Project Boards"] --> M20["20: PRs & Code Reviews"]
        M20 --> M21["21: Branch Protection & CODEOWNERS"]
        M21 --> M22["22: Actions CI/CD Fundamentals"]
        M22 --> M23["23: Actions Advanced Workflows"]
        M23 --> M24["24: Security & Dependabot"]
    end

    subgraph PhaseE["⚫ Phase E: DevOps & Capstone (Months 5 & 6)"]
        M25["25: Git Flow & Branching"] --> M26["26: Open-Source Guide"]
        M26 --> M27["27: Monorepos & Scaling"]
        M27 --> M28["28: Repo Best Practices"]
        M28 --> M29["29: Git in Production & GitOps"]
        M29 --> M30["30: Capstone Project 🏆"]
    end

    PhaseA --> PhaseB --> PhaseC --> PhaseD --> PhaseE

    style PhaseA fill:#e8f5e9,stroke:#4caf50,stroke-width:2px
    style PhaseB fill:#fffde7,stroke:#fbc02d,stroke-width:2px
    style PhaseC fill:#e3f2fd,stroke:#1e88e5,stroke-width:2px
    style PhaseD fill:#f3e5f5,stroke:#8e24aa,stroke-width:2px
    style PhaseE fill:#eceff1,stroke:#37474f,stroke-width:2px
    style M30 fill:#ffd54f,stroke:#ff8f00,stroke-width:3px
```

---

## 📚 Curriculum Index

### 🟢 Phase A — Git Foundations (Beginner)
*Master the foundational mental models, the 3 areas of Git, local branch operations, and conflict resolution.*

| # | Module | Core Topics | Est. Time | Status |
|---|--------|-------------|-----------|:------:|
| 01 | [Introduction to Version Control](./01-Introduction-to-Version-Control/README.md) | VCS Evolution, Centralized vs Distributed, Snapshot model, 3 Areas of Git | 2.0 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |
| 02 | [Git Installation & Configuration](./02-Git-Installation-and-Configuration/README.md) | Multi-OS Setup, Config Hierarchy, SSH Authentication, GPG Signing | 1.5 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |
| 03 | [Your First Repository](./03-Your-First-Repository/README.md) | `git init`, `.git` Anatomy, Object Directory, `git clone` Mechanics | 2.0 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |
| 04 | [Staging, Committing & History](./04-Staging-Committing-and-History/README.md) | Index Mechanics, Atomic Commits, `git log` Filtering, `.gitignore` Rules | 3.0 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |
| 05 | [Branching Basics](./05-Branching-Basics/README.md) | Pointer Mechanics, `HEAD` Dereferencing, `switch` vs `checkout`, Detached `HEAD` | 2.5 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |
| 06 | [Merging & Conflict Resolution](./06-Merging-and-Conflict-Resolution/README.md) | Fast-Forward, 3-Way Merge, Conflict Markers, `git mergetool` Integration | 3.0 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |

---

### 🟡 Phase B — Intermediate Git & Collaboration
*Learn remote collaboration patterns, linear rebasing, stash internals, and automated debugging with bisect.*

| # | Module | Core Topics | Est. Time | Status |
|---|--------|-------------|-----------|:------:|
| 07 | [Remote Repositories & GitHub Basics](./07-Remote-Repositories-and-GitHub-Basics/README.md) | Remotes, `origin` Architecture, HTTPS vs SSH, GitHub Web UI Tour | 3.0 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |
| 08 | [Collaboration — Push, Pull, Fetch](./08-Collaboration-Push-Pull-Fetch/README.md) | Upstream Tracking, `git fetch` vs `git pull`, Fast-Forward Pull Strategies | 3.0 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |
| 09 | [Rebasing & Cherry-Picking](./09-Rebasing-and-Cherry-Picking/README.md) | Linear History, Base Commits, Golden Rule of Rebasing, Cherry-Picking | 3.0 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |
| 10 | [Stashing & Cleaning](./10-Stashing-and-Cleaning/README.md) | Stash Stack LIFO Mechanics, Untracked Stashes, Git Worktrees, `git clean` | 2.0 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |
| 11 | [Tags, Releases & Semantic Versioning](./11-Tags-Releases-and-Semantic-Versioning/README.md) | Lightweight vs Annotated Tags, Semantic Versioning (MAJOR.MINOR.PATCH), Releases | 2.5 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |
| 12 | [Git Diff, Blame & Bisect](./12-Git-Diff-Blame-and-Bisect/README.md) | Revision Comparison, Line History with `blame`, Binary Search Debugging (`bisect`) | 2.5 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |

---

### 🔵 Phase C — Advanced Git & Internals
*Unpack Git's low-level engine: content-addressable storage, DAG graphs, history rewriting, and hooks.*

| # | Module | Core Topics | Est. Time | Status |
|---|--------|-------------|-----------|:------:|
| 13 | [Git Internals — Objects, SHA, DAG](./13-Git-Internals-Objects-SHA-DAG/README.md) | Blobs, Trees, Commits, Tags, SHA-1 Hashing, Plumbing Commands (`cat-file`) | 4.0 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |
| 14 | [Reset, Revert & Reflog](./14-Reset-Revert-and-Reflog/README.md) | Soft vs Mixed vs Hard Resets, Safe History Reverts, Disaster Recovery via Reflog | 3.0 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |
| 15 | [Interactive Rebase & History Rewriting](./15-Interactive-Rebase-and-History-Rewriting/README.md) | Squash, Fixup, Reorder, Drop, Edit Commits, Splitting Commits, Filter-Repo | 3.0 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |
| 16 | [Git Hooks & Automation](./16-Git-Hooks-and-Automation/README.md) | Client vs Server Hooks, Husky Integration, `lint-staged`, Commitlint Standards | 3.0 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |
| 17 | [Submodules & Subtrees](./17-Submodules-and-Subtrees/README.md) | Nested Repositories, Submodule Pointers, Updating & Cloning, Git Subtrees | 3.0 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |
| 18 | [Advanced Merge Strategies](./18-Advanced-Merge-Strategies/README.md) | Recursive/Ort Strategies, Ours vs Theirs, Octopus Merges, `git rerere` | 2.5 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |

---

### 🟣 Phase D — GitHub Deep-Dive & CI/CD
*Professional GitHub collaboration, branch governance, GitHub Actions automation, and security scanning.*

| # | Module | Core Topics | Est. Time | Status |
|---|--------|-------------|-----------|:------:|
| 19 | [GitHub Issues, Projects & Wikis](./19-GitHub-Issues-Projects-and-Wikis/README.md) | Issue Templates, Milestones, Project Kanban Boards, Team Collaboration | 3.0 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |
| 20 | [Pull Requests & Code Reviews](./20-Pull-Requests-and-Code-Reviews/README.md) | PR Lifecycle, Review Etiquette, Inline Suggestions, Draft PRs, Merge Types | 3.0 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |
| 21 | [Branch Protection & CODEOWNERS](./21-Branch-Protection-and-CODEOWNERS/README.md) | Required Reviews, Status Checks, Linear History Enforcement, CODEOWNERS Rules | 2.5 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |
| 22 | [GitHub Actions — CI/CD Fundamentals](./22-GitHub-Actions-CI-CD-Fundamentals/README.md) | Workflows, Runners, Events, Jobs, Steps, Secrets, Automated Testing Pipeline | 4.0 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |
| 23 | [GitHub Actions — Advanced Workflows](./23-GitHub-Actions-Advanced-Workflows/README.md) | Matrix Strategies, Dependency Caching, Reusable Workflows, Composite Actions | 4.0 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |
| 24 | [GitHub Security — Dependabot & Secrets](./24-GitHub-Security-Dependabot-and-Secrets/README.md) | Secret Scanning, Push Protection, Dependabot Vulnerability Alerts, CodeQL | 3.0 hrs | ![Ready](https://img.shields.io/badge/-Ready-brightgreen) |

---

## 🧭 How to Use This Coursework

1. Follow modules in order
2. Practice every command

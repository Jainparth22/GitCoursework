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

---

## 🧭 How to Use This Coursework

1. Follow modules in order
2. Practice every command
